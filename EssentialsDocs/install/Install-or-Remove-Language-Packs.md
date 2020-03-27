---
title: Installazione o rimozione dei Language Pack
description: Viene descritto come usare Windows Server Essentials
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 98f13f63-4480-40ba-a7ef-d1d9b7582e5f
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 4f7657f3ecb3450b5253d1cfe2813c12ecc2718e
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/26/2020
ms.locfileid: "80311725"
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

