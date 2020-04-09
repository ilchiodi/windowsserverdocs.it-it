---
title: Aggiunta di personalizzazione alla dashboard, ad Accesso Web remoto e alla Finestra di avvio
description: Viene descritto come usare Windows Server Essentials
ms.date: 04/10/2014
ms.prod: windows-server
ms.topic: article
ms.assetid: 166262f8-b2a5-4b1c-a4a7-a141e1c54f10
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 50f132d7f6422c32b2a72948ca96b5bd82e701df
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80817744"
---
# <a name="add-branding-to-the-dashboard-remote-web-access-and-launchpad"></a>Aggiunta di personalizzazione alla dashboard, ad Accesso Web remoto e alla Finestra di avvio

>Si applica a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

##  <a name="add-branding-to-the-dashboard-remote-web-access-and-launchpad"></a><a name="BKMK_Branding"></a>Aggiungere la personalizzazione al dashboard, al Accesso Web remoto e alla finestra di avvio  
 È possibile aggiungere svariate personalizzazioni mediante aggiunta di voci al Registro di sistema. Tutte le voci di personalizzazione nel Registro di sistema per il sistema operativo si trovano in HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Server\OEM.  
  
 Il co-branding deve soddisfare i requisiti del logo seguenti:  
  
-   Il logo di Windows Server Essentials deve avere una larghezza minima di **170 pixel**, mantenendo le proporzioni corrette, con **96 dpi**.  
  
#### <a name="to-add-branding-by-changing-the-registry"></a>Per aggiungere la personalizzazione mediante modifica del Registro di sistema  
  
1.  Sul server, spostare il puntatore del mouse verso l'angolo superiore destro dello schermo e fare clic su **Trova**.  
  
2.  Nella casella di ricerca digitare **regedit** e quindi fare clic sull'applicazione **Regedit**.  
  
3.  Nel riquadro di navigazione, espandere **HKEY_LOCAL_MACHINE**, espandere **SOFTWARE**, espandere **Microsoft**, espandere **Windows Server**. Se la chiave **OEM** non è presente, completare i seguenti passaggi per crearla:  
  
    1.  Fare clic con il pulsante destro del mouse su **Windows Server**, fare clic su **Nuovo**, quindi fare clic su **Chiave**.  
  
    2.  Digitare **OEM** come nome della chiave.  
  
4.  (Facoltativo) Se si sta creando una voce per un logo, è possibile creare diverse chiavi per distinguere le versioni in lingue diverse del logo. Ad esempio, se si dispone sia della versione in inglese che della versione in tedesco di un logo, è possibile creare una chiave en-us e una chiave de-de. Dal momento che tutti i file del logo sono archiviati nella stessa cartella, è necessario fornire alle istanze del file di immagine logo un nome univoco per ogni lingua. Ad esempio, creare un file denominato DashboardLogo_en.png e uno denominato DashboardLogo_de.png.  
  
5.  Fare clic con il tasto destro del mouse su **OEM** o sulla chiave della lingua appropriata, quindi selezionare **Nuovo** seguito da **Valore stringa**.  
  

