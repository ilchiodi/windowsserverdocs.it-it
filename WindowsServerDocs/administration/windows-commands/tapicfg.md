---
title: tapicfg
description: Informazioni su come gestire una partizione di directory applicativa TAPI.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: c0e642ce-5d98-4edb-9a65-1dff09aef4e1
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 07/11/2018
ms.openlocfilehash: 694c32d9bbb8a873f34132e90c7201d3c9fe5132
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/01/2020
ms.locfileid: "82721561"
---
# <a name="tapicfg"></a>tapicfg

> Si applica a: Windows Server (canale semestrale), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Consente di creare, rimuovere o visualizzare una partizione di directory applicativa TAPI o di impostare una partizione di directory applicativa predefinita. I client TAPI 3,1 possono usare le informazioni contenute in questa partizione di directory applicativa con il servizio di localizzazione del servizio directory per trovare e comunicare con le directory TAPI. È anche possibile usare **Tapicfg** per creare o rimuovere punti di connessione del servizio, che consentono ai client TAPI di individuare in modo efficiente le partizioni di directory applicative TAPI in un dominio. Per ulteriori informazioni, vedere la sezione Osservazioni. Per visualizzare la sintassi del comando, fare clic su un comando. 
-   [installazione di tapicfg](#BKMK_install)
-   [tapicfg rimuovere](#BKMK_remove)
-   [publishscp tapicfg](#BKMK_publishscp)
-   [removescp tapicfg](#BKMK_removescp)
-   [tapicfg show](#BKMK_show)
-   [makedefault tapicfg](#BKMK_makedefault)

## <a name="tapicfg-install"></a><a name="BKMK_install"></a>installazione di tapicfg
Crea una partizione di directory applicativa TAPI.

### <a name="syntax"></a>Sintassi
```
tapicfg install /directory:<PartitionName> [/server:<DCName>] [/forcedefault]
```
#### <a name="parameters"></a>Parametri
|Parametro|Descrizione|
|-------|--------|
|installare/Directory:\<partitionname>|Obbligatorio. Specifica il nome DNS della partizione di directory applicativa TAPI da creare. Questo nome deve essere un nome di dominio completo.|
|/Server: \<DCNAME>|Specifica il nome DNS del controller di dominio in cui viene creata la partizione di directory applicativa TAPI. Se il nome del controller di dominio non è specificato, viene utilizzato il nome del computer locale.|
|/forcedefault|Specifica che questa directory è la partizione di directory dell'applicazione TAPI predefinita per il dominio. In un dominio possono essere presenti più partizioni di directory applicative TAPI.<p>Se questa directory è la prima partizione di directory applicativa TAPI creata nel dominio, viene automaticamente impostata come predefinita, indipendentemente dal fatto che si usi l'opzione **/forcedefault** .|
|/?|Visualizza la guida al prompt dei comandi.|

## <a name="tapicfg-remove"></a><a name="BKMK_remove"></a>tapicfg rimuovere
Rimuove una partizione di directory applicativa TAPI.

### <a name="syntax"></a>Sintassi
```
tapicfg remove /directory:<PartitionName>
```
#### <a name="parameters"></a>Parametri
|Parametro|Descrizione|
|-------|--------|
|rimuovere/directory:\<partitionname>|Obbligatorio. Specifica il nome DNS della partizione di directory applicativa TAPI da rimuovere. Si noti che questo nome deve essere un nome di dominio completo.|
|/?|Visualizza la guida al prompt dei comandi.|

## <a name="tapicfg-publishscp"></a><a name="BKMK_publishscp"></a>publishscp tapicfg
Crea un punto di connessione del servizio per pubblicare una partizione di directory applicativa TAPI.

### <a name="syntax"></a>Sintassi
```
tapicfg publishscp /directory:<PartitionName> [/domain:<DomainName>] [/forcedefault]
```
#### <a name="parameters"></a>Parametri
|Parametro|Descrizione|
|-------|--------|
|publishscp/directory:\<partitionname>|Obbligatorio. Specifica il nome DNS della partizione di directory applicativa TAPI che il punto di connessione del servizio pubblicherà.|
|/Domain:\<domainname>|Specifica il nome DNS del dominio in cui viene creato il punto di connessione del servizio. Se il nome di dominio non è specificato, viene utilizzato il nome del dominio locale.|
|/forcedefault|Specifica che questa directory è la partizione di directory dell'applicazione TAPI predefinita per il dominio. In un dominio possono essere presenti più partizioni di directory applicative TAPI.|
|/?|Visualizza la guida al prompt dei comandi.|

## <a name="tapicfg-removescp"></a><a name="BKMK_removescp"></a>removescp tapicfg
Rimuove un punto di connessione del servizio per una partizione di directory applicativa TAPI.

### <a name="syntax"></a>Sintassi
```
tapicfg removescp /directory:<PartitionName> [/domain:<DomainName>]
```
#### <a name="parameters"></a>Parametri
|Parametro|Descrizione|
|-------|--------|
|removescp/directory:\<partitionname>|Obbligatorio. Specifica il nome DNS della partizione di directory applicativa TAPI per la quale viene rimosso un punto di connessione del servizio.|
|/Domain: \<domainname>|Specifica il nome DNS del dominio da cui viene rimosso il punto di connessione del servizio. Se il nome di dominio non è specificato, viene utilizzato il nome del dominio locale.|
|/?|Visualizza la guida al prompt dei comandi.|

## <a name="tapicfg-show"></a><a name="BKMK_show"></a>tapicfg show
Consente di visualizzare i nomi e i percorsi delle partizioni di directory applicative TAPI nel dominio.

### <a name="syntax"></a>Sintassi
```
tapicfg show [/defaultonly][ /domain:<DomainName>]
```
#### <a name="parameters"></a>Parametri
|Parametro|Descrizione|
|-------|--------|
|/defaultonly|Consente di visualizzare i nomi e i percorsi solo della partizione di directory applicativa TAPI predefinita nel dominio.|
|/Domain: \<domainname>|Specifica il nome DNS del dominio per cui vengono visualizzate le partizioni di directory dell'applicazione TAPI. Se il nome di dominio non è specificato, viene utilizzato il nome del dominio locale.|
|/?|Visualizza la guida al prompt dei comandi.|

## <a name="tapicfg-makedefault"></a><a name="BKMK_makedefault"></a>makedefault tapicfg
Imposta la partizione di directory dell'applicazione TAPI predefinita per il dominio.

### <a name="syntax"></a>Sintassi
```
tapicfg makedefault /directory:<PartitionName> [/domain:<DomainName>]  
```
#### <a name="parameters"></a>Parametri
|Parametro|Descrizione|
|-------|--------|
|makedefault/directory:\<partitionname>|Obbligatorio. Specifica il nome DNS della partizione di directory dell'applicazione TAPI impostata come partizione predefinita per il dominio. Si noti che questo nome deve essere un nome di dominio completo. Specifica il nome DNS del dominio per il quale la partizione di directory applicativa TAPI è impostata come predefinita. Se il nome di dominio non è specificato, viene utilizzato il nome del dominio locale.|
|/?|Visualizza la guida al prompt dei comandi.|

## <a name="remarks"></a>Osservazioni
È necessario essere un membro del gruppo Enterprise Admins in Active Directory per eseguire **tapicfg install** (per creare una partizione di directory applicativa TAPI) o **tapicfg remove** (per rimuovere una partizione di directory applicativa TAPI).

Questo strumento da riga di comando può essere eseguito in qualsiasi computer membro del dominio.

Il testo fornito dall'utente, ad esempio i nomi delle partizioni di directory applicative TAPI, dei server e dei domini, con caratteri internazionali o Unicode, viene visualizzato correttamente solo se sono installati i tipi di carattere e il supporto della lingua appropriati.

È comunque possibile usare i server del servizio di localizzazione Internet (ILS) nell'organizzazione, se è necessario il supporto di ILS per alcune applicazioni, perché i client TAPI che eseguono Windows XP o un sistema operativo Windows Server 2003 possono eseguire query su server ILS o partizioni di directory applicative TAPI.

È possibile usare **Tapicfg** per creare o rimuovere punti di connessione del servizio. Se la partizione di directory applicativa TAPI viene rinominata per qualsiasi motivo (ad esempio, se si rinomina il dominio in cui risiede), è necessario rimuovere il punto di connessione del servizio esistente e crearne uno nuovo contenente il nuovo nome DNS della partizione di directory applicativa TAPI da pubblicare. In caso contrario, i client TAPI non sono in grado di individuare e accedere alla partizione di directory applicativa TAPI. È anche possibile rimuovere un punto di connessione del servizio per scopi di manutenzione o sicurezza (ad esempio, se non si vuole esporre i dati TAPI in una partizione di directory applicativa TAPI specifica).

## <a name="examples"></a>Esempi
Per creare una partizione di directory applicativa TAPI denominata tapifiction.testdom.microsoft.com in un server denominato testdc.testdom.microsoft.com e quindi impostarla come partizione di directory dell'applicazione TAPI predefinita per il nuovo dominio, digitare:
```
tapicfg install /directory:tapifiction.testdom.microsoft.com /server:testdc.testdom.microsoft.com /forcedefault
```
Per visualizzare il nome della partizione di directory dell'applicazione TAPI predefinita per il nuovo dominio, digitare:
```
tapicfg show /defaultonly
```
## <a name="additional-references"></a>Altri riferimenti
-   - [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)
