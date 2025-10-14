---
title: Symlinks correct toevoegen aan GIT
description: Instructies over hoe en waar u symlinks wilt toevoegen wanneer u werkt aan uw Dispatcher-configuraties.
version: Experience Manager 6.5
topic: Administration
feature: Dispatcher
role: Admin
level: Beginner
thumbnail: xx.jpg
doc-type: Article
exl-id: 6e751586-e92e-482d-83ce-6fcae4c1102c
duration: 295
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '1231'
ht-degree: 0%

---

# Symlinks toevoegen aan GIT

[Inhoudsopgave](./overview.md)

[&lt;- Vorige: Dispatcher Health Check](./health-check.md)

In AMS krijgt u een vooraf ingevulde GIT-opslagplaats die de broncode van uw verzender bevat en die klaar is voor u om de ontwikkeling en aanpassing te starten.

Nadat u het eerste `.vhost` -bestand of het bovenste `farm.any` -bestand hebt gemaakt, moet u een symbolische koppeling maken vanuit de map `available_*` naar de map `enabled_*` . Het gebruiken van het juiste verbindingstype zal zeer belangrijk aan een succesvolle plaatsing door de pijpleiding van Cloud Manager zijn. Deze pagina helpt u te weten hoe u dit kunt doen.

## Dispatcher Archetype

