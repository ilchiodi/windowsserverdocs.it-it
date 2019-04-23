---
title: Active Directory Federation Services in Azure | Microsoft Docs
description: In questo documento si apprenderà come distribuire ADFS in Azure per ottenere una disponibilità elevata.
services: active-directory
documentationcenter: ''
author: billmath
manager: mtillman
editor: ''
ms.assetid: 692a188c-badc-44aa-ba86-71c0e8074510
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 10/28/2018
ms.subservice: hybrid
ms.author: billmath
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 588bc3f87c78feccac47d18d31d37be3b1a02d2f
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59835102"
---
# <a name="deploying-active-directory-federation-services-in-azure"></a>Distribuzione di Active Directory Federation Services in Azure
AD FS offre funzionalità di single sign-on (SSO) Web e federazione delle identità semplificate e protette. Federazione con Azure AD o O365 consente agli utenti di eseguire l'autenticazione usando le credenziali in locale e accedere a tutte le risorse nel cloud. Di conseguenza, diventa importante disporre di un'infrastruttura AD FS a disponibilità elevata per garantire l'accesso alle risorse sia in locale e nel cloud. Distribuzione di AD FS in Azure consente di ottenere la disponibilità elevata necessaria con sforzi minimi.
Esistono diversi vantaggi della distribuzione di AD FS in Azure, di seguito sono elencate alcune di esse:

* **Disponibilità elevata** -grazie alla potenza del set di disponibilità di Azure, garantire un'infrastruttura a disponibilità elevata.
* **Scalabilità semplificata** – necessarie prestazioni superiori? Eseguire facilmente la migrazione a computer più potenti da soli pochi clic in Azure
* **La ridondanza tra aree geografiche** – ridondanza geografica di Azure si possono avere la certezza che l'infrastruttura sia a disponibilità elevata in tutto il mondo
* **Facilità di gestione** – con opzioni di gestione estremamente semplificate nel portale di Azure, la gestione dell'infrastruttura è molto semplice e pratico 

## <a name="design-principles"></a>Principi di progettazione
![Progettazione distribuzione](./media/how-to-connect-fed-azure-adfs/deployment.png)

Il diagramma precedente mostra la topologia di base consigliata per iniziare a distribuire l'infrastruttura AD FS in Azure. Di seguito sono elencati i principi alla base di vari componenti della topologia:

* **Controller di dominio / server ad FS**: Se si dispone di meno di 1000 utenti, è sufficiente installare il ruolo di AD FS nei controller di dominio. Se è necessario evitare qualsiasi impatto sulle prestazioni nei controller di dominio o se sono presenti più di 1.000 utenti, distribuire AD FS su server separati.
* **Server WAP** : è necessario distribuire i server Proxy applicazione Web, in modo che gli utenti possano raggiungere AD FS quando si trovano nella rete aziendale anche.
* **DMZ**: Il server Proxy applicazione Web verranno inseriti nella rete Perimetrale e l'accesso solo TCP/443 è consentito tra la rete Perimetrale e la subnet interna.
* **Bilanciamento del carico**: Per garantire un'elevata disponibilità dei server AD FS e Proxy applicazione Web, è consigliabile usare un servizio di bilanciamento del carico interno per i server AD FS e Azure Load Balancer per i server Proxy applicazione Web.
* **Set di disponibilità**: Per fornire ridondanza per la distribuzione di AD FS, è consigliabile raggruppare due o più macchine virtuali in un Set di disponibilità per carichi di lavoro simili. Questa configurazione assicura infatti che durante un evento di manutenzione pianificata o non pianificata, almeno una macchina virtuale sarà disponibile
* **Gli account di archiviazione**: È consigliabile avere due account di archiviazione. Presenza di un singolo account di archiviazione può causare la creazione di un singolo punto di errore e può causare la distribuzione è più disponibile nell'improbabile scenario in cui l'account di archiviazione diventa inattivo. Due account di archiviazione consente di associare un account di archiviazione per ogni linea di guasto.
* **Separazione delle reti**:  Server Proxy applicazione Web devono essere distribuiti in una rete Perimetrale separata. È possibile dividere una rete virtuale in due subnet e quindi distribuire il server Proxy applicazione Web in una subnet isolata. È sufficiente, è possibile configurare le impostazioni di gruppo di sicurezza di rete per ogni subnet e consentire solo la comunicazione necessaria tra le due subnet. Altri dettagli sono disponibili per ogni scenario di distribuzione seguente

