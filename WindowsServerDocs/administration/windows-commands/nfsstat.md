---
title: nfsstat
description: Argomento di riferimento per il comando nfsstat, che visualizza informazioni statistiche sulle chiamate di NFS (Network File System) e RPC (Remote Procedure Call).
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: da7a9768-44bd-404b-97ee-c388d00dc395
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 85eb1184d3eb8ee731cf698a6d805e3f11d878ce
ms.sourcegitcommit: 99d548141428c964facf666c10b6709d80fbb215
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 06/12/2020
ms.locfileid: "84721514"
---
# <a name="nfsstat"></a>nfsstat

Utilità da riga di comando che consente di visualizzare informazioni statistiche sulle chiamate NFS (Network File System) e RPC (Remote Procedure Call). Usato senza parametri, questo comando Visualizza tutti i dati statistici senza reimpostare alcun elemento.

## <a name="syntax"></a>Sintassi

```
nfsstat [-c][-s][-n][-r][-z][-m]
```

### <a name="parameters"></a>Parametri

| Parametro | Descrizione |
| --------- | ----------- |
| -c | Visualizza solo le chiamate NFS e RPC e NFS sul lato client inviate e rifiutate dal client. Per visualizzare solo le informazioni NFS o RPC, combinare questo flag con il parametro **-n** o **-r** . |
| -S | Visualizza solo le chiamate NFS sul lato server e RPC e NFS inviate e rifiutate dal server. Per visualizzare solo le informazioni NFS o RPC, combinare questo flag con il parametro **-n** o **-r** . |
| -M | Visualizza informazioni sui flag di montaggio impostati dalle opzioni di montaggio, i flag di montaggio interni al sistema e altre informazioni di montaggio. |
| -n | Visualizza le informazioni NFS sia per il client che per il server. Per visualizzare solo le informazioni sul client o sul server NFS, combinare questo flag con il parametro **-c** o **-s** . |
| -r | Visualizza le informazioni RPC sia per il client che per il server. Per visualizzare solo le informazioni sul client o sul server RPC, combinare questo flag con il parametro **-c** o **-s** . |
| -Z | Reimposta le statistiche della chiamata. Questo flag è disponibile solo per l'utente root e può essere combinato con qualsiasi altro parametro per reimpostare determinati set di statistiche dopo la visualizzazione. |

### <a name="examples"></a>Esempio

Per visualizzare informazioni sul numero di chiamate RPC e NFS inviate e rifiutate dal client, digitare:

```
nfsstat -c
```

Per visualizzare e stampare le informazioni relative alla chiamata NFS del client, digitare:

```
nfsstat -cn
```

Per visualizzare le informazioni relative alla chiamata RPC per il client e il server, digitare:

```
nfsstat -r
```

Per visualizzare informazioni sul numero di chiamate RPC e NFS ricevute e rifiutate dal server, digitare:

```
nfsstat -s
```

Per reimpostare tutte le informazioni relative alla chiamata su zero nel client e nel server, digitare:

```
nfsstat -z
```

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)

- [Informazioni di riferimento sui comandi di Servizi per NFS](services-for-network-file-system-command-reference.md)

- [Riferimenti per i cmdlet NFS](https://docs.microsoft.com/powershell/module/nfs)
