---
title: Creare il File Oobe.xml con Logo ed EULA
description: Viene descritto come utilizzare Windows Server Essentials
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 8a7b3cc1-21bb-4344-8110-f5d5959b370d
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: f8f99a2051e114b3c890f1cdac23aebf58689980
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/12/2017
---
# <a name="create-the-oobexml-file-including-logo-and-eula"></a>Creare il File Oobe.xml con Logo ed EULA

>Si applica a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

È possibile aggiungere il proprio contratto di licenza con l'utente finale (EULA) alla configurazione iniziale utilizzando il file Oobe.xml. Il Oobe.xml è un file utilizzato per fornire testo e immagini per la configurazione iniziale dell'installazione di Windows e altre pagine presentate all'utente finale. È possibile aggiungere più file Oobe.xml per personalizzare il contenuto in base alle selezioni lingua e paese / area geografica dell'utente finale. Per ulteriori informazioni, vedere il [Windows Assessment and Deployment Kit per Windows 8](https://go.microsoft.com/fwlink/?LinkId=248694) documentazione.  
  
 L'EULA della società viene visualizzato oltre all'EULA di Microsoft. Verrà l'EULA di Microsoft il primo contratto visualizzato durante l'esperienza utente finale di configurazione iniziale, quindi sarà EULA visualizzato. L'EULA personale può essere collocata ovunque sul server e specificare il percorso del file Oobe.xml.  
  
#### <a name="to-add-your-company-eula-and-logo"></a>Per aggiungere il contratto di licenza aziendale e logo  
  
1.  Aprire il file Oobe.xml in un editor di testo come blocco note.  
  
2.  All'interno di < logopath\ >< / logopath\ > tag, immettere il percorso assoluto del file del logo. Questo file deve contenere un file di (.png) grafica di rete mobile a 32 bit di 240X 100 pixel.  
  
3.  All'interno di < eulafilename\ >< / eulafilename\ > tag, immettere il percorso assoluto del file EULA. Il file EULA deve essere un file (.rtf) in formato RTF.  
  
4.  All'interno di < name\ >< / name\ > tag, immettere il nome della società.  
  
     L'esempio seguente mostra i tag in un file Oobe.xml:  
  
    ```  
  
    <FirstExperience>  
       <oobe>  
          <oem>  
             <name>Fabrikam</name>  
             <logopath>c:\fabrikam\fabrikam.png</logopath>  
             <eulafilename>c:\fabrikam\fabrikam_eula.rtf</eulafilename>  
          </oem>  
       </oobe>  
    </FirstExperience>  
  
    ```  
  
5.  Salvare il file.  
  
6.  Inserire il file Oobe.xml in uno dei seguenti percorsi:  
  
    |Percorso Oobe.xml|Condizione per determinare la posizione|  
    |-----------------------|----------------------------------------|  
    |%windir%\system32\oobe\info\|Il server è destinato ad un unico paese/area geografica e un unico sistema linguistico.|  
    |%windir%\system32\oobe\info\default\\ < lingua >|Il server è destinato ad un unico paese/area geografica e sistema multilingua.|  
    |%windir%\system32\oobe\info\\ < paese/area geografica > \ e %windir%\system32\oobe\info\\ < paese/area geografica > \ < lingua > \|Il server è destinato a più di un paese/area geografica e le impostazioni richiedono personalizzazioni in base al paese/area, ognuna con una sola lingua. Dove < paese/area geografica > è alla versione decimale dell'identificatore della posizione geografica (GeoID) del paese o area geografica in cui viene distribuito il server e < lingua > corrisponde alla versione decimale dell'identificatore delle impostazioni locali (LCID).|  
  
 Se si dispone di un logo aziendale alternativo con del testo bianco, potrebbero visualizzare una migliore del flusso di installazione a causa di sfondo blu.  Facoltativamente, è possibile specificare questo logo impostando un chiave del Registro di sistema e il valore.  
  
#### <a name="to-specify-a-company-logo-by-setting-the-oem-registry-key"></a>Per specificare un logo aziendale impostando la chiave del Registro di sistema OEM  
  
1.  Nel server, sposta il mouse nell'angolo superiore destro dello schermo e fare clic su **ricerca**.  
  
2.  Nella casella di ricerca, digitare **regedit**, quindi fare clic sull'applicazione Regedit.  
  
3.  Nel riquadro di spostamento, passare a **HKEY_LOCAL_MACHINE**, espandere **SOFTWARE**, espandere **Microsoft**, espandere **Windows Server**. Se la chiave OEM non esiste, creare la chiave come segue:  
  
    1.  Fare doppio clic su **Windows Server**, fare clic su **New**, quindi fare clic su **chiave**.  
  
    2.  Nome della chiave, digitare **OEM**.  
  
4.  (Facoltativo) Se si sta creando una voce per un logo, è possibile creare diverse chiavi per distinguere le versioni di lingua del logo. Ad esempio, se si dispone sia inglese e tedesche versioni di un logo, è possibile creare un en-us chiave e una chiave de-de. Poiché tutti i file del logo sono archiviati nella stessa cartella, è necessario fornire le istanze del file di immagine logo un nome univoco per ogni lingua. Ad esempio, è possibile creare un file denominato LogoWithWhiteText_en.png e LogoWithWhiteText_de.png.  
  
5.  Uno dei due pulsante destro del mouse **OEM** o fare clic sulla chiave della lingua appropriata, fare clic su **New**, quindi fare clic su **valore stringa**.  
  
6.  Immettere LogoWithWhiteText come stringa e quindi premere INVIO.  
  
7.  Fare clic sulla nuova stringa e quindi fare clic su **modifica**.  
  
8.  Immettere il percorso che contiene l'immagine del logo e quindi fare clic su OK.  
  
## <a name="see-also"></a>Vedere anche  
 [Guida introduttiva a Windows Server Essentials ADK](Getting-Started-with-the-Windows-Server-Essentials-ADK.md)   
 [Creazione e personalizzazione dell'immagine](Creating-and-Customizing-the-Image.md)   
 [Ulteriori personalizzazioni](Additional-Customizations.md)   
 [Preparazione dell'immagine per la distribuzione](Preparing-the-Image-for-Deployment.md)   
 [Test di analisi utilizzo software](Testing-the-Customer-Experience.md)