---
title: Passaggio 1 pianificare l'infrastruttura DirectAccess di base
description: Questo argomento fa parte della Guida distribuire un server DirectAccess singolo usando la procedura guidata di Introduzione per Windows Server 2016
manager: brianlic
ms.prod: windows-server
ms.technology: networking-da
ms.topic: article
ms.author: lizross
author: eross-msft
ms.openlocfilehash: 6bab4adbcf006556c73c96f403afe7538079e967
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80819344"
---
# <a name="step-1-plan-the-basic-directaccess-infrastructure"></a>Passaggio 1 pianificare l'infrastruttura DirectAccess di base
Il primo passaggio per una distribuzione di base di DirectAccess in un singolo server consiste nell'eseguire la pianificazione per l'infrastruttura necessaria per la distribuzione. In questo argomento vengono descritti i passaggi per la pianificazione dell'infrastruttura:  
  
|Attività|Descrizione|  
|----|--------|  
|Pianificare la topologia di rete e le impostazioni|Decidere dove collocare il server DirectAccess \(al perimetro o dietro una traduzione degli indirizzi di rete \(NAT\) dispositivo o firewall\)e pianificare gli indirizzi IP e il routing.|  
|Pianificare i requisiti del firewall|Pianificare DirectAccess attraverso i firewall periferici.|  
|Pianificare i requisiti dei certificati|DirectAccess può usare Kerberos o i certificati per l'autenticazione client. In questa distribuzione di base di DirectAccess, un proxy Kerberos viene configurato automaticamente e l'autenticazione viene eseguita con le credenziali Active Directory.|  
|Pianificare i requisiti DNS|Pianificare le impostazioni DNS per il server DirectAccess, i server dell'infrastruttura e la connettività client.|  
|Pianificare Active Directory|Pianificare i requisiti dei controller di dominio e di Active Directory.|  
|Pianificare gli oggetti Criteri di gruppo|Decidere quali oggetti Criteri di gruppo sono necessari nell'organizzazione e come creare o modificare gli oggetti Criteri di gruppo.|  
  
L'esecuzione di queste attività di pianificazione non richiede un ordine specifico.  
  
## <a name="plan-network-topology-and-settings"></a><a name="bkmk_1_1_Network_svr_top_settings"></a>Pianificare la topologia di rete e le impostazioni  
  
### <a name="plan-network-adapters-and-ip-addressing"></a>Pianificare le schede di rete e gli indirizzi IP  
  
1.  Identificare la topologia delle schede di rete da usare. DirectAccess può essere impostato con una delle seguenti topologie:  
  
    -   Con due schede di rete, al perimetro con una scheda di rete connessa a Internet e l'altra alla rete interna, oppure dietro un dispositivo NAT, firewall o router, con una scheda di rete connessa a una rete perimetrale e l'altra alla rete interna rete.  
  
    -   Dietro un dispositivo NAT con una scheda di rete, il server DirectAccess viene installato dietro un dispositivo NAT e l'unica scheda di rete è connessa alla rete interna.  
  
