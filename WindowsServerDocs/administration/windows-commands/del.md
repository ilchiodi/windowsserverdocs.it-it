---
title: del
description: Argomento di riferimento per il comando CANC, che consente di eliminare uno o più file.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 346eede2-2085-44f5-9936-6877b5d5a833
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: f32efd6e29a715cdc67b5a1ddcb166922d1cfcc9
ms.sourcegitcommit: fad2ba64bbc13763772e21ed3eabd010f6a5da34
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/09/2020
ms.locfileid: "82993132"
---
# <a name="del"></a>del

Elimina uno o più file. Questo comando esegue le stesse azioni del comando di **cancellazione** .

Il comando **Canc** può essere eseguito anche dalla Console ripristino Windows utilizzando parametri diversi. Per ulteriori informazioni, vedere [ambiente ripristino Windows (WinRE)](https://docs.microsoft.com/windows-hardware/manufacture/desktop/windows-recovery-environment--windows-re--technical-reference).

> [!WARNING]
> Se si usa **Canc** per eliminare un file dal disco, non è possibile recuperarlo.

## <a name="syntax"></a>Sintassi

```
del [/p] [/f] [/s] [/q] [/a[:]<attributes>] <names>
erase [/p] [/f] [/s] [/q] [/a[:]<attributes>] <names>
```

### <a name="parameters"></a>Parametri

| Parametro | Descrizione |
| --------- | ----------- |
| `<names>` | Specifica un elenco di uno o più file o directory. Per eliminare più file, è possibile utilizzare caratteri jolly. Se viene specificata una directory, verranno eliminati tutti i file all'interno della directory. |
| / p | Richiede una conferma prima di eliminare il file specificato. |
| /f | Eliminazione di forza dei file di sola lettura. |
| /s | Elimina i file dalla directory corrente e tutte le sottodirectory specificati. Visualizza i nomi dei file di come vengono eliminati. |
| /q | Specifica la modalità non interattiva. Non viene chiesto di confermare l'eliminazione. |
| /a [:]`<attributes>` | Elimina i file in base ai seguenti attributi di file:<ul><li>**r** i file di sola lettura</li><li>**h** file nascosti</li><li>**i** non contenuti i file indicizzati</li><li>**s** i file di sistema</li><li>**un** pronto per l'archiviazione dei file</li><li>**l** Reparse Point</li><li>**-** Usato come prefisso che significa ' not '</li></ul>. |
| /? | Visualizza la guida al prompt dei comandi. |

#### <a name="remarks"></a>Osservazioni

- Se si usa il `del /p` comando, verrà visualizzato il messaggio seguente:

    `FileName, Delete (Y/N)?`

    Per confermare l'eliminazione, premere **Y**. Per annullare l'eliminazione e visualizzare il nome del file successivo (se è stato specificato un gruppo di file), premere **N**. Per arrestare il comando **Canc** , premere CTRL + C.

- Se si disabilita l'estensione del comando, nel parametro **/s** verranno visualizzati i nomi di tutti i file che non sono stati trovati, anziché visualizzare i nomi dei file da eliminare.

- Se si specificano cartelle specifiche nel `<names>` parametro, verranno eliminati anche tutti i file inclusi. Ad esempio, se si desidera eliminare tutti i file nella cartella *\Work* , digitare:

  ```
  del \work
  ```

- È possibile utilizzare caratteri jolly (**&#42;** e **?**) per eliminare più di un file alla volta. Tuttavia, per evitare di eliminare i file in modo involontario, è consigliabile usare i caratteri jolly con cautela. Ad esempio, se si digita il comando seguente:

  ```
  del *.*
  ```

  Il **Canc** comando Visualizza il messaggio seguente:

  `Are you sure (Y/N)?`

  Per eliminare tutti i file nella directory corrente, premere **Y** e quindi premere INVIO. Per annullare l'eliminazione, premere **N** e quindi premere INVIO.

  > [!NOTE]
  > Prima di utilizzare caratteri jolly con il **CANC** comando, utilizzare gli stessi caratteri jolly con il **dir** comando per elencare tutti i file che verranno eliminati.

## <a name="examples"></a>Esempi

Per eliminare tutti i file in una cartella denominata Test sull'unità C, digitare uno dei seguenti:

```
del c:\test
del c:\test\*.*
```

Per eliminare tutti i file con estensione. bat dalla directory corrente, digitare:

```
del *.bat
```

Per eliminare tutti i file di sola lettura nella directory corrente, digitare:

```
del /a:r *.*
```

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)

- [Ambiente ripristino Windows (WinRE)](https://docs.microsoft.com/windows-hardware/manufacture/desktop/windows-recovery-environment--windows-re--technical-reference)
