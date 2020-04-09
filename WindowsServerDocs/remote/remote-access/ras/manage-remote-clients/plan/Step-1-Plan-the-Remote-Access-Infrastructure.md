---
title: Passaggio 1 pianificare l'infrastruttura di accesso remoto
description: Questo argomento fa parte della Guida relativa alla gestione remota dei client DirectAccess in Windows Server 2016.
manager: brianlic
ms.prod: windows-server
ms.technology: networking-ras
ms.topic: article
ms.assetid: a1ce7af5-f3fe-4fc9-82e8-926800e37bc1
ms.author: lizross
author: eross-msft
ms.openlocfilehash: 2df4d64727de403264258e2f377e3df94b9b3d25
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80860584"
---
# <a name="step-1-plan-the-remote-access-infrastructure"></a>Passaggio 1 pianificare l'infrastruttura di accesso remoto

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016

> [!NOTE]
> Windows Server 2016 combina DirectAccess e il servizio Routing e accesso remoto (RRAS) in un singolo ruolo accesso remoto.  
  
In questo argomento vengono descritti i passaggi per la pianificazione di un'infrastruttura che è possibile utilizzare per configurare un singolo server di accesso remoto per la gestione remota dei client DirectAccess. Nella tabella seguente sono elencati i passaggi, ma queste attività di pianificazione non devono essere eseguite in un ordine specifico.  
  
