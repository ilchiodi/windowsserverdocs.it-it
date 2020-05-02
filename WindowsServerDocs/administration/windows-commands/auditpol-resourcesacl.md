---
title: resourceSACL auditpol
description: Argomento di riferimento per il comando auditpol resourceSACL, che consente di configurare gli elenchi di controllo di accesso (SAcl) globali del sistema delle risorse.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 28771ba7-967a-45e9-9bf0-b2a2673070f0
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: befb76a21880171740b051c987dfd4d9329ecc9e
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/01/2020
ms.locfileid: "82719044"
---
# <a name="auditpol-resourcesacl"></a>resourceSACL auditpol

> Si applica a: Windows 7 e Windows Server 2008 R2

Configura gli elenchi di controllo di accesso (SACL) globali del sistema di risorse.

Per eseguire operazioni *resourceSACL* , è necessario disporre delle autorizzazioni di **controllo completo** o di **scrittura** per tale set di oggetti nel descrittore di sicurezza. È anche possibile eseguire operazioni *resourceSACL* se si dispone del diritto utente **Gestisci il registro di controllo e di protezione** (SeSecurityPrivilege).

## <a name="syntax"></a>Sintassi

```
auditpol /resourceSACL
[/set /type:<resource> [/success] [/failure] /user:<user> [/access:<access flags>]]
[/remove /type:<resource> /user:<user> [/type:<resource>]]
[/clear [/type:<resource>]]
[/view [/user:<user>] [/type:<resource>]]
```

### <a name="parameters"></a>Parametri

| Parametro | Descrizione |
| --------- | ----------- |
| /set | Aggiunge una nuova voce a o aggiorna una voce esistente nel SACL della risorsa per il tipo di risorsa specificato. |
| /remove | Rimuove tutte le voci per l'utente specificato nell'elenco di controllo di accesso agli oggetti globale. |
| /Clear | Rimuove tutte le voci dall'elenco di controllo di accesso agli oggetti globale.|
| /View | Elenca le voci di controllo di accesso agli oggetti globali in un SACL di risorsa. I tipi di utente e di risorsa sono facoltativi. |
| /? | Visualizza la guida al prompt dei comandi. |

### <a name="arguments"></a>Argomenti

| Argomento | Descrizione |
| -------- | ----------- |
| /type | Risorsa per la quale è in corso la configurazione del controllo di accesso agli oggetti. I valori di argomento supportati e con distinzione tra maiuscole e minuscole sono *file* (per directory e file) e *chiave* (per le chiavi del registro di sistema). |
| /Success | Specifica il controllo di esito positivo. |
| /Failure | Specifica il controllo degli errori. |
| /User | Specifica un utente in uno dei formati seguenti:<ul><li> DomainName\Account (ad esempio DOM\Administrators)</li><li>Account StandaloneServer\Group (vedere la [funzione LookupAccountName](https://docs.microsoft.com/windows/win32/api/winbase/nf-winbase-lookupaccountnamea))</li><li>{S-1-x-x-x-x} (x è espresso in decimale e l'intero SID deve essere racchiuso tra parentesi graffe). Ad esempio: {S-1-5-21-5624481-130208933-164394174-1001}<p>**Nota:** Se viene usato il modulo SID, non viene eseguita alcuna verifica per verificare l'esistenza di questo account.</li></ul> |
| /access | Specifica una maschera di autorizzazione che può essere specificata tramite:<p>Diritti di accesso generici, tra cui:<ul><li>GA-GENERICO ALL</li><li>GR-LETTURA GENERICA</li><li>GW-SCRITTURA GENERICA</li><li>GX-ESECUZIONE GENERICA</li></ul><p>Diritti di accesso per i file, tra cui:<ul><li>FA-FILE TUTTI GLI ACCESSI</li><li>FR-FILE LETTO GENERICO</li><li>SCRITTURA GENERICA FILE FW</li><li>ESECUZIONE GENERICA FX-FILE</li></ul><p>Diritti di accesso per le chiavi del registro di sistema, tra cui:<ul><li>ACCESSO A TUTTE LE CHIAVI KA</li><li>KR-LETTURA CHIAVE</li><li>KW-SCRITTURA CHIAVE</li><li>ESECUZIONE DELLA CHIAVE KX</li></ul><p>Ad esempio: `/access:FRFW` Abilita gli eventi di controllo per le operazioni di lettura e scrittura.<p>Valore esadecimale che rappresenta la maschera di accesso (ad esempio 0x1200a9)<p>Questa operazione è utile quando si utilizzano maschere di bit specifiche delle risorse che non fanno parte dello standard SDDL (Security Descriptor Definition Language). Se omesso, viene utilizzato l'accesso completo. |

## <a name="examples"></a>Esempi

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

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)

- [comandi auditpol](auditpol.md)
