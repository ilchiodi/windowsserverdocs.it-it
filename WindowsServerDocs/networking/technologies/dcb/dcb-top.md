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
ms.openlocfilehash: 7cb488192a873743db27d9c1d09c5912bc810bb8
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59834182"
---
# <a name="data-center-bridging-dcb"></a>Data Center Bridging \(DCB\)

>Si applica a: Windows Server (canale semestrale), Windows Server 2016

È possibile usare questo argomento per informazioni introduttive su Data Center Bridging \(DCB\).

DCB è una suite di Institute of Electrical and Electronics Engineers \(IEEE\) standard che abilita infrastrutture con convergenza nei centri dati, in cui archiviazione, rete, dati cluster Inter\-comunicazione di processi \(IPC\), e tutto il traffico di gestione condividono la stessa infrastruttura di rete Ethernet.

>[!NOTE]
>Oltre a questo argomento, è disponibile la seguente documentazione di Data Center Bridging
>
>- [Installare i Data Center Bridging in Windows Server 2016 o Windows 10](dcb-install.md)
>- [Gestire Data Center Bridging (DCB)](dcb-manage.md)

DCB fornisce hardware\-basata di allocazione della larghezza di banda a un tipo specifico di traffico di rete e migliora l'affidabilità del trasporto Ethernet con l'uso di priorità\-basato su controllo di flusso.

Hardware\-allocazione della larghezza di banda in base è essenziale se il traffico ignora il sistema operativo e viene eseguito l'offload a una scheda di rete convergente, che potrebbe supportare Internet Small Computer System Interface \(iSCSI\), Accesso diretto a memoria remota \(RDMA\) over Ethernet o Fiber Channel over Ethernet \(FCoE\).

Priorità\-controllo di flusso basato su è essenziale se il protocollo di livello superiore, ad esempio Fiber Channel, presuppone un trasporto sottostante senza perdita di dati.

## <a name="dcb-protocols-and-management-options"></a>DCB protocolli e opzioni di gestione

DCB è costituito da set di protocolli di seguito. 

- Migliorato il servizio di trasmissione \(ETS\) – IEEE 802.1Qaz, che si basa sulle 802.1P e 802.1Q standard
- Controllo di flusso prioritario \(PFS\), IEEE 802.1Qbb 
- Protocollo di scambio del Data Center Bridging \(DCBX\), IEEE 802.1AB, allargata il 802.1Qaz standard.

Il protocollo DCBX consente di configurare Data Center Bridging in un commutatore, che quindi può configurare automaticamente un dispositivo finale, ad esempio un computer che esegue Windows Server 2016.

Se si preferisce gestire Data Center Bridging da un'opzione, non devi installare DCB come funzionalità in Windows Server 2016, tuttavia questo approccio presenta alcune limitazioni.

Poiché DCBX può informare solo la scheda di rete host di abilitazione della base alla priorità e dimensioni di classe ETS, tuttavia, gli host Windows Server in genere richiedono che DCB è installato in modo che il traffico viene eseguito il mapping a classi ETS.

Le applicazioni di Windows non sono in genere progettate per gli scambi DCBX. Per questo motivo, l'host deve essere configurato separatamente dai commutatori di rete, ma con impostazioni identiche.

Se si sceglie di gestire Data Center Bridging da un commutatore, è comunque possibile visualizzare le configurazioni in Windows Server 2016 usando i comandi di Windows PowerShell.

##  <a name="important-dcb-functionality"></a>Importanti funzionalità di Data Center Bridging

Nell'elenco seguente sono riepilogate le funzionalità offerte da DCB.

1. Assicura l'interoperabilità tra Data Center Bridging\-schede di rete in grado di supportare e Data Center Bridging\-commutatori che supportano.

2. Fornisce un trasporto Ethernet senza perdita di dati tra un computer che esegue Windows Server 2016 e il commutatore adiacente, attivando la priorità\-basato su controllo di flusso per la scheda di rete.

3. Offre la possibilità di allocare larghezza di banda per un controllo del traffico \(TC\) percentuale, in cui il che potrebbe consistere di uno o più classi di traffico differenziate mediante la classe di traffico 802.1P \(priorità\) indicatori.

4. Consente agli amministratori di server o di rete di assegnare un'applicazione a una specifica classe di traffico o priorità in base a protocolli noti, a una porta TCP/UDP nota o alla porta NetworkDirect utilizzata da tale applicazione.

