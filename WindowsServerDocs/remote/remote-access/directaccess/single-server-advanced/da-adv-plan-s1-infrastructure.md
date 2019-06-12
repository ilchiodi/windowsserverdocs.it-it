---
title: Passaggio 1 piano, l'infrastruttura DirectAccess avanzato
description: Questo argomento fa parte della Guida di distribuire un DirectAccess Server singolo con Advanced le impostazioni per Windows Server 2016
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology:
- networking-da
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: aa3174f3-42af-4511-ac2d-d8968b66da87
ms.author: pashort
author: shortpatti
ms.openlocfilehash: c3b7751ca6d0b62ee078d5da7084cbc007edc155
ms.sourcegitcommit: d888e35f71801c1935620f38699dda11db7f7aad
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 06/07/2019
ms.locfileid: "66805174"
---
# <a name="step-1-plan-the-advanced-directaccess-infrastructure"></a>Passaggio 1 piano, l'infrastruttura DirectAccess avanzato

>Si applica a: Windows Server (canale semestrale), Windows Server 2016

Il primo passaggio della pianificazione della distribuzione avanzata DirectAccess in un solo server consiste nel pianificare l'infrastruttura necessaria per la distribuzione. In questo argomento vengono descritti i passaggi per la pianificazione dell'infrastruttura. Il completamento di queste attività di pianificazione non richiede un ordine specifico.  
  
