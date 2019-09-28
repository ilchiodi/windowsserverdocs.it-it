---
title: Descrizioni delle funzionalità per le macchine virtuali Linux e FreeBSD in Hyper-V
description: Descrive le funzionalità che interessano i componenti di base, ad esempio la rete, l'archiviazione e la memoria quando si usano Linux e FreeBSD in una macchina virtuale
ms.prod: windows-server
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: a9ee931d-91fc-40cf-9a15-ed6fa6965cb6
author: shirgall
ms.author: kathydav
ms.date: 10/03/2016
ms.openlocfilehash: 1690230d326d7e32175ccde5da1e5fae421a76d0
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71366805"
---
# <a name="feature-descriptions-for-linux-and-freebsd-virtual-machines-on-hyper-v"></a>Descrizioni delle funzionalità per le macchine virtuali Linux e FreeBSD in Hyper-V

>Si applica a: Windows Server 2016, Hyper-V Server 2016, Windows Server 2012 R2, Hyper-V Server 2012 R2, Windows Server 2012, Hyper-V Server 2012, Windows Server 2008 R2, Windows 10, Windows 8.1, Windows 8, Windows 7,1, Windows 7

Questo articolo descrive le funzionalità disponibili in componenti quali core, rete, archiviazione e memoria quando si usa Linux e FreeBSD in una macchina virtuale.

## <a name="core"></a>Core

