---
title: Forniscono le descrizioni per le macchine virtuali Linux e FreeBSD in Hyper-V
description: Vengono descritte le funzionalità che influiscono sui componenti di base, ad esempio reti, archiviazione, della memoria quando si usa Linux e FreeBSD in una macchina virtuale
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: a9ee931d-91fc-40cf-9a15-ed6fa6965cb6
author: shirgall
ms.author: kathydav
ms.date: 10/03/2016
ms.openlocfilehash: a574275f6d3495a9cc9bff36fa785f28a7cd8d6f
ms.sourcegitcommit: 8ba2c4de3bafa487a46c13c40e4a488bf95b6c33
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/25/2019
ms.locfileid: "66222878"
---
# <a name="feature-descriptions-for-linux-and-freebsd-virtual-machines-on-hyper-v"></a>Forniscono le descrizioni per le macchine virtuali Linux e FreeBSD in Hyper-V

>Si applica a: Windows Server 2016, Hyper-V Server 2016, Windows Server 2012 R2, Hyper-V Server 2012 R2, Windows Server 2012, Hyper-V Server 2012, Windows Server 2008 R2, Windows 10, Windows 8.1, Windows 8, Windows 7.1, Windows 7

Questo articolo descrive le funzionalità disponibili nei componenti, ad esempio core, funzionalità di rete, archiviazione e memoria quando si usa Linux e FreeBSD in una macchina virtuale.

## <a name="core"></a>Core

