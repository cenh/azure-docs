---
title: Connect hybrid machines to Azure at scale
description: In this article, you learn how to connect machines to Azure using Azure Arc-enabled servers using a service principal.
ms.date: 02/10/2022
ms.topic: conceptual 
ms.custom: devx-track-azurepowershell
---

# Connect hybrid machines to Azure at scale

You can enable Azure Arc-enabled servers for multiple Windows or Linux machines in your environment with several flexible options depending on your requirements. Using the template script we provide, you can automate every step of the installation, including establishing the connection to Azure Arc. However, you are required to interactively execute this script with an account that has elevated permissions on the target machine and in Azure.

To connect the machines to Azure Arc-enabled servers, you can use an Azure Active Directory [service principal](../../active-directory/develop/app-objects-and-service-principals.md) instead of using your privileged identity to [interactively connect the machine](onboard-portal.md). This service principal is a special limited management identity that is granted only the minimum permission necessary to connect machines to Azure using the `azcmagent` command. This is safer than using a higher privileged account like a Tenant Administrator, and follows our access control security best practices. The service principal is used only during onboarding; it is not used for any other purpose.  

The installation methods to install and configure the Connected Machine agent requires that the automated method you use has administrator permissions on the machines: on Linux by using the root account, and on Windows as a member of the Local Administrators group.

