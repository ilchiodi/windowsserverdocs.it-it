---
title: ASCII FTP
description: Argomento dei comandi di Windows per FTP ASCII
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 523be48e-eab0-4237-8fb5-ca222824f0b6 vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: c4930d0d726acaa222f802d7aaef59578030db5f
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80843784"
---
# <a name="ftp-ascii"></a>FTP: ASCII

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Imposta il tipo di trasferimento di file in formato ASCII.   
## <a name="syntax"></a>Sintassi  
```  
ascii  
```  
#### <a name="parameters"></a>Parametri  
nessuno  
## <a name="remarks"></a>Note  
- Il tipo di trasferimento di file predefinito è ASCII.  
- In QUESTA modalità, vengono eseguite le conversioni di caratteri da e verso il set di caratteri standard di rete. Ad esempio, caratteri di fine della riga vengono convertiti in base alle esigenze, in base al sistema operativo di destinazione.  
- **FTP** supporta i tipi di trasferimento di file di immagine binari e ASCII. Utilizzare ASCII durante il trasferimento dei file di testo. Per ulteriori informazioni sul trasferimento di file binari, vedere **FTP: Binary** in riferimenti aggiuntivi.  
  ## <a name="examples"></a><a name=BKMK_Examples></a>Esempi  
  Impostare il tipo di trasferimento di file ASCII.  
  ```  
  ascii  
  ```  
  ## <a name="additional-references"></a>Altre informazioni di riferimento  
- [FTP: binario](ftp-binary.md)  
- - [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)  
