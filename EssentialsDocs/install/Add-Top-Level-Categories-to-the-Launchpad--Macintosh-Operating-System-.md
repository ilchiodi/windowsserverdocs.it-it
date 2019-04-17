---
title: Aggiunta di categorie principali alla finestra di avvio (sistema operativo Macintosh)
description: Viene descritto come utilizzare Windows Server Essentials
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: ee2173c3-e464-4001-9f43-6d926a575092
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: ae4eb5943d37b4a9d3b554af28cb425420782cf8
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/12/2017
---
# <a name="add-top-level-categories-to-the-launchpad-macintosh-operating-system"></a>Aggiunta di categorie principali alla finestra di avvio (sistema operativo Macintosh)

>Si applica a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

È possibile aggiungere le categorie principali alla finestra di avvio in un computer che eseguono il sistema operativo Macintosh. Per creare un componente aggiuntivo finestra di avvio che aggiunge categorie di livello superiore, è possibile utilizzare una combinazione di informazioni in questa pagina e dell'argomento intitolato procedura: aggiungere attività e categorie alla finestra di avvio? nel [Windows Server Solutions SDK](https://go.microsoft.com/fwlink/?LinkID=248648).  
  
 L'esempio seguente mostra come è possibile specificare la voce di avvio per una categoria principale nel file. LaunchPad:  
  
```  
  
<?xml version="1.0" encoding="utf-8" ?>  
<LaunchPad resFile="Localizable">  
   <Category id="Microsoft.Launchpad.HomeCategory" name="SampleAddin"  image="SampleImage01.png">  
      <Task id="Microsoft.Launchpad.SampleAddin.Pictures"   
          name="Pictures"       
           src="smb://%ServerAddress%/Pictures"   
         image="SampleImage02.png"/>  
   </Category>  
</LaunchPad>  
```  
  
 Per la voce sia una categoria principale, l'attributo Id dell'elemento categoria deve essere "Homecategory".  
  
## <a name="see-also"></a>Vedere anche  
 [Creazione e personalizzazione dell'immagine](Creating-and-Customizing-the-Image.md)   
 [Ulteriori personalizzazioni](Additional-Customizations.md)   
 [Preparazione dell'immagine per la distribuzione](Preparing-the-Image-for-Deployment.md)   
 [Test di analisi utilizzo software](Testing-the-Customer-Experience.md)