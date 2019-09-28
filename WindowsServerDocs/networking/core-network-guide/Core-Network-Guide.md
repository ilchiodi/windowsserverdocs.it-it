---
title: Componenti di una rete core
description: In questa guida vengono fornite istruzioni su come pianificare e distribuire i componenti di base necessari per una rete completamente funzionante e un nuovo dominio di Active Directory in una nuova foresta con Windows Server 2016
manager: brianlic
ms.prod: windows-server
ms.technology: networking
ms.topic: article
ms.assetid: b3cd60f7-d380-4712-9a78-0a8f551e1121
ms.author: pashort
author: shortpatti
ms.openlocfilehash: cae789974c3f2b4f83c9120558d77dbe27f4190a
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71356407"
---
# <a name="core-network-components"></a>Componenti di una rete core

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016

In questa guida vengono fornite istruzioni su come pianificare e distribuire i componenti di base necessari per una rete completamente funzionante e un nuovo dominio di Active Directory in una nuova foresta.

> [!NOTE]
> In questa guida è disponibile per il download in formato Microsoft Word dalla TechNet Gallery. Per ulteriori informazioni, vedere [Guida alla rete Core per Windows Server 2016](https://gallery.technet.microsoft.com/Core-Network-Guide-for-9da2e683).

Questa guida contiene le sezioni seguenti.

- [Informazioni sulla guida](#BKMK_about)

- [Panoramica della rete core](#BKMK_overview)

- [Pianificazione della rete core](#BKMK_planning)

- [Distribuzione di rete core](#BKMK_deployment)

- [Risorse tecniche aggiuntive](#BKMK_resources)

- [Appendici da A A E](#BKMK_appendix)

## <a name="BKMK_about"></a>Informazioni su questa guida
Questa guida è destinata agli amministratori di rete e di sistema che devono installare una nuova rete o che desiderano creare una rete basata su domini in sostituzione di una rete costituita da gruppi di lavoro. Lo scenario di distribuzione illustrato in questa guida è particolarmente utile se si prevede di dover aggiungere alla rete ulteriori funzionalità e servizi in futuro.

È consigliabile consultare le guide di progettazione e distribuzione per ognuna delle tecnologie utilizzate in questo scenario di distribuzione, al fine di verificare che questa guida includa tutti i servizi e le impostazioni di configurazione necessari.

Una rete core è un insieme di componenti hardware, dispositivi e componenti software di rete che forniscono i servizi essenziali per i requisiti IT dell'organizzazione.

Una rete core basata su Windows Server offre numerosi vantaggi, inclusi i seguenti.

-   Protocolli di base per la connettività di rete tra computer e altri dispositivi compatibili con TCP/IP (Transmission Control Protocol/Internet Protocol), una suite di protocolli standard per la connessione dei computer e la creazione di reti. TCP/IP è il software dei protocolli di rete fornito con i sistemi operativi Microsoft Windows che implementano e supportano la suite di protocolli TCP/IP.

-   Assegnazione automatica degli indirizzi IP con DHCP (Dynamic Host Configuration Protocol) ai computer e agli altri dispositivi configurati come client DHCP. La configurazione manuale degli indirizzi IP in tutti i computer di una rete richiede molto tempo ed è meno flessibile rispetto all'assegnazione dinamica delle configurazioni degli indirizzi IP ai computer e agli altri dispositivi tramite un server DHCP.

-   Servizi di risoluzione dei nomi DNS (Domain Name System). Il sistema DNS consente a utenti, computer, applicazioni e servizi di trovare gli indirizzi IP di computer e dispositivi della rete, utilizzando il nome di dominio completo del computer o dispositivo.

-   Una foresta, ovvero un contenitore che include uno o più domini di Active Directory che condividono le stesse definizioni di classi e attributi (schema), le informazioni relative ai siti e alla replica (configurazione) e funzionalità di ricerca a livello di foresta (catalogo globale).

-   Un dominio radice della foresta, ovvero il primo dominio creato in una nuova foresta. I gruppi Enterprise Admins e Schema Admins, che sono gruppi amministrativi a livello di foresta, si trovano nel dominio radice della foresta. Come tutti gli altri domini, un dominio radice della foresta è un insieme di oggetti computer, utente e gruppo che vengono definiti dall'amministratore in Servizi di dominio Active Directory. Tali oggetti condividono un database di directory e criteri di sicurezza comuni. Possono inoltre condividere relazioni di sicurezza con gli altri domini eventualmente aggiunti a mano a mano che l'organizzazione si espande. Il servizio directory gestisce inoltre l'archiviazione dei dati di directory e consente l'accesso a tali dati da parte di computer, applicazioni e utenti autorizzati.

-   Un database di account di utenti e computer. Il servizio directory fornisce un database centralizzato di account utente che consente di creare gli account per utenti e computer autorizzati a connettersi alla rete e ad accedere alle relative risorse, quali applicazioni, database, stampanti e cartelle e file condivisi.

Una rete core può essere inoltre ridimensionata a mano a mano che l'organizzazione cresce e i requisiti IT cambiano. Ad esempio, con una rete core è possibile aggiungere domini, subnet IP, servizi di accesso remoto, servizi wireless e altre funzionalità e ruoli server offerti da Windows Server 2016.

### <a name="network-hardware-requirements"></a>Requisiti hardware della rete
Per impostare in modo corretto una rete core, è necessario distribuire alcuni componenti hardware di rete, ad esempio:

-   Cavi Ethernet, Fast Ethernet o Gigabyte Ethernet.

-   Un hub, un commutatore livello 2 o 3, un router o un altro dispositivo in grado di inoltrare il traffico di rete tra computer e dispositivi.

-   Computer che soddisfino i requisiti hardware minimi per i rispettivi sistemi operativi client e server.

## <a name="what-this-guide-does-not-provide"></a>Informazioni non contenute in questa guida
Questa guida non contiene istruzioni per la distribuzione dei componenti seguenti:

-   Hardware di rete, ad esempio cavi, router, commutatori e hub

-   Risorse di rete aggiuntive, ad esempio stampanti e file server

-   Connettività Internet

-   Accesso remoto

-   Accesso wireless

-   Distribuzione computer client

> [!NOTE]
> I computer che eseguono sistemi operativi client Windows configurati per impostazione predefinita per la ricezione di lease di indirizzi IP dal server DHCP. Non è pertanto necessario eseguire ulteriori operazioni di configurazione DHCP o IPv4 (Protocollo IP versione 4) sui computer client.

## <a name="technology-overviews"></a>Panoramiche sulle tecnologie
Nelle sezioni seguenti vengono fornite brevi descrizioni generali delle tecnologie che è necessario distribuire per creare una rete core.

### <a name="active-directory-domain-services"></a>Servizi di dominio di Active Directory
Una directory è una struttura gerarchica in cui vengono archiviate informazioni sugli oggetti della rete, ad esempio utenti e computer. Un servizio di directory, ad esempio servizi di dominio Active Directory, fornisce i metodi per l'archiviazione dei dati di directory e renderli disponibili per gli amministratori e gli utenti della rete. Ad esempio, di dominio Active Directory archivia le informazioni sugli account utente, inclusi nomi, indirizzi di posta elettronica, password e numeri di telefono e consente ad altri utenti autorizzati nella stessa rete accedere a queste informazioni.

### <a name="dns"></a>DNS
DNS è un protocollo di risoluzione dei nomi per le reti TCP/IP, ad esempio Internet o la rete di un'organizzazione. Un server DNS ospita informazioni che consentono a computer client e servizi di risolvere i nomi DNS alfanumerici facilmente riconoscibili convertendoli negli indirizzi IP utilizzati dai computer per comunicare tra loro.

### <a name="dhcp"></a>DHCP
DHCP è uno standard IP che semplifica la gestione della configurazione IP degli host. Lo standard DHCP prevede l'utilizzo di server DHCP nella gestione dell'allocazione dinamica degli indirizzi IP e in altri dettagli correlati alla configurazione dei client della rete abilitati per DHCP.

Lo standard DHCP consente di assegnare dinamicamente un indirizzo IP a un computer o un altro dispositivo della rete locale, ad esempio una stampante, tramite un server DHCP. Ogni computer di una rete TCP/IP deve avere un indirizzo IP univoco, poiché questo indirizzo IP e la subnet mask correlata identificano sia il computer host, sia la subnet alla quale il computer è collegato. Utilizzando DHCP si assicura che tutti i computer configurati come client DHCP ricevano un indirizzo IP idoneo per il percorso di rete e la subnet. Inoltre utilizzando le opzioni DHCP, ad esempio gateway e server DNS predefiniti, è possibile fornire automaticamente ai client DHCP le informazioni necessarie per il loro corretto funzionamento nella rete.

Per le reti basate su TCP/IP, il protocollo DHCP riduce la complessità e la quantità delle attività amministrative necessarie per la riconfigurazione dei computer.

### <a name="tcpip"></a>TCP/IP
TCP/IP in Windows Server 2016 è il seguente:

-   Software di rete basato su protocolli di rete standard.

-   Un protocollo instradabile per reti aziendali che supporta la connessione di un computer basato su Windows sia alla rete locale (LAN), sia ad ambienti di rete WAN (Wide Area Network).

-   Utilità e tecnologie di base per la connessione di computer basati su Windows a sistemi di tipo diverso al fine di condividere informazioni.

-   Funzionalità di base per l'accesso a servizi Internet globali, ad esempio al World Wide Web e ai server FTP (File Transfer Protocol).

-   Un framework client/server multipiattaforma affidabile e scalabile.

Il servizio TCP/IP include utilità TCP/IP di base che consentono ai computer basati su Windows di connettersi e condividere informazioni con altri sistemi Microsoft e non Microsoft, quali:

-    Windows Server 2016

-   Windows 10

-    Windows Server 2012 R2

-   Windows 8.1

-    Windows Server 2012

-   Windows 8

-    Windows Server 2008 R2

-    Windows 7

-    Windows Server 2008

-   Windows Vista

-   Host Internet

-   Sistemi Apple Macintosh

-   Mainframe IBM

-   Sistemi UNIX e Linux

-   Sistemi Open VMS

-   Stampanti di rete

-   Tablet e telefoni cellulari con Ethernet cablate e senza fili 802.11 abilitato

## <a name="BKMK_overview"></a>Panoramica della rete core
Nella figura seguente è illustrata la topologia di una rete core basata su Windows Server.

![Topologia di rete di Windows Server Core](../media/Core-Network-Guide/cng16_overview.jpg)

> [!NOTE]
> Questa guida contiene inoltre informazioni sull'aggiunta di Server dei criteri di rete e Server Web (IIS) alla topologia della rete per gettare le basi per soluzioni sicure di accesso alla rete, ad esempio distribuzioni 802.1X tramite rete cablata e wireless che possono essere implementate utilizzando le Guide complementari alla rete core. Per ulteriori informazioni, vedere [distribuzione di funzionalità facoltative per l'autenticazione di accesso di rete e servizi Web](#BKMK_optionalfeatures).

### <a name="core-network-components"></a>Componenti di una rete core
I componenti di una rete core sono illustrati di seguito.

##### <a name="router"></a>Router
In questa guida per la distribuzione sono disponibili istruzioni per l'implementazione di una rete core con due subnet separate da un router in cui è abilitato l'inoltro DHCP. È tuttavia possibile implementare un commutatore livello 2 o 3 oppure un hub, a seconda dei requisiti e delle risorse dell'organizzazione. Se si implementa un commutatore, è necessario sceglierne uno che supporti l'inoltro DHCP oppure inserire un server DHCP in ogni subnet. Se si implementa un hub, significa che si sta distribuendo una sola subnet e non è necessario l'inoltro DHCP o un secondo ambito nel server DHCP.

##### <a name="static-tcpip-configurations"></a>Configurazioni TCP/IP statiche
I server nella distribuzione illustrata sono configurati con indirizzi IPv4 statici. I computer client sono configurati per impostazione predefinita in modo da ricevere lease di indirizzi IP dal server DHCP.

##### <a name="active-directory-domain-services-global-catalog-and-dns-server-dc1"></a>Server DC1 catalogo globale di Servizi di dominio Active Directory e DNS
Entrambi i servizi di dominio Active Directory (AD DS) e del sistema DNS (Domain Name) installate nel server denominato DC1, che consente di directory e servizi di risoluzione per tutti i computer e dispositivi nella rete.

##### <a name="dhcp-server-dhcp1"></a>Server DHCP1
Il server DHCP, denominato DHCP1, è configurato con un ambito che fornisce lease di indirizzi IP (Internet Protocol) ai computer della subnet locale. Nel server DHCP possono essere configurati anche ambiti aggiuntivi per fornire lease di indirizzi IP ai computer delle altre subnet, se nei router è configurato l'inoltro DHCP.

##### <a name="client-computers"></a>Computer client
I computer che eseguono sistemi operativi client Windows configurati per impostazione predefinita come client DHCP, che ottiene gli indirizzi IP e le opzioni DHCP automaticamente dal server DHCP.

## <a name="BKMK_planning"></a>Pianificazione della rete core
Prima di distribuire una rete core, è necessario pianificare gli elementi seguenti.

-   [Pianificazione delle subnet](#bkmk_NetFndtn_Pln_Subnt)

-   [Pianificazione della configurazione di base di tutti i server](#bkmk_NetFndtn_Pln_AllSrvrs)

-   [Pianificazione della distribuzione di DC1](#bkmk_NetFndtn_Pln_AD-DNS-01)

-   [Pianificazione dell'accesso al dominio](#bkmk_NetFndtn_Pln_DomAccess)

-   [Pianificazione della distribuzione di DHCP1](#bkmk_NetFndtn_Pln_DHCP-01)

Nelle sezioni successive sono disponibili ulteriori dettagli su ognuno di questi elementi.

> [!NOTE]
> Per ulteriori informazioni sulla pianificazione della distribuzione, vedere anche [Appendice E - foglio di preparazione alla pianificazione della rete Core](#BKMK_E).

### <a name="bkmk_NetFndtn_Pln_Subnt"></a>Pianificazione delle subnet
Nelle reti TCP/IP (Transmission Control Protocol/Internet Protocol) vengono utilizzati i router per interconnettere componenti hardware e software che si trovano in segmenti diversi della rete fisica, o subnet. I router consentono inoltre di inoltrare i pacchetti IP tra le varie subnet. Prima di proseguire con l'attuazione delle istruzioni contenute in questa guida, è necessario definire il layout fisico della rete, incluso il numero di router e di subnet che serviranno.

Per configurare i server della rete con indirizzi IP statici, è inoltre necessario determinare l'intervallo di indirizzi IP che si desidera utilizzare per la subnet in cui risiedono i server della rete core. In questa Guida, l'indirizzo IP privato intervalli 10.0.0.1 - 10.0.0.254 e 10.0.1.1 - 10.0.1.254 vengono utilizzati come esempi, ma è possibile utilizzare qualsiasi intervallo di indirizzi IP privati che si preferisce.

> [!IMPORTANT]
> Dopo avere selezionato gli intervalli di indirizzi IP che si desidera utilizzare per ogni subnet, accertarsi che il router sia configurato con un indirizzo IP compreso nello stesso intervallo di indirizzi IP utilizzati dalla subnet in cui è installato il router. Se ad esempio il router è configurato per impostazione predefinita con un indirizzo IP 192.168.1.1, ma viene installato in una subnet con un intervallo di indirizzi IP 10.0.0.0/24, è necessario riconfigurare il router in modo che utilizzi un indirizzo IP compreso nell'intervallo 10.0.0.0/24.

Nella RFC (Request for Comments) Internet 1918 sono specificati i seguenti intervalli di indirizzi IP privati riconosciuti:

-   10.0.0.0-10.255.255.255

-   172.16.0.0-172.31.255.255

-   192.168.0.0-192.168.255.255

Quando si utilizzano gli intervalli di indirizzi IP privati specificati nella RFC 1918, non è possibile connettersi direttamente a Internet tramite un indirizzo IP privato perché le richieste in entrata e in uscita da tali indirizzi vengono automaticamente scartate dai router dei provider di servizi Internet (ISP). Per aggiungere in seguito connettività Internet alla rete core, sarà necessario sottoscrivere un contratto con un ISP per ottenere un indirizzo IP pubblico.

> [!IMPORTANT]
> Quando si utilizzano gli indirizzi IP privati, è necessario utilizzare un server proxy o NAT (Network Address Translation) per convertire gli intervalli di indirizzi IP privati della rete locale in un indirizzo IP pubblico che è possibile instradare in Internet. La maggior parte dei router supporta i servizi NAT, pertanto dovrebbe essere facile selezionare un router con funzionalità NAT.

Per ulteriori informazioni, vedere [Pianificazione della distribuzione di DHCP-01](#bkmk_NetFndtn_Pln_DHCP-01).

### <a name="bkmk_NetFndtn_Pln_AllSrvrs"></a>Pianificazione della configurazione di base di tutti i server
Per ogni server nella rete core è necessario ridenominare il computer e assegnare e configurare un indirizzo IPv4 statico e altre proprietà TCP/IP relative al computer.

#### <a name="planning-naming-conventions-for-computers-and-devices"></a>Pianificazione delle convenzioni di denominazione per computer e dispositivi
Per ragioni di uniformità della rete, è in genere consigliabile utilizzare nomi coerenti per server, stampanti e altri dispositivi. I nomi dei computer possono essere descrittivi per aiutare utenti e amministratori a identificare facilmente la funzione e la posizione di un server, una stampante o un altro dispositivo. Ad esempio, se si dispone di tre server DNS, uno a San Francisco, uno a Los Angeles e uno a Chicago, è possibile utilizzare la convenzione di denominazione *funzione server*-*percorso*-*numero*:

-   DNS-DEN-01. Questo nome rappresenta il server DNS a Denver, Colorado. Se vengono aggiunti ulteriori server DNS a Denver, verrà incrementato il valore numerico nel nome, ottenendo ad esempio DNS-DEN-02 e DNS-DEN-03.

-   DNS-SPAS-01. Questo nome rappresenta il server DNS a South Pasadena, California.

-   DNS-ORL-01. Questo nome rappresenta il server DNS a Orlando, Florida.

In questa guida è stata adottata una convenzione di denominazione molto semplice, costituita dalla funzione principale del server e da un numero. Ad esempio, il controller di dominio è denominato DC1 e il server DHCP è denominato DHCP1.

È consigliabile scegliere una convenzione di denominazione prima di procedere all'installazione della rete core in base alle istruzioni contenute in questa guida.

#### <a name="planning-static-ip-addresses"></a>Pianificazione degli indirizzi IP statici
Prima di configurare un indirizzo IP statico per ogni computer è necessario pianificare le subnet e gli intervalli di indirizzi IP. È inoltre necessario determinare gli indirizzi IP dei server DNS. Se si prevede di installare un router che consente di accedere ad altre reti, ad esempio ad altre subnet o a Internet, per configurare gli indirizzi IP statici sarà necessario conoscere l'indirizzo IP del router, denominato anche gateway predefinito.

Nella tabella seguente sono riportati alcuni valori di esempio per la configurazione degli indirizzi IP statici.

|Elementi di configurazione|Valori di esempio|
|-----------------------|------------------|
|L'indirizzo IP|10.0.0.2|
|Subnet mask|255.255.255.0|
|Gateway predefinito (indirizzo IP router)|10.0.0.1|
|Server DNS preferito|10.0.0.2|

> [!NOTE]
> Se si prevede di distribuire più di un server DNS, è possibile pianificare anche l'indirizzo IP del server DNS alternativo.

### <a name="bkmk_NetFndtn_Pln_AD-DNS-01"></a>Pianificazione della distribuzione di DC1
Di seguito sono riportati i passaggi di pianificazione chiave prima di installare servizi di dominio Active Directory (AD DS) e DNS in DC1.

#### <a name="planning-the-name-of-the-forest-root-domain"></a>Pianificazione del nome del dominio radice della foresta
Un primo passaggio nel processo di progettazione di Active Directory è stabilire quante foreste necessario nell'organizzazione. Un insieme di strutture è il contenitore di dominio Active Directory di livello superiore e costituito da uno o più domini che condividono uno schema comune e un catalogo globale. Un'organizzazione può includere più foreste, ma nella maggior parte delle organizzazioni viene preferita una struttura con una singola foresta, che è più semplice da amministrare.

Quando si crea il primo controller di dominio dell'organizzazione si creano anche il primo dominio, o dominio radice della foresta, e la prima foresta. Prima di effettuare questa operazione seguendo le istruzioni contenute in questa guida, è tuttavia necessario scegliere il nome di dominio ottimale per la propria organizzazione. Nella maggior parte dei casi, per il nome di dominio viene utilizzato il nome dell'organizzazione e si provvede alla sua registrazione. Se si pianifica la distribuzione di server Web con connessioni Internet per fornire informazioni e servizi ai propri clienti o partner, è opportuno scegliere un nome di dominio che non sia già stato utilizzato e registrarlo in modo che diventi proprietà dell'organizzazione.

#### <a name="planning-the-forest-functional-level"></a>Pianificazione del livello di funzionalità della foresta
Durante l'installazione di Active Directory, è necessario scegliere il livello di funzionalità che si desidera utilizzare. Funzionalità del dominio e foresta, introdotta in Windows Server 2003 Active Directory consente di abilitare le funzionalità di Active Directory all'interno dell'ambiente di rete a dominio o foresta wide. A seconda dell'ambiente, sono disponibili vari livelli di funzionalità per domini e foreste.

La funzionalità a livello di foresta abilita le funzionalità in tutti i domini della foresta. Per le foreste sono disponibili i livelli di funzionalità seguenti:

-    Windows Server 2008. Questo livello di funzionalità foresta supporta solo i controller di dominio che eseguono Windows Server 2008 e versioni successive del sistema operativo Windows Server.

-    Windows Server 2008 R2. Questo livello di funzionalità foresta supporta controller di dominio di Windows Server 2008 R2 e controller di dominio che eseguono versioni successive del sistema operativo Windows Server.

-    Windows Server 2012. Questo livello di funzionalità foresta supporta controller di dominio di Windows Server 2012 e i controller di dominio che eseguono versioni successive del sistema operativo Windows Server.

-    Windows Server 2012 R2. Questo livello di funzionalità foresta supporta controller di dominio di Windows Server 2012 R2 e controller di dominio che eseguono versioni successive del sistema operativo Windows Server.

-    Windows Server 2016. Questo livello di funzionalità foresta supporta solo i controller di dominio di Windows Server 2016 e controller di dominio che eseguono versioni successive del sistema operativo Windows Server.

Se si distribuisce un nuovo dominio in una nuova foresta e tutti i controller di dominio sarà in esecuzione Windows Server 2016, è consigliabile configurare Active Directory con il livello di funzionalità foresta Windows Server 2016 durante l'installazione di Active Directory.

> [!IMPORTANT]
> Dopo aver aumentato il livello di funzionalità della foresta, i controller di dominio che eseguono sistemi operativi precedenti non possono essere inseriti nella foresta. Ad esempio, se si aumenta il livello funzionale di foresta a Windows Server 2016, i controller di dominio che eseguono Windows Server 2012 R2 o Windows Server 2008 Impossibile aggiungere all'insieme di strutture.

Nella tabella seguente vengono forniti gli elementi di configurazione per Active Directory.

|Elementi di configurazione|Valori di esempio|
|------------------------|-------------------|
|Nome DNS completo|Esempi:<br /><br />-corp.contoso.com<br />-example.com|
|Livello di funzionalità della foresta|-Windows Server 2008 <br />-Windows Server 2008 R2 <br />-Windows Server 2012 <br />-Windows Server 2012 R2 <br />-Windows Server 2016|
|Percorso della cartella per il database di Servizi di dominio Active Directory|E:\Configuration @ no__t-0<br /><br />In alternativa accettare il percorso predefinito.|
|Percorso della cartella per i file di registro di Servizi di dominio Active Directory|E:\Configuration @ no__t-0<br /><br />In alternativa accettare il percorso predefinito.|
|Percorso della cartella SYSVOL di Servizi di dominio Active Directory|E:\Configuration @ no__t-0<br /><br />In alternativa accettare il percorso predefinito.|
|Password di amministratore modalità ripristino servizi directory|**J @ no__t-1p2leO4 $ F**|
|Nome file di risposte (facoltativo)|**DS_AnswerFile AD**|

#### <a name="planning-dns-zones"></a>Pianificazione delle zone DNS
Nei server DNS primari integrati in Active Directory viene creata una zona di ricerca diretta per impostazione predefinita, durante l'installazione del ruolo server DNS. La zona di ricerca diretta consente a computer e dispositivi di eseguire una query sull'indirizzo IP di un altro computer o dispositivo in base al nome DNS. Oltre alla zona di ricerca diretta, è consigliabile creare anche una zona DNS di ricerca inversa. Con una query DNS di ricerca inversa, un computer o dispositivo può scoprire il nome di un altro computer o dispositivo a partire dall'indirizzo IP. L'implementazione di una zona di ricerca inversa migliora in genere le prestazioni DNS e aumenta notevolmente il numero delle query DNS con esito positivo.

Quando si crea una zona di ricerca inversa, nel sistema DNS viene configurato il dominio in-addr.arpa, che è definito negli standard DNS ed è riservato nello spazio dei nomi DNS Internet in modo da offrire uno strumento pratico e affidabile per l'esecuzione delle query inverse. Per creare lo spazio dei nomi inverso, nel dominio in-addr.arpa vengono formati alcuni sottodomini utilizzando l'ordine inverso dei numeri nella notazione decimale puntata degli indirizzi IP.

Il dominio in-addr. arpa si applica a tutte le reti TCP/IP che si basano sul protocollo Internet versione 4 (IPv4) addressing. La Creazione guidata nuova zona presuppone l'utilizzo di questo dominio durante la creazione di una nuova zona di ricerca inversa.

Durante l'esecuzione della Creazione guidata nuova zona è consigliabile specificare le impostazioni seguenti:

|Elementi di configurazione|Valori di esempio|
|-----------------------|------------------|
|Tipo di zona|Le opzioni **Zona primaria** e **Archivia la zona in Active Directory** sono selezionate|
|Ambito di replica zona Active Directory|**A tutti i server DNS in questo dominio**|
|Prima pagina della creazione guidata Nome della zona di ricerca inversa|**Zona di ricerca inversa IPv4**|
|Seconda pagina della creazione guidata Nome della zona di ricerca inversa|ID rete = 10.0.0.|
|Aggiornamenti dinamici|**Consenti solo aggiornamenti dinamici sicuri**|

### <a name="bkmk_NetFndtn_Pln_DomAccess"></a>Pianificazione dell'accesso al dominio
Per accedere al dominio, il computer deve essere un computer membro del dominio e l'account utente deve essere creato in Active Directory prima di tentare l'accesso.

> [!NOTE]
> I singoli computer che eseguono Windows hanno un database degli account utente per utenti e gruppi locali, definito database del sistema di gestione degli account di sicurezza (SAM). Quando si crea un account utente nel computer locale nel database SAM, è possibile accedere al computer locale ma non a un dominio. Gli account utente di dominio vengono invece creati con lo lo snap-in MMC Utenti e computer di Active Directory in un controllo di dominio, non con gli utenti e i gruppi locali nel computer locale.

Dopo il primo accesso riuscito grazie alle credenziali di dominio, le impostazioni di accesso restano invariate finché non vengono modificate manualmente o il computer viene rimosso dal dominio.

Prima di effettuare l'accesso al dominio:

-   Creare gli account utente in Utenti e computer di Active Directory. Ogni utente deve disporre di un account di Servizi di dominio Active Directory in Utenti e computer di Active Directory. Per ulteriori informazioni, vedere [Creazione di un account utente in Utenti e computer di Active Directory](#BKMK_createUA).

-   Verificare che la configurazione degli indirizzi IP sia corretta. Un computer può essere aggiunto al dominio solo se ha un indirizzo IP. In questa guida i server sono configurati con indirizzi IP statici e i computer client ricevono lease di indirizzi IP dal server DHCP. Per questo motivo è necessario distribuire il server DHCP prima di aggiungere client al dominio. Per ulteriori informazioni, vedere [distribuzione di DHCP1](#BKMK_deployDHCP01).

-   Aggiungere il computer al dominio. È necessario aggiungere al dominio tutti i computer che forniscono o accedono a risorse di rete. Per ulteriori informazioni, vedere [aggiunta di computer Server al dominio ed effettuare l'accesso](#BKMK_joinlogserver) e [aggiunta di computer Client al dominio ed effettuare l'accesso](#BKMK_joinlogclients).

### <a name="bkmk_NetFndtn_Pln_DHCP-01"></a>Pianificazione della distribuzione di DHCP1
Di seguito sono riportati i passaggi chiave per la pianificazione, che dovranno essere eseguiti prima di installare il ruolo server DHCP in DHCP1.

#### <a name="planning-dhcp-servers-and-dhcp-forwarding"></a>Pianificazione dei server DHCP e dell'inoltro DHCP
Poiché i messaggi DHCP sono messaggi trasmessi, non vengono inoltrati fra le subnet dai router. Se sono disponibili più subnet e si desidera fornire il servizio DHCP per ogni subnet, è necessario eseguire una delle operazioni seguenti:

-   Installare un server DHCP in ogni subnet

-   Configurare i router per l'inoltro dei messaggi trasmessi DHCP tra le subnet e configurare un ambito per ogni subnet sul server DHCP.

Nella maggior parte dei casi è più conveniente configurare i router per l'inoltro dei messaggi trasmessi DHCP che non distribuire un server DHCP in ogni segmento fisico della rete.

#### <a name="planning-ip-address-ranges"></a>Pianificazione degli intervalli di indirizzi IP
Ogni subnet deve disporre di un proprio intervallo di indirizzi IP univoco. In un server DHCP tali intervalli sono rappresentati dagli ambiti.

Un ambito è un raggruppamento amministrativo di indirizzi IP per i computer di una subnet che utilizzano il servizio DHCP. L'amministratore crea innanzitutto un ambito per ogni subnet fisica, dopodiché lo utilizza per definire i parametri utilizzati dai client.

Ogni ambito è caratterizzato dalle proprietà seguenti:

-   Un intervallo di indirizzi IP in cui includere o escludere gli indirizzi utilizzati per le offerte di lease del servizio DHCP.

-   Una subnet mask, che determina il prefisso subnet per un determinato indirizzo IP.

-   Un nome di ambito assegnato al momento della creazione.

-   I valori di durata dei lease, che vengono assegnati ai client DHCP che ricevono gli indirizzi IP allocati dinamicamente.

-   Tutte le opzioni relative all'ambito DHCP configurate per l'assegnazione ai client DHCP, ad esempio l'indirizzo IP del server DNS e l'indirizzo IP del router/gateway predefinito.

-   Facoltativamente è possibile utilizzare le prenotazioni per garantire che un client DHCP riceva sempre lo stesso indirizzo IP.

Prima di distribuire i server, specificare le subnet e l'intervallo di indirizzi IP che si desidera utilizzare per ogni subnet.

#### <a name="planning-subnet-masks"></a>Pianificazione delle subnet mask
All'interno di un indirizzo IP, l'ID di rete e l'ID dell'host vengono distinti applicando una subnet mask. Una subnet mask è un numero di 32 bit che utilizza gruppi di bit consecutivi composti da soli uno (1) per identificare la parte dell'indirizzo IP corrispondente all'ID di rete e da soli zero (0) per identificare la parte corrispondente all'ID dell'host.

La subnet mask normalmente utilizzata con l'indirizzo IP 131.107.16.200, ad esempio, è il seguente numero binario di 32 bit:

```
11111111 11111111 00000000 00000000
```

Questo numero di subnet mask è 16 uno (1) seguiti da 16 bit zero, che indica che le sezioni di ID host e ID di rete dell'indirizzo IP sono entrambi 16 bit. Questa subnet mask viene in genere visualizzata in notazione decimale puntata, ovvero 255.255.0.0.

Nella tabella seguente sono riportate le subnet mask per le classi di indirizzi Internet.

|Classe di indirizzi|Subnet mask in formato binario|Subnet mask|
|-----------------|------------------------|---------------|
|Classe A|11111111 00000000 00000000 00000000|255.0.0.0|
|Classe B|11111111 11111111 00000000 00000000|255.255.0.0|
|Classe C|11111111 11111111 11111111 00000000|255.255.255.0|

Quando si crea un ambito in DHCP e si immette l'intervallo di indirizzi IP per tale ambito, DHCP fornisce queste subnet mask predefinite. In genere i valori delle subnet mask predefinite sono accettabili per la maggioranza delle reti senza requisiti particolari, in cui il segmento della rete IP corrisponde a una sola rete fisica.

In alcuni casi è possibile utilizzare subnet mask personalizzate per implementare la suddivisione in subnet degli indirizzi IP. Con la suddivisione in subnet degli indirizzi IP è possibile suddividere la porzione predefinita relativa all'ID dell'host di un indirizzo IP in modo da specificare le subnet, che sono a loro volta suddivisioni dell'ID di rete basato su classe originale.

Personalizzando la lunghezza della subnet mask è possibile ridurre il numero di bit utilizzato per l'ID effettivo dell'host.

Per evitare problemi di indirizzamento e routing, è consigliabile verificare che tutti i computer TCP/IP connessi a un determinato segmento di rete utilizzino la stessa subnet e che ogni computer o dispositivo utilizzi un indirizzo IP univoco.

#### <a name="planning-exclusion-ranges"></a>Pianificazione degli intervalli di esclusione
Quando si crea un ambito in un server DHCP, si specifica un intervallo di indirizzi IP comprendente tutti gli indirizzi IP che il server DHCP è autorizzato a concedere in lease ai client DHCP, ad esempio computer e altri dispositivi. Se in seguito si configurano manualmente alcuni server e altri dispositivi con indirizzi IP statici compresi nello stesso intervallo di indirizzi IP utilizzato dal server DHCP, potrebbe crearsi un confitto involontario di indirizzi IP per cui l'utente e il server DHCP assegnano lo stesso indirizzo IP a dispositivi diversi.

Per risolvere questo problema, è possibile creare un intervallo di esclusione per l'ambito DHCP. Intervallo di esclusione è un contigui intervallo di indirizzi IP nell'intervallo di indirizzi IP dell'ambito che il server DHCP non è consentito utilizzare. Se si crea un intervallo di esclusione, il server DHCP non assegnerà gli indirizzi compresi in tale intervallo e consentirà così l'assegnazione manuale di tali indirizzi senza creare nessun conflitto di indirizzi IP.

È possibile impedire al server DHCP di distribuire specifici indirizzi IP creando un intervallo di esclusione per ogni ambito. È consigliabile utilizzare le esclusioni per tutti i dispositivi configurati con un indirizzo IP statico. Dovrebbero far parte degli indirizzi esclusi tutti gli indirizzi IP che sono stati assegnati manualmente ad altri server, a client non DHCP, a workstation senza disco o a client di Routing e Accesso remoto e PPP.

È consigliabile configurare l'intervallo di esclusione con alcuni indirizzi supplementari per tenere conto dell'eventuale espansione futura della rete. Nella tabella seguente fornisce un intervallo di esclusione di esempio per un ambito con un indirizzo IP 10.0.0.1 - 10.0.0.254 e una subnet mask 255.255.255.0.

|Elementi di configurazione|Valori di esempio|
|-----------------------|------------------|
|Indirizzo IP iniziale dell'intervallo di esclusione|10.0.0.1|
|Indirizzo IP finale dell'intervallo di esclusione|10.0.0.25|

#### <a name="planning-tcpip-static-configuration"></a>Pianificazione della configurazione degli indirizzi TCP/IP statici
Alcuni dispositivi, ad esempio i router, i server DHCP e i server DNS, devono essere configurati con un indirizzo IP statico. Nella rete potrebbero essere presenti anche altri dispositivi, ad esempio stampanti, che si desidera abbiano sempre lo stesso indirizzo IP. Creare un elenco dei dispositivi da configurare in modo statico per ogni subnet, quindi pianificare l'intervallo di esclusione da utilizzare sul server DHCP per garantire che quest'ultimo non assegni in lease l'indirizzo IP di un dispositivo configurato in modo statico. L'intervallo di esclusione è una sequenza limitata di indirizzi IP all'interno di un ambito, che viene esclusa dalle offerte del servizio DHCP. Gli indirizzi inclusi in un intervallo di esclusione non possono essere offerti dal server ai client DHCP della rete.

Se a una determinata subnet è associato l'intervallo di indirizzi IP da 192.168.0.1 a 192.168.0.254 e sono presenti dieci dispositivi che si desidera configurare con un indirizzo IP statico, per l'ambito 192.168.0.*x* sarà possibile creare un intervallo di esclusione contenente dieci o più indirizzi IP, ad esempio da 192.168.0.1 a 192.168.0.15.

In questo esempio dieci degli indirizzi IP esclusi vengono utilizzati per configurare i server e gli altri dispositivi con gli indirizzi IP statici, mentre altri cinque indirizzi IP restano a disposizione per l'eventuale configurazione statica di nuovi dispositivi aggiunti in seguito. Con questo intervallo di esclusione, il server DHCP ha ancora a disposizione gli indirizzi compresi tra 192.168.0.16 e 192.168.0.254.

Nella tabella seguente vengono forniti gli elementi di configurazione di esempio aggiuntive per Active Directory e DNS.

|Elementi di configurazione|Valori di esempio|
|-----------------------|------------------|
|Binding connessioni di rete|Ethernet|
|Impostazioni server DNS|DC1.corp.contoso.com|
|Indirizzo IP del server DNS preferito|10.0.0.2|
|Valori per la finestra di dialogo Aggiunta ambito<br /><br />1.  Nome dell'ambito<br />2.  Indirizzo IP iniziale<br />3.  Indirizzo IP finale<br />4.  Subnet mask<br />5.  Gateway predefinito (facoltativo)<br />6.  Durata del lease|1.  Subnet principale<br />2.10.0.0.1<br />3.10.0.0.254<br />4.255.255.255.0<br />5.10.0.0.1<br />6. 8 giorni|
|Modalità operativa server DHCP IPv6|Non abilitato|

## <a name="BKMK_deployment"></a>Distribuzione di rete core
La procedura di base per distribuire una rete core è la seguente:

1.  [Configurazione di tutti i server](#BKMK_configuringAll)

2.  [Distribuzione di DC1](#BKMK_deployADDNS01)

3.  [Aggiunta di computer server al dominio e accesso](#BKMK_joinlogserver)

4.  [Distribuzione di DHCP1](#BKMK_deployDHCP01)

5.  [Aggiunta di computer client al dominio e accesso](#BKMK_joinlogclients)

6.  [Distribuzione di funzionalità facoltative per l'autenticazione dell'accesso alla rete e i servizi Web](#BKMK_optionalfeatures)

> [!NOTE]
> -   Per la maggior parte delle procedure descritte in questa guida vengono forniti anche i comandi equivalenti in Windows PowerShell. Prima di eseguire tali cmdlet in Windows PowerShell, sostituire i valori di esempio con valori adatti per la distribuzione di rete. È inoltre necessario immettere ogni cmdlet su un'unica riga di Windows PowerShell. È possibile che i singoli cmdlet siano visualizzati su più righe in questa guida, a causa dei vincoli di formattazione e della visualizzazione del documento in un browser o altra applicazione.
> -   Le procedure illustrate in questa guida non includono istruzioni per i casi in cui viene visualizzata la finestra di dialogo **Controllo dell'account utente** per richiedere l'autorizzazione a continuare. Se si apre questa finestra di dialogo mentre si eseguono le procedure descritte in questa Guida, se è stata aperta la finestra di dialogo in risposta alle azioni dell'utente, fare clic su **Continue**.

### <a name="BKMK_configuringAll"></a>Configurazione di tutti i server
Prima di installare altre tecnologie, ad esempio Servizi di dominio Active Directory o DHCP, è importante configurare gli elementi seguenti.

-   [Rinominare il computer](#BKMK_rename)

-   [Configurare un indirizzo IP statico](#BKMK_ip)

Per eseguire tali azioni in ogni server, è possibile utilizzare le sezioni seguenti.

L'appartenenza a **amministratori**, o equivalente è il requisito minimo necessario per eseguire queste procedure.

#### <a name="BKMK_rename"></a>Rinominare il computer
È possibile utilizzare la procedura in questa sezione per modificare il nome di un computer. La ridenominazione del computer è utile qualora il sistema operativo abbia creato automaticamente un nome computer che non si desidera utilizzare.

> [!NOTE]
> Per eseguire questa procedura mediante Windows PowerShell, aprire PowerShell e digitare i cmdlet seguenti su righe distinte, quindi premere INVIO. È inoltre necessario sostituire il *NomeComputer* con il nome che si desidera utilizzare.
>
> `Rename-Computer`*nomecomputer*
>
> `Restart-Computer`

###### <a name="to-rename-computers-running-windows-server-2016-windows-server-2012-r2-and-windows-server-2012"></a>Per rinominare i computer che eseguono Windows Server 2016, Windows Server 2012 R2 e Windows Server 2012

1.  In Server Manager fare clic su **Server locale**. Il computer **proprietà** vengono visualizzati nel riquadro dei dettagli.

2.  In **proprietà**, in **nome Computer**, fare clic sul nome del computer. Il **le proprietà di sistema** verrà visualizzata la finestra di dialogo. Fare clic su **Cambia**. Il **Cambiamenti dominio/nome Computer** verrà visualizzata la finestra di dialogo.

3.  Nella finestra di dialogo **Cambiamenti dominio/nome computer** digitare un nuovo nome per il computer in **Nome computer**. Se ad esempio si desidera assegnare al computer il nome DC1, digitare **DC1**.

4.  Fare clic due volte su **OK** e quindi su **Chiudi**. Se si desidera riavviare immediatamente il computer per completare la modifica del nome, fare clic su **Riavvia**. In caso contrario fare clic su **Riavvia in seguito**.

> [!NOTE]
> Per informazioni su come rinominare i computer che eseguono altri sistemi operativi Microsoft, vedere [Appendice A - ridenominazione dei computer](#BKMK_A).

#### <a name="BKMK_ip"></a>Configurare un indirizzo IP statico
È possibile utilizzare le procedure descritte in questo argomento per configurare il protocollo Internet versione 4 (IPv4) le proprietà della connessione di rete con un indirizzo IP statico di indirizzi per i computer che eseguono Windows Server 2016.

> [!NOTE]
> Per eseguire questa procedura mediante Windows PowerShell, aprire PowerShell e digitare i cmdlet seguenti su righe distinte, quindi premere INVIO. È inoltre necessario sostituire i nomi di interfaccia e gli indirizzi IP riportati in questo esempio con i valori che si desidera utilizzare per configurare il proprio computer.
>
> `New-NetIPAddress -IPAddress 10.0.0.2 -InterfaceAlias "Ethernet" -DefaultGateway 10.0.0.1 -AddressFamily IPv4 -PrefixLength 24`
>
> `Set-DnsClientServerAddress -InterfaceAlias "Ethernet" -ServerAddresses 127.0.0.1`

###### <a name="to-configure-a-static-ip-address-on-computers-running-windows-server-2016-windows-server-2012-r2-and-windows-server-2012"></a>Per configurare un indirizzo IP statico in computer che eseguono Windows Server 2016, Windows Server 2012 R2 e Windows Server 2012

1.  Nella barra delle applicazioni fare clic con il pulsante destro del mouse sull'icona della rete, quindi scegliere **Apri Centro connessioni di rete e condivisione**.

2.  In  **Centro rete e condivisione**, fare clic su **modificare le impostazioni della scheda**. Viene aperta la cartella **Connessioni di rete**, dove sono visualizzate le connessioni di rete disponibili.

3.  In **connessioni di rete**, pulsante destro del mouse sulla connessione che si desidera configurare e quindi fare clic su **proprietà**. La connessione di rete **proprietà** verrà visualizzata la finestra di dialogo.

4.  Nella connessione di rete **proprietà** della finestra di dialogo **la connessione utilizza i seguenti elementi**, selezionare **Internet Protocol versione 4 (TCP/IPv4)** , quindi fare clic su **proprietà**. Il **proprietà protocollo Internet versione 4 (TCP/IPv4)** verrà visualizzata la finestra di dialogo.

5.  In **proprietà protocollo Internet versione 4 (TCP/IPv4)** , via il **Generale** scheda, fare clic su **utilizzare il seguente indirizzo IP**. In **indirizzo IP**, digitare l'indirizzo IP che si desidera utilizzare.

6.  Premere TAB per posizionare il cursore in **Subnet mask**. Viene immesso automaticamente un valore predefinito per la subnet mask. Accettare la subnet mask predefinita oppure digitare la subnet mask che si desidera utilizzare.

7.  In **gateway predefinito**, digitare l'indirizzo IP del gateway predefinito.

    > [!NOTE]
    > È necessario configurare **gateway predefinito** con lo stesso indirizzo IP utilizzato nell'interfaccia di rete locale (LAN) del router. Ad esempio, se si dispone di un router che è connesso a una rete WAN (WAN), ad esempio Internet nonché LAN, configurare l'interfaccia LAN con lo stesso indirizzo IP che si specificherà quindi come il **gateway predefinito**. Un altro esempio potrebbe essere quello di un router che è collegato a due reti LAN, dove la rete LAN A utilizza l'intervallo di indirizzi 10.0.0.0/24 e la rete LAN B utilizza l'intervallo di indirizzi 192.168.0.0/24. In questo caso è necessario configurare l'indirizzo IP del router LAN A con un indirizzo compreso nell'intervallo di indirizzi corrispondente, ad esempio 10.0.0.1. Inoltre, nell'ambito DHCP per questo intervallo di indirizzi, configurare **gateway predefinito** con l'indirizzo IP 10.0.0.1. Per LAN B, configurare l'interfaccia del router LAN B con un indirizzo di tale intervallo di indirizzi, ad esempio 192.168.0.1 e quindi la 192.168.0.0/24 ambito LAN B con un **gateway predefinito** valore 192.168.0.1.

8.  In **server DNS preferito**, digitare l'indirizzo IP del server DNS. Se si prevede di utilizzare il computer locale come server DNS preferito, digitare l'indirizzo IP del computer locale.

9. In **Server DNS alternativo** digitare l'indirizzo IP del server DNS alternativo, se disponibile. Se si prevede di utilizzare il computer locale come server DNS alternativo, digitare l'indirizzo IP del computer locale.

10. Fare clic su **OK** e quindi fare clic su **Chiudi**.

> [!NOTE]
> Per informazioni su come configurare un indirizzo IP statico in computer che eseguono altri sistemi operativi Microsoft, vedere [Appendice B - configurazione di indirizzi IP statici](#BKMK_B).

#### <a name="BKMK_deployADDNS01"></a>Distribuzione di DC1
Per distribuire DC1, ovvero è il computer che esegue servizi di dominio Active Directory (AD DS) e DNS, è necessario completare questi passaggi nell'ordine seguente:

-   Eseguire i passaggi nella sezione [configurazione di tutti i server](#BKMK_configuringAll).

-   [Installare servizi di dominio Active Directory e DNS per una nuova foresta](#BKMK_installAD-DNS)

-   [Creare un account utente in Active Directory utenti e computer](#BKMK_createUA)

-   [Assegnare l'appartenenza al gruppo](#BKMK_assigngroup)

-   [Configurare una zona DNS di ricerca inversa](#BKMK_reverse)

**Privilegi amministrativi**

Se si installa una rete di piccole dimensioni e si è il solo amministratore di rete, è consigliabile creare un account utente personale e quindi aggiungerlo come membro dei gruppi Enterprise Admins e Domain Admins. In questo modo sarà più semplice eseguire le attività di amministrazione per tutte le risorse di rete È inoltre consigliabile effettuare l'accesso con questo account solo per eseguire le attività amministrative e creare un account utente separato per le attività non correlate alla gestione IT.

Se si dispone di un'organizzazione più grande con più amministratori, fare riferimento alla documentazione di Active Directory per determinare l'appartenenza al gruppo migliore per i dipendenti dell'organizzazione.

**Differenze tra account utente di dominio e account utente nel computer locale**

Uno dei vantaggi di un'infrastruttura basata sul dominio è non dover creare gli account utente in ogni computer del dominio. Ciò vale per tutti i computer, sia client che server.

Per questo motivo non è dunque necessario creare gli account utente in ogni computer del dominio. Creare tutti gli account utente in Utenti e computer di Active Directory e utilizzare le procedure descritte in precedenza per assegnare l'appartenenza ai gruppi. Per impostazione predefinita, tutti gli account utente sono membri del gruppo Domain Users.

Tutti i membri del gruppo Domain Users possono eseguire l'accesso a qualsiasi computer client dopo che questo è stato aggiunto al dominio.

È possibile configurare gli account utente specificando i giorni e gli orari in cui l'utente è autorizzato ad accedere ai computer. È inoltre possibile specificare i computer che l'utente è autorizzato a utilizzare. Per configurare queste impostazioni, aprire Utenti e computer di Active Directory, individuare l'account utente che si desidera configurare e fare doppio clic sull'account. Nella finestra **Proprietà** dell'account utente fare clic sulla scheda **Account** e quindi su **Orario di accesso** o su **Accedi a**.

#### <a name="BKMK_installAD-DNS"></a>Installare servizi di dominio Active Directory e DNS per una nuova foresta

È possibile utilizzare una delle seguenti procedure per installare Active Directory Domain Services (AD DS) e DNS e per creare un nuovo dominio in una nuova foresta. 

La prima procedura fornisce istruzioni sull'esecuzione di queste azioni mediante Windows PowerShell, mentre nella seconda procedura viene illustrato come installare servizi di dominio Active Directory e DNS utilizzando Server Manager.

>[!IMPORTANT]
>Al termine dell'esecuzione dei passaggi di questa procedura, il computer viene riavviato automaticamente.

**Installare servizi di dominio Active Directory e DNS con Windows PowerShell**

È possibile utilizzare i comandi seguenti per installare e configurare servizi di dominio Active Directory e DNS. È necessario sostituire il nome di dominio in questo esempio con il valore che si desidera utilizzare per il dominio.

>[!NOTE]
>Per ulteriori informazioni su questi comandi di Windows PowerShell, vedere gli argomenti di riferimento seguenti.
>- [Install-WindowsFeature](https://docs.microsoft.com/powershell/module/servermanager/install-windowsfeature?view=win10-ps)
>- [Install-ADDSForest](https://docs.microsoft.com/powershell/module/addsdeployment/install-addsforest?view=win10-ps)

Per eseguire questa procedura è necessaria almeno l'appartenenza al gruppo **Administrators**.

- Eseguire Windows PowerShell come amministratore, digitare il comando seguente e quindi premere INVIO:  

`Install-WindowsFeature AD-Domain-Services -IncludeManagementTools`

Al termine dell'installazione, viene visualizzato il messaggio seguente in Windows PowerShell.


    Success Restart Needed  Exit Code   Feature Result
    ------- --------------  ---------   --------------
    True    No              Success     {Active Directory Domain Services, Group P...


- In Windows PowerShell digitare il comando seguente, sostituendo il testo **Corp.contoso.com** con il nome di dominio e quindi premere INVIO:

````
Install-ADDSForest -DomainName "corp.contoso.com"
````

- Durante il processo di installazione e configurazione, che è visibile nella parte superiore della finestra di Windows PowerShell, viene visualizzato il prompt seguente. Una volta visualizzato, digitare una password, quindi premere INVIO.

    **SafeModeAdministratorPassword**

- Dopo aver digitato una password e premuto INVIO, viene visualizzata la seguente richiesta di conferma. Digitare la stessa password e premere INVIO.

    **Conferma SafeModeAdministratorPassword:**

- Quando viene visualizzato il prompt seguente digitare la lettera **Y** , quindi premere INVIO.


~~~
The target server will be configured as a domain controller and restarted when this operation is complete.
Do you want to continue with this operation?
[Y] Yes  [A] Yes to All  [N] No  [L] No to All  [S] Suspend  [?] Help (default is "Y"):
~~~

- Se lo si desidera, è possibile leggere i messaggi di avviso visualizzati durante la normale installazione di servizi di dominio Active Directory e DNS. Questi messaggi sono normali e non indicano errori di installazione.

- Una volta completata l'installazione, viene visualizzato un messaggio che informa che l'utente sta per essere disconnesso dal computer in modo che il computer possa essere riavviato. Se si fa clic su **Chiudi**, viene immediatamente effettuata la disconnessione dal computer e il computer viene riavviato. Se non si fa clic su **Chiudi**, il computer viene riavviato dopo un periodo di tempo predefinito.

- Una volta riavviato il server, è possibile verificare la corretta installazione di Active Directory Domain Services e DNS. Aprire Windows PowerShell, digitare il comando seguente e premere INVIO.

````
Get-WindowsFeature
````

I risultati di questo comando vengono visualizzati in Windows PowerShell e dovrebbero essere simili ai risultati nell'immagine seguente. Per le tecnologie installate, le parentesi quadre a sinistra del nome della tecnologia contengono il carattere **X**e il valore di **Install state** è **installato**.

![Risultati del comando Get-WindowsFeature](../media/Core-Network-Guide/server-roles-installed.jpg)

**Installare servizi di dominio Active Directory e DNS usando Server Manager**

1.  Su DC1, in **Server Manager**, fare clic su **Gestisci**, quindi fare clic su **Aggiungi ruoli e funzionalità**. Viene avviata l'Aggiunta guidata ruoli e funzionalità.

2.  In **Prima di iniziare** fare clic su **Avanti**.

    > [!NOTE]
    > La pagina **Prima di iniziare** dell'Aggiunta guidata ruoli e funzionalità non viene visualizzata se durante un'esecuzione precedente della procedura guidata è stata selezionata la casella di controllo **Ignora questa pagina per impostazione predefinita**.

3.  In **Select Installation Type**, assicurarsi che **installazione basata su ruoli o basata su funzionalità** sia selezionata e quindi fare clic su **Avanti**.

4.  In **Selezione server di destinazione** assicurarsi che sia selezionata l'opzione **Selezionare un server dal pool di server**. Assicurarsi che in **Pool di server** sia selezionato il computer locale. Fare clic su **Avanti**.

5.  In **Selezione ruoli server** fare clic su **Servizi di dominio Active Directory** in **Ruoli**. In **Aggiungere le funzionalità necessarie per Servizi di dominio Active Directory** fare clic su **Aggiungi funzionalità**. Fare clic su **Avanti**.

6.  In **Selezione funzionalità** fare clic su **Avanti**, quindi in **Servizi di dominio Active Directory** esaminare le informazioni fornite e quindi fare clic su **Avanti**.

7.  In **Conferma selezioni per l'installazione**, fare clic su **installare**. Nella pagina di stato dell'installazione viene visualizzato lo stato di avanzamento del processo di installazione. Al termine del processo, nei dettagli del messaggio fare clic su **Alza di livello il server a controller di dominio**. Si apre la Configurazione guidata Servizi di dominio Active Directory.

8.  In **Configurazione distribuzione** selezionare **Nuova foresta**. In **nome di dominio radice**, digitare il nome di dominio completo (FQDN) per il dominio. Ad esempio, se il nome del DOMINIO è corp.contoso.com, digitare **corp.contoso.com**. Fare clic su **Avanti**.

9. In **Opzioni controller di dominio** specificare il livello di funzionalità della foresta e del dominio che si desidera utilizzare in **Selezionare il livello di funzionalità della nuova foresta e del dominio radice**. In **specificare funzionalità del controller di dominio**, assicurarsi che **server Domain Name System (DNS)** e **catalogo globale (GC)** siano selezionate. In **Password** e **Conferma password** digitare la password per la modalità ripristino servizi directory. Fare clic su **Avanti**.

10. In **Opzioni DNS**, fare clic su **Avanti**.

11. In **Opzioni aggiuntive** verificare il nome NetBIOS che è assegnato al dominio e modificarlo solo se necessario. Fare clic su **Avanti**.

12. In **Percorsi**, in **Specificare il percorso del database di Servizi di dominio Active Directory, dei file di log e di SYSVOL**, eseguire una delle operazioni seguenti:

    -   Accettare i valori predefiniti.

    -   Digitare i percorsi che si desidera utilizzare per **cartella Database**, **cartella file di registro**, e **cartella SYSVOL**.

13. Fare clic su **Avanti**.

14. In **Verifica opzioni** verificare le impostazioni selezionate.

15. Se si desidera esportare le impostazioni in uno script di Windows PowerShell, fare clic su **visualizzare script**. Lo script viene aperto in Blocco note e può essere salvato nel percorso di cartelle desiderato. Fare clic su **Avanti**. In **Controllo dei prerequisiti** vengono confermate le impostazioni selezionate. Al termine del controllo fare clic su **Installa**. Al prompt di Windows fare clic su **Chiudi**. Il server viene riavviato per completare l'installazione di servizi di dominio Active Directory e DNS.

16. Per verificare se l'installazione è riuscita, visualizzare la console di Server Manager dopo il riavvio del server. I servizi di dominio Active Directory e DNS verranno visualizzati nel riquadro sinistro, come gli elementi evidenziati nell'immagine seguente.

![Servizi di dominio Active Directory e DNS in Server Manager](../media/Core-Network-Guide/server-roles-installed-sm.jpg)

##### <a name="BKMK_createUA"></a>Creare un account utente in Active Directory utenti e computer
Questa procedura consente di creare un nuovo account utente di dominio nello snap-in di Microsoft Management Console (MMC) Utenti e computer di Active Directory.

Per eseguire questa procedura è necessaria almeno l'appartenenza al gruppo **Domain Admins** oppure a un gruppo equivalente.

> [!NOTE]
> Per eseguire questa procedura mediante Windows PowerShell, aprire PowerShell e digitare i cmdlet seguenti su una riga, quindi premere INVIO. È necessario sostituire il nome dell'account utente utilizzato in questo esempio con il valore che si desidera utilizzare.
>
> `New-ADUser -SamAccountName User1 -AccountPassword (read-host "Set user password" -assecurestring) -name "User1" -enabled $true -PasswordNeverExpires $true -ChangePasswordAtLogon $false`
>
> Dopo aver premuto INVIO, digitare la password per l'account utente. Viene creato un account che, per impostazione predefinita, appartiene al gruppo Domain Users.
>
> Con il cmdlet seguente è possibile assegnare al nuovo account utente ulteriori appartenenze ai gruppi. Nell'esempio seguente l'account utente denominato User1 viene aggiunto ai gruppi Domain Admins ed Enterprise Admins. Prima di eseguire questo comando, assicurarsi di aver modificato il nome dell'account utente, il nome del dominio e i gruppi in base ai propri requisiti.
>
> `Add-ADPrincipalGroupMembership -Identity "CN=User1,CN=Users,DC=corp,DC=contoso,DC=com" -MemberOf "CN=Enterprise Admins,CN=Users,DC=corp,DC=contoso,DC=com","CN=Domain Admins,CN=Users,DC=corp,DC=contoso,DC=com"`

###### <a name="to-create-a-user-account"></a>Per creare un account utente

1.  Su DC1, in Server Manager fare clic su **strumenti**, quindi fare clic su **Active Directory Users and Computers**. Viene aperto lo snap-in MMC Utenti e computer di Active Directory. Se non è già selezionato, fare clic sul nodo corrispondente al proprio dominio. Ad esempio, se il dominio è corp.contoso.com, fare clic su **corp.contoso.com**.

2.  Nel riquadro dei dettagli fare clic con il pulsante destro del mouse sulla cartella in cui si desidera aggiungere un account utente.

    **In cui?**

    -   Active Directory Users and Computers /*nodo dominio*/*cartella*

3.  Scegliere **nuovo**, quindi fare clic su **utente**. Il **nuovo oggetto - utente** viene visualizzata la finestra di dialogo.

4.  In **nome**, digitare il nome dell'utente.

5.  In **Iniziali** digitare le iniziali dell'utente.

6.  In **Cognome**, digitare il cognome dell'utente.

7.  Modificare **nome completo** aggiungendo le iniziali o invertire l'ordine di nome e cognome.

8.  In **nome accesso utente**, digitare il nome di accesso utente. Fare clic su **Avanti**.

9. In **nuovo oggetto - utente**, in **Password** e **Conferma password**, digitare la password dell'utente e quindi selezionare le opzioni appropriate.

10. Fare clic su **Avanti**, esaminare le nuove impostazioni dell'account utente e quindi fare clic su **Fine**.

##### <a name="BKMK_assigngroup"></a>Assegnare l'appartenenza al gruppo
Questa procedura consente di aggiungere un utente, un computer o un gruppo a un gruppo nello snap-in di Microsoft Management Console (MMC) Utenti e computer di Active Directory.

L'appartenenza a **Domain Admins**, o equivalente è il requisito minimo necessario per eseguire questa procedura.

###### <a name="to-assign-group-membership"></a>Per assegnare l'appartenenza a un gruppo

1.  Su DC1, in Server Manager fare clic su **strumenti**, quindi fare clic su **Active Directory Users and Computers**. Viene aperto lo snap-in MMC Utenti e computer di Active Directory. Se non è già selezionato, fare clic sul nodo corrispondente al proprio dominio. Ad esempio, se il dominio è corp.contoso.com, fare clic su **corp.contoso.com**.

2.  Nel riquadro dei dettagli fare doppio clic sulla cartella che contiene il gruppo a cui si desidera aggiungere un membro.

    Percorso

    -   **Active Directory Users and Computers**/*nodo dominio*/*cartella che contiene il gruppo*

3.  Nel riquadro dei dettagli fare clic con il pulsante destro del mouse sull'oggetto che si desidera aggiungere a un gruppo, ad esempio un utente o un computer, quindi scegliere **Proprietà**. L'oggetto **proprietà** verrà visualizzata la finestra di dialogo. Fare clic sui **membro del** scheda.

4.  Nel **membro del** scheda, fare clic su **Aggiungi**.

5.  In **Immettere i nomi degli oggetti da selezionare** digitare il nome del gruppo al quale si desidera aggiungere l'oggetto, quindi fare clic su **OK**.

6.  Per assegnare l'appartenenza al gruppo per altri utenti, gruppi o computer, ripetere i passaggi 4 e 5 della procedura.

##### <a name="BKMK_reverse"></a>Configurare una zona DNS di ricerca inversa
Questa procedura consente di configurare una zona di ricerca inversa in DNS (Domain Name System).

L'appartenenza a **Domain Admins** è il requisito minimo necessario per eseguire questa procedura.

> [!NOTE]
> -   Per le organizzazioni di medie e grandi dimensioni, si consiglia di configurare e utilizzare il gruppo DNSAdmins in Active Directory Users and Computers. Per ulteriori informazioni, vedere [Risorse tecniche aggiuntive](#BKMK_resources).
> -   Per eseguire questa procedura mediante Windows PowerShell, aprire PowerShell e digitare i cmdlet seguenti su una riga, quindi premere INVIO. È necessario sostituire i nomi della zona di ricerca inversa DNS e del file di zona utilizzati in questo esempio con i valori che si desidera utilizzare. Assicurarsi di invertire l'ID di rete per il nome della zona inversa. Ad esempio, se l'ID di rete è 192.168.0, creare il nome di zona di ricerca inversa **0.168.192.in-addr.arpa**.
>
> `Add-DnsServerPrimaryZone 0.0.10.in-addr.arpa -ZoneFile 0.0.10.in-addr.arpa.dns`

###### <a name="to-configure-a-dns-reverse-lookup-zone"></a>Per configurare una zona DNS di ricerca inversa

1.  Su DC1, in Server Manager fare clic su **strumenti**, quindi fare clic su **DNS**. Si apre lo snap-in MMC DNS.

2.  In DNS fare doppio clic sul nome del server per espandere l'albero, se non è già espanso. Se ad esempio il nome del server DNS è DC1, fare doppio clic su **DC1**.

3.  Selezionare **Zone di ricerca inversa**, fare clic con il pulsante destro del mouse su **Zone di ricerca inversa** e quindi fare clic su **Nuova zona**. Si apre Creazione guidata nuova zona.

4.  In **iniziale per la creazione guidata nuova zona**, fare clic su **Avanti**.

5.  In **Tipo di zona** selezionare **Zona primaria**.

6.  Se il server DNS è un controller di dominio scrivibile, assicurarsi che sia selezionata l'opzione **Archivia la zona in Active Directory**. Fare clic su **Avanti**.

7.  In **Ambito di replica zona Active Directory** selezionare **In tutti i server DNS eseguiti nei controller di dominio del dominio seguente**, a meno che non sia opportuno selezionare un'altra opzione per un motivo specifico. Fare clic su **Avanti**.

8.  Nel primo **Nome zona di ricerca inversa** selezionare **zona di ricerca inversa IPv4**. Fare clic su **Avanti**.

9. Nella seconda pagina **Nome della zona di ricerca inversa** selezionare una delle opzioni seguenti:

    -   In **ID di rete** digitare l'ID della rete per l'intervallo di indirizzi IP. Se ad esempio l'intervallo di indirizzi IP è compreso tra 10.0.0.1 e 10.0.0.254, digitare **10.0.0**.

    -   In **Nome zona di ricerca inversa**, viene aggiunto automaticamente il nome di zona di ricerca inversa IPv4. Fare clic su **Avanti**.

10. In **aggiornamento dinamico**, selezionare il tipo di aggiornamenti dinamici che si desidera consentire. Fare clic su **Avanti**.

11. In **completare la creazione guidata nuova zona**, rivedere le scelte effettuate e quindi fare clic su **Fine**.

#### <a name="BKMK_joinlogserver"></a>Aggiunta di computer server al dominio e accesso
Dopo avere installato Servizi di dominio Active Directory (AD DS) e creato uno o più account utente che dispone delle autorizzazioni per aggiungere un computer al dominio, è possibile aggiungere server della rete core al dominio e accesso a tali server per installare tecnologie aggiuntive, ad esempio Dynamic Host Configuration Protocol (DHCP).

In tutti i server che si desidera distribuire, ad eccezione del server che esegue servizi di dominio Active Directory, eseguire le operazioni seguenti:

1.  Completare le procedure descritte [configurazione di tutti i server](#BKMK_configuringAll).

2.  Utilizzare le istruzioni contenute nelle due procedure seguenti per aggiungere i server al dominio ed effettuare l'accesso a tali server per eseguire ulteriori attività di distribuzione:

> [!NOTE]
> Per eseguire questa procedura mediante Windows PowerShell, aprire PowerShell e digitare il cmdlet seguente, quindi premere INVIO. È inoltre necessario sostituire il nome del dominio con il nome che si desidera utilizzare.
>
> `Add-Computer -DomainName corp.contoso.com`
>
> Al prompt, digitare il nome e la password per un account che dispone delle autorizzazioni necessarie per aggiungere un computer al dominio. Per riavviare il computer, digitare il comando seguente e premere INVIO.
>
> `Restart-Computer`

###### <a name="to-join-computers-running--windows-server-2016--windows-server-2012-r2--and--windows-server-2012--to-the-domain"></a>Per aggiungere i computer che esegue Windows Server 2016, Windows Server 2012 R2 e Windows Server 2012 al dominio

1.  In Server Manager fare clic su **Server locale**. Nel riquadro dei dettagli, fare clic su **gruppo di LAVORO**. Il **le proprietà di sistema** verrà visualizzata la finestra di dialogo.

2.  Nella finestra di dialogo **Proprietà del sistema** fare clic su **Cambia**. Il **Cambiamenti dominio/nome Computer** verrà visualizzata la finestra di dialogo.

3.  In **Nome computer** fare clic su **Dominio** in **Membro di**, quindi digitare il nome del dominio a cui si desidera aggiungere il computer. Se ad esempio il nome del dominio è corp.contoso.com, digitare **corp.contoso.com**.

4.  Fare clic su **OK**. Il **la protezione di Windows** verrà visualizzata la finestra di dialogo.

5.  In **Cambiamenti dominio/nome Computer**, in **nome utente**, digitare il nome utente e in **Password**, digitare la password e quindi fare clic su **OK**. Viene visualizzata la finestra di dialogo **Cambiamenti dominio/nome computer**, che contiene un messaggio di benvenuto. Fare clic su **OK**.

6.  Nella finestra di dialogo **Cambiamenti dominio/nome computer** viene visualizzato un messaggio per segnalare che è necessario riavviare il computer in modo da rendere effettive le modifiche. Fare clic su **OK**.

7.  Nella scheda **Nome computer** della finestra di dialogo **Proprietà del sistema** fare clic su **Chiudi**. Viene visualizzata la finestra di dialogo **Microsoft Windows**, che contiene un messaggio per segnalare nuovamente la necessità di riavviare il computer in modo da rendere effettive le modifiche. Fare clic su **Riavvia**.

> [!NOTE]
> Per informazioni su come aggiungere i computer che eseguono altri sistemi operativi Microsoft per il dominio, vedere [Appendice C - aggiunta di computer al dominio](#BKMK_C).

###### <a name="to-log-on-to-the-domain-using-computers-running-windows-server-2016"></a>Per accedere al dominio utilizzando computer che eseguono Windows Server 2016

1.  Disconnettersi o riavviare il computer.

2.  Premere CTRL+ALT+CANC. Viene visualizzata la schermata di accesso.

3.  Nell'angolo inferiore sinistro, fare clic su **altro utente**.

4.  In **nome utente**, digitare il nome utente.

5.  In **Password** digitare la password per il dominio, quindi fare clic sulla freccia o premere INVIO.

> [!NOTE]
> Per informazioni su come effettuare l'accesso al dominio utilizzando computer che eseguono altri sistemi operativi Microsoft, vedere [Appendice D - accesso al dominio](#BKMK_D).

#### <a name="BKMK_deployDHCP01"></a>Distribuzione di DHCP1
Prima di distribuire questo componente della rete core, è necessario eseguire le procedure seguenti:

-   Eseguire i passaggi nella sezione [configurazione di tutti i server](#BKMK_configuringAll).

-   Eseguire i passaggi nella sezione [aggiunta di computer Server al dominio ed effettuare l'accesso](#BKMK_joinlogserver).

Per distribuire DHCP1, ovvero il computer che esegue il ruolo server DHCP (Dynamic Host Configuration Protocol), è necessario completare questi passaggi nell'ordine seguente:

-   [Installa Dynamic Host Configuration Protocol (DHCP)](#BKMK_installDHCP)

-   [Creare e attivare un nuovo ambito DHCP](#BKMK_newscopeDHCP)

> [!NOTE]
> Per eseguire queste procedure mediante Windows PowerShell, aprire PowerShell e digitare i cmdlet seguenti su righe distinte, quindi premere INVIO. È inoltre necessario sostituire il nome dell'ambito, gli intervalli iniziale e finale degli indirizzi IP, la subnet mask e gli altri valori utilizzati in questo esempio con i valori che si desidera utilizzare.
>
> `Install-WindowsFeature DHCP -IncludeManagementTools`
>
> `Add-DhcpServerv4Scope -name "Corpnet" -StartRange 10.0.0.1 -EndRange 10.0.0.254 -SubnetMask 255.255.255.0 -State Active`
>
> `Add-DhcpServerv4ExclusionRange -ScopeID 10.0.0.0 -StartRange 10.0.0.1 -EndRange 10.0.0.15`
>
> `Set-DhcpServerv4OptionValue -OptionID 3 -Value 10.0.0.1 -ScopeID 10.0.0.0 -ComputerName DHCP1.corp.contoso.com`
>
> `Add-DhcpServerv4Scope -name "Corpnet2" -StartRange 10.0.1.1 -EndRange 10.0.1.254 -SubnetMask 255.255.255.0 -State Active`
>
> `Add-DhcpServerv4ExclusionRange -ScopeID 10.0.1.0 -StartRange 10.0.1.1 -EndRange 10.0.1.15`
>
> `Set-DhcpServerv4OptionValue -OptionID 3 -Value 10.0.1.1 -ScopeID 10.0.1.0 -ComputerName DHCP1.corp.contoso.com`
>
> `Set-DhcpServerv4OptionValue -DnsDomain corp.contoso.com -DnsServer 10.0.0.2`
>
> `Add-DhcpServerInDC -DnsName DHCP1.corp.contoso.com`

##### <a name="BKMK_installDHCP"></a>Installa Dynamic Host Configuration Protocol (DHCP)
Questa procedura consente di installare e configurare il ruolo server DHCP utilizzando Aggiunta guidata ruoli e funzionalità.

Per eseguire questa procedura è necessaria almeno l'appartenenza al gruppo **Domain Admins** oppure a un gruppo equivalente.

###### <a name="to-install-dhcp"></a>Per installare DHCP

1.  Su DHCP1, in Server Manager fare clic su **Gestisci** e quindi scegliere **Aggiungi ruoli e funzionalità**. Viene avviata l'Aggiunta guidata ruoli e funzionalità.

2.  In **Prima di iniziare** fare clic su **Avanti**.

    > [!NOTE]
    > La pagina **Prima di iniziare** dell'Aggiunta guidata ruoli e funzionalità non viene visualizzata se durante un'esecuzione precedente della procedura guidata è stata selezionata la casella di controllo **Ignora questa pagina per impostazione predefinita**.

3.  In **Select Installation Type**, assicurarsi che **installazione basata su ruoli o basata su funzionalità** sia selezionata e quindi fare clic su **Avanti**.

4.  In **Selezione server di destinazione** assicurarsi che sia selezionata l'opzione **Selezionare un server dal pool di server**. Assicurarsi che in **Pool di server** sia selezionato il computer locale. Fare clic su **Avanti**.

5.  In **Selezione ruoli Server**, in **ruoli**, selezionare **Server DHCP**. In **Aggiungere le funzionalità necessarie per Server DHCP** fare clic su **Aggiungi funzionalità**. Fare clic su **Avanti**.

6.  In **Selezione funzionalità** fare clic su **Avanti**, quindi in **Server DHCP** esaminare le informazioni fornite e quindi fare clic su **Avanti**.

7.  In **Conferma selezioni per l'installazione**, fare clic su **Riavvia automaticamente il server di destinazione se necessario**. Quando viene chiesto di confermare la selezione, fare clic su **Sì**, quindi fare clic su **installare**. Il **lo stato dell'installazione** pagina Visualizza stato durante il processo di installazione. Al termine del processo, il messaggio "configurazione necessaria. Installazione completata in *nomecomputer*"viene visualizzato, in cui *nomecomputer* è il nome del computer su cui è installato il Server DHCP. Nella finestra del messaggio fare clic su **Completa configurazione DHCP**. Si apre Configurazione guidata post-installazione DHCP. Fare clic su **Avanti**.

8.  In **Autorizzazione** specificare le credenziali che si desidera utilizzare per autorizzare il server DHCP in Servizi di dominio Active Directory, quindi fare clic su **Commit**. Al termine dell'autorizzazione fare clic su **Chiudi**.

##### <a name="BKMK_newscopeDHCP"></a>Creare e attivare un nuovo ambito DHCP
Questa procedura consente di creare un nuovo ambito DHCP utilizzando lo snap-in DHCP di Microsoft Management Console (MMC). Al termine della procedura, l'ambito viene attivato e l'intervallo di esclusione che viene creato impedisce al server DHCP di concedere in lease gli indirizzi IP che sono già stati utilizzati per configurare in modo statico i server e gli altri dispositivi che necessitano di un indirizzo IP statico.

Per eseguire questa procedura è necessaria almeno l'appartenenza al gruppo **DHCP Administrators** oppure a un gruppo equivalente.

###### <a name="to-create-and-activate-a-new-dhcp-scope"></a>Per creare e attivare un nuovo ambito DHCP

1.  Su DHCP1, in Server Manager, fare clic su **strumenti**, quindi fare clic su **DHCP**. Si apre lo snap-in MMC DHCP.

2.  In **DHCP**, espandere il nome del server. Ad esempio, se il nome del server DHCP è DHCP1, fare clic sulla freccia accanto a **DHCP1**.

3.  Sotto il nome del server, fare doppio clic su **IPv4**, quindi fare clic su **nuovo ambito**. Si apre Creazione guidata ambito.

4.  In **iniziale per la creazione guidata ambito**, fare clic su **Avanti**.

5.  In **nome ambito**, in **nome**, digitare un nome per l'ambito. Ad esempio, digitare **Subnet 1**.

6.  In **Descrizione** digitare una descrizione per il nuovo ambito e quindi fare clic su **Avanti**.

7.  In **intervallo di indirizzi IP**, eseguire le operazioni seguenti:

    1.  In **Indirizzo IP iniziale** digitare il primo indirizzo IP dell'intervallo, Ad esempio, digitare **10.0.0.1**.

    2.  In **Indirizzo IP finale** digitare l'ultimo indirizzo IP dell'intervallo. Ad esempio, digitare **10.0.0.254**. I valori del parametro **lunghezza** e **Subnet mask** vengono inseriti automaticamente in base all'indirizzo IP immesso per **indirizzo IP iniziale**.

    3.  Se necessario, modificare i valori di **Lunghezza** o **Subnet mask** nel modo appropriato per lo schema di indirizzamento.

    4.  Fare clic su **Avanti**.

8.  In **Aggiungi esclusioni**, eseguire le operazioni seguenti:

    1.  In **indirizzo IP iniziale**, digitare l'indirizzo IP che è il primo indirizzo IP dell'intervallo di esclusione. Ad esempio, digitare **10.0.0.1**.

    2.  In **indirizzo IP finale**, digitare l'indirizzo IP che è l'indirizzo IP ultimo indirizzo dell'intervallo di esclusione, ad esempio, digitare **10.0.0.15**.

9. Fare clic su **Aggiungi** e quindi su **Avanti**.

10. In **durata del Lease**, modificare i valori predefiniti per **giorni**, **ore**, e **minuti**, come appropriato per la rete e quindi fare clic su **Avanti**.

11. In **Configura opzioni DHCP** selezionare **Sì, configurare le opzioni adesso** e quindi fare clic su **Avanti**.

12. In **Router (gateway predefinito)** eseguire una delle operazioni seguenti:

    -   Se non è router sulla rete, fare clic su **Avanti**.

    -   In **Indirizzo IP** digitare l'indirizzo IP del router o del gateway predefinito. Ad esempio, digitare **10.0.0.1**. Fare clic su **Aggiungi** e quindi su **Avanti**.

13. In **nome di dominio e server DNS**, eseguire le operazioni seguenti:

    1.  In **Dominio padre** digitare il nome del dominio DNS utilizzato dai client per la risoluzione dei nomi. Ad esempio, digitare **corp.contoso.com**.

    2.  In **Nome server** digitare il nome del computer DNS utilizzato dai client per la risoluzione dei nomi. Ad esempio, digitare **DC1**.

    3.  Fare clic su **Risolvi**. L'indirizzo IP del server DNS viene aggiunto a **Indirizzo IP**. Fare clic su **Aggiungi**, attendere la convalida dell'indirizzo IP server DNS per il completamento e quindi fare clic su **Avanti**.

14. Dal momento che non sono presenti server WINS nella rete, in **Server WINS** fare clic su **Avanti**.

15. In **Attiva ambito**, selezionare **Sì, attiva l'ambito adesso**.

16. Fare clic su **Avanti**e quindi su **Fine**.

> [!IMPORTANT]
> Per creare nuovi ambiti per ulteriori subnet, ripetere questa procedura. Utilizzare un diverso intervallo di indirizzi IP per ogni subnet che si deve distribuire, assicurandosi che l'inoltro dei messaggi DHCP sia attivato su tutti i router che rimandano ad altre subnet.

### <a name="BKMK_joinlogclients"></a>Aggiunta di computer client al dominio e accesso

> [!NOTE]
> Per eseguire questa procedura mediante Windows PowerShell, aprire PowerShell e digitare il cmdlet seguente, quindi premere INVIO. È inoltre necessario sostituire il nome del dominio con il nome che si desidera utilizzare.
>
> `Add-Computer -DomainName corp.contoso.com`
>
> Al prompt, digitare il nome e la password per un account che dispone delle autorizzazioni necessarie per aggiungere un computer al dominio. Per riavviare il computer, digitare il comando seguente e premere INVIO.
>
> `Restart-Computer`

##### <a name="to-join-computers-running-windows-10-to-the-domain"></a>Per aggiungere i computer che esegue Windows 10 al dominio

1.  Accedere al computer con l'account Administrator locale.

2.  In **eseguire una ricerca web e Windows**, tipo **sistema**. Nei risultati della ricerca, fare clic su **sistema (Pannello di controllo)** . Viene visualizzata la finestra di dialogo **Sistema**.

3.  In **sistema**, fare clic su **impostazioni di sistema avanzate**. Il **le proprietà di sistema** verrà visualizzata la finestra di dialogo. Fare clic su di **nome Computer** scheda.

4.  In **nome Computer**, fare clic su **Modifica**. Il **Cambiamenti dominio/nome Computer** verrà visualizzata la finestra di dialogo.

5.  In **Cambiamenti dominio/nome Computer** , in **membro del**, fare clic su **dominio**, e quindi digitare il nome del dominio di cui si desidera aggiungere. Se ad esempio il nome del dominio è corp.contoso.com, digitare **corp.contoso.com**.

6.  Fare clic su **OK**. Il **la protezione di Windows** verrà visualizzata la finestra di dialogo.

7.  In **Cambiamenti dominio/nome Computer**, in **nome utente**, digitare il nome utente e in **Password**, digitare la password e quindi fare clic su **OK**. Viene visualizzata la finestra di dialogo **Cambiamenti dominio/nome computer**, che contiene un messaggio di benvenuto. Fare clic su **OK**.

8.  Nella finestra di dialogo **Cambiamenti dominio/nome computer** viene visualizzato un messaggio per segnalare che è necessario riavviare il computer in modo da rendere effettive le modifiche. Fare clic su **OK**.

9. Nella scheda **Nome computer** della finestra di dialogo **Proprietà del sistema** fare clic su **Chiudi**. Viene visualizzata la finestra di dialogo **Microsoft Windows**, che contiene un messaggio per segnalare nuovamente la necessità di riavviare il computer in modo da rendere effettive le modifiche. Fare clic su **Riavvia**.

##### <a name="to-join-computers-running-windows-81-to-the-domain"></a>Per aggiungere i computer che eseguono Windows 8.1 al dominio

1.  Accedere al computer con l'account Administrator locale.

2.  Fare doppio clic su **avviare**, quindi fare clic su **sistema**. Viene visualizzata la finestra di dialogo **Sistema**.

3.  In **sistema**, fare clic su **impostazioni di sistema avanzate**. Il **le proprietà di sistema** verrà visualizzata la finestra di dialogo. Fare clic su di **nome Computer** scheda.

4.  In **nome Computer**, fare clic su **Modifica**. Il **Cambiamenti dominio/nome Computer** verrà visualizzata la finestra di dialogo.

5.  In **Cambiamenti dominio/nome Computer** , in **membro del**, fare clic su **dominio**, e quindi digitare il nome del dominio di cui si desidera aggiungere. Se ad esempio il nome del dominio è corp.contoso.com, digitare **corp.contoso.com**.

6.  Fare clic su **OK**. Il **la protezione di Windows** verrà visualizzata la finestra di dialogo.

7.  In **Cambiamenti dominio/nome Computer**, in **nome utente**, digitare il nome utente e in **Password**, digitare la password e quindi fare clic su **OK**. Viene visualizzata la finestra di dialogo **Cambiamenti dominio/nome computer**, che contiene un messaggio di benvenuto. Fare clic su **OK**.

8.  Nella finestra di dialogo **Cambiamenti dominio/nome computer** viene visualizzato un messaggio per segnalare che è necessario riavviare il computer in modo da rendere effettive le modifiche. Fare clic su **OK**.

9. Nella scheda **Nome computer** della finestra di dialogo **Proprietà del sistema** fare clic su **Chiudi**. Viene visualizzata la finestra di dialogo **Microsoft Windows**, che contiene un messaggio per segnalare nuovamente la necessità di riavviare il computer in modo da rendere effettive le modifiche. Fare clic su **Riavvia**.

##### <a name="to-log-on-to-the-domain-using-computers-running-windows-10"></a>Per accedere al dominio utilizzando computer che esegue Windows 10

1.  Disconnettersi o riavviare il computer.

2.  Premere CTRL+ALT+CANC. Viene visualizzata la schermata di accesso.

3.  In basso a sinistra, fare clic su **altro utente**.

4.  In **nome utente**, digitare il nome di dominio e utente nel formato *dominio\utente*. Ad esempio, per accedere al dominio corp.contoso.com con un account denominato **utente-01**, tipo **corp\utente-01**.

5.  In **Password** digitare la password per il dominio, quindi fare clic sulla freccia o premere INVIO.

### <a name="BKMK_optionalfeatures"></a>Distribuzione di funzionalità facoltative per l'autenticazione dell'accesso alla rete e i servizi Web
Se si intende distribuire server di accesso alla rete, ad esempio punti di accesso wireless o server VPN, dopo l'installazione della rete core è consigliabile distribuire sia un server dei criteri di rete che un server Web. Per le distribuzioni di accesso alla rete, è consigliato l'utilizzo di metodi di autenticazione sicuri, basati sui certificati. Server dei criteri di rete consente di gestire i criteri di accesso alla rete e di distribuire metodi di autenticazione sicuri. È possibile utilizzare un server Web per pubblicare l'elenco di revoche di certificati (CRL) dell'autorità di certificazione (CA) che emette i certificati per l'autenticazione sicura.

> [!NOTE]
> È possibile distribuire i certificati server e altre funzionalità aggiuntive utilizzando le Guide complementari alla rete core. Per ulteriori informazioni, vedere [Risorse tecniche aggiuntive](#BKMK_resources).

Nella figura seguente è illustrata la topologia di rete di Windows Server Core con l'aggiuntivi dei criteri di RETE e dei server Web.

![Topologia di rete di Windows Server Core con l'aggiuntivi dei criteri di RETE e dei server Web](../media/Core-Network-Guide/cng16_overview_2.jpg)

Nelle sezioni seguenti sono disponibili informazioni sull'aggiunta dei Server dei criteri di rete e dei server Web alla rete.

-   [Distribuzione di NPS1](#BKMK_deployNPS1)

-   [Distribuzione di WEB1](#BKMK_IIS)

#### <a name="BKMK_deployNPS1"></a>Distribuzione di NPS1
Server dei criteri di rete viene installato come passaggio preliminare alla distribuzione di altre tecnologie di accesso alla rete, ad esempio server di rete privata virtuale (VPN), punti di accesso wireless e commutatori di autenticazione 802.1X.

Server dei criteri di rete consente di configurare e gestire centralmente i criteri di rete con le seguenti funzionalità: Server Remote Authentication Dial-In User Service (RADIUS) e proxy RADIUS.

Server dei criteri di rete è un componente facoltativo in una rete core, tuttavia è consigliabile installarlo in presenza di una qualsiasi delle condizioni seguenti:

-   Si prevede di espandere la rete per includere i server di accesso remoto compatibili con il protocollo RADIUS, ad esempio un computer che esegue Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2 o Windows Server 2008 e Servizio Routing e accesso remoto, gateway di Servizi terminal o Gateway Desktop remoto.


-   Si prevede di distribuire l'autenticazione 802.1 X per reti cablate o wireless access.

Prima di distribuire questo servizio ruolo, è necessario eseguire i passaggi seguenti nel computer che si sta configurando come server dei criteri di server.

-   Eseguire i passaggi nella sezione [configurazione di tutti i server](#BKMK_configuringAll).

-   Eseguire i passaggi nella sezione [aggiunta di computer Server al dominio ed effettuare l'accesso](#BKMK_joinlogserver)

Per distribuire il server NPS1, ovvero il computer che esegue il servizio ruolo Server dei criteri di rete del ruolo server Servizi di accesso e criteri di rete, è necessario completare il passaggio seguente:

-   [Pianificazione della distribuzione di NPS1](#bkmk_NetFndtn_Pln_NPS-01)

-   [Installare Server dei criteri di rete (NPS)](#BKMK_installNPS)

-   [Registrare il server dei criteri di dominio nel dominio predefinito](#BKMK_registerNPS)

> [!NOTE]
> Questa guida vengono fornite istruzioni per la distribuzione dei criteri di RETE in un server autonomo o macchina Virtuale denominata NPS1.  Un altro modello di distribuzione consigliato è l'installazione di criteri di RETE in un controller di dominio. Se si preferisce l'installazione dei criteri di RETE in un controller di dominio anziché in un server autonomo, installare il server NPS in DC1.

##### <a name="bkmk_NetFndtn_Pln_NPS-01"></a>Pianificazione della distribuzione di NPS1
Se si prevede di distribuire server di accesso alla rete, ad esempio punti di accesso wireless o server VPN, dopo la distribuzione della rete core è consigliabile distribuire Server dei criteri di rete.

Quando si utilizza Server dei criteri di rete come server RADIUS (Remote Authentication Dial-In User Service), il servizio Server dei criteri di rete esegue l'autenticazione e l'autorizzazione per le richieste di connessione ricevute tramite i server di accesso alla rete. Server dei criteri di rete consente inoltre di configurare e gestire centralmente i criteri di rete che regolano l'accesso degli utenti, stabilendo quali utenti possono accedere alla rete e quando.

Di seguito sono riportati i passaggi chiave per la pianificazione, che dovranno essere eseguiti prima di installare Server dei criteri di rete.

- Pianificare il database degli account utente. Per impostazione predefinita, se si aggiunge il server dei criteri di RETE a un dominio di Active Directory, dei criteri di RETE esegue l'autenticazione e autorizzazione tramite il database degli account utente di dominio Active Directory. In alcuni casi, ad esempio nelle reti di grandi dimensioni che utilizzano Server dei criteri di rete come proxy RADIUS per inoltrare le richieste di connessione ad altri server RADIUS, può essere necessario installare Server dei criteri di rete in un computer che non appartiene a nessun dominio.

- Pianificare l'accounting RADIUS. Server dei criteri di rete consente di registrare i dati di accounting in un database di SQL Server o in un file di testo nel computer locale. Se si desidera utilizzare le funzionalità di registrazione di SQL Server, pianificare l'installazione e la configurazione del server che esegue SQL Server.

##### <a name="BKMK_installNPS"></a>Installare Server dei criteri di rete (NPS)
È possibile utilizzare questa procedura per installare Server dei criteri di rete (NPS) tramite l'aggiunta guidata ruoli e funzionalità. Server dei criteri di rete è un servizio ruolo del ruolo server Servizi di accesso e criteri di rete.

> [!NOTE]
> Per impostazione predefinita, Server dei criteri di rete rimane in ascolto del traffico RADIUS sulle porte 1812, 1813, 1645 e 1646 di tutte le schede di rete installate. Se Windows Firewall con sicurezza avanzata è abilitata quando si installa NPS, le eccezioni del firewall per queste porte vengono create automaticamente durante il processo di installazione sia per il protocollo Internet versione 6 \(IPv6 @ no__t-1 che per il traffico IPv4. Se i server di accesso di rete sono configurati per l'invio di traffico RADIUS su porte diverse da quelle predefinite, rimuovere le eccezioni create durante l'installazione dei criteri di RETE in Windows Firewall con sicurezza avanzata e creare eccezioni per le porte utilizzate per il traffico RADIUS.

**Credenziali amministrative**

Per completare questa procedura, è necessario essere un membro del **Domain Admins** gruppo.

> [!NOTE]
> Per eseguire questa procedura mediante Windows PowerShell, aprire PowerShell e digitare il comando seguente, quindi premere INVIO.
>
> `Install-WindowsFeature NPAS -IncludeManagementTools`

###### <a name="to-install-nps"></a>Per installare Server dei criteri di rete

1.  Su NPS1, in Server Manager fare clic su **Gestisci** e quindi scegliere **Aggiungi ruoli e funzionalità**. Viene avviata l'Aggiunta guidata ruoli e funzionalità.

2.  In **Prima di iniziare** fare clic su **Avanti**.

    > [!NOTE]
    > La pagina **Prima di iniziare** dell'Aggiunta guidata ruoli e funzionalità non viene visualizzata se durante un'esecuzione precedente della procedura guidata è stata selezionata la casella di controllo **Ignora questa pagina per impostazione predefinita**.

3.  In **Select Installation Type**, assicurarsi che **installazione basata su ruoli o basata su funzionalità** sia selezionata e quindi fare clic su **Avanti**.

4.  In **Selezione server di destinazione** assicurarsi che sia selezionata l'opzione **Selezionare un server dal pool di server**. Assicurarsi che in **Pool di server** sia selezionato il computer locale. Fare clic su **Avanti**.

5.  In **Selezione ruoli server**, in **ruoli**, selezionare **servizi di accesso e criteri di rete**. Verrà visualizzata una finestra di dialogo in cui viene chiesto se aggiungere funzionalità richieste per servizi di accesso e criteri di rete. Fare clic su **Aggiungi funzionalità necessarie**e quindi su **Avanti**.

6.  In **Selezione funzionalità** fare clic su **Avanti**, quindi in **Servizi di accesso e criteri di rete** esaminare le informazioni fornite e quindi fare clic su **Avanti**.

7.  In **Selezione servizi ruolo** fare clic su **Server dei criteri di rete**.  In **aggiungere le funzionalità necessarie per Server dei criteri di rete**, fare clic su **Aggiungi funzionalità**. Fare clic su **Avanti**.

8.  In **Conferma selezioni per l'installazione**, fare clic su **Riavvia automaticamente il server di destinazione se necessario**. Quando viene chiesto di confermare la selezione, fare clic su **Sì**, quindi fare clic su **installare**. Nella pagina di stato dell'installazione viene visualizzato lo stato di avanzamento del processo di installazione. Al termine del processo, il messaggio "Installazione completata in *nomecomputer*" viene visualizzato, in cui *nomecomputer* è il nome del computer su cui è installato Server dei criteri di rete. Fare clic su **Chiudi**.

##### <a name="BKMK_registerNPS"></a>Registrare il server dei criteri di dominio nel dominio predefinito
È possibile utilizzare questa procedura per registrare un server dei criteri di dominio nel dominio in cui il server è un membro di dominio.

NPSs deve essere registrato in Active Directory in modo che disponga delle autorizzazioni per leggere le proprietà di connessione di account utente durante il processo di autorizzazione. La registrazione di un server dei criteri di gruppo aggiunge il server al gruppo di **server RAS e IAS** in Active Directory.

**Credenziali amministrative**

Per completare questa procedura, è necessario essere un membro del **Domain Admins** gruppo.

> [!NOTE]
> Per eseguire questa procedura utilizzando comandi della shell (Netsh) di rete all'interno di Windows PowerShell, aprire PowerShell e digitare il comando seguente e quindi premere INVIO.
>
> `netsh nps add registeredserver domain=corp.contoso.com server=NPS1.corp.contoso.com`

###### <a name="to-register-an-nps-in-its-default-domain"></a>Per registrare un server dei criteri di dominio nel dominio predefinito

1.  Su NPS1, in Server Manager, fare clic su strumenti e quindi fare clic su **Server dei criteri di rete**. Si apre lo snap-in MMC Server dei criteri di rete.

2.  Fare doppio clic su **dei criteri di RETE (locale)** , quindi fare clic su **Registra server in Active Directory**. Il **Server dei criteri di rete** verrà visualizzata la finestra di dialogo.

3.  In **Server dei criteri di rete**, fare clic su **OK**, quindi fare clic su **OK** nuovamente.

Per ulteriori informazioni su server dei criteri di rete, vedere [Server dei criteri di rete (NPS)](../technologies/nps/nps-top.md).

#### <a name="BKMK_IIS"></a>Distribuzione di WEB1

Il ruolo Server Web (IIS) in Windows Server 2016 fornisce una piattaforma protetta, semplice da gestire, modulare ed estensibile per l'hosting affidabile di siti web, servizi e applicazioni. Con Internet Information Services (IIS), è possibile condividere informazioni con utenti su Internet, intranet o extranet. IIS è una piattaforma web unificata che integra IIS, ASP.NET, servizi FTP, PHP e Windows Communication Foundation (WCF).

Oltre a consentire di pubblicare un CRL per l'accesso dal computer membri del dominio, il ruolo server Server Web (IIS) consente di configurare e gestire più siti web, applicazioni web e siti FTP. IIS fornisce anche i seguenti vantaggi:

-   Ottimizzazione della sicurezza Web grazie a un ridotto footprint del server e all'isolamento automatico delle applicazioni.

-   Distribuzione facilitata di applicazioni Web basate su ASP.NET, ASP classico e PHP sullo stesso server.

-   Isolamento delle applicazioni grazie a processi di lavoro che hanno un'identità univoca e una configurazione sandbox per impostazione predefinita, con un'ulteriore riduzione dei rischi per la sicurezza.

-   Aggiunta, rimozione e persino sostituzione facilitate di componenti IIS integrati con moduli personalizzati, adattati alle esigenze del cliente.

-   Aumento della velocità del sito Web grazie all'archiviazione nella cache dinamica integrata e alla compressione avanzata.

Per distribuire WEB1, ovvero il computer che eseguire il ruolo server Server Web (IIS), è necessario svolgere le procedure seguenti:

-   Eseguire i passaggi nella sezione [configurazione di tutti i server](#BKMK_configuringAll).

-   Eseguire i passaggi nella sezione [aggiunta di computer Server al dominio ed effettuare l'accesso](#BKMK_joinlogserver)

-   [Installare il ruolo server server Web (IIS)](#BKMK_install_IIS)

##### <a name="BKMK_install_IIS"></a>Installare il ruolo server server Web (IIS)
Per completare questa procedura, è necessario essere un membro del **amministratori** gruppo.

> [!NOTE]
> Per eseguire questa procedura mediante Windows PowerShell, aprire PowerShell e digitare il comando seguente, quindi premere INVIO.
>
> `Install-WindowsFeature Web-Server -IncludeManagementTools`

1.  In **Server Manager** fare clic su **Gestione**, quindi su **Aggiungi ruoli e funzionalità**. Viene avviata l'Aggiunta guidata ruoli e funzionalità.

2.  In **Prima di iniziare** fare clic su **Avanti**.

    > [!NOTE]
    > La pagina **Prima di iniziare** dell'Aggiunta guidata ruoli e funzionalità non viene visualizzata se durante un'esecuzione precedente della procedura guidata è stata selezionata la casella di controllo **Ignora questa pagina per impostazione predefinita**.

3.  Nel **Select Installation Type** pagina, fare clic su **Avanti**.

4.  Nel **server di destinazione** pagina, assicurarsi che il computer locale è selezionato, quindi fare clic su **Avanti**.

5.  Nel **Selezione ruoli server** pagina, scorrere e selezionare **Server Web (IIS)** . Il **aggiungere le funzionalità necessarie per Server Web (IIS)** verrà visualizzata la finestra di dialogo. Fare clic su **Aggiungi funzionalità necessarie**e quindi su **Avanti**.

6.  Fare clic su **Avanti** fino a quando non è stato accettato tutte l'impostazione predefinita le impostazioni del server web e quindi fare clic su **installare**.

7.  Verificare che tutte le installazioni hanno avuto esito positivo e quindi fare clic su **Chiudi**.

## <a name="BKMK_resources"></a>Risorse tecniche aggiuntive
Per ulteriori informazioni sulle tecnologie citate in questa guida, vedere le risorse seguenti:

 Risorse di documentazione tecnica di Windows Server 2012, Windows Server 2012 R2 e Windows Server 2016

-   [Novità di Active Directory Domain Services (AD DS) in Windows Server 2016](https://technet.microsoft.com/library/mt163897.aspx)

-   [Active Directory Domain Services panoramica](https://technet.microsoft.com/library/hh831484.aspx) https://technet.microsoft.com/library/hh831484.aspx.

-   [Panoramica di Domain Name System (DNS)](https://technet.microsoft.com/library/hh831667.aspx) in https://technet.microsoft.com/library/hh831667.aspx.

-   [Implementazione del ruolo DNS Admins](https://technet.microsoft.com/library/cc756152(WS.10).aspx)

-   [Panoramica di Dynamic Host Configuration Protocol (DHCP)](https://technet.microsoft.com/library/hh831825.aspx) in https://technet.microsoft.com/library/hh831825.aspx.

-   [Panoramica di servizi di accesso e criteri di rete](https://technet.microsoft.com/library/hh831683.aspx) in https://technet.microsoft.com/library/hh831683.aspx.

-   [Panoramica di server Web (IIS)](https://technet.microsoft.com/library/hh831725.aspx) in https://technet.microsoft.com/library/hh831725.aspx.

## <a name="BKMK_appendix"></a>Appendici da A A E
Nelle sezioni seguenti contengono informazioni di configurazione aggiuntive per i computer che eseguono sistemi operativi diversi da Windows Server 2016, Windows 10, Windows Server 2012 e Windows 8. Inoltre, viene fornito un foglio di lavoro di preparazione di rete per facilitare la distribuzione.

1.  [Appendice A-ridenominazione dei computer](#BKMK_A)

2.  [Appendice B-configurazione di indirizzi IP statici](#BKMK_B)

3.  [Appendice C-aggiunta di computer al dominio](#BKMK_C)

4.  [Appendice D-accesso al dominio](#BKMK_D)

5.  [Appendice E-foglio di preparazione alla pianificazione della rete core](#BKMK_E)

## <a name="BKMK_A"></a>Appendice A-ridenominazione dei computer
È possibile utilizzare le procedure descritte in questa sezione per fornire i computer che eseguono Windows Server 2008 R2, Windows 7, Windows Server 2008 e Windows Vista con un nome computer diverso.

-   [Windows Server 2008 R2 e Windows 7](#bkmk_NetFndtn_Pln_rename_R2)

-   [Windows Server 2008 e Windows Vista](#bkmk_NetFndtn_Pln_Renam08)

### <a name="bkmk_NetFndtn_Pln_rename_R2"></a>Windows Server 2008 R2 e Windows 7
L'appartenenza a **amministratori**, o equivalente è il requisito minimo necessario per eseguire queste procedure.

##### <a name="to-rename-computers-running-windows-server-2008-r2-and-windows-7"></a>Per rinominare computer che eseguono Windows Server 2008 R2 e Windows 7

1.  Fare clic sul pulsante **Start**, fare clic con il pulsante destro del mouse su **Computer**, quindi scegliere **Proprietà**. Viene visualizzata la finestra di dialogo **Sistema**.

2.  In **Impostazioni relative a nome computer, dominio e gruppo di lavoro** fare clic su **Cambia impostazioni**. Il **le proprietà di sistema** verrà visualizzata la finestra di dialogo.

    > [!NOTE]
    > Nei computer che eseguono Windows 7, prima di **le proprietà di sistema** verrà visualizzata la finestra di dialogo, il **controllo Account utente** verrà visualizzata la finestra di dialogo che richiede l'autorizzazione per continuare. Fare clic su **Continue** per continuare.

3.  Fare clic su **Cambia**. Il **Cambiamenti dominio/nome Computer** verrà visualizzata la finestra di dialogo.

4.  In **nome Computer**, digitare il nome del computer. Se ad esempio si desidera assegnare al computer il nome DC1, digitare **DC1**.

5.  Fare clic due volte su **OK**, fare clic su **Chiudi**, quindi su **Riavvia** per riavviare il computer.

### <a name="bkmk_NetFndtn_Pln_Renam08"></a>Windows Server 2008 e Windows Vista
L'appartenenza a **amministratori**, o equivalente è il requisito minimo necessario per eseguire queste procedure.

##### <a name="to-rename-computers-running-windows-server-2008-and-windows-vista"></a>Per rinominare computer che eseguono Windows Server 2008 e Windows Vista

1.  Fare clic sul pulsante **Start**, fare clic con il pulsante destro del mouse su **Computer**, quindi scegliere **Proprietà**. Viene visualizzata la finestra di dialogo **Sistema**.

2.  In **Impostazioni relative a nome computer, dominio e gruppo di lavoro** fare clic su **Cambia impostazioni**. Il **le proprietà di sistema** verrà visualizzata la finestra di dialogo.

    > [!NOTE]
    > Nei computer che eseguono Windows Vista, prima di **le proprietà di sistema** verrà visualizzata la finestra di dialogo, il **controllo Account utente** verrà visualizzata la finestra di dialogo che richiede l'autorizzazione per continuare. Fare clic su **Continue** per continuare.

3.  Fare clic su **Cambia**. Il **Cambiamenti dominio/nome Computer** verrà visualizzata la finestra di dialogo.

4.  In **nome Computer**, digitare il nome del computer. Se ad esempio si desidera assegnare al computer il nome DC1, digitare **DC1**.

5.  Fare clic due volte su **OK**, fare clic su **Chiudi**, quindi su **Riavvia** per riavviare il computer.

## <a name="BKMK_B"></a>Appendice B-configurazione di indirizzi IP statici
Le procedure illustrate in questa sezione consentono di configurare gli indirizzi IP statici in computer che eseguono i sistemi operativi seguenti:

-   [Windows Server 2008 R2](#bkmk_R2Cng_WS08R2IP)

-   [Windows Server 2008](#bkmk_NetFndtn_Pln_CfgStatic08)

### <a name="bkmk_R2Cng_WS08R2IP"></a>Windows Server 2008 R2
Per eseguire questa procedura è necessaria almeno l'appartenenza al gruppo **Administrators** oppure a un gruppo equivalente.

##### <a name="to-configure-a-static-ip-address-on-a-computer-running-windows-server-2008-r2"></a>Per configurare un indirizzo IP statico in un computer che esegue Windows Server 2008 R2

1.  Fare clic sul pulsante **Start** e quindi scegliere **Pannello di controllo**.

2.  In **Pannello di controllo** fare clic su **Rete e Internet**. Viene visualizzata la pagina **Rete e Internet**.

    In **Rete e Internet** fare clic su **Centro connessioni di rete e condivisione**. Viene visualizzata la pagina **Centro connessioni di rete e condivisione**.

3.  In **Centro connessioni di rete e condivisione** fare clic su **Modifica impostazioni scheda**. **Connessioni di rete** apre.

4.  In **connessioni di rete**, fare doppio clic su connessione di rete che si desidera configurare e quindi fare clic su **proprietà**.

5.  In **Proprietà connessione alla rete locale (LAN)** , in **La connessione utilizza gli elementi seguenti**, selezionare **Protocollo IP versione 4 (TCP/IPv4)** , quindi fare clic su **Proprietà**. Il **proprietà protocollo Internet versione 4 (TCP/IPv4)** verrà visualizzata la finestra di dialogo.

6.  In **proprietà protocollo Internet versione 4 (TCP/IPv4)** , via il **Generale** scheda, fare clic su **utilizzare il seguente indirizzo IP**. In **indirizzo IP**, digitare l'indirizzo IP che si desidera utilizzare.

7.  Premere TAB per posizionare il cursore in **Subnet mask**. Viene immesso automaticamente un valore predefinito per la subnet mask. Accettare la subnet mask predefinita oppure digitare la subnet mask che si desidera utilizzare.

8.  In **gateway predefinito**, digitare l'indirizzo IP del gateway predefinito.

9. In **server DNS preferito**, digitare l'indirizzo IP del server DNS. Se si prevede di utilizzare il computer locale come server DNS preferito, digitare l'indirizzo IP del computer locale.

10. In **Server DNS alternativo** digitare l'indirizzo IP del server DNS alternativo, se disponibile. Se si prevede di utilizzare il computer locale come server DNS alternativo, digitare l'indirizzo IP del computer locale.

11. Fare clic su **OK** e quindi fare clic su **Chiudi**.

### <a name="bkmk_NetFndtn_Pln_CfgStatic08"></a>Windows Server 2008
L'appartenenza a **amministratori**, o equivalente è il requisito minimo necessario per eseguire queste procedure.

##### <a name="to-configure-a-static-ip-address-on-a-computer-running-windows-server-2008"></a>Per configurare un indirizzo IP statico in un computer che esegue Windows Server 2008

1.  Fare clic sul pulsante **Start** e quindi scegliere **Pannello di controllo**.

2.  Nel **Pannello di controllo** verificare che sia selezionata la **Visualizzazione classica** e quindi fare doppio clic su **Centro connessioni di rete e condivisione**.

3.  In **Centro rete e condivisione**, in **attività**, fare clic su **Gestisci connessioni di rete**.

4.  In **connessioni di rete**, fare doppio clic su connessione di rete che si desidera configurare e quindi fare clic su **proprietà**.

5.  In **Proprietà connessione alla rete locale (LAN)** , in **La connessione utilizza gli elementi seguenti**, selezionare **Protocollo IP versione 4 (TCP/IPv4)** , quindi fare clic su **Proprietà**. Il **proprietà protocollo Internet versione 4 (TCP/IPv4)** verrà visualizzata la finestra di dialogo.

6.  In **proprietà protocollo Internet versione 4 (TCP/IPv4)** , via il **Generale** scheda, fare clic su **utilizzare il seguente indirizzo IP**. In **indirizzo IP**, digitare l'indirizzo IP che si desidera utilizzare.

7.  Premere TAB per posizionare il cursore in **Subnet mask**. Viene immesso automaticamente un valore predefinito per la subnet mask. Accettare la subnet mask predefinita oppure digitare la subnet mask che si desidera utilizzare.

8.  In **gateway predefinito**, digitare l'indirizzo IP del gateway predefinito.

9. In **server DNS preferito**, digitare l'indirizzo IP del server DNS. Se si prevede di utilizzare il computer locale come server DNS preferito, digitare l'indirizzo IP del computer locale.

10. In **Server DNS alternativo** digitare l'indirizzo IP del server DNS alternativo, se disponibile. Se si prevede di utilizzare il computer locale come server DNS alternativo, digitare l'indirizzo IP del computer locale.

11. Fare clic su **OK** e quindi fare clic su **Chiudi**.

## <a name="BKMK_C"></a>Appendice C-aggiunta di computer al dominio
È possibile utilizzare queste procedure per aggiungere i computer che esegue Windows Server 2008 R2, Windows 7, Windows Server 2008 e Windows Vista al dominio.

-   [Windows Server 2008 R2 e Windows 7](#BKMK_c1)

-   [Windows Server 2008 e Windows Vista](#BKMK_c2)

> [!IMPORTANT]
> Per aggiungere un computer a un dominio è necessario accedere al computer con l'account Administrator locale oppure, se l'accesso al computer viene effettuato con un account utente privo di credenziali amministrative per il computer locale, è necessario specificare le credenziali dell'account Administrator locale durante il processo di aggiunta del computer al dominio. È inoltre necessario disporre di un account utente nel dominio a cui si intende aggiungere il computer. Durante il processo di aggiunta del computer al dominio viene richiesta l'immissione delle credenziali (nome utente e password) di un account di dominio.

### <a name="BKMK_c1"></a>Windows Server 2008 R2 e Windows 7
L'appartenenza a **gli utenti del dominio**, o equivalente è il requisito minimo necessario per eseguire questa procedura.

##### <a name="to-join-computers-running-windows-server-2008-r2-and-windows-7-to-the-domain"></a>Per aggiungere i computer che eseguono Windows Server 2008 R2 e Windows 7 al dominio

1.  Accedere al computer con l'account Administrator locale.

2.  Fare clic sul pulsante **Start**, fare clic con il pulsante destro del mouse su **Computer**, quindi scegliere **Proprietà**. Viene visualizzata la finestra di dialogo **Sistema**.

3.  In **Impostazioni relative a nome computer, dominio e gruppo di lavoro** fare clic su **Cambia impostazioni**. Il **le proprietà di sistema** verrà visualizzata la finestra di dialogo.

    > [!NOTE]
    > Nei computer che eseguono Windows 7, prima di **le proprietà di sistema** verrà visualizzata la finestra di dialogo, il **controllo Account utente** verrà visualizzata la finestra di dialogo che richiede l'autorizzazione per continuare. Fare clic su **Continue** per continuare.

4.  Fare clic su **Cambia**. Il **Cambiamenti dominio/nome Computer** verrà visualizzata la finestra di dialogo.

5.  In **Nome computer** selezionare **Dominio** in **Membro di** e quindi digitare il nome del dominio a cui si desidera aggiungere il computer. Se ad esempio il nome del dominio è corp.contoso.com, digitare **corp.contoso.com**.

6.  Fare clic su **OK**. Il **la protezione di Windows** verrà visualizzata la finestra di dialogo.

7.  In **Cambiamenti dominio/nome Computer**, in **nome utente**, digitare il nome utente e in **Password**, digitare la password e quindi fare clic su **OK**. Viene visualizzata la finestra di dialogo **Cambiamenti dominio/nome computer**, che contiene un messaggio di benvenuto. Fare clic su **OK**.

8.  Nella finestra di dialogo **Cambiamenti dominio/nome computer** viene visualizzato un messaggio per segnalare che è necessario riavviare il computer in modo da rendere effettive le modifiche. Fare clic su **OK**.

9. Nella scheda **Nome computer** della finestra di dialogo **Proprietà del sistema** fare clic su **Chiudi**. Viene visualizzata la finestra di dialogo **Microsoft Windows**, che contiene un messaggio per segnalare nuovamente la necessità di riavviare il computer in modo da rendere effettive le modifiche. Fare clic su **Riavvia**.

### <a name="BKMK_c2"></a>Windows Server 2008 e Windows Vista
L'appartenenza a **gli utenti del dominio**, o equivalente è il requisito minimo necessario per eseguire questa procedura.

##### <a name="to-join-computers-running-windows-server-2008-and-windows-vista-to-the-domain"></a>Per aggiungere i computer che eseguono Windows Server 2008 e Windows Vista al dominio

1.  Accedere al computer con l'account Administrator locale.

2.  Fare clic sul pulsante **Start**, fare clic con il pulsante destro del mouse su **Computer**, quindi scegliere **Proprietà**. Viene visualizzata la finestra di dialogo **Sistema**.

3.  In **Impostazioni relative a nome computer, dominio e gruppo di lavoro** fare clic su **Cambia impostazioni**. Il **le proprietà di sistema** verrà visualizzata la finestra di dialogo.

4.  Fare clic su **Cambia**. Il **Cambiamenti dominio/nome Computer** verrà visualizzata la finestra di dialogo.

5.  In **Nome computer** selezionare **Dominio** in **Membro di** e quindi digitare il nome del dominio a cui si desidera aggiungere il computer. Se ad esempio il nome del dominio è corp.contoso.com, digitare **corp.contoso.com**.

6.  Fare clic su **OK**. Il **la protezione di Windows** verrà visualizzata la finestra di dialogo.

7.  In **Cambiamenti dominio/nome Computer**, in **nome utente**, digitare il nome utente e in **Password**, digitare la password e quindi fare clic su **OK**. Viene visualizzata la finestra di dialogo **Cambiamenti dominio/nome computer**, che contiene un messaggio di benvenuto. Fare clic su **OK**.

8.  Nella finestra di dialogo **Cambiamenti dominio/nome computer** viene visualizzato un messaggio per segnalare che è necessario riavviare il computer in modo da rendere effettive le modifiche. Fare clic su **OK**.

9. Nella scheda **Nome computer** della finestra di dialogo **Proprietà del sistema** fare clic su **Chiudi**. Viene visualizzata la finestra di dialogo **Microsoft Windows**, che contiene un messaggio per segnalare nuovamente la necessità di riavviare il computer in modo da rendere effettive le modifiche. Fare clic su **Riavvia**.

## <a name="BKMK_D"></a>Appendice D-accesso al dominio
È possibile utilizzare queste procedure per accedere al dominio utilizzando computer che eseguono Windows Server 2008 R2, Windows 7, Windows Server 2008 e Windows Vista.

-   [Windows Server 2008 R2 e Windows 7](#BKMK_d1)

-   [Windows Server 2008 e Windows Vista](#BKMK_d2)

### <a name="BKMK_d1"></a>Windows Server 2008 R2 e Windows 7
L'appartenenza a **gli utenti del dominio**, o equivalente è il requisito minimo necessario per eseguire questa procedura.

##### <a name="log-on-to-the-domain-using-computers-running-windows-server-2008-r2-and-windows-7"></a>Accedere al dominio utilizzando computer che eseguono Windows Server 2008 R2 e Windows 7

1.  Disconnettersi o riavviare il computer.

2.  Premere CTRL+ALT+CANC. Viene visualizzata la schermata di accesso.

3.  Fare clic su **Cambia utente** e quindi su **Altro utente**.

4.  In **nome utente**, digitare il nome di dominio e utente nel formato *dominio\utente*. Ad esempio, per accedere al dominio corp.contoso.com con un account denominato **utente-01**, tipo **corp\utente-01**.

5.  In **Password** digitare la password per il dominio, quindi fare clic sulla freccia o premere INVIO.

### <a name="BKMK_d2"></a>Windows Server 2008 e Windows Vista
L'appartenenza a **gli utenti del dominio**, o equivalente è il requisito minimo necessario per eseguire questa procedura.

##### <a name="log-on-to-the-domain-using-computers-running-windows-server-2008-and-windows-vista"></a>Accedere al dominio utilizzando computer che eseguono Windows Server 2008 e Windows Vista

1.  Disconnettersi o riavviare il computer.

2.  Premere CTRL+ALT+CANC. Viene visualizzata la schermata di accesso.

3.  Fare clic su **Cambia utente** e quindi su **Altro utente**.

4.  In **nome utente**, digitare il nome di dominio e utente nel formato *dominio\utente*. Ad esempio, per accedere al dominio corp.contoso.com con un account denominato **utente-01**, tipo **corp\utente-01**.

5.  In **Password** digitare la password per il dominio, quindi fare clic sulla freccia o premere INVIO.

## <a name="BKMK_E"></a>Appendice E-foglio di preparazione alla pianificazione della rete core
È possibile utilizzare questo foglio di preparazione alla pianificazione della rete per raccogliere le informazioni necessarie per installare una rete core. In questa sezione sono disponibili tabelle contenenti i singoli elementi di configurazione per ogni computer server per cui è necessario fornire informazioni o valori specifici durante il processo di installazione o configurazione. Per ogni elemento di configurazione sono riportati valori di esempio.

Ai fini della pianificazione e della registrazione, in ogni tabella sono disponibili spazi in cui è possibile inserire i valori utilizzati nella distribuzione. Se si specificano valori correlati alla sicurezza in queste tabelle, è consigliabile archiviare le informazioni in un luogo sicuro.

I collegamenti seguenti rimandano alle sezioni di questo argomento in cui sono illustrati gli elementi di configurazione e i valori di esempio associati alle procedure di distribuzione descritte in questa guida.

1.  [Installazione di Active Directory Domain Services e DNS](#BKMK_FndtnPrep_InstallAD)

    -   [Configurazione di una zona DNS di ricerca inversa](#BKMK_FndtnPrep_DNSRevrsLook)

2.  [Installazione di DHCP](#BKMK_FndtnPrep_InstallDHCP)

    -   [Creazione di un intervallo di esclusione in DHCP](#BKMK_FndtnPrep_DHCP_Exclusn)

    -   [Creazione di un nuovo ambito DHCP](#bkmk_NetFndtn_Pln_DHCP_NewScope)

3.  [Installazione del server dei criteri di rete (facoltativo)](#BKMK_FndtnPrep_InstallNPS)

### <a name="BKMK_FndtnPrep_InstallAD"></a>Installazione di Active Directory Domain Services e DNS
Le tabelle in questa sezione elencano gli elementi di configurazione per la preinstallazione e installazione di servizi di dominio Active Directory (AD DS) e DNS.

##### <a name="pre-installation-configuration-items-for-ad-ds-and-dns"></a>Elementi di configurazione di preinstallazione per Active Directory e DNS
Gli elementi di configurazione di pre-installazione elenco nelle tabelle seguenti, come descritto in [configurazione di tutti i server](#BKMK_configuringAll):

-   [Configurare un indirizzo IP statico](#BKMK_ip)

|Elementi di configurazione|Valori di esempio|Valori|
|-----------------------|------------------|----------|
|L'indirizzo IP|10.0.0.2||
|Subnet mask|255.255.255.0||
|Gateway predefinito|10.0.0.1||
|Server DNS preferito|127.0.0.1||
|Server DNS alternativo|10.0.0.15||

-   [Rinominare il computer](#BKMK_rename)

|Elemento di configurazione|Valore di esempio|Value|
|----------------------|-----------------|---------|
|Il nome del computer|DC1||

##### <a name="ad-ds-and-dns-installation-configuration-items"></a>Elementi di configurazione di installazione di Active Directory e DNS
Elementi di configurazione per la procedura di distribuzione di rete di Windows Server Core [installare Active Directory e DNS per una nuova foresta](#BKMK_installAD-DNS):

|Elementi di configurazione|Valori di esempio|Valori|
|-----------------------|------------------|----------|
|Nome DNS completo|corp.contoso.com||
|Livello di funzionalità della foresta|Windows Server 2003||
|Percorso della cartella per il database di Servizi di dominio Active Directory|E:\Configuration @ no__t-0<br /><br />In alternativa accettare il percorso predefinito.||
|Percorso della cartella per i file di registro di Servizi di dominio Active Directory|E:\Configuration @ no__t-0<br /><br />In alternativa accettare il percorso predefinito.||
|Percorso della cartella SYSVOL di Servizi di dominio Active Directory|E:\Configuration @ no__t-0<br /><br />In alternativa accettare il percorso predefinito.||
|Password di amministratore modalità ripristino servizi directory|J*p2leO4$F||
|Nome file di risposte (facoltativo)|DS_AnswerFile di Active Directory||

#### <a name="BKMK_FndtnPrep_DNSRevrsLook"></a>Configurazione di una zona DNS di ricerca inversa

|Elementi di configurazione|Valori di esempio|Valori|
|-----------------------|------------------|----------|
|Tipo di zona:|-Zona principale<br />-Zona secondaria<br />: Zona di stub||
|Tipo di zona<br /><br />**Archivia la zona in Active Directory**|-Selezionato<br />-Non selezionata||
|Ambito di replica zona Active Directory|-Per tutti i server DNS della foresta<br />-Per tutti i server DNS nel dominio<br />-Per tutti i controller di dominio nel dominio<br />-Per tutti i controller di dominio specificati nell'ambito di questa partizione di directory||
|Nome della zona di ricerca inversa<br /><br />(tipo IP)|: Zona di ricerca inversa IPv4<br />: Zona di ricerca inversa IPv6||
|Nome della zona di ricerca inversa<br /><br />(ID rete)|10.0.0||

### <a name="BKMK_FndtnPrep_InstallDHCP"></a>Installazione di DHCP
Nelle tabelle riportate in questa sezione sono elencati gli elementi di configurazione per le procedure di preinstallazione e installazione di DHCP.

##### <a name="pre-installation-configuration-items-for-dhcp"></a>Elementi di configurazione per la preinstallazione di DHCP
Gli elementi di configurazione di pre-installazione elenco nelle tabelle seguenti, come descritto in [configurazione di tutti i server](#BKMK_configuringAll):

-   [Configurare un indirizzo IP statico](#BKMK_ip)

|Elementi di configurazione|Valori di esempio|Valori|
|-----------------------|------------------|----------|
|L'indirizzo IP|10.0.0.3||
|Subnet mask|255.255.255.0||
|Gateway predefinito|10.0.0.1||
|Server DNS preferito|10.0.0.2||
|Server DNS alternativo|10.0.0.15||

-   [Rinominare il computer](#BKMK_rename)

|Elemento di configurazione|Valore di esempio|Value|
|----------------------|-----------------|---------|
|Il nome del computer|DHCP1||

##### <a name="dhcp-installation-configuration-items"></a>Elementi di configurazione per l'installazione di DHCP
Gli elementi di configurazione per la procedura di distribuzione di una rete core basata su Windows Server sono illustrati in [Installazione di DHCP (Dynamic Host Configuration Protocol)](#BKMK_installDHCP):

|Elementi di configurazione|Valori di esempio|Valori|
|-----------------------|------------------|----------|
|Binding connessioni di rete|Ethernet||
|Impostazioni server DNS|DC1||
|Indirizzo IP del server DNS preferito|10.0.0.2||
|Indirizzo IP del server DNS alternativo|10.0.0.15||
|Nome ambito|Corp1||
|Indirizzo IP iniziale|10.0.0.1||
|Indirizzo IP finale|10.0.0.254||
|Subnet mask|255.255.255.0||
|Gateway predefinito (facoltativo)|10.0.0.1||
|Durata del lease|8 giorni||
|Modalità operativa server DHCP IPv6|Non abilitato||

#### <a name="BKMK_FndtnPrep_DHCP_Exclusn"></a>Creazione di un intervallo di esclusione in DHCP
Elementi di configurazione per creare un intervallo di esclusione durante la creazione di un ambito in DHCP.

|Elementi di configurazione|Valori di esempio|Valori|
|-----------------------|------------------|----------|
|Nome ambito|Corp1||
|Descrizione dell'ambito|Subnet 1 sede||
|Indirizzo IP iniziale dell'intervallo di esclusione|10.0.0.1||
|Indirizzo IP finale dell'intervallo di esclusione|10.0.0.15||

#### <a name="bkmk_NetFndtn_Pln_DHCP_NewScope"></a>Creazione di un nuovo ambito DHCP
Elementi di configurazione per la procedura di distribuzione di rete di Windows Server Core [creare e attivare un nuovo ambito DHCP](#BKMK_newscopeDHCP):

|Elementi di configurazione|Valori di esempio|Valori|
|-----------------------|------------------|----------|
|Nome del nuovo ambito|Corp2||
|Descrizione dell'ambito|Subnet dell'ufficio principale 2||
|(Intervallo di indirizzi IP)<br /><br />Indirizzo IP iniziale|10.0.1.1||
|(Intervallo di indirizzi IP)<br /><br />Indirizzo IP finale|10.0.1.254||
|Lunghezza|8||
|Subnet mask|255.255.255.0||
|Indirizzo IP iniziale dell'intervallo di esclusione|10.0.1.1||
|Indirizzo IP finale dell'intervallo di esclusione|10.0.1.15||
|Durata del lease<br /><br />Days<br /><br />Ore<br /><br />Minuti|-8<br />-   0<br />-   0||
|Router (gateway predefinito)<br /><br />L'indirizzo IP|10.0.1.1||
|Dominio padre DNS|corp.contoso.com||
|Server DNS<br /><br />L'indirizzo IP|10.0.0.2||

### <a name="BKMK_FndtnPrep_InstallNPS"></a>Installazione del server dei criteri di rete (facoltativo)
Nelle tabelle riportate in questa sezione sono elencati gli elementi di configurazione per le procedure di preinstallazione e installazione di Server dei criteri di rete.

##### <a name="pre-installation-configuration-items"></a>Elementi di configurazione per la preinstallazione
Le seguenti tre tabelle elencano gli elementi di configurazione pre-installazione come descritto in [configurazione di tutti i server](#BKMK_configuringAll):

-   [Configurare un indirizzo IP statico](#BKMK_ip)

|Elementi di configurazione|Valori di esempio|Valori|
|-----------------------|------------------|----------|
|L'indirizzo IP|10.0.0.4||
|Subnet mask|255.255.255.0||
|Gateway predefinito|10.0.0.1||
|Server DNS preferito|10.0.0.2||
|Server DNS alternativo|10.0.0.15||

-   [Rinominare il computer](#BKMK_rename)

|Elemento di configurazione|Valore di esempio|Value|
|----------------------|-----------------|---------|
|Il nome del computer|NPS1||

##### <a name="network-policy-server-installation-configuration-items"></a>Elementi di configurazione per l'installazione di Server dei criteri di rete
Gli elementi di configurazione per le procedure di distribuzione del server dei criteri di rete di Windows Server Core [installano server dei criteri di rete](#BKMK_installNPS) e [registrano il server dei criteri di rete nel dominio predefinito](#BKMK_registerNPS)

-   Per installare e registrare Server dei criteri di rete non sono necessari ulteriori elementi di configurazione.

