---
title: unlodctr
description: 'Argomento dei comandi di Windows per * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: fc8aa6f0-c1d9-47ea-937a-28152148e774
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 85a66b521f404358705962078f33af4bec1ebae5
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71363897"
---
# <a name="unlodctr"></a>unlodctr

>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Rimuove i nomi dei contatori di prestazioni e la descrizione per un servizio o driver di periferica dal Registro di sistema.   

## <a name="syntax"></a>Sintassi  
```  
Unlodctr <DriverName>   
```  
### <a name="parameters"></a>Parametri  
|Parametro|Descrizione|  
|-------|--------|  
|\<DriverName >|rimuove le impostazioni del nome del contatore delle prestazioni e il testo esplicativo per il driver o il servizio <DriverName> dal registro di sistema di Windows Server 2003.|  
|/?|Visualizza la guida al prompt dei comandi.|  

## <a name="remarks"></a>Note  
> [!WARNING]  
> La modifica non corretta del Registro di sistema potrebbe danneggiare gravemente il sistema. Prima di apportare modifiche al Registro di sistema, si consiglia di effettuare il backup di tutti i dati importanti presenti sul computer.  

Se le informazioni fornite contengono spazi, utilizzare il testo tra virgolette (ad esempio, "<DriverName>").  

## <a name="BKMK_Examples"></a>Esempi  
Per rimuovere le impostazioni del Registro di sistema corrente e il testo esplicativo per il servizio Simple Mail Transfer Protocol (SMTP) del contatore:  
```  
unlodctr SMTPSVC  
```  
## <a name="additional-references"></a>Altri riferimenti  
-   [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)  
  