6.  Immettere il nome della stringa e quindi premere INVIO. Per i valori dati e i nomi stringhe, consultare la tabella [Valori e stringhe del Registro di sistema](Add-Branding-to-the-Dashboard--Remote-Web-Access--and-Launchpad.md#BKMK_RegStrings).  

6.  Immettere il nome della stringa e quindi premere INVIO. Per i valori dati e i nomi stringhe, consultare la tabella [Valori e stringhe del Registro di sistema](../install/Add-Branding-to-the-Dashboard--Remote-Web-Access--and-Launchpad.md#BKMK_RegStrings).  

  
7.  Fare clic con il tasto destro del mouse sulla stringa nuova, quindi selezionare **Modifica**.  
  
8.  Immettere il valore dalla tabella associata al nome della stringa, quindi fare clic su **OK**.  
  
9. Se si sta creando una voce per un'immagine logo o per collegamenti aggiunti, copiare il file in %programFiles%\Windows Server\Bin\OEM. Se la directory OEM non esiste, crearla.  
  
10. Se si apportano modifiche che influiscono sull’accesso Web remoto, una volta che l’utente prende possesso del server deve attivare Accesso Web remoto. Consigliare al cliente di effettuare le seguenti operazioni:  
  
    1.  Sul dashboard, fare clic su **Impostazioni** e quindi sulla scheda **Accesso remoto via Internet**.  
  
    2.  Se Accesso remoto via Internet è attivato, fare clic su **Configura** e quindi deselezionare la casella Accesso Web remoto sulla pagina **Scegli le funzionalità di Accesso remoto via Internet da abilitare** della Configurazione guidata di Accesso remoto via Internet.  
  
    3.  Fare clic su **Configura**.  
  
###  <a name="the-following-table-lists-the-location-where-registry-changes-affect-branding-the-string-name-and-the-data-value"></a><a name="BKMK_RegStrings"></a>La tabella seguente elenca il percorso in cui le modifiche al registro di sistema influiscono sulla personalizzazione, sul nome della stringa e sul valore dei dati.  
  
### <a name="registry-strings-and-values"></a>Valori e stringhe del Registro di sistema  
  
|Posizione della personalizzazione|Descrizione|Nome della stringa|Valore dei dati|  
|--------------------------|-----------------|-----------------|----------------|  
|Logo del dashboard|Aggiunge l’immagine del logo al dashboard. Il logo della dashboard deve essere in formato .png e non deve superare i 350 pixel di larghezza e gli 38 pixel di altezza.<br /><br /> **Importante:** Per personalizzare il dashboard con il logo, è necessario modificare il riquadro artwork fornito nel DVD OPK e aggiungere il logo della società all'immagine seguendo i requisiti di spazio vuoto appropriati. Per ulteriori informazioni, vedere l’immagine di esempio fornita.|DashboardLogo|Nome del file di immagine del logo|  
|DashboardClientLogo|Aggiunge l'immagine del logo alla schermata di accesso del client del dashboard.|DashboardClientLogo|Nome del file di immagine del logo|  
|Immagine di sfondo del sito Web|Consente di cambiare l’immagine di sfondo visualizzata nella pagina di accesso del servizio Accesso Web remoto. Le risoluzioni tipiche appariranno come segue:<br /><br /> -la risoluzione del pixel 1024x768 riempirà con precisione la pagina di accesso<br /><br /> -la risoluzione di 800x600 pixel verrà centrata sulla pagina e visualizzata con un bordo nero<br /><br /> -la risoluzione di 1280x720 pixel verrà centrata e i pixel che superano 1024x768 non verranno visualizzati|LogonBackground|Nome del file dell’immagine di sfondo|  
|Titolo del sito Web|Sostituisce il titolo del sito Accesso Web remoto da Windows Server Essentials a un titolo scelto.|WebsiteName|Nuovo titolo del sito di Accesso Web remoto|  
|Logo del sito Web|Consente di cambiare il logo predefinito nel sito di Accesso Web remoto. Le dimensioni previste del logo sono 32 pixel per 32 pixel. Se il logo personale è più grande o più piccolo, verrà allungato o ridotto per adattarlo a queste dimensioni|WebsiteLogo|Nome del file di immagine del logo|  
|Logo del sito Web aggiunto|Il logo del partner verrà visualizzato proprio sotto il logo Microsoft visualizzato nel sito di Accesso Web remoto. Le dimensioni previste del logo sono 200 pixel in altezza per 50 pixel in larghezza. Se le dimensioni sono superiori, il logo verrà ridotto per adattarlo ai valori previsti mantenendo al contempo le proporzioni originali. Se le dimensioni sono inferiori, il logo verrà centrato nello spazio di 200 per 50 pixel e non verrà apportata alcuna modifica alle dimensioni o alle proporzioni.|OEMLogo|Nome del file di immagine del logo|  

| Collegamenti nel home page e nella pagina di accesso del sito Web | Aggiungere collegamenti alla pagina di accesso e home page del sito di Accesso Web remoto. Il file .xml contenente le informazioni sui collegamenti deve trovarsi in %programFiles%\Windows Server\Bin\OEM. Nell’esempio seguente viene mostrato il formato del file .xml:<br /><br /> < OemLinks\><br /> < LogonLinks\><br /> Nome del collegamento <\=LogonLinkName ><br /> Testo <\>LogonLinkDescription </text\><br /> < URL\>LogonLinkURL </URL\><br /> Icona <\>LinkIcon </icona\><br /> </link\><br /> </LogonLinks\><br /> < HomepageLinks\><br /> Nome del collegamento <\=HomepageLinkName ><br /> Testo <\>HomepageLinkDescription </text\><br /> < URL\>HomepageLinkURL </URL\><br /> </link\><br /> </HomepageLinks\><br /> \>/OemLinks < | LinksXML | Per un elenco degli elementi e delle descrizioni, vedere la tabella [elementi LinksXML](Add-Branding-to-the-Dashboard--Remote-Web-Access--and-Launchpad.md#BKMK_Links) . |  

| Collegamenti nel home page e nella pagina di accesso del sito Web | Aggiungere collegamenti alla pagina di accesso e home page del sito di Accesso Web remoto. Il file .xml contenente le informazioni sui collegamenti deve trovarsi in %programFiles%\Windows Server\Bin\OEM. Nell’esempio seguente viene mostrato il formato del file .xml:<br /><br /> < OemLinks\><br /> < LogonLinks\><br /> Nome del collegamento <\=LogonLinkName ><br /> Testo <\>LogonLinkDescription </text\><br /> < URL\>LogonLinkURL </URL\><br /> Icona <\>LinkIcon </icona\><br /> </link\><br /> </LogonLinks\><br /> < HomepageLinks\><br /> Nome del collegamento <\=HomepageLinkName ><br /> Testo <\>HomepageLinkDescription </text\><br /> < URL\>HomepageLinkURL </URL\><br /> </link\><br /> </HomepageLinks\><br /> \>/OemLinks < | LinksXML | Per un elenco degli elementi e delle descrizioni, vedere la tabella [elementi LinksXML](../install/Add-Branding-to-the-Dashboard--Remote-Web-Access--and-Launchpad.md#BKMK_Links) . |  

| Logo Launchpad | Aggiunge l'immagine del logo alla finestra di avvio. Il logo Launchpad deve essere in formato PNG e non deve avere una altezza superiore a 64 pixel. | LaunchpadLogo | Nome del file di immagine del logo |  
  
###  <a name="the-following-table-lists-and-describes-the-linksxml-string-name-elements"></a><a name="BKMK_Links"></a>La tabella seguente elenca e descrive gli elementi del nome di stringa LinksXML.  
  
### <a name="linksxml-elements"></a>Elementi LinksXML  
  
|Elemento LinksXML|Descrizione|  
|----------------------|-----------------|  
|**LogonLinks**|  
|LogonLinkName|Nome del collegamento di accesso.|  
|LogonLinkDescription|Testo visualizzato come collegamento alla pagina di accesso.|  
|LogonLinkURL|URL che consente di risolvere il collegamento della pagina di accesso.|  
|LinkIcon|Nome del file icona per il collegamento di accesso. Questo file deve trovarsi nella stessa cartella del file XML. Le immagini icona di collegamento devono avere dimensioni pari a 16x16 pixel ed essere in formato .png. Se non viene fornito un valore LinkIcon, viene utilizzata l’immagine icona predefinita del collegamento.|  
|**HomepageLinks**|  
|HomepageLinkName|Nome del collegamento alla home page.|  
|HomepageLinkDescription|Il testo visualizzato come collegamento alla home page.|  
|HomepageLinkURL|URL che consente di risolvere il collegamento alla home page.|  
|HomepageLinkIcon|Nome del file icona per il collegamento alla home page. Questo file deve trovarsi nella stessa cartella del file XML. Le immagini HomepageLinkIcon devono avere dimensioni pari a 16x16 pixel ed essere in formato .png. Se non viene fornito un valore HomepageLinkIcon, viene utilizzata l'immagine icona del collegamento alla home page predefinita.|  
  
## <a name="see-also"></a>Vedi anche  

 [Creazione e personalizzazione dell'immagine](Creating-and-Customizing-the-Image.md)   
 [Personalizzazioni aggiuntive](Additional-Customizations.md)   
 [Preparazione dell'immagine per la distribuzione](Preparing-the-Image-for-Deployment.md)   
 [Test di Analisi utilizzo software](Testing-the-Customer-Experience.md)

 [Creazione e personalizzazione dell'immagine](../install/Creating-and-Customizing-the-Image.md)   
 [Personalizzazioni aggiuntive](../install/Additional-Customizations.md)   
 [Preparazione dell'immagine per la distribuzione](../install/Preparing-the-Image-for-Deployment.md)   
 [Test di Analisi utilizzo software](../install/Testing-the-Customer-Experience.md)

