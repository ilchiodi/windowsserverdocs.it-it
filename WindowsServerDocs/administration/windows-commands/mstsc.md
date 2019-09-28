---
title: mstsc
description: 'Argomento dei comandi di Windows per * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 59801227-1e7e-4dbd-96e6-f54102a3ce92
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: bf813c75c83154c76d4aeb53a259495d4ad1369e
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71373349"
---
# <a name="mstsc"></a>mstsc

>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

consente di creare connessioni ai server host sessione Desktop remoto (host sessione Desktop remoto) o ad altri computer remoti, modificare un file di configurazione con estensione RDP (Connessione Desktop remoto) esistente e migrare i file di connessione legacy creati con gestione connessione client in nuovi file di connessione RDP.
Per esempi relativi all'uso di questo comando, vedere [esempi](#BKMK_examples).
> [!NOTE]
> In Windows Server 2008 R2, Servizi terminal si chiama ora Servizi Desktop remoto. Per informazioni sulle novità della versione più recente, vedere Novità di [Servizi Desktop remoto in Windows server 2012](https://technet.microsoft.com/library/hh831527) nella libreria TechNet di Windows Server.

## <a name="syntax"></a>Sintassi
```
mstsc.exe [<Connection File>] [/v:<Server>[:<Port>]] [/admin] [/f] [/w:<Width> /h:<Height>] [/public] [/span]
mstsc.exe /edit <Connection File>
mstsc.exe /migrate
```

## <a name="parameters"></a>Parametri

|        Parametro        |                                                         Descrizione                                                         |
|-------------------------|-----------------------------------------------------------------------------------------------------------------------------|
|    <Connection File>    |                                   Specifica il nome di un file con estensione RDP per la connessione.                                    |
|   /v: < server [: <Port>]   |                Specifica il computer remoto e, facoltativamente, il numero di porta a cui si desidera connettersi.                 |
|         /admin          |                                   Consente di connettersi a una sessione per l'amministrazione del server.                                   |
|           /f            |                                    Avvia Connessione Desktop remoto in modalità schermo intero.                                    |
|       /w: <Width>        |                                      Specifica la larghezza della finestra di Desktop remoto.                                      |
|       /h: <Height>       |                                     Specifica l'altezza della finestra di Desktop remoto.                                      |
|         /Public         |                  Esegue Desktop remoto in modalità pubblica. In modalità pubblica, le password e le bitmap non vengono memorizzate nella cache.                  |
|          /span          | Consente di abbinare la larghezza e l'altezza Desktop remoto al desktop virtuale locale, che si estende tra più monitor, se necessario. |
| /Edit <Connection File> |                                         Apre il file con estensione rdp specificato per la modifica.                                          |
|        /migrate         |       Esegue la migrazione dei file di connessione legacy creati con gestione connessione client a nuovi file di connessione RDP.       |
|           /?            |                                            Visualizza la guida al prompt dei comandi.                                             |

## <a name="remarks"></a>Note
-   Default. RDP viene archiviato per ogni utente come file nascosto nella cartella documenti dell'utente. I file con estensione RDP creati dall'utente vengono salvati per impostazione predefinita nella cartella documenti dell'utente, ma possono essere salvati ovunque.
-   Per estendersi tra i monitoraggi, è necessario che i monitoraggi usino la stessa risoluzione e siano allineati orizzontalmente, ovvero affiancati. Attualmente non è disponibile alcun supporto per l'espansione verticale di più monitoraggi nel sistema client.

## <a name="BKMK_examples"></a>Esempi
-   Per connettersi a una sessione in modalità a schermo intero, digitare:
    ```
    mstsc /f
    ```
-   Per aprire un file denominato nomefile. RDP per la modifica, digitare:
    ```
    mstsc /edit filename.rdp
    ```

#### <a name="additional-references"></a>Riferimenti aggiuntivi
-   [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)
-   [Guida &#40;di riferimento&#41; ai comandi di Servizi Desktop remoto Servizi terminal](remote-desktop-services-terminal-services-command-reference.md)
