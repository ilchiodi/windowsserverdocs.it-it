---
title: Panoramica di Azure Stack HCI
description: Azure Stack HCI è un cluster Windows Server 2019 iperconvergente che usa hardware convalidato per eseguire in locale carichi di lavoro virtualizzati collegandosi facoltativamente a servizi di Azure per il backup basato sul cloud, il ripristino del sito e altro ancora. Le soluzioni Azure Stack HCI usano hardware convalidato da Microsoft per garantire prestazioni e affidabilità ottimali e includono il supporto per tecnologie quali le unità NVMe, la memoria persistente e la rete RDMA (Remote-Direct Memory Access).
ms.technology: storage
ms.topic: article
author: jasongerend
ms.author: jgerend
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.date: 05/22/2019
ms.openlocfilehash: 92d600eeb833cd70bd714702b1fd950c4fe2cd87
ms.sourcegitcommit: b190fac4bfa5599751a60d3fc3b4c4a64dd9afd7
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 05/22/2019
ms.locfileid: "66008972"
---
# <a name="azure-stack-hci-overview"></a>Panoramica di Azure Stack HCI

>Si applica a: Windows Server 2019

Azure Stack HCI è un cluster Windows Server 2019 iperconvergente che usa hardware convalidato per eseguire in locale carichi di lavoro virtualizzati collegandosi facoltativamente a servizi di Azure per il backup basato sul cloud, il ripristino del sito e altro ancora. Le soluzioni Azure Stack HCI usano hardware convalidato da Microsoft per garantire prestazioni e affidabilità ottimali e includono il supporto per tecnologie quali le unità NVMe, la memoria persistente e la rete RDMA (Remote-Direct Memory Access).

Azure Stack HCI è una soluzione che combina diversi prodotti:

- Hardware di un partner OEM

- Windows Server 2019 Datacenter Edition

- Windows Admin Center

- Servizi di Azure (facoltativi)

![Azure Stack HCI è la soluzione iperconvergente di Microsoft disponibile presso numerosi partner hardware.](media/AS_HCI_solution.png)

Azure Stack HCI è la soluzione iperconvergente di Microsoft disponibile presso numerosi partner hardware. Valuta gli scenari seguenti relativi a una soluzione iperconvergente per stabilire se Azure Stack HCI è la soluzione più adatta alle tue esigenze:

- **Aggiornamento di hardware obsoleto.** Sostituzione dei server e dell'infrastruttura di archiviazione meno recente ed esecuzione di macchine virtuali Windows e Linux in locale e nella rete perimetrale sfruttando competenze e strumenti IT esistenti.

- **Consolidamento dei carichi di lavoro virtualizzati.** Consolidamento delle app legacy in un'infrastruttura iperconvergente ed efficiente. Accesso agli stessi tipi di funzionalità cloud usate per eseguire data center iperscalabili come Microsoft Azure.

- **Connessione ad Azure per i servizi cloud ibridi.** Accesso semplificato ai servizi di sicurezza e gestione del cloud in Azure, inclusi il backup in un altro luogo, il ripristino del sito, il monitoraggio basato sul cloud e altro ancora.

## <a name="the-azure-stack-family"></a>La famiglia Azure Stack

Azure Stack HCI fa parte della famiglia Azure ed Azure Stack e usa lo stesso software di rete, di archiviazione e di elaborazione software-defined di Azure Stack. Ecco un breve riepilogo delle diverse soluzioni:

