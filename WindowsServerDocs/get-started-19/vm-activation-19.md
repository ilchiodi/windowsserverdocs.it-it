---
title: Attivazione automatica della macchina virtuale
TOCTitle: Automatic VM Activation
description: Come attivare macchine virtuali in Windows Server 2019, Windows Server 2016 e Windows Server 2012 R2
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: server-general
ms.tgt_pltfrm: na
ms.topic: article
author: lizap
ms.author: elizapo
ms.localizationpriority: medium
ms.openlocfilehash: 4d1cdcfe8325a246e2752e1d1f2a3ad536598658
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 09/10/2019
ms.locfileid: "70868573"
---
# <a name="automatic-virtual-machine-activation"></a>Attivazione automatica della macchina virtuale

> Si applica a: Windows Server 2019, Windows Server (Canale semestrale), Windows Server 2016, Windows Server 2012 R2

L'attivazione automatica della macchina virtuale funziona come una prova d'acquisto per assicurare che i prodotti Windows vengano usati nel rispetto dei diritti di utilizzo del prodotto e delle Condizioni di licenza software Microsoft.

L'attivazione automatica della macchina virtuale consente di installare le macchine virtuali in un server Windows correttamente attivato, senza dover gestire i codici Product Key per ogni singola macchina virtuale, anche in ambienti disconnessi. Con questa funzionalità, l'attivazione della macchina virtuale viene associata al server di virtualizzazione dotato di licenza e la macchina virtuale viene attivata all'avvio. Questa funzionalità offre inoltre rapporti in tempo reale sull'utilizzo e dati cronologici sullo stato della licenza della macchina virtuale. I dati dei rapporti e della tracciabilità sono disponibili nel server di virtualizzazione.

## <a name="practical-applications"></a>Applicazioni pratiche

Nei server di virtualizzazione attivati mediante i contratti multilicenza o le licenze OEM, l'attivazione automatica della macchina virtuale offre diversi vantaggi.

I responsabili della gestione di Server Datacenter possono utilizzare l'attivazione automatica della macchina virtuale per eseguire le operazioni seguenti:

  - Attivare macchine virtuali in posizioni remote

  - Attivare macchine virtuali con o senza una connessione Internet

  - Tenere traccia dell'utilizzo e delle licenze delle macchine virtuali dal server di virtualizzazione, senza che siano necessari diritti di accesso alle macchine virtuali

Non è necessario gestire codici Product Key o leggere i codici riportati in adesivi sui server. La macchina virtuale viene attivata e continua a funzionare anche in caso di migrazione in un array di server di virtualizzazione.

I partner con contratti SPLA (Service Provider License Agreement) e altri provider di hosting non devono condividere i codici Product Key con i tenant o accedere a una macchina virtuale di un tenant per attivarla. L'attivazione della macchina virtuale è invisibile per il tenant quando si utilizza l'attivazione automatica della macchina virtuale. I provider di hosting possono utilizzare i log del server per verificare la conformità delle licenze e tenere traccia della cronologia di utilizzo dei client.

## <a name="system-requirements"></a>Requisiti di sistema

Per l'attivazione automatica della macchina virtuale è necessario un server di virtualizzazione Microsoft con Windows Server 2019 Datacenter, Windows Server 2016 Datacenter o Windows Server 2012 R2. 

Ecco i guest che i diversi host possono attivare:

|Versione host del server|Windows Server 2019|Windows Server 2016|Windows Server 2012 R2|
|-|-|-|-|
|Windows Server 2019|X|X|X|
|Windows Server 2016| |X|X|
|Windows Server 2012 R2| ||X|

Tenere presente che attivano tutte le edizioni (Datacenter, Standard o Essentials).

Questo strumento non funziona con altri server di virtualizzazione.

## <a name="how-to-implement-avma"></a>Come implementare l'attivazione automatica della macchina virtuale

1.  In un server di virtualizzazione Windows Server Datacenter installare e configurare il ruolo Microsoft Hyper-V Server. Per altre informazioni, vedere [Installare Hyper-V Server](../virtualization/hyper-v/get-started/install-the-hyper-v-role-on-windows-server.md).

2.  [Creare una macchina virtuale](../virtualization/hyper-v/get-started/create-a-virtual-machine-in-hyper-v.md) e installare un sistema operativo server supportato.

3.  Installare la chiave di attivazione automatica della macchina virtuale nella macchina virtuale. Da un prompt dei comandi con privilegi elevati eseguire il comando seguente:
    
    ``` 
    slmgr /ipk <AVMA_key>  
    ```

La macchina virtuale attiverà automaticamente la licenza in base al server di virtualizzazione.


> [!TIP]
> È inoltre possibile utilizzare le chiavi di attivazione automatica della macchina virtuale in qualsiasi file di installazione Unattend.exe.


