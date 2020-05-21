---
title: driverquery
description: Argomento di riferimento per il comando driverquery, che consente a un amministratore di visualizzare un elenco di driver di dispositivo installati e le relative proprietà.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 92ca4b84-e4e2-405b-9f31-bf6db9f66839
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: f4754cba8cf4cb3a5f01b0aeb0095f727a072a5c
ms.sourcegitcommit: bf887504703337f8ad685d778124f65fe8c3dc13
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/16/2020
ms.locfileid: "83436936"
---
# <a name="driverquery"></a>driverquery

Consente agli amministratori di visualizzare un elenco di driver di periferica installati e le relative proprietà. Se utilizzata senza parametri, **driverquery** viene eseguito nel computer locale.

## <a name="syntax"></a>Sintassi

```
driverquery [/s <system> [/u [<domain>\]<username> [/p <password>]]] [/fo {table | list | csv}] [/nh] [/v | /si]
```

### <a name="parameters"></a>Parametri

| Parametro | Descrizione |
| --------- |------------ |
| /s`<system>` | Specifica il nome o indirizzo IP di un computer remoto. Non utilizzare le barre rovesciate. Il valore predefinito è il computer locale. |
| /u`[<domain>]<username>` | Esegue il comando con le credenziali dell'account utente come specificato dall' *utente* o *dominio\utente*. Per impostazione predefinita, */s* utilizza le credenziali dell'utente attualmente connesso al computer in cui viene eseguito il comando. Impossibile utilizzare **/u** a meno che non sia specificato **/s** . |
| /p`<password>` | Specifica la password dell'account utente specificato nella **/u** parametro. **/p** non può essere utilizzato a meno che non **/u** specificato. |
| tabella/fo | Formatta l'output come tabella. Questo è il valore predefinito. |
| elenco/fo | Formatta l'output come elenco. |
| /FO CSV | Formatta l'output con valori delimitati da virgole. |
| /NH | Omette la riga di intestazione dalle informazioni sui driver visualizzate. Se non è valido il **/fo** parametro è impostato su **elenco**. |
| /v | Visualizza output dettagliato. **/v** non è valido per i driver firmati. |
| /si | Vengono fornite informazioni sui driver firmati. |
| /? | Visualizza la guida al prompt dei comandi. |

### <a name="examples"></a>Esempi

Per visualizzare un elenco di driver di periferica installati nel computer locale, digitare:

```
driverquery
```

Per visualizzare l'output in un formato con valori delimitati da virgole (CSV), digitare:

```
driverquery /fo csv
```

Per nascondere la riga di intestazione nell'output, digitare:

```
driverquery /nh
```

Utilizzare il **driverquery** comando in un server remoto denominato *server1* utilizzando le credenziali correnti del computer locale, digitare:

```
driverquery /s server1
```

Utilizzare il **driverquery** comando in un server remoto denominato *server1* utilizzando le credenziali *user1* nel dominio *maindom*, tipo:

```
driverquery /s server1 /u maindom\user1 /p p@ssw3d
```

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)