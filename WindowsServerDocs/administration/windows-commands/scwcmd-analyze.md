---
title: Scwcmd analyze
description: 'Argomento dei comandi di Windows per * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 0259271b-be5b-48d7-a51d-8b9b6786efb4
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 14426ae33144ae9bdd8f8154b4be74a3f088606b
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71371257"
---
# <a name="scwcmd-analyze"></a>Scwcmd: analizzare

> Si applica a: Windows Server 2012 R2, Windows Server 2012

Determina se un computer è in conformità con i criteri. Vengono restituiti in un file XML. Accetta inoltre un elenco di nomi di computer come input. Per visualizzare i risultati nel browser, utilizzare **visualizzazione scwcmd** e specificare **%windir%\security\msscw\TransformFiles\scwanalysis.xsl** come la trasformazione XSL. Per esempi di come è possibile utilizzare questo comando, vedere [esempi](#BKMK_Examples).

## <a name="syntax"></a>Sintassi

```
scwcmd analyze [[[/m:<ComputerName> | /ou:<Ou>] /p:<Policy>] | /i:<ComputerList>] [/o:
<ResultDir>] [/u:<UserName>] [/pw:<Password>] [/t:<Threads>] [/l] [/e]
```

### <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|/m: \<ComputerName >|Specifica il nome NetBIOS, un nome DNS o un indirizzo IP del computer da analizzare. Se il **/m** viene specificato, il **/p** parametro deve inoltre essere specificato.|
|/ou: > \<OuName|Specifica il nome di dominio completo (FQDN) di un'unità organizzativa (OU) in servizi di dominio Active Directory. Se il **/ou** viene specificato, il **/p** parametro deve inoltre essere specificato. Tutti i computer nell'unità Organizzativa verranno analizzati in base al criterio specificato.|
|/p: \<Policy >|Specifica il percorso e il nome del file di criteri con estensione XML da utilizzare per eseguire l'analisi.|
|/i: \<ComputerList >|Specifica il percorso e il nome di un file XML che contiene un elenco di computer insieme ai relativi file di criteri previsto. Verranno analizzati tutti i computer nel file XML con i relativi file di criteri corrispondente. Un file XML di esempio è % windir%\security\SampleMachineList.xml.|
|/o: \<ResultDir >|Specifica il percorso e la directory in cui salvare il file dei risultati di analisi. Il valore predefinito è la directory corrente.|
|/u: \<UserName >|Specifica una credenziale utente alternativo da utilizzare durante l'analisi in un computer remoto. Il valore predefinito è connesso per utente.|
|/PW: > \<Password|Specifica una credenziale utente alternativo da utilizzare durante l'analisi in un computer remoto. Il valore predefinito è la password dell'utente connesso.|
|/t: \<Threads >|Specifica il numero di operazioni di analisi in sospeso simultanee che deve essere mantenuta durante l'analisi (valore predefinito = 40, MinValue = 1, MaxValue = 1000).|
|/l|Fa sì che il processo di analisi da registrare. Verrà generato un file di log per ogni computer che si sta analizzando. Nella stessa directory di file dei risultati verranno archiviati i file di log. Utilizzare il **/o** opzione per specificare la directory per il file dei risultati.|
|/e|Registrare un evento nel registro eventi dell'applicazione se viene trovata una mancata corrispondenza.|
|/?|Visualizza la guida al prompt dei comandi.|

## <a name="remarks"></a>Note

Scwcmd.exe è disponibile solo nei computer che eseguono Windows Server 2008 R2, Windows Server 2008 o Windows Server 2003.

## <a name="BKMK_Examples"></a>Esempi

Per analizzare un criterio di protezione contro la webpolicy.xml di file, digitare:
```
scwcmd analyze /p:webpolicy.xml

```
Per analizzare un criterio di protezione nel computer denominato webserver contro webpolicy.xml il file utilizzando le credenziali dell'account di esso, digitare:
```
scwcmd analyze /m:webserver /p:webpolicy.xml /u:webadmin

```
Per analizzare un criterio di protezione contro la webpolicy.xml di file, con un massimo di 100 thread e restituire i risultati in un file denominato risultati nella condivisione di resultserver, digitare:
```
scwcmd analyze /i:webpolicy.xml /t:100 /o:\\resultserver\results

```
Per analizzare un criterio di protezione per le OU WebServers contro webpolicy.xml il file utilizzando le credenziali DomainAdmin, digitare:
```
scwcmd analyze /ou:OU=WebServers,DC=Marketing,DC=ABCCompany,DC=com /p:webpolicy.xml /u:DomainAdmin
```

#### <a name="additional-references"></a>Altri riferimenti

-   [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)