|**Funzionalità**|**Descrizione**|
|-|-|
|Arresto integrato|Con questa funzionalità, un amministratore può arrestare le macchine virtuali dalla console di gestione di Hyper-V. Per ulteriori informazioni, vedere [arresto del sistema operativo](https://technet.microsoft.com/library/dn798297(WS.11).aspx#BKMK_Shutdown).|
|Sincronizzazione degli orari|Questa funzionalità garantisce che il tempo di mantenimento all'interno di una macchina virtuale venga mantenuto sincronizzato con il tempo di manutenzione nell'host. Per ulteriori informazioni, vedere [sincronizzazione dell'ora](https://technet.microsoft.com/library/dn798297(WS.11).aspx#BKMK_time).|
|Ora esatta di Windows Server 2016|Questa funzionalità consente al Guest di usare la funzionalità ora esatta di Windows Server 2016, che migliora la sincronizzazione dell'ora con l'host per 1 MS la precisione. Per altre informazioni, vedere [ora esatta di Windows Server 2016](https://technet.microsoft.com/windows-server-docs/identity/ad-ds/get-started/windows-time-service/windows-2016-accurate-time)|
|Supporto per la multielaborazione|Con questa funzionalità, una macchina virtuale può usare più processori nell'host configurando più CPU virtuali.|
|Heartbeat|Con questa funzionalità, l'host per può tenere traccia dello stato della macchina virtuale. Per ulteriori informazioni, vedere [Heartbeat](https://technet.microsoft.com/library/dn798297(WS.11).aspx#BKMK_heartbeat).|
|Supporto del mouse integrato|Con questa funzionalità, è possibile utilizzare un mouse sul desktop di una macchina virtuale e utilizzare il mouse senza interruzioni tra il desktop di Windows Server e la console di Hyper-V per la macchina virtuale.|
|Dispositivo di archiviazione specifico di Hyper-V|Questa funzionalità concede l'accesso ad alte prestazioni ai dispositivi di archiviazione collegati a una macchina virtuale.|
|Dispositivo di rete specifico di Hyper-V|Questa funzionalità concede l'accesso ad alte prestazioni alle schede di rete collegate a una macchina virtuale.|

## <a name="networking"></a>Rete

|**Funzionalità**|**Descrizione**|
|-|-|
|Frame jumbo|Con questa funzionalità, un amministratore può aumentare le dimensioni dei frame di rete oltre 1500 byte, causando un aumento significativo delle prestazioni di rete.|
|Assegnazione di tag e trunking VLAN|Questa funzionalità consente di configurare virtuale traffico LAN (VLAN) per le macchine virtuali.|
|Migrazione in tempo reale|Con questa funzionalità, è possibile eseguire la migrazione di una macchina virtuale da un host a un altro. Per ulteriori informazioni, vedere [Virtual Machine Live Migration Overview](https://technet.microsoft.com/library/hh831435.aspx).|
|Inserimento IP statico|Con questa funzionalità, è possibile replicare l'indirizzo IP statico di una macchina virtuale dopo che è stato eseguito il failover nella replica in un host diverso. Tale replica IP garantisce che i carichi di lavoro di rete continuino a funzionare in modo uniforme dopo un evento di failover.|
|vRSS (Receive-Side Scaling virtuale)|Distribuisce il carico da una scheda di rete virtuale tra più processori virtuali in una macchina virtuale. Per ulteriori informazioni, vedere la pagina relativa [a Receive-Side Scaling virtuale in Windows Server 2012 R2](https://technet.microsoft.com/library/dn383582.aspx).|
|Offload di segmentazione e checksum TCP|Trasferisce il lavoro di segmentazione e checksum dalla CPU Guest al Commuter virtuale host o alla scheda di rete durante i trasferimenti di dati di rete.|
|Grandi ricezione Offload (e)|Aumenta la velocità effettiva in ingresso di connessioni a banda larga da aggregare più pacchetti in un buffer più grande, riducendo l'overhead della CPU.|
|SR-IOV|I dispositivi di I/O a radice singola utilizzano DDA per consentire ai guest di accedere a parti di schede NIC specifiche, consentendo una latenza ridotta e una maggiore velocità effettiva. SR-IOV richiede driver di funzioni fisiche (PF) aggiornati nei driver delle funzioni host e delle funzioni virtuali (VF) sul Guest.|

## <a name="storage"></a>Archiviazione

|**Funzionalità**|**Descrizione**|
|-|-|
|Ridimensionamento VHDX|Con questa funzionalità, un amministratore può ridimensionare un file con estensione VHDX a dimensione fissa collegato a una macchina virtuale. Per ulteriori informazioni, vedere [Online Panoramica dischi rigidi virtuali ridimensionamento](https://technet.microsoft.com/library/dn282286.aspx).|
|Fibre Channel virtuale|Con questa funzionalità, le macchine virtuali possono riconoscere un dispositivo Fiber Channel e montarlo in modalità nativa. Per ulteriori informazioni, vedere [Panoramica di Hyper-V Virtual Fibre Channel](https://technet.microsoft.com/library/hh831413.aspx).|
|Backup della macchina virtuale in tempo reale|Questa funzionalità facilita il backup del tempo zero per le macchine virtuali attive.<br /><br />Si noti che l'operazione di backup non riesce se la macchina virtuale dispone di dischi rigidi virtuali (VHD) ospitati nell'archiviazione remota, ad esempio una condivisione SMB (Server Message Block) o un volume iSCSI. Assicurarsi inoltre che la destinazione di backup non risieda nello stesso volume del volume di cui si esegue il backup.|
|Supporto TRIM|Gli hint TRIM segnalano all'unità che determinati settori precedentemente allocati non sono più necessari per l'app e possono essere eliminati. Questo processo viene in genere usato quando un'app esegue allocazioni di spazio di grandi dimensioni tramite un file e quindi gestisce automaticamente le allocazioni al file, ad esempio, ai file del disco rigido virtuale.|
|WWN SCSI|Il driver storvsc estrae le informazioni di nome universale (WWN) dalla porta e il nodo dei dispositivi collegati alla macchina virtuale e crea i file sysfs appropriato. |

## <a name="memory"></a>Memoria

|**Funzionalità**|**Descrizione**|
|-|-|
|Supporto del kernel PAE|La tecnologia indirizzo Extension (PAE) fisico consente un kernel a 32 bit accedere a uno spazio di indirizzo fisico è superiore a 4GB. Distribuzioni di Linux precedenti, ad esempio RHEL 5. x utilizzato per fornire un kernel separato che è stato PAE attivata. Distribuzioni più recenti, ad esempio RHEL 6. x sono predefiniti supporto PAE.|
|Configurazione di MMIO Gap|Con questa funzionalità, i produttori di appliance possono configurare il percorso del gap di I/O mappato alla memoria (MMIO). Il gap MMIO viene in genere usato per dividere la memoria fisica disponibile tra un sistema operativo di un appliance sufficiente (JeOS) e l'infrastruttura software effettiva che alimenta l'appliance.|
|Memoria dinamica - aggiunta a caldo|L'host in modo dinamico può aumentare o diminuire la quantità di memoria disponibile per una macchina virtuale mentre è in operazione. Prima del provisioning, l'amministratore abilita la memoria dinamica nel pannello impostazioni della macchina virtuale e specificare la memoria di avvio, memoria minima e massima di memoria per la macchina virtuale. Quando la macchina virtuale è operazione che non è possibile disabilitare la memoria dinamica e solo le impostazioni minimo e massimo può essere modificato. (È consigliabile specificare le dimensioni della memoria in multipli di 128MB).<br /><br />Quando la macchina virtuale è iniziata disponibile è uguale a memoria di **memoria di avvio**. Man mano che aumenta la richiesta di memoria a causa di carichi di lavoro dell'applicazione Hyper-V in modo dinamico può allocare altra memoria alla macchina virtuale tramite il meccanismo di aggiunta a caldo, se supportato da tale versione del kernel. La quantità massima di memoria allocata è limitata dal valore della **massima di memoria** parametro.<br /><br />La scheda di memoria di Hyper-V manager verrà visualizzata la quantità di memoria assegnata alla macchina virtuale, ma le statistiche di memoria all'interno della macchina virtuale verranno Mostra la quantità massima di memoria allocata.<br /><br />Per ulteriori informazioni, vedere [Panoramica della memoria dinamica di Hyper-V](https://technet.microsoft.com/library/hh831766.aspx).<br /><br />|
|Memoria dinamica - Ballooning|L'host in modo dinamico può aumentare o diminuire la quantità di memoria disponibile per una macchina virtuale mentre è in operazione. Prima del provisioning, l'amministratore abilita la memoria dinamica nel pannello impostazioni della macchina virtuale e specificare la memoria di avvio, memoria minima e massima di memoria per la macchina virtuale. Quando la macchina virtuale è operazione che non è possibile disabilitare la memoria dinamica e solo le impostazioni minimo e massimo può essere modificato. (È consigliabile specificare le dimensioni della memoria in multipli di 128MB).<br /><br />Quando la macchina virtuale è iniziata disponibile è uguale a memoria di **memoria di avvio**. Man mano che aumenta la richiesta di memoria a causa di carichi di lavoro dell'applicazione Hyper-V in modo dinamico può allocare più memoria per la macchina virtuale tramite il meccanismo di aggiunta a caldo (sopra). Come la richiesta di memoria riduce Hyper-V può automaticamente il deprovisioning memoria dalla macchina virtuale tramite il meccanismo di fumetto. Hyper-V non comporta il deprovisioning di memoria riportata di seguito il **memoria minima** parametro.<br /><br />La scheda di memoria di Hyper-V manager verrà visualizzata la quantità di memoria assegnata alla macchina virtuale, ma le statistiche di memoria all'interno della macchina virtuale verranno Mostra la quantità massima di memoria allocata.<br /><br />Per ulteriori informazioni, vedere [Panoramica della memoria dinamica di Hyper-V](https://technet.microsoft.com/library/hh831766.aspx).<br /><br />|
|Ridimensionamento della memoria di runtime|Un amministratore può impostare la quantità di memoria disponibile per una macchina virtuale mentre è attivo, aumentando la memoria ("caldo") o ridurlo ("a caldo rimuovere"). La memoria viene restituita a Hyper-V tramite il driver del fumetto (vedere "memoria dinamica-Ballooning"). Il driver del fumetto mantiene una quantità minima di memoria libera dopo la bolla, denominata "Floor", quindi la memoria assegnata non può essere ridotta al di sotto della richiesta corrente più questo importo del piano. La scheda di memoria di Hyper-V manager verrà visualizzata la quantità di memoria assegnata alla macchina virtuale, ma le statistiche di memoria all'interno della macchina virtuale verranno Mostra la quantità massima di memoria allocata. (È consigliabile specificare i valori di memoria in multipli di 128MB).|

## <a name="video"></a>Video

|**Funzionalità**|**Descrizione**|
|-|-|
|Dispositivo video specifico di Hyper-V|Questa funzionalità offre grafica a prestazioni elevate e risoluzione superiore per le macchine virtuali. Questo dispositivo non fornisce la modalità sessione avanzata o le funzionalità RemoteFX.|

## <a name="miscellaneous"></a>Miscellaneous

|**Funzionalità**|**Descrizione**|
|-|-|
|Scambio di KVP (coppia chiave-valore)|Questa funzionalità fornisce un servizio di scambio di coppie chiave/valore (KVP) per le macchine virtuali. In genere, gli amministratori usano il meccanismo KVP per eseguire operazioni di lettura e scrittura sui dati personalizzate in una macchina virtuale. Per ulteriori informazioni, vedere [Apparecchiature per Exchange: Uso di coppie chiave-valore per condividere informazioni tra l'host e il Guest in Hyper-V @ no__t-0.|
|Interrupt non mascherabile|Con questa funzionalità, un amministratore può emettere interrupt non mascherabili (NMI) a una macchina virtuale. NMIs sono utili per ottenere i dump di arresto anomalo dei sistemi operativi che non rispondono a causa di bug dell'applicazione. I dump di arresto anomalo del sistema possono essere diagnosticati dopo il riavvio.|
|Copia di file da host a Guest|Questa funzionalità consente di copiare i file dal computer fisico host alle macchine virtuali guest senza usare la scheda di rete. Per ulteriori informazioni, vedere [servizi Guest](https://technet.microsoft.com/library/dn798297(WS.11).aspx#BKMK_guest).|
|comando lsvmbus|Questo comando ottiene informazioni sui dispositivi in Hyper-V Virtual Machine Bus (VMBus) simili a comandi di informazioni come lspci.|
|Socket di Hyper-V|Si tratta di un canale di comunicazione aggiuntivo tra il sistema operativo host e guest. Per caricare e utilizzare il modulo kernel Sockets Hyper-V, vedere [rendere i propri servizi di integrazione](https://msdn.microsoft.com/virtualization/hyperv_on_windows/develop/make_mgmt_service).|
|Pass-through/DDA PCI|Con Windows Server 2016 gli amministratori possono passare attraverso dispositivi PCI Express tramite il meccanismo di assegnazione dispositivo discreti. Dispositivi comuni sono le schede di rete, schede grafiche e i dispositivi di archiviazione speciale. La macchina virtuale richiederà il driver appropriato per utilizzare l'hardware esposto. L'hardware deve essere assegnato alla macchina virtuale per poter essere utilizzato.<br /><br />Per ulteriori informazioni, vedere [discreti dispositivo assegnazione - descrizione e lo sfondo](https://blogs.technet.microsoft.com/virtualization/2015/11/19/discrete-device-assignment-description-and-background/).<br /><br />DDA è un prerequisito per la rete SR-IOV. Le porte virtuali dovranno essere assegnati alla macchina virtuale e la macchina virtuale è necessario utilizzare driver VF (Virtual Function) per il multiplexing di dispositivo.|

## <a name="generation-2-virtual-machines"></a>Macchine virtuali di seconda generazione

|**Funzionalità**|**Descrizione**|
|-|-|
|Avvio tramite UEFI|Questa funzionalità consente l'avvio delle macchine virtuali utilizzando Unified Extensible Firmware Interface (UEFI).<br /><br />Per ulteriori informazioni, vedere la [panoramica delle macchine virtuali di seconda generazione](https://technet.microsoft.com/library/dn282285.aspx).|
|Avvio protetto|Questa funzionalità consente alle macchine virtuali di usare la modalità di avvio protetto basato su UEFI. Quando una macchina virtuale viene avviata in modalità protetta, diversi componenti del sistema operativo vengono verificati usando le firme presenti nell'archivio dati UEFI.<br /><br />Per ulteriori informazioni, vedere [avvio protetto](https://technet.microsoft.com/library/dn486875.aspx).|

## <a name="see-also"></a>Vedere anche

* [Macchine virtuali CentOS e Red Hat Enterprise Linux supportate in Hyper-V](Supported-CentOS-and-Red-Hat-Enterprise-Linux-virtual-machines-on-Hyper-V.md)

* [Macchine virtuali Debian supportate in Hyper-V](Supported-Debian-virtual-machines-on-Hyper-V.md)

* [Macchine virtuali Oracle Linux supportate in Hyper-V](Supported-Oracle-Linux-virtual-machines-on-Hyper-V.md)

* [Macchine virtuali SUSE supportate in Hyper-V](Supported-SUSE-virtual-machines-on-Hyper-V.md)

* [Macchine virtuali Ubuntu supportate in Hyper-V](Supported-Ubuntu-virtual-machines-on-Hyper-V.md)

* [Macchine virtuali FreeBSD supportate in Hyper-V](Supported-FreeBSD-virtual-machines-on-Hyper-V.md)

* [Procedure consigliate per l'esecuzione di Linux in Hyper-V](Best-Practices-for-running-Linux-on-Hyper-V.md)

* [Procedure consigliate per l'esecuzione di FreeBSD in Hyper-V](Best-practices-for-running-FreeBSD-on-Hyper-V.md)