- [Azure](https://azure.microsoft.com): uso di servizi cloud pubblici
- [Azure Stack](https://azure.microsoft.com/overview/azure-stack) esecuzione di servizi cloud in locale
- [Azure Stack HCI](https://azure.microsoft.com/overview/azure-stack/hci): esecuzione di app virtualizzate in locale, con connessioni facoltative ad Azure

![Azure ed Azure Stack eseguono servizi cloud, mentre Azure Stack HCI esegue applicazioni virtualizzate in locale](media/azure_family.png)

|Azure: uso di servizi cloud pubblici|Azure Stack: esecuzione di servizi cloud in locale|Azure Stack HCI: esecuzione di app virtualizzate in locale|
|-----------------|-----------------|-----------------|
|Per risorse di elaborazione self-service su richiesta che consentono di modernizzare ed eseguire la migrazione di app esistenti e creare nuove app cloud native.|Creazione ed esecuzione di applicazioni cloud nella rete perimetrale, quando disconnesse, oppure, per soddisfare i requisiti normativi, usando normali servizi di Azure in locale.| Esecuzione di applicazioni virtualizzate in locale, sostituzione e consolidamento dell'infrastruttura server obsoleta e connessione ad Azure per i servizi cloud.|
|Oltre 100 servizi disponibili in 54 aree geografiche in tutto il mondo|Macchine virtuali di Azure per Windows e Linux, App Web e Funzioni di Azure, Azure Key Vault, Azure Resource Manager, Azure Marketplace, Contenitori, Hub IoT di Azure e Hub eventi, strumenti di amministrazione (piani, offerte, Controllo degli accessi in base al ruolo)|Soluzioni HCI convalidate basate su Hyper V e Spazi di archiviazione diretta con Windows Server 2019 e Windows Admin Center per la gestione e l'accesso integrato ai servizi di Azure.|

Per altre informazioni:

- Per altre informazioni, visita il sito Web dedicato alle soluzioni [Azure Stack HCI](https://azure.microsoft.com/overview/azure-stack/hci).
- Guarda il video in cui gli esperti Microsoft Jeff Woolsey e Vijay Tewari [illustrano le nuove soluzioni Azure Stack HCI](https://aka.ms/AzureStackOverviewVideo).

## <a name="hyperconverged-efficiencies"></a>Funzionalità iperconvergenti

Le soluzioni Azure Stack HCI combinano funzionalità altamente virtualizzate di elaborazione, archiviazione e reti in server e componenti x86 standard di settore. La combinazione di risorse nello stesso cluster facilita la distribuzione, la gestione e il ridimensionamento. Per la gestione puoi scegliere tra l'automazione da riga di comando e Windows Admin Center.

Per macchine virtuali con prestazioni leader del settore per le applicazioni server, usa Hyper-V, la tecnologia hypervisor su cui si basa il cloud Microsoft, e la tecnologia Spazi di archiviazione diretta con supporto integrato per NVMe, memoria persistente e rete RDMA (Remote-Direct Memory Access).

App e dati sono al sicuro grazie a macchine virtuali schermate, microsegmentazione della rete e crittografia per i dati inattivi e in movimento.

## <a name="hybrid-capabilities"></a>Funzionalità ibride

Puoi sfruttare la combinazione di ambiente cloud e locale in esecuzione con una piattaforma di infrastruttura iperconvergente nel cloud pubblico. Il tuo team può iniziare a creare competenze cloud grazie all'integrazione predefinita con i servizi di gestione dell'infrastruttura di Azure:

- Azure Site Recovery per la disponibilità elevata e il ripristino di emergenza distribuito come servizio (DRaaS).

- Monitoraggio di Azure, un hub centralizzato per tenere traccia degli eventi nelle applicazioni, nella rete e nell'infrastruttura, che include anche funzionalità di analisi avanzate basate sull'intelligenza artificiale.

- Cloud di controllo, per usare Azure come alternativa più leggera al quorum del cluster.

- Backup di Azure per la protezione dei dati in un altro luogo e la protezione dal ransomware.

- Gestione aggiornamenti di Azure per la valutazione e le distribuzioni degli aggiornamenti per le macchine virtuali Windows in esecuzione in Azure e in locale.

- Scheda di rete di Azure per connettere le risorse locali alle macchine virtuali in Azure tramite una rete privata virtuale da punto a sito.

- Per la sincronizzazione del file server con il cloud, usa Sincronizzazione file di Azure.

Per informazioni dettagliate, vedi [Connecting Windows Server to Azure hybrid services](..\manage\windows-admin-center\azure\index.md) (Connessione di Windows Server a servizi ibridi di Azure).

## <a name="management-tools-and-system-center"></a>Strumenti di gestione e System Center

Azure Stack HCI usa lo stesso software di virtualizzazione, di rete e di archiviazione software-defined di Azure Stack. Con Azure Stack HCI hai diritti completi di amministratore sul cluster: puoi usare [Windows Admin Center](..\manage\windows-admin-center\overview.md), [System Center](https://www.microsoft.com/cloud-platform/system-center) e qualsiasi funzionalità di [Hyper-V](../virtualization/hyper-v/hyper-v-on-windows-server.md), [Spazi di archiviazione diretta](..\storage\storage-spaces\storage-spaces-direct-overview.md), PowerShell, oltre a strumenti di terze parti, come 5Nine Manager.

Microsoft Azure viene eseguito sulle stesse piattaforme Windows Server e Hyper-V incluse in Windows Server. Windows Server e System Center includono miglioramenti e procedure consigliate, risultato dell'esperienza di Microsoft nella gestione di reti di data center su scala globale come Microsoft Azure, che consentono di distribuire le stesse tecnologie che offrono flessibilità, automazione e controllo quando si usano le tecnologie di Software Defined Networking.

Per distribuire e gestire l'infrastruttura, usa System Center Virtual Machine Management (VMM) e System Center Operations Manager. Con VMM puoi effettuare il provisioning e gestire le risorse necessarie per creare e distribuire macchine virtuali e servizi nei cloud privati. Con Operations Manager puoi monitorare servizi, dispositivi e operazioni in ambito aziendale per identificare problemi e intervenire tempestivamente.

## <a name="hardware-partners"></a>Partner hardware

Puoi acquistare le soluzioni Azure Stack HCI convalidate che eseguono Windows Server 2019 presso 15 partner. Il tuo partner Microsoft preferito ti aiuterà a predisporre il sistema senza dover dedicare tempo prezioso alla progettazione e sarà il tuo punto di contatto dedicato per i servizi di supporto e implementazione.

Visita il [sito Web di Azure Stack HCI](https://azure.microsoft.com/overview/azure-stack/hci) per visualizzare le oltre 70 soluzioni Azure Stack HCI attualmente disponibili presso i partner Microsoft seguenti: ASUS, Axellio, bluechip, DataON, Dell EMC, Fujitsu, HPE, Hitachi, Huawei, Lenovo, NEC, primeLine Solutions, QCT, SecureGUARD e Supermicro.

## <a name="faq"></a>Domande frequenti

### <a name="what-do-azure-stack-and-azure-stack-hci-solutions-have-in-common"></a>Cosa hanno in comune le soluzioni Azure Stack ed Azure Stack HCI?

Le soluzioni Azure Stack HCI includono le stesse tecnologie di rete, archiviazione ed elaborazione software-defined basate su Hyper-V di Azure Stack. Entrambe le offerte rispondono a rigorosi criteri di test e convalida per garantirne affidabilità e compatibilità con la piattaforma hardware sottostante.

### <a name="how-are-they-different"></a>In che cosa differiscono?

Con Azure Stack esegui i servizi cloud in locale. Puoi eseguire servizi IaaS e PaaS di Azure in locale per creare ed eseguire applicazioni cloud ovunque e gestirle con il portale di Azure in locale.

Con Azure Stack HCI esegui carichi di lavoro virtualizzati in locale, gestendoli con Windows Admin Center e i già noti strumenti di Windows Server. Facoltativamente, puoi connetterti ad Azure per scenari ibridi, come ripristino del sito basato sul cloud, monitoraggio e di altro tipo.

### <a name="why-is-microsoft-bringing-its-hci-offering-to-the-azure-stack-family"></a>Perché Microsoft ha scelto di aggiungere l'offerta HCI alla famiglia Azure Stack?

La tecnologia iperconvergente di Microsoft costituisce già la base di Azure Stack.

Gli ambienti IT di molti clienti Microsoft sono complessi. Il nostro obiettivo è offrire soluzioni che soddisfano le esigenze di tali ambienti proponendo la tecnologia più adatta. Azure Stack HCI è un'evoluzione delle soluzioni software-defined per Windows Server basate su Windows Server 2016 disponibili in precedenza presso i nostri partner hardware. Abbiamo scelto di aggiungere Azure Stack HCI alla famiglia Azure Stack perché abbiamo iniziato a proporre nuove opzioni per connettere i servizi di gestione dell'infrastruttura ad Azure.

### <a name="does-azure-stack-hci-need-to-be-connected-to-azure"></a>Azure Stack HCI deve essere connesso ad Azure?

No, è assolutamente facoltativo. Puoi sfruttare l'integrazione con Azure per scenari ibridi come il backup e il ripristino di emergenza in un altro luogo, il monitoraggio basato sul cloud e la gestione degli aggiornamenti, ma si tratta di un'opzione facoltativa. Non comporta alcun problema, quindi, l'esecuzione in modalità non connessa.

### <a name="how-does-azure-stack-hci-relate-to-windows-server"></a>In che modo Azure Stack HCI è correlato a Windows Server?

Windows Server 2019 costituisce la base di quasi tutti prodotti Azure. Tutte le funzionalità che ti servono continueranno a essere incluse e supportate in Windows Server. Azure Stack HCI è la soluzione consigliata per distribuire HCI in locale, usando hardware prodotto dai nostri partner e convalidato da Microsoft.

### <a name="will-i-be-able-to-upgrade-from-azure-stack-hci-to-azure-stack"></a>Sarà possibile eseguire l'aggiornamento da Azure Stack HCI ad Azure Stack? 

No, ma i clienti possono eseguire la migrazione dei carichi di lavoro da Azure Stack HCI ad Azure Stack o ad Azure.

### <a name="what-azure-services-can-i-connect-to-azure-stack-hci"></a>Quali servizi di Azure posso connettere ad Azure Stack HCI?

Per un elenco aggiornato di servizi di Azure a cui è possibile connettere Azure Stack HCI, vedi [Connecting Windows Server to Azure hybrid services](../azure-hybrid-services/index.md) (Connessione di Windows Server a servizi ibridi di Azure).

### <a name="how-does-the-cost-of-azure-stack-hci-compare-to-azure-stack"></a>Quali sono le differenze tra Azure Stack HCI e Azure Stack in termini di costo? 

Azure Stack viene venduto come sistema completamente integrato che include servizi e supporto. Puoi acquistare Azure Stack come sistema e gestirlo personalmente oppure come servizio completamente gestito dai nostri partner. Oltre al sistema di base, è possibile acquistare servizi di Azure eseguiti in Azure Stack o Azure che prevedono il pagamento in base al consumo.

Per le soluzioni Azure Stack HCI viene adottato il modello di acquisto tradizionale. Puoi acquistare l'hardware convalidato presso partner di Azure Stack HCI e il software (Windows Server 2019 Datacenter Edition con funzionalità di datacenter software-defined e Windows Admin Center) presso i diversi canali esistenti. Per il pagamento dei servizi di Azure utilizzabili con Windows Admin Center userai una sottoscrizione di Azure.

### <a name="how-do-i-buy-azure-stack-hci-solutions"></a>Come faccio ad acquistare le soluzioni Azure Stack HCI?

Attenersi ai passaggi descritti di seguito.

1. Acquista un sistema hardware convalidato da Microsoft presso il tuo partner hardware preferito.
1. Installa Windows Server 2019 Datacenter Edition and Windows Admin Center per la gestione e la funzionalità per connettere i servizi cloud ad Azure.
1. Facoltativamente, usa il tuo account di Azure per collegare i servizi di gestione e sicurezza basati sul cloud ai tuoi carichi di lavoro.

![Per acquistare soluzioni Azure Stack HCI, scegli il partner e la configurazione hardware più adatti alle tue esigenze.](media/howbuy_ashci.png)

## <a name="compare-azure-stack-and-azure-stack-hci"></a>Confronto tra Azure Stack e Azure Stack HCI

Durante il processo di trasformazione digitale della tua organizzazione potresti accorgerti che puoi velocizzare il passaggio se usi servizi cloud pubblici che ti consentono di creare architetture moderne e aggiornare le applicazioni legacy. Tuttavia, per vari motivi, tra cui ostacoli tecnologici e normativi, molti carichi di lavoro devono rimanere in locale. La tabella seguente ti aiuta a determinare la strategia cloud ibrida Microsoft più adatta alle tue esigenze e che ti permette di implementare l'innovazione cloud per qualsiasi carico di lavoro ovunque si trovi.

|Azure Stack|Azure Stack HCI|
|--------|-------|
|Nuove competenze, processi innovativi|Stesse competenze, processi già noti|
|Servizi di Azure nel data center|Connessione del data center a servizi di Azure|

### <a name="when-to-use-azure-stack"></a>Quando usare Azure Stack

|Azure Stack|Azure Stack HCI|
|--------|-------|
|Usa Azure Stack per la modalità IaaS (Infrastructure-as-a-Service) self-service, caratterizzato da un elevato isolamento e un monitoraggio e chargeback accurati dell'utilizzo per più tenant con percorso condiviso. Ideale per provider di servizi e cloud privati aziendali. Modelli disponibili nel Azure Marketplace.|Azure Stack HCI non applica né supporta in modo nativo la modalità multi-tenancy.|
|Usa Azure Stack per sviluppare ed eseguire app che si basano su servizi PaaS (Platform-as-a-Service), come App Web, Funzioni o Hub eventi in locale. Questi servizi vengono eseguiti in Azure Stack con le stesse modalità di Azure, di conseguenza contribuiscono a realizzare un ambiente di sviluppo e runtime ibrido realmente coerente.|Azure Stack HCI non esegue servizi PaaS in locale.
|Usa Azure Stack per modernizzare la distribuzione e l'utilizzo delle app con le funzionalità DevOps, come l'infrastruttura come codice, l'integrazione continua e la distribuzione continua (CI/CD), oltre a comode funzionalità come Estensioni macchina virtuale coerenti con Azure. Ideale per team di sviluppo e DevOps.|Azure Stack HCI non include in modo nativo alcuno strumento DevOps.

### <a name="when-to-use-azure-stack-hci"></a>Quando usare Azure Stack HC

|Azure Stack|Azure Stack HCI|
|---------------|---------------|
|Azure Stack richiede un minimo di 4 nodi e appositi switch di rete.|Usa Azure Stack HCI per ridurre al minimo il footprint per uffici remoti/filiali. Inizia con due soli nodi server e la rete back to back switchless per la massima semplicità e convenienza. Le offerte hardware partono con 4 unità, 64 GB di memoria, per un costo decisamente inferiore a 10.000 dollari/nodo.
|Azure Stack vincola la configurabilità di Hyper-V e il set di funzionalità per garantire la coerenza con Azure.|Usa Azure Stack HCI per una virtualizzazione Hyper-V semplificata per le classiche app aziendali come Exchange, SharePoint e SQL Server e per virtualizzare i ruoli di Windows Server come File Server, DNS, DHCP, IIS e AD. Accesso illimitato a tutte le funzionalità di Hyper-V, come le VM schermate.|
|Azure Stack non espone queste tecnologie strutturali.|Usa Azure Stack HCI per sostituire array di archiviazione o appliance di rete obsoleti con l'infrastruttura software-defined, senza dover ridefinire completamente l'architettura. Le funzionalità integrate Spazi di archiviazione diretta e SDN (Software Defined Networking) garantiscono un'agevole integrazione con gli ambienti Hyper-V.|