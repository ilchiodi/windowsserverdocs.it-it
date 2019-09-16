---
title: Panoramica di Windows Admin Center
description: Scopri come gestire Windows Server con Windows Admin Center (Project Honolulu)
ms.technology: manage
ms.topic: article
author: nwashburn-ms
ms.author: niwashbu
ms.date: 08/22/2019
ms.localizationpriority: high
ms.prod: windows-server-threshold
ms.openlocfilehash: cdd7986486fd3cad07f5e4577aaf0ab404bfb5d3
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 09/10/2019
ms.locfileid: "70869591"
---
# <a name="windows-admin-center"></a>Windows Admin Center

> Si applica a: Windows Admin Center, Windows Admin Center Preview

**Windows Admin Center** (in precedenza noto con il nome in codice **Project Honolulu**) è un'evoluzione degli strumenti di gestione predefiniti di Windows Server. Si tratta di un'unica interfaccia che consolida tutti gli aspetti della gestione locale e remota dei server. In quanto esperienza di gestione basata su browser e distribuita localmente, non sono necessari una connessione Internet e Azure. Windows Admin Center ti offre il controllo completo di tutti gli aspetti della distribuzione, incluse le reti private non connesse a Internet.

## <a name="introduction"></a>Introduzione

>[!VIDEO https://www.youtube.com/embed/PcQj6ZklmK0]

![Infografica di Windows Admin Center](media/WAC1809Poster_thumb.PNG)

[Scarica il PDF](https://github.com/MicrosoftDocs/windowsserverdocs/raw/master/WindowsServerDocs/manage/windows-admin-center/media/WindowsAdminCenter1809Poster.pdf)

## <a name="quick-start"></a>Avvio rapido

Puoi rendere operativo Windows Admin Center nel tuo ambiente in pochi minuti:

1. [Download](https://aka.ms/windowsadmincenter)
2. [Installa](deploy/install.md)
3. [Per iniziare](use/get-started.md)

## <a name="contents-at-a-glance"></a>Contenuto in breve

<table>
    <tr></tr>
    <tr>
        <td style="vertical-align: top;">
            <h3>Nozioni di base</h3>
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
            <br>
            </ul>
        </td>
    </tr>
    <tr>
        <td style="vertical-align: top;">
            <h3>Distribuisci</h3>
            <ul>
            <li><a href="deploy/prepare-environment.md">Preparare l'ambiente</a>
            <li><a href="deploy/install.md">Installare Windows Admin Center</a>
            <li><a href="deploy/high-availability.md">Abilitare la disponibilità elevata</a>
         </ul>
        </td>
        <td style="vertical-align: top;">
            <h3>Configura</h3>
            <ul>
            <li><a href="configure/settings.md">Impostazioni di Windows Admin Center</a>
            <li><a href="configure/user-access-control.md">Autorizzazioni e controllo dell'accesso utente</a>
            <li><a href="configure/shared-connections.md">Connessioni condivise</a>
            <li><a href="configure/using-extensions.md">Estensioni</a>
            </ul>
        </td>
    </tr>
    <tr>
        <td style="vertical-align: top;">
            <h3>Uso</h3>
            <ul>
            <li><a href="use/get-started.md">Avviare e aggiungere connessioni</a>
            <li><a href="use/manage-servers.md">Gestire i server</a>
            <li><a href="use/manage-hyper-converged.md">Gestire l'infrastruttura iperconvergente</a>
            <li><a href="use/manage-failover-clusters.md">Gestire i cluster di failover</a>
            <li><a href="use/manage-virtual-machines.md">Gestire le macchine virtuali</a>
            <li><a href="use/logging.md">Registrazione</a>
            </ul>
        </td>
        <td style="vertical-align: top;">
            <h3>Connettiti ad Azure</h3>
            <ul>
            <li><a href="azure/index.md">Servizi ibridi di Azure</a></li>
            <li><a href="azure/azure-integration.md">Connettere Windows Admin Center ad Azure</a></li>
            <li><a href="azure/deploy-wac-in-azure.md">Distribuire Windows Admin Center in Azure</a></li>
            <li><a href="azure/manage-azure-vms.md">Gestire macchine virtuali di Azure con Windows Admin Center</a></li>
            </ul>
        </td>
    </tr>
    <tr>
            <td style="vertical-align: top;">
            <h3>Supporto</h3>
            <ul>
            <li><a href="support/index.md">Criteri di supporto</a>
            <li><a href="support/troubleshooting.md">Passaggi di risoluzione dei problemi comuni</a>
            <li><a href="support/known-issues.md">Problemi noti</a>
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

## <a name="release-history"></a>Cronologia delle versioni

Informazioni sulle ultime funzionalità rilasciate:

- Versione [1908](https://aka.ms/wac1908) : include aggiornamenti visivi, Packetmon, FlowLog Audit, onboarding di Monitoraggio di Azure per i cluster e supporto di WinRM su HTTPS (porta 5986).
- Versione [1907](https://aka.ms/wac1907): aggiunta di collegamenti per la stima dei costi di Azure e miglioramenti apportati alle attività di importazione/esportazione e aggiunta di tag per le macchine virtuali.
- Versione [1906](https://aka.ms/wac1906): aggiunta di importazione/esportazione di macchine virtuali, cambiare account di Azure, aggiungere connessioni da Azure, sperimentare le impostazioni di connettività, miglioramenti delle prestazioni e strumento di profilatura delle prestazioni.
- La versione 1904.1 è la versione disponibile a livello generale più recente: un aggiornamento di manutenzione per migliorare la stabilità dei plug-in gateway.
- La versione [1904](https://aka.ms/wac1904) era una versione disponibile a livello generale in cui è stato introdotto lo strumento dei servizi ibridi di Azure e sono state incluse funzionalità precedentemente in anteprima per il canale disponibile a livello generale.
- Versione [1903](https://aka.ms/wac1903): aggiunta delle notifiche tramite posta elettronica da Monitoraggio di Azure, della possibilità di aggiungere connessioni server o PC da Active Directory e di nuovi strumenti per gestire Active Directory, DHCP e DNS.
- Versione [1902](https://aka.ms/wac1902): aggiunta di un elenco di connessioni condivise e miglioramenti per la gestione di reti software-defined (SDN), inclusi nuovi strumenti SDN per gestire gli ACL, le connessioni gateway e le reti logiche.
- Versione [1812](https://aka.ms/wac1812): aggiunta del tema scuro (in anteprima), impostazioni di configurazione di risparmio energia, info BMC e supporto di PowerShell per gestire [estensioni](./configure/using-extensions.md#manage-extensions-with-powershell) e [connessioni](./use/get-started.md#use-powershell-to-import-or-export-your-connections-with-tags).
- La versione [1809.5](https://aka.ms/wac1809.5) era un aggiornamento cumulativo disponibile a livello generale che includeva vari miglioramenti qualitativi e funzionali, correzioni di bug in tutta la piattaforma e alcune nuove funzionalità nella soluzione di gestione dell'infrastruttura iperconvergente.
- La versione [1809](https://cloudblogs.microsoft.com/windowsserver/2018/09/20/windows-admin-center-1809-and-sdk-now-generally-available/) era una versione disponibile a livello generale in cui sono state introdotte funzionalità precedentemente in anteprima per il canale di disponibilità a livello generale.
- Versione [1808](https://aka.ms/WACPreview1808-InsiderBlog): aggiunta dello strumento App installate, numerosi miglioramenti dietro le quinte e importanti aggiornamenti all'SDK in anteprima.
- Versione [1807](https://aka.ms/WACPreview1807-InsiderBlog): aggiunta di un'esperienza semplificata per la connessione ad Azure, miglioramenti alla pagina di inventario della macchina virtuale, funzionalità di condivisione file, integrazione della gestione degli aggiornamenti di Azure e altro ancora. 
- Versione [1806](https://aka.ms/WACPreview1806-InsiderBlog): aggiunta dello script di PowerShell show, gestione SDN, connessioni 2008 R2, SDN, attività pianificate e molti altri miglioramenti.
- Versione 1804.25: aggiornamento di manutenzione per supportare gli utenti che installano Windows Admin Center in ambienti completamente offline.
- Versione [1804](https://cloudblogs.microsoft.com/windowsserver/2018/04/12/announcing-windows-admin-center-our-reimagined-management-experience/): Project Honolulu diventa Windows Admin Center e aggiunge funzionalità di sicurezza e controllo degli accessi in base al ruolo. Prima versione disponibile a livello generale.
- Versione [1803](https://blogs.windows.com/windowsexperience/2018/03/13/announcing-project-honolulu-technical-preview-1803-and-rsat-insider-preview-for-windows-10): aggiunta del supporto per il controllo dell'accesso di Azure AD, registrazione dettagliata, contenuto ridimensionabile e una serie di miglioramenti agli strumenti.
- Versione [1802](https://blogs.windows.com/windowsexperience/2018/02/13/announcing-windows-server-insider-preview-build-17093-project-honolulu-technical-preview-1802): aggiunta del supporto per accessibilità, localizzazione, distribuzioni a disponibilità elevata, aggiunta di tag, impostazioni host Hyper-V e autenticazione gateway.
- Versione [1712](https://blogs.windows.com/windowsexperience/2017/12/19/announcing-project-honolulu-technical-preview-1712-build-05002): aggiunta di altre funzionalità per le macchine virtuali e miglioramenti delle prestazioni per tutti gli strumenti.
- Versione [1711](https://cloudblogs.microsoft.com/windowsserver/2017/12/01/1711-update-to-project-honolulu-technical-preview-is-now-available/): aggiunta di strumenti a lungo attesi (Desktop remoto e PowerShell) e altri miglioramenti.
- La versione [1709](https://cloudblogs.microsoft.com/windowsserver/2017/09/22/project-honolulu-technical-preview-is-now-available-for-download/) è stata lanciata come prima versione in anteprima pubblica.

## <a name="stay-updated"></a>Resta sempre aggiornato

![ ](//img-prod-cms-rt-microsoft-com.akamaized.net/cms/api/am/imageFileData/REOolR)[Seguici su Twitter](https://twitter.com/servermgmt)

![ ](//img-prod-cms-rt-microsoft-com.akamaized.net/cms/api/am/imageFileData/REOtyw)[Leggi i blog Microsoft](https://blogs.technet.microsoft.com/servermanagement/)
