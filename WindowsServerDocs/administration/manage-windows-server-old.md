---
title: Gestire Windows Server
description: Informazioni su strumenti, consigli e indicazioni per la gestione di Windows Server
ms.prod: windows-server-threshold
ms.technology: manage
ms.topic: article
author: lizap
ms.author: elizapo
ms.date: 03/16/2018
ms.localizationpriority: medium
ms.openlocfilehash: 4faadb811927626c26a5b01e2ce0598d40792b68
ms.sourcegitcommit: 4893d79345cea85db427224bb106fc1bf88ffdbc
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/05/2018
ms.locfileid: "6066935"
---
# Gestire Windows Server

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

>[!TIP]
> Cerchi informazioni sull'installazione delle versioni precedenti di Windows Server? Vedi le nostre altre [librerie di Windows Server](/previous-versions/windows/) in docs.microsoft.com. Puoi anche [cercare informazioni specifiche nel sito](https://docs.microsoft.com/search/index?search=Windows+Server&dataSource=previousVersions).

 <ul class="cardse panelContent cols cols3">
    <li>
        <a href="https://docs.microsoft.com/windows-insider/at-work-pro/wip-4-biz-feedback-hub">
        <div class="cardSize">
            <div class="cardPadding">
                <div class="card">
                    <div class="cardImageOuter">
                        <div class="cardImage">
                            <img src="../media/i-manage.svg" alt="" />
                        </div>
                    </div>
                    <div class="cardText">
                        <h2>Gestire</h2>
                <p>Dopo che hai distribuito Windows Server nell'ambiente in uso, compresi i ruoli specifici per le funzioni di cui hai bisogno, dovrai proseguire con la gestione di tali server. Windows Server include una serie di strumenti che ti consentono di comprendere l'ambiente Windows Server in uso, gestire server specifici, ottimizzare le prestazioni e automatizzare molte attività di gestione. </p>
                    </div>
                </div>
            </div>
        </div>
        </a>
    </li> 
</ul> 

## Gestire gli ambienti e i sistemi Windows Server
Gli strumenti usati per gestire le istanze di Windows Server dipendono, in gran parte, dai tipi di sistemi distribuiti (Windows Server con Esperienza desktop o Server Core), dall'uso di macchine virtuali o computer fisici e dalla posizione in cui si trovano i server. Utilizza le seguenti informazioni per eseguire attività di gestione di base su Windows Server.

Utilizza la tabella seguente per determinare quali strumenti usare in contesti specifici.

| Contesto   | Installare ed eseguire Windows Admin Center | Eseguire Server Manager su Windows Server | Eseguire Server Manager in Strumenti di amministrazione remota del server su Windows 10 |
|--------|----------------------|--------------------------------------|------------------------------------------|
| Davanti a un PC Windows 10 | X  |                                      | X                                        |
| Davanti a un sistema Windows Server che esegue Esperienza desktop | X | X | X |
| Davanti a un sistema Windows Server che esegue Server Core |X (installare su Windows 10, utilizzare per gestire Server Core) | | X |
| Lontano dal sistema Windows Server |X | | X |
| Lontano dal sistema Windows Server, ma CON Esperienza desktop |X | Utilizzare Servizi Desktop remoto per connettersi in remoto al server, quindi utilizzare Server Manager | X |

Oltre agli strumenti indicati di seguito, puoi anche utilizzare [Servizi Desktop remoto](../remote/remote-desktop-services/welcome-to-rds.md) per accedere a server locali, remoti e virtuali. Puoi quindi utilizzare Server Manager per eseguire attività di gestione.

### Gestire sistemi locali, sistemi remoti e sistemi senza dell'interfaccia utente con Windows Admin Center
[Windows Admin Center](../manage/windows-admin-center/overview.md) è un'applicazione di gestione basata su browser che consente l'amministrazione in locale di Windows Server senza dipendenze Azure o cloud. Windows Admin Center offre il controllo completo su tutti gli aspetti dell'infrastruttura server ed è particolarmente utile per la gestione delle reti private non connesse a Internet. Puoi installare Windows Admin Center in Windows 10, in un server gateway o direttamente nel sistema di Windows Server che vuoi gestire.

>[!NOTE]
>Windows Admin Center è il nome ufficiale del prodotto noto in precedenza come "Project Honolulu".

### Gestire sistemi locali con Server Manager
[Server Manager](server-manager/server-manager.md) è una console di gestione inclusa nell'installazione completa di Windows Server. Non è disponibile per le installazioni che non dispongono dell'interfaccia utente: Server Core non include Server Manager. Utilizza Server Manager per installare e rimuovere ruoli del server, aggiungere e rimuovere server remoti, avviare e arrestare servizi e visualizzare i dati raccolti relativi al tuo ambiente.

### Gestire sistemi remoti e sistemi senza interfaccia utente con Strumenti di amministrazione remota del server
Se l'ambiente include le installazioni di Server Core o server remoti (macchine virtuali o locali), puoi utilizzare [Strumenti di amministrazione remota del server](../remote/remote-server-administration-tools.md) per gestire tali sistemi. Strumenti di amministrazione remota del server include Server Manager, pertanto puoi utilizzarlo per gestire tutti i server.

