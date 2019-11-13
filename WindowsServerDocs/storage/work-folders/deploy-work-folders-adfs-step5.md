---
title: "Distribuire Cartelle di lavoro con AD FS e Proxy dell'applicazione Web: passaggio 5, configurare i client"
ms.prod: windows-server
ms.technology: storage-work-folders
ms.topic: article
manager: klaasl
ms.author: jeffpatt
author: JeffPatt24
ms.date: 4/5/2017
ms.assetid: f168292b-0dbc-44b9-965f-d480e5134a0c
ms.openlocfilehash: f0a50913cbcf7773f792df4ce119b83d796a7155
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71402774"
---
# <a name="deploy-work-folders-with-ad-fs-and-web-application-proxy-step-5-set-up-clients"></a>Distribuire Cartelle di lavoro con AD FS e Proxy applicazione Web: passaggio 5, configurare i client

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016

Questo argomento descrive il quinto passaggio nella distribuzione di Cartelle di lavoro con Active Directory Federation Services (AD FS) e Proxy applicazione Web. È possibile trovare gli altri passaggi di questo processo negli argomenti seguenti:  
  
-   [Distribuire cartelle di lavoro con AD FS e il proxy dell'applicazione Web: Panoramica](deploy-work-folders-adfs-overview.md)  
  
-   [Distribuire cartelle di lavoro con AD FS e il proxy dell'applicazione Web: passaggio 1, configurazione AD FS](deploy-work-folders-adfs-step1.md)  
  
-   [Distribuire cartelle di lavoro con AD FS e il proxy dell'applicazione Web: passaggio 2 AD FS lavoro di post-configurazione](deploy-work-folders-adfs-step2.md)  
  
-   [Distribuire cartelle di lavoro con AD FS e il proxy dell'applicazione Web: passaggio 3, configurare cartelle di lavoro](deploy-work-folders-adfs-step3.md)  
  
-   [Distribuire cartelle di lavoro con AD FS e il proxy dell'applicazione Web: passaggio 4, configurare il proxy dell'applicazione Web](deploy-work-folders-adfs-step4.md)  
  
Utilizzare le procedure seguenti per configurare i client Windows aggiunti al dominio e non appartenenti a un dominio. È possibile utilizzare questi client per verificare se i file si sincronizzano correttamente tra Cartelle di lavoro dei client.  
  
## <a name="set-up-a-domain-joined-client"></a>Configurazione di un client aggiunto al dominio  
  
### <a name="install-the-ad-fs-and-work-folder-certificates"></a>Installare i certificati di AD FS e Cartelle di lavoro  
È necessario installare i certificati di AD FS e Cartelle di lavoro creati in precedenza nell'archivio certificati del computer locale nel computer client aggiunto al dominio.  
  
Poiché si stanno installando i certificati autofirmati che non risalgono a un server di pubblicazione nell'archivio certificati Autorità di certificazione radice attendibili, è inoltre necessario copiare i certificati in tale archivio.  
  
Per installare i certificati, effettuare le seguenti operazioni:  
  
1.  Fare clic su **Start**, quindi scegliere **Esegui**.  
  
2.  Tipo **MMC**.  
  
3.  Scegliere **Aggiungi/Rimuovi snap-in** dal menu **File**.  
  
4.  Nella lista **Snap-in disponibili**, seleziona **Certificati**, quindi fai clic su **Aggiungi**. Verrà avviata la procedura guidata Snap\-in certificati.  
  
5.  Seleziona **Account del computer** e quindi fai clic su **Avanti**.  
  
6.  Seleziona **Computer locale: (il computer su cui è in esecuzione questa console)** , quindi fai clic su **Fine**.  
  
7.  Fare clic su **OK**.  
  
8.  Espandi la cartella Radice della console\Certificati\(Computer locale)\Personale\Certificati.  
  
9. Fai clic con il pulsante destro del mouse su **Certificati**, fai clic su **Tutte le attività**, quindi fai clic su **Importa**.  
  
10. Individuare la cartella contenente il certificato di AD FS e seguire le istruzioni della procedura guidata per importare il file e inserirlo nell'archivio certificati.  
  
11. Ripetere i passaggi 9 e 10, questa volta individuando il certificato di Cartelle di lavoro e importandolo.  
  
12. Espandere la cartella Radice della console\Certificati\(Computer locale)\Autorità di certificazione radice attendibili\Certificati.  
  
13. Fai clic con il pulsante destro del mouse su **Certificati**, fai clic su **Tutte le attività**, quindi fai clic su **Importa**.  
  
14. Individuare la cartella contenente il certificato di AD FS e seguire le istruzioni della procedura guidata per importare il file e inserirlo nell'archivio Autorità di certificazione radice attendibili.  
  
15. Ripetere i passaggi 13 e 14, questa volta individuando il certificato di Cartelle di lavoro e importandolo.  
  
### <a name="configure-work-folders-on-the-client"></a>Configurazione di Cartelle di lavoro nel client  
Per configurare cartelle di lavoro sul computer client, procedere come segue:  
  
1. Nel computer client, aprire **Pannello di controllo** e fare clic su **Cartelle di lavoro**.  
  
2. Fare clic su **Installa Cartelle di lavoro**.  
  
3. Nella pagina **immettere l'indirizzo di posta elettronica dell'ufficio** immettere l'indirizzo di posta elettronica dell'utente, ad esempio user@contoso.com, o l'URL di cartelle di lavoro (nell'esempio di test, https:\//workfolders.contoso.com), quindi fare clic su **Avanti**.  
  
4. Se l'utente è connesso alla rete aziendale, l'autenticazione viene eseguita da Autenticazione integrata di Windows. Se l'utente non è connesso alla rete aziendale, l'autenticazione viene eseguita da AD FS (OAuth) e all'utente verranno richieste le credenziali. Immettere le credenziali e fare clic su **OK**.  
  
5. In seguito all'autenticazione, viene visualizzata la pagina **Introduzione a Cartelle di lavoro**, in cui è possibile modificare facoltativamente il percorso della directory di Cartelle di lavoro. Fare clic su **Avanti**.  
  
6. Nella pagina **Criteri di sicurezza** vengono elencati i criteri di sicurezza configurati per Cartelle di lavoro. Fare clic su **Avanti**.  
  
7. Viene visualizzato un messaggio che informa che Cartelle di lavoro ha iniziata la sincronizzazione con il PC. Fare clic su **Chiudi**.  
  
8. La pagina **Gestisci Cartelle di lavoro** mostra lo spazio disponibile sul server, lo stato di sincronizzazione e così via. Se necessario, qui è possibile immettere nuovamente le credenziali. Chiudere la finestra.  
  
9. La cartella di Cartelle di lavoro si apre automaticamente. È possibile aggiungere contenuti in questa cartella per la sincronizzazione tra i dispositivi.  
  
    Ai fini dell'esempio di test, aggiungere un file di test a questa cartella di Cartelle di lavoro. Dopo aver configurato Cartelle di lavoro sul computer non appartenente a un dominio, sarà possibile sincronizzare i file tra Cartelle di lavoro su ciascun computer.  
  
## <a name="set-up-a-non-domain-joined-client"></a>Configurazione di un client non appartenente a un dominio  
  
### <a name="install-the-ad-fs-and-work-folder-certificates"></a>Installare i certificati di AD FS e Cartelle di lavoro  
Installare i certificati di AD FS e Cartelle di lavoro sul computer non appartenente a un dominio, mediante la stessa procedura utilizzata per il computer aggiunto al dominio.  
  
### <a name="update-the-hosts-file"></a>Aggiornare il file host  
Il file host nel client non appartenente a un dominio deve essere aggiornato per l'ambiente di test, perché non è stato creato alcun record DNS pubblico per Cartelle di lavoro. Aggiungere queste voci al file host:  
  
-  cartellelavoro.dominio  
  
-  nome.dominio del servizio AD FS  
  
-  enterpriseregistration.dominio  
  
Nell'esempio di test, usare questi valori:  
  
-  **workfolders.contoso.com 10.0.0.10**  
  
-  **blueadfs.contoso.com 10.0.0.10**  
  
-  **enterpriseregistration.contoso.com 10.0.0.10**  
  
### <a name="configure-work-folders-on-the-client"></a>Configurazione di Cartelle di lavoro nel client  
Configurare Cartelle di lavoro sul computer non appartenente a un dominio mediante la stessa procedura utilizzata per il computer aggiunto al dominio.  
  
Quando si apre la nuova cartella di Cartelle di lavoro in questo client, è possibile osservare che il file proveniente dal computer aggiunto al dominio si è già sincronizzato al computer non appartenente a un dominio. È possibile iniziare ad aggiungere contenuti alla cartella per la sincronizzazione tra i dispositivi.  
  
La procedura di distribuzione di Cartelle di lavoro, AD FS e Proxy applicazione Web tramite l'Interfaccia utente di Windows Server è conclusa.  
  
## <a name="see-also"></a>Vedi anche  
[Panoramica di cartelle di lavoro](Work-Folders-Overview.md)  
  