|Attività|Descrizione|
|----|--------|  
|[1.1 pianificare la topologia di rete e impostazioni](#11-plan-network-topology-and-settings)|Decidere dove collocare il server DirectAccess (alla periferia o dietro un dispositivo Network Address Translation (NAT) o un firewall) e pianificare gli indirizzi IP, il routing e il tunneling forzato.|  
|[1.2 pianificare i requisiti del firewall](#12-plan-firewall-requirements)|Pianificare il passaggio del traffico DirectAccess attraverso i firewall periferici.|  
|[1.3 pianificare i requisiti dei certificati](#13-plan-certificate-requirements)|Decidere se usare Kerberos o i certificati per l'autenticazione client e pianificare i certificati del sito Web. IP-HTTPS è un protocollo di transizione usato dai client DirectAccess per eseguire il tunneling del traffico IPv6 in reti IPv4. Decidere se autenticare al server IP-HTTPS con un certificato emesso da un'autorità di certificazione (CA) o con un certificato autofirmato emesso automaticamente dal server DirectAccess.|  
|[1.4 pianificare i requisiti DNS](#14-plan-dns-requirements)|Pianificare le impostazioni Domain Name System (DNS) per il server DirectAccess, i server dell'infrastruttura, le opzioni di risoluzione dei nomi locali e la connettività client.|  
|[1.5 pianificare il server dei percorsi di rete](#15-plan-the-network-location-server)|Il server dei percorsi di rete viene usato dai client DirectAccess per determinare se si trovano sulla rete interna. Decidere dove collocare il sito Web del server dei percorsi di rete nell'organizzazione (nel server DirectAccess o in un server alternativo) e pianificare i requisiti dei certificati se il server dei percorsi di rete si trova nel server DirectAccess.|  
|[1.6 pianificare i server di gestione](#16-plan-management-servers)|È possibile gestire in remoto i computer client DirectAccess che si trovano all'esterno della rete aziendale su Internet. Pianificare i server di gestione, ad esempio i server di aggiornamento, utilizzati durante la gestione remota dei client.|  
|[1.7 pianificare Active Directory Domain Services](#17-plan-active-directory-domain-services)|Pianificare i controller di dominio, i requisiti di Active Directory, l'autenticazione client e più di un dominio.|  
|[1.8 oggetti Criteri di gruppo pianificare](#18-plan-group-policy-objects)|Decidere quali oggetti Criteri di gruppo sono necessari nell'organizzazione e come creare o modificare gli oggetti Criteri di gruppo.|  
  
## <a name="11-plan-network-topology-and-settings"></a>1.1 Pianificare la topologia di rete e le impostazioni

In questa sezione viene spiegato come pianificare la rete, incluse le seguenti operazioni:  
  
- [1.1.1 pianificare le schede di rete e gli indirizzi IP](#111-plan-network-adapters-and-ip-addressing)  
  
- [1.1.2 pianificare la connettività intranet IPv6](#112-plan-ipv6-intranet-connectivity)  
  
- [1.1.3 pianificare il tunneling forzato](#113-plan-for-force-tunneling)  
  
### <a name="111-plan-network-adapters-and-ip-addressing"></a>1.1.1 Pianificare le schede di rete e gli indirizzi IP  
  
1. Identificare la topologia delle schede di rete da usare. DirectAccess può essere impostato con una delle seguenti topologie:  
  
    - **Due schede di rete**. Il server DirectAccess può essere installato nella periferia con una scheda di rete connessa a Internet e l'altra alla rete interna oppure può essere installato dietro un dispositivo NAT, firewall o router, con una scheda di rete connessa alla rete perimetrale e l'altra alla rete interna.  
  
    - **Una scheda di rete**. Il server DirectAccess viene installato dietro un dispositivo NAT e l'unica scheda di rete viene connessa alla rete interna.  
  
2. Identificare i requisiti degli indirizzi IP:  
  
    DirectAccess usa IPv6 con IPsec per creare una connessione sicura tra i computer client DirectAccess e la rete aziendale interna. Tuttavia, DirectAccess non richiede necessariamente la connettività a Internet IPv6 o il supporto IPv6 nativo nelle reti interne, ma configura e usa automaticamente tecnologie di transizione IPv6 per eseguire il tunneling del traffico IPv6 nella rete Internet IPv4 (con 6to4, Teredo o IP-HTTPS) e nella rete Intranet solo IPv4 (con NAT64 o ISATAP). Per una panoramica di queste tecnologie di transizione, vedere le seguenti risorse:  
  
    - [Tecnologie di transizione IPv6](https://technet.microsoft.com/library/bb726951.aspx)  
  
    - [Specifica del protocollo di Tunneling IP-HTTPS](https://msdn.microsoft.com/library/dd358571(PROT.10).aspx)  
  
3. Configurare le schede e gli indirizzi necessari in base alla seguente tabella. Per le distribuzioni che utilizzano una sola scheda di rete e sono impostate dietro un dispositivo NAT, configurare gli indirizzi IP utilizzando solo il **scheda di rete interna** colonna.  
  
    ||Scheda di rete esterna|Scheda di rete interna|Requisiti di routing|  
    |-|--------------|--------------|------------|  
    |Internet IPv4 e Intranet IPv4|Configurare due indirizzi IPv4 pubblici statici consecutivi con le subnet mask appropriate (richiesto solo per Teredo).<br/><br/>Configurare anche l'indirizzo IPv4 del gateway predefinito del firewall Internet o del router del provider di servizi Internet (ISP) locale. **Nota:** Il server DirectAccess necessita di due indirizzi IPv4 pubblici consecutivi per poter essere usato come server Teredo e i client basati su Windows possono usare il server DirectAccess per rilevare il tipo di dispositivo NAT da cui sono protetti.|Configurare quanto segue:<br/><br/>-Un indirizzo intranet IPv4 con subnet mask appropriata.<br/>-Il suffisso DNS specifico della connessione dello spazio dei nomi intranet. Configurare anche un server DNS nell'interfaccia interna. **Attenzione:** Non configurare un gateway predefinito in alcuna interfaccia Intranet.|Per configurare il server DirectAccess affinché possa raggiungere tutte le subnet nella rete IPv4 interna, eseguire le operazioni seguenti:<br/><br/>: Elenca gli spazi di indirizzi IPv4 per tutti i percorsi nella rete intranet.<br/>-Utilizzare il **route aggiungere -p** o**netsh interface ipv4 aggiungere route** comando per aggiungere gli spazi degli indirizzi IPv4 come route statiche nella tabella di routing IPv4 del server DirectAccess.|  
    |Internet IPv6 e Intranet IPv6|Configurare quanto segue:<br/><br/>-Utilizzare la configurazione degli indirizzi fornita dall'ISP.<br/>-Utilizzare il **Route Print** comando per verificare se esiste una route IPv6 predefinita che punta al router ISP nella tabella di routing IPv6.<br/>-Determinare se i router ISP e intranet stanno usando le preferenze del router predefinite descritte in RFC 4191 e una preferenza predefinita superiore rispetto ai router intranet locali.<br/>    Se entrambe queste condizioni sono vere, non sono richieste altre configurazioni per la route predefinita. La preferenza superiore per il router ISP garantisce che la route IPv6 predefinita attiva del server DirectAccess punta alla rete Internet IPv6.<br/><br/>Poiché il server DirectAccess è un router IPv6, se si ha un'infrastruttura IPv6 nativa, anche l'interfaccia Internet può raggiungere i controller di dominio sulla Intranet. In questo caso, aggiungere filtri pacchetti al controller di dominio nella rete perimetrale per impedire la connettività all'indirizzo IPv6 dell'interfaccia con connessione Internet del server DirectAccess.|Configurare quanto segue:<br/><br/>-Se non si utilizza livelli di preferenza predefiniti, è possibile configurare le interfacce intranet utilizzando il comando seguente**netsh interface ipv6 set InterfaceIndex ignoredefaultroutes = abilitato**.<br/>    Il comando verifica che route predefinite aggiuntive che puntano ai router della Intranet non verranno aggiunte alla tabella di routing IPv6. È possibile ottenere l'indice dell'interfaccia delle interfacce intranet utilizzando il comando seguente: **netsh interface ipv6 show interface**.|Se si ha una Intranet IPv6, per configurare il server DirectAccess affinché possa raggiungere tutti i percorsi IPv6, eseguire le operazioni seguenti:<br/><br/>: Elenca gli spazi degli indirizzi IPv6 per tutti i percorsi nella rete intranet.<br/>-Utilizzare il **netsh interface ipv6 aggiungere route** comando per aggiungere gli spazi degli indirizzi IPv6 come route statiche nella tabella di routing IPv6 del server DirectAccess.|  
    |Internet IPv4 e Intranet IPv6|Il server DirectAccess inoltra il traffico della route IPv6 predefinita usando la scheda Microsoft 6to4 a un relay 6to4 nella rete Internet IPv4. È possibile configurare un server DirectAccess per l'indirizzo IPv4 della scheda Microsoft 6to4 usando il comando seguente: `netsh interface ipv6 6to4 set relay name=<ipaddress> state=enabled`.|||  
  
    > [!NOTE]  
    > - Se al client DirectAccess è stato assegnato un indirizzo IPv4 pubblico, per connettersi alla Intranet viene usata la tecnologia di transizione 6to4. Se è stato assegnato un indirizzo IPv4 privato, userà Teredo. Se il client DirectAccess non riesce a connettersi al server DirectAccess con 6to4 o Teredo, userà IP-HTTPS.  
    > - Per usare Teredo, è necessario configurare due indirizzi IP consecutivi nella scheda di rete con connessione esterna.  
    > - Se il server DirectAccess ha una sola scheda di rete, Teredo non può essere usato.  
    > - I computer client IPv6 nativi possono connettersi al server DirectAccess con un IPv6 nativo e non è richiesta alcuna tecnologia di transizione.  
  
### <a name="112-plan-ipv6-intranet-connectivity"></a>1.1.2 Pianificare la connettività Intranet IPv6

Per gestire i client DirectAccess remoti è necessario IPv6. IPv6 consente ai server di gestione DirectAccess di connettersi ai client DirectAccess che si trovano su Internet per gestirli in remoto.  
  
> [!NOTE]  
> - Non è necessario usare IPv6 nella rete per supportare le connessioni avviate dai computer client DirectAccess alle risorse IPv4 nella rete dell'organizzazione. Per questo scopo, viene usato NAT64/DNS64.  
> - Se non si gestiscono client DirectAccess remoti, non è necessario distribuire IPv6.  
> - Il protocollo ISATAP (Intra-Site Automatic Tunnel Addressing Protocol) non è supportato nelle distribuzioni DirectAccess.  
> - Quando si usa IPv6, è possibile abilitare le query dei record di risorse dell'host IPv6 (AAAA) per DNS64 con il seguente comando di Windows PowerShell:   **Set-NetDnsTransitionConfiguration -OnlySendAQuery $false**.  
  
### <a name="113-plan-for-force-tunneling"></a>1.1.3 Pianificare il tunneling forzato

Per impostazione predefinita, con IPv6 e la tabella dei criteri di risoluzione dei nomi (NRPT), i client DirectAccess separano il traffico Intranet e Internet come segue:  
  
- Lo scambio delle query relative ai nomi DNS per i nomi di dominio completi (FQDN) e di tutto il traffico Intranet avviene attraverso i tunnel creati con il server DirectAccess o direttamente con i server della Intranet. Il traffico Intranet dai client DirectAccess è traffico di tipo IPv6.  
  
- Lo scambio delle query relative ai nomi DNS per i nomi di dominio completi (FQDN) che corrispondono alle regole di esenzione o che non corrispondono allo spazio dei nomi Intranet e di tutto il traffico ai server Internet avviene attraverso l'interfaccia fisica connessa a Internet. Il traffico Internet dai client DirectAccess è generalmente traffico di tipo IPv4.  
  
Al contrario, per impostazione predefinita, alcune implementazioni di rete privata virtuale (VPN) di accesso remoto, incluso il client VPN, inviano tutto il traffico Intranet e Internet sulla connessione VPN di accesso remoto. Il traffico associato a Internet viene indirizzato dal server VPN ai server proxy Web IPv4 della Intranet per l'accesso alle risorse della rete Internet IPv4. È possibile separare il traffico Intranet e Internet per i client VPN di accesso remoto con lo split tunneling che prevede la configurazione della tabella di routing del protocollo IP nei client VPN in modo che il traffico diretto ai percorsi Intranet venga inviato con la connessione VPN, mentre quello diretto a tutti gli altri percorsi venga inviato con l'interfaccia fisica connessa a Internet.  
  
È possibile configurare i client DirectAccess per l'invio di tutto il loro traffico i tunnel al server DirectAccess mediante il tunneling forzato. Quando è configurato il tunneling forzato, i client DirectAccess rilevano di essere connessi a Internet e rimuovono la route predefinita IPv4. Fatta eccezione per traffico della subnet locale, tutto il traffico inviato dal client DirectAccess è traffico di tipo IPv6 inviato al server DirectAccess attraverso i tunnel.  
  
> [!IMPORTANT]  
> Se si pianifica l'uso del tunneling forzato o se si pensa di aggiungerlo in futuro, distribuire una configurazione a due tunnel. Per motivi di sicurezza, il tunneling forzato in una configurazione a un solo tunnel non è supportato.  
  
L'abilitazione del tunneling forzato presente la seguenti conseguenze:  
  
- I client DirectAccess utilizzano solo IP-HTTPS (Internet Protocol over Secure Hypertext Transfer Protocol) per ottenere la connettività IPv6 al server DirectAccess tramite Internet IPv4.  
  
- I soli percorsi che un client DirectAccess può raggiungere per impostazione predefinita con traffico IPv4 sono quelli della subnet locale. Il traffico rimanente inviato dalle applicazioni e dai servizi in esecuzione nel client DirectAccess è di tipo IPv6, inviato con la connessione DirectAccess. Di conseguenza, le applicazioni solo IPv4 sul client DirectAccess non possono essere utilizzate per raggiungere risorse Internet, salvo quelle che si trovano sulla subnet locale.  
  
> [!IMPORTANT]  
> Per il tunneling forzato attraverso DNS64 e NAT64 è necessario implementare la connettività Internet IPv6. Un modo per farlo consiste nel rendere indirizzabile il prefisso IP-HTTPS a livello globale in modo che ipv6.msftncsi.com risulti raggiungibile con IPv6 e la risposta dal server Internet ai client IP-HTTPS possa essere restituita attraverso il server DirectAccess.  
>
> Poiché questa operazione non è consentita in molti casi, si consiglia di creare server NCSI virtuali all'interno della rete aziendale, come descritto di seguito:  
>
> 1. Aggiungere una voce NRPT per ipv6.msftncsi.com e risolverla con DNS64 in un sito Web interno (ad esempio, un sito Web IPv4).  
> 2. Aggiungere una voce NRPT per dns.msftncsi.com e risolverla con un server DNS aziendale per restituire il record di risorse dell'host IPv6 (AAAA) fd3e:4f5a:5b81::1. (L'uso di DNS64 per il solo invio di query dei record di risorse dell'host (A) per questo nome di dominio completo potrebbe non funzionare perché è configurato nelle distribuzioni solo IPv4, quindi è necessario configurarlo per la risoluzione diretta con il DNS aziendale).  
  
## <a name="12-plan-firewall-requirements"></a>1.2 Pianificare i requisiti firewall

Se il server DirectAccess è protetto da un firewall periferico, sono necessarie le seguenti eccezioni per il traffico di Accesso remoto quando il server DirectAccess si trova nella rete Internet IPv4:  
  
- Porta di destinazione Datagram Protocol (UDP) 3544 utente di traffico Teredo in entrata e porta UDP di origine 3544 in uscita.  
  
- Protocollo IP traffico 6to4 41 in entrata e in uscita.  
  
- Porta di destinazione IP-HTTPS-protocollo TCP (Transmission Control) 443 e porta TCP di origine 443 in uscita.  
  
- Se si distribuisce Accesso remoto con una sola scheda di rete e si installa il server dei percorsi di rete nel server DirectAccess, l'esenzione deve essere applicata anche alla porta TCP 62000.  
  
    > [!NOTE]  
    > Questa esenzione si trova nel server DirectAccess, mentre le altre nel firewall periferico.  
  
Per il traffico Teredo e 6to4, queste eccezioni vanno applicate a entrambi gli indirizzi Ipv4 pubblici consecutivi con connessione Internet nel server DirectAccess. Per IP-HTTPS, le eccezioni devono essere applicate all'indirizzo registrato nel server DNS pubblico.  
  
Le seguenti eccezioni sono necessarie per il traffico di Accesso remoto quando il server DirectAccess si trova nella rete Internet IPv6:  
  
- ID protocollo IP 50  
  
- Porta UDP di destinazione 500 in entrata e porta UDP di origine 500 in uscita  
  
- Traffico ICMPv6 in entrata e in uscita (solo quando si usa Teredo)  
  
Quando si usano altri firewall, applicare le seguenti eccezioni firewall della rete interna per il traffico di Accesso remoto:  
  
- ISATAP-protocollo 41 in entrata e in uscita  
  
- TCP/UDP per tutto il traffico IPv4 e IPv6  
  
- ICMP per tutto il traffico IPv4 e IPv6 (solo quando si usa Teredo)  
  
## <a name="13-plan-certificate-requirements"></a>1.3 Pianificare i requisiti dei certificati

Quando si distribuisce un server DirectAccess singolo, i certificati sono richiesti in tre scenari:  
  
- [1.3.1 pianificare i certificati del computer per l'autenticazione IPsec](#131-plan-computer-certificates-for-ipsec-authentication)  
  
    I requisiti dei certificati per IPsec includono un certificato del computer usato dai computer client DirectAccess quando stabiliscono la connessione IPsec tra il client e il server DirectAccess e un certificato del computer usato dal server DirectAccess per stabilire le connessioni IPsec con i client DirectAccess.  
  
    L'utilizzo di questi certificati IPsec non è obbligatorio per DirectAccess in Windows Server 2012. In alternativa, il server DirectAccess può fungere da proxy Kerberos per eseguire l'autenticazione IPsec senza necessità di certificati. Se viene usato, il protocollo Kerberos viene eseguito su SSL e il proxy Kerberos usa il certificato configurato per IP-HTTPS a questo scopo. In alcuni scenari di grandi imprese (incluse la distribuzione multisito e l'autenticazione client con One-Time Password, OTP) è richiesta l'autenticazione dei certificati e non il protocollo Kerberos.  
  
-   [1.3.2 pianificare i certificati per IP-HTTPS](#132-plan-certificates-for-ip-https)  
  
    Quando si configura Accesso remoto, il server DirectAccess viene configurato automaticamente per funzionare come listener IP-HTTPS. Il sito IP-HTTPS richiede un certificato del sito Web e i computer client devono riuscire a contattare il sito dell'elenco di revoche di certificati (CRL) per il certificato.  
  
-   [1.3.3 pianificare i certificati del sito Web per il server dei percorsi di rete](#133-plan-website-certificates-for-the-network-location-server)  
  
    Il server dei percorsi rete è un sito Web usato per rilevare se i computer client si trovano nella rete aziendale. Il server dei percorsi di rete richiede un certificato di sito Web. I client DirectAccess devono poter contattare il sito CRL per il certificato.  
  
I requisiti dell'autorità di certificazione (CA) per ogni scenario sono riepilogati nella seguente tabella.  
  
|Autenticazione IPsec|Server IP-HTTPS|Server dei percorsi di rete|  
|------------|----------|--------------|  
|Una CA interna è necessaria per emettere certificati del computer per il server DirectAccess e i client per l'autenticazione IPsec quando non si utilizza il proxy Kerberos per l'autenticazione|CA interna:<br/><br/>È possibile usare una CA interna per emettere il certificato IP-HTTPS, però è necessario verificare che il punto di distribuzione CRL sia disponibile esternamente.|CA interna:<br/><br/>È possibile usare una CA interna per emettere il certificato del sito Web del server dei percorsi di rete. Verificare che il punto di distribuzione CRL disponga di elevata disponibilità nella rete interna.|  
||Certificato autofirmato:<br/><br/>È possibile usare un certificato autofirmato per il server IP-HTTPS, però è necessario verificare che il punto di distribuzione CRL sia disponibile esternamente.<br/><br/>Un certificato autofirmato non può essere usato in distribuzioni multisito.|Certificato autofirmato:<br/><br/>È possibile usare un certificato autofirmato per il sito Web del server dei percorsi di rete.<br/><br/>Un certificato autofirmato non può essere usato in distribuzioni multisito.|  
||**Consigliato**<br/><br/>CA pubblica:<br/><br/>Si consiglia di usare una CA pubblica per emettere il certificato IP-HTTPS perché assicura che il punto di distribuzione CRL sia disponibile esternamente.|  
  
### <a name="131-plan-computer-certificates-for-ipsec-authentication"></a>1.3.1 Pianificare i certificati del computer per l'autenticazione IPsec  
Se si usa l'autenticazione IPsec basata su certificato, il server e i client DirectAccess devono ottenere un certificato del computer. Il modo più semplice per installare i certificati consiste nel configurare una registrazione automatica basata sui Criteri di gruppo per i certificati del computer. In questo modo, tutti i membri del dominio ottengono un certificato da una CA globale (enterprise). Se non è una CA impostato all'interno dell'organizzazione, vedere [Servizi certificati Active Directory](https://technet.microsoft.com/library/cc770357.aspx).  
  
Questo certificato ha i seguenti requisiti:  
  
-   Il certificato deve avere un utilizzo chiave esteso (EKU) per l'autenticazione client.  
  
-   I certificati client e server devono essere concatenati allo stesso certificato radice, che deve essere selezionato nelle impostazioni di configurazione DirectAccess.  
  
### <a name="132-plan-certificates-for-ip-https"></a>1.3.2 Pianificare i certificati per IP-HTTPS  
Il server DirectAccess funge da listener IP-HTTPS ed è necessario installare un certificato del sito Web HTTPS nel server. Durante la pianificazione, prendere in considerazione gli elementi seguenti:  
  
-   L'uso di una CA pubblica è consigliato perché consente di avere subito disponibili gli elenchi di revoche di certificati (CRL).  
  
-   Nel campo **Soggetto**, specificare l'indirizzo IPv4 della scheda Internet del server DirectAccess o il nome di dominio completo dell'URL IP-HTTPS (indirizzo ConnectTo). Se il server DirectAccess si trova dietro un dispositivo NAT, deve essere specificato il nome pubblico o l'indirizzo del dispositivo NAT.  
  
-   Il nome comune del certificato deve corrispondere al nome del sito IP-HTTPS.  
  
-   Per il **Enhanced Key Usage** campo, utilizzare l'identificatore di oggetto (OID) autenticazione server.  
  
-   Per il campo **Punti di distribuzione Elenco di revoche di certificati (CRL)** , specificare un punto di distribuzione CRL accessibile dai client DirectAccess connessi a Internet.  
  
-   Il certificato IP-HTTPS deve avere una chiave privata.  
  
-   Il certificato IP-HTTPS deve essere importato direttamente nell'archivio personale.  
  
-   Nei nomi dei certificati IP-HTTPS possono essere presenti caratteri jolly.  
  
Se si pianifica l'uso di IP-HTTPS in una porta non standard, eseguire i passaggi seguenti nel server DirectAccess:  
  
1.  Rimuovere il binding al certificato esistente per 0.0.0.0:443 e sostituirlo con un binding al certificato per la porta prescelta. In questo esempio, viene usata la porta 44500. Prima di eliminare l'associazione del certificato, visualizzare e copiare il **appid**.  
  
    1.  Per eliminare il binding al certificato, immettere:  
  
        ```  
        netsh http delete ssl ipport=0.0.0.0:443  
        ```  
  
    2.  Per aggiungere il nuovo binding al certificato, immettere:  
  
        ```  
        netsh http add ssl ipport=0.0.0.0:44500 certhash=<use the thumbprint from the DirectAccess server SSL cert> appid=<use the appid from the binding that was deleted>  
        ```  
  
2.  Per modificare l'URL IP-HTTPS nel server, immettere:  
  
    ```  
    Netsh int http set int url=https://<DirectAccess server name (for example server.contoso.com)>:44500/IPHTTPS  
    ```  
  
    ```  
    Net stop iphlpsvc & net start iphlpsvc  
    ```  
  
3.  Modificare la prenotazione URL per kdcproxy.  
  
    1.  Per eliminare la prenotazione URL esistente, immettere:  
  
        ```  
        netsh http del urlacl url=https://+:443/KdcProxy/  
        ```  
  
    2.  Per aggiungere una nuova prenotazione URL, immettere:  
  
        ```  
        netsh http add urlacl url=https://+:44500/KdcProxy/ sddl=D:(A;;GX;;;NS)  
        ```  
  
4.  Aggiungere l'impostazione per mettere kppsvc in ascolto sulla porta non standard. Per aggiungere la voce del Registro di sistema, immettere:  
  
    ```  
    HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\KPSSVC\Settings /v HttpsUrlGroup /t REG_MULTI_SZ /d +:44500 /f  
    ```  
  
5.  Per riavviare il servizio kdcproxy nel controller di dominio, immettere:  
  
    ```  
    net stop kpssvc & net start kpssvc  
    ```  
  
Per usare IP-HTTPS in una porta non standard, eseguire i passaggi seguenti nel controller di dominio:  
  
1.  Modificare l'impostazione IP-HTTPS nell'oggetto Criteri di gruppo del client.  
  
    1.  Aprire l'Editor Criteri di gruppo.  
  
    2.  Passare in Configurazione computer =>Criteri=>Modelli amministrativi=> Rete=>Impostazioni TCPIP =>Tecnologie di transizione IPv6.  
  
    3.  Aprire l'impostazione dello stato IP-HTTPS e modificare l'URL **https://<DirectAccess nome del server (ad esempio server.contoso.com) >: 44500/IPHTTPS**.  
  
    4.  Fare clic su **Applica**.  
  
2.  Modificare le impostazioni del client proxy Kerberos nell'oggetto Criteri di gruppo del client.  
  
    1.  Nell'Editor Criteri di gruppo, passare in Configurazione computer=>Criteri=>Modelli amministrativi=> Sistema=>Kerberos => Specificare i server proxy KDC per i client Kerberos.  
  
    2.  Aprire l'impostazione dello stato IPHTTPS e modificare l'URL **https://<DirectAccess nome del server (ad esempio server.contoso.com) >: 44500/IPHTTPS**.  
  
    3.  Fare clic su **Applica**.  
  
3.  Modificare le impostazioni dei criteri IPsec del client per usare ComputerKerb e UserKerb.  
  
    1.  Nell'Editor Criteri di gruppo, passare in Configurazione computer=>Criteri=> Impostazioni di Windows=> Impostazioni di sicurezza=> Windows Firewall con sicurezza avanzata.  
  
    2.  Fare clic su **Regole di sicurezza della connessione**, quindi fare doppio clic su **Regola IPsec**.  
  
    3.  Nella scheda **Autenticazione**, fare clic su **Avanzate**.  
  
    4.  Per Auth1: rimuovere il metodo di autenticazione esistente e sostituirlo con ComputerKerb. Per Auth2: rimuovere il metodo di autenticazione esistente e sostituirlo con UserKerb.  
  
    5.  Fare clic su **Applica**, quindi su **OK**.  
  
Per completare la procedura manuale per l'utilizzo di una porta non standard IP-HTTPS, eseguire **gpupdate /force** sul computer client e il server DirectAccess.  
  
### <a name="133-plan-website-certificates-for-the-network-location-server"></a>1.3.3 Pianificare i certificati del sito Web per il server dei percorsi di rete  
Quando si pianifica il sito Web del server dei percorsi di rete, tenere presente quanto segue:  
  
-   Nel **soggetto** specificare l'indirizzo IP dell'interfaccia intranet del server dei percorsi di rete o il nome di dominio COMPLETO dell'URL del percorso di rete.  
  
-   Nel **Enhanced Key Usage** campo, usare l'OID di autenticazione Server.  
  
-   Nel campo **Punti di distribuzione Elenco di revoche di certificati (CRL)** , usare un punto di distribuzione CRL accessibile dai client DirectAccess connessi alla Intranet. Questo punto di distribuzione CRL non deve essere accessibile fuori dalla rete interna.  
  
-   Se si pianifica la successiva configurazione di una distribuzione multisito o cluster, il nome del certificato non deve corrispondere al nome interno dei server DirectAccess aggiunti alla distribuzione.  
  
    > [!NOTE]  
    > Verificare che i certificati per IP-HTTPS e il server dei percorsi di rete abbiano un **nome soggetto**. Se il certificato non dispone di un **nome soggetto**, ma ha un **nome alternativo**, non verrà accettato dalla procedura guidata accesso remoto.  
  
## <a name="14-plan-dns-requirements"></a>1.4 Pianificare i requisiti DNS  
In questa sezione vengono spiegati i requisiti DNS per le richieste dei client DirectAccess e i server dell'infrastruttura in una distribuzione di Accesso remoto. Sono incluse le sottosezioni seguenti:  
  
-   [1.4.1 pianificare i requisiti del server DNS](#141-plan-for-dns-server-requirements)  
  
-   [1.4.2 pianificare la risoluzione dei nomi locali](#142-plan-for-local-name-resolution)  
  
**Richieste dei client DirectAccess**  
  
DNS viene usato per risolvere le richieste provenienti dai computer client DirectAccess non presenti nella rete interna (o aziendale). I client DirectAccess tentano di connettersi al server dei percorsi di rete DirectAccess per determinare se si trovano sulla rete Internet o su quella interna.  
  
-   Se la connessione riesce, viene rilevato che i client si trovano sulla rete interna, DirectAccess non viene usato e le richieste client vengono risolte usando il server DNS configurato nella scheda di rete del computer client.  
  
-   Se la connessione non riesce, si presuppone che i client si trovino sulla rete Internet e i client DirectAccess usano la tabella dei criteri di risoluzione dei nomi (NRPT) per determinare quale server DNS usare per la risoluzione delle richieste di nomi.  
  
È possibile specificare che i client usano DNS64 DirectAccess per risolvere i nomi o un server DNS interno alternativo. Quando si esegue la risoluzione dei nomi, la tabella dei criteri di risoluzione dei nomi viene usata dai client DirectAccess per stabilire come gestire una richiesta. I client richiedono un nome di dominio completo o un nome con etichetta singola, ad esempio <https://internal>. Se è richiesto un nome con etichetta singola, viene aggiunto un suffisso DNS per trasformarlo in un nome di dominio completo. Se la query DNS corrisponde a una voce nella tabella dei criteri di risoluzione dei nomi ed è specificato DNS64 o un server DNS nella rete interna per la voce, la query viene inviata per la risoluzione dei nomi attraverso il server specificato. Se esiste una corrispondenza, ma non sono specificati server DNS, si è in presenza di una regola di esenzione e viene applicata la normale risoluzione dei nomi.  
  
> [!NOTE]  
> Quando viene aggiunto un nuovo suffisso alla tabella dei criteri di risoluzione dei nomi nella Console di gestione Accesso remoto, i server DNS predefiniti per il suffisso possono essere individuati automaticamente facendo clic su **Rileva**.  
  
Il rilevamento automatico funziona in questo modo:  
  
-   Se la rete aziendale è basata su IPv4 o usa IPv4 e IPv6, l'indirizzo predefinito è l'indirizzo DNS64 della scheda interna nel server DirectAccess.  
  
-   Se la rete aziendale è basata su IPv6, l'indirizzo predefinito è l'indirizzo IPv6 dei server DNS nella rete aziendale.  
  
**Server di infrastruttura**  
  
-   **Server dei percorsi di rete**  
  
    I client DirectAccess tentano di raggiungere il server dei percorsi di rete per determinare se si trovano sulla rete interna. I client nella rete interna devono poter risolvere il nome del server dei percorsi di rete, ma non quello del percorso in cui si trovano sulla rete Internet. Per verificarlo, il nome di dominio completo del server dei percorsi di rete viene aggiunto per impostazione predefinita come regola di esenzione nella tabella dei criteri di risoluzione dei nomi. Quando si configura Accesso remoto vengono create automaticamente anche le seguenti regole:  
  
    -   Una regola per il suffisso DNS per il dominio radice o il nome dominio del server DirectAccess e gli indirizzi IPv6 corrispondenti all'indirizzo DNS64. Nelle reti aziendali solo IPv6, i server DNS Intranet sono configurati nel server DirectAccess. Ad esempio, se il server DirectAccess è membro del dominio corp.contoso.com, viene creata una regola per il suffisso DNS corp.contoso.com.  
  
    -   Una regola di esenzione per il nome di dominio completo del server del percorso di rete. Ad esempio, se l'URL di server percorsi di rete è <https://nls.corp.contoso.com>, viene creata una regola di esenzione per il nome di dominio completo nls.corp.contoso.com.  
  
-   **IP-HTTPS server**  
  
    Il server DirectAccess funge da listener IP-HTTPS e usa il certificato del server per autenticarsi nei client IP-HTTPS. Il nome IP-HTTPS deve essere risolvibile dai client DirectAccess che usano i server DNS pubblici.  
  
-   **Delle revoche CRL**  
  
    DirectAccess usa il rilevamento delle revoche del certificato per la connessione IP-HTTPS tra i client DirectAccess e il server DirectAccess e per la connessione basata su HTTPS tra il client DirectAccess e il server dei percorsi di rete. In entrambi i casi, i client DirectAccess devono poter risolvere e accedere al percorso del punto di distribuzione CRL.  
  
-   **ISATAP**  
  
    ISATAP consente ai computer aziendali di acquisire un indirizzo IPv6 e incapsula i pacchetti IPv6 in un'intestazione IPv4. Viene usato dal server DirectAccess per fornire la connettività IPv6 agli host ISATAP in una Intranet. In un ambiente di rete IPv6 non nativo, il server DirectAccess si configura automaticamente come router ISATAP.  
  
    ISATAP non è più supportato in DirectAccess, quindi è necessario verificare che i server DNS siano configurati in modo da non rispondere alle query ISATAP. Per impostazione predefinita, il servizio Server DNS blocca la risoluzione dei nomi per il nome ISATAP con l'elenco globale delle query DNS da bloccare. Non rimuovere il nome ISATAP dall'elenco globale delle query da bloccare.  
  
-   **Strumenti di verifica della connettività**  
  
    Accesso remoto crea un probe Web predefinito usato dai computer client DirectAccess per verificare la connettività alla rete interna. Per verificare il corretto funzionamento del probe, è necessario registrare manualmente i seguenti nomi in DNS:  
  
    -   **DirectAccess-webprobehost**-dovrebbe risolvere l'indirizzo IPv4 interno del server DirectAccess o l'indirizzo IPv6 in un ambiente solo IPv6.  
  
    -   **DirectAccess-corpconnectivityhost**-dovrebbe risolvere l'indirizzo dell'host locale (loopback). Devono essere creati i seguenti record di risorse dell'host (A) e (AAAA): un record di risorse dell'host (A) con valore 127.0.0.1 e un record di risorse dell'host (AAAA) con valore costruito a partire dal prefisso NAT64 con 127.0.0.1 come ultimi 32 bit. Il prefisso NAT64 può essere recuperato eseguendo il comando di Windows PowerShell **get-netnattransitionconfiguration**.  
  
        > [!NOTE]  
        > Questa procedura è valida esclusivamente in un ambiente solo IPv4. In un ambiente IPv4 più IPv6 o solo IPv6, deve essere creato un solo record di risorse dell'host (AAAA) con l'indirizzo IP di loopback ::1.  
  
    È possibile creare altri strumenti di verifica della connettività con altri indirizzi Web su HTTP o usando **ping**. Per ogni strumento di verifica della connettività deve esistere una voce DNS.  
  
### <a name="141-plan-for-dns-server-requirements"></a>1.4.1 Pianificare i requisiti del server DNS  
Di seguito sono descritti i requisiti per DNS quando si distribuisce DirectAccess.  
  
-   Per i client DirectAccess, è necessario utilizzare un server DNS che esegue Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2, Windows Server 2008 o qualsiasi altro server DNS che supporti IPv6.  
  
    > [!NOTE]  
    > Non è consigliabile usare server DNS che eseguono Windows Server 2003 quando si distribuisce DirectAccess. Sebbene i server DNS di Windows Server 2003 supportino i record IPv6, Windows Server 2003 non è più supportato da Microsoft. Inoltre, DirectAccess non deve essere distribuito se i controller di dominio eseguono Windows Server 2003 a causa di un problema con il servizio Replica file. Per ulteriori informazioni, vedere [DirectAccess configurazioni non supportate](https://technet.microsoft.com/library/dn464274.aspx).  
  
-   Usare un server DNS che supporti gli aggiornamenti dinamici. Si possono usare server DNS che non supportano gli aggiornamenti dinamici, ma è necessario aggiornare manualmente le voci nei server.  
  
-   Il nome di dominio completo per i punti di distribuzione CRL accessibili da Internet deve essere risolvibile con i server DNS Internet. Ad esempio, se URL <https://crl.contoso.com/crld/corp-DC1-CA.crl> è il **punti di distribuzione CRL** campo del certificato IP-HTTPS del server DirectAccess, è necessario assicurarsi che il nome di dominio completo crld.contoso.com sia risolvibile mediante server DNS Internet.  
  
### <a name="142-plan-for-local-name-resolution"></a>1.4.2 Pianificare la risoluzione dei nomi locali  
Quando si pianifica la risoluzione dei nomi locali, tenere presenti i seguenti problemi:  
  
**NRPT**  
  
Nei seguenti casi, potrebbe essere necessario creare regole NRPT aggiuntive:  
  
-   Per aggiungere più suffissi DNS allo spazio dei nomi Intranet.  
  
-   Se il nome di dominio completo (FQDN) dei punti di distribuzione CRL si basa sullo spazio dei nomi Intranet, aggiungere delle regole di esenzione per i nomi di dominio completi dei punti di distribuzione CRL.  
  
-   In un ambiente DNS "split brain" è necessario aggiungere regole di esenzione per i nomi delle risorse per cui si desidera che i client DirectAccess in Internet accedano alla versione Internet, anziché alla versione Intranet.  
  
-   Se si reindirizza il traffico a un sito Web esterno attraverso i server proxy Web Intranet, il sito Web esterno è disponibile solo dalla Intranet e usa gli indirizzi dei server proxy Web per consentire le richieste in entrata. In questo caso, aggiungere una regola di esenzione per il nome di dominio completo del sito Web esterno e specificare che tale regola usa il server proxy Web Intranet e non gli indirizzi IPv6 dei server DNS Intranet.  
  
    Ad esempio, se si sta testando un sito Web esterno denominato test.contoso.com, questo nome non è risolvibile con i server DNS Internet, mentre il server proxy Web di Contoso è possibile risolvere il nome e indirizzare le richieste per il sito Web al server Web esterno. Per impedire l'accesso al sito da parte di utenti che non si trovano nella Intranet di Contoso, il sito Web esterno consente le richieste solo dall'indirizzo Internet IPv4 del proxy Web di Contoso. Quindi, gli utenti della Intranet possono accedere al sito Web perché usano il proxy Web di Contoso, a differenza degli utenti DirectAccess che non lo usano. Configurando una regola di esenzione della tabella dei criteri di risoluzione dei nomi per test.contoso.com che usa il proxy Web di Contoso, le richieste di pagine Web per test.contoso.com vengono indirizzate al server proxy Web Intranet sulla rete Internet IPv4.  
  
**Nomi con etichetta singola**  
  
Singola, ad esempio i nomi delle etichette, <https://paycheck>, vengono usati talvolta per i server intranet. Se è richiesto un nome con etichetta singola ed è configurato un elenco di ricerca dei suffissi DNS, i suffissi DNS nell'elenco vengono aggiunti al nome con etichetta singola. Ad esempio, quando un utente in un computer che è un membro del dominio corp.contoso.com digita <https://paycheck> nel web browser, il nome di dominio completo che viene costruito come il nome è paycheck.corp.contoso.com. Per impostazione predefinita, il suffisso aggiunto si basa sul suffisso DNS primario del client computer.  
  
> [!NOTE]  
> In uno scenario di spazio dei nomi indipendente (in cui uno o più computer del dominio hanno un suffisso DNS non corrispondente al dominio Active Directory a cui appartengono i computer), verificare che l'elenco di ricerca sia personalizzato e includa tutti i suffissi richiesti. Per impostazione predefinita, la Configurazione guidata accesso remoto configura il nome DNS di Active Directory come suffisso DNS primario nel client. Aggiungere il suffisso DNS emesso dai client per la risoluzione dei nomi.  
  
Se vengono distribuiti più domini e Windows Internet Name Service (WINS) nell'organizzazione e si è connessi in remoto, i nomi singoli possono essere risolti come segue:  
  
-   Distribuire una zona di ricerca diretta WINS in DNS. Quando si risolve computername.dns.zone1.corp.contoso.com, la richiesta viene indirizzata al server WINS che usa solo computername. Il client ritiene di emettere un record di risorse di un normale host DNS (A), ma in realtà si tratta di una richiesta NetBIOS. Per altre informazioni, vedere Gestione di una zona di ricerca diretta.  
  
-   Aggiungere un suffisso DNS, ad esempio dns.zone1.corp.contoso.com, all'oggetto Criteri di gruppo Criterio dominio predefinito.  
  
**DNS "split brain"**  
  
Per DNS "split-brain" si intende l'uso dello stesso dominio DNS per la risoluzione dei nomi Internet e Intranet.  
  
Per le distribuzioni DNS "split Brain", è necessario elencare i nomi di dominio completi duplicati in Internet e intranet e decidere quali risorse il client DirectAccess deve raggiungere la rete intranet o la versione di Internet. Per ogni nome corrispondente a una risorsa per cui si desidera che i client DirectAccess raggiungano la versione Internet, è necessario aggiungere il nome di dominio completo corrispondente come regola di esenzione per la tabella dei criteri di risoluzione dei nomi per i client DirectAccess.  
  
In un ambiente DNS "split brain", per rendere disponibili entrambe le versioni della risorsa, configurare le risorse Intranet con nomi alternativi che non siano duplicati dei nomi usati in Internet e comunicare agli utenti di usare il nome alternativo nella Intranet. Configurare ad esempio il nome alternativo www.internal.contoso.com per il nome interno www.contoso.com.  
  
In un ambiente DNS non "split brain", lo spazio dei nomi Internet è diverso da quello Intranet. Contoso Corporation utilizza ad esempio contoso.com in Internet e corp.contoso.com nella Intranet. Poiché tutte le risorse Intranet utilizzano il suffisso DNS corp.contoso.com, la regola della tabella dei criteri di risoluzione dei nomi per corp.contoso.com indirizza tutte le query relative ai nomi DNS per le risorse Intranet ai server DNS Intranet. Le query relative ai nomi DNS per i nomi con suffisso contoso.com non corrispondono alla regola dello spazio dei nomi Intranet di corp.contoso.com nella tabella dei criteri di risoluzione dei nomi e vengono inviate ai server DNS Internet. Con una distribuzione DNS non "split brain", poiché i nomi di dominio completi non vengono duplicati per le risorse Intranet e Internet, non è necessaria alcuna configurazione aggiuntiva per la tabella dei criteri di risoluzione dei nomi. I client DirectAccess possono accedere alle risorse Internet e Intranet per l'organizzazione.  
  
**Comportamento di risoluzione dei nomi locali per client DirectAccess**  
  
Se un nome non può essere risolto con DNS, per risolvere il nome della subnet locale, il servizio Client DNS in Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2, Windows 8 e Windows 7 consente la risoluzione dei nomi locali, con il Link-Local Multicast Name LLMNR (Resolution) e NetBIOS su protocolli TCP/IP.  
  
La risoluzione dei nomi locali è in genere necessaria per la connettività peer-to-peer se il computer si trova in reti private, ad esempio reti domestiche a subnet singola. Se il servizio Client DNS esegue la risoluzione dei nomi locali per i nomi di server Intranet e il computer è connesso a una subnet condivisa in Internet, utenti malintenzionati possono acquisire LLMNR e NetBIOS su messaggi TCP/IP per determinare i nomi di server Intranet. Nella pagina DNS della Configurazione guidata server di infrastruttura, il comportamento di risoluzione dei nomi locali viene configurato in base ai tipi di risposte ricevuti dai server DNS Intranet. Sono disponibili le seguenti opzioni:  
  
-   **Utilizzare la risoluzione dei nomi locali se il nome non esiste nel DNS**. Questa opzione è la più sicura perché il client DirectAccess esegue la risoluzione dei nomi locali solo per i nomi dei server che non possono essere risolti dal server DNS Intranet. Se i server DNS Intranet sono raggiungibili, i nomi di server Intranet vengono risolti. Se i server DNS Intranet non sono raggiungibili o se si verificano altri tipi di errori DNS, i nomi di server Intranet non vengono comunicati alla subnet mediante la risoluzione dei nomi locali.  
  
-   **Usa risoluzione dei nomi locali se il nome non esiste nel DNS o i server DNS non sono raggiungibili quando il computer client si trova in una rete privata (opzione consigliata)** . Questa opzione è consigliata perché consente l'uso della risoluzione dei nomi locali in una rete privata solo quando i server DNS Intranet non sono raggiungibili.  
  
-   **Utilizzare la risoluzione dei nomi locali per qualsiasi tipo di errore di risoluzione DNS (opzione meno sicura)** . Si tratta dell'opzione meno sicura, poiché i nomi dei server della rete Intranet possono essere comunicati alla subnet locale mediante risoluzione dei nomi locali.  
  
## <a name="15-plan-the-network-location-server"></a>1.5 Pianificare il server dei percorsi di rete  
Il server dei percorsi di rete è un sito Web usato per rilevare se i client DirectAccess si trovano nella rete aziendale. I client nella rete aziendale non usano DirectAccess per raggiungere le risorse interne, ma vi si connettono direttamente.  
  
Il sito Web del server dei percorsi di rete può essere ospitato nel server DirectAccess o in un altro server dell'organizzazione. Se si ospita il server dei percorsi di rete nel server DirectAccess, il sito Web viene creato automaticamente quando si installa il ruolo server Accesso remoto. Se si ospita il server dei percorsi di rete in un altro server dell'organizzazione che esegue un sistema operativo Windows, verificare che Internet Information Services (IIS) sia installato nel server e che il sito Web sia stato creato. DirectAccess non configura le impostazioni in un server dei percorsi di rete remoto.  
  
Verificare che il sito Web del server dei percorsi di rete soddisfi i seguenti requisiti:  
  
-   È un sito Web con un certificato del server HTTPS.  
  
-   I computer client DirectAccess devono considerare attendibile la CA che ha emesso il certificato del server nel sito Web del server dei percorsi di rete.  
  
-   I computer client DirectAccess nella rete interna devono poter risolvere il nome del sito del server dei percorsi di rete.  
  
-   Il sito del server dei percorsi di rete deve site fornire una disponibilità elevata ai computer della rete interna.  
  
-   Il server dei percorsi di rete non deve essere accessibile dai computer client DirectAccess in Internet.  
  
-   Il certificato del server deve essere verificato in base a un CRL.  
  
### <a name="151-plan-certificates-for-the-network-location-server"></a>1.5.1 Pianificare i certificati per il server dei percorsi di rete  
Quando si richiede il certificato del sito Web da usare per il server dei percorsi di rete, tenere presente quanto segue:  
  
1.  Nel **soggetto** specificare un indirizzo IP dell'interfaccia intranet del server dei percorsi di rete o il nome di dominio COMPLETO dell'URL del percorso di rete.  
  
2.  Nel **Enhanced Key Usage** campo, usare l'OID di autenticazione Server.  
  
3.  Nel campo **Punti di distribuzione Elenco di revoche di certificati (CRL)** , usare un punto di distribuzione CRL accessibile dai client DirectAccess connessi alla Intranet. Questo punto di distribuzione CRL non deve essere accessibile fuori dalla rete interna.  
  
### <a name="152-plan-dns-for-the-network-location-server"></a>1.5.2 Pianificare il DNS per il server dei percorsi di rete  
I client DirectAccess tentano di raggiungere il server dei percorsi di rete per determinare se si trovano sulla rete interna. I client nella rete interna devono poter risolvere il nome del server dei percorsi di rete, ma non quello del percorso in cui si trovano sulla rete Internet. Per verificarlo, il nome di dominio completo del server dei percorsi di rete viene aggiunto per impostazione predefinita come regola di esenzione nella tabella dei criteri di risoluzione dei nomi.  
  
## <a name="16-plan-management-servers"></a>1.6 Pianificare i server di gestione  
I client DirectAccess avviano le comunicazioni con i server di gestione che forniscono servizi quali Windows Update e gli aggiornamenti antivirus. I client DirectAccess usano anche il protocollo Kerberos per contattare i controller di dominio per l'autenticazione prima dell'accesso alla rete interna. Durante la gestione remota dei client DirectAccess, i server di gestione comunicano con i computer client per eseguire funzioni di gestione quali le valutazioni degli inventari software o hardware. Accesso remoto può individuare automaticamente alcuni server di gestione, tra cui:  
  
-   Dominio controller--l'individuazione automatica dei controller di dominio viene eseguita per tutti i domini nella stessa foresta come i computer client e server DirectAccess.  
  
-   System Center Configuration Manager Server-rilevamento automatico dei server di System Center Configuration Manager viene eseguito per tutti i domini nella stessa foresta come i computer client e server DirectAccess.  
  
I controller di dominio e i server System Center Configuration Manager vengono rilevati automaticamente alla prima configurazione di DirectAccess. I controller di dominio rilevati non vengono visualizzati nella console, ma le impostazioni possono essere recuperate tramite il cmdlet Windows PowerShell **Get-DAMgmtServer-tutti di tipo**. Se vengono modificati i controller di dominio o server System Center Configuration Manager, fare clic su **Aggiorna server di gestione** in Gestione accesso remoto console Aggiorna l'elenco dei server di gestione.  
  
**Requisiti del server di gestione**  
  
-   I server di gestione devono essere accessibili sul primo tunnel (infrastruttura). Quando si configura Accesso remoto, l'aggiunta di server all'elenco dei server di gestione li rende automaticamente accessibili su questo tunnel.  
  
-   I server di gestione che avviano connessioni ai client DirectAccess devono supportare completamente IPv6 con un indirizzo IPv6 nativo o usando un altro indirizzo assegnato da ISATAP.  
  
## <a name="17-plan-active-directory-domain-services"></a>1.7 Pianificare Servizi di dominio Active Directory  
Questa sezione illustra come viene usato Servizi di dominio Active Directory (AD DS) in DirectAccess e comprende le seguenti sottosezioni:  
  
-   [1.7.1 pianificare l'autenticazione client](#171-plan-client-authentication)  
  
-   [1.7.2 pianificare più domini](#172-plan-multiple-domains)  
  
DirectAccess utilizza oggetti Criteri di dominio Active Directory e Active Directory gruppo (GPO) come indicato di seguito:  
  
-   **Autenticazione**  
  
    AD DS viene usato per l'autenticazione. Il tunnel dell'infrastruttura usa l'autenticazione NTLMv2 per l'account del computer che si connette al server DirectAccess, che deve essere elencato in un dominio Active Directory. Il tunnel Intranet usa l'autenticazione Kerberos per la creazione del secondo tunnel da parte dell'utente.  
  
-   **Oggetti Criteri di gruppo**  
  
    DirectAccess raccoglie le impostazioni di configurazione in oggetti Criteri di gruppo che vengono applicati ai server DirectAccess, ai client e ai server applicazioni interni.  
  
-   **Gruppi di sicurezza**  
  
    DirectAccess usa gruppi di sicurezza per raccogliere e identificare i computer client DirectAccess. Gli oggetti Criteri di gruppo vengono applicati al gruppo di sicurezza richiesto.  
  
-   **Criteri IPsec estesi**  
  
    DirectAccess può usare l'autenticazione IPsec e la crittografia tra i client e il server DirectAccess. È possibile estendere l'autenticazione IPSec e la crittografia dal client ai server di applicazioni interni specificati. A questo scopo, aggiungere i server applicazioni a un gruppo di sicurezza.  
  
**Requisiti di Active Directory Domain Services**  
  
Quando si pianifica AD DS per una distribuzione DirectAccess, tenere presenti i seguenti requisiti:  
  
-   Almeno un controller di dominio deve essere installato con il sistema operativo Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2 o Windows Server 2008.  
  
    Se il controller di dominio si trova su una rete perimetrale (ed è pertanto raggiungibile dalla scheda di rete con connessione Internet del server DirectAccess), impedire al server DirectAccess di raggiungerlo aggiungendo filtri pacchetti sul controller di dominio, in modo da ostacolare la connettività all'indirizzo IP della scheda Internet.  
  
-   Il server DirectAccess deve essere membro di un dominio.  
  
-   I client DirectAccess devono essere membri di un dominio. I client possono appartenere a:  
  
    -   Qualsiasi dominio nella stessa foresta del server DirectAccess.  
  
    -   Qualsiasi dominio che abbia un trust bidirezionale con il dominio del server DirectAccess.  
  
    -   Qualsiasi dominio in una foresta che abbia un trust bidirezionale con la foresta alla quale appartiene il dominio DirectAccess.  
  
> [!NOTE]  
> -   Il server DirectAccess non può essere un controller di dominio.  
> -   Il controller di dominio AD DS usato per DirectAccess non deve essere raggiungibile dalla scheda Internet esterna del server DirectAccess (la scheda non deve trovarsi nel profilo di dominio di Windows Firewall).  
  
### <a name="171-plan-client-authentication"></a>1.7.1 Pianificare l'autenticazione client  
DirectAccess consente di scegliere se usare i certificati per l'autenticazione dei computer IPsec o un proxy Kerberos incorporato che esegue l'autenticazione con nomi utente e password.  
  
Se si sceglie di usare le credenziali Ad DS per l'autenticazione, DirectAccess usa un tunnel di sicurezza con Kerberos computer per la prima autenticazione e Kerberos utente per la seconda. Quando si usa questa modalità per l'autenticazione, DirectAccess usa un solo tunnel di sicurezza che fornisce l'accesso al server DNS, al controller di dominio e ad altri server sulla rete interna.  
  
Se si sceglie di usare l'autenticazione a due fattori o Protezione accesso alla rete, DirectAccess usa due tunnel di sicurezza. La Configurazione guidata Accesso remoto configura le regole di sicurezza della connessione di Windows Firewall con sicurezza avanzata che specificano l'uso dei seguenti tipi di credenziali quando si negoziano le associazioni di sicurezza IPsec per i tunnel nel server DirectAccess:  
  
-   Il tunnel dell'infrastruttura usa le credenziali Kerberos computer per la prima autenticazione e Kerberos utente per la seconda.  
  
-   Il tunnel Intranet usa le credenziali del certificato del computer per la prima autenticazione e Kerberos utente per la seconda.  
  
Quando DirectAccess sceglie di consentire l'accesso ai client che eseguono Windows 7 o in una distribuzione multisito, usa due tunnel di sicurezza. La Configurazione guidata Accesso remoto configura le regole di sicurezza della connessione di Windows Firewall con sicurezza avanzata che specificano l'uso dei seguenti tipi di credenziali quando si negoziano le associazioni di sicurezza IPsec per i tunnel nel server DirectAccess:  
  
-   Il tunnel dell'infrastruttura usa le credenziali del certificato del computer per la prima autenticazione e NTLMv2 per la seconda. Le credenziali NTLMv2 forzano l'uso di Authenticated Internet Protocol (AuthIP) e forniscono l'accesso a un server DNS e al controller di dominio prima che il client DirectAccess possa usare le credenziali Kerberos per il tunnel Intranet.  
  
-   Il tunnel Intranet usa le credenziali del certificato del computer per la prima autenticazione e Kerberos utente per la seconda.  
  
### <a name="172-plan-multiple-domains"></a>1.7.2 Pianificare più domini  
L'elenco dei server di gestione deve includere i controller di dominio di tutti i domini che contengono gruppi di sicurezza con computer client DirectAccess. Deve contenere tutti i domini con account utente che potrebbero usare i computer configurati come client DirectAccess. In questo modo, si assicura gli utenti non presenti nello stesso dominio del computer client che stanno usando vengano autenticati con un controller di dominio nel dominio utente. Questa operazione viene eseguita automaticamente se i domini si trovano nella stessa foresta.  
  
> [!NOTE]  
> Se nei gruppi di sicurezza sono presenti computer usati per i computer client o i server applicazioni di foreste diverse, i controller di dominio di queste foreste non vengono rilevati automaticamente. È possibile eseguire l'attività **Aggiorna server di gestione** nella Console di gestione Accesso remoto per rilevare questi controller di dominio.  
  
Dove possibile, aggiungere i suffissi dei nomi di dominio comuni alla tabella dei criteri di risoluzione dei nomi (NRPT) durante la distribuzione di Accesso remoto. Ad esempio, se si hanno due domini, domain1.corp.contoso.com e domain2.corp.contoso.com, è possibile aggiungere una voce del suffisso DNS comune con il suffisso del nome di dominio corp.contoso.com, invece di due voci nella tabella dei criteri di risoluzione dei nomi. Questa operazione è automatica nei domini nella stessa radice, mentre i domini che non si trovano nella stessa radice devono essere aggiunti manualmente.  
  
Se Windows Internet Name Service (WINS) viene distribuito in un ambiente con più domini, è necessario distribuire una zona di ricerca diretta WINS in DNS. Per ulteriori informazioni, vedere **i nomi con etichetta singola** nel [1.4.2 pianificare la risoluzione dei nomi locali](#142-plan-for-local-name-resolution) in precedenza in questo documento.  
  
## <a name="18-plan-group-policy-objects"></a>1.8 Pianificare gli oggetti Criteri di gruppo  
Questa sezione illustra il ruolo degli oggetti Criteri di gruppo nell'infrastruttura di Accesso remoto e comprende le seguenti sottosezioni:  
  
-   [1.8.1 configurare oggetti Criteri di gruppo creati automaticamente](#181-configure-automatically-created-gpos)  
  
-   [1.8.2 configurare oggetti Criteri di gruppo creati manualmente](#182-configure-manually-created-gpos)  
  
-   [1.8.3 gestire oggetti Criteri di gruppo in un ambiente multidominio controller](#183-manage-gpos-in-a-multi-domain-controller-environment)  
  
-   [1.8.4 gestire GPO di accesso remoto con autorizzazioni limitate](#184-manage-remote-access-gpos-with-limited-permissions)  
  
-   [1.8.5 ripristinare da un oggetto Criteri di gruppo eliminato](#185-recover-from-a-deleted-gpo)  
  
Le impostazioni DirectAccess configurate durante la configurazione di Accesso remoto vengono raccolte negli oggetti Criteri di gruppo. I seguenti tipi di oggetti Criteri di gruppo vengono popolati con le impostazioni DirectAccess e distribuiti come segue:  
  
-   **GRUPPO di client DirectAccess**  
  
    Questo oggetto Criteri di gruppo contiene le impostazioni del client, incluse le impostazioni per la tecnologia di transizione IPv6, le voci NRPT e le regole di sicurezza della connessione di Windows Firewall con sicurezza avanzata. L'oggetto Criteri di gruppo viene applicato ai gruppi di sicurezza specificati per i computer client.  
  
-   **DirectAccess server oggetto Criteri di gruppo**  
  
    Questo oggetto Criteri di gruppo contiene le impostazioni di configurazione DirectAccess applicate a qualsiasi server configurato come server DirectAccess nella distribuzione. Contiene anche le regole di sicurezza della connessione di Windows Firewall con sicurezza avanzata.  
  
-   **Application Server oggetto Criteri di gruppo**  
  
    Questo oggetto Criteri di gruppo contiene impostazioni per server applicazioni selezionati ai quali si estendono facoltativamente l'autenticazione e la crittografia dai client DirectAccess. Se l'autenticazione e la crittografia non vengono estese, questo oggetto Criteri di gruppo non verrà usato.  
  
È possibile configurare gli oggetti Criteri di gruppo in due modi:  
  
-   **Automaticamente**-è possibile specificare che vengono creati automaticamente. Per ogni oggetto Criteri di gruppo viene specificato un nome predefinito.  
  
-   **Manualmente**-è possibile utilizzare oggetti Criteri di gruppo che sono state definite dall'amministratore di Active Directory.  
  
> [!NOTE]  
> Dopo aver configurato DirectAccess in modo da usare specifici oggetti Criteri di gruppo, non sarà possibile configurarlo in modo da usarne altri.  
  
Se si usano oggetti Criteri di gruppo configurati automaticamente o manualmente, è necessario aggiungere un criterio per il rilevamento dei collegamenti lenti se i client usano le reti 3G. Il percorso per **criteri: Configurare il rilevamento collegamento lento Criteri di gruppo** è: **Configurazione computer/Criteri/Modelli amministrativi/Sistema/Criteri di gruppo**.  
  
> [!CAUTION]  
> Usare la procedura seguente per eseguire il backup di tutti gli oggetti Criteri di gruppo di Accesso remoto prima di eseguire i cmdlet DirectAccess: [Backup e ripristino della configurazione di Accesso remoto](https://go.microsoft.com/fwlink/?LinkID=257928).  
  
Se non esistono autorizzazioni corrette (elencate nelle seguenti sezioni) per il collegamento degli oggetti Criteri di gruppo, viene emesso un avviso. L'operazione di Accesso remoto continua, ma il collegamento non verrà eseguito. Se viene emesso questo avviso non verranno creati automaticamente i collegamenti, nemmeno dopo che saranno state aggiunte le autorizzazioni. L'amministratore dovrà invece creare i collegamenti manualmente.  
  
### <a name="181-configure-automatically-created-gpos"></a>1.8.1 Configurare gli oggetti Criteri di gruppo creati automaticamente  
Tenere presente quanto segue quando si usano gli oggetti Criteri di gruppo creati automaticamente.  
  
Gli oggetti Criteri di gruppo creati automaticamente vengono applicati secondo i parametri di percorso e destinazione collegamento, come segue:  
  
-   Per l'oggetto Criteri di gruppo del server DirectAccess, sia il parametro di percorso che di collegamento puntano al dominio contenente il server DirectAccess.  
  
-   Quando si creano oggetti Criteri di gruppo del server applicazioni e del client, il percorso viene impostato su un unico dominio in cui viene creato l'oggetto Criteri di gruppo. Il nome dell'oggetto Criteri di gruppo viene cercato in tutti i domini e, se esistente, viene compilato con le impostazioni DirectAccess. La destinazione collegamento viene impostata sulla radice del dominio in cui è stato creato l'oggetto Criteri di gruppo. Viene creato un oggetto Criteri di gruppo per ogni dominio che contiene computer client o server applicazioni, che sarà quindi collegato alla radice del rispettivo dominio.  
  
Quando si usano oggetti Criteri di gruppo creati automaticamente, per applicare le impostazioni DirectAccess all'amministratore di Accesso remoto saranno necessarie le autorizzazioni seguenti:  
  
-   Autorizzazioni di creazione oggetto Criteri di gruppo per ogni dominio  
  
-   Autorizzazioni per il collegamento di tutte le radici del dominio client selezionate  
  
-   Autorizzazioni per il collegamento delle radici del dominio oggetto Criteri di gruppo del server  
  
Inoltre, sono necessarie le seguenti autorizzazioni:  
  
-   Le autorizzazioni di sicurezza di creazione, modifica ed eliminazione sono necessarie per gli oggetti Criteri di gruppo.  
  
-   È consigliabile che l'amministratore di Accesso remoto disponga delle autorizzazioni di lettura dell'oggetto Criteri di gruppo per ogni dominio richiesto. Ciò consente ad Accesso remoto di verificare che non esistano oggetti Criteri di gruppo con nomi duplicati durante la creazione degli oggetti Criteri di gruppo.  
  
### <a name="182-configure-manually-created-gpos"></a>1.8.2 Configurare gli oggetti Criteri di gruppo creati manualmente  
Tenere presente quanto segue quando si usano gli oggetti Criteri di gruppo creati manualmente:  
  
-   Gli oggetti Criteri di gruppo devono essere presenti prima di eseguire la Configurazione guidata Accesso remoto.  
  
-   Per applicare le impostazioni DirectAccess, all'amministratore di Accesso remoto sono necessarie le autorizzazioni complete per gli oggetti Criteri di gruppo creati manualmente (autorizzazioni di modifica ed eliminazione sicurezza).  
  
-   Viene eseguita una ricerca nell'intero dominio per un collegamento all'oggetto Criteri di gruppo. Se l'oggetto Criteri di gruppo non è collegato nel dominio, verrà automaticamente creato un collegamento nel dominio radice. Se le autorizzazioni richieste per creare il collegamento non sono disponibili verrà emesso un avviso.  
  
### <a name="183-manage-gpos-in-a-multi-domain-controller-environment"></a>1.8.3 Gestire gli oggetti Criteri di gruppo in un ambiente con più controller di dominio  
Ogni oggetto Criteri di gruppo viene gestito da uno specifico controller di dominio, come descritto di seguito:  
  
-   L'oggetto Criteri di gruppo del server viene gestito da uno dei controller di dominio nel sito Active Directory associato al server. Se i controller di dominio nel sito sono di sola lettura, l'oggetto Criteri di gruppo del server viene gestito dal controller di dominio con autorizzazioni di scrittura più prossimo al server DirectAccess.  
  
-   Gli oggetti Criteri di gruppo del server applicazioni e del client vengono gestiti dal controller di dominio in esecuzione come controller di dominio primario (PDC).  
  
Per modificare manualmente le impostazioni degli oggetti Criteri di gruppo, tenere presente quanto segue:  
  
-   Per l'oggetto Criteri di gruppo del server, per identificare il controller di dominio associato al server DirectAccess, eseguire **nltest /dsgetdc: /writable** a un prompt dei comandi con privilegi elevati nel server DirectAccess.  
  
-   Per impostazione predefinita, quando si apportano modifiche con i cmdlet di Windows PowerShell di rete o dalla Console Gestione Criteri di gruppo, viene usato il controller di dominio che funge da PDC.  
  
Inoltre, se si modificano le impostazioni in un controller di dominio diverso da quello associato al server DirectAccess (per l'oggetto Criteri di gruppo del server) o il PDC (per gli oggetti Criteri di gruppo del server applicazioni e del client), tenere presente quanto segue:  
  
-   Prima di modificare le impostazioni, verificare che il controller di dominio venga replicato con un oggetto Criteri di gruppo aggiornato ed eseguire il backup delle impostazioni dell'oggetto Criteri di gruppo. Per ulteriori informazioni, vedere [backup e ripristino configurazione di accesso remoto](https://go.microsoft.com/fwlink/?LinkID=257928). Se l'oggetto Criteri di gruppo non è aggiornato, potrebbero verificarsi conflitti di unione durante la replica che possono portare a una configurazione di Accesso remoto danneggiata  
  
-   Dopo aver modificato le impostazioni, attendere la replica delle modifiche nei controller di dominio associati agli oggetti Criteri di gruppo. Non apportare altre modifiche con la Console di gestione Accesso remoto o con i cmdlet di PowerShell per Accesso remoto finché la replica non è completa. Se un oggetto Criteri di gruppo viene modificato in due controller di dominio prima che la replica sia completa, potrebbero verificarsi conflitti di unione che possono portare a una configurazione di Accesso remoto danneggiata.  
  
In alternativa, è possibile modificare l'impostazione predefinita dalla finestra di dialogo **Cambia controller di dominio** nella console Gestione Criteri di gruppo o con il cmdlet di Windows PowerShell **Open-NetGPO**, in modo che le modifiche si basino sul controller di dominio specificato.  
  
-   Per eseguire questa operazione nella console Gestione Criteri di gruppo, fare clic con il pulsante destro del mouse sul dominio o sul contenitore dei siti e fare clic su **Cambia controller di dominio**.  
  
-   Per eseguire questa operazione in Windows PowerShell, specificare il parametro **DomainController** per il cmdlet **Open-NetGPO**. Ad esempio, per abilitare i profili privati e pubblici in Windows Firewall in un oggetto Criteri di gruppo denominato domain1\DA_Server_GPO _Europe usando un controller di dominio europe-dc.corp.contoso.com, immettere quanto segue:  
  
    ```powershell
    $gpoSession = Open-NetGPO -PolicyStore "domain1\DA_Server_GPO _Europe" -DomainController "europe-dc.corp.contoso.com"  
    Set-NetFirewallProfile -GpoSession $gpoSession -Name @("Private","Public") -Enabled True  
    Save-NetGPO -GpoSession $gpoSession  
    ```  
  
### <a name="184-manage-remote-access-gpos-with-limited-permissions"></a>1.8.4 Gestire gli oggetti Criteri di gruppo di Accesso remoto con autorizzazioni limitate  
Per gestire una distribuzione di Accesso remoto, all'amministratore di Accesso remoto sono necessarie le autorizzazioni complete per gli oggetti Criteri di gruppo (autorizzazioni di sicurezza di lettura, modifica ed eliminazione) usati nella distribuzione. Ciò è richiesto perché la Console di gestione Accesso remoto e i moduli PowerShell di Accesso remoto leggono e scrivono la configurazione negli oggetti Criteri di gruppo di Accesso remoto (cioè, gli oggetti Criteri di gruppo del client, del server e del server applicazioni).  
  
In molte organizzazioni, l'amministratore di dominio responsabile delle operazioni con gli oggetti Criteri di gruppo non coincide con l'amministratore di Accesso remoto responsabile della configurazione di Accesso remoto. Queste organizzazioni possono avere dei criteri che limitano le autorizzazioni dell'amministratore di Accesso remoto sugli oggetti Criteri di gruppo nel dominio. All'amministratore di dominio può essere anche richiesto di esaminare la configurazione dei criteri prima di applicarla a tutti i computer nel dominio.  
  
Per gestire questi requisiti, l'amministratore di dominio deve creare due copie di ciascun oggetto Criteri di gruppo: gestione temporanea e produzione. All'amministratore di Accesso remoto vengono concesse le autorizzazioni complete sugli oggetti Criteri di gruppo di gestione temporanea. L'amministratore di Accesso remoto specifica gli oggetti Criteri di gruppo di gestione temporanea nella Console di gestione Accesso remoto e nei cmdlet di Windows PowerShell come oggetti Criteri di gruppo usati per la distribuzione di Accesso remoto. Ciò consente all'amministratore di Accesso remoto di leggere e modificare la configurazione di Accesso remoto come e quando richiesto.  
  
L'amministratore di dominio deve verificare che gli oggetti Criteri di gruppo di gestione temporanea non siano collegati ad alcun ambito di gestione nel dominio e che l'amministratore di Accesso remoto non abbia autorizzazioni per il collegamento agli oggetti Criteri di gruppo nel dominio. Ciò garantisce che le modifiche apportate dall'amministratore di Accesso remoto agli oggetti Criteri di gruppo di gestione temporanea non abbiano effetti sui computer nel dominio.  
  
L'amministratore di dominio collega gli oggetti Criteri di gruppo di produzione all'ambito di gestione richiesto e applica i filtri di sicurezza appropriati. Ciò garantisce che le modifiche a questi oggetti Criteri di gruppo siano applicate ai computer nel dominio (computer client, server DirectAccess e server applicazioni). L'amministratore di Accesso remoto non ha autorizzazioni sugli oggetti Criteri di gruppo di produzione.  
  
Quando vengono apportate modifiche agli oggetti Criteri di gruppo di gestione temporanea, l'amministratore di dominio può rivedere la configurazione dei criteri in questi oggetti Criteri di gruppo per verificare che soddisfi i requisiti di sicurezza nell'organizzazione. L'amministratore di dominio quindi esporta le impostazioni dagli oggetti Criteri di gruppo di gestione temporanea usando la funzionalità di backup e le importa negli oggetti Criteri di gruppo di produzione corrispondenti, che saranno applicati ai computer nel dominio.  
  
Questa configurazione viene illustrata nel seguente diagramma.  
  
![Gestire oggetti Criteri di gruppo di accesso remoto](../../../media/Step-1-Plan-the-DirectAccess-Infrastructure/DA_Plan_Advanced_Step1_GPOS.png)  
  
### <a name="185-recover-from-a-deleted-gpo"></a>1.8.5 Eseguire un ripristino da un oggetto Criteri di gruppo eliminato  
Se un oggetto Criteri di gruppo del client, del server DirectAccess o del server applicazioni è stato eliminato per errore e non è disponibile un backup, è necessario rimuovere le impostazioni di configurazione e riconfigurarle. Se è disponibile un backup sarà possibile ripristinare l'oggetto Criteri di gruppo.  
  
La Console di gestione Accesso remoto visualizza il seguente messaggio di errore: **Impossibile trovare l'oggetto Criteri di gruppo (nome oggetto Criteri di gruppo)** . Per rimuovere le impostazioni di configurazione eseguire la procedura seguente:  
  
1.  Eseguire il cmdlet di Windows PowerShell **Uninstall-remoteaccess**.  
  
2.  Aprire la Console di gestione Accesso remoto.  
  
3.  Verrà visualizzato un messaggio di errore che informa che l'oggetto Criteri di gruppo non è stato trovato. Fare clic su **rimuovere impostazioni di configurazione**. Al termine, verrà ripristinato lo stato non configurato del server.  
  
## <a name="next-steps"></a>Passaggi successivi  
  
-   [Passaggio 2: Pianificare le distribuzioni di DirectAccess](da-adv-plan-s2-deployments.md)  
  


