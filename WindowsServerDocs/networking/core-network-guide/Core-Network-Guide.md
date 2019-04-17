---
title: Guida alla rete core
description: Questa guida vengono fornite istruzioni su come pianificare e distribuire i componenti di base necessari per una rete completamente funzionante e un nuovo dominio di Active Directory in una nuova foresta con Windows Server 2016
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: b3cd60f7-d380-4712-9a78-0a8f551e1121
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 90390a7b975bc358fb96715d23fe542bc261220e
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/28/2018
---
# <a name="core-network-guide"></a>Guida alla rete core

>Si applica a: Windows Server (canale annuale e virgola), Windows Server 2016

Questa guida fornisce istruzioni su come pianificare e distribuire i componenti di base necessari per una rete completamente funzionante e un nuovo dominio di Active Directory in una nuova foresta.

> [!NOTE]
> Questa guida è disponibile per il download in formato Microsoft Word dalla TechNet Gallery. Per ulteriori informazioni, vedere [Guida alla rete Core per Windows Server 2016](https://gallery.technet.microsoft.com/Core-Network-Guide-for-9da2e683).

Questa guida contiene le sezioni seguenti.

- [Informazioni sulla Guida](#BKMK_about)

- [Panoramica sulle reti core](#BKMK_overview)

- [Pianificazione di una rete core](#BKMK_planning)

- [Distribuzione di rete core](#BKMK_deployment)

- [Risorse tecniche aggiuntive](#BKMK_resources)

- [Appendici A-E](#BKMK_appendix)

## <a name="BKMK_about"></a>Informazioni sulla Guida
Questa guida è progettata per gli amministratori di rete e di sistema che devono installare una nuova rete o che desiderano creare una rete basata su dominio per sostituire una rete costituita da gruppi di lavoro. Lo scenario di distribuzione fornito in questa guida è particolarmente utile se si prevede di dover aggiungere ulteriori funzionalità e servizi di rete in futuro.

È consigliabile leggere progettazione e distribuzione Guide per ognuna delle tecnologie utilizzate in questo scenario di distribuzione al fine di verificare se in questa guida fornisce i servizi e le impostazioni di configurazione.

Una rete core è una raccolta di hardware di rete, dispositivi, e deve software che fornisce i servizi essenziali per l'IT dell'organizzazione (IT).

Una rete core di Windows Server offre numerosi vantaggi, inclusi i seguenti.

-   Protocolli di base per la connettività di rete tra computer e altri dispositivi compatibili Protocol/Internet Protocol (TCP). TCP/IP è una suite di protocolli standard per la connessione di computer e la creazione di reti. TCP/IP è il software dei protocolli di rete fornito con i sistemi operativi Microsoft Windows che implementano e supportano la suite di protocolli TCP/IP.

-   Dynamic Host Configuration Protocol (DHCP) automatica assegnazione di indirizzi IP ai computer e altri dispositivi che sono configurati come client DHCP. Configurazione manuale degli indirizzi IP in tutti i computer nella rete è richiede molto tempo ed è meno flessibile rispetto all'assegnazione dinamica di computer e altri dispositivi con configurazioni di indirizzi IP tramite un server DHCP.

-   Servizio di risoluzione dei nome Domain Name System (DNS). DNS consente agli utenti, computer, applicazioni e servizi trovare gli indirizzi IP dei computer e dispositivi nella rete utilizzando il nome di dominio completo del computer o dispositivo.

-   Un insieme di strutture, ovvero uno o più domini di Active Directory che condividono il definizioni di classi e attributi (schema), le informazioni del sito e di replica (configurazione) e le funzionalità di ricerca a livello di foresta (catalogo globale).

-   Un dominio radice della foresta, ovvero il primo dominio creato in una nuova foresta. I gruppi Enterprise Admins e Schema Admins, che sono gruppi amministrativi a livello di foresta, si trovano nel dominio radice della foresta. Inoltre, un dominio radice della foresta, come con altri domini, è una raccolta di computer, utente e gruppo oggetti definiti dall'amministratore in servizi di dominio Active Directory (AD DS). Tali oggetti condividono un comune directory database e criteri di sicurezza. Possono inoltre condividere relazioni di sicurezza con altri domini eventualmente aggiunti come alla crescita dell'organizzazione. Il servizio directory, inoltre, vengono archiviati i dati di directory e consente di computer autorizzati, applicazioni e utenti di accedere ai dati.

-   Un database degli account utente e computer. Il servizio directory fornisce un database degli account utente centralizzata che consente di creare account utente e computer per utenti e computer che sono autorizzati a connettersi alla rete di accesso rete e risorse, ad esempio applicazioni, database, file condivisi e le cartelle e stampanti.

Una rete core consente anche di ridimensionare la rete come alla crescita dell'organizzazione e i requisiti IT cambiano. Ad esempio, con una rete core è possibile aggiungere domini, subnet IP, servizi di accesso remoto, servizi wireless e altre funzionalità e ruoli server offerti da Windows Server 2016.

### <a name="network-hardware-requirements"></a>Requisiti hardware di rete
Per distribuire correttamente una rete core, è necessario distribuire hardware di rete, inclusi i seguenti:

-   Ethernet, Fast Ethernet o Gigabyte Ethernet cavi

-   Un hub, livello 2 o 3 commutatore, router o altro dispositivo che esegue la funzione di inoltrare il traffico di rete tra computer e dispositivi.

-   Computer che soddisfano i requisiti hardware minimi per i rispettivi sistemi operativi client e server.

## <a name="what-this-guide-does-not-provide"></a>Cosa non fornisce questa Guida
Questa Guida non fornisce istruzioni per la distribuzione dei componenti seguenti:

-   Hardware di rete, ad esempio cavi, router, commutatori e hub

-   Altre risorse di rete, ad esempio stampanti e file server

-   Connettività Internet

-   Accesso remoto

-   Accesso wireless

-   Distribuzione computer client

> [!NOTE]
> I computer che eseguono sistemi operativi client Windows configurati per impostazione predefinita per ricevere lease di indirizzi IP dal server DHCP. Pertanto, nessun aggiuntive DHCP o protocollo Internet versione 4 (IPv4) non è necessaria una configurazione di computer client.

## <a name="technology-overviews"></a>Panoramiche delle tecnologie
Nelle sezioni seguenti vengono forniscono brevi descrizioni generali delle tecnologie necessarie distribuite per creare una rete core.

### <a name="active-directory-domain-services"></a>Servizi di dominio Active Directory
Una directory è una struttura gerarchica che archivia le informazioni sugli oggetti di rete, ad esempio utenti e computer. Un servizio di directory, ad esempio servizi di dominio Active Directory, fornisce i metodi per l'archiviazione dei dati di directory e renderli disponibili per gli amministratori e gli utenti della rete. Ad esempio, di dominio Active Directory archivia le informazioni sugli account utente, inclusi nomi, indirizzi di posta elettronica, password e numeri di telefono e consente ad altri utenti autorizzati nella stessa rete accedere a queste informazioni.

### <a name="dns"></a>DNS
DNS è un protocollo di risoluzione nome per le reti TCP/IP, ad esempio Internet o rete di un'organizzazione. Un server DNS ospita informazioni che consentono ai computer client e servizi di risolvere facilmente riconosciuto, i nomi DNS alfanumerici per gli indirizzi IP utilizzati dai computer per comunicare tra loro.

### <a name="dhcp"></a>DHCP
DHCP è uno standard IP per semplificare la gestione della configurazione IP dell'host. Lo standard DHCP prevede l'utilizzo di server DHCP come un modo per gestire l'allocazione dinamica degli indirizzi IP e altri dettagli relativi alla configurazione per client DHCP sulla rete.

DHCP consente di utilizzare un server DHCP per assegnare dinamicamente un indirizzo IP a un computer o un altro dispositivo, ad esempio una stampante, della rete locale. In ogni computer in una rete TCP/IP deve essere un indirizzo IP univoco, poiché l'indirizzo IP e il subnet mask correlata identificano sia il computer host e la subnet a cui è collegato al computer. Mediante il protocollo DHCP, è possibile verificare che tutti i computer che sono configurati come client DHCP ricevano un indirizzo IP appropriato per il percorso di rete e subnet e utilizzando le opzioni DHCP, ad esempio gateway predefinito e i server DNS, è possibile fornire automaticamente i client DHCP con le informazioni necessarie per funzionare correttamente nella rete.

Per le reti basate su TCP/IP, il protocollo DHCP riduce la complessità e la quantità di lavoro amministrativo riconfigurazione dei computer.

### <a name="tcpip"></a>TCP/IP
TCP/IP in Windows Server 2016 è la seguente:

-   Software di rete basato su protocolli di rete standard del settore.

-   Protocollo di rete instradabile enterprise che supporta la connessione del computer basato su Windows dalla rete locale (LAN) sia gli ambienti di wide area network (WAN).

-   Tecnologie di base e le utilità per la connessione di computer basati su Windows con sistemi di tipo diverso al fine di condividere informazioni.

-   Una base per l'accesso a servizi Internet globali, ad esempio i server World Wide Web e FTP File Transfer Protocol ().

-   Un framework client/server multipiattaforma, scalabile e affidabile.

TCP/IP include utilità TCP/IP di base che consentono ai computer basati su Windows di connettersi e condividere informazioni con altri sistemi Microsoft e non Microsoft, tra cui:

-    Windows Server 2016

-   Windows 10

-    Windows Server 2012 R2

-   Windows 8.1

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

-   Tablet e telefoni cellulari con Ethernet cablate o wireless 802.11 abilitato

## <a name="BKMK_overview"></a>Panoramica sulle reti core
Nella figura seguente è illustrata la topologia di rete di Windows Server Core.

![Topologia di rete di Windows Server Core](../media/Core-Network-Guide/cng16_overview.jpg)

> [!NOTE]
> Questa guida include inoltre istruzioni per l'aggiunta di server di Server dei criteri di rete (NPS) e Server Web (IIS) facoltativi per la topologia di rete per gettare le basi per soluzioni di accesso di rete protetta, ad esempio 802.1 X cablate e wireless distribuzioni che possono essere implementate utilizzando le guide dei componenti di base rete complementare. Per ulteriori informazioni, vedere [distribuzione di funzionalità facoltative per l'autenticazione di accesso di rete e servizi Web](#BKMK_optionalfeatures).

### <a name="core-network-components"></a>Componenti di base rete
Di seguito sono i componenti di una rete core.

##### <a name="router"></a>Router
Questa Guida alla distribuzione vengono fornite istruzioni per la distribuzione di una rete core con due subnet separate da un router che ha abilitato l'inoltro DHCP. È possibile, tuttavia, distribuire un commutatore livello 2, un commutatore di livello 3 o un hub, a seconda dei requisiti e risorse. Se si distribuisce un commutatore, il commutatore deve essere in grado di inoltro DHCP oppure è necessario inserire un server DHCP in ogni subnet. Se si distribuisce un hub, si distribuisce una singola subnet e non è necessario l'inoltro DHCP o un secondo ambito nel server DHCP.

##### <a name="static-tcpip-configurations"></a>Configurazioni TCP/IP statiche
I server nella distribuzione illustrata sono configurati con indirizzi IPv4 statici. I computer client sono configurati per impostazione predefinita per ricevere lease di indirizzi IP dal server DHCP.

##### <a name="active-directory-domain-services-global-catalog-and-dns-server-dc1"></a>Catalogo globale di Active Directory servizi di dominio e server DNS DC1
Entrambi i servizi di dominio Active Directory (AD DS) e del sistema DNS (Domain Name) installate nel server denominato DC1, che consente di directory e servizi di risoluzione per tutti i computer e dispositivi nella rete.

##### <a name="dhcp-server-dhcp1"></a>Server DHCP DHCP1
Il server DHCP, denominato DHCP1, è configurato con un ambito che fornisce lease di indirizzi IP (Internet Protocol) ai computer nella subnet locale. Il server DHCP può anche essere configurato con ambiti aggiuntivi per fornire lease di indirizzi IP ai computer di altre subnet, se nei router è configurato l'inoltro DHCP.

##### <a name="client-computers"></a>Computer client
I computer che eseguono sistemi operativi client Windows configurati per impostazione predefinita come client DHCP, che ottenere gli indirizzi IP e le opzioni DHCP automaticamente dal server DHCP.

## <a name="BKMK_planning"></a>Pianificazione di una rete core
Prima di distribuire una rete core, è necessario pianificare gli elementi seguenti.

-   [Pianificazione delle subnet](#bkmk_NetFndtn_Pln_Subnt)

-   [Pianificazione della configurazione di base di tutti i server](#bkmk_NetFndtn_Pln_AllSrvrs)

-   [Pianificazione della distribuzione di DC1](#bkmk_NetFndtn_Pln_AD-DNS-01)

-   [Pianificazione di accesso al dominio](#bkmk_NetFndtn_Pln_DomAccess)

-   [Pianificazione della distribuzione di DHCP1](#bkmk_NetFndtn_Pln_DHCP-01)

Le sezioni seguenti forniscono ulteriori dettagli su ognuno di questi elementi.

> [!NOTE]
> Per ulteriori informazioni sulla pianificazione della distribuzione, vedere anche [appendice E - foglio di preparazione alla pianificazione della rete Core](#BKMK_E).

### <a name="bkmk_NetFndtn_Pln_Subnt"></a>Pianificazione delle subnet
In rete Protocol/Internet Protocol (TCP), vengono utilizzati i router per interconnettere componenti hardware e software utilizzati su segmenti di rete fisiche diverse subnet. I router consentono inoltre di inoltrare pacchetti IP tra ogni subnet. Definire il layout fisico della rete, compresi il numero di router e subnet, che è necessario, prima di procedere con le istruzioni riportate in questa Guida.

Inoltre, per configurare il server sulla rete con indirizzi IP statici, è necessario determinare l'intervallo di indirizzi IP che si desidera utilizzare per la subnet in cui si trovano i server della rete core. In questa Guida, gli intervalli di indirizzi IP privati 10.0.0.1 - 10.0.0.254 e 10.0.1.1 - 10.0.1.254 vengono utilizzati come esempi, ma è possibile utilizzare qualsiasi intervallo di indirizzi IP privati che preferisci.

> [!IMPORTANT]
> Dopo aver selezionato gli intervalli di indirizzi IP che si desidera utilizzare per ogni subnet, assicurarsi che il router sia configurato con un indirizzo IP dallo stesso intervallo di indirizzi IP utilizzati dalla subnet in cui è installato il router. Ad esempio, se il router è configurato per impostazione predefinita con un indirizzo IP di 192.168.1.1, ma si sta installando il router in una subnet con un intervallo di indirizzi IP di 10.0.0.0/24, è necessario riconfigurare il router per l'utilizzo di un indirizzo IP nell'intervallo di indirizzi IP 10.0.0.0/24.

Indirizzo IP privato intervalli specificati dal Internet Request for Comments (RFC) 1918 riconosciuti i seguenti:

-   10.0.0.0 - 10.255.255.255

-   172.16.0.0 - 172.31.255.255

-   192.168.0.0 - 192.168.255.255

Quando si utilizzano gli intervalli di indirizzi IP privati specificati nella RFC 1918, è possibile connettersi direttamente a Internet tramite un indirizzo IP privato perché le richieste di passare a o da questi indirizzi vengono automaticamente scartate dai router del provider del servizio Internet. Per aggiungere la connettività Internet alla rete core in un secondo momento, è necessario sottoscrivere un contratto con un ISP per ottenere un indirizzo IP pubblico.

> [!IMPORTANT]
> Quando si utilizzano indirizzi IP privati, è necessario utilizzare un tipo di proxy server o network address translation (NAT) per convertire gli intervalli di indirizzi IP privati della rete locale in un indirizzo IP pubblico che è possibile instradare in Internet. La maggior parte dei router supporta i servizi NAT, pertanto dovrebbe essere facile selezionare un router con funzionalità NAT.

Per ulteriori informazioni, vedere [pianificazione della distribuzione di DHCP1](#bkmk_NetFndtn_Pln_DHCP-01).

### <a name="bkmk_NetFndtn_Pln_AllSrvrs"></a>Pianificazione della configurazione di base di tutti i server
Per ogni server nella rete principale, è necessario rinominare il computer e assegnare e configurare un indirizzo IPv4 statico e altre proprietà TCP/IP per il computer.

#### <a name="planning-naming-conventions-for-computers-and-devices"></a>Pianificazione delle convenzioni di denominazione per computer e dispositivi
Per garantire la coerenza della rete, è consigliabile utilizzare nomi coerenti per server, stampanti e altri dispositivi. I nomi dei computer può essere utilizzati per aiutare gli utenti e amministratori a identificare facilmente lo scopo e la posizione del server, stampante o altro dispositivo. Ad esempio, se si dispone di tre server DNS, uno a San Francisco, uno a Los Angeles e uno a Chicago, è possibile utilizzare la convenzione di denominazione *funzione server*-*percorso*-*numero*:

-   DNS-DEN-01. Questo nome rappresenta il server DNS a Denver, Colorado. Se il server DNS aggiuntivi vengono aggiunti a Denver, può essere incrementato il valore numerico nel nome, ad esempio DNS-DEN-02 e DNS-DEN-03.

-   STABILIMENTI TERMALI-DNS-01. Questo nome rappresenta il server DNS a South Pasadena, California.

-   ORL-DNS-01. Questo nome rappresenta il server DNS a Orlando, Florida.

In questa Guida, la convenzione di denominazione server è molto semplice e include la funzione di server primario e un numero. Ad esempio, il controller di dominio è denominato DC1 e il server DHCP è denominato DHCP1.

È consigliabile scegliere una convenzione di denominazione prima di installare della rete core utilizzando questa Guida.

#### <a name="planning-static-ip-addresses"></a>Pianificazione degli indirizzi IP statici
Prima di configurare ogni computer con un indirizzo IP statico, è necessario pianificare le subnet e intervalli di indirizzi IP. Inoltre, è necessario determinare gli indirizzi IP dei server DNS. Se si prevede di installare un router che consente di accedere ad altre reti, ad esempio altre subnet o a Internet, è necessario conoscere l'indirizzo IP del router, denominato anche un gateway predefinito, per la configurazione di indirizzo IP statico.

La tabella seguente fornisce i valori di esempio per la configurazione di indirizzo IP statico.

|Elementi di configurazione|Valori di esempio|
|-----------------------|------------------|
|Indirizzo IP|10.0.0.2|
|La subnet mask|255.255.255.0|
|Gateway predefinito (indirizzo IP Router)|10.0.0.1|
|Server DNS preferito|10.0.0.2|

> [!NOTE]
> Se prevede di distribuire più di un server DNS, è possibile pianificare anche l'indirizzo IP del Server DNS alternativo.

### <a name="bkmk_NetFndtn_Pln_AD-DNS-01"></a>Pianificazione della distribuzione di DC1
Di seguito sono i passaggi di pianificazione chiave prima di installare servizi di dominio Active Directory (AD DS) e DNS in DC1.

#### <a name="planning-the-name-of-the-forest-root-domain"></a>Pianificazione del nome del dominio radice della foresta
Un primo passaggio nel processo di progettazione di Active Directory consiste nel determinare il numero di foreste necessario nell'organizzazione. Un insieme di strutture è il contenitore di dominio Active Directory di livello superiore e costituito da uno o più domini che condividono uno schema comune e un catalogo globale. Un'organizzazione può includere più foreste, ma per la maggior parte delle organizzazioni, un'architettura a foresta singola preferita e più semplice da amministrare.

Quando si crea il primo controller di dominio all'interno dell'organizzazione, si sta creando il primo dominio (anche denominato dominio radice della foresta) e la prima foresta. Prima di effettuare questa operazione utilizzando questa Guida, tuttavia, è necessario determinare il nome di dominio ottimale per l'organizzazione. Nella maggior parte dei casi, il nome dell'organizzazione viene utilizzato come il nome di dominio e in molti casi questo nome di dominio è registrato. Se si prevede di distribuire esterna Internet basato su server Web per fornire informazioni e servizi ai propri clienti o partner, scegli un nome di dominio che non sia già in uso e quindi registrare il nome di dominio in modo che diventi proprietà dell'organizzazione.

#### <a name="planning-the-forest-functional-level"></a>Il livello di funzionalità di pianificazione
Durante l'installazione di servizi di dominio Active Directory, è necessario scegliere il livello di funzionalità foresta che si desidera utilizzare. Funzionalità del dominio e foresta, introdotta in Windows Server 2003 Active Directory offre un modo per abilitare la funzionalità di Active Directory all'interno dell'ambiente di rete a dominio o foresta wide. Diversi livelli di funzionalità del dominio e funzionalità della foresta sono disponibili, a seconda dell'ambiente.

Funzionalità della foresta Abilita le funzionalità in tutti i domini della foresta. Sono disponibili i seguenti livelli di funzionalità foresta:

-    Windows Server 2008. Questo livello di funzionalità foresta supporta solo controller di dominio che eseguono Windows Server 2008 e versioni successive del sistema operativo Windows Server.

-    Windows Server 2008 R2. Questo livello di funzionalità foresta supporta controller di dominio di Windows Server 2008 R2 e controller di dominio che eseguono versioni successive del sistema operativo Windows Server.

-    Windows Server 2012. Questo livello di funzionalità foresta supporta controller di dominio di Windows Server 2012 e controller di dominio che eseguono versioni successive del sistema operativo Windows Server.

-    Windows Server 2012 R2. Questo livello di funzionalità foresta supporta controller di dominio di Windows Server 2012 R2 e controller di dominio che eseguono versioni successive del sistema operativo Windows Server.

-    Windows Server 2016. Questo livello di funzionalità foresta supporta solo controller di dominio di Windows Server 2016 e controller di dominio che eseguono versioni successive del sistema operativo Windows Server.

Se si distribuisce un nuovo dominio in una nuova foresta e tutti i controller di dominio verrà eseguito Windows Server 2016, è consigliabile configurare Active Directory con il livello di funzionalità foresta Windows Server 2016 durante l'installazione di Active Directory.

> [!IMPORTANT]
> Dopo il livello di funzionalità foresta, i controller di dominio che eseguono sistemi operativi precedenti non possono essere inseriti nella foresta. Ad esempio, se si aumenta il livello di funzionalità della foresta a Windows Server 2016, i controller di dominio che eseguono Windows Server 2012 R2 o Windows Server 2008 non possono essere aggiunti alla foresta.

Nella tabella seguente vengono forniti gli elementi di configurazione per Active Directory.

|Elementi di configurazione:|Valori di esempio:|
|------------------------|-------------------|
|Nome DNS completo|Esempi:<br /><br />-corp.contoso.com<br />-example.com|
|Livello di funzionalità foresta|-Windows Server 2008 <br />-Windows Server 2008 R2 <br />-Windows Server 2012 <br />-Windows Server 2012 R2 <br />-Windows Server 2016|
|Percorso della cartella Active Directory Domain Services Database|E:\Configuration\\<br /><br />Oppure accettare il percorso predefinito.|
|Percorso della cartella file di registro di Active Directory Domain Services|E:\Configuration\\<br /><br />Oppure accettare il percorso predefinito.|
|Percorso della cartella Active Directory SYSVOL di servizi di dominio|E:\Configuration\\<br /><br />Oppure accettare il percorso predefinito|
|Password di amministratore modalità ripristino di directory|**J\ * p2leO4$ F**|
|Nome file di risposte (facoltativo)|**Ds_answerfile di Active Directory**|

#### <a name="planning-dns-zones"></a>Pianificazione delle zone DNS
Nei server DNS primario, integrato in Active Directory, una zona di ricerca diretta viene creata per impostazione predefinita durante l'installazione del ruolo Server DNS. Zona di ricerca diretta consente a computer e dispositivi per eseguire una query per l'indirizzo IP del computer o del dispositivo in base al nome DNS. Oltre a una zona di ricerca diretta, è consigliabile creare una zona di ricerca inversa DNS. Con un DNS inverse query, un computer o dispositivo possono individuare il nome di un altro computer o dispositivo tramite il relativo indirizzo IP. Distribuire una zona di ricerca inversa migliora in genere le prestazioni di DNS e aumenta notevolmente il successo delle query DNS.

Quando si crea una zona di ricerca inversa, il dominio in-addr.arpa, che è definito negli standard DNS ed è riservato nello spazio dei nomi DNS Internet per fornire un modo pratico e affidabile per l'esecuzione delle query inverse, è configurato in DNS. Per creare lo spazio dei nomi inverso, vengono formati alcuni sottodomini dominio in-addr.arpa utilizzando l'ordine inverso dei numeri nella notazione decimale puntata degli indirizzi IP.

Il dominio in-addr.arpa si applica a tutte le reti TCP/IP che si basano sul protocollo Internet versione 4 (IPv4) addressing. La creazione guidata nuova zona presuppone che si utilizza questo dominio quando si crea una nuova zona di ricerca inversa.

Mentre si esegue la creazione guidata nuova zona, sono consigliate le seguenti selezioni:

|Elementi di configurazione|Valori di esempio|
|-----------------------|------------------|
|Tipo di zona|**Zona primaria**, e **archivia la zona in Active Directory** è selezionata|
|Ambito di replica zona Active Directory|**Per tutti i server DNS nel dominio**|
|Pagina della procedura guidata nome prima zona di ricerca inversa|**Zona di ricerca inversa IPv4**|
|Pagina della procedura guidata Nome zona di ricerca inversa secondo|ID rete = 10.0.0.|
|Aggiornamenti dinamici|**Consentire solo aggiornamenti dinamici protetti**|

### <a name="bkmk_NetFndtn_Pln_DomAccess"></a>Pianificazione di accesso al dominio
Per accedere al dominio, il computer deve essere un computer membro del dominio e l'account utente deve essere creato in servizi di dominio Active Directory prima di tentare l'accesso.

> [!NOTE]
> Singoli computer che eseguono Windows hanno un locale utenti e gruppi account database che viene chiamato il database degli account utente di un account di protezione (SAM). Quando si crea un account utente nel computer locale nel database SAM, è possibile accedere al computer locale, ma non effettuare l'accesso a un dominio. Account utente di dominio vengono creati con la Active Directory Users e computer di Microsoft Management Console (MMC) in un controller di dominio, non con gli utenti locali e i gruppi nel computer locale.

Dopo il primo accesso riuscito con credenziali di dominio, le impostazioni di accesso restano invariate finché non viene rimosso il computer dal dominio o le impostazioni di accesso vengono modificate manualmente.

Prima di accedere al dominio:

-   Creare account utente in Active Directory Users and Computers. Ogni utente deve avere un account utente di servizi di dominio Active Directory in Active Directory Users and Computers. Per ulteriori informazioni, vedere [creare un Account utente in Active Directory Users and Computers](#BKMK_createUA).

-   Verificare la configurazione dell'indirizzo IP corretta. Per aggiungere un computer al dominio, il computer deve disporre di un indirizzo IP. In questa Guida, i server sono configurati con indirizzi IP statici e i computer client ricevono lease di indirizzi IP dal server DHCP. Per questo motivo, il server DHCP deve essere distribuito prima di aggiungere client al dominio. Per ulteriori informazioni, vedere [distribuzione di DHCP1](#BKMK_deployDHCP01).

-   Aggiungere il computer al dominio. Qualsiasi computer che forniscono o accedono a risorse di rete devono essere aggiunti al dominio. Per ulteriori informazioni, vedere [aggiunta di computer Server al dominio ed effettuare l'accesso](#BKMK_joinlogserver) e [aggiunta di computer Client al dominio ed effettuare l'accesso](#BKMK_joinlogclients).

### <a name="bkmk_NetFndtn_Pln_DHCP-01"></a>Pianificazione della distribuzione di DHCP1
Di seguito sono i passaggi di pianificazione chiave prima di installare il ruolo server DHCP su DHCP1.

#### <a name="planning-dhcp-servers-and-dhcp-forwarding"></a>Pianificazione dei server DHCP e l'inoltro DHCP
Poiché i messaggi DHCP sono messaggi trasmessi, non vengono inoltrati fra le subnet dai router. Se si dispone di più subnet e si desidera fornire il servizio DHCP per ogni subnet, è necessario eseguire una delle operazioni seguenti:

-   Installare un server DHCP in ogni subnet

-   Configurare i router per l'inoltro dei messaggi trasmessi DHCP tra le subnet e configurare più ambiti nel server DHCP, un ambito per ogni subnet.

Nella maggior parte dei casi, configurare i router per l'inoltro dei messaggi trasmessi DHCP è più conveniente della distribuzione di un server DHCP in ogni segmento fisico della rete.

#### <a name="planning-ip-address-ranges"></a>Pianificazione di intervalli di indirizzi IP
Ogni subnet deve disporre di un proprio intervallo di indirizzi IP univoco. Tali intervalli sono rappresentati in un server DHCP con ambiti.

Un ambito è un raggruppamento amministrativo di indirizzi IP per i computer in una subnet che utilizzano il servizio DHCP. L'amministratore crea innanzitutto un ambito per ogni subnet fisica e quindi Usa l'ambito per definire i parametri utilizzati dai client.

Un ambito ha le proprietà seguenti:

-   Un intervallo di indirizzi IP da cui si desidera includere o escludere gli indirizzi utilizzati per offerte di lease del servizio DHCP.

-   Una subnet mask, che determina il prefisso di subnet per un determinato indirizzo IP.

-   Un nome di ambito assegnato quando viene creato.

-   Valori di durata del lease, che vengono assegnati ai client DHCP che ricevono gli indirizzi IP allocati dinamicamente.

-   Le opzioni di ambito DHCP configurate per l'assegnazione ai client DHCP, ad esempio l'indirizzo IP del server DNS e indirizzo IP del gateway predefinito al router.

-   Le prenotazioni possono essere utilizzate per garantire che un client DHCP riceva sempre lo stesso indirizzo IP.

Prima di distribuire i server, specificare le subnet e l'intervallo di indirizzi IP che si desidera utilizzare per ogni subnet.

#### <a name="planning-subnet-masks"></a>Pianificazione delle subnet mask
ID di rete e ID all'interno di un indirizzo IP dell'host vengono distinti applicando una subnet mask. Ogni subnet mask è un numero a 32 bit che utilizza gruppi di bit consecutivi di tutti gli uno (1) per identificare la rete ID e da soli zero (0) per identificare le parti di ID host di un indirizzo IP.

Ad esempio, la subnet mask normalmente utilizzata con l'indirizzo IP 131.107.16.200 è il seguente numero binario a 32 bit:

```
11111111 11111111 00000000 00000000
```

Questo numero di subnet mask è 16 uno (1) seguiti da 16 bit zero, che indica che le sezioni di ID host e ID rete dell'indirizzo IP sono entrambi 16 bit. In genere, questa subnet mask viene visualizzata nella notazione decimale come 255.255.0.0.

Nella tabella seguente mostra le subnet mask per le classi di indirizzi Internet.

|Classe di indirizzo|BITS per la subnet mask|La subnet mask|
|-----------------|------------------------|---------------|
|Classe A|11111111 00000000 00000000 00000000|255.0.0.0|
|Classe B|11111111 11111111 00000000 00000000|255.255.0.0|
|Classe C|11111111 11111111 11111111 00000000|255.255.255.0|

Quando si crea un ambito in DHCP e si immette l'intervallo di indirizzi IP per l'ambito, DHCP fornisce queste subnet mask predefinite. In genere, subnet mask predefinite sono accettabili per la maggior parte delle reti senza requisiti particolari e in ogni segmento di rete IP corrisponde a una singola rete fisica.

In alcuni casi, è possibile utilizzare subnet mask personalizzate per implementare l'utilizzo di subnet IP. Con la subnet IP, è possibile suddividere la parte di ID host predefinita di un indirizzo IP per specificare le subnet, che sono suddivisioni dell'ID di rete basata su classe originale.

Personalizzando la lunghezza della subnet mask, è possibile ridurre il numero di bit che vengono utilizzati per l'ID host effettivo.

Per evitare problemi di indirizzamento e routing, è necessario assicurarsi che tutti i computer TCP/IP in un segmento di rete utilizzino la stessa subnet mask e che ogni computer o dispositivo ha un indirizzo IP univoco.

#### <a name="planning-exclusion-ranges"></a>Pianificazione degli intervalli di esclusione
Quando si crea un ambito in un server DHCP, specificare un intervallo di indirizzi IP che include tutti gli indirizzi IP che il server DHCP è autorizzato a concedere in lease ai client DHCP, ad esempio computer e altri dispositivi. Se si passa quindi e configurare manualmente alcuni server e altri dispositivi con static indirizzi IP dallo stesso intervallo di indirizzi IP che utilizza il server DHCP, è possibile creare accidentalmente un conflitto di indirizzi IP, entrambi in cui il server DHCP avere assegnato lo stesso indirizzo IP a dispositivi diversi.

Per risolvere questo problema, è possibile creare un intervallo di esclusione per l'ambito DHCP. Intervallo di esclusione è un contigui intervallo di indirizzi IP all'interno di intervallo di indirizzi IP dell'ambito che il server DHCP non è consentito utilizzare. Se si crea un intervallo di esclusione, il server DHCP non assegnare gli indirizzi in tale intervallo e consente di assegnare manualmente tali indirizzi senza creare un conflitto di indirizzi IP.

È possibile escludere gli indirizzi IP dalla distribuzione dal server DHCP mediante la creazione di un intervallo di esclusione per ogni ambito. È necessario utilizzare esclusioni per tutti i dispositivi che sono configurati con un indirizzo IP statico. Gli indirizzi esclusi devono includere tutti gli indirizzi IP assegnati manualmente ad altri server, i client non DHCP, workstation senza dischi o i client di Routing e accesso remoto e PPP.

È consigliabile configurare l'intervallo di esclusione con alcuni indirizzi supplementari per supportare l'espansione futura della rete. Nella tabella seguente fornisce un intervallo di esclusione di esempio per un ambito con un intervallo di indirizzi IP di 10.0.0.1 - 10.0.0.254 e una subnet mask 255.255.255.0.

|Elementi di configurazione|Valori di esempio|
|-----------------------|------------------|
|Indirizzo IP iniziale dell'intervallo di esclusione|10.0.0.1|
|Indirizzo IP finale dell'intervallo di esclusione|10.0.0.25|

#### <a name="planning-tcpip-static-configuration"></a>Pianificazione della configurazione di TCP/IP statici
Alcuni dispositivi, ad esempio router, server DHCP e server DNS, devono essere configurati con un indirizzo IP statico. Inoltre, potrebbe essere altri dispositivi, ad esempio stampanti, che si desidera abbiano sempre lo stesso indirizzo IP. Elencare i dispositivi che si desidera configurare in modo statico per ogni subnet e quindi pianificare l'intervallo di esclusione che si desidera utilizzare nel server DHCP per verificare che il server DHCP non assegni in lease l'indirizzo IP di un dispositivo configurato in modo statico. Intervallo di esclusione è una sequenza limitata di indirizzi IP all'interno di un ambito esclusi dalle offerte del servizio DHCP. Gli intervalli di esclusione assicurano che tutti gli indirizzi in questi intervalli non sono disponibili dal server per i client DHCP sulla rete.

Ad esempio, se l'intervallo di indirizzi IP per una subnet è 192.168.0.1 tramite 192.168.0.254 e sono presenti dieci dispositivi che si desidera configurare con un indirizzo IP statico, è possibile creare un intervallo di esclusione per il 192.168.0. *x* ambito contenente dieci o più indirizzi IP: 192.168.0.1 tramite 192.168.0.15.

In questo esempio dieci degli indirizzi IP esclusi usi per configurare i server e altri dispositivi con indirizzi IP statici e altri cinque indirizzi IP restano a disposizione per configurazione statica di nuovi dispositivi che si desidera aggiungere in futuro. Con questo intervallo di esclusione, il server DHCP viene lasciato con un pool di indirizzi di 192.168.0.16 tramite 192.168.0.254.

Nella tabella seguente vengono forniti gli elementi di configurazione di esempio aggiuntive per Active Directory e DNS.

|Elementi di configurazione|Valori di esempio|
|-----------------------|------------------|
|Binding connessioni di rete|Ethernet|
|Impostazioni del Server DNS|DC1.corp.contoso.com|
|Indirizzo IP del server DNS preferito|10.0.0.2|
|Aggiungere valori per la finestra di dialogo ambito<br /><br />1. Nome ambito<br />2. Indirizzo IP iniziale<br />3. Indirizzo IP finale<br />4. Subnet Mask<br />5. Gateway predefinito (facoltativo)<br />6. Durata del lease|1. Subnet principale<br />2.  10.0.0.1<br />3.  10.0.0.254<br />4.  255.255.255.0<br />5.  10.0.0.1<br />6. 8 giorni|
|Modalità operativa di Server DHCP IPv6|Non è abilitato|

## <a name="BKMK_deployment"></a>Distribuzione di rete core
Per distribuire una rete core, i passaggi di base sono i seguenti:

1.  [Configurazione di tutti i server](#BKMK_configuringAll)

2.  [Distribuzione di DC1](#BKMK_deployADDNS01)

3.  [Aggiunta di computer Server al dominio e accesso](#BKMK_joinlogserver)

4.  [Distribuzione di DHCP1](#BKMK_deployDHCP01)

5.  [Aggiunta di computer Client al dominio e accesso](#BKMK_joinlogclients)

6.  [Distribuzione di funzionalità facoltative per l'autenticazione di accesso di rete e servizi Web](#BKMK_optionalfeatures)

> [!NOTE]
> -   Per la maggior parte delle procedure descritte in questa guida vengono forniti i comandi di Windows PowerShell equivalenti. Prima di eseguire questi cmdlet in Windows PowerShell, sostituire i valori di esempio con i valori appropriati per la distribuzione di rete. Inoltre, è necessario immettere ogni cmdlet su una singola riga in Windows PowerShell. In questa Guida, i singoli cmdlet potrebbe essere visualizzato su più righe a causa di vincoli e la visualizzazione del documento di formattazione da browser o un'altra applicazione.
> -   Le procedure descritte in questa Guida non includono istruzioni per i casi in cui il **controllo dell'Account utente** viene visualizzata la finestra di dialogo per richiedere l'autorizzazione a continuare. Se viene visualizzata questa finestra di dialogo mentre si siano eseguendo le procedure in questa Guida, se è stata aperta la finestra di dialogo in risposta alle azioni dell'utente, fare clic su **continua**.

### <a name="BKMK_configuringAll"></a>Configurazione di tutti i server
Prima di installare altre tecnologie, ad esempio servizi di dominio Active Directory o DHCP, è importante configurare gli elementi seguenti.

-   [Rinominare il computer](#BKMK_rename)

-   [Configurare un indirizzo IP statico](#BKMK_ip)

È possibile utilizzare le sezioni seguenti per eseguire queste azioni per ogni server.

Appartenenza al gruppo **amministratori**, o equivalente è il requisito minimo necessario per eseguire queste procedure.

#### <a name="BKMK_rename"></a>Rinominare il computer
In questa sezione è possibile utilizzare la procedura per modificare il nome di un computer. La ridenominazione del computer è utile per i casi in cui il sistema operativo abbia creato automaticamente un nome di computer che non si desidera utilizzare.

> [!NOTE]
> Per eseguire questa procedura mediante Windows PowerShell, aprire PowerShell e digitare i cmdlet seguenti su righe distinte, quindi premere INVIO. È inoltre necessario sostituire *ComputerName* con il nome che si desidera utilizzare.
>
> `Rename-Computer`*ComputerName*
>
> `Restart-Computer`

###### <a name="to-rename-computers-running-windows-server-2016-windows-server-2012-r2-and-windows-server-2012"></a>Per rinominare computer che eseguono Windows Server 2016, Windows Server 2012 R2 e Windows Server 2012

1.  In Server Manager, fare clic su **Server locale**. Il computer **proprietà** vengono visualizzati nel riquadro dei dettagli.

2.  In **proprietà**, in **nome Computer**, fare clic sul nome di computer esistenti. Il **proprietà del sistema** apre la finestra di dialogo. Fare clic su **modifica**. Il **cambiamenti dominio/nome Computer** apre la finestra di dialogo.

3.  Nel **cambiamenti dominio/nome Computer** della finestra di dialogo **nome Computer**, digitare un nuovo nome per il computer. Ad esempio, se si desidera che il nome computer DC1, digitare **DC1**.

4.  Fare clic su **OK** due volte, quindi fare clic su **Chiudi**. Se si desidera riavviare immediatamente il computer per completare la modifica del nome, fare clic su **Riavvia ora**. In caso contrario, fare clic su **riavvia in seguito**.

> [!NOTE]
> Per informazioni su come rinominare computer che eseguono altri sistemi operativi Microsoft, vedere [appendice A - ridenominazione dei computer](#BKMK_A).

#### <a name="BKMK_ip"></a>Configurare un indirizzo IP statico
È possibile utilizzare le procedure in questo argomento per configurare il protocollo Internet versione 4 (IPv4) le proprietà della connessione di rete con un indirizzo IP statico di indirizzi per i computer che eseguono Windows Server 2016.

> [!NOTE]
> Per eseguire questa procedura mediante Windows PowerShell, aprire PowerShell e digitare i cmdlet seguenti su righe distinte, quindi premere INVIO. È inoltre necessario sostituire i nomi di interfaccia e gli indirizzi IP in questo esempio con i valori che si desidera utilizzare per configurare il computer.
>
> `New-NetIPAddress -IPAddress 10.0.0.2 -InterfaceAlias "Ethernet" -DefaultGateway 10.0.0.1 -AddressFamily IPv4 -PrefixLength 24`
>
> `Set-DnsClientServerAddress -InterfaceAlias "Ethernet" -ServerAddresses 127.0.0.1`

###### <a name="to-configure-a-static-ip-address-on-computers-running-windows-server-2016-windows-server-2012-r2-and-windows-server-2012"></a>Per configurare un indirizzo IP statico in computer che eseguono Windows Server 2016, Windows Server 2012 R2 e Windows Server 2012

1.  Nella barra delle applicazioni, fare doppio clic sull'icona della rete e quindi fare clic su **centro aprire rete e condivisione**.

2.  In **centro rete e condivisione**, fare clic su **modificare le impostazioni della scheda**. Il **connessioni di rete** cartella viene aperto e visualizza le connessioni di rete disponibili.

3.  In **connessioni di rete**, fare clic sulla connessione che si desidera configurare, quindi fare clic su **proprietà**. La connessione di rete **proprietà** apre la finestra di dialogo.

4.  Nella connessione di rete **proprietà** della finestra di dialogo **la connessione utilizza gli elementi seguenti**selezionare **Internet Protocol versione 4 (TCP/IPv4)**, quindi fare clic su **proprietà**. Il **proprietà protocollo IP versione 4 (TCP/IPv4)** apre la finestra di dialogo.

5.  In **proprietà protocollo IP versione 4 (TCP/IPv4)**via il **generale** scheda, fare clic su **utilizzare il seguente indirizzo IP**. In **indirizzo IP**, digitare l'indirizzo IP che si desidera utilizzare.

6.  Premere tab per posizionare il cursore **Subnet mask**. Un valore predefinito per la subnet mask viene immesso automaticamente. Accettare la subnet mask predefinita oppure digitare la subnet mask che si desidera utilizzare.

7.  In **gateway predefinito**, digitare l'indirizzo IP del gateway predefinito.

    > [!NOTE]
    > È necessario configurare **gateway predefinito** con lo stesso indirizzo IP che usi nell'interfaccia di rete locale (LAN) del router. Ad esempio, se si dispone di un router che è connesso a una rete wide area network (WAN), ad esempio Internet nonché LAN, configurare l'interfaccia LAN con lo stesso indirizzo IP che verrà specificato come il **gateway predefinito**. In un altro esempio, se si dispone di un router che è connesso a due reti LAN, dove LAN A utilizza l'indirizzo intervallo 10.0.0.0/24 e LAN B utilizza l'indirizzo intervallo 192.168.0.0/24, configurare l'indirizzo IP del router LAN A con un indirizzo compreso nell'intervallo di indirizzi, ad esempio 10.0.0.1. Inoltre, nell'ambito DHCP per questo intervallo di indirizzi, configurare **gateway predefinito** con l'indirizzo IP 10.0.0.1. Per LAN B, configurare l'interfaccia di router LAN B con un indirizzo compreso nell'intervallo di indirizzi corrispondente, ad esempio 192.168.0.1 e quindi configurare la LAN B ambito 192.168.0.0/24 con un **gateway predefinito** valore 192.168.0.1.

8.  In **server DNS preferito**, digitare l'indirizzo IP del server DNS. Se si prevede di utilizzare il computer locale come server DNS preferito, digitare l'indirizzo IP del computer locale.

9. In **Server DNS alternativo**, digitare l'indirizzo IP del server DNS alternativo, se disponibile. Se si prevede di utilizzare il computer locale come un server DNS alternativo, digitare l'indirizzo IP del computer locale.

10. Fare clic su **OK**, quindi fare clic su **Chiudi**.

> [!NOTE]
> Per informazioni su come configurare un indirizzo IP statico in computer che eseguono altri sistemi operativi Microsoft, vedere [appendice B - configurazione degli indirizzi IP statici](#BKMK_B).

#### <a name="BKMK_deployADDNS01"></a>Distribuzione di DC1
Per distribuire DC1, ovvero è il computer che esegue servizi di dominio Active Directory (AD DS) e DNS, è necessario completare questi passaggi nell'ordine seguente:

-   Eseguire i passaggi nella sezione [configurazione di tutti i server](#BKMK_configuringAll).

-   [Installare Active Directory e DNS per una nuova foresta](#BKMK_installAD-DNS)

-   [Creare un Account utente in utenti e computer Active Directory](#BKMK_createUA)

-   [Assegnare l'appartenenza](#BKMK_assigngroup)

-   [Configurare una zona di ricerca inversa DNS](#BKMK_reverse)

**Privilegi amministrativi**

Se si sta installando una piccola rete e non il solo amministratore di rete, è consigliabile creare un account utente personale e quindi aggiungere l'account utente come membro del gruppo Enterprise Admins e Domain Admins. Questa operazione renderà più semplice agire come amministratore per tutte le risorse di rete. Si consiglia inoltre di accedere con questo account solo quando è necessario eseguire le attività amministrative e creare un account utente separato per l'esecuzione non IT attività correlate.

Se si dispone di un'organizzazione più grande con più amministratori, fare riferimento alla documentazione di Active Directory per determinare l'appartenenza al gruppo migliore per i dipendenti dell'organizzazione.

**Differenze tra account utente di dominio e gli account utente nel computer locale**

Uno dei vantaggi di un'infrastruttura basata su dominio è che non è necessario creare gli account utente in ogni computer nel dominio. Ciò vale se il computer è un computer client o server.

Per questo motivo, non è necessario creare gli account utente in ogni computer nel dominio. Creare tutti gli account utente in Active Directory Users and Computers e utilizzare le procedure descritte in precedenza per assegnare l'appartenenza al gruppo. Per impostazione predefinita, tutti gli account utente sono membri del gruppo Domain Users.

Tutti i membri del gruppo Domain Users possono accedere a qualsiasi computer client dopo l'aggiunta al dominio.

È possibile configurare gli account utente specificando i giorni e che l'utente è autorizzato per accedere al computer. È inoltre possibile designare i computer che ogni utente è autorizzato a utilizzare. Per configurare queste impostazioni, aprire Active Directory Users and Computers, individuare l'account utente che si desidera configurare e fare doppio clic sull'account. L'account utente **proprietà**, fare clic su di **Account** scheda e quindi fare clic su **orario di accesso** o **Accedi a**.

#### <a name="BKMK_installAD-DNS"></a>Installare Active Directory e DNS per una nuova foresta

È possibile utilizzare una delle procedure seguenti per installare servizi di dominio Active Directory (AD DS) e DNS e creare un nuovo dominio in una nuova foresta. 

La prima procedura vengono fornite istruzioni su come eseguire queste azioni tramite Windows PowerShell, mentre la seconda procedura illustra come installare Active Directory e DNS tramite Server Manager.

>[!IMPORTANT]
>Dopo aver completato i passaggi in questa procedura, il computer viene riavviato automaticamente.

**Installare Active Directory e DNS mediante Windows PowerShell**

È possibile utilizzare i comandi seguenti per installare e configurare Active Directory e DNS. È necessario sostituire il nome di dominio in questo esempio con il valore che si desidera utilizzare per il dominio.

>[!NOTE]
>Per ulteriori informazioni su questi comandi di Windows PowerShell, vedere gli argomenti di riferimento seguenti.
>- [Install-WindowsFeature](https://technet.microsoft.com/itpro/powershell/windows/server-manager/install-windowsfeature)
>- [Install-ADDSForest](https://technet.microsoft.com/itpro/powershell/windows/adds/deployment/install-addsforest)

Appartenenza al gruppo **amministratori** è il requisito minimo necessario per eseguire questa procedura.

- Eseguire Windows PowerShell come amministratore, digitare il comando seguente e quindi premere INVIO:  

`Install-WindowsFeature AD-Domain-Services -IncludeManagementTools`

Al termine dell'installazione, viene visualizzato il messaggio seguente in Windows PowerShell.

    
    Success Restart Needed  Exit Code   Feature Result
    ------- --------------  ---------   --------------
    True    No              Success     {Active Directory Domain Services, Group P...
    

- In Windows PowerShell, digitare il comando seguente, sostituendo il testo **corp.contoso.com** con il nome di dominio e quindi premere INVIO:

````
Install-ADDSForest -DomainName "corp.contoso.com"
````

- Durante il processo di installazione e la configurazione è visibile nella parte superiore della finestra di Windows PowerShell, viene visualizzato il messaggio seguente. Dopo che viene visualizzato, digitare una password e quindi premere INVIO.

    **SafeModeAdministratorPassword:**

- Dopo aver digitare una password e premere INVIO, viene visualizzato il seguente messaggio di conferma. Digitare la stessa password e quindi premere INVIO.

    **Verificare SafeModeAdministratorPassword:**

- Quando viene visualizzato il messaggio seguente, digitare la lettera **Y** e quindi premere INVIO.

    
    Il server di destinazione verrà configurato come controller di dominio e riavviato quando questa operazione è stata completata.
    Si desidera continuare con questa operazione?
    [Y] [A] Sì Sì a tutte le [N] non [L] non a tutti [S] sospendere [?] Guida in linea (valore predefinito è "Y"):
    
- Se si desidera, è possibile leggere i messaggi di avviso visualizzate durante il normale installazione di Active Directory e DNS. Questi messaggi normali e non sono un'indicazione di errore di installazione.

- Dopo l'installazione abbia esito positivo, verrà visualizzato un messaggio che informa che si sta per essere scollegati dal computer in modo che sia possibile riavviare il computer. Se si sceglie **Chiudi**, si viene disconnessi immediatamente il computer e il riavvio del computer. Se non si sceglie **Chiudi**, il computer viene riavviato dopo un periodo predefinito di tempo.

- Dopo il riavvio del server, è possibile verificare corretta installazione di servizi di dominio Active Directory e DNS. Aprire Windows PowerShell, digitare il comando seguente e premere INVIO.

````
Get-WindowsFeature
````

I risultati di questo comando vengono visualizzati in Windows PowerShell e devono essere simili a quelli nell'immagine seguente. Per le tecnologie installate, le parentesi a sinistra del nome della tecnologia contengono il carattere **X**e il valore di **lo stato di installazione** è **installati**.

![Risultati del comando Get-WindowsFeature](../media/Core-Network-Guide/server-roles-installed.jpg)

**Installare Active Directory e DNS mediante Server Manager**

1.  Su DC1, in **Server Manager**, fare clic su **Gestisci**, quindi fare clic su **Aggiungi ruoli e funzionalità**. Verrà avviata l'aggiunta guidata ruoli e funzionalità.

2.  In **prima di iniziare**, fare clic su **Avanti**.

    > [!NOTE]
    > Il **prima di iniziare** pagina l'aggiunta guidata ruoli e funzionalità non viene visualizzato se è stato selezionato in precedenza **Ignora questa pagina per impostazione predefinita** quando è stato eseguito l'aggiunta guidata ruoli e funzionalità.

3.  In **Selezione tipo di installazione**, assicurarsi che **installazione basata su ruoli o basata su funzionalità** sia selezionata e quindi fare clic su **Avanti**.

4.  In **server di destinazione**, assicurarsi che **selezionare un server dal pool di server** sia selezionata. In **Pool di Server**, verificare che sia selezionato il computer locale. Fare clic su **Avanti**.

5.  In **Selezione ruoli server**, in **ruoli**, fare clic su **servizi di dominio Active Directory**. In **Aggiungi funzionalità necessarie per servizi di dominio Active Directory**, fare clic su **Aggiungi funzionalità**. Fare clic su **Avanti**.

6.  In **selezionare le funzionalità**, fare clic su **Avanti**e in **servizi di dominio Active Directory**, esaminare le informazioni fornite e quindi fare clic su **Avanti**.

7.  In **Conferma selezioni per l'installazione**, fare clic su **installare**. La pagina di avanzamento installazione Visualizza stato durante il processo di installazione. Al termine del processo, nei dettagli del messaggio, fare clic su **promuovere il server a un controller di dominio**. Verrà visualizzata la configurazione guidata servizi di dominio Active Directory.

8.  In **configurazione distribuzione**selezionare **aggiungere una nuova foresta**. In **nome dominio radice**, digitare il nome di dominio completo (FQDN) per il dominio. Ad esempio, se il nome FQDN è corp.contoso.com, digitare **corp.contoso.com**. Fare clic su **Avanti**.

9. In **opzioni Controller di dominio**, in **selezionare a livello di funzionalità della nuova foresta e del dominio radice**, selezionare il livello di funzionalità foresta e il livello funzionale di dominio che si desidera utilizzare. In **specificare funzionalità del controller di dominio**, assicurarsi che **server del sistema DNS (Domain Name)** e **catalogo globale (GC)** siano selezionate. In **Password** e **Conferma password**, digitare la password modalità ripristino servizi Directory (DSRM) che si desidera utilizzare. Fare clic su **Avanti**.

10. In **opzioni DNS**, fare clic su **Avanti**.

11. In **opzioni aggiuntive**, verificare il nome NetBIOS assegnato al dominio e modificarlo solo se necessario. Fare clic su **Avanti**.

12. In **percorsi**, in **specificare il percorso del database di Active Directory, i file di registro e SYSVOL**, effettuare una delle operazioni seguenti:

    -   Accettare i valori predefiniti.

    -   Digitare i percorsi che si desidera utilizzare per **cartella Database**, **cartella file di registro**, e **cartella SYSVOL**.

13. Fare clic su **Avanti**.

14. In **verifica opzioni**, rivedere le selezioni.

15. Se si desidera esportare le impostazioni in uno script di Windows PowerShell, fare clic su **visualizzare script**. Lo script viene aperto nel blocco note ed è possibile salvare il percorso della cartella che vuoi. Fare clic su **Avanti**. In **controllo dei prerequisiti**, le selezioni vengono convalidate. Al termine del controllo, fare clic su **installare**. Quando richiesto da Windows, fare clic su **Chiudi**. Il server viene riavviato per completare l'installazione di Active Directory e DNS.

16. Per verificare l'installazione ha esito positivo, visualizzare la console di Server Manager dopo il riavvio del server. Entrambi i servizi di dominio Active Directory e DNS dovrebbe essere visualizzato nel riquadro a sinistra, come gli elementi evidenziati nell'immagine seguente.

![Active Directory e DNS in Server Manager](../media/Core-Network-Guide/server-roles-installed-sm.jpg)

##### <a name="BKMK_createUA"></a>Creare un Account utente in utenti e computer Active Directory
È possibile utilizzare questa procedura per creare un nuovo account utente di dominio in Active Directory Users e computer di Microsoft Management Console (MMC).

Appartenenza al gruppo **Domain Admins**, o equivalente è il requisito minimo necessario per eseguire questa procedura.

> [!NOTE]
> Per eseguire questa procedura mediante Windows PowerShell, aprire PowerShell e digitare i cmdlet seguenti su una riga, quindi premere INVIO. È inoltre necessario sostituire il nome dell'account utente in questo esempio con il valore che si desidera utilizzare.
>
> `New-ADUser -SamAccountName User1 -AccountPassword (read-host "Set user password" -assecurestring) -name "User1" -enabled $true -PasswordNeverExpires $true -ChangePasswordAtLogon $false`
>
> Dopo aver premuto INVIO, digitare la password per l'account utente. L'account viene creato e, per impostazione predefinita, appartiene al gruppo Domain Users.
>
> Con il cmdlet seguente, è possibile assegnare l'appartenenza al gruppo aggiuntive per il nuovo account utente. L'esempio seguente aggiunge User1 ai gruppi Enterprise Admins e Domain Admins. Assicurarsi prima di eseguire questo comando che modifichi il nome dell'account utente, nome di dominio e gruppi in base ai propri requisiti.
>
> `Add-ADPrincipalGroupMembership -Identity "CN=User1,CN=Users,DC=corp,DC=contoso,DC=com" -MemberOf "CN=Enterprise Admins,CN=Users,DC=corp,DC=contoso,DC=com","CN=Domain Admins,CN=Users,DC=corp,DC=contoso,DC=com"`

###### <a name="to-create-a-user-account"></a>Per creare un account utente

1.  Su DC1, in Server Manager fare clic su **strumenti**, quindi fare clic su **Active Directory Users and Computers**. Verrà visualizzata la Active Directory MMC Utenti e computer. Se non è già selezionata, fare clic sul nodo per il dominio. Ad esempio, se il dominio è corp.contoso.com, fare clic su **corp.contoso.com**.

2.  Nel riquadro dei dettagli, fare clic sulla cartella in cui si desidera aggiungere un account utente.

    **Dove?**

    -   Active Directory Users and Computers /*nodo dominio*/*cartella*

3.  Scegliere **New**, quindi fare clic su **utente**. Il **nuovo oggetto - utente** apre la finestra di dialogo.

4.  In **nome**, digitare il nome dell'utente.

5.  In **iniziali**, digitare le iniziali dell'utente.

6.  In **cognome**, digitare il cognome dell'utente.

7.  Modificare **nome completo** per aggiungere le iniziali o invertire l'ordine dei nomi e cognomi.

8.  In **nome accesso utente**, digitare il nome di accesso utente. Fare clic su **Avanti**.

9. In **nuovo oggetto - utente**, in **Password** e **Conferma password**, digitare la password dell'utente e quindi selezionare le opzioni appropriate per la password.

10. Fare clic su **Avanti**, rivedere le nuove impostazioni di account utente e quindi fare clic su **fine**.

##### <a name="BKMK_assigngroup"></a>Assegnare l'appartenenza
È possibile utilizzare questa procedura per aggiungere un utente, computer o gruppo a un gruppo in Active Directory Users e computer di Microsoft Management Console (MMC).

Appartenenza al gruppo **Domain Admins**, o equivalente è il requisito minimo necessario per eseguire questa procedura.

###### <a name="to-assign-group-membership"></a>Per assegnare l'appartenenza al gruppo

1.  Su DC1, in Server Manager fare clic su **strumenti**, quindi fare clic su **Active Directory Users and Computers**. Verrà visualizzata la Active Directory MMC Utenti e computer. Se non è già selezionata, fare clic sul nodo per il dominio. Ad esempio, se il dominio è corp.contoso.com, fare clic su **corp.contoso.com**.

2.  Nel riquadro dei dettagli fare doppio clic sulla cartella che contiene il gruppo a cui si desidera aggiungere un membro.

    Dove?

    -   **Active Directory Users and Computers**/*nodo dominio*/*cartella che contiene il gruppo*

3.  Nel riquadro dei dettagli fare doppio clic sull'oggetto che si desidera aggiungere a un gruppo, ad esempio un utente o computer, quindi fare clic su **proprietà**. L'oggetto **proprietà** apre la finestra di dialogo. Fare clic su di **appartenente** scheda.

4.  Nel **appartenente** scheda, fare clic su **Aggiungi**.

5.  In **immettere i nomi degli oggetti da selezionare**, digitare il nome del gruppo a cui si desidera aggiungere l'oggetto e quindi fare clic su **OK**.

6.  Per assegnare l'appartenenza al gruppo ad altri utenti, gruppi o computer, ripetere i passaggi 4 e 5 della procedura.

##### <a name="BKMK_reverse"></a>Configurare una zona di ricerca inversa DNS
È possibile utilizzare questa procedura per configurare una zona di ricerca inversa nel sistema DNS (Domain Name).

Appartenenza al gruppo **Domain Admins** è il requisito minimo necessario per eseguire questa procedura.

> [!NOTE]
> -   Per le organizzazioni di medie e grandi dimensioni, è consigliabile configurare e utilizzare il gruppo DNSAdmins in Active Directory Users and Computers. Per ulteriori informazioni, vedere [risorse tecniche aggiuntive](#BKMK_resources)
> -   Per eseguire questa procedura mediante Windows PowerShell, aprire PowerShell e digitare i cmdlet seguenti su una riga, quindi premere INVIO. È inoltre necessario sostituire la ricerca inversa zona e i file di zona utilizzati i nomi DNS in questo esempio con i valori che si desidera utilizzare. Assicurarsi che si inverte l'ID di rete per il nome della zona inversa. Ad esempio, se l'ID di rete è 192.168.0, creare il nome di zona di ricerca inversa **0.168.192.in-addr. arpa**.
>
> `Add-DnsServerPrimaryZone 0.0.10.in-addr.arpa -ZoneFile 0.0.10.in-addr.arpa.dns`

###### <a name="to-configure-a-dns-reverse-lookup-zone"></a>Per configurare una zona di ricerca inversa DNS

1.  Su DC1, in Server Manager fare clic su **strumenti**, quindi fare clic su **DNS**. Verrà visualizzata la finestra di MMC DNS.

2.  Se non è già espanso, in DNS, fare doppio clic il nome del server per espandere l'albero. Ad esempio, se il nome del server DNS è DC1, fare doppio clic su **DC1**.

3.  Selezionare **zone di ricerca inversa**, fare doppio clic su **zone di ricerca inversa**, quindi fare clic su **nuova zona**. Apre la creazione guidata nuova zona.

4.  In **iniziale per la creazione guidata nuova zona**, fare clic su **Avanti**.

5.  In **tipo di zona**selezionare **zona primaria**.

6.  Se il server DNS è un controller di dominio scrivibile, assicurarsi che **archivia la zona in Active Directory** sia selezionata. Fare clic su **Avanti**.

7.  In **ambito di replica di Active Directory zona**selezionare **a tutti i server DNS in esecuzione sul controller di dominio nel dominio**, a meno che non hai un motivo specifico per scegliere un'opzione diversa. Fare clic su **Avanti**.

8.  Nel primo **nome della zona di ricerca inversa** selezionare **zona di ricerca inversa IPv4**. Fare clic su **Avanti**.

9. Nella seconda **nome della zona di ricerca inversa** pagina, effettuare una delle operazioni seguenti:

    -   In **ID di rete**, digitare l'ID di rete dell'intervallo di indirizzi IP. Ad esempio, se l'intervallo di indirizzi IP è 10.0.0.1 tramite 10.0.0.254, digitare **10.0.0**.

    -   In **nome della zona di ricerca inversa**, viene aggiunto automaticamente il nome di zona di ricerca inversa IPv4. Fare clic su **Avanti**.

10. In **aggiornamento dinamico**, selezionare il tipo di aggiornamenti dinamici che si desidera consentire. Fare clic su **Avanti**.

11. In **completare la creazione guidata nuova zona**, rivedere le scelte effettuate e quindi fare clic su **fine**.

#### <a name="BKMK_joinlogserver"></a>Aggiunta di computer Server al dominio e accesso
Dopo aver installato Servizi di dominio Active Directory (AD DS) e creato uno o più account utente che dispone delle autorizzazioni per aggiungere un computer al dominio, è possibile aggiungere server della rete core al dominio e l'accesso a tali server per installare tecnologie aggiuntive, ad esempio Dynamic Host Configuration Protocol (DHCP).

In tutti i server che si desidera distribuire, ad eccezione del server che esegue servizi di dominio Active Directory, eseguire le operazioni seguenti:

1.  Completare le procedure illustrate nella [configurazione di tutti i server](#BKMK_configuringAll).

2.  Utilizzare le istruzioni nelle due procedure seguenti per aggiungere i server al dominio ed effettuare l'accesso a tali server per eseguire ulteriori attività di distribuzione:

> [!NOTE]
> Per eseguire questa procedura mediante Windows PowerShell, aprire PowerShell e digitare il cmdlet seguente e quindi premere INVIO. È inoltre necessario sostituire il nome di dominio con il nome che si desidera utilizzare.
>
> `Add-Computer -DomainName corp.contoso.com`
>
> Quando richiesto, digitare il nome utente e la password per un account che dispone dell'autorizzazione per aggiungere un computer al dominio. Per riavviare il computer, digitare il comando seguente e premere INVIO.
>
> `Restart-Computer`

###### <a name="to-join-computers-running--windows-server-2016--windows-server-2012-r2--and--windows-server-2012--to-the-domain"></a>Per aggiungere i computer che esegue Windows Server 2016, Windows Server 2012 R2 e Windows Server 2012 al dominio

1.  In Server Manager, fare clic su **Server locale**. Nel riquadro dei dettagli, fare clic su **gruppo di lavoro**. Il **proprietà del sistema** apre la finestra di dialogo.

2.  Nel **proprietà del sistema** la finestra di dialogo, fare clic su **modifica**. Il **cambiamenti dominio/nome Computer** apre la finestra di dialogo.

3.  In **nome Computer**, in **appartenente**, fare clic su **dominio**e quindi digitare il nome del dominio che si desidera aggiungere. Ad esempio, se il nome di dominio è corp.contoso.com, digitare **corp.contoso.com**.

4.  Fare clic su **OK**. Il **la protezione di Windows** apre la finestra di dialogo.

5.  In **cambiamenti dominio/nome Computer**, in **nome utente**, digitare il nome utente e in **Password**, digitare la password e quindi fare clic su **OK**. Il **cambiamenti dominio/nome Computer** si apre la finestra di dialogo, benvenuto al dominio. Fare clic su **OK**.

6.  Il **cambiamenti dominio/nome Computer** la finestra di dialogo viene visualizzato un messaggio che indica che è necessario riavviare il computer per rendere effettive le modifiche. Fare clic su **OK**.

7.  Nel **proprietà del sistema** della finestra di dialogo di **nome Computer** scheda, fare clic su **Chiudi**. Il **Microsoft Windows** la finestra di dialogo verrà visualizzato un messaggio per segnalare nuovamente che è necessario riavviare il computer per rendere effettive le modifiche. Fare clic su **Riavvia ora**.

> [!NOTE]
> Per informazioni su come aggiungere i computer che eseguono altri sistemi operativi Microsoft per il dominio, vedere [appendice C - aggiunta di computer al dominio](#BKMK_C).

###### <a name="to-log-on-to-the-domain-using-computers-running-windows-server-2016"></a>Per accedere al dominio utilizzando computer che eseguono Windows Server 2016

1.  Disconnettersi dal computer o riavviare il computer.

2.  Premere CTRL + ALT + CANC. Verrà visualizzata la schermata di accesso.

3.  Nell'angolo inferiore sinistro, fare clic su **altro utente**.

4.  In **nome utente**, digitare il nome utente.

5.  In **Password**, digitare la password di dominio e quindi fare clic sulla freccia o premere INVIO.

> [!NOTE]
> Per informazioni su come accedere al dominio utilizzando computer che eseguono altri sistemi operativi Microsoft, vedere [appendice D - accesso al dominio](#BKMK_D).

#### <a name="BKMK_deployDHCP01"></a>Distribuzione di DHCP1
Prima di distribuire questo componente della rete core, è necessario eseguire le operazioni seguenti:

-   Eseguire i passaggi nella sezione [configurazione di tutti i server](#BKMK_configuringAll).

-   Eseguire i passaggi nella sezione [aggiunta di computer Server al dominio ed effettuare l'accesso](#BKMK_joinlogserver).

Per distribuire DHCP1, ovvero il computer esegue il ruolo server Dynamic Host Configuration Protocol (DHCP), è necessario completare questi passaggi nell'ordine seguente:

-   [Installare Dynamic Host Configuration Protocol (DHCP)](#BKMK_installDHCP)

-   [Creare e attivare un nuovo ambito DHCP](#BKMK_newscopeDHCP)

> [!NOTE]
> Per eseguire queste procedure mediante Windows PowerShell, aprire PowerShell e digitare i cmdlet seguenti su righe distinte, quindi premere INVIO. È inoltre necessario sostituire il nome dell'ambito, start indirizzo IP e gli intervalli di fine, la subnet mask e altri valori in questo esempio con i valori che si desidera utilizzare.
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

##### <a name="BKMK_installDHCP"></a>Installare Dynamic Host Configuration Protocol (DHCP)
È possibile utilizzare questa procedura per installare e configurare il ruolo Server DHCP utilizzando l'aggiunta guidata ruoli e funzionalità.

Appartenenza al gruppo **Domain Admins**, o equivalente è il requisito minimo necessario per eseguire questa procedura.

###### <a name="to-install-dhcp"></a>Per installare DHCP

1.  Su DHCP1, in Server Manager, fare clic su **Gestisci**, quindi fare clic su **Aggiungi ruoli e funzionalità**. Verrà avviata l'aggiunta guidata ruoli e funzionalità.

2.  In **prima di iniziare**, fare clic su **Avanti**.

    > [!NOTE]
    > Il **prima di iniziare** pagina l'aggiunta guidata ruoli e funzionalità non viene visualizzato se è stato selezionato in precedenza **Ignora questa pagina per impostazione predefinita** quando è stato eseguito l'aggiunta guidata ruoli e funzionalità.

3.  In **Selezione tipo di installazione**, assicurarsi che **installazione basata su ruoli o basata su funzionalità** sia selezionata e quindi fare clic su **Avanti**.

4.  In **server di destinazione**, assicurarsi che **selezionare un server dal pool di server** sia selezionata. In **Pool di Server**, verificare che sia selezionato il computer locale. Fare clic su **Avanti**.

5.  In **Selezione ruoli Server**, in **ruoli**selezionare **Server DHCP**. In **aggiungere le funzionalità necessarie per Server DHCP**, fare clic su **Aggiungi funzionalità**. Fare clic su **Avanti**.

6.  In **selezionare le funzionalità**, fare clic su **Avanti**e in **Server DHCP**, esaminare le informazioni fornite e quindi fare clic su **Avanti**.

7.  In **Conferma selezioni per l'installazione**, fare clic su **riavvia automaticamente il server di destinazione se necessario**. Quando viene chiesto di confermare la selezione, fare clic su **Sì**, quindi fare clic su **installare**. Il **lo stato dell'installazione** pagina Visualizza stato durante il processo di installazione. Al termine del processo, il messaggio "configurazione necessaria. Installazione completata in *ComputerName*"viene visualizzato, in cui *ComputerName* è il nome del computer su cui è installato Server DHCP. Nella finestra di messaggio, fare clic su **completa configurazione DHCP**. Apre la procedura guidata configurazione DHCP post-Post-Install. Fare clic su **Avanti**.

8.  In **autorizzazione**, specificare le credenziali che si desidera utilizzare per autorizzare il server DHCP in servizi di dominio Active Directory e quindi fare clic su **Commit**. Al termine dell'autorizzazione, fare clic su **Chiudi**.

##### <a name="BKMK_newscopeDHCP"></a>Creare e attivare un nuovo ambito DHCP
È possibile utilizzare questa procedura per creare un nuovo ambito DHCP utilizzando DHCP Microsoft Management Console (MMC). Dopo aver completato la procedura, l'ambito viene attivato e l'intervallo di esclusione che crei impedisce che il server DHCP leasing gli indirizzi IP che consente di configurare in modo statico i server e altri dispositivi che richiedono un indirizzo IP statico.

Appartenenza al gruppo **DHCP Administrators**, o equivalente è il requisito minimo necessario per eseguire questa procedura.

###### <a name="to-create-and-activate-a-new-dhcp-scope"></a>Per creare e attivare un nuovo ambito DHCP

1.  Su DHCP1, in Server Manager, fare clic su **strumenti**, quindi fare clic su **DHCP**. Verrà visualizzata la finestra di MMC DHCP.

2.  In **DHCP**, espandere il nome del server. Ad esempio, se il nome del server DHCP è DHCP1.corp.contoso.com, fare clic sulla freccia accanto a **DHCP1.corp.contoso.com**.

3.  Sotto il nome del server, fare doppio clic su **IPv4**, quindi fare clic su **nuovo ambito**. Verrà visualizzata la creazione guidata ambito.

4.  In **iniziale per la creazione guidata ambito**, fare clic su **Avanti**.

5.  In **nome ambito**, in **nome**, digitare un nome per l'ambito. Ad esempio, digitare **Subnet 1**.

6.  In **descrizione**, digitare una descrizione per il nuovo ambito e quindi fare clic su **Avanti**.

7.  In **intervallo di indirizzi IP**, eseguire le operazioni seguenti:

    1.  In **indirizzo IP iniziale**, digitare l'indirizzo IP che è il primo indirizzo IP nell'intervallo. Ad esempio, digitare **10.0.0.1**.

    2.  In **indirizzo IP finale**, digitare l'indirizzo IP che è l'ultimo indirizzo IP nell'intervallo. Ad esempio, digitare **10.0.0.254**. I valori del parametro **lunghezza** e **Subnet mask** vengono inseriti automaticamente in base all'indirizzo IP immesso per **indirizzo IP iniziale**.

    3.  Se necessario, modificare i valori in **lunghezza** o **Subnet mask**, come appropriato per lo schema di indirizzamento.

    4.  Fare clic su **Avanti**.

8.  In **Aggiungi esclusioni**, eseguire le operazioni seguenti:

    1.  In **indirizzo IP iniziale**, digitare l'indirizzo IP che è il primo indirizzo IP dell'intervallo di esclusione. Ad esempio, digitare **10.0.0.1**.

    2.  In **indirizzo IP finale**, digitare l'indirizzo IP è l'ultimo IP indirizzo nell'intervallo di esclusione, ad esempio, digitare **10.0.0.15**.

9. Fare clic su **Aggiungi**, quindi fare clic su **Avanti**.

10. In **durata del Lease**, modificare i valori predefiniti per **giorni**, **ore**, e **minuti**, come appropriato per la rete e quindi fare clic su **Avanti**.

11. In **Configura opzioni DHCP**selezionare **Sì, desidero configurare le opzioni ora**, quindi fare clic su **Avanti**.

12. In **Router (Gateway predefinito)**, effettuare una delle operazioni seguenti:

    -   Se non è router nella rete, fare clic su **Avanti**.

    -   In **indirizzo IP**, digitare l'indirizzo IP del router o gateway predefinito. Ad esempio, digitare **10.0.0.1**. Fare clic su **Aggiungi**, quindi fare clic su **Avanti**.

13. In **nome di dominio e server DNS**, eseguire le operazioni seguenti:

    1.  In **dominio padre**, digitare il nome del dominio DNS utilizzato dai client per la risoluzione dei nomi. Ad esempio, digitare **corp.contoso.com**.

    2.  In **nome Server**, digitare il nome del computer DNS utilizzato dai client per la risoluzione dei nomi. Ad esempio, digitare **DC1**.

    3.  Fare clic su **risolvere**. L'indirizzo IP del server DNS viene aggiunto **indirizzo IP**. Fare clic su **Aggiungi**, attendere convalida indirizzo IP del server DNS completare e quindi fare clic su **Avanti**.

14. In **server WINS**, perché non hai server WINS nella rete, fare clic su **Avanti**.

15. In **Attiva ambito**selezionare **Sì, attiva l'ambito adesso**.

16. Fare clic su **Avanti**, quindi fare clic su **fine**.

> [!IMPORTANT]
> Per creare nuovi ambiti per ulteriori subnet, ripetere questa procedura. Utilizzare un intervallo di indirizzi IP diverso per ogni subnet che si prevede di distribuire e verificare che l'inoltro dei messaggi DHCP sia abilitato su tutti i router che rimandano ad altre subnet.

### <a name="BKMK_joinlogclients"></a>Aggiunta di computer Client al dominio e accesso

> [!NOTE]
> Per eseguire questa procedura mediante Windows PowerShell, aprire PowerShell e digitare il cmdlet seguente e quindi premere INVIO. È inoltre necessario sostituire il nome di dominio con il nome che si desidera utilizzare.
>
> `Add-Computer -DomainName corp.contoso.com`
>
> Quando richiesto, digitare il nome utente e la password per un account che dispone dell'autorizzazione per aggiungere un computer al dominio. Per riavviare il computer, digitare il comando seguente e premere INVIO.
>
> `Restart-Computer`

##### <a name="to-join-computers-running-windows-10-to-the-domain"></a>Per aggiungere i computer che eseguono Windows 10 al dominio

1.  Accedere al computer con l'account amministratore locale.

2.  In **ricerca web e Windows**, tipo **sistema**. Nei risultati della ricerca, fare clic su **sistema (Pannello di controllo)**. Il **sistema** apre la finestra di dialogo.

3.  In **sistema**, fare clic su **impostazioni di sistema avanzate**. Il **proprietà del sistema** apre la finestra di dialogo. Fare clic su di **nome Computer** scheda.

4.  In **nome Computer**, fare clic su **modifica**. Il **cambiamenti dominio/nome Computer** apre la finestra di dialogo.

5.  In **cambiamenti dominio/nome Computer**, In **appartenente**, fare clic su **dominio**e quindi digitare il nome del dominio di cui si desidera aggiungere. Ad esempio, se il nome di dominio è corp.contoso.com, digitare **corp.contoso.com**.

6.  Fare clic su **OK**. Il **la protezione di Windows** apre la finestra di dialogo.

7.  In **cambiamenti dominio/nome Computer**, in **nome utente**, digitare il nome utente e in **Password**, digitare la password e quindi fare clic su **OK**. Il **cambiamenti dominio/nome Computer** si apre la finestra di dialogo, benvenuto al dominio. Fare clic su **OK**.

8.  Il **cambiamenti dominio/nome Computer** la finestra di dialogo viene visualizzato un messaggio che indica che è necessario riavviare il computer per rendere effettive le modifiche. Fare clic su **OK**.

9. Nel **proprietà del sistema** della finestra di dialogo di **nome Computer** scheda, fare clic su **Chiudi**. Il **Microsoft Windows** la finestra di dialogo verrà visualizzato un messaggio per segnalare nuovamente che è necessario riavviare il computer per rendere effettive le modifiche. Fare clic su **Riavvia ora**.

##### <a name="to-join-computers-running-windows-81-to-the-domain"></a>Per aggiungere i computer che eseguono Windows 8.1 al dominio

1.  Accedere al computer con l'account amministratore locale.

2.  Fare doppio clic su **Start**, quindi fare clic su **sistema**. Il **sistema** apre la finestra di dialogo.

3.  In **sistema**, fare clic su **impostazioni di sistema avanzate**. Il **proprietà del sistema** apre la finestra di dialogo. Fare clic su di **nome Computer** scheda.

4.  In **nome Computer**, fare clic su **modifica**. Il **cambiamenti dominio/nome Computer** apre la finestra di dialogo.

5.  In **cambiamenti dominio/nome Computer**, In **appartenente**, fare clic su **dominio**e quindi digitare il nome del dominio di cui si desidera aggiungere. Ad esempio, se il nome di dominio è corp.contoso.com, digitare **corp.contoso.com**.

6.  Fare clic su **OK**. Il **la protezione di Windows** apre la finestra di dialogo.

7.  In **cambiamenti dominio/nome Computer**, in **nome utente**, digitare il nome utente e in **Password**, digitare la password e quindi fare clic su **OK**. Il **cambiamenti dominio/nome Computer** si apre la finestra di dialogo, benvenuto al dominio. Fare clic su **OK**.

8.  Il **cambiamenti dominio/nome Computer** la finestra di dialogo viene visualizzato un messaggio che indica che è necessario riavviare il computer per rendere effettive le modifiche. Fare clic su **OK**.

9. Nel **proprietà del sistema** della finestra di dialogo di **nome Computer** scheda, fare clic su **Chiudi**. Il **Microsoft Windows** la finestra di dialogo verrà visualizzato un messaggio per segnalare nuovamente che è necessario riavviare il computer per rendere effettive le modifiche. Fare clic su **Riavvia ora**.

##### <a name="to-log-on-to-the-domain-using-computers-running-windows-10"></a>Per accedere al dominio utilizzando computer che eseguono Windows 10

1.  Disconnettersi dal computer o riavviare il computer.

2.  Premere CTRL + ALT + CANC. Verrà visualizzata la schermata di accesso.

3.  In basso a sinistra, fare clic su **altro utente**.

4.  In **nome utente**, digitare il nome di dominio e dell'utente nel formato *dominio\utente*. Ad esempio, per accedere al dominio con un account denominato corp.contoso.com **utente-01**, tipo **corp\utente-01**.

5.  In **Password**, digitare la password di dominio e quindi fare clic sulla freccia o premere INVIO.

### <a name="BKMK_optionalfeatures"></a>Distribuzione di funzionalità facoltative per l'autenticazione di accesso di rete e servizi Web
Se si intende distribuire server di accesso di rete, ad esempio punti di accesso wireless o server VPN, dopo l'installazione della rete core, è consigliabile distribuire un server dei criteri di rete e un server Web. Per le distribuzioni di accesso di rete, è consigliabile utilizzare i metodi di autenticazione sicura basata su certificati. È possibile utilizzare criteri di rete per gestire i criteri di accesso di rete e per distribuire metodi di autenticazione sicuri. È possibile utilizzare un server Web per pubblicare l'elenco di revoche di certificati (CRL) dell'autorità di certificazione (CA) che emette i certificati per l'autenticazione sicura.

> [!NOTE]
> È possibile distribuire certificati server e altre funzionalità aggiuntive utilizzando le guide complementari alla rete Core. Per ulteriori informazioni, vedere [risorse tecniche aggiuntive](#BKMK_resources).

Nella figura seguente è illustrata la topologia di rete di Windows Server Core con l'aggiunti dei criteri di rete e dei server Web.

![Topologia di rete di Windows Server Core con l'aggiunti dei criteri di rete e dei server Web](../media/Core-Network-Guide/cng16_overview_2.jpg)

Le sezioni seguenti forniscono informazioni sull'aggiunta di server Web e dei criteri di rete alla rete.

-   [Distribuzione di NPS1](#BKMK_deployNPS1)

-   [Distribuzione di WEB1](#BKMK_IIS)

#### <a name="BKMK_deployNPS1"></a>Distribuzione di NPS1
Il server di Server dei criteri di rete (NPS) viene installato come passaggio preliminare alla distribuzione di altre tecnologie di accesso di rete, ad esempio server di rete privata virtuale (VPN), punti di accesso wireless e commutatori di autenticazione 802.1 X.

Server dei criteri di rete (NPS) consente di configurare e gestire centralmente le criteri di rete con le seguenti funzionalità: server RADIUS Remote Authentication Dial-In User Service () e proxy RADIUS.

Criteri di rete è un componente facoltativo di una rete core, ma è consigliabile installare dei criteri di rete se si verifica una delle operazioni seguenti:

-   Si prevede di espandere la rete per includere server di accesso remoto compatibili con il protocollo RADIUS, ad esempio un computer che esegue Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2 o Windows Server 2008 e servizio Routing e accesso remoto, Gateway di servizi Terminal o Gateway Desktop remoto.


-   Si prevede di distribuire l'autenticazione 802.1 X per reti cablate o wireless access.

Prima di distribuire questo servizio ruolo, è necessario eseguire i passaggi seguenti nel computer che viene configurato come un server dei criteri di rete.

-   Eseguire i passaggi nella sezione [configurazione di tutti i server](#BKMK_configuringAll).

-   Eseguire i passaggi nella sezione [aggiunta di computer Server al dominio ed effettuare l'accesso](#BKMK_joinlogserver)

Per distribuire il server NPS1, ovvero il computer che esegue il servizio ruolo Server dei criteri di rete (NPS) del ruolo server Servizi di accesso e criteri di rete, è necessario completare questo passaggio:

-   [Pianificazione della distribuzione di NPS1](#bkmk_NetFndtn_Pln_NPS-01)

-   [Installare Server dei criteri di rete (NPS)](#BKMK_installNPS)

-   [Registrare il Server dei criteri di rete nel dominio predefinito](#BKMK_registerNPS)

> [!NOTE]
> Questa guida fornisce istruzioni per la distribuzione dei criteri di rete in un server autonomo o macchina virtuale denominata NPS1.  Un altro modello di distribuzione consigliato è l'installazione di criteri di rete in un controller di dominio. Se si preferisce l'installazione dei criteri di rete in un controller di dominio anziché in un server autonomo, è possibile installare NPS in DC1.

##### <a name="bkmk_NetFndtn_Pln_NPS-01"></a>Pianificazione della distribuzione di NPS1
Se si intende distribuire server di accesso di rete, ad esempio punti di accesso wireless o server VPN, dopo la distribuzione della rete core, è consigliabile distribuire criteri di rete.

Quando si utilizza criteri di rete come server RADIUS Remote Authentication Dial-In User Service (), dei criteri di rete esegue l'autenticazione e autorizzazione per le richieste di connessione tramite i server di accesso di rete. Criteri di rete consente inoltre di configurare e gestire i criteri di rete a livello centrale che determinare chi può accedere alla rete e le modalità di accesso alla rete quando accedono alla rete.

Di seguito sono i passaggi di pianificazione chiave prima di installare dei criteri di rete.

- Pianificare il database degli account utente. Per impostazione predefinita, se si aggiunge il server dei criteri di rete a un dominio di Active Directory, dei criteri di rete esegue l'autenticazione e autorizzazione utilizzando il database degli account utente di dominio Active Directory. In alcuni casi, ad esempio con le reti di grandi dimensioni che usano criteri di rete come proxy RADIUS per inoltrare le richieste di connessione ad altri server RADIUS, si desidera installare il server NPS in un computer non appartenenti al dominio.

- Pianificare l'accounting RADIUS. Criteri di rete consente di registrare i dati di accounting in un database di SQL Server o a un file di testo nel computer locale. Se si desidera utilizzare la registrazione di SQL Server, pianificare l'installazione e la configurazione del server che esegue SQL Server.

##### <a name="BKMK_installNPS"></a>Installare Server dei criteri di rete (NPS)
È possibile utilizzare questa procedura per installare Server dei criteri di rete (NPS) tramite l'aggiunta guidata ruoli e funzionalità. Criteri di rete è un servizio ruolo del ruolo server Servizi di accesso e criteri di rete.

> [!NOTE]
> Per impostazione predefinita, dei criteri di rete è in ascolto del traffico RADIUS sulle porte 1812, 1813, 1645 e 1646 su tutte le schede di rete installate. Se Windows Firewall con sicurezza avanzata è abilitata quando si installa NPS, eccezioni del firewall per queste porte vengono create automaticamente durante il processo di installazione per il protocollo Internet versione 6 \(IPv6\) sia il traffico IPv4. Se i server di accesso di rete sono configurati per inviare il traffico RADIUS su porte diverse da quelle predefinite, rimuovere le eccezioni create durante l'installazione dei criteri di rete in Windows Firewall con sicurezza avanzata e creare eccezioni per le porte utilizzate per il traffico RADIUS.

**Credenziali amministrative**

Per completare questa procedura, è necessario essere un membro del **Domain Admins** gruppo.

> [!NOTE]
> Per eseguire questa procedura mediante Windows PowerShell, aprire PowerShell e digitare il comando seguente e quindi premere INVIO.
>
> `Install-WindowsFeature NPAS -IncludeManagementTools`

###### <a name="to-install-nps"></a>Per installare il server NPS

1.  Su NPS1, in Server Manager, fare clic su **Gestisci**, quindi fare clic su **Aggiungi ruoli e funzionalità**. Verrà avviata l'aggiunta guidata ruoli e funzionalità.

2.  In **prima di iniziare**, fare clic su **Avanti**.

    > [!NOTE]
    > Il **prima di iniziare** pagina l'aggiunta guidata ruoli e funzionalità non viene visualizzato se è stato selezionato in precedenza **Ignora questa pagina per impostazione predefinita** quando è stato eseguito l'aggiunta guidata ruoli e funzionalità.

3.  In **Selezione tipo di installazione**, assicurarsi che **installazione basata su ruoli o basata su funzionalità** sia selezionata e quindi fare clic su **Avanti**.

4.  In **server di destinazione**, assicurarsi che **selezionare un server dal pool di server** sia selezionata. In **Pool di Server**, verificare che sia selezionato il computer locale. Fare clic su **Avanti**.

5.  In **Selezione ruoli Server**, in **ruoli**selezionare **servizi di accesso e criteri di rete**. Una finestra di dialogo viene visualizzata se è necessario aggiungere le funzionalità necessarie per servizi di accesso e criteri di rete. Fare clic su **Aggiungi funzionalità**, quindi fare clic su **Avanti**.

6.  In **selezionare le funzionalità**, fare clic su **Avanti**e in **servizi di accesso e criteri di rete**, esaminare le informazioni fornite e quindi fare clic su **Avanti**.

7.  In **Selezione servizi ruolo**, fare clic su **Server dei criteri di rete**.  In **Aggiungi funzionalità necessarie per Server dei criteri di rete**, fare clic su **Aggiungi funzionalità**. Fare clic su **Avanti**.

8.  In **Conferma selezioni per l'installazione**, fare clic su **riavvia automaticamente il server di destinazione se necessario**. Quando viene chiesto di confermare la selezione, fare clic su **Sì**, quindi fare clic su **installare**. La pagina di avanzamento installazione Visualizza stato durante il processo di installazione. Al termine del processo, il messaggio "Installazione completata in *ComputerName*" viene visualizzato, in cui *ComputerName* è il nome del computer su cui è installato Server dei criteri di rete. Fare clic su **Chiudi**.

##### <a name="BKMK_registerNPS"></a>Registrare il Server dei criteri di rete nel dominio predefinito
È possibile utilizzare questa procedura per registrare un server dei criteri di rete nel dominio in cui il server è un membro del dominio.

Server dei criteri di rete devono essere registrati in Active Directory in modo che dispongano dell'autorizzazione per leggere le proprietà di connessione remota degli account utente durante il processo di autorizzazione. La registrazione di un server dei criteri di rete consente di aggiungere il server per il **server RAS e IAS** gruppo in Active Directory.

**Credenziali amministrative**

Per completare questa procedura, è necessario essere un membro del **Domain Admins** gruppo.

> [!NOTE]
> Per eseguire questa procedura utilizzando comandi della shell (Netsh) di rete all'interno di Windows PowerShell, aprire PowerShell e digitare il comando seguente e quindi premere INVIO.
>
> `netsh nps add registeredserver domain=corp.contoso.com server=NPS1.corp.contoso.com`

###### <a name="to-register-an-nps-server-in-its-default-domain"></a>Per registrare un server dei criteri di rete nel dominio predefinito

1.  Su NPS1, in Server Manager, fare clic su strumenti e quindi fare clic su **Server dei criteri di rete**. Verrà visualizzata la finestra di MMC Server criteri di rete.

2.  Fare doppio clic su **dei criteri di rete (locale)**, quindi fare clic su **Registra server in Active Directory**. Il **Server dei criteri di rete** apre la finestra di dialogo.

3.  In **Server dei criteri di rete**, fare clic su **OK**, quindi fare clic su **OK** nuovamente.

Per ulteriori informazioni sui Server dei criteri di rete, vedere [Server dei criteri di rete (NPS)](../technologies/nps/nps-top.md).

#### <a name="BKMK_IIS"></a>Distribuzione di WEB1

Il ruolo Server Web (IIS) in Windows Server 2016 offre una piattaforma sicura, facile da gestire, modulare ed estensibile per l'hosting affidabile di siti Web, servizi e applicazioni. Con Internet Information Services (IIS), è possibile condividere informazioni con utenti su Internet, intranet o extranet. IIS è una piattaforma web unificata che integra IIS, ASP.NET, servizi FTP, PHP e Windows Communication Foundation (WCF).

Oltre a consentire di pubblicare un CRL per l'accesso da computer membri del dominio, il ruolo server Server Web (IIS) consente di configurare e gestire più siti Web, applicazioni web e i siti FTP. IIS offre i vantaggi seguenti:

-   Ottimizzare la sicurezza web grazie a un isolamento delle applicazioni di stampa e automatico piedi ridotto di server.

-   Distribuire facilmente e le applicazioni nello stesso server web di esecuzione ASP.NET, ASP classico e PHP.

-   Isolamento delle applicazioni offrendo i processi di lavoro un'identità univoca e una configurazione sandbox per impostazione predefinita, un'ulteriore riduzione dei rischi di protezione.

-   Aggiungere, rimuovere e persino sostituzione facilitate di componenti IIS con moduli personalizzati, adattati alle esigenze del cliente.

-   Consente di velocizzare il sito Web tramite predefiniti dinamica la memorizzazione nella cache e alla compressione avanzata.

Per distribuire WEB1, ovvero il computer che esegue il ruolo server Server Web (IIS), è necessario eseguire le operazioni seguenti:

-   Eseguire i passaggi nella sezione [configurazione di tutti i server](#BKMK_configuringAll).

-   Eseguire i passaggi nella sezione [aggiunta di computer Server al dominio ed effettuare l'accesso](#BKMK_joinlogserver)

-   [Installare il ruolo server Server Web (IIS)](#BKMK_install_IIS)

##### <a name="BKMK_install_IIS"></a>Installare il ruolo server Server Web (IIS)
Per completare questa procedura, è necessario essere un membro del **amministratori** gruppo.

> [!NOTE]
> Per eseguire questa procedura mediante Windows PowerShell, aprire PowerShell e digitare il comando seguente e quindi premere INVIO.
>
> `Install-WindowsFeature Web-Server -IncludeManagementTools`

1.  In **Server Manager**, fare clic su **Gestisci**, quindi fare clic su **Aggiungi ruoli e funzionalità**. Verrà avviata l'aggiunta guidata ruoli e funzionalità.

2.  In **prima di iniziare**, fare clic su **Avanti**.

    > [!NOTE]
    > Il **prima di iniziare** pagina l'aggiunta guidata ruoli e funzionalità non viene visualizzato se è stato selezionato in precedenza **Ignora questa pagina per impostazione predefinita** quando è stato eseguito l'aggiunta guidata ruoli e funzionalità.

3.  Nel **Selezione tipo di installazione** pagina, fare clic su **Avanti**.

4.  Nel **server di destinazione** pagina, assicurarsi che il computer locale sia selezionata e quindi fare clic su **Avanti**.

5.  Nel **Selezione ruoli server** pagina, scorrere e selezionare **Server Web (IIS)**. Il **Aggiungi funzionalità necessarie per Server Web (IIS)** apre la finestra di dialogo. Fare clic su **Aggiungi funzionalità**, quindi fare clic su **Avanti**.

6.  Fare clic su **Avanti** fino a quando non è stata accettata tutte l'impostazione predefinita le impostazioni del server web e quindi fare clic su **installare**.

7.  Verificare che tutte le installazioni hanno avuto esito positivo, quindi fare clic su **Chiudi**.

## <a name="BKMK_resources"></a>Risorse tecniche aggiuntive
Per ulteriori informazioni sulle tecnologie in questa Guida, vedere le risorse seguenti:

 Risorse di raccolta di documentazione tecnica di Windows Server 2012, Windows Server 2012 R2 e Windows Server 2016

-   [Novità di servizi Active Directory dominio (AD DS) in Windows Server 2016](https://technet.microsoft.com/en-us/library/mt163897.aspx)

-   [Active Directory Panoramica di servizi di dominio](https://technet.microsoft.com/library/hh831484.aspx) in https://technet.microsoft.com/library/hh831484.aspx.

-   [Panoramica di Domain Name System (DNS)](https://technet.microsoft.com/library/hh831667.aspx) in https://technet.microsoft.com/library/hh831667.aspx.

-   [Implementazione del ruolo DNS Admins](https://technet.microsoft.com/library/cc756152(WS.10).aspx)

-   [Panoramica di Dynamic Host Configuration Protocol (DHCP)](https://technet.microsoft.com/library/hh831825.aspx) in https://technet.microsoft.com/library/hh831825.aspx.

-   [Panoramica di servizi di accesso e criteri di rete](https://technet.microsoft.com/library/hh831683.aspx) in https://technet.microsoft.com/library/hh831683.aspx.

-   [Panoramica di Server (IIS) Web](https://technet.microsoft.com/library/hh831725.aspx) in https://technet.microsoft.com/library/hh831725.aspx.

## <a name="BKMK_appendix"></a>Appendici A-E
Le sezioni seguenti contengono informazioni di configurazione aggiuntive per i computer che eseguono sistemi operativi diversi da Windows Server 2016, Windows 10, Windows Server 2012 e Windows 8. Inoltre, viene fornito un foglio di lavoro di preparazione di rete per facilitare la distribuzione.

1.  [Appendice A - ridenominazione dei computer](#BKMK_A)

2.  [Appendice B - configurazione IP statico di indirizzi](#BKMK_B)

3.  [Appendice C - aggiunta di computer al dominio](#BKMK_C)

4.  [Appendice D - accesso al dominio](#BKMK_D)

5.  [Appendice E - foglio di preparazione di pianificazione di una rete Core](#BKMK_E)

## <a name="BKMK_A"></a>Appendice A - ridenominazione dei computer
È possibile utilizzare le procedure in questa sezione per fornire ai computer che esegue Windows Server 2008 R2, Windows 7, Windows Server 2008 e Windows Vista con un nome computer diverso.

-   [Windows Server 2008 R2 e Windows 7](#bkmk_NetFndtn_Pln_rename_R2)

-   [Windows Server 2008 e Windows Vista](#bkmk_NetFndtn_Pln_Renam08)

### <a name="bkmk_NetFndtn_Pln_rename_R2"></a>Windows Server 2008 R2 e Windows 7
Appartenenza al gruppo **amministratori**, o equivalente è il requisito minimo necessario per eseguire queste procedure.

##### <a name="to-rename-computers-running-windows-server-2008-r2-and-windows-7"></a>Per rinominare computer che eseguono Windows Server 2008 R2 e Windows 7

1.  Fare clic su **Start**, fare doppio clic su **Computer**, quindi fare clic su **proprietà**. Il **sistema** apre la finestra di dialogo.

2.  In **impostazioni del nome di dominio e gruppo di lavoro Computer**, fare clic su **modificare le impostazioni**. Il **proprietà del sistema** apre la finestra di dialogo.

    > [!NOTE]
    > Nei computer che eseguono Windows 7, prima di **proprietà del sistema** si apre la finestra di dialogo, il **controllo dell'Account utente** verrà visualizzata la finestra di dialogo che richiede l'autorizzazione per continuare. Fare clic su **continua** per procedere.

3.  Fare clic su **modifica**. Il **cambiamenti dominio/nome Computer** apre la finestra di dialogo.

4.  In **nome Computer**, digitare il nome del computer. Ad esempio, se si desidera che il nome computer DC1, digitare **DC1**.

5.  Fare clic su **OK** due volte, fare clic su **Chiudi**, quindi fare clic su **Riavvia ora** per riavviare il computer.

### <a name="bkmk_NetFndtn_Pln_Renam08"></a>Windows Server 2008 e Windows Vista
Appartenenza al gruppo **amministratori**, o equivalente è il requisito minimo necessario per eseguire queste procedure.

##### <a name="to-rename-computers-running-windows-server-2008-and-windows-vista"></a>Per rinominare computer che eseguono Windows Server 2008 e Windows Vista

1.  Fare clic su **Start**, fare doppio clic su **Computer**, quindi fare clic su **proprietà**. Il **sistema** apre la finestra di dialogo.

2.  In **impostazioni del nome di dominio e gruppo di lavoro Computer**, fare clic su **modificare le impostazioni**. Il **proprietà del sistema** apre la finestra di dialogo.

    > [!NOTE]
    > Nei computer che eseguono Windows Vista, prima di **proprietà del sistema** si apre la finestra di dialogo, il **controllo dell'Account utente** verrà visualizzata la finestra di dialogo che richiede l'autorizzazione per continuare. Fare clic su **continua** per procedere.

3.  Fare clic su **modifica**. Il **cambiamenti dominio/nome Computer** apre la finestra di dialogo.

4.  In **nome Computer**, digitare il nome del computer. Ad esempio, se si desidera che il nome computer DC1, digitare **DC1**.

5.  Fare clic su **OK** due volte, fare clic su **Chiudi**, quindi fare clic su **Riavvia ora** per riavviare il computer.

## <a name="BKMK_B"></a>Appendice B - configurazione IP statico di indirizzi
In questo argomento descrive le procedure per la configurazione di indirizzi IP statici in computer che eseguono sistemi operativi seguenti:

-   [Windows Server 2008 R2](#bkmk_R2Cng_WS08R2IP)

-   [Windows Server 2008](#bkmk_NetFndtn_Pln_CfgStatic08)

### <a name="bkmk_R2Cng_WS08R2IP"></a>Windows Server 2008 R2
Appartenenza al gruppo **amministratori**, o equivalente è il requisito minimo necessario per eseguire questa procedura.

##### <a name="to-configure-a-static-ip-address-on-a-computer-running-windows-server-2008-r2"></a>Per configurare un indirizzo IP statico in un computer che esegue Windows Server 2008 R2

1.  Fare clic su **Start**, quindi fare clic su **Pannello di controllo**.

2.  In **Pannello di controllo**, fare clic su **rete e Internet**. **Rete e Internet** apre.

    In **rete e Internet**, fare clic su **centro rete e condivisione**. **Centro rete e condivisione** apre.

3.  In **centro rete e condivisione**, fare clic su **modificare le impostazioni della scheda**. **Connessioni di rete** apre.

4.  In **connessioni di rete**, fare clic sulla connessione di rete che si desidera configurare, quindi fare clic su **proprietà**.

5.  In **proprietà connessione alla rete locale**, in **la connessione utilizza gli elementi seguenti**selezionare **Internet Protocol versione 4 (TCP/IPv4)**, quindi fare clic su **proprietà**. Il **proprietà protocollo IP versione 4 (TCP/IPv4)** apre la finestra di dialogo.

6.  In **proprietà protocollo IP versione 4 (TCP/IPv4)**via il **generale** scheda, fare clic su **utilizzare il seguente indirizzo IP**. In **indirizzo IP**, digitare l'indirizzo IP che si desidera utilizzare.

7.  Premere tab per posizionare il cursore **Subnet mask**. Un valore predefinito per la subnet mask viene immesso automaticamente. Accettare la subnet mask predefinita oppure digitare la subnet mask che si desidera utilizzare.

8.  In **gateway predefinito**, digitare l'indirizzo IP del gateway predefinito.

9. In **server DNS preferito**, digitare l'indirizzo IP del server DNS. Se si prevede di utilizzare il computer locale come server DNS preferito, digitare l'indirizzo IP del computer locale.

10. In **Server DNS alternativo**, digitare l'indirizzo IP del server DNS alternativo, se disponibile. Se si prevede di utilizzare il computer locale come un server DNS alternativo, digitare l'indirizzo IP del computer locale.

11. Fare clic su **OK**, quindi fare clic su **Chiudi**.

### <a name="bkmk_NetFndtn_Pln_CfgStatic08"></a>Windows Server 2008
Appartenenza al gruppo **amministratori**, o equivalente è il requisito minimo necessario per eseguire queste procedure.

##### <a name="to-configure-a-static-ip-address-on-a-computer-running-windows-server-2008"></a>Per configurare un indirizzo IP statico in un computer che esegue Windows Server 2008

1.  Fare clic su **Start**, quindi fare clic su **Pannello di controllo**.

2.  In **Pannello di controllo**, verificare che **visualizzazione classica** sia selezionata e quindi fare doppio clic su **centro rete e condivisione**.

3.  In **centro rete e condivisione**, in **attività**, fare clic su **Gestisci connessioni di rete**.

4.  In **connessioni di rete**, fare clic sulla connessione di rete che si desidera configurare, quindi fare clic su **proprietà**.

5.  In **proprietà connessione alla rete locale**, in **la connessione utilizza gli elementi seguenti**selezionare **Internet Protocol versione 4 (TCP/IPv4)**, quindi fare clic su **proprietà**. Il **proprietà protocollo IP versione 4 (TCP/IPv4)** apre la finestra di dialogo.

6.  In **proprietà protocollo IP versione 4 (TCP/IPv4)**via il **generale** scheda, fare clic su **utilizzare il seguente indirizzo IP**. In **indirizzo IP**, digitare l'indirizzo IP che si desidera utilizzare.

7.  Premere tab per posizionare il cursore **Subnet mask**. Un valore predefinito per la subnet mask viene immesso automaticamente. Accettare la subnet mask predefinita oppure digitare la subnet mask che si desidera utilizzare.

8.  In **gateway predefinito**, digitare l'indirizzo IP del gateway predefinito.

9. In **server DNS preferito**, digitare l'indirizzo IP del server DNS. Se si prevede di utilizzare il computer locale come server DNS preferito, digitare l'indirizzo IP del computer locale.

10. In **Server DNS alternativo**, digitare l'indirizzo IP del server DNS alternativo, se disponibile. Se si prevede di utilizzare il computer locale come un server DNS alternativo, digitare l'indirizzo IP del computer locale.

11. Fare clic su **OK**, quindi fare clic su **Chiudi**.

## <a name="BKMK_C"></a>Appendice C - aggiunta di computer al dominio
È possibile utilizzare queste procedure per aggiungere i computer che esegue Windows Server 2008 R2, Windows 7, Windows Server 2008 e Windows Vista al dominio.

-   [Windows Server 2008 R2 e Windows 7](#BKMK_c1)

-   [Windows Server 2008 e Windows Vista](#BKMK_c2)

> [!IMPORTANT]
> Per aggiungere un computer a un dominio, è necessario essere connessi al computer con l'account amministratore locale o, se si è connessi al computer con un account utente che non dispone di credenziali amministrative del computer locale, è necessario fornire le credenziali per l'account amministratore locale durante il processo di aggiunta del computer al dominio. Inoltre, è necessario un account utente nel dominio a cui si desidera aggiungere il computer. Durante il processo di aggiunta del computer al dominio, verrà richiesto per le credenziali dell'account di dominio (nome utente e password).

### <a name="BKMK_c1"></a>Windows Server 2008 R2 e Windows 7
Appartenenza al gruppo **Domain Users**, o equivalente è il requisito minimo necessario per eseguire questa procedura.

##### <a name="to-join-computers-running-windows-server-2008-r2-and-windows-7-to-the-domain"></a>Per aggiungere i computer che eseguono Windows Server 2008 R2 e Windows 7 al dominio

1.  Accedere al computer con l'account amministratore locale.

2.  Fare clic su **Start**, fare doppio clic su **Computer**, quindi fare clic su **proprietà**. Il **sistema** apre la finestra di dialogo.

3.  In **impostazioni del nome di dominio e gruppo di lavoro Computer**, fare clic su **modificare le impostazioni**. Il **proprietà del sistema** apre la finestra di dialogo.

    > [!NOTE]
    > Nei computer che eseguono Windows 7, prima di **proprietà del sistema** si apre la finestra di dialogo, il **controllo dell'Account utente** verrà visualizzata la finestra di dialogo che richiede l'autorizzazione per continuare. Fare clic su **continua** per procedere.

4.  Fare clic su **modifica**. Il **cambiamenti dominio/nome Computer** apre la finestra di dialogo.

5.  In **nome Computer**, in **appartenente**selezionare **dominio**e quindi digitare il nome del dominio di cui si desidera aggiungere. Ad esempio, se il nome di dominio è corp.contoso.com, digitare **corp.contoso.com**.

6.  Fare clic su **OK**. Il **la protezione di Windows** apre la finestra di dialogo.

7.  In **cambiamenti dominio/nome Computer**, in **nome utente**, digitare il nome utente e in **Password**, digitare la password e quindi fare clic su **OK**. Il **cambiamenti dominio/nome Computer** si apre la finestra di dialogo, benvenuto al dominio. Fare clic su **OK**.

8.  Il **cambiamenti dominio/nome Computer** la finestra di dialogo viene visualizzato un messaggio che indica che è necessario riavviare il computer per rendere effettive le modifiche. Fare clic su **OK**.

9. Nel **proprietà del sistema** della finestra di dialogo di **nome Computer** scheda, fare clic su **Chiudi**. Il **Microsoft Windows** la finestra di dialogo verrà visualizzato un messaggio per segnalare nuovamente che è necessario riavviare il computer per rendere effettive le modifiche. Fare clic su **Riavvia ora**.

### <a name="BKMK_c2"></a>Windows Server 2008 e Windows Vista
Appartenenza al gruppo **Domain Users**, o equivalente è il requisito minimo necessario per eseguire questa procedura.

##### <a name="to-join-computers-running-windows-server-2008-and-windows-vista-to-the-domain"></a>Per aggiungere i computer che eseguono Windows Server 2008 e Windows Vista al dominio

1.  Accedere al computer con l'account amministratore locale.

2.  Fare clic su **Start**, fare doppio clic su **Computer**, quindi fare clic su **proprietà**. Il **sistema** apre la finestra di dialogo.

3.  In **impostazioni del nome di dominio e gruppo di lavoro Computer**, fare clic su **modificare le impostazioni**. Il **proprietà del sistema** apre la finestra di dialogo.

4.  Fare clic su **modifica**. Il **cambiamenti dominio/nome Computer** apre la finestra di dialogo.

5.  In **nome Computer**, in **appartenente**selezionare **dominio**e quindi digitare il nome del dominio di cui si desidera aggiungere. Ad esempio, se il nome di dominio è corp.contoso.com, digitare **corp.contoso.com**.

6.  Fare clic su **OK**. Il **la protezione di Windows** apre la finestra di dialogo.

7.  In **cambiamenti dominio/nome Computer**, in **nome utente**, digitare il nome utente e in **Password**, digitare la password e quindi fare clic su **OK**. Il **cambiamenti dominio/nome Computer** si apre la finestra di dialogo, benvenuto al dominio. Fare clic su **OK**.

8.  Il **cambiamenti dominio/nome Computer** la finestra di dialogo viene visualizzato un messaggio che indica che è necessario riavviare il computer per rendere effettive le modifiche. Fare clic su **OK**.

9. Nel **proprietà del sistema** della finestra di dialogo di **nome Computer** scheda, fare clic su **Chiudi**. Il **Microsoft Windows** la finestra di dialogo verrà visualizzato un messaggio per segnalare nuovamente che è necessario riavviare il computer per rendere effettive le modifiche. Fare clic su **Riavvia ora**.

## <a name="BKMK_D"></a>Appendice D - accesso al dominio
È possibile utilizzare queste procedure per accedere al dominio utilizzando computer che eseguono Windows Server 2008 R2, Windows 7, Windows Server 2008 e Windows Vista.

-   [Windows Server 2008 R2 e Windows 7](#BKMK_d1)

-   [Windows Server 2008 e Windows Vista](#BKMK_d2)

### <a name="BKMK_d1"></a>Windows Server 2008 R2 e Windows 7
Appartenenza al gruppo **Domain Users**, o equivalente è il requisito minimo necessario per eseguire questa procedura.

##### <a name="log-on-to-the-domain-using-computers-running-windows-server-2008-r2-and-windows-7"></a>Accedere al dominio utilizzando computer che eseguono Windows Server 2008 R2 e Windows 7

1.  Disconnettersi dal computer o riavviare il computer.

2.  Premere CTRL + ALT + CANC. Verrà visualizzata la schermata di accesso.

3.  Fare clic su **Cambia utente**, quindi fare clic su **altro utente**.

4.  In **nome utente**, digitare il nome di dominio e dell'utente nel formato *dominio\utente*. Ad esempio, per accedere al dominio con un account denominato corp.contoso.com **utente-01**, tipo **corp\utente-01**.

5.  In **Password**, digitare la password di dominio e quindi fare clic sulla freccia o premere INVIO.

### <a name="BKMK_d2"></a>Windows Server 2008 e Windows Vista
Appartenenza al gruppo **Domain Users**, o equivalente è il requisito minimo necessario per eseguire questa procedura.

##### <a name="log-on-to-the-domain-using-computers-running-windows-server-2008-and-windows-vista"></a>Accedere al dominio utilizzando computer che eseguono Windows Server 2008 e Windows Vista

1.  Disconnettersi dal computer o riavviare il computer.

2.  Premere CTRL + ALT + CANC. Verrà visualizzata la schermata di accesso.

3.  Fare clic su **Cambia utente**, quindi fare clic su **altro utente**.

4.  In **nome utente**, digitare il nome di dominio e dell'utente nel formato *dominio\utente*. Ad esempio, per accedere al dominio con un account denominato corp.contoso.com **utente-01**, tipo **corp\utente-01**.

5.  In **Password**, digitare la password di dominio e quindi fare clic sulla freccia o premere INVIO.

## <a name="BKMK_E"></a>Appendice E - foglio di preparazione di pianificazione di una rete Core
È possibile utilizzare questo foglio di preparazione alla pianificazione della rete per raccogliere le informazioni necessarie per installare una rete core. Questo argomento fornisce le tabelle contenenti i singoli elementi di configurazione per ogni computer del server per cui è necessario fornire informazioni o valori specifici durante il processo di installazione o configurazione. I valori di esempio vengono forniti per ogni elemento di configurazione.

Per la pianificazione e la registrazione, gli spazi sono incluse in ogni tabella è possibile inserire i valori utilizzati per la distribuzione. Se Esegui l'accesso in queste tabelle valori correlati alla sicurezza, è consigliabile archiviare le informazioni in un luogo sicuro.

I collegamenti seguenti rimandano alle sezioni in questo argomento che forniscono gli elementi di configurazione e i valori di esempio associati alle procedure di distribuzione descritte in questa Guida.

1.  [Installazione di servizi di dominio Active Directory e DNS](#BKMK_FndtnPrep_InstallAD)

    -   [Configurazione di una zona di ricerca inversa DNS](#BKMK_FndtnPrep_DNSRevrsLook)

2.  [Installazione di DHCP](#BKMK_FndtnPrep_InstallDHCP)

    -   [Creazione di un intervallo di esclusione in DHCP](#BKMK_FndtnPrep_DHCP_Exclusn)

    -   [Creazione di un nuovo ambito DHCP](#bkmk_NetFndtn_Pln_DHCP_NewScope)

3.  [L'installazione di Server dei criteri di rete (facoltativo)](#BKMK_FndtnPrep_InstallNPS)

### <a name="BKMK_FndtnPrep_InstallAD"></a>Installazione di servizi di dominio Active Directory e DNS
Le tabelle in questa sezione sono elencati gli elementi di configurazione per la preinstallazione e installazione di servizi di dominio Active Directory (AD DS) e DNS.

##### <a name="pre-installation-configuration-items-for-ad-ds-and-dns"></a>Elementi di configurazione di preinstallazione per Active Directory e DNS
Nelle tabelle seguenti sono gli elementi di configurazione di pre-installazione elenco come descritto in [configurazione di tutti i server](#BKMK_configuringAll):

-   [Configurare un indirizzo IP statico](#BKMK_ip)

|Elementi di configurazione|Valori di esempio|Valori|
|-----------------------|------------------|----------|
|Indirizzo IP|10.0.0.2||
|La subnet mask|255.255.255.0||
|Gateway predefinito|10.0.0.1||
|Server DNS preferito|127.0.0.1||
|Server DNS alternativo|10.0.0.15||

-   [Rinominare il Computer](#BKMK_rename)

|Elemento di configurazione|Valore di esempio|Valore|
|----------------------|-----------------|---------|
|Nome del computer|DC1||

##### <a name="ad-ds-and-dns-installation-configuration-items"></a>Elementi di configurazione di installazione di Active Directory e DNS
Elementi di configurazione per la procedura di distribuzione di rete di Windows Server Core [installare Active Directory e DNS per una nuova foresta](#BKMK_installAD-DNS):

|Elementi di configurazione|Valori di esempio|Valori|
|-----------------------|------------------|----------|
|Nome DNS completo|corp.contoso.com||
|Livello di funzionalità foresta|Windows Server 2003||
|Percorso della cartella del database di servizi di dominio Active Directory|E:\Configuration\\<br /><br />Oppure accettare il percorso predefinito.||
|Percorso della cartella file di log servizi di dominio Active Directory|E:\Configuration\\<br /><br />Oppure accettare il percorso predefinito.||
|Percorso della cartella Active Directory SYSVOL di servizi di dominio|E:\Configuration\\<br /><br />Oppure accettare il percorso predefinito||
|Password di amministratore modalità ripristino di directory|J * p2leO4$ F||
|Nome file di risposte (facoltativo)|Ds_answerfile di Active Directory||

#### <a name="BKMK_FndtnPrep_DNSRevrsLook"></a>Configurazione di una zona di ricerca inversa DNS

|Elementi di configurazione|Valori di esempio|Valori|
|-----------------------|------------------|----------|
|Tipo di zona:|-Zona principale<br />-Zona secondaria<br />: Zona di stub||
|Tipo di zona<br /><br />**Archivia la zona in Active Directory**|-Selezionato<br />-Non selezionata||
|Ambito di replica di Active Directory zona|-Per tutti i server DNS della foresta<br />-Per tutti i server DNS nel dominio<br />-Per tutti i controller di dominio nel dominio<br />-Per tutti i controller di dominio specificati nell'ambito di questa partizione di directory||
|Nome della zona di ricerca inversa<br /><br />(Tipo IP)|-Zona di ricerca inversa IPv4<br />-Zona di ricerca inversa IPv6||
|Nome della zona di ricerca inversa<br /><br />(ID rete)|10.0.0||

### <a name="BKMK_FndtnPrep_InstallDHCP"></a>Installazione di DHCP
Le tabelle in questa sezione sono elencati gli elementi di configurazione per la preinstallazione e installazione di DHCP.

##### <a name="pre-installation-configuration-items-for-dhcp"></a>Elementi di configurazione pre-installazione per DHCP
Nelle tabelle seguenti sono gli elementi di configurazione di pre-installazione elenco come descritto in [configurazione di tutti i server](#BKMK_configuringAll):

-   [Configurare un indirizzo IP statico](#BKMK_ip)

|Elementi di configurazione|Valori di esempio|Valori|
|-----------------------|------------------|----------|
|Indirizzo IP|10.0.0.3||
|La subnet mask|255.255.255.0||
|Gateway predefinito|10.0.0.1||
|Server DNS preferito|10.0.0.2||
|Server DNS alternativo|10.0.0.15||

-   [Rinominare il Computer](#BKMK_rename)

|Elemento di configurazione|Valore di esempio|Valore|
|----------------------|-----------------|---------|
|Nome del computer|DHCP1||

##### <a name="dhcp-installation-configuration-items"></a>Elementi di configurazione di installazione di DHCP
Elementi di configurazione per la procedura di distribuzione di rete di Windows Server Core [installare Dynamic Host Configuration Protocol (DHCP)](#BKMK_installDHCP):

|Elementi di configurazione|Valori di esempio|Valori|
|-----------------------|------------------|----------|
|Binding connessioni di rete|Ethernet||
|Impostazioni del server DNS|DC1||
|Indirizzo IP del server DNS preferito|10.0.0.2||
|Indirizzo IP del server DNS alternativo|10.0.0.15||
|Nome ambito|Corp1||
|Indirizzo IP iniziale|10.0.0.1||
|Indirizzo IP finale|10.0.0.254||
|La subnet mask|255.255.255.0||
|Gateway predefinito (facoltativo)|10.0.0.1||
|Durata del lease|8 giorni||
|Modalità operativa di server DHCP IPv6|Non è abilitato||

#### <a name="BKMK_FndtnPrep_DHCP_Exclusn"></a>Creazione di un intervallo di esclusione in DHCP
Elementi di configurazione per creare un intervallo di esclusione durante la creazione di un ambito in DHCP.

|Elementi di configurazione|Valori di esempio|Valori|
|-----------------------|------------------|----------|
|Nome ambito|Corp1||
|Descrizione dell'ambito|Subnet 1 sede||
|Indirizzo IP di inizio intervallo di esclusione|10.0.0.1||
|Indirizzo IP finale intervallo di esclusione|10.0.0.15||

#### <a name="bkmk_NetFndtn_Pln_DHCP_NewScope"></a>Creazione di un nuovo ambito DHCP
Elementi di configurazione per la procedura di distribuzione di rete di Windows Server Core [creare e attivare un nuovo ambito DHCP](#BKMK_newscopeDHCP):

|Elementi di configurazione|Valori di esempio|Valori|
|-----------------------|------------------|----------|
|Nome del nuovo ambito|Corp2||
|Descrizione dell'ambito|Subnet dell'ufficio principale 2||
|(Intervallo di indirizzi IP)<br /><br />Indirizzo IP iniziale|10.0.1.1||
|(Intervallo di indirizzi IP)<br /><br />Indirizzo IP finale|10.0.1.254||
|Lunghezza|8||
|La subnet mask|255.255.255.0||
|(Intervallo di esclusione) Indirizzo IP iniziale|10.0.1.1||
|Indirizzo IP finale intervallo di esclusione|10.0.1.15||
|Durata del lease<br /><br />Giorni<br /><br />Ore<br /><br />Minuti|-   8<br />-   0<br />-   0||
|Router (gateway predefinito)<br /><br />Indirizzo IP|10.0.1.1||
|Dominio padre DNS|corp.contoso.com||
|Server DNS<br /><br />Indirizzo IP|10.0.0.2||

### <a name="BKMK_FndtnPrep_InstallNPS"></a>L'installazione di Server dei criteri di rete (facoltativo)
Le tabelle in questa sezione sono elencati gli elementi di configurazione per la preinstallazione e installazione di criteri di rete.

##### <a name="pre-installation-configuration-items"></a>Elementi di configurazione pre-installazione
Le tre tabelle seguenti sono elencati gli elementi di configurazione pre-installazione come descritto in [configurazione di tutti i server](#BKMK_configuringAll):

-   [Configurare un indirizzo IP statico](#BKMK_ip)

|Elementi di configurazione|Valori di esempio|Valori|
|-----------------------|------------------|----------|
|Indirizzo IP|10.0.0.4||
|La subnet mask|255.255.255.0||
|Gateway predefinito|10.0.0.1||
|Server DNS preferito|10.0.0.2||
|Server DNS alternativo|10.0.0.15||

-   [Rinominare il Computer](#BKMK_rename)

|Elemento di configurazione|Valore di esempio|Valore|
|----------------------|-----------------|---------|
|Nome del computer|NPS1||

##### <a name="network-policy-server-installation-configuration-items"></a>Gli elementi di configurazione di installazione di Server dei criteri di rete
Elementi di configurazione per le procedure di distribuzione dei criteri di rete di Windows Server Core rete [installare Server criteri di rete (NPS)](#BKMK_installNPS) e [registrare il Server dei criteri di rete nel dominio predefinito](#BKMK_registerNPS).

-   Ulteriori elementi di configurazione non sono necessari per installare e registrare i criteri di rete.

