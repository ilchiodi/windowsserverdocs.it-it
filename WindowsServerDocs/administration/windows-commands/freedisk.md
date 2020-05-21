---
title: freedisk
description: Argomento di riferimento per il comando freedisk, che consente di verificare se la quantità di spazio su disco specificata è disponibile prima di continuare con un processo di installazione.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 91c15166-5baa-4b80-9e0c-4cd815d00530
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 2135bd24e24235de7c687ed58e0603db20c68262
ms.sourcegitcommit: bf887504703337f8ad685d778124f65fe8c3dc13
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/16/2020
ms.locfileid: "83436976"
---
# <a name="freedisk"></a>freedisk

> Si applica a: Windows Server (canale semestrale), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Verifica se la quantità di spazio su disco specificata è disponibile prima di continuare con un processo di installazione.

## <a name="syntax"></a>Sintassi

```
freedisk [/s <computer> [/u [<domain>\]<user> [/p [<password>]]]] [/d <drive>] [<value>]
```

### <a name="parameters"></a>Parametri

| Parametro | Descrizione |
| --------- | ----------- |
| /s`<computer>` | Specifica il nome o l'indirizzo IP di un computer remoto (non utilizzare barre rovesciate). Il valore predefinito è il computer locale. Questo parametro si applica a tutti i file e le cartelle specificati nel comando. |
| /u`[<domain>\]<user>` | Esegue lo script con le autorizzazioni dell'account utente specificato. Il valore predefinito è le autorizzazioni di sistema. |
| /p [<password>] | Specifica la password dell'account utente specificato in **/u**. |
| /d`<drive>` | Specifica l'unità per cui si vuole trovare la disponibilità di spazio libero. È necessario specificare `<drive>` per un computer remoto. |
| `<value>` | Verifica la presenza di una quantità specifica di spazio libero su disco. È possibile specificare `<value>` in byte, KB, MB, GB, TB, PB, EB, es o YB. |

#### <a name="remarks"></a>Osservazioni

- Utilizzare le opzioni della riga di comando **/s**, **/u**e **/p** sono disponibili solo quando si utilizza **/s**. È necessario utilizzare **/p** con **/u**per fornire la password dell'utente.

- Per le installazioni automatiche, è possibile usare **freedisk** nei file batch di installazione per verificare la quantità di spazio libero prerequisito prima di continuare con l'installazione.

- Quando si usa **freedisk** in un file batch, viene restituito **0** se lo spazio è sufficiente e **1** se lo spazio è insufficiente.

### <a name="examples"></a>Esempi

Per determinare se sono disponibili almeno 50 MB di spazio libero nell'unità C:, digitare:

```
freedisk 50mb
```

Sullo schermo viene visualizzato un output simile al seguente:

```
INFO: The specified 52,428,800 byte(s) of free space is available on current drive.
```

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)