## <a name="steps-to-deploy-ad-fs-in-azure"></a>Passaggi per distribuire AD FS in Azure
I passaggi indicati in questa sezione offrono una Guida per distribuire la seguente infrastruttura AD FS illustrata in Azure.

### <a name="1-deploying-the-network"></a>1. Distribuire la rete
Come illustrato in precedenza, è possibile sia creare due subnet in una singola rete virtuale oppure creare due reti virtuali completamente diverse (VNet). Questo articolo sarà incentrato sulla distribuzione di una singola rete virtuale e dividerlo in due subnet. È attualmente un approccio più semplice perché due reti virtuali separate richiederebbero un gateway di rete virtuale per le comunicazioni.

**1.1 creare una rete virtuale**

![Creare una rete virtuale](./media/how-to-connect-fed-azure-adfs/deploynetwork1.png)

Nel portale di Azure, selezionare la rete virtuale ed è possibile distribuire la rete virtuale e una subnet immediatamente con un solo clic. Subnet INT viene definita anche che è pronta per le macchine virtuali da aggiungere.
Il passaggio successivo consiste nell'aggiungere un'altra subnet alla rete, vale a dire le subnet della rete Perimetrale. Per creare la subnet di rete Perimetrale, è sufficiente

* Selezionare la rete appena creata
* Nelle proprietà selezionare subnet
* Nella subnet del pannello fare clic sul pulsante Aggiungi
* Fornire le informazioni di spazio nome e indirizzo di subnet per creare la subnet

![Subnet](./media/how-to-connect-fed-azure-adfs/deploynetwork2.png)

![Subnet DMZ](./media/how-to-connect-fed-azure-adfs/deploynetwork3.png)

**1.2. Creazione di gruppi di sicurezza di rete**

Un gruppo di sicurezza di rete (NSG) contiene un elenco di regole di elenco di controllo di accesso (ACL) che consentono o rifiutano il traffico di rete alle istanze di macchina virtuale in una rete virtuale. Gli Nsg possono essere associati a una subnet o singole istanze VM in tale subnet. Quando un NSG è associato a una subnet, le regole ACL si applicano a tutte le istanze di macchina virtuale in tale subnet.
Ai fini di questa Guida, si creerà due Nsg: una per una rete interna e una rete Perimetrale. Si sarà denominate NSG_INT e NSG_DMZ rispettivamente.

![Creare NSG](./media/how-to-connect-fed-azure-adfs/creatensg1.png)

Dopo aver creato il gruppo di sicurezza, saranno presenti 0 regole in ingresso e 0 in uscita. Dopo i ruoli nei rispettivi server sono installati e funzionanti, le regole in ingresso e in uscita possono essere reso a seconda del livello di sicurezza desiderato.

![Inizializzare NSG](./media/how-to-connect-fed-azure-adfs/nsgint1.png)

Dopo aver creato il Nsg, associare nsg\_int alla subnet INT e nsg\_dmz alla subnet della rete Perimetrale. Uno screenshot di esempio è indicato di seguito:

![Configurazione di sicurezza di rete](./media/how-to-connect-fed-azure-adfs/nsgconfigure1.png)

* Fare clic su subnet per aprire il pannello delle subnet
* Selezionare la subnet da associare il NSG 

Dopo la configurazione, il pannello delle subnet appariranno come indicato di seguito:

![Subnet dopo la sicurezza di rete](./media/how-to-connect-fed-azure-adfs/nsgconfigure2.png)

**1.3. Crea connessione ai servizi locali**

È necessaria una connessione a un'istanza locale per distribuire il controller di dominio (DC) in azure. Azure offre varie opzioni di connettività per connettere l'infrastruttura locale all'infrastruttura di Azure.