2.  Identificare i requisiti degli indirizzi IP:  
  
    DirectAccess usa IPv6 con IPsec per creare una connessione sicura tra i computer client DirectAccess e la rete aziendale interna. Tuttavia, DirectAccess non richiede necessariamente la connettività a Internet IPv6 o il supporto IPv6 nativo nelle reti interne, Al contrario, configura e utilizza automaticamente le tecnologie di transizione IPv6 per eseguire il tunneling del traffico IPv6 attraverso la rete Internet IPv4 \(6to4, Teredo, IP\-HTTPS\) e nell'\-IPv4 solo Intranet \(NAT64 o ISATAP\). Per una panoramica di queste tecnologie di transizione, vedere le seguenti risorse:  
  
    -   [Tecnologie di transizione IPv6](https://technet.microsoft.com/library/bb726951.aspx)  
  
    -   [Specifica del protocollo di tunneling IP-HTTPS](https://msdn.microsoft.com/library/dd358571(PROT.10).aspx)  
  
3.  Configurare le schede e gli indirizzi necessari in base alla seguente tabella. Per le distribuzioni dietro un dispositivo NAT che usa una singola scheda di rete, configurare gli indirizzi IP usando solo la colonna della **scheda di rete interna** .  
  
    ||Scheda di rete esterna|Scheda di rete interna<sup>1</sup>|Requisiti di routing|  
    |-|--------------|--------------------|------------|  
    |Intranet IPv4 e Internet IPv4|Configurare le opzioni seguenti:<p>-Un indirizzo IPv4 pubblico statico con il subnet mask appropriato.<br />-Un indirizzo IPv4 del gateway predefinito del firewall Internet o del provider di servizi Internet locale \(router\) ISP.|Configurare le opzioni seguenti:<p>-Un indirizzo intranet IPv4 con subnet mask appropriata.<br />-Una connessione\-suffisso DNS specifico dello spazio dei nomi Intranet. È necessario configurare anche un server DNS nell'interfaccia interna.<br />-Non configurare un gateway predefinito in alcuna interfaccia Intranet.|Per configurare il server DirectAccess in modo che raggiunga tutte le subnet nella rete IPv4 interna, procedere come segue:<p>1. elencare gli spazi degli indirizzi IPv4 per tutti i percorsi nella Intranet.<br />2. usare i comandi **route add \-p** o **netsh interface IPv4 add route** per aggiungere gli spazi degli indirizzi IPv4 come route statiche nella tabella di routing IPv4 del server DirectAccess.|  
    |Internet IPv6 e Intranet IPv6|Configurare le opzioni seguenti:<p>-Usare la configurazione degli indirizzi configurata automaticamente fornita dall'ISP.<br />-Utilizzare il comando **Route Print** per assicurarsi che una route IPv6 predefinita che punta al router ISP esista nella tabella di routing IPv6.<br />-Determinare se i router ISP e Intranet utilizzano le preferenze del router predefinite descritte in RFC 4191 e utilizzando una preferenza predefinita superiore rispetto ai router Intranet locali. Se entrambe queste condizioni sono vere, non sono richieste altre configurazioni per la route predefinita. La preferenza superiore per il router ISP garantisce che la route IPv6 predefinita attiva del server DirectAccess punta alla rete Internet IPv6.<p>Poiché il server DirectAccess è un router IPv6, se si ha un'infrastruttura IPv6 nativa, anche l'interfaccia Internet può raggiungere i controller di dominio sulla Intranet. In questo caso, aggiungere filtri pacchetti al controller di dominio nella rete perimetrale per impedire la connettività all'indirizzo IPv6 dell'interfaccia Internet\-rivolte al server DirectAccess.|Configurare le opzioni seguenti:<p>-Se non si usano livelli di preferenza predefiniti, configurare le interfacce Intranet con il comando **netsh interface ipv6 set IndiceInterfaccia ignoredefaultroutes\=Enabled** . Il comando verifica che route predefinite aggiuntive che puntano ai router della Intranet non verranno aggiunte alla tabella di routing IPv6. È possibile ottenere l'elemento InterfaceIndex delle interfacce Intranet dalla visualizzazione del comando netsh interface show interface.|Se si ha una Intranet IPv6, per configurare il server DirectAccess affinché possa raggiungere tutti i percorsi IPv6, eseguire le operazioni seguenti:<p>1. elencare gli spazi degli indirizzi IPv6 per tutti i percorsi nella Intranet.<br />2. utilizzare il comando **netsh interface ipv6 add route** per aggiungere gli spazi degli indirizzi IPv6 come route statiche nella tabella di routing IPv6 del server DirectAccess.|  
    |Internet IPv4 e Intranet IPv6|Il server DirectAccess inoltra il traffico della route IPv6 predefinita usando l'interfaccia della scheda Microsoft 6to4 a un relay 6to4 nella rete Internet IPv4. È possibile configurare un server DirectAccess per l'indirizzo IPv4 del relè Microsoft 6to4 sulla rete Internet IPv4 \(usato quando IPv6 nativo non è distribuito nella rete aziendale\) con il comando seguente: netsh interface IPv6 6to4 set relay Name\=192.88.99.1 state\=Enabled Command.|||  
  
    > [!NOTE]  
    > Tenere presente quanto segue:  
    >   
    > 1.  Se al client DirectAccess è stato assegnato un indirizzo IPv4 pubblico, per connettersi alla Intranet viene usata la tecnologia di transizione 6to4. Se il client DirectAccess non è in grado di connettersi al server DirectAccess con 6to4, utilizzerà IP\-HTTPS.  
    > 2.  I computer client IPv6 nativi possono connettersi al server DirectAccess con un IPv6 nativo e non è richiesta alcuna tecnologia di transizione.  
  
### <a name="plan-firewall-requirements"></a><a name="ConfigFirewalls"></a>Pianificare i requisiti del firewall  
Se il server DirectAccess è protetto da un firewall periferico, sono necessarie le seguenti eccezioni per il traffico di DirectAccess quando il server DirectAccess si trova nella rete Internet IPv4:  
  
-   traffico 6to4: protocollo IP 41 in ingresso e in uscita.  
  
-   IP\-HTTPS-Transmission Control Protocol \(porta TCP\) destinazione 443 e porta TCP di origine 443 in uscita.  
  
-   Se si distribuisce DirectAccess con una sola scheda di rete e si installa il server dei percorsi di rete nel server DirectAccess, l'esenzione deve essere applicata anche alla porta TCP 62000.  
  
    > [!NOTE]  
    > Questa esenzione si trova nel server DirectAccess. Tutte le altre eccezioni si trovano nel firewall periferico.  
  
Le seguenti eccezioni sono necessarie per il traffico di DirectAccess quando il server DirectAccess si trova nella rete Internet IPv6:  
  
-   Protocollo IP 50  
  
-   Porta UDP di destinazione 500 in entrata e porta UDP di origine 500 in uscita.  
  
Quando si usano firewall aggiuntivi, applicare le seguenti eccezioni del firewall della rete interna per il traffico DirectAccess:  
  
-   ISATAP-protocollo 41 in ingresso e in uscita  
  
-   TCP\/UDP per tutto il traffico IPv4\/IPv6  
  
### <a name="plan-certificate-requirements"></a><a name="bkmk_1_2_CAs_and_certs"></a>Pianificare i requisiti dei certificati  
I requisiti dei certificati per IPsec includono un certificato del computer usato dai computer client DirectAccess quando stabiliscono la connessione IPsec tra il client e il server DirectAccess e un certificato del computer usato dai server DirectAccess per stabilire le connessioni IPsec con i client DirectAccess. Per DirectAccess in Windows Server 2012 R2 e Windows Server 2012, l'uso di questi certificati IPsec non è obbligatorio. Attività iniziali guidate consente di configurare il server DirectAccess in modo da fungere da proxy Kerberos per eseguire l'autenticazione IPsec senza necessità di certificati.
  
1.  **IP\-server HTTPS**. Quando si configura DirectAccess, il server DirectAccess viene configurato automaticamente per fungere da IP\-listener Web HTTPS. Il sito IP\-HTTPS richiede un certificato del sito Web e i computer client devono essere in grado di contattare l'elenco di revoche di certificati \(CRL\) sito per il certificato. L'Abilitazione guidata DirectAccess prova a usare il certificato VPN SSTP. Se SSTP non è configurato, verifica se un certificato per IP\-HTTPS è presente nell'archivio personale del computer. Se non è disponibile, viene creato automaticamente un certificato auto\-firmato.
  
2.  **Server dei percorsi di rete**. Il server dei percorsi di rete è un sito Web usato per rilevare se i computer client si trovano nella rete aziendale. Il server dei percorsi di rete richiede un certificato del sito Web. I client DirectAccess devono poter contattare il sito CRL per il certificato. L'abilitazione guidata accesso remoto controlla se un certificato per il server dei percorsi di rete è presente nell'archivio personale del computer. Se non è presente, viene creato automaticamente un certificato auto\-firmato.
  
I requisiti di certificazione per ogni scenario sono riepilogati nella seguente tabella:  
  
|Autenticazione IPsec|Server HTTPS IP\-|Server dei percorsi di rete|  
|------------|----------|--------------|  
|Una CA interna è necessaria per emettere certificati del computer per il server DirectAccess e i client per l'autenticazione IPsec quando non si utilizza il proxy Kerberos per l'autenticazione|CA pubblica: è consigliabile usare una CA pubblica per emettere il certificato IP\-HTTPS, in modo da garantire che il punto di distribuzione CRL sia disponibile esternamente.|CA interna: è possibile usare una CA interna per emettere il certificato del sito Web del server dei percorsi di rete. Verificare che il punto di distribuzione CRL sia a disponibilità elevata nella rete interna.|  
||CA interna: è possibile usare una CA interna per emettere il certificato IP\-HTTPS. Tuttavia, è necessario assicurarsi che il punto di distribuzione CRL sia disponibile esternamente.|Certificato autofirmato\-: è possibile usare un certificato auto\-firmato per il sito Web del server dei percorsi di rete. Tuttavia, non è possibile usare un certificato auto\-firmato nelle distribuzioni multisito.|  
||Certificato autofirmato\-: è possibile usare un certificato auto\-firmato per il server IP\-HTTPS. Tuttavia, è necessario assicurarsi che il punto di distribuzione CRL sia disponibile esternamente. Non è possibile usare un certificato auto\-firmato in una distribuzione multisito.||  
  
#### <a name="plan-certificates-for-ip-https-and-network-location-server"></a><a name="bkmk_website_cert_IPHTTPS"></a>Pianificare i certificati per IP\-HTTPS e il server dei percorsi di rete  
Per eseguire il provisioning di un certificato per questi scopi, fare riferimento a [Distribuire un server DirectAccess singolo con impostazioni avanzate](../single-server-advanced/Deploy-a-Single-DirectAccess-Server-with-Advanced-Settings.md). Se non sono disponibili certificati, la procedura guidata Introduzione crea automaticamente certificati autofirmati\-per questi scopi.
  
> [!NOTE]
> Se si effettua il provisioning dei certificati per IP\-HTTPS e il server dei percorsi di rete manualmente, verificare che i certificati abbiano un nome soggetto. Se il certificato non ha un nome soggetto, ma un nome alternativo, non sarà accettato dalla procedura guidata DirectAccess.  
  
#### <a name="plan-dns-requirements"></a>Pianificare i requisiti DNS  
In una distribuzione DirectAccess, DNS è richiesto per quanto segue:  
  
-   **Richieste del client DirectAccess**. DNS viene usato per risolvere le richieste provenienti dai computer client DirectAccess non presenti nella rete interna. I client DirectAccess tentano di connettersi al server dei percorsi di rete DirectAccess per determinare se si trovano su Internet o nella rete aziendale: se la connessione ha esito positivo, i client vengono determinati nell'Intranet e DirectAccess non viene usato e le richieste client vengono risolte usando il server DNS configurato nella scheda di rete del computer client. Se la connessione non riesce, si presuppone che i client si trovino sulla rete Internet. I client DirectAccess utilizzeranno la tabella dei criteri di risoluzione dei nomi \(tabella dei criteri di risoluzione dei nomi\) per determinare quale server DNS utilizzare per la risoluzione delle richieste di nomi. È possibile specificare che i client debbano usare DNS64 DirectAccess per risolvere i nomi o un server DNS interno alternativo. Quando si esegue la risoluzione dei nomi, la tabella dei criteri di risoluzione dei nomi viene usata dai client DirectAccess per stabilire come gestire una richiesta. I client richiedono un nome di dominio completo o un nome di etichetta singolo\-, ad esempio http:\/\/interno. Se un singolo nome di\-etichetta è richieste, viene aggiunto un suffisso DNS per creare un FQDN. Se la query DNS corrisponde a una voce nella tabella dei criteri di risoluzione dei nomi ed è specificato DNS4 o un server DNS Intranet per la voce, la query viene inviata per la risoluzione dei nomi attraverso il server specificato. Se esiste una corrispondenza, ma non sono specificati server DNS, si è in presenza di una regola di esenzione e viene applicata la normale risoluzione dei nomi.  
  
    Quando viene aggiunto un nuovo suffisso alla tabella dei criteri di risoluzione dei nomi nella console di gestione DirectAccess, i server DNS predefiniti per il suffisso possono essere rilevati automaticamente facendo clic su **Rileva**. Il rilevamento automatico funziona in questo modo:  
  
    1.  Se la rete aziendale è basata su IPv4\-o IPv4 e IPv6, l'indirizzo predefinito è l'indirizzo DNS64 della scheda interna nel server DirectAccess.  
  
    2.  Se la rete aziendale è basata su IPv6\-, l'indirizzo predefinito è l'indirizzo IPv6 dei server DNS nella rete aziendale.  
  
-   **Server di infrastruttura**
  
    1.  **Server dei percorsi di rete**. I client DirectAccess tentano di raggiungere il server dei percorsi di rete per determinare se si trovano sulla rete interna. I client nella rete interna devono poter risolvere il nome del server dei percorsi di rete, ma non quando si trovano sulla rete Internet. Per verificarlo, il nome di dominio completo del server dei percorsi di rete viene aggiunto per impostazione predefinita come regola di esenzione nella tabella dei criteri di risoluzione dei nomi. Inoltre, quando si configura DirectAccess, vengono create automaticamente le seguenti regole:  
  
        1.  Una regola per il suffisso DNS per il dominio radice oppure il nome dominio del server DirectAccess e gli indirizzi IPv6 corrispondenti ai server DNS Intranet configurati nel server DirectAccess. Ad esempio, se il server DirectAccess è membro del dominio corp.contoso.com, viene creata una regola per il suffisso DNS corp.contoso.com.  
  
        2.  Una regola di esenzione per il nome di dominio completo del server del percorso di rete. Ad esempio, se l'URL del server dei percorsi di rete è https:\/\/nls.corp.contoso.com, viene creata una regola di esenzione per il nome di dominio completo nls.corp.contoso.com.  
  
        **IP\-server HTTPS**. Il server DirectAccess funge da listener IP\-HTTPS e usa il certificato del server per autenticarsi nei client IP\-HTTPS. Il nome HTTPS del\-IP deve essere risolvibile dai client DirectAccess che usano i server DNS pubblici.  
  
        **Strumenti di verifica della connettività**. DirectAccess crea un probe Web predefinito usato dai computer client DirectAccess per verificare la connettività alla rete interna. Per verificare il corretto funzionamento del probe, è necessario registrare manualmente i seguenti nomi in DNS:  
  
        1.  DirectAccess\-webprobehost-deve essere risolto nell'indirizzo IPv4 interno del server DirectAccess o nell'indirizzo IPv6 in un ambiente IPv6\-solo.  
  
        2.  DirectAccess\-corpconnectivityhost-deve essere risolto nell'indirizzo localhost \(\) di loopback. Devono essere creati i record A e AAAA, il primo col valore 127.0.0.1 e il secondo con il valore costruito a partire dal prefisso NAT64 con 127.0.0.1 come ultimi 32 bit. Il prefisso NAT64 può essere recuperato eseguendo il cmdlet Get\-netnattransitionconfiguration.  
  
        È possibile creare altri strumenti di verifica della connettività con altri indirizzi Web su HTTP oppure PING. Per ogni strumento di verifica della connettività deve esistere una voce DNS.  
  
#### <a name="dns-server-requirements"></a>Requisiti del server DNS  
  
-   Per i client DirectAccess, è necessario usare un server DNS che esegue Windows Server 2008, Windows Server 2008 R2, Windows Server 2012, Windows Server 2012 R2, Windows Server 2016 o qualsiasi server DNS che supporti IPv6.  
  
> [!NOTE]  
> Non è consigliabile usare server DNS che eseguono Windows Server 2003 quando si distribuisce DirectAccess. Sebbene i server DNS di Windows Server 2003 supportino i record IPv6, Windows Server 2003 non è più supportato da Microsoft. Inoltre, DirectAccess non deve essere distribuito se i controller di dominio eseguono Windows Server 2003 a causa di un problema con il servizio Replica file. Per ulteriori informazioni, vedere [DirectAccess configurazioni non supportate](../DirectAccess-Unsupported-Configurations.md).  
  
### <a name="plan-the-network-location-server"></a><a name="bkmk_1_4_NLS"></a>Pianificare il server dei percorsi di rete  
Il server dei percorsi di rete è un sito Web usato per rilevare se i client DirectAccess si trovano nella rete aziendale. I client nella rete aziendale non usano DirectAccess per raggiungere le risorse interne, ma vi si connettono direttamente.  
  
Attività iniziali guidate imposta automaticamente il server dei percorsi di rete nel server DirectAccess e il sito Web viene creato automaticamente quando si distribuisce DirectAccess. Ciò consente un'installazione semplice senza dover usare un'infrastruttura di certificati.
  
Se si vuole distribuire un server dei percorsi di rete senza usare i certificati auto\-firmati, fare riferimento a [distribuire un server DirectAccess singolo con impostazioni avanzate](../single-server-advanced/Deploy-a-Single-DirectAccess-Server-with-Advanced-Settings.md).
  
### <a name="plan-active-directory"></a><a name="bkmk_1_6_AD"></a>Pianificare Active Directory  
DirectAccess usa Active Directory e Active Directory oggetti Criteri di gruppo come segue:
  
-   **Autenticazione**. Active Directory viene usato per l'autenticazione. Il tunnel DirectAccess usa l'autenticazione Kerberos in modo che l'utente possa accedere alle risorse interne.
  
-   **Oggetti Criteri di gruppo**. DirectAccess raccoglie le impostazioni di configurazione negli oggetti Criteri di gruppo applicati ai server e ai client DirectAccess.
  
-   **Gruppi di sicurezza**. DirectAccess usa i gruppi di sicurezza per raccogliere e identificare i computer client e i server DirectAccess. I criteri di gruppo vengono applicati al gruppo di sicurezza richiesto.

**Requisiti di Active Directory**  
  
Durante la pianificazione di Active Directory per una distribuzione di DirectAccess, è richiesto quanto segue:  
  
-   Almeno un controller di dominio installato in Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2 o Windows Server 2008. 
  
    Se il controller di dominio si trova su una rete perimetrale \(e pertanto raggiungibile dalla scheda di rete\-Internet del server DirectAccess\) impedire al server DirectAccess di raggiungerlo aggiungendo filtri pacchetti sul controller di dominio, per impedire la connettività all'indirizzo IP della scheda Internet.  
  
-   Il server DirectAccess deve essere membro di un dominio.  
  
-   I client DirectAccess devono essere membri di un dominio. I client possono appartenere a:  
  
    -   Qualsiasi dominio nella stessa foresta del server DirectAccess.  
  
    -   Qualsiasi dominio che abbia una relazione di trust tra due\-con il dominio del server DirectAccess.  
  
    -   Qualsiasi dominio in una foresta che disponga di due\-modo di considerare attendibile la foresta a cui appartiene il dominio DirectAccess.  
  
> [!NOTE]
> - Il server DirectAccess non può essere un controller di dominio.  
> - Il controller di dominio Active Directory usato per DirectAccess non deve essere raggiungibile dalla scheda Internet esterna del server DirectAccess \(la scheda non deve trovarsi nel profilo di dominio di Windows Firewall\).  
  
### <a name="plan-group-policy-objects"></a><a name="bkmk_1_7_GPOs"></a>Pianificare oggetti Criteri di gruppo  
Le impostazioni di DirectAccess configurate quando si configura DirectAccess vengono raccolte in oggetti Criteri di gruppo \(oggetto Criteri di gruppo\). Due diversi oggetti Criteri di gruppo vengono popolati con impostazioni DirectAccess e distribuiti come segue:  
  
-   **Oggetto Criteri di gruppo del client DirectAccess**. Questo oggetto Criteri di gruppo contiene le impostazioni del client, incluse le impostazioni per la tecnologia di transizione IPv6, le voci NRPT e le regole di sicurezza della connessione di Windows Firewall con sicurezza avanzata. L'oggetto Criteri di gruppo viene applicato ai gruppi di sicurezza specificati per i computer client.  
  
-   **Oggetto Criteri di gruppo del server DirectAccess**. Questo oggetto Criteri di gruppo contiene le impostazioni di configurazione DirectAccess applicate a qualsiasi server configurato come server DirectAccess nella distribuzione. Contiene anche le regole di sicurezza della connessione di Windows Firewall con sicurezza avanzata.  
  
È possibile configurare gli oggetti Criteri di gruppo in due modi:  
  
1.  **Automaticamente**. Per specificare la creazione automatica. Per ogni oggetto Criteri di gruppo viene specificato un nome predefinito. Gli oggetti Criteri di gruppo vengono creati automaticamente da Attività iniziali guidate.
  
2.  **Manualmente**. È possibile usare gli oggetti Criteri di gruppo predefiniti dall'amministratore di Active Directory.
  
Dopo aver configurato DirectAccess in modo da usare specifici oggetti Criteri di gruppo non sarà possibile configurarlo in modo da usarne altri.
  
> [!IMPORTANT]
> Se si usano oggetti Criteri di gruppo configurati automaticamente o manualmente, è necessario aggiungere un criterio per il rilevamento dei collegamenti lenti se i client usano 3G. Il percorso di Criteri di gruppo per **criterio: configura criteri di gruppo rilevamento collegamento lento** è: **Configurazione Computer \/ criteri \/ modelli amministrativi \/ sistema**\/ criteri di gruppo.  
  
> [!CAUTION]  
> Usare la procedura seguente per eseguire il backup di tutti gli oggetti Criteri di gruppo di DirectAccess prima di eseguire i cmdlet di DirectAccess: [backup e ripristino della configurazione di DirectAccess](https://go.microsoft.com/fwlink/?LinkID=257928)  
  
#### <a name="automatically-created-gpos"></a>Oggetti Criteri di gruppo creati automaticamente\-  
Tenere presente quanto segue quando si usano gli oggetti Criteri di gruppo\-creati automaticamente:  
  
Gli oggetti Criteri di gruppo creati automaticamente vengono applicati secondo i parametri di percorso e destinazione collegamento, come segue:  
  
-   Per l'oggetto Criteri di gruppo del server DirectAccess, entrambi i parametri di percorso e collegamento puntano al dominio contenente il server DirectAccess.  
  
-   Quando si creano oggetti Criteri di gruppo del client, il percorso viene impostato su un unico dominio in cui verrà creato l'oggetto Criteri di gruppo. Il nome dell'oggetto Criteri di gruppo viene cercato in tutti i domini e, se esistente, viene compilato con le impostazioni DirectAccess. La destinazione collegamento viene impostata sulla radice del dominio in cui è stato creato l'oggetto Criteri di gruppo. Viene creato un oggetto Criteri di gruppo per ogni dominio che contiene computer client, che sarà quindi collegato alla radice del rispettivo dominio.  
  
Quando si usano oggetti Criteri di gruppo creati automaticamente, all'amministratore del server DirectAccess saranno necessarie le seguenti autorizzazioni per applicare le impostazioni di DirectAccess:  
  
-   Autorizzazioni di creazione oggetto Criteri di gruppo per ogni dominio.  
  
-   Autorizzazioni per il collegamento di tutte le radici del dominio client selezionate.  
  
-   Autorizzazioni per il collegamento delle radici del dominio oggetto Criteri di gruppo del server.  
  
-   Le autorizzazioni di sicurezza di creazione, modifica ed eliminazione sono necessarie per gli oggetti Criteri di gruppo.  
  
-   È consigliabile che l'amministratore DirectAccess disponga delle autorizzazioni di lettura dell'oggetto Criteri di gruppo per ogni dominio richiesto. Ciò consente a DirectAccess di verificare che non esistano oggetti Criteri di gruppo con nomi duplicati durante la creazione degli oggetti Criteri di gruppo.  
  
Se non esistono le autorizzazioni corrette per il collegamento degli oggetti Criteri di gruppo, viene emesso un avviso. L'operazione di DirectAccess continua, ma il collegamento non verrà eseguito. Se viene emesso questo avviso non verranno creati automaticamente i collegamenti, nemmeno dopo che saranno state aggiunte le autorizzazioni. L'amministratore dovrà invece creare i collegamenti manualmente.  
  
#### <a name="manually-created-gpos"></a>Oggetti Criteri di gruppo creati manualmente\-  
Quando si utilizza manualmente\-oggetti Criteri di gruppo creati, tenere presente quanto segue:  
  
-   Gli oggetti Criteri di gruppo devono essere presenti prima di eseguire Attività iniziali guidate di Accesso remoto.  
  
-   Quando si utilizza manualmente\-oggetti Criteri di gruppo creati, per applicare le impostazioni di DirectAccess l'amministratore DirectAccess richiede autorizzazioni complete per gli oggetti Criteri di gruppo \(modificare, eliminare, modificare la sicurezza\) sugli oggetti Criteri di gruppo creati manualmente\-.  
  
-   Quando si usano oggetti Criteri di gruppo creati manualmente viene eseguita una ricerca di un collegamento all'oggetto Criteri di gruppo nell'intero dominio. Se l'oggetto Criteri di gruppo non è collegato nel dominio, verrà automaticamente creato un collegamento nel dominio radice. Se le autorizzazioni richieste per creare il collegamento non sono disponibili, verrà emesso un avviso.  
  
Se non esistono le autorizzazioni corrette per il collegamento degli oggetti Criteri di gruppo, viene emesso un avviso. L'operazione di DirectAccess continua, ma il collegamento non verrà eseguito. Se viene emesso questo avviso non verranno creati automaticamente i collegamenti, nemmeno dopo che saranno state aggiunte le autorizzazioni. L'amministratore dovrà invece creare i collegamenti manualmente.  
  
#### <a name="recovering-from-a-deleted-gpo"></a>Ripristino da un oggetto Criteri di gruppo eliminato  
Se un server DirectAccess, un client o un oggetto Criteri di gruppo del server applicazioni è stato eliminato per errore e non è disponibile alcun backup, è necessario rimuovere le impostazioni di configurazione e ri\-configurare nuovamente. Se è disponibile un backup sarà possibile ripristinare l'oggetto Criteri di gruppo.  
  
**Gestione DirectAccess** visualizzerà il messaggio di errore seguente: **Impossibile trovare l'oggetto criteri**di gruppo <GPO name>. Per rimuovere le impostazioni di configurazione eseguire la procedura seguente:  
  
1.  Eseguire il cmdlet di PowerShell **Uninstall\-RemoteAccess**.  
  
2.  Ri\-aprire **Gestione DirectAccess**.  
  
3.  Verrà visualizzato un messaggio di errore che informa che l'oggetto Criteri di gruppo non è stato trovato. Fare clic su **rimuovere impostazioni di configurazione**. Al termine, il server verrà ripristinato in uno stato di\-configurato.  
  
### <a name="next-step"></a><a name="BKMK_Links"></a>Passaggio successivo  
  
-   [Passaggio 2: pianificare la distribuzione di base di DirectAccess](da-basic-plan-s2-deployment.md)  
  
