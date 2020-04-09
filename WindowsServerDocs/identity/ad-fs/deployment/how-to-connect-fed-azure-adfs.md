---
title: Active Directory Federation Services in Azure | Microsoft Docs
description: In questo documento si apprenderà come distribuire AD FS in Azure per disponibilità elevati.
author: billmath
manager: mtillman
ms.assetid: 692a188c-badc-44aa-ba86-71c0e8074510
ms.topic: get-started-article
ms.date: 10/28/2018
ms.subservice: hybrid
ms.author: billmath
ms.openlocfilehash: 5701e7955a3baff248c0f7efc0b1de03088a8aa0
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80854954"
---
# <a name="deploying-active-directory-federation-services-in-azure"></a>Distribuzione di Active Directory Federation Services in Azure
AD FS offre funzionalità semplificate e protette di Federazione delle identità e di Single Sign-On Web (SSO). La Federazione con Azure AD o O365 consente agli utenti di eseguire l'autenticazione usando le credenziali locali e di accedere a tutte le risorse nel cloud. Di conseguenza, diventa importante disporre di un'infrastruttura AD FS a disponibilità elevata per garantire l'accesso alle risorse sia in locale che nel cloud. La distribuzione di AD FS in Azure consente di ottenere la disponibilità elevata necessaria con attività minime.
La distribuzione di AD FS in Azure presenta diversi vantaggi, alcune delle quali sono elencate di seguito:

* **Disponibilità elevata** : con la potenza dei set di disponibilità di Azure, si garantisce un'infrastruttura a disponibilità elevata.
* **Facile da scalare** : è necessario migliorare le prestazioni? Puoi eseguire facilmente la migrazione a computer più potenti con pochi clic in Azure
* **Ridondanza tra Geo** : con la ridondanza geografica di Azure è possibile garantire che l'infrastruttura sia a disponibilità elevata in tutto il mondo
* **Facile da gestire** : con opzioni di gestione estremamente semplificate in portale di Azure, la gestione dell'infrastruttura è molto semplice e senza problemi 

## <a name="design-principles"></a>Principi di progettazione
![Progettazione della distribuzione](./media/how-to-connect-fed-azure-adfs/deployment.png)

Il diagramma precedente Mostra la topologia di base consigliata per iniziare a distribuire l'infrastruttura di AD FS in Azure. Di seguito sono elencati i principi alla base dei vari componenti della topologia:

* **Server DC/ADFS**: se sono presenti meno di 1.000 utenti, è possibile installare semplicemente ad FS ruolo nei controller di dominio. Se non si desidera alcun effetto sulle prestazioni dei controller di dominio o se sono presenti più di 1.000 utenti, distribuire AD FS in server distinti.
* **Server WAP** : è necessario distribuire i server proxy applicazione Web, in modo che gli utenti possano raggiungere il ad FS anche quando non sono presenti nella rete aziendale.
* **DMZ**: i server proxy applicazione Web verranno inseriti nella rete perimetrale ed è consentito solo l'accesso TCP/443 tra la rete perimetrale e la subnet interna.
* **Bilanciamento del carico**: per garantire la disponibilità elevata dei server proxy applicazione Web e di ad FS, è consigliabile usare un servizio di bilanciamento del carico interno per ad FS server e Azure Load Balancer per i server proxy applicazione Web.
* **Set di disponibilità**: per garantire la ridondanza per la distribuzione di ad FS, è consigliabile raggruppare due o più macchine virtuali in un set di disponibilità per carichi di lavoro simili. Questa configurazione garantisce che, durante un evento di manutenzione pianificata o non pianificata, sarà disponibile almeno una macchina virtuale
* **Account di archiviazione**: è consigliabile avere due account di archiviazione. La presenza di un singolo account di archiviazione può causare la creazione di un singolo punto di errore e può causare la disattivazione della distribuzione in uno scenario improbabile in cui l'account di archiviazione diventa inattivo. Due account di archiviazione consentiranno di associare un account di archiviazione per ogni riga di errore.
* **Separazione di rete**: i server proxy applicazione Web devono essere distribuiti in una rete perimetrale separata. È possibile dividere una rete virtuale in due subnet e quindi distribuire i server proxy applicazione Web in una subnet isolata. È sufficiente configurare le impostazioni del gruppo di sicurezza di rete per ogni subnet e consentire solo la comunicazione richiesta tra le due subnet. Per uno scenario di distribuzione di seguito vengono forniti altri dettagli

