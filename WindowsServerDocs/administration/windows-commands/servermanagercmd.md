---
title: ServerManagerCmd
description: 'Argomento i comandi di Windows per * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 507c4b87-8e13-4872-8b34-0c7508eecbc1
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 07/11/2018
ms.openlocfilehash: ba0b85814d942323b12e1874b852fcf28b8ac068
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59883242"
---
# <a name="servermanagercmd"></a>ServerManagerCmd

>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

> [!IMPORTANT]
> Questo comando è disponibile solo in server che eseguono Windows Server 2008 o Windows Server 2008 R2. **Servermanagercmd.exe** è stato deprecato e non è disponibile in Windows Server 2012. Per informazioni su come installare o rimuovere ruoli, servizi ruolo e funzionalità in Windows Server 2012, vedere [installare o disinstallare ruoli, servizi ruolo e funzionalità](https://go.microsoft.com/fwlink/?LinkID=239563) su Microsoft TechNet.

Installa e rimuove i ruoli, servizi ruolo e funzionalità. Inoltre viene visualizzato l'elenco di tutti i ruoli, servizi ruolo e funzionalità disponibili e illustra quali sono installate nel computer. Per altre informazioni sui ruoli, servizi ruolo e funzionalità che è possibile specificare utilizzando questo strumento, vedere la [Guida di Server Manager](https://go.microsoft.com/fwlink/?LinkID=137387). Per esempi di utilizzo di questo comando, vedere [Esempi](#BKMK_examples).

## <a name="syntax"></a>Sintassi
```
servermanagercmd -query [[[<Drive>:]<path>]<query.xml>] [-logpath   [[<Drive>:]<path>]<log.txt>]
servermanagercmd -inputpath  [[<Drive>:]<path>]<answer.xml> [-resultpath <result.xml> [-restart] | -whatif] [-logpath [[<Drive>:]<path>]<log.txt>]
servermanagercmd -install <Id> [-allSubFeatures] [-resultpath   [[<Drive>:]<path>]<result.xml> [-restart] | -whatif] [-logpath   [[<Drive>:]<path>]<log.txt>]
servermanagercmd -remove <Id> [-resultpath    <result.xml> [-restart] | -whatif] [-logpath  [[<Drive>:]<path>]<log.txt>]
servermanagercmd [-help | -?]
servermanagercmd -version
```

## <a name="parameters"></a>Parametri
|Parametro|Descrizione|
|-------|--------|
|-query [[[\<unità >:]\<path >]\<*query*>]|Visualizza un elenco di tutti i ruoli, servizi ruolo e funzionalità installate e disponibili per l'installazione nel server. È inoltre possibile utilizzare la versione abbreviata di questo parametro, **- q**. Se si desidera che i risultati della query salvati in un file XML, specificare un file XML per sostituire *query*.|
|-inputpath < [[\<unità >:]\<path >]*Answer*>|Installa o rimuove i ruoli, servizi ruolo e funzionalità specificate in un file di risposte XML rappresentato da *Answer*. È inoltre possibile utilizzare la versione abbreviata di questo parametro, **-p.**|
|-installare \< *Id*>|Installa il ruolo, servizio ruolo o funzionalità specificato da *Id*. Gli identificatori di maiuscole e minuscole. Più ruoli, servizi ruolo e funzionalità devono essere separate da spazi. I seguenti parametri facoltativi vengono utilizzati con il **-installare** parametro.<br /><br />-   **-impostazione** \< *SettingName*>=\<*SettingValue*> specifica le impostazioni necessarie per l'installazione.<br />-   **-allSubFeatures** specifica l'installazione di tutti i servizi subordinati e funzionalità con il ruolo padre, servizio ruolo o funzionalità denominata nel *Id* valore. **Nota:**     Alcuni contenitori di ruolo non è un identificatore di riga di comando per consentire l'installazione di tutti i servizi ruolo. Questo avviene quando i servizi ruolo non possono essere installati nella stessa istanza di comando di Server Manager. Ad esempio, il servizio ruolo servizio federativo di active directory Federation Services e il servizio ruolo Proxy servizio federativo non può essere installato usando la stessa istanza di comando di Server Manager.<br />-   **-resultpath** \< *result > Salva i risultati dell'installazione in un file XML rappresentato da *result*. È inoltre possibile utilizzare la versione abbreviata di questo parametro, **- r**. **Nota:**     Non è possibile eseguire **servermanagercmd** con entrambe le **- resultpath** parametro e il **- whatif** parametro specificato.<br /> -    **-riavviare** riavvia il computer automaticamente durante l'installazione è stata completata (se il riavvio è richiesto per i ruoli o funzionalità installate).<br /> -    **- whatif** consente di visualizzare le operazioni specificate per il **-installare** parametro. È anche possibile usare la versione abbreviata del **- whatif** parametro **-w**. Non è possibile eseguire **servermanagercmd** con entrambe le **- resultpath** parametro e il **- whatif** parametro specificato.<br /> -    **- logpath** \<[[\<unità >:]\<percorso >]* log.txt* > specifica un nome e percorso del file di log, diverso da quello predefinito **%windir%\temp\servermanager.log**.|
|-rimuovere \< *Id*>|Rimuove il ruolo, servizio ruolo o funzionalità specificato da *Id*. Gli identificatori di maiuscole e minuscole. Più ruoli, servizi ruolo e funzionalità devono essere separate da spazi. I seguenti parametri facoltativi vengono utilizzati con il **-rimuovere** parametro.<br /><br />-   **-resultpath** \<[[\<unità >:]\<path >]*result*> Salva i risultati della rimozione in un file XML rappresentato da *result*. È inoltre possibile utilizzare la versione abbreviata di questo parametro, **- r**. **Nota:**     Non è possibile eseguire **servermanagercmd** con entrambe le **- resultpath** parametro e il **- whatif** parametro specificato.<br />-   **-Riavviare** riavvia il computer automaticamente quando rimozione è stata completata (se il riavvio è richiesto da altri ruoli o funzionalità).<br />-   **-whatif** consente di visualizzare le operazioni specificate per il **-rimuovere** parametro. È anche possibile usare la versione abbreviata del **- whatif** parametro **-w**. Non è possibile eseguire **servermanagercmd** con entrambe le **- resultpath** parametro e il **- whatif** parametro specificato.<br />-   **-logpath**\<[[\<unità >:]\<path >]*txt*> specifica un nome e percorso del file di log, diverso da quello predefinito, **%windir%\temp\ ServerManager.log**.|
|-Guida in linea|Visualizza la Guida nella finestra del prompt dei comandi. È inoltre possibile utilizzare la forma abbreviata, **-?**.|
|-versione|Visualizza il numero di versione di Server Manager. È inoltre possibile utilizzare la forma abbreviata, **- v**.|

## <a name="remarks"></a>Note
**ServerManagerCmd** è deprecato e non è garantito a essere supportato nelle versioni future di Windows. Che se si eseguono Server Manager nei computer che eseguono Windows Server 2008 R2, è consigliabile usare i cmdlet di Windows PowerShell disponibili per Server Manager. Per ulteriori informazioni, vedere [cmdlet di Server Manager](https://go.microsoft.com/fwlink/?LinkID=137653).
Unità locale del server, è possibile eseguire ServerManagerCmd da qualsiasi directory. È necessario essere un membro del gruppo Administrators nel server in cui si desidera installare o rimuovere il software.

> [!IMPORTANT]
> A causa di restrizioni di sicurezza imposte da controllo Account utente in Windows Server 2008 R2, è necessario eseguire **Servermanagercmd** in una finestra del prompt dei comandi aperto con autorizzazioni elevate. A tale scopo, fare doppio clic su prompt dei comandi eseguibile, o il **prompt dei comandi** dell'oggetto sulle **avviare** menu e quindi fare clic su **Esegui come amministratore**.

## <a name="BKMK_examples"></a>Esempi
Nell'esempio seguente viene illustrato come utilizzare **servermanagercmd** per visualizzare un elenco di tutti i ruoli, servizi ruolo e funzionalità disponibili e quali ruoli, servizi ruolo e funzionalità installate nel computer.
```
servermanagercmd -query
```
Nell'esempio seguente viene illustrato come utilizzare **servermanagercmd** per installare il ruolo del Server Web (IIS) e salvare i risultati di installazione in un file XML rappresentato da *installResult.xml*.
```
servermanagercmd -install Web-Server -resultpath installResult.xml
```
Nell'esempio seguente viene illustrato come utilizzare la * * il parametro whatif * * con **servermanagercmd** per visualizzare informazioni dettagliate sui ruoli, servizi ruolo e funzionalità che è installata o rimossa, in base al istruzioni che vengono specificati in un file di risposte XML rappresentato da *Install*.
```
servermanagercmd -inputpath install.xml -whatif
```

#### <a name="additional-references"></a>Altri riferimenti
-   per un elenco completo di ruolo, servizio ruolo o gli identificatori di funzionalità, è possibile specificare per il *Id* parametro o altre informazioni sull'uso di un file di risposte XML con **Servermanagercmd**, vedere il [Guida di server Manager](https://go.microsoft.com/fwlink/?LinkID=137387). (https://go.microsoft.com/fwlink/?LinkID=137387).
-   Vedere [cmdlet di Server Manager](https://go.microsoft.com/fwlink/?LinkID=137653) per un elenco dei cmdlet di Windows PowerShell sono disponibili per Server Manager.
-   [Chiave sintassi della riga di comando](command-line-syntax-key.md)
