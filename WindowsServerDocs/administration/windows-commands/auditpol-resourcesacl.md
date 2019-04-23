---
title: auditpol resourceSACL
description: Argomento i comandi di Windows per **uditpol resourceSACL** -consente di configurare gli elenchi di controllo accesso risorse globali del sistema (SACL).
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 28771ba7-967a-45e9-9bf0-b2a2673070f0
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 375f37250404dd6740027cb18959697626c1ffc1
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59837492"
---
# <a name="auditpol-resourcesacl"></a>auditpol resourceSACL



Consente di configurare gli elenchi di controllo accesso risorse globali del sistema (SACL).

> [!NOTE]
> Si applica solo a Windows 7 e Windows Server 2008 R2.

Per esempi di come è possibile utilizzare questo comando, vedere [esempi](#BKMK_Examples).

## <a name="syntax"></a>Sintassi

```
auditpol /resourceSACL
[/set /type:<resource> [/success] [/failure] /user:<user> [/access:<access flags>]]
[/remove /type:<resource> /user:<user> [/type:<resource>]]
[/clear [/type:<resource>]]
[/view [/user:<user>] [/type:<resource>]]
```

## <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|/set|Aggiunge una nuova voce alla o ne aggiorna una voce esistente nella risorsa SACL per la risorsa di tipo specificato.|
|/remove|Rimuove tutte le voci per l'utente specificato l'accesso agli oggetti globale il controllo elenco.|
|/clear|Rimuove tutte le voci di accesso agli oggetti globale il controllo elenco.|
|/View|Elenca le voci di controllo di accesso oggetto globale in una risorsa SACL. I tipi di utente e risorse sono facoltativi.|
|/?|Visualizza la guida al prompt dei comandi.|

## <a name="arguments"></a>Argomenti

|Argomento|Descrizione|
|--------|-----------|
|/type|La risorsa per l'oggetto controllo dell'accesso è stata configurata. I valori di argomento supportati sono File (per file e directory) e la chiave (per le chiavi del Registro di sistema).</br>Nota: I valori di file e la chiave sono tra maiuscole e minuscole.|
|/success|Specifica il controllo della riuscita.|
|/failure|Specifica il controllo di un errore.|
|/User|Specifica un utente in uno dei formati seguenti:</br>-DomainName\Account (ad esempio DOM\Administrators)</br>-Account StandaloneServer\Group (vedere [funzione LookupAccountName](https://msdn.microsoft.com/library/windows/desktop/aa379159(v=vs.85).aspx))</br>-{S-1-x-x-x-x} (x è espresso in numeri decimali, e il SID intero deve essere racchiuso tra parentesi graffe); ad esempio: {S-1-5-21-5624481-130208933-164394174-1001}</br>    Nota:     Se viene usato il modulo di SID, non viene effettuato alcun controllo per verificare l'esistenza di questo account.|
|/access|Specifica una maschera di autorizzazione che può essere specificata in uno dei due formati:</br>-Una sequenza di diritti di base:</br>    -Diritti di accesso generico:</br>        -GA - GENERICO TUTTI</br>        -GR - GENERICO DI LETTURA</br>        -WRITE GW - GENERICO</br>        -GX - GENERICO ESEGUIRE</br>    -Diritti di accesso per i file:</br>        -FA - FILE TUTTI GLI ACCESSI</br>        LEGGERE FILE - FR - GENERICO</br>        SCRITTURA GENERICA DEL - SC: FILE</br>        ESECUZIONE - FX - FILE GENERICA</br>    -Diritti di accesso per le chiavi del Registro di sistema:</br>        -KA - CHIAVE TUTTI GLI ACCESSI</br>        LETTURA DI CHIAVI - KR-</br>        SCRITTURA DI CHIAVI - KW-</br>        KEY - KX - EXECUTE</br>    Ad esempio: ' / accesso: FRFW' verrà abilitare gli eventi di controllo per la lettura e operazioni di scrittura</br>-Un valore esadecimale che rappresenta la maschera di accesso (ad esempio 0x1200a9)</br>    Ciò è utile quando si utilizzano maschere di bit specifici della risorsa che non fanno parte dello standard security descriptor definition language (SDDL). Se omesso, viene utilizzato l'accesso completo.|

## <a name="remarks"></a>Note

Per le operazioni resourceSACL, è necessario disporre dell'autorizzazione di scrittura o controllo completo su tale oggetto impostato nel descrittore di sicurezza. È anche possibile eseguire operazioni resourceSACL che presenta il **gestione file registro di controllo e protezione** diritto utente (SeSecurityPrivilege). Tuttavia, questo diritto consente accesso aggiuntivo che non è necessario eseguire l'operazione di rimozione.

## <a name="BKMK_Examples"></a>Esempi

Per impostare una risorsa globale SACL per controllare gli accessi riusciti effettuati da un utente su una chiave del Registro di sistema:
```
auditpol /resourceSACL /set /type:Key /user:MYDOMAIN\myuser /success
```
Per impostare un SACL per controllare i tentativi riusciti e non da un utente di eseguire generico di lettura e scrittura delle funzioni in cartelle o file di risorsa globale:
```
auditpol /resourceSACL /set /type:File /user:MYDOMAIN\myuser /success /failure /access:FRFW
```
Per rimuovere tutte le voci di risorsa globale SACL per file o cartelle:
```
auditpol /resourceSACL /type:File /clear
```
Per rimuovere tutte le voci di elenco SACL di risorsa globale per un particolare utente dal file o cartelle:
```
auditpol /resourceSACL /remove /type:File /user:{S-1-5-21-56248481-1302087933-1644394174-1001}
```
Per elencare l'accesso agli oggetti globale impostato su file o cartelle di voci di controllo:
```
auditpol /resourceSACL /type:File /view
```
Per elencare l'oggetto globale accedere alle voci di controllo per un particolare utente impostate nel file o cartelle:
```
auditpol /resourceSACL /type:File /view /user:MYDOMAIN\myuser
```

#### <a name="additional-references"></a>Altri riferimenti

[Chiave sintassi della riga di comando](command-line-syntax-key.md)