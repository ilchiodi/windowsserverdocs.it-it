---
title: Aggiunta di categorie principali alla finestra di avvio (sistema operativo Macintosh)
description: Viene descritto come usare Windows Server Essentials
ms.date: 10/03/2016
ms.prod: windows-server
ms.topic: article
ms.assetid: ee2173c3-e464-4001-9f43-6d926a575092
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: c8a15a831a89afc55d20db4e1c1195173d466b3c
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80817514"
---
# <a name="add-top-level-categories-to-the-launchpad-macintosh-operating-system"></a>Aggiunta di categorie principali alla finestra di avvio (sistema operativo Macintosh)

>Si applica a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

È possibile aggiungere delle categorie principali alla finestra di avvio su un computer sul quale è in esecuzione il sistema operativo Macintosh. Per creare un componente aggiuntivo della finestra di avvio che aggiunge categorie di livello superiore, è possibile usare una combinazione di informazioni da questa pagina e dall'argomento intitolato procedura: aggiungere attività e categorie alla finestra di avvio? in [Windows Server Solutions SDK](https://go.microsoft.com/fwlink/?LinkID=248648).  
  
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
  
## <a name="see-also"></a>Vedi anche  
 [Creazione e personalizzazione dell'immagine](Creating-and-Customizing-the-Image.md)   
 [Personalizzazioni aggiuntive](Additional-Customizations.md)   
 [Preparazione dell'immagine per la distribuzione](Preparing-the-Image-for-Deployment.md)   
 [Test di Analisi utilizzo software](Testing-the-Customer-Experience.md)