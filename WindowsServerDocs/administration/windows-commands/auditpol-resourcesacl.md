---
title: resourceSACL auditpol
description: 'Argomento dei comandi di Windows per **Uditpol resourceSACL** : Configura gli elenchi di controllo di accesso (SACL) globale delle risorse del sistema.'
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 2acffd75298f0f36a9c15e0622816feaae57cb64
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71382433"
---
# <a name="auditpol-resourcesacl"></a>resourceSACL auditpol



Configura gli elenchi di controllo di accesso (SACL) globali del sistema di risorse.

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
|/set|Aggiunge una nuova voce a o aggiorna una voce esistente nel SACL della risorsa per il tipo di risorsa specificato.|
|/remove|Rimuove tutte le voci per l'utente specificato nell'elenco di controllo di accesso agli oggetti globale.|
|/Clear|Rimuove tutte le voci dall'elenco di controllo di accesso agli oggetti globale.|
|/View|Elenca le voci di controllo di accesso agli oggetti globali in un SACL di risorsa. I tipi di utente e di risorsa sono facoltativi.|
|/?|Visualizza la guida al prompt dei comandi.|

## <a name="arguments"></a>Argomenti

|Argomento|Descrizione|
|--------|-----------|
|/Type|Risorsa per la quale è in corso la configurazione del controllo di accesso agli oggetti. I valori di argomento supportati sono file (per directory e file) e chiave (per le chiavi del registro di sistema).</br>Nota: I valori del file e della chiave fanno distinzione tra maiuscole e minuscole.|
|/Success|Specifica il controllo di esito positivo.|
|/Failure|Specifica il controllo degli errori.|
|/User|Specifica un utente in uno dei formati seguenti:</br>-DomainName\Account (ad esempio DOM\Administrators)</br>-Account StandaloneServer\Group (vedere la [funzione LookupAccountName](https://msdn.microsoft.com/library/windows/desktop/aa379159(v=vs.85).aspx))</br>-{S-1-x-x-x-x} (x è espresso in decimale e l'intero SID deve essere racchiuso tra parentesi graffe); ad esempio: {S-1-5-21-5624481-130208933-164394174-1001}</br>    Nota:     Se viene usato il modulo SID, non viene eseguita alcuna verifica per verificare l'esistenza di questo account.|
|/access|Specifica una maschera di autorizzazione che può essere specificata in uno dei due formati seguenti:</br>-Una sequenza di diritti semplici:</br>    -Diritti di accesso generici:</br>        -GA-GENERICO ALL</br>        -GR-LETTURA GENERICA</br>        -GW-SCRITTURA GENERICA</br>        -GX-ESECUZIONE GENERICA</br>    -Diritti di accesso per i file:</br>        -FA-TUTTI I FILE DI ACCESSO</br>        -FR-FILE LETTO GENERICO</br>        -FW-SCRITTURA GENERICA FILE</br>        -FX-FILE GENERICO EXECUTE</br>    -Diritti di accesso per le chiavi del registro di sistema:</br>        -KA-CHIAVE TUTTI GLI ACCESSI</br>        -KR-LETTURA CHIAVE</br>        -KW-SCRITTURA CHIAVE</br>        -KX-ESECUZIONE CHIAVE</br>    Ad esempio:'/Access: FRFW ' consentirà gli eventi di controllo per le operazioni di lettura e scrittura</br>: Valore esadecimale che rappresenta la maschera di accesso (ad esempio 0x1200a9)</br>    Questa operazione è utile quando si utilizzano maschere di bit specifiche delle risorse che non fanno parte dello standard SDDL (Security Descriptor Definition Language). Se omesso, viene utilizzato l'accesso completo.|

## <a name="remarks"></a>Note

Per le operazioni resourceSACL, è necessario disporre dell'autorizzazione di controllo completo o di scrittura per tale set di oggetti nel descrittore di sicurezza. È anche possibile eseguire operazioni resourceSACL possedendo il diritto utente **Gestisci il registro di controllo e di protezione** (SeSecurityPrivilege). Tuttavia, questo diritto consente un accesso aggiuntivo che non è necessario per eseguire l'operazione di rimozione.

## <a name="BKMK_Examples"></a>Esempi

Per impostare un SACL di risorsa globale per controllare i tentativi di accesso riusciti da un utente su una chiave del registro di sistema:
```
auditpol /resourceSACL /set /type:Key /user:MYDOMAIN\myuser /success
```
Per impostare un SACL di risorsa globale per controllare i tentativi riusciti e non riusciti da un utente per eseguire funzioni generiche di lettura e scrittura su file o cartelle:
```
auditpol /resourceSACL /set /type:File /user:MYDOMAIN\myuser /success /failure /access:FRFW
```
Per rimuovere tutte le voci SACL di risorse globali per file o cartelle:
```
auditpol /resourceSACL /type:File /clear
```
Per rimuovere tutte le voci SACL delle risorse globali per un determinato utente da file o cartelle:
```
auditpol /resourceSACL /remove /type:File /user:{S-1-5-21-56248481-1302087933-1644394174-1001}
```
Per elencare le voci di controllo di accesso agli oggetti globali impostate per i file o le cartelle:
```
auditpol /resourceSACL /type:File /view
```
Per elencare le voci di controllo di accesso agli oggetti globali per un determinato utente che sono impostate su file o cartelle:
```
auditpol /resourceSACL /type:File /view /user:MYDOMAIN\myuser
```

#### <a name="additional-references"></a>Altri riferimenti

[Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)