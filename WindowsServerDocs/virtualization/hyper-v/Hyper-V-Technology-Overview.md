---
title: Panoramica della tecnologia Hyper-V
description: Viene descritto il funzionamento di Hyper-V, come ottenerlo, le funzionalità chiave e gli usi comuni.
ms.prod: windows-server
manager: dongill
ms.technology: compute-hyper-v
ms.topic: article
ms.assetid: ac069fed-7bf5-4cc3-aff5-25a2766040b8
author: kbdazure
ms.author: kathydav
ms.date: 11/29/2016
ms.openlocfilehash: d21bec24a22607213771bdad0b48df18fd88eb4d
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80853244"
---
# <a name="hyper-v-technology-overview"></a>Panoramica della tecnologia Hyper-V

>Si applica a: Windows Server 2016, Microsoft Hyper-V Server 2016, Windows Server 2019 Microsoft Hyper-V Server 2019

Hyper-V è prodotto di virtualizzazione hardware Microsoft. Consente di creare ed eseguire una versione del software di un computer, denominato un *macchina virtuale*. Ogni macchina virtuale funziona come un computer completo, che esegue un sistema operativo e i programmi. Quando è necessario che le risorse di elaborazione, le macchine virtuali consentono una maggiore flessibilità, consentono di risparmiare tempo e denaro e sono un modo più efficiente di utilizzare hardware più semplice l'esecuzione di un sistema operativo su hardware fisico.

Hyper-V viene eseguito ogni macchina virtuale nel proprio spazio isolato, ovvero che è possibile eseguire più macchine virtuali nello stesso hardware nello stesso momento. È possibile effettuare questa operazione per evitare problemi, ad esempio un arresto anomalo influenzare altri carichi di lavoro o per concedere l'accesso di utenti, gruppi o servizi diversi per diversi sistemi.

## <a name="some-ways-hyper-v-can-help-you"></a>Alcuni metodi Hyper-V consente di

Hyper-V consentono di:

- **Definire o espandere un ambiente cloud privato.** Fornire servizi IT più flessibili e su richiesta da verso o espande l'utilizzo delle risorse condivise e regolare l'utilizzo come modifiche di richiesta.

- **Utilizzare l'hardware in modo più efficace.** Consolidare server e carichi di lavoro in computer fisici più potenti e meno di utilizzare meno energia e spazio fisico.

- **Migliorare la continuità aziendale.** Ridurre al minimo l'impatto del tempo di inattività sia pianificati e dei carichi di lavoro.

- **Definire o espandere un'infrastruttura VDI (Virtual Desktop Infrastructure).** Utilizzare una strategia desktop centralizzata con VDI consentono di aumentare la flessibilità aziendale e la sicurezza dei dati, nonché semplificando la conformità alle normative e la gestione di sistemi operativi desktop e applicazioni. Distribuire Hyper-V e Host di virtualizzazione Desktop remoto (host di virtualizzazione Desktop remoto) nello stesso server per rendere disponibili agli utenti i desktop virtuali personali o i pool di desktop virtuali.

- **Rendere più efficienti lo sviluppo e il test.** Riprodurre ambienti di elaborazione diversi senza dover acquistare o gestire tutto l'hardware che è necessario se si utilizza solo sistemi fisici.

## <a name="hyper-v-and-other-virtualization-products"></a>Hyper-V e altri prodotti di virtualizzazione

Hyper-V in Windows e Windows Server sostituisce prodotti di virtualizzazione hardware precedenti, ad esempio Microsoft Virtual PC, Microsoft Virtual Server e Windows Virtual PC. Hyper-V offre funzionalità di rete, prestazioni, archiviazione e sicurezza non disponibili in questi prodotti meno recenti.

