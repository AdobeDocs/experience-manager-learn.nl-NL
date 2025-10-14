---
title: Gegevens samenvoegen met de XDP-sjabloon
description: PDF-document maken door gegevens samen te voegen met de sjabloon
feature: Adaptive Forms
version: Experience Manager 6.5
jira: KT-15344
topic: Development
role: User
level: Intermediate
exl-id: 6a865402-db3d-4e0e-81a0-a15dace6b7ab
duration: 15
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '80'
ht-degree: 0%

---

# Gegevens samenvoegen met de XDP-sjabloon

De volgende stap bestaat uit het samenvoegen van de XML-gegevens met de sjabloon om de PDF te genereren. Deze PDF wordt vervolgens ter ondertekening verzonden met Adobe Sign.

## De PDF genereren met OutputService

De [&#x200B; generatePDF &#x200B;](https://developer.adobe.com/experience-manager/reference-materials/6-5/forms/javadocs/com/adobe/fd/output/api/OutputService.html#generatePDFOutput-com.adobe.aemfd.docmanager.Document-com.adobe.aemfd.docmanager.Document-com.adobe.fd.output.api.PDFOutputOptions-) methode van OutputService werd gebruikt om PDF te produceren.
De gegenereerde PDF is vervolgens voor handtekeningen verzonden met de Adobe Sign REST API.