## <a name="avma-keys"></a>Chiavi di attivazione automatica della macchina virtuale

Le chiavi di attivazione automatica della macchina virtuale seguenti possono essere utilizzate per Windows Server 2019.

|Edizione|   Chiave di attivazione automatica della macchina virtuale|
|-|-|
|Datacenter|    H3RNG-8C32Q-Q8FRX-6TDXV-WMBMW|
|Standard|  TNK62-RXVTB-4P47B-2D623-4GF74|
|Essentials|    2CTP7-NHT64-BP62M-FV6GG-HFV28|
 
Le chiavi di attivazione automatica della macchina virtuale seguenti possono essere utilizzate per Windows Server 1809.

|Edizione|   Chiave di attivazione automatica della macchina virtuale|
|-|-|
|Datacenter|    H3RNG-8C32Q-Q8FRX-6TDXV-WMBMW|
|Standard|  TNK62-RXVTB-4P47B-2D623-4GF74|

Le chiavi di attivazione automatica della macchina virtuale seguenti possono essere utilizzate per Windows Server versione 1803 e 1709.

|Edizione|Chiave di attivazione automatica della macchina virtuale|
|-|-|
|Datacenter|TMJ3Y-NTRTM-FJYXT-T22BY-CWG3J|
|Standard|C3RCX-M6NRP-6CXC9-TW2F2-4RHYD|


Le chiavi di attivazione automatica della macchina virtuale seguenti possono essere utilizzate per Windows Server 2016.

|Edizione|Chiave di attivazione automatica della macchina virtuale|
|-|-|
|Datacenter|TMJ3Y-NTRTM-FJYXT-T22BY-CWG3J|
|Standard|C3RCX-M6NRP-6CXC9-TW2F2-4RHYD|
|Essentials|B4YNW-62DX9-W8V6M-82649-MHBKQ|


Le chiavi di attivazione automatica della macchina virtuale possono essere utilizzate per Windows Server 2012 R2.

|Edizione|Chiave di attivazione automatica della macchina virtuale|
|-|-|
|Datacenter|Y4TGP-NPTV9-HTC2H-7MGQ3-DV4TW|
|Standard|DBGBW-NPF86-BJVTX-K3WKJ-MTB6V|
|Essentials|K2XGM-NMBT3-2R6Q8-WF2FK-P36R2|

## <a name="reporting-and-tracking"></a>Rapporti e tracciabilità

Il Registro di sistema (coppia chiave-valore) nel server di virtualizzazione fornisce dati di tracciabilità in tempo reale per i sistemi operativi guest. Poiché le chiavi del Registro di sistema si spostano con la macchina virtuale, è possibile ottenere anche le informazioni relative alle licenze. Per impostazione predefinita, la coppia chiave-valore restituisce informazioni sulla macchina virtuale che includono:

  - Nome di dominio completo

  - Sistema operativo e Service Pack installati

  - Architettura del processore

  - Indirizzi di rete IPv4 e IPv6

  - Indirizzi RDP

Per altre informazioni su come recuperare queste informazioni, vedere[Hyper-V Script: Looking at KVP GuestIntrinsicExchangeItems](http://blogs.msdn.com/b/virtual_pc_guy/archive/2008/11/18/hyper-v-script-looking-at-kvp-guestintrinsicexchangeitems.aspx) (Script Hyper-V: cercare KVP GuestIntrinsicExchangeItems).


> [!NOTE]
> I dati della coppia chiave-valore non sono protetti. La coppia chiave-valore può essere modificata e le modifiche non vengono monitorate.



> [!IMPORTANT]
> I dati della coppia chiave-valore devono essere rimossi se la chiave di attivazione automatica della macchina virtuale viene sostituita con un altro codice Product Key (codice Product Key per attivazione singola, OEM o con contratto multilicenza).


I dati cronologici sulle richieste di attivazione automatica della macchina virtuale sono disponibili in un file di log nel server di virtualizzazione (EventID 12310).

Poiché il processo di attivazione automatica della macchina virtuale è trasparente, non vengono visualizzati messaggi di errore. Gli eventi seguenti vengono comunque acquisiti in un file di log nelle macchine virtuali (EventID 12309).

|Notifica|Descrizione|
|-|-|
|Attivazione automatica della macchina virtuale riuscita|La macchina virtuale è stata attivata.|
|Host non valido|Il server di virtualizzazione non risponde. È possibile che questo errore si verifichi quando il server non esegue una versione di Windows supportata.|
|Dati non validi|Questo problema è in genere il risultato di una comunicazione non riuscita tra il server di virtualizzazione e la macchina virtuale, spesso causata da dati danneggiati, crittografati o non corrispondenti.|
|Attivazione negata|Il server di virtualizzazione non è in grado di attivare il sistema operativo guest perché l'ID dell'attivazione automatica della macchina virtuale non corrisponde.|

