---
title: Installare i componenti aggiuntivi
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
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59833072"
---
# <a name="install-add-ins"></a>Installare i componenti aggiuntivi

>Si applica a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

È possibile includere componenti aggiuntivi su tutti i computer server o client installandoli prima di creare un'immagine. Questi componenti verranno quindi inclusi automaticamente su tutti i computer replicati utilizzando tale immagine. È possibile installare un componente aggiuntivo eseguendo il file WSSX oppure si possono installare i singoli del componente aggiuntivo seguendo le istruzioni riportate nella [documentazione di SDK](https://go.microsoft.com/fwlink/?LinkID=248648)per ciascun tipo di componente aggiuntivo. Se l'installazione viene effettuata tramite il file wssx, il componente aggiuntivo può essere disinstallato utilizzando Gestione componenti aggiuntivi. Se l'installazione viene effettuata tramite file singoli, il componente aggiuntivo non potrà essere gestito da Gestione componenti aggiuntivi.  
  
> [!NOTE]
>  È necessario che l'interfaccia localizzata di tutti i prodotti software installati o scaricati sul server (incluse le schede del dashboard e le notifiche di integrità) corrisponda alla lingua del sistema operativo installato sul server.  
  
#### <a name="to-install-an-add-in"></a>Per installare un componente aggiuntivo  
  
1.  (Facoltativo) Se il componente aggiuntivo viene installato utilizzando un file .wssx file, effettuare le operazioni seguenti:  
  
    1.  Salvare il < Nomecompaggiuntivo\>. wssx nel computer di riferimento.  
  
    2.  Fare doppio clic sul file .wssx per aprire la procedura guidata di installazione del componente aggiuntivo.  
  
    3.  Seguire le istruzioni della procedura guidata per completare l'installazione.  
  
2.  (Facoltativo) Installare i singoli file del componente aggiuntivo nelle posizioni appropriate, come specificato nell'SDK di ogni tipo di componente.  
  
## <a name="see-also"></a>Vedere anche  
 [Creazione e personalizzazione dell'immagine](Creating-and-Customizing-the-Image.md)   
 [Personalizzazioni aggiuntive](Additional-Customizations.md)   
 [Preparazione dell'immagine per la distribuzione](Preparing-the-Image-for-Deployment.md)   
 [Testare l'esperienza dei clienti](Testing-the-Customer-Experience.md)