## <a name="steps-to-deploy-ad-fs-in-azure"></a>Passaggi per la distribuzione di AD FS in Azure
I passaggi descritti in questa sezione descrivono la guida per distribuire il seguente AD FS infrastruttura in Azure.

### <a name="1-deploying-the-network"></a>1. distribuzione della rete
Come descritto in precedenza, è possibile creare due subnet in una singola rete virtuale oppure creare due reti virtuali completamente diverse (VNet). Questo articolo è incentrato sulla distribuzione di una singola rete virtuale e sulla relativa suddivisione in due subnet. Si tratta attualmente di un approccio più semplice perché due reti virtuali separate richiederebbero un VNet per il gateway VNet per le comunicazioni.

**1,1 creare una rete virtuale**

![Crea rete virtuale](./media/how-to-connect-fed-azure-adfs/deploynetwork1.png)

Nella portale di Azure selezionare rete virtuale ed è possibile distribuire immediatamente la rete virtuale e una subnet con un solo clic. La subnet INT è definita anche ed è ora pronta per l'aggiunta di macchine virtuali.
Il passaggio successivo consiste nell'aggiungere un'altra subnet alla rete, ad esempio la subnet DMZ. Per creare la subnet della rete perimetrale, è sufficiente

* Selezionare la rete appena creata
* In Proprietà selezionare subnet
* Nel pannello subnet fare clic sul pulsante Aggiungi
* Specificare il nome della subnet e le informazioni sullo spazio di indirizzi per creare la subnet

![Subnet](./media/how-to-connect-fed-azure-adfs/deploynetwork2.png)

![DMZ subnet](./media/how-to-connect-fed-azure-adfs/deploynetwork3.png)

**1,2. creazione dei gruppi di sicurezza di rete**

Un gruppo di sicurezza di rete (NSG) contiene un elenco di regole dell'elenco di controllo di accesso (ACL) che consentono o negano il traffico di rete alle istanze di VM in una rete virtuale. Gruppi può essere associato a subnet o singole istanze di macchine virtuali all'interno di tale subnet. Quando un NSG è associato a una subnet, le regole ACL si applicano a tutte le istanze di VM nella subnet.
Ai fini di questa guida, si creeranno due gruppi: uno per una rete interna e una rete perimetrale. Verranno etichettate rispettivamente NSG_INT e NSG_DMZ.

![Crea NSG](./media/how-to-connect-fed-azure-adfs/creatensg1.png)

Dopo la creazione di NSG, saranno presenti 0 regole in ingresso e 0 in uscita. Una volta che i ruoli nei rispettivi server sono installati e funzionanti, le regole in ingresso e in uscita possono essere effettuate in base al livello di sicurezza desiderato.

![Inizializzare NSG](./media/how-to-connect-fed-azure-adfs/nsgint1.png)

Una volta creati i gruppi, associare NSG_INT alla subnet INT e NSG_DMZ con subnet DMZ. Di seguito è riportata una schermata di esempio:

![Configurazione di NSG](./media/how-to-connect-fed-azure-adfs/nsgconfigure1.png)

* Fare clic su subnet per aprire il pannello per le subnet
* Selezionare la subnet da associare a NSG 

Dopo la configurazione, il pannello per le subnet dovrebbe essere simile al seguente:

![Subnet dopo NSG](./media/how-to-connect-fed-azure-adfs/nsgconfigure2.png)

