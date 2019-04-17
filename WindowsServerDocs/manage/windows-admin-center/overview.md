---
title: Panoramica di Windows Admin Center
description: Informazioni sulla gestione di Windows Server utilizzando Windows Admin Center (Project Honolulu)
ms.technology: manage
ms.topic: article
author: nwashburn-ms
ms.author: niwashbu
ms.localizationpriority: high
ms.prod: windows-server-threshold
ms.openlocfilehash: d941e9884dced40ce750645b662d8df73503bc41
ms.sourcegitcommit: 475292afc919c6d17569f05007a97bc6b92dd225
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 03/28/2019
ms.locfileid: "9267777"
---
# Windows Admin Center

>Si applica a: Windows Admin Center, Windows Admin Center Preview

Benvenuti in Windows Admin Center!

**Windows Admin Center** (nome in codice **Project Honolulu**) è un'evoluzione degli strumenti di gestione inclusi in Windows Server; è un singolo riquadro interfaccia che consolida tutti gli aspetti della gestione dei server locali e remoti. In quanto esperienza di gestione basata su browser e distribuita localmente, non sono necessari una connessione Internet e Azure. Windows Admin Center offre il controllo completo di tutti gli aspetti della distribuzione, incluse le reti private che non sono connesse a Internet.

## Introduzione