* Point-to-site
* Rete virtuale Site-to-site
* ExpressRoute

È consigliabile usare ExpressRoute. ExpressRoute consente di creare connessioni private tra Data Center di Azure e l'infrastruttura presente localmente o in un ambiente con percorso condiviso. Le connessioni ExpressRoute non entrano sulla rete Internet pubblica. Offrono più affidabilità, velocità più elevate, latenze più basse e maggiore sicurezza rispetto alle tipiche connessioni tramite Internet.
Sebbene sia consigliabile usare ExpressRoute, è possibile scegliere qualsiasi metodo di connessione più adatto per l'organizzazione. Per altre informazioni su ExpressRoute e le diverse opzioni di connettività con ExpressRoute, leggere [Panoramica tecnica di ExpressRoute](https://aka.ms/Azure/ExpressRoute).

### <a name="2-create-storage-accounts"></a>2. Creare gli account di archiviazione
Per mantenere la disponibilità elevata e non dipendere da un singolo account di archiviazione, è possibile creare due account di archiviazione. Dividere i computer di ogni set di disponibilità in due gruppi e quindi assegnare ogni gruppo un account di archiviazione separato.

![Creare gli account di archiviazione](./media/how-to-connect-fed-azure-adfs/storageaccount1.png)

### <a name="3-create-availability-sets"></a>3. Creare set di disponibilità
Per ogni ruolo (controller di dominio/AD FS e WAP), creare set di disponibilità che conterrà 2 computer ognuno minimo. Ciò consente di ottenere una maggiore disponibilità per ogni ruolo. Sebbene la disponibilità di creazione di set, è essenziale stabilire quanto segue:

* **Domini di errore**: Le macchine virtuali nello stesso dominio di errore condividono la stessa alimentazione e switch di rete fisica. Sono consigliati almeno 2 domini di errore. Il valore predefinito è 3 ed è possibile lasciare ai fini di questa distribuzione
* **Domini di aggiornamento**: Le macchine appartenenti allo stesso dominio di aggiornamento vengono riavviate contemporaneamente durante un aggiornamento. Opportuno avere almeno 2 domini di aggiornamento. Il valore predefinito è 5 ed è possibile lasciare ai fini di questa distribuzione

![Set di disponibilità](./media/how-to-connect-fed-azure-adfs/availabilityset1.png)

Creare i seguenti set di disponibilità

| Set di disponibilità | Ruolo | Domini di errore | Domini di aggiornamento |
|:---:|:---:|:---:|:--- |
| contosodcset |DC/ADFS |3 |5 |
| contosowapset |WAP |3 |5 |

### <a name="4-deploy-virtual-machines"></a>4. Distribuire macchine virtuali
Il passaggio successivo consiste nel distribuire le macchine virtuali che ospiteranno i ruoli diversi all'interno dell'infrastruttura. Un minimo di due macchine sono consigliate in ogni set di disponibilità. Creare quattro macchine virtuali per la distribuzione di base.

| Computer | Ruolo | Subnet | Set di disponibilità | Account di archiviazione | Indirizzo IP |
|:---:|:---:|:---:|:---:|:---:|:---:|
| contosodc1 |DC/ADFS |INT |contosodcset |contososac1 |Static |
| contosodc2 |DC/ADFS |INT |contosodcset |contososac2 |Static |
| contosowap1 |WAP |DMZ |contosowapset |contososac1 |Static |
| contosowap2 |WAP |DMZ |contosowapset |contososac2 |Static |

