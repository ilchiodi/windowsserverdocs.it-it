---
title: secedit
description: Argomento di riferimento per * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 58ed57ed-08e3-403d-a363-0620b358637a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 56c9cdecfe2a5553230f1c8120c827e7f05d0ba9
ms.sourcegitcommit: 4f407b82435afe3111c215510b0ef797863f9cb4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/24/2020
ms.locfileid: "83821094"
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

|Parametro|Description|
|---------|-----------|
|[Secedit:analyze](secedit-analyze.md)|Consente di analizzare le impostazioni correnti di sistemi con le impostazioni di base che vengono archiviate in un database.  I risultati dell'analisi vengono archiviati in un'area separata del database e possono essere visualizzati nella configurazione della protezione e snap-in analisi.|
|[Secedit:configure](secedit-configure.md)|Consente di configurare un sistema con le impostazioni di sicurezza archiviate in un database.|
|[Secedit:export](secedit-export.md)|Consente di esportare le impostazioni di protezione memorizzate in un database.|
|[Secedit:generaterollback](secedit-generaterollback.md)|Consente di generare un modello di ripristino rispetto a un modello di configurazione.|
|[Secedit:import](secedit-import.md)|Consente di importare un modello di protezione in un database in modo che le impostazioni specificate nel modello possono essere applicate a un sistema o analizzate contro un sistema.|
|[Secedit:validate](secedit-validate.md)|Consente di convalidare la sintassi di un modello di sicurezza.|

## <a name="remarks"></a>Osservazioni

Per tutti i nomi file, viene utilizzata la directory corrente, se viene specificato alcun percorso.

Quando viene creato un modello di sicurezza utilizzando lo snap-in modello di protezione e la configurazione della sicurezza e snap-in analisi viene eseguito, vengono creati i file seguenti:


|           File           |                                                                                                                                                                                                                                                               Descrizione                                                                                                                                                                                                                                                                |
|--------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|        Scesrv.log        |                                                                                                                             **Percorso**: %windir%\security\logs.</br>**Creato da**: sistema operativo</br>**Tipo di file**: testo</br>**Frequenza di aggiornamento**: sovrascritti quando secedit, analizzare, configurare ed esportare o /import vengono eseguiti.</br>**Contenuto**: contiene i risultati dell'analisi raggruppati per tipo di criteri.                                                                                                                             |
| *Nome utente selezionato*sdb |                                                                                    **Percorso**: % windir %\*account utente<em>\Documents\Security\Database</br></em>*Creato da* <em> : esecuzione dello snap-in analisi e configurazione della sicurezza</br></em>*Tipo di file* <em> : proprietario</br></em>*Frequenza di aggiornamento* <em> : aggiornata ogni volta che viene creato un nuovo modello di sicurezza.</br></em>*Contenuto* \* : criteri di sicurezza locali e modelli di sicurezza creati dall'utente.                                                                                    |
| *Nome utente selezionato*. log | **Percorso**: valore predefinito è definito dall'utente ma % windir %\*account utente<em>\Documents\Security\Logs</br></em>*Creato da* <em> : esecuzione dei sottocomandi/Analyze e/Configure (o utilizzando lo snap-in configurazione e analisi della sicurezza)</br></em>*Tipo di file* <em> : testo</br></em>*Frequenza di aggiornamento* <em> : esecuzione dei sottocomandi/Analyze e/Configure (o utilizzando lo snap-in configurazione di sicurezza e analisi); sovrascritto.</br></em>*Contenuto* \* :</br>1. nome del file di log</br>2. Data e ora</br>3. risultati dell'analisi o dell'analisi. |
| *Nome utente selezionato*. inf |                                                                                     **Percorso**: % windir %\*account utente<em>\Documents\Security\Templates</br></em>*Creato da* <em> : esecuzione dello snap-in del modello di sicurezza</br></em>*Tipo di file* <em> : testo</br></em>*Frequenza* <em> di aggiornamento: ogni volta che viene aggiornato il modello di sicurezza</br></em>*Contenuto* \* : contiene le informazioni di configurazione per il modello per ogni criterio selezionato tramite lo snap-in.                                                                                     |

> [!NOTE]
> Microsoft Management Console (MMC) e la configurazione della protezione e snap-in analisi non sono disponibili in Server Core.

## <a name="additional-references"></a>Riferimenti aggiuntivi

Per esempi di come è possibile utilizzare questo comando, vedere la sezione esempi in qualsiasi file sottocomando.
- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)