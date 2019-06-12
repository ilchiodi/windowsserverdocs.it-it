---
title: nfsshare
description: 'Argomento i comandi di Windows per * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 437a2615-335a-442f-9713-d50d5f3983a3
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: dc77825d63875861839ecdb22bee5a62375aaa13
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/31/2019
ms.locfileid: "66437047"
---
# <a name="nfsshare"></a>nfsshare



È possibile usare **nfsshare** al controllo condivisioni di File System NFS (Network).

## <a name="syntax"></a>Sintassi

```
nfsshare <ShareName>=<Drive:Path> [-o <Option=value>...]
nfsshare {<ShareName> | <Drive>:<Path> | * } /delete
```

## <a name="description"></a>Descrizione

Senza argomenti, il **nfsshare** utilità della riga di comando Elenca tutte le condivisioni di File System NFS (Network) esportate dal Server per NFS. Con *ShareName* come unico argomento, **nfsshare** sono elencate le proprietà della condivisione NFS identificati da *ShareName*. Quando *ShareName* e <em>unità</em> **:** <em>Path</em> vengono forniti **nfsshare** Esporta la cartella identificato da <em>Drive</em> **:** <em>Path</em> come *nomecondivisione*. Quando la **/Elimina** opzione viene utilizzata, la cartella specificata non è più viene resa disponibile ai client NFS.

## <a name="options"></a>Opzioni

Il **nfsshare** comando accetta le opzioni e gli argomenti seguenti:


|             Nome              |                                                                                                                                                                                                                      Definizione                                                                                                                                                                                                                       |
|-------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|         / o di autenticazione anon = {Sì          |                                                                                                                                                                                                                          no}                                                                                                                                                                                                                          |
|  rw -o [=\<Host > [:<Host>]...]  |                       Fornisce accesso in lettura-scrittura alla directory condivisa dall'host o client gruppi specificati da *Host*. Separare i nomi host e gruppi con due punti ( **:** ). Se *Host* non viene specificato, tutti gli host e gruppi di client (ad eccezione di quelle specificate con il **ro** opzione) hanno accesso in lettura / scrittura. Se non si specifica la **ro** né la **rw** opzione è impostata, tutti i client hanno accesso in lettura-scrittura alla directory condivisa.                       |
|  ro -o [=\<Host > [:<Host>]...]  | Fornisce accesso in lettura alla directory condivisa dall'host o client gruppi specificati da *Host*. Separare i nomi host e gruppi con due punti ( **:** ). Se *Host* non viene specificato, tutti i client (ad eccezione di quelle specificate con il **rw** opzione) hanno accesso in sola lettura. Se il **ro** opzione è impostata per uno o più client, ma la **rw** opzione non è impostata, solo i client specificati con il **ro** opzione hanno accesso alla directory condivisa. |
|       -o codifica = {big5       |                                                                                                                                                                                                                        euc-jp                                                                                                                                                                                                                         |
|       -o anongid =\<gid >       |                                                                                     Specifica che gli utenti (non mappati) anonimi accederanno nella directory di condivisione usando *gid* come l'identificatore di gruppo (GID). Il valore predefinito è -2. GID anonimo verrà utilizzato per segnalare il proprietario di un file di proprietà di un utente non mappato, anche se l'accesso anonimo è disabilitato.                                                                                      |
|      -o anonuid =\<uid >       |                                                                                      Specifica che gli utenti (non mappati) anonimi accederanno nella directory di condivisione usando *uid* come ID utente (UID). Il valore predefinito è -2. UID anonimo verrà utilizzato per segnalare il proprietario di un file di proprietà di un utente non mappato, anche se l'accesso anonimo è disabilitato.                                                                                      |
| radice -o [=\<Host > [:<Host>]...] |                                                                         Fornisce l'accesso radice nella directory condivisa dall'host o client gruppi specificati da *Host*. Separare i nomi host e gruppi con due punti ( **:** ). Se *Host* non viene specificato, tutti i client hanno accesso alla directory radice. Se il **radice** opzione non è impostata, nessun client ha accesso alla directory radice per la directory condivisa.                                                                         |
|            /delete            |                                                                                                                                                       Se *ShareName* oppure <em>Drive</em> **:** <em>percorso</em> è specificato, consente di eliminare la condivisione specificata. Se \* è specificata, Elimina tutte le condivisioni NFS.                                                                                                                                                       |

> [!NOTE]
> Per visualizzare la sintassi completa del comando, al prompt dei comandi digitare:</br>> **nfsshare /?**

# #

[Riferimento ai comandi di sistema di servizi per la rete File](services-for-network-file-system-command-reference.md) vedere anche