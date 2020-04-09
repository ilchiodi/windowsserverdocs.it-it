---
ms.assetid: e6fa9069-ec9c-4615-b266-957194b49e11
title: Aggiornamento AD RMS a Windows Server 2016
author: msmbaldwin
ms.author: esaggese
ms.date: 05/30/2019
ms.prod: windows-server
ms.topic: article
ms.openlocfilehash: 88af85f8e670b9c23b503e0f79af2ce8f10d045e
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80854854"
---
# <a name="upgrading-ad-rms-to-windows-server-2016"></a>Aggiornamento AD RMS a Windows Server 2016

## <a name="introduction"></a>Introduzione

Active Directory Rights Management Services (AD RMS) è un servizio Microsoft che protegge documenti e messaggi di posta elettronica riservati. A differenza dei metodi di protezione tradizionali, ad esempio firewall e ACL, AD RMS la crittografia e la protezione sono permanenti indipendentemente dalla posizione di un file o dal modo in cui vengono trasportati. 

Questo documento fornisce indicazioni per la migrazione da Windows Server 2012 R2 con SQL Server 2012 a Windows Server 2016 e SQL Server 2016. Lo stesso processo può essere utilizzato per eseguire la migrazione da versioni precedenti, ma supportate, di AD RMS.
Si noti che Active Directory Rights Management Services non è più in fase di sviluppo attivo e per le funzionalità più recenti, i clienti devono considerare la migrazione a [Azure Information Protection](https://azure.microsoft.com/services/information-protection/), che offre un set di funzionalità più completo con supporto più completo per dispositivi e applicazioni. 

Per informazioni sulla migrazione a Azure Information Protection da AD RMS senza la necessità di riproteggere il contenuto, vedere [la documentazione relativa alla migrazione Azure Information Protection](https://docs.microsoft.com/azure/information-protection/migrate-from-ad-rms-to-azure-rms).

## <a name="about-the-environment-used-in-this-guide"></a>Informazioni sull'ambiente utilizzato in questa guida

AD FS è un componente facoltativo di un'installazione di AD RMS. In questa guida si presuppone l'utilizzo di ADFS. Se ADFS non è stato usato nell'ambiente per supportare AD RMS utenti, è possibile ignorare tutti i passaggi che fanno riferimento ad ADFS.

In questa Guida SQL Server viene aggiornato al SQL Server 2016 eseguendo un'installazione parallela e spostando i database tramite un backup. In alternativa, se è possibile aggiornare i server di database AD RMS e ADFS a SQL Server 2016 sul posto, è possibile passare alla sezione successiva di questo documento dopo aver eseguito questa operazione senza dover seguire i passaggi descritti in questa sezione.  

## <a name="installation"></a>Installazione

### <a name="configuring-sql-server-2016"></a>Configurazione di SQL Server 2016

La sezione seguente illustra in dettaglio le attività di implementazione correlate direttamente alla configurazione di SQL Server 2016. Questa guida è incentrata sull'uso della Server Manager e della SQL Server Management Studio per completare queste attività.

Questi passaggi devono essere completati in un'installazione di SQL Server 2016. Installare SQL Server 2016 nell'hardware appropriato in base alle procedure e ai criteri standard dell'organizzazione. 

#### <a name="preparing-the-sql-server"></a>Preparazione del SQL Server

Nella sezione seguente vengono illustrate le procedure per preparare il SQL Server in modo che sia possibile aggiornarlo SQL Server 2016 prima di aggiornare altri servizi nella piattaforma AD RMS per l'utilizzo di Windows Server 2016.

##### <a name="adding-cname-for-sql-server-2016-to-dns"></a>Aggiunta di CNAME per SQL Server 2016 a DNS

Il record CNAME viene usato per garantire che il programma di installazione di Windows Server 2016 otterrà i dati appropriati poiché verrà indicato il nuovo SQL Server 2016. **Nota: se si usa già un record CNAME per il servizio ADFS e AD RMS, è possibile passare ai passaggi successivi.**


**Per aggiungere un record CNAME per SQL Server 2016 a DNS**

1.  Accedere al controller di dominio di Windows Server 2012 R2 con le credenziali di amministratore di dominio.

2.  Aprire Gestione server.

3.  Fare clic su **strumenti** e selezionare **DNS** per aprire il gestore DNS.

4.  Dal riquadro di spostamento a sinistra espandere il controller di dominio e aprire le **zone di ricerca diretta**.

5.  Aprire le risorse di dominio appropriate, quindi fare clic con il pulsante destro del mouse nel riquadro di visualizzazione a destra e scegliere **nuovo alias (CNAME)** per iniziare a creare il CNAME.

6.  Per il nome dell'alias immettere in un nome logico per distinguerlo da altri che potrebbero essere presenti (ad esempio, SQLADRMS o sqladfs)

7.  Dopo aver immesso il nome, specificare il nome di dominio completo per l'host di destinazione, che sarà il nuovo server SQL Server 2016. ex. SQL2016.contoso.com)

8.  Dopo aver immesso tutte le informazioni, fare clic su **OK**.

##### <a name="backup-the-ad-rms-and-adfs-databases"></a>Eseguire il backup dei database AD RMS e ADFS

I database AD RMS e ADFS contengono informazioni critiche necessarie per AD RMS, ad esempio la chiave pubblica del certificato concessore di licenze server, i modelli di criteri per i diritti, i dati di configurazione di ADFS e le informazioni di registrazione. Senza questi database, i client non possono emettere licenze per l'utilizzo di contenuto protetto, tra gli altri problemi.

Dei database, il database di configurazione AD RMS è considerato il più importante, perché archivia i modelli di criteri per i diritti, i codici, le chiavi degli utenti e le informazioni di configurazione. Pertanto, anche se è necessario prestare attenzione a eseguire il backup di tutti i database AD RMS e ADFS, è consigliabile pianificare regolarmente il backup del database di configurazione.

Il database di registrazione archivia le informazioni sulle richieste utente nel cluster AD RMS per i certificati e le licenze di utilizzo. La strategia di backup di questo database deve essere basata sui criteri aziendali per mantenere questo tipo di informazioni.

Il database dei servizi directory non è essenziale per AD RMS funzionalità e, in caso di perdita dei dati più recenti, il database verrà ripopolato con le informazioni quando il server AD RMS riceve le richieste di certificati e utilizza le licenze. Non è necessario eseguire regolarmente il backup di questo database, ma è necessario disporre almeno di una copia del database come è stato originariamente configurato dopo la distribuzione di AD RMS.

**Per eseguire il backup di un database AD RMS e/o ADFS con Microsoft SQL Server**

1.  Accedere al server di database AD RMS di Windows Server 2012 R2 con SQL 2012.

2.  Fare clic sul pulsante **Start**, scegliere **tutti i programmi**, fare clic su **Microsoft SQL Server**e quindi su **SQL Server Management Studio**.

3.  Nella finestra **Connetti al server** , verificare che il server che ospita il ad RMS database si trovi nella casella **nome server** e fare clic su **Connetti**.

4.  Espandere **database**. Fare clic con il pulsante destro del mouse sul database appropriato (**DRM** e **ADFS**), scegliere **attività**e selezionare **backup**.

5.  Ripetere il passaggio 4 per i database rimanenti.

6.  Verificare che sia possibile accedere al backup dei database da altri computer della rete o da un dispositivo di archiviazione perché saranno necessari per i passaggi successivi durante la migrazione.

A questo punto è possibile archiviare le copie del database in un luogo sicuro. Ricordarsi di eseguire il backup dei database di frequente.

##### <a name="adding-domain-admin-sql-ad-rms-andor-adfs-service-account-to-sql-server-2016"></a>Aggiunta dell'account di servizio Domain Admin, SQL, AD RMS e/o ADFS al SQL Server 2016

Nei passaggi seguenti viene illustrato come aggiungere i vari account del servizio a SQL Server 2016 per facilitare la migrazione dei dati dall'ambiente Windows Server 2012 R2. In questo modo si otterranno le autorizzazioni appropriate quando si tenta di accedere al contenuto e di gestire i dati.

**Per aggiungere l'account del servizio Domain Admin, SQL, AD RMS e/o ADFS a SQL Server**

1.  Accedere al server con SQL Server 2016 come account amministratore locale.

2.  Fare clic sul pulsante **Start**, scegliere **tutti i programmi**, fare clic su **Microsoft SQL Server**e quindi su **SQL Server Management Studio**.

3.  Nella finestra **Connetti al server** , verificare che il server che ospita il ad RMS database si trovi nella casella **nome server** , quindi per autenticazione fare clic sul menu a discesa e selezionare **SQL Server autenticazione**.

4.  Nel campo **login** immettere il nome dell'account amministratore locale (ad esempio, LocalAdmin) e quindi specificare la password appropriata e fare clic su **Connetti**.

5.  Espandere **sicurezza** , quindi fare clic con il pulsante destro del mouse su **account di accesso** e scegliere **nuovo account di accesso** dal menu di scelta rapida visualizzato.

6.  Una volta visualizzata la finestra, immettere nell'account Domain Admin nel campo **nome account di accesso** (ad esempio, Contoso\\ContosoAdmin)

7.  Dal riquadro di spostamento a sinistra scegliere **ruoli server**.

8.  Selezionare quindi la casella di controllo per **sysadmin** sotto i ruoli del server e fare clic su **OK**.

9.  Riavviare **gestione SQL Server**.

10. Nella finestra **Connetti al server** , verificare che il server che ospita il ad RMS database si trovi nella casella **nome server** , quindi per autenticazione fare clic sul menu a discesa e selezionare **autenticazione di Windows** e fare clic su **Connetti**.

##### <a name="restoring-the-ad-rms-and-adfs-databases-to-sql-server-2016"></a>Ripristino dei database AD RMS e ADFS in SQL Server 2016

Nei passaggi seguenti viene illustrato come ripristinare i dati dall'istanza di SQL Server precedente alla nuova istanza di 2016. Questo consentirà al nuovo SQL di utilizzare i dati di configurazione rilevanti dai database AD RMS e ADFS precedenti.

**Per ripristinare i dati dal SQL Server precedente alla nuova SQL Server**

1.  Accedere al server con SQL Server 2016 con l'account appropriato.

2.  Nel riquadro di spostamento sinistro fare clic con il pulsante destro del mouse su **database** e scegliere **Ripristina database** per avviare il processo di ripristino.

3.  In **origine** scegliere **dispositivo** , quindi cercare il percorso in cui sono stati archiviati i file di database nei passaggi precedenti.

4.  Una volta selezionati i file, fare clic su **OK**.

5.  Verificare che tutti i file di database siano stati aggiunti e completare il processo facendo clic su **OK**.

### <a name="configuring-windows-server-2016-active-directory-federation-services-ad-fs"></a>Configurazione di Windows Server 2016 Active Directory Federation Services (AD FS)

AD FS è stato distribuito per fornire l'accesso Single Sign-On (SSO) a AD RMS come applicazione. È stata anche configurata con il AD RMS Mobile Device Extension (MDE), che consente il supporto per Mac e dispositivi mobili per gli utenti finali.

Le sezioni seguenti forniscono indicazioni sulle attività operative che possono essere necessarie per eseguire la distribuzione di AD FS.

#### <a name="adding-a-2016-ad-fs-server-to-the-farm"></a>Aggiunta di un server di AD FS 2016 alla farm

È possibile distribuire ulteriori server di AD FS per supportare la distribuzione di AD RMS. È possibile scegliere di eseguire questa azione in caso di aumento del traffico verso i server AD RMS, o applicazioni aggiuntive, oppure se è necessario ritirare uno dei server attualmente in uso per AD FS.

**Per aggiungere il server di AD FS 2016 alla farm**

1.  Dal server Azure AD Connect fare doppio clic sull'icona **Azure ad Connect** per avviare la procedura guidata Azure ad Connect.

2.  Nella pagina di benvenuto fare clic su **Configura**.

3.  Nella pagina attività aggiuntive fare clic su **Distribuisci un server federativo aggiuntivo** e quindi fare clic su **Avanti**.

4.  Nella pagina Connetti a Azure AD immettere il nome utente e la password di un account con autorizzazioni amministrative globali, quindi fare clic su **Avanti**.

5.  Nella pagina credenziali amministratore di dominio immettere il nome utente e la password di un account con autorizzazioni di amministratore di dominio e fare clic su **Avanti**.

6.  Fare clic su **Sfoglia** e selezionare il file di certificato usato durante la configurazione della farm ad FS usando il Azure ad Connect.

7.  Fare clic su **Immetti password** per aprire la finestra di dialogo password certificato.

8.  Immettere la password del certificato nel campo password e quindi fare clic su **OK**.

9.  Fare clic su **Avanti**.

10. Nella pagina server AD FS, immettere il nome o l'indirizzo IP del nuovo server AD FS e fare clic su **Aggiungi**.

11. Nella pagina pronto per la configurazione fare clic su **Installa**.

12. Nella pagina installazione completata fare clic su **Esci**.

#### <a name="raising-the-adfs-farm-behavior-level"></a>Innalzamento del livello di comportamento della farm ADFS

Quando si distribuisce un server ADFs che supera il livello di ambiente corrente, ad esempio, si dispone di un ADFS in Windows Server 2012 R2 e quindi si aggiunge un Windows Server 2016 ADFS, è necessario aumentare il livello di comportamento della farm. Questa operazione è necessaria per garantire che l'ambiente usi le informazioni e le funzioni più aggiornate.

**Per aumentare il livello di comportamento della farm ADFS**

1.  Passare al server ADFS di Windows Server 2016.

2.  Aprire una sessione di amministrazione di PowerShell.

3.  Immettere il comando seguente: **\$cred = Get-Credential**

4.  Verrà visualizzata una finestra in cui vengono chieste le credenziali. Immettere le credenziali di amministratore di dominio.

5.  Immettere quindi il comando: **Invoke-AdfsFarmBehaviorLevelRaise-Credential \$cred**

6.  Verrà visualizzato un messaggio che chiede **se si desidera continuare con questa operazione?** Immettere quindi **un** per accettare la richiesta.

7.  Al termine del comando, il livello di comportamento della farm sarà configurato e pronto.

#### <a name="enabling-mobile-device-extension-logging"></a>Abilitazione della registrazione dell'estensione per dispositivi mobili

L'estensione per dispositivi mobili può registrare le richieste ricevute dai dispositivi degli utenti finali. La registrazione è disabilitata per impostazione predefinita ed è consigliabile abilitare la registrazione solo in uno scenario di risoluzione dei problemi. Tutte le richieste, provenienti da dispositivi mobili e computer desktop, per il bootstrap o l'acquisizione di una licenza di utilizzo finale vengono registrate nel database di registrazione AD RMS o nell'account di archiviazione di Azure. La registrazione MDE creerà due tabelle aggiuntive per la SQL Server utilizzata da AD RMS: la tabella del log di debug del client e la tabella del log delle prestazioni del client.

**Per abilitare la registrazione dell'estensione per dispositivi mobili**

1.  Da un server di AD RMS aprire Windows PowerShell come amministratore.

2.  Digitare il comando seguente e premere **invio**: **Import-Module ADRMSADMIN**

3.  Digitare il comando seguente e premere **invio**: **New-PSDrive-Name AdrmsCluster-PSProvider AdRmsAdmin-root https://localhost**

4.  Digitare il comando seguente e premere **invio**: **Set-ItemProperty-Path AdrmsCluster:\\-Name IsLoggingEnabled-value \$true**

Se si usa la registrazione MDE per la risoluzione dei problemi, è consigliabile disabilitarla dopo aver risolto il problema.

**Per disabilitare la registrazione dell'estensione per dispositivi mobili**

1.  Da un server di AD RMS aprire Windows PowerShell come amministratore.

2.  Digitare il comando seguente e premere **invio**: **Import-Module ADRMSADMIN**

3.  Digitare il comando seguente e premere **invio**: **New-PSDrive-Name AdrmsCluster-PSProvider AdRmsAdmin-root https://localhost**

4.  Digitare il comando seguente e premere **invio**: **Set-ItemProperty-Path AdrmsCluster:\\-Name IsLoggingEnabled-value \$false**

### <a name="upgrading-ad-rms-to-windows-server-2016"></a>Aggiornamento AD RMS a Windows Server 2016

Le sezioni seguenti forniscono indicazioni su come aggiungere un server di AD RMS basato su Windows Server 2016 nel cluster Windows Server 2012 R2 corrente. Il server verrà aggiunto al cluster e le informazioni verranno replicate in modo che il server AD RMS precedente possa essere deprecato per liberare risorse.

Dopo aver aggiunto un server di AD RMS basato su Windows Server 2016 al cluster AD RMS, tutti i nodi basati su versioni precedenti di Windows diventeranno inattivi. Al termine di questa operazione, è possibile effettuare il deprovisioning dei server (ad esempio, arrestare, reimpiegare o reinstallare con Windows Server 2016 per aggiungere il cluster AD RMS). 

È possibile distribuire ulteriori server di AD RMS al cluster per supportare il carico nella distribuzione di AD RMS. È anche possibile scegliere di eseguire questa azione in caso di aumento del traffico verso i server AD RMS.

Questa guida non illustra i passaggi necessari per modificare i meccanismi di bilanciamento del carico che potrebbero essere usati nell'ambiente in uso per escludere i server deprecati e per includere quelli da aggiungere al cluster. 

#### <a name="adding-a-2016-ad-rms-server"></a>Aggiunta di un server di AD RMS 2016

Se il cluster AD RMS usa un modulo di protezione hardware anziché una chiave gestita a livello centralizzato per il certificato concessore di licenze server, sarà necessario installare il software e altri artefatti HSM (ad esempio, file chiave e configurazione) nel server prima di installare AD RMS. Sarà inoltre necessario connettere il modulo di protezione hardware al server, fisicamente o tramite le configurazioni di rete pertinenti. Seguire le indicazioni del modulo di protezione hardware per questa procedura. 

**Per aggiungere un server di AD RMS 2016**

1.  Installare il ruolo AD RMS nella distribuzione di Windows Server 2016 desiderata.

2.  Al termine dell'installazione, selezionare il collegamento per **eseguire ulteriori operazioni di configurazione**.

3.  Selezionare **partecipa a un cluster di ad RMS esistente** e fare clic su **Avanti**.

4.  Nella pagina **Seleziona database di configurazione** immettere il record CNAME specificato nel DNS per il server SQL 2016 (FQDN).

5.  Fare clic su elenco nella seconda riga e selezionare **DefaultInstance** dall' **elenco** a discesa.

6.  In **nome database di configurazione**selezionare il menu a discesa e scegliere la configurazione DRM visualizzata. Fare quindi clic su **Avanti**.

7.  Nella pagina **informazioni database** immettere la password della chiave del cluster nel campo fornito. Successivamente, fare clic su **Avanti**.

8.  Nella pagina successiva della procedura guidata, specificare l'account del servizio AD RMS e fornire la password e fare clic su **Avanti** dopo che è stata verificata.

9.  Una volta visualizzata la pagina **sito Web cluster** , verificare che sia stato selezionato il sito Web appropriato e fare clic su **Avanti**.

10. Nella pagina **scegliere un certificato di autenticazione server** selezionare il certificato SSL importato e fare clic su **Avanti**.

11. Fare clic su **Installa** per avviare l'installazione.

12. Al termine della configurazione, sarà necessario disconnettersi e tornare indietro per amministrare AD RMS.

13. Dopo aver eseguito l'accesso, aprire **Server Manager** selezionare **strumenti** , quindi **Active Directory Rights Management**. Verrà visualizzata la finestra di gestione che indica che il cluster ha il server aggiuntivo nel cluster.

14. Se il AD RMS estensione per dispositivi mobili è stato installato nel cluster di AD RMS originale, è necessario installare anche il servizio MDE nei nodi del cluster aggiornati. Seguire le istruzioni riportate nella documentazione di MDE per aggiungere MDE al cluster AD RMS. A questo punto, è possibile reimpiegare tutti i nodi preesistenti o aggiornarli a Windows Server 2016 e aggiungerli nuovamente al cluster AD RMS usando lo stesso processo illustrato in precedenza. 

### <a name="configuring-windows-server-2016-web-application-proxy-wap"></a>Configurazione del proxy applicazione Web di Windows Server 2016 (WAP)

Le sezioni seguenti forniscono indicazioni sulle attività operative che possono essere necessarie per eseguire la distribuzione del proxy dell'applicazione Web. Si tratta di un passaggio facoltativo, non obbligatorio se si pubblica AD RMS a Internet tramite altri meccanismi. 

#### <a name="adding-a-windows-server-2016-wap-server"></a>Aggiunta di un server WAP Windows Server 2016

È possibile distribuire altri server proxy applicazione Web per supportare la distribuzione di AD RMS. È possibile scegliere di eseguire questa azione in caso di aumento del traffico verso i server AD RMS o se è necessario ritirare uno dei server attualmente in uso per il proxy dell'applicazione Web.

**Per aggiungere un server proxy applicazione Web 2016**

1.  Dal server che si desidera configurare come proxy applicazione Web, passare alla console di Server Manager e fare clic su **Aggiungi ruoli e funzionalità**.

2.  Nell' **Aggiunta guidata ruoli e funzionalità**fare clic su **Avanti** fino a visualizzare la schermata di selezione del ruolo server.

3.  Nella schermata Selezione ruoli server selezionare **accesso remoto**e quindi fare clic su **Avanti** fino a quando non si torna alla schermata Selezione ruoli server.

4.  Nella schermata Selezione ruoli server selezionare **proxy applicazione Web**, fare clic su **Aggiungi funzionalità**e quindi fare clic su **Avanti**.

5.  Nella schermata Conferma selezioni per l'installazione fare clic su **Installa**.

6.  Al termine dell'installazione, fare clic su **Chiudi**.

7.  A questo punto è necessario configurare il server. A tale scopo, aprire la console di gestione accesso remoto nel server proxy applicazione Web. Aprire il menu **Start** , digitare **RAMgmtUI. exe**, quindi selezionare l'applicazione.

8.  Nel riquadro di spostamento fare clic su **Proxy applicazione Web**.

9.  Nella console di gestione accesso remoto fare clic su **Esegui la configurazione guidata del proxy applicazione Web**. Una volta nella procedura guidata, fare clic su **Avanti**.

10. Nella schermata server federativo immettere il nome di dominio completo del server di AD FS (ad esempio, adfs.contoso.com), quindi immettere le credenziali per un amministratore nel server AD FS.

11. Nella schermata AD FS certificato proxy, nell'elenco dei certificati attualmente installati nel server proxy applicazione Web, selezionare un certificato che deve essere utilizzato da proxy applicazione Web per AD FS proxy, quindi fare clic su **Avanti**.

12. Nella schermata di conferma, rivedere le impostazioni e fare clic su **Configura**.

13. Al termine della configurazione, fare clic su **Chiudi**.

#### <a name="dns-configuration-for-2016-wap-server"></a>Configurazione DNS per il server WAP 2016

Una volta implementato il server proxy applicazione Web di Windows Server 2016, sarà necessario apportare alcune modifiche DNS. Questa operazione richiede l'uso di un servizio, ad esempio GoDaddy, per puntare i servizi ADFS e AD RMS al server WAP 2016.

**Per puntare il DNS al server WAP**

1.  Passare al sito Web del provider (ad esempio, GoDaddy).

2.  Passare alla gestione del dominio e quindi alla gestione DNS.

3.  Individuare il servizio ADFS e AD RMS e sostituire la parte **Points to** con l'indirizzo IP pubblico del server WAP 2016 e **salvare**.

4.  La propagazione delle modifiche può richiedere tempo, ma una volta completata questa installazione.

#### <a name="enabling-debugging-logs"></a>Abilitazione dei log di debug

Informazioni dettagliate sulla registrazione sono disponibili nei server proxy applicazione Web. È possibile configurare la registrazione avanzata del debug usando il Visualizzatore eventi. È anche possibile selezionare impostazioni aggiuntive per le dimensioni dei log, in modo da garantire che le analisi siano utili per il visualizzatore.

**Abilitazione dei log di debug per il proxy dell'applicazione Web**

1.  Aprire la console di **Visualizzatore eventi** sul proxy applicazione Web.

2.  Espandere il nodo **Microsoft** .

3.  Espandere il nodo **Windows** .

4.  Aprire i log del **proxy dell'applicazione Web** .

5.  Sarà quindi possibile aprire i log **amministrativi** .

6.  Aprire il menu **azione** , che si trova in alto a sinistra, quindi selezionare **Proprietà**.

7.  Nella scheda **generale** scegliere l'opzione per abilitare la **registrazione**.

8.  Infine, è possibile personalizzare le dimensioni massime del log e ciò che accade quando viene raggiunta la dimensione massima del registro eventi.

### <a name="configuring-high-availability-for-windows-server-2016-services"></a>Configurazione della disponibilità elevata per i servizi Windows Server 2016

Le sezioni seguenti forniscono indicazioni sulle attività operative che potrebbero essere necessarie per configurare l'ambiente Windows Server 2016 in disponibilità elevata.

#### <a name="adding-a-2016-ad-rms-server-for-high-availability"></a>Aggiunta di un server di AD RMS 2016 per la disponibilità elevata

Per configurare la disponibilità elevata, è possibile distribuire altri server AD RMS. È possibile scegliere di eseguire questa azione in caso di aumento del traffico verso i server AD RMS.

**Per aggiungere un server di AD RMS 2016 per la disponibilità elevata**

1.  Installare il ruolo AD RMS nella distribuzione di Windows Server 2016 desiderata.

2.  Al termine dell'installazione, selezionare il collegamento per **eseguire ulteriori operazioni di configurazione**.

3.  Selezionare **partecipa a un cluster di ad RMS esistente** e fare clic su **Avanti**.

4.  Nella pagina **Seleziona database di configurazione** immettere il record CNAME specificato nel DNS per il server SQL 2016 (FQDN).

5.  Fare clic su elenco nella seconda riga e selezionare **DefaultInstance** dall' **elenco** a discesa.

6.  In **nome database di configurazione**selezionare il menu a discesa e scegliere la configurazione DRM visualizzata. Fare quindi clic su **Avanti**.

7.  Nella pagina **informazioni database** immettere la password della chiave del cluster nel campo fornito. Successivamente, fare clic su **Avanti**.

8.  Nella pagina successiva della procedura guidata, specificare l'account del servizio AD RMS e fornire la password e fare clic su **Avanti** dopo che è stata verificata.

9.  Una volta visualizzata la pagina **sito Web cluster** , verificare che sia stato selezionato il sito Web appropriato e fare clic su **Avanti**.

10. Nella pagina **scegliere un certificato di autenticazione server** selezionare il certificato SSL importato e fare clic su **Avanti**.

11. Fare clic su **Installa** per avviare l'installazione.

12. Al termine della configurazione, sarà necessario disconnettersi e tornare indietro per amministrare AD RMS.

13. Dopo aver eseguito l'accesso, aprire **Server Manager** selezionare **strumenti** , quindi **Active Directory Rights Management**. Verrà visualizzata la finestra di gestione che indica che il cluster ha il server aggiuntivo nel cluster.

14. Dopo aver confermato l'installazione del server, configurare il servizio di bilanciamento del carico per bilanciare il carico tra i diversi server AD RMS nel cluster.

#### <a name="adding-a-windows-server-2016-ad-fs-server-for-high-availability"></a>Aggiunta di un server di AD FS Windows Server 2016 per la disponibilità elevata

Per configurare la disponibilità elevata, è possibile distribuire altri server AD FS. È possibile scegliere di eseguire questa azione in caso di aumento del traffico verso i server AD FS. **Nota: dopo aver generato il livello di comportamento della farm, verrà immessa una nuova voce del database in SQL Server 2016 (ADFS Configv3) e il database di configurazione precedente deve essere eliminato prima di continuare con questa procedura.**

**Per aggiungere il server Windows Server 2016 AD FS per la disponibilità elevata**

1.  Installare il ruolo AD RMS nella distribuzione di Windows Server 2016 desiderata.

2.  Al termine dell'installazione, selezionare il collegamento per **configurare il servizio federativo in questo server**.

3.  Nella sezione introduttiva della procedura guidata, scegliere l'opzione per **aggiungere un server federativo a un server farm di Federazione** e quindi fare clic su **Avanti**.

4.  Specificare l'account amministratore appropriato e fare clic su **Avanti**.

5.  Nella pagina **specifica Farm** selezionare il percorso del **database per una farm esistente utilizzando SQL Server** quindi immettere il record CNAME per il servizio SQL per il nome host del database e fare clic su **Avanti**.

6.  Nell'area **Specifica account del servizio** della procedura guidata immettere le credenziali per l'account del servizio ad FS e quindi fare clic su **Avanti**.

7.  In **verifica opzioni**fare clic su **Avanti**.

8.  Quando il pulsante diventa disponibile, fare clic su **Configura** .

9.  Dopo la configurazione, riavviare il computer.

10. Dopo aver confermato la configurazione del server, bilanciare il carico dei server AD FS come richiesto.

#### <a name="adding-a-windows-server-2016-wap-server-for-high-availability"></a>Aggiunta di un server Windows Server 2016 WAP per la disponibilità elevata

Per configurare la disponibilità elevata, è possibile distribuire altri server WAP. È possibile scegliere di eseguire questa azione in caso di aumento del traffico verso i server AD RMS.

**Per aggiungere un server proxy applicazione Web Windows Server 2016 per la disponibilità elevata**

1.  Dal server che si desidera configurare come proxy applicazione Web, passare alla console di Server Manager e fare clic su **Aggiungi ruoli e funzionalità**.

2.  Nell' **Aggiunta guidata ruoli e funzionalità**fare clic su **Avanti** fino a visualizzare la schermata di selezione del ruolo server.

3.  Nella schermata Selezione ruoli server selezionare **accesso remoto**e quindi fare clic su **Avanti** fino a quando non si torna alla schermata Selezione ruoli server.

4.  Nella schermata Selezione ruoli server selezionare **proxy applicazione Web**, fare clic su **Aggiungi funzionalità**e quindi fare clic su **Avanti**.

5.  Nella schermata Conferma selezioni per l'installazione fare clic su **Installa**.

6.  Al termine dell'installazione, fare clic su **Chiudi**.

7.  A questo punto è necessario configurare il server. A tale scopo, aprire la console di gestione accesso remoto nel server proxy applicazione Web. Aprire il menu **Start** , digitare **RAMgmtUI. exe**, quindi selezionare l'applicazione.

8.  Nel riquadro di spostamento fare clic su **Proxy applicazione Web**.

9.  Nella console di gestione accesso remoto fare clic su **Esegui la configurazione guidata del proxy applicazione Web**. Una volta nella procedura guidata, fare clic su **Avanti**.

10. Nella schermata server federativo immettere il nome di dominio completo del server di AD FS (ad esempio, adfs.contoso.com), quindi immettere le credenziali per un amministratore nel server AD FS.

11. Nella schermata AD FS certificato proxy, nell'elenco dei certificati attualmente installati nel server proxy applicazione Web, selezionare un certificato che deve essere utilizzato da proxy applicazione Web per AD FS proxy, quindi fare clic su **Avanti**.

12. Nella schermata di conferma, rivedere le impostazioni e fare clic su **Configura**.

13. Al termine della configurazione, fare clic su **Chiudi**.

14. Dopo aver confermato la configurazione del server, bilanciare il carico dei server WAP nella rete perimetrale.

#### <a name="adding-a-sql-server-2016-node-for-always-on-high-availability"></a>Aggiunta di un nodo SQL Server 2016 per la disponibilità elevata Always On

È possibile distribuire ulteriori server SQL per il programma di installazione Always On disponibilità elevata. È possibile scegliere di eseguire questa azione in caso di aumento del traffico verso i server AD RMS. **Nota: assicurarsi che la porta in ingresso 5022 sia aperta in entrambi i server SQL.**

**Per aggiungere un server SQL Server 2016 per Always On disponibilità elevata**

1.  Dal server che si desidera configurare come server aggiuntivo SQL Server 2016, passare alla console di Server Manager e fare clic su **Aggiungi ruoli e funzionalità**.

2.  Fare clic su **Avanti** fino alla finestra di dialogo **Seleziona funzionalità** .

3.  Selezionare la casella di controllo **clustering di failover** . **Nota: seguire questo passaggio anche per il server SQL Server 2016 originale, in modo che entrambi i server SQL dispongano della funzionalità clustering di failover.**

4.  Fare clic su **Installa** per installare la funzionalità clustering di failover.

5.  A questo punto, aprire **Server Manager** e selezionare **strumenti** , quindi **Gestione cluster di failover**.

6.  Dal riquadro del menu a sinistra, fare clic con il pulsante destro del mouse su **Gestione cluster di failover** e scegliere **Crea cluster**

7.  Verrà aperta la **creazione guidata cluster**.

8.  Cercare i server SQL Server 2016 che verranno usati per Always On disponibilità elevata e immetterli in, quindi fare clic su **Avanti**.

9.  Verrà visualizzato un avviso di convalida. Selezionare **Sì** per convalidare i nodi del cluster e quindi fare clic su **Avanti**.

10. Nella pagina **Opzioni di testing** selezionare l'opzione **Esegui tutti i test** e fare clic su **Avanti.**

11. **Nota: è previsto che la convalida guidata del cluster restituisca diversi messaggi di avviso, specialmente se non si intende usare l'archiviazione condivisa. A parte questo, se vengono trovati messaggi di errore è necessario correggerli prima di creare il cluster di failover di Windows Server**.

12. Nella finestra di dialogo **punto di accesso per l'amministrazione del cluster** immettere il nome del cluster e l'indirizzo IP virtuale per il cluster di failover di Windows Server, quindi fare clic su **Avanti**.

13. Verificare che la configurazione sia stata completata **correttamente e fare** clic su **fine**.

14. Tornare all' **Gestione cluster di failover,** fare clic con il pulsante destro del mouse sul cluster e selezionare **altre azioni** , quindi scegliere **Configura impostazioni quorum del cluster**

15. Fare clic su **Avanti** e quindi selezionare l'opzione per **selezionare il quorum** di controllo e premere di nuovo **Avanti** .

16. Nella pagina **selezione quorum** di controllo selezionare l'opzione **Configura condivisione file** di controllo. Fare quindi clic su **Avanti**.

17. Selezionare **Sfoglia** e individuare il percorso della condivisione file che si desidera utilizzare nella finestra di dialogo percorso condivisione file. Fare clic su **Avanti**.

18. Nella pagina Conferma fare clic su **Avanti**.

19. Nella pagina Riepilogo fare clic su **fine**.

20. A questo punto, aprire il menu **Start** e cercare **Gestione configurazione SQL Server**.

21. Fare clic con il pulsante destro del mouse sul nome del SQL Server e scegliere **Proprietà**.

22. Nella finestra di dialogo Proprietà selezionare la scheda **disponibilità elevata AlwaysOn** . Selezionare la casella di controllo **Abilita gruppi di disponibilità AlwaysOn** . Fare clic su **OK**. **Nota: eseguire questa operazione su entrambi i server SQL Server 2016.**

23. Riavviare quindi il servizio SQL Server.

24. A questo punto, aprire il menu **Start** e cercare **SQL Server Management Studio** e nel riquadro di spostamento a sinistra, fare clic con il pulsante destro del mouse su **gruppi di disponibilità** e scegliere **creazione guidata gruppo di disponibilità** , quindi fare clic su **Avanti**.

25. Nella pagina **Specifica nome del gruppo di disponibilità** selezionare il nome di un gruppo (ad esempio, SQLAvailabilityGroup2016). Fare quindi clic su **Avanti**.

26. Nella sezione **selezionare i database** specificare i database. Scegliere quindi Avanti. **Nota: è possibile che sia necessario eseguire nuovamente il backup di un database o attivare la modalità di ripristino completo**.

27. Nella pagina **Specifica repliche** fare clic sul pulsante **Aggiungi replica** e selezionare l'altro SQL Server 2016.

28. Dopo aver aggiunto l'altro server, fare clic sulle caselle di controllo e impostare il server secondario come secondario leggibile.

29. Passare alla scheda **endpoint** e fare clic sull'opzione **Aggiorna** . Anche in questo caso scorrere e verificare che lo stesso account del servizio sia presente nel nodo primario e secondario.

30. A questo punto, scegliere la scheda **Preferenze di backup** e selezionare l'opzione **preferisci secondario** .

31. Passare alla scheda **listener** .

32. Specificare un nome (ad esempio Sqllistener) e verificare che la porta sia **1433** , quindi fare clic su **Avanti**.

33. Nella pagina **Seleziona sincronizzazione dati iniziale** della procedura guidata, scegliere l'opzione **completo** e specificare il percorso di rete accessibile da tutti i server SQL e quindi fare clic su **Avanti**.

34. Infine, fare clic su **fine** e il processo viene completato.

### <a name="decommission-windows-server-2012-r2-nodes"></a>Rimuovere i nodi di Windows Server 2012 R2

Le sezioni seguenti forniscono indicazioni sulle attività operative che potrebbero essere necessarie per rimuovere i server Windows Server 2012 R2 dopo aver aggiornato correttamente il cluster AD RMS a Windows Server 2016.

#### <a name="removing-a-windows-server-2012-r2-ad-rms-server"></a>Rimozione di un server AD RMS Windows Server 2012 R2

È possibile rimuovere server AD RMS non necessari dopo un aggiornamento. È possibile scegliere di eseguire questa azione quando diventa necessario rimuovere le autorizzazioni di AD RMS Server.

**Per rimuovere un** **server AD RMS Windows Server 2012 R2**

1.  Nel server AD RMS Windows Server 2012 R2 Server Manager selezionare **Gestisci** dal menu in alto a destra e quindi scegliere **Rimuovi ruoli e funzionalità**.

2.  La **rimozione guidata ruoli e funzionalità** verrà visualizzata e nella schermata **prima di iniziare** fare clic su **Avanti**.

3.  Nella schermata **Selezione server** fare clic su **Avanti**.

4.  Nella schermata **ruoli server** , rimuovere il segno di spunta accanto a **Active Directory Rights Management Services** e fare clic su **Avanti**.

5.  Nella schermata **funzionalità** fare clic su **Avanti**.

6.  Nella schermata di **conferma** fare clic su **Rimuovi**.

7.  Al termine dell'operazione, riavviare il server.

8.  È ora possibile arrestare questo server e riallocare le risorse in base alle esigenze.
