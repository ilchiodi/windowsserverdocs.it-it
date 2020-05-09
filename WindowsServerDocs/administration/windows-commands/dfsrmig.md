---
title: dfsrmig
description: Argomento di riferimento per il comando dfsrmig, che esegue la migrazione della replica SYSvol da FRS a Replica DFS, fornisce informazioni sullo stato di avanzamento della migrazione e modifica gli oggetti di Active Directory Domain Services per supportare la migrazione.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: e1b6a464-6a93-4e66-9969-04f175226d8d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: a0329bd5829941595f9e1938f68eefb17036c132
ms.sourcegitcommit: fad2ba64bbc13763772e21ed3eabd010f6a5da34
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/09/2020
ms.locfileid: "82992724"
---
# <a name="dfsrmig"></a>dfsrmig

> Si applica a: Windows Server (canale semestrale), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Lo strumento di migrazione per il servizio Replica DFS, dfsrmig. exe, viene installato con il servizio Replica DFS. Questo strumento consente di eseguire la migrazione della replica SYSvol dal servizio Replica file (FRS) alla replica file system distribuito (DFS). Vengono inoltre fornite informazioni sullo stato di avanzamento della migrazione e vengono modificati gli oggetti Active Directory Domain Services (AD DS) per supportare la migrazione.

## <a name="syntax"></a>Sintassi

```
dfsrmig [/setglobalstate <state> | /getglobalstate | /getmigrationstate | /createglobalobjects |
/deleterontfrsmember [<read_only_domain_controller_name>] | /deleterodfsrmember [<read_only_domain_controller_name>] | /?]
```

### <a name="parameters"></a>Parametri

| Parametro | Descrizione |
| --------- | ----------- |
| `/setglobalstate <state>` | Imposta lo stato di migrazione globale del dominio su uno che corrisponde al valore specificato da *stato*. È possibile impostare lo stato di migrazione globale solo a uno stato stabile. I valori di *stato* includono:<ul><li>**0** -stato di avvio</li><li>**1** -stato preparato</li><li>**2** -stato reindirizzato</li><li>**3** -stato eliminato</li></ul> |
| /getglobalstate | Recupera lo stato di migrazione globale corrente per il dominio alla copia locale del database di Active Directory, quando viene eseguito l'emulatore PDC. Utilizzare questa opzione per verificare di aver impostato lo stato di migrazione globale corretto.<p>**Importante:** Eseguire questo comando solo sull'emulatore PDC. |
| /getmigrationstate | Recupera lo stato di migrazione locale corrente per tutti i controller di dominio nel dominio e determina se tali stati locali corrispondono allo stato di migrazione globale corrente. Utilizzare questa opzione per determinare se tutti i controller di dominio sono stato raggiunto lo stato di migrazione globale. |
| /createglobalobjects | Crea gli oggetti globali e le impostazioni in servizi di dominio Active Directory utilizzati da Replica DFS utilizza. Le uniche situazioni in cui è consigliabile utilizzare questa opzione per creare manualmente oggetti e impostazioni sono:<ul><li>**Un nuovo controller di dominio di sola lettura viene promosso durante la migrazione**. Se un nuovo controller di dominio di sola lettura viene promosso nel dominio dopo lo spostamento nello stato **preparato** , ma prima della migrazione allo stato **eliminato** , gli oggetti che corrispondono al nuovo controller di dominio non vengono creati, causando un errore di replica e la migrazione.</li><li>**Le impostazioni globali per il servizio Replica DFS sono mancanti o sono state eliminate**. Se queste impostazioni risultano mancanti per un controller di dominio, la migrazione dallo stato **iniziale** allo stato **preparato** verrà stallo allo stato di **preparazione** della transizione. **Nota:** poiché vengono create le impostazioni globali di dominio Active Directory per il servizio Replica DFS per un controller di dominio di sola lettura nell'emulatore PDC, queste impostazioni devono essere replicati al controller di dominio di sola lettura dall'emulatore del PDC prima che il servizio Replica DFS nel controller di dominio di sola lettura è possibile utilizzare queste impostazioni. A causa delle latenze di replica Active Directory, questa replica può richiedere del tempo. |
| `/deleterontfrsmember [<read_only_domain_controller_name>]` | Elimina le impostazioni globali di servizi di dominio Active Directory per la replica FRS che corrispondono al controller di dominio di sola lettura specificato oppure Elimina le impostazioni globali di servizi di dominio Active Directory per la replica FRS per tutti i controller `<read_only_domain_controller_name>`di dominio di sola lettura se non è specificato alcun valore per.<p>Non è necessario utilizzare questa opzione durante un processo di migrazione normale, poiché il servizio Replica DFS Elimina automaticamente le impostazioni di servizi di dominio Active Directory durante la migrazione dallo stato **reindirizzato** allo stato **eliminato** . Utilizzare questa opzione per eliminare manualmente le impostazioni di servizi di dominio Active Directory solo quando l'eliminazione automatica non riesce in un controller di dominio di sola lettura e blocca il controller di dominio di sola lettura per un IME lungo durante la migrazione dallo stato **reindirizzato** allo stato **eliminato** . |
| `/deleterodfsrmember [<read_only_domain_controller_name>]` | Elimina le impostazioni globali di servizi di dominio Active Directory per Replica DFS che corrispondono al controller di dominio di sola lettura specificato oppure Elimina le impostazioni globali di servizi di dominio Active Directory per Replica DFS per tutti i controller di dominio `<read_only_domain_controller_name>`di sola lettura se per non viene specificato alcun valore.<p>Utilizzare questa opzione per eliminare manualmente le impostazioni di servizi di dominio Active Directory solo quando l'eliminazione automatica non riesce in un controller di dominio di sola lettura e blocca il controller di dominio di sola lettura per un lungo periodo di tempo quando si esegue il rollback della migrazione dallo stato preparato allo stato di avvio. |
| /? | Visualizza la guida al prompt dei comandi. |

