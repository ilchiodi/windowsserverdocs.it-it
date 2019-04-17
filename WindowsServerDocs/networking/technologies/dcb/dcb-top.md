---
title: Data Center Bridging (DCB)
description: È possibile utilizzare questo argomento per informazioni introduttive su Data Center Bridging in Windows Server 2016.
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: da58f312-bd3b-4bb6-98ca-6177869dd6ad
manager: brianlic
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 3d465e855adc387d7136919ac11fbab56c792c34
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/28/2018
---
# <a name="data-center-bridging-dcb"></a>Data Center Bridging \(DCB\)

>Si applica a: Windows Server (canale annuale e virgola), Windows Server 2016

È possibile utilizzare questo argomento per informazioni introduttive su Data Center Bridging \(DCB\).

DCB è una famiglia di standard \(IEEE\) Institute of Electrical and Electronics Engineers che abilita infrastrutture con convergenza nei centri dati, in cui archiviazione, dati in rete, comunicazione Inter\ processi \(IPC\) cluster e traffico di gestione condividono la stessa infrastruttura di rete Ethernet.

>[!NOTE]
>Oltre a questo argomento, è disponibile la seguente documentazione DCB
>
>- [Installare Data Center Bridging in Windows Server 2016 o Windows 10](dcb-install.md)
>- [Gestire Data Center Bridging (DCB)](dcb-manage.md)

DCB fornisce l'allocazione hardware\ della larghezza di banda a un tipo specifico di traffico di rete e migliora l'affidabilità del trasporto Ethernet con l'utilizzo del controllo di flusso basato sulla priority\.

Allocazione Hardware\ della larghezza di banda è essenziale se il traffico ignora il sistema operativo e viene eseguito l'offload di una scheda di rete convergente, che supportino \(iSCSI\) Internet Small Computer System Interface, Remote Direct Memory Access \(RDMA\) over Ethernet o Fiber Channel tramite \(FCoE\) Ethernet.

Controllo di flusso basato sulla Priority\ è essenziale se il protocollo di livello superiore, ad esempio Fiber Channel, presuppone un trasporto sottostante senza perdita di dati.

## <a name="dcb-protocols-and-management-options"></a>DCB protocolli e opzioni di gestione

DCB è costituito da set di protocolli di seguito. 

- Avanzato servizio trasmissione \(ETS\) – IEEE 802.1Qaz, che si basa sulle 802.1 q e 802.1 P standard
- Controllo di flusso priorità \(PFS\), IEEE 802.1 qbb 
- DCB Exchange Protocol \(DCBX\), IEEE 802.1AB, come estesa nel 802.1Qaz standard.

Il protocollo DCBX consente di configurare Data Center Bridging in un commutatore, che quindi può configurare automaticamente un dispositivo di fine, ad esempio un computer che esegue Windows Server 2016.

Se si preferisce per la gestione DCB da un commutatore, non devi installare DCB come funzionalità in Windows Server 2016, tuttavia questo approccio include alcune limitazioni.

Poiché DCBX possono comunicare solo la scheda di rete host di dimensioni di classe ETS e l'abilitazione di base alla priorità, tuttavia, gli host Windows Server in genere richiedono DCB è installato in modo che il traffico viene mappato a classi ETS.

Applicazioni di Windows non sono in genere progettate per gli scambi DCBX. Per questo motivo, l'host deve essere configurato separatamente dai commutatori di rete, ma con le stesse impostazioni.

Se si sceglie di gestire DCB da un commutatore, è comunque possibile visualizzare le configurazioni in Windows Server 2016 usando i comandi di Windows PowerShell.

##  <a name="important-dcb-functionality"></a>Funzionalità importanti di DCB

Ecco un elenco che sono riepilogate le funzionalità offerte da DCB.

1. Assicura l'interoperabilità tra schede di rete che supportano DCB\ e commutatori che supportano DCB\.

2. Fornisce un trasporto Ethernet senza perdita di dati tra un computer che esegue Windows Server 2016 e il commutatore adiacente, attivando il controllo di flusso basato sulla priority\ nella scheda di rete.

3. Offre la possibilità di allocare larghezza di banda per un controllo del traffico \(TC\) percentuale, in cui il TC possono essere costituiti da uno o più classi di traffico differenziate mediante indicatori \(priority\) classe di traffico 802.1p.

4. Consente agli amministratori di server o agli amministratori di rete assegnare un'applicazione a una specifica classe di traffico o priorità in base a protocolli noti, porta TCP/UDP nota o alla porta NetworkDirect utilizzata da tale applicazione.