De ontwikkelaar van AEM begint typisch hun project van [&#x200B; AEM archetype &#x200B;](https://github.com/adobe/aem-project-archetype)

Hier volgt een voorbeeld van het gebied van de broncode waar u de gebruikte symlinks kunt zien:

```
$ tree dispatcher
dispatcher
└── src
   ├── conf.d
.....SNIP.....
    │   └── available_vhosts
    │   │   ├── 000_unhealthy_author.vhost
    │   │   ├── 000_unhealthy_publish.vhost
    │   │   ├── aem_author.vhost
    │   │   ├── aem_flush.vhost
    │   │   ├── aem_health.vhost
    │   │   ├── aem_lc.vhost
    │   │   └── aem_publish.vhost
    └── dispatcher_vhost.conf
    │   └── enabled_vhosts
    │   │   ├── aem_author.vhost -> ../available_vhosts/aem_author.vhost
    │   │   ├── aem_flush.vhost -> ../available_vhosts/aem_flush.vhost
    │   │   ├── aem_health.vhost -> ../available_vhosts/aem_health.vhost
    │   │   └── aem_publish.vhost -> ../available_vhosts/aem_publish.vhost
.....SNIP.....
    └── conf.dispatcher.d
    │   ├── available_farms
    │   │   ├── 000_ams_author_farm.any
    │   │   ├── 001_ams_lc_farm.any
    │   │   └── 002_ams_publish_farm.any
.....SNIP.....
    │   └── enabled_farms
    │   │   ├── 000_ams_author_farm.any -> ../available_farms/000_ams_author_farm.any
    │   │   └── 002_ams_publish_farm.any -> ../available_farms/002_ams_publish_farm.any
.....SNIP.....
17 directories, 60 files
```

De map `/etc/httpd/conf.d/available_vhosts/` bevat bijvoorbeeld de gefaseerde potentiële `.vhost` -bestanden die we in onze actieve configuratie kunnen gebruiken.

De ingeschakelde `.vhost` bestanden worden weergegeven als relatieve paden `symlinks` in de map `/etc/httpd/conf.d/enabled_vhosts/` .

## Een symlink maken

We gebruiken symbolische koppelingen naar het bestand, zodat het doelbestand op de Apache-webserver als hetzelfde bestand wordt behandeld.  We willen het bestand niet in beide mappen dupliceren.  In plaats daarvan hoeft u alleen maar een sneltoets te gebruiken van de ene map (symbolische koppeling) naar de andere.

Erken dat uw geïmplementeerde configuraties zich richten op een Linux-host.  Het creëren van een symlink die niet compatibel met het doelsysteem is zal mislukkingen en ongewenste resultaten veroorzaken.

Als uw werkstation geen Linux-computer is, vraagt u zich waarschijnlijk af met welke opdrachten deze koppelingen op de juiste wijze kunnen worden gemaakt, zodat ze zich kunnen vastleggen op GIT.

> `TIP:` Het is belangrijk relatieve koppelingen te gebruiken, omdat als u een lokale kopie van Apache Webserver hebt geïnstalleerd en een andere installatielocatie hebt, de koppelingen nog steeds werken.  Als u een absoluut pad gebruikt, moeten uw werkstation of andere systemen precies overeenkomen met dezelfde mapstructuur.

### OSX/Linux

Symbolen zijn native voor deze besturingssystemen en hier zijn enkele voorbeelden van hoe u deze koppelingen kunt maken.  Open uw favoriete eindtoepassing en gebruik de volgende voorbeeldbevelen om de verbinding tot stand te brengen:

```
$ cd <LOCATION OF CLONED GIT REPO>\src\conf.d\enabled_vhosts
$ ln -s ../available_vhosts/<Destination File Name> <Target File Name>
```

Hier volgt een voorbeeld van een gevulde opdracht ter referentie:

```
$ git clone https://github.com/adobe/aem-project-archetype.git
$ cd aem-project-archetype/src/main/archetype/dispatcher.ams/src/conf.d/enabled_vhosts/
$ ln -s ../available_vhosts/aem_flush.vhost aem_flush.vhost
```

Hier volgt een voorbeeld van de koppeling als u het bestand opsomt met de opdracht `ls` :

```
ls -l
total 0
lrwxrwxrwx. 1 root root 35 Oct 13 21:38 aem_flush.vhost -> ../available_vhosts/aem_flush.vhost
```

### Windows

> `Note:` Geeft aan dat MS Windows (beter, NTFS) symbolische koppelingen ondersteunt sinds.. Windows Vista!

![&#x200B; Beeld van de Herinnering van het Bevel van Vensters die de hulpoutput van het mklink bevel tonen &#x200B;](./assets/git-symlinks/windows-terminal-mklink.png)

> Voor het uitvoeren van de opdracht mklink voor het maken van symlinks zijn beheerdersrechten vereist. `Warning:` Zelfs als Admin-account, moet u de opdrachtprompt &quot;Als beheerder&quot; uitvoeren, tenzij de ontwikkelaarsmodus is ingeschakeld
> <br/>Onjuiste machtigingen:
> ![Beeld van de Herinnering van het Bevel van Vensters die het bevel tonen ontbreekt toe te schrijven aan toestemmingen &#x200B;](./assets/git-symlinks/windows-mklink-underpriv.png)
> <br/>Juiste machtigingen:
> ![Beeld van de Herinnering van het Bevel van Vensters liep als Beheerder &#x200B;](./assets/git-symlinks/windows-mklink-properpriv.png)

Hier volgt de opdracht(en) om de koppeling te maken:

```
C:\<PATH TO SRC>\enabled_vhosts> mklink <Target File Name> ..\available_vhosts\<Destination File Name>
```


Hier volgt een voorbeeld van een gevulde opdracht ter referentie:

```
C:\> git clone https://github.com/adobe/aem-project-archetype.git
C:\> cd aem-project-archetype\src\main\archetype\dispatcher.ams\src\conf.d\enabled_vhosts\
C:\aem-project-archetype\src\main\archetype\dispatcher.ams\src\conf.d\enabled_vhosts> mklink aem_flush.vhost ..\available_vhost\aem_flush.vhost
symbolic link created for aem_flush.vhost <<===>> ..\available_vhosts\aem_flush.vhost
```

#### Modus Ontwikkelaar (Windows 10)

Wanneer gezet in [&#x200B; Wijze van de Ontwikkelaar &#x200B;](https://docs.microsoft.com/en-us/windows/apps/get-started/enable-your-device-for-development), staat Vensters 10 u toe om apps gemakkelijker te testen u ontwikkelt, het shell milieu van Ubuntu Bash gebruiken, een verscheidenheid van ontwikkelaar-gerichte montages veranderen, en andere dergelijke dingen doen.

Microsoft lijkt eigenschappen aan de Wijze van de Ontwikkelaar te blijven toevoegen, of sommige van die eigenschappen toe te laten door gebrek zodra zij een meer wijdverspreide goedkeuring bereiken en als stabiel (bijvoorbeeld met de Update van Scheppers, vereist het milieu van Ubuntu Bash Shell niet meer de Wijze van de Ontwikkelaar) worden beschouwd.

Hoe zit het met symlinks? Met INGESCHAKELDE Wijze van de Ontwikkelaar, is er geen behoefte om een bevelherinnering met opgeheven voorrechten in werking te stellen om tot symlinks te kunnen leiden. Daarom kunnen gebruikers, zodra de Wijze van de Ontwikkelaar wordt toegelaten, symlinks tot stand brengen.

> Na het toelaten van de Wijze van de Ontwikkelaar, zouden de gebruikers moeten logoff/opening van een sessie voor de veranderingen van kracht worden.

Nu kunt u zonder als Beheerder te lopen zien het bevel werkt

![&#x200B; Beeld van de Herinnering van het Bevel van Vensters in werking gesteld als normale gebruiker met toegelaten de Wijze van de Ontwikkelaar &#x200B;](./assets/git-symlinks/windows-mklink-devmode.png)

#### Alternatieve/programmatische aanpak

Er is een specifiek beleid dat bepaalde gebruikers kan toestaan om symbolische verbindingen tot stand te brengen → [&#x200B; creeer symbolische verbindingen (Vensters 10) - de veiligheid van Vensters | Microsoft Docs &#x200B;](https://docs.microsoft.com/en-us/windows/security/threat-protection/security-policy-settings/create-symbolic-links)

PRO&#39;s:
- Dit zou door klanten kunnen worden leveraged om symbolische verbindingen verwezenlijking aan alle ontwikkelaars binnen hun (d.w.z. Actieve Folder) programmatically toe te staan zonder het moeten de Wijze van de Ontwikkelaar op elk apparaat manueel toelaten.
- Bovendien, zou dit beleid in vroegere versies van MS Windows beschikbaar moeten zijn die de Wijze van de Ontwikkelaar niet aanbieden.

CON&#39;s:
- Dit beleid lijkt geen effect te hebben op gebruikers die tot de groep van Beheerders behoren. De beheerders zouden nog de herinnering van het Bevel met opgeheven voorrechten moeten in werking stellen. Vreemd.

> De gebruiker logoff/opening van een sessie zal worden vereist voor de veranderingen in het lokale/groepsbeleid om van kracht te worden.

Voer `gpedit.msc` uit, voeg gebruikers toe of wijzig gebruikers naar wens. Beheerders zijn er standaard

![&#x200B; het venster van de Redacteur van het Beleid van de Groep die toestemming toont om &#x200B;](./assets/git-symlinks/windows-localpolicies-symlinks.png) aan te passen

#### Symbolen inschakelen in GIT

De handvatten van de it volgens de core.symlinks optie

Source: [&#x200B; it - git-config Documentatie &#x200B;](https://git-scm.com/docs/git-config#Documentation/git-config.txt-coresymlinks)

*als core.symlinks vals is, worden de symbolische verbindingen gecontroleerd als kleine gewone dossiers die de verbindingstekst bevatten. `git-update-index[1]` en `git-add[1]` veranderen het opgenomen type niet in een normaal bestand. Nuttig op bestandssystemen zoals FAT die geen ondersteuning bieden voor symbolische koppelingen.
Het gebrek is waar, behalve `git-clone[1]` of `git-init[1] will probe and set core.symlinks false if appropriate when the repository is created.` in de meeste gevallen, Git zal veronderstellen Vensters niet goed voor symlinks is en zal dit aan vals plaatsen.*

Het gedrag van Git op Vensters wordt duidelijk hier: Symbolische Verbindingen ・ git-for-windows/git Wiki ・ GitHub

> `Info`: De veronderstellingen die worden vermeld in de documentatie verbonden hierboven lijken o.k. met een mogelijke opstelling van de Ontwikkelaar van AEM op Vensters, met name NTFS en het feit dat wij slechts dossiersymlinks vs. de symlinks van de Folder hebben

Hier is het goede nieuws, aangezien [&#x200B; Git voor versie 2.10.2 van Vensters &#x200B;](https://github.com/git-for-windows/git/releases/tag/v2.10.2.windows.1) de installateur een [&#x200B; expliciete optie heeft om symbolische verbindingssteun toe te laten.](https://github.com/git-for-windows/git/issues/921)

> `Warning`: De optie core.symlink kan bij uitvoering worden opgegeven terwijl de gegevensopslagruimte wordt gekloond, of anders als een algemene configuratie worden opgeslagen.

![&#x200B; tonend de installateur van GIT die steun voor symlinks toont &#x200B;](./assets/git-symlinks/windows-git-install-symlink.png)

Git voor Windows slaat algemene voorkeuren op in `"C:\Program Files\Git\etc\gitconfig"` . Deze instellingen worden mogelijk niet in aanmerking genomen door andere Git-bureaubladclient-apps.
Hier is vangst, niet zullen alle ontwikkelaars de inheemse cliënt van het Git (d.w.z. Cmd van het Git, Bash) gebruiken, en sommige van de Apps van de Desktop van het Git (b.v. Desktop GitHub, Atlassian Bronree) kunnen verschillende montages/gebreken hebben om Systeem of een ingebedde Git te gebruiken

Hier volgt een voorbeeld van wat zich in het `gitconfig` -bestand bevindt

```
[diff "astextplain"]
    textconv = astextplain
[filter "lfs"]
    clean = git-lfs clean -- %f
    smudge = git-lfs smudge -- %f
    process = git-lfs filter-process
    required = true
[http]
    sslBackend = openssl
    sslCAInfo = C:/Program Files/Git/mingw64/ssl/certs/ca-bundle.crt
[core]
    autocrlf = true
    fscache = true
    symlinks = true
[pull]
    rebase = false
[credential]
    helper = manager-core
[credential "https://dev.azure.com"]
    useHttpPath = true
[init]
    defaultBranch = master
```

#### Tips voor opdrachtregel voor it

Er kunnen scenario&#39;s zijn waar u nieuwe symbolische verbindingen (b.v. het toevoegen van een nieuwe gastheer of een nieuw landbouwbedrijf) moet tot stand brengen.

In de bovenstaande documentatie hebben we gezien dat Windows de opdracht &quot;mklink&quot; biedt om symbolische koppelingen te maken.

Als u in een Git Bash-omgeving werkt, kunt u in plaats daarvan de standaard Bash-opdracht `ln -s` gebruiken, maar deze moet vooraf worden gegaan door een speciale instructie zoals in het volgende voorbeeld:

```
MSYS=winsymlinks:nativestrict ln -s test_vhost_symlink ../dispatcher/src/conf.d/available_vhosts/default.vhost
```

#### Samenvatting

Voor een correcte verwerking van de Git (minstens voor het werkingsgebied van de huidige de configuratiebasislijn van AEM dispatcher) op Microsoft Windows OS, zult u nodig hebben:

| Item | Minimale versie / configuratie | Aanbevolen versie / configuratie |
|------|---------------------------------|-------------------------------------|
| Besturingssysteem | Windows Vista of hoger | Windows 10 Creator-update of nieuwer |
| Bestandssysteem | NTFS | NTFS |
| Mogelijkheid om symlinks voor de Windows-gebruiker af te handelen | `"Create symbolic links"` group / local Policy `under "Group Computer Configuration\Windows Settings\Security Settings\Local Policies\User Rights Assignment"` | Windows 10 Developer Mode ingeschakeld |
| GIT | Systeemeigen clientversie 1.5.3 | Systeemeigen clientversie 2.10.2 of hoger |
| Git Config | `--core.symlinks=true` wanneer u een kloon maakt via de opdrachtregel | Globale configuratie ophalen <br/>`[core]`<br/>    symlinks = true <br/> Native Git client config path: `C:\Program Files\Git\etc\gitconfig` <br/> Standaardlocatie voor Git Desktop-clients: `%HOMEPATH%\.gitconfig` |

> `Note:` Als u al een lokale opslagplaats hebt, moet u een nieuwe kloon maken van de oorsprong. U kunt klonen naar een nieuwe locatie en uw niet-vastgelegde/niet-gestarte lokale wijzigingen handmatig samenvoegen in de nieuw gekloonde opslagplaats.
