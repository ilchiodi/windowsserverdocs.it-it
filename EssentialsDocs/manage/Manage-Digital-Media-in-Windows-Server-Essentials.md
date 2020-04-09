---
title: Gestire i supporti digitali in Windows Server Essentials
description: Viene descritto come usare Windows Server Essentials
ms.date: 10/03/2016
ms.prod: windows-server
ms.topic: article
ms.assetid: 9378bffa-487c-43ca-9ec3-7e7864d2dd9a
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: bc94e7617960c789638324f67574f22c84ca656c
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80852804"
---
# <a name="manage-digital-media-in-windows-server-essentials"></a>Gestire i supporti digitali in Windows Server Essentials

>Si applica a: Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

L'argomento seguente descrive la funzionalità dei flussi multimediali del server e illustra come configurarla e usarla nella rete.  
  
> [!NOTE]
>  Per impostazione predefinita, la funzionalità di streaming multimediale non è disponibile in Windows Server Essentials e Windows Server 2012 R2 con il ruolo esperienza Windows Server Essentials installato. Per aggiungerla in queste versioni, [scaricare e installare Windows Server Essentials Media Pack](https://www.microsoft.com/download/details.aspx?id=40837) dall'Area download Microsoft.  
  
-   [Panoramica dei supporti digitali](Manage-Digital-Media-in-Windows-Server-Essentials.md#BKMK_1)  
  
-   [Gestire il server multimediale usando il dashboard](Manage-Digital-Media-in-Windows-Server-Essentials.md#BKMK_2)  
  
-   [Funzionamento dei flussi multimediali](Manage-Digital-Media-in-Windows-Server-Essentials.md#BKMK_3)  
  
-   [Attivare o disattivare i flussi multimediali](Manage-Digital-Media-in-Windows-Server-Essentials.md#BKMK_4)  
  
-   [Aggiungere file multimediali digitali al server](Manage-Digital-Media-in-Windows-Server-Essentials.md#BKMK_5)  
  
-   [Consentire o limitare l'accesso a un catalogo multimediale nel server](Manage-Digital-Media-in-Windows-Server-Essentials.md#BKMK_6)  
  
-   [Rinominare il catalogo multimediale](Manage-Digital-Media-in-Windows-Server-Essentials.md#BKMK_8)  
  
-   [Interrompi condivisione di file multimediali digitali](Manage-Digital-Media-in-Windows-Server-Essentials.md#BKMK_9)  
  
-   [Abilitare i dispositivi multimediali che usano il protocollo Server Message Block (SMB) per accedere ai file condivisi nel server](Manage-Digital-Media-in-Windows-Server-Essentials.md#BKMK_10)  
  
-   [Processori comuni e profili video supportati](Manage-Digital-Media-in-Windows-Server-Essentials.md#BKMK_CommonProcessors)  
  
-   [Problemi noti relativi ai tipi di file multimediali](Manage-Digital-Media-in-Windows-Server-Essentials.md#BKMK_KnownIssues)  
  
##  <a name="digital-media-overview"></a><a name="BKMK_1"></a>Panoramica dei supporti digitali  
 I file multimediali digitali sono contenuti audio, video e foto codificati, ovvero compressi in modo digitale. La codifica di contenuto comporta la conversione di input audio e video in un file multimediale digitale, ad esempio un file di Windows Media. Dopo la codifica, un file multimediale digitale potrà essere modificato, distribuito e riprodotto con facilità dai computer e potrà essere trasmesso in modo semplice nelle reti di computer.  
  
 I file multimediali digitali includono, ad esempio, Windows Media Audio (WMA), Windows Media Video (WMV), MP3, JPEG e AVI. Per informazioni sui tipi di file multimediali digitali supportati da Windows Media Player, vedere [Tipi di file supportati da Windows Media Player](https://support.microsoft.com/kb/316992).  
  
### <a name="why-would-i-want-to-stream-my-digital-media"></a>Vantaggi della trasmissione in flussi dei file multimediali digitali  
 Molti utenti archiviano musica, video e immagini in cartelle condivise in Windows Server Essentials. È possibile che si vogliano eseguire le operazioni seguenti:  
  
-   **Guardare video**. Il server può essere usato per archiviare e trasmettere grandi raccolte di video e trasmissioni televisive registrate nei computer o in altri dispositivi per la riproduzione disponibili in rete. È possibile trasmettere video a una console Xbox 360 o a un computer usando Windows Media Player.  
  
-   **Riprodurre musica**. Quando si attiva la Condivisione file multimediali per la cartella condivisa **Musica**, sarà possibile accedere alla musica dai dispositivi che supportano Windows Media Connect. Dopo l'attivazione della condivisione, non è necessario abilitare o configurare account utente per la trasmissione di flussi dalla cartella condivisa **Musica**.  
  
-   **Eseguire presentazioni di fotografie**. È possibile archiviare le foto digitali nella cartella condivisa **Foto** nel server, quindi accedervi da qualsiasi computer o da una console Xbox 360 connessa a un televisore in casa o in ufficio. È possibile usare il televisore come se fosse una grande cornice per guardare presentazioni fotografiche.  
  
###  <a name="sharing-copy-protected-media"></a><a name="BKMK_1.5"></a>Condivisione di file multimediali protetti da copia  
  Windows Server Essentials non supporta la condivisione di file multimediali protetti da copia. inclusi i file musicali acquistati tramite un negozio online.  
  
 I file multimediali protetti da copia possono essere riprodotti solo nel computer o nel dispositivo usato per acquistarli. La protezione contro la copia impedisce la riproduzione di file multimediali in più computer o dispositivi, anche se si copia il file multimediale sul server e lo si riproduce da questa posizione. Tuttavia, è possibile archiviare il supporto protetto da copia in Windows Server Essentials e continuare a riprodurre il supporto nel computer o nel dispositivo usato per acquistarlo.  
  
##  <a name="manage-media-server-using-the-dashboard"></a><a name="BKMK_2"></a>Gestire il server multimediale usando il dashboard  
  Windows Server Essentials consente di eseguire attività amministrative comuni tramite il dashboard di Windows Server Essentials. La scheda **Media** della pagina **Impostazioni** del dashboard contiene quanto segue:  
  
|Sezione|Funzionalità|  
|-------------|-------------------|  
|Server dei contenuti multimediali|Il pulsante **Attiva/Disattiva** consente di attivare o disattivare i flussi multimediali.|  
|Qualità flusso video|Questa freccia a discesa consente di scegliere la qualità del flusso dei video riprodotti dal server.|  
|Catalogo multimediale|Visualizza il nome del catalogo multimediale. Il nome predefinito è **Catalogo multimediale digitale**, che viene creato quando si attivano i flussi multimediali. Per cambiare questo nome, vedere [Rinominare il catalogo multimediale](Manage-Digital-Media-in-Windows-Server-Essentials.md#BKMK_8). È possibile fare clic su **Personalizza** per personalizzare le cartelle condivise nel catalogo multimediale.|  
  
 Per altre informazioni, vedere [Consentire o limitare l'accesso a un catalogo multimediale nel server](Manage-Digital-Media-in-Windows-Server-Essentials.md#BKMK_6) e sulla [Condivisione di file multimediali protetti da copia](Manage-Digital-Media-in-Windows-Server-Essentials.md#BKMK_1.5).  
  
##  <a name="how-media-streaming-works"></a><a name="BKMK_3"></a>Funzionamento dei flussi multimediali  
 La funzionalità di streaming multimediale di Windows Server Essentials consente ai computer in rete e ad alcuni dispositivi multimediali digitali in rete di riprodurre file multimediali digitali archiviati nel server.  
  
 Se si attiva il server dei contenuti multimediali, i contenuti condivisi nei cataloghi multimediali saranno disponibili per la riproduzione nei dispositivi della rete in grado di ricevere flussi multimediali dal server. È possibile trasmettere la maggior parte dei tipi di file multimediali digitali. I tipi di file più comuni includono:  
  
- Formati Windows Media (.asf, .wma, .wmv, .wm)  
  
- Audio Visual Interleave (.avi)  
  
- Moving Pictures Experts Group (.mpeg, .mpg, .mp3)  
  
- Audio per Windows (.wav)  
  
- CD Audio Track (.cda)  
  
  Per riprodurre un file, individuare e fare doppio clic su un brano, un video o un'immagine in una cartella condivisa. Il contenuto verrà trasmesso dal server e riprodotto nel computer. Per informazioni su come trovare e riprodurre i file multimediali digitali archiviati nel server, vedere [riprodurre file multimediali digitali](../use/Play-Digital-Media-in-Windows-Server-Essentials.md).  
  
  Per trasmettere i flussi multimediali, sono necessari i componenti hardware seguenti:  
  
- Una rete privata cablata o wireless.  
  
- Un altro computer della rete o un dispositivo chiamato ricevitore multimediale digitale o lettore multimediale digitale di rete. I ricevitori multimediali digitali sono dispositivi hardware connessi alla rete cablata o wireless che è possibile controllare usando il computer? anche se il computer si trova in un'altra stanza.  
  
  Per altre informazioni, vedere [attivare o disattivare i flussi multimediali](Manage-Digital-Media-in-Windows-Server-Essentials.md#BKMK_4).  
  
##  <a name="turn-media-streaming-on-or-off"></a><a name="BKMK_4"></a>Attivare o disattivare i flussi multimediali  
 È possibile condividere musica, video e immagini da Windows Server Essentials mediante lo streaming di file in qualsiasi ricevitore multimediale digitale supportato (ricevitore), ad esempio computer, telefoni cellulari, televisori, ricevitori multimediali digitali, Extender per Windows Media Center (incluso Xbox 360) e altri dispositivi elettronici personali.  
  
 Per un elenco aggiornato di dispositivi multimediali digitali compatibili con Windows Server Essentials, vedere [Windows Compatibility Center](https://www.microsoft.com/windows/compatibility/CompatCenter/Home).  
  
### <a name="enabling-media-sharing"></a>Abilitazione della condivisione di file multimediali  
 Per condividere i file multimediali archiviati in Windows Server Essentials, è necessario attivare i flussi multimediali. Per impostazione predefinita, i flussi multimediali sono disattivati.  
  
####  <a name="to-turn-media-streaming-on-or-off"></a><a name="BKMK_2.5"></a>Per attivare o disattivare i flussi multimediali  
  
1. Aprire il dashboard di Windows Server Essentials.  
  
2. Fare clic su **Impostazioni**, su **Media** e quindi eseguire una delle operazioni seguenti:  
  
   -   Fare clic su **Attiva** per iniziare a condividere tutti i file archiviati nel catalogo multimediale del server.  
  
   -   Fare clic su **Disattiva** per interrompere la condivisione di tutti i file archiviati nel catalogo multimediale del server.  
  
3. Se si vogliono condividere altre cartelle del catalogo multimediale, fare clic su **Personalizza** e quindi selezionare **Sì** per ogni cartella condivisa da includere nel catalogo.  
  
4. Fare clic su **OK** per salvare le modifiche.  
  
   Per informazioni sui tipi di file multimediali digitali supportati da Windows Media Player, vedere [Tipi di file supportati da Windows Media Player](https://support.microsoft.com/kb/316992).  
  
   Per altre informazioni, vedere [Consentire o limitare l'accesso a un catalogo multimediale nel server](Manage-Digital-Media-in-Windows-Server-Essentials.md#BKMK_6).  
  
##  <a name="add-digital-media-files-to-the-server"></a><a name="BKMK_5"></a>Aggiungere file multimediali digitali al server  
 L'amministratore del server può aggiungere file multimediali digitali alle cartelle condivise nel Catalogo multimediale tramite l'accesso diretto al server oppure usando il sito di Accesso Web remoto per accedere al dashboard. Altri utenti possono aggiungere file multimediali al server usando la connessione a **cartelle condivise** nella finestra di avvio, usando il sito di accesso Web remoto o l'app My server per Windows Phone. Per informazioni sulla riproduzione di file multimediali, vedere [riprodurre file multimediali digitali](../use/Play-Digital-Media-in-Windows-Server-Essentials.md).  
  
> [!NOTE]
>  È anche possibile caricare file multimediali sul server usando l'app My Server per Windows Phone, L'app My Server è disponibile per il download da [Windows Phone Store](https://www.windowsphone.com/store/app/my-server/6c2f98d5-6fcf-4e1d-b8b1-cde62ea1a94a). Per ulteriori informazioni sull'app My Server per Windows Phone, vedere il post di Blog [relativo all'app My Server per telefoni per Windows Server Essentials](https://blogs.technet.com/b/sbs/archive/2012/09/18/my-server-phone-app-for-windows-server-2012-essentials.aspx).  
  
#### <a name="to-add-digital-media-files-to-shared-folders-on-the-server"></a>Per aggiungere file multimediali alle cartelle condivise sul server  
  
1.  Usare uno dei metodi seguenti per accedere al server:  
  
    1.  Per informazioni sull'accesso ad Accesso Web remoto, vedere [Accedere ad Accesso Web remoto](../use/Use-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_1).  
  
    2.  Per informazioni sull'accesso con Launchpad, vedere [Panoramica di Launchpad](Overview-of-the-Launchpad-in-Windows-Server-Essentials.md).  
  
2.  Cercare e selezionare la cartella dei file multimediali da aggiungere.  
  
3.  Copiare e incollare oppure trascinare i file multimediali da aggiungere nella cartella condivisa appropriata sul server.  
  
##  <a name="allow-or-restrict-access-to-a-media-library-on-the-server"></a><a name="BKMK_6"></a>Consentire o limitare l'accesso a un catalogo multimediale nel server  
  
-   Quando si attiva la condivisione di file multimediali, vengono create quattro cartelle predefinite: Musica, Immagini, Video e Registrazioni TV. Se una di queste cartelle preesiste nel server, viene riutilizzata come cartella condivisa per la condivisione di file multimediali. Tutti i contenuti multimediali e le autorizzazioni utente della cartella esistente vengono conservati e vengono condivisi con tutti gli utenti di rete.  
  
-   Prima di attivare la condivisione del catalogo multimediale per una cartella condivisa, tenere presente che questo tipo di condivisione ignora qualsiasi tipo di accesso basato su account utente impostato per la cartella condivisa. Si supponga ad esempio di attivare la condivisione del catalogo multimediale per la cartella condivisa **Foto**, che è impostata su **Nessun accesso** per un account utente chiamato Federico. Federico può comunque trasmettere qualsiasi file multimediale digitale dalla cartella condivisa **Video** in qualsiasi ricevitore o lettore multimediale digitale supportato. Se non si vuole trasmettere in questo modo alcuni file multimediali digitali, archiviarli in una cartella per cui la condivisione del catalogo multimediale non è attivata.  
  
-   Se si attiva la condivisione del catalogo multimediale per una cartella condivisa, qualsiasi ricevitore o lettore multimediale digitale supportato che può accedere alla rete di Windows Server Essentials può accedere anche al supporto digitale nella cartella condivisa. Se ad esempio si ha una rete wireless non protetta, chiunque si trovi nel raggio di copertura di questa rete può in teoria accedere ai file multimediali digitali contenuti in questa cartella. Prima di attivare la condivisione del catalogo multimediale, assicurarsi di proteggere la rete wireless. Per altre informazioni, vedere la documentazione del punto di accesso wireless.  
  
##  <a name="rename-the-media-library"></a><a name="BKMK_8"></a>Rinominare il catalogo multimediale  
 Il nome predefinito del catalogo multimediale è **Server multimediale digitale** Viene creato quando si attivano i flussi multimediali in Windows Server Essentials. Per ulteriori informazioni sull'attivazione dei flussi multimediali, vedere [per attivare o disattivare i flussi multimediali](Manage-Digital-Media-in-Windows-Server-Essentials.md#BKMK_2.5). È possibile modificare il nome del catalogo multimediale in qualsiasi momento tramite il dashboard del server.  
  
#### <a name="to-rename-the-media-library"></a>Per rinominare il catalogo multimediale  
  
1.  Aprire il dashboard del server. Per aprire il dashboard dalla finestra di avvio, fare clic su **Dashboard**, digitare la password del server e quindi fare clic sulla freccia per accedere.  
  
2.  Nel dashboard del server fare clic su **Impostazioni**.  
  
3.  Nella pagina **Impostazioni** fare clic sulla scheda **Media**.  
  
4.  Nella sezione **Catalogo multimediale** della pagina **Impostazioni multimediali** fare clic sul nome del catalogo multimediale.  
  
5.  Nella finestra di dialogo **Modifica nome Catalogo multimediale** immettere un nuovo nome per il catalogo e quindi fare clic su **OK**.  
  
##  <a name="stop-sharing-digital-media"></a><a name="BKMK_9"></a>Interrompi condivisione di file multimediali digitali  
 L'amministratore del server può interrompere la condivisione di contenuti multimediali digitali archiviati in cartelle condivise in un server che esegue Windows Server Essentials.  
  
#### <a name="to-stop-sharing-media-in-shared-folders"></a>Per interrompere la condivisione di contenuti multimediali nelle cartelle condivise  
  
1.  Aprire il dashboard del server.  
  
2.  Nella pagina **Home** del dashboard fare clic su **Configura**, su **Configura server dei contenuti multimediali** e quindi su **Fare clic per configurare il server dei contenuti multimediali**.  
  
3.  Nella pagina di impostazioni **Media** è possibile scegliere una delle opzioni seguenti:  
  
    -   Fare clic su **Disattiva** per interrompere la condivisione di tutti i file archiviati nel server.  
  
    -   Fare clic su **Personalizza** e quindi selezionare **No** per le specifiche cartelle per cui interrompere la condivisione.  
  
4.  Fare clic su **Applica** o su **OK** per salvare le modifiche.  
  
##  <a name="enable-media-devices-that-use-the-server-message-block-smb-protocol-to-access-shared-files-on-the-server"></a><a name="BKMK_10"></a>Abilitare i dispositivi multimediali che usano il protocollo Server Message Block (SMB) per accedere ai file condivisi nel server  
 Per i dispositivi che usano il protocollo SMB per l'accesso a file e condivisioni di rete al posto di DLNA (per i flussi multimediali), è necessario attivare un account Guest. In questo modo, qualsiasi dispositivo o utente della rete potrà visualizzare i contenuti delle cartelle condivise senza autenticazione.  
  
> [!CAUTION]
>  Quando si abilita l'account Guest, per impostazione predefinita chiunque può accedere alle risorse condivise del server.  
  
##  <a name="common-processors-and-the-video-profiles-they-support"></a><a name="BKMK_CommonProcessors"></a>Processori comuni e profili video supportati  
 Per eseguire lo streaming di file multimediali dal server di Windows Server Essentials, è possibile usare un computer che esegue il sistema operativo Windows 7 o Windows 8 o altri dispositivi di rete, ad esempio lettori multimediali digitali, o Media Center Extender (ad esempio, Xbox 360). Quando si è lontani dalla rete, usare il servizio Media Player di Accesso Web remoto per riprodurre i file archiviati nel server.  
  
 È necessaria una velocità di trasferimento dati compresa tra 200 KBps e 10 MBps. È necessario usare formati di file multimediali che il computer e i dispositivi riconoscano e siano in grado di riprodurre. Non tutti i dispositivi supportano gli stessi formati di file multimediali, quindi è necessario fare in modo che il computer e i dispositivi riproducano i propri file multimediali.  
  
  Windows Server Essentials contiene il supporto per la transcodifica che determina le funzionalità del computer o del dispositivo e quindi converte dinamicamente un file video non supportato in uno supportato. In generale, se Windows Media Player 12 può riprodurre il contenuto in un computer che esegue Windows 7 o Windows 8, il contenuto del server verrà riprodotto sul dispositivo connesso alla rete.  
  
 Il formato e la velocità in bit scelti per la transcodifica dipendono in larga misura dalle prestazioni del processore del server. Le prestazioni del processore vengono identificate nell'ambito dell'Indice prestazioni Windows. Per determinare l'indice di prestazioni del proprio server, eseguire una delle operazioni seguenti:  
  
- In un computer di rete che esegue Windows 7 o Windows 8 con lo stesso processore del server, accedere al **Pannello di controllo**, fare clic su **informazioni e strumenti**per le prestazioni, quindi controllare le informazioni sulla pagina **velocità e miglioramento delle prestazioni del computer** .  
  
- Contattare il produttore del processore.  
  
  Per l'esperienza utente ottimale, scegliere una qualità di risoluzione dei flussi video appropriata per il processore del server. Il server adeguerà automaticamente la velocità in bit a una di queste impostazioni:  
  
- **Bassa** se il punteggio del processore è minore di 3,6.  
  
- **Media** se il punteggio del processore è maggiore di 3,6 e minore di 4,2.  
  
- **Alta** se il punteggio del processore è maggiore di 4,2 e minore di 6,0.  
  
- **Massima** se il punteggio del processore è maggiore di 6,0.  
  
  Se si sceglie una risoluzione dei flussi video che richiede una potenza di elaborazione maggiore di quella offerta dal server, potrebbero verificarsi buffer e interruzioni durante la trasmissione di contenuti multimediali dal server.  
  
> [!NOTE]
>  Per trasmettere video in alta definizione tramite Accesso Web remoto, è necessario un processore con un punteggio di almeno 6,0.  
  
##  <a name="known-issues-with-media-file-types"></a><a name="BKMK_KnownIssues"></a>Problemi noti relativi ai tipi di file multimediali  
 La funzionalità di flussi multimediali di Accesso Web remoto usa il Servizio di condivisione in rete Windows Media Player 12. I flussi multimediali di Accesso Web remoto supportano i tipi di file audio, video e immagine supportati da Windows Media Player 12 e Silverlight 4.  
  
 La tabella seguente contiene un elenco dei tipi di file (formati) supportati dai flussi multimediali di Accesso Web remoto. I tipi di file multimediali non inclusi nella tabella non possono essere trasmessi tramite Accesso Web remoto.  
  
|Tipo file|Estensione di file|  
|---------------|-------------------------|  
|File 3GPP|.3gp, .3gpp, .3g2 e .3gp2|  
|File audio ADTS (Audio Data Transport Stream)|.adts e .adt|  
|File audio AU|.au e .snd|  
|File audio AIFF (Audio Interchange File Format)|.aif, .aifc e .aiff|  
|File AVCHD|.m2ts, .m2t e .mts|  
|Disco audio CD|.cda|  
|Disco video DVD|.vob|  
|File di immagine JPEG|.jpg|  
|File audio MP3|.mp3 e .m3u|  
|File video MPEG|.mpeg, .mpg, .m1v, .mpa, .mpe, .mp4, .mp4v, .m4v, .m4a e .mov|  
|File audio e video di Windows|.avi e .wav|  
|File audio e video di Windows Media|.asf, .asx, .wax, .wm, .wma, .wmd, .wmp, .wmv, .wmx, .wpl e .wvx|  
  
> [!NOTE]
>  Se un tipo di file elencato nella tabella non è utilizzabile, potrebbe essere stato codificato con un codec non supportato da Windows Media Player.  
  
 Per altre informazioni sui formati di file supportati, vedere [Tipi di file supportati da Windows Media Player](https://go.microsoft.com/fwlink/p/?LinkID=196118) e l'argomento sui [formati multimediali, protocolli e campi di log supportati](https://go.microsoft.com/fwlink/p/?LinkId=203339) per Silverlight.  
  
## <a name="see-also"></a>Vedere anche  
  
-   [Riprodurre file multimediali digitali](../use/Play-Digital-Media-in-Windows-Server-Essentials.md)  
  
-   [Gestire Windows Server Essentials](Manage-Windows-Server-Essentials.md)
