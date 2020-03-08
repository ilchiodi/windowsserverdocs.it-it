---
title: Usare Accesso Web remoto in Windows Server Essentials
description: Viene descritto come usare Windows Server Essentials
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 47ea21a0-5e05-4b4b-8fa4-338c82601276
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: f6a5d6fd42c5cd7e92821e1157748054c741ef04
ms.sourcegitcommit: 06ae7c34c648538e15c4d9fe330668e7df32fbba
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/05/2020
ms.locfileid: "78371175"
---
# <a name="use-remote-web-access-in-windows-server-essentials"></a>Usare Accesso Web remoto in Windows Server Essentials

>Si applica a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials
  
  Remote Accesso Web è una funzionalità di Windows Server Essentials che consente di accedere a file, cartelle e computer della rete tramite un Web browser da qualsiasi luogo con connettività Internet. 
  
  Accesso Web remoto permette di rimanere connessi alla rete di Windows Server Essentials ovunque ci si trovi. Quando si accede a Accesso Web remoto, è possibile connettersi ai computer nella rete di Windows Server Essentials, aprire il dashboard per gestire la rete di Windows Server Essentials e accedere a tutte le cartelle condivise e ai file multimediali nel server.  
  
 In questo argomento sono incluse le sezioni seguenti:  
  

-   [Connetti a Accesso Web remoto](Use-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_Connect)  
  
-   [Condivisione di file e cartelle](Use-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_SharedFolders)  
  
-   [Connettersi da un dispositivo mobile](Use-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_ConnectMobile)  
  
##  <a name="BKMK_Connect"></a>Connetti a Accesso Web remoto  
  
-   [Accedi a Accesso Web remoto](Use-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_1)  
  
-   [Accedere in remoto al computer](Use-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_1.5)  

-   [Connetti a Accesso Web remoto](../use/Use-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_Connect)  
  
-   [Condivisione di file e cartelle](../use/Use-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_SharedFolders)  
  
-   [Connettersi da un dispositivo mobile](../use/Use-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_ConnectMobile)  
  
##  <a name="BKMK_Connect"></a>Connetti a Accesso Web remoto  
  
-   [Accedi a Accesso Web remoto](../use/Use-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_1)  
  
-   [Accedere in remoto al computer](../use/Use-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_1.5)  

  
###  <a name="BKMK_1"></a>Accedi a Accesso Web remoto  
 Quando si accede a Accesso Web remoto da un computer locale o remoto, è possibile accedere alle risorse del server che esegue Windows Server Essentials e ai computer della rete.  
  
##### <a name="to-log-on-to-remote-web-access-from-a-network-computer"></a>Per accedere ad Accesso Web remoto da un computer di rete  
  
1.  Aprire una Web browser, digitare **https://** _< nomeserver\>_ **/Remote** nella barra degli indirizzi, quindi premere INVIO.  
  
    > [!NOTE]
    >  Assicurarsi di includere le s in HTTPS.  
  
2.  Nella pagina accesso remoto Accesso Web digitare il nome utente e la password nelle caselle di testo e quindi fare clic sulla freccia.  
  
##### <a name="to-log-on-to-remote-web-access-from-a-remote-computer"></a>Per accedere ad Accesso Web remoto da un computer remoto  
  
1.  Aprire una Web browser, digitare **https://** _< NomeDominio\>_ **/Remote** nella barra degli indirizzi, quindi premere INVIO.  
  
    > [!NOTE]
    >  È possibile ottenere le informazioni sul nome di dominio dall'amministratore di rete. Assicurarsi di includere le s in HTTPS.  
  
2.  Nella pagina accesso remoto Accesso Web digitare il nome utente e la password nelle caselle di testo e quindi fare clic sulla freccia.  
  
