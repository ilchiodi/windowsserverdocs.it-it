---
title: cd
description: Argomento di riferimento per il comando CD, che consente di visualizzare il nome di o di modificare la directory corrente.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 932d9cc1-3dff-40da-835c-1cb0894874f1
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 7c9ee57590cf165ba46f394cab06817c7c13f0a9
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/01/2020
ms.locfileid: "82719652"
---
# <a name="cd"></a>cd

> Si applica a: Windows Server (canale semestrale), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Consente di visualizzare il nome della directory corrente o di modificare la directory corrente. Se utilizzata solo con una lettera di unità (ad esempio `cd C:`,), **CD** Visualizza i nomi della directory corrente nell'unità specificata. Se utilizzata senza parametri, **CD** Visualizza l'unità e la directory correnti.

> [!NOTE]
> Questo comando corrisponde al [comando chdir](chdir.md).

## <a name="syntax"></a>Sintassi

```
cd [/d] [<drive>:][<path>]
cd [..]
chdir [/d] [<drive>:][<path>]
chdir [..]
```

### <a name="parameters"></a>Parametri

| Parametro | Descrizione |
| --------- | ----------- |
| /d | Cambia l'unità corrente e la directory corrente per un'unità. |
| `<drive>:` | Specifica l'unità da visualizzare o modificare (se diversa dall'unità corrente). |
| `<path>` | Specifica il percorso della directory che si desidera visualizzare o modificare. |
| [..] | Specifica che si desidera passare alla cartella padre. |
| /? | Visualizza la guida al prompt dei comandi. |

## <a name="remarks"></a>Osservazioni

Se sono abilitate le estensioni dei comandi, al comando **CD** si applicano le condizioni seguenti:

- La stringa di directory corrente viene convertita in modo da usare la stessa combinazione di maiuscole e minuscole dei nomi sul disco. Ad esempio, `cd c:\temp` imposta la directory corrente su C:\Temp se questo è il caso sul disco.

- Gli spazi non vengono considerati delimitatori, `<path>` quindi possono contenere spazi senza racchiudere tra virgolette. Ad esempio:

  ```
  cd username\programs\start menu
  ```

  è lo stesso di:  
  
  ```
  cd "username\programs\start menu"
  ```

  Se le estensioni sono disabilitate, le virgolette sono obbligatorie.

- Per disabilitare le estensioni del comando, digitare:

  ```
  cmd /e:off
  ```

## <a name="examples"></a>Esempi

Per tornare alla directory radice, la parte superiore della gerarchia di directory per un'unità:

```
cd\
```

Per modificare la directory predefinita in un'unità diversa da quella in cui si è connessi:

```
cd [<drive>:[<directory>]]
```

Per verificare la modifica alla directory, digitare:

```
cd [<drive>:]
```

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)

- [comando chdir](chdir.md)