**1,3. creare una connessione a un'istanza locale**

Per distribuire il controller di dominio in Azure, è necessaria una connessione all'ambiente locale. Azure offre diverse opzioni di connettività per connettere l'infrastruttura locale all'infrastruttura di Azure.

* Da punto a sito
* Da sito a sito di rete virtuale
* ExpressRoute

Si consiglia di usare ExpressRoute. ExpressRoute consente di creare connessioni private tra i Data Center di Azure e l'infrastruttura locale o in un ambiente con percorso condiviso. Le connessioni ExpressRoute non passano attraverso la rete Internet pubblica. Offrono maggiore affidabilità, velocità più elevate, latenze più basse e maggiore sicurezza rispetto alle connessioni Internet tradizionali.
Sebbene sia consigliabile usare ExpressRoute, è possibile scegliere qualsiasi metodo di connessione più adatto per l'organizzazione. Per altre informazioni su ExpressRoute e sulle varie opzioni di connettività con ExpressRoute, vedere [Panoramica tecnica di ExpressRoute](https://aka.ms/Azure/ExpressRoute).

### <a name="2-create-storage-accounts"></a>2. creare gli account di archiviazione
Per mantenere la disponibilità elevata ed evitare la dipendenza da un singolo account di archiviazione, è possibile creare due account di archiviazione. Dividere i computer in ogni set di disponibilità in due gruppi e quindi assegnare a ogni gruppo un account di archiviazione separato.

![Creare gli account di archiviazione](./media/how-to-connect-fed-azure-adfs/storageaccount1.png)

### <a name="3-create-availability-sets"></a>3. creare i set di disponibilità
Per ogni ruolo (DC/AD FS e WAP), creare i set di disponibilità che conterranno almeno 2 computer ciascuno. Questo consente di ottenere una maggiore disponibilità per ogni ruolo. Durante la creazione dei set di disponibilità, è fondamentale decidere quanto segue:

* **Domini di errore**: le macchine virtuali nello stesso dominio di errore condividono le stesse opzioni di alimentazione e di rete fisica. Sono consigliati almeno 2 domini di errore. Il valore predefinito è 3 e può essere lasciato invariato ai fini della distribuzione
* **Domini di aggiornamento**: i computer appartenenti allo stesso dominio di aggiornamento vengono riavviati insieme durante un aggiornamento. Si vuole avere almeno 2 domini di aggiornamento. Il valore predefinito è 5 e può essere lasciato invariato ai fini della distribuzione

![Set di disponibilità](./media/how-to-connect-fed-azure-adfs/availabilityset1.png)

Creare i set di disponibilità seguenti

| Set di disponibilità | Role | Domini di errore | Domini di aggiornamento |
|:---:|:---:|:---:|:--- |
| contosodcset |DC/ADFS |3 |5 |
| contosowapset |WAP |3 |5 |

### <a name="4-deploy-virtual-machines"></a>4. distribuire macchine virtuali
Il passaggio successivo consiste nel distribuire le macchine virtuali che ospiteranno i diversi ruoli nell'infrastruttura. In ogni set di disponibilità sono consigliate almeno due macchine. Creare quattro macchine virtuali per la distribuzione di base.

| Computer | Role | Subnet | Set di disponibilità | Account di archiviazione | Indirizzo IP |
|:---:|:---:|:---:|:---:|:---:|:---:|
| ContosoDC1 |DC/ADFS |INT |contosodcset |contososac1 |Static |
| contosodc2 |DC/ADFS |INT |contosodcset |contososac2 |Static |
| contosowap1 |WAP |DMZ |contosowapset |contososac1 |Static |
| contosowap2 |WAP |DMZ |contosowapset |contososac2 |Static |

