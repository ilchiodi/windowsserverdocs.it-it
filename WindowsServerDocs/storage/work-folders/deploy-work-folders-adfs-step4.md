---
title: 'Distribuire Cartelle di lavoro con AD FS e Proxy dell''applicazione Web: passaggio 4, configurare Proxy dell''applicazione Web'
ms.prod: windows-server-threshold
ms.technology: storage-work-folders
ms.topic: article
manager: klaasl
ms.author: jeffpatt
author: JeffPatt24
ms.date: 6/242017
ms.assetid: 4a11ede0-b000-4188-8190-790971504e17
ms.openlocfilehash: 1f452fd1e2f054c449660eb0ee12642fefe4da8f
ms.sourcegitcommit: 583355400f6b0d880dc0ac6bc06f0efb50d674f7
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 10/17/2017
---
# <a name="deploy-work-folders-with-ad-fs-and-web-application-proxy-step-4-set-up-web-application-proxy"></a>Distribuire Cartelle di lavoro con AD FS e Proxy applicazione Web: passaggio 4, configurare Proxy applicazione Web

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016

Questo argomento descrive il quarto passaggio nella distribuzione di Cartelle di lavoro con Active Directory Federation Services (AD FS) e Proxy applicazione Web. È possibile trovare gli altri passaggi di questo processo negli argomenti seguenti:  
  