Hyper-V e la maggior parte delle applicazioni di virtualizzazione di terze parti che richiedono le stesse funzionalità del processore non sono compatibili. Ciò è dovuto al fatto che le funzionalità del processore, note come estensioni di virtualizzazione hardware, sono progettate per non essere condivise. Per informazioni dettagliate, vedere [le applicazioni di virtualizzazione non funzionano insieme a Hyper-V, Device Guard e Credential Guard](https://support.microsoft.com/kb/3204980).

## <a name="what-features-does-hyper-v-have"></a>Quali funzionalità dispone di Hyper-V?

Hyper-V offre numerose funzionalità. Ecco una panoramica, raggruppata per ciò che forniscono le funzionalità o consentono di eseguire.

**Ambiente di elaborazione** -macchina virtuale Hyper-V A include le stesse parti di base come un computer fisico, ad esempio memoria, processore, archiviazione e rete. Tutte queste parti sono incluse funzionalità e opzioni che è possibile configurare diversi modi per soddisfare esigenze diverse. Archiviazione e rete può ogni essere considerati categorie autonomamente, a causa delle diverse modalità è possibile configurarle.

**Backup e ripristino di emergenza** -per il ripristino di emergenza, la Replica Hyper-V consente di creare copie delle macchine virtuali, deve essere archiviati in un'altra posizione fisica, pertanto è possibile ripristinare la macchina virtuale dalla copia. Per il backup, Hyper-V offre due tipi. Uno utilizza gli stati salvati e l'altro utilizza servizio Copia Shadow del Volume (VSS), pertanto è possibile effettuare backup coerenti con l'applicazione per i programmi che supportano VSS.

**Ottimizzazione** -ogni sistema operativo guest supportato è un set personalizzato di servizi e driver, chiamato *servizi di integrazione*, che rendono più semplice utilizzare il sistema operativo in una macchina virtuale Hyper-V.

**Portabilità** : caratteristiche, ad esempio live migration, migrazione dell'archiviazione e l'importazione/esportazione rendono più semplice spostare o distribuire una macchina virtuale.

**Connettività remota** -Hyper-V include Virtual Machine Connection, uno strumento di connessione remota per l'utilizzo con Windows e Linux. A differenza di Desktop remoto, in questo modo lo strumento console accesso, è così possibile visualizzare ciò che avviene nel guest anche quando il sistema operativo non è stato ancora avviato.

**Sicurezza** -avvio protetto e macchine virtuali schermate proteggersi da malware e altri accessi non autorizzati per una macchina virtuale e i relativi dati.

Per un riepilogo delle funzionalità introdotte in questa versione, vedere Novità di [Hyper-V in Windows Server](What-s-new-in-Hyper-V-on-Windows.md). Alcune funzionalità o parti hanno un limite per il numero può essere configurato. Per informazioni dettagliate, vedere [pianificare la scalabilità di Hyper-V in Windows Server 2016](plan/Plan-for-Hyper-V-scalability-in-Windows-Server-2016.md).

## <a name="how-to-get-hyper-v"></a>Procedura: ottenere Hyper-V

Hyper-V è disponibile in Windows Server e Windows, come un ruolo di server disponibile per x64 versioni di Windows Server. Per istruzioni di server, vedere [installare il ruolo Hyper-V in Windows Server](get-started/Install-the-Hyper-V-role-on-Windows-Server.md). In Windows, è disponibile come [funzionalità](https://docs.microsoft.com/virtualization/hyper-v-on-windows/index) in alcune versioni a 64 bit di Windows. È inoltre disponibile come prodotto scaricabile, server autonomo, [Microsoft Hyper-V Server](https://www.microsoft.com/evalcenter/evaluate-hyper-v-server-2019).

## <a name="supported-operating-systems"></a>Sistemi operativi supportati

Molti sistemi operativi vengono eseguiti su macchine virtuali. In generale, un sistema operativo che utilizza un x86 architettura verrà eseguito in una macchina virtuale Hyper-V. Non tutti i sistemi operativi che possono essere eseguiti sono testati e supportati da Microsoft, tuttavia. Per gli elenchi di elementi supportati, vedere:

- [Macchine virtuali Linux e FreeBSD supportate per Hyper-V in Windows](Supported-Linux-and-FreeBSD-virtual-machines-for-Hyper-V-on-Windows.md)

- [Sistemi operativi guest Windows supportati per Hyper-V in Windows Server](Supported-Windows-guest-operating-systems-for-Hyper-V-on-Windows.md)

## <a name="how-hyper-v-works"></a>Funzionamento di Hyper-V

Hyper-V è una tecnologia di virtualizzazione basata su hypervisor. Hyper-V utilizza Windows hypervisor, che richiede un processore fisico con funzionalità specifiche. Per informazioni dettagliate sull'hardware, vedere [requisiti di sistema per Hyper-V in Windows Server](System-requirements-for-Hyper-V-on-Windows.md).

Nella maggior parte dei casi, l'hypervisor gestisce le interazioni tra l'hardware e le macchine virtuali. L'accesso controllato hypervisor all'hardware offre le macchine virtuali all'ambiente di isolamento in cui vengono eseguiti. In alcune configurazioni, una macchina virtuale o il sistema operativo in esecuzione nella macchina virtuale ha accesso diretto all'hardware di archiviazione, rete o grafica.

## <a name="what-does-hyper-v-consist-of"></a>Cosa Hyper-V sono costituiti da?

Hyper-V ha richiesto le parti che interagiscono in modo è possibile creare ed eseguire macchine virtuali. Insieme, queste parti costituiscono la piattaforma di virtualizzazione. Siano installate come un insieme quando si installa il ruolo Hyper-V. Le parti necessarie includono Windows hypervisor, servizio Virtual Machine Management Hyper-V, il provider WMI di virtualizzazione, il bus macchina virtuale (VMbus), di provider di servizi di virtualizzazione (VSP) e infrastruttura virtuale driver (VID).

Hyper-V include anche gli strumenti per la gestione e la connettività. È possibile installarli nello stesso computer che il ruolo Hyper-V sia installato su e su computer senza il ruolo Hyper-V installato. Questi strumenti sono:

- Console di gestione di Hyper-V
- [Modulo Hyper-V per Windows PowerShell](https://docs.microsoft.com/powershell/module/hyper-v/index)
- La [connessione alla macchina virtuale](https://docs.microsoft.com/windows-server/virtualization/hyper-v/learn-more/hyper-v-virtual-machine-connect) \(talvolta denominata VMConnect\)
- [Windows PowerShell Direct](manage/Manage-Windows-virtual-machines-with-PowerShell-Direct.md)

## <a name="related-technologies"></a>Tecnologie correlate

Queste sono alcune tecnologie di Microsoft che vengono spesso utilizzati con Hyper-v:

- [Clustering di failover](../../failover-clustering/whats-new-in-failover-clustering.md)
- [Servizi Desktop remoto](../../remote/remote-desktop-services/Host-desktops-and-apps-in-Remote-Desktop-Services.md)
- [System Center Virtual Machine Manager](https://docs.microsoft.com/system-center/vmm/overview)

Varie tecnologie di archiviazione: spazio di archiviazione SMB 3.0, volumi condivisi cluster diretti

Contenitori di Windows offrono un altro approccio per la virtualizzazione. Vedere il [contenitori Windows](https://docs.microsoft.com/virtualization/windowscontainers/index) in MSDN library.
