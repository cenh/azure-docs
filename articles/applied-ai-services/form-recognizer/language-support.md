---
title: Language support - Form Recognizer
titleSuffix: Azure Applied AI Services
description: Learn more about the human languages that are available with Form Recognizer.
author: laujan
manager: nitinme
ms.service: applied-ai-services
ms.subservice: forms-recognizer
ms.topic: overview
ms.date: 10/07/2021
ms.author: lajanuar
---

# Language support for Form Recognizer

 This table lists the written languages supported by each Form Recognizer service.

<!-- markdownlint-disable MD001 -->
<!-- markdownlint-disable MD024 -->

## Read, Layout, and Custom form (template) model


The following lists include the currently GA languages in the the 2.1 version and new ones in the most recent 3.0 preview. These languages are supported by Read, Layout, and Custom form (template) model features.

> [!NOTE]
> **Language code optional**
>
> Form Recognizer's deep learning based universal models extract all multi-lingual text in your documents, including text lines with mixed languages, and do not require specifying a language code. Do not provide the language code as the parameter unless you are sure about the language and want to force the service to apply only the relevant model. Otherwise, the service may return incomplete and incorrect text.

To use the preview languages, refer to the [v3.0 REST API migration guide](/rest/api/media/#changes-to-the-rest-api-endpoints) to understand the differences from the v2.1 GA API and explore the [v3.0 preview SDK quickstarts](quickstarts/try-v3-python-sdk.md).

### Handwritten languages

The following table lists the handwritten languages.

|Language| Language code (optional) | Language| Language code (optional) |
|:-----|:----:|:-----|:----:|
|English|`en`|Japanese (preview) |`ja`|
|Chinese Simplified (preview)  |`zh-Hans`|Korean (preview)|`ko`|
|French (preview) |`fr`|Portuguese (preview)|`pt`|
|German (preview) |`de`|Spanish (preview) |`es`|
|Italian (preview) |`it`|

### Print languages (preview)

This section lists the supported languages in the latest preview.

|Language| Code (optional) |Language| Code (optional) |
|:-----|:----:|:-----|:----:|
|Angika (Devanagiri) | `anp`|Lakota | `lkt`
|Arabic | `ar`|Latin | `la`
|Awadhi-Hindi (Devanagiri) | `awa`|Lithuanian | `lt`
|Azerbaijani (Latin) | `az`|Lower Sorbian | `dsb`
|Bagheli | `bfy`|Lule Sami | `smj`
|Belarusian (Cyrillic)  | `be`, `be-cyrl`|Mahasu Pahari (Devanagiri) | `bfz`
|Belarusian (Latin) | `be`, `be-latn`|Maltese | `mt`
|Bhojpuri-Hindi (Devanagiri) | `bho`|Malto (Devanagiri) | `kmj`
|Bodo (Devanagiri) | `brx`|Maori | `mi`
|Bosnian (Latin) | `bs`|Marathi | `mr`
|Brajbha | `bra`|Mongolian (Cyrillic)  | `mn`
|Bulgarian  | `bg`|Montenegrin (Cyrillic)  | `cnr-cyrl`
|Bundeli | `bns`|Montenegrin (Latin) | `cnr-latn`
|Buryat (Cyrillic) | `bua`|Nepali | `ne`
|Chamling | `rab`|Niuean | `niu`
|Chhattisgarhi (Devanagiri)| `hne`|Nogay | `nog`
|Croatian | `hr`|Northern Sami (Latin) | `sme`
|Dari | `prs`|Ossetic  | `os`
|Dhimal (Devanagiri) | `dhi`|Pashto | `ps`
|Dogri (Devanagiri) | `doi`|Persian | `fa`
|Erzya (Cyrillic) | `myv`|Punjabi (Arabic) | `pa`
|Faroese | `fo`|Ripuarian | `ksh`
|Gagauz (Latin) | `gag`|Romanian | `ro`
|Gondi (Devanagiri) | `gon`|Russian | `ru`
|Gurung (Devanagiri) | `gvr`|Sadri  (Devanagiri) | `sck`
|Halbi (Devanagiri) | `hlb`|Samoan (Latin) | `sm`
|Haryanvi | `bgc`|Sanskrit (Devanagari) | `sa`
|Hawaiian | `haw`|Santali(Devanagiri) | `sat`
|Hindi | `hi`|Serbian (Latin) | `sr`, `sr-latn`
|Ho(Devanagiri) | `hoc`|Sherpa (Devanagiri) | `xsr`
|Icelandic | `is`|Sirmauri (Devanagiri) | `srx`
|Inari Sami | `smn`|Skolt Sami | `sms`
|Jaunsari (Devanagiri) | `Jns`|Slovak | `sk`
|Kangri (Devanagiri) | `xnr`|Somali (Arabic) | `so`
|Karachay-Balkar  | `krc`|Southern Sami | `sma`
|Kara-Kalpak (Cyrillic) | `kaa-cyrl`|Tajik (Cyrillic)  | `tg`
|Kazakh (Cyrillic)  | `kk-cyrl`|Thangmi | `thf`
|Kazakh (Latin) | `kk-latn`|Tongan | `to`
|Khaling | `klr`|Turkmen (Latin) | `tk`
|Korku | `kfq`|Tuvan | `tyv`
|Koryak | `kpy`|Urdu  | `ur`
|Kosraean | `kos`|Uyghur (Arabic) | `ug`
|Kumyk (Cyrillic) | `kum`|Uzbek (Arabic) | `uz-arab`
|Kurdish (Arabic) | `ku-arab`|Uzbek (Cyrillic)  | `uz-cyrl`
|Kurukh (Devanagiri) | `kru`|Welsh | `cy`
|Kyrgyz (Cyrillic)  | `ky`

### Print languages (GA)

This section lists the supported languages in the latest GA version.

|Language| Code (optional) |Language| Code (optional) |
|:-----|:----:|:-----|:----:|
|Afrikaans|`af`|Japanese | `ja` |
|Albanian |`sq`|Javanese | `jv` |
|Asturian |`ast`|K'iche'  | `quc` |
|Basque  |`eu`|Kabuverdianu | `kea` |
|Bislama   |`bi`|Kachin (Latin) | `kac` |
|Breton    |`br`|Kara-Kalpak (Latin) | `kaa` |
|Catalan    |`ca`|Kashubian | `csb` |
|Cebuano    |`ceb`|Khasi  | `kha` |
|Chamorro  |`ch`|Korean | `ko` |
|Chinese Simplified | `zh-Hans`|Kurdish (Latin) | `ku-latn`
|Chinese Traditional | `zh-Hant`|Luxembourgish  | `lb` |
|Cornish     |`kw`|Malay (Latin) | `ms` |
|Corsican      |`co`|Manx  | `gv` |
|Crimean Tatar (Latin)|`crh`|Neapolitan   | `nap` |
|Czech | `cs` |Norwegian | `no` |
|Danish | `da` |Occitan | `oc` |
|Dutch | `nl` |Polish | `pl` |
|English | `en` |Portuguese | `pt` |
|Estonian  |`et`|Romansh  | `rm` |
|Fijian |`fj`|Scots  | `sco` |
|Filipino  |`fil`|Scottish Gaelic  | `gd` |
|Finnish | `fi` |Slovenian  | `sl` |
|French | `fr` |Spanish | `es` |
|Friulian  | `fur` |Swahili (Latin)  | `sw` |
|Galician   | `gl` |Swedish | `sv` |
|German | `de` |Tatar (Latin)  | `tt` |
|Gilbertese    | `gil` |Tetum    | `tet` |
|Greenlandic   | `kl` |Turkish | `tr` |
|Haitian Creole  | `ht` |Upper Sorbian  | `hsb` |
|Hani  | `hni` |Uzbek (Latin)     | `uz` |
|Hmong Daw (Latin)| `mww` |Volapük   | `vo` |
|Hungarian | `hu` |Walser    | `wae` |
|Indonesian   | `id` |Western Frisian | `fy` |
|Interlingua  | `ia` |Yucatec Maya | `yua` |
|Inuktitut (Latin) | `iu` |Zhuang | `za` |
|Irish    | `ga` |Zulu  | `zu` |
|Italian | `it` |

## Custom neural model

Language| Locale code |
|:-----|:----:|
|English (United States)|en-us|

## Receipt and business card models

>[!NOTE]
 > It's not necessary to specify a locale. This is an optional parameter. The Form Recognizer deep-learning technology will auto-detect the language of the text in your image.

Pre-Built Receipt and Business Cards support all English receipts and business cards with the following locales:

|Language| Locale code |
|:-----|:----:|
|English (Australia)|`en-au`|
|English (Canada)|`en-ca`|
|English (United Kingdom)|`en-gb`|
|English (India|`en-in`|
|English (United States)| `en-us`|

## Invoice model

Language| Locale code |
|:-----|:----:|
|English (United States)|en-us|
|Spanish (preview) | es |

## ID documents

This technology is currently available for US driver licenses and the biographical page from international passports (excluding visa and other travel documents).

## General Document

Language| Locale code |
|:-----|:----:|
|English (United States)|en-us|