|Attività|Descrizione|  
|----|--------|  
|[Pianificare la topologia di rete e le impostazioni del server](#plan-network-topology-and-settings)|Decidere dove collocare il server di accesso remoto (alla periferia o dietro un dispositivo NAT (Network Address Translation) o un firewall) e pianificare gli indirizzi IP e il routing.|  
|[Pianificare i requisiti del firewall](#plan-firewall-requirements)|Pianificare il passaggio di Accesso remoto attraverso i firewall perimetrali.|  
|[Pianificare i requisiti dei certificati](#plan-certificate-requirements)|Decidere se usare il protocollo Kerberos o i certificati per l'autenticazione client e pianificare i certificati del sito Web.<br/><br/>IP-HTTPS è un protocollo di transizione usato dai client DirectAccess per eseguire il tunneling del traffico IPv6 in reti IPv4. Decidere se autenticare IP-HTTPS per il server usando un certificato emesso da un'autorità di certificazione (CA) o usando un certificato autofirmato emesso automaticamente dal server di accesso remoto.|  
|[Pianificare i requisiti DNS](#plan-dns-requirements)|Pianificare le impostazioni di Domain Name System (DNS) per il server di accesso remoto, i server di infrastruttura, le opzioni di risoluzione dei nomi locali e la connettività client.| 
|[Pianificare la configurazione del server dei percorsi di rete](#plan-the-network-location-server-configuration)|Decidere dove collocare il sito Web del server dei percorsi di rete nell'organizzazione (nel server di accesso remoto o in un server alternativo) e pianificare i requisiti dei certificati se il server dei percorsi di rete si trova nel server di accesso remoto. **Nota:** Il server dei percorsi di rete viene usato dai client DirectAccess per determinare se si trovano nella rete interna.|  
|[Pianificare le configurazioni dei server di gestione](#plan-management-servers-configuration)|Pianificare i server di gestione, ad esempio i server di aggiornamento, utilizzati durante la gestione remota dei client. **Nota:** Gli amministratori possono gestire in remoto i computer client DirectAccess che si trovano all'esterno della rete aziendale tramite Internet.|  
|[Pianificare i requisiti di Active Directory](#plan-active-directory-requirements)|Pianificare i controller di dominio, i requisiti di Active Directory, l'autenticazione client e la struttura di più domini.|  
|[Pianificare Criteri di gruppo la creazione di oggetti](#plan-group-policy-object-creation)|Decidere quali oggetti Criteri di gruppo sono necessari nell'organizzazione e come creare e modificare gli oggetti Criteri di gruppo.|  
  
## <a name="plan-network-topology-and-settings"></a>Pianificare la topologia di rete e le impostazioni  
Quando si pianifica la rete, è necessario prendere in considerazione la topologia delle schede di rete, le impostazioni per gli indirizzi IP e i requisiti per ISATAP.  
  
### <a name="plan-network-adapters-and-ip-addressing"></a>Pianificare le schede di rete e gli indirizzi IP  
  
1.  Identificare la topologia delle schede di rete che si desidera utilizzare. Accesso remoto può essere configurato con una delle seguenti topologie:  
  
    -   Con due schede di rete: il server di accesso remoto viene installato sul perimetro con una scheda di rete connessa a Internet e l'altra alla rete interna.  
  
    -   Con due schede di rete: il server di accesso remoto viene installato dietro un dispositivo NAT, un firewall o un router, con una scheda di rete connessa a una rete perimetrale e l'altra alla rete interna.  
  
    -   Con una scheda di rete: il server di accesso remoto viene installato dietro un dispositivo NAT e l'unica scheda di rete è connessa alla rete interna.  
  
2.  Identificare i requisiti degli indirizzi IP:  
  
    DirectAccess usa IPv6 con IPsec per creare una connessione sicura tra i computer client DirectAccess e la rete aziendale interna. Tuttavia, DirectAccess non richiede necessariamente la connettività a Internet IPv6 o il supporto IPv6 nativo nelle reti interne, Al contrario, configura e utilizza automaticamente le tecnologie di transizione IPv6 per eseguire il tunneling del traffico IPv6 attraverso la rete Internet IPv4 (6to4, Teredo o IP-HTTPS) e nell'Intranet solo IPv4 (NAT64 o ISATAP). Per una panoramica di queste tecnologie di transizione, vedere le seguenti risorse:  
  
    -   [Tecnologie di transizione IPv6](https://technet.microsoft.com/library/bb726951.aspx)  
  
    -   [Specifica del protocollo di tunneling IP-HTTPS](https://msdn.microsoft.com/library/dd358571.aspx)  
  
3.  Configurare le schede e gli indirizzi necessari in base alla seguente tabella. Per le distribuzioni che si trovano dietro un dispositivo NAT che usa una singola scheda di rete, configurare gli indirizzi IP usando solo la colonna della **scheda di rete interna** .  
  
    ||Scheda di rete esterna|Scheda di rete interna<sup>1, sopra</sup>|Requisiti di routing|  
    |-|--------------|------------------------|------------|  
    |Internet IPv4 e Intranet IPv4|Configurare le opzioni seguenti:<br/><br/>-Due indirizzi IPv4 pubblici consecutivi statici con le subnet mask appropriate (necessario solo per Teredo).<br/>: Indirizzo IPv4 del gateway predefinito per il firewall Internet o il router del provider di servizi Internet (ISP) locale. **Nota:** Il server di accesso remoto richiede due indirizzi IPv4 pubblici consecutivi in modo che possa fungere da server Teredo e i client Teredo basati su Windows possono usare il server di accesso remoto per rilevare il tipo di dispositivo NAT.|Configurare le opzioni seguenti:<br/><br/>-Un indirizzo intranet IPv4 con subnet mask appropriata.<br/>: Un suffisso DNS specifico della connessione per lo spazio dei nomi Intranet. Configurare anche un server DNS nell'interfaccia interna. **Attenzione:** non si configura un gateway predefinito in alcuna interfaccia intranet.|Per configurare il server di accesso remoto per raggiungere tutte le subnet nella rete IPv4 interna, eseguire le operazioni seguenti:<br/><br/>: Elenca gli spazi di indirizzi IPv4 per tutti i percorsi nella rete intranet.<br/>-Usare i comandi `route add -p` o `netsh interface ipv4 add route` per aggiungere gli spazi degli indirizzi IPv4 come route statiche nella tabella di routing IPv4 del server di accesso remoto.|  
    |Internet IPv6 e Intranet IPv6|Configurare le opzioni seguenti:<br/><br/>-Usare la configurazione degli indirizzi configurata automaticamente fornita dall'ISP.<br/>-Utilizzare il comando `route print` per assicurarsi che esista una route IPv6 predefinita che punta al router ISP nella tabella di routing IPv6.<br/>-Determinare se i router ISP e Intranet utilizzano le preferenze del router predefinite, come descritto in RFC 4191, e se utilizzano una preferenza predefinita superiore rispetto ai router Intranet locali. Se entrambe queste condizioni sono vere, non sono richieste altre configurazioni per la route predefinita. La preferenza superiore per il router ISP garantisce che la route IPv6 predefinita attiva del server di Accesso remoto punta alla rete Internet IPv6.<br/><br/>Poiché il server di Accesso remoto è un router IPv6, se si ha un'infrastruttura IPv6 nativa, anche l'interfaccia Internet può raggiungere i controller di dominio sulla Intranet. In questo caso, aggiungere filtri pacchetti al controller di dominio nella rete perimetrale per impedire la connettività all'indirizzo IPv6 dell'interfaccia Internet del server di accesso remoto.|Configurare le opzioni seguenti:<br/><br/>Se non si utilizzano i livelli di preferenza predefiniti, configurare le interfacce Intranet utilizzando il comando `netsh interface ipv6 set InterfaceIndex ignoredefaultroutes=enabled`. Il comando verifica che route predefinite aggiuntive che puntano ai router della Intranet non verranno aggiunte alla tabella di routing IPv6. È possibile ottenere il IndiceInterfaccia delle interfacce Intranet dalla visualizzazione del comando `netsh interface show interface`.|Se si ha una Intranet IPv6, per configurare il server di Accesso remoto in modo da raggiungere tutti i percorsi IPv6, eseguire le operazioni seguenti:<br/><br/>: Elenca gli spazi degli indirizzi IPv6 per tutti i percorsi nella rete intranet.<br/>-Utilizzare il `netsh interface ipv6 add route` comando per aggiungere gli spazi degli indirizzi IPv6 come route statiche nella tabella di routing IPv6 del server di accesso remoto.|  
    |Internet IPv4 e Intranet IPv6|Il server di accesso remoto inoltra il traffico della route IPv6 predefinita usando l'interfaccia della scheda Microsoft 6to4 a un relay 6to4 nella rete Internet IPv4. Quando IPv6 nativo non è distribuito nella rete aziendale, è possibile usare il comando seguente per configurare un server di accesso remoto per l'indirizzo IPv4 del relè Microsoft 6to4 nella rete Internet IPv4: `netsh interface ipv6 6to4 set relay name=<ipaddress> state=enabled`.|||  
  
    > [!NOTE]  
    > -   Se al client DirectAccess è stato assegnato un indirizzo IPv4 pubblico, utilizzerà la tecnologia di inoltro 6to4 per connettersi alla Intranet. Se al client viene assegnato un indirizzo IPv4 privato, userà Teredo. Se il client DirectAccess non riesce a connettersi al server DirectAccess con 6to4 o Teredo, userà IP-HTTPS.  
    > -   Per usare Teredo, è necessario configurare due indirizzi IP consecutivi nella scheda di rete con connessione esterna.  
    > -   Non è possibile utilizzare Teredo se il server di accesso remoto dispone di una sola scheda di rete.  
    > -   Il computer client IPv6 nativo può connettersi al server di Accesso remoto con un IPv6 nativo e non è richiesta alcuna tecnologia di transizione.  
  
### <a name="plan-isatap-requirements"></a>Pianificare i requisiti ISATAP  
ISATAP è necessario per la gestione remota di DirectAccessclients, in modo che i server di Gestione DirectAccess possano connettersi ai client DirectAccess che si trovano su Internet. ISATAP non è necessario per supportare le connessioni avviate dai computer client DirectAccess alle risorse IPv4 nella rete aziendale. Per questo scopo, viene usato NAT64/DNS64. Se la distribuzione richiede ISATAP, usare la tabella seguente per identificare i requisiti.  
  
|Scenario di distribuzione ISATAP|Requisiti|  
|---------------|--------|  
|Intranet IPv6 nativa esistente (non è richiesto alcun ISATAP)|Con un'infrastruttura IPv6 nativa esistente, è possibile specificare il prefisso dell'organizzazione durante la distribuzione di accesso remoto e il server di accesso remoto non si configura come router ISATAP. eseguire le operazioni descritte di seguito.<br/><br/>1. per assicurarsi che i client DirectAccess siano raggiungibili dalla Intranet, è necessario modificare il routing IPv6 in modo che il traffico della route predefinita venga inoltrato al server di accesso remoto. Se lo spazio degli indirizzi IPv6 della Intranet utilizza un indirizzo diverso da un singolo prefisso di indirizzo IPv6 a 48 bit, è necessario specificare il prefisso IPv6 dell'organizzazione pertinente durante la distribuzione.<br/>2. Se si è attualmente connessi alla rete Internet IPv6, è necessario configurare il traffico della route predefinita in modo che venga inoltrato al server di accesso remoto e quindi configurare le connessioni e le route appropriate nel server di accesso remoto, in modo che la route predefinita il traffico viene inviato al dispositivo connesso alla rete Internet IPv6.|  
|Distribuzione ISATAP esistente|Se si dispone di un'infrastruttura ISATAP esistente, durante la distribuzione viene richiesto il prefisso a 48 bit dell'organizzazione e il server di accesso remoto non si configura come router ISATAP. Per assicurarsi che i client DirectAccess siano raggiungibili dalla Intranet, è necessario modificare l'infrastruttura di routing IPv6 in modo che il traffico della route predefinita venga inoltrato al server di accesso remoto. Questa modifica deve essere eseguita sul router ISATAP esistente a cui i client Intranet devono già inviare il traffico predefinito.|  
|Nessuna connettività IPv6 esistente|Quando la configurazione guidata accesso remoto rileva che il server non dispone di connettività IPv6 nativa o basata su ISATAP, viene automaticamente derivato un prefisso a 48 bit basato su 6to4 per la Intranet e il server di accesso remoto viene configurato come router ISATAP per fornire IPv6 connettività agli host ISATAP nell'Intranet. Un prefisso basato su 6to4 viene utilizzato solo se il server dispone di indirizzi pubblici; in caso contrario, il prefisso viene generato automaticamente da un intervallo di indirizzi locali univoco.<br/><br/>Per utilizzare ISATAP, procedere come segue:<br/><br/>1. registrare il nome ISATAP in un server DNS per ogni dominio in cui si desidera abilitare la connettività basata su ISATAP, in modo che il nome ISATAP sia risolvibile dal server DNS interno all'indirizzo IPv4 interno del server di accesso remoto.<br/>2. per impostazione predefinita, i server DNS che eseguono Windows Server 2012, Windows Server 2008 R2, Windows Server 2008 o Windows Server 2003 bloccano la risoluzione del nome ISATAP con l'elenco globale delle query da bloccare. Per abilitare ISATAP, è necessario rimuovere il nome ISATAP dall'elenco dei blocchi. Per ulteriori informazioni, vedere [rimuovere ISATAP dall'elenco globale delle query DNS da bloccare](https://go.microsoft.com/fwlink/p/?LinkId=168593).<br/><br/>Gli host ISATAP basati su Windows in grado di risolvere il nome ISATAP configurano automaticamente un indirizzo con il server di accesso remoto come segue:<br/><br/>1. indirizzo IPv6 basato su ISATAP in un'interfaccia di tunneling ISATAP<br/>2. Route a 64 bit che fornisce la connettività agli altri host ISATAP nella Intranet<br/>3. route IPv6 predefinita che punta al server di accesso remoto. La route predefinita garantisce che gli host ISATAP Intranet possano raggiungere i client DirectAccess<br/><br/>Quando gli host ISATAP basati su Windows ottengono un indirizzo IPv6 basato su ISATAP, iniziano a utilizzare il traffico incapsulato da ISATAP per comunicare se la destinazione è anche un host ISATAP. Poiché ISATAP usa una singola subnet a 64 bit per l'intera Intranet, la comunicazione passa da un modello di comunicazione IPv4 segmentato a un modello di comunicazione a subnet singola con IPv6. Questo può influire sul comportamento di alcuni Active Directory Domain Services (AD DS) e delle applicazioni che si basano sulla configurazione dei siti e dei servizi di Active Directory. Se, ad esempio, si utilizza lo snap-in siti e servizi Active Directory per configurare siti, subnet basate su IPv4 e trasporti tra siti per l'invio di richieste ai server all'interno dei siti, questa configurazione non viene utilizzata dagli host ISATAP.<br/><br/><ol><li>Per configurare Active Directory siti e servizi per l'invio all'interno di siti per gli host ISATAP, per ogni oggetto subnet IPv4 è necessario configurare un oggetto subnet IPv6 equivalente, in cui il prefisso dell'indirizzo IPv6 per la subnet esprime lo stesso intervallo di host ISATAP indirizzi come subnet IPv4. Ad esempio, per la subnet IPv4 192.168.99.0/24 e il prefisso dell'indirizzo ISATAP 64 bit 2002:836b: 1:8000::/64, il prefisso dell'indirizzo IPv6 equivalente per l'oggetto subnet IPv6 è 2002:836b: 1:8000:0: 5EFE: 192.168.99.0/120. Per una lunghezza del prefisso IPv4 arbitraria (impostata su 24 nell'esempio), è possibile determinare la lunghezza del prefisso IPv6 corrispondente dalla formula 96 + IPv4PrefixLength.</li><li>Per gli indirizzi IPv6 dei client DirectAccess, aggiungere quanto segue:<br/><br/><ul><li>Per i client DirectAccess basati su Teredo: una subnet IPv6 per l'intervallo 2001:0: WWXX: YYZZ::/64, in cui WWXX: YYZZ è la versione esadecimale con due punti del primo indirizzo IPv4 con connessione Internet del server di accesso remoto. .</li><li>Per i client DirectAccess basati su IP-HTTPS: una subnet IPv6 per l'intervallo 2002: WWXX: YYZZ: 8100::/56, in cui WWXX: YYZZ è la versione esadecimale con due punti del primo indirizzo IPv4 con connessione Internet (w. x. y. z) del server di accesso remoto. .</li><li>Per i client DirectAccess basati su 6to4: una serie di prefissi IPv6 basati su 6to4 che iniziano con 2002: e rappresentano i prefissi degli indirizzi IPv4 pubblici internazionali, gestiti da IANA (Internet Assigned Numbers Authority) e registri regionali. Il prefisso basato su 6to4 per un prefisso di indirizzo IPv4 pubblico w.x.y.z/n è 2002:WWXX:YYZZ::/[16 +n] in cui WWXX:YYZZ è la versione esadecimale con due punti di w.x.y.z.<br/><br/>        Ad esempio, l'intervallo 7.0.0.0/8 è amministrato da ARIN (American Registry for Internet Numbers (ARIN) per il Nord America. Il prefisso basato su 6to4 corrispondente per questo intervallo di indirizzi IPv6 pubblici è 2002:700::/24. Per informazioni sullo spazio degli indirizzi pubblici IPv4, vedere il [Registro di sistema per lo spazio degli indirizzi IANA IPv4](https://www.iana.org/assignments/ipv4-address-space/ipv4-address-space.xml). .</li></ul></li></ol>|  
  
> [!IMPORTANT]  
> Assicurarsi che non siano presenti indirizzi IP pubblici sull'interfaccia interna del server DirectAccess. Se si dispone di un indirizzo IP pubblico sull'interfaccia interna, la connettività tramite ISATAP potrebbe non riuscire.  
  
### <a name="plan-firewall-requirements"></a>Pianificare i requisiti del firewall  
Se il server di Accesso remoto è protetto da un firewall periferico, sono necessarie le seguenti eccezioni per il traffico di Accesso remoto quando il server di Accesso remoto si trova nella rete Internet IPv4:  
  
-   Per IP-HTTPS: Transmission Control Protocol (TCP) porta di destinazione 443 e porta TCP di origine 443 in uscita.  
  
-   Per il traffico Teredo: porta di destinazione UDP (User Datagram Protocol) 3544 in ingresso e porta UDP di origine 3544 in uscita.  
  
-   Per il traffico 6to4: protocollo IP 41 in ingresso e in uscita.  
  
    > [!NOTE]  
    > Per il traffico Teredo e 6to4, tali esenzioni devono essere applicate per entrambi gli indirizzi IPv4 pubblici consecutivi con connessione Internet sul server di Accesso remoto.  
    >   
    > Per IP-HTTPS le eccezioni devono essere applicate all'indirizzo registrato nel server DNS pubblico.  
  
-   Se si distribuisce accesso remoto con una singola scheda di rete e si installa il server dei percorsi di rete nel server di accesso remoto, porta TCP 62000.  
  
    > [!NOTE]  
    > Questa esenzione si trova nel server di accesso remoto e le esenzioni precedenti si trovano sul firewall perimetrale.  
  
Le seguenti eccezioni sono necessarie per il traffico di accesso remoto quando il server di accesso remoto si trova nella rete Internet IPv6:  
  
-   Protocollo IP 50  
  
-   Porta UDP di destinazione 500 in entrata e porta UDP di origine 500 in uscita.  
  
-   Traffico ICMPv6 in ingresso e in uscita (solo quando si usa Teredo).  
  
Quando si usano altri firewall, applicare le seguenti eccezioni del firewall di rete interna per il traffico di accesso remoto:  
  
-   Per ISATAP: protocollo 41 in ingresso e in uscita  
  
-   Per tutto il traffico IPv4/IPv6: TCP/UD  
  
-   Per Teredo: ICMP per tutto il traffico IPv4/IPv6  
  
### <a name="plan-certificate-requirements"></a>Pianificare i requisiti dei certificati  
Esistono tre scenari che richiedono certificati quando si distribuisce un singolo server di accesso remoto.  
  
-   **Autenticazione IPSec**: i requisiti dei certificati per IPsec includono un certificato del computer usato dai computer client DirectAccess quando stabiliscono la connessione IPSec con il server di accesso remoto e un certificato del computer usato dai server di accesso remoto per stabilire le connessioni IPSec con i client DirectAccess.  
  
    Per DirectAccess in Windows Server 2012, l'uso di questi certificati IPsec non è obbligatorio. In alternativa, il server di accesso remoto può fungere da proxy per l'autenticazione Kerberos senza richiedere i certificati. Se viene utilizzata l'autenticazione Kerberos, la funzione viene eseguita su SSL e il protocollo Kerberos usa il certificato configurato per IP-HTTPS. Alcuni scenari aziendali (incluse la distribuzione multisito e l'autenticazione client per password monouso) richiedono l'uso dell'autenticazione del certificato e non l'autenticazione Kerberos.  
  
-   **Server IP-HTTPS**: quando si configura accesso remoto, il server di accesso remoto viene configurato automaticamente per fungere da listener Web IP-HTTPS. Il sito IP-HTTPS richiede un certificato del sito Web e i computer client devono riuscire a contattare il sito dell'elenco di revoche di certificati (CRL) per il certificato.  
  
-   **Server dei percorsi di rete**: il server dei percorsi di rete è un sito Web usato per rilevare se i computer client si trovano nella rete aziendale. Il server dei percorsi di rete richiede un certificato di sito Web. I client DirectAccess devono poter contattare il sito CRL per il certificato.  
  
I requisiti dell'autorità di certificazione (CA) per ognuno di questi scenari sono riepilogati nella tabella seguente.  
  
|Autenticazione IPsec|Server IP-HTTPS|Server dei percorsi di rete|  
|------------|----------|--------------|  
|Una CA interna è necessaria per emettere certificati del computer al server di accesso remoto e ai client per l'autenticazione IPsec quando non si usa il protocollo Kerberos per l'autenticazione.|CA interna: è possibile usare una CA interna per emettere il certificato IP-HTTPS. Tuttavia, è necessario assicurarsi che il punto di distribuzione CRL sia disponibile esternamente.|CA interna: è possibile usare una CA interna per emettere il certificato del sito Web del server dei percorsi di rete. Verificare che il punto di distribuzione CRL sia a disponibilità elevata nella rete interna.|  
||Certificato autofirmato: è possibile usare un certificato autofirmato per il server IP-HTTPS. Non è possibile usare un certificato autofirmato in una distribuzione multisito.|Certificato autofirmato: è possibile usare un certificato autofirmato per il sito Web del server dei percorsi di rete. Tuttavia, non è possibile usare un certificato autofirmato nelle distribuzioni multisito.|  
||CA pubblica: si consiglia di usare una CA pubblica per emettere il certificato IP-HTTPS, in modo da garantire che il punto di distribuzione CRL sia disponibile esternamente.||  
  
#### <a name="plan-computer-certificates-for-ipsec-authentication"></a>Pianificare i certificati del computer per l'autenticazione IPsec  
Se si usa l'autenticazione IPsec basata su certificato, il server di accesso remoto e i client sono necessari per ottenere un certificato del computer. Il modo più semplice per installare i certificati consiste nell'usare Criteri di gruppo per configurare la registrazione automatica per i certificati del computer. In questo modo, tutti i membri del dominio ottengono un certificato da una CA globale (enterprise). Se non è una CA impostato all'interno dell'organizzazione, vedere [Servizi certificati Active Directory](https://technet.microsoft.com/library/cc770357.aspx).  
  
Questo certificato ha i seguenti requisiti:  
  
-   Il certificato deve avere l'utilizzo chiavi avanzato (EKU) di autenticazione client.  
  
-   Il client e i certificati del server devono essere correlati allo stesso certificato radice. che deve essere selezionato nelle impostazioni di configurazione DirectAccess.  
  
#### <a name="plan-certificates-for-ip-https"></a>Pianificare i certificati per IP-HTTPS  
Il server di Accesso remoto funge da listener IP-HTTPS ed è necessario installare manualmente un certificato del sito Web HTTPS nel server. Durante la pianificazione, prendere in considerazione gli elementi seguenti:  
  
-   L'uso di una CA pubblica è consigliato perché consente di avere subito disponibili gli elenchi di revoche di certificati (CRL).  
  
-   Nel campo soggetto specificare l'indirizzo IPv4 della scheda Internet del server di accesso remoto o il nome di dominio completo dell'URL IP-HTTPS (indirizzo ConnectTo). Se il server di Accesso remoto si trova dietro un dispositivo NAT, deve essere specificato il nome pubblico o l'indirizzo del dispositivo NAT.  
  
-   Il nome comune del certificato deve corrispondere al nome del sito IP-HTTPS.  
  
-   Per il **Enhanced Key Usage** campo, utilizzare l'identificatore di oggetto (OID) di autenticazione Server.  
  
-   Per il campo **Punti di distribuzione Elenco di revoche di certificati (CRL)** , specificare un punto di distribuzione CRL accessibile dai client DirectAccess connessi a Internet.  
  
    > [!NOTE]  
    > Questa operazione è necessaria solo per i client che eseguono Windows 7.  
  
-   Il certificato IP-HTTPS deve avere una chiave privata.  
  
-   Il certificato IP-HTTPS deve essere importato direttamente nell'archivio personale.  
  
-   Nei nomi dei certificati IP-HTTPS possono essere presenti caratteri jolly.  
  
#### <a name="plan-website-certificates-for-the-network-location-server"></a>Pianificare i certificati del sito Web per il server dei percorsi di rete  
Quando si pianifica il sito Web del server dei percorsi di rete, tenere presente quanto segue:  
  
-   Nel **soggetto** specificare un indirizzo IP dell'interfaccia intranet del server dei percorsi di rete o il nome di dominio COMPLETO dell'URL del percorso di rete.  
  
-   Per il campo **utilizzo chiavi avanzato** , usare l'OID di autenticazione server.  
  
-   Per il campo **punti di distribuzione CRL** , utilizzare un punto di distribuzione CRL accessibile dai client DirectAccess connessi alla Intranet. Questo punto di distribuzione CRL non deve essere accessibile fuori dalla rete interna.  
  
> [!NOTE]  
> Verificare che i certificati per IP-HTTPS e server dei percorsi di rete abbiano un nome soggetto. Se il certificato utilizza un nome alternativo, non verrà accettato dalla procedura guidata di accesso remoto.  
  
#### <a name="plan-dns-requirements"></a>Pianificare i requisiti DNS  
In questa sezione vengono illustrati i requisiti DNS per i client e i server in una distribuzione di accesso remoto.  
  
##### <a name="directaccess-client-requests"></a>Richieste dei client DirectAccess  
DNS viene usato per risolvere le richieste provenienti dai computer client DirectAccess non presenti nella rete interna. I client DirectAccess tentano di connettersi al server dei percorsi di rete DirectAccess per determinare se si trovano in Internet o nella rete aziendale.  
  
-   Se la connessione ha esito positivo, i client vengono determinati nella Intranet, DirectAccess non viene usato e le richieste client vengono risolte usando il server DNS configurato nella scheda di rete del computer client.  
  
-   Se la connessione non riesce, si presuppone che i client si trovino sulla rete Internet. I client DirectAccess usano la tabella dei criteri di risoluzione dei nomi (NRPT) per determinare quale server DNS usare per la risoluzione delle richieste di nomi. È possibile specificare che i client debbano usare DNS64 DirectAccess per risolvere i nomi o un server DNS interno alternativo.  
  
Quando si esegue la risoluzione dei nomi, la tabella dei criteri di risoluzione dei nomi viene usata dai client DirectAccess per stabilire come gestire una richiesta. I client richiedono un nome di dominio completo o un nome con etichetta singola, ad esempio <https://internal>. Se è richiesto un nome con etichetta singola, viene aggiunto un suffisso DNS per trasformarlo in un nome di dominio completo. Se la query DNS corrisponde a una voce nella tabella dei criteri di risoluzione dei nomi e DNS4 o viene specificato un server DNS Intranet per la voce, la query viene inviata per la risoluzione dei nomi usando il server specificato. Se esiste una corrispondenza, ma non viene specificato alcun server DNS, viene applicata una regola di esenzione e una risoluzione dei nomi normale.  
  
Quando viene aggiunto un nuovo suffisso alla tabella dei criteri di risoluzione dei nomi nella console di gestione accesso remoto, i server DNS predefiniti per il suffisso possono essere individuati automaticamente facendo clic sul pulsante **rileva** . Il rilevamento automatico funziona nel modo seguente:  
  
-   Se la rete aziendale è basata su IPv4 o usa IPv4 e IPv6, l'indirizzo predefinito è l'indirizzo DNS64 della scheda interna nel server di accesso remoto.  
  
-   Se la rete aziendale è basata su IPv6, l'indirizzo predefinito è l'indirizzo IPv6 dei server DNS nella rete aziendale.  
  
##### <a name="infrastructure-servers"></a>Server dell'infrastruttura  
  
-   **Server del percorso di rete**  
  
    I client DirectAccess tentano di raggiungere il server dei percorsi di rete per determinare se si trovano sulla rete interna. I client nella rete interna devono essere in grado di risolvere il nome del server dei percorsi di rete ed è necessario impedire la risoluzione del nome quando si trovano su Internet. Per verificarlo, il nome di dominio completo del server dei percorsi di rete viene aggiunto per impostazione predefinita come regola di esenzione nella tabella dei criteri di risoluzione dei nomi. Quando si configura Accesso remoto vengono create automaticamente anche le seguenti regole:  
  
    -   Una regola per il suffisso DNS per il dominio radice o il nome di dominio del server di accesso remoto e gli indirizzi IPv6 corrispondenti ai server DNS Intranet configurati nel server di accesso remoto. Ad esempio, se il server di Accesso remoto è membro del dominio corp.contoso.com, viene creata una regola per il suffisso DNS corp.contoso.com.  
  
    -   Una regola di esenzione per il nome di dominio completo del server del percorso di rete. Se, ad esempio, l'URL del server dei percorsi di rete è <https://nls.corp.contoso.com>, viene creata una regola di esenzione per il nome di dominio completo nls.corp.contoso.com.  
  
-   **Server IP-HTTPS**  
  
    Il server di accesso remoto funge da listener IP-HTTPS e usa il certificato del server per autenticarsi nei client IP-HTTPS. Il nome IP-HTTPS deve essere risolvibile dai client DirectAccess che usano i server DNS pubblici.  
  
##### <a name="connectivity-verifiers"></a>Strumenti di verifica della connettività  
Accesso remoto crea un probe Web predefinito usato dai computer client DirectAccess per verificare la connettività alla rete interna. Per verificare il corretto funzionamento del probe, è necessario registrare manualmente i seguenti nomi in DNS:  
  
-   **DirectAccess-webprobehost** deve essere risolto nell'indirizzo IPv4 interno del server di accesso remoto o nell'indirizzo IPv6 in un ambiente solo IPv6.  
  
-   **DirectAccess-corpconnectivityhost** deve essere risolto nell'indirizzo dell'host locale (loopback). È necessario creare i record A e AAAA. Il valore del record A è 127.0.0.1 e il valore del record AAAA viene costruito dal prefisso NAT64 con gli ultimi 32 bit come 127.0.0.1. Il prefisso NAT64 può essere recuperato eseguendo il cmdlet **Get-netnatTransitionConfiguration** di Windows PowerShell.  
  
    > [!NOTE]  
    > Questa operazione è valida solo negli ambienti solo IPv4. In un ambiente IPv4 più IPv6 o solo IPv6, creare solo un record AAAA con l'indirizzo IP di loopback:: 1.  
  
È possibile creare altri sistemi di verifica della connettività usando altri indirizzi Web su HTTP o PING. Per ogni strumento di verifica della connettività deve esistere una voce DNS.  
  
##### <a name="dns-server-requirements"></a>Requisiti del server DNS  
  
-   Per i client DirectAccess, è necessario usare un server DNS che esegue Windows Server 2012, Windows Server 2008 R2, Windows Server 2008, Windows Server 2003 o qualsiasi server DNS che supporti IPv6.  
  
-   È necessario usare un server DNS che supporti gli aggiornamenti dinamici. È possibile utilizzare i server DNS che non supportano gli aggiornamenti dinamici, ma è necessario aggiornare manualmente le voci.  
  
-   Il nome di dominio completo per i punti di distribuzione CRL deve essere risolvibile mediante server DNS Internet. Se, ad esempio, <https://crl.contoso.com/crld/corp-DC1-CA.crl> URL si trova nel campo **punti di distribuzione CRL** del certificato IP-HTTPS del server di accesso remoto, è necessario assicurarsi che il nome di dominio completo CRLD.contoso.com sia risolvibile mediante server DNS Internet.  
  
#### <a name="plan-for-local-name-resolution"></a>Pianificare la risoluzione dei nomi locali  
Quando si pianifica la risoluzione dei nomi locali, tenere presente quanto segue:  
  
##### <a name="nrpt"></a>NRPT  
Potrebbe essere necessario creare regole della tabella dei criteri di risoluzione dei nomi aggiuntive nelle situazioni seguenti:  
  
-   È necessario aggiungere altri suffissi DNS per lo spazio dei nomi Intranet.  
  
-   Se gli FQDN dei punti di distribuzione CRL sono basati sullo spazio dei nomi Intranet, è necessario aggiungere regole di esenzione per i nomi di dominio completi dei punti di distribuzione CRL.  
  
-   Se si dispone di un ambiente DNS "Split Brain", è necessario aggiungere regole di esenzione per i nomi delle risorse per cui si desidera che i client DirectAccess che si trovano in Internet accedano alla versione Internet, anziché alla versione Intranet.  
  
-   Se si reindirizza il traffico a un sito Web esterno attraverso i server proxy Web Intranet, il sito Web esterno è disponibile solo dalla Intranet. USA gli indirizzi dei server proxy Web per consentire le richieste in ingresso. In questa situazione, aggiungere una regola di esenzione per il nome di dominio completo del sito Web esterno e specificare che la regola utilizza il server proxy Web Intranet anziché gli indirizzi IPv6 dei server DNS Intranet.  
  
    Si immagini, ad esempio, che si sta testando un sito Web esterno denominato test.contoso.com. Questo nome non è risolvibile tramite i server DNS Internet, ma il server proxy Web di Contoso sa come risolvere il nome e come indirizzare le richieste per il sito Web al server Web esterno. Per impedire l'accesso al sito da parte di utenti che non si trovano nella Intranet di Contoso, il sito Web esterno consente le richieste solo dall'indirizzo Internet IPv4 del proxy Web di Contoso. Pertanto, gli utenti Intranet possono accedere al sito Web perché usano il proxy Web di Contoso, ma gli utenti di DirectAccess non possono usare il proxy Web di contoso. Configurando una regola di esenzione della tabella dei criteri di risoluzione dei nomi per test.contoso.com che usa il proxy Web di Contoso, le richieste di pagine Web per test.contoso.com vengono indirizzate al server proxy Web Intranet sulla rete Internet IPv4.  
  
##### <a name="single-label-names"></a>Nomi con etichetta singola  
I nomi con etichetta singola, ad esempio <https://paycheck>, vengono talvolta utilizzati per i server Intranet. Se viene richiesto un nome di etichetta singola e viene configurato un elenco di ricerca di suffissi DNS, i suffissi DNS nell'elenco verranno aggiunti al nome dell'etichetta singola. Ad esempio, quando un utente in un computer che è membro dei tipi di dominio corp.contoso.com <https://paycheck> nel Web browser, il nome di dominio completo (FQDN) costruito come nome è paycheck.corp.contoso.com. Per impostazione predefinita, il suffisso aggiunto si basa sul suffisso DNS primario del computer client.  
  
> [!NOTE]  
> In uno scenario di spazio dei nomi indipendente (in cui uno o più computer del dominio hanno un suffisso DNS che non corrisponde al dominio Active Directory al quale i computer sono membri), è necessario assicurarsi che l'elenco di ricerca sia personalizzato per includere tutti i suffissi richiesti. Per impostazione predefinita, la configurazione guidata accesso remoto configura il nome DNS Active Directory come suffisso DNS primario nel client. Aggiungere il suffisso DNS emesso dai client per la risoluzione dei nomi.  
  
Se nell'organizzazione vengono distribuiti più domini e Windows Internet Name Service (WINS) e ci si connette in remoto, i nomi singoli possono essere risolti come segue:  
  
-   Tramite la distribuzione di una zona di ricerca diretta WINS in DNS. Quando si tenta di risolvere computername.dns.zone1.corp.contoso.com, la richiesta viene indirizzata al server WINS che usa solo il nome del computer. Il client ritiene che stia inviando una normale richiesta di record DNS A, ma si tratta in realtà di una richiesta NetBIOS.  
  
    Per ulteriori informazioni, vedere [gestione di una zona di ricerca diretta](https://technet.microsoft.com/library/cc816891(WS.10).aspx).  
  
-   Aggiungendo un suffisso DNS, ad esempio dns.zone1.corp.contoso.com, all'oggetto Criteri di gruppo del dominio predefinito.  
  
##### <a name="split-brain-dns"></a>DNS "split brain"  
Il DNS "Split Brain" si riferisce all'uso dello stesso dominio DNS per la risoluzione dei nomi Internet e Intranet.  
  
Per le distribuzioni DNS "split Brain", è necessario elencare i nomi di dominio completi duplicati in Internet e intranet e decidere quali risorse il client DirectAccess deve raggiungere la rete intranet o la versione di Internet. Quando si desidera che i client DirectAccess raggiungano la versione Internet, è necessario aggiungere il nome di dominio completo corrispondente come regola di esenzione per la tabella dei criteri di risoluzione dei nomi per ogni risorsa.  
  
In un ambiente DNS "Split Brain", se si vuole che siano disponibili entrambe le versioni della risorsa, configurare le risorse Intranet con nomi che non duplicano i nomi usati in Internet. Quindi, indicare agli utenti di usare il nome alternativo quando accedono alla risorsa nella Intranet. Ad esempio, configurare www\.internal.contoso.com per il nome interno di www\.contoso.com.  
  
In un ambiente DNS non "split brain", lo spazio dei nomi Internet è diverso da quello Intranet. Contoso Corporation utilizza ad esempio contoso.com in Internet e corp.contoso.com nella Intranet. Poiché tutte le risorse Intranet utilizzano il suffisso DNS corp.contoso.com, la regola della tabella dei criteri di risoluzione dei nomi per corp.contoso.com indirizza tutte le query relative ai nomi DNS per le risorse Intranet ai server DNS Intranet. Le query DNS per i nomi con suffisso contoso.com non corrispondono alla regola dello spazio dei nomi Intranet corp.contoso.com nella tabella dei criteri di risoluzione dei nomi e vengono inviate ai server DNS Internet. Con una distribuzione DNS non "split brain", poiché i nomi di dominio completi non vengono duplicati per le risorse Intranet e Internet, non è necessaria alcuna configurazione aggiuntiva per la tabella dei criteri di risoluzione dei nomi. I client DirectAccess possono accedere alle risorse Internet e Intranet per l'organizzazione.  
  
##### <a name="plan-local-name-resolution-behavior-for-directaccess-clients"></a>Pianificare il comportamento della risoluzione dei nomi locali per i client DirectAccess  
Se non è possibile risolvere un nome con DNS, il servizio client DNS in Windows Server 2012, Windows 8, Windows Server 2008 R2 e Windows 7 può utilizzare la risoluzione dei nomi locali, con i protocolli LLMNR (Link-Local Multicast Name Resolution) e NetBIOS su TCP/IP, per risolvere il nome nella subnet locale. La risoluzione dei nomi locali è in genere necessaria per la connettività peer-to-peer se il computer si trova in reti private, ad esempio reti domestiche a subnet singola.  
  
Quando il servizio client DNS esegue la risoluzione dei nomi locali per i nomi di server Intranet e il computer è connesso a una subnet condivisa su Internet, gli utenti malintenzionati possono acquisire LLMNR e NetBIOS su messaggi TCP/IP per determinare i nomi dei server Intranet. Nella pagina DNS della configurazione guidata server di infrastruttura è possibile configurare il comportamento di risoluzione dei nomi locali in base ai tipi di risposte ricevuti dai server DNS Intranet. Sono disponibili le seguenti opzioni:  
  
-   **Usa risoluzione dei nomi locali se il nome non esiste nel DNS**: questa opzione è la più sicura perché il client DirectAccess esegue la risoluzione dei nomi locali solo per i nomi di server che non possono essere risolti dai server DNS Intranet. Se i server DNS Intranet sono raggiungibili, i nomi di server Intranet vengono risolti. Se i server DNS Intranet non sono raggiungibili o se si verificano altri tipi di errori DNS, i nomi di server Intranet non vengono comunicati alla subnet mediante la risoluzione dei nomi locali.  
  
-   **Usare la risoluzione dei nomi locali se il nome non esiste nel DNS o i server DNS non sono raggiungibili quando il computer client si trova in una rete privata (scelta consigliata)** : questa opzione è consigliata perché consente l'uso della risoluzione dei nomi locali in una rete privata solo quando i server DNS Intranet non sono raggiungibili.  
  
-   **Usa la risoluzione dei nomi locali per qualsiasi tipo di errore di risoluzione DNS (meno sicuro)** : questa è l'opzione meno sicura perché i nomi dei server di rete Intranet possono essere comunicati alla subnet locale tramite la risoluzione dei nomi locali.  
  
#### <a name="plan-the-network-location-server-configuration"></a>Pianificare la configurazione del server dei percorsi di rete  
Il server dei percorsi di rete è un sito Web usato per rilevare se i client DirectAccess si trovano nella rete aziendale. I client nella rete aziendale non usano DirectAccess per raggiungere le risorse interne; ma si connettono direttamente.  
  
Il sito Web del server dei percorsi di rete può essere ospitato nel server di accesso remoto o in un altro server dell'organizzazione. Se si ospita il server dei percorsi di rete nel server di accesso remoto, il sito Web viene creato automaticamente quando si distribuisce accesso remoto. Se si ospita il server dei percorsi di rete in un altro server che esegue un sistema operativo Windows, è necessario assicurarsi che Internet Information Services (IIS) sia installato nel server e che il sito Web sia stato creato. Accesso remoto non configura le impostazioni nel server dei percorsi di rete.  
  
Verificare che il sito Web del server dei percorsi di rete soddisfi i requisiti seguenti:  
  
-   Dispone di un certificato server HTTPS.  
  
-   Dispone di disponibilità elevata per i computer nella rete interna.  
  
-   Non è accessibile ai computer client DirectAccess su Internet.  
  
-  
  
Inoltre, tenere presenti i requisiti seguenti per i client durante la configurazione del sito Web del server dei percorsi di rete:  
  
-   I computer client DirectAccess devono considerare attendibile la CA che ha emesso il certificato del server nel sito Web del server dei percorsi di rete.  
  
-   I computer client DirectAccess nella rete interna devono poter risolvere il nome del sito del server dei percorsi di rete.  
  
##### <a name="plan-certificates-for-the-network-location-server"></a>Pianificare i certificati per il server dei percorsi di rete  
Quando si ottiene il certificato del sito Web da usare per il server dei percorsi di rete, tenere presente quanto segue:  
  
-   Nel **soggetto** specificare l'indirizzo IP dell'interfaccia intranet del server dei percorsi di rete o il nome di dominio COMPLETO dell'URL del percorso di rete.  
  
-   Per il campo **utilizzo chiavi avanzato** , usare l'OID di autenticazione server.  
  
-   Il certificato del server dei percorsi di rete deve essere verificato in base a un elenco di revoche di certificati (CRL). Per il campo **punti di distribuzione CRL** , utilizzare un punto di distribuzione CRL accessibile dai client DirectAccess connessi alla Intranet. Questo punto di distribuzione CRL non deve essere accessibile fuori dalla rete interna.  
  
##### <a name="plan-dns-for-the-network-location-server"></a>Pianificare il DNS per il server dei percorsi di rete  
I client DirectAccess tentano di raggiungere il server dei percorsi di rete per determinare se si trovano sulla rete interna. I client nella rete interna devono poter risolvere il nome del server dei percorsi di rete, ma non quando si trovano sulla rete Internet. Per verificarlo, il nome di dominio completo del server dei percorsi di rete viene aggiunto per impostazione predefinita come regola di esenzione nella tabella dei criteri di risoluzione dei nomi.  
  
### <a name="plan-management-servers-configuration"></a>Pianificare la configurazione dei server di gestione  
I client DirectAccess avviano le comunicazioni con i server di gestione che forniscono servizi quali gli aggiornamenti di Windows Update e antivirus. I client DirectAccess usano anche il protocollo Kerberos per eseguire l'autenticazione ai controller di dominio prima di accedere alla rete interna. Durante la gestione remota dei client DirectAccess, i server di gestione comunicano con i computer client per eseguire funzioni di gestione quali le valutazioni degli inventari software o hardware. Accesso remoto può individuare automaticamente alcuni server di gestione, tra cui:  
  
-   Controller di dominio: l'individuazione automatica dei controller di dominio viene eseguita per i domini che contengono i computer client e per tutti i domini nella stessa foresta del server di accesso remoto.  
  
-   Server Microsoft endpoint Configuration Manager  
  
I controller di dominio e i server di Configuration Manager vengono rilevati automaticamente alla prima configurazione di DirectAccess. I controller di dominio rilevati non vengono visualizzati nella console di, ma è possibile recuperare le impostazioni utilizzando i cmdlet di Windows PowerShell. Se il controller di dominio o i server Configuration Manager vengono modificati, facendo clic su **Gestione aggiornamenti server** nella console viene aggiornato l'elenco dei server di gestione.  
  
**Requisiti del server di gestione**  
  
-   I server di gestione devono essere accessibili tramite il tunnel dell'infrastruttura. Quando si configura Accesso remoto, l'aggiunta di server all'elenco dei server di gestione li rende automaticamente accessibili su questo tunnel.  
  
-   I server di gestione che avviano connessioni ai client DirectAccess devono supportare completamente IPv6 per mezzo di un indirizzo IPv6 nativo o tramite un indirizzo assegnato da ISATAP.  
  
### <a name="plan-active-directory-requirements"></a>Pianificare i requisiti di Active Directory  
Accesso remoto usa Active Directory come indicato di seguito:  
  
-   **Autenticazione**: il tunnel dell'infrastruttura usa l'autenticazione NTLMv2 per l'account del computer che si connette al server di accesso remoto e l'account deve trovarsi in un dominio Active Directory. Il tunnel Intranet usa l'autenticazione Kerberos per la creazione del tunnel della Intranet da parte dell'utente.  
  
-   **Oggetti Criteri di gruppo**: accesso remoto raccoglie le impostazioni di configurazione in oggetti di criteri di gruppo (GPO), che vengono applicati ai server di accesso remoto, ai client e ai server applicazioni interni.  
  
-   **Gruppi di sicurezza**: accesso remoto usa i gruppi di sicurezza per raccogliere e identificare i computer client DirectAccess. Gli oggetti Criteri di gruppo vengono applicati ai gruppi di sicurezza richiesti.  
  
Quando si pianifica un ambiente di Active Directory per una distribuzione di accesso remoto, considerare i requisiti seguenti:  
  
-   Almeno un controller di dominio è installato nel sistema operativo Windows Server 2012, Windows Server 2008 R2 Windows Server 2008 o Windows Server 2003.  
  
    Se il controller di dominio si trova su una rete perimetrale (e pertanto raggiungibile dalla scheda di rete con connessione Internet del server di accesso remoto), impedire al server di accesso remoto di raggiungerlo. È necessario aggiungere filtri pacchetti sul controller di dominio per impedire la connettività all'indirizzo IP della scheda Internet.  
  
-   Il server di Accesso remoto deve essere membro di un dominio.  
  
-   I client DirectAccess devono essere membri di un dominio. I client possono appartenere a:  
  
    -   Qualsiasi dominio nella stessa foresta del server di Accesso remoto.  
  
    -   Qualsiasi dominio che abbia un trust bidirezionale con il dominio del server di Accesso remoto.  
  
    -   Qualsiasi dominio in una foresta che abbia una relazione di trust bidirezionale con la foresta del dominio del server di accesso remoto.  
  
> [!NOTE]  
> -   Il server di Accesso remoto non può essere un controller di dominio.  
> -   Il controller di dominio Active Directory usato per l'accesso remoto non deve essere raggiungibile dalla scheda Internet esterna del server di accesso remoto (la scheda non deve trovarsi nel profilo di dominio di Windows Firewall).  
  
#### <a name="plan-client-authentication"></a>Pianificare l'autenticazione client  
In accesso remoto in Windows Server 2012 è possibile scegliere di usare l'autenticazione Kerberos predefinita, che usa nomi utente e password, oppure i certificati per l'autenticazione del computer IPsec.  
  
**Autenticazione Kerberos**: quando si sceglie di usare Active Directory credenziali per l'autenticazione, DirectAccess USA prima di tutto l'autenticazione Kerberos per il computer e quindi usa l'autenticazione Kerberos per l'utente. Quando si usa questa modalità di autenticazione, DirectAccess usa un solo tunnel di sicurezza che fornisce l'accesso al server DNS, al controller di dominio e a qualsiasi altro server nella rete interna  
  
**Autenticazione IPSec**: quando si sceglie di usare l'autenticazione a due fattori o protezione accesso alla rete, DirectAccess usa due tunnel di sicurezza. La configurazione guidata accesso remoto configura le regole di sicurezza delle connessioni in Windows Firewall con sicurezza avanzata. Queste regole specificano le credenziali seguenti per la negoziazione della sicurezza IPsec per il server di accesso remoto:  
  
-   Il tunnel dell'infrastruttura usa le credenziali del certificato del computer per la prima autenticazione e le credenziali utente (NTLMv2) per la seconda autenticazione. Le credenziali utente forzano l'uso di Authenticated Internet Protocol (AuthIP) e forniscono l'accesso a un server DNS e a un controller di dominio prima che il client DirectAccess possa usare le credenziali Kerberos per il tunnel della Intranet.  
  
-   Il tunnel Intranet usa le credenziali del certificato del computer per la prima autenticazione e le credenziali utente (Kerberos V5) per la seconda autenticazione.  
  
#### <a name="plan-multiple-domains"></a>Pianificare più domini  
L'elenco dei server di gestione deve includere i controller di dominio di tutti i domini che contengono gruppi di sicurezza con computer client DirectAccess. Deve contenere tutti i domini che contengono account utente che potrebbero usare i computer configurati come client DirectAccess. In questo modo, si assicura gli utenti non presenti nello stesso dominio del computer client che stanno usando vengano autenticati con un controller di dominio nel dominio utente.  
  
Questa autenticazione è automatica se i domini si trovano nella stessa foresta. Se è presente un gruppo di sicurezza con computer client o server applicazioni che si trovano in foreste diverse, i controller di dominio di tali foreste non vengono rilevati automaticamente. Anche le foreste non vengono rilevate automaticamente. È possibile eseguire l'attività **Gestione aggiornamenti server** in **gestione accesso remoto** per rilevare questi controller di dominio.  
  
Laddove possibile, è necessario aggiungere i suffissi dei nomi di dominio comuni alla tabella dei criteri di risoluzione dei nomi durante la distribuzione di accesso remoto Ad esempio, se si hanno due domini, domain1.corp.contoso.com e domain2.corp.contoso.com, è possibile aggiungere una voce del suffisso DNS comune con il suffisso del nome di dominio corp.contoso.com, invece di due voci nella tabella dei criteri di risoluzione dei nomi. Questa operazione viene eseguita automaticamente per i domini nella stessa radice. I domini che non si trovano nella stessa radice devono essere aggiunti manualmente.  
  
### <a name="plan-group-policy-object-creation"></a>Pianificare Criteri di gruppo la creazione di oggetti  
Quando si configura accesso remoto, le impostazioni DirectAccess vengono raccolte in oggetti Criteri di gruppo (GPO). Due oggetti Criteri di gruppo vengono popolati con le impostazioni DirectAccess e distribuiti come segue:  
  
-   **Oggetto Criteri**di gruppo del client DirectAccess: questo oggetto Criteri di gruppo contiene le impostazioni client, incluse le impostazioni della tecnologia di transizione IPv6, le voci della tabella e le regole di sicurezza delle connessioni per Windows Firewall con sicurezza avanzata L'oggetto Criteri di gruppo viene applicato ai gruppi di sicurezza specificati per i computer client.  
  
-   **Oggetto Criteri**di gruppo del server DirectAccess: questo oggetto Criteri di gruppo contiene le impostazioni di configurazione DirectAccess applicate a qualsiasi server configurato come server di accesso remoto nella distribuzione. Contiene anche le regole di sicurezza delle connessioni per Windows Firewall con sicurezza avanzata.  
  
> [!NOTE]  
> La configurazione dei server applicazioni non è supportata nella gestione remota dei client DirectAccess perché i client non possono accedere alla rete interna del server DirectAccess in cui risiedono i server applicazioni. Il passaggio 4 della schermata di configurazione della configurazione di accesso remoto non è disponibile per questo tipo di configurazione.  
  
È possibile configurare gli oggetti Criteri di gruppo in modo automatico o manuale.  
  
**Automaticamente**: quando si specifica che gli oggetti Criteri di gruppo vengono creati automaticamente, per ogni oggetto Criteri di gruppo viene specificato un nome predefinito.  
  
**Manualmente**: è possibile usare gli oggetti Criteri di gruppo predefiniti dall'amministratore Active Directory.  
  
Quando si configurano gli oggetti Criteri di gruppo, tenere presenti gli avvisi seguenti:  
  
-   Dopo aver configurato DirectAccess in modo da usare specifici oggetti Criteri di gruppo, non sarà possibile configurarlo in modo da usarne altri.  
  
-   Usare la procedura seguente per eseguire il backup di tutti gli oggetti di Criteri di gruppo di accesso remoto prima di eseguire i cmdlet di DirectAccess:  
  
    [Backup e ripristino della configurazione di Accesso remoto](https://go.microsoft.com/fwlink/?LinkID=257928).  
  
-   Se si usano oggetti Criteri di gruppo configurati automaticamente o manualmente, è necessario aggiungere un criterio per il rilevamento dei collegamenti lenti se i client utilizzeranno il 3G. Il percorso per **criteri: configura criteri di gruppo il rilevamento dei collegamenti lenti** è:  
  
    **Configurazione computer/Criteri/Modelli amministrativi/Sistema/Criteri di gruppo**.  
  
-   Se le autorizzazioni corrette per il collegamento degli oggetti Criteri di gruppo non esistono, viene emesso un avviso. L'operazione di accesso remoto continuerà, ma il collegamento non verrà eseguito. Se viene generato questo avviso, i collegamenti non verranno creati automaticamente, anche se le autorizzazioni vengono aggiunte in un secondo momento. L'amministratore dovrà invece creare i collegamenti manualmente.  
  
#### <a name="automatically-created-gpos"></a>Oggetti Criteri di gruppo creati automaticamente  
Quando si usano oggetti Criteri di gruppo creati automaticamente, tenere presente quanto segue:  
  
Gli oggetti Criteri di gruppo creati automaticamente vengono applicati in base al percorso e alla destinazione del collegamento, come indicato di seguito:  
  
-   Per l'oggetto Criteri di gruppo del server DirectAccess, il percorso e la destinazione del collegamento puntano al dominio contenente il server di accesso remoto.  
  
-   Quando vengono creati oggetti Criteri di gruppo del server applicazioni e del client, il percorso viene impostato su un singolo dominio. Il nome dell'oggetto Criteri di gruppo viene cercato in ogni dominio e il dominio viene compilato con le impostazioni DirectAccess, se esistente.  
  
-   La destinazione collegamento viene impostata sulla radice del dominio in cui è stato creato l'oggetto Criteri di gruppo. Viene creato un oggetto Criteri di gruppo per ogni dominio che contiene computer client o server applicazioni, che sarà quindi collegato alla radice del rispettivo dominio.  
  
Quando si usano gli oggetti Criteri di gruppo creati automaticamente per applicare le impostazioni DirectAccess, l'amministratore del server di accesso remoto richiede le autorizzazioni seguenti:  
  
-   Autorizzazioni per la creazione di oggetti Criteri di gruppo per ogni dominio.  
  
-   Autorizzazioni per il collegamento a tutte le radici del dominio client selezionate.  
  
-   Autorizzazioni per il collegamento alle radici del dominio dell'oggetto Criteri di gruppo del server.  
  
-   Autorizzazioni di sicurezza per la creazione, la modifica, l'eliminazione e la modifica degli oggetti Criteri di gruppo.  
  
-   Autorizzazioni di lettura dell'oggetto Criteri di gruppo per ogni dominio richiesto. Questa autorizzazione non è necessaria, ma è consigliata perché consente l'accesso remoto per verificare che gli oggetti Criteri di gruppo con nomi duplicati non esistano durante la creazione degli oggetti Criteri di gruppo.  
  
#### <a name="manually-created-gpos"></a>Oggetti Criteri di gruppo creati manualmente  
Tenere presente quanto segue quando si usano gli oggetti Criteri di gruppo creati manualmente:  
  
-   Gli oggetti Criteri di gruppo devono essere presenti prima di eseguire la Configurazione guidata Accesso remoto.  
  
-   Per applicare le impostazioni DirectAccess, l'amministratore del server di accesso remoto richiede autorizzazioni di sicurezza complete per la creazione, la modifica, l'eliminazione e la modifica degli oggetti Criteri di gruppo creati manualmente.  
  
-   Viene eseguita una ricerca per un collegamento all'oggetto Criteri di gruppo nell'intero dominio. Se l'oggetto Criteri di gruppo non è collegato nel dominio, verrà automaticamente creato un collegamento nel dominio radice. Se le autorizzazioni richieste per creare il collegamento non sono disponibili verrà emesso un avviso.  
  
#### <a name="recovering-from-a-deleted-gpo"></a>Ripristino da un oggetto Criteri di gruppo eliminato  
Se un oggetto Criteri di gruppo su un server di accesso remoto, un client o un server applicazioni è stato eliminato per errore, verrà visualizzato il seguente messaggio di errore: **Impossibile trovare l'oggetto Criteri di gruppo (nome oggetto Criteri**di gruppo).  
  
Se è disponibile un backup sarà possibile ripristinare l'oggetto Criteri di gruppo. Se non è disponibile alcun backup, è necessario rimuovere le impostazioni di configurazione e configurarle nuovamente.  
  
###### <a name="to-remove-configuration-settings"></a>Per rimuovere le impostazioni di configurazione  
  
1.  Eseguire il cmdlet di Windows PowerShell **Uninstall-RemoteAccess**.  
  
2.  Aprire **gestione accesso remoto**.  
  
3.  Verrà visualizzato un messaggio di errore che informa che l'oggetto Criteri di gruppo non è stato trovato. Fare clic su **rimuovere impostazioni di configurazione**. Al termine, il server verrà ripristinato in uno stato non configurato e sarà possibile riconfigurare le impostazioni.  
  


