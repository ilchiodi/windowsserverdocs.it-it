---
title: Data Center Bridging (DCB)
description: Questo argomento può essere usato per informazioni introduttive su Data Center Bridging in Windows Server 2016.
ms.prod: windows-server
ms.technology: networking
ms.topic: article
ms.assetid: da58f312-bd3b-4bb6-98ca-6177869dd6ad
manager: brianlic
ms.author: lizross
author: eross-msft
ms.openlocfilehash: 5401f0409ce46e5b7a6da32e1e0a914581956279
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/26/2020
ms.locfileid: "80312739"
---
# <a name="data-center-bridging-dcb"></a>Data Center Bridging \(DCB\)

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016

Per informazioni introduttive su Data Center Bridging \(\)DCB, è possibile usare questo argomento.

DCB è una suite di tecnici dell'Istituto di elettrotecnica e elettronica \(gli standard IEEE\) che consentono infrastrutture con convergenza nei data center, in cui l'archiviazione, la rete di dati, la comunicazione tra\-processi del cluster \(IPC\)e il traffico di gestione condividono la stessa infrastruttura di rete Ethernet.

>[!NOTE]
>Oltre a questo argomento, è disponibile la documentazione di DCB seguente
>
>- [Installare DCB in Windows Server 2016 o Windows 10](dcb-install.md)
>- [Gestire Data Center Bridging (DCB)](dcb-manage.md)

DCB fornisce l'allocazione della larghezza di banda basata su\-hardware a un tipo specifico di traffico di rete e migliora l'affidabilità del trasporto Ethernet con l'utilizzo del controllo di flusso basato sulla priorità\-.

L'allocazione della larghezza di banda basata\-hardware è essenziale se il traffico ignora il sistema operativo e viene trasferito a una scheda di rete convergente, che potrebbe supportare l'interfaccia Internet Small Computer System \(iSCSI\), accesso diretto a memoria remota \(RDMA\) over Ethernet o Fiber Channel over Ethernet \(FCoE\).

Il controllo di flusso basato sulla priorità\-è essenziale se il protocollo di livello superiore, ad esempio Fiber Channel, presuppone un trasporto sottostante senza perdita di perdite.

## <a name="dcb-protocols-and-management-options"></a>Protocolli e opzioni di gestione di DCB

DCB è costituito dal set di protocolli seguente. 

- Servizio di trasmissione avanzato \(ETS\)-IEEE 802.1 qaz, che si basa sugli standard 802.1 P e 802.1 Q
- Controllo del flusso di priorità \(PFS\), IEEE 802.1 QBB 
- Il protocollo DCB Exchange \(DCBX\), IEEE 802.1 AB, come esteso nello standard 802.1 Qaz.

Il protocollo DCBX consente di configurare DCB in uno switch, che può quindi configurare automaticamente un dispositivo finale, ad esempio un computer che esegue Windows Server 2016.

Se si preferisce gestire DCB da un'opzione, non è necessario installare DCB come funzionalità di Windows Server 2016, tuttavia questo approccio include alcune limitazioni.

Poiché DCBX è in grado di informare solo la scheda di rete host delle dimensioni della classe ETS e l'abilitazione del PFC, tuttavia, gli host Windows Server richiedono in genere che DCB sia installato in modo che venga eseguito il mapping del traffico alle classi ETS.

Le applicazioni Windows non sono in genere progettate per partecipare agli scambi DCBX. Per questo motivo, l'host deve essere configurato separatamente dai commutatori di rete, ma con impostazioni identiche.

Se si sceglie di gestire DCB da un'opzione, è comunque possibile visualizzare le configurazioni in Windows Server 2016 usando i comandi di Windows PowerShell.

##  <a name="important-dcb-functionality"></a>Funzionalità importanti di DCB

Nell'elenco seguente sono riepilogate le funzionalità offerte da DCB.

1. Fornisce l'interoperabilità tra DCB\-schede di rete in grado di supportare DCB\-commutatori idonei.

2. Fornisce un trasporto Ethernet senza perdita di risorse tra un computer che esegue Windows Server 2016 e il commutatore adiacente, attivando il controllo di flusso basato sulla priorità\-sulla scheda di rete.

3. Consente di allocare larghezza di banda a un controllo del traffico \(TC\) in base alla percentuale, in cui il TC può essere costituito da una o più classi di traffico differenziate dalla classe di traffico 802.1 p \(indicatori di\) di priorità.

4. Consente agli amministratori di server o di rete di assegnare un'applicazione a una specifica classe di traffico o priorità in base a protocolli noti, a una porta TCP/UDP nota o alla porta NetworkDirect utilizzata da tale applicazione.

