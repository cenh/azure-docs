---
title: Performance impact of Kerberos on Azure NetApp Files NFSv4.1 volumes | Microsoft Docs
description: Describes the available security options, the tested performance vectors, and the expected performance impact of kerberos on Azure NetApp Files NFSv4.1 volumes.  
services: azure-netapp-files
documentationcenter: ''
author: b-hchen
manager: ''
editor: ''

ms.assetid:
ms.service: azure-netapp-files
ms.workload: storage
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 02/18/2021
ms.author: anfdocs
---
# Performance impact of Kerberos on Azure NetApp Files NFSv4.1 volumes

Azure NetApp Files supports [NFS client encryption in Kerberos](configure-kerberos-encryption.md) modes (krb5, krb5i, and krb5p) with AES-256 encryption. This article describes the performance impact of Kerberos on NFSv4.1 volumes. 

## Available security options 

The security options currently available for NFSv4.1 volumes are as follows: 

* **sec=sys** uses local UNIX UIDs and GIDs by using AUTH_SYS to authenticate NFS operations.
* **sec=krb5** uses Kerberos V5 instead of local UNIX UIDs and GIDs to authenticate users.
* **sec=krb5i** uses Kerberos V5 for user authentication and performs integrity checking of NFS operations using secure checksums to prevent data tampering.
* **sec=krb5p** uses Kerberos V5 for user authentication and integrity checking. It encrypts NFS traffic to prevent traffic sniffing. This option is the most secure setting, but it also involves the most performance overhead.

## Performance vectors tested

This section describes the single client-side performance impact of the various `sec=*` options.

* Performance impact was tested at two levels: low concurrency (low load) and high concurrency (upper limits of I/O and throughput).  
* Three types of workloads were tested:  
    * Small operation random read/write (using FIO)
    * Large operation sequential read/write (using FIO)
    * Metadata heavy workload as generated by applications such as git

## Expected performance impact 

There are two areas of focus: light load and upper limit. The following lists describe the performance impact security setting by security setting and scenario by scenario. All comparisons are made against the `sec=sys` security parameter. The test was done on a single volume, using a single client. 

Performance impact of krb5:

* Low concurrency (r/w):
    * Sequential latency increased 0.3 ms.
    * Random I/O latency increased 0.2 ms.
    * Metadata I/O latency increased 0.2 ms.
* High concurrency (r/w): 
    * Maximum sequential throughput was unimpacted by krb5.
    * Maximum random I/O decreased by 30% for pure read workloads with the overall impact dropping to zero as the workload shifts to pure write. 
    * Maximum metadata workload decreased 30%.

Performance impact of krb5i: 

* Low concurrency (r/w):
    * Sequential latency increased 0.5 ms.
    * Random I/O latency increased 0.2 ms.
    * Metadata I/O latency increased 0.2 ms.
* High concurrency (r/w): 
    * Maximum sequential throughput decreased by 70% overall regardless of the workload mixture.
    * Maximum random I/O decreased by 50% for pure read workloads with the overall impact decreasing to 25% as the workload shifts to pure write. 
    * Maximum metadata workload decreased 30%.

Performance impact of krb5p:

* Low concurrency (r/w):
    * Sequential latency increased 0.8 ms.
    * Random I/O latency increased 0.2 ms.
    * Metadata I/O latency increased 0.2 ms.
* High concurrency (r/w): 
    * Maximum sequential throughput decreased by 85% overall regardless of the workload mixture. 
    * Maximum random I/O decreased by 65% for pure read workloads with the overall impact decreasing to 43% as the workload shifts to pure write. 
    * Maximum metadata workload decreased 30%.

## Next steps  

* [Configure NFSv4.1 Kerberos encryption for Azure NetApp Files](configure-kerberos-encryption.md) 
