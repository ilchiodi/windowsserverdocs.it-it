---
title: Passaggio 1 piano l'infrastruttura DirectAccess
description: Questo argomento fa parte della Guida di aggiungere DirectAccess a una distribuzione di accesso remoto esistente (VPN) per Windows Server 2016
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-da
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 4ca50ea8-6987-4081-acd5-5bf9ead62acd
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 0a761f644fbae489124392f195465bf2acf8e66f
ms.sourcegitcommit: d888e35f71801c1935620f38699dda11db7f7aad
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 06/07/2019
ms.locfileid: "66805095"
---
# <a name="step-1-plan-directaccess-infrastructure"></a>Passaggio 1 piano l'infrastruttura DirectAccess

>Si applica a: Windows Server (canale semestrale), Windows Server 2016

Il primo passaggio della pianificazione della distribuzione di base di Accesso remoto in un solo server consiste nel pianificare l'infrastruttura necessaria per la distribuzione. In questo argomento vengono descritti i passaggi per la pianificazione dell'infrastruttura:  
  
|Attività|Descrizione|  
|----|--------|  
|Pianificare la topologia di rete e le impostazioni|Decidere dove collocare il server di Accesso remoto (periferico o protetto da un dispositivo o un firewall Network Address Translation (NAT)) e pianificare gli indirizzi IP e il routing.|  
|Pianificare i requisiti del firewall|Pianificare il passaggio di Accesso remoto attraverso i firewall perimetrali.|  
|Pianificare i requisiti dei certificati|Accesso remoto può usare Kerberos o i certificati per l'autenticazione dei client. In questa distribuzione di base di Accesso remoto, Kerberos viene configurato automaticamente e l'autenticazione viene eseguita con un certificato autofirmato emesso automaticamente dal server di Accesso remoto.|  
|Pianificare i requisiti DNS|Pianificare le impostazioni DNS per il server di Accesso remoto, i server dell'infrastruttura, le opzioni di risoluzione dei nomi locali e la connettività client.|  
|Pianificare Active Directory|Pianificare i requisiti dei controller di dominio e di Active Directory.|  
|Pianificare gli oggetti Criteri di gruppo|Decidere quali oggetti Criteri di gruppo sono necessari nell'organizzazione e come creare o modificare gli oggetti Criteri di gruppo.|  
  
L'esecuzione di queste attività di pianificazione non richiede un ordine specifico.  
  
## <a name="plan-network-topology-and-settings"></a>Pianificare la topologia di rete e le impostazioni  
  
### <a name="plan-network-adapters-and-ip-addressing"></a>Pianificare le schede di rete e gli indirizzi IP  
  
1. Identificare la topologia delle schede di rete da usare. Accesso remoto può essere impostato con una delle seguenti topologie:  
  
    - Con due schede di rete: Sia nella periferia con una scheda di rete connessa a Internet e l'altra alla rete interna o dietro un NAT, firewall o un dispositivo di router, con una scheda di rete connesse a una rete perimetrale e l'altra alla rete interna.  
  
    - Dietro un dispositivo NAT con una scheda di rete: Il server di accesso remoto viene installato dietro un dispositivo NAT e l'unica scheda di rete è connessa alla rete interna.  
  
