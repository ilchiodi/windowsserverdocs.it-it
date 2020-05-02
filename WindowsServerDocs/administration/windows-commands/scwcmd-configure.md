---
title: Configurazione di Scwcmd
description: Argomento di riferimento per * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 6528b9dc-3d82-4228-b734-ed717458d74c
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 6ffaff594a8927b3fcdfc871ec380fd5f134ce90
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/01/2020
ms.locfileid: "82722153"
---
# <a name="scwcmd-configure"></a>Scwcmd: configurare

> Si applica a: Windows Server 2012 R2, Windows Server 2012

Si applica un criterio di sicurezza generato configurazione guidata Impostazioni di sicurezza a un computer. Questo strumento da riga di comando accetta inoltre un elenco di nomi di computer come input.

## <a name="syntax"></a>Sintassi

```
scwcmd configure [[[/m:<ComputerName> | /ou:<OuName>] /p:<Policy>] | /i:<ComputerList>] [/u:<UserName>] [/pw:<Password>] [/t:<Threads>]
```

#### <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|/m:\<nomecomputer>|Specifica il nome NetBIOS, un nome DNS o un indirizzo IP del computer da configurare. Se il **/m** viene specificato, il **/p** parametro deve inoltre essere specificato.|
|/ou:\<OuName>|Specifica il nome di dominio completo (FQDN) di un'unità organizzativa (OU) in servizi di dominio Active Directory. Se il **/ou** viene specificato, il **/p** parametro deve inoltre essere specificato. Tutti i computer nell'unità Organizzativa verranno analizzati in base al criterio specificato.|
|/p:\<> criteri|Specifica il percorso e il nome del file di criteri con estensione XML da utilizzare per eseguire la configurazione.|
|/i:\<> di computer|Specifica il percorso e il nome di un file XML che contiene un elenco di computer insieme ai relativi file di criteri previsto. Tutti i computer nel file XML verranno configurati in base ai relativi file di criteri corrispondenti. Un file XML di esempio è % windir%\security\SampleMachineList.xml.|
|/u:\<nome utente>|Specifica una credenziale utente alternativo da utilizzare quando si configura un computer remoto. Il valore predefinito è connesso per utente.|
|/PW:\<password>|Specifica una credenziale utente alternativo da utilizzare quando si configura un computer remoto. Il valore predefinito è la password dell'utente connesso.|
|/t:\<thread>|Specifica il numero di operazioni di configurazione in sospeso simultanee che deve essere garantita durante il processo di configurazione (valore predefinito = 40, MinValue = 1, MaxValue = 1000).|
|/?|Visualizza la guida al prompt dei comandi.|

## <a name="remarks"></a>Osservazioni

Scwcmd.exe è disponibile solo nei computer che eseguono Windows Server 2008 R2, Windows Server 2008 o Windows Server 2003.

## <a name="examples"></a>Esempi

Per configurare un criterio di protezione contro la webpolicy.xml di file, digitare:
```
scwcmd configure /p:webpolicy.xml
```
Per configurare un criterio di sicurezza per il computer utilizzando le credenziali dell'account esso 172.16.0.0 contro webpolicy.xml il file, digitare:
```
scwcmd configure /m:172.16.0.0 /p:webpolicy.xml /u:webadmin
```
Per configurare un criterio di protezione in tutti i computer campusmachines.xml l'elenco con un massimo di 100 thread, digitare:
```
scwcmd configure /i:campusmachines.xml /t:100
```
Per configurare un criterio di protezione in tutti i computer nella OU WebServers contro webpolicy.xml il file utilizzando le credenziali dell'account DomainAdmin, digitare:
```
scwcmd configure /ou:OU=WebServers,DC=Marketing,DC=ABCCompany,DC=com /p:webpolicy.xml /u:DomainAdmin
```

## <a name="additional-references"></a>Riferimenti aggiuntivi

-   - [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)