Come si può notare, non è stato specificato alcun NSG. Questo perché Azure consente di usare NSG a livello di subnet. Quindi, è possibile controllare il traffico di rete del computer usando i singoli NSG associati alla subnet o altrimenti l'oggetto NIC. Per altre informazioni, vedere [che cos'è un gruppo di sicurezza di rete (NSG)](https://aka.ms/Azure/NSG).
Se si gestisce il DNS, è consigliabile usare l'indirizzo IP statico. È possibile usare DNS di Azure e invece nei record DNS per il dominio, fare riferimento ai nuovi computer in base ai nomi di dominio completi di Azure.
Al termine della distribuzione, il riquadro della macchina virtuale avrà un aspetto simile al seguente:

![Macchine virtuali distribuite](./media/how-to-connect-fed-azure-adfs/virtualmachinesdeployed_noadfs.png)

### <a name="5-configuring-the-domain-controller--ad-fs-servers"></a>5. configurazione del controller di dominio/server AD FS
 Per autenticare una richiesta in ingresso, AD FS necessario contattare il controller di dominio. Per salvare il costo del viaggio da Azure al controller di dominio locale per l'autenticazione, è consigliabile distribuire una replica del controller di dominio in Azure. Per ottenere la disponibilità elevata, è consigliabile creare un set di disponibilità di almeno due controller di dominio.

| Controller di dominio | Role | Account di archiviazione |
|:---:|:---:|:---:|
| ContosoDC1 |Replica |contososac1 |
| contosodc2 |Replica |contososac2 |

* Innalzare di livello i due server come controller di dominio di replica con DNS
* Configurare i server di AD FS installando il ruolo AD FS tramite Server Manager.

### <a name="6-deploying-internal-load-balancer-ilb"></a>6. distribuzione di Load Balancer interni (ILB)
**6,1. creare il ILB**

Per distribuire un ILB, selezionare servizio di bilanciamento del carico nella portale di Azure e fare clic su Aggiungi (+).

> [!NOTE]
> Se nel menu non sono visualizzati i **bilanciamenti del carico** , fare clic su **Sfoglia** in basso a sinistra nel portale e scorrere fino a visualizzare i **bilanciamenti del carico**.  Fare quindi clic sulla stella gialla per aggiungerla al menu. Selezionare ora la nuova icona del servizio di bilanciamento del carico per aprire il pannello per avviare la configurazione del servizio di bilanciamento del carico.
> 
> 

![Sfoglia il servizio di bilanciamento del carico](./media/how-to-connect-fed-azure-adfs/browseloadbalancer.png)

* **Nome**: assegnare un nome appropriato al servizio di bilanciamento del carico
* **Schema**: poiché questo servizio di bilanciamento del carico verrà posizionato davanti ai server ad FS ed è destinato solo a connessioni di rete interne, selezionare "interno"
* **Rete virtuale**: scegliere la rete virtuale in cui si distribuisce la ad FS
* **Subnet**: scegliere la subnet interna qui
* **Assegnazione di indirizzi IP**: statica

![Servizio di bilanciamento del carico interno](./media/how-to-connect-fed-azure-adfs/ilbdeployment1.png)

Dopo aver fatto clic su Crea e ILB viene distribuito, dovrebbe essere visualizzato nell'elenco dei bilanciamenti del carico:

![Bilanciamento del carico dopo ILB](./media/how-to-connect-fed-azure-adfs/ilbdeployment2.png)

Il passaggio successivo consiste nel configurare il pool back-end e il probe back-end.

**6,2. configurare il pool back-end ILB**

Selezionare il ILB appena creato nel pannello del servizio di bilanciamento del carico. Verrà aperto il pannello impostazioni. 

1. Selezionare pool back-end nel pannello impostazioni
2. Nel pannello Aggiungi pool back-end fare clic su Aggiungi macchina virtuale
3. Verrà visualizzato un pannello in cui è possibile scegliere un set di disponibilità
4. Scegliere il set di disponibilità AD FS

![Configurare il pool back-end ILB](./media/how-to-connect-fed-azure-adfs/ilbdeployment3.png)

**6,3. configurazione del probe**

Nel pannello impostazioni ILB selezionare Probe di integrità.

1. Fare clic su Aggiungi
2. Specificare i dettagli per il probe a. **Nome**: nome Probe b. **Protocollo**: http c. **Porta**: 80 (http) d. **Percorso**:/ADFS/Probe e. **Intervallo**: 5 (valore predefinito): si tratta dell'intervallo con cui ILB esegue il probe dei computer nel pool back-end f. **Limite di soglia non integro**: 2 (valore predefinito): questa è la soglia di errori di probe consecutivi dopo i quali ILB dichiara una macchina nel pool back-end che non risponde e interrompe l'invio del traffico.

![Configurare il probe ILB](./media/how-to-connect-fed-azure-adfs/ilbdeployment4.png)

Si sta usando l'endpoint/ADFS/Probe creato esplicitamente per i controlli di integrità in un ambiente AD FS in cui non è possibile eseguire un controllo completo del percorso HTTPS.  Si tratta di una situazione sostanzialmente migliore rispetto a una verifica della porta di base 443, che non riflette accuratamente lo stato di una distribuzione di AD FS moderna.  Altre informazioni su questo problema sono disponibili in https://blogs.technet.microsoft.com/applicationproxyblog/2014/10/17/hardware-load-balancer-health-checks-and-web-application-proxy-ad-fs-2012-r2/.

**6,4. creare regole di bilanciamento del carico**

Per bilanciare efficacemente il traffico, il ILB deve essere configurato con regole di bilanciamento del carico. Per creare una regola di bilanciamento del carico, 

1. Selezionare regola di bilanciamento del carico nel pannello impostazioni di ILB
2. Fare clic su Aggiungi nel pannello regola di bilanciamento del carico
3. Nel pannello Aggiungi regola di bilanciamento del carico a. **Nome**: specificare un nome per la regola b. **Protocollo**: selezionare TCP c. **Porta**: 443 d. **Porta back-end**: 443 e. **Pool back-end**: selezionare il pool creato per il cluster ad FS precedente f. **Probe**: selezionare in precedenza il probe creato per i server ad FS

![Configurare le regole di bilanciamento del carico di ILB](./media/how-to-connect-fed-azure-adfs/ilbdeployment5.png)

**6,5. aggiornare DNS con ILB**

Passare al server DNS e creare un record CNAME per ILB. Il record CNAME deve essere per il servizio federativo con l'indirizzo IP che punta all'indirizzo IP del ILB. Se ad esempio l'indirizzo DIP ILB è 10.3.0.8 e il servizio federativo installato è fs.contoso.com, creare un record CNAME per fs.contoso.com che punta a 10.3.0.8.
In questo modo, tutte le comunicazioni relative a fs.contoso.com finiranno in ILB e vengono indirizzate in modo appropriato.

### <a name="7-configuring-the-web-application-proxy-server"></a>7. configurazione del server proxy applicazione Web
**7,1. configurazione dei server proxy applicazione Web per raggiungere i server AD FS**

Per assicurarsi che i server proxy applicazione Web siano in grado di raggiungere i server AD FS dietro la ILB, creare un record in%systemroot%\system32\drivers\etc\hosts per ILB. Si noti che il nome distinto (DN) deve corrispondere al nome del servizio federativo, ad esempio fs.contoso.com. La voce IP deve essere quella dell'indirizzo IP di ILB (10.3.0.8 come nell'esempio).

