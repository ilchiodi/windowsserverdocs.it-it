---
title: Attivazione automatica macchina virtuale
TOCTitle: Automatic VM Activation
description: Come attivare le macchine virtuali in Windows Server 2019, Windows Server 2016 e Windows Server 2012 R2
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
ms.openlocfilehash: 62873140c8e114ba537dc4fd3ff7c44868c33243
ms.sourcegitcommit: ca5c80bdb903b282e292488172a7cc92546574c0
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 10/04/2018
ms.locfileid: "4375488"
---
# Attivazione automatica macchina virtuale

> Si applica a: Windows Server 2019, canale semestrale di Windows Server, Windows Server 2016, Windows Server 2012 R2

L'attivazione di macchina virtuale automatica (AVMA) agisce come un meccanismo di prova di acquisto, contribuisce a garantire che i prodotti Windows vengono utilizzati in conformità con i diritti di uso prodotto e le condizioni di licenza Software Microsoft.

AVMA consente di installare le macchine virtuali in un server Windows correttamente attivato senza la necessità di gestire i codici product key per ogni singola macchina virtuale, anche in ambienti disconnessi. AVMA associa l'attivazione della macchina virtuale con il server di virtualizzazione concesso in licenza e attiva la macchina virtuale all'avvio. AVMA fornisce inoltre la creazione di report in tempo reale in uso e i dati storici dello stato di licenza della macchina virtuale. Creazione di report e la tracciabilità dei dati è disponibile nel server di virtualizzazione.

## Applicazioni pratiche

Nel server di virtualizzazione che vengono attivate tramite contratti multilicenza o OEM le licenze, AVMA offre numerosi vantaggi.

Server datacenter responsabili possono usare AVMA per eseguire le operazioni seguenti:

  - Attivare le macchine virtuali in sedi remote

  - Attivare le macchine virtuali con o senza una connessione internet

  - Tenere traccia di utilizzo di macchina virtuale e le licenze dal server di virtualizzazione, senza la necessità di eventuali diritti di accesso nei sistemi virtualizzati

Ci sono senza codici product key per gestire e nessun adesivi nei server da leggere. La macchina virtuale viene attivata e continua a funzionare anche quando si è eseguita tra una matrice di server di virtualizzazione.

I partner di servizio Provider License Agreement (SPLA) e altri provider di hosting non è necessario condividere i codici product key con tenant o accedere alla macchina virtuale del tenant per attivarla. Attivazione della macchina virtuale è trasparente per il tenant quando viene usato AVMA. Provider di hosting può utilizzare i registri di server per verificare la conformità di licenza e tenere traccia della cronologia di utilizzo di client.

## Requisiti di sistema

AVMA richiede un Server di virtualizzazione di Microsoft che esegue Windows Server 2019 Datacenter, Windows Server 2016 Datacenter o Windows Server 2012 R2. 

Ecco i guest possono attivare gli host versione diversa:

|Versione dell'host server|Windows Server 2019|WindowsServer 2016|Windows Server 2012 R2|
|-|-|-|-|
|Windows Server 2019|X|X|X|
|WindowsServer 2016| |X|X|
|Windows Server 2012 R2| ||X|

Tieni presente che questi attivare tutte le edizioni (Datacenter, Standard o Essentials).

Questo strumento non funziona con altre tecnologie di virtualizzazione Server.

## Come implementare AVMA

1.  In un server di virtualizzazione di Windows Server Datacenter, installare e configurare il ruolo Server Hyper-V di Microsoft. Per altre informazioni, vedi [Installare Hyper-V Server](../virtualization/hyper-v/get-started/install-the-hyper-v-role-on-windows-server.md).

2.  [Creare una macchina virtuale](../virtualization/hyper-v/get-started/create-a-virtual-machine-in-hyper-v.md) e installare un sistema operativo server supportato su di esso.

3.  Installare la chiave AVMA nella macchina virtuale. Da un prompt dei comandi con privilegi elevati Esegui il comando seguente:
    
    ``` 
    slmgr /ipk <AVMA_key>  
    ```

La macchina virtuale verrà attivato automaticamente la licenza contro il server di virtualizzazione.


> [!TIP]
> È inoltre possibile utilizzare le chiavi AVMA in qualsiasi file di installazione Unattend.exe.