###  <a name="BKMK_1.5"></a>Accedere in remoto al computer  
 Quando si è fuori sede, è possibile usare la Web browser per accedere al sito Accesso Web remoto per accedere in modalità remota al dashboard di Windows Server Essentials, alle cartelle condivise e ai computer della rete.  
  
 Quando ci si connette al dashboard, è possibile gestire Windows Server Essentials esattamente come se ci si trovasse in ufficio. Si possono eseguire tutte le attività amministrative normali, ad esempio l'aggiunta di account utente, l'aggiunta di cartelle condivise, la configurazione dell'accesso alle cartelle condivise e così via. Quando ci si connette a computer disponibili nella rete, sarà possibile accedere ai desktop dei computer, come se li si stesse usando in ufficio.  
  
 La colonna **Stato** mostra se è possibile connettersi a un computer nella rete e può includere i valori seguenti:  
  
-   **Disponibile**  
  
     Il computer è attivato e disponibile per una connessione remota. Anche se questo stato è visualizzato, è possibile che non si riesca comunque a connettersi a questo computer se la connessione è bloccata da un firewall di terze parti.  
  
-   **Offline o in sospensione**  
  
     Il computer è disattivato o si trova in modalità sospensione o ibernazione. Se un computer è offline o inattivo, lo stato sarà aggiornato in tempo reale, per permettere di sapere quando il computer diventa disponibile.  
  
-   **Sistema operativo non supportato**  
  
     Il sistema operativo del computer non supporta Desktop remoto. In caso di modifica, potrebbero essere necessarie fino a sei ore per l'aggiornamento di questo stato sul server.  
  
-   **Connessione disabilitata**  
  
     La connessione al computer è bloccata da un firewall o da Criteri di gruppo oppure il desktop remoto è disabilitato nel computer. In caso di modifica, potrebbero essere necessarie fino a sei ore per l'aggiornamento di questo stato sul server.  
  
#### <a name="to-connect-to-a-computer-on-your-network"></a>Per connettersi a un computer nella rete  
 Nella scheda **DISPOSITIVI** fare clic sul nome del computer. È possibile selezionare solo i computer con stato **Disponibile**.  
  
#### <a name="to-connect-to-the-server-dashboard"></a>Per connettersi al dashboard del server  
 Nella scheda **DISPOSITIVI** fare clic sul nome del server. È possibile selezionare solo i computer con stato **Disponibile**. Per usare il dashboard, è necessario specificare un account utente e una password da amministratore nel server.  
  
##  <a name="BKMK_SharedFolders"></a>Condivisione di file e cartelle  
  

-   [Caricare e scaricare file in Accesso Web remoto](Use-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_UploadRWA)  
  
-   [Creare, rinominare, spostare, eliminare o copiare file e cartelle in un Accesso Web remoto](Use-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_2)  

-   [Caricare e scaricare file in Accesso Web remoto](../use/Use-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_UploadRWA)  
  
-   [Creare, rinominare, spostare, eliminare o copiare file e cartelle in un Accesso Web remoto](../use/Use-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_2)  

  
###  <a name="BKMK_UploadRWA"></a>Caricare e scaricare file in Accesso Web remoto  
 Nella scheda **Cartelle condivise** di Accesso Web remoto è possibile eseguire le operazioni seguenti:  
  
