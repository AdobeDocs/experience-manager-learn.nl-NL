---
title: Gegevens samenvoegen met de XDP-sjabloon
description: PDF-document maken door gegevens samen te voegen met de sjabloon
feature: Adaptive Forms
version: 6.5
jira: KT-15344
topic: Development
role: User
level: Intermediate
source-git-commit: eb4463ae0270725c5b0bd96e9799bada25b06303
workflow-type: tm+mt
source-wordcount: '80'
ht-degree: 0%

---

# Gegevens samenvoegen met de XDP-sjabloon

De volgende stap bestaat uit het samenvoegen van de XML-gegevens met de sjabloon om de PDF te genereren. Deze PDF wordt vervolgens ter ondertekening verzonden met Adobe Sign.

## OutputService gebruiken om de PDF te genereren

De [generatePDF](https://developer.adobe.com/experience-manager/reference-materials/6-5/forms/javadocs/com/adobe/fd/output/api/OutputService.html#generatePDFOutput-com.adobe.aemfd.docmanager.Document-com.adobe.aemfd.docmanager.Document-com.adobe.fd.output.api.PDFOutputOptions-) De methode van OutputService werd gebruikt om PDF te produceren.
De gegenereerde PDF is vervolgens verzonden voor handtekeningen met de Adobe Sign REST API.