## Tasti AVMA

Le seguenti chiavi AVMA possono essere usate per Windows Server 2019.

|Edizione|   Chiave AVMA|
|-|-|
|Datacenter|    H3RNG-8C32Q-Q8FRX-6TDXV-WMBMW|
|Standard|  TNK62-RXVTB-4P47B - 2D 623-4GF74|
|Elementi essenziali|    2CTP7-NHT64-BP62M-FV6GG-HFV28|
 
Le seguenti chiavi AVMA possono essere usate per Windows Server, versione 1809.

|Edizione|   Chiave AVMA|
|-|-|
|Datacenter|    H3RNG-8C32Q-Q8FRX-6TDXV-WMBMW|
|Standard|  TNK62-RXVTB-4P47B - 2D 623-4GF74|

Le seguenti chiavi AVMA possono essere usate per Windows Server, versione 1803 e 1709.

|Edizione|Chiave AVMA|
|-|-|
|Datacenter|TMJ3Y-NTRTM-FJYXT-T22BY-CWG3J|
|Standard|C3RCX-M6NRP-6CXC9-TW2F2-4RHYD|


Le seguenti chiavi AVMA possono essere usate per Windows Server 2016.

|Edizione|Chiave AVMA|
|-|-|
|Datacenter|TMJ3Y-NTRTM-FJYXT-T22BY-CWG3J|
|Standard|C3RCX-M6NRP-6CXC9-TW2F2-4RHYD|
|Elementi essenziali|B4YNW-62DX9-W8V6M-82649-MHBKQ|


Le seguenti chiavi AVMA possono essere usate per Windows Server 2012 R2.

|Edizione|Chiave AVMA|
|-|-|
|Datacenter|Y4TGP-NPTV9-HTC2H-7MGQ3-DV4TW|
|Standard|DBGBW-NPF86-BJVTX-K3WKJ-MTB6V|
|Elementi essenziali|K2XGM-NMBT3-2R6Q8-WF2FK-P36R2|

## Reporting e monitoraggio

Il Registro di sistema (KVP) nel server di virtualizzazione fornisce i dati di monitoraggio in tempo reale per i sistemi operativi guest. Poiché la chiave del Registro di sistema si sposta con la macchina virtuale, è possibile ottenere anche le informazioni sulla licenza. Per impostazione predefinita il KVP restituisce informazioni nella macchina virtuale, tra cui le seguenti:

  - Nome di dominio completo

  - Sistema operativo e service pack installati

  - Architettura del processore

  - Indirizzi di rete IPv4 e IPv6

  - Indirizzi RDP

Per ulteriori informazioni su come ottenere queste informazioni, vedere [Script Hyper-V: osservano KVP GuestIntrinsicExchangeItems](http://blogs.msdn.com/b/virtual_pc_guy/archive/2008/11/18/hyper-v-script-looking-at-kvp-guestintrinsicexchangeitems.aspx).


> [!NOTE]
> I dati KVP non è protetto. Può essere modificato e non è monitorato per le modifiche.



> [!IMPORTANT]
> I dati KVP devono essere rimosso se la chiave AVMA viene sostituita con un altro codice product key (chiave delle licenze di vendita al dettaglio, OEM o volume).


Dati cronologici sulle richieste AVMA sono disponibili in un file di log nel server di virtualizzazione (EventID 12310).

Dato che il processo di attivazione AVMA è trasparente, non vengono visualizzati i messaggi di errore. Tuttavia, gli eventi seguenti vengono acquisiti in un file di log nelle macchine virtuali (EventID 12309).

|Notifica|Descrizione|
|-|-|
|Operazione riuscita AVMA|La macchina virtuale è stata attivata.|
|Host non valido|Il server di virtualizzazione è che non rispondono. Questa situazione può verificarsi quando il server non è in esecuzione una versione supportata di Windows.|
|Dati non validi|Ciò è dovuto in genere un errore di comunicazione tra il server di virtualizzazione e la macchina virtuale, spesso causato dalla mancata corrispondenza del danneggiamento, la crittografia o dati.|
|Attivazione negato|Il server di virtualizzazione potrebbe non attivare il sistema operativo guest perché non corrisponde all'ID AVMA.|