5. Fornisce la gestione di DCB tramite Windows Server 2016 Strumentazione gestione Windows \(WMI\) e Windows PowerShell. Per ulteriori informazioni, vedere la sezione [comandi di Windows PowerShell per DCB](#bkmk_wps) più avanti in questo argomento, oltre ai seguenti argomenti.
    - [Componenti DCB forniti dal sistema](https://msdn.microsoft.com/windows/hardware/drivers/network/system-provided-dcb-components)
    - [Requisiti QoS di NDIS per Data Center Bridging](https://msdn.microsoft.com/windows/hardware/drivers/network/ndis-qos-requirements-for-data-center-bridging)

6. Fornisce la gestione di DCB tramite Windows Server 2016 Criteri di gruppo.

7. Supporta la coesistenza di Windows Server 2016 Quality of Service \(QoS\) soluzioni.

>[!NOTE]
>Prima di usare qualsiasi RDMA Ethernet \(RoCE\) versione di RDMA, è necessario abilitare DCB. Sebbene non sia necessario per il protocollo Internet wide area RDMA \(iWARP\) reti, il test ha determinato che tutte le tecnologie RDMA basate su\-Ethernet funzionano meglio con DCB. Per questo motivo, è consigliabile usare DCB per le distribuzioni RDMA di iWARP. Per ulteriori informazioni, vedere [accesso diretto a memoria remota (RDMA) e switch Embedded Teaming (set)](../../../virtualization/hyper-v-virtual-switch/RDMA-and-Switch-Embedded-Teaming.md).

##  <a name="practical-applications-of-dcb"></a>Applicazioni pratiche di DCB

Molte organizzazioni dispongono di grandi fibre channel \(FC\) la rete di archiviazione \(SAN\) installazioni per il servizio di archiviazione. Le SAN FC richiedono speciali schede di rete nei server e commutatori FC nella rete. Queste organizzazioni usano in genere anche le schede di rete Ethernet e i commutatori.

Nella maggior parte dei casi, l'hardware FC è molto più costoso da distribuire rispetto all'hardware Ethernet, il che comporta un numero elevato di spese di capitale. Inoltre, il requisito per la scheda di rete Ethernet e la scheda di rete SAN FC e l'hardware del commutere richiede spazio aggiuntivo, potenza e capacità di raffreddamento in un Data Center, il che comporta costi operativi aggiuntivi e continui.

Dal punto di vista dei costi, è vantaggioso per molte organizzazioni unire la tecnologia FC con la soluzione hardware\-basata su Ethernet per fornire servizi di archiviazione e di rete di dati.

### <a name="using-dcb-for-an-ethernet-based-converged-fabric-for-storage-and-data-networking"></a>Uso di DCB per un'infrastruttura convergente basata su\-Ethernet per l'archiviazione e la rete dati

Per le organizzazioni che dispongono già di una SAN FC di grandi dimensioni, ma che desiderano eseguire la migrazione da ulteriori investimenti nella tecnologia FC, DCB consente di creare un'infrastruttura convergente basata su Ethernet sia per l'archiviazione che per la rete dati. Un'infrastruttura convergente DCB può ridurre il costo totale di proprietà futuro \(\) TCO e semplificare la gestione.

Per i provider di hosting che hanno già adottato o che prevedono di adottare iSCSI come soluzione di archiviazione, DCB può fornire l'hardware\-prenotazione della larghezza di banda assistita per il traffico iSCSI per garantire l'isolamento delle prestazioni. Diversamente da altre soluzioni proprietarie, DCB è basato su standard\-e quindi relativamente facile da distribuire e gestire in un ambiente di rete eterogeneo.

Un'implementazione di DCB basata su Windows Server 2016\-allevia molti dei problemi che possono verificarsi quando le soluzioni di infrastruttura con convergenza vengono fornite da più produttori di apparecchiature originali \(\)OEM.

Le implementazioni di soluzioni proprietarie fornite da più OEM potrebbero non interagire tra loro, potrebbe essere difficile da gestire e in genere avranno pianificazioni diverse per la manutenzione del software. 

Al contrario, Windows Server 2016 DCB è basato su standard\-ed è possibile distribuire e gestire DCB in una rete eterogenea.

## <a name="windows-powershell-commands-for-dcb"></a><a name="bkmk_wps"></a>Comandi di Windows PowerShell per DCB

Sono disponibili comandi di Windows PowerShell DCB per Windows Server 2016 e Windows Server 2012 R2. È possibile usare tutti i comandi per Windows Server 2012 R2 in Windows Server 2016.

### <a name="windows-server-2016-windows-powershell-commands-for-dcb"></a>Comandi di Windows PowerShell per Windows Server 2016 per DCB

L'argomento seguente per Windows Server 2016 fornisce le descrizioni e la sintassi dei cmdlet di Windows PowerShell per tutti i Data Center Bridging \(DCB\) qualità del servizio \(QoS\)\-cmdlet specifici. I cmdlet sono elencati in ordine alfabetico, in base al verbo iniziale.

- [Modulo DcbQoS](https://technet.microsoft.com/itpro/powershell/windows/dcbqos/dcbqos)

### <a name="windows-server-2012-r2-windows-powershell-commands-for-dcb"></a>Comandi di Windows PowerShell per Windows Server 2012 R2 per DCB

L'argomento seguente per Windows Server 2012 R2 fornisce le descrizioni e la sintassi dei cmdlet di Windows PowerShell per tutti i Data Center Bridging \(DCB\) qualità del servizio \(QoS\)\-cmdlet specifici. I cmdlet sono elencati in ordine alfabetico, in base al verbo iniziale.

- [Cmdlet di qualità del servizio (QoS) di Data Center Bridging (DCB) in Windows PowerShell](https://technet.microsoft.com/library/hh967440.aspx)