2. Identificare i requisiti degli indirizzi IP:  
  
    DirectAccess usa IPv6 con IPsec per creare una connessione sicura tra i computer client DirectAccess e la rete aziendale interna. Tuttavia, DirectAccess non richiede necessariamente la connettività a Internet IPv6 o il supporto IPv6 nativo nelle reti interne, ma configura e usa automaticamente tecnologie di transizione IPv6 per eseguire il tunneling del traffico IPv6 nella rete Internet IPv4 (6to4, Teredo, IP-HTTPS) e nella rete Intranet solo IPv4 (NAT64 o ISATAP). Per una panoramica di queste tecnologie di transizione, vedere le seguenti risorse:  
  
    - [Tecnologie di transizione IPv6](https://technet.microsoft.com/library/bb726951.aspx)  
  
    - [Specifica del protocollo di Tunneling IP-HTTPS](https://msdn.microsoft.com/library/dd358571.aspx)  
  
3. Configurare le schede e gli indirizzi necessari in base alla seguente tabella. Per le distribuzioni dietro un dispositivo NAT con una sola scheda di rete, configurare gli indirizzi IP usando solo la colonna 'scheda di rete interna'.  
  
    |IP address type|Scheda di rete esterna|Scheda di rete interna|Requisiti di routing|  
    |-|--------------|--------------------|------------|  
    |Intranet IPv4 e Internet IPv4|Configurare quanto segue:<br/><br/>-Un indirizzo IPv4 pubblico statico con la subnet mask appropriate.<br/>-Un gateway predefinito IPv4 indirizzo del firewall Internet o router dei provider di servizi Internet locale.|Configurare quanto segue:<br/><br/>-Un indirizzo intranet IPv4 con subnet mask appropriata.<br/>-Un suffisso DNS specifico della connessione dello spazio dei nomi intranet. È necessario configurare anche un server DNS nell'interfaccia interna.<br/>-Non si configura un gateway predefinito in alcuna interfaccia intranet.|Per configurare il server di Accesso remoto in modo da raggiungere tutte le subnet nella rete IPv4 interna, eseguire le operazioni seguenti:<br/><br/>1.  Elencare gli spazi degli indirizzi IPv4 per tutti i percorsi nella Intranet.<br/>2.  Usare il comando **route add -p** o **netsh interface ipv4 add route** per aggiungere gli spazi degli indirizzi IPv4 come route statiche nella tabella di routing IPv4 del server di Accesso remoto.|  
    |Internet IPv6 e Intranet IPv6|Configurare quanto segue:<br/><br/>-Usare la configurazione automatica degli indirizzi fornita dall'ISP.<br/>-Usare il **route print** comando per verificare l'esistenza di una route IPv6 predefinita che punta al router ISP nella tabella di routing IPv6.<br/>-Determinare se i router ISP e intranet utilizzano preferenze del router predefinite descritte in RFC 4191 e una preferenza predefinita superiore rispetto ai router intranet locali. Se entrambe queste condizioni sono vere, non sono richieste altre configurazioni per la route predefinita. La preferenza superiore per il router ISP garantisce che la route IPv6 predefinita attiva del server di Accesso remoto punta alla rete Internet IPv6.<br/><br/>Poiché il server di Accesso remoto è un router IPv6, se si ha un'infrastruttura IPv6 nativa, anche l'interfaccia Internet può raggiungere i controller di dominio sulla Intranet. In questo caso, aggiungere filtri pacchetti al controller di dominio nella rete perimetrale per impedire la connettività all'indirizzo IPv6 dell'interfaccia con connessione Internet del server di Accesso remoto.|Configurare quanto segue:<br/><br/>-Se non si usa livelli di preferenza predefiniti, configurare le interfacce intranet con il **netsh interface ipv6 set InterfaceIndex ignoredefaultroutes = abilitato** comando. Il comando verifica che route predefinite aggiuntive che puntano ai router della Intranet non verranno aggiunte alla tabella di routing IPv6. È possibile ottenere l'elemento InterfaceIndex delle interfacce Intranet dalla visualizzazione del comando netsh interface show interface.|Se si ha una Intranet IPv6, per configurare il server di Accesso remoto in modo da raggiungere tutti i percorsi IPv6, eseguire le operazioni seguenti:<br/><br/>1.  Elencare gli spazi degli indirizzi IPv6 per tutti i percorsi nella Intranet.<br/>2.  Usare il comando **netsh interface ipv6 add route** per aggiungere gli spazi degli indirizzi IPv6 come route statiche nella tabella di routing IPv6 del server di Accesso remoto.|  
    |Internet IPv6 e Intranet IPv4|Il server di Accesso remoto inoltra il traffico della route IPv6 predefinita usando l'interfaccia Scheda Microsoft 6to4 a un relay 6to4 nella rete Internet IPv4. È possibile configurare un server di Accesso remoto per l'indirizzo IPv4 del relay Microsoft 6to4 nella rete Internet IPv4 (usato quando l'indirizzo IPv6 nativo non viene distribuito nella rete aziendale) con il comando seguente: netsh interface ipv6 6to4 set relay name=192.88.99.1 state=enabled.|||  
  
    > [!NOTE]
    > 1. Se al client DirectAccess è stato assegnato un indirizzo IPv4 pubblico, per connettersi alla Intranet viene usata la tecnologia di transizione 6to4. Se il client DirectAccess non riesce a connettersi al server DirectAccess con 6to4, userà IP-HTTPS.  
    > 2. Il computer client IPv6 nativo può connettersi al server di Accesso remoto con un IPv6 nativo e non è richiesta alcuna tecnologia di transizione.  
  
### <a name="plan-firewall-requirements"></a>Pianificare i requisiti del firewall

Se il server di Accesso remoto è protetto da un firewall periferico, sono necessarie le seguenti eccezioni per il traffico di Accesso remoto quando il server di Accesso remoto si trova nella rete Internet IPv4:  
  
- 6to4 il traffico IP - protocollo 41 in entrata e in uscita.  
  
- IP-HTTPS-protocollo TCP (Transmission Control) destinazione porta 443 e porta TCP di origine 443 in uscita.  
  
- Se si distribuisce Accesso remoto con una sola scheda di rete e si installa il server dei percorsi di rete nel server di Accesso remoto, l'esenzione deve essere applicata anche alla porta TCP 62000.  
  
Le seguenti eccezioni sono necessarie per il traffico di Accesso remoto quando il server di Accesso remoto si trova nella rete Internet IPv6:  
  
- Protocollo IP 50  
  
- Porta UDP di destinazione 500 in entrata e porta UDP di origine 500 in uscita.  
  
Quando si utilizzano altri firewall, applicare le eccezioni firewall delle rete interna seguenti per il traffico di Accesso remoto:  
  
- ISATAP-protocollo 41 in entrata e in uscita  
  
- TCP/UDP per tutto il traffico IPv4/IPv6  
  
### <a name="plan-certificate-requirements"></a>Pianificare i requisiti dei certificati

I requisiti dei certificati per IPsec includono un certificato del computer usato dai computer client DirectAccess quando stabiliscono la connessione IPsec tra il client e il server di Accesso remoto e un certificato del computer usato dai server di Accesso remoto per stabilire le connessioni IPsec con i client DirectAccess. L'uso di questi certificati IPsec non è obbligatorio per DirectAccess in Windows Server 2012. L'Abilitazione guidata DirectAccess consente di configurare il server di Accesso remoto in modo da fungere da proxy Kerberos per eseguire l'autenticazione IPsec senza necessità di certificati.  
  
1. **Server IP-HTTPS**: Quando si configura accesso remoto, il server di accesso remoto viene configurato automaticamente per funzionare come listener web IP-HTTPS. Il sito IP-HTTPS richiede un certificato del sito Web e i computer client devono riuscire a contattare il sito dell'elenco di revoche di certificati (CRL) per il certificato. L'Abilitazione guidata DirectAccess prova a usare il certificato VPN SSTP. Se SSTP non è configurato, verifica la presenza di un certificato per IP-HTTPS nell'archivio personale sul computer. Se non sono disponibili certificati, viene creato automaticamente un certificato autofirmato.  
  
2. **Server dei percorsi di rete**: Il server dei percorsi di rete è un sito Web usato per rilevare se i computer client si trovano nella rete aziendale. Richiede un certificato del sito Web. I client DirectAccess devono poter contattare il sito CRL per il        certificato. L'Abilitazione guidata DirectAccess verifica la presenza di un certificato per il Server dei percorsi di rete nell'archivio personale sul computer. Se non presente, viene creato automaticamente un certificato autofirmato.  
  
I requisiti di certificazione per ogni scenario sono riepilogati nella seguente tabella:  
  
|Autenticazione IPsec|Server IP-HTTPS|Server dei percorsi di rete|  
|------------|----------|--------------|  
|Una CA interna è necessaria per emettere certificati del computer per il server di accesso remoto e i client per l'autenticazione IPsec quando non si usa il proxy Kerberos per l'autenticazione|CA pubblica: È consigliabile usare una CA pubblica per emettere il certificato IP-HTTPS, si garantisce che il punto di distribuzione CRL sia disponibile esternamente.|CA interna: È possibile usare una CA interna per emettere il certificato del sito Web del server dei percorsi di rete. Verificare che il punto di distribuzione CRL sia a disponibilità elevata nella rete interna.|  
||CA interna: È possibile usare una CA interna per emettere il certificato IP-HTTPS, però è necessario verificare che il punto di distribuzione CRL sia disponibile esternamente.|Certificato autofirmato: È possibile usare un certificato autofirmato per il sito Web server percorsi di rete; Tuttavia, è possibile utilizzare un certificato autofirmato in distribuzioni multisito.|  
||Certificato autofirmato: È possibile usare un certificato autofirmato per il server IP-HTTPS, però è necessario verificare che il punto di distribuzione CRL sia disponibile esternamente. Non è possibile usare un certificato autofirmato in una distribuzione multisito.||  
  
#### <a name="plan-certificates-for-ip-https"></a>Pianificare i certificati per IP-HTTPS

Il server di Accesso remoto funge da listener IP-HTTPS ed è necessario installare manualmente un certificato del sito Web HTTPS nel server. Durante la pianificazione tenere presente quanto segue:  
  
- L'uso di una CA pubblica è consigliato perché consente di avere subito disponibili gli elenchi di revoche di certificati (CRL).  
  
- Nel campo Soggetto, specificare l'indirizzo IPv4 della scheda Internet del server di Accesso remoto o il nome di dominio completo dell'URL IP-HTTPS (indirizzo ConnectTo). Se il server di Accesso remoto si trova dietro un dispositivo NAT, deve essere specificato il nome pubblico o l'indirizzo del dispositivo NAT.  
  
- Il nome comune del certificato deve corrispondere al nome del sito IP-HTTPS.  
  
- Per il campo Utilizzo chiavi avanzato usare l'identificatore di oggetto (OID) di autenticazione server.  
  
- Per il campo Punti di distribuzione Elenco di revoche di certificati (CRL), specificare un punto di distribuzione CRL accessibile dai client DirectAccess connessi a Internet.  
  
- Il certificato IP-HTTPS deve avere una chiave privata.  
  
- Il certificato IP-HTTPS deve essere importato direttamente nell'archivio personale.  
  
- Nei nomi dei certificati IP-HTTPS possono essere presenti caratteri jolly.  
  
#### <a name="plan-website-certificates-for-the-network-location-server"></a>Pianificare i certificati del sito Web per il server dei percorsi di rete

Quando si pianifica il sito Web del server dei percorsi di rete, tenere presente quanto segue:  
  
- Nel campo Soggetto, specificare l'indirizzo IP dell'interfaccia Intranet del server dei percorsi di rete o il nome di dominio completo dell'URL del percorso di rete.  
  
- Nel campo Utilizzo chiavi avanzato, usare l'OID di autenticazione server.  
  
- Per il campo Punti di distribuzione Elenco di revoche di certificati (CRL) un punto di distribuzione CRL che sia accessibile da parte dei client DirectAccess connessi alla Intranet. Questo punto di distribuzione CRL non deve essere accessibile fuori dalla rete interna.  
  
- Se si pianifica la successiva configurazione di una distribuzione multisito o cluster, il nome del certificato non deve corrispondere al nome interno del server di Accesso remoto.  

    > [!NOTE]  
    > Verificare che i certificati per IP-HTTPS e il server dei percorsi di rete abbiano un **Nome soggetto**. Se il certificato non ha un **Nome soggetto**, ma un **Nome alternativo**, non sarà accettato dalla Configurazione guidata Accesso remoto.  
  
#### <a name="plan-dns-requirements"></a>Pianificare i requisiti DNS

In una distribuzione di Accesso remoto, è richiesto il DNS per quanto segue:  
  
- **Richieste dei client DirectAccess**: DNS viene usato per risolvere le richieste provenienti dai computer client DirectAccess non presenti nella rete interna. I client DirectAccess provano a connettersi al server dei percorsi di rete DirectAccess per determinare se si trovano sulla rete Internet o su quella aziendale: Se la connessione riesce, viene rilevato che i client si trovano sulla rete Intranet, DirectAccess non viene usato e le richieste dei client vengono risolte usando il server DNS configurato nella scheda di rete del computer client. Se la connessione non riesce, si presuppone che i client si trovino sulla rete Internet. I client DirectAccess usano la tabella dei criteri di risoluzione dei nomi (NRPT) per determinare quale server DNS usare per la risoluzione delle richieste di nomi. È possibile specificare che i client debbano usare DNS64 DirectAccess per risolvere i nomi o un server DNS interno alternativo. Quando si esegue la risoluzione dei nomi, la tabella dei criteri di risoluzione dei nomi viene usata dai client DirectAccess per stabilire come gestire una richiesta. I client richiedono un nome di dominio completo o un nome con etichetta singola, ad esempio <https://internal>. Se è richiesto un nome con etichetta singola, viene aggiunto un suffisso DNS per trasformarlo in un nome di dominio completo. Se la query DNS corrisponde a una voce nella tabella dei criteri di risoluzione dei nomi ed è specificato DNS4 o un server DNS Intranet per la voce, la query viene inviata per la risoluzione dei nomi attraverso il server specificato. Se esiste una corrispondenza, ma non sono specificati server DNS, si è in presenza di una regola di esenzione e viene applicata la normale risoluzione dei nomi.  
  
    Quando viene aggiunto un nuovo suffisso alla tabella dei criteri di risoluzione dei nomi nella console di gestione Accesso remoto, i server DNS predefiniti per il suffisso possono essere individuati automaticamente facendo clic su **Rileva**. Il rilevamento automatico funziona in questo modo:  
  
    1. Se la rete aziendale è basata su IPv4 oppure IPv4 e IPv6, l'indirizzo predefinito è l'indirizzo DNS64 della scheda interna nel server di Accesso remoto.  
  
    2. Se la rete aziendale è basata su IPv6, l'indirizzo predefinito è l'indirizzo IPv6 dei server DNS nella rete aziendale.  
  
-  **Server di infrastruttura**  
  
    1. **Server dei percorsi di rete**: I client DirectAccess tentano di raggiungere il server dei percorsi di rete per determinare se si trovano sulla rete interna. I client nella rete interna devono poter risolvere il nome del server dei percorsi di rete, ma non quando si trovano sulla rete Internet. Per verificarlo, il nome di dominio completo del server dei percorsi di rete viene aggiunto per impostazione predefinita come regola di esenzione nella tabella dei criteri di risoluzione dei nomi. Quando si configura Accesso remoto vengono create automaticamente anche le seguenti regole:  
  
        1. Una regola per il suffisso DNS per il dominio radice oppure il nome dominio del server di Accesso remoto e gli indirizzi IPv6 corrispondenti ai server DNS Intranet configurati nel server di Accesso remoto. Ad esempio, se il server di Accesso remoto è membro del dominio corp.contoso.com, viene creata una regola per il suffisso DNS corp.contoso.com.  
  
        2. Una regola di esenzione per il nome di dominio completo del server del percorso di rete. Ad esempio, se l'URL di server percorsi di rete è <https://nls.corp.contoso.com>, viene creata una regola di esenzione per il nome di dominio completo nls.corp.contoso.com.  
  
        **Server IP-HTTPS**: Il server di accesso remoto funge da listener IP-HTTPS e Usa il certificato del server per autenticarsi nei client IP-HTTPS. Il nome IP-HTTPS deve essere risolvibile dai client DirectAccess che usano i server DNS pubblici.  
  
        **Strumenti di verifica della connettività**: Accesso remoto crea un probe web predefinito usato dai client DirectAccess i computer per verificare la connettività alla rete interna. Per verificare il corretto funzionamento del probe, è necessario registrare manualmente i seguenti nomi in DNS:  
  
        1.  DirectAccess-webprobehost deve risolvere l'indirizzo IPv4 interno del server di accesso remoto o l'indirizzo IPv6 in un ambiente solo IPv6.  
  
        2.  DirectAccess-corpconnectivityhost deve risolvere indirizzo localhost (loopback). Devono essere creati i record A e AAAA, il primo col valore 127.0.0.1 e il secondo con il valore costruito a partire dal prefisso NAT64 con 127.0.0.1 come ultimi 32 bit. Il prefisso NAT64 può essere recuperato eseguendo il cmdlet get-netnattransitionconfiguration.  
  
            > [!NOTE]  
            > Questa procedura è valida esclusivamente in un ambiente solo IPv4. In un ambiente IPv4 più IPv6 o solo IPv6, deve essere creato un solo record AAAA con l'indirizzo IP di loopback ::1.  
  
        È possibile creare altri strumenti di verifica della connettività con altri indirizzi Web su HTTP oppure PING. Per ogni strumento di verifica della connettività deve esistere una voce DNS.  
  
#### <a name="dns-server-requirements"></a>Requisiti del server DNS  
  
- Per i client DirectAccess, è necessario utilizzare un server DNS che esegue Windows Server 2003, Windows Server 2008, Windows Server 2008 R2, Windows Server 2012 o qualsiasi server DNS che supporti IPv6.  
  
### <a name="plan-active-directory"></a>Pianificare Active Directory

Accesso remoto Usa Active Directory e gli oggetti Criteri di gruppo Active Directory come segue:  
  
- **Autenticazione**: Active Directory viene usato per l'autenticazione. Il tunnel Intranet usa l'autenticazione Kerberos in modo che l'utente possa accedere alle risorse interne.  
  
- **Oggetti Criteri di gruppo**: Accesso remoto riunisce le impostazioni di configurazione in oggetti Criteri di gruppo applicati ai server di accesso remoto, i client e server applicazioni interni.  
  
- **Gruppi di sicurezza**: Accesso remoto Usa i gruppi di sicurezza per raccogliere e identificare i computer client DirectAccess e server di accesso remoto. I criteri di gruppo vengono applicati al gruppo di sicurezza richiesto.  
  
- **I criteri IPsec estesi**: Accesso remoto può usare l'autenticazione IPsec e la crittografia tra i client e server di accesso remoto. È possibile estendere l'autenticazione IPSec e la crittografia attraverso server applicazioni interni specificati.   
  
#### <a name="active-directory-requirements"></a>Requisiti per Active Directory  
  
Durante la pianificazione di Active Directory per una distribuzione di Accesso remoto, è richiesto quanto segue:  
  
- Almeno un controller di dominio installato nei sistemi operativi Windows Server 2012, Windows Server 2008 R2 Windows Server 2008 o Windows Server 2003.  
  
    Se il controller di dominio si trova su una rete perimetrale (ed è quindi raggiungibile dalla scheda di rete con connessione Internet del server di Accesso remoto), impedire al server di Accesso remoto di raggiungerlo aggiungendo filtri pacchetti sul controller di dominio, in modo da ostacolare la connettività all'indirizzo IP della scheda di rete Internet.  
  
- Il server di Accesso remoto deve essere membro di un dominio.  
  
- I client DirectAccess devono essere membri di un dominio. I client possono appartenere a:  
  
    - Qualsiasi dominio nella stessa foresta del server di Accesso remoto.  
  
    - Qualsiasi dominio che abbia un trust bidirezionale con il dominio del server di Accesso remoto.  
  
    - Qualsiasi dominio in una foresta che abbia un trust bidirezionale con la foresta alla quale appartiene il dominio di Accesso remoto.  
  
> [!NOTE]  
> - Il server di Accesso remoto non può essere un controller di dominio.  
> - Il controller di dominio di Active Directory usato per Accesso remoto non deve essere raggiungibile dalla scheda di rete Internet esterna del server di Accesso remoto (la scheda di rete non deve trovarsi nel profilo del dominio di Windows Firewall).  
  
### <a name="plan-group-policy-objects"></a>Pianificare gli oggetti Criteri di gruppo

Le impostazioni di DirectAccess configurate quando si configura Accesso remoto vengono raccolte in oggetti Criteri di gruppo. Tre diversi oggetti Criteri di gruppo vengono popolati con impostazioni DirectAccess e distribuiti come segue:  
  
- **GRUPPO di client DirectAccess**: Questo oggetto Criteri di gruppo contiene le impostazioni del client, incluse le impostazioni per la tecnologia di transizione IPv6, le voci NRPT e le regole di sicurezza della connessione di Windows Firewall con sicurezza avanzata. L'oggetto Criteri di gruppo viene applicato ai gruppi di sicurezza specificati per i computer client.  
  
- **DirectAccess server oggetto Criteri di gruppo**: Questo oggetto Criteri di gruppo contiene le impostazioni di configurazione DirectAccess applicate a qualsiasi server configurato come server di accesso remoto nella distribuzione. Contiene anche le regole di sicurezza della connessione di Windows Firewall con sicurezza avanzata.  
  
- **Application Server oggetto Criteri di gruppo**: Questo oggetto Criteri di gruppo contiene impostazioni per server applicazioni selezionati ai quali si estendono facoltativamente l'autenticazione e la crittografia dai client DirectAccess. Se l'autenticazione e la crittografia non vengono estese, questo oggetto Criteri di gruppo non viene usato.  
  
Gli oggetti Criteri di gruppo vengono creati automaticamente con l'Abilitazione guidata DirectAccess e viene specificato un nome predefinito per ogni oggetto Criteri di gruppo.  
  
> [!CAUTION]  
> Usare la procedura seguente per eseguire il backup di tutti gli oggetti Criteri di gruppo remoti prima di eseguire i cmdlet di DirectAccess: [Eseguire il backup e ripristino della configurazione di accesso remoto](https://go.microsoft.com/fwlink/?LinkID=257928)  
  
È possibile configurare gli oggetti Criteri di gruppo in due modi:  
  
1. **Automaticamente**: Per specificare la creazione automatica. Per ogni oggetto Criteri di gruppo viene specificato un nome predefinito.  
  
2. **Manualmente**: È possibile usare gli oggetti Criteri di gruppo predefiniti dall'amministratore di Active Directory.  
  
Dopo aver configurato DirectAccess in modo da usare specifici oggetti Criteri di gruppo non sarà possibile configurarlo in modo da usarne altri.  
  
#### <a name="automatically-created-gpos"></a>Oggetti Criteri di gruppo creati automaticamente

Tenere presente quanto segue quando si usano oggetti Criteri di gruppo creati automaticamente:  
  
Gli oggetti Criteri di gruppo creati automaticamente vengono applicati secondo i parametri di percorso e destinazione collegamento, come segue:  
  
- Per l'oggetto Criteri di gruppo del server DirectAccess, entrambi i parametri di percorso e collegamento puntano al dominio contenente il server di Accesso remoto.  
  
- Quando si creano oggetti Criteri di gruppo del client, il percorso viene impostato su un unico dominio in cui verrà creato l'oggetto Criteri di gruppo. Il nome dell'oggetto Criteri di gruppo viene cercato in tutti i domini e, se esistente, viene compilato con le impostazioni DirectAccess. La destinazione collegamento viene impostata sulla radice del dominio in cui è stato creato l'oggetto Criteri di gruppo. Viene creato un oggetto Criteri di gruppo per ogni dominio che contiene computer client o server applicazioni, che sarà quindi collegato alla radice del rispettivo dominio.  
  
Quando si usano oggetti Criteri di gruppo creati automaticamente, per applicare le impostazioni di DirectAccess l'amministratore del server di Accesso remoto saranno necessarie le autorizzazioni seguenti:  
  
- Autorizzazioni di creazione oggetto Criteri di gruppo per ogni dominio.  
  
- Autorizzazioni per il collegamento di tutte le radici del dominio client selezionate.  
  
- Autorizzazioni per il collegamento delle radici del dominio oggetto Criteri di gruppo del server.  
  
- Le autorizzazioni di sicurezza di creazione, modifica ed eliminazione sono necessarie per gli oggetti Criteri di gruppo.  
  
- È consigliabile che l'amministratore di Accesso remoto disponga delle autorizzazioni di lettura dell'oggetto Criteri di gruppo per ogni dominio richiesto. Ciò consente ad Accesso remoto di verificare che non esistano oggetti Criteri di gruppo con nomi duplicati durante la creazione degli oggetti Criteri di gruppo.  
  
Se non esistono le autorizzazioni corrette per il collegamento degli oggetti Criteri di gruppo, viene emesso un avviso. L'operazione di Accesso remoto continua, ma il collegamento non verrà eseguito. Se viene emesso questo avviso non verranno creati automaticamente i collegamenti, nemmeno dopo che saranno state concesse le autorizzazioni. L'amministratore dovrà invece creare i collegamenti manualmente.  
  
#### <a name="manually-created-gpos"></a>Oggetti Criteri di gruppo creati manualmente

Tenere presente quanto segue quando si usano oggetti Criteri di gruppo creati manualmente:  
  
- Gli oggetti Criteri di gruppo devono essere presenti prima di eseguire la Configurazione guidata Accesso remoto.  
  
- Per applicare le impostazioni DirectAccess, all'amministratore di Accesso remoto sono necessarie le autorizzazioni complete per gli oggetti Criteri di gruppo creati manualmente (autorizzazioni di sicurezza di modifica ed eliminazione).  
  
- Quando si usano oggetti Criteri di gruppo creati manualmente viene eseguita una ricerca di un collegamento all'oggetto Criteri di gruppo nell'intero dominio. Se l'oggetto Criteri di gruppo non è collegato nel dominio, verrà automaticamente creato un collegamento nel dominio radice. Se le autorizzazioni richieste per creare il collegamento non sono disponibili, verrà emesso un avviso.  
  
Se non esistono le autorizzazioni corrette per il collegamento degli oggetti Criteri di gruppo, viene emesso un avviso. L'operazione di Accesso remoto continua, ma il collegamento non verrà eseguito. Se viene emesso questo avviso non verranno creati automaticamente i collegamenti, nemmeno dopo che saranno state aggiunte le autorizzazioni. L'amministratore dovrà invece creare i collegamenti manualmente.  
  
#### <a name="recovering-from-a-deleted-gpo"></a>Ripristino da un oggetto Criteri di gruppo eliminato

Se un server di Accesso remoto, un client o un oggetto Criteri di gruppo del server applicazioni è stato eliminato per errore e non è disponibile alcun backup, sarà necessario rimuovere le impostazioni di configurazione e riconfigurare. Se è disponibile un backup sarà possibile ripristinare l'oggetto Criteri di gruppo.  
  
**Gestione Accesso remoto** visualizza il messaggio di errore seguente: **Impossibile trovare l'oggetto Criteri di gruppo (nome oggetto Criteri di gruppo)** . Per rimuovere le impostazioni di configurazione eseguire la procedura seguente:  
  
1. Eseguire il cmdlet **Uninstall-remoteaccess** di PowerShell.  
  
2. Riaprire **Gestione accesso remoto**.  
  
3. Verrà visualizzato un messaggio di errore che informa che l'oggetto Criteri di gruppo non è stato trovato. Fare clic su **rimuovere impostazioni di configurazione**. Al termine, verrà ripristinato lo stato non configurato del server.  

