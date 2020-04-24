---
title: Management
description: Informazioni su strumenti, consigli e indicazioni per la gestione di Windows Server
ms.prod: windows-server
layout: LandingPage
ms.technology: manage
ms.topic: landing-page
author: lizap
ms.author: elizapo
ms.localizationpriority: high
ms.openlocfilehash: 4166d4e8d2819946cdc859ef643cf53315e7290a
ms.sourcegitcommit: 3a3d62f938322849f81ee9ec01186b3e7ab90fe0
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 04/23/2020
ms.locfileid: "71370424"
---
# <a name="management"></a>Management


>[!TIP]
> Per informazioni sulle versioni precedenti di Windows Server, vedere le altre [librerie di Windows Server](/previous-versions/windows/) in docs.microsoft.com. È anche possibile cercare informazioni specifiche [in questo sito](https://docs.microsoft.com/search/index?search=Windows+Server&dataSource=previousVersions).

<hr />

Dopo la distribuzione di Windows Server nell'ambiente in uso, compresi i ruoli specifici delle funzioni necessarie, sarà necessario affrontare le modalità di gestione dei server. Windows Server include una serie di strumenti che consentono di comprendere l'ambiente Windows Server in uso, gestire server specifici, ottimizzare le prestazioni e automatizzare molte attività di gestione. 

Gli strumenti usati per gestire le istanze di Windows Server dipendono, in gran parte, dai tipi di sistemi distribuiti (Windows Server con Esperienza desktop o Server Core), dall'uso di macchine virtuali o computer fisici e dalla posizione in cui si trovano i server. Usa le informazioni seguenti per eseguire attività di gestione di base su Windows Server.

Usa la tabella seguente per determinare quali strumenti usare in contesti specifici.

| Contesto   | Installare ed eseguire Windows Admin Center | Eseguire Server Manager su Windows Server | Eseguire Server Manager in Strumenti di amministrazione remota del server su Windows 10 |
|--------|----------------------|--------------------------------------|------------------------------------------|
| Davanti a un PC Windows 10 | X  |                                      | X                                        |
| Davanti a un sistema Windows Server che esegue Esperienza desktop | X | X | X |
| Davanti a un sistema Windows Server che esegue Server Core |X (installare su Windows 10, usare per gestire Server Core) | | X |
| Lontano dal sistema Windows Server |X | | X |
| Lontano dal sistema Windows Server, ma CON Esperienza desktop |X | Usare Servizi Desktop remoto per connettersi in remoto al server e quindi usare Server Manager | X |

Oltre agli strumenti indicati di seguito, puoi usare anche [Servizi Desktop remoto](../remote/remote-desktop-services/welcome-to-rds.md) per accedere a server locali, remoti e virtuali. Puoi quindi usare Server Manager per eseguire attività di gestione.

<HR />

<ul class="cardsI panelContent">
<li>
        <div class="cardSize">
            <div class="cardPadding">
                <div class="card">
                    <div class="cardImageOuter">
                        <div class="cardImage">
                            <img src="../media/i-manage.svg" alt="" />
                        </div>
                    </div>
                    <div class="cardText">
                    <h3>Gestire gli ambienti e i sistemi Windows Server</h3>
<HR />
                        <p><h3><a href="../manage/windows-admin-center/overview.md">Gestire sistemi locali, sistemi remoti e sistemi senza interfaccia utente con Windows Admin Center</a></h3>Un'app di gestione basata su browser che consente l'amministrazione in locale di Windows Server senza dipendenze Azure o cloud. Windows Admin Center (in precedenza denominato "Project Honolulu") offre il controllo completo su tutti gli aspetti dell'infrastruttura server ed è particolarmente utile per la gestione di reti private non connesse a Internet. Puoi installare Windows Admin Center in Windows 10, in un server gateway o direttamente nel sistema Windows Server che vuoi gestire.</p>
<HR />
                        <p><h3><a href="server-manager/server-manager.md">Gestire sistemi locali con Server Manager</a></h3>Una console di gestione inclusa nell'installazione completa di Windows Server. Non è disponibile per le installazioni che non prevedono l'interfaccia utente: Server Core non include Server Manager. Usa Server Manager per installare e rimuovere ruoli del server, aggiungere e rimuovere server remoti, avviare e arrestare servizi e visualizzare i dati raccolti relativi al tuo ambiente.</p>
<HR />
                        <p><h3><a href="../remote/remote-server-administration-tools.md">Gestire sistemi remoti e sistemi senza interfaccia utente con Strumenti di amministrazione remota del server</a></h3>Se l'ambiente include installazioni di Server Core o server remoti (locali o macchine virtuali), per la loro gestione puoi usare Strumenti di amministrazione remota del server. Strumenti di amministrazione remota del server include Server Manager e può essere quindi usato per gestire tutti i server. Strumenti di amministrazione remota del server viene eseguito in Windows 10 e non può essere installato in Windows Server Core. Puoi gestire le installazioni Server Core anche dalla riga di comando. Vedi <a href="server-core/server-core-administer.md">Attività di amministrazione di base in Server Core</a>
<HR />
                        <p><h3><a href="windows-server-update-services/get-started/windows-server-update-services-wsus.md">Gestire gli aggiornamenti nei sistemi Windows Server</a></h3>Usa Windows Server Update Services (WSUS) per gestire e distribuire gli aggiornamenti dei sistemi nell'ambiente Windows Server.</p>
                    </div>
                </div>
            </div>
        </div>
    </li>
<li>
        <div class="cardSize">
            <div class="cardPadding">
                <div class="card">
                    <div class="cardImageOuter">
                        <div class="cardImage">
                            <img src="../media/i-manage.svg" alt="" />
                        </div>
                    </div>
                    <div class="cardText">
                    <h3>Raccogliere informazioni sull'ambiente</h3>
<HR />
                        <p><h3><a href="get-started-with-setup-and-boot-event-collection.md">Raccolta eventi installazione e avvio</a></h3>Raccolta eventi installazione e avvio consente di designare un computer di "raccolta" in grado di raccogliere un'ampia gamma di eventi importanti che si verificano su altri computer quando vengono avviati o sottoposti al processo di installazione. Quindi è possibile analizzare gli eventi raccolti con i cmdlet di Windows PowerShell, Message Analyzer, Wevtutil o il Visualizzatore eventi in un secondo momento. </p>
<HR />
                        <p><h3><a href="software-inventory-logging/get-started-with-software-inventory-logging.md">Registrazione inventario software (SIL)</a></h3>Registrazione inventario software in Windows Server è una funzionalità costituita da una semplice serie di cmdlet PowerShell che consentono agli amministratori di server di recuperare un elenco del software Microsoft installato nei server. Offre anche la capacità di raccogliere e inoltrare periodicamente per l'aggregazione questi dati attraverso la rete a un server Web di destinazione usando il protocollo HTTPS. Con i comandi di PowerShell viene eseguita anche la gestione della funzionalità, principalmente per la raccolta oraria dei dati e per l'inoltro.</p>
<HR />
                        <p><h3><a href="user-access-logging/get-started-with-user-access-logging.md">Registrazione accesso utenti</a></h3>Registrazione accesso utenti aggrega gli eventi univoci relativi alle richieste dei dispositivi e degli utenti client registrati su un computer che esegue Windows Server 2016, Windows Server 2012 R2 o Windows Server 2012 in un database locale. Tali record vengono quindi resi disponibili (con una query eseguita da un amministratore del server) per consentire il recupero delle quantità e delle istanze in base al ruolo del server, all'utente, al dispositivo, al server locale e alla data. Registrazione accesso utenti consente anche agli sviluppatori non Microsoft di instrumentare gli eventi di Registrazione accesso utenti per l'aggregazione. </a>
                    </div>
                </div>
            </div>
        </div>
    </li>
<li>
        <div class="cardSize">
            <div class="cardPadding">
                <div class="card">
                    <div class="cardImageOuter">
                        <div class="cardImage">
                            <img src="../media/i-manage.svg" alt="" />
                        </div>
                    </div>
                    <div class="cardText">
                    <h3>Ottimizzare le prestazioni dell'ambiente Windows Server</h3>
<HR />
                        <p><h3><a href="performance-tuning/index.md">Linee guida di ottimizzazione delle prestazioni</a></h3>Raccolta di linee guida che puoi usare per ottimizzare le impostazioni del server in Windows Server e ottenere un miglioramento incrementale delle prestazioni o una maggiore efficienza energetica, soprattutto quando la natura del carico di lavoro varia leggermente nel tempo.</p>
<HR />
                        <p><h3><a href="server-performance-advisor/microsoft-server-performance-advisor.md">Microsoft Server Performance Advisor</a></h3>Con Microsoft Server Performance Advisor (SPA), puoi raccogliere le metriche per diagnosticare i problemi di prestazioni nelle istanze di Windows Server in modo discreto, senza aggiungere agenti software né riconfigurare i server di produzione. Microsoft Server Performance Advisor genera report di prestazioni completi e grafici di andamento storico con suggerimenti.</p>
                    </div>
                </div>
            </div>
        </div>
    </li>
<li>
        <div class="cardSize">
            <div class="cardPadding">
                <div class="card">
                    <div class="cardImageOuter">
                        <div class="cardImage">
                            <img src="../media/i-manage.svg" alt="" />
                        </div>
                    </div>
                    <div class="cardText">
                    <h3>Automatizzare la gestione di Windows Server</h3>
<HR />
                        <p><h3><a href="https://docs.microsoft.com/powershell/scripting/powershell-scripting?view=powershell-5.1">Windows PowerShell</a></h3>Windows PowerShell è una shell da riga di comando e un linguaggio di scripting progettato per consentirti di automatizzare rapidamente le attività amministrative. </p>
<HR />
                        <p><h3><a href="windows-commands/windows-commands.md">Comandi di Windows</a></h3>Gli strumenti da riga di comando di Windows vengono usati per eseguire attività amministrative in Windows. Puoi usare la guida di riferimento ai comandi per acquisire familiarità con gli strumenti da riga di comando, ottenere informazioni sulla shell dei comandi e automatizzare le attività da riga di comando usando file batch o strumenti di scripting.</p>
                    </div>
                </div>
            </div>
        </div>
    </li>
<li>
        <div class="cardSize">
            <div class="cardPadding">
                <div class="card">
                    <div class="cardImageOuter">
                        <div class="cardImage">
                            <img src="../media/i-manage.svg" alt="" />
                        </div>
                    </div>
                    <div class="cardText">
                    <h3>Automatizzare la gestione di Windows Server</h3>
<HR />
                        <p><h3><a href="..\manage\system-insights\overview.md">Informazioni dettagliate di sistema</h3></a>L'analisi predittiva nativa analizza in locale i dati di sistema di Windows Server, come i contatori delle prestazioni e gli eventi ETW, consentendo agli amministratori IT di rilevare e risolvere problemi di funzionamento in modo proattivo nelle distribuzioni.</p>
                    </div>
                </div>
            </div>
        </div>
    </li>
</ul>