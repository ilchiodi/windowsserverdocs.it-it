---
title: Riprodurre file multimediali digitali in Windows Server Essentials
description: Viene descritto come utilizzare Windows Server Essentials
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 5f570492-ee21-471b-92c1-3fd9bfb84f55
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 8228d0b17a58858ed893181ddceb465715ffdeb5
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59874102"
---
# <a name="play-digital-media-in-windows-server-essentials"></a>Riprodurre file multimediali digitali in Windows Server Essentials

>Si applica a: Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

I file multimediali digitali sono contenuti audio, video e foto compressi in modo digitale.  Windows Server Essentials permette ai computer in rete e alcuni dispositivi multimediali digitali di riprodurre file multimediali digitali archiviati sul server collegati alla rete.  
  
 Gli argomenti seguenti forniscono informazioni sull'accesso e la riproduzione di file multimediali digitali archiviati in Windows Server Essentials:  
  

-   [Panoramica di file multimediali digitali](Play-Digital-Media-in-Windows-Server-Essentials.md#BKMK_1)  
  
-   [Riprodurre e condividere file multimediali digitali](Play-Digital-Media-in-Windows-Server-Essentials.md#BKMK_2)  
  
-   [Riprodurre file multimediali digitali condivisi da una posizione remota](Play-Digital-Media-in-Windows-Server-Essentials.md#BKMK_3)  
  
-   [Aggiungere file multimediali digitali al server](Play-Digital-Media-in-Windows-Server-Essentials.md#BKMK_4)  
  
-   [Opzioni di formato di download](Play-Digital-Media-in-Windows-Server-Essentials.md#BKMK_5)  
  
-   [Strumento caricamento semplice File](Play-Digital-Media-in-Windows-Server-Essentials.md#BKMK_6)  
  
-   [Visualizzare ed esplorare i file multimediali digitali condivisi](Play-Digital-Media-in-Windows-Server-Essentials.md#BKMK_7)  

-   [Panoramica di file multimediali digitali](../use/Play-Digital-Media-in-Windows-Server-Essentials.md#BKMK_1)  
  
-   [Riprodurre e condividere file multimediali digitali](../use/Play-Digital-Media-in-Windows-Server-Essentials.md#BKMK_2)  
  
-   [Riprodurre file multimediali digitali condivisi da una posizione remota](../use/Play-Digital-Media-in-Windows-Server-Essentials.md#BKMK_3)  
  
-   [Aggiungere file multimediali digitali al server](../use/Play-Digital-Media-in-Windows-Server-Essentials.md#BKMK_4)  
  
-   [Opzioni di formato di download](../use/Play-Digital-Media-in-Windows-Server-Essentials.md#BKMK_5)  
  
-   [Strumento caricamento semplice File](../use/Play-Digital-Media-in-Windows-Server-Essentials.md#BKMK_6)  
  
-   [Visualizzare ed esplorare i file multimediali digitali condivisi](../use/Play-Digital-Media-in-Windows-Server-Essentials.md#BKMK_7)  

  
##  <a name="BKMK_1"></a> Panoramica di file multimediali digitali  
 I file multimediali digitali sono contenuti audio, video e foto codificati, ovvero compressi in modo digitale. La codifica di contenuto comporta la conversione di input audio e video in un file multimediale digitale, ad esempio un file di Windows Media. Dopo la codifica, un file multimediale digitale potrà essere modificato, distribuito e riprodotto con facilità dai computer e potrà essere trasmesso in modo semplice nelle reti di computer.  
  
 I file multimediali digitali includono, ad esempio, Windows Media Audio (WMA), Windows Media Video (WMV), MP3, JPEG e AVI. Per informazioni sui tipi di file multimediali digitali supportati da Windows Media Player, vedere [Tipi di file supportati da Windows Media Player](https://support.microsoft.com/kb/316992).  
  
### <a name="why-would-i-want-to-stream-my-digital-media"></a>Vantaggi della trasmissione in flussi dei file multimediali digitali  
 Molte persone archiviano musica, video e immagini in cartelle condivise in Windows Server Essentials. È possibile che si vogliano eseguire le operazioni seguenti:  
  
-   **Guardare video**. Il server può essere usato per archiviare e trasmettere grandi raccolte di video e trasmissioni televisive registrate nei computer o in altri dispositivi per la riproduzione disponibili in rete. È possibile trasmettere video a una console Xbox 360 o a un computer usando Windows Media Player.  
  
-   **Riprodurre musica**. Quando si attiva la Condivisione file multimediali per la cartella condivisa **Musica**, sarà possibile accedere alla musica dai dispositivi che supportano Windows Media Connect. Dopo l'attivazione della condivisione, non è necessario abilitare o configurare account utente per la trasmissione di flussi dalla cartella condivisa **Musica** .  
  
-   **Eseguire presentazioni di fotografie**. È possibile archiviare le foto digitali nella cartella condivisa **Foto** nel server, quindi accedervi da qualsiasi computer o da una console Xbox 360 connessa a un televisore in casa o in ufficio. È possibile usare il televisore come se fosse una grande cornice per guardare presentazioni fotografiche.  
  
### <a name="sharing-copy-protected-media"></a>Condivisione di file multimediali protetti da copia  
  Windows Server Essentials non supporta la condivisione file multimediali protetti da copia. inclusi i file musicali acquistati tramite un negozio online.  
  
 I file multimediali protetti da copia possono essere riprodotti solo nel computer o nel dispositivo usato per acquistarli. La protezione contro la copia impedisce la riproduzione di file multimediali in più computer o dispositivi, anche se si copia il file multimediale sul server e lo si riproduce da questa posizione. Tuttavia, è possibile archiviare i file multimediali protetti da copia, in Windows Server Essentials e continuare a riprodurlo nel computer o dispositivo che è usato per acquistarlo.  
  
##  <a name="BKMK_2"></a> Riprodurre e condividere file multimediali digitali  
 Dopo avere configurato la rete e avere connesso correttamente i computer e i dispositivi multimediali alla rete di server, è possibile cercare eventuali file multimediali digitali archiviati e condivisi sul server.  
  
> [!NOTE]
>  Per un elenco aggiornato di dispositivi di riproduzione/ricezione multimediali digitali compatibili con Windows Server Essentials, vedere la [Windows Compatibility Center](https://www.microsoft.com/windows/compatibility/CompatCenter/Home).  
  
 È possibile usare uno dei modi seguenti per cercare e riprodurre file multimediali digitali archiviati sul server:  
  

-   [Cercare e riprodurre file multimediali in Windows Server Essentials da un computer o un lettore multimediale digitale in rete](Play-Digital-Media-in-Windows-Server-Essentials.md#BKMK_2.1)  
  
-   [Inviare file multimediali in Windows Server Essentials a Windows Media Player, Xbox 360 o a un lettore multimediale digitale in rete](Play-Digital-Media-in-Windows-Server-Essentials.md#BKMK_SendToDevice)  

-   [Cercare e riprodurre file multimediali in Windows Server Essentials da un computer o un lettore multimediale digitale in rete](../use/Play-Digital-Media-in-Windows-Server-Essentials.md#BKMK_2.1)  
  
-   [Inviare file multimediali in Windows Server Essentials a Windows Media Player, Xbox 360 o a un lettore multimediale digitale in rete](../use/Play-Digital-Media-in-Windows-Server-Essentials.md#BKMK_SendToDevice)  

  
###  <a name="BKMK_2.1"></a> Cercare e riprodurre file multimediali in Windows Server Essentials da un computer o un lettore multimediale digitale in rete  
 Quando il dispositivo alla rete di Windows Server Essentials, è possibile cercare e riprodurre file multimediali digitali in uno dei modi seguenti:  
  

-   [Cercare e riprodurre file multimediali da un computer in cui è in esecuzione Windows Media Center](Play-Digital-Media-in-Windows-Server-Essentials.md#BKMK_WMC)  
  
-   [Cercare e riprodurre file multimediali da un computer che esegue Windows usando Windows Media Player](Play-Digital-Media-in-Windows-Server-Essentials.md#BKMK_MWP)  
  
-   [Cercare e riprodurre file multimediali tramite Xbox 360](Play-Digital-Media-in-Windows-Server-Essentials.md#BKMK_Xbox)  
  
-   [Cercare e riprodurre file multimediali tramite altri lettori multimediali digitali o i ricevitori che sono compatibili con Windows Server Essentials](Play-Digital-Media-in-Windows-Server-Essentials.md#BKMK_Other)  
  
-   [Cercare e riprodurre file multimediali usando la funzionalità cartelle condivise della finestra di avvio](Play-Digital-Media-in-Windows-Server-Essentials.md#BKMK_SharedFolders)  
  
-   [Cercare e riprodurre file multimediali condivisi usando accesso Web remoto](Play-Digital-Media-in-Windows-Server-Essentials.md#BKMK_RWA2)  

-   [Cercare e riprodurre file multimediali da un computer in cui è in esecuzione Windows Media Center](../use/Play-Digital-Media-in-Windows-Server-Essentials.md#BKMK_WMC)  
  
-   [Cercare e riprodurre file multimediali da un computer che esegue Windows usando Windows Media Player](../use/Play-Digital-Media-in-Windows-Server-Essentials.md#BKMK_MWP)  
  
-   [Cercare e riprodurre file multimediali tramite Xbox 360](../use/Play-Digital-Media-in-Windows-Server-Essentials.md#BKMK_Xbox)  
  
-   [Cercare e riprodurre file multimediali tramite altri lettori multimediali digitali o i ricevitori che sono compatibili con Windows Server Essentials](../use/Play-Digital-Media-in-Windows-Server-Essentials.md#BKMK_Other)  
  
-   [Cercare e riprodurre file multimediali usando la funzionalità cartelle condivise della finestra di avvio](../use/Play-Digital-Media-in-Windows-Server-Essentials.md#BKMK_SharedFolders)  
  
-   [Cercare e riprodurre file multimediali condivisi usando accesso Web remoto](../use/Play-Digital-Media-in-Windows-Server-Essentials.md#BKMK_RWA2)  

  
####  <a name="BKMK_WMC"></a> Cercare e riprodurre file multimediali da un computer in cui è in esecuzione Windows Media Center  
  
1.  Fare clic su **Start**, scegliere **Tutti i programmi**, quindi fare clic su **Windows Media Center**.  
  
2.  Nella pagina **Windows Media Center** scorrere per visualizzare il tipo di file multimediali da usare, quindi fare clic sul Catalogo multimediale.  
  
3.  Cercare manualmente il file a cui si è interessati oppure fare clic su **Cerca** e digitare il nome del file da trovare.  
  
4.  Fare clic sull'immagine del file multimediale da visualizzare o riprodurre il file.  
  
####  <a name="BKMK_MWP"></a> Cercare e riprodurre file multimediali da un computer che esegue Windows usando Windows Media Player  
  
-   Nel computer o nel dispositivo multimediale aprire **Windows Media Player** e cercare il Catalogo multimediale.  
  
    > [!NOTE]
    >  I passaggi per la procedura dipendono dalla versione di Windows Media Player usata. Per informazioni dettagliate, vedere la Guida per la versione specifica.  
  
####  <a name="BKMK_Xbox"></a> Cercare e riprodurre file multimediali tramite Xbox 360  
  
1.  Connettere la console Xbox 360 alla rete domestica usando un connessione cablata o wireless.  
  
2.  Nel computer che esegue Windows Server Essentials, attivare la condivisione di file multimediali. Per altre informazioni, vedere l'argomento [attivare o disattivare i flussi multimediali](../manage/Manage-Digital-Media-in-Windows-Server-Essentials.md#BKMK_4).  
  
3.  Per riprodurre i file multimediali digitali usando la console Xbox 360:  
  
    1.  Passare a **Xbox personale**, quindi selezionare **Catalogo video**, **Catalogo musicale**o **Raccolta immagini**, in base al tipo di file multimediale da visualizzare o riprodurre.  
  
    2.  Selezionare il nome del server.  
  
        > [!NOTE]
        >  Se il nome del server non è incluso nell'elenco, selezionare **Computer**, quindi fare clic su **Connessione di prova**.  
  
    3.  Selezionare l'elemento da riprodurre nell'elenco di file.  
  
####  <a name="BKMK_Other"></a> Cercare e riprodurre file multimediali tramite altri lettori multimediali digitali o i ricevitori che sono compatibili con Windows Server Essentials  
  
1.  Passare al [Windows Compatibility Center](https://www.microsoft.com/windows/compatibility/CompatCenter/Home) e assicurarsi che il lettore multimediale digitale o il dispositivo di ricezione siano disponibili nell'elenco di dispositivi compatibili.  
  
2.  Poiché i passaggi della ricerca dipendono dal lettore multimediale digitale usato, per istruzioni dettagliate vedere la Guida relativa al dispositivo specifico.  
  
####  <a name="BKMK_SharedFolders"></a> Cercare e riprodurre file multimediali usando la funzionalità cartelle condivise della finestra di avvio  
  
1.  Accedi per la finestra di avvio di Windows Server Essentials.  
  
2.  Nella finestra di avvio fare clic su **Cartelle condivise**. Sarà visualizzata una finestra di Esplora risorse, che include le cartelle condivise disponibili sul server.  
  
3.  Nella casella **Cerca** digitare il nome di un file multimediale. Saranno visualizzati i risultati della query di ricerca.  
  
    > [!NOTE]
    >  È anche possibile fare doppio clic su una cartella condivisa per visualizzarne i contenuti.  
  
####  <a name="BKMK_RWA2"></a> Cercare e riprodurre file multimediali condivisi usando accesso Web remoto  
  
1.  Accedere ad Accesso Web remoto.  
  
2.  Fare clic su **Cartelle condivise**. Nella sezione **Cartelle condivise** della pagina Web è visualizzato un elenco di cartelle condivise disponibili sul server.  
  
3.  Fare doppio clic su una cartella per visualizzarne il contenuto.  
  
###  <a name="BKMK_SendToDevice"></a> Inviare file multimediali in Windows Server Essentials a Windows Media Player, Xbox 360 o a un lettore multimediale digitale in rete  
 Usare **Windows Media Player** per cercare il file multimediale da usare. Fare clic con il pulsante destro del mouse sul file multimediale, quindi scegliere **Riproduci in** per inviare il file multimediale a un dispositivo multimediale in rete.  
  
##  <a name="BKMK_3"></a> Riprodurre file multimediali digitali condivisi da una posizione remota  
 È possibile riprodurre file multimediali quando si è lontani dalla rete di Windows Server Essentials tramite accesso Web remoto. Per cercare e riprodurre i file multimediali condivisi archiviati sul server, è possibile usare un telefono cellulare oppure un lettore multimediale digitale.  
  
#### <a name="to-play-shared-media-files-when-you-are-away-from-the-network"></a>Per riprodurre file multimediali digitali quando si è lontani dalla rete  
  
1.  Aprire un browser Internet.  
  
2.  Passare al sito Web di Accesso Web remoto. Tipo di **https://<YourDomainName\>/remote** nella barra degli indirizzi del browser Internet e quindi premere INVIO.  
  
    > [!NOTE]
    >  *< NomeDominio\>*  è un segnaposto. Sarà un nome univoco per il server, in modo che l'indirizzo digitato sarà simile **https://contoso.com/remote**. Se non si conosce il nome del dominio, rivolgersi all'amministratore che ha scelto il nome di dominio quando la funzionalità Accesso Web remoto è stata configurata sul server. Per altre informazioni, vedere [Turn on Remote Web Access](../manage/Manage-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_TurnOnRWA).  
  
3.  Nella pagina di accesso di Accesso Web remoto digitare il nome account e la password, quindi fare clic sulla freccia.  
  
4.  Usare il metodo preferito per cercare il file multimediale da riprodurre.  
  
    > [!NOTE]

    >  Per informazioni sui vari metodi di ricerca, vedere [cercare e riprodurre file multimediali in Windows Server Essentials da un computer o un lettore multimediale digitale in rete](Play-Digital-Media-in-Windows-Server-Essentials.md#BKMK_2.1).  

    >  Per informazioni sui vari metodi di ricerca, vedere [cercare e riprodurre file multimediali in Windows Server Essentials da un computer o un lettore multimediale digitale in rete](../use/Play-Digital-Media-in-Windows-Server-Essentials.md#BKMK_2.1).  

  
5.  Quando è visualizzato il nome del file multimediale, fare clic sul nome per riprodurlo.  
  
##  <a name="BKMK_4"></a> Aggiungere file multimediali digitali al server  

 L'amministratore del server è possibile aggiungere file multimediali digitali alle cartelle condivise nel catalogo multimediale tramite l'accesso diretto al server, oppure usando il sito accesso Web remoto per accedere al Dashboard. Altri utenti possono aggiungere file multimediali al server usando il **cartelle condivise** connessione nella finestra di avvio, tramite il sito accesso Web remoto o tramite l'app My Server per Windows Phone. Per informazioni sulla riproduzione di file multimediali, vedere [Riprodurre e condividere file multimediali digitali](Play-Digital-Media-in-Windows-Server-Essentials.md#BKMK_2).  

 L'amministratore del server è possibile aggiungere file multimediali digitali alle cartelle condivise nel catalogo multimediale tramite l'accesso diretto al server, oppure usando il sito accesso Web remoto per accedere al Dashboard. Altri utenti possono aggiungere file multimediali al server usando il **cartelle condivise** connessione nella finestra di avvio, tramite il sito accesso Web remoto o tramite l'app My Server per Windows Phone. Per informazioni sulla riproduzione di file multimediali, vedere [Riprodurre e condividere file multimediali digitali](../use/Play-Digital-Media-in-Windows-Server-Essentials.md#BKMK_2).  

  
> [!NOTE]
>  È anche possibile caricare file multimediali sul server usando l'app My Server per Windows Phone, L'app My Server è disponibile per il download da [Windows Phone Store](http://www.windowsphone.com/store/app/my-server/6c2f98d5-6fcf-4e1d-b8b1-cde62ea1a94a). Per altre informazioni sull'app My Server per Windows Phone, vedere il post di blog [app My Server per Windows Server Essentials](http://blogs.technet.com/b/sbs/archive/2012/09/18/my-server-phone-app-for-windows-server-2012-essentials.aspx).  
  
#### <a name="to-add-digital-media-files-to-shared-folders-on-the-server"></a>Per aggiungere file multimediali alle cartelle condivise sul server  
  
1.  Usare uno dei metodi seguenti per accedere al server:  
  

    1.  Per informazioni sull'accesso ad accesso Web remoto, vedere [Use Remote Web Access](Use-Remote-Web-Access-in-Windows-Server-Essentials.md).  

    1.  Per informazioni sull'accesso ad accesso Web remoto, vedere [Use Remote Web Access](../use/Use-Remote-Web-Access-in-Windows-Server-Essentials.md).  

  
    2.  Per informazioni sull'accesso con la finestra di avvio, vedere [Panoramica di Launchpad](../manage/Overview-of-the-Launchpad-in-Windows-Server-Essentials.md).  
  
2.  Cercare e selezionare la cartella dei file multimediali da aggiungere.  
  
3.  Copiare e incollare oppure trascinare e rilasciare i file multimediali da aggiungere nella cartella condivisa appropriata sul server.  
  
##  <a name="BKMK_5"></a> Opzioni di formato di download  
 Sono disponibili due opzioni per il download di file. Queste opzioni sono disponibili solo quando si scaricano più file o una cartella in un computer basato su Windows.  
  
 Scegliere l'opzione seguente più adatta alle proprie esigenze per i download:  
  
-   **File ZIP compresso (zip)**  
  
     La compressione di un file permette di creare una versione compressa del file, con dimensioni inferiori a quelle del file originale. La versione compressa del file avrà un nome file con estensione zip. I tipi di file per cui si ottiene la compressione maggiore sono i file orientati al testo, ad esempio i file con estensione txt, doc e xls, e i file di immagine che usano tipi di file non compressi, ad esempio i file con estensione bmp. Alcuni file di immagine, ad esempio i file con estensione jpg e gif, usano già la compressione, quindi la compressione permetterà solo una riduzione minima delle dimensioni dei file. Se si comprime un documento di Word contenente un numero elevato di elementi grafici si otterrà una riduzione minore rispetto alla compressione di un documento che include principalmente testo.  
  
    > [!NOTE]
    >  Questa opzione offre supporto limitato per nomi di file internazionali.  
  
-   **File eseguibile autoestraente (.exe)**  
  
     Un file eseguibile autoestraente è un file scaricabile che unisce il programma di decompressione (eseguibile) ai file compressi. Quando lo si esegue, il programma eseguibile decomprime automaticamente i file compressi. Si tratta di una modalità comune per distribuire i dati compressi senza doversi preoccupare della disponibilità o meno dell'utilità di compressione corretta da parte del destinatario.  
  
    > [!NOTE]
    >  Questa opzione supporta i caratteri Unicode.  
  
 Prima dell'inizio del download effettivo, sarà creato il file con estensione exe o zip. In base al numero di file e alla dimensione totale dei file da scaricare, questa operazione potrebbe richiedere alcuni minuti. Dopo la creazione del file di download, i file saranno scaricati in background. Ciò permette di continuare a lavorare durante il completamento del processo di download.  
  
##  <a name="BKMK_6"></a> Strumento caricamento semplice File  
 Lo strumento caricamento semplice File semplifica il processo di caricamento file nel server Windows Server Essentials. È possibile aggiungere il numero di file come si desidera che lo strumento caricamento semplice File e quindi caricarli nelle cartelle condivise nel server di Windows Server Essentials in un unico batch. Per altre informazioni, vedere il post di blog sulla [Comprensione della condivisione di file in Accesso Web remoto](http://blogs.technet.com/b/sbs/archive/2012/04/19/understanding-remote-web-access-file-sharing.aspx).  
  
##  <a name="BKMK_7"></a> Visualizzare ed esplorare i file multimediali digitali condivisi  
 È possibile visualizzare o selezionare le risorse tramite il dashboard, la finestra di avvio, il sito Web di Accesso Web remoto o l'app My Server per Windows Phone.  
  
#### <a name="to-view-and-browse-shared-media-from-the-dashboard"></a>Per visualizzare e selezionare file multimediali condivisi dal dashboard  
  
1.  Aprire il dashboard del server.  
  
2.  Sulla barra di spostamento principale fare clic su **ARCHIVIAZIONE**.  
  
3.  Fare clic sulla scheda **Cartelle server** . Sarà visualizzato un elenco di cartelle.  
  
4.  Fare doppio clic su una cartella per visualizzarne il contenuto.  
  
#### <a name="to-view-and-browse-shared-media-using-the-launchpad-from-a-network-computer"></a>Per visualizzare e selezionare i file multimediali condivisi usando la finestra di avvio da un computer in rete  
  
1.  Accedere alla finestra di avvio.  
  
2.  Fare clic su **Cartelle condivise**. Sarà visualizzata una finestra di Esplora risorse, che include un elenco delle cartelle condivise disponibili sul server.  
  
3.  Fare doppio clic su una cartella per visualizzarne il contenuto.  
  
#### <a name="to-view-and-browse-shared-media-using-remote-web-access"></a>Per visualizzare e selezionare file multimediali condivisi usando Accesso Web remoto  
  
1.  Accedere ad Accesso Web remoto.  
  
2.  Fare clic su **Cartelle condivise**. Nella sezione **Cartelle condivise** della pagina Web è visualizzato un elenco di cartelle condivise disponibili sul server.  
  
3.  Fare doppio clic su una cartella per visualizzarne il contenuto.  
  
## <a name="see-also"></a>Vedere anche  
  
-   [Gestire file multimediali digitali](../manage/Manage-Digital-Media-in-Windows-Server-Essentials.md)  
  

-   [Connettiti](Get-Connected-in-Windows-Server-Essentials.md)  
  
-   [Usare le cartelle condivise](Use-Shared-Folders-in-Windows-Server-Essentials.md)  
  
-   [Lavorare in modalità remota](Work-Remotely-in-Windows-Server-Essentials.md)

-   [Connettiti](../use/Get-Connected-in-Windows-Server-Essentials.md)  
  
-   [Usare le cartelle condivise](../use/Use-Shared-Folders-in-Windows-Server-Essentials.md)  
  
-   [Lavorare in modalità remota](../use/Work-Remotely-in-Windows-Server-Essentials.md)

