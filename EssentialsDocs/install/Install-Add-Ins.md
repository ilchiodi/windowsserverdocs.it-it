---
title: Installare i componenti aggiuntivi
description: Viene descritto come usare Windows Server Essentials
ms.date: 10/03/2016
ms.prod: windows-server
ms.topic: article
ms.assetid: e62e4f07-c2ba-4c5e-b30c-bdc287cd654e
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: f8428a0198459e1dc036647a3d47fa1a49c00c01
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80820134"
---
# <a name="install-add-ins"></a>Installare i componenti aggiuntivi

>Si applica a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

È possibile includere componenti aggiuntivi su tutti i computer server o client installandoli prima di creare un'immagine. Questi componenti verranno quindi inclusi automaticamente su tutti i computer replicati utilizzando tale immagine. È possibile installare un componente aggiuntivo eseguendo il file WSSX oppure si possono installare i singoli del componente aggiuntivo seguendo le istruzioni riportate nella [documentazione di SDK](https://go.microsoft.com/fwlink/?LinkID=248648)per ciascun tipo di componente aggiuntivo. Se l'installazione viene effettuata tramite il file wssx, il componente aggiuntivo può essere disinstallato utilizzando Gestione componenti aggiuntivi. Se l'installazione viene effettuata tramite file singoli, il componente aggiuntivo non potrà essere gestito da Gestione componenti aggiuntivi.  
  
> [!NOTE]
>  È necessario che l'interfaccia localizzata di tutti i prodotti software installati o scaricati sul server (incluse le schede del dashboard e le notifiche di integrità) corrisponda alla lingua del sistema operativo installato sul server.  
  
#### <a name="to-install-an-add-in"></a>Per installare un componente aggiuntivo  
  
1.  (Facoltativo) Se il componente aggiuntivo viene installato utilizzando un file .wssx file, effettuare le operazioni seguenti:  
  
    1.  Salvare il file < AddInName\>. wssx nel computer di riferimento.  
  
    2.  Fare doppio clic sul file .wssx per aprire la procedura guidata di installazione del componente aggiuntivo.  
  
    3.  Seguire le istruzioni della procedura guidata per completare l'installazione.  
  
2.  (Facoltativo) Installare i singoli file del componente aggiuntivo nelle posizioni appropriate, come specificato nell'SDK di ogni tipo di componente.  
  
## <a name="see-also"></a>Vedi anche  
 [Creazione e personalizzazione dell'immagine](Creating-and-Customizing-the-Image.md)   
 [Personalizzazioni aggiuntive](Additional-Customizations.md)   
 [Preparazione dell'immagine per la distribuzione](Preparing-the-Image-for-Deployment.md)   
 [Test di Analisi utilizzo software](Testing-the-Customer-Experience.md)