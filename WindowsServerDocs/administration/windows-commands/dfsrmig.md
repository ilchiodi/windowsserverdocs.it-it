---
title: dfsrmig
description: Argomento dei comandi di Windows per dfsrmig, che esegue la migrazione della replica di SYSvol dal servizio Replica file (FRS) alla replica file system distribuito (DFS), fornisce informazioni sullo stato di avanzamento della migrazione e modifica gli oggetti di servizi di dominio Active Directory (AD DS) per supportare la migrazione.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: e1b6a464-6a93-4e66-9969-04f175226d8d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: d688832169cf216e628fe761f85d708a78103d89
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80846164"
---
# <a name="dfsrmig"></a>dfsrmig

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Esegue la migrazione della replica SYSvol dal servizio Replica file (FRS) alla replica file system distribuito (DFS), fornisce informazioni sullo stato di avanzamento della migrazione e modifica gli oggetti di servizi di dominio Active Directory (AD DS) per supportare la migrazione.

Per esempi di come utilizzare questo comando, vedere il [esempi](#BKMK_examples) sezione più avanti in questo documento.

## <a name="syntax"></a>Sintassi

```
dfsrmig [/SetGlobalState <state> | /GetGlobalState | /GetMigrationState | /createGlobalObjects | 
/deleteRoNtfrsMember [<read_only_domain_controller_name>] | /deleteRoDfsrMember [<read_only_domain_controller_name>] | /?]
```

### <a name="parameters"></a>Parametri

| Parametro | Descrizione | | -----_ | ------ | | <state>/SetGlobalState | Imposta lo stato di migrazione globale desiderato per il dominio sullo stato che corrisponde al valore specificato da *stato*.<p>Per eseguire i passaggi della migrazione o i processi di ripristino, utilizzare questo comando per scorrere gli stati validi. Questa opzione consente di avviare e controllare il processo di migrazione impostando lo stato di migrazione globale in Active Directory per l'emulatore PDC. Se l'emulatore PDC non è disponibile, questo comando non riesce.<p>È possibile impostare lo stato di migrazione globale solo a uno stato stabile. I valori *validi per lo stato,* pertanto, sono **0** per lo stato di avvio, **1** per lo stato preparato, **2** per lo stato reindirizzato e **3** per lo stato eliminato.<p>La migrazione allo stato eliminato è irreversibile e il rollback da tale stato non è possibile, quindi utilizzare un valore pari a **3** per *lo stato* solo quando si esegue il commit completo di utilizzando replica DFS per la replica SYSvol. | | /GetGlobalState | Recupera lo stato di migrazione globale corrente per il dominio dalla copia locale del database di servizi di dominio Active Directory, quando viene eseguito sull'emulatore PDC.<p>Utilizzare questa opzione per verificare di aver impostato lo stato di migrazione globale corretto. Solo la migrazione stabile possono essere stati migrazione globale, pertanto i risultati che il **dfsrmig** comando report con il **/GetGlobalState** opzione corrispondono agli stati di cui è possibile impostare con il **/SetGlobalState** (opzione).<p>È consigliabile eseguire il **dfsrmig** comando con il **/GetGlobalState** opzione solo sull'emulatore PDC. la replica di Active Directory replica lo stato globale agli altri controller di dominio nel dominio, ma le latenze di replica possono causare incoerenze se si esegue il comando **dfsrmig** con l'opzione **/GetGlobalState** in un controller di dominio diverso dall'emulatore PDC. Per verificare lo stato di migrazione locale di un controller di dominio diverso dall'emulatore PDC, utilizzare il **/GetMigrationState** opzione. | | /GetMigrationState | Recupera lo stato di migrazione locale corrente per tutti i controller di dominio nel dominio e determina se tali stati locali corrispondono allo stato di migrazione globale corrente.<p>Utilizzare questa opzione per determinare se tutti i controller di dominio sono stato raggiunto lo stato di migrazione globale. L'output di **dsfrmig** comando quando si utilizza il **/GetMigrationState** opzione indica o meno la migrazione allo stato globale corrente è stata completata e viene elencato lo stato di migrazione locale per tutti i controller di dominio che non hanno raggiunto lo stato corrente di migrazione globale. Stato migrazione locale per i controller di dominio può includere gli stati di transizione per i controller di dominio che non hanno raggiunto lo stato corrente di migrazione globale. | | /createGlobalObjects | Crea gli oggetti globali e le impostazioni in servizi di dominio Active Directory utilizzati Replica DFS.<p>Non è necessario utilizzare questa opzione durante un processo di migrazione normale, poiché il servizio Replica DFS crea automaticamente questi oggetti e impostazioni di servizi di dominio Active Directory durante la migrazione dallo stato di avvio allo stato preparato. Utilizzare questa opzione per creare manualmente questi oggetti e le impostazioni nelle situazioni seguenti:<p>-  **viene promosso un nuovo controller di dominio di sola lettura durante la migrazione**. Il servizio Replica DFS crea automaticamente gli oggetti e le impostazioni di servizi di dominio Active Directory per Replica DFS durante la migrazione dallo stato iniziale allo stato preparato. Se un nuovo controller di dominio di sola lettura viene promosso nel dominio al termine della transizione, ma prima della migrazione allo stato eliminato, quindi gli oggetti che corrispondono al controller di dominio di sola lettura appena attivati non vengono creati in Active Directory che causano la replica e la migrazione di.<br />-In questo caso, è possibile eseguire il comando **dfsrmig** con l'opzione **/createGlobalObjects** per creare manualmente gli oggetti in tutti i controller di dominio di sola lettura che non li hanno già. Questo comando non interessa i controller di dominio che dispongono già di oggetti e delle impostazioni per il servizio Replica DFS.<p>- **le impostazioni globali per il servizio Replica DFS mancano o sono state eliminate**. Se queste impostazioni risultano mancanti per un particolare controller di dominio, la migrazione dallo stato iniziale allo stato preparato viene stallo nello stato preparazione transizione per il controller di dominio. In questo caso, è possibile usare il comando **dfsrmig** con l'opzione **/createGlobalObjects** per creare manualmente le impostazioni. **Nota:** poiché vengono create le impostazioni globali di dominio Active Directory per il servizio Replica DFS per un controller di dominio di sola lettura nell'emulatore PDC, queste impostazioni devono essere replicati al controller di dominio di sola lettura dall'emulatore del PDC prima che il servizio Replica DFS nel controller di dominio di sola lettura è possibile utilizzare queste impostazioni. A causa delle latenze di replica irectory attive, la replica può richiedere del tempo. | | /deleteRoNtfrsMember [< read_only_domain_controller_name >] | Elimina le impostazioni globali di servizi di dominio Active Directory per la replica FRS che corrispondono al controller di dominio di sola lettura specificato oppure Elimina le impostazioni globali di servizi di dominio Active Directory per replica FRS per tutti i controller di dominio di sola lettura se non viene specificato alcun valore per *read_only_domain_controller_name*.<p>Non è necessario utilizzare questa opzione durante un processo di migrazione normale, poiché il servizio Replica DFS Elimina automaticamente queste impostazioni di dominio Active Directory durante la migrazione dello stato reindirizzato allo stato eliminato. Poiché i controller di dominio di sola lettura non possono eliminare queste impostazioni da servizi di dominio Active Directory, l'emulatore PDC esegue questa operazione e le modifiche vengono infine replicate nei controller di dominio di sola lettura dopo le latenze applicabili per la replica di Active Directory.<p>Utilizzare questa opzione per eliminare manualmente le impostazioni di dominio Active Directory solo durante l'eliminazione automatica non riesce in un controller di dominio di sola lettura e si blocca il controller di dominio di sola lettura per un ime lungo durante la migrazione dello stato reindirizzato allo stato eliminato. | | /deleteRoDfsrMember [< read_only_domain_controller_name >] | Elimina le impostazioni globali di servizi di dominio Active Directory per Replica DFS che corrispondono al controller di dominio di sola lettura specificato oppure Elimina le impostazioni globali di servizi di dominio Active Directory per Replica DFS per tutti i controller di dominio di sola lettura se per *read_only_domain_controller_name*non è specificato alcun valore.<p>Utilizzare questa opzione per eliminare manualmente le impostazioni di servizi di dominio Active Directory solo quando l'eliminazione automatica non riesce in un controller di dominio di sola lettura e blocca il controller di dominio di sola lettura per un lungo periodo di tempo quando si esegue il rollback della migrazione dallo stato preparato allo stato di avvio. | | /? | Visualizza la guida al prompt dei comandi. Equivale a eseguire **dfsrmig** senza opzioni. |

## <a name="remarks"></a>Note
- dfsrmig. exe, lo strumento di migrazione per il servizio Replica DFS, viene installato con il servizio Replica DFS.
 per un nuovo server Windows Server 2008, Dcpromo. exe installa e avvia il servizio Replica DFS quando si innalza di livello il computer a controller di dominio. Quando si esegue l'aggiornamento di un server da Windows Server 2003 a Windows Server 2008, il processo di aggiornamento installa e avvia il servizio Replica DFS. Non è necessario installare il servizio ruolo di replica DFS per il servizio Replica DFS installato e avviato.
- Lo strumento **dfsrmig** è supportato solo nei controller di dominio eseguiti al livello di funzionalità del dominio windows Server 2008, perché la migrazione di SYSVOL da FRS a replica DFS è possibile solo nei controller di dominio che operano al livello di funzionalità del dominio windows Server 2008.
- È possibile eseguire il **dfsrmig** comando in qualsiasi controller di dominio, ma le operazioni di creazione o la modifica di dominio Active Directory gli oggetti sono consentiti solo nei controller di dominio in grado di lettura / scrittura (non sui controller di dominio di sola lettura).
- L'esecuzione di **dfsrmig** senza alcuna opzione Visualizza la guida al prompt dei comandi.

## <a name="examples"></a><a name=BKMK_examples></a>Esempi
Per impostare lo stato di migrazione globale preparata (**1**) e di avviare la migrazione o il rollback dallo stato preparato, tipo:
 ```
 dfsrmig /SetGlobalState 1
 ```
 Per impostare lo stato di migrazione globale su Start (**0**) e avviare il rollback allo stato Start, digitare:
 ```
 dfsrmig /SetGlobalState 0
 ```
 Per visualizzare lo stato di migrazione globale, digitare:
 ```
 dfsrmig /GetGlobalState
 ```
 Questo esempio viene illustrato un output tipico per la **dfsrmig /GetGlobalState** comando.
 ```
 Current DFSR global state: Prepared 
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
  Domain Controller (Local Migration State)  DC type
  =========
  CONTOSO-DC2 ( start )  ReadOnly DC
  CONTOSO-DC3 ( Preparing )  Writable DC
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
## <a name="additional-references"></a>Altre informazioni di riferimento
- [Indicazioni generali sulla sintassi della riga di comando](https://go.microsoft.com/fwlink/?LinkId=122056)

- [Serie di migrazione SYSvol: parte 2 dfsrmig. exe: strumento di migrazione SYSvol](https://go.microsoft.com/fwlink/?LinkID=121757)
