---
ms.assetid: da035189-e87f-4597-9933-49bf391a8d5d
title: Aggiungere un collegamento alla home page
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: f4a6210b1b7475a4ec34bbe0575915f376381fe1
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80859384"
---
# <a name="add-home-link"></a>Aggiungere un collegamento alla home page 

Per aggiungere il collegamento alla home page viene visualizzato il segno\-nella pagina, utilizzare la sintassi e i cmdlet Windows PowerShell seguente. 


![Aggiungi collegamento alla home page](media/AD-FS-user-sign-in-customization/ADFS_Blue_Custom2.png) 
  

`Set-AdfsGlobalWebContent -HomeLink https://fs1.contoso.com/home/ -HomeLinkText Home ` 
 
  
> [!IMPORTANT]  
> Il parametro `linkText` in questo cmdlet non è obbligatorio, a meno che venga usato un valore diverso da quello predefinito, cioè *Home*. L'uso del valore predefinito ha il vantaggio di essere localizzato in tutte le impostazioni locali del client. Dopo il segno di\-nella pagina è personalizzato, la personalizzazione ha la precedenza; pertanto, è necessario applicarli a tutte le lingue che si desidera supportare.

## <a name="additional-references"></a>Altri riferimenti 
[Personalizzazione dell'accesso utente AD FS](AD-FS-user-sign-in-customization.md)  