Come si può notare, non è stato specificato alcun gruppo di sicurezza di rete. In questo modo azure ti permette di usare NSG a livello di subnet. Quindi, è possibile controllare il traffico di rete di computer utilizzando il NSG singoli associato non la subnet oppure l'oggetto di interfaccia di rete. Altre informazioni, Leggi [che cos'è un gruppo di sicurezza di rete (NSG)](https://aka.ms/Azure/NSG).
Indirizzo IP statico è consigliato se si sta gestendo il DNS. È possibile usare DNS di Azure e invece nei record DNS per il dominio, fare riferimento a nuovi computer con i nomi di dominio completo di Azure.
Il riquadro della macchina virtuale dovrebbe essere simile di sotto al termine della distribuzione:

![Macchine virtuali distribuite](./media/how-to-connect-fed-azure-adfs/virtualmachinesdeployed_noadfs.png)

### <a name="5-configuring-the-domain-controller--ad-fs-servers"></a>5. Configurazione del controller di dominio / server AD FS
 Per autenticare qualsiasi richiesta in ingresso, AD FS sarà necessario contattare il controller di dominio. Per evitare il costoso trasferimento da Azure al controller di dominio locale per l'autenticazione, è consigliabile distribuire una replica del controller di dominio in Azure. Per ottenere una disponibilità elevata, è consigliabile creare un set di disponibilità di almeno 2 controller di dominio.

| Controller di dominio | Ruolo | Account di archiviazione |
|:---:|:---:|:---:|
| contosodc1 |Replica |contososac1 |
| contosodc2 |Replica |contososac2 |

* Promuovere i due server come controller di dominio di replica con DNS
* Configurare i server AD FS installando il ruolo di AD FS con server manager.

### <a name="6-deploying-internal-load-balancer-ilb"></a>6. Distribuzione di servizio di bilanciamento del carico interno (ILB)
**6.1. Creare il bilanciamento del carico interno**

Per distribuire un bilanciamento del carico interno, selezionare servizi di bilanciamento del carico nel portale di Azure e fare clic su Aggiungi (+).

> [!NOTE]
> Se non viene visualizzata **servizi di bilanciamento del carico** nel menu, fare clic su **Sfoglia** in basso a sinistra nel portale e scorrere fino a quando non viene visualizzato **servizi di bilanciamento del carico**.  Scegliere la stella gialla per aggiungerla al menu. Selezionare ora la nuova icona di servizio di bilanciamento di carico per aprire il pannello per avviare la configurazione del bilanciamento del carico.
> 
> 

![Sfoglia bilanciamento del carico](./media/how-to-connect-fed-azure-adfs/browseloadbalancer.png)

* **Nome**: Assegnare qualsiasi nome idoneo al bilanciamento del carico
* **Schema**: Poiché questo servizio di bilanciamento del carico verrà inserito a monte i server AD FS e è destinata solo le connessioni di rete interna, selezionare "Interno"
* **Rete virtuale**: Scegliere la rete virtuale in cui si distribuisce ADFS
* **Subnet**: Scegliere la subnet interna
* **Assegnazione di indirizzi IP**: Static

![Servizio di bilanciamento del carico interno](./media/how-to-connect-fed-azure-adfs/ilbdeployment1.png)

Dopo aver fatto clic creare e il bilanciamento del carico interno viene distribuito, è necessario visualizzarlo nell'elenco dei servizi di bilanciamento del carico:

![Servizi di bilanciamento del carico dopo il bilanciamento del carico interno](./media/how-to-connect-fed-azure-adfs/ilbdeployment2.png)

Passaggio successivo consiste nel configurare il pool back-end e il probe di back-end.

**6.2. Configurare pool di back-end di bilanciamento del carico interno**

Selezionare il bilanciamento del carico interno appena creato nel pannello dei bilanciamenti del carico. Verrà aperto il pannello impostazioni. 

1. Selezionare i pool back-end nel pannello delle impostazioni
2. Nel Pannello di pool back-end di aggiunta, fare clic su Aggiungi una macchina virtuale
3. Verrà visualizzato un pannello in cui è possibile scegliere set di disponibilità
4. Scegliere il set di disponibilità AD FS

![Configurare pool di back-end di bilanciamento del carico interno](./media/how-to-connect-fed-azure-adfs/ilbdeployment3.png)

**6.3. Configurare il probe**

Nel pannello delle impostazioni di bilanciamento del carico interno, selezionare probe di integrità.

1. Fare clic su Aggiungi
2. Fornire i dettagli per il probe una. **Nome**: Nome del probe b. **Protocollo**: HTTP c. **Porta**: 80 (HTTP) d. **Percorso**: probe/adfs/e. **Interval**: 5 (valore predefinito) – si tratta dell'intervallo in corrispondenza del quale bilanciamento del carico interno sarà il probe dei computer nel pool di back-end f. **Soglia di non integrità**: 2 (valore predefinito) – si tratta della soglia degli errori di probe consecutivi dopo il quale bilanciamento del carico interno dichiarerà che un computer nel pool di back-end non risponde e interromperà l'invio di traffico.

![Configurare il probe di bilanciamento del carico interno](./media/how-to-connect-fed-azure-adfs/ilbdeployment4.png)

Si sta usando l'endpoint /adfs/probe creato esplicitamente per i controlli di integrità in un ambiente di AD FS in cui non può verificarsi un controllo completo di percorso HTTPS.  Questo è sostanzialmente migliore rispetto a un controllo di base porta 443, non riflettere lo stato di una distribuzione moderna di ADFS.  Altre informazioni su questo sono reperibile in https://blogs.technet.microsoft.com/applicationproxyblog/2014/10/17/hardware-load-balancer-health-checks-and-web-application-proxy-ad-fs-2012-r2/.

**6.4. Creare regole di bilanciamento del carico**

Per bilanciare efficacemente il traffico, il bilanciamento del carico interno deve essere configurato con regole di bilanciamento del carico. Per creare una regola di bilanciamento del carico 

1. Selezionare il bilanciamento del carico regola nel pannello delle impostazioni del servizio ILB
2. Fare clic su Aggiungi nel pannello regole di bilanciamento del carico
3. Nel pannello regole di bilanciamento del carico di Aggiungi una. **Nome**: Specificare un nome per la regola b. **Protocollo**: Selezionare TCP c. **Porta**: 443 d. **Porta back-end**: 443 e. **Pool back-end**: Selezionare il pool creato per il cluster AD FS f precedenti. **Probe**: Selezionare il probe creato in precedenza per i server AD FS

![Configurare regole di bilanciamento del bilanciamento del carico interno](./media/how-to-connect-fed-azure-adfs/ilbdeployment5.png)

**6.5. Aggiornare il DNS con bilanciamento del carico interno**

Passare al server DNS e creare un record CNAME per il bilanciamento del carico interno. Deve essere il record CNAME per il servizio federativo con l'indirizzo IP che punta all'indirizzo IP dell'ILB. Ad esempio se l'indirizzo DIP di bilanciamento del carico interno è 10.3.0.8 e il servizio federativo installato è fs.contoso.com, quindi creare un record CNAME per fs.contoso.com che punta a 10.3.0.8.
Ciò garantisce che tutte le comunicazioni relativa a FS.contoso.com raggiungerà il servizio ILB e sono indirizzate in modo appropriato.

### <a name="7-configuring-the-web-application-proxy-server"></a>7. Configurazione del server Proxy applicazione Web
**7.1. Configurare i server Proxy applicazione Web per raggiungere i server AD FS**

Per garantire che i server Proxy applicazione Web sono in grado di raggiungere i server AD FS dietro il bilanciamento del carico interno, creare un record nel %systemroot%\system32\drivers\etc\hosts per il bilanciamento del carico interno. Si noti che il nome distinto (DN) deve essere il nome del servizio federativo, ad esempio fs.contoso.com. E la voce dell'indirizzo IP deve essere quella dell'indirizzo IP del bilanciamento del carico interno (10.3.0.8 nell'esempio).

**7.2. Installare il ruolo Proxy applicazione Web**

Dopo essersi assicurati che i server Proxy applicazione Web sono in grado di raggiungere i server AD FS dietro bilanciamento del carico interno, è possibile installare il server Proxy applicazione Web. Server Proxy applicazione Web non devono essere aggiunti al dominio. Installare i ruoli Proxy applicazione Web nei due server Proxy applicazione Web selezionando il ruolo Accesso remoto. Server manager guiderà l'utente per completare l'installazione di WAP.
Per altre informazioni su come distribuire WAP, leggere [installare e configurare il Server Proxy applicazione Web](https://technet.microsoft.com/library/dn383662.aspx).

### <a name="8--deploying-the-internet-facing-public-load-balancer"></a>8.  Distribuzione di Internet al bilanciamento del carico (pubblico)
**8.1.  Crea servizio di bilanciamento del carico (pubblico) è connessa a Internet**

Nel portale di Azure, selezionare servizi di bilanciamento del carico e quindi fare clic su Aggiungi. Nel pannello del servizio di bilanciamento carico Create, immettere le informazioni seguenti

1. **Nome**: Nome per il bilanciamento del carico
2. **Schema**: Pubblico: questa opzione indica ad Azure che questo servizio di bilanciamento del carico sarà necessario un indirizzo pubblico.
3. **Indirizzo IP**: Creare un nuovo indirizzo IP (dinamico)

![Bilanciamento del carico con connessione Internet](./media/how-to-connect-fed-azure-adfs/elbdeployment1.png)

Dopo la distribuzione, il servizio di bilanciamento del carico verrà visualizzato nell'elenco di servizi di bilanciamento di carico.

![Elenco di Load balancer](./media/how-to-connect-fed-azure-adfs/elbdeployment2.png)

**8.2. Assegnare un'etichetta DNS all'IP pubblico**

Fare clic sulla voce del servizio di bilanciamento carico appena creato nel pannello servizi di bilanciamento di carico per visualizzare il pannello per la configurazione. Seguire questi passaggi per configurare l'etichetta DNS per l'indirizzo IP pubblico:

1. Fare clic sull'indirizzo IP pubblico. Si aprirà il pannello per l'indirizzo IP pubblico e le relative impostazioni
2. Fare clic su configurazione
3. Specificare un'etichetta DNS. Che diventerà l'etichetta DNS pubblica accessibile da qualsiasi posizione, ad esempio contosofs.westus.cloudapp.azure.com. È possibile aggiungere una voce nel DNS esterno per il servizio federativo (ad esempio, fs.contoso.com) che viene risolta nell'etichetta DNS del servizio di bilanciamento del carico esterno (contosofs.westus.cloudapp.azure.com).

![Configurare il bilanciamento del carico con connessione internet](./media/how-to-connect-fed-azure-adfs/elbdeployment3.png) 

![Configurare il bilanciamento del carico (DNS) internet](./media/how-to-connect-fed-azure-adfs/elbdeployment4.png)

**8.3. Configurare i pool back-end di bilanciamento del carico con connessione Internet (pubblico)** 

Seguire la stessa procedura indicata per la creazione di bilanciamento del carico interno, per configurare il pool back-end per con connessione Internet (pubblico) Azure Load Balancer come set di disponibilità per i server WAP. Ad esempio contosowapset.

![Configurare il pool back-end di bilanciamento del carico con connessione Internet](./media/how-to-connect-fed-azure-adfs/elbdeployment5.png)

**8.4. Configurare il probe**

Seguire la stessa procedura impiegata durante la configurazione di bilanciamento del carico interno per configurare il probe per il pool back-end dei server WAP.

![Configurare il probe di bilanciamento del carico con connessione Internet](./media/how-to-connect-fed-azure-adfs/elbdeployment6.png)

**8.5. Creare regole di bilanciamento del carico**

Seguire la stessa procedura indicata bilanciamento del carico interno per configurare il bilanciamento del carico regola per la porta TCP 443.

![Configurare le regole di bilanciamento del carico di bilanciamento del carico con connessione Internet](./media/how-to-connect-fed-azure-adfs/elbdeployment7.png)

### <a name="9-securing-the-network"></a>9. Protezione della rete
**9.1. Proteggere la subnet interna**

In generale, è necessario le regole seguenti per proteggere efficacemente la subnet interna (nell'ordine indicato di seguito)

| Regola | Descrizione | Flusso |
|:--- |:--- |:---:|
| AllowHTTPSFromDMZ |Consente la comunicazione HTTPS dalla rete Perimetrale |In entrata |
| DenyInternetOutbound |Nessun accesso a internet |In uscita |

![Regole di accesso interno (in ingresso)](./media/how-to-connect-fed-azure-adfs/nsg_int.png)

<!--
[comment]: <> (![INT access rules (inbound)](./media/how-to-connect-fed-azure-adfs/nsgintinbound.png))
[comment]: <> (![INT access rules (outbound)](./media/how-to-connect-fed-azure-adfs/nsgintoutbound.png))
-->

**9.2. Proteggere la subnet Perimetrale**

| Regola | Descrizione | Flusso |
|:--- |:--- |:---:|
| AllowHTTPSFromInternet |Consente il traffico HTTPS da internet alla rete Perimetrale |In entrata |
| DenyInternetOutbound |Diverso da HTTPS a internet è bloccato |In uscita |

![Regole di accesso esterno (in ingresso)](./media/how-to-connect-fed-azure-adfs/nsg_dmz.png)

<!--
[comment]: <> (![EXT access rules (inbound)](./media/how-to-connect-fed-azure-adfs/nsgdmzinbound.png))
[comment]: <> (![EXT access rules (outbound)](./media/how-to-connect-fed-azure-adfs/nsgdmzoutbound.png))
-->

> [!NOTE]
> Se l'autenticazione del certificato client utente (autenticazione clientTLS utilizzando X509 i certificati utente) è obbligatorio, ADFS richiede che TCP sia attivato 49443 la porta per l'accesso in ingresso.
> 
> 

### <a name="10-test-the-ad-fs-sign-in"></a>10. Testare l'accesso aggiuntivo di AD FS
Il modo più semplice è per testare AD FS consiste nell'usare la pagina Idpinitiatedsignon. Per essere in grado di eseguire operazioni che, è necessario per abilitare IdpInitiatedSignOn nelle proprietà di AD FS. Attenersi alla procedura seguente per verificare l'installazione di AD FS

1. Eseguire il seguente cmdlet nel server di AD FS, uso di PowerShell, per impostare l'abilitazione.
   Set-AdfsProperties -EnableIdPInitiatedSignonPage $true 
2. Da qualsiasi computer esterno, accedere a https:\//adfs-server.contoso.com/adfs/ls/IdpInitiatedSignon.aspx.  
3. Verrà visualizzata la pagina di AD FS come il seguente:

![Pagina di accesso di test](./media/how-to-connect-fed-azure-adfs/test1.png)

Su riuscito Accedi, fornirà all'utente un messaggio di conferma come illustrato di seguito:

![Test riuscito](./media/how-to-connect-fed-azure-adfs/test2.png)

## <a name="template-for-deploying-ad-fs-in-azure"></a>Modello per la distribuzione di AD FS in Azure
Il modello distribuisce una configurazione a 6 computer, 2 per i controller di dominio, AD FS e WAP.

[AD FS nel modello di distribuzione di Azure](https://github.com/paulomarquesc/adfs-6vms-regular-template-based)

È possibile usare una rete virtuale esistente o crearne una nuova rete virtuale durante la distribuzione di questo modello. I diversi parametri disponibili per la personalizzazione della distribuzione sono elencati di seguito con la descrizione dell'uso del parametro nel processo di distribuzione. 

| Parametro | Descrizione |
|:--- |:--- |
| Location |L'area per distribuire le risorse, ad esempio East US. |
| StorageAccountType |Il tipo di Account di archiviazione creato |
| VirtualNetworkUsage |Indica se una nuova rete virtuale verrà creata o usarne uno esistente |
| VirtualNetworkName |Il nome della rete virtuale da creare, obbligatorio in caso di utilizzo della rete virtuale nuova o esistente |
| VirtualNetworkResourceGroupName |Specifica il nome del gruppo di risorse in cui si trova la rete virtuale esistente. Quando si usa una rete virtuale esistente, questo diventa un parametro obbligatorio per consentire la distribuzione di trovare l'ID della rete virtuale esistente |
| VirtualNetworkAddressRange |Intervallo di indirizzi della nuova rete virtuale, obbligatorio in caso di creazione di una nuova rete virtuale |
| InternalSubnetName |Il nome della subnet interna, obbligatorio in caso di entrambe le opzioni di utilizzo della rete virtuale (nuove o esistente) |
| InternalSubnetAddressRange |L'intervallo di indirizzi della subnet interna, che contiene il server controller di dominio e ad FS, obbligatorio se la creazione di una nuova rete virtuale. |
| DMZSubnetAddressRange |Intervallo di indirizzi della subnet della rete perimetrale, che contiene server proxy applicazione di Windows, obbligatorio in caso di creazione di una nuova rete virtuale. |
| DMZSubnetName |Il nome della subnet interna, obbligatorio in caso di entrambe le opzioni di utilizzo della rete virtuale (nuove o esistente). |
| ADDC01NICIPAddress |L'indirizzo IP interno del primo Controller di dominio, questo indirizzo IP verrà assegnato in modo statico al controller di dominio e deve essere un indirizzo ip valido nella subnet interna |
| ADDC02NICIPAddress |L'indirizzo IP interno del secondo Controller di dominio, questo indirizzo IP verrà assegnato in modo statico al controller di dominio e deve essere un indirizzo ip valido nella subnet interna |
| ADFS01NICIPAddress |L'indirizzo IP interno del primo server ad FS, questo indirizzo IP verrà assegnato in modo statico al server ad FS e deve essere un indirizzo ip valido nella subnet interna |
| ADFS02NICIPAddress |L'indirizzo IP interno del secondo server ad FS, questo indirizzo IP verrà assegnato in modo statico al server ad FS e deve essere un indirizzo ip valido nella subnet interna |
| WAP01NICIPAddress |L'indirizzo IP interno del primo server WAP, questo indirizzo IP verrà assegnato in modo statico al server WAP e deve essere un indirizzo ip valido nella subnet della rete Perimetrale |
| WAP02NICIPAddress |L'indirizzo IP interno del secondo server WAP, questo indirizzo IP verrà assegnato in modo statico al server WAP e deve essere un indirizzo ip valido nella subnet della rete Perimetrale |
| ADFSLoadBalancerPrivateIPAddress |L'indirizzo IP interno del bilanciamento del carico ad FS, questo indirizzo IP verrà assegnato in modo statico al bilanciamento del carico e deve essere un indirizzo ip valido nella subnet interna |
| ADDCVMNamePrefix |Prefisso del nome macchina virtuale per i controller di dominio |
| ADFSVMNamePrefix |Prefisso del nome macchina virtuale per i server ad FS |
| WAPVMNamePrefix |Prefisso del nome macchina virtuale per i server WAP |
| ADDCVMSize |Le dimensioni della vm dei controller di dominio |
| ADFSVMSize |Le dimensioni della vm dei server ad FS |
| WAPVMSize |Le dimensioni della vm dei server WAP |
| AdminUserName |Il nome dell'amministratore locale delle macchine virtuali |
| AdminPassword |La password per l'account amministratore locale delle macchine virtuali |

## <a name="additional-resources"></a>Risorse aggiuntive
* [Set di disponibilità](https://aka.ms/Azure/Availability) 
* [Servizio di bilanciamento del carico di Azure](https://aka.ms/Azure/ILB)
* [Servizio di bilanciamento del carico interno](https://aka.ms/Azure/ILB/Internal)
* [Servizio di bilanciamento del carico con connessione Internet](https://aka.ms/Azure/ILB/Internet)
* [Account di archiviazione](https://aka.ms/Azure/Storage)
* [Reti virtuali di Azure](https://aka.ms/Azure/VNet)
* [AD FS e i collegamenti di Proxy applicazione Web](https://aka.ms/ADFSLinks) 

## <a name="next-steps"></a>Passaggi successivi
* [Integrazione delle identità locali con Azure Active Directory](https://docs.microsoft.com/azure/active-directory/hybrid/whatis-hybrid-identity)
* [Configurazione e gestione di AD FS con Azure AD Connect](/azure/active-directory/hybrid/how-to-connect-fed-whatis)
* [Distribuzione di disponibilità elevata tra aree geografiche AD FS in Azure con gestione traffico di Azure](active-directory-adfs-in-azure-with-azure-traffic-manager.md)
