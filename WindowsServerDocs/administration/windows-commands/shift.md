---
title: shift
description: 'Argomento i comandi di Windows per * * *- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 52b3012a39409f9d48ae8aa7608e7bd0af5787d8
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59858822"
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
|/n \<N>|Specifica per iniziare lo spostamento di *N*esimo argomento, dove *N* è qualsiasi valore compreso tra 0 e 8. Richiede le estensioni dei comandi, sono abilitati per impostazione predefinita.|
|/?|Visualizza la guida al prompt dei comandi.|

## <a name="remarks"></a>Note

-   Il **shift** comando Modifica i valori dei parametri di batch **%0** tramite **%9** mediante la copia di ogni parametro in quella precedente, ovvero il valore di **%1** viene copiato **%0**, il valore di **%2** viene copiato **%1**e così via. Ciò è utile per la scrittura di un file batch che esegue la stessa operazione su un qualsiasi numero di parametri.
-   Se sono abilitate le estensioni dei comandi, il **shift** comando supporta la **/n** opzione della riga di comando. Il **/n** opzione specifica per iniziare lo spostamento l'ennesimo argomento, dove **N** è qualsiasi valore compreso tra 0 e 8. Ad esempio, **MAIUSC /2** comporterebbe un cambiamento **%3** al **%2**, **%4** a **%3**e così via e lasciare **%0** e **%1** interessate. Le estensioni comando sono abilitate per impostazione predefinita.
-   È possibile usare la **MAIUSC** comando per creare un file batch che può accettare parametri batch più di 10. Se si specificano più di 10 parametri nella riga di comando, quelli che vengono visualizzati dopo il decimo (**%9**) saranno spostate uno alla volta nel **%9**.
-   Il **shift** comando non ha alcun effetto **% \*** batch parametro.
-   Non è non con le versioni precedenti **MAIUSC** comando. Dopo aver implementato il **shift** comando, non sarà possibile recuperare il parametro batch (**%0**) che esisteva prima il turno.

## <a name="BKMK_examples"></a>Esempi

Le righe seguenti da un file batch di esempio denominato Miacopia illustrano come usare **MAIUSC** con qualsiasi numero di parametri di batch. In questo esempio Miacopia copia un elenco di file in una directory specifica. I parametri batch sono rappresentati dagli argomenti nome di file e directory.
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

[Chiave sintassi della riga di comando](command-line-syntax-key.md)