---
title: Installare Add-Ins
description: Viene descritto come utilizzare Windows Server Essentials
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: e62e4f07-c2ba-4c5e-b30c-bdc287cd654e
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: d00cb6886e812ee2b780ad79e1fba44442e279ad
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/12/2017
---
# <a name="install-add-ins"></a>Installare Add-Ins

>Si applica a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

È possibile includere componenti aggiuntivi su tutti i server o computer client installandoli prima di creare un'immagine. Questi componenti aggiuntivi quindi verranno inclusi automaticamente in tutti i computer replicati utilizzando tale immagine. È possibile installare un componente aggiuntivo eseguendo il file. wssx oppure è possibile installare i singoli file componente aggiuntivo seguendo le indicazioni fornite nel [documentazione del SDK](https://go.microsoft.com/fwlink/?LinkID=248648)per ogni tipo di componente aggiuntivo. Se si installa utilizzando un file. wssx, il componente aggiuntivo può essere disinstallato tramite Gestione componenti aggiuntivi. Se si installano i singoli file, il componente aggiuntivo non è gestito da Gestione componenti aggiuntivi.  
  
> [!NOTE]
>  Qualsiasi software installato o scaricato sul server (inclusi schede del Dashboard e le notifiche di integrità) deve avere un'interfaccia localizzata che corrisponde alla lingua del sistema operativo installato nel server.  
  
#### <a name="to-install-an-add-in"></a>Per installare un componente aggiuntivo  
  
1.  (Facoltativo) Se si installa il componente aggiuntivo usando un file. wssx, completare i passaggi seguenti:  
  
    1.  Salvare il file. wssx < AddinName\ > nel computer di riferimento.  
  
    2.  Fare doppio clic sul file. wssx per aprire la procedura guidata installazione componente aggiuntivo.  
  
    3.  Seguire le istruzioni della procedura guidata per completare l'installazione.  
  
2.  (Facoltativo) Installare i singoli file nelle posizioni appropriate, come definito nel SDK di ogni tipo di componente aggiuntivo.  
  
## <a name="see-also"></a>Vedere anche  
 [Creazione e personalizzazione dell'immagine](Creating-and-Customizing-the-Image.md)   
 [Ulteriori personalizzazioni](Additional-Customizations.md)   
 [Preparazione dell'immagine per la distribuzione](Preparing-the-Image-for-Deployment.md)   
 [Test di analisi utilizzo software](Testing-the-Customer-Experience.md)