5. Fornisce la gestione DCB tramite Strumentazione gestione Windows di Windows Server 2016 \(WMI\) e Windows PowerShell. Per altre informazioni, vedere la sezione [comandi di Windows PowerShell per il Data Center Bridging](#bkmk_wps) più avanti in questo argomento, oltre agli argomenti seguenti.
    - [Componenti di Data Center Bridging fornita dal sistema](https://msdn.microsoft.com/windows/hardware/drivers/network/system-provided-dcb-components)
    - [NDIS requisiti QoS per il Data Center Bridging](https://msdn.microsoft.com/windows/hardware/drivers/network/ndis-qos-requirements-for-data-center-bridging)

6. Fornisce la gestione DCB tramite criteri di gruppo di Windows Server 2016.

7. Supporta la coesistenza di Windows Server 2016 qualità del servizio \(QoS\) soluzioni.

>[!NOTE]
>Prima di usare qualsiasi RDMA over Converged Ethernet \(RoCE\) versione RDMA, è necessario abilitare DCB. Sebbene non sia necessario per il protocollo Internet ampia Area RDMA \(iWARP\) ha determinato che le reti, test Ethernet tutti\-basato lavoro le tecnologie RDMA migliore con DCB. Per questo motivo, è consigliabile usare DCB per le distribuzioni di RDMA iWARP. Per altre informazioni, vedere [diretta accesso memoria remota (RDMA) e Switch Embedded Teaming (SET)](../../../virtualization/hyper-v-virtual-switch/RDMA-and-Switch-Embedded-Teaming.md).

##  <a name="practical-applications-of-dcb"></a>Applicazioni pratiche del Data Center Bridging

Molte organizzazioni dispongono di grandi dimensioni Fiber Channel \(FC\) SAN \(SAN\) installazioni per il servizio di archiviazione. Le SAN FC richiedono speciali schede di rete nei server e commutatori FC nella rete. Queste organizzazioni utilizzano in genere anche commutatori e nelle schede di rete Ethernet.

Nella maggior parte dei casi, hardware FC è molto più costoso da distribuire rispetto all'hardware Ethernet, con conseguente delle spese di grandi dimensioni. Inoltre, il requisito per separato Ethernet e SAN FC schede e commutatori hardware rete richiede spazio aggiuntivo, potenza e la capacità in un Data Center, con conseguente aggiuntive delle spese di raffreddamento.

Relativamente ai costi, è consigliabile per molte organizzazioni unire la tecnologia FC loro Ethernet\-soluzione hardware per fornire dati servizi di rete sia all'archiviazione basata su.

### <a name="using-dcb-for-an-ethernet-based-converged-fabric-for-storage-and-data-networking"></a>Uso di Data Center Bridging per un cavo Ethernet\-basati su infrastruttura con convergenza per l'archiviazione e dati in rete

Per le organizzazioni che già dispone di una SAN FC di grandi dimensioni, ma intendono eseguire la migrazione di evitare ulteriori investimenti in tecnologia FC, DCB consente di compilare un cavo Ethernet basato convergente dell'infrastruttura di archiviazione e dati in rete. Un Data Center Bridging converged fabric può ridurre il costo totale futuro di proprietà \(TCO\) e semplificare la gestione.

Servizi di hosting che hanno già adottato o intendono adottare, iSCSI come soluzione di archiviazione, DCB può fornire hardware\-assistito prenotazione della larghezza di banda per il traffico iSCSI garantire l'isolamento delle prestazioni. E a differenza di altre soluzioni proprietarie, DCB è standard\-basato - e quindi relativamente facile da distribuire e gestire in un ambiente di rete eterogenea.

Windows Server 2016\-implementazione di DCB basata su allevia molti dei problemi che possono verificarsi quando convergente soluzioni di infrastruttura fornite da più gli original equipment manufacturer \(OEM\).

Le implementazioni di soluzioni proprietarie fornite da più OEM potrebbero non interagire tra loro, potrebbero essere difficili da gestire e in genere avranno pianificazioni di manutenzione software diverso. 

Al contrario, Windows Server 2016 Data Center Bridging è standard\-basato, ed è possibile distribuire e gestire Data Center Bridging in una rete eterogenea.

## <a name="bkmk_wps"></a>Comandi di Windows PowerShell per il Data Center Bridging

Sono disponibili comandi DCB Windows PowerShell per Windows Server 2016 e Windows Server 2012 R2. È possibile usare tutti i comandi di Windows Server 2012 R2 in Windows Server 2016.

### <a name="windows-server-2016-windows-powershell-commands-for-dcb"></a>Comandi di Windows Server 2016 Windows PowerShell per Data Center Bridging

L'argomento seguente per Windows Server 2016 fornisce descrizioni dei cmdlet di Windows PowerShell e la sintassi per tutti i Data Center Bridging \(DCB\) Quality of Service \(QoS\)\-cmdlet specifici. I cmdlet sono elencati in ordine alfabetico, in base al verbo presente all'inizio del cmdlet.

- [DcbQoS Module](https://technet.microsoft.com/itpro/powershell/windows/dcbqos/dcbqos)

### <a name="windows-server-2012-r2-windows-powershell-commands-for-dcb"></a>Comandi Windows Server 2012 R2 Windows PowerShell per Data Center Bridging

L'argomento seguente per Windows Server 2012 R2 fornisce descrizioni dei cmdlet di Windows PowerShell e la sintassi per tutti i Data Center Bridging \(DCB\) Quality of Service \(QoS\)\-cmdlet specifici. I cmdlet sono elencati in ordine alfabetico, in base al verbo presente all'inizio del cmdlet.

- [Data Center Bridging (DCB) cmdlet per qualità del servizio (QoS) in Windows PowerShell](https://technet.microsoft.com/library/hh967440.aspx)
