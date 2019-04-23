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
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59869962"
---
# <a name="add-top-level-categories-to-the-launchpad-macintosh-operating-system"></a>Aggiunta di categorie principali alla finestra di avvio (sistema operativo Macintosh)

>Si applica a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

È possibile aggiungere delle categorie principali alla finestra di avvio su un computer sul quale è in esecuzione il sistema operativo Macintosh. Per creare un componente aggiuntivo finestra di avvio che aggiunge categorie di livello superiore, è possibile usare una combinazione di informazioni da questa pagina e dell'argomento su come: Aggiungere attività e categorie alla finestra di avvio? nel [Windows Server Solutions SDK](https://go.microsoft.com/fwlink/?LinkID=248648).  
  
 L'esempio seguente mostra come indicare una voce della finestra di avvio come categoria principale nel file .launchpad.  
  
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
  
 Perché la voce sia una categoria principale, l'attributo Id dell'elemento categoria deve essere "Microsoft.Launchpad.HomeCategory".  
  
## <a name="see-also"></a>Vedere anche  
 [Creazione e personalizzazione dell'immagine](Creating-and-Customizing-the-Image.md)   
 [Personalizzazioni aggiuntive](Additional-Customizations.md)   
 [Preparazione dell'immagine per la distribuzione](Preparing-the-Image-for-Deployment.md)   
 [Testare l'esperienza dei clienti](Testing-the-Customer-Experience.md)