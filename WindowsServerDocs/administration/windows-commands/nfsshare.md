---
title: nfsshare
description: Argomento di riferimento per il comando nfsshare, che controlla le condivisioni NFS (Network File System).
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 437a2615-335a-442f-9713-d50d5f3983a3
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 4774d5ce929de5e79e2cde78e45b0cd9bdca163c
ms.sourcegitcommit: 99d548141428c964facf666c10b6709d80fbb215
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 06/12/2020
ms.locfileid: "84721524"
---
# <a name="nfsshare"></a>nfsshare

Controlla le condivisioni NFS (Network File System). Usato senza parametri, questo comando Visualizza tutte le condivisioni NFS (Network File System) esportate da server per NFS.

## <a name="syntax"></a>Sintassi

```
nfsshare <sharename>=<drive:path> [-o <option=value>...]
nfsshare {<sharename> | <drive>:<path> | * } /delete
```

### <a name="parameters"></a>Parametri

| Parametro | Descrizione |
| --------- | ----------- |
| -o anon =`{yes|no}` | Specifica se gli utenti anonimi (non mappati) possono accedere alla directory di condivisione. |
| -o RW =`[<host>[:<host>]...]` | Fornisce accesso in lettura/scrittura alla directory condivisa dagli host o dai gruppi di client specificati dall' *host*. È necessario separare i nomi host e gruppo con i due punti (**:**). Se l' *host* non è specificato, tutti gli host e i gruppi di client (ad eccezione di quelli specificati con l'opzione **ro** ) ottengono l'accesso in lettura/scrittura. Se non è impostata né l'opzione **ro** né l'opzione **RW** , tutti i client avranno accesso in lettura/scrittura alla directory condivisa. |
| -o ro =`[<host>[:<host>]...]` | Fornisce accesso in sola lettura alla directory condivisa dagli host o dai gruppi di client specificati dall' *host*. È necessario separare i nomi host e gruppo con i due punti (**:**). Se l' *host* non è specificato, tutti i client (eccetto quelli specificati con l'opzione **RW** ) ottengono l'accesso in sola lettura. Se l'opzione **ro** è impostata per uno o più client, ma l'opzione **RW** non è impostata, solo i client specificati con l'opzione **ro** hanno accesso alla directory condivisa. |
| -o codifica =`{euc-jp|euc-tw|euc-kr|shift-jis|Big5|Ksc5601|Gb2312-80|Ansi)` | Specifica la codifica della lingua da configurare in una condivisione NFS. È possibile utilizzare una sola lingua nella condivisione. Questo valore può includere uno dei valori seguenti:<ul><li>**EUC-JP:** Giapponese</li><li>**EUC-TW:** Cinese</li><li>**EUC-KR:** Coreano</li><li>**MAIUSC-JIS:** Giapponese</li><li>**Big5:** Cinese</li><li>**Ksc5601:** Coreano</li><li>**GB2312-80:** Cinese semplificato</li><li>**ANSI:** Con codifica ANSI</li></ul> |
| -o anongid =`<gid>` | Specifica che gli utenti anonimi (non mappati) accedono alla directory Share usando *GID* come identificatore del gruppo (GID). Il valore predefinito è **-2**. Il GID anonimo viene usato quando si segnala il proprietario di un file di proprietà di un utente non mappato, anche se l'accesso anonimo è disabilitato. |
| -o anonuid =`<uid>` | Specifica che gli utenti anonimi (non mappati) accedono alla directory Share usando *UID* come identificatore utente (UID). Il valore predefinito è **-2**. L'UID anonimo viene usato quando si segnala il proprietario di un file di proprietà di un utente non mappato, anche se l'accesso anonimo è disabilitato. |
| -o root =`[<host>[:<host>]...]` | Fornisce l'accesso radice alla directory condivisa dagli host o dai gruppi di client specificati dall' *host*. È necessario separare i nomi host e gruppo con i due punti (**:**). Se l' *host* non è specificato, tutti i client ottengono l'accesso alla radice. Se l'opzione **radice** non è impostata, nessun client dispone di accesso radice alla directory condivisa. |
| /delete | Se viene specificato *ShareName* o `<drive>:<path>` , questo parametro Elimina la condivisione specificata. Se viene specificato un carattere jolly (*), questo parametro Elimina tutte le condivisioni NFS. |
| /? | Visualizza la guida al prompt dei comandi. |

#### <a name="remarks"></a>Commenti

- Se *ShareName* è l'unico parametro, questo comando elenca le proprietà della condivisione NFS identificata da *ShareName*.

- Se si usa *ShareName* e `<drive>:<path>` , questo comando Esporta la cartella identificata da `<drive>:<path>` come *ShareName*. Se si utilizza l'opzione **/Delete** , la cartella specificata verrà arrestata per i client NFS.

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)

- [Informazioni di riferimento sui comandi di Servizi per NFS](services-for-network-file-system-command-reference.md)

- [Riferimenti per i cmdlet NFS](https://docs.microsoft.com/powershell/module/nfs)