#### <a name="remarks"></a>Osservazioni

- Usare il `/setglobalstate <state>` comando per impostare lo stato di migrazione globale in servizi di dominio Active Directory nell'emulatore PDC per avviare e controllare il processo di migrazione. Se l'emulatore PDC non è disponibile, il comando ha esito negativo.

- La migrazione allo stato **eliminato** è irreversibile e non è possibile eseguire il rollback. utilizzare quindi il valore **3** per *lo stato* solo quando si esegue il commit completo dell'utilizzo di replica DFS per la replica di SYSvol.

- Gli Stati di migrazione globali devono essere uno stato di migrazione stabile.

- Active Directory replica replica lo stato globale ad altri controller di dominio nel dominio, ma a causa delle latenze di replica, è possibile ottenere incoerenze se si `dfsrmig /getglobalstate` esegue in un controller di dominio diverso dall'emulatore PDC.

- L'output di `dsfrmig /getmigrationstate` indica se la migrazione allo stato globale corrente è stata completata, elencando lo stato di migrazione locale per i controller di dominio che non hanno ancora raggiunto lo stato corrente di migrazione globale. Lo stato di migrazione locale per i controller di dominio può includere anche stati di transizione per i controller di dominio che non hanno raggiunto lo stato corrente di migrazione globale.

- I controller di dominio di sola lettura non possono eliminare le impostazioni da servizi di dominio Active Directory, l'emulatore PDC esegue questa operazione e le modifiche alla fine vengono replicate nei controller di dominio di sola lettura dopo le latenze applicabili per la replica di Active Directory.

- Il comando **dfsrmig** è supportato solo nei controller di dominio eseguiti a livello di funzionalità del dominio di Windows Server, perché la migrazione SYSVOL da FRS a replica DFS è possibile solo nei controller di dominio che operano a tale livello.

