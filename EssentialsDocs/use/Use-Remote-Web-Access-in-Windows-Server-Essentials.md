---
title: Usare accesso Web remoto in Windows Server Essentials
description: Viene descritto come utilizzare Windows Server Essentials
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
ms.openlocfilehash: ac384b545fe61b4a832debdc8aedcc81a75c669a
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2017
---
# <a name="use-remote-web-access-in-windows-server-essentials"></a>Usare accesso Web remoto in Windows Server Essentials

>Si applica a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials
  
  Accesso Web remoto permette di rimanere connessi alla rete di Windows Server Essentials ovunque ci si trovi. Quando effettua l'accesso ad accesso Web remoto, è possibile connettersi ai computer nella rete di Windows Server Essentials, aprire il Dashboard per gestire la rete di Windows Server Essentials e accedere a tutti i file di cartelle e file multimediali condivisi sul server.  
  
 In questo argomento include le sezioni seguenti:  
  

-   [Connettersi a accesso Web remoto](Use-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_Connect)  
  
-   [Condivisione file e cartelle](Use-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_SharedFolders)  
  
-   [La connessione da un dispositivo mobile](Use-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_ConnectMobile)  
  
##  <a name="BKMK_Connect"></a>Connettersi a accesso Web remoto  
  
-   [Accedere ad accesso Web remoto](Use-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_1)  
  
-   [Accedere in remoto ai computer](Use-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_1.5)  

-   [Connettersi a accesso Web remoto](../use/Use-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_Connect)  
  
-   [Condivisione file e cartelle](../use/Use-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_SharedFolders)  
  
-   [La connessione da un dispositivo mobile](../use/Use-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_ConnectMobile)  
  
##  <a name="BKMK_Connect"></a>Connettersi a accesso Web remoto  
  
-   [Accedere ad accesso Web remoto](../use/Use-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_1)  
  
-   [Accedere in remoto ai computer](../use/Use-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_1.5)  

  
###  <a name="BKMK_1"></a>Accedere ad accesso Web remoto  
 Quando si accede ad accesso Web remoto da un computer locale o remoto, è possibile accedere alle risorse sul server che esegue Windows Server Essentials e ai computer della rete.  
  
##### <a name="to-log-on-to-remote-web-access-from-a-network-computer"></a>Per accedere ad accesso Web remoto da un computer di rete  
  
1.  Aprire un Web browser, digitare **https://***< YourServerName\ >***/remoto** nella barra degli indirizzi e quindi premere INVIO.  
  
    > [!NOTE]
    >  Assicurati di includere il s https.  
  
2.  Nella pagina di accesso di accesso Web remoto, digitare il nome utente e password nelle caselle di testo e quindi fare clic sulla freccia.  
  
##### <a name="to-log-on-to-remote-web-access-from-a-remote-computer"></a>Per accedere ad accesso Web remoto da un computer remoto  
  
1.  Aprire un Web browser, digitare **https://***< YourDomainName\ >***/remoto** nella barra degli indirizzi e quindi premere INVIO.  
  
    > [!NOTE]
    >  È possibile ottenere le informazioni sul nome di dominio dall'amministratore di rete. Assicurati di includere il s https.  
  
2.  Nella pagina di accesso di accesso Web remoto, digitare il nome utente e password nelle caselle di testo e quindi fare clic sulla freccia.  
  
###  <a name="BKMK_1.5"></a>Accedere in remoto ai computer  
 Quando si è fuori sede, è possibile utilizzare il Web browser per accedere al sito accesso Web remoto per accedere in modalità remota di Windows Server Essentials Dashboard e le cartelle condivise, computer della rete.  
  
 Quando si connette al Dashboard, è possibile gestire Windows Server Essentials come se trovasse in ufficio. È possibile eseguire tutte le attività amministrative normali, ad esempio l'aggiunta di account utente, aggiunta di cartelle condivise, l'impostazione di accesso alle cartelle condivise e così via. Quando si connette al computer della rete, è possibile accedere ai desktop come se tu fossi li in ufficio.  
  
 Il **stato** colonna Mostra se è possibile connettersi a un computer della rete e può includere i valori seguenti:  
  
