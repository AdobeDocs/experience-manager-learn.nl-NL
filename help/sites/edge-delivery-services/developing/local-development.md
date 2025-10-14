---
title: Een lokale ontwikkelomgeving instellen voor Edge Delivery Services
description: Een lokale ontwikkelomgeving instellen voor Edge Delivery Services.
version: Experience Manager 6.5, Experience Manager as a Cloud Service
feature: Edge Delivery Services
topic: Development
role: Developer
level: Beginner
doc-type: Technical Video
last-substantial-update: 2023-11-15T00:00:00Z
jira: KT-14483
thumbnail: 3425717.jpeg
duration: 169
exl-id: 0f3e50f0-88d8-46be-be8b-0f547c3633a6
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '87'
ht-degree: 0%

---

# Een lokale ontwikkelomgeving instellen

Een lokale ontwikkelomgeving instellen voor ontwikkeling voor Edge Delivery Services.

>[!VIDEO](https://video.tv.adobe.com/v/3434737/?learn=on&captions=dut)


## Stappen die in video worden beschreven

1. AEM CLI installeren

   ```
   $ sudo npm install -g @adobe/aem-cli
   ```

1. De folder van de verandering aan uw projectfolder die een git bewaarplaats is die van het [&#x200B; AEM boilerplate &#x200B;](https://github.com/adobe/aem-boilerplate) malplaatje wordt gemaakt.

   ```
   $ git clone git@github.com:my-org/my-project.git
   $ cd my-project
   ```

1. Voer de AEM CLI uit om de lokale AEM-instantie te starten.

   ```
   $ pwd
     /Users/my-user/my-project
   
   $ aem up
       ___    ________  ___                          __      __ 
      /   |  / ____/  |/  /  _____(_)___ ___  __  __/ /___ _/ /_____  _____
     / /| | / __/ / /|_/ /  / ___/ / __ `__ \/ / / / / __ `/ __/ __ \/ ___/
    / ___ |/ /___/ /  / /  (__  ) / / / / / / /_/ / / /_/ / /_/ /_/ / /
   /_/  |_/_____/_/  /_/  /____/_/_/ /_/ /_/\__,_/_/\__,_/\__/\____/_/
   
   info: Starting AEM dev server vx.x.x
   info: Local AEM dev server up and running: http://localhost:3000/
   info: Enabled reverse proxy to https://main--my-project--my-org.hlx.page
   opening default browser: http://localhost:3000/
   ```

1. Open http://localhost:3000/ uw webbrowser om uw AEM-website te bekijken.