-   [Distribuire Cartelle di lavoro con AD FS e Proxy dell'applicazione Web: Panoramica](deploy-work-folders-adfs-overview.md)  
  
-   [Distribuire Cartelle di lavoro con AD FS e Proxy dell'applicazione Web: passaggio 1, configurare AD FS](deploy-work-folders-adfs-step1.md)  
  
-   [Distribuire Cartelle di lavoro con AD FS e Proxy dell'applicazione Web: passaggio 2, lavoro post-configurazione di AD FS](deploy-work-folders-adfs-step2.md)  
  
-   [Distribuire Cartelle di lavoro con AD FS e Proxy dell'applicazione Web: passaggio 3, configurare Cartelle di lavoro](deploy-work-folders-adfs-step3.md)  
  
-   [Distribuire Cartelle di lavoro con AD FS e Proxy applicazione Web: passaggio 5, configurare i client](deploy-work-folders-adfs-step5.md)  

> [!NOTE]
>   Le istruzioni descritte in questa sezione sono per un ambiente Windows Server 2016. Se usi Windows Server 2012 R2, segui le [istruzioni di Windows Server 2012 R2](https://technet.microsoft.com/library/dn747208(v=ws.11).aspx).

Per configurare Proxy applicazione Web per l'utilizzo con Cartelle di lavoro, utilizzare le procedure seguenti.  
  
## <a name="install-the-ad-fs-and-work-folder-certificates"></a>Installare i certificati di AD FS e Cartelle di lavoro  
È necessario installare i certificati di AD FS e Cartelle di lavoro creati in precedenza (nel passaggio 1, Configurare AD FS e nel passaggio 3, Installare Cartelle di lavoro) nell'archivio certificati del computer locale nel computer su cui verrà installato il ruolo di Proxy applicazione Web.  
  
Poiché si stanno installando i certificati autofirmati che non risalgono a un server di pubblicazione nell'archivio certificati Autorità di certificazione radice attendibili, è inoltre necessario copiare i certificati in tale archivio.  
  
Per installare i certificati, effettuare le seguenti operazioni:  
  
1.  Fare clic su **Start**, quindi scegliere **Esegui**.  
  
2.  Tipo **MMC**.  
  
3.  Fai clic su **Aggiungi/Rimuovi snap-in** dal menu **File**.  
  
4.  Nella lista **Snap-in disponibili**, seleziona **Certificati**, quindi fai clic su **Aggiungi**. Verrà avviata la procedura guidata Snap-in certificati.  
  
5.  Seleziona **Account del computer** quindi fai clic su **Avanti**.  
  
6.  Seleziona **Computer locale: (il computer su cui è in esecuzione questa console)**, quindi fai clic su **Fine**.  
  
7.  Fai clic su **OK**.  
  
8.  Espandi la cartella **Console Root\Certificates\(Local Computer)\Personal\Certificates**.  
  
9. Fai clic con il pulsante destro del mouse su **Certificati**, fai clic su **Tutte le attività**, quindi fai clic su **Importa**.  
  
10. Individuare la cartella contenente il certificato di AD FS e seguire le istruzioni della procedura guidata per importare il file e inserirlo nell'archivio certificati.  
  
11. Ripetere i passaggi 9 e 10, questa volta individuando il certificato di Cartelle di lavoro e importandolo.  
  
12. Espandi la cartella **Console Root\Certificates\(Local Computer)\Trusted Root Certification Authorities\Certificates**.  
  
13. Fai clic con il pulsante destro del mouse su **Certificati**, fai clic su **Tutte le attività**, quindi fai clic su **Importa**.  
  
14. Individuare la cartella contenente il certificato di AD FS e seguire le istruzioni della procedura guidata per importare il file e inserirlo nell'archivio Autorità di certificazione radice attendibili.  
  
15. Ripetere i passaggi 13 e 14, questa volta individuando il certificato di Cartelle di lavoro e importandolo.  
  
## <a name="install-web-application-proxy"></a>Installare Proxy applicazione Web  
Per installare Proxy applicazione Web, attenersi alla seguente procedura:  
  
1.  Nel server in cui si prevede di installare Proxy applicazione Web, aprire **Server Manager** e avviare la procedura guidata **Aggiungi ruoli e funzionalità**.  
  
2.  Fare clic su **Avanti** nelle pagine prima e seconda della procedura guidata.  
  
3.  Nella pagina **Selezione dei server**, selezionare il server, quindi fare clic su **Avanti**.  
  
4.  Nella pagina **Ruolo del server**, selezionare il ruolo **Accesso remoto**, quindi fare clic su **Avanti**.  
  
5.  Nella pagina Funzionalità e nella pagina Accesso remoto, fare clic su **Avanti**.  
  
6.  Nella pagina **Servizi ruolo**, selezionare **Proxy applicazione Web**, fare clic su **Aggiungi funzionalità**, quindi fare clic su **Avanti**.

7.  Nella pagina **Confirm installation selections** fai clic su **Install**.  
  
## <a name="configure-web-application-proxy"></a>Configurare Proxy applicazione Web  
Per configurare Proxy applicazione Web, attenersi alla seguente procedura:  
  
1.  Fare clic sul flag di avviso nella parte superiore di Server Manager, quindi fare clic sul collegamento per aprire la configurazione guidata di Proxy applicazione Web.  
  
2.  Nella pagina iniziale, premere su **Avanti**.  
  
3.  Nella pagina **Server federativo**, immettere il nome del Servizio federativo. Nell'esempio di test, questo è **blueadfs.contoso.com**.  
  
4.  Immettere le credenziali di un account amministratore locale nei server federativi. Non immettere le credenziali di dominio (ad esempio, contoso\amministratore), bensì le credenziali locali (ad esempio, amministratore).  
  
5.  Nella pagina **Certificato proxy AD FS**, selezionare il certificato di AD FS importato in precedenza. Nel caso di test, questo è **blueadfs.contoso.com**. Fare clic su **Avanti**.  
  
6.  La pagina di conferma mostra il comando di Windows PowerShell che consentirà di eseguire la configurazione del servizio. Fai clic su **Configura**.  
  
## <a name="publish-the-work-folders-web-application"></a>Pubblicare l'applicazione Web di Cartelle di lavoro  
Il passaggio successivo consiste nel pubblicare un'applicazione Web che renderà Cartelle di lavoro disponibile ai client. Per pubblicare l'applicazione Web di Cartelle di lavoro, attenersi alla seguente procedura:  
  
1.  Aprire **Server Manager**e nel menu **Strumenti**, fare clic su **Gestione accesso remoto** per aprire la console di gestione Accesso remoto.  
  
2.  In **Configurazione**, fare clic su **Proxy applicazione Web**.  
  
3.  In **Attività**, fare clic su **Pubblica**. Si apre Pubblicazione guidata nuova applicazione.  
  
4.  Nella pagina iniziale, fare clic su **Avanti**.  
  
5.  Nella pagina **Preautenticazione**, selezionare **Active Directory Federation Services (AD FS)**, quindi fare clic su **Avanti**.  
  
6.  Nella pagina **Supporta client**, selezionare **OAuth2** e fare clic su **Avanti**.

7.  Nella pagina **Relying Party**, selezionare **Cartelle di lavoro**, quindi fare clic su **Avanti**. Questo elenco viene pubblicato su Proxy applicazione Web da AD FS.  
  
8.  Nella pagina **Impostazioni di pubblicazione**, immettere quanto segue, quindi fare clic su **Avanti**:  
  
    -   Il nome che si desidera utilizzare per l'applicazione Web  
  
    -   L'URL esterno di Cartelle di lavoro  
  
    -   Il nome del certificato di Cartelle di lavoro  
  
    -   L'URL di back-end di Cartelle di lavoro  
  
    Per impostazione predefinita, la procedura guidata rende uguali l'URL di back-end e l'URL esterno.  
  
    Nell'esempio di test, usare questi valori:  
  
    Nome: **WorkFolders**  
  
    URL esterno: **https://workfolders.contoso.com**  
  
    Certificato esterno: **il certificato di Cartelle di lavoro installato in precedenza**  
  
    URL del server di back-end: **https://workfolders.contoso.com**  
  
9.  La pagina di conferma mostra il comando di Windows PowerShell che consentirà di eseguire la pubblicazione dell'applicazione. Fare clic su **Pubblica**.  
  
10. Nella pagina **Risultati**, si dovrebbe osservare l'avvenuta pubblicazione dell'applicazione.
   >[!NOTE]
   > Se si dispone di più server Cartelle di lavoro, è necessario pubblicare un'applicazione Web Cartelle di lavoro per ogni server Cartelle di lavoro (ripetere i passaggi da 1 a 10).  
  
Prossimo passaggio: [Distribuire Cartelle di lavoro con AD FS e Proxy dell'applicazione Web: passaggio 5, configurare i client](deploy-work-folders-adfs-step5.md)  
  
## <a name="see-also"></a>Vedere anche  
[Panoramica di Cartelle di lavoro](Work-Folders-Overview.md)  
  

