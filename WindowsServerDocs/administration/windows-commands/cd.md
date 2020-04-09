---
title: cd
description: Windows Commands Topic for CD, che Visualizza il nome o modifica la directory corrente.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 932d9cc1-3dff-40da-835c-1cb0894874f1
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 2d62f529ab6c45957f0fdea24358a2f13151adb6
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80848224"
---
# <a name="cd"></a>cd

Consente di visualizzare il nome di o di modificare la directory corrente. Se utilizzata solo con una lettera di unità (ad esempio, `cd C:`), **CD** Visualizza i nomi della directory corrente nell'unità specificata. Se utilizzata senza parametri, **CD** Visualizza l'unità e la directory correnti.

> [!NOTE]
> Questo comando corrisponde al comando **chdir** .

Per esempi di utilizzo di questo comando, vedere [Esempi](#BKMK_examples).

## <a name="syntax"></a>Sintassi

```
cd [/d] [<Drive>:][<Path>]
cd [..]
chdir [/d] [<Drive>:][<Path>]
chdir [..]
```

### <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|/d|Cambia l'unità corrente e la directory corrente per un'unità.|
|> unità \<:|Specifica l'unità da visualizzare o modificare (se diversa dall'unità corrente).|
|Percorso \<>|Specifica il percorso della directory che si desidera visualizzare o modificare.|
|[..]|Specifica che si desidera passare alla cartella padre.|
|/?|Visualizza la guida al prompt dei comandi.|

## <a name="remarks"></a>Note

Se sono abilitate le estensioni dei comandi, al comando **CD** si applicano le condizioni seguenti:
- La stringa di directory corrente viene convertita in modo da usare la stessa combinazione di maiuscole e minuscole dei nomi sul disco. Ad esempio, `cd C:\TEMP` imposta la directory corrente su C:\Temp se questo è il caso sul disco.
- Gli spazi non vengono considerati delimitatori, quindi il *percorso* può contenere spazi senza virgolette di inclusione. Ad esempio,  
  ```
  cd username\programs\start menu
  ```  
  è uguale a:  
  ```
  cd username\programs\start menu
  ```  
  Tuttavia, le virgolette sono obbligatorie se le estensioni sono disabilitate.

Per disabilitare le estensioni del comando, digitare:
```
cmd /e:off
```

## <a name="examples"></a><a name=BKMK_examples></a>Esempi

La directory radice è la parte superiore della gerarchia di directory per un'unità. Per tornare alla directory radice, digitare:
```
cd\
```
Per modificare la directory predefinita in un'unità diversa da quella in cui si è connessi, digitare:
```
cd [<Drive>:\[<Directory>]]
```
Per verificare la modifica alla directory, digitare:
```
cd [<Drive>:]
```

## <a name="additional-references"></a>Altre informazioni di riferimento

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)