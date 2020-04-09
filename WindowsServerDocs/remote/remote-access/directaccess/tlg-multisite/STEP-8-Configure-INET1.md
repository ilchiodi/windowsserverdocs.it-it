---
title: PASSAGGIO 8 configurare computer INET1
description: 'Questo argomento fa parte della Guida al Lab di test: illustra una distribuzione multisito di DirectAccess per Windows Server 2016'
manager: brianlic
ms.prod: windows-server
ms.technology: networking-da
ms.topic: article
ms.assetid: 693acb5c-dffc-4484-8286-163bb67724c9
ms.author: coreyp
author: coreyp-at-msft
ms.openlocfilehash: ca8a49e612bef9c4a72c47e8d0e147a900686f84
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80861544"
---
# <a name="step-8-configure-inet1"></a>PASSAGGIO 8: configurare INET1

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016

Per abilitare i computer client per connettersi al server di accesso remoto via Internet, è necessario configurare una voce DNS per EDGE1 2 sul computer INET1.  
  
### <a name="to-create-the-2-edge1-dns-entry"></a>Per creare la voce DNS EDGE1 2  
  
1.  Nel **avviare** digitare**dnsmgmt. msc**, quindi premere INVIO.  
  
2.  Nell'albero della console, aprire **zone di ricerca**, fare clic su **contoso.com**, quindi fare doppio clic su **contoso.com**, quindi fare clic su **Nuovo Host (A o AAAA)** .  
  
3.  In **nome**, tipo **2 EDGE1**. In **indirizzo IP**, tipo **131.107.0.20**. Fare clic su **Aggiungi host**, fare clic su **OK** e quindi fare clic su **Chiudi**.  
  


