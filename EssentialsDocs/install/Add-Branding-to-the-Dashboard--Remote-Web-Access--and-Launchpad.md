---
title: Aggiunta della personalizzazione al Dashboard, accesso Web remoto e finestra di avvio
description: Viene descritto come utilizzare Windows Server Essentials
ms.custom: na
ms.date: 04/10/2014
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 166262f8-b2a5-4b1c-a4a7-a141e1c54f10
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 02088b169e44cdcf87385425e1949232ffa408a6
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2017
---
# <a name="add-branding-to-the-dashboard-remote-web-access-and-launchpad"></a>Aggiunta della personalizzazione al Dashboard, accesso Web remoto e finestra di avvio

>Si applica a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

##  <a name="BKMK_Branding"></a>Aggiunta della personalizzazione al Dashboard, accesso Web remoto e finestra di avvio  
 È possibile aggiungere svariate personalizzazioni mediante aggiunta di voci nel Registro di sistema. Tutte le voci di personalizzazione nel Registro di sistema per il sistema operativo si trovano in HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows server\OEM..  
  
 Il co-branding deve soddisfare i requisiti del logo seguenti:  
  
-   Il logo di Windows Server Essentials deve avere una larghezza minima di **170 pixel**, mantenendo le corrette proporzioni, con **96 DPI**.  
  
#### <a name="to-add-branding-by-changing-the-registry"></a>Per aggiungere la personalizzazione modificando il Registro di sistema  
  
1.  Nel server, sposta il mouse nell'angolo superiore destro dello schermo e fare clic su **ricerca**.  
  
2.  Nella casella di ricerca, digitare **regedit**, quindi fare clic su di **Regedit** applicazione.  
  
3.  Nel riquadro di spostamento, espandere **HKEY_LOCAL_MACHINE**, espandere **SOFTWARE**, espandere **Microsoft**, espandere **Windows Server**. Se il **OEM** chiave non esiste, completare i passaggi seguenti per crearla:  
  
    1.  Fare doppio clic su **Windows Server**, fare clic su **New**, quindi fare clic su **chiave**.  
  
    2.  Tipo **OEM** per il nome della chiave.  
  
4.  (Facoltativo) Se si sta creando una voce per un logo, è possibile creare diverse chiavi per distinguere le versioni di lingua del logo. Ad esempio, se si dispone sia inglese e tedesche versioni di un logo, è possibile creare un en-us chiave e una chiave de-de. Poiché tutti i file del logo sono archiviati nella stessa cartella, è necessario fornire le istanze del file di immagine logo un nome univoco per ogni lingua. Ad esempio, creare un file denominato DashboardLogo_en.png e DashboardLogo_de.png.  
  
5.  Uno dei due pulsante destro del mouse **OEM** o fare clic sulla chiave della lingua appropriata, fare clic su **New**, quindi fare clic su **valore stringa**.  
  

