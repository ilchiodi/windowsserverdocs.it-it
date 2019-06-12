---
title: Requisiti di sistema per Hyper-V in Windows Server
description: Vengono elencati i requisiti hardware e firmware per Hyper-V in Windows Server
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: bc4a4971-f727-40cd-91f5-2ee6d24b54cb
author: KBDAzure
ms.author: kathydav
ms.date: 9/30/2016
ms.openlocfilehash: 97fb1b9003705ba8ad26c2b3e71eda34e88642ee
ms.sourcegitcommit: 6ef4986391607bb28593852d06cc6645e548a4b3
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 06/07/2019
ms.locfileid: "66812613"
---
# <a name="system-requirements-for-hyper-v-on-windows-server"></a>Requisiti di sistema per Hyper-V in Windows Server

>Si applica a: Windows Server 2016, Microsoft Hyper-V Server 2016, Windows Server 2019, Microsoft Hyper-V Server 2019

Hyper-V presenta specifici requisiti hardware e alcune funzionalità di Hyper-V sono requisiti aggiuntivi. Utilizzare i dettagli in questo articolo per stabilire i requisiti che il sistema deve soddisfare in modo è possibile utilizzare il modo in cui di che si prevede Hyper-V. Quindi, esaminare il [catalogo di Windows Server](https://www.windowsservercatalog.com/). Tenere presente che i requisiti per Hyper-V superano i requisiti minimi generali per Windows Server 2016 poiché un ambiente di virtualizzazione richiede più risorse di elaborazione.

Se si utilizza Hyper-V, è probabile che è possibile utilizzare l'hardware esistente. I requisiti hardware generali non sono state modificate in modo significativo da Windows Server 2012 R2.  Tuttavia, è necessario hardware più recente utilizza schermato macchine virtuali o assegnazione dispositivo discreti. Queste funzionalità si basano sul supporto di hardware specifico, come descritto di seguito. A parte ciò, la differenza principale nell'hardware è l'indirizzo di secondo livello traduzione (SLAT) viene ora richiesto anziché consigliato.

Per informazioni dettagliate sulle configurazioni supportate massime per Hyper-V, ad esempio il numero di macchine virtuali in esecuzione, vedere [pianificare la scalabilità di Hyper-V in Windows Server 2016](plan/Plan-for-Hyper-V-scalability-in-Windows-Server-2016.md). Viene descritta l'elenco dei sistemi operativi è possibile eseguire nelle macchine virtuali in [Windows supportati i sistemi operativi guest per Hyper-V in Windows Server](Supported-Windows-guest-operating-systems-for-Hyper-V-on-Windows.md).

## <a name="general-requirements"></a>Requisiti generali

Indipendentemente dalla funzionalità di Hyper-V che si desidera utilizzare, è necessario:

- Un processore a 64 bit con la traduzione da indirizzo di secondo livello (SLAT). Per installare i componenti di virtualizzazione Hyper-V, ad esempio Windows hypervisor, il processore deve disporre SLAT. Tuttavia, non è necessario per installare strumenti di gestione di Hyper-V come Virtual Machine Connection (VMConnect), gestione di Hyper-V e i cmdlet di Hyper-V per Windows PowerShell. Vedere "Come verificare la presenza di requisiti di Hyper-V," più avanti, per determinare se il processore ha SLAT.

- Estensioni di macchina Virtuale in modalità di monitoraggio

- Memoria insufficiente - piano per *almeno* 4 GB di RAM. Quantità di memoria è migliore. È necessario memoria sufficiente per l'host e tutte le macchine virtuali che si desidera eseguire contemporaneamente.

- Supporto per la virtualizzazione attivato nel BIOS o UEFI:

  - Virtualizzazione assistita mediante hardware. È disponibile nei processori che includono un'opzione di virtualizzazione, in particolare i processori con tecnologia di virtualizzazione AMD (AMD-V) o Intel Virtualization Technology (Intel VT).

  - È necessario rendere disponibile e abilitare Protezione esecuzione programmi applicata a livello di hardware. Per i sistemi Intel, si tratta XD bit (execute disable bit). Per i sistemi AMD, non si tratta di NX bit (execute).

## <a name="how-to-check-for-hyper-v-requirements"></a>Come verificare la presenza di requisiti di Hyper-V

Aprire Windows PowerShell o un prompt dei comandi e digitare:

```cmd
Systeminfo.exe
```

Scorrere fino alla sezione requisiti di Hyper-V per esaminare il report.

## <a name="requirements-for-specific-features"></a>Requisiti per funzionalità specifiche

Ecco i requisiti per l'assegnazione di dispositivo discreti e macchine virtuali schermate. Per una descrizione di queste funzionalità, vedere [quali sono le novità in Hyper-V in Windows Server](What-s-new-in-Hyper-V-on-Windows.md).

### <a name="discrete-device-assignment"></a>Assegnazione di dispositivi discreti

**Host** requisiti sono simili ai requisiti esistenti per la funzionalità di SR-IOV in Hyper-V.

- Il processore deve disporre Extended pagina tabella (Accetta di entrambi Intel) o annidati pagina tabella (TNP AMD).

- Il chipset necessario:

  - Interrupt remapping - Intel VT-d con la funzionalità di mapping di Interrupt (VT-d2) o qualsiasi versione di unità di gestione di AMD i/o memoria (MMU dei / o).

  - Modifica del mapping DMA - Intel VT-d con le convalide in coda o qualsiasi MMU dei / o AMD.

  - Servizi di controllo di accesso (ACS) sulle porte radice PCI Express.

- Le tabelle del firmware devono esporre il / o MMU nell'hypervisor di Windows. Si noti che questa funzionalità potrebbe essere disabilitata nel BIOS o UEFI. Per istruzioni, vedere la documentazione dell'hardware o contattare il produttore dell'hardware.

**Dispositivi** necessario express GPU o memoria non volatile (NVMe). Per GPU, solo determinati dispositivi supportano l'assegnazione di dispositivo discreti. Per verificare, vedere la documentazione dell'hardware o contattare il produttore dell'hardware. Per informazioni dettagliate su questa funzionalità, compreso l'utilizzo e considerazioni, vedere il post "[discreti dispositivo assegnazione - descrizione e lo sfondo](https://blogs.technet.com/b/virtualization/archive/2015/11/19/discrete-device-assignment.aspx)" nel blog di virtualizzazione.

### <a name="shielded-virtual-machines"></a>Macchine virtuali schermate

Queste macchine virtuali si basano sulla sicurezza basata sulla virtualizzazione e sono disponibili a partire da Windows Server 2016.

**Host** requisiti sono:

- UEFI 2.3.1c - supporta avvio protetto, misurato

  Le seguenti due sono facoltativi per la sicurezza basata su virtualizzazione in generale, ma necessari per l'host, se si desidera che la protezione di che queste funzionalità offrono:

- TPM 2.0 - consente di proteggere gli asset di sicurezza della piattaforma
- IOMMU (Intel VT-D) - l'hypervisor può fornire protezione (DMA) l'accesso diretto a memoria

**Macchina virtuale** requisiti sono:

- Seconda generazione
- Windows Server 2012 o versioni successive del sistema operativo guest

