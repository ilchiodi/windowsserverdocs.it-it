---
title: shift
description: 'Argomento dei comandi di Windows per * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: b56574e8-570a-4cc9-bbac-1b94fbf6a47a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: f74e0f1f9041a4a7b95d83772ea79376c82876de
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71371255"
---
# <a name="shift"></a>shift



Modifica la posizione dei parametri di batch in un file batch.

Per esempi di utilizzo di questo comando, vedere [Esempi](#BKMK_examples).

## <a name="syntax"></a>Sintassi

```
shift [/n <N>]
```

## <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|/n \<N >|Specifica di iniziare a spostare in corrispondenza dell'argomento *n*, dove *n* è qualsiasi valore compreso tra 0 e 8. Sono necessarie le estensioni dei comandi, abilitate per impostazione predefinita.|
|/?|Visualizza la guida al prompt dei comandi.|

## <a name="remarks"></a>Osservazioni

- Il **comando shift** consente di modificare i valori dei parametri batch da **%0** a **%9** copiando ogni parametro in quello precedente. il valore di **%1** viene copiato in **%0**, il valore di **%2** viene copiato in **%1**e così via. Questa operazione è utile per la scrittura di un file batch che esegue la stessa operazione su un numero qualsiasi di parametri.
- Se sono abilitate le estensioni dei comandi, il comando **Shift** supporta l'opzione della riga di comando **/n** . L'opzione **/n** specifica di iniziare lo spostamento in corrispondenza dell'ennesimo argomento, dove **n** è qualsiasi valore compreso tra 0 e 8. Ad esempio, **Shift/2** sposterebbe **%3** in **%2**, **%4** in **%3**e così via e lascerebbe **%0** e **%1** non interessato. Le estensioni dei comandi sono abilitate per impostazione predefinita.
- È possibile utilizzare il comando **Shift** per creare un file batch in grado di accettare più di 10 parametri batch. Se nella riga di comando si specificano più di 10 parametri, quelli visualizzati dopo il decimo ( **%9**) verranno spostati uno alla volta in **%9**.
- Il comando **Shift** non ha alcun effetto sul parametro batch **%\\** *.
- Nessun comando di **spostamento** all'indietro. Dopo aver implementato il comando **Shift** , non è possibile recuperare il parametro batch ( **%0**) esistente prima del turno.

## <a name="BKMK_examples"></a>Esempi

Le righe seguenti di un file batch di esempio denominato copy. bat illustrano come usare **Shift** con un numero qualsiasi di parametri batch. In questo esempio, copia. bat copia un elenco di file in una directory specifica. I parametri batch sono rappresentati dagli argomenti directory e nome file.
```
@echo off 
rem MYCOPY.BAT copies any number of files
rem to a directory.
rem The command uses the following syntax:
rem mycopy dir file1 file2 ... 
set todir=%1
:getfile
shift
if "%1"=="" goto end
copy %1 %todir%
goto getfile
:end
set todir=
echo All done
```

#### <a name="additional-references"></a>Altri riferimenti

[Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)