|**Funzionalità**|**Descrizione**|
|-|-|
|Arresto integrata|Con questa funzionalità, un amministratore può arrestare le macchine virtuali di gestione di Hyper-V. Per ulteriori informazioni, vedere [arresto del sistema operativo](https://technet.microsoft.com/library/dn798297(WS.11).aspx#BKMK_Shutdown).|
|Sincronizzazione dell'ora|Questa funzionalità garantisce che il tempo mantenuto all'interno di una macchina virtuale viene mantenuto sincronizzato con l'ora di manutenzione sull'host. Per ulteriori informazioni, vedere [sincronizzazione dell'ora](https://technet.microsoft.com/library/dn798297(WS.11).aspx#BKMK_time).|
|Ora esatta di Windows Server 2016|Questa funzionalità consente al guest di usare la funzionalità ora esatta di Windows Server 2016, che migliora la sincronizzazione dell'ora con l'host a 1 ms accuratezza. Per altre informazioni, vedere [ora esatta di Windows Server 2016](https://technet.microsoft.com/windows-server-docs/identity/ad-ds/get-started/windows-time-service/windows-2016-accurate-time)|
|Supporto di più processori|Con questa funzionalità, una macchina virtuale può utilizzare più processori nell'host tramite la configurazione di più CPU virtuale.|
|Heartbeat|Con questa funzionalità, l'host del possibile rilevare lo stato della macchina virtuale. Per ulteriori informazioni, vedere [Heartbeat](https://technet.microsoft.com/library/dn798297(WS.11).aspx#BKMK_heartbeat).|
|Supporto mouse integrati|Con questa funzionalità, è possibile utilizzare il mouse sul desktop della macchina virtuale e anche utilizzare il mouse senza problemi tra il desktop di Windows Server e la console di Hyper-V per la macchina virtuale.|
|Dispositivo di archiviazione specifico di Hyper-V|Questa funzionalità concede l'accesso ad alte prestazioni per i dispositivi di archiviazione collegati a una macchina virtuale.|
|Dispositivo di rete specifico di Hyper-V|Questa funzionalità concede l'accesso ad alte prestazioni alle schede di rete associati a una macchina virtuale.|

## <a name="networking"></a>Rete

|**Funzionalità**|**Descrizione**|
|-|-|
|Frame jumbo|Con questa funzionalità, un amministratore può aumentare le dimensioni dei fotogrammi di rete oltre 1500 byte, che comportano un aumento significativo delle prestazioni di rete.|
|La codifica VLAN e trunking|Questa funzionalità consente di configurare virtuale traffico LAN (VLAN) per le macchine virtuali.|
|Migrazione in tempo reale|Con questa funzionalità, è possibile eseguire la migrazione di una macchina virtuale da un host a un altro host. Per altre informazioni, vedere [Panoramica di Virtual Machine Live Migration](https://technet.microsoft.com/library/hh831435.aspx).|
|Indirizzo IP statico Injection|Con questa funzionalità, è possibile replicare l'indirizzo IP statico di una macchina virtuale dopo che è stato eseguito il failover alla replica in un host diverso. Tale replica IP garantisce che i carichi di lavoro di rete continuino a funzionare senza problemi dopo un evento di failover.|
|vRSS (Virtual Receive Side Scaling)|Distribuisce il carico da una scheda di rete virtuale tra più processori virtuali in una macchina virtuale. Per altre informazioni, vedere [Receive-side Scaling virtuale in Windows Server 2012 R2](https://technet.microsoft.com/library/dn383582.aspx).|
|Segmentazione di TCP e gli offload Checksum|Segmentazione di trasferimenti e checksum funziona dalla CPU guest alla scheda dell'host virtuale switch o di rete durante i trasferimenti di dati di rete.|
|Grandi ricezione Offload (e)|Aumenta la velocità effettiva in ingresso di connessioni a banda larga da aggregare più pacchetti in un buffer più grande, riducendo l'overhead della CPU.|
|SR-IOV|I dispositivi di Single Root i/o usano DDA per consentire l'accesso guest a parti di schede di interfaccia di rete specifiche che consente di aumentare la velocità effettiva e latenza ridotta. SR-IOV richiede driver di funzione fisico aggiornati (FP) nell'host e di funzioni virtual (VF) nel guest.|

## <a name="storage"></a>Archiviazione

|**Funzionalità**|**Descrizione**|
|-|-|
|Ridimensionamento di VHDX|Con questa funzionalità, un amministratore può ridimensionare un file con estensione vhdx a dimensione fissa che è collegato a una macchina virtuale. Per ulteriori informazioni, vedere [Online Panoramica dischi rigidi virtuali ridimensionamento](https://technet.microsoft.com/library/dn282286.aspx).|
|Fibre Channel virtuale|Con questa funzionalità, le macchine virtuali può riconoscere un dispositivo di canale in fibra ottica e montarlo in modo nativo. Per ulteriori informazioni, vedere [Panoramica di Hyper-V Virtual Fibre Channel](https://technet.microsoft.com/library/hh831413.aspx).|
|Backup della macchina virtuale in tempo reale|Questa funzionalità semplifica zero verso il basso del backup delle macchine virtuali in tempo reale.<br /><br />Si noti che l'operazione di backup non riesce se la macchina virtuale contiene dischi rigidi virtuali (VHD) che sono ospitati in archiviazione remota, ad esempio una condivisione Server Message Block (SMB) o un volume iSCSI. Inoltre, assicurarsi che la destinazione di backup non si trova nello stesso volume come volume di cui esegue il backup.|
|Supporto per TRIM|Gli hint TRIM notificare l'unità che alcuni settori che sono stati allocati in precedenza non sono più necessari per l'app e possono essere eliminati. Questo processo viene in genere usato quando un'app rende le allocazioni di spazio di grandi dimensioni tramite un file e quindi self-consente di gestire le allocazioni del file, ad esempio, ai file di disco rigido virtuale.|
|WWN SCSI|Il driver storvsc estrae le informazioni di nome universale (WWN) dalla porta e il nodo dei dispositivi collegati alla macchina virtuale e crea i file sysfs appropriato. |

## <a name="memory"></a>Memoria

|**Funzionalità**|**Descrizione**|
|-|-|
|Supporto PAE Kernel|La tecnologia indirizzo Extension (PAE) fisico consente un kernel a 32 bit accedere a uno spazio di indirizzo fisico è superiore a 4GB. Distribuzioni di Linux precedenti, ad esempio RHEL 5. x utilizzato per fornire un kernel separato che è stato PAE attivata. Distribuzioni più recenti, ad esempio RHEL 6. x sono predefiniti supporto PAE.|
|Configurazione di gap MMIO|Con questa funzionalità, i produttori di appliance possono configurare il percorso dello spazio i/o eseguito il mapping della memoria (MMIO). Il divario MMIO viene generalmente utilizzato per dividere la memoria fisica disponibile tra solo sufficientemente sistemi operativi dell'appliance un (JeOS) e l'infrastruttura del software effettivo che consente il funzionamento dell'appliance.|
|Memoria dinamica - aggiunta a caldo|L'host in modo dinamico può aumentare o diminuire la quantità di memoria disponibile per una macchina virtuale mentre è in operazione. Prima del provisioning, l'amministratore abilita la memoria dinamica nel pannello impostazioni della macchina virtuale e specificare la memoria di avvio, memoria minima e massima di memoria per la macchina virtuale. Quando la macchina virtuale è operazione che non è possibile disabilitare la memoria dinamica e solo le impostazioni minimo e massimo può essere modificato. (È consigliabile specificare le dimensioni della memoria in multipli di 128MB).<br /><br />Quando la macchina virtuale è iniziata disponibile è uguale a memoria di **memoria di avvio**. Man mano che aumenta la richiesta di memoria a causa di carichi di lavoro dell'applicazione Hyper-V in modo dinamico può allocare altra memoria alla macchina virtuale tramite il meccanismo di aggiunta a caldo, se supportato da tale versione del kernel. La quantità massima di memoria allocata è limitata dal valore della **massima di memoria** parametro.<br /><br />La scheda di memoria di Hyper-V manager verrà visualizzata la quantità di memoria assegnata alla macchina virtuale, ma le statistiche di memoria all'interno della macchina virtuale verranno Mostra la quantità massima di memoria allocata.<br /><br />Per ulteriori informazioni, vedere [Panoramica della memoria dinamica di Hyper-V](https://technet.microsoft.com/library/hh831766.aspx).<br /><br />|
|Memoria dinamica - Ballooning|L'host in modo dinamico può aumentare o diminuire la quantità di memoria disponibile per una macchina virtuale mentre è in operazione. Prima del provisioning, l'amministratore abilita la memoria dinamica nel pannello impostazioni della macchina virtuale e specificare la memoria di avvio, memoria minima e massima di memoria per la macchina virtuale. Quando la macchina virtuale è operazione che non è possibile disabilitare la memoria dinamica e solo le impostazioni minimo e massimo può essere modificato. (È consigliabile specificare le dimensioni della memoria in multipli di 128MB).<br /><br />Quando la macchina virtuale è iniziata disponibile è uguale a memoria di **memoria di avvio**. Man mano che aumenta la richiesta di memoria a causa di carichi di lavoro dell'applicazione Hyper-V in modo dinamico può allocare più memoria per la macchina virtuale tramite il meccanismo di aggiunta a caldo (sopra). Come la richiesta di memoria riduce Hyper-V può automaticamente il deprovisioning memoria dalla macchina virtuale tramite il meccanismo di fumetto. Hyper-V non comporta il deprovisioning di memoria riportata di seguito il **memoria minima** parametro.<br /><br />La scheda di memoria di Hyper-V manager verrà visualizzata la quantità di memoria assegnata alla macchina virtuale, ma le statistiche di memoria all'interno della macchina virtuale verranno Mostra la quantità massima di memoria allocata.<br /><br />Per ulteriori informazioni, vedere [Panoramica della memoria dinamica di Hyper-V](https://technet.microsoft.com/library/hh831766.aspx).<br /><br />|
|Ridimensionamento della memoria di runtime|Un amministratore può impostare la quantità di memoria disponibile per una macchina virtuale mentre è attivo, aumentando la memoria ("caldo") o ridurlo ("a caldo rimuovere"). Memoria verrà restituita a Hyper-V tramite il driver a forma di fumetto (vedere "Dinamica della memoria – Ballooning"). Il driver fumetto mantiene una quantità minima di memoria libera dopo ballooning, denominata "piano", assegnati in modo tale memoria non può essere ridotto di sotto la domanda corrente oltre questa quantità floor. La scheda di memoria di Hyper-V manager verrà visualizzata la quantità di memoria assegnata alla macchina virtuale, ma le statistiche di memoria all'interno della macchina virtuale verranno Mostra la quantità massima di memoria allocata. (È consigliabile specificare i valori di memoria in multipli di 128MB).|

## <a name="video"></a>Video

|**Funzionalità**|**Descrizione**|
|-|-|
|Dispositivo video specifico Hyper-V|Questa funzionalità offre grafica ad alte prestazioni e risoluzione superiore per le macchine virtuali. Questo dispositivo non fornisce le funzionalità RemoteFX o modalità sessione avanzata.|

## <a name="miscellaneous"></a>Varie

|**Funzionalità**|**Descrizione**|
|-|-|
|Scambio di coppia chiave-valore (coppia chiave-valore)|Questa funzionalità offre una coppia chiave/valore servizio exchange (coppia chiave-valore) per le macchine virtuali. In genere gli amministratori usano il meccanismo KVP eseguire lettura e scrittura dati personalizzati in una macchina virtuale. Per altre informazioni, vedere [lo scambio di dati: Utilizzo di coppie chiave-valore per condividere informazioni tra l'host e guest in Hyper-V](https://technet.microsoft.com/library/dn798287.aspx).|
|Interrupt non mascherabile|Con questa funzionalità, un amministratore può eseguire gli interrupt Non mascherabile (NMI) a una macchina virtuale. NMIs sono utili per ottenere i dump di arresto anomalo del sistema di sistemi operativi che non rispondono a causa di errori dell'applicazione. Dopo il riavvio, è possono diagnosticare i dump di arresto anomalo del sistema.|
|Copiare i file dall'host al guest|Questa funzionalità consente di file da copiare dal computer host fisico per le macchine virtuali guest senza utilizzare la scheda di rete. Per ulteriori informazioni, vedere [servizi Guest](https://technet.microsoft.com/library/dn798297(WS.11).aspx#BKMK_guest).|
|comando lsvmbus|Questo comando Ottiene le informazioni sui dispositivi del bus di macchina virtuale Hyper-V (VMBus) simile ai comandi di informazioni come lspci.|
|Socket di Hyper-V|Si tratta di un canale di comunicazione aggiuntivo tra il sistema operativo host e guest. Per caricare e utilizzare il modulo kernel Sockets Hyper-V, vedere [rendere i propri servizi di integrazione](https://msdn.microsoft.com/virtualization/hyperv_on_windows/develop/make_mgmt_service).|
|Pass-through/DDA PCI|Con Windows Server 2016 gli amministratori possono passare attraverso dispositivi PCI Express tramite il meccanismo di assegnazione dispositivo discreti. Dispositivi comuni sono le schede di rete, schede grafiche e i dispositivi di archiviazione speciale. La macchina virtuale richiederà il driver appropriato per utilizzare l'hardware esposto. L'hardware deve essere assegnato alla macchina virtuale per poter essere utilizzato.<br /><br />Per ulteriori informazioni, vedere [discreti dispositivo assegnazione - descrizione e lo sfondo](https://blogs.technet.microsoft.com/virtualization/2015/11/19/discrete-device-assignment-description-and-background/).<br /><br />DDA è un prerequisito per la rete SR-IOV. Le porte virtuali dovranno essere assegnati alla macchina virtuale e la macchina virtuale è necessario utilizzare driver VF (Virtual Function) per il multiplexing di dispositivo.|

## <a name="generation-2-virtual-machines"></a>Macchine virtuali di seconda generazione

|**Funzionalità**|**Descrizione**|
|-|-|
|Avvio UEFI|Questa funzionalità consente alle macchine virtuali per l'avvio con Unified Extensible Firmware Interface (UEFI).<br /><br />Per ulteriori informazioni, vedere la [panoramica delle macchine virtuali di seconda generazione](https://technet.microsoft.com/library/dn282285.aspx).|
|Avvio protetto|Questa funzionalità consente alle macchine virtuali usare la modalità di avvio protetto UEFI basato. Quando una macchina virtuale viene avviata in modalità protetta, vari componenti del sistema operativo vengono verificati mediante le firme presenti nell'archivio dati UEFI.<br /><br />Per ulteriori informazioni, vedere [avvio protetto](https://technet.microsoft.com/library/dn486875.aspx).|

## <a name="see-also"></a>Vedere anche

* [CentOS è supportato e Red Hat Enterprise Linux macchine virtuali in Hyper-V](Supported-CentOS-and-Red-Hat-Enterprise-Linux-virtual-machines-on-Hyper-V.md)

* [Macchine virtuali Debian supportate in Hyper-V](Supported-Debian-virtual-machines-on-Hyper-V.md)

* [Macchine virtuali Oracle Linux supportate in Hyper-V](Supported-Oracle-Linux-virtual-machines-on-Hyper-V.md)

* [Macchine virtuali SUSE supportate in Hyper-V](Supported-SUSE-virtual-machines-on-Hyper-V.md)

* [Macchine virtuali Ubuntu supportate in Hyper-V](Supported-Ubuntu-virtual-machines-on-Hyper-V.md)

* [Macchine virtuali FreeBSD supportate in Hyper-V](Supported-FreeBSD-virtual-machines-on-Hyper-V.md)

* [Le procedure consigliate per l'esecuzione di Linux in Hyper-V](Best-Practices-for-running-Linux-on-Hyper-V.md)

* [Le procedure consigliate per l'esecuzione di FreeBSD in Hyper-V](Best-practices-for-running-FreeBSD-on-Hyper-V.md)