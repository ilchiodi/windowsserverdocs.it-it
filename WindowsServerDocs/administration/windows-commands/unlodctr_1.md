---
title: unlodctr
description: 'Argomento i comandi di Windows per * * *- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: e1a662da10acc65b4ad2fd0d055cf9d46de603be
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59886652"
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
|\<DriverName >|Rimuove le prestazioni del contatore delle impostazioni del nome e il testo esplicativo per servizio o driver <DriverName> dal Registro di sistema Windows Server 2003.|  
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
-   [Chiave sintassi della riga di comando](command-line-syntax-key.md)  
  
