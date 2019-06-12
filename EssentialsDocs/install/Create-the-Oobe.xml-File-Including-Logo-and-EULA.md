---
title: Creare il file Oobe.xml con logo ed EULA
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
ms.openlocfilehash: 5d7dac41ba6d6f73b0d3d65d3481fe45ff99a6bc
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/31/2019
ms.locfileid: "66433614"
---
# <a name="create-the-oobexml-file-including-logo-and-eula"></a>Creare il file Oobe.xml con logo ed EULA

>Si applica a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

È possibile aggiungere il proprio contratto di licenza con l'utente finale (EULA) alla configurazione iniziale utilizzando il file Oobe.xml. Il file Oobe.xml viene utilizzato per fornire testo e immagini alla configurazione iniziale, a Configurazione e personalizzazione di Windows e ad altre pagine presentate all'utente finale. È possibile aggiungere più file Oobe.xml per personalizzare il contenuto in base alla selezione di lingua e paese/area geografica dell'utente finale. Per altre informazioni, vedere la documentazione di [Windows Assessment and Deployment Kit per Windows 8](https://go.microsoft.com/fwlink/?LinkId=248694) .  
  
 L'EULA della società viene visualizzato oltre all'EULA di Microsoft. L'EULA di Microsoft sarà il primo ad essere visualizzato durante la configurazione iniziale dell'utente finale, quindi verrà visualizzato l'EULA personale. L'EULA personale può essere collocato ovunque sul server, specificandone la posizione nel file Oobe.xml.  
  
#### <a name="to-add-your-company-eula-and-logo"></a>Aggiunta dell'EULA e del logo della società  
  
1. Aprire il file Oobe.xml in un editor di testo, come Blocco note.  
  
2. All'interno di < logopath\>< / logopath\> tag, immettere il percorso assoluto del file del logo. Questo file deve contenere un file .png a 32 bit (portable network graphics) di 240x 100 pixel.  
  
3. All'interno di < eulafilename\>< / eulafilename\> tag, immettere il percorso assoluto del file EULA. Il file EULA deve essere in formato di testo .rtf (rich-text-format).  
  
4. All'interno di < nome\>< / name\> tag, immettere il nome della società.  
  
    Nell'esempio seguente sono illustrati i tag contenuti in un file Oobe.xml:  
  
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
  
5. Salvare il file.  
  
6. Collocare il file Oobe.xml in uno dei seguenti percorsi:  
  
   |Percorso Oobe.Xml|Condizione per la determinazione del percorso|  
   |-----------------------|----------------------------------------|  
   |%WINDIR%\System32\Oobe\Info\|il server è destinato in un unico paese/area geografica e un unico sistema linguistico.|  
   |%windir%\system32\oobe\info\default\\<language\>|Il server è destinato ad un unico paese/area geografica o a un sistema multilingua.|  
   |%WINDIR%\System32\Oobe\Info\\< paese/area geografica > \ e %windir%\system32\oobe\info\\< paese/area geografica >\\< linguaggio\>\|il server è destinato a più di un paese / area e le impostazioni richiedono personalizzazioni in base al paese/regione, ognuno con una sola lingua. Dove < paese/area geografica > corrisponde alla versione decimale dell'identificatore della posizione geografica (GeoID) del paese / area geografica in cui viene distribuito il server, e < language\> corrisponde alla versione decimale dell'identificatore delle impostazioni locali (LCID).|  
  
   Se si dispone di un logo aziendale alternativo con del testo bianco, la visualizzazione durante la procedura di installazione potrebbe essere migliore in virtù dello sfondo di colore blu.  È possibile specificare questo logo impostando un chiave del Registro di sistema con relativo valore.  
  
#### <a name="to-specify-a-company-logo-by-setting-the-oem-registry-key"></a>Per specificare un logo aziendale impostando la chiave del Registro di sistema OEM  
  
1.  Sul server, spostare il puntatore del mouse verso l'angolo superiore destro dello schermo e fare clic su **Trova**.  
  
2.  Nella casella di ricerca digitare **regedit** e quindi fare clic sull'applicazione Regedit.  
  
3.  Nel riquadro di navigazione, espandere  **HKEY_LOCAL_MACHINE**, espandere **SOFTWARE**, espandere **Microsoft**, espandere **Windows Server**. Se la chiave OEM non esiste, crearne una facendo quanto segue:  
  
    1.  Fare clic con il pulsante destro del mouse su **Windows Server**, fare clic su **Nuovo**, quindi fare clic su **Chiave**.  
  
    2.  Digitare **OEM**come nome della chiave.  
  
4.  (Facoltativo) Se si sta creando una voce per un logo, è possibile creare diverse chiavi per distinguere le versioni in lingue diverse del logo. Ad esempio, se si dispone sia della versione in inglese che della versione in tedesco di un logo, è possibile creare una chiave en-us e una chiave de-de. Dal momento che tutti i file del logo sono archiviati nella stessa cartella, è necessario fornire alle istanze del file di immagine logo un nome univoco per ogni lingua. Ad esempio, creare un file denominato LogoWithWhiteText_en.png e uno denominato LogoWithWhiteText_de.png.  
  
5.  Fare clic con il tasto destro del mouse su **OEM** o sulla chiave della lingua appropriata, quindi selezionare **Nuovo** seguito da **Valore stringa**.  
  
6.  Immettere LogoWithWhiteText come stringa, quindi premere INVIO.  
  
7.  Fare clic con il tasto destro del mouse sulla stringa nuova, quindi selezionare **Modifica**.  
  
8.  Immettere il percorso del logo, quindi fare clic su OK.  
  
## <a name="see-also"></a>Vedere anche  
 [Guida introduttiva a Windows Server Essentials ADK](Getting-Started-with-the-Windows-Server-Essentials-ADK.md)   
 [Creazione e personalizzazione dell'immagine](Creating-and-Customizing-the-Image.md)   
 [Personalizzazioni aggiuntive](Additional-Customizations.md)   
 [Preparazione dell'immagine per la distribuzione](Preparing-the-Image-for-Deployment.md)   
 [Test di Analisi utilizzo software](Testing-the-Customer-Experience.md)