---
title: nfsshare
description: Argomento dei comandi di Windows per * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 437a2615-335a-442f-9713-d50d5f3983a3
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 032baaf3013d2658b1040345da3a35cb6a1631f2
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80838904"
---
# <a name="nfsshare"></a>nfsshare



È possibile utilizzare **nfsshare** per controllare le condivisioni NFS (Network File System).

## <a name="syntax"></a>Sintassi

```
nfsshare <ShareName>=<Drive:Path> [-o <Option=value>...]
nfsshare {<ShareName> | <Drive>:<Path> | * } /delete
```

## <a name="description"></a>Descrizione

Senza argomenti, l'utilità da riga di comando **nfsshare** elenca tutte le condivisioni NFS (Network File System) esportate da server per NFS. Con *ShareName* come unico argomento, **nfsshare** elenca le proprietà della condivisione NFS identificata da *ShareName*. Quando vengono specificati *ShareName* e <em>Drive</em> **:** <em>path</em> , **nfsshare** Esporta la cartella identificata da <em>unità</em> **:** <em>percorso</em> come *ShareName*. Quando si utilizza l'opzione **/Delete** , la cartella specificata non viene più resa disponibile ai client NFS.

## <a name="options"></a>Opzioni

Il comando **nfsshare** accetta le opzioni e gli argomenti seguenti:


|             Termine              |                                                                                                                                                                                                                      Definizione                                                                                                                                                                                                                       |
|-------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|         -o anon = {Yes          |                                                                                                                                                                                                                          Non                                                                                                                                                                                                                          |
|  -o RW [=\<host > [:<Host>]...]  |                       Fornisce accesso in lettura/scrittura alla directory condivisa dagli host o dai gruppi di client specificati dall' *host*. Separare i nomi host e gruppo con i due punti ( **:** ). Se *host* non è specificato, tutti gli host e i gruppi di client (ad eccezione di quelli specificati con l'opzione **ro** ) avranno accesso in lettura/scrittura. Se non è impostata né l'opzione **ro** né l'opzione **RW** , tutti i client avranno accesso in lettura/scrittura alla directory condivisa.                       |
|  -o ro [=\<host > [:<Host>]...]  | Fornisce accesso in sola lettura alla directory condivisa dagli host o dai gruppi di client specificati dall' *host*. Separare i nomi host e gruppo con i due punti ( **:** ). Se l' *host* non è specificato, tutti i client (eccetto quelli specificati con l'opzione **RW** ) avranno accesso in sola lettura. Se l'opzione **ro** è impostata per uno o più client, ma l'opzione **RW** non è impostata, solo i client specificati con l'opzione **ro** possono accedere alla directory condivisa. |
|       -o codifica = {Big5       |                                                                                                                                                                                                                        EUC-JP                                                                                                                                                                                                                         |
|       -o anongid =\<GID >       |                                                                                     Specifica che gli utenti anonimi (non mappati) accederanno alla directory Share usando *GID* come identificatore del gruppo (GID). Il valore predefinito è-2. Il GID anonimo verrà usato quando si segnala il proprietario di un file di proprietà di un utente non mappato, anche se l'accesso anonimo è disabilitato.                                                                                      |
|      -o anonuid =\<UID >       |                                                                                      Specifica che gli utenti anonimi (non mappati) accederanno alla directory Share usando *UID* come identificatore utente (UID). Il valore predefinito è-2. L'UID anonimo verrà usato quando si segnala il proprietario di un file di proprietà di un utente non mappato, anche se l'accesso anonimo è disabilitato.                                                                                      |
| -o root [=\<host > [:<Host>]...] |                                                                         Fornisce l'accesso radice alla directory condivisa dagli host o dai gruppi di client specificati dall' *host*. Separare i nomi host e gruppo con i due punti ( **:** ). Se l' *host* non è specificato, tutti i client hanno accesso alla radice. Se l'opzione **radice** non è impostata, nessun client dispone di accesso radice alla directory condivisa.                                                                         |
|            /delete            |                                                                                                                                                       Se viene specificato *ShareName* o <em>Drive</em> **:** <em>path</em> , Elimina la condivisione specificata. Se \* è specificato, Elimina tutte le condivisioni NFS.                                                                                                                                                       |

> [!NOTE]
> Per visualizzare la sintassi completa del comando, al prompt dei comandi digitare:</br>> **nfsshare/?**

## <a name="see-also"></a>Vedere anche

[Informazioni di riferimento sui comandi di Servizi per NFS](services-for-network-file-system-command-reference.md)