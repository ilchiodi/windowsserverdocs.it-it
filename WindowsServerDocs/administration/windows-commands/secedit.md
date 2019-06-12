---
title: secedit
description: 'Argomento i comandi di Windows per * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 58ed57ed-08e3-403d-a363-0620b358637a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 4c70211cc029cec7e6bb0290877089ecb9a86f22
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/31/2019
ms.locfileid: "66441466"
---
# <a name="secedit"></a>secedit



Configura e analizza la protezione del sistema confrontando la configurazione corrente ai modelli di sicurezza specificato.

## <a name="syntax"></a>Sintassi

```
secedit 
[/analyze /db <database file name> /cfg <configuration file name> [/overwrite] /log <log file name> [/quiet]]
[/configure /db <database file name> [/cfg <configuration filename>] [/overwrite] [/areas [securitypolicy | group_mgmt | user_rights | regkeys | filestore | services]] [/log <log file name>] [/quiet]]
[/export /db <database file name> [/mergedpolicy] /cfg <configuration file name> [/areas [securitypolicy | group_mgmt | user_rights | regkeys | filestore | services]] [/log <log file name>]]
[/generaterollback /db <database file name> /cfg <configuration file name> /rbk <rollback file name> [/log <log file name>] [/quiet]]
[/import /db <database file name> /cfg <configuration file name> [/overwrite] [/areas [securitypolicy | group_mgmt | user_rights | regkeys | filestore | services]] [/log <log file name>] [/quiet]]
[/validate <configuration file name>]
```

### <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|[Secedit:analyze](secedit-analyze.md)|Consente di analizzare le impostazioni correnti di sistemi con le impostazioni di base che vengono archiviate in un database.  I risultati dell'analisi vengono archiviati in un'area separata del database e possono essere visualizzati nella configurazione della protezione e snap-in analisi.|
|[Secedit:configure](secedit-configure.md)|Consente di configurare un sistema con le impostazioni di sicurezza archiviate in un database.|
|[Secedit:export](secedit-export.md)|Consente di esportare le impostazioni di protezione memorizzate in un database.|
|[Secedit:generaterollback](secedit-generaterollback.md)|Consente di generare un modello di ripristino rispetto a un modello di configurazione.|
|[Secedit:import](secedit-import.md)|Consente di importare un modello di protezione in un database in modo che le impostazioni specificate nel modello possono essere applicate a un sistema o analizzate contro un sistema.|
|[Secedit:validate](secedit-validate.md)|Consente di convalidare la sintassi di un modello di sicurezza.|

## <a name="remarks"></a>Note

Per tutti i nomi file, viene utilizzata la directory corrente, se viene specificato alcun percorso.

Quando viene creato un modello di sicurezza utilizzando lo snap-in modello di protezione e la configurazione della sicurezza e snap-in analisi viene eseguito, vengono creati i file seguenti:


|           File           |                                                                                                                                                                                                                                                               Descrizione                                                                                                                                                                                                                                                                |
|--------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|        Scesrv.log        |                                                                                                                             **Percorso**: %windir%\security\logs.</br>**Creato da**: sistema operativo</br>**Tipo di file**: testo</br>**Frequenza di aggiornamento**: Sovrascritti quando secedit, analizzare / configurare ed esportare o /import vengono eseguiti.</br>**Contenuto**: Contiene i risultati dell'analisi raggruppati per tipo di criteri.                                                                                                                             |
| *Nome utente selezionato*sdb |                                                                                    **Ubicazione**: % windir %\*account utente<em>\Documents\Security\Database</br></em>*Creato da*<em>: in esecuzione lo snap-in analisi e configurazione della sicurezza</br></em>*Tipo di file*<em>: proprietario</br></em>*Frequenza di aggiornamento*<em>: Aggiornata ogni volta che viene creato un nuovo modello di sicurezza.</br></em>*Contenuto*\*: Criteri di sicurezza locali e i modelli di protezione creati dall'utente.                                                                                    |
| *Nome utente selezionato*. log | **Percorso**: Definito dall'utente ma il valore predefinito è % windir %\*account utente<em>\Documents\Security\Logs</br></em>*Creato da*<em>: Esecuzione di /ANALYZE e / configurare sottocomandi (o utilizzando lo snap-in analisi e configurazione della sicurezza)</br></em>*Tipo di file*<em>: testo</br></em>*Frequenza di aggiornamento*<em>: Esecuzione di /ANALYZE e / configurare sottocomandi (o utilizzando lo snap-in analisi e configurazione della protezione); sovrascritto.</br></em>*Contenuto*\*:</br>1.  Nome file di log</br>2.  Data e ora</br>3.  Risultati dell'analisi o analisi. |
| *Nome utente selezionato*. inf |                                                                                     **Ubicazione**: % windir %\*account utente<em>\Documents\Security\Templates</br></em>*Creato da*<em>: in esecuzione lo snap-in modello di sicurezza</br></em>*Tipo di file*<em>: testo</br></em>*Frequenza di aggiornamento*<em>: ogni volta che viene aggiornato il modello di sicurezza</br></em>*Contenuto*\*: Contiene il set di backup delle informazioni per il modello per ogni criterio selezionato utilizzando lo snap-in.                                                                                     |

> [!NOTE]
> Microsoft Management Console (MMC) e la configurazione della protezione e snap-in analisi non sono disponibili in Server Core.

#### <a name="additional-references"></a>Altri riferimenti

Per esempi di come è possibile utilizzare questo comando, vedere la sezione esempi in qualsiasi file sottocomando.
-   [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)