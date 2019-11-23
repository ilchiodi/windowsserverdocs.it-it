---
ms.assetid: a4526500-24b3-423d-805c-24b0d8061aba
title: Modificare l'illustrazione nella pagina di accesso AD FS
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 3da7726ca625c32728fb0ae64d291ae599b6cd8d
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71358294"
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
  
  
