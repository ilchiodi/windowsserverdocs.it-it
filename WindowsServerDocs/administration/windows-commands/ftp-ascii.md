---
title: ASCII FTP
description: 'Argomento dei comandi di Windows per FTP ASCII '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: d5ae0064f9c1679bb8b386271f042d589b158c73
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71376625"
---
# <a name="ftp-ascii"></a>FTP: ASCII

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
- **FTP** supporta i tipi di trasferimento di file di immagine binari e ASCII. Utilizzare ASCII durante il trasferimento dei file di testo. Per ulteriori informazioni sul trasferimento di file binari, vedere **FTP: Binary** in riferimenti aggiuntivi.  
  ## <a name="BKMK_Examples"></a>Esempi  
  Impostare il tipo di trasferimento di file ASCII.  
  ```  
  ascii  
  ```  
  ## <a name="additional-references"></a>Riferimenti aggiuntivi  
- [FTP: binario](ftp-binary.md)  
- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)  
