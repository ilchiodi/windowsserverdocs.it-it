---
title: 'secedit: generaterollback'
description: 'Argomento dei comandi di Windows per * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 385a6799-51a7-4fe3-bd73-10c7998b6680
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 797f93f010a903d6ec21f568d8d665cb2e42e692
ms.sourcegitcommit: 5ed817fff1262e390403a4f4f6c38fdc12300c29
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/07/2020
ms.locfileid: "75702742"
---
# <a name="seceditgeneraterollback"></a>secedit: generaterollback



Consente di generare un modello di ripristino per un modello di configurazione specificato. Per esempi di come è possibile utilizzare questo comando, vedere [esempi](#BKMK_Examples).

## <a name="syntax"></a>Sintassi

```
Secedit /generaterollback /db <database file name> /cfg <configuration file name> /rbk <rollback template file name> [/log <log file name>] [/quiet]
```

### <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|DB|Obbligatorio.</br>Specifica il percorso e il nome di un database che contiene la configurazione memorizzata con cui verrà eseguita l'analisi.</br>Se il nome di file specifica un database che non ha un modello di sicurezza (come indicato dal file di configurazione) associato, il `/cfg \<configuration file name>` essere specificata anche l'opzione della riga di comando.|
|config.|Obbligatorio.</br>Specifica il percorso e il nome del modello di protezione che verrà importato nel database per l'analisi.</br>Questa opzione /cfg è valida solo quando utilizzato con il `/db \<database file name>` parametro. Se non viene specificato, l'analisi viene eseguita con le configurazioni già memorizzate nel database.|
|rbk|Obbligatorio.</br>Specifica un modello di sicurezza in cui viene scritto le informazioni di ripristino. Modelli di protezione vengono creati utilizzando lo snap-in modelli di protezione. File di rollback possono essere creati con questo comando.|
|log|Facoltativo.</br>Specifica il percorso e il nome del file di log per il processo.|
|quiet|Facoltativo.</br>Disattiva l'output dello schermo e di log. È possibile visualizzare i risultati di analisi tramite l'analisi e configurazione della protezione snap-in a Microsoft Management Console (MMC).|

## <a name="remarks"></a>Osservazioni

Se il percorso del file di log non viene specificato, viene utilizzato il file di log predefinito, ovvero*systemroot*\Users \*AccountUtente<em>\My Documents\Security\Logs\*DatabaseName</em>. log.

A partire da Windows Server 2008, `Secedit /refreshpolicy` è stato sostituito con `gpupdate`. Per informazioni su come aggiornare le impostazioni di sicurezza, vedere [Gpupdate](gpupdate.md).

La corretta esecuzione di questo comando verrà "l'attività è stata completata correttamente". e i registri solo le mancate corrispondenze tra il modello di sicurezza indicati e configurazione dei criteri di sicurezza. Vengono elencate tali mancate corrispondenze nel scesrv.log.

Se viene specificato un modello di ripristino esistenti, questo comando viene sovrascritto. Con questo comando, è possibile creare un nuovo modello di rollback. Parametri aggiuntivi non necessari per una delle due condizioni.

## <a name="BKMK_Examples"></a>Esempi:

Dopo aver creato il modello di sicurezza tramite la configurazione della protezione e snap-in analisi, SecTmplContoso.inf, creare il file di configurazione di rollback per salvare le impostazioni originali. Scrivere l'azione per il file di registro FY11.
```
Secedit /generaterollback /db C:\Security\FY11\SecDbContoso.sdb /cfg sectmplcontoso.inf /rbk sectmplcontosoRBK.inf /log C:\Security\FY11\SecAnalysisContosoFY11.log
```

#### <a name="additional-references"></a>Altri riferimenti

-   [Secedit](secedit.md)
-   [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)