**7,2. installazione del ruolo Proxy applicazione Web**

Dopo aver assicurato che i server proxy applicazione Web siano in grado di raggiungere i server AD FS dietro ILB, è possibile installare i server proxy applicazione Web. I server proxy applicazione Web non devono essere aggiunti al dominio. Installare i ruoli proxy applicazione Web nei due server proxy applicazione Web selezionando il ruolo accesso remoto. Server Manager guiderà l'utente per completare l'installazione WAP.
Per altre informazioni su come distribuire WAP, vedere [installare e configurare il server proxy applicazione Web](https://technet.microsoft.com/library/dn383662.aspx).

### <a name="8--deploying-the-internet-facing-public-load-balancer"></a>8. distribuzione del Load Balancer (Public) con connessione Internet
**8,1. creazione Load Balancer con connessione Internet (pubblico)**

Nella portale di Azure selezionare bilanciamenti del carico e quindi fare clic su Aggiungi. Nel pannello Crea servizio di bilanciamento del carico immettere le informazioni seguenti

1. **Nome**: nome del servizio di bilanciamento del carico
2. **Schema**: public: questa opzione indica ad Azure che questo servizio di bilanciamento del carico dovrà avere un indirizzo pubblico.
3. **Indirizzo IP**: creare un nuovo indirizzo IP (dinamico)

![Bilanciamento del carico con connessione Internet](./media/how-to-connect-fed-azure-adfs/elbdeployment1.png)

Dopo la distribuzione, il servizio di bilanciamento del carico verrà visualizzato nell'elenco dei bilanciamenti del carico.

![Elenco del servizio di bilanciamento del carico](./media/how-to-connect-fed-azure-adfs/elbdeployment2.png)

**8,2. assegnare un'etichetta DNS all'indirizzo IP pubblico**

Fare clic sulla voce del servizio di bilanciamento del carico appena creato nel pannello dei bilanciamenti del carico per visualizzare il pannello per la configurazione. Seguire questa procedura per configurare l'etichetta DNS per l'indirizzo IP pubblico:

1. Fare clic sull'indirizzo IP pubblico. Verrà aperto il pannello per l'indirizzo IP pubblico e le relative impostazioni
2. Fare clic su configurazione
3. Specificare un'etichetta DNS. Questa diventerà l'etichetta DNS pubblica a cui è possibile accedere da qualsiasi posizione, ad esempio contosofs.westus.cloudapp.azure.com. È possibile aggiungere una voce nel DNS esterno per il servizio federativo (ad esempio fs.contoso.com) che viene risolto nell'etichetta DNS del servizio di bilanciamento del carico esterno (contosofs.westus.cloudapp.azure.com).

![Configurare il servizio di bilanciamento del carico con connessione Internet](./media/how-to-connect-fed-azure-adfs/elbdeployment3.png) 

![Configurare il servizio di bilanciamento del carico con connessione Internet (DNS)](./media/how-to-connect-fed-azure-adfs/elbdeployment4.png)

**8,3. configurare il pool back-end per Internet (Public) Load Balancer** 

Seguire la stessa procedura descritta in creazione del servizio di bilanciamento del carico interno per configurare il pool back-end per Internet (Public) Load Balancer come set di disponibilità per i server WAP. Ad esempio, contosowapset.

![Configurare il pool back-end di Load Balancer con connessione Internet](./media/how-to-connect-fed-azure-adfs/elbdeployment5.png)

**8,4. configurare il probe**

Seguire la stessa procedura descritta in configurazione del servizio di bilanciamento del carico interno per configurare il probe per il pool back-end dei server WAP.

![Configurare il probe di Load Balancer con connessione Internet](./media/how-to-connect-fed-azure-adfs/elbdeployment6.png)

**8,5. creare regole di bilanciamento del carico**

Seguire la stessa procedura descritta in ILB per configurare la regola di bilanciamento del carico per TCP 443.

![Configurare le regole di bilanciamento del Load Balancer con connessione Internet](./media/how-to-connect-fed-azure-adfs/elbdeployment7.png)

### <a name="9-securing-the-network"></a>9. protezione della rete
**9,1. protezione della subnet interna**

In generale, sono necessarie le regole seguenti per proteggere in modo efficiente la subnet interna (nell'ordine indicato di seguito)

| Regola | Descrizione | Flusso |
|:--- |:--- |:---:|
| AllowHTTPSFromDMZ |Consenti la comunicazione HTTPS dalla rete perimetrale |Inserimento in |
| DenyInternetOutbound |Nessun accesso a Internet |Inserimento in |

![Regole di accesso INT (in ingresso)](./media/how-to-connect-fed-azure-adfs/nsg_int.png)

**9,2. protezione della subnet della rete perimetrale**

| Regola | Descrizione | Flusso |
|:--- |:--- |:---:|
| AllowHTTPSFromInternet |Consenti HTTPS da Internet alla rete perimetrale |Inserimento in |
| DenyInternetOutbound |Tutto tranne HTTPS per Internet è bloccato |Inserimento in |

![Regole di accesso esterno (in ingresso)](./media/how-to-connect-fed-azure-adfs/nsg_dmz.png)

> [!NOTE]
> Se è richiesta l'autenticazione del certificato utente client (autenticazione clientTLS con certificati utente X509), AD FS richiede che la porta TCP 49443 sia abilitata per l'accesso in ingresso.
> 
> 

### <a name="10-test-the-ad-fs-sign-in"></a>10. testare l'accesso AD FS
Il modo più semplice consiste nel testare AD FS consiste nell'usare la pagina IdpInitiatedSignon. aspx. Per poter eseguire questa operazione, è necessario abilitare IdpInitiatedSignOn nelle proprietà AD FS. Attenersi alla procedura seguente per verificare la configurazione del AD FS

1. Eseguire il cmdlet seguente nel server AD FS, usando PowerShell, per impostarlo su abilitato.
   Set-AdfsProperties-EnableIdPInitiatedSignonPage $true 
2. Da qualsiasi computer esterno, accedere a https:\//adfs-server.contoso.com/adfs/ls/IdpInitiatedSignon.aspx.  
3. Dovrebbe essere visualizzata la pagina AD FS come riportato di seguito:

![Pagina di accesso per il test](./media/how-to-connect-fed-azure-adfs/test1.png)

Al termine dell'accesso, verrà visualizzato un messaggio di operazione completata, come illustrato di seguito:

![Test riuscito](./media/how-to-connect-fed-azure-adfs/test2.png)

## <a name="template-for-deploying-ad-fs-in-azure"></a>Modello per la distribuzione di AD FS in Azure
Il modello distribuisce una configurazione di 6 computer, 2 per i controller di dominio AD FS e WAP.

[AD FS nel modello di distribuzione di Azure](https://github.com/paulomarquesc/adfs-6vms-regular-template-based)

È possibile usare una rete virtuale esistente o creare una nuova VNET durante la distribuzione di questo modello. I vari parametri disponibili per la personalizzazione della distribuzione sono elencati di seguito con la descrizione dell'utilizzo del parametro nel processo di distribuzione. 

| Parametro | Descrizione |
|:--- |:--- |
| Location |Area in cui distribuire le risorse, ad esempio Stati Uniti orientali. |
| StorageAccountType |Tipo di account di archiviazione creato |
| VirtualNetworkUsage |Indica se verrà creata una nuova rete virtuale o se ne userà una esistente |
| VirtualNetworkName |Nome della rete virtuale da creare, obbligatoria per l'utilizzo di reti virtuali nuove o esistenti |
| VirtualNetworkResourceGroupName |Specifica il nome del gruppo di risorse in cui risiede la rete virtuale esistente. Quando si usa una rete virtuale esistente, questo diventa un parametro obbligatorio, in modo che la distribuzione possa trovare l'ID della rete virtuale esistente |
| VirtualNetworkAddressRange |Intervallo di indirizzi del nuovo VNET, obbligatorio se si crea una nuova rete virtuale |
| InternalSubnetName |Nome della subnet interna obbligatoria per entrambe le opzioni di utilizzo della rete virtuale (nuova o esistente) |
| InternalSubnetAddressRange |L'intervallo di indirizzi della subnet interna, che contiene i controller di dominio e i server ADFS, è obbligatorio se si crea una nuova rete virtuale. |
| DMZSubnetAddressRange |L'intervallo di indirizzi della subnet DMZ, che contiene i server proxy applicazione Windows, obbligatorio se si crea una nuova rete virtuale. |
| DMZSubnetName |Nome della subnet interna obbligatoria per entrambe le opzioni di utilizzo della rete virtuale (nuova o esistente). |
| ADDC01NICIPAddress |L'indirizzo IP interno del primo controller di dominio, questo indirizzo IP verrà assegnato in modo statico al controller di dominio e deve essere un indirizzo IP valido nella subnet interna |
| ADDC02NICIPAddress |L'indirizzo IP interno del secondo controller di dominio, questo indirizzo IP verrà assegnato in modo statico al controller di dominio e deve essere un indirizzo IP valido nella subnet interna |
| ADFS01NICIPAddress |L'indirizzo IP interno del primo server ADFS, questo indirizzo IP verrà assegnato in modo statico al server ADFS e deve essere un indirizzo IP valido nella subnet interna |
| ADFS02NICIPAddress |L'indirizzo IP interno del secondo server ADFS, questo indirizzo IP verrà assegnato in modo statico al server ADFS e deve essere un indirizzo IP valido nella subnet interna |
| WAP01NICIPAddress |L'indirizzo IP interno del primo server WAP, questo indirizzo IP verrà assegnato in modo statico al server WAP e deve essere un indirizzo IP valido nella subnet della rete perimetrale |
| WAP02NICIPAddress |L'indirizzo IP interno del secondo server WAP, questo indirizzo IP verrà assegnato in modo statico al server WAP e deve essere un indirizzo IP valido nella subnet della rete perimetrale |
| ADFSLoadBalancerPrivateIPAddress |L'indirizzo IP interno del servizio di bilanciamento del carico di ADFS, questo indirizzo IP verrà assegnato in modo statico al servizio di bilanciamento del carico e deve essere un indirizzo IP valido nella subnet interna |
| ADDCVMNamePrefix |Prefisso del nome della macchina virtuale per i controller di dominio |
| ADFSVMNamePrefix |Prefisso del nome della macchina virtuale per i server ADFS |
| WAPVMNamePrefix |Prefisso del nome della macchina virtuale per i server WAP |
| ADDCVMSize |Dimensioni della macchina virtuale dei controller di dominio |
| ADFSVMSize |Dimensioni della VM dei server ADFS |
| WAPVMSize |Dimensioni della VM dei server WAP |
| Con valori AdminUsername |Nome dell'amministratore locale delle macchine virtuali |
| AdminPassword |Password per l'account amministratore locale delle macchine virtuali |

## <a name="additional-resources"></a>Risorse aggiuntive
* [Set di disponibilità](https://aka.ms/Azure/Availability) 
* [Azure Load Balancer](https://aka.ms/Azure/ILB)
* [Load Balancer interno](https://aka.ms/Azure/ILB/Internal)
* [Load Balancer con connessione Internet](https://aka.ms/Azure/ILB/Internet)
* [Account di archiviazione](https://aka.ms/Azure/Storage)
* [Reti virtuali di Azure](https://aka.ms/Azure/VNet)
* [Collegamenti a AD FS e proxy applicazione Web](https://aka.ms/ADFSLinks) 

## <a name="next-steps"></a>Passaggi successivi
* [Integrazione delle identità locali con Azure Active Directory](https://docs.microsoft.com/azure/active-directory/hybrid/whatis-hybrid-identity)
* [Configurazione e gestione dei AD FS tramite Azure AD Connect](/azure/active-directory/hybrid/how-to-connect-fed-whatis)
* [Distribuzione di AD FS di disponibilità elevata tra aree geografiche in Azure con gestione traffico di Azure](active-directory-adfs-in-azure-with-azure-traffic-manager.md)
