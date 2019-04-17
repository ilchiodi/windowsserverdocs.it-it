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
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/12/2017
---
# <a name="play-digital-media-in-windows-server-essentials"></a>Riprodurre file multimediali digitali in Windows Server Essentials

>Si applica a: Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

File multimediali digitali fa riferimento a audio, video e contenuto foto compressi in modo digitale.  Windows Server Essentials permette ai computer in rete e alcuni dispositivi multimediali digitali per riprodurre file multimediali digitali archiviati nel server in rete.  
  
 Gli argomenti seguenti forniscono informazioni sull'accesso e la riproduzione di file multimediali digitali archiviati in Windows Server Essentials:  
  

-   [Panoramica di file multimediali digitali](Play-Digital-Media-in-Windows-Server-Essentials.md#BKMK_1)  
  
-   [Riprodurre e condividere file multimediali digitali](Play-Digital-Media-in-Windows-Server-Essentials.md#BKMK_2)  
  
-   [Riprodurre file multimediali digitali condivisi da una posizione remota](Play-Digital-Media-in-Windows-Server-Essentials.md#BKMK_3)  
  
-   [Aggiungere file multimediali digitali al server](Play-Digital-Media-in-Windows-Server-Essentials.md#BKMK_4)  
  
-   [Opzioni di formato di download](Play-Digital-Media-in-Windows-Server-Essentials.md#BKMK_5)  
  
-   [Strumento caricamento semplice File](Play-Digital-Media-in-Windows-Server-Essentials.md#BKMK_6)  
  
-   [Visualizzare e selezionare file multimediali digitali condivisi](Play-Digital-Media-in-Windows-Server-Essentials.md#BKMK_7)  

-   [Panoramica di file multimediali digitali](../use/Play-Digital-Media-in-Windows-Server-Essentials.md#BKMK_1)  
  
-   [Riprodurre e condividere file multimediali digitali](../use/Play-Digital-Media-in-Windows-Server-Essentials.md#BKMK_2)  
  
-   [Riprodurre file multimediali digitali condivisi da una posizione remota](../use/Play-Digital-Media-in-Windows-Server-Essentials.md#BKMK_3)  
  
-   [Aggiungere file multimediali digitali al server](../use/Play-Digital-Media-in-Windows-Server-Essentials.md#BKMK_4)  
  
-   [Opzioni di formato di download](../use/Play-Digital-Media-in-Windows-Server-Essentials.md#BKMK_5)  
  
-   [Strumento caricamento semplice File](../use/Play-Digital-Media-in-Windows-Server-Essentials.md#BKMK_6)  
  
-   [Visualizzare e selezionare file multimediali digitali condivisi](../use/Play-Digital-Media-in-Windows-Server-Essentials.md#BKMK_7)  

  
##  <a name="BKMK_1"></a>Panoramica di file multimediali digitali  
 File multimediali digitali fa riferimento a audio, video e foto contenuto che è stato codificato (digitale compresso). La codifica di contenuto comporta la conversione di input audio e video in un file multimediali digitali, ad esempio un file Windows Media. Dopo la codifica file multimediali digitali, può essere facilmente modificato, distribuito e riprodotto dai computer e la trasmissione facilmente nelle reti di computer.  
  
 Esempi di tipi di file multimediali digitali: Windows Media Audio (WMA), Windows Media Video (WMV), MP3, JPEG e AVI. Per informazioni sui tipi di file multimediali digitali supportati da Windows Media Player, vedere [tipi supportati da Windows Media Player di File](https://support.microsoft.com/kb/316992).  
  
### <a name="why-would-i-want-to-stream-my-digital-media"></a>Perché dovrei trasmettere file multimediali digitali?  
 Molte persone archiviano musica, video e immagini in cartelle condivise in Windows Server Essentials. È possibile che quando si desidera eseguire seguenti:  
  
-   **Guarda i video**. Il server può essere utilizzato per archiviare e trasmettere grandi raccolte di video e trasmissioni televisive registrate nei computer o la riproduzione di altri dispositivi nella rete. È possibile trasmettere video a una console Xbox 360 o a un computer con Windows Media Player.  
  
-   **Riprodurre musica**. Quando si attiva la condivisione di file multimediali per la **musica** cartella condivisa, è possibile accedere alla musica dai dispositivi che supportano Windows Media Connect. Non è necessario abilitare o configurare gli account utente di eseguire lo streaming dal **musica** cartella condivisa dopo l'attivazione della condivisione.  
  
-   **Presentare presentazioni di fotografie**. È possibile archiviare le foto digitali nel **foto** condivise nella cartella del server, quindi accedervi da qualsiasi computer o da una console Xbox 360 connessa a un televisore in casa o in ufficio. Consente di guardare presentazioni di fotografie, ovvero il televisore come se in un frame di immagine di grandi dimensioni.  
  
### <a name="sharing-copy-protected-media"></a>Condivisione di file multimediali protetti da copia  
  Windows Server Essentials non supporta la condivisione file multimediali protetti da copia. Inclusi i file musicali acquistati tramite un negozio online.  
  
 Supporto Copy-protected può essere riprodotti solo nel computer o dispositivo usato per acquistarlo. Protezione contro la copia impedisce la riproduzione multimediale in uno o più computer o dispositivo, anche se si copia il supporto per il server e lo si riproduce da lì. Tuttavia, è possibile archiviare il file multimediale protetto da copia in Windows Server Essentials e continuare a riprodurlo nel computer o dispositivo usato per acquistarlo.  
  
##  <a name="BKMK_2"></a>Riprodurre e condividere file multimediali digitali  
 Dopo aver configurato la rete e connettersi computer e dispositivi multimediali alla rete di server, è possibile cercare eventuali file multimediali digitali archiviati e condivisi sul server.  
  
> [!NOTE]
>  Per un elenco aggiornato di dispositivi di riproduzione/ricezione multimediali digitali compatibili con Windows Server Essentials, vedere il [Windows Compatibility Center](https://www.microsoft.com/windows/compatibility/CompatCenter/Home).  
  
 È possibile utilizzare uno dei modi seguenti per cercare e riprodurre file multimediali digitali archiviati sul server:  
  

-   [Cercare e riprodurre file multimediali in Windows Server Essentials da un computer o un lettore multimediale digitale in rete](Play-Digital-Media-in-Windows-Server-Essentials.md#BKMK_2.1)  
  
-   [Inviare file multimediali in Windows Server Essentials a Windows Media Player, Xbox 360 o a un lettore multimediale digitale in rete](Play-Digital-Media-in-Windows-Server-Essentials.md#BKMK_SendToDevice)  

-   [Cercare e riprodurre file multimediali in Windows Server Essentials da un computer o un lettore multimediale digitale in rete](../use/Play-Digital-Media-in-Windows-Server-Essentials.md#BKMK_2.1)  
  
-   [Inviare file multimediali in Windows Server Essentials a Windows Media Player, Xbox 360 o a un lettore multimediale digitale in rete](../use/Play-Digital-Media-in-Windows-Server-Essentials.md#BKMK_SendToDevice)  

  
###  <a name="BKMK_2.1"></a>Cercare e riprodurre file multimediali in Windows Server Essentials da un computer o un lettore multimediale digitale in rete  
 Quando si aggiunge il dispositivo alla rete di Windows Server Essentials, è possibile cercare e riprodurre file multimediali digitali in uno dei modi seguenti:  
  

-   [Cercare e riprodurre file multimediali da un computer che esegue Windows Media Center](Play-Digital-Media-in-Windows-Server-Essentials.md#BKMK_WMC)  
  
-   [Cercare e riprodurre file multimediali da un computer che esegue Windows usando Windows Media Player](Play-Digital-Media-in-Windows-Server-Essentials.md#BKMK_MWP)  
  
-   [Cercare e riprodurre file multimediali tramite Xbox 360](Play-Digital-Media-in-Windows-Server-Essentials.md#BKMK_Xbox)  
  
-   [Cercare e riprodurre file multimediali tramite altri lettori multimediali digitali o i ricevitori compatibili con Windows Server Essentials](Play-Digital-Media-in-Windows-Server-Essentials.md#BKMK_Other)  
  
-   [Cercare e riprodurre file multimediali tramite la funzionalità cartelle condivise della finestra di avvio](Play-Digital-Media-in-Windows-Server-Essentials.md#BKMK_SharedFolders)  
  
-   [Cercare e riprodurre file multimediali condivisi usando accesso Web remoto](Play-Digital-Media-in-Windows-Server-Essentials.md#BKMK_RWA2)  

-   [Cercare e riprodurre file multimediali da un computer che esegue Windows Media Center](../use/Play-Digital-Media-in-Windows-Server-Essentials.md#BKMK_WMC)  
  
-   [Cercare e riprodurre file multimediali da un computer che esegue Windows usando Windows Media Player](../use/Play-Digital-Media-in-Windows-Server-Essentials.md#BKMK_MWP)  
  
-   [Cercare e riprodurre file multimediali tramite Xbox 360](../use/Play-Digital-Media-in-Windows-Server-Essentials.md#BKMK_Xbox)  
  
-   [Cercare e riprodurre file multimediali tramite altri lettori multimediali digitali o i ricevitori compatibili con Windows Server Essentials](../use/Play-Digital-Media-in-Windows-Server-Essentials.md#BKMK_Other)  
  
-   [Cercare e riprodurre file multimediali tramite la funzionalità cartelle condivise della finestra di avvio](../use/Play-Digital-Media-in-Windows-Server-Essentials.md#BKMK_SharedFolders)  
  
-   [Cercare e riprodurre file multimediali condivisi usando accesso Web remoto](../use/Play-Digital-Media-in-Windows-Server-Essentials.md#BKMK_RWA2)  

  
####  <a name="BKMK_WMC"></a>Cercare e riprodurre file multimediali da un computer che esegue Windows Media Center  
  
1.  Fare clic su **Start**, fare clic su **tutti i programmi**, quindi fare clic su **Windows Media Center**.  
  
2.  Nel **Windows Media Center** pagina, scorrere il tipo di supporto che si sta cercando e scegliere il catalogo multimediale.  
  
3.  Cercare manualmente il file si è interessati oppure fare clic su **ricerca** e digitare il nome del file di cui si desidera trovare.  
  
4.  Scegliere l'immagine del file multimediale da visualizzare o riprodurre il file.  
  
####  <a name="BKMK_MWP"></a>Cercare e riprodurre file multimediali da un computer che esegue Windows usando Windows Media Player  
  
-   Il computer o nel dispositivo multimediale aprire **Windows Media Player** e cercare il catalogo multimediale.  
  
    > [!NOTE]
    >  La procedura di ricerca varia a seconda della versione di Windows Media Player in uso. Per informazioni dettagliate, consultare la Guida per la versione.  
  
####  <a name="BKMK_Xbox"></a>Cercare e riprodurre file multimediali tramite Xbox 360  
  
1.  Connettere la console Xbox 360 alla rete domestica usando un connessione cablato o wireless.  
  
2.  Nel computer che esegue Windows Server Essentials, attivare la condivisione di file multimediali. Per ulteriori informazioni, vedere l'argomento [attivare o disattivare i flussi multimediali](../manage/Manage-Digital-Media-in-Windows-Server-Essentials.md#BKMK_4).  
  
3.  Per riprodurre file multimediali digitali usando la console Xbox 360:  
  
    1.  Vai a **Xbox personale**e quindi seleziona **raccolta Video**, **musica**, o **raccolta immagini**, a seconda del tipo di supporto che si desidera visualizzare o riprodurre.  
  
    2.  Selezionare il nome del server.  
  
        > [!NOTE]
        >  Se il nome del server non è elencato, selezionare **Computer**, quindi fare clic su **Test connessione**.  
  
    3.  Scorrere l'elenco di file e selezionare l'elemento che vuoi riprodurre.  
  
####  <a name="BKMK_Other"></a>Cercare e riprodurre file multimediali tramite altri lettori multimediali digitali o i ricevitori compatibili con Windows Server Essentials  
  
1.  Passa alla schermata di [Windows Compatibility Center](https://www.microsoft.com/windows/compatibility/CompatCenter/Home) e assicurarsi che il lettore multimediale digitale o un ricevitore visualizzato nell'elenco dei dispositivi compatibili.  
  
2.  Poiché i passaggi della ricerca dipendono dal lettore multimediale digitale che si sta utilizzando, consultare la Guida per il tuo dispositivo per istruzioni dettagliate.  
  
####  <a name="BKMK_SharedFolders"></a>Cercare e riprodurre file multimediali tramite la funzionalità cartelle condivise della finestra di avvio  
  
1.  Accedi alla finestra di avvio di Windows Server Essentials.  
  
2.  Dalla finestra di avvio, fare clic su **cartelle condivise**. Finestra di Esplora apre e visualizza le cartelle condivise sul server.  
  
3.  Nel **ricerca**, digitare il nome di un file multimediale. Vengono visualizzati i risultati della query di ricerca.  
  
    > [!NOTE]
    >  In alternativa, è anche possibile fare doppio clic su una cartella condivisa per esplorare i contenuti della cartella.  
  
####  <a name="BKMK_RWA2"></a>Cercare e riprodurre file multimediali condivisi usando accesso Web remoto  
  
1.  Accedere ad accesso Web remoto.  
  
2.  Fare clic su **cartelle condivise**. Il **cartelle condivise** sezione della pagina web Visualizza un elenco di cartelle condivise sul server.  
  
3.  Fare doppio clic su una cartella per visualizzarne il contenuto della cartella.  
  
###  <a name="BKMK_SendToDevice"></a>Inviare file multimediali in Windows Server Essentials a Windows Media Player, Xbox 360 o a un lettore multimediale digitale in rete  
 Utilizzare **Windows Media Player** per cercare il file multimediale che vuoi. Fare doppio clic il file multimediale e quindi fare clic su **Riproduci su** per inviare il file multimediale a un dispositivo multimediale in rete.  
  
##  <a name="BKMK_3"></a>Riprodurre file multimediali digitali condivisi da una posizione remota  
 È possibile riprodurre file multimediali quando si è lontani dalla rete di Windows Server Essentials tramite accesso Web remoto. Per cercare e riprodurre i file multimediali condivisi archiviati sul server, è possibile utilizzare un telefono cellulare, un computer remoto o un lettore multimediale digitale.  
  
#### <a name="to-play-shared-media-files-when-you-are-away-from-the-network"></a>Per riprodurre file multimediali condivisi quando si è lontani dalla rete  
  
1.  Aprire un browser Internet.  
  
2.  Vai al sito Web accesso Web remoto. Tipo **https://<YourDomainName\>/remote** nella barra degli indirizzi del browser Internet e quindi premere INVIO.  
  
    > [!NOTE]
    >  *< YourDomainName\ >* è un segnaposto. Sarà un nome univoco per il server, in modo che l'indirizzo digitato sarà simile a **https://contoso.com/remote**. Se non si conosce il nome del dominio, chiedere all'amministratore che ha scelto il nome di dominio quando la funzionalità di accesso remoto è stata impostata sul server. Per ulteriori informazioni, vedere [attivare accesso Web remoto](../manage/Manage-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_TurnOnRWA).  
  
3.  Nella pagina accesso Web remoto accesso, digitare il nome dell'account utente e la password e quindi fare clic sulla freccia.  
  
4.  Usare il metodo preferito per cercare il file multimediale da riprodurre.  
  
    > [!NOTE]

    >  Per informazioni sui diversi metodi di ricerca, vedere [cercare e riprodurre file multimediali in Windows Server Essentials da un computer o un lettore multimediale digitale in rete](Play-Digital-Media-in-Windows-Server-Essentials.md#BKMK_2.1).  

    >  Per informazioni sui diversi metodi di ricerca, vedere [cercare e riprodurre file multimediali in Windows Server Essentials da un computer o un lettore multimediale digitale in rete](../use/Play-Digital-Media-in-Windows-Server-Essentials.md#BKMK_2.1).  

  
5.  Quando viene visualizzato il nome del file multimediale, fare clic sul nome di file per riprodurre il file multimediale.  
  
##  <a name="BKMK_4"></a>Aggiungere file multimediali digitali al server  

 L'amministratore del server può aggiungere file multimediali digitali alle cartelle condivise nel catalogo multimediale tramite l'accesso diretto al server, oppure tramite il sito accesso Web remoto per accedere al Dashboard. Altri utenti possono aggiungere file multimediali al server tramite il **cartelle condivise** connessione nella finestra di avvio, tramite il sito accesso Web remoto o tramite l'app My Server per Windows Phone. Per informazioni sulla riproduzione di elementi multimediali, vedere [riprodurre e condividere file multimediali digitali](Play-Digital-Media-in-Windows-Server-Essentials.md#BKMK_2).  

 L'amministratore del server può aggiungere file multimediali digitali alle cartelle condivise nel catalogo multimediale tramite l'accesso diretto al server, oppure tramite il sito accesso Web remoto per accedere al Dashboard. Altri utenti possono aggiungere file multimediali al server tramite il **cartelle condivise** connessione nella finestra di avvio, tramite il sito accesso Web remoto o tramite l'app My Server per Windows Phone. Per informazioni sulla riproduzione di elementi multimediali, vedere [riprodurre e condividere file multimediali digitali](../use/Play-Digital-Media-in-Windows-Server-Essentials.md#BKMK_2).  

  
> [!NOTE]
>  È inoltre possibile caricare file multimediali al server tramite l'app My Server per Windows Phone. È possibile scaricare l'app My Server dal [archivio Windows Phone](http://www.windowsphone.com/store/app/my-server/6c2f98d5-6fcf-4e1d-b8b1-cde62ea1a94a). Per ulteriori informazioni sull'app My Server per Windows Phone, vedere il post di blog [app telefonica My Server per Windows Server Essentials](http://blogs.technet.com/b/sbs/archive/2012/09/18/my-server-phone-app-for-windows-server-2012-essentials.aspx).  
  
#### <a name="to-add-digital-media-files-to-shared-folders-on-the-server"></a>Per aggiungere file multimediali digitali alle cartelle condivise nel server di  
  
1.  Per accedere al server, utilizzare uno dei metodi seguenti:  
  

    1.  Per informazioni sull'accesso ad accesso Web remoto, vedere [usare accesso Web remoto](Use-Remote-Web-Access-in-Windows-Server-Essentials.md).  

    1.  Per informazioni sull'accesso ad accesso Web remoto, vedere [usare accesso Web remoto](../use/Use-Remote-Web-Access-in-Windows-Server-Essentials.md).  

  
    2.  Per informazioni sull'accesso con una finestra di avvio, vedere [Panoramica della finestra di avvio](../manage/Overview-of-the-Launchpad-in-Windows-Server-Essentials.md).  
  
2.  Cercare e fare clic sulla cartella per il supporto che si desidera aggiungere.  
  
3.  Copiare e incollare oppure drag-and-drop i file multimediali che si desidera aggiungere l'oggetto condiviso cartella sul server.  
  
##  <a name="BKMK_5"></a>Opzioni di formato di download  
 Sono disponibili due opzioni per il download dei file. Queste opzioni sono disponibili solo quando si scaricano più file o una cartella a un computer basato su Windows.  
  
 Scegliere l'opzione seguente adatta alle proprie esigenze per i download:  
  
-   **Compresso ZIP file (.zip)**  
  
     Compressione di un file crea una versione compressa del file è minore del file originale. La versione compressa del file con estensione zip. Tipi di file che sono Ottiene la compressione maggiore sono i tipi di file orientati al testo (ad esempio con estensione txt, doc e xls) e i file di immagine che usano tipi di file non compressi (ad esempio con estensione bmp). Alcuni file di immagine, ad esempio i file con estensione jpg e GIF, usano già la compressione e viene ridotto le dimensioni del file poco compressione. Inoltre, un documento di Word contenente un numero elevato di elementi grafici non viene ridotto per quanto un documento che è principalmente testo.  
  
    > [!NOTE]
    >  Questa opzione offre supporto limitato per nomi di file internazionali.  
  
-   **File eseguibile autoestraente (.exe)**  
  
     Un file eseguibile autoestraente è un file scaricabile che unisce il programma di decompressione (eseguibile) i file compressi. Quando si esegue il programma eseguibile decomprime automaticamente i file compressi. Questo è un modo comune per distribuire i dati compressi senza doversi preoccupare indica se il destinatario è l'utilità decompressione destra.  
  
    > [!NOTE]
    >  Questa opzione supporta i caratteri Unicode.  
  
 Prima di inizia il download effettivo, viene creato il file .exe o ZIP. A seconda del numero di file e le dimensioni totali dei file da scaricare, ciò potrebbe richiedere alcuni minuti. Dopo aver creato il file di download, il download del file avviene in background. Ciò consente di continuare a lavorare durante il processo di download di completamento.  
  
##  <a name="BKMK_6"></a>Strumento caricamento semplice File  
 Lo strumento caricamento semplice File semplifica il processo di caricamento file nel server Windows Server Essentials. È possibile aggiungere un numero di file che si desidera eseguire lo strumento caricamento semplice File e quindi caricarli nelle cartelle condivise nel server di Windows Server Essentials in un unico batch. Per ulteriori informazioni, vedere il post di blog [informazioni sulla condivisione accesso Web remoto File](http://blogs.technet.com/b/sbs/archive/2012/04/19/understanding-remote-web-access-file-sharing.aspx).  
  
##  <a name="BKMK_7"></a>Visualizzare e selezionare file multimediali digitali condivisi  
 È possibile visualizzare o selezionare le risorse tramite il Dashboard, la finestra di avvio, il sito Web accesso Web remoto o l'app My Server per Windows Phone.  
  
#### <a name="to-view-and-browse-shared-media-from-the-dashboard"></a>Per visualizzare e selezionare file multimediali condivisi dal Dashboard  
  
1.  Aprire il Dashboard del server.  
  
2.  Nella barra di spostamento principale fare clic su **archiviazione**.  
  
3.  Fare clic su di **cartelle Server** scheda. Verrà visualizzato un elenco di cartelle del server.  
  
4.  Fare doppio clic su una cartella per visualizzarne il contenuto della cartella.  
  
#### <a name="to-view-and-browse-shared-media-using-the-launchpad-from-a-network-computer"></a>Per visualizzare e selezionare file multimediali condivisi usando la finestra di avvio da un computer di rete  
  
1.  Accedere alla finestra di avvio.  
  
2.  Fare clic su **cartelle condivise**. Apre Esplora risorse e visualizza un elenco di cartelle condivise sul server.  
  
3.  Fare doppio clic su una cartella per visualizzarne il contenuto della cartella.  
  
#### <a name="to-view-and-browse-shared-media-using-remote-web-access"></a>Per visualizzare e selezionare file multimediali condivisi usando accesso Web remoto  
  
1.  Accedere ad accesso Web remoto.  
  
2.  Fare clic su **cartelle condivise**. Il **cartelle condivise** sezione della pagina web Visualizza un elenco di cartelle condivise sul server.  
  
3.  Fare doppio clic su una cartella per visualizzarne il contenuto della cartella.  
  
## <a name="see-also"></a>Vedere anche  
  
-   [Gestire file multimediali digitali](../manage/Manage-Digital-Media-in-Windows-Server-Essentials.md)  
  

-   [Connessione](Get-Connected-in-Windows-Server-Essentials.md)  
  
-   [Usare le cartelle condivise](Use-Shared-Folders-in-Windows-Server-Essentials.md)  
  
-   [Lavorare in modalità remota](Work-Remotely-in-Windows-Server-Essentials.md)

-   [Connessione](../use/Get-Connected-in-Windows-Server-Essentials.md)  
  
-   [Usare le cartelle condivise](../use/Use-Shared-Folders-in-Windows-Server-Essentials.md)  
  
-   [Lavorare in modalità remota](../use/Work-Remotely-in-Windows-Server-Essentials.md)