5. Fornisce la gestione DCB tramite Windows PowerShell e Windows Server 2016 Windows Management Instrumentation \(WMI\). Per ulteriori informazioni, vedere la sezione [comandi di Windows PowerShell per DCB](#bkmk_wps) più avanti in questo argomento, oltre agli argomenti seguenti.
    - [Componenti DCB fornito dal sistema](https://msdn.microsoft.com/windows/hardware/drivers/network/system-provided-dcb-components)
    - [Requisiti di QoS NDIS per Data Center Bridging](https://msdn.microsoft.com/windows/hardware/drivers/network/ndis-qos-requirements-for-data-center-bridging)

6. Fornisce la gestione DCB tramite criteri di gruppo di Windows Server 2016.

7. Supporta la coesistenza di soluzioni \(QoS\) di Windows Server 2016 Quality of Service.

>[!NOTE]
>Prima di utilizzare qualsiasi RDMA sulla versione \(RoCE\) Converged Ethernet di RDMA, è necessario abilitare DCB. Anche se non è richiesto per le reti \(iWARP\) protocollo Internet Wide Area RDMA, il test ha determinato che tutte le tecnologie basate su Ethernet\ RDMA funzionano meglio con DCB. Per questo motivo, è consigliabile usare DCB per le distribuzioni di RDMA iWARP. Per ulteriori informazioni, vedere [diretta accesso memoria remota (RDMA) e Switch Embedded Teaming (SET)](../../../virtualization/hyper-v-virtual-switch/RDMA-and-Switch-Embedded-Teaming.md).

##  <a name="practical-applications-of-dcb"></a>Applicazioni pratiche di DCB

Molte organizzazioni utilizzano Fiber Channel \(FC\) storage area network \(SAN\) installazioni di grandi dimensioni per il servizio di archiviazione. Le SAN FC richiedono speciali schede di rete nel server e commutatori FC nella rete. Le organizzazioni in genere anche usano commutatori e schede di rete Ethernet.

Nella maggior parte dei casi, l'hardware FC è molto più costoso da distribuire hardware Ethernet e dunque comporta ingenti spese in conto. Inoltre, il requisito per separato Ethernet SAN FC scheda e commutatore hardware di rete e richiede ulteriore spazio, energia e capacità di raffreddamento nei centri dati comporta delle spese di esercizio aggiuntive, in corso.

Dal punto di vista dei costi, è più vantaggiosa per molte organizzazioni unire la tecnologia FC con la propria soluzione hardware basata su Ethernet\ per fornire servizi di rete dati sia all'archiviazione.

### <a name="using-dcb-for-an-ethernet-based-converged-fabric-for-storage-and-data-networking"></a>Utilizza DCB per un'infrastruttura convergente basata su Ethernet\ per l'archiviazione e i dati in rete

Per le organizzazioni che già dispone di una SAN FC di grandi dimensioni, ma desidera eseguire la migrazione evitare ulteriori investimenti in tecnologia FC, DCB consente di creare un cavo Ethernet basata su convergente dell'infrastruttura di archiviazione sia dati in rete. Convergenza DCB un'infrastruttura può ridurre il costo totale futuro di proprietà \(TCO\) e semplificare la gestione.

Per gli host che hanno già adottato o intendono adottare, iSCSI come soluzione di archiviazione, DCB può fornire prenotazione della larghezza di banda assistita mediante hardware\ per il traffico iSCSI, per garantire l'isolamento delle prestazioni. A differenza di altre soluzioni proprietarie, DCB è basata su standards\ e quindi relativamente facile da distribuire e gestire in un ambiente di rete eterogenea.

Un'implementazione Windows Server basato su 2016\ di DCB allevia molti dei problemi che possono verificarsi quando convergente fornite da \(OEMs\) original equipment manufacturer più soluzioni di infrastruttura.

Le implementazioni di soluzioni proprietarie fornite da più OEM potrebbero non interagire tra loro, potrebbe essere difficile da gestire e in genere avranno pianificazioni di manutenzione di un software diverso. 

Al contrario, Windows Server 2016 DCB è basato su standards\ ed è possibile distribuire e gestire Data Center Bridging in una rete eterogenea.

## <a name="bkmk_wps"></a>Comandi di Windows PowerShell per DCB

Sono disponibili i comandi di Windows PowerShell DCB per Windows Server 2016 e Windows Server 2012 R2. È possibile utilizzare tutti i comandi per Windows Server 2012 R2 in Windows Server 2016.

### <a name="windows-server-2016-windows-powershell-commands-for-dcb"></a>Comandi di Windows Server 2016 Windows PowerShell per DCB

L'argomento seguente per Windows Server 2016 fornisce le descrizioni di cmdlet di Windows PowerShell e la sintassi per tutti i \(DCB\) Data Center Bridging Quality of Service \ cmdlet \-specific (QoS\). Elenca i cmdlet in ordine alfabetico in base al verbo all'inizio del cmdlet.

- [Modulo DcbQoS](https://technet.microsoft.com/itpro/powershell/windows/dcbqos/dcbqos)

### <a name="windows-server-2012-r2-windows-powershell-commands-for-dcb"></a>Windows Server 2012 R2 comandi di Windows PowerShell per DCB

L'argomento seguente per Windows Server 2012 R2 fornisce le descrizioni di cmdlet di Windows PowerShell e la sintassi per tutti i \(DCB\) Data Center Bridging Quality of Service \ cmdlet \-specific (QoS\). Elenca i cmdlet in ordine alfabetico in base al verbo all'inizio del cmdlet.

- [Data Center Bridging (DCB) cmdlet per qualità del servizio (QoS) in Windows PowerShell](https://technet.microsoft.com/library/hh967440.aspx)