6.  Immettere il nome della stringa e quindi premere INVIO. Fare riferimento al [valori e stringhe del Registro di sistema](Add-Branding-to-the-Dashboard--Remote-Web-Access--and-Launchpad.md#BKMK_RegStrings) tabella per i valori di nomi e i dati stringa.  

6.  Immettere il nome della stringa e quindi premere INVIO. Fare riferimento al [valori e stringhe del Registro di sistema](../install/Add-Branding-to-the-Dashboard--Remote-Web-Access--and-Launchpad.md#BKMK_RegStrings) tabella per i valori di nomi e i dati stringa.  

  
7.  Fare clic sulla nuova stringa e quindi fare clic su **modifica**.  
  
8.  Immettere il valore dalla tabella associata con il nome della stringa, quindi fare clic su **OK**.  
  
9. Se si sta creando una voce per un'immagine del logo o per collegamenti aggiunti, copiare il file in %programFiles%\Windows server\bin\OEM.. Se la directory OEM non esiste, crearla.  
  
10. Se vengono apportate modifiche che influiscono sull'accesso Web remoto, dopo che l'utente prende possesso del server, è necessario attivare accesso Web remoto. Consigliare al cliente di eseguire le operazioni seguenti:  
  
    1.  Nel Dashboard, fare clic su **impostazioni**, quindi fare clic su di **accesso remoto via Internet** scheda.  
  
    2.  Se accesso remoto via Internet è attivata, fare clic su **configura**, quindi deselezionare la casella di controllo di accesso Web remoto nel **funzionalità scegliere accesso remoto via Internet da abilitare** pagina della configurazione guidata accesso remoto via Internet.  
  
    3.  Fare clic su **configurare**.  
  
###  <a name="BKMK_RegStrings"></a>La tabella seguente elenca il percorso in cui le modifiche del Registro di sistema influiscono sulla personalizzazione, il nome della stringa e il valore dei dati.  
  
### <a name="registry-strings-and-values"></a>Valori e stringhe del Registro di sistema  
  
|Posizione della personalizzazione|Descrizione|Nome della stringa|Valore dei dati|  
|--------------------------|-----------------|-----------------|----------------|  
|Logo del dashboard|Aggiunge l'immagine del logo al Dashboard. Il logo della Dashboard deve essere in formato PNG e non deve essere maggiore di 350 pixel in larghezza per 38 pixel di altezza.<br /><br /> **Importante:** per personalizzare la Dashboard con il logo, è necessario modificare l'illustrazione, è disponibile nel DVD di OPK e aggiunta il logo della società all'immagine assicurandosi di rispettare i requisiti relativi allo spazio bianco. Per ulteriori informazioni, vedere il riquadro di esempio fornito.|DashboardLogo|Nome del file di immagine logo|  
|DashboardClientLogo|Aggiunge l'immagine del logo alla schermata di accesso client Dashboard.|DashboardClientLogo|Nome del file di immagine logo|  
|Immagine di sfondo sito Web|Modifica l'immagine di sfondo che viene visualizzato nella pagina di accesso di accesso Web remoto. Le risoluzioni tipiche appariranno come segue:<br /><br /> -1024 x 768 pixel la risoluzione riempirà esattamente tutta la pagina di accesso<br /><br /> -800 x Risoluzione 600 pixel verrà centrata nella pagina e apparirà con un bordo nero.<br /><br /> -1280 x 720 pixel la risoluzione verrà centrata e non verranno visualizzati i pixel eccedenti 1024 × 768.|LogonBackground|Nome del file di immagine di sfondo|  
|Titolo del sito Web|Sostituisce il titolo del sito accesso Web remoto da Windows Server Essentials con un titolo che preferisci.|WebsiteName|Nuovo titolo del sito accesso Web remoto|  
|Logo del sito Web|Modifica il logo predefinito nel sito di accesso Web remoto. Le dimensioni previste del logo sono 32 pixel per 32 pixel. Se il logo inferiori o superiori, verrà allungato o ridotto per adattarlo a queste dimensioni|WebsiteLogo|Nome del file di immagine logo|  
|Logo del sito Web aggiunto|Il logo del partner verrà visualizzato proprio sotto il logo Microsoft visualizzato nel sito di accesso Web remoto. Le dimensioni previste del logo sono 200 pixel in altezza per 50 pixel in larghezza. Se il logo è superiore, questo sarà reso più piccoli in modo da adattarsi mantenendo le proporzioni originali. Se il logo è inferiore, verrà centrato nello spazio di 200 per 50 pixel e nessuna delle due dimensioni e proporzioni verrà modificata.|OEMLogo|Nome del file di immagine logo|  

| I collegamenti nella pagina di accesso e home page sito Web | Aggiungere collegamenti alla pagina di accesso e home page del sito di accesso Web remoto. Il codice XML che contiene le informazioni sui collegamenti deve trovarsi in %programFiles%\Windows server\bin\OEM.. L'esempio seguente mostra il formato del file XML:<br /><br /> < OemLinks\ ><br /> < LogonLinks\ ><br /> < link Name\ = LogonLinkName ><br /> < testo > LogonLinkDescription < / testo ><br /> < Url\ > LogonLinkURL < / Url\ ><br /> Valore LinkIcon < Icon\ > < / Icon\ ><br /> < / Link\ ><br /> < / LogonLinks\ ><br /> < HomepageLinks\ ><br /> < link Name\ = HomepageLinkName ><br /> < testo > HomepageLinkDescription < / testo ><br /> < Url\ > HomepageLinkURL < / Url\ ><br /> < / Link\ ><br /> < / HomepageLinks\ ><br /> < / OemLinks\ > & LinksXML | Fare riferimento al [elementi LinksXML](Add-Branding-to-the-Dashboard--Remote-Web-Access--and-Launchpad.md#BKMK_Links) tabella per un elenco degli elementi e descrizioni. |  

| I collegamenti nella pagina di accesso e home page sito Web | Aggiungere collegamenti alla pagina di accesso e home page del sito di accesso Web remoto. Il codice XML che contiene le informazioni sui collegamenti deve trovarsi in %programFiles%\Windows server\bin\OEM.. L'esempio seguente mostra il formato del file XML:<br /><br /> < OemLinks\ ><br /> < LogonLinks\ ><br /> < link Name\ = LogonLinkName ><br /> < testo > LogonLinkDescription < / testo ><br /> < Url\ > LogonLinkURL < / Url\ ><br /> Valore LinkIcon < Icon\ > < / Icon\ ><br /> < / Link\ ><br /> < / LogonLinks\ ><br /> < HomepageLinks\ ><br /> < link Name\ = HomepageLinkName ><br /> < testo > HomepageLinkDescription < / testo ><br /> < Url\ > HomepageLinkURL < / Url\ ><br /> < / Link\ ><br /> < / HomepageLinks\ ><br /> < / OemLinks\ > & LinksXML | Fare riferimento al [elementi LinksXML](../install/Add-Branding-to-the-Dashboard--Remote-Web-Access--and-Launchpad.md#BKMK_Links) tabella per un elenco degli elementi e descrizioni. |  

| Logo LaunchPad | Aggiunge l'immagine del logo alla finestra di avvio. Il logo Launchpad deve essere in formato PNG e non più alto di 64 pixel deve. | LaunchpadLogo | Nome del file di immagine logo |  
  
###  <a name="BKMK_Links"></a>Nella tabella seguente elenca e descrive gli elementi di nome di stringa LinksXML.  
  
### <a name="linksxml-elements"></a>Elementi LinksXML  
  
|Elemento LinksXML|Descrizione|  
|----------------------|-----------------|  
|**LogonLinks**|  
|LogonLinkName|Il nome di collegamento di accesso.|  
|LogonLinkDescription|Il testo che viene visualizzato come il collegamento di pagina di accesso.|  
|LogonLinkURL|L'url che consente di risolvere il collegamento di pagina di accesso.|  
|Valore LinkIcon|Il nome del file icona per il collegamento di accesso. Questo file deve trovarsi nello stesso percorso di cartella del file XML. Le immagini icona di collegamento devono avere dimensioni pari a 16x16 pixel e devono essere formato. PNG. Se non si specifica un valore LinkIcon, viene utilizzata l'immagine dell'icona di collegamento predefinito.|  
|**HomepageLinks**|  
|HomepageLinkName|Il nome del collegamento alla home page.|  
|HomepageLinkDescription|Il testo visualizzato come collegamento alla home page.|  
|HomepageLinkURL|L'URL che consente di risolvere il collegamento alla home page.|  
|Valore HomepageLinkIcon|Il nome del file icona per il collegamento alla home page. Questo file deve trovarsi nello stesso percorso di cartella del file XML. Immagini HomepageLinkIcon devono essere 16x16 pixel ed essere formato. PNG. Se non si specifica un valore HomepageLinkIcon, viene utilizzata l'immagine icona del collegamento pagina iniziale predefinita.|  
  
## <a name="see-also"></a>Vedere anche  

 [Creazione e personalizzazione dell'immagine](Creating-and-Customizing-the-Image.md)   
 [Ulteriori personalizzazioni](Additional-Customizations.md)   
 [Preparazione dell'immagine per la distribuzione](Preparing-the-Image-for-Deployment.md)   
 [Test di analisi utilizzo software](Testing-the-Customer-Experience.md)

 [Creazione e personalizzazione dell'immagine](../install/Creating-and-Customizing-the-Image.md)   
 [Ulteriori personalizzazioni](../install/Additional-Customizations.md)   
 [Preparazione dell'immagine per la distribuzione](../install/Preparing-the-Image-for-Deployment.md)   
 [Test di analisi utilizzo software](../install/Testing-the-Customer-Experience.md)