-   Caricare (inviare) file dal computer a Windows Server Essentials.  
  
    > [!NOTE]
    >  È possibile caricare solo file, non cartelle, in Accesso Web remoto. Se si vuole mantenere la stessa gerarchia di file e cartelle del computer in **Cartelle condivise** nel server, sarà necessario creare le cartelle nel server in Accesso Web remoto, quindi caricare i file nelle cartelle create. Per informazioni sulla creazione di cartelle del server, vedere [Aggiungere o spostare una cartella del server](../manage/Manage-Server-Folders-in-Windows-Server-Essentials.md#BKMK_5).  
  
-   Scaricare (ricevere) file e cartelle da Windows Server Essentials nel computer.  
  
-   Creare una cartella in una cartella condivisa in Windows Server Essentials.  
  

-   Spostare, eliminare e rinominare file e cartelle in Windows Server Essentials. Per altre informazioni, vedere [creare, rinominare, spostare, eliminare o copiare file e cartelle in accesso Web remoto](Use-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_2).  

-   Spostare, eliminare e rinominare file e cartelle in Windows Server Essentials. Per altre informazioni, vedere [creare, rinominare, spostare, eliminare o copiare file e cartelle in accesso Web remoto](../use/Use-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_2).  

  
#### <a name="upload-files"></a>Caricare file  
  
###### <a name="to-upload-files"></a>Per caricare file  
  
1. In Accesso Web remoto fare clic sulla scheda **Cartelle condivise**, quindi selezionare un collegamento di cartella condivisa. Sarà visualizzato un elenco di file e cartelle disponibili nella cartella condivisa.  
  
2. Nell'elenco di file e cartelle disponibili nella cartella condivisa fare clic sulla cartella in cui si vuole caricare il file, quindi fare clic su **Carica**.  
  
3. Se lo strumento di caricamento standard non è già stato caricato, fare clic su **Usa metodo di caricamento standard**.  
  
4. Fare clic su **Sfoglia**  per trovare un file nel computer.  
  
5. Spostarsi tra le cartelle del computer per individuare il file da caricare e quindi fare clic su **Apri**.  
  
6. Ripetere i passaggi 2 e 3 per ogni file da caricare.  
  
7. Dopo avere aggiunto tutti i file da caricare, fare clic su **Carica**.  
  
   Lo strumento caricamento semplice file semplifica il processo di caricamento di file nel server che esegue Windows Server Essentials. È possibile aggiungere il numero desiderato di file allo strumento caricamento semplice file tramite la funzionalità di trascinamento della selezione, quindi caricarli nelle cartelle condivise nel server.  
  
> [!NOTE]
>  Il caricamento di più file è supportato in modalità nativa nei Web browser compatibili con HTML5. Questo strumento è necessario solo se il Web browser non supporta HTML5.  
  
###### <a name="to-upload-files-using-the-easy-file-upload-tool"></a>Per caricare file usando lo strumento caricamento semplice file  
  
1.  In Accesso Web remoto fare clic sulla scheda **Cartelle condivise**, quindi selezionare un collegamento di cartella condivisa. Sarà visualizzato un elenco di file e cartelle disponibili nella cartella condivisa.  
  
2.  Nell'elenco di file e cartelle disponibili nella cartella condivisa fare clic sulla cartella in cui si vuole caricare i file, quindi fare clic su **Carica**. Se la cartella di destinazione del caricamento non esiste, fare clic su **Nuova cartella**, digitare il nome della nuova cartella nella finestra di dialogo, quindi fare clic su **OK**.  
  
3.  Potrebbe essere necessario eseguire il componente aggiuntivo Soluzioni Windows Server. In questo caso, fare clic sulla striscia gialla nella parte superiore della schermata, quindi su **Componente aggiuntivo** e infine su **Esegui** nella finestra di dialogo.  
  
4.  Se lo strumento caricamento semplice file non è già stato caricato, fare clic su **Usa strumento di caricamento semplice**.  
  
5.  È possibile trascinare i file da Esplora risorse allo strumento di caricamento semplice oppure si può fare clic su **individuare i file da selezionare**.  
  
6.  Dopo avere aggiunto i file da caricare nella cartella selezionata, fare clic su **Carica**.  
  
7.  Al termine del caricamento corretto dei file, fare clic su **Chiudi**.  
  
#### <a name="download-files-or-folders"></a>Scaricare file o cartelle  
  
###### <a name="to-download-a-single-file"></a>Per scaricare un singolo file  
  
1. In Accesso Web remoto fare clic sulla scheda **Cartelle condivise**, quindi selezionare un collegamento di cartella condivisa. Sarà visualizzato un elenco di file e cartelle disponibili nella cartella condivisa.  
  
2. Nell'elenco di file di cartelle condivise fare clic sulla casella di controllo accanto al file da scaricare nel computer di casa.  
  
3. Fare clic su **Scarica** per avviare il download.  
  
4. Nella finestra di dialogo **Download del file** fare clic su **Salva** per salvare il file nel computer.  
  
5. Nella finestra di dialogo **Salva con nome** selezionare il percorso in cui salvare il file, quindi fare clic su **Salva**. Un file singolo non sarà compresso prima del download.  
  
   Per il download di più file o cartelle sono disponibili due opzioni. Scegliere l'opzione adatta alle proprie esigenze:  
  
> [!NOTE]
>  Queste opzioni sono disponibili solo se si scaricano più file o cartelle nel computer.  
  
- **File eseguibile autoestraente (. exe)**  
  
  > [!NOTE]
  >   Questa sezione si applica a un server che esegue Windows Server Essentials.  
  
   Un file eseguibile autoestraente è un file scaricabile che unisce il programma di decompressione (eseguibile) ai file compressi. Quando lo si esegue, il programma eseguibile decomprime automaticamente i file compressi (autoestraente). Si tratta di una modalità comune per distribuire i dati compressi senza doversi preoccupare della disponibilità o meno dell'utilità di compressione corretta da parte del destinatario.  
  
  > [!NOTE]
  >  Questa opzione supporta i caratteri Unicode.  
  
- **Cartella compressa di Windows (. zip)**  
  
   La compressione di un file permette di creare una versione compressa del file, con dimensioni inferiori a quelle del file originale. La versione compressa del file avrà un nome file con estensione zip. I tipi di file per cui si ottiene la compressione maggiore sono i file orientati al testo, ad esempio i file con estensione txt, doc, xls, e i file di immagine che usano tipi di file non compressi, ad esempio i file con estensione bmp. Alcuni file di immagine, ad esempio i file con estensione jpg e gif, usano già la compressione, quindi la compressione permetterà solo una riduzione minima delle dimensioni dei file. Se si comprime un documento di Word contenente un numero elevato di elementi grafici si otterrà una riduzione minore rispetto alla compressione di un documento che include principalmente testo.  
  
  > [!NOTE]
  >  Questa opzione offre un supporto limitato per i nomi di file internazionali in Windows Server Essentials.  
  
  Prima dell'inizio del download effettivo, sarà creato il file con estensione exe o zip. In base al numero di file e alla dimensione totale dei file da scaricare, questa operazione potrebbe richiedere alcuni minuti. Dopo la creazione del file di download, i file saranno scaricati in background. Ciò permette di continuare a lavorare durante il completamento del processo di download.  
  
###### <a name="to-download-multiple-files-or-folders"></a>Per scaricare più file o cartelle  
  
1.  In Accesso Web remoto fare clic sulla scheda **Cartelle condivise**, quindi selezionare un collegamento di cartella condivisa. Sarà visualizzato un elenco di file e cartelle disponibili nella cartella condivisa.  
  
2.  Nell'elenco di file di cartelle condivise fare clic sulla casella di controllo accanto ai file o alle cartelle da scaricare nel computer di casa.  
  
3.  Fare clic su **Scarica** per avviare il download.  
  
4.  Nella finestra di dialogo **Scegli un formato per il download** fare clic per selezionare l'opzione preferita per il formato di download, quindi fare clic su **OK**. Il file compresso sarà preparato usando l'opzione selezionata per il formato.  
  
5.  Nella finestra di dialogo **Download del file** fare clic su **Salva** per salvare il file nel computer.  
  
6.  Nella finestra di dialogo **Salva con nome** selezionare il percorso in cui salvare il file, quindi fare clic su **Salva**.  
  
#### <a name="retrieve-compressed-files-downloaded-to-your-computer"></a>Recuperare file compressi scaricati nel computer  
  
> [!NOTE]
>   Questa sezione si applica a un server che esegue Windows Server Essentials.  
  
 Se si selezionano più file o cartelle per il download, sarà possibile ricevere un file eseguibile compresso autoestraente (con estensione exe) o un file compresso (con estensione zip).  
  
###### <a name="to-retrieve-a-file-from-the-compressed-exe-file"></a>Per recuperare un file dal file compresso (con estensione exe)  
  
1.  Nel computer fare doppio clic sul file compresso per aprirlo.  
  
2.  Seguire le istruzioni per estrarre i file in una cartella nel computer.  
  
###### <a name="to-retrieve-a-file-from-the-compressed-zip-file"></a>Per recuperare un file dal file compresso (con estensione zip)  
  
1.  Nel computer fare doppio clic sul file compresso per aprirlo.  
  
2.  Selezionare i file da recuperare, quindi trascinarli nella cartella del computer da usare per l'archiviazione.  
  
    > [!NOTE]
    >  Se si usa un programma di compressione file di terze parti, seguire le procedure del programma specifico per estrarre i file dal file compresso.  
  
###  <a name="BKMK_2"></a>Creare, rinominare, spostare, eliminare o copiare file e cartelle in un Accesso Web remoto  
 È possibile usare Accesso Web remoto per creare nuove cartelle in una cartella condivisa esistente, per rinominare file e cartelle, per spostare e copiare file e cartelle e per eliminare file e cartelle dal server.  
  
> [!NOTE]
>  Per aggiungere nuove cartelle condivise in un server che esegue Windows Server Essentials, è necessario usare il dashboard. Per connettersi alla console del server da Accesso Web remoto, nella scheda **Computer** fare clic sul nome del server, quindi su **Connetti** e infine seguire le istruzioni per l'accesso al server. Per informazioni su come creare cartelle condivise, vedere [Aggiungere o spostare una cartella del server](../manage/Manage-Server-Folders-in-Windows-Server-Essentials.md#BKMK_5).  
  
##### <a name="to-create-a-new-folder"></a>Per creare una nuova cartella  
  
1.  In Accesso Web remoto fare clic sulla scheda **Cartelle condivise**, quindi selezionare un collegamento di cartella condivisa. Sarà visualizzato un elenco di file e cartelle disponibili nella cartella condivisa.  
  
2.  Sulla barra delle applicazioni fare clic su **Nuova cartella**.  
  
3.  Digitare un nome per cartella, quindi fare clic su **OK**.  
  
##### <a name="to-rename-a-file-or-folder"></a>Per rinominare un file o una cartella  
  
1.  In Accesso Web remoto fare clic sulla scheda **Cartelle condivise**, quindi selezionare un collegamento di cartella condivisa. Sarà visualizzato un elenco di file e cartelle disponibili nella cartella condivisa.  
  
2.  Fare clic con il pulsante destro del mouse sul file o sulla cartella da rinominare, quindi scegliere **Rinomina**.  
  
3.  Digitare un nuovo nome nella casella di testo, quindi fare clic su **OK**.  
  
##### <a name="to-move-files-or-folders"></a>Per spostare file o cartelle  
  
1.  In Accesso Web remoto fare clic sulla scheda **Cartelle condivise**, quindi selezionare un collegamento di cartella condivisa. Sarà visualizzato un elenco di file e cartelle disponibili nella cartella condivisa.  
  
2.  Selezionare la casella di controllo accanto ai file o alle cartelle da spostare, fare clic con il pulsante destro del mouse su uno dei file o delle cartelle selezionati, quindi scegliere **Taglia**.  
  
3.  Fare clic con il pulsante destro del mouse sulla cartella in cui si vogliono spostare i file o le cartelle, quindi scegliere **Incolla**.  
  
##### <a name="to-delete-a-file-or-folder"></a>Per eliminare un file o una cartella  
  
1.  In Accesso Web remoto fare clic sulla scheda **Cartelle condivise**, quindi selezionare un collegamento di cartella condivisa. Sarà visualizzato un elenco di file e cartelle disponibili nella cartella condivisa.  
  
2.  Selezionare la casella di controllo accanto ai file o alle cartelle da eliminare, fare clic con il pulsante destro del mouse su uno dei file o delle cartelle selezionati, quindi scegliere **Elimina**.  
  
3.  Per confermare che si vogliono eliminare i file e le cartelle selezionati, fare clic su **Sì**.  
  
##### <a name="to-copy-files-or-folders"></a>Per copiare file o cartelle  
  
1.  In Accesso Web remoto fare clic sulla scheda **Cartelle condivise**, quindi selezionare un collegamento di cartella condivisa. Sarà visualizzato un elenco di file e cartelle disponibili nella cartella condivisa.  
  
2.  Selezionare la casella di controllo accanto ai file o alle cartelle da copiare, fare clic con il pulsante destro del mouse su uno dei file o delle cartelle selezionati, quindi scegliere **Copia**.  
  
3.  Fare clic con il pulsante destro del mouse sulla cartella in cui si vogliono copiare i file o le cartelle, quindi scegliere **Incolla**.  
  
##  <a name="BKMK_ConnectMobile"></a>Connettersi da un dispositivo mobile  
  

-   [Usare Accesso Web remoto da un dispositivo mobile](Use-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_8)  
  
-   [Web browser supportati per i dispositivi mobili](Use-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_9)  

-   [Usare Accesso Web remoto da un dispositivo mobile](../use/Use-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_8)  
  
-   [Web browser supportati per i dispositivi mobili](../use/Use-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_9)  

  
###  <a name="BKMK_8"></a>Usare Accesso Web remoto da un dispositivo mobile  
 È possibile accedere ad Accesso Web remoto dallo smartphone per visualizzare i file e le cartelle disponibili nelle cartelle condivise nel server.  
  
> [!NOTE]
>  È anche possibile scaricare e usare l'app My Server per Windows Server Essentials da [Windows Phone Marketplace](http://www.windowsphone.com/apps/6c2f98d5-6fcf-4e1d-b8b1-cde62ea1a94a) per accedere alle cartelle condivise e ai file multimediali archiviati sul server.  
  
##### <a name="to-log-on-to-remote-web-access-from-a-mobile-device"></a>Per connettersi ad Accesso Web remoto da un dispositivo mobile  
  
1.  Aprire una Web browser e digitare **https://** _< NomeDominio\>_ **/Remote** nella barra degli indirizzi.  Assicurarsi di includere le s in HTTPS.  
  
2.  Nella pagina accesso remoto Accesso Web digitare il nome utente e la password nelle caselle di testo e quindi fare clic sulla freccia. Si è connessi alla versione di Accesso Web remoto per dispositivi mobili.  
  
##### <a name="to-switch-to-the-desktop-version-of-remote-web-access"></a>Per passare alla versione desktop di Accesso Web remoto  
  
1.  Aprire una Web browser e digitare **https://** _< NomeDominio\>_ **/Remote** nella barra degli indirizzi.  Assicurarsi di includere le s in HTTPS.  
  
2.  Nella pagina accesso remoto Accesso Web digitare il nome utente e la password nelle caselle di testo, fare clic su **Visualizza versione desktop**, quindi fare clic sulla freccia. Si è connessi alla versione desktop di Accesso Web remoto.  
  
##### <a name="to-return-to-the-mobile-version-of-remote-web-access"></a>Per tornare alla versione di Accesso Web remoto per dispositivi mobili  
  
1. Disconnettersi.  
  
2. Aprire una Web browser e digitare **https://** _< NomeDominio\>_ **/Remote/m** nella barra degli indirizzi. Assicurarsi di includere le s in HTTPS.  
  
3. Viene visualizzata la versione per dispositivi mobili di Accesso Web remoto. Nella pagina accesso remoto Accesso Web digitare il nome utente e la password nelle caselle di testo e quindi fare clic sulla freccia. Si è connessi alla versione per dispositivi mobili di Accesso Web remoto.  
  
   È possibile cercare file e cartelle nelle cartelle condivise sul server.  
  
###  <a name="BKMK_9"></a>Web browser supportati per i dispositivi mobili  
 I Web browser supportati per i dispositivi mobili includono i seguenti:  
  
-   Internet Explorer Mobile 6.0 o versioni successive  
  
-   Safari  
  
-   Blackberry  
  
-   Symbian 6.0 o versioni successive  
  
-   Android  
  
-   Google Chrome  
  
-   Firefox  
  
## <a name="see-also"></a>Vedere anche  
  
-   [Gestisci Accesso Web Remote](../manage/Manage-Remote-Web-Access-in-Windows-Server-Essentials.md)  
  

-   [Lavorare in modalità remota](Work-Remotely-in-Windows-Server-Essentials.md)  
  
-   [Usare Windows Server Essentials](Use-Windows-Server-Essentials.md)

-   [Lavorare in modalità remota](../use/Work-Remotely-in-Windows-Server-Essentials.md)  
  
-   [Usare Windows Server Essentials](../use/Use-Windows-Server-Essentials.md)

