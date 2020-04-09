---
title: Installazione o rimozione dei Language Pack
description: Viene descritto come usare Windows Server Essentials
ms.date: 10/03/2016
ms.prod: windows-server
ms.topic: article
ms.assetid: 98f13f63-4480-40ba-a7ef-d1d9b7582e5f
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: c1fd1d21277d32672398d1dd201e2dda24682c2d
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80820024"
---
# <a name="install-or-remove-language-packs"></a>Installazione o rimozione dei Language Pack

>Si applica a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

> [!NOTE]
>  Prima di aggiungere il Language Pack di Windows Server Essentials, è necessario creare un'immagine multilingue di Windows come descritto in [Language Pack e distribuzione](https://technet.microsoft.com/library/hh824829) .  
  
 I Language Pack sono disponibili solo per la creazione di immagini multilingue. Le informazioni contenute in questa sezione sono specifiche per l'installazione o la rimozione dei Language Pack in Windows Server Essentials.  
  
> [!NOTE]
>  Se si intende eseguire la configurazione iniziale da un computer client che non supporta le lingue orientali, come ja-jp e se l'inglese non è incluso nell'immagine multilingue sul server, sulla pagina Web della configurazione iniziale vengono visualizzati dei quadrati. Affinché la pagina Web dell'IC passi per impostazione predefinita all'inglese, è necessario che l'immagine multilingue creata includa l'inglese.  
  
## <a name="adding-language-packs-to-an-image"></a>Aggiunta di Language Pack a un'immagine  
 I Language Pack sono disponibili sul DVD di personalizzazione OEM. Si consiglia di copiare i Language Pack sul computer del tecnico prima di aggiungerli all'immagine.  
  
 Per installare i Language Pack, è necessario utilizzare il seguente comando:  
  
 **DISM. exe/online/Add-Package/PackagePath: C:\\< percorso completo della directory del file CAB\>\lp.cab**  
  
 Ad esempio, il seguente comando, mostra come aggiungere il Language Pack per il tedesco:  
  
 **DISM. exe/online/Add-Package/PackagePath: C:\Users\Administrator\Desktop\WindowsHomeServer-Product-r\de-de\lp.cab**  
  
> [!IMPORTANT]
>  È inoltre necessario applicare i Language Pack per Windows Server Essentials per localizzare completamente il sistema operativo.  
  
## <a name="removing-language-packs-from-an-image"></a>Rimozione di Language Pack da un'immagine  
 Per rimuovere un Language Pack che non si desidera più inserire in un'immagine, è possibile utilizzare il seguente comando:  
  
 **DISM. exe/online/Remove-Package/PackagePath: C:\\< percorso completo della directory del file CAB\>\lp.cab**  
  
 Ad esempio, il seguente comando, mostra come rimuovere il Language Pack per il tedesco:  
  
 **DISM. exe/online/Remove-Package/PackagePath: C:\Users\Administrator\Desktop\WindowsHomeServer-Product-r\de-de\lp.cab**  
  
## <a name="see-also"></a>Vedi anche  

 [Creazione e personalizzazione dell'immagine](Creating-and-Customizing-the-Image.md)   
 [Personalizzazioni aggiuntive](Additional-Customizations.md)   
 [Preparazione dell'immagine per la distribuzione](Preparing-the-Image-for-Deployment.md)   
 [Test di Analisi utilizzo software](Testing-the-Customer-Experience.md)

 [Creazione e personalizzazione dell'immagine](../install/Creating-and-Customizing-the-Image.md)   
 [Personalizzazioni aggiuntive](../install/Additional-Customizations.md)   
 [Preparazione dell'immagine per la distribuzione](../install/Preparing-the-Image-for-Deployment.md)   
 [Test di Analisi utilizzo software](../install/Testing-the-Customer-Experience.md)

