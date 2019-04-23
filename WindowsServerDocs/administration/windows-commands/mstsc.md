---
title: mstsc
description: 'Argomento i comandi di Windows per * * *- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 17c050a3504e763488a34bd19faad80558847965
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59868822"
---
# <a name="mstsc"></a>mstsc

>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Crea connessioni ai server Host sessione Desktop remoto (Host sessione rd) o ad altri computer remoti, consente di modificare un file di configurazione di connessione Desktop remoto (RDP) e viene eseguita la migrazione di file di connessione preesistenti che sono stati creati con Client Connection Manager ai nuovi file di connessione RDP.
Per esempi di come usare questo comando, vedere [esempi](#BKMK_examples).
> [!NOTE]
> In Windows Server 2008 R2, Servizi terminal si chiama ora Servizi Desktop remoto. Per scoprire quali sono le novità nella versione più recente, vedere [novità in Servizi Desktop remoto in Windows Server 2012](https://technet.microsoft.com/library/hh831527) nella libreria TechNet di Windows Server.

## <a name="syntax"></a>Sintassi
```
mstsc.exe [<Connection File>] [/v:<Server>[:<Port>]] [/admin] [/f] [/w:<Width> /h:<Height>] [/public] [/span]
mstsc.exe /edit <Connection File>
mstsc.exe /migrate
```

## <a name="parameters"></a>Parametri
|Parametro|Descrizione|
|-------|--------|
|<Connection File>|Specifica il nome di un file RDP per la connessione.|
|/v:<Server[:<Port>]|Specifica il computer remoto e, facoltativamente, il numero di porta a cui si desidera connettersi.|
|/admin|Consente di connettersi a una sessione per l'amministrazione del server.|
|/f|Avvia connessione Desktop remoto in modalità schermo intero.|
|/w:<Width>|Specifica la larghezza della finestra di Desktop remoto.|
|/h:<Height>|Specifica l'altezza della finestra di Desktop remoto.|
|/public|Viene eseguito Desktop remoto in modalità pubblica. In modalità pubblica, bitmap e le password non vengono memorizzate.|
|/span|Corrisponde a Desktop remoto larghezza e altezza con il desktop virtuale, che si estende su più monitor, se necessario.|
|/edit <Connection File>|Apre il file rdp specificato per la modifica.|
|/ migrazione|Esegue la migrazione di file di connessione preesistenti che sono stati creati con Client Connection Manager in nuovi file di connessione RDP.|
|/?|Visualizza la guida al prompt dei comandi.|

## <a name="remarks"></a>Note
-   Default. rdp viene archiviato per ogni utente come un file nascosto nella cartella documenti dell'utente. File con estensione rdp creato dall'utente vengono salvati per impostazione predefinita nella cartella documenti dell'utente ma può essere salvato ovunque.
-   Per estendersi su monitora, i monitoraggi devono usare la stessa risoluzione e devono essere allineati orizzontalmente (vale a dire, side-by-side). Non è attualmente alcun supporto per che si estende su più monitor in verticale nel sistema client.

## <a name="BKMK_examples"></a>Esempi
-   Per connettersi a una sessione in modalità schermo intero, digitare:
    ```
    mstsc /f
    ```
-   Per aprire un file RDP per la modifica, digitare:
    ```
    mstsc /edit filename.rdp
    ```
    
#### <a name="additional-references"></a>Riferimenti aggiuntivi
-   [Chiave sintassi della riga di comando](command-line-syntax-key.md)
-   [Servizi Desktop remoto &#40;servizi Terminal&#41; Guida comandi](remote-desktop-services-terminal-services-command-reference.md)
