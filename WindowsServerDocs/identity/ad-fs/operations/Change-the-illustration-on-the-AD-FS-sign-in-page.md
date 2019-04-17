---
ms.assetid: a4526500-24b3-423d-805c-24b0d8061aba
title: Cambiare l'illustrazione nella pagina di accesso di ADFS
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: ac5e60aaad864248b58a3908e7aa9622165fbc14
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2017
---
# <a name="change-the-illustration-on-the-ad-fs-sign-in-page"></a>Cambiare l'illustrazione nella pagina di accesso di ADFS

>Si applica a: Windows Server 2016, Windows Server 2012 R2

## <a name="change-the-illustration"></a>Cambiare l'illustrazione  


Per cambiare l'illustrazione, l'elemento grafico a sinistra, visualizzato nella pagina di accesso, usare la sintassi e i cmdlet PowerShell di Windows PowerShell seguente.  

![cambiare l'illustrazione](media/AD-FS-user-sign-in-customization/ADFS_Blue_Custom2.png)
  
> [!IMPORTANT]  
> È consigliabile usare per l'illustrazione 1420 x 1080 pixel @ 96 DPI con una dimensione di file non superiore a 200 KB.  
  
 
    Set-AdfsWebTheme -TargetName default -Illustration @{path="c:\Contoso\illustration.png"}  

## <a name="additional-references"></a>Riferimenti aggiuntivi 
[AD FS Sign-personalizzazione utente](AD-FS-user-sign-in-customization.md)  
  
  
