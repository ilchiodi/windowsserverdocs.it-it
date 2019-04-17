---
title: Gestire file multimediali digitali in Windows Server Essentials
description: Viene descritto come utilizzare Windows Server Essentials
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 9378bffa-487c-43ca-9ec3-7e7864d2dd9a
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 6906c1dafc6d4131e07c008b9db47ebebe9b7770
ms.sourcegitcommit: 5c8d8acc59d61756377c9c4e459a47a9ab555b39
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/18/2018
---
# <a name="manage-digital-media-in-windows-server-essentials"></a>Gestire file multimediali digitali in Windows Server Essentials

>Si applica a: Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

Gli argomenti seguenti descrivono la funzionalità del server dei flussi multimediali e viene illustrato come configurare e utilizzare il flusso multimediale in rete.  
  
> [!NOTE]
>  Per impostazione predefinita, la funzionalità di streaming multimediale non è disponibile in Windows Server Essentials e Windows Server 2012 R2 con il ruolo esperienza Windows Server Essentials installato. Per aggiungere la funzionalità in queste versioni, dei flussi multimediali [scaricare e installare Windows Server Essentials Media Pack](https://www.microsoft.com/download/details.aspx?id=40837) dal Microsoft Download Center.  
  
-   [Panoramica di file multimediali digitali](Manage-Digital-Media-in-Windows-Server-Essentials.md#BKMK_1)  
  
-   [Gestire server dei contenuti multimediali tramite il Dashboard](Manage-Digital-Media-in-Windows-Server-Essentials.md#BKMK_2)  
  
-   [Come funzionano i flussi multimediali](Manage-Digital-Media-in-Windows-Server-Essentials.md#BKMK_3)  
  
-   [Attivare o disattivare i flussi multimediali](Manage-Digital-Media-in-Windows-Server-Essentials.md#BKMK_4)  
  
-   [Aggiungere file multimediali digitali al server](Manage-Digital-Media-in-Windows-Server-Essentials.md#BKMK_5)  
  
-   [Consentire o limitare l'accesso a un catalogo multimediale nel server di](Manage-Digital-Media-in-Windows-Server-Essentials.md#BKMK_6)  
  
-   [Rinominare il catalogo multimediale](Manage-Digital-Media-in-Windows-Server-Essentials.md#BKMK_8)  
  
-   [Interrompere la condivisione di file multimediali digitali](Manage-Digital-Media-in-Windows-Server-Essentials.md#BKMK_9)  
  
-   [Abilitare i dispositivi multimediali che usano il protocollo Server Message Block (SMB) per accedere ai file condivisi nel server di](Manage-Digital-Media-in-Windows-Server-Essentials.md#BKMK_10)  
  
-   [Processori comuni e profili video supportati](Manage-Digital-Media-in-Windows-Server-Essentials.md#BKMK_CommonProcessors)  
  
-   [Problemi noti con tipi di file multimediali](Manage-Digital-Media-in-Windows-Server-Essentials.md#BKMK_KnownIssues)  
  
##  <a name="BKMK_1"></a>Panoramica di file multimediali digitali  
 File multimediali digitali fa riferimento a audio, video e foto contenuto che è stato codificato (digitale compresso). La codifica di contenuto comporta la conversione di input audio e video in un file multimediali digitali, ad esempio un file Windows Media. Dopo la codifica file multimediali digitali, può essere facilmente modificato, distribuito e riprodotto dai computer e la trasmissione facilmente nelle reti di computer.  
  
 Esempi di tipi di file multimediali digitali: Windows Media Audio (WMA), Windows Media Video (WMV), MP3, JPEG e AVI. Per informazioni sui tipi di file multimediali digitali supportati da Windows Media Player, vedere [tipi supportati da Windows Media Player di File](https://support.microsoft.com/kb/316992).  
  
### <a name="why-would-i-want-to-stream-my-digital-media"></a>Perché dovrei trasmettere file multimediali digitali?  
 Molte persone archiviano musica, video e immagini in cartelle condivise in Windows Server Essentials. È possibile che quando si desidera eseguire le operazioni seguenti:  
  
-   **Guarda i video**. Il server può essere utilizzato per archiviare e trasmettere grandi raccolte di video e trasmissioni televisive registrate nei computer o la riproduzione di altri dispositivi nella rete. È possibile trasmettere video a una console Xbox 360 o a un computer con Windows Media Player.  
  
-   **Riprodurre musica**. Quando si attiva la condivisione di file multimediali per la **musica** cartella condivisa, è possibile accedere alla musica dai dispositivi che supportano Windows Media Connect. Non è necessario abilitare o configurare gli account utente di eseguire lo streaming dal **musica** cartella condivisa dopo l'attivazione della condivisione.  
  
-   **Presentare presentazioni di fotografie**. È possibile archiviare le foto digitali nel **foto** condivise nella cartella del server, quindi accedervi da qualsiasi computer o da una console Xbox 360 connessa a un televisore in casa o in ufficio. Consente di guardare presentazioni di fotografie, ovvero il televisore come se in un frame di immagine di grandi dimensioni.  
  
###  <a name="BKMK_1.5"></a>Condivisione di file multimediali protetti da copia  
  Windows Server Essentials non supporta la condivisione file multimediali protetti da copia. Inclusi i file musicali acquistati tramite un negozio online.  
  
 Supporto Copy-protected può essere riprodotti solo nel computer o dispositivo usato per acquistarlo. Protezione contro la copia impedisce la riproduzione multimediale in uno o più computer o dispositivo, anche se si copia il supporto per il server e lo si riproduce da lì. Tuttavia, è possibile archiviare il file multimediale protetto da copia in Windows Server Essentials e continuare a riprodurlo nel computer o dispositivo usato per acquistarlo.  
  
##  <a name="BKMK_2"></a>Gestire server dei contenuti multimediali tramite il Dashboard  
  Windows Server Essentials consente di eseguire attività amministrative comuni tramite il Dashboard di Windows Server Essentials. Il **Media** scheda del server **impostazioni** pagina del Dashboard contiene quanto segue:  
  
|Sezione|Funzionalità|  
|-------------|-------------------|  
|Server dei contenuti multimediali|Il **attiva / disattiva** pulsante consente di attivare o disattivare i flussi multimediali.|  
|Qualità flusso video|Freccia a discesa consente di scegliere la qualità del video riprodotti dal server di flusso.|  
|Catalogo multimediale|Visualizza il nome del catalogo multimediale. Il nome predefinito della libreria viene chiamato **catalogo multimediale digitale**, che viene creato quando attivano i flussi multimediali. Per modificare il nome della Raccolta multimediale, vedere [rinominare il catalogo multimediale](Manage-Digital-Media-in-Windows-Server-Essentials.md#BKMK_8). È possibile fare clic su **Personalizza** per personalizzare le cartelle condivise nel catalogo multimediale.|  
  
 Per ulteriori informazioni, vedere [consentire o limitare l'accesso a un catalogo multimediale nel server di](Manage-Digital-Media-in-Windows-Server-Essentials.md#BKMK_6) e [condivisione di file multimediali protetti da copia](Manage-Digital-Media-in-Windows-Server-Essentials.md#BKMK_1.5).  
  
##  <a name="BKMK_3"></a>Come funzionano i flussi multimediali  
 La funzionalità di Windows Server Essentials dei flussi multimediali rende possibile per i computer della rete e alcuni dispositivi multimediali digitali in rete di riprodurre file multimediali digitali archiviati sul server.  
  
 Quando si attiva il server dei contenuti multimediali, contenuti condivisi nei cataloghi multimediali saranno disponibili per la riproduzione nei dispositivi della rete che sono in grado di ricevere flussi multimediali dal server. È possibile trasmettere la maggior parte dei tipi di file multimediali digitali. Alcuni tipi di file che è possibile trasmettere più comuni includono:  
  
-   Formati Windows Media Player (.asf,. wma, wmv,. WM)  
  
-   Audio Visual Interleave (.avi)  
  
-   Spostamento delle immagini Experts Group (.mpeg, mpg, MP3)  
  
-   Audio per Windows (.wav)  
  
-   (.cda) traccia Audio CD  
  
 Per riprodurre un file, semplicemente individuare un brano, video o un'immagine in una cartella condivisa, fare doppio clic sul file e il contenuto verrà trasmesso dal server al computer e riprodurre. Per informazioni su come trovare e riprodurre file multimediali digitali archiviati sul server, vedere [riprodurre file multimediali digitali](../use/Play-Digital-Media-in-Windows-Server-Essentials.md).  
  
 Per trasmettere file multimediali, è necessario hardware seguenti:  
  
-   Una rete privata cablata o wireless  
  
-   Un altro computer della rete o un dispositivo noto come un ricevitore multimediale digitale (definito a volte un lettore multimediale digitale). I ricevitori multimediali digitali sono dispositivi hardware connessi alla rete cablata o wireless che è possibile controllare con il computer? anche se il computer si trova in un'altra stanza.  
  
 Per ulteriori informazioni, vedere [attivare o disattivare i flussi multimediali](Manage-Digital-Media-in-Windows-Server-Essentials.md#BKMK_4).  
  
##  <a name="BKMK_4"></a>Attivare o disattivare i flussi multimediali  
 È possibile condividere musica, video e immagini da Windows Server Essentials trasmettendo i file in qualsiasi ricevitore multimediale digitale supportato (DMR), ad esempio computer, telefoni cellulari, televisori, ricevitori multimediali digitali, Extender per Windows Media Center (inclusi Xbox 360) e altri dispositivi elettronici digitali.  
  
 Per un elenco aggiornato di dispositivi multimediali digitali compatibili con Windows Server Essentials, vedere il [Windows Compatibility Center](https://www.microsoft.com/windows/compatibility/CompatCenter/Home).  
  
### <a name="enabling-media-sharing"></a>Abilitazione di condivisione file multimediali  
 Per condividere i file multimediali archiviati in Windows Server Essentials, è necessario attivare i flussi multimediali. Flussi multimediali è disattivato per impostazione predefinita.  
  
####  <a name="BKMK_2.5"></a>Per attivare o disattivare i flussi multimediali  
  
1.  Aprire il Dashboard di Windows Server Essentials.  
  
2.  Fare clic su **impostazioni**, fare clic su **Media**, quindi effettuare una delle operazioni seguenti:  
  
    -   Fare clic su **attiva** per iniziare a condividere tutti i file archiviati nel catalogo multimediale del server.  
  
    -   Fare clic su **disattivare** per interrompere la condivisione di tutti i file archiviati nel catalogo multimediale del server.  
  
3.  Se si vogliono condividere altre cartelle nel catalogo multimediale, fare clic su **Personalizza**e quindi seleziona **Sì** per ogni cartella condivisa che si desidera includere nel catalogo multimediale.  
  
4.  Fare clic su **OK** per salvare le modifiche.  
  
 Per informazioni sui tipi di file multimediali digitali supportati da Windows Media Player, vedere [tipi supportati da Windows Media Player di File](https://support.microsoft.com/kb/316992).  
  
 Per ulteriori informazioni, vedere [consentire o limitare l'accesso a un catalogo multimediale nel server di](Manage-Digital-Media-in-Windows-Server-Essentials.md#BKMK_6).  
  
##  <a name="BKMK_5"></a>Aggiungere file multimediali digitali al server  
 L'amministratore del server può aggiungere file multimediali digitali alle cartelle condivise nel catalogo multimediale tramite l'accesso diretto al server, oppure tramite il sito accesso Web remoto per accedere al Dashboard. Altri utenti possono aggiungere file multimediali al server tramite il **cartelle condivise** connessione nella finestra di avvio, tramite il sito accesso Web remoto o tramite l'app My Server per Windows Phone. Per informazioni sulla riproduzione di elementi multimediali, vedere [riprodurre file multimediali digitali](../use/Play-Digital-Media-in-Windows-Server-Essentials.md).  
  
> [!NOTE]
>  È inoltre possibile caricare file multimediali al server tramite l'app My Server per Windows Phone. È possibile scaricare l'app My Server dal [archivio Windows Phone](http://www.windowsphone.com/store/app/my-server/6c2f98d5-6fcf-4e1d-b8b1-cde62ea1a94a). Per ulteriori informazioni sull'app My Server per Windows phone, vedere il post di blog [app telefonica My Server per Windows Server Essentials](http://blogs.technet.com/b/sbs/archive/2012/09/18/my-server-phone-app-for-windows-server-2012-essentials.aspx).  
  
#### <a name="to-add-digital-media-files-to-shared-folders-on-the-server"></a>Per aggiungere file multimediali digitali alle cartelle condivise nel server di  
  
1.  Per accedere al server, utilizzare uno dei metodi seguenti:  
  
    1.  Per informazioni sull'accesso ad accesso Web remoto, vedere [accedere ad accesso Web remoto](../use/Use-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_1).  
  
    2.  Per informazioni sull'accesso con una finestra di avvio, vedere [Panoramica della finestra di avvio](Overview-of-the-Launchpad-in-Windows-Server-Essentials.md).  
  
2.  Cercare e fare clic sulla cartella per il supporto che si desidera aggiungere.  
  
3.  Copiare e incollare oppure trascinare e i file multimediali da aggiungono alla cartella condivisa appropriata sul server.  
  
##  <a name="BKMK_6"></a>Consentire o limitare l'accesso a un catalogo multimediale nel server di  
  
-   Quando attiva la condivisione di file multimediali, vengono create quattro cartelle predefinite: musica, immagini, video e registrazioni TV. Se una di queste cartelle preesiste nel server, la cartella esistente viene riutilizzata come una cartella condivisa per la condivisione di file multimediali. Tutte la cartella s media contenuto e utente le autorizzazioni esistenti vengono preservate e condivisi con tutti gli utenti di rete.  
  
-   Prima di attivare la condivisione del catalogo multimediale per una cartella condivisa, tenere presente che condivisione del catalogo multimediale ignora qualsiasi tipo di accesso di account utente che è impostato per la cartella condivisa. Ad esempio, supponiamo che attivare Condivisione del catalogo multimediale per la **foto** cartella condivisa e si imposta il **foto** cartella condivisa per **Nessun accesso** per un utente account chiamato Federico. Federico può comunque trasmettere qualsiasi file multimediale digitale dal **video** cartella condivisa per qualsiasi ricevitore o lettore multimediale digitale supportato. Se si dispone di file multimediali digitali che non vuoi trasmettere in questo modo, è possibile archiviare i file in una cartella che non dispone di condivisione del catalogo multimediale attivata.  
  
-   Se si attiva Condivisione del catalogo multimediale per una cartella condivisa, qualsiasi ricevitore che può accedere alla rete di Windows Server Essentials o lettore multimediale digitale supportato può accedere anche i file multimediali digitali contenuti nella cartella condivisa. Ad esempio, se si dispone di una rete wireless e non protetta, chiunque si trovi all'interno della rete senza fili intervallo potenzialmente possono accedere i file multimediali digitali in tale cartella. Prima di attivare Condivisione del catalogo multimediale, assicurarsi di proteggere la rete wireless. Per ulteriori informazioni, vedere la documentazione del punto di accesso wireless.  
  
##  <a name="BKMK_8"></a>Rinominare il catalogo multimediale  
 Il nome predefinito del catalogo multimediale è **Server multimediale digitale**. Viene creato quando attivano i flussi multimediali in Windows Server Essentials. Per ulteriori informazioni sull'attivazione flussi multimediali, vedere [per attivare o disattivare i flussi multimediali](Manage-Digital-Media-in-Windows-Server-Essentials.md#BKMK_2.5). È possibile modificare il nome della Raccolta multimediale in qualsiasi momento tramite il Dashboard del server.  
  
#### <a name="to-rename-the-media-library"></a>Per rinominare il catalogo multimediale  
  
1.  Aprire il Dashboard del server. Per aprire il Dashboard dalla finestra di avvio, fare clic su **Dashboard**, digitare la password del server e quindi fare clic sulla freccia per accedere.  
  
2.  Nel Dashboard del server, fare clic su **impostazioni**.  
  
3.  Nel **impostazioni** pagina, fare clic su di **Media** scheda.  
  
4.  Nel **catalogo multimediale** sezione il **impostazioni multimediali** pagina, fare clic sul nome del catalogo multimediale.  
  
5.  Nel **modificare il nome della Raccolta multimediale** la finestra di dialogo, digitare un nuovo nome per il catalogo multimediale e quindi fare clic su **OK**.  
  
##  <a name="BKMK_9"></a>Interrompere la condivisione di file multimediali digitali  
 L'amministratore del server può interrompere la condivisione file multimediali digitali archiviati in cartelle condivise in un server che esegue Windows Server Essentials.  
  
#### <a name="to-stop-sharing-media-in-shared-folders"></a>Per interrompere la condivisione file multimediali in cartelle condivise  
  
1.  Aprire il Dashboard del server.  
  
2.  Nel Dashboard **Home** pagina, fare clic su **installazione**, fare clic su **configurare il Server dei contenuti multimediali**, quindi fare clic su **fare clic per configurare il Server dei contenuti multimediali**.  
  
3.  Nel **Media** pagina delle impostazioni, è possibile scegliere una delle opzioni seguenti:  
  
    -   Fare clic su **disattivare** per interrompere la condivisione di tutti i file archiviati nel server.  
  
    -   Fare clic su **Personalizza**e quindi seleziona **No** per le specifiche cartelle che si desidera interrompere la condivisione.  
  
4.  Fare clic su **applica** o **OK** per salvare le modifiche.  
  
##  <a name="BKMK_10"></a>Abilitare i dispositivi multimediali che usano il protocollo Server Message Block (SMB) per accedere ai file condivisi nel server di  
 I dispositivi che utilizzano Server Message Block (SMB) per file e condivisione di accesso alla rete posto di DLNA (per i flussi multimediali) richiedono che l'account Guest essere attivato. In questo modo qualsiasi dispositivo o utente sulla rete per visualizzare il contenuto delle cartelle condivise senza autenticazione.  
  
> [!CAUTION]
>  Quando si abilita l'account Guest, chiunque può accedere a risorse condivise nel server per impostazione predefinita.  
  
##  <a name="BKMK_CommonProcessors"></a>Processori comuni e profili video supportati  
 Per trasmettere file multimediali dal server di Windows Server Essentials, è possibile utilizzare un computer che esegue il sistema operativo Windows 7 o Windows 8 o altri dispositivi collegati alla rete (ad esempio lettori multimediali digitali) o Media Center Extender (come la Xbox 360). Quando si è lontani dalla rete, usare Windows Media Player supporti di accesso Web remoto per riprodurre i file archiviati nel server.  
  
 È necessario una velocità di trasferimento dati compresa tra 200 KBps e 10 MBps. È necessario usare formati di file multimediali che possono riconoscere e riprodurre il computer e dispositivi. Non tutti i dispositivi supportano gli stessi formati multimediali, quindi deve essere presente un modo per il computer e dispositivi per riprodurre i file multimediali presenti.  
  
  Windows Server Essentials contiene supporto per la transcodifica che determina le funzionalità del computer o del dispositivo e quindi converte un file video non supportati in file supportati. In generale, se Windows Media Player 12 può riprodurre il contenuto in un computer che esegue Windows 7 o Windows 8, il contenuto nel server verrà riprodotto sul dispositivo connesso alla rete.  
  
 La velocità di formato e bit scelta per la transcodifica è dipende molto le prestazioni del processore del server. Le prestazioni del processore vengono identificate nell'ambito dell'indice prestazioni Windows. Per determinare l'indice di prestazioni del server, effettuare una delle operazioni seguenti:  
  
-   In un computer di rete che esegue Windows 7 o Windows 8 dotato dello stesso processore del server, Vai al **Pannello di controllo**, fare clic su **strumenti e informazioni sulle prestazioni**e quindi esaminare le informazioni nel **velocità e migliorare le prestazioni del computer s** pagina.  
  
-   Contattare il produttore del processore.  
  
 Per l'esperienza utente ottimale, scegliere una qualità appropriato per il processore del server di risoluzione dei flussi video. Il server adeguerà automaticamente la velocità in bit a una di queste impostazioni:  
  
-   **Basso** se il punteggio del processore è minore di 3,6.  
  
-   **Media** se il punteggio del processore è maggiore di 3,6 e minore di 4,2.  
  
-   **Elevata** se il punteggio del processore è maggiore di 4,2 e minore di 6,0.  
  
-   **Migliore** se il punteggio del processore è maggiore di 6,0.  
  
 Se si sceglie una risoluzione che richiede maggiore potenza di elaborazione maggiore di quello del server ha dei flussi video, potrebbero verificarsi buffer e interruzioni durante flussi multimediali dal server.  
  
> [!NOTE]
>  Per trasmettere ad alta definizione video tramite accesso Web remoto, è necessario un processore con un punteggio di almeno 6,0.  
  
##  <a name="BKMK_KnownIssues"></a>Problemi noti con tipi di file multimediali  
 La funzionalità di flussi multimediali in accesso Web remoto utilizza il servizio Windows Media Player 12 rete condivisione. Flussi multimediali accesso Web remoto supporta l'audio, video e tipi di file di immagine supportati da Windows Media Player 12 e Silverlight 4.  
  
 La tabella seguente elenca i tipi di file (formati) supportati da flussi multimediali di accesso Web remoto. Se sono presenti tipi nel server che non sono inclusi nella tabella di file multimediali, è possibile trasmessi tramite i flussi multimediali di accesso Web remoto.  
  
|Tipo di file|Estensione di file|  
|---------------|-------------------------|  
|File 3GPP|3GP, .3gpp, 3G2 e .3gp2|  
|File audio audio dati trasporto flusso (ADTS)|.adts e .adt|  
|File audio AU|AU e snd|  
|File audio audio File AIFF (Interchange Format)|AIF, aifc e AIFF|  
|File AVCHD|m2ts e M2T MTS|  
|Disco audio CD|cda|  
|Disco Video DVD|VOB|  
|File di immagine JPEG|. jpg|  
|File audio MP3|MP3 e m3u|  
|File video MPEG|. MPEG, mpg, m1v,. MPA, mpe, MP4, MP4V, m4v, M4A e Mov|  
|File audio e video di Windows|AVI e WAV|  
|File audio e video Windows Media|ASF, ASX, wax, WM, wma, wmd, WMP,. wmv, wmx, wpl e wvx|  
  
> [!NOTE]
>  Se non è possibile utilizzare un tipo di file elencati in questa tabella, il file potrebbe essere stato codificato con un codec non supportato da Windows Media Player.  
  
 Per ulteriori informazioni sui formati di file supportati, vedere [tipi supportati da Windows Media Player di File](https://go.microsoft.com/fwlink/p/?LinkID=196118) e [supportati formati multimediali, protocolli e campi di log](https://go.microsoft.com/fwlink/p/?LinkId=203339) per Silverlight.  
  
## <a name="see-also"></a>Vedere anche  
  
-   [Riprodurre file multimediali digitali](../use/Play-Digital-Media-in-Windows-Server-Essentials.md)  
  
-   [Gestire Windows Server Essentials](Manage-Windows-Server-Essentials.md)