> [!IMPORTANT]
> Strumenti di amministrazione remota del server viene eseguito su Windows 10. Non è possibile installarlo su Windows Server Core.

Puoi inoltre gestire le installazioni Server Core dalla riga di comando. Vedi [Attività di amministrazione di base in Server Core](server-core/server-core-administer.md).

### Gestire gli aggiornamenti nei sistemi Windows Server
Puoi utilizzare [Windows Server Update Services (WSUS)](windows-server-update-services/get-started/windows-server-update-services-wsus.md) per gestire gli aggiornamenti e distribuirli ai sistemi nell'ambiente Windows Server.

## Raccogliere informazioni sull'ambiente
Molte delle decisioni che prendi in qualità di amministratore dipendono dai dati relativi ai sistemi e agli utenti nell'ambiente in uso. Utilizza le informazioni e gli strumenti seguenti per raccogliere tali dati.

Inizia con [Configurare i dati di diagnostica di Windows nell'organizzazione](/windows/configuration/configure-windows-diagnostic-data-in-your-organization) per informazioni sui dati diagnostici che possono essere raccolti da Windows 10 e Windows Server.

### [Raccolta eventi di configurazione e avvio](get-started-with-setup-and-boot-event-collection.md)
Raccolta eventi di configurazione e avvio ti consente di designare un computer di "raccolta" in grado di raccogliere un'ampia gamma di eventi importanti che si verificano su altri computer quando vengono avviati o sottoposti al processo di configurazione. Puoi quindi analizzare gli eventi raccolti con i cmdlet di Windows PowerShell, Message Analyzer, Wevtutil o il Visualizzatore eventi in un secondo momento. 

### [Registrazione inventario software (SIL)](software-inventory-logging/get-started-with-software-inventory-logging.md)

Registrazione inventario software in Windows Server è una funzionalità costituita da una semplice serie di cmdlet PowerShell che consentono agli amministratori di server di recuperare un elenco del software Microsoft installato nei server. Offre anche la capacità di raccogliere e inoltrare periodicamente questi dati attraverso la rete a un server Web di destinazione usando il protocollo HTTPS, ai fini dell'aggregazione. Con i comandi di PowerShell viene eseguita anche la gestione della funzionalità, principalmente per la raccolta oraria dei dati e per l'inoltro.

### [Registrazione accesso utenti](user-access-logging/get-started-with-user-access-logging.md)

Registrazione accesso utenti aggrega gli eventi univoci relativi alle richieste dei dispositivi e degli utenti client registrati su un computer che esegue Windows Server 2016, Windows Server 2012 R2 o Windows Server 2012 in un database locale. Tali record vengono quindi resi disponibili (con una query eseguita da un amministratore del server) per consentire il recupero delle quantità e delle istanze in base a ruolo del server, utente, dispositivo, server locale e data. Inoltre, Registrazione accesso utenti consente agli sviluppatori di software non Microsoft di instrumentare gli eventi di Registrazione accesso utenti per l'aggregazione. 

## Ottimizzare le prestazioni dell'ambiente Windows Server
Utilizza le seguenti informazioni per ottimizzare le prestazioni dell'ambiente.

### [Linee guida di ottimizzazione delle prestazioni](performance-tuning/index.md)
Esamina una serie di linee guida utili per ottimizzare le impostazioni del server in Windows Server 2016 e ottenere un miglioramento incrementale in termini di prestazioni ed efficienza energetica, soprattutto quando la natura del carico di lavoro varia leggermente nel tempo.

### [Microsoft Server Performance Advisor](server-performance-advisor/microsoft-server-performance-advisor.md)

Con Microsoft Server Performance Advisor (SPA), puoi raccogliere le metriche per diagnosticare i problemi di prestazioni nelle istanze di Windows Server in modo discreto, senza aggiungere agenti software né riconfigurare i server di produzione. SPA genera report di prestazioni completi e grafici di andamento storico con suggerimenti.


## Automatizzare la gestione di Windows Server

Windows Server include un set di comandi e moduli Windows PowerShell che è possibile utilizzare per automatizzare le attività di gestione.

### [Windows PowerShell](/powershell/scripting/powershell-scripting?view=powershell-5.1)
Windows PowerShell è una shell da riga di comando, nonché un linguaggio di scripting progettato per consentirti di automatizzare rapidamente le attività amministrative. 

### [Comandi di Windows](windows-commands/windows-commands.md)

Gli strumenti da riga di comando di Windows vengono utilizzati per eseguire attività amministrative in Windows. Puoi utilizzare la guida di riferimento ai comandi per acquisire familiarità con gli strumenti da riga di comando, ottenere informazioni sulla shell dei comandi e automatizzare le attività da riga di comando utilizzando file batch o strumenti di scripting.

## Anteprima Windows Server Insider
### [Informazioni dettagliate di sistema](..\manage\system-insights\overview.md)
Informazioni dettagliate di sistema è una nuova funzionalità che introduce analisi predittiva in modo nativo in Windows Server. Tali funzionalità di previsione consentono di analizzare localmente i dati di sistema di Windows Server, ad esempio i contatori delle prestazioni o gli eventi ETW, aiutando in modo proattivo gli amministratori IT a rilevare e risolvere i problemi di funzionamento nelle loro distribuzioni. 
