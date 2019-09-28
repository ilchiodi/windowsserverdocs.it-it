---
title: PASSAGGIO 8 configurare computer INET1
description: 'Questo argomento fa parte della Guida al Lab di test: illustra una distribuzione multisito di DirectAccess per Windows Server 2016'
manager: brianlic
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-da
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 693acb5c-dffc-4484-8286-163bb67724c9
ms.author: coreyp
author: coreyp-at-msft
ms.openlocfilehash: 54674adc33f45a58f2515d07fed4c8a070ded5a1
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71404706"
---
# <a name="step-8-configure-inet1"></a>PASSAGGIO 8: Configurare computer INET1

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016

Per abilitare i computer client per connettersi al server di accesso remoto via Internet, è necessario configurare una voce DNS per EDGE1 2 sul computer INET1.  
  
### <a name="to-create-the-2-edge1-dns-entry"></a>Per creare la voce DNS EDGE1 2  
  
1.  Nel **avviare** digitare**dnsmgmt. msc**, quindi premere INVIO.  
  
2.  Nell'albero della console, aprire **zone di ricerca**, fare clic su **contoso.com**, quindi fare doppio clic su **contoso.com**, quindi fare clic su **Nuovo Host (A o AAAA)** .  
  
3.  In **nome**, tipo **2 EDGE1**. In **indirizzo IP**, tipo **131.107.0.20**. Fare clic su **Aggiungi host**, fare clic su **OK** e quindi fare clic su **Chiudi**.  
  


