---
title: cmdkey
description: Argomento di riferimento per il comando cmdkey, che consente di creare, elencare ed eliminare nomi utente e password archiviati e credenziali.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 5fcd68ee-a14a-4b71-9300-c3f5c5d31e8e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 4005707785101fcc1fb0030ffe895668bd65f730
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/01/2020
ms.locfileid: "82712160"
---
# <a name="cmdkey"></a>cmdkey

> Si applica a: Windows Server (canale semestrale), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Crea, elenca ed elimina nomi utente e password o credenziali archiviati.

## <a name="syntax"></a>Sintassi

```
cmdkey [{/add:<targetname>|/generic:<targetname>}] {/smartcard | /user:<username> [/pass:<password>]} [/delete{:<targetname> | /ras}] /list:<targetname>
```

### <a name="parameters"></a>Parametri

| Parametri | Descrizione |
| ---------- | ----------- |
| /Add`<targetname>` | Aggiunge un nome utente e una password all'elenco.<p>Richiede il parametro `<targetname>` che identifica il nome del computer o del dominio a cui verrà associata questa voce. |
| /problemi generici`<targetname>` | Aggiunge credenziali generiche all'elenco.<p>Richiede il parametro `<targetname>` che identifica il nome del computer o del dominio a cui verrà associata questa voce. |
| /Smartcard | Recupera le credenziali da una smart card. Se viene trovata più di una smart card nel sistema quando viene usata questa opzione, **cmdkey** Visualizza informazioni su tutte le smart card disponibili, quindi chiede all'utente di specificare quale usare. |
| /User`<username>` | Specifica il nome dell'utente o dell'account da archiviare con questa voce. Se `<username>` non è specificato, verrà richiesto. |
|passare`<password>` | Specifica la password da archiviare con questa voce. Se `<password>` non è specificato, verrà richiesto. Le password non vengono visualizzate dopo che sono state archiviate. |
| /delete{:`<targetname>` | Ras | Elimina un nome utente e una password dall'elenco. Se `<targetname>` si specifica, la voce viene eliminata. Se `/ras` si specifica, la voce di accesso remoto archiviata viene eliminata. |
| /List`<targetname>` | Visualizza l'elenco dei nomi utente e delle credenziali archiviati. Se `<targetname>` non è specificato, vengono elencati tutti i nomi utente e le credenziali archiviati. |
| /? | Visualizza la guida al prompt dei comandi. |

## <a name="examples"></a>Esempi

Per visualizzare un elenco di tutti i nomi utente e le credenziali archiviati, digitare:

```
cmdkey /list
```

Per aggiungere un nome utente e una password per l'utente *Zaffaronif* per accedere al computer *Server01* con la password *Kleo*, digitare:

```
cmdkey /add:server01 /user:mikedan /pass:Kleo
```

Per aggiungere un nome utente e una password per l'utente *Zaffaronif* per accedere al computer *Server01* e richiedere la password ogni volta che viene eseguito l'accesso a Server01, digitare:

```
cmdkey /add:server01 /user:mikedan
```

Per eliminare una credenziale archiviata da accesso remoto, digitare:

```
cmdkey /delete /ras
```

Per eliminare una credenziale archiviata per *Server01*, digitare:

```
cmdkey /delete:server01
```

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)
