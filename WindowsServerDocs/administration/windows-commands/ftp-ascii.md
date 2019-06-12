---
title: ftp ascii
description: 'Argomento i comandi di Windows per ftp ascii '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 523be48e-eab0-4237-8fb5-ca222824f0b6 vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 8e38c57b7a5ffd9afe677c4b49787383412621fe
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/31/2019
ms.locfileid: "66438806"
---
# <a name="ftp-ascii"></a>ftp: ascii

>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Imposta il tipo di trasferimento di file in formato ASCII.   
## <a name="syntax"></a>Sintassi  
```  
ascii  
```  
### <a name="parameters"></a>Parametri  
nessuno  
## <a name="remarks"></a>Note  
- Il tipo di trasferimento di file predefinito è ASCII.  
- In QUESTA modalità, vengono eseguite le conversioni di caratteri da e verso il set di caratteri standard di rete. Ad esempio, caratteri di fine della riga vengono convertiti in base alle esigenze, in base al sistema operativo di destinazione.  
- **FTP** supporta ASCII e tipi di trasferimento di file di immagine binari. Utilizzare ASCII durante il trasferimento dei file di testo. Per altre informazioni sul trasferimento di file binari, vedere **ftp: binario** in riferimenti aggiuntivi.  
  ## <a name="BKMK_Examples"></a>Esempi  
  Impostare il tipo di trasferimento di file ASCII.  
  ```  
  ascii  
  ```  
  ## <a name="additional-references"></a>Riferimenti aggiuntivi  
- [ftp: binary](ftp-binary.md)  
- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)  
