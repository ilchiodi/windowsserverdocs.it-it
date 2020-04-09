---
ms.assetid: a4526500-24b3-423d-805c-24b0d8061aba
title: Modificare l'illustrazione nella pagina di accesso AD FS
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: de0fc7af4c4eecad59b7af6b08426ef207da5db1
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80859954"
---
# <a name="change-the-illustration-on-the-ad-fs-sign-in-page"></a>Modificare l'illustrazione nella pagina di accesso AD FS

## <a name="change-the-illustration"></a>Modificare l'illustrazione  


Per modificare l'illustrazione, l'elemento grafico a sinistra, visualizzato nella pagina Sign\-in, usare la sintassi e il cmdlet di Windows PowerShell seguenti.  

![modifica illustrazione](media/AD-FS-user-sign-in-customization/ADFS_Blue_Custom2.png)
  
> [!IMPORTANT]  
> È consigliabile usare un'illustrazione con dimensioni di 1420x1080, 96 dpi, in un file con dimensioni massime di 200 KB.  
  
 
    Set-AdfsWebTheme -TargetName default -Illustration @{path="c:\Contoso\illustration.png"}  

## <a name="additional-references"></a>Altri riferimenti 
[Personalizzazione dell'accesso utente AD FS](AD-FS-user-sign-in-customization.md)  
  
  
