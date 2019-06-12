---
ms.assetid: e6fa9069-ec9c-4615-b266-957194b49e11
title: L'aggiornamento di AD RMS per Windows Server 2016
description: ''
author: msmbaldwin
ms.author: esaggese
ms.date: 05/30/2019
ms.topic: article
ms.openlocfilehash: ce058a2885315c84d2c1c6701ad2801790d3c590
ms.sourcegitcommit: 6ef4986391607bb28593852d06cc6645e548a4b3
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 06/07/2019
ms.locfileid: "66814074"
---
# <a name="upgrading-ad-rms-to-windows-server-2016"></a>L'aggiornamento di AD RMS per Windows Server 2016

## <a name="introduction"></a>Introduzione

Active Directory Rights Management Services (AD RMS) è un servizio Microsoft che consente di proteggere messaggi di posta elettronica e documenti riservati. A differenza dei metodi di protezione tradizionali, ad esempio firewall e ACL, protezione e crittografia di AD RMS sono persistenti indipendentemente da dove passa un file o come il trasporto. 

Questo documento fornisce materiale sussidiario per la migrazione da Windows Server 2012 R2 con SQL Server 2012 a Windows Server 2016 e SQL Server 2016. Lo stesso processo utilizzabile per eseguire la migrazione da versioni precedenti ma supportate di AD RMS.
Si noti che Active Directory Rights Management Services non è più in fase di sviluppo e per le funzionalità più recenti, i clienti dovranno valutare la migrazione a [Azure Information Protection](https://azure.microsoft.com/services/information-protection/), che offre un altro ancora set completo di funzionalità con più completa del dispositivo e il supporto dell'applicazione. 

Per informazioni sulla migrazione ad Azure Information Protection da AD RMS senza la necessità di proteggere nuovamente il contenuto, vedere [la documentazione sulla migrazione di Azure Information Protection](https://docs.microsoft.com/azure/information-protection/migrate-from-ad-rms-to-azure-rms).

## <a name="about-the-environment-used-in-this-guide"></a>Sull'ambiente usato in questa Guida

ADFS è un componente facoltativo di un'installazione di AD RMS. In questa guida presuppone l'uso di ad FS. Se ad FS non è stato usato nell'ambiente in uso per il supporto agli utenti di AD RMS, è possibile ignorare tutti i passaggi che fanno riferimento ad ad FS.

In questa Guida, SQL Server viene aggiornato a SQL Server 2016 eseguendo un'installazione parallela e lo spostamento dei database tramite un backup. In alternativa, se è possibile aggiornare i server di database AD RMS e ad FS per SQL Server 2016 sul posto, è possibile spostare alla sezione successiva in questo documento dopo avere eseguito questa operazione senza dover seguire i passaggi descritti in questa sezione.  

## <a name="installation"></a>Installazione

### <a name="configuring-sql-server-2016"></a>Configurazione di SQL Server 2016

Le attività di implementazione la seguente sezione dettagli correlati direttamente alla configurazione di SQL Server 2016. Questa guida è incentrata sull'uso di Server Manager e SQL Server Management Studio per completare queste attività.

Questi passaggi devono essere completati in un'installazione di SQL Server 2016. Installare SQL Server 2016 su hardware idoneo in base ai criteri e le procedure standard della propria organizzazione. 

#### <a name="preparing-the-sql-server"></a>Preparazione di SQL Server

La sezione seguente illustra come preparare il Server SQL in modo che possono essere aggiornato a SQL Server 2016 prima di aggiornare altri servizi nella piattaforma di AD RMS per usare Windows Server 2016.

##### <a name="adding-cname-for-sql-server-2016-to-dns"></a>Aggiunta di record CNAME per SQL Server 2016 a DNS

Il record CNAME viene usato per garantire che il programma di installazione di Windows Server 2016 verrà visualizzati i dati appropriati dal momento che verrà puntare al nuovo SQL Server 2016. **Nota: Se si usa già un record CNAME per il servizio di ad FS e AD RMS, è possibile passare alla procedura seguente.**


**Per aggiungere un record CNAME per SQL Server 2016 al DNS**

1.  Accedere a Windows Server 2012 R2 Controller di dominio con credenziali di amministratore di dominio.

2.  Aprire Server Manager.

3.  Fare clic su **degli strumenti** e selezionare **DNS** per aprire Gestore DNS.

4.  Nel riquadro di spostamento a sinistra, espandere il controller di dominio e apriamo **zone di ricerca diretta**.

5.  Aprire le risorse di dominio appropriato, quindi fare clic con il pulsante destro nel riquadro di visualizzazione a destra e selezionare **nuovo Alias (CNAME)** per iniziare a creare il record CNAME.

6.  Per immettere il nome dell'alias in un nome logico per distinguerlo da altri che può essere presente (ex. SQLADRMS o SQLADFS)

7.  Dopo aver immesso il nome, specificare il FQDN dell'host di destinazione che sarà il nuovo server SQL Server 2016. (ad esempio SQL2016.contoso.com)

8.  Dopo aver immesso tutte le informazioni, fare clic su **OK**.

##### <a name="backup-the-ad-rms-and-adfs-databases"></a>Backup di AD RMS e ad FS di database

I database AD RMS e ad FS contengono informazioni critiche necessarie per AD RMS, ad esempio la chiave pubblica del certificato concessore di licenze Server, modelli di criteri di diritti di utilizzo, ad FS dati di configurazione e le informazioni di registrazione. Senza tali database, i client non possono emettere licenze per utilizzare il contenuto protetto, tra altri problemi.

I database, il database di configurazione di AD RMS è considerato la più importante, poiché archivia il certificato concessore di licenze, i diritti di modelli di criteri, degli utenti le chiavi e le informazioni di configurazione. Pertanto, anche se è necessario prestare attenzione a eseguire il backup di tutti i database AD RMS e ad FS, è consigliabile eseguire regolarmente il backup del database di configurazione.

Tale database archivia le informazioni relative alle richieste utente per il cluster AD RMS per i certificati e le licenze d'uso. La strategia di backup di questo database deve essere basata su criteri aziendali per il mantenimento di questo tipo di informazioni.

Il database di servizi di directory non è fondamentale per funzionalità di AD RMS e, se i dati più recenti vanno persi, il database verrà ripopolare con le informazioni come il server AD RMS riceve le richieste per i certificati e licenze d'uso. Non è necessario eseguire il backup di questo database regolarmente, ma è necessario disporre almeno di una copia del database perché è stato originariamente configurato dopo la distribuzione di AD RMS.

**Eseguire il backup di un database AD RMS e/o ad FS con Microsoft SQL Server**

1.  Accedere al server di database di Windows Server 2012 R2 AD RMS con SQL 2012.

2.  Fare clic su **avviare**, fare clic su **tutti i programmi**, fare clic su **Microsoft SQL Server**, fare clic su **SQL Server Management Studio**.

3.  Nel **Connetti al Server** finestra, verificare che il server che ospita i database AD RMS sia nel **nome Server** casella e fare clic su **Connect**.

4.  Espandere **database**. Fare clic con il pulsante destro del database appropriato (**DRM** e **Adfs**), scegliere **le attività**e selezionare **Backup**.

5.  Ripetere il passaggio 4 per i database rimanenti.

6.  Assicurarsi che il backup dei database siano accessibili da altri computer nella rete o usando un dispositivo di archiviazione, perché saranno necessarie per i passaggi successivi durante la migrazione.

A questo punto è possibile archiviare le copie di database in un luogo sicuro. Ricordarsi di eseguire il backup dei database di frequente.

##### <a name="adding-domain-admin-sql-ad-rms-andor-adfs-service-account-to-sql-server-2016"></a>Aggiunta di amministratore di dominio, Account SQL, AD RMS e/o il servizio ad FS per SQL Server 2016

I passaggi seguenti illustrano come aggiungere gli account del servizio diversi per SQL Server 2016 per il supporto per la migrazione dei dati dall'ambiente di Windows Server 2012 R2. Questo fornirà le autorizzazioni appropriate, quando si tenta di accedere al contenuto e gestire i dati.

**Per aggiungere l'amministratore di dominio, SQL, AD RMS e/o Account del servizio ad FS per SQL Server**

1.  Accedere al server con SQL Server 2016 come l'account amministratore locale.

2.  Fare clic su **avviare**, fare clic su **tutti i programmi**, fare clic su **Microsoft SQL Server**, fare clic su **SQL Server Management Studio**.

3.  Nel **Connetti al Server** finestra, verificare che il server che ospita i database AD RMS sia nel **nome Server** casella, quindi per l'autenticazione fare clic sul menu di riepilogo a discesa e selezionare **SQL Server Autenticazione**.

4.  Nel **account di accesso** campo immettere il nome dell'account amministratore locale (es: LocalAdmin) e quindi fornire la password appropriata e fare clic su **Connect**.

5.  Espandere **sicurezza** e quindi fare doppio clic su **account di accesso** e selezionare **nuovo account di accesso** dal menu di scelta rapida visualizzato.

6.  Una volta che viene visualizzata la finestra immettere nell'account di amministratore di dominio nel **nome account di accesso** campo (es: Contoso\\ContosoAdmin)

7.  Nel riquadro di spostamento a sinistra, scegliere **ruoli predefiniti del Server**.

8.  Quindi contrassegnare la casella di controllo **sysadmin** sotto i ruoli del server e fare clic su **OK**.

9.  Riavviare **SQL Server Management**.

10. Nel **Connetti al Server** finestra, verificare che il server che ospita i database AD RMS sia nel **nome Server** casella, quindi per l'autenticazione fare clic sul menu di riepilogo a discesa e selezionare **Windows L'autenticazione** e fare clic su **Connect**.

##### <a name="restoring-the-ad-rms-and-adfs-databases-to-sql-server-2016"></a>Il ripristino di AD RMS e i database ADFS a SQL Server 2016

I passaggi seguenti illustrano come ripristinare i dati dall'istanza di SQL Server precedente a quella nuova 2016. In questo modo il nuovo codice SQL utilizzare i dati di configurazione rilevanti dai database AD RMS e ad FS precedenti.

**Per ripristinare i dati da SQL Server precedente al nuovo Server SQL**

1.  Accedere al server con SQL Server 2016 con l'account appropriato.

2.  Nel riquadro di spostamento a sinistra, fare doppio clic su **database** e selezionare **Ripristina Database** per iniziare il processo di ripristino.

3.  Sotto **origine** sceglie **dispositivo** e quindi cercare il percorso in cui sono stati archiviati i file di database nei passaggi precedenti.

4.  Dopo aver selezionati i file, fare clic su **OK**.

5.  Assicurarsi che tutti i file di database sono stati aggiunti e completare il processo facendo clic **OK**.

### <a name="configuring-windows-server-2016-active-directory-federation-services-ad-fs"></a>Configurazione di Active Directory Federation Services (ADFS) di Windows Server 2016

AD FS è stata distribuita per fornire solo accesso sign-on (SSO) ad AD RMS come applicazione. È anche stato configurato con AD RMS per dispositivi mobili dispositivo estensione (MDE), che consente il supporto per gli utenti finali dei dispositivi mobili e Mac.

Le sezioni seguenti forniscono informazioni aggiuntive sulle attività operative, che potrebbe essere necessario eseguire nella distribuzione di AD FS.

#### <a name="adding-a-2016-ad-fs-server-to-the-farm"></a>Aggiunta di un Server ADFS 2016 alla Farm

È possibile distribuire altri server AD FS per supportare la distribuzione di AD RMS. È possibile scegliere di eseguire questa azione in caso di aumento del traffico ai server AD RMS, o altre applicazioni, o se è necessario ritirare uno dei server attualmente in uso per AD FS.

**Per aggiungere il server ADFS 2016 alla farm**

1.  Dal server di Azure AD Connect, fare doppio clic il **Azure AD Connect** sull'icona per avviare la procedura guidata Azure AD Connect.

2.  Nella pagina di benvenuto, fare clic su **configura**.

3.  Nella pagina attività aggiuntive, fare clic su **distribuire un Server federativo aggiuntivo** e quindi fare clic su **successivo**.

4.  Nella pagina Connetti a Azure AD, immettere il nome utente e password di un account con autorizzazioni di amministratore globale e quindi fare clic su **successivo**.

5.  Nella pagina credenziali amministratore di dominio, immettere il nome utente e password di un account con autorizzazioni di amministratore di dominio e fare clic su **successivo**.

6.  Fare clic su **esplorare** e selezionare il file del certificato usata quando la configurazione della farm AD FS con Azure AD Connect.

7.  Fare clic su **immettere la Password** per aprire la finestra di dialogo Password del certificato.

8.  Immettere la password del certificato nel campo della Password e quindi fare clic su **OK**.

9.  Fare clic su **Avanti**.

10. Nella pagina server AD FS, immettere il nome o l'indirizzo IP del nuovo server ADFS e fare clic su **Add**.

11. Nella pagina Configura inizio, fare clic su **installare**.

12. Nella pagina Installazione completata, fare clic su **Exit**.

#### <a name="raising-the-adfs-farm-behavior-level"></a>L'aumento del livello di comportamento Farm ad FS

Quando si distribuisce un server ADFs che supera il livello di ambiente corrente, ad esempio, avere un ADFS in Windows Server 2012 R2 e l'aggiunta di un Windows Server ad FS 2016, il livello di comportamento Farm dovrà essere aumentato. Ciò è necessario per garantire che l'ambiente Usa le informazioni e le funzioni più aggiornati.

**Per aumentare il livello di comportamento Farm ad FS**

1.  Passare l'ADFS di Windows Server 2016.

2.  Aprire una sessione di PowerShell di amministratore.

3.  Immettere il comando seguente:  **\$cred = Get-Credential**

4.  Verrà visualizzata una finestra che richiede credenziali, immettere le credenziali di amministratore di dominio.

5.  Immettere quindi questo comando: **Invoke-AdfsFarmBehaviorLevelRaise -Credential \$cred**

6.  Verrà visualizzato un messaggio in cui viene chiesto, **si desidera continuare con questa operazione?** Quindi immettere **un** per accettare la richiesta.

7.  Dopo aver completato il comando, il livello di comportamento Farm sarà essere configurato e pronto.

#### <a name="enabling-mobile-device-extension-logging"></a>Abilitazione della registrazione di estensione per dispositivi mobili

L'estensione per dispositivi mobili è possibile registrare le richieste ricevute dai dispositivi degli utenti finali. La registrazione è disabilitata per impostazione predefinita ed è consigliabile abilitare solo la registrazione in uno scenario di risoluzione dei problemi. Tutte le richieste, da dispositivi mobili e computer desktop, eseguire il bootstrap o acquistare una licenza di utilizzo finale vengono registrate nel database di registrazione di AD RMS o account di archiviazione di Azure. La registrazione MDE verrà create due tabelle aggiuntive al Server SQL usati da AD RMS: il client di eseguire il debug tabella del log e la tabella del log delle prestazioni client.

**Per abilitare la registrazione di estensione per dispositivi mobili**

1.  Da un server AD RMS, aprire Windows PowerShell come amministratore.

2.  Digitare il seguente comando e premere **invio**: **Import-Module AdRmsAdmin**

3.  Digitare il seguente comando e premere **invio**: **New-PSDrive -Name AdrmsCluster -PsProvider AdRmsAdmin -Root https://localhost**

4.  Digitare il seguente comando e premere **invio**: **Set-ItemProperty-Path AdrmsCluster:\\ -IsLoggingEnabled nome-valore \$true**

Se si usa la registrazione MDE per la risoluzione dei problemi, è consigliabile disabilitarlo dopo avere risolto il problema.

**Per disabilitare la registrazione di estensione per dispositivi mobili**

1.  Da un server AD RMS, aprire Windows PowerShell come amministratore.

2.  Digitare il seguente comando e premere **invio**: **Import-Module AdRmsAdmin**

3.  Digitare il seguente comando e premere **invio**: **New-PSDrive -Name AdrmsCluster -PsProvider AdRmsAdmin -Root https://localhost**

4.  Digitare il seguente comando e premere **invio**: **Set-ItemProperty-Path AdrmsCluster:\\ -IsLoggingEnabled nome-valore \$false**

### <a name="upgrading-ad-rms-to-windows-server-2016"></a>L'aggiornamento di AD RMS per Windows Server 2016

Le sezioni seguenti forniscono indicazioni su come aggiungere un Server basato su Windows Server 2016 AD RMS nel cluster di Windows Server 2012 R2 corrente. Il server verrà aggiunto al cluster e le informazioni verranno replicate ad esso in modo che il server AD RMS precedente può essere deprecato per liberare risorse.

Dopo aver aggiunto un server basato su Windows Server 2016 AD RMS è stato aggiunto al cluster AD RMS, tutti i nodi basati su versioni precedenti di Windows diventerà inattivi. A questo punto è possibile effettuare il deprovisioning di tali server (ad esempio, arrestare, reimpiegare o reinstallare Windows Server 2016 aggiunti al cluster AD RMS). 

È possibile distribuire ulteriori server di AD RMS nel cluster per supportare il carico sulla distribuzione di AD RMS. È anche possibile scegliere eseguire questa azione in caso di aumento del traffico ai server AD RMS.

Questa Guida non illustra i passaggi necessari per modificare i meccanismi che si stia utilizzando nell'ambiente in uso per escludere i server che sono deprecazione e includere quelle che si sta aggiungendo al cluster di bilanciamento del carico. 

#### <a name="adding-a-2016-ad-rms-server"></a>Aggiunta di un Server 2016 AD RMS

Se il cluster AD RMS Usa un modulo di protezione Hardware anziché una chiave gestita a livello centrale per il certificato del concessore di licenze Server, è necessario installare il software e altri artefatti di modulo di protezione hardware (ad esempio, i file di chiave e configurazione) nel server prima di installare AD RMS. È necessario anche connettere tali moduli nel server fisicamente o tramite le configurazioni di rete rilevanti. Seguire le linee guida per moduli di protezione hardware per questi passaggi. 

**Per aggiungere un Server 2016 AD RMS**

1.  Installare il ruolo AD RMS nella distribuzione di Windows Server 2016 desiderata.

2.  Al termine dell'installazione, selezionare il collegamento a **eseguire la configurazione aggiuntiva**.

3.  Selezionare **partecipare a un cluster AD RMS esistente** e fare clic su **successivo**.

4.  Nel **Database di configurazione selezionare** pagina, immettere il record CNAME nel DNS per il server SQL 2016 (FQDN) specificato.

5.  Fare clic su **elenco** sulla seconda riga e selezionare il **DefaultInstance** dall'elenco a discesa.

6.  Sotto **Nome Database di configurazione**, selezionare il menu a discesa e scegliere la configurazione viene visualizzato. Fare quindi clic su **Avanti**.

7.  Nel **informazioni sul Database** pagina, immettere la password della chiave cluster nel campo disponibile. Successivamente, fare clic su **successivo**.

8.  Nella pagina successiva della procedura guidata, specificare l'account del servizio AD RMS e fornire la password per tale e fare clic su **successivo** dopo che è stato verificato.

9.  Una volta il **sito Web Cluster** verrà visualizzata la pagina, è sufficiente Assicurarsi che sia stato selezionato il sito web appropriato e fare clic su **successivo**.

10. Nel **scegliere un certificato di autenticazione Server** pagina, selezionare il certificato SSL importato e fare clic su **successivo**.

11. Fare clic su **Installa** per avviare l'installazione.

12. Al termine della configurazione, è necessario se si disconnettano ed eseguano di accesso per amministrare AD RMS.

13. Una volta connessi back, aprire **Server Manager** selezionate **Tools** e quindi **Active Directory Rights Management**. La finestra Gestione dovrà essere visualizzato e indicare che il cluster dispone di server aggiuntivi nel cluster.

14. 14. Se l'estensione per dispositivi mobili di AD RMS è stato installato nel cluster AD RMS originale, è necessario installare anche il MDE nei nodi del cluster aggiornato. Seguire le istruzioni nella documentazione di MDE aggiungere MDE per il cluster AD RMS. A questo punto, è possibile reimpiegare tutti i nodi preesistenti o eseguirne l'aggiornamento a Windows Server 2016 e aggiungere nuovamente al cluster AD RMS tramite lo stesso processo descritto in precedenza. 

### <a name="configuring-windows-server-2016-web-application-proxy-wap"></a>Configurazione di Proxy applicazione Web di Windows Server 2016 (WAP)

Le sezioni seguenti forniscono informazioni aggiuntive sulle attività operative, che potrebbe essere necessario eseguire nella distribuzione di Proxy applicazione Web. Questo è un passaggio facoltativo, non è necessario se si pubblica AD RMS a Internet tramite altri meccanismi. 

#### <a name="adding-a-windows-server-2016-wap-server"></a>Aggiunta di un Server WAP di Windows Server 2016

È possibile distribuire ulteriori server Proxy applicazione Web per supportare la distribuzione di AD RMS. È possibile scegliere di eseguire questa azione in caso di aumento del traffico ai server AD RMS o se è necessario ritirare uno dei server attualmente in uso per il Proxy applicazione Web.

**Per aggiungere un server Proxy applicazione Web 2016**

1.  Dal server di cui si desidera configurare come Proxy applicazione Web, passare alla console di Server Manager e fare clic su **Aggiungi ruoli e funzionalità**.

2.  Nel **Aggiunta guidata ruoli e funzionalità**, fare clic su **successivo** finché non viene visualizzata la schermata di selezione ruolo del Server.

3.  Nella schermata di selezione ruoli Server, selezionare **accesso remoto**, quindi fare clic su **successivo** finché non si torna alla schermata di selezione ruoli Server.

4.  Nella schermata di selezione ruoli Server, selezionare **Proxy applicazione Web**, fare clic su **Aggiungi funzionalità**, quindi fare clic su **Next**.

5.  Nella schermata di conferma selezioni per l'installazione, fare clic su **installare**.

6.  Dopo aver completato l'installazione, fare clic su **Chiudi**.

7.  A questo punto è possibile configurare il server. A tale scopo, aprire la console Gestione accesso remoto nel server Proxy applicazione Web. Aprire il **avviare** menu, digitare **RAMgmtUI.exe**e quindi selezionare l'applicazione.

8.  Nel riquadro di spostamento, fare clic su **Proxy applicazione Web**.

9.  Nella console di gestione accesso remoto fare clic su **eseguire la configurazione guidata Proxy applicazione Web**. Una volta nella procedura guidata, fare clic su **successivo**.

10. Nella schermata di Server federativo, immettere il nome di dominio completo del server AD FS (ex. ADFS.contoso.com) e quindi immettere le credenziali per un amministratore nel server AD FS.

11. Nella schermata certificato Proxy ADFS nell'elenco dei certificati installati nel server Proxy applicazione Web, selezionare un certificato utilizzabile da Proxy applicazione Web per il proxy ADFS e quindi fare clic su **successivo**.

12. Nella schermata di conferma, rivedere le impostazioni, quindi fare clic su **configura**.

13. Dopo aver completata la configurazione, fare clic su **Chiudi**.

#### <a name="dns-configuration-for-2016-wap-server"></a>Configurazione DNS per 2016 Server WAP

Dopo che il server Proxy applicazione Web di Windows Server 2016 è stato attivato sul posto, dovrà essere apportate alcune modifiche al DNS. Questo richiederà l'utilizzo di un servizio, ad esempio GoDaddy in modo da puntare di ad FS e servizi AD RMS nel server WAP 2016.

**In modo da indicare il DNS al server WAP**

1.  Passare al sito Web del provider (ad esempio GoDaddy).

2.  Andare nella gestione del dominio e quindi la gestione del DNS.

3.  Individuare il servizio ad FS e AD RMS e sostituire il **punta a** parte con l'indirizzo IP pubblico del server WAP 2016 e **salvare**.

4.  Le modifiche potrebbero richiedere tempo per propagarsi, ma una volta fatto questo programma di installazione sarà completata.

#### <a name="enabling-debugging-logs"></a>Abilitazione dei log di debug

Informazioni di registrazione dettagliate sono disponibili sul server Proxy applicazione Web. È possibile configurare la registrazione di debug avanzata tramite il Visualizzatore eventi. Inoltre è possibile selezionare le impostazioni aggiuntive per le dimensioni dei log al fine di garantire che l'analitica è utili per il visualizzatore.

**Abilitazione dei log di debug per il Proxy applicazione Web**

1.  Aprire il **Visualizzatore eventi** console nel Proxy applicazione Web.

2.  Espandere la **Microsoft** nodo.

3.  Espandere la **Windows** nodo.

4.  Aprire il **Proxy applicazione Web** i log.

5.  Quindi sarà possibile aprire il **Admin** i log.

6.  Aprire il **azione** dal menu si trova in alto a sinistra, quindi seleziona **proprietà**.

7.  Sotto il **generali** scheda, scegliere l'opzione **Abilita registrazione**.

8.  Infine, si è in grado di personalizzare le dimensioni massime per il registro e cosa accade quando viene raggiunta la dimensione massima del log eventi.

### <a name="configuring-high-availability-for-windows-server-2016-services"></a>Configurazione della disponibilità elevata per i servizi di Windows Server 2016

Le sezioni seguenti forniscono informazioni aggiuntive sulle attività operative, che potrebbe essere necessario configurare l'ambiente di Windows Server 2016 con disponibilità elevata.

#### <a name="adding-a-2016-ad-rms-server-for-high-availability"></a>Aggiunta di un Server 2016 AD RMS per la disponibilità elevata

È possibile distribuire ulteriori server di AD RMS per configurare la disponibilità elevata. È possibile scegliere di eseguire questa azione in caso di aumento del traffico ai server AD RMS.

**Per aggiungere un server 2016 AD RMS per la disponibilità elevata**

1.  Installare il ruolo AD RMS nella distribuzione di Windows Server 2016 desiderata.

2.  Al termine dell'installazione, selezionare il collegamento a **eseguire la configurazione aggiuntiva**.

3.  Selezionare **partecipare a un cluster AD RMS esistente** e fare clic su **successivo**.

4.  Nel **Database di configurazione selezionare** pagina, immettere il record CNAME nel DNS per il server SQL 2016 (FQDN) specificato.

5.  Fare clic su **elenco** sulla seconda riga e selezionare il **DefaultInstance** dall'elenco a discesa.

6.  Sotto **Nome Database di configurazione**, selezionare il menu a discesa e scegliere la configurazione viene visualizzato. Fare quindi clic su **Avanti**.

7.  Nel **informazioni sul Database** pagina, immettere la password della chiave cluster nel campo disponibile. Successivamente, fare clic su **successivo**.

8.  Nella pagina successiva della procedura guidata, specificare l'account del servizio AD RMS e fornire la password per tale e fare clic su **successivo** dopo che è stato verificato.

9.  Una volta il **sito Web Cluster** verrà visualizzata la pagina, è sufficiente Assicurarsi che sia stato selezionato il sito web appropriato e fare clic su **successivo**.

10. Nel **scegliere un certificato di autenticazione Server** pagina, selezionare il certificato SSL importato e fare clic su **successivo**.

11. Fare clic su **Installa** per avviare l'installazione.

12. Al termine della configurazione, è necessario se si disconnettano ed eseguano di accesso per amministrare AD RMS.

13. Una volta connessi back, aprire **Server Manager** selezionate **Tools** e quindi **Active Directory Rights Management**. La finestra Gestione dovrà essere visualizzato e indicare che il cluster dispone di server aggiuntivi nel cluster.

14. Dopo aver confermato l'installazione del server, configurare il servizio di bilanciamento del carico per bilanciare il carico tra i diversi server di AD RMS nel cluster.

#### <a name="adding-a-windows-server-2016-ad-fs-server-for-high-availability"></a>Aggiunta di un Server di Windows Server 2016 AD FS per la disponibilità elevata

È possibile distribuire altri server AD FS per configurare la disponibilità elevata. È possibile scegliere di eseguire questa azione in caso di aumento del traffico ai server AD FS. **Nota: dopo avere generato il livello di comportamento farm, una nuova voce di database verrà immesso in SQL Server 2016 (Adfs Configv3) e il database di configurazione precedente deve essere eliminato prima di continuare con questi passaggi.**

**Per aggiungere il server AD FS di Windows Server 2016 per la disponibilità elevata**

1.  Installare il ruolo AD RMS nella distribuzione di Windows Server 2016 desiderata.

2.  Al termine dell'installazione, selezionare il collegamento a **configurare il servizio federativo nel server**.

3.  Nella sezione iniziale della procedura guidata, scegliere l'opzione **aggiungere un server federativo a una server farm federativa** e quindi fare clic su **successivo**.

4.  Specificare l'account amministratore appropriato e fare clic su **successivo**.

5.  Nel **impostazione Farm** pagina, selezionare la **specificare il percorso del database per una farm esistente con SQL Server** quindi immettere il record CNAME per il servizio SQL per il nome Host Database e fare clic su **Next**.

6.  Sotto il **impostazione Account del servizio** area della procedura guidata, immettere le credenziali per l'account del servizio AD FS e quindi fare clic su **successivo**.

7.  Nelle **opzioni di revisione**, fare clic su **successivo**.

8.  Fare clic su **configura** quando il pulsante diventa disponibile.

9.  Dopo la configurazione, riavviare il computer.

10. Dopo aver confermato l'installazione di server, il bilanciamento del carico i server AD FS in base alle esigenze.

#### <a name="adding-a-windows-server-2016-wap-server-for-high-availability"></a>Aggiunta di un Server WAP di Windows Server 2016 per la disponibilità elevata

È possibile distribuire ulteriori server WAP per configurare la disponibilità elevata. È possibile scegliere di eseguire questa azione in caso di aumento del traffico ai server AD RMS.

**Per aggiungere un server Proxy applicazione Web di Windows Server 2016 per la disponibilità elevata**

1.  Dal server di cui si desidera configurare come Proxy applicazione Web, passare alla console di Server Manager e fare clic su **Aggiungi ruoli e funzionalità**.

2.  Nel **Aggiunta guidata ruoli e funzionalità**, fare clic su **successivo** finché non viene visualizzata la schermata di selezione ruolo del Server.

3.  Nella schermata di selezione ruoli Server, selezionare **accesso remoto**, quindi fare clic su **successivo** finché non si torna alla schermata di selezione ruoli Server.

4.  Nella schermata di selezione ruoli Server, selezionare **Proxy applicazione Web**, fare clic su **Aggiungi funzionalità**, quindi fare clic su **Next**.

5.  Nella schermata di conferma selezioni per l'installazione, fare clic su **installare**.

6.  Dopo aver completato l'installazione, fare clic su **Chiudi**.

7.  A questo punto è possibile configurare il server. A tale scopo, aprire la console Gestione accesso remoto nel server Proxy applicazione Web. Aprire il **avviare** menu, digitare **RAMgmtUI.exe**e quindi selezionare l'applicazione.

8.  Nel riquadro di spostamento, fare clic su **Proxy applicazione Web**.

9.  Nella console di gestione accesso remoto fare clic su **eseguire la configurazione guidata Proxy applicazione Web**. Una volta nella procedura guidata, fare clic su **successivo**.

10. Nella schermata di Server federativo, immettere il nome di dominio completo del server AD FS (ex. ADFS.contoso.com) e quindi immettere le credenziali per un amministratore nel server AD FS.

11. Nella schermata certificato Proxy ADFS nell'elenco dei certificati installati nel server Proxy applicazione Web, selezionare un certificato utilizzabile da Proxy applicazione Web per il proxy ADFS e quindi fare clic su **successivo**.

12. Nella schermata di conferma, rivedere le impostazioni, quindi fare clic su **configura**.

13. Dopo aver completata la configurazione, fare clic su **Chiudi**.

14. Dopo aver confermato l'installazione del server, il bilanciamento del carico i server WAP nella rete Perimetrale.

#### <a name="adding-a-sql-server-2016-node-for-always-on-high-availability"></a>Aggiunta di un nodo di SQL Server 2016 per la disponibilità elevata Always On

È possibile distribuire altri server SQL per la configurazione a disponibilità elevata Always On. È possibile scegliere di eseguire questa azione in caso di aumento del traffico ai server AD RMS. **Nota: assicurarsi che entrambi i server SQL abbiano la porta 5022 open in ingresso.**

**Per aggiungere un server SQL server 2016 per la disponibilità elevata Always On**

1.  Dal server di cui si desidera configurare come un ulteriore server di SQL Server 2016, passare alla console di Server Manager e fare clic su **Aggiungi ruoli e funzionalità**.

2.  Fare clic su **successivo** fino a quando il **Seleziona funzionalità** nella finestra di dialogo.

3.  Selezionare il **Clustering di Failover** casella di controllo. **Nota: eseguire questo passaggio per l'originale di SQL server 2016 server anche in modo che entrambi i server SQL abbiano la funzionalità Clustering di Failover.**

4.  Fare clic su **installare** per installare la funzionalità Clustering di Failover.

5.  A questo punto, aprire **Server Manager** e selezionare **Tools** quindi **gestione Cluster di Failover**.

6.  Nel riquadro del menu a sinistra, fare doppio clic su **gestione Cluster di Failover** e selezionare **crea Cluster**

7.  Verrà aperta la **Creazione guidata Cluster**.

8.  Cercare il server SQL server 2016 che verranno usate per la disponibilità elevata Always On e inserirle nella quindi fare clic su **successivo**.

9.  Si riceverà un avviso di convalida. Selezionare **Yes** per convalidare i nodi del Cluster e quindi fare clic su **successivo**.

10. Sotto il **opzioni di Testing** pagina, selezionare l'opzione **Esegui tutti i test** e fare clic su **successivo.**

11. **Nota: La convalida guidata del Cluster deve restituire diversi messaggi di avviso, in particolare se si utilizzerà non archiviazione condivisa. A parte questo, se si trovano eventuali messaggi di errore è necessario correggerli prima di creare il Cluster di Failover di Windows Server**.

12. Nel **punto di accesso per l'amministrazione del Cluster** della finestra di dialogo immettere il nome del cluster e indirizzo IP virtuale per il Cluster di Failover di Windows Server, quindi fare clic su **successivo**.

13. Verificare che la configurazione sia corretta in **Summary** e fare clic su **fine**.

14. Nel **gestione Cluster di Failover** pulsante destro del mouse sul cluster, quindi scegliere **altre azioni** quindi **Configura impostazioni quorum del Cluster**

15. Fare clic su **successivo** e quindi selezionare l'opzione relativa **seleziona il quorum di controllo** e fare clic su **Avanti** nuovamente.

16. Nel **selezione Quorum di controllo** pagina, selezionare la **Configura condivisione file di controllo** opzione. Fare quindi clic su **Avanti**.

17. Selezionare **esplorare** e individuare il percorso della condivisione file che si desidera utilizzare nella finestra di dialogo Percorso condivisione File. Fare clic su **Avanti**.

18. Nella pagina di conferma, fare clic su **successivo**.

19. Nella pagina di riepilogo, fare clic su **fine**.

20. A questo punto, aprire il **avviare** dal menu e cercare **Gestione configurazione SQL Server**.

21. Fare doppio clic il nome del Server SQL e scegliere **proprietà**.

22. Nella finestra di dialogo proprietà, selezionare la **disponibilità elevata AlwaysOn** scheda. Verificare i **Abilita gruppi di disponibilità AlwaysOn** casella di controllo. Fare clic su **OK**. **Nota: eseguire questa operazione nel server SQL Server 2016.**

23. Quindi riavviare il servizio SQL Server.

24. A questo punto, aprire il **avviare** dal menu e cercare **SQL Server Management Studio** e nel riquadro di spostamento a sinistra, fare doppio clic su **gruppi di disponibilità** e fare clic su **Creazione guidata nuovo gruppo di disponibilità** quindi fare clic su **successiva**.

25. Nel **Specifica nome del gruppo di disponibilità** pagina scegliere un nome di gruppo (Ex.SQLAvailabilityGroup2016). Fare quindi clic su **Avanti**.

26. Sotto il **Seleziona database** sezione, specificare i database. Quindi fare clic su Avanti. **Nota: alcuni database potrebbe essere necessario essere eseguito nuovamente il backup o inserita la modalità di recupero con registrazione completa**.

27. Una volta nel **specifica repliche** pagina, fare clic sui **Add Replica** pulsante e selezionare l'altro Server di SQL 2016.

28. Dopo aver aggiunto altro server, selezionare le caselle di controllo e impostare il server secondario come una replica secondaria leggibile.

29. Passare al **endpoint** scheda e fare clic sui **aggiornare** opzione. Anche in questo caso, durante lo scorrimento e assicurarsi che l'account del servizio stesso sia nel nodo primario e secondario.

30. A questo punto, scegliere il **preferenze di Backup** scheda e selezionare il **preferisce secondario** opzione.

31. Procedere con il **Listener** scheda.

32. Specificare un nome (ad esempio: SQLListener) e assicurarsi che la porta sia **1433** e quindi fare clic su **successivo**.

33. Nel **seleziona sincronizzazione dati iniziale** pagina della procedura guidata, scegliere il **completo** opzione e specificare il percorso di rete accessibile da tutti i server SQL e quindi fare clic su **successivo**.

34. Infine, fare clic su **fine** e il processo verrà completato.

### <a name="decommission-windows-server-2012-r2-nodes"></a>Rimozione di nodi di Windows Server 2012 R2

Le sezioni seguenti forniscono informazioni aggiuntive sulle attività operative, che potrebbe essere necessario rimuovere i server Windows Server 2012 R2 dopo l'aggiornamento del cluster AD RMS per Windows Server 2016.

#### <a name="removing-a-windows-server-2012-r2-ad-rms-server"></a>Rimozione di un Server Windows Server 2012 R2 AD RMS

È possibile rimuovere i server AD RMS non necessari dopo un aggiornamento. È possibile scegliere di eseguire questa azione quando diventa necessario per rimuovere le autorizzazioni dei server AD RMS.

**Per rimuovere un** **server Windows Server 2012 R2 AD RMS**

1.  Nel server di Windows Server 2012 R2 AD RMS in Server Manager, selezionare **Manage** nella parte superiore destra i menu e quindi scegliere **Rimuovi ruoli e funzionalità**.

2.  Il **Rimozione guidata ruoli e funzionalità** verrà aperto e le informazioni sul **prima di iniziare** schermata, fare clic su **Next**.

3.  Nel **selezione del Server** schermata, fare clic su **successivo**.

4.  Nel **ruoli predefiniti del Server** schermata, rimuovere il segno di spunta accanto a **Active Directory Rights Management Services** e fare clic su **Next**.

5.  Nel **caratteristiche** schermata, fare clic su **successivo**.

6.  Nel **conferma** schermata, fare clic su **rimuovere**.

7.  Al termine, riavviare il server.

8.  È ora possibile chiudere questo server e riallocare le risorse in base alle esigenze.
