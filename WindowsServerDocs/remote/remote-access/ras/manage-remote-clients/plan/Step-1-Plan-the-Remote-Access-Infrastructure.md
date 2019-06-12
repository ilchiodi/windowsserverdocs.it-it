---
title: Passaggio 1 pianificare l'infrastruttura di accesso remoto
description: Questo argomento fa parte della Guida di client DirectAccess di gestire in remoto in Windows Server 2016.
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology:
- networking-ras
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: a1ce7af5-f3fe-4fc9-82e8-926800e37bc1
ms.author: pashort
author: shortpatti
ms.openlocfilehash: f0a7d19ff571a5172d2dbd9a3d11584581c00370
ms.sourcegitcommit: d888e35f71801c1935620f38699dda11db7f7aad
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 06/07/2019
ms.locfileid: "66805036"
---
# <a name="step-1-plan-the-remote-access-infrastructure"></a>Passaggio 1 pianificare l'infrastruttura di accesso remoto

>Si applica a: Windows Server (canale semestrale), Windows Server 2016

> [!NOTE]
> Windows Server 2016 combina DirectAccess e Routing e accesso remoto (RRAS) in un unico ruolo Accesso remoto.  
  
In questo argomento vengono descritti i passaggi per la pianificazione di un'infrastruttura che è possibile usare per configurare un singolo server di accesso remoto per la gestione remota dei client DirectAccess. Nella tabella seguente elenca i passaggi, ma queste attività di pianificazione non sono necessario essere eseguita in un ordine specifico.  
  
