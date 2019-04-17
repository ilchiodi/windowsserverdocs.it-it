---
title: Installazione o rimozione dei Language Pack
description: Viene descritto come utilizzare Windows Server Essentials
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 98f13f63-4480-40ba-a7ef-d1d9b7582e5f
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: a41b491bbe4b4a8ee7f9743dc85e5bdaffb08496
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/12/2017
---
# <a name="install-or-remove-language-packs"></a>Installazione o rimozione dei Language Pack

>Si applica a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

> [!NOTE]
>  È innanzitutto necessario creare un'immagine multilingue di Windows come descritto nel [Language Pack e distribuzione](https://technet.microsoft.com/library/hh824829) prima di aggiungere il language pack di Windows Server Essentials.  
  
 Language Pack disponibili solo per la creazione di immagini multilingue. Le informazioni contenute in questa sezione sono specifiche per l'installazione o rimozione dei language pack in Windows Server Essentials.  
  
> [!NOTE]
>  Se si intende eseguire configurazione iniziale (IC) da un computer client che non supporta le lingue orientali, come ja-jp e inglese non è incluso nell'immagine multilingue sul server, la pagina Web IC vengono visualizzati dei quadrati. Per la pagina Web IC per impostazione predefinita all'inglese, l'immagine multilingue creata è necessario includere inglese.  
  
## <a name="adding-language-packs-to-an-image"></a>Aggiunta di language pack a un'immagine  
 Language Pack sono disponibili nel programma di personalizzazione OEM DVD. Si consiglia di copiare i language pack nel computer tecnico prima di aggiungere i language pack per l'immagine.  
  
 Per installare i language pack, è necessario utilizzare il comando seguente:  
  
 **DISM.exe /online /Add-Package /PackagePath: \ \ < percorso completo di directory\ file cab > \lp.cab**  
  
 Ad esempio, il comando seguente viene illustrato come aggiungere un language pack tedesco:  
  
 **DISM.exe /online /Add-Package /PackagePath:C:\Users\Administrator\Desktop\WindowsHomeServer-Product-r\de-de\lp.cab**  
  
> [!IMPORTANT]
>  È anche necessario applicare i language pack per Windows Server Essentials per poter localizzare completamente il sistema operativo.  
  
## <a name="removing-language-packs-from-an-image"></a>Rimozione dei language pack da un'immagine  
 Per rimuovere un language pack che si desidera includere in un'immagine non è possibile utilizzare il comando seguente:  
  
 **DISM.exe /online /Remove-Package /PackagePath: \ \ < percorso completo di directory\ file cab > \lp.cab**  
  
 Ad esempio, il comando seguente mostra come rimuovere un language pack tedesco:  
  
 **DISM.exe /online /Remove-Package /PackagePath:C:\Users\Administrator\Desktop\WindowsHomeServer-Product-r\de-de\lp.cab**  
  
## <a name="see-also"></a>Vedere anche  

 [Creazione e personalizzazione dell'immagine](Creating-and-Customizing-the-Image.md)   
 [Ulteriori personalizzazioni](Additional-Customizations.md)   
 [Preparazione dell'immagine per la distribuzione](Preparing-the-Image-for-Deployment.md)   
 [Test di analisi utilizzo software](Testing-the-Customer-Experience.md)

 [Creazione e personalizzazione dell'immagine](../install/Creating-and-Customizing-the-Image.md)   
 [Ulteriori personalizzazioni](../install/Additional-Customizations.md)   
 [Preparazione dell'immagine per la distribuzione](../install/Preparing-the-Image-for-Deployment.md)   
 [Test di analisi utilizzo software](../install/Testing-the-Customer-Experience.md)