Before you get started, be sure to review the [prerequisites](agent-overview.md#prerequisites) and verify that your subscription and resources meet the requirements. For information about supported regions and other related considerations, see [supported Azure regions](overview.md#supported-regions). Also review our [at-scale planning guide](plan-at-scale-deployment.md) to understand the design and deployment criteria, as well as our management and monitoring recommendations.  

If you don't have an Azure subscription, create a [free account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) before you begin.

## Create a service principal for onboarding at scale

You can create a service principal in the Azure portal or by using Azure PowerShell.

> [!NOTE]
> To create a service principal and assign roles, your account must be a member of the **Owner** or **User Access Administrator** role in the subscription that you want to use for onboarding. If you don't have sufficient permissions to configure role assignments, the service principal might still be created, but it won't be able to onboard machines.

### Azure portal

The Azure Arc service in the Azure portal provides a streamlined way to create a service principal that can be used to connect your hybrid machines to Azure.

1. In the Azure portal, navigate to Azure Arc, then select **Service principals** in the left menu.
1. Select **Add**.
1. Enter a name for your service principal.
1. Choose whether the service principal will have access to an entire subscription, or only to a specific resource group.
1. Select the subscription (and resource group, if applicable) to which the service principal will have access.
1. In the **Client secret** section, select the duration for which your generated client secret will be in use. You can optionally enter a friendly name of your choice in the **Description** field.
1. In the **Role assignment** section, select **Azure Connected Machine Onboarding**.
1. Select **Create**.

:::image type="content" source="media/onboard-service-principal/new-azure-arc-service-principal.png" alt-text="Screenshot of the Azure Arc service principal creation screen in the Azure portal.":::

### Azure PowerShell

You can use [Azure PowerShell](/powershell/azure/install-az-ps) to create a service principal with the [New-AzADServicePrincipal](/powershell/module/Az.Resources/New-AzADServicePrincipal) cmdlet.

1. Run the following command. You must store the output of the [`New-AzADServicePrincipal`](/powershell/module/az.resources/new-azadserviceprincipal) cmdlet in a variable, or you will not be able to retrieve the password needed in a later step.

    ```azurepowershell-interactive
    $sp = New-AzADServicePrincipal -DisplayName "Arc-for-servers" -Role "Azure Connected Machine Onboarding"
    $sp
    ```

    ```output
    Secret                : System.Security.SecureString
    ServicePrincipalNames : {ad9bcd79-be9c-45ab-abd8-80ca1654a7d1, https://Arc-for-servers}
    ApplicationId         : ad9bcd79-be9c-45ab-abd8-80ca1654a7d1
    ObjectType            : ServicePrincipal
    DisplayName           : Hybrid-RP
    Id                    : 5be92c87-01c4-42f5-bade-c1c10af87758
    Type                  :
    ```

2. To retrieve the password stored in the `$sp` variable, run the following command:

    ```azurepowershell-interactive
    $credential = New-Object pscredential -ArgumentList "temp", $sp.Secret
    $credential.GetNetworkCredential().password
    ```

3. In the output, find the values for the fields **password** and **ApplicationId**. You'll need these values later, so save them in a secure place. If you forget or lose your service principal password, you can reset it using the [`New-AzADSpCredential`](/powershell/module/az.resources/new-azadspcredential) cmdlet.

The values from the following properties are used with parameters passed to the `azcmagent`:

- The value from the **ApplicationId** property is used for the `--service-principal-id` parameter value
- The value from the **password** property is used for the  `--service-principal-secret` parameter used to connect the agent.

> [!TIP]
> Make sure to use the service principal **ApplicationId** property, not the **Id** property.

The **Azure Connected Machine Onboarding** role contains only the permissions required to onboard a machine. You can assign the service principal permission to allow its scope to include a resource group or a subscription. To add role assignments, see [Assign Azure roles using the Azure portal](../../role-based-access-control/role-assignments-portal.md) or [Assign Azure roles using Azure CLI](../../role-based-access-control/role-assignments-cli.md).

## Generate the installation script from the Azure portal

The script to automate the download and installation, and to establish the connection with Azure Arc, is available from the Azure portal. To complete the process, do the following steps:

1. From your browser, go to the [Azure portal](https://portal.azure.com).

1. On the **Servers - Azure Arc** page, select **Add** at the upper left.

1. On the **Select a method** page, select the **Add multiple servers** tile, and then select **Generate script**.

1. On the **Generate script** page, select the subscription and resource group where you want the machine to be managed within Azure. Select an Azure location where the machine metadata will be stored. This location can be the same or different, as the resource group's location.

1. On the **Prerequisites** page, review the information and then select **Next: Resource details**.

1. On the **Resource details** page, provide the following:

    1. In the **Resource group** drop-down list, select the resource group the machine will be managed from.
    1. In the **Region** drop-down list, select the Azure region to store the servers metadata.
    1. In the **Operating system** drop-down list, select the operating system that the script is configured to run on.
    1. If the machine is communicating through a proxy server to connect to the internet, specify the proxy server IP address or the name and port number that the machine will use to communicate with the proxy server. Using this configuration, the agent communicates through the proxy server using the HTTP protocol. Enter the value in the format `http://<proxyURL>:<proxyport>`.
    1. Select **Next: Authentication**.

1. On the **Authentication** page, under the **service principal** drop-down list, select **Arc-for-servers**.  Then select, **Next: Tags**.

1. On the **Tags** page, review the default **Physical location tags** suggested and enter a value, or specify one or more **Custom tags** to support your standards.

1. Select **Next: Download and run script**.

1. On the **Download and run script** page, review the summary information, and then select **Download**. If you still need to make changes, select **Previous**.

For Windows, you are prompted to save `OnboardingScript.ps1`, and for Linux `OnboardingScript.sh` to your computer.

## Install the agent and connect to Azure

Taking the script template created earlier, you can install and configure the Connected Machine agent on multiple hybrid Linux and Windows machines using your organizations preferred automation tool. The script performs similar steps described in the [Connect hybrid machines to Azure from the Azure portal](onboard-portal.md) article. The difference is in the final step, where you establish the connection to Azure Arc using the `azcmagent` command using the service principal.

The following are the settings that you configure the `azcmagent` command to use for the service principal.

- `service-principal-id` : The unique identifier (GUID) that represents the application ID of the service principal.
- `service-principal-secret` | The service principal password.
- `tenant-id` : The unique identifier (GUID) that represents your dedicated instance of Azure AD.
- `subscription-id` : The subscription ID (GUID) of your Azure subscription that you want the machines in.
- `resource-group` : The resource group name where you want your connected machines to belong to.
- `location` : See [supported Azure regions](overview.md#supported-regions). This location can be the same or different, as the resource group's location.
- `resource-name` : (*Optional*) Used for the Azure resource representation of your on-premises machine. If you do not specify this value, the machine hostname is used.

You can learn more about the `azcmagent` command-line tool by reviewing the [Azcmagent Reference](./manage-agent.md).

>[!NOTE]
>The Windows PowerShell script only supports running from a 64-bit version of Windows PowerShell.

After you install the agent and configure it to connect to Azure Arc-enabled servers, go to the Azure portal to verify that the server has successfully connected. View your machines in the [Azure portal](https://aka.ms/hybridmachineportal).

![Screenshot showing a successful server connection in the Azure portal.](./media/onboard-portal/arc-for-servers-successful-onboard.png)

## Next steps

- Review the [Planning and deployment guide](plan-at-scale-deployment.md) to plan for deploying Azure Arc-enabled servers at any scale and implement centralized management and monitoring.
- Learn how to [troubleshoot agent connection issues](troubleshoot-agent-onboard.md).
- Learn how to manage your machines using [Azure Policy](../../governance/policy/overview.md) for such things as VM [guest configuration](../../governance/policy/concepts/guest-configuration.md), verifying that machines are reporting to the expected Log Analytics workspace, monitoring with [VM insights](../../azure-monitor/vm/vminsights-enable-policy.md), and more.
