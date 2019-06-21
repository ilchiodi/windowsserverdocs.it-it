---
title: PASSAGGIO 8 configurare computer INET1
description: 'Questo argomento fa parte della Guida al Lab di Test: dimostrare una distribuzione multisito DirectAccess per Windows Server 2016'
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-da
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 693acb5c-dffc-4484-8286-163bb67724c9
ms.author: coreyp
author: coreyp-at-msft
ms.openlocfilehash: d6967b975b3a950c90de465872832d623755a494
ms.sourcegitcommit: afb0602767de64a76aaf9ce6a60d2f0e78efb78b
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 06/20/2019
ms.locfileid: "67281411"
---
# <a name="step-8-configure-inet1"></a>PASSAGGIO 8: Configurare i computer INET1

>Si applica a: Windows Server (canale semestrale), Windows Server 2016

Per abilitare i computer client per connettersi al server di accesso remoto via Internet, è necessario configurare una voce DNS per EDGE1 2 sul computer INET1.  
  
### <a name="to-create-the-2-edge1-dns-entry"></a>Per creare la voce DNS EDGE1 2  
  
1.  Nel **avviare** digitare**dnsmgmt. msc**, quindi premere INVIO.  
  
2.  Nell'albero della console, aprire **zone di ricerca**, fare clic su **contoso.com**, quindi fare doppio clic su **contoso.com**, quindi fare clic su **Nuovo Host (A o AAAA)** .  
  
3.  In **nome**, tipo **2 EDGE1**. In **indirizzo IP**, tipo **131.107.0.20**. Fare clic su **Aggiungi host**, fare clic su **OK** e quindi fare clic su **Chiudi**.  
  


