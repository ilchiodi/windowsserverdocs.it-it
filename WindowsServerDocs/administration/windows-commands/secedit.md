---
title: secedit
description: Argomento dei comandi di Windows per * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 58ed57ed-08e3-403d-a363-0620b358637a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: e80217c201cde4dc1df58c0e8976fbe1422511fe
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80834844"
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

#### <a name="parameters"></a>Parametri

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
|        Scesrv.log        |                                                                                                                             **Percorso**: %windir%\security\logs.</br>**Creato da**: sistema operativo</br>**Tipo di file**: testo</br>**Frequenza di aggiornamento**: sovrascritti quando secedit, analizzare, configurare ed esportare o /import vengono eseguiti.</br>**Contenuto**: contiene i risultati dell'analisi raggruppati per tipo di criteri.                                                                                                                             |
| *Nome utente selezionato*sdb |                                                                                    **Percorso**:% windir%\*account utente<em>\Documents\Security\Database</br></em>*creato da*<em>: esecuzione dello snap-in analisi e configurazione della sicurezza</br>*tipo di file* di </em><em>: proprietario</br>*frequenza di aggiornamento* </em><em>: aggiornata ogni volta che viene creato un nuovo modello di sicurezza.</br></em>*contenuto*\*: criteri di sicurezza locali e modelli di sicurezza creati dall'utente.                                                                                    |
| *Nome utente selezionato*. log | **Location**: definito dall'utente, ma il valore predefinito è% windir%\*account utente<em>\Documents\Security\Logs</br></em>*creato da*<em>: esecuzione dei sottocomandi/Analyze e/Configure (o utilizzando lo snap-in configurazione e analisi della sicurezza)</br>*tipo di file* di </em><em>: testo</br>*frequenza di aggiornamento* </em><em>: esecuzione dei sottocomandi/Analyze e/Configure (o utilizzando lo snap-in configurazione e analisi della sicurezza); sovrascritto.</br>\*</em>*contenuto* :</br>1. nome del file di log</br>2. Data e ora</br>3. risultati dell'analisi o dell'analisi. |
| *Nome utente selezionato*. inf |                                                                                     **Percorso**:% windir%\*account utente<em>\Documents\Security\Templates</br></em>*creato da*<em>: esecuzione dello snap-in del modello di sicurezza</br>*tipo di file* di </em><em>: testo</br>*frequenza di aggiornamento* </em><em>: ogni volta che viene aggiornato il modello di sicurezza</br></em>*Content*\*: contiene le informazioni di configurazione per il modello per ogni criterio selezionato tramite lo snap-in.                                                                                     |

> [!NOTE]
> Microsoft Management Console (MMC) e la configurazione della protezione e snap-in analisi non sono disponibili in Server Core.

## <a name="additional-references"></a>Altre informazioni di riferimento

Per esempi di come è possibile utilizzare questo comando, vedere la sezione esempi in qualsiasi file sottocomando.
-   - [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)