-   **Disponibile**  
  
     Il computer è attivato ed è disponibile per una connessione remota. Anche se questo stato è visualizzato, è comunque potrebbe non essere in grado di connettersi al computer, se un firewall di terze parti che blocca la connessione.  
  
-   **Offline o inattivo**  
  
     Il computer è disattivato o si trova in modalità sospensione o ibernazione. Se un computer è offline o inattivo, lo stato viene aggiornato in tempo reale in modo da sapere quando il computer diventa disponibile.  
  
-   **Sistema operativo non supportato**  
  
     Il sistema operativo nel computer non supporta Desktop remoto. Può richiedere fino a 6 ore per essere aggiornati nel server se è presente una modifica di questo stato.  
  
-   **Connessione disattivata**  
  
     La connessione al computer è bloccata da un firewall o il desktop remoto è disabilitato nel computer o tramite criteri di gruppo. Può richiedere fino a 6 ore per essere aggiornati nel server se è presente una modifica di questo stato.  
  
#### <a name="to-connect-to-a-computer-on-your-network"></a>Per connettersi a un computer in rete  
 Nel **dispositivi** scheda, fare clic sul nome del computer. È possibile selezionare solo i computer con un **disponibile** stato.  
  
#### <a name="to-connect-to-the-server-dashboard"></a>Per connettersi al Dashboard del server  
 Nel **dispositivi** scheda, fare clic sul nome del server. È possibile selezionare solo i computer con un **disponibile** stato. È necessario essere in grado di fornire un account utente e una password nel server a usare il Dashboard.  
  
##  <a name="BKMK_SharedFolders"></a>Condivisione file e cartelle  
  

-   [Caricare e scaricare file in accesso Web remoto](Use-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_UploadRWA)  
  
-   [Creare, rinominare, spostare, eliminare o copiare file e cartelle in accesso Web remoto](Use-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_2)  

-   [Caricare e scaricare file in accesso Web remoto](../use/Use-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_UploadRWA)  
  
-   [Creare, rinominare, spostare, eliminare o copiare file e cartelle in accesso Web remoto](../use/Use-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_2)  

  
###  <a name="BKMK_UploadRWA"></a>Caricare e scaricare file in accesso Web remoto  
 In accesso Web remoto **cartelle condivise** scheda, è possibile eseguire le operazioni seguenti:  
  
