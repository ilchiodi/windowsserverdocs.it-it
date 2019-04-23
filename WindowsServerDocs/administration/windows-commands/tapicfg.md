---
title: tapicfg
description: Informazioni su come gestire una partizione di directory applicativa TAPI.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: c0e642ce-5d98-4edb-9a65-1dff09aef4e1
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 07/11/2018
ms.openlocfilehash: 6e89b1e3d7638fbcbe0140658d2d2a9af1ccd852
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59886512"
---
# <a name="tapicfg"></a>tapicfg

>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Crea, rimuove, o una partizione di directory applicativa TAPI Visualizza o imposta una partizione di directory applicativa predefinita TAPI. 3.1 TAPI client possono utilizzare le informazioni in questa partizione di directory dell'applicazione con l'individuazione del servizio di directory per individuare e comunicare con le directory TAPI. È anche possibile usare **tapicfg** per creare o rimuovere i punti di connessione del servizio, che consentono ai client TAPI individuare in modo efficiente le partizioni di directory applicative TAPI in un dominio. Per altre informazioni, vedere la sezione Osservazioni. Per visualizzare la sintassi del comando, fare clic su un comando. 
-   [tapicfg install](#BKMK_install)
-   [Rimuovi tapicfg](#BKMK_remove)
-   [tapicfg publishscp](#BKMK_publishscp)
-   [tapicfg removescp](#BKMK_removescp)
-   [tapicfg show](#BKMK_show)
-   [tapicfg makedefault](#BKMK_makedefault)

## <a name="BKMK_install"></a>installazione tapicfg
Crea una partizione di directory applicativa TAPI.

### <a name="syntax"></a>Sintassi
```
tapicfg install /directory:<PartitionName> [/server:<DCName>] [/forcedefault]
```
### <a name="parameters"></a>Parametri
|Parametro|Descrizione|
|-------|--------|
|installare /directory:\<PartitionName >|Obbligatorio. Specifica il nome DNS della partizione di directory dell'applicazione TAPI da creare. Questo nome deve essere un nome di dominio completo.|
|/Server: \<DCName>|Specifica il nome DNS del controller di dominio in cui viene creata la partizione di directory applicativa TAPI. Se il nome del controller di dominio viene omesso, viene utilizzato il nome del computer locale.|
|/forcedefault|Specifica che la directory è la partizione di directory applicativa TAPI predefinito per il dominio. Possono esserci più partizioni di directory applicative TAPI in un dominio.<br /><br />Se questa directory è la partizione di directory applicativa TAPI prima creata nel dominio, viene impostato automaticamente come il valore predefinito, indipendentemente dal fatto che si usi il **/forcedefault** opzione.|
|/?|Visualizza la guida al prompt dei comandi.|

## <a name="BKMK_remove"></a>Rimuovi tapicfg
Rimuove una partizione di directory applicativa TAPI.

### <a name="syntax"></a>Sintassi
```
tapicfg remove /directory:<PartitionName>
```
### <a name="parameters"></a>Parametri
|Parametro|Descrizione|
|-------|--------|
|rimuovere /directory:\<PartitionName >|Obbligatorio. Specifica il nome DNS della partizione di directory dell'applicazione TAPI da rimuovere. Si noti che questo nome deve essere un nome di dominio completo.|
|/?|Visualizza la guida al prompt dei comandi.|

## <a name="BKMK_publishscp"></a>tapicfg publishscp
Crea un punto di connessione del servizio per pubblicare una partizione di directory applicativa TAPI.

### <a name="syntax"></a>Sintassi
```
tapicfg publishscp /directory:<PartitionName> [/domain:<DomainName>] [/forcedefault]
```
### <a name="parameters"></a>Parametri
|Parametro|Descrizione|
|-------|--------|
|publishscp /directory:\<PartitionName >|Obbligatorio. Specifica il nome DNS della partizione di directory dell'applicazione TAPI che il servizio punto di connessione verrà pubblicati.|
|/domain:\<DomainName>|Specifica il nome DNS del dominio in cui punto di connessione del servizio viene creato. Se il nome di dominio viene omesso, viene utilizzato il nome del dominio locale.|
|/forcedefault|Specifica che la directory è la partizione di directory applicativa TAPI predefinito per il dominio. Possono esserci più partizioni di directory applicative TAPI in un dominio.|
|/?|Visualizza la guida al prompt dei comandi.|

## <a name="BKMK_removescp"></a>tapicfg removescp
Rimuove un punto di connessione del servizio per una partizione di directory applicativa TAPI.

### <a name="syntax"></a>Sintassi
```
tapicfg removescp /directory:<PartitionName> [/domain:<DomainName>]
```
### <a name="parameters"></a>Parametri
|Parametro|Descrizione|
|-------|--------|
|removescp /directory:\<PartitionName >|Obbligatorio. Specifica il nome DNS della partizione di directory dell'applicazione TAPI per il quale viene rimosso un punto di connessione del servizio.|
|/Domain: \<DomainName>|Specifica il nome DNS del dominio da cui viene rimosso il punto di connessione del servizio. Se il nome di dominio viene omesso, viene utilizzato il nome del dominio locale.|
|/?|Visualizza la guida al prompt dei comandi.|

## <a name="BKMK_show"></a>tapicfg show
Visualizza i nomi e percorsi di directory applicativa TAPI nel dominio.

### <a name="syntax"></a>Sintassi
```
tapicfg show [/defaultonly][ /domain:<DomainName>]
```
### <a name="parameters"></a>Parametri
|Parametro|Descrizione|
|-------|--------|
|/defaultonly|Visualizza i nomi e percorsi di solo l'impostazione predefinita TAPI partizione di directory applicativa nel dominio.|
|/Domain: \<DomainName>|Specifica il nome DNS del dominio per il quale vengono visualizzate le partizioni di directory applicative TAPI. Se il nome di dominio viene omesso, viene utilizzato il nome del dominio locale.|
|/?|Visualizza la guida al prompt dei comandi.|

## <a name="BKMK_makedefault"></a>tapicfg makedefault
Imposta la partizione di directory applicativa TAPI predefinito per il dominio.

### <a name="syntax"></a>Sintassi
```
tapicfg makedefault /directory:<PartitionName> [/domain:<DomainName>]  
```
### <a name="parameters"></a>Parametri
|Parametro|Descrizione|
|-------|--------|
|makedefault /directory:\<PartitionName >|Obbligatorio. Specifica il nome DNS della partizione di directory dell'applicazione TAPI imposta come partizione predefinita per il dominio. Si noti che questo nome deve essere un nome di dominio completo. Specifica il nome DNS del dominio per il quale la partizione di directory applicativa TAPI è impostata come predefinito. Se il nome di dominio viene omesso, viene utilizzato il nome del dominio locale.|
|/?|Visualizza la guida al prompt dei comandi.|

## <a name="remarks"></a>Note
È necessario essere un membro del gruppo Enterprise Admins in active directory per eseguire uno **installare tapicfg** (per creare una partizione di directory applicativa TAPI) o **tapicfg rimuovere** (per rimuovere un'applicazione di TAPI partizione di directory).

Questo strumento da riga di comando può essere eseguito in qualsiasi computer che è un membro del dominio.

Testo specificato dall'utente (ad esempio i nomi di directory applicative, server e domini TAPI) con i caratteri Unicode o International vengono visualizzati correttamente solo se sono installati i tipi di carattere appropriati e supporto del linguaggio.

È possibile usare ancora il server di individuazione del servizio ILS (Internet) nell'organizzazione, se sono richiesti per supportare determinate applicazioni, perché client TAPI che eseguono un sistema operativo Windows Server 2003 o Windows XP può eseguire query su server ILS o applicazione TAPI partizioni di directory.

È possibile usare **tapicfg** per creare o rimuovere punti di connessione del servizio. Se la partizione di directory applicativa TAPI viene rinominata per qualsiasi motivo (ad esempio, se si rinomina il dominio in cui si trova), è necessario rimuovere il punto di connessione del servizio esistente e crearne uno nuovo che contiene il nuovo nome DNS della directory dell'applicazione TAPI partizione da pubblicare. In caso contrario, i client TAPI sono in grado di individuare e accedere la partizione di directory applicativa TAPI. È anche possibile rimuovere un punto di connessione del servizio per scopi di manutenzione o di sicurezza (ad esempio, se si desidera esporre i dati TAPI su una specifica partizione di directory applicativa TAPI).

## <a name="examples"></a>Esempi
Per creare una partizione di directory applicativa TAPI tapifiction.testdom.microsoft.com denominata in un server denominato testdc.testdom.microsoft.com e successivamente impostata come partizione di directory applicativa TAPI l'impostazione predefinita per il nuovo dominio, digitare:
```
tapicfg install /directory:tapifiction.testdom.microsoft.com /server:testdc.testdom.microsoft.com /forcedefault
```
Per visualizzare il nome della partizione di directory predefinito TAPI dell'applicazione per il nuovo dominio, digitare:
```
tapicfg show /defaultonly
```
## <a name="additional-references"></a>Altri riferimenti
-   [Chiave sintassi della riga di comando](command-line-syntax-key.md)
