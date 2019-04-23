---
title: dfsrmig
description: 'Argomento i comandi di Windows per * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: e1b6a464-6a93-4e66-9969-04f175226d8d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: da05aec5ca5a5634585c5f5406181c5c90ee3a30
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59856942"
---
# <a name="dfsrmig"></a>dfsrmig

>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Il `dfsrmig` comando esegue la migrazione di replica di SYSvol dal servizio Replica File (FRS) per la replica del File System distribuito (DFS), fornisce informazioni sullo stato di avanzamento della migrazione e modifica gli oggetti di active directory Domain Services (AD DS) per supporta la migrazione.
Per esempi di come usare questo comando, vedere la [esempi](#BKMK_examples) sezione più avanti in questo documento.
## <a name="syntax"></a>Sintassi
```
dfsrmig [/SetGlobalState <state> | /GetGlobalState | /GetMigrationState | /createGlobalObjects | 
/deleteRoNtfrsMember [<read_only_domain_controller_name>] | /deleteRoDfsrMember [<read_only_domain_controller_name>] | /?]
```
## <a name="parameters"></a>Parametri
|Parametro|Descrizione|
|-------|--------|
|/SetGlobalState <state>|Imposta lo stato di migrazione globale desiderato per il dominio per lo stato che corrisponde al valore specificato da *stato*.<br /><br />Per eseguire i passaggi della migrazione o i processi di ripristino, utilizzare questo comando per scorrere gli stati validi. Questa opzione consente di avviare e controllare il processo di migrazione impostando lo stato di migrazione globale in Active Directory per l'emulatore PDC. Se l'emulatore PDC non è disponibile, questo comando non riesce.<br /><br /> È possibile impostare lo stato di migrazione globale solo a uno stato stabile. I valori validi per *lo stato*, di conseguenza, sono **0** per lo stato di avvio **1** per lo stato preparato, **2** per lo stato reindirizzato, e **3** per lo stato eliminato.<br /><br />Migrazione allo stato eliminato è irreversibile e non è possibile eseguire il rollback di tale stato, quindi utilizzare un valore di **3** per *stato* solo quando si è completamente committd da utilizzare Replica DFS per SYSvol replica.|
|/ GetGlobalState|Recupera lo stato di migrazione globale corrente per il dominio alla copia locale del database di Active Directory, quando viene eseguito l'emulatore PDC.<br /><br />Utilizzare questa opzione per verificare di aver impostato lo stato di migrazione globale corretto. Solo la migrazione stabile possono essere stati migrazione globale, pertanto i risultati che il **dfsrmig** comando report con il **/GetGlobalState** opzione corrispondono agli stati di cui è possibile impostare con il **/SetGlobalState** (opzione).<br /><br />È consigliabile eseguire il **dfsrmig** comando con il **/GetGlobalState** opzione solo sull'emulatore PDC. replica di Active directory replica lo stato globale per altri controller di dominio nel dominio, ma le latenze di replica possono causare incoerenze se si esegue la **dfsrmig** con il **/GetGlobalState**  opzione in un controller di dominio diverso dall'emulatore PDC. Per verificare lo stato di migrazione locale di un controller di dominio diverso dall'emulatore PDC, utilizzare il **/GetMigrationState** opzione.|
|/ GetMigrationState|Recupera lo stato di migrazione locale corrente per tutti i controller di dominio nel dominio e determina se tali Stati locale corrispondano allo stato corrente di migrazione globale.<br /><br />Utilizzare questa opzione per determinare se tutti i controller di dominio sono stato raggiunto lo stato di migrazione globale. L'output di **dsfrmig** comando quando si utilizza il **/GetMigrationState** opzione indica o meno la migrazione allo stato globale corrente è stata completata e viene elencato lo stato di migrazione locale per tutti i controller di dominio che non hanno raggiunto lo stato corrente di migrazione globale. Stato migrazione locale per i controller di dominio può includere gli stati di transizione per i controller di dominio che non hanno raggiunto lo stato corrente di migrazione globale.|
|/createGlobalObjects|Crea gli oggetti globali e le impostazioni di dominio Active Directory che utilizza la replica DFS.<br /><br />Non è necessario usare questa opzione durante un processo di migrazione normale, poiché il servizio Replica DFS crea automaticamente questi oggetti di Active Directory Domain Services e le impostazioni durante la migrazione dello stato iniziale allo stato preparato. Utilizzare questa opzione per creare manualmente questi oggetti e le impostazioni nelle situazioni seguenti:<br /><br />-   **Un nuovo controller di dominio di sola lettura viene promosso durante la migrazione**. Il servizio Replica DFS crea automaticamente gli oggetti di Active Directory Domain Services e le impostazioni per la replica DFS durante la migrazione dello stato iniziale allo stato preparato. Se un nuovo controller di dominio di sola lettura viene promosso nel dominio al termine della transizione, ma prima della migrazione allo stato eliminato, quindi gli oggetti che corrispondono al controller di dominio di sola lettura appena attivati non vengono creati in Active Directory che causano la replica e la migrazione di.<br />: In questo caso, è possibile eseguire la **dfsrmig** wth comando il **/createGlobalObjects** opzione per creare manualmente gli oggetti in qualsiasi controller di dominio di sola lettura che non sono già disponibili. Questo comando non interessa i controller di dominio che dispongono già di oggetti e delle impostazioni per il servizio Replica DFS.<br />-   **Le impostazioni globali per il servizio Replica DFS sono mancante o sono state eliminate**. Se queste impostazioni sono disponibili per un particolare controller di dominio, si blocca la migrazione dello stato iniziale allo stato preparato nello stato di transizione di preparazione per il controller di dominio. In questo caso, è possibile usare la **dfsrmig** con il **/createGlobalObjects** opzione per creare manualmente le impostazioni. **Nota:** Poiché le impostazioni di dominio Active Directory globali per il servizio Replica DFS per un controller di dominio di sola lettura vengono create nell'emulatore PDC, queste impostazioni è necessario eseguire la replica al controller di dominio di sola lettura dall'emulatore del PDC prima che il servizio Replica DFS nel controller di dominio di sola lettura può usare queste impostazioni. A causa di latenze di replica active irectory, questa replica può richiedere tempo a verificarsi.|
|/deleteRoNtfrsMember [<read_only_domain_controller_name>]|Elimina le impostazioni di dominio Active Directory globali per la replica di FRS che corrispondono al controller di dominio di sola lettura specificato oppure le impostazioni globali di Active Directory Domain Services per la replica di FRS per tutti i controller di dominio di sola lettura se viene specificato alcun valore per *read_only_ nome_controller_dominio*.<br /><br />Non è necessario utilizzare questa opzione durante un processo di migrazione normale, poiché il servizio Replica DFS Elimina automaticamente queste impostazioni di dominio Active Directory durante la migrazione dello stato reindirizzato allo stato eliminato. Poiché i controller di dominio di sola lettura non è possibile eliminare queste impostazioni da Active Directory Domain Services, l'emulatore PDC esegue questa operazione e la replica delle modifiche alla fine per i controller di dominio di sola lettura dopo le latenze applicabile per la replica di active directory.<br /><br />Utilizzare questa opzione per eliminare manualmente le impostazioni di dominio Active Directory solo durante l'eliminazione automatica non riesce in un controller di dominio di sola lettura e si blocca il controller di dominio di sola lettura per un ime lungo durante la migrazione dello stato reindirizzato allo stato eliminato.|
|/deleteRoDfsrMember [< read_only_domain_controller_name >]|Elimina le impostazioni di dominio Active Directory globali per replica DFS che corrispondono al controller di dominio di sola lettura specificato oppure le impostazioni globali di dominio Active Directory per replica DFS per tutti i controller di dominio di sola lettura se viene specificato alcun valore per *read_only_ nome_controller_dominio*.<br /><br />Usare questa opzione per eliminare manualmente le impostazioni di Active Directory Domain Services solo quando l'eliminazione automatica non riesce in un controller di dominio di sola lettura e si blocca il controller di dominio di sola lettura per lungo tempo durante il rollback della migrazione da stato predisposto per lo stato di avvio.|
|/?|Visualizza la guida al prompt dei comandi. Equivale a eseguire **dfsrmig** senza opzioni.|
## <a name="remarks"></a>Note
-   DFSRMIG.exe, lo strumento di migrazione per il servizio Replica DFS, viene installato con il servizio Replica DFS.
    per un nuovo server Windows Server 2008, Dcpromo.exe installa e avvia il servizio Replica DFS quando si Alza di livello il computer a un controller di dominio. Quando si aggiorna un server da Windows Server 2003 a Windows Server 2008, il processo di aggiornamento installa e avvia il servizio Replica DFS. Non è necessario installare il servizio ruolo di replica DFS per il servizio Replica DFS installato e avviato.
-   Il **dfsrmig** strumento è supportato solo nei controller di dominio che eseguono a livello funzionale di dominio di Windows Server 2008, perché la migrazione di SYSvol dal servizio Replica file a replica DFS è solo possibile nei controller di dominio che operano al  Livello funzionale del dominio Windows Server 2008.
-   È possibile eseguire il **dfsrmig** comando in qualsiasi controller di dominio, ma le operazioni di creazione o la modifica di dominio Active Directory gli oggetti sono consentiti solo nei controller di dominio in grado di lettura / scrittura (non sui controller di dominio di sola lettura).
-   In esecuzione **dfsrmig** senza specificare alcuna opzione Visualizza la Guida al prompt dei comandi.
## <a name="BKMK_examples"></a>Esempi
Per impostare lo stato di migrazione globale preparata (**1**) e di avviare la migrazione o il rollback dallo stato preparato, tipo:
```
dfsrmig /SetGlobalState 1
```
Per impostare lo stato di migrazione globale avviare (**0**) e avviare il rollback allo stato di avvio, tipo:
```
dfsrmig /SetGlobalState 0
```
Per visualizzare lo stato di migrazione globale, digitare:
```
dfsrmig /GetGlobalState
```
Questo esempio viene illustrato un output tipico per la **dfsrmig /GetGlobalState** comando.
```
Current DFSR global state:  Prepared 
Succeeded.
```
Per visualizzare le informazioni che gli stati di migrazione locale su tutti i controller di dominio corrispondono lo stato di migrazione globale e gli stati di migrazione locale per i controller di dominio in cui lo stato locale non corrispondono allo stato globale, digitare:
```
dfsrmig /GetMigrationState
```
Questo esempio viene illustrato un output tipico per la **dfsrmig /GetMigrationState** comando quando gli stati di migrazione locale su tutti i controller di dominio corrispondono allo stato di migrazione globale.
```
All Domain Controllers have migrated successfully to Global state ( Prepared ).
Migration has reached a consistent state on all Domain Controllers.
Succeeded.
```
Questo esempio viene illustrato un output tipico per la **dfsrmig /GetMigrationState** comando quando gli stati di migrazione locale alcuni controller di dominio non corrispondono allo stato di migrazione globale.
```
The following Domain Controllers are not in sync with Global state ( Prepared ):
Domain Controller (Local Migration State)   DC type
=========
CONTOSO-DC2 ( start )   ReadOnly DC
CONTOSO-DC3 ( Preparing )   Writable DC
Migration has not yet reached a consistent state on all domain controllers
State information might be stale due to AD latency.
```
Per creare gli oggetti globali e le impostazioni che utilizza la replica DFS in Active Directory nei controller di dominio in cui tali impostazioni non sono state create automaticamente durante la migrazione o in cui tali impostazioni non sono presenti, digitare:
```
dfsrmig /createGlobalObjects
```
Per eliminare le impostazioni globali di dominio Active Directory per la replica di FRS per un controller di dominio di sola lettura denominato contoso-dc2, se tali impostazioni non vengono eliminate automaticamente eliminato dal processo di migrazione, digitare:
```
dfsrmig /deleteRoNtfrsMember contoso-dc2
```
Per eliminare le impostazioni globali di dominio Active Directory per la replica di FRS per tutti i controller di dominio di sola lettura se tali impostazioni non vengono eliminate automaticamente dal processo di migrazione, digitare:
```
dfsrmig /deleteRoNtfrsMember
```
Per eliminare le impostazioni globali di dominio Active Directory per replica DFS per un controller di dominio di sola lettura denominato contoso-dc2, se tali impostazioni non vengono eliminate automaticamente dal processo di migrazione, digitare:
```
dfsrmig /deleteRoDfsrMember contoso-dc2
```
Per eliminare le impostazioni globali di dominio Active Directory per replica DFS per tutti i controller di dominio di sola lettura se tali impostazioni non vengono eliminate automaticamente dal processo di migrazione, digitare:
```
dfsrmig /deleteRoDfsrMember
```
## <a name="additional-references"></a>Riferimenti aggiuntivi
[Chiave sintassi della riga di comando](https://go.microsoft.com/fwlink/?LinkId=122056)

[Serie di migrazione SYSvol: Dfsrmig.exe parte 2: Lo strumento di migrazione SYSvol](https://go.microsoft.com/fwlink/?LinkID=121757)