-   Caricare (inviare) file dal computer a Windows Server Essentials.  
  
    > [!NOTE]
    >  È possibile caricare solo file e cartelle non ad accesso Web remoto. Se si desidera che la stessa gerarchia di file e cartelle di **cartelle condivise** nel server del computer, è necessario creare le cartelle nel server di accesso Web remoto e quindi caricare i file nella cartella creata. Per informazioni sulla creazione di cartelle del server, vedere [aggiungere o spostare una cartella del server](../manage/Manage-Server-Folders-in-Windows-Server-Essentials.md#BKMK_5).  
  
-   Scaricare (ricevere) file e cartelle da Windows Server Essentials al computer.  
  
-   Creare una cartella all'interno di una cartella condivisa in Windows Server Essentials.  
  

-   Spostare, eliminare e rinominare file e cartelle in Windows Server Essentials. Per ulteriori informazioni, vedere [crea, Rinomina, spostare, eliminare o copiare file e cartelle in accesso Web remoto](Use-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_2).  

-   Spostare, eliminare e rinominare file e cartelle in Windows Server Essentials. Per ulteriori informazioni, vedere [crea, Rinomina, spostare, eliminare o copiare file e cartelle in accesso Web remoto](../use/Use-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_2).  

  
#### <a name="upload-files"></a>Caricare file  
  
###### <a name="to-upload-files"></a>Per caricare file  
  
1.  In accesso Web remoto, fare clic su di **cartelle condivise** scheda e quindi fare clic su un collegamento di cartella condivisa. Viene visualizzato un elenco dei file e cartelle nella cartella condivisa.  
  
2.  Dall'elenco di cartelle condivise di file e cartelle, fare clic sulla cartella in cui si desidera caricare il file, quindi fare clic su **caricare**.  
  
3.  Se lo strumento di caricamento standard non è già stato caricato, fare clic su **utilizzare il metodo di caricamento standard**.  
  
4.  Fare clic su **Sfoglia** per trovare un file nel computer.  
  
5.  Spostarsi tra le cartelle del computer per trovare il file che si desidera caricare, quindi fare clic su **aprire**.  
  
6.  Ripetere i passaggi 2 e 3 per ogni file che si desidera caricare.  
  
7.  Dopo aver aggiunto tutti i file che si desidera caricare, fare clic su **caricare**.  
  
 Lo strumento caricamento semplice File semplifica il processo di caricamento file nel server che esegue Windows Server Essentials. Puoi aggiungere il numero di file che si desidera lo strumento caricamento semplice File tramite la funzionalità di trascinamento e e quindi caricarli nelle cartelle condivise sul server.  
  
> [!NOTE]
>  Caricamento di più file in modo nativo è supportato nei web browser compatibili con HTML5. Questo strumento è necessario solo quando il web browser non supporta HTML5.  
  
###### <a name="to-upload-files-using-the-easy-file-upload-tool"></a>Per caricare file usando lo strumento caricamento semplice File  
  
1.  In accesso Web remoto, fare clic su di **cartelle condivise** scheda e quindi fare clic su un collegamento di cartella condivisa. Viene visualizzato un elenco dei file e cartelle nella cartella condivisa.  
  
2.  Dall'elenco di cartelle condivise di file e cartelle, fare clic sulla cartella in cui si desidera caricare i file, quindi fare clic su **caricare**. Se la cartella che si desidera caricare non esiste, fare clic su **nuova cartella**, digitare il nome della nuova cartella nella finestra di dialogo e quindi fare clic su **OK**.  
  
3.  Potrebbe essere necessario eseguire il componente aggiuntivo soluzioni Windows Server. In tal caso, fare clic sulla striscia gialla nella parte superiore dello schermo, fare clic su Esegui **componente aggiuntivo**, quindi fare clic su **eseguire** nella finestra di dialogo.  
  
4.  Se lo strumento caricamento semplice File non è già stato caricato, fare clic su **utilizzare lo strumento caricamento semplice File**.  
  
5.  È possibile trascinare e i file da Esplora risorse allo strumento caricamento semplice File, o fare clic su **Sfoglia per selezionare i file**.  
  
6.  Dopo avere aggiunto i file che si desidera caricare nella cartella selezionata, fare clic su **caricare**.  
  
7.  Quando i file vengono caricati correttamente, fare clic su **Chiudi**.  
  
#### <a name="download-files-or-folders"></a>Scaricare file o cartelle  
  
###### <a name="to-download-a-single-file"></a>Per scaricare un singolo file  
  
1.  In accesso Web remoto, fare clic su di **cartelle condivise** scheda e quindi fare clic su un collegamento di cartella condivisa. Viene visualizzato un elenco dei file e cartelle nella cartella condivisa.  
  
2.  Nell'elenco di file di cartelle condivise, fare clic sulla casella di controllo accanto al file che si desidera scaricare nel computer di casa.  
  
3.  Fare clic su **scaricare** per avviare il download.  
  
4.  Nel **Download del File** la finestra di dialogo, fare clic su **salvare** per salvare il file nel computer.  
  
5.  Nel **Salva con nome** finestra di dialogo, selezionare il percorso in cui salvare il file, quindi fare clic su **salvare**. Un singolo file non sarà compresso prima che è stato scaricato.  
  
 Sono disponibili due opzioni per il download di più file o cartelle. Scegliere l'opzione adatta alle proprie esigenze:  
  
> [!NOTE]
>  Queste opzioni sono disponibili solo quando si scaricano più file o cartelle nel computer.  
  
-   **File eseguibile autoestraente (.exe)**  
  
    > [!NOTE]
    >   In questa sezione si applica a un server che esegue Windows Server Essentials.  
  
     Un file eseguibile autoestraente è un file scaricabile che unisce il programma di decompressione (eseguibile) i file compressi. Quando si esegue il programma eseguibile decomprime automaticamente i file compressi (autoestraente). Questo è un modo comune per distribuire i dati compressi senza doversi preoccupare indica se il destinatario è l'utilità decompressione destra.  
  
    > [!NOTE]
    >  Questa opzione supporta i caratteri Unicode.  
  
-   **Cartella compressa di Windows (con estensione zip)**  
  
     Compressione di un file crea una versione compressa del file è minore del file originale. La versione compressa del file con estensione zip. Tipi di file che sono Ottiene la compressione maggiore sono i tipi di file orientati al testo, ad esempio con estensione txt, doc, xls, e i file di immagine che usano tipi di file non compressi, ad esempio BMP. Alcuni file di immagine, ad esempio i file con estensione jpg e GIF, usano già la compressione e viene ridotto le dimensioni del file poco compressione. Inoltre, un documento di Word contenente un numero elevato di elementi grafici non viene ridotto per quanto un documento che è principalmente testo.  
  
    > [!NOTE]
    >  Questa opzione offre supporto limitato per nomi di file internazionali in Windows Server Essentials.  
  
 Prima di inizia il download effettivo, viene creato il file con estensione exe o zip. A seconda del numero di file e le dimensioni totali dei file da scaricare, ciò potrebbe richiedere alcuni minuti. Dopo aver creato il file di download, il download del file avviene in background. Ciò consente di continuare a lavorare durante il processo di download di completamento.  
  
###### <a name="to-download-multiple-files-or-folders"></a>Per scaricare più file o cartelle  
  
1.  In accesso Web remoto, fare clic su di **cartelle condivise** scheda e quindi fare clic su un collegamento di cartella condivisa. Viene visualizzato un elenco dei file e cartelle nella cartella condivisa.  
  
2.  Nell'elenco di file di cartelle condivise, fare clic sulla casella di controllo accanto ai file o cartelle che si desidera scaricare nel computer di casa.  
  
3.  Fare clic su **scaricare** per avviare il download.  
  
4.  Nel **scegliere un formato per il Download** la finestra di dialogo, fare clic per selezionare l'opzione di formato di download che si preferisce e quindi fare clic su **OK**. Il file compresso sarà preparato usando l'opzione di formato selezionato.  
  
5.  Nel **Download del File** la finestra di dialogo, fare clic su **salvare** per salvare il file nel computer.  
  
6.  Nel **Salva con nome** finestra di dialogo, selezionare il percorso in cui salvare il file, quindi fare clic su **salvare**.  
  
#### <a name="retrieve-compressed-files-downloaded-to-your-computer"></a>Recuperare file compressi scaricati nel computer  
  
> [!NOTE]
>   In questa sezione si applica a un server che esegue Windows Server Essentials.  
  
 Se si seleziona più file o cartelle per il download, è possibile ricevere un file eseguibile compresso autoestraente (.exe) o un file compresso (con estensione zip).  
  
###### <a name="to-retrieve-a-file-from-the-compressed-exe-file"></a>Per recuperare un file dal file compresso (.exe)  
  
1.  Nel computer, fare doppio clic sul file compresso per aprirlo.  
  
2.  Seguire le istruzioni per estrarre i file in una cartella nel computer.  
  
###### <a name="to-retrieve-a-file-from-the-compressed-zip-file"></a>Per recuperare un file dal file compresso (con estensione zip)  
  
1.  Nel computer, fare doppio clic sul file compresso per aprirlo.  
  
2.  Selezionare i file che si desidera recuperare e quindi trascinare i file in una cartella sul computer in cui si desidera archiviarli.  
  
    > [!NOTE]
    >  Se si utilizza un programma di compressione del file di terze parti, seguire le procedure per il programma per estrarre i file dal file compresso.  
  
###  <a name="BKMK_2"></a>Creare, rinominare, spostare, eliminare o copiare file e cartelle in accesso Web remoto  
 È possibile usare accesso Web remoto per creare nuove cartelle in una cartella condivisa esistente, per rinominare file e cartelle, per spostare e copiare file e cartelle e per eliminare file e cartelle nel server.  
  
> [!NOTE]
>  Per aggiungere nuove cartelle condivise in un server che esegue Windows Server Essentials, è necessario utilizzare il Dashboard. Connettersi alla console di server da accesso Web remoto, nel **computer** scheda, fare clic sul nome del server, fare clic su **Connetti**e quindi segui le istruzioni per l'accesso al server. Per informazioni su come creare le cartelle condivise, vedere [aggiungere o spostare una cartella del server](../manage/Manage-Server-Folders-in-Windows-Server-Essentials.md#BKMK_5).  
  
##### <a name="to-create-a-new-folder"></a>Per creare una nuova cartella  
  
1.  In accesso Web remoto, fare clic su di **cartelle condivise** scheda e quindi fare clic su un collegamento di cartella condivisa. Viene visualizzato un elenco dei file e cartelle nella cartella condivisa.  
  
2.  Nella barra delle applicazioni, fare clic su **nuova cartella**.  
  
3.  Digitare un nome per la cartella, quindi fare clic su **OK**.  
  
##### <a name="to-rename-a-file-or-folder"></a>Per rinominare un file o una cartella  
  
1.  In accesso Web remoto, fare clic su di **cartelle condivise** scheda e quindi fare clic su un collegamento di cartella condivisa. Viene visualizzato un elenco dei file e cartelle nella cartella condivisa.  
  
2.  Fare doppio clic su file o della cartella che si desidera rinominare e quindi fare clic su **rinominare**.  
  
3.  Digitare un nuovo nome nella casella di testo, quindi fare clic su **OK**.  
  
##### <a name="to-move-files-or-folders"></a>Per spostare file o cartelle  
  
1.  In accesso Web remoto, fare clic su di **cartelle condivise** scheda e quindi fare clic su un collegamento di cartella condivisa. Viene visualizzato un elenco dei file e cartelle nella cartella condivisa.  
  
2.  Selezionare la casella di controllo accanto ai file o cartelle che si desidera spostare, fare doppio clic su uno dei file selezionati o delle cartelle e quindi fare clic su **Taglia**.  
  
3.  Fare clic sulla cartella che si desidera spostare i file o cartelle, quindi fare clic su **Incolla**.  
  
##### <a name="to-delete-a-file-or-folder"></a>Per eliminare un file o una cartella  
  
1.  In accesso Web remoto, fare clic su di **cartelle condivise** scheda e quindi fare clic su un collegamento di cartella condivisa. Viene visualizzato un elenco dei file e cartelle nella cartella condivisa.  
  
2.  Selezionare la casella di controllo accanto ai file o cartelle che si desidera eliminare, fare doppio clic su uno dei file selezionati o delle cartelle e quindi fare clic su **eliminare**.  
  
3.  Per confermare che si desidera eliminare il file e cartelle selezionati, fare clic su **Sì**.  
  
##### <a name="to-copy-files-or-folders"></a>Per copiare file o cartelle  
  
1.  In accesso Web remoto, fare clic su di **cartelle condivise** scheda e quindi fare clic su un collegamento di cartella condivisa. Viene visualizzato un elenco dei file e cartelle nella cartella condivisa.  
  
2.  Selezionare la casella di controllo accanto ai file o cartelle che si desidera copiare, fare doppio clic su uno dei file selezionati o delle cartelle e quindi fare clic su **copia**.  
  
3.  Fare clic sulla cartella che si desidera copiare i file o cartelle, quindi fare clic su **Incolla**.  
  
##  <a name="BKMK_ConnectMobile"></a>La connessione da un dispositivo mobile  
  

-   [Usare accesso Web remoto da un dispositivo mobile](Use-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_8)  
  
-   [Web browser supportati per i dispositivi mobili](Use-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_9)  

-   [Usare accesso Web remoto da un dispositivo mobile](../use/Use-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_8)  
  
-   [Web browser supportati per i dispositivi mobili](../use/Use-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_9)  

  
###  <a name="BKMK_8"></a>Usare accesso Web remoto da un dispositivo mobile  
 È possibile accedere ad accesso Web remoto dallo Smartphone per visualizzare i file e cartelle nelle cartelle condivise sul server.  
  
> [!NOTE]
>  È anche possibile scaricare e usare l'app My Server per Windows Server Essentials dal [Windows Phone Marketplace](http://www.windowsphone.com/apps/6c2f98d5-6fcf-4e1d-b8b1-cde62ea1a94a) per accedere ai file di cartelle e file multimediali condivisi archiviati sul server.  
  
##### <a name="to-log-on-to-remote-web-access-from-a-mobile-device"></a>Per accedere ad accesso Web remoto da un dispositivo mobile  
  
1.  Aprire un Web browser e digitare **https://***< YourDomainName\ >***/remoto** nella barra degli indirizzi.  Assicurati di includere il s https.  
  
2.  Nella pagina di accesso di accesso Web remoto, digitare il nome utente e password nelle caselle di testo e quindi fare clic sulla freccia. Si è connessi alla versione mobile di accesso Web remoto.  
  
##### <a name="to-switch-to-the-desktop-version-of-remote-web-access"></a>Per passare alla versione desktop di accesso Web remoto  
  
1.  Aprire un Web browser e digitare **https://***< YourDomainName\ >***/remoto** nella barra degli indirizzi.  Assicurati di includere il s https.  
  
2.  Nella pagina di accesso di accesso Web remoto, digitare il nome utente e password nelle caselle di testo, fare clic su **Visualizza versione desktop**e quindi fare clic sulla freccia. Si è connessi alla versione desktop di accesso Web remoto.  
  
##### <a name="to-return-to-the-mobile-version-of-remote-web-access"></a>Per tornare alla versione mobile di accesso Web remoto  
  
1.  Disconnettersi.  
  
2.  Aprire un Web browser e digitare **https://***< YourDomainName\ >***/remota o m** nella barra degli indirizzi. Assicurati di includere il s https.  
  
3.  Viene visualizzata la versione mobile di accesso Web remoto. Nella pagina di accesso di accesso Web remoto, digitare il nome utente e password nelle caselle di testo e quindi fare clic sulla freccia. Si è connessi alla versione mobile di accesso Web remoto.  
  
 È possibile cercare file e cartelle nelle cartelle condivise sul server.  
  
###  <a name="BKMK_9"></a>Web browser supportati per i dispositivi mobili  
 Web browser supportati per i dispositivi mobili includono:  
  
-   Internet Explorer Mobile 6.0 o versione successiva  
  
-   Safari  
  
-   BlackBerry  
  
-   Symbian 6.0 o versione successiva  
  
-   Android  
  
-   Google Chrome  
  
-   Firefox  
  
## <a name="see-also"></a>Vedere anche  
  
-   [Gestire accesso Web remoto](../manage/Manage-Remote-Web-Access-in-Windows-Server-Essentials.md)  
  

-   [Lavorare in modalità remota](Work-Remotely-in-Windows-Server-Essentials.md)  
  
-   [Usare Windows Server Essentials](Use-Windows-Server-Essentials.md)

-   [Lavorare in modalità remota](../use/Work-Remotely-in-Windows-Server-Essentials.md)  
  
-   [Usare Windows Server Essentials](../use/Use-Windows-Server-Essentials.md)