|Attività|Descrizione|  
|----|--------|  
|[Pianificare le impostazioni di server e topologia di rete](#plan-network-topology-and-settings)|Decidere dove collocare il server di accesso remoto (alla periferia o dietro un dispositivo Network Address Translation (NAT) o un firewall) e pianificare gli indirizzi IP e il routing.|  
|[Pianificare i requisiti del firewall](#plan-firewall-requirements)|Pianificare il passaggio di Accesso remoto attraverso i firewall perimetrali.|  
|[Pianificare i requisiti dei certificati](#plan-certificate-requirements)|Decidere se si usa il protocollo Kerberos o i certificati per l'autenticazione client e pianificare i certificati del sito Web.<br/><br/>IP-HTTPS è un protocollo di transizione usato dai client DirectAccess per eseguire il tunneling del traffico IPv6 in reti IPv4. Decidere se eseguire l'autenticazione IP-HTTPS per il server con un certificato emesso da un'autorità di certificazione (CA) o tramite un certificato autofirmato emesso automaticamente dal server di accesso remoto.|  
|[Pianificare i requisiti DNS](#plan-dns-requirements)|Pianificare le impostazioni di sistema DNS (Domain Name) per il server Accesso remoto, i server di infrastruttura, le opzioni di risoluzione dei nomi locali e la connettività dei client.| 
|[Pianificare la configurazione di rete percorso server](#plan-the-network-location-server-configuration)|Decidere dove collocare il sito Web server percorsi di rete dell'organizzazione (nel server di accesso remoto o un server alternativo) e pianificare i requisiti del certificato se il server dei percorsi di rete si troverà nel server di accesso remoto. **Nota:** Il server dei percorsi di rete viene usato dai client DirectAccess per determinare se si trovano sulla rete interna.|  
|[Pianificare le configurazioni di server di gestione](#plan-management-servers-configuration)|Pianificare i server di gestione, ad esempio i server di aggiornamento, utilizzati durante la gestione remota dei client. **Nota:** gli amministratori possono gestire in remoto i computer client DirectAccess che si trovano all'esterno delle rete aziendale usando Internet.|  
|[Pianificare i requisiti di Active Directory](#plan-active-directory-requirements)|Pianificare i controller di dominio, i requisiti di Active Directory, l'autenticazione client e più struttura del dominio.|  
|[Creazione di oggetti Criteri di gruppo del piano](#plan-group-policy-object-creation)|Decidere quali oggetti Criteri di gruppo sono necessari nell'organizzazione e illustrato come creare e modificare oggetti Criteri di gruppo.|  
  
## <a name="plan-network-topology-and-settings"></a>Pianificare la topologia di rete e le impostazioni  
Quando si pianifica la rete, è necessario prendere in considerazione la topologia delle schede di rete, le impostazioni per gli indirizzi IP e i requisiti per ISATAP.  
  
### <a name="plan-network-adapters-and-ip-addressing"></a>Pianificare le schede di rete e gli indirizzi IP  
  
1.  Identificare la topologia delle schede di rete che si desidera utilizzare. Accesso remoto può essere impostato con una qualsiasi delle seguenti topologie:  
  
    -   Con due schede di rete: Il server di accesso remoto è installato nella periferia con una scheda di rete connessa a Internet e l'altra alla rete interna.  
  
    -   Con due schede di rete: Il server di accesso remoto viene installato dietro un dispositivo NAT, firewall o router, con una scheda di rete connessa a una rete perimetrale e l'altra alla rete interna.  
  
    -   Con una scheda di rete: Il server di accesso remoto viene installato dietro un dispositivo NAT e l'unica scheda di rete è connessa alla rete interna.  
  
2.  Identificare i requisiti degli indirizzi IP:  
  
    DirectAccess usa IPv6 con IPsec per creare una connessione sicura tra i computer client DirectAccess e la rete aziendale interna. Tuttavia, DirectAccess non richiede necessariamente la connettività a Internet IPv6 o il supporto IPv6 nativo nelle reti interne, Al contrario, lo configura automaticamente e utilizza tecnologie di transizione IPv6 per eseguire il tunneling del traffico IPv6 nella rete Internet IPv4 (6to4, Teredo o IP-HTTPS) e nella rete intranet solo IPv4 (NAT64 o ISATAP). Per una panoramica di queste tecnologie di transizione, vedere le seguenti risorse:  
  
    -   [Tecnologie di transizione IPv6](https://technet.microsoft.com/library/bb726951.aspx)  
  
    -   [Specifica del protocollo di Tunneling IP-HTTPS](https://msdn.microsoft.com/library/dd358571.aspx)  
  
3.  Configurare le schede e gli indirizzi necessari in base alla seguente tabella. Per le distribuzioni che si trovano dietro un dispositivo NAT con una sola scheda di rete, configurare gli indirizzi IP usando solo il **scheda di rete interna** colonna.  
  
    ||Scheda di rete esterna|Scheda di rete interna<sup>1 precedente</sup>|Requisiti di routing|  
    |-|--------------|------------------------|------------|  
    |Internet IPv4 e Intranet IPv4|Configurare quanto segue:<br/><br/>-Static due indirizzi IPv4 pubblici consecutivi con le subnet mask appropriate (richiesto solo per Teredo).<br/>-Un gateway predefinito IPv4 indirizzo per il firewall Internet o del router del provider di servizi Internet locale. **Nota:** Il server di accesso remoto richiede due indirizzi IPv4 pubblici consecutivi in modo che possa fungere da server Teredo e i client Teredo basati su Windows possono usare il server di accesso remoto per rilevare il tipo di dispositivo NAT.|Configurare quanto segue:<br/><br/>-Un indirizzo intranet IPv4 con subnet mask appropriata.<br/>-Un suffisso DNS specifico della connessione per lo spazio dei nomi intranet. Configurare anche un server DNS nell'interfaccia interna. **Attenzione:** Non configurare un gateway predefinito in alcuna interfaccia Intranet.|Per configurare il server di accesso remoto per raggiungere tutte le subnet nella rete IPv4 interna, eseguire le operazioni seguenti:<br/><br/>: Elenca gli spazi di indirizzi IPv4 per tutti i percorsi nella rete intranet.<br/>-Usare il `route add -p` o `netsh interface ipv4 add route` spazi degli indirizzi di comandi per aggiungere IPv4 come route statiche nella tabella di routing IPv4 del server di accesso remoto.|  
    |Internet IPv6 e Intranet IPv6|Configurare quanto segue:<br/><br/>-Usare la configurazione automatica degli indirizzi fornita dall'ISP.<br/>-Usare il `route print` comando per verificare l'esistenza di una route IPv6 predefinita che punta al router ISP nella tabella di routing IPv6.<br/>-Verificare se i router ISP e intranet utilizzano preferenze del router predefinito, come descritto in RFC 4191 e in tal caso, utilizzare una preferenza predefinita superiore rispetto ai router intranet locali. Se entrambe queste condizioni sono vere, non sono richieste altre configurazioni per la route predefinita. La preferenza superiore per il router ISP garantisce che la route IPv6 predefinita attiva del server di Accesso remoto punta alla rete Internet IPv6.<br/><br/>Poiché il server di Accesso remoto è un router IPv6, se si ha un'infrastruttura IPv6 nativa, anche l'interfaccia Internet può raggiungere i controller di dominio sulla Intranet. In questo caso, aggiungere filtri pacchetti al controller di dominio nella rete perimetrale che impedisce la connettività all'indirizzo IPv6 dell'interfaccia Internet del server di accesso remoto.|Configurare quanto segue:<br/><br/>Se non si usa livelli di preferenza predefiniti, configurare le interfacce intranet utilizzando il `netsh interface ipv6 set InterfaceIndex ignoredefaultroutes=enabled` comando. Il comando verifica che route predefinite aggiuntive che puntano ai router della Intranet non verranno aggiunte alla tabella di routing IPv6. È possibile ottenere l'elemento InterfaceIndex delle interfacce intranet dalla visualizzazione del `netsh interface show interface` comando.|Se si ha una Intranet IPv6, per configurare il server di Accesso remoto in modo da raggiungere tutti i percorsi IPv6, eseguire le operazioni seguenti:<br/><br/>: Elenca gli spazi degli indirizzi IPv6 per tutti i percorsi nella rete intranet.<br/>-Usare il `netsh interface ipv6 add route` comando per aggiungere gli spazi degli indirizzi IPv6 come route statiche nella tabella di routing IPv6 del server di accesso remoto.|  
    |Internet IPv4 e Intranet IPv6|Il server di accesso remoto inoltra il traffico della route IPv6 predefinita usando l'interfaccia di scheda Microsoft 6to4 a un relay 6to4 nella rete Internet IPv4. Quando IPv6 nativo non viene distribuito nella rete aziendale, è possibile usare il comando seguente per configurare un server di accesso remoto per l'indirizzo IPv4 del relay Microsoft 6to4 nella rete Internet IPv4: `netsh interface ipv6 6to4 set relay name=<ipaddress> state=enabled`.|||  
  
    > [!NOTE]  
    > -   Se il client DirectAccess è stato assegnato un indirizzo IPv4 pubblico, utilizza la tecnologia di relay 6to4 per la connessione alla intranet. Se il client viene assegnato un indirizzo IPv4 privato, userà Teredo. Se il client DirectAccess non riesce a connettersi al server DirectAccess con 6to4 o Teredo, userà IP-HTTPS.  
    > -   Per usare Teredo, è necessario configurare due indirizzi IP consecutivi nella scheda di rete con connessione esterna.  
    > -   Se il server di accesso remoto con una sola scheda di rete, è possibile usare Teredo.  
    > -   Il computer client IPv6 nativo può connettersi al server di Accesso remoto con un IPv6 nativo e non è richiesta alcuna tecnologia di transizione.  
  
### <a name="plan-isatap-requirements"></a>Pianificare i requisiti di ISATAP  
ISATAP è richiesto per la gestione remota di DirectAccessclients, in modo che i server di Gestione DirectAccess possono connettersi per i client DirectAccess in Internet. ISATAP non è necessaria per supportare le connessioni avviate dai computer client DirectAccess alle risorse IPv4 nella rete aziendale. Per questo scopo, viene usato NAT64/DNS64. Se la distribuzione richiede ISATAP, usare la tabella seguente per identificare i requisiti.  
  
|Scenario di distribuzione di ISATAP|Requisiti|  
|---------------|--------|  
|Esistente intranet IPv6 nativa (nessun ISATAP è obbligatorio)|Con un'infrastruttura IPv6 nativa esistente, si specifica il prefisso dell'organizzazione durante la distribuzione di accesso remoto e server di accesso remoto non configurerà automaticamente come router ISATAP. Eseguire le operazioni seguenti:<br/><br/>1.  Per garantire che i client DirectAccess siano raggiungibili dalla intranet, è necessario modificare il routing IPv6 in modo che il traffico della route predefinita venga inoltrato al server di accesso remoto. Se lo spazio degli indirizzi IPv6 intranet utilizza un indirizzo diverso da un singolo prefisso di indirizzo IPv6 a 48 bit, è necessario specificare il prefisso IPv6 dell'organizzazione rilevanti durante la distribuzione.<br/>2.  Se si è attualmente connessi alla rete Internet IPv6, è necessario configurare il traffico della route predefinita in modo che venga inoltrato al server di accesso remoto e quindi configurare le connessioni appropriate e le route nel server di accesso remoto, in modo che il route predefinito il traffico viene inoltrato al dispositivo che è connesso alla rete Internet IPv6.|  
|Distribuzione di ISATAP esistente|Se si dispone di un'infrastruttura ISATAP esistente, durante la distribuzione richiesto per il prefisso a 48 bit dell'organizzazione e il server di accesso remoto non configurerà automaticamente come router ISATAP. Per garantire che i client DirectAccess siano raggiungibili dalla intranet, è necessario modificare l'infrastruttura di routing IPv6 in modo che il traffico della route predefinita venga inoltrato al server di accesso remoto. Questa modifica deve essere eseguita sul router ISATAP esistente a cui i client intranet devono essere inoltra il traffico predefinito.|  
|Nessuna connettività IPv6 esistente|Quando la configurazione guidata accesso remoto rileva che il server non dispone di alcuna connettività IPv6 nativa o basata su ISATAP, viene automaticamente deriva un prefisso a 48 bit basato su 6to4 per la rete intranet e configura il server di accesso remoto come router ISATAP per fornire IPv6 connettività agli host ISATAP nella Intranet. (Un prefisso basato su 6to4 viene utilizzato solo se il server dispone di indirizzi pubblici, in caso contrario, il prefisso viene generato automaticamente da un intervallo di indirizzi locali univoci).<br/><br/>Per usare ISATAP eseguire le operazioni seguenti:<br/><br/>1.  Registrare il nome ISATAP in un server DNS per ogni dominio in cui si desidera abilitare la connettività basata su ISATAP, in modo che il nome ISATAP sia risolvibile dal server DNS interno per l'indirizzo IPv4 interno del server di accesso remoto.<br/>2.  Per impostazione predefinita, i server DNS che eseguono Windows Server 2012, Windows Server 2008 R2, Windows Server 2008 o Windows Server 2003 bloccano la risoluzione del nome ISATAP con l'elenco di blocchi query globali. Per abilitare ISATAP, è necessario rimuovere il nome ISATAP dall'elenco di blocco. Per altre informazioni, vedere [rimuovere ISATAP dall'elenco di blocchi Query globali DNS](https://go.microsoft.com/fwlink/p/?LinkId=168593).<br/><br/>Host ISATAP basati su Windows in grado di risolvere automaticamente il nome ISATAP configurare un indirizzo con il server di accesso remoto come indicato di seguito:<br/><br/>1.  Un indirizzo IPv6 basato su ISATAP in un'interfaccia di tunneling ISATAP<br/>2.  Una route a 64 bit che fornisce la connettività ad altri host ISATAP nella intranet<br/>3.  Una route IPv6 predefinita che punta al server di accesso remoto. La route predefinita garantisce che gli host ISATAP intranet possano raggiungere i client DirectAccess<br/><br/>Quando gli host ISATAP basati su Windows ottengono un indirizzo IPv6 basato su ISATAP, iniziano a usare il traffico incapsulato ISATAP per comunicare se la destinazione è anche un host ISATAP. Poiché ISATAP utilizza una singola subnet a 64 bit per l'intera rete intranet, la comunicazione passa da un modello di IPv4 segmentato di comunicazione, a un modello di comunicazione singola subnet con IPv6. Ciò può influenzare il comportamento di alcuni servizi di dominio Active Directory (AD DS) e le applicazioni che usano la configurazione di siti di Active Directory e servizi. Ad esempio, se è stato usato i siti di Active Directory e snap-in servizi per configurare siti, subnet basate su IPv4 e trasporti tra siti per inoltrare le richieste ai server all'interno dei siti, questa configurazione non viene utilizzata dagli host ISATAP.<br/><br/><ol><li>Per configurare siti di Active Directory e servizi per l'inoltro all'interno dei siti per gli host ISATAP, per ogni oggetto di subnet IPv4, è necessario configurare un oggetto di subnet IPv6 equivalente, in cui il prefisso dell'indirizzo IPv6 per la subnet esprima lo stesso host intervallo di ISATAP indirizzi della subnet IPv4. Ad esempio, per IPv4 192.168.99.0/24 subnet e l'indirizzo ISATAP a 64 bit del prefisso 2002:836b:1:8000::/ 64, il prefisso di indirizzo IPv6 equivalente per l'oggetto della subnet IPv6 è 2002:836b:1:8000:0:5efe:192.168.99.0/120. Per IPv4 prefisso lunghezza arbitraria (impostato su 24 nell'esempio), è possibile determinare la lunghezza del prefisso IPv6 corrispondente dalla formula 96 + IPv4PrefixLength.</li><li>Per gli indirizzi IPv6 dei client DirectAccess, aggiungere quanto segue:<br/><br/><ul><li>Per i client DirectAccess basati su Teredo: Una subnet IPv6 per l'intervallo 2001:0:WWXX:YYZZ::/ / 64, in cui WWXX: YYZZ è la versione esadecimale con due punti del primo indirizzo IPv4 con connessione Internet del server di accesso remoto. .</li><li>Per i client DirectAccess basato su IP-HTTPS: Una subnet IPv6 per l'intervallo 2002:WWXX:YYZZ:8100::/ / 56, in cui WWXX: YYZZ è la versione esadecimale con due punti del primo indirizzo IPv4 con connessione Internet (w.x.y. z) del server di accesso remoto. .</li><li>Per i client DirectAccess basato su 6to4: Una serie di prefissi IPv6 basati su 6to4-che iniziano con 2002 e rappresentano i prefissi degli indirizzi IPv4 pubblici di area amministrati da IANA (Internet Assigned Numbers Authority) e autorità di registrazione regionali. Il prefisso basato su 6to4 per un w.x.y.z/n prefisso di indirizzo IPv4 pubblico è 2002:WWXX:YYZZ::/ / [16 + n], in cui WWXX: YYZZ è la versione esadecimale con due punti w.x.y.z.<br/><br/>        Ad esempio, l'intervallo 7.0.0.0/8 è amministrato da ARIN (American Registry for Internet Numbers (ARIN) per il Nord America. Il corrispondente prefisso basato su 6to4 per questo intervallo di indirizzi IPv6 pubblici è 2002:700::/ / 24. Per informazioni sullo spazio di indirizzi pubblici IPv4, vedere [registro dello spazio degli indirizzi IPv4 di IANA](https://www.iana.org/assignments/ipv4-address-space/ipv4-address-space.xml). .</li></ul></li></ol>|  
  
> [!IMPORTANT]  
> Assicurarsi di non avere indirizzi IP pubblici nell'interfaccia interna del server DirectAccess. Se si dispone di indirizzo IP pubblico nell'interfaccia interna, la connettività tramite ISATAP potrebbe non riuscire.  
  
### <a name="plan-firewall-requirements"></a>Pianificare i requisiti del firewall  
Se il server di Accesso remoto è protetto da un firewall periferico, sono necessarie le seguenti eccezioni per il traffico di Accesso remoto quando il server di Accesso remoto si trova nella rete Internet IPv4:  
  
-   Per IP-HTTPS: Transmission Control Protocol (TCP) destinazione porta 443 e porta TCP di origine 443 in uscita.  
  
-   Per il traffico Teredo: Porta di destinazione User Datagram Protocol (UDP) 3544 in entrata e porta UDP di origine 3544 in uscita.  
  
-   Per il traffico 6to4: Protocollo 41 in entrata e in uscita.  
  
    > [!NOTE]  
    > Per il traffico Teredo e 6to4, tali esenzioni devono essere applicate per entrambi gli indirizzi IPv4 pubblici consecutivi con connessione Internet sul server di Accesso remoto.  
    >   
    > Per IP-HTTPS le eccezioni devono essere applicate all'indirizzo registrato nel server DNS pubblico.  
  
-   Se si distribuisce accesso remoto con una sola scheda di rete e si installa il server dei percorsi di rete nel server di accesso remoto, la porta TCP 62000.  
  
    > [!NOTE]  
    > Questa esenzione si trova nel server di accesso remoto e le esenzioni precedente sono sul firewall periferico.  
  
Le seguenti eccezioni sono necessarie per il traffico di accesso remoto quando il server di accesso remoto nella rete Internet IPv6:  
  
-   Protocollo IP 50  
  
-   Porta UDP di destinazione 500 in entrata e porta UDP di origine 500 in uscita.  
  
-   Traffico ICMPv6 in entrata e in uscita (solo quando si usa Teredo).  
  
Quando si usano altri firewall, applicare le seguenti eccezioni firewall di rete interna per il traffico di accesso remoto:  
  
-   Per ISATAP: Protocollo 41 in entrata e in uscita  
  
-   Per tutto il traffico IPv4 e IPv6: TCP/UD  
  
-   Per Teredo: ICMP per tutto il traffico IPv4/IPv6  
  
### <a name="plan-certificate-requirements"></a>Pianificare i requisiti dei certificati  
Esistono tre scenari che richiedono certificati quando si distribuisce un singolo server di accesso remoto.  
  
-   **L'autenticazione IPsec**: Requisiti dei certificati per IPsec includono un certificato del computer usato dai computer client DirectAccess quando stabiliscono la connessione IPsec con il server di accesso remoto e un certificato del computer che viene usato dal server di accesso remoto per stabilire Connessioni IPsec con i client DirectAccess.  
  
    Per DirectAccess in Windows Server 2012, l'uso di questi certificati IPsec non è obbligatorio. In alternativa, il server di accesso remoto può fungere da proxy per l'autenticazione Kerberos senza necessità di certificati. Se viene utilizzata l'autenticazione Kerberos, viene eseguito su SSL e il protocollo Kerberos viene utilizzato il certificato è stato configurato per IP-HTTPS. Alcuni scenari aziendali (inclusi distribuzione multisito e l'autenticazione client One Time password) richiedono l'uso dell'autenticazione del certificato e non l'autenticazione Kerberos.  
  
-   **Server IP-HTTPS**: Quando si configura accesso remoto, il server di accesso remoto viene configurato automaticamente per funzionare come listener web IP-HTTPS. Il sito IP-HTTPS richiede un certificato del sito Web e i computer client devono riuscire a contattare il sito dell'elenco di revoche di certificati (CRL) per il certificato.  
  
-   **Server dei percorsi di rete**: Il server dei percorsi rete è un sito Web usato per rilevare se i computer client si trovano nella rete aziendale. Il server dei percorsi di rete richiede un certificato di sito Web. I client DirectAccess devono poter contattare il sito CRL per il certificato.  
  
I requisiti di autorità (CA) certificazione per ognuno di questi scenari è riepilogato nella tabella seguente.  
  
|Autenticazione IPsec|Server IP-HTTPS|Server dei percorsi di rete|  
|------------|----------|--------------|  
|Una CA interna è necessaria per emettere certificati del computer per il server di accesso remoto e i client per l'autenticazione IPsec quando non si usa il protocollo Kerberos per l'autenticazione.|CA interna: È possibile usare una CA interna per emettere il certificato IP-HTTPS, però è necessario verificare che il punto di distribuzione CRL sia disponibile esternamente.|CA interna: È possibile usare una CA interna per emettere il certificato del sito Web del server dei percorsi di rete. Verificare che il punto di distribuzione CRL sia a disponibilità elevata nella rete interna.|  
||Certificato autofirmato: È possibile usare un certificato autofirmato per il server IP-HTTPS. Non è possibile usare un certificato autofirmato in una distribuzione multisito.|Certificato autofirmato: È possibile usare un certificato autofirmato per il sito Web server percorsi di rete; Tuttavia, è possibile utilizzare un certificato autofirmato in distribuzioni multisito.|  
||CA pubblica: È consigliabile utilizzare una CA pubblica per emettere il certificato IP-HTTPS, in questo modo che il punto di distribuzione CRL sia disponibile esternamente.||  
  
#### <a name="plan-computer-certificates-for-ipsec-authentication"></a>Pianificare i certificati del computer per l'autenticazione IPsec  
Se si usa l'autenticazione IPsec basata su certificato, il server di accesso remoto e i client sono necessari per ottenere un certificato del computer. Il modo più semplice per installare i certificati è usare criteri di gruppo per configurare la registrazione automatica per i certificati del computer. In questo modo, tutti i membri del dominio ottengono un certificato da una CA globale (enterprise). Se non è una CA impostato all'interno dell'organizzazione, vedere [Servizi certificati Active Directory](https://technet.microsoft.com/library/cc770357.aspx).  
  
Questo certificato ha i seguenti requisiti:  
  
-   Il certificato deve avere l'autenticazione del client utilizzo chiave esteso (EKU).  
  
-   Il client e i certificati del server devono essere correlati per lo stesso certificato radice. che deve essere selezionato nelle impostazioni di configurazione DirectAccess.  
  
#### <a name="plan-certificates-for-ip-https"></a>Pianificare i certificati per IP-HTTPS  
Il server di Accesso remoto funge da listener IP-HTTPS ed è necessario installare manualmente un certificato del sito Web HTTPS nel server. Durante la pianificazione, prendere in considerazione gli elementi seguenti:  
  
-   L'uso di una CA pubblica è consigliato perché consente di avere subito disponibili gli elenchi di revoche di certificati (CRL).  
  
-   Nel campo soggetto, specificare l'indirizzo IPv4 della scheda Internet del server di accesso remoto o il nome di dominio completo dell'URL IP-HTTPS (indirizzo ConnectTo). Se il server di Accesso remoto si trova dietro un dispositivo NAT, deve essere specificato il nome pubblico o l'indirizzo del dispositivo NAT.  
  
-   Il nome comune del certificato deve corrispondere al nome del sito IP-HTTPS.  
  
-   Per il **Enhanced Key Usage** campo, utilizzare l'identificatore di oggetto (OID) di autenticazione Server.  
  
-   Per il campo **Punti di distribuzione Elenco di revoche di certificati (CRL)** , specificare un punto di distribuzione CRL accessibile dai client DirectAccess connessi a Internet.  
  
    > [!NOTE]  
    > Questo è solo necessario per i client che eseguono Windows 7.  
  
-   Il certificato IP-HTTPS deve avere una chiave privata.  
  
-   Il certificato IP-HTTPS deve essere importato direttamente nell'archivio personale.  
  
-   Nei nomi dei certificati IP-HTTPS possono essere presenti caratteri jolly.  
  
#### <a name="plan-website-certificates-for-the-network-location-server"></a>Pianificare i certificati del sito Web per il server dei percorsi di rete  
Quando si pianifica il sito Web server percorsi di rete, considerare quanto segue:  
  
-   Nel **soggetto** specificare un indirizzo IP dell'interfaccia intranet del server dei percorsi di rete o il nome di dominio COMPLETO dell'URL del percorso di rete.  
  
-   Per il **Enhanced Key Usage** campo, usare l'OID di autenticazione Server.  
  
-   Per il **punti di distribuzione CRL** campo, utilizzare un punto di distribuzione CRL accessibile dai client DirectAccess connessi alla intranet. Questo punto di distribuzione CRL non deve essere accessibile fuori dalla rete interna.  
  
> [!NOTE]  
> Verificare che i certificati per server dei percorsi di rete e IP-HTTPS abbiano un nome soggetto. Se il certificato viene utilizzato un nome alternativo, sarà non accettato dalla procedura guidata accesso remoto.  
  
#### <a name="plan-dns-requirements"></a>Pianificare i requisiti DNS  
In questa sezione illustra i requisiti DNS per i client e server in una distribuzione di accesso remoto.  
  
##### <a name="directaccess-client-requests"></a>Richieste dei client DirectAccess  
DNS viene usato per risolvere le richieste provenienti dai computer client DirectAccess non presenti nella rete interna. I client DirectAccess tentano di connettersi al server dei percorsi di rete DirectAccess per determinare se si trovano su Internet o alla rete aziendale.  
  
-   Se la connessione ha esito positivo, i client vengono determinati per essere sulla intranet, DirectAccess non viene usato e le richieste client vengono risolte usando il server DNS configurato nella scheda di rete del computer client.  
  
-   Se la connessione non riesce, si presuppone che i client si trovino sulla rete Internet. I client DirectAccess usano la tabella dei criteri di risoluzione dei nomi (NRPT) per determinare quale server DNS usare per la risoluzione delle richieste di nomi. È possibile specificare che i client debbano usare DNS64 DirectAccess per risolvere i nomi o un server DNS interno alternativo.  
  
Quando si esegue la risoluzione dei nomi, la tabella dei criteri di risoluzione dei nomi viene usata dai client DirectAccess per stabilire come gestire una richiesta. I client richiedono un nome di dominio completo o un nome con etichetta singola, ad esempio <https://internal>. Se è richiesto un nome con etichetta singola, viene aggiunto un suffisso DNS per trasformarlo in un nome di dominio completo. Se la query DNS corrisponde a una voce nella tabella NRPT e per la voce è specificato DNS4 o un server DNS intranet, la query viene inviata per la risoluzione dei nomi usando il server specificato. Se esiste una corrispondenza, ma si specifica alcun server DNS, una regola di esenzione e normale viene applicata la risoluzione dei nomi.  
  
Quando viene aggiunto un nuovo suffisso alla tabella NRPT nella console di gestione accesso remoto, i server DNS predefiniti per il suffisso possono essere individuati automaticamente facendo il **rileva** pulsante. Il rilevamento automatico funziona nel modo seguente:  
  
-   Se la rete aziendale è basata su IPv4 o Usa IPv4 e IPv6, l'indirizzo predefinito è l'indirizzo DNS64 della scheda interna nel server di accesso remoto.  
  
-   Se la rete aziendale è basata su IPv6, l'indirizzo predefinito è l'indirizzo IPv6 dei server DNS nella rete aziendale.  
  
##### <a name="infrastructure-servers"></a>Server dell'infrastruttura  
  
-   **Server dei percorsi di rete**  
  
    I client DirectAccess tentano di raggiungere il server dei percorsi di rete per determinare se si trovano sulla rete interna. I client nella rete interna devono essere in grado di risolvere il nome del server dei percorsi di rete ed è necessario impedire che la risoluzione del nome quando si trovano su Internet. Per verificarlo, il nome di dominio completo del server dei percorsi di rete viene aggiunto per impostazione predefinita come regola di esenzione nella tabella dei criteri di risoluzione dei nomi. Quando si configura Accesso remoto vengono create automaticamente anche le seguenti regole:  
  
    -   Regola di dominio radice o il nome di dominio del server di accesso remoto e gli indirizzi IPv6 corrispondenti ai server DNS intranet configurati nel server di accesso remoto di suffisso di un DNS. Ad esempio, se il server di Accesso remoto è membro del dominio corp.contoso.com, viene creata una regola per il suffisso DNS corp.contoso.com.  
  
    -   Una regola di esenzione per il nome di dominio completo del server del percorso di rete. Ad esempio, se l'URL di server percorsi di rete è <https://nls.corp.contoso.com>, viene creata una regola di esenzione per il nome di dominio completo nls.corp.contoso.com.  
  
-   **IP-HTTPS server**  
  
    Il server di accesso remoto funge da listener IP-HTTPS e Usa il certificato del server per autenticarsi nei client IP-HTTPS. Il nome IP-HTTPS deve essere risolvibile dai client DirectAccess che usano server DNS pubblici.  
  
##### <a name="connectivity-verifiers"></a>Strumenti di verifica della connettività  
Accesso remoto crea un probe Web predefinito usato dai computer client DirectAccess per verificare la connettività alla rete interna. Per verificare il corretto funzionamento del probe, è necessario registrare manualmente i seguenti nomi in DNS:  
  
-   **DirectAccess-webprobehost** dovrebbe risolvere l'indirizzo IPv4 interno del server di accesso remoto o l'indirizzo IPv6 in un ambiente solo IPv6.  
  
-   **DirectAccess-corpconnectivityhost** dovrebbe risolvere l'indirizzo localhost (loopback). È consigliabile creare i record AAAA. Il valore del record è 127.0.0.1 e il valore del record AAAA viene costruito al prefisso NAT64 127.0.0.1 come ultimi 32 bit. Il prefisso NAT64 può essere recuperato eseguendo il **Get-netnatTransitionConfiguration** cmdlet di Windows PowerShell.  
  
    > [!NOTE]  
    > Questo è valido solo in ambienti solo IPv4. In IPv4 più IPv6 o un ambiente solo IPv6, creare un solo record AAAA con l'indirizzo IP di loopback:: 1.  
  
È possibile creare strumenti di verifica della connettività aggiuntivi con altri indirizzi web su HTTP oppure PING. Per ogni strumento di verifica della connettività deve esistere una voce DNS.  
  
##### <a name="dns-server-requirements"></a>Requisiti del server DNS  
  
-   Per i client DirectAccess, è necessario utilizzare un server DNS che esegue Windows Server 2012, Windows Server 2008 R2, Windows Server 2008, Windows Server 2003 o qualsiasi server DNS che supporti IPv6.  
  
-   È consigliabile usare un server DNS che supporti gli aggiornamenti dinamici. È possibile usare server DNS che non supportano gli aggiornamenti dinamici, ma le voci è quindi necessario aggiornare manualmente.  
  
-   il FQDN per i punti di distribuzione CRL deve essere risolvibile mediante server DNS Internet. Ad esempio, se URL <https://crl.contoso.com/crld/corp-DC1-CA.crl> è il **punti di distribuzione CRL** campo del certificato IP-HTTPS del server Accesso remoto, è necessario assicurarsi che il nome di dominio completo crld.contoso.com sia risolvibile mediante server DNS Internet.  
  
#### <a name="plan-for-local-name-resolution"></a>Piano per la risoluzione dei nomi locali  
Quando si pianifica la risoluzione dei nomi locali, tenere presente quanto segue:  
  
##### <a name="nrpt"></a>NRPT  
Potrebbe essere necessario creare regole dei criteri tabella (NRPT) di risoluzione dei nomi aggiuntivi nelle situazioni seguenti:  
  
-   È necessario aggiungere più suffissi DNS per lo spazio dei nomi intranet.  
  
-   Se i nomi FQDN dei punti di distribuzione CRL sono basati sullo spazio dei nomi intranet, è necessario aggiungere regole di esenzione per i nomi FQDN dei punti di distribuzione CRL.  
  
-   Se si dispone di un ambiente DNS "split Brain", è necessario aggiungere regole di esenzione per i nomi delle risorse per il quale si desidera che i client DirectAccess che si trovano su Internet per accedere la versione Internet, anziché la versione intranet.  
  
-   Se si reindirizza traffico a un sito Web esterno tramite i server di proxy web intranet, il sito Web esterno è disponibile solo dalla rete intranet. Usa gli indirizzi dei server proxy web per consentire le richieste in ingresso. In questo caso, aggiungere una regola di esenzione per il nome FQDN del sito Web esterno e specificare che la regola Usa server proxy web intranet, anziché gli indirizzi IPv6 dei server DNS intranet.  
  
    Ad esempio, si supponga che si sta testando un sito Web esterno denominato test.contoso.com. Questo nome non è risolvibile tramite i server DNS Internet, ma il server proxy web di Contoso sa come risolvere il nome e come indirizzare le richieste per il sito Web per il server web esterno. Per impedire l'accesso al sito da parte di utenti che non si trovano nella Intranet di Contoso, il sito Web esterno consente le richieste solo dall'indirizzo Internet IPv4 del proxy Web di Contoso. Di conseguenza, gli utenti della intranet possono accedere al sito Web poiché utilizzano il proxy web di Contoso, ma gli utenti di DirectAccess non possono perché non usano il proxy web di Contoso. Configurando una regola di esenzione della tabella dei criteri di risoluzione dei nomi per test.contoso.com che usa il proxy Web di Contoso, le richieste di pagine Web per test.contoso.com vengono indirizzate al server proxy Web Intranet sulla rete Internet IPv4.  
  
##### <a name="single-label-names"></a>Nomi con etichetta singola  
Singola, ad esempio i nomi delle etichette, <https://paycheck>, vengono usati talvolta per i server intranet. Se è richiesto un nome con etichetta singola e un elenco di ricerca suffisso DNS è configurato, i suffissi DNS nell'elenco verranno aggiunto al nome con etichetta singola. Ad esempio, quando un utente in un computer che è un membro del dominio corp.contoso.com digita <https://paycheck> nel web browser, il nome di dominio completo che viene costruito come il nome è paycheck.corp.contoso.com. Per impostazione predefinita, il suffisso aggiunto si basa sul suffisso DNS primario del computer client.  
  
> [!NOTE]  
> In un scenario di spazio dei nomi indipendente (in cui uno o più computer di dominio hanno un suffisso DNS che corrisponde al dominio di Active Directory in cui i computer sono membri), è necessario assicurarsi che l'elenco di ricerca sia personalizzato e includa tutti i suffissi richiesti. Per impostazione predefinita, la configurazione guidata accesso remoto configura il nome DNS di Active Directory come suffisso DNS primario nel client. Aggiungere il suffisso DNS emesso dai client per la risoluzione dei nomi.  
  
Se più domini e Windows Internet Name Service (WINS) vengono distribuiti all'interno dell'organizzazione e ci si connette in remoto, i nomi singoli possono essere risolti come segue:  
  
-   Tramite la distribuzione di una zona di ricerca diretta WINS in DNS. Quando si risolve computername.dns.zone1.corp.contoso.com, la richiesta viene indirizzata al server WINS che usa solo il nome del computer. Il client ritiene di emettere una richiesta di record DNS A regolare, ma è in realtà una richiesta NetBIOS.  
  
    Per altre informazioni, vedere [la gestione di una zona di ricerca diretta](https://technet.microsoft.com/library/cc816891(WS.10).aspx).  
  
-   Aggiungendo un suffisso DNS (ad esempio dns.zone1.corp.contoso.com) per l'oggetto Criteri di gruppo di dominio predefinito.  
  
##### <a name="split-brain-dns"></a>DNS "split brain"  
DNS "split Brain" si intende l'uso dello stesso dominio DNS per la risoluzione dei nomi Internet e intranet.  
  
Per le distribuzioni DNS "split Brain", è necessario elencare i nomi di dominio completi duplicati in Internet e intranet e decidere quali risorse il client DirectAccess deve raggiungere la rete intranet o la versione di Internet. Quando si desidera che i client DirectAccess raggiungano la versione Internet, è necessario aggiungere il nome di dominio completo corrispondente come regola di esenzione a NRPT per ogni risorsa.  
  
In un ambiente DNS "split Brain", se si vuole che entrambe le versioni della risorsa sia disponibile, configurare le risorse intranet con nomi che non duplicare i nomi utilizzati in Internet. Indicare quindi agli utenti di usare il nome alternativo quando accedono a una risorsa nella intranet. Ad esempio, configurare www.internal.contoso.com per il nome interno della www.contoso.com.  
  
In un ambiente DNS non "split brain", lo spazio dei nomi Internet è diverso da quello Intranet. Contoso Corporation utilizza ad esempio contoso.com in Internet e corp.contoso.com nella Intranet. Poiché tutte le risorse Intranet utilizzano il suffisso DNS corp.contoso.com, la regola della tabella dei criteri di risoluzione dei nomi per corp.contoso.com indirizza tutte le query relative ai nomi DNS per le risorse Intranet ai server DNS Intranet. Le query DNS per i nomi con suffisso contoso.com non corrispondono la regola dello spazio dei nomi intranet di corp.contoso.com nella tabella NRPT e vengono inviati ai server DNS Internet. Con una distribuzione DNS non "split brain", poiché i nomi di dominio completi non vengono duplicati per le risorse Intranet e Internet, non è necessaria alcuna configurazione aggiuntiva per la tabella dei criteri di risoluzione dei nomi. I client DirectAccess possono accedere alle risorse Internet e intranet per la propria organizzazione.  
  
##### <a name="plan-local-name-resolution-behavior-for-directaccess-clients"></a>Pianificare il comportamento di risoluzione dei nomi locali per i client DirectAccess  
Se un nome non può essere risolto con DNS, il servizio Client DNS in Windows Server 2012, Windows 8, Windows Server 2008 R2 e Windows 7 è possibile usare la risoluzione dei nomi locali, con Link-Local Multicast Name LLMNR (Resolution) e NetBIOS su protocolli TCP/IP, per risolvere il  nome della subnet locale. La risoluzione dei nomi locali è in genere necessaria per la connettività peer-to-peer se il computer si trova in reti private, ad esempio reti domestiche a subnet singola.  
  
Quando il servizio Client DNS esegue la risoluzione dei nomi locali per i nomi dei server intranet e il computer è connesso a una subnet condivisa in Internet, gli utenti malintenzionati possono acquisire LLMNR e NetBIOS su messaggi TCP/IP per determinare i nomi dei server intranet. Nella pagina DNS della configurazione guidata Server di infrastruttura, è possibile configurare il comportamento di risoluzione dei nomi locali in base ai tipi di risposte ricevuti dai server DNS intranet. Sono disponibili le seguenti opzioni:  
  
-   **Usare la risoluzione dei nomi locali se il nome non esiste nel DNS**: Questa opzione è la più sicura perché il client DirectAccess esegue la risoluzione dei nomi locali solo per i nomi dei server che non possono essere risolti dal server DNS Intranet. Se i server DNS Intranet sono raggiungibili, i nomi di server Intranet vengono risolti. Se i server DNS Intranet non sono raggiungibili o se si verificano altri tipi di errori DNS, i nomi di server Intranet non vengono comunicati alla subnet mediante la risoluzione dei nomi locali.  
  
-   **Usare la risoluzione dei nomi locali se il nome non esiste nel DNS o server DNS non sono raggiungibili quando il computer client si trova in una rete privata (scelta consigliata)** : Questa opzione è consigliata perché consente l'uso della risoluzione dei nomi locali in una rete privata solo quando i server DNS Intranet non sono raggiungibili.  
  
-   **Usare la risoluzione dei nomi locali per qualsiasi tipo di errore di risoluzione DNS (opzione meno sicura)** : Si tratta dell'opzione meno sicura, poiché i nomi dei server della rete Intranet possono essere comunicati alla subnet locale mediante risoluzione dei nomi locali.  
  
#### <a name="plan-the-network-location-server-configuration"></a>Pianificare la configurazione di rete percorso server  
Il server dei percorsi di rete è un sito Web usato per rilevare se i client DirectAccess si trovano nella rete aziendale. I client nella rete aziendale non usano DirectAccess per raggiungere le risorse interne; ma vi si connettono direttamente.  
  
Il sito Web server percorsi di rete può essere ospitato nel server di accesso remoto o in un altro server all'interno dell'organizzazione. Se si ospita il server dei percorsi rete sul server di accesso remoto, il sito Web viene creato automaticamente quando si distribuisce accesso remoto. Se si ospita il server dei percorsi rete in un altro server che eseguono un sistema operativo Windows, è necessario assicurarsi che sia installato Internet Information Services (IIS) in tale server e che viene creato il sito Web. Accesso remoto non configura le impostazioni nel server del percorso di rete.  
  
Assicurarsi che il sito Web server percorsi di rete soddisfi i requisiti seguenti:  
  
-   Ha un certificato server HTTPS.  
  
-   Con disponibilità elevata per i computer nella rete interna.  
  
-   Non è accessibile ai computer client DirectAccess su Internet.  
  
-  
  
Inoltre, prendere in considerazione i requisiti seguenti per i client quando si configura il sito Web server percorsi di rete:  
  
-   I computer client DirectAccess devono considerare attendibile la CA che ha emesso il certificato del server nel sito Web del server dei percorsi di rete.  
  
-   I computer client DirectAccess nella rete interna devono poter risolvere il nome del sito del server dei percorsi di rete.  
  
##### <a name="plan-certificates-for-the-network-location-server"></a>Pianificare i certificati per server dei percorsi di rete  
Una volta ottenuto il certificato del sito Web da utilizzare per il server dei percorsi di rete, considerare quanto segue:  
  
-   Nel **soggetto** specificare l'indirizzo IP dell'interfaccia intranet del server dei percorsi di rete o il nome di dominio COMPLETO dell'URL del percorso di rete.  
  
-   Per il **Enhanced Key Usage** campo, usare l'OID di autenticazione Server.  
  
-   Il certificato server percorsi di rete deve essere confrontato con un elenco di revoche di certificati (CRL). Per il **punti di distribuzione CRL** campo, utilizzare un punto di distribuzione CRL accessibile dai client DirectAccess connessi alla intranet. Questo punto di distribuzione CRL non deve essere accessibile fuori dalla rete interna.  
  
##### <a name="plan-dns-for-the-network-location-server"></a>Pianificare il DNS per il server dei percorsi di rete  
I client DirectAccess tentano di raggiungere il server dei percorsi di rete per determinare se si trovano sulla rete interna. I client nella rete interna devono poter risolvere il nome del server dei percorsi di rete, ma non quando si trovano sulla rete Internet. Per verificarlo, il nome di dominio completo del server dei percorsi di rete viene aggiunto per impostazione predefinita come regola di esenzione nella tabella dei criteri di risoluzione dei nomi.  
  
### <a name="plan-management-servers-configuration"></a>Pianificare la configurazione dei server di gestione  
I client DirectAccess avviano le comunicazioni con i server di gestione che forniscono servizi quali Windows Update e aggiornamenti antivirus. Inoltre, i client DirectAccess utilizzano il protocollo Kerberos per l'autenticazione a controller di dominio prima di accedere alla rete interna. Durante la gestione remota dei client DirectAccess, i server di gestione comunicano con i computer client per eseguire funzioni di gestione quali le valutazioni degli inventari software o hardware. Accesso remoto può individuare automaticamente alcuni server di gestione, tra cui:  
  
-   Controller di dominio: Per i domini che contengono i computer client e per tutti i domini nella stessa foresta del server di accesso remoto, viene eseguita l'individuazione automatica dei controller di dominio.  
  
-   Server di System Center Configuration Manager  
  
I controller di dominio e System Center Configuration Manager Server vengono rilevati automaticamente la prima volta DirectAccess è configurato. I controller di dominio rilevati non vengono visualizzati nella console, ma le impostazioni possono essere recuperate usando i cmdlet di Windows PowerShell. Se vengono modificati i controller di dominio o server System Center Configuration Manager, fare clic su **server di gestione aggiornamento** nella console Aggiorna l'elenco di server di gestione.  
  
**Requisiti del server di gestione**  
  
-   Server di gestione devono essere accessibili tramite il tunnel dell'infrastruttura. Quando si configura Accesso remoto, l'aggiunta di server all'elenco dei server di gestione li rende automaticamente accessibili su questo tunnel.  
  
-   Server di gestione che avviano connessioni ai client DirectAccess devono supportare completamente IPv6, tramite un indirizzo IPv6 nativo o tramite un indirizzo assegnato da ISATAP.  
  
### <a name="plan-active-directory-requirements"></a>Pianificare i requisiti di Active Directory  
Accesso remoto Usa Active Directory come segue:  
  
-   **Autenticazione**: Il tunnel dell'infrastruttura Usa l'autenticazione NTLMv2 per l'account computer che si connette al server di accesso remoto e l'account deve trovarsi in un dominio di Active Directory. Il tunnel intranet Usa l'autenticazione Kerberos per l'utente per creare il tunnel della intranet.  
  
-   **Oggetti Criteri di gruppo**: Accesso remoto riunisce le impostazioni di configurazione in oggetti di criteri di gruppo (GPO), che vengono applicate ai server di accesso remoto, i client e server applicazioni interni.  
  
-   **Gruppi di sicurezza**: Accesso remoto Usa i gruppi di sicurezza per raccogliere e identificare i computer client DirectAccess. Oggetti Criteri di gruppo vengono applicati ai gruppi di sicurezza necessarie.  
  
Quando si pianifica un ambiente Active Directory per una distribuzione di accesso remoto, prendere in considerazione i requisiti seguenti:  
  
-   Almeno un controller di dominio viene installato nel sistema operativo Windows Server 2012, Windows Server 2008 R2 Windows Server 2008 o Windows Server 2003.  
  
    Se il controller di dominio in una rete perimetrale (ed è pertanto raggiungibile dalla scheda di rete con connessione Internet del server di accesso remoto), impedire il server di accesso remoto di raggiungerlo. È necessario aggiungere filtri pacchetti sul controller di dominio per impedire la connettività all'indirizzo IP della scheda Internet.  
  
-   Il server di Accesso remoto deve essere membro di un dominio.  
  
-   I client DirectAccess devono essere membri di un dominio. I client possono appartenere a:  
  
    -   Qualsiasi dominio nella stessa foresta del server di Accesso remoto.  
  
    -   Qualsiasi dominio che abbia un trust bidirezionale con il dominio del server di Accesso remoto.  
  
    -   Qualsiasi dominio in una foresta che abbia una relazione di trust bidirezionale con la foresta del dominio del server Accesso remoto.  
  
> [!NOTE]  
> -   Il server di Accesso remoto non può essere un controller di dominio.  
> -   Il controller di dominio Active Directory che viene usato per l'accesso remoto non deve essere raggiungibile dalla scheda Internet esterna del server di accesso remoto (la scheda non deve trovarsi nel profilo del dominio di Windows Firewall).  
  
#### <a name="plan-client-authentication"></a>Pianificare l'autenticazione client  
In accesso remoto in Windows Server 2012, è possibile scegliere tra l'uso dell'autenticazione Kerberos incorporata, che Usa nomi utente e password, o utilizzo dei certificati per l'autenticazione IPsec dei computer.  
  
**L'autenticazione Kerberos**: Quando si sceglie di usare le credenziali di Active Directory per l'autenticazione, DirectAccess utilizza innanzitutto l'autenticazione Kerberos per il computer e quindi Usa l'autenticazione Kerberos per l'utente. Quando si usa questa modalità di autenticazione, DirectAccess Usa un solo tunnel di sicurezza che fornisce l'accesso per il server DNS, controller di dominio e altri server nella rete interna  
  
**L'autenticazione IPsec**: Quando si sceglie di usare l'autenticazione a due fattori o protezione accesso alla rete, DirectAccess usa due tunnel di sicurezza. La configurazione guidata accesso remoto Configura regole di sicurezza delle connessioni in Windows Firewall con sicurezza avanzata. Quando la negoziazione della protezione IPsec al server di accesso remoto, queste regole specificano le credenziali seguenti:  
  
-   Il tunnel dell'infrastruttura Usa le credenziali del certificato computer per la prima autenticazione e le credenziali utente (NTLMv2) per la seconda autenticazione. Le credenziali utente forzano l'uso di Authenticated Internet Protocol (AuthIP) e forniscono l'accesso a un server DNS e controller di dominio prima che il client DirectAccess può utilizzare le credenziali Kerberos per il tunnel della intranet.  
  
-   Il tunnel intranet Usa le credenziali del certificato computer per la prima autenticazione e le credenziali utente (Kerberos V5) per la seconda autenticazione.  
  
#### <a name="plan-multiple-domains"></a>Pianificare più domini  
L'elenco dei server di gestione deve includere i controller di dominio di tutti i domini che contengono gruppi di sicurezza con computer client DirectAccess. Deve contenere tutti i domini che contengono gli account utente che potrebbero usare i computer configurati come client DirectAccess. In questo modo, si assicura gli utenti non presenti nello stesso dominio del computer client che stanno usando vengano autenticati con un controller di dominio nel dominio utente.  
  
L'autenticazione avviene automaticamente se i domini appartengono alla stessa foresta. Se è presente un gruppo di sicurezza con i computer client o server applicazioni che si trovano in foreste diverse, è possibile che i controller di dominio di queste foreste non vengono rilevati automaticamente. Gli insiemi di strutture inoltre non vengono rilevati automaticamente. È possibile eseguire l'attività **server di gestione aggiornamenti** nel **Gestione accesso remoto** per rilevare questi controller di dominio.  
  
Dove possibile, devono essere aggiunti suffissi comuni per la risoluzione dei nomi durante la distribuzione di accesso remoto. Ad esempio, se si hanno due domini, domain1.corp.contoso.com e domain2.corp.contoso.com, è possibile aggiungere una voce del suffisso DNS comune con il suffisso del nome di dominio corp.contoso.com, invece di due voci nella tabella dei criteri di risoluzione dei nomi. Ciò avviene automaticamente per i domini nella stessa radice. I domini che non si trovano nella stessa radice devono essere aggiunti manualmente.  
  
### <a name="plan-group-policy-object-creation"></a>Creazione di oggetti Criteri di gruppo del piano  
Quando si configura accesso remoto, le impostazioni DirectAccess vengono raccolte in oggetti Criteri di gruppo (GPO). Due oggetti Criteri di gruppo vengono popolati con impostazioni DirectAccess e vengono distribuiti come indicato di seguito:  
  
-   **GRUPPO di client DirectAccess**: Questo oggetto Criteri di gruppo contiene le impostazioni client, incluse impostazioni della tecnologia di transizione IPv6, le voci NRPT e le regole di sicurezza connessione per Windows Firewall con sicurezza avanzata. L'oggetto Criteri di gruppo viene applicato ai gruppi di sicurezza specificati per i computer client.  
  
-   **DirectAccess server oggetto Criteri di gruppo**: Questo oggetto Criteri di gruppo contiene le impostazioni di configurazione DirectAccess applicate a qualsiasi server configurato come server di accesso remoto nella distribuzione. Contiene inoltre le regole di sicurezza connessione per Windows Firewall con sicurezza avanzata.  
  
> [!NOTE]  
> Configurazione dei server applicazioni non è supportata nella gestione remota dei client DirectAccess perché i client non è possibile accedere alla rete interna del server DirectAccess in cui risiedono i server applicazioni. Passaggio 4 nella schermata di configurazione di configurazione dell'accesso remoto non è disponibile per questo tipo di configurazione.  
  
È possibile configurare oggetti Criteri di gruppo manualmente o automaticamente.  
  
**Automaticamente**: Quando si specifica che i GPO vengono creati automaticamente, viene specificato un nome predefinito per ogni oggetto Criteri di gruppo.  
  
**Manualmente**: È possibile usare oggetti Criteri di gruppo predefiniti dall'amministratore di Active Directory.  
  
Quando si configura il GPO, prendere in considerazione i seguenti avvisi:  
  
-   Dopo aver configurato DirectAccess in modo da usare specifici oggetti Criteri di gruppo, non sarà possibile configurarlo in modo da usarne altri.  
  
-   Usare la procedura seguente per eseguire il backup di oggetti tutti di criteri di gruppo l'accesso remoto prima di eseguire i cmdlet DirectAccess:  
  
    [Backup e ripristino della configurazione di Accesso remoto](https://go.microsoft.com/fwlink/?LinkID=257928).  
  
-   Se usano automaticamente o manualmente configurati oggetti Criteri di gruppo, è necessario aggiungere un criterio di rilevamento dei collegamenti lenti se i client usano 3G. Il percorso per **criteri: Configurare il rilevamento collegamento lento Criteri di gruppo** è:  
  
    **Configurazione computer/Criteri/Modelli amministrativi/Sistema/Criteri di gruppo**.  
  
-   Se non esistono autorizzazioni corrette per il collegamento di oggetti Criteri di gruppo, viene generato un avviso. L'operazione di accesso remoto continuerà, ma il collegamento non verrà eseguito. Se viene emesso questo avviso, verranno creati i collegamenti non essere automaticamente, anche se le autorizzazioni vengono aggiunti in un secondo momento. L'amministratore dovrà invece creare i collegamenti manualmente.  
  
#### <a name="automatically-created-gpos"></a>Creati automaticamente oggetti Criteri di gruppo  
Tenere presente quanto segue quando si usa automaticamente creati oggetti Criteri di gruppo:  
  
Creati automaticamente oggetti Criteri di gruppo vengono applicati in base alla posizione o la destinazione del collegamento, come indicato di seguito:  
  
-   Per l'oggetto Criteri di gruppo del server DirectAccess, il percorso e destinazione collegamento puntano al dominio che contiene il server di accesso remoto.  
  
-   Quando vengono creati oggetti Criteri di gruppo del server applicazioni e client, la posizione è impostata su un singolo dominio. Il nome oggetto Criteri di gruppo viene cercato in ogni dominio e il dominio viene riempito con impostazioni DirectAccess, se presente.  
  
-   La destinazione collegamento viene impostata sulla radice del dominio in cui è stato creato l'oggetto Criteri di gruppo. Viene creato un oggetto Criteri di gruppo per ogni dominio che contiene computer client o server applicazioni, che sarà quindi collegato alla radice del rispettivo dominio.  
  
Quando si usano automaticamente creati oggetti Criteri di gruppo per applicare le impostazioni di DirectAccess, l'amministratore del server Accesso remoto richiede le autorizzazioni seguenti:  
  
-   Autorizzazioni per creare oggetti Criteri di gruppo per ogni dominio.  
  
-   Autorizzazioni per collegare a tutte le radici del dominio client selezionate.  
  
-   Autorizzazioni per collegare le radici del dominio oggetto Criteri di gruppo.  
  
-   Autorizzazioni di sicurezza per creare, modificare, eliminare e modificare oggetti Criteri di gruppo.  
  
-   Oggetto Criteri di gruppo autorizzazioni di lettura per ogni dominio richiesto. Non è richiesta l'autorizzazione, ma è consigliabile perché consente l'accesso remoto verificare che non esistano oggetti Criteri di gruppo con nomi duplicati quando vengono creati oggetti Criteri di gruppo.  
  
#### <a name="manually-created-gpos"></a>Creazione manuale oggetti Criteri di gruppo  
Tenere presente quanto segue quando si usano gli oggetti Criteri di gruppo creati manualmente:  
  
-   Gli oggetti Criteri di gruppo devono essere presenti prima di eseguire la Configurazione guidata Accesso remoto.  
  
-   Per applicare le impostazioni di DirectAccess, l'amministratore del server Accesso remoto richiede autorizzazioni di sicurezza completo per creare, modificare, eliminare e modificare oggetti Criteri di gruppo creati manualmente.  
  
-   Viene eseguita una ricerca per un collegamento a oggetto Criteri di gruppo nell'intero dominio. Se l'oggetto Criteri di gruppo non è collegato nel dominio, verrà automaticamente creato un collegamento nel dominio radice. Se le autorizzazioni richieste per creare il collegamento non sono disponibili verrà emesso un avviso.  
  
#### <a name="recovering-from-a-deleted-gpo"></a>Ripristino da un oggetto Criteri di gruppo eliminato  
Se un oggetto Criteri di gruppo in un server Accesso remoto, client o server applicazioni è stato eliminato per errore, verrà visualizzato il messaggio di errore seguente: **Impossibile trovare l'oggetto Criteri di gruppo (nome oggetto Criteri di gruppo)** .  
  
Se è disponibile un backup sarà possibile ripristinare l'oggetto Criteri di gruppo. Se non è disponibile alcun backup, è necessario rimuovere le impostazioni di configurazione e configurarli nuovamente.  
  
###### <a name="to-remove-configuration-settings"></a>Per rimuovere le impostazioni di configurazione  
  
1.  Eseguire il cmdlet Windows PowerShell **Uninstall-RemoteAccess**.  
  
2.  Aprire **Gestione accesso remoto**.  
  
3.  Verrà visualizzato un messaggio di errore che informa che l'oggetto Criteri di gruppo non è stato trovato. Fare clic su **rimuovere impostazioni di configurazione**. Dopo il completamento, verrà ripristinato il server a uno stato non configurato, e sarà possibile riconfigurare le impostazioni.  
  


