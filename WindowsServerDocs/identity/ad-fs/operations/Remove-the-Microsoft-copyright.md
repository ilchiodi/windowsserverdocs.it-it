---
ms.assetid: c89a977c-b09f-44ec-be42-41e76a6cf3ad
title: Rimuovere il copyright Microsoft
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 9306950ab83ea94c1ff814ea9a404c0efeff0e40
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80816214"
---
# <a name="remove-the-microsoft-copyright"></a>Rimuovere il copyright Microsoft 


 
Per impostazione predefinita, le pagine di ADFS includono il copyright Microsoft. Per rimuoverlo dalle pagine personalizzate, è possibile usare la procedura seguente. 

![rimuovere il copyright](media/AD-FS-user-sign-in-customization/ADFS_Blue_Custom1.png) 
  
## <a name="to-remove-the-microsoft-copyright"></a>Per rimuovere il copyright Microsoft  
  
1. Creare un tema personalizzato basato su quello predefinito.

   ```powershell
   New-AdfsWebTheme –Name custom –SourceName default
   ```

2. Esportare il tema specificando la cartella di output.  

   ```powershell
   Export-AdfsWebTheme -Name custom -DirectoryPath C:\CustomWebTheme
   ```

3. Individuare il file `Style.css` che si trova nella cartella di output. Utilizzando l'esempio precedente, il percorso è `C:\CustomWebTheme\Css\Style.css.`
  
4. Aprire il file di `Style.css` con un editor, ad esempio Blocco note.  
  
5. Trovare la parte `#copyright` e modificarla come segue:  

   ```css
   #copyright {color:#696969; display:none;}
   ```

6. Creare un tema personalizzato basato sul nuovo file di `Style.css`.  

   ```powershell
   Set-AdfsWebTheme -TargetName custom -StyleSheet @{locale="";path="C:\customWebTheme\css\style.css"}
   ```

7. Attivare il nuovo tema.  

   ```powershell
   Set-AdfsWebConfig -ActiveThemeName custom
   ```

A questo punto, non dovrebbe essere il copyright nella parte inferiore della pagina di accesso.

![rimuovere il copyright](media/AD-FS-user-sign-in-customization/ADFS_Blue_Custom1a.png) 

## <a name="additional-references"></a>Altri riferimenti 
[Personalizzazione dell'accesso utente AD FS](AD-FS-user-sign-in-customization.md) 