>[!VIDEO https://www.youtube.com/embed/PcQj6ZklmK0]

![Infografica di Windows Admin Center](media/WAC1809Poster_thumb.PNG)

[Scarica il PDF](https://github.com/MicrosoftDocs/windowsserverdocs/raw/master/WindowsServerDocs/manage/windows-admin-center/media/WindowsAdminCenter1809Poster.pdf)

## Avvio rapido

Puoi rendere operativo Windows Admin Center nel tuo ambiente in pochi minuti:

1. [Download](https://aka.ms/windowsadmincenter)
2. [Installare](deploy/install.md)
3. [Informazioni di base](use/get-started.md)

## Contenuti in breve

<table>
    <tr></tr>
    <tr>
        <td style="vertical-align: top;">
            <h3>Esaminare con attenzione</h3>
            <ul>
            <li><a href="understand/what-is.md">Che cos'è Windows Admin Center?</a>
            <li><a href="understand/faq.md">Domande frequenti</a>
            <li><a href="understand/case-studies.md">Case study</a>
            <li><a href="understand/related-management.md">Prodotti di gestione correlati</a>
            <li><a href="understand/videos.md">Video</a>
            </ul>
        </td>
        <td style="vertical-align: top;">
            <h3>Pianificazione</h3>
            <ul>
            <li><a href="plan/installation-options.md">Che tipo di installazione è adatto alle tue esigenze?</a>
            <li><a href="plan/user-access-options.md">Opzioni di accesso dell'utente</a>
            <li><a href="plan/azure-integration-options.md">Quali opzioni di integrazione Azure sono incluse?</a>
            <br>
            </ul>
        </td>
    </tr>
    <tr>
        <td style="vertical-align: top;">
            <h3>Distribuisci</h3>
            <ul>
            <li><a href="deploy/prepare-environment.md">Preparazione dell'ambiente</a>
            <li><a href="deploy/install.md">Installare Windows Admin Center</a>
            <li><a href="deploy/high-availability.md">Offrire una disponibilità elevata</a>
         </ul>
        </td>
        <td style="vertical-align: top;">
            <h3>Configurare</h3>
            <ul>
            <li><a href="configure/settings.md">Impostazioni di Windows Admin Center</a>
            <li><a href="configure/user-access-control.md">Autorizzazioni e controllo dell'accesso utente</a>
            <li><a href="configure/using-extensions.md">Estensioni</a>
            <li><a href="configure/azure-integration.md">Integrazione con Azure</a>
            <li><a href="configure/manage-azure-vms.md">Gestisci le macchine virtuali di Azure con Windows Admin Center</a>
            </ul>
        </td>
    </tr>
    <tr>
        <td style="vertical-align: top;">
            <h3>Usare</h3>
            <ul>
            <li><a href="use/get-started.md">Avvio & aggiungere connessioni</a>
            <li><a href="use/manage-servers.md">Gestire server</a>
            <li><a href="use/manage-hyper-converged.md">Gestire l'infrastruttura iperconvergente</a>
            <li><a href="use/manage-failover-clusters.md">Gestire i cluster di failover</a>
            <li><a href="use/manage-virtual-machines.md">Gestire le macchine virtuali</a>
            <li><a href="use/azure-services.md">Utilizzare i servizi di Azure</a>
            <li><a href="use/troubleshooting.md">Passaggi di risoluzione dei problemi comuni</a>
            <li><a href="use/logging.md">Registrazione</a>
            <li><a href="use/known-issues.md">Problemi noti</a>
            </ul>
        </td>
        <td style="vertical-align: top;">
            <h3>Estendere</h3>
            <ul>
            <li><a href="extend/extensibility-overview.md">Panoramica delle estensioni</a>
            <li><a href="extend/understand-extensions.md">Informazioni sulle estensioni</a>
            <li><a href="extend/developing-extensions.md">Sviluppare un'estensione</a>
            <li><a href="extend/publish-extensions.md">Guide</a>
            <li><a href="extend/publish-extensions.md">Pubblicazione delle estensioni</a>
            </ul>
        </td>
    </tr>

</table>

## Cronologia delle versioni

Informazioni sulle ultime funzionalità rilasciate:

- Versione [1903] (https://aka.ms/wac1903) vengono visualizzate le notifiche di posta elettronica di Monitoraggio di Azure, è possibile aggiungere connessioni server o PC da Active Directory e sono disponibili nuovi strumenti per la gestione di Active Directory, DHCP e DNS.
- Versione [1902] (https://aka.ms/wac1902) miglioramenti & elenco connessione condivisa per la gestione della rete (SDN) software-definito, inclusi i nuovi strumenti SDN per gestire gli ACL, le connessioni gateway e reti logiche.
- Versione [1812](https://aka.ms/wac1812) aggiunto tema scuro (in anteprima), le impostazioni di configurazione di risparmio energia, info BMC e il supporto di PowerShell per gestire [le estensioni](./configure/using-extensions.md#manage-extensions-with-powershell) e [le connessioni](./use/get-started.md#use-powershell-to-import-or-export-your-connections-with-tags).
- Versione [1809.5](https://aka.ms/wac1809.5) è un aggiornamento cumulativo GA che include vari qualità e funzionali miglioramenti e correzioni di bug in tutta la piattaforma e alcune nuove funzionalità della soluzione di gestione dell'infrastruttura iperconvergente.
- Versione [1809](https://cloudblogs.microsoft.com/windowsserver/2018/09/20/windows-admin-center-1809-and-sdk-now-generally-available/) era una versione GA che lo ha portato funzionalità che sono state in precedenza nell'anteprima al canale GA.
- Nella versione [1808](https://aka.ms/WACPreview1808-InsiderBlog) è stato aggiunto lo strumento delle app installate, numerosi miglioramenti e importanti aggiornamenti all'anteprima SDK.
- Versione [1807](https://aka.ms/WACPreview1807-InsiderBlog) aggiunto un semplificata Azure esperienza, miglioramenti alla pagina di inventario della macchina virtuale di connessione, file funzionalità condivisione, integrazione con gestione aggiornamenti Azure e altro ancora. 
- Versione [1806](https://aka.ms/WACPreview1806-InsiderBlog) aggiunto mostrano script PowerShell, gestione SDN, le connessioni di 2008 R2, SDN, attività pianificate e molti altri miglioramenti.
- Versione 1804.25 - aggiornamento di manutenzione per supportare gli utenti che installano Windows Admin Center in ambienti completamente offline.
- Versione [1804](https://cloudblogs.microsoft.com/windowsserver/2018/04/12/announcing-windows-admin-center-our-reimagined-management-experience/) - Project Honolulu diventa Windows Admin Center e aggiunge funzionalità di sicurezza e controllo dell'accesso basato sui ruoli. La prima versione GA.
- La versione [1803](https://blogs.windows.com/windowsexperience/2018/03/13/announcing-project-honolulu-technical-preview-1803-and-rsat-insider-preview-for-windows-10) ha aggiunto il supporto per il controllo dell'accesso di Azure AD, registrazione dettagliata, contenuto ridimensionabile e una serie di miglioramenti agli strumenti.
- La versione [1802](https://blogs.windows.com/windowsexperience/2018/02/13/announcing-windows-server-insider-preview-build-17093-project-honolulu-technical-preview-1802) ha aggiunto il supporto per accessibilità, localizzazione, distribuzioni a disponibilità elevata, tagging, impostazioni host Hyper-V e autenticazione gateway.
- La versione [1712](https://blogs.windows.com/windowsexperience/2017/12/19/announcing-project-honolulu-technical-preview-1712-build-05002) ha aggiunto altre funzionalità per le macchine virtuali e miglioramenti delle prestazioni per tutti gli strumenti.
- La versione [1711](https://cloudblogs.microsoft.com/windowsserver/2017/12/01/1711-update-to-project-honolulu-technical-preview-is-now-available/) ha aggiunto strumenti a lungo attesi (Desktop remoto e PowerShell) e altri miglioramenti.
- La versione [1709](https://cloudblogs.microsoft.com/windowsserver/2017/09/22/project-honolulu-technical-preview-is-now-available-for-download/) è stata lanciata come prima versione di anteprima pubblica.

## Mantieniti aggiornato

<a target="_blank" class="mscom-link twitter-follow-link" title="Seguici su Twitter" aria-label="Follow us on Twitter" data-info="Twitter" href="https://twitter.com/servermgmt"><picture><source srcset="//img-prod-cms-rt-microsoft-com.akamaized.net/cms/api/am/imageFileData/REOolR" media="(min-width:0)"><img srcset="//img-prod-cms-rt-microsoft-com.akamaized.net/cms/api/am/imageFileData/REOolR" alt="Follow us on Twitter" src="//img-prod-cms-rt-microsoft-com.akamaized.net/cms/api/am/imageFileData/REOolR"></picture></a>
 | 
<a target="_blank" class="mscom-link blogs-follow-link" title="Leggi il blog" aria-label="Visit our Blogs" data-info="Blogs" href="https://blogs.technet.microsoft.com/servermanagement/"><picture><source srcset="//img-prod-cms-rt-microsoft-com.akamaized.net/cms/api/am/imageFileData/REOtyw" media="(min-width:0)"><img srcset="//img-prod-cms-rt-microsoft-com.akamaized.net/cms/api/am/imageFileData/REOtyw" alt="Follow us on Blogs" src="//img-prod-cms-rt-microsoft-com.akamaized.net/cms/api/am/imageFileData/REOtyw"></picture></a>
