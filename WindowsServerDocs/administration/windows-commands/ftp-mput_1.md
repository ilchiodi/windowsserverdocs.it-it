---
title: mput_1 FTP
description: 'Argomento i comandi di Windows per * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 980f15e7-7cf1-4813-9946-a8cc4edfb198 vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: dd19a97246aa6155182cb055deceb4b5a5019f6c
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/31/2019
ms.locfileid: "66438586"
---
# <a name="ftp-mput1"></a>ftp: mput_1

>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Tipo di trasferimento copia i file locali in computer remoto utilizzando il file corrente.   
## <a name="syntax"></a>Sintassi  
```  
mput <LocalFile>[ ]  
```  
### <a name="parameters"></a>Parametri  

|  Parametro  |                       Descrizione                        |
|-------------|----------------------------------------------------------|
| <LocalFile> | Specifica il file locale da copiare nel computer remoto. |

## <a name="BKMK_Examples"></a>Esempi  
Copia **Program1.exe** e **Program2.exe** al computer remoto usando il tipo di trasferimento di file corrente.  
```  
mput Program1.exe Program2.exe  
```  
## <a name="additional-references"></a>Riferimenti aggiuntivi  
-   [ftp: ascii](ftp-ascii.md)  
-   [ftp: binary](ftp-binary.md)  
-   [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)  