- È possibile eseguire il **dfsrmig** comando in qualsiasi controller di dominio, ma le operazioni di creazione o la modifica di dominio Active Directory gli oggetti sono consentiti solo nei controller di dominio in grado di lettura / scrittura (non sui controller di dominio di sola lettura).

## <a name="examples"></a>Esempi

Per impostare lo stato di migrazione globale su preparato (**1**) e avviare la migrazione o eseguire il rollback dallo stato preparato, digitare:

```
dfsrmig /setglobalstate 1
```

Per impostare lo stato di migrazione globale su Start (**0**) e per avviare il rollback allo stato Start, digitare:

```
dfsrmig /setglobalstate 0
```

Per visualizzare lo stato di migrazione globale, digitare:

```
dfsrmig /getglobalstate
```

Output del `dfsrmig /getglobalstate` comando:

```
Current DFSR global state: Prepared
Succeeded.
```

Per visualizzare informazioni sul fatto che gli Stati di migrazione locale su tutti i controller di dominio corrispondano allo stato di migrazione globale e se sono presenti stati di migrazione locale in cui lo stato locale non corrisponde allo stato globale, digitare:

```
dfsrmig /GetMigrationState
```

Output del `dfsrmig /getmigrationstate` comando quando gli Stati di migrazione locale su tutti i controller di dominio corrispondono allo stato di migrazione globale:

```
All Domain Controllers have migrated successfully to Global state (Prepared).
Migration has reached a consistent state on all Domain Controllers.
Succeeded.
```

Output del `dfsrmig /getmigrationstate` comando quando gli Stati di migrazione locale in alcuni controller di dominio non corrispondono allo stato di migrazione globale.

```
The following Domain Controllers are not in sync with Global state (Prepared):
Domain Controller (Local Migration State) DC type
=========
CONTOSO-DC2 (start) ReadOnly DC
CONTOSO-DC3 (Preparing) Writable DC
Migration has not yet reached a consistent state on all domain controllers
State information might be stale due to AD latency.
```

Per creare gli oggetti globali e le impostazioni che utilizza la replica DFS in Active Directory nei controller di dominio in cui tali impostazioni non sono state create automaticamente durante la migrazione o in cui tali impostazioni non sono presenti, digitare:

```
dfsrmig /createglobalobjects
```

Per eliminare le impostazioni globali di dominio Active Directory per la replica di FRS per un controller di dominio di sola lettura denominato contoso-dc2, se tali impostazioni non vengono eliminate automaticamente eliminato dal processo di migrazione, digitare:

```
dfsrmig /deleterontfrsmember contoso-dc2
```

Per eliminare le impostazioni globali di dominio Active Directory per la replica di FRS per tutti i controller di dominio di sola lettura se tali impostazioni non vengono eliminate automaticamente dal processo di migrazione, digitare:

```
dfsrmig /deleterontfrsmember
```

Per eliminare le impostazioni globali di dominio Active Directory per replica DFS per un controller di dominio di sola lettura denominato contoso-dc2, se tali impostazioni non vengono eliminate automaticamente dal processo di migrazione, digitare:

```
dfsrmig /deleterodfsrmember contoso-dc2
```

Per eliminare le impostazioni globali di dominio Active Directory per replica DFS per tutti i controller di dominio di sola lettura se tali impostazioni non vengono eliminate automaticamente dal processo di migrazione, digitare:

```
dfsrmig /deleterodfsrmember
```

Per visualizzare la guida al prompt dei comandi:

```
dfsrmig
```

```
dfsrmig /?
```

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](https://go.microsoft.com/fwlink/?LinkId=122056)

- [Serie di migrazione SYSvol: parte 2 dfsrmig. exe: strumento di migrazione SYSvol](https://techcommunity.microsoft.com/t5/storage-at-microsoft/sysvol-migration-series-part-2-8211-dfsrmig-exe-the-sysvol/ba-p/423470)

- [Active Directory Domain Services](../../identity/ad-ds/get-started/virtual-dc/active-directory-domain-services-overview.md)
