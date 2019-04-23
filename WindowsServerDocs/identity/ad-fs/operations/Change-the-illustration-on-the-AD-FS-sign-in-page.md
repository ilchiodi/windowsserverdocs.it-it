---
ms.assetid: a4526500-24b3-423d-805c-24b0d8061aba
title: Cambiare l'illustrazione nella pagina di accesso di AD FS
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 4f1cba9862766092c2beadb894cbac092d146887
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59858242"
---
# <a name="change-the-illustration-on-the-ad-fs-sign-in-page"></a>Cambiare l'illustrazione nella pagina di accesso di AD FS

>Si applica a: Windows Server 2016, Windows Server 2012 R2

## <a name="change-the-illustration"></a>Cambiare l'illustrazione  


Per cambiare l'illustrazione, il grafico a sinistra, che viene visualizzato il segno\-nella pagina, utilizzare la sintassi e i cmdlet di Windows PowerShell seguente.  

![cambiare l'illustrazione](media/AD-FS-user-sign-in-customization/ADFS_Blue_Custom2.png)
  
> [!IMPORTANT]  
> È consigliabile usare un'illustrazione con dimensioni di 1420x1080, 96 dpi, in un file con dimensioni massime di 200 KB.  
  
 
    Set-AdfsWebTheme -TargetName default -Illustration @{path="c:\Contoso\illustration.png"}  

## <a name="additional-references"></a>Altri riferimenti 
[AD FS Sign-in personalizzazione dell'utente](AD-FS-user-sign-in-customization.md)  
  
  
