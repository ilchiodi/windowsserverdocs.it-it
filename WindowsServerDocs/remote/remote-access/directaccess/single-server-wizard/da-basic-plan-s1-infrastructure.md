---
title: Passaggio 1 pianificare l'infrastruttura DirectAccess di base
description: Questo argomento fa parte della Guida distribuire un server DirectAccess singolo usando la procedura guidata di Introduzione per Windows Server 2016
manager: brianlic
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-da
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: ''
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 6f4c727dc8f7905502d47119bd0e911537e827aa
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71404876"
---
# <a name="step-1-plan-the-basic-directaccess-infrastructure"></a>Passaggio 1 pianificare l'infrastruttura DirectAccess di base
Il primo passaggio per una distribuzione di base di DirectAccess in un singolo server consiste nell'eseguire la pianificazione per l'infrastruttura necessaria per la distribuzione. In questo argomento vengono descritti i passaggi per la pianificazione dell'infrastruttura:  
  
|Attività|Descrizione|  
|----|--------|  
|Pianificare la topologia di rete e le impostazioni|Decidere dove collocare il server DirectAccess \(AT The Edge o dietro a Network Address Translation \(NAT @ no__t-2 device o firewall @ no__t-3 e pianificare gli indirizzi IP e il routing.|  
|Pianificare i requisiti del firewall|Pianificare DirectAccess attraverso i firewall periferici.|  
|Pianificare i requisiti dei certificati|DirectAccess può usare Kerberos o i certificati per l'autenticazione client. In questa distribuzione di base di DirectAccess, un proxy Kerberos viene configurato automaticamente e l'autenticazione viene eseguita con le credenziali Active Directory.|  
|Pianificare i requisiti DNS|Pianificare le impostazioni DNS per il server DirectAccess, i server dell'infrastruttura e la connettività client.|  
|Pianificare Active Directory|Pianificare i requisiti dei controller di dominio e di Active Directory.|  
|Pianificare gli oggetti Criteri di gruppo|Decidere quali oggetti Criteri di gruppo sono necessari nell'organizzazione e come creare o modificare gli oggetti Criteri di gruppo.|  
  
L'esecuzione di queste attività di pianificazione non richiede un ordine specifico.  
  
## <a name="bkmk_1_1_Network_svr_top_settings"></a>Pianificare la topologia di rete e le impostazioni  
  
### <a name="plan-network-adapters-and-ip-addressing"></a>Pianificare le schede di rete e gli indirizzi IP  
  
1.  Identificare la topologia delle schede di rete da usare. DirectAccess può essere impostato con una delle seguenti topologie:  
  
    -   Con due schede di rete, al perimetro con una scheda di rete connessa a Internet e l'altra alla rete interna, oppure dietro un dispositivo NAT, firewall o router, con una scheda di rete connessa a una rete perimetrale e l'altra alla rete interna rete.  
  
    -   Dietro un dispositivo NAT con una scheda di rete, il server DirectAccess viene installato dietro un dispositivo NAT e l'unica scheda di rete è connessa alla rete interna.  
  
2.  Identificare i requisiti degli indirizzi IP:  
  
    DirectAccess usa IPv6 con IPsec per creare una connessione sicura tra i computer client DirectAccess e la rete aziendale interna. Tuttavia, DirectAccess non richiede necessariamente la connettività a Internet IPv6 o il supporto IPv6 nativo nelle reti interne, Al contrario, configura e usa automaticamente le tecnologie di transizione IPv6 per eseguire il tunneling del traffico IPv6 attraverso la rete Internet IPv4 \(6to4, Teredo, IP @ no__t-1HTTPS @ no__t-2 e nell'Intranet IPv4 @ no__t-3only \(NAT64 o ISATAP @ no__t-5. Per una panoramica di queste tecnologie di transizione, vedere le seguenti risorse:  
  
    -   [Tecnologie di transizione IPv6](https://technet.microsoft.com/library/bb726951.aspx)  
  
    -   [Specifica del protocollo di tunneling IP-HTTPS](https://msdn.microsoft.com/library/dd358571(PROT.10).aspx)  
  
3.  Configurare le schede e gli indirizzi necessari in base alla seguente tabella. Per le distribuzioni dietro un dispositivo NAT che usa una singola scheda di rete, configurare gli indirizzi IP usando solo la colonna della **scheda di rete interna** .  
  
    ||Scheda di rete esterna|Scheda di rete interna<sup>1</sup>|Requisiti di routing|  
    |-|--------------|--------------------|------------|  
    |Intranet IPv4 e Internet IPv4|Configurare quanto segue:<br /><br />-Un indirizzo IPv4 pubblico statico con il subnet mask appropriato.<br />-Un indirizzo IPv4 del gateway predefinito del firewall Internet o del provider di servizi Internet locale \(ISP @ no__t-1 router.|Configurare quanto segue:<br /><br />-Un indirizzo intranet IPv4 con subnet mask appropriata.<br />-Un suffisso DNS per la connessione @ no__t-0specific dello spazio dei nomi Intranet. È necessario configurare anche un server DNS nell'interfaccia interna.<br />-Non configurare un gateway predefinito in alcuna interfaccia Intranet.|Per configurare il server DirectAccess in modo che raggiunga tutte le subnet nella rete IPv4 interna, procedere come segue:<br /><br />1.  Elencare gli spazi degli indirizzi IPv4 per tutti i percorsi nella Intranet.<br />2.  Usare il comando **route add \-P** o **netsh interface IPv4 add route** per aggiungere gli spazi degli indirizzi IPv4 come route statiche nella tabella di routing IPv4 del server DirectAccess.|  
    |Internet IPv6 e Intranet IPv6|Configurare quanto segue:<br /><br />-Usare la configurazione degli indirizzi configurata automaticamente fornita dall'ISP.<br />-Utilizzare il comando **Route Print** per assicurarsi che una route IPv6 predefinita che punta al router ISP esista nella tabella di routing IPv6.<br />-Determinare se i router ISP e Intranet utilizzano le preferenze del router predefinite descritte in RFC 4191 e utilizzando una preferenza predefinita superiore rispetto ai router Intranet locali. Se entrambe queste condizioni sono vere, non sono richieste altre configurazioni per la route predefinita. La preferenza superiore per il router ISP garantisce che la route IPv6 predefinita attiva del server DirectAccess punta alla rete Internet IPv6.<br /><br />Poiché il server DirectAccess è un router IPv6, se si ha un'infrastruttura IPv6 nativa, anche l'interfaccia Internet può raggiungere i controller di dominio sulla Intranet. In questo caso, aggiungere filtri pacchetti al controller di dominio nella rete perimetrale per impedire la connettività all'indirizzo IPv6 dell'interfaccia Internet @ no__t-0facing del server DirectAccess.|Configurare quanto segue:<br /><br />-Se non si utilizzano i livelli di preferenza predefiniti, configurare le interfacce Intranet con il comando **netsh interface ipv6 set IndiceInterfaccia ignoredefaultroutes @ no__t-1enabled** . Il comando verifica che route predefinite aggiuntive che puntano ai router della Intranet non verranno aggiunte alla tabella di routing IPv6. È possibile ottenere l'elemento InterfaceIndex delle interfacce Intranet dalla visualizzazione del comando netsh interface show interface.|Se si ha una Intranet IPv6, per configurare il server DirectAccess affinché possa raggiungere tutti i percorsi IPv6, eseguire le operazioni seguenti:<br /><br />1.  Elencare gli spazi degli indirizzi IPv6 per tutti i percorsi nella Intranet.<br />2.  Usare il comando **netsh interface ipv6 add route** per aggiungere gli spazi degli indirizzi IPv6 come route statiche nella tabella di routing IPv6 del server DirectAccess.|  
    |Internet IPv4 e Intranet IPv6|Il server DirectAccess inoltra il traffico della route IPv6 predefinita usando l'interfaccia della scheda Microsoft 6to4 a un relay 6to4 nella rete Internet IPv4. È possibile configurare un server DirectAccess per l'indirizzo IPv4 del relè Microsoft 6to4 nella rete Internet IPv4 \(used quando il protocollo IPv6 nativo non è distribuito nella rete aziendale @ no__t-1 con il comando seguente: netsh interface IPv6 6to4 set relay Name @ no__ t-2192.88.99.1 stato @ no__t-comando 3enabled.|||  
  
    > [!NOTE]  
    > Tenere presente quanto segue:  
    >   
    > 1.  Se al client DirectAccess è stato assegnato un indirizzo IPv4 pubblico, per connettersi alla Intranet viene usata la tecnologia di transizione 6to4. Se il client DirectAccess non è in grado di connettersi al server DirectAccess con 6to4, utilizzerà IP @ no__t-0HTTPS.  
    > 2.  I computer client IPv6 nativi possono connettersi al server DirectAccess con un IPv6 nativo e non è richiesta alcuna tecnologia di transizione.  
  
### <a name="ConfigFirewalls"></a>Pianificare i requisiti del firewall  
Se il server DirectAccess è protetto da un firewall periferico, sono necessarie le seguenti eccezioni per il traffico di DirectAccess quando il server DirectAccess si trova nella rete Internet IPv4:  
  
-   traffico 6to4: protocollo IP 41 in ingresso e in uscita.  
  
-   IP @ no__t-0HTTPS-Transmission Control Protocol \(TCP @ no__t-2 porta di destinazione 443 e porta TCP di origine 443 in uscita.  
  
-   Se si distribuisce DirectAccess con una sola scheda di rete e si installa il server dei percorsi di rete nel server DirectAccess, l'esenzione deve essere applicata anche alla porta TCP 62000.  
  
    > [!NOTE]  
    > Questa esenzione si trova nel server DirectAccess. Tutte le altre eccezioni si trovano nel firewall periferico.  
  
Le seguenti eccezioni sono necessarie per il traffico di DirectAccess quando il server DirectAccess si trova nella rete Internet IPv6:  
  
-   Protocollo IP 50  
  
-   Porta UDP di destinazione 500 in entrata e porta UDP di origine 500 in uscita.  
  
Quando si usano firewall aggiuntivi, applicare le seguenti eccezioni del firewall della rete interna per il traffico DirectAccess:  
  
-   ISATAP-protocollo 41 in ingresso e in uscita  
  
-   TCP @ no__t-0UDP per tutto il traffico IPv4 @ no__t-1IPv6  
  
### <a name="bkmk_1_2_CAs_and_certs"></a>Pianificare i requisiti dei certificati  
I requisiti dei certificati per IPsec includono un certificato del computer usato dai computer client DirectAccess quando stabiliscono la connessione IPsec tra il client e il server DirectAccess e un certificato del computer usato dai server DirectAccess per stabilire le connessioni IPsec con i client DirectAccess. Per DirectAccess in Windows Server 2012 R2 e Windows Server 2012, l'uso di questi certificati IPsec non è obbligatorio. Attività iniziali guidate consente di configurare il server DirectAccess in modo da fungere da proxy Kerberos per eseguire l'autenticazione IPsec senza necessità di certificati.
  
1.  **IP @ no__t-server 1HTTPS**. Quando si configura DirectAccess, il server DirectAccess viene configurato automaticamente per fungere da listener Web IP @ no__t-0HTTPS. Il sito IP @ no__t-0HTTPS richiede un certificato del sito Web e i computer client devono essere in grado di contattare il sito dell'elenco di revoche di certificati \(CRL @ no__t-2 per il certificato. L'Abilitazione guidata DirectAccess prova a usare il certificato VPN SSTP. Se SSTP non è configurato, verifica se un certificato per IP @ no__t-0HTTPS è presente nell'archivio personale del computer. Se non è disponibile, viene creato automaticamente un certificato self @ no__t-0signed.
  
2.  **Server dei percorsi di rete**. Il server dei percorsi di rete è un sito Web usato per rilevare se i computer client si trovano nella rete aziendale. Il server dei percorsi di rete richiede un certificato del sito Web. I client DirectAccess devono poter contattare il sito CRL per il certificato. L'abilitazione guidata accesso remoto controlla se un certificato per il server dei percorsi di rete è presente nell'archivio personale del computer. Se non è presente, viene creato automaticamente un certificato self @ no__t-0signed.
  
I requisiti di certificazione per ogni scenario sono riepilogati nella seguente tabella:  
  
|Autenticazione IPsec|IP @ no__t-server 0HTTPS|Server dei percorsi di rete|  
|------------|----------|--------------|  
|Una CA interna è necessaria per emettere certificati del computer per il server DirectAccess e i client per l'autenticazione IPsec quando non si utilizza il proxy Kerberos per l'autenticazione|CA pubblica: è consigliabile usare una CA pubblica per emettere il certificato IP @ no__t-0HTTPS, in modo da garantire che il punto di distribuzione CRL sia disponibile esternamente.|CA interna: è possibile usare una CA interna per emettere il certificato del sito Web del server dei percorsi di rete. Verificare che il punto di distribuzione CRL sia a disponibilità elevata nella rete interna.|  
||CA interna: è possibile usare una CA interna per emettere il certificato IP @ no__t-0HTTPS. Tuttavia, è necessario assicurarsi che il punto di distribuzione CRL sia disponibile esternamente.|Self @ no__t-0signed certificate: è possibile usare un certificato self @ no__t-1signed per il sito Web del server dei percorsi di rete; Tuttavia, non è possibile usare un certificato self @ no__t-2signed nelle distribuzioni multisito.|  
||Self @ no__t-0signed certificate: è possibile usare un certificato self @ no__t-1signed per il server IP @ no__t-2HTTPS; Tuttavia, è necessario assicurarsi che il punto di distribuzione CRL sia disponibile esternamente. Non è possibile usare un certificato self @ no__t-0signed in una distribuzione multisito.||  
  
#### <a name="bkmk_website_cert_IPHTTPS"></a>Pianificare i certificati per IP @ no__t-1HTTPS e il server dei percorsi di rete  
Per eseguire il provisioning di un certificato per questi scopi, fare riferimento a [Distribuire un server DirectAccess singolo con impostazioni avanzate](../single-server-advanced/Deploy-a-Single-DirectAccess-Server-with-Advanced-Settings.md). Se non sono disponibili certificati, la procedura guidata Introduzione crea automaticamente i certificati self @ no__t-0signed per questi scopi.
  
> [!NOTE]
> Se si esegue il provisioning dei certificati per IP @ no__t-0HTTPS e il server dei percorsi di rete manualmente, verificare che i certificati abbiano un nome soggetto. Se il certificato non ha un nome soggetto, ma un nome alternativo, non sarà accettato dalla procedura guidata DirectAccess.  
  
#### <a name="plan-dns-requirements"></a>Pianificare i requisiti DNS  
In una distribuzione DirectAccess, DNS è richiesto per quanto segue:  
  
-   **Richieste del client DirectAccess**. DNS viene usato per risolvere le richieste provenienti dai computer client DirectAccess non presenti nella rete interna. I client DirectAccess provano a connettersi al server dei percorsi di rete DirectAccess per determinare se si trovano sulla rete Internet o su quella aziendale: Se la connessione riesce, viene rilevato che i client si trovano sulla rete Intranet, DirectAccess non viene usato e le richieste dei client vengono risolte usando il server DNS configurato nella scheda di rete del computer client. Se la connessione non riesce, si presuppone che i client si trovino sulla rete Internet. I client DirectAccess utilizzeranno la tabella dei criteri di risoluzione dei nomi \(NRPT @ no__t-1 per determinare quale server DNS utilizzare per la risoluzione delle richieste di nomi. È possibile specificare che i client debbano usare DNS64 DirectAccess per risolvere i nomi o un server DNS interno alternativo. Quando si esegue la risoluzione dei nomi, la tabella dei criteri di risoluzione dei nomi viene usata dai client DirectAccess per stabilire come gestire una richiesta. I client richiedono un nome di dominio completo o un nome Single @ no__t-0label, ad esempio http: \/ @ no__t-2Internal. Se un singolo nome @ no__t-0label è richiesto, viene aggiunto un suffisso DNS per creare un nome di dominio completo. Se la query DNS corrisponde a una voce nella tabella dei criteri di risoluzione dei nomi ed è specificato DNS4 o un server DNS Intranet per la voce, la query viene inviata per la risoluzione dei nomi attraverso il server specificato. Se esiste una corrispondenza, ma non sono specificati server DNS, si è in presenza di una regola di esenzione e viene applicata la normale risoluzione dei nomi.  
  
    Quando viene aggiunto un nuovo suffisso alla tabella dei criteri di risoluzione dei nomi nella console di gestione DirectAccess, i server DNS predefiniti per il suffisso possono essere rilevati automaticamente facendo clic su **Rileva**. Il rilevamento automatico funziona in questo modo:  
  
    1.  Se la rete aziendale è IPv4 @ no__t-0based o IPv4 e IPv6, l'indirizzo predefinito è l'indirizzo DNS64 della scheda interna nel server DirectAccess.  
  
    2.  Se la rete aziendale è IPv6 @ no__t-0based, l'indirizzo predefinito è l'indirizzo IPv6 dei server DNS nella rete aziendale.  
  
-   **Server di infrastruttura**
  
    1.  **Server dei percorsi di rete**. I client DirectAccess tentano di raggiungere il server dei percorsi di rete per determinare se si trovano sulla rete interna. I client nella rete interna devono poter risolvere il nome del server dei percorsi di rete, ma non quando si trovano sulla rete Internet. Per verificarlo, il nome di dominio completo del server dei percorsi di rete viene aggiunto per impostazione predefinita come regola di esenzione nella tabella dei criteri di risoluzione dei nomi. Inoltre, quando si configura DirectAccess, vengono create automaticamente le seguenti regole:  
  
        1.  Una regola per il suffisso DNS per il dominio radice oppure il nome dominio del server DirectAccess e gli indirizzi IPv6 corrispondenti ai server DNS Intranet configurati nel server DirectAccess. Ad esempio, se il server DirectAccess è membro del dominio corp.contoso.com, viene creata una regola per il suffisso DNS corp.contoso.com.  
  
        2.  Una regola di esenzione per il nome di dominio completo del server del percorso di rete. Ad esempio, se l'URL del server dei percorsi di rete è https: @no__t -0\/nls.corp.contoso.com, viene creata una regola di esenzione per il nome di dominio completo nls.corp.contoso.com.  
  
        **IP @ no__t-server 1HTTPS**. Il server DirectAccess funge da listener IP @ no__t-0HTTPS e usa il certificato del server per autenticarsi nei client IP @ no__t-1HTTPS. Il nome IP @ no__t-0HTTPS deve essere risolvibile dai client DirectAccess che usano i server DNS pubblici.  
  
        **Strumenti di verifica della connettività**. DirectAccess crea un probe Web predefinito usato dai computer client DirectAccess per verificare la connettività alla rete interna. Per verificare il corretto funzionamento del probe, è necessario registrare manualmente i seguenti nomi in DNS:  
  
        1.  DirectAccess @ no__t-0webprobehost-deve essere risolto nell'indirizzo IPv4 interno del server DirectAccess o nell'indirizzo IPv6 in un ambiente IPv6 @ no__t-1Solo.  
  
        2.  DirectAccess @ no__t-0corpconnectivityhost-deve essere risolto nell'indirizzo localhost \(loopback @ no__t-2. Devono essere creati i record A e AAAA, il primo col valore 127.0.0.1 e il secondo con il valore costruito a partire dal prefisso NAT64 con 127.0.0.1 come ultimi 32 bit. Il prefisso NAT64 può essere recuperato eseguendo il cmdlet Get @ no__t-0netnattransitionconfiguration.  
  
        È possibile creare altri strumenti di verifica della connettività con altri indirizzi Web su HTTP oppure PING. Per ogni strumento di verifica della connettività deve esistere una voce DNS.  
  
#### <a name="dns-server-requirements"></a>Requisiti del server DNS  
  
-   Per i client DirectAccess, è necessario usare un server DNS che esegue Windows Server 2008, Windows Server 2008 R2, Windows Server 2012, Windows Server 2012 R2, Windows Server 2016 o qualsiasi server DNS che supporti IPv6.  
  
> [!NOTE]  
> Non è consigliabile usare server DNS che eseguono Windows Server 2003 quando si distribuisce DirectAccess. Sebbene i server DNS di Windows Server 2003 supportino i record IPv6, Windows Server 2003 non è più supportato da Microsoft. Inoltre, DirectAccess non deve essere distribuito se i controller di dominio eseguono Windows Server 2003 a causa di un problema con il servizio Replica file. Per ulteriori informazioni, vedere [DirectAccess configurazioni non supportate](../DirectAccess-Unsupported-Configurations.md).  
  
### <a name="bkmk_1_4_NLS"></a>Pianificare il server dei percorsi di rete  
Il server dei percorsi di rete è un sito Web usato per rilevare se i client DirectAccess si trovano nella rete aziendale. I client nella rete aziendale non usano DirectAccess per raggiungere le risorse interne, ma vi si connettono direttamente.  
  
Attività iniziali guidate imposta automaticamente il server dei percorsi di rete nel server DirectAccess e il sito Web viene creato automaticamente quando si distribuisce DirectAccess. Ciò consente un'installazione semplice senza dover usare un'infrastruttura di certificati.
  
Se si vuole distribuire un server dei percorsi di rete senza usare i certificati self-no__t-0signed, vedere [distribuire un server DirectAccess singolo con impostazioni avanzate](../single-server-advanced/Deploy-a-Single-DirectAccess-Server-with-Advanced-Settings.md).
  
### <a name="bkmk_1_6_AD"></a>Pianificare Active Directory  
DirectAccess usa Active Directory e Active Directory oggetti Criteri di gruppo come segue:
  
-   **Autenticazione**. Active Directory viene usato per l'autenticazione. Il tunnel DirectAccess usa l'autenticazione Kerberos in modo che l'utente possa accedere alle risorse interne.
  
-   **Oggetti Criteri di gruppo**. DirectAccess raccoglie le impostazioni di configurazione negli oggetti Criteri di gruppo applicati ai server e ai client DirectAccess.
  
-   **Gruppi di sicurezza**. DirectAccess usa i gruppi di sicurezza per raccogliere e identificare i computer client e i server DirectAccess. I criteri di gruppo vengono applicati al gruppo di sicurezza richiesto.

**Requisiti di Active Directory**  
  
Durante la pianificazione di Active Directory per una distribuzione di DirectAccess, è richiesto quanto segue:  
  
-   Almeno un controller di dominio installato in Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2 o Windows Server 2008. 
  
    Se il controller di dominio si trova su una rete perimetrale \(and pertanto raggiungibile dalla scheda di rete Internet @ no__t-1facing del server DirectAccess @ no__t-2 impedire al server DirectAccess di raggiungerlo aggiungendo filtri pacchetti nel dominio , per impedire la connettività all'indirizzo IP della scheda Internet.  
  
-   Il server DirectAccess deve essere membro di un dominio.  
  
-   I client DirectAccess devono essere membri di un dominio. I client possono appartenere a:  
  
    -   Qualsiasi dominio nella stessa foresta del server DirectAccess.  
  
    -   Qualsiasi dominio con due @ no__t-0way trust con il dominio del server DirectAccess.  
  
    -   Qualsiasi dominio in una foresta con due @ no__t-0way trust con la foresta a cui appartiene il dominio DirectAccess.  
  
> [!NOTE]
> - Il server DirectAccess non può essere un controller di dominio.  
> - Il controller di dominio Active Directory usato per DirectAccess non deve essere raggiungibile dalla scheda Internet esterna del server DirectAccess @no__t-l'adattatore 0The non deve trovarsi nel profilo di dominio di Windows Firewall @ no__t-1.  
  
### <a name="bkmk_1_7_GPOs"></a>Pianificare oggetti Criteri di gruppo  
Le impostazioni di DirectAccess configurate quando si configura DirectAccess vengono raccolte in oggetti Criteri di gruppo \(GPO @ no__t-1. Due diversi oggetti Criteri di gruppo vengono popolati con impostazioni DirectAccess e distribuiti come segue:  
  
-   **Oggetto Criteri di gruppo del client DirectAccess**. Questo oggetto Criteri di gruppo contiene le impostazioni del client, incluse le impostazioni per la tecnologia di transizione IPv6, le voci NRPT e le regole di sicurezza della connessione di Windows Firewall con sicurezza avanzata. L'oggetto Criteri di gruppo viene applicato ai gruppi di sicurezza specificati per i computer client.  
  
-   **Oggetto Criteri di gruppo del server DirectAccess**. Questo oggetto Criteri di gruppo contiene le impostazioni di configurazione DirectAccess applicate a qualsiasi server configurato come server DirectAccess nella distribuzione. Contiene anche le regole di sicurezza della connessione di Windows Firewall con sicurezza avanzata.  
  
È possibile configurare gli oggetti Criteri di gruppo in due modi:  
  
1.  **Automaticamente**. Per specificare la creazione automatica. Per ogni oggetto Criteri di gruppo viene specificato un nome predefinito. Gli oggetti Criteri di gruppo vengono creati automaticamente da Attività iniziali guidate.
  
2.  **Manualmente**. È possibile usare gli oggetti Criteri di gruppo predefiniti dall'amministratore di Active Directory.
  
Dopo aver configurato DirectAccess in modo da usare specifici oggetti Criteri di gruppo non sarà possibile configurarlo in modo da usarne altri.
  
> [!IMPORTANT]
> Se si usano oggetti Criteri di gruppo configurati automaticamente o manualmente, è necessario aggiungere un criterio per il rilevamento dei collegamenti lenti se i client usano 3G. Percorso Criteri di gruppo per **Policy: Configurare Criteri di gruppo rilevamento collegamento lento @ no__t-0 è: **Configurazione Computer \/ criteri \/ Modelli amministrativi \/ sistema \/ criteri di gruppo**.  
  
> [!CAUTION]  
> Usare la procedura seguente per eseguire il backup di tutti gli oggetti Criteri di gruppo di DirectAccess prima di eseguire i cmdlet di DirectAccess: [Backup e ripristino della configurazione DirectAccess](https://go.microsoft.com/fwlink/?LinkID=257928)  
  
#### <a name="automatically-created-gpos"></a>Oggetti Criteri di gruppo @ no__t-0created  
Quando si usano automaticamente gli oggetti Criteri di gruppo @ no__t-0created, tenere presente quanto segue:  
  
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
  
#### <a name="manually-created-gpos"></a>Oggetti Criteri di gruppo @ no__t-0created manualmente  
Quando si usano gli oggetti Criteri di gruppo @ no__t-0created manualmente, tenere presente quanto segue:  
  
-   Gli oggetti Criteri di gruppo devono essere presenti prima di eseguire Attività iniziali guidate di Accesso remoto.  
  
-   Quando si usano gli oggetti Criteri di gruppo @ no__t-0created manualmente, per applicare le impostazioni di DirectAccess l'amministratore DirectAccess richiede le autorizzazioni complete per gli oggetti Criteri di gruppo \(Edit, DELETE, Modify Security @ no__t-2 sugli oggetti Criteri di gruppo @ no__t-3created manualmente.  
  
-   Quando si usano oggetti Criteri di gruppo creati manualmente viene eseguita una ricerca di un collegamento all'oggetto Criteri di gruppo nell'intero dominio. Se l'oggetto Criteri di gruppo non è collegato nel dominio, verrà automaticamente creato un collegamento nel dominio radice. Se le autorizzazioni richieste per creare il collegamento non sono disponibili, verrà emesso un avviso.  
  
Se non esistono le autorizzazioni corrette per il collegamento degli oggetti Criteri di gruppo, viene emesso un avviso. L'operazione di DirectAccess continua, ma il collegamento non verrà eseguito. Se viene emesso questo avviso non verranno creati automaticamente i collegamenti, nemmeno dopo che saranno state aggiunte le autorizzazioni. L'amministratore dovrà invece creare i collegamenti manualmente.  
  
#### <a name="recovering-from-a-deleted-gpo"></a>Ripristino da un oggetto Criteri di gruppo eliminato  
Se un server DirectAccess, un client o un oggetto Criteri di gruppo del server applicazioni è stato eliminato per errore e non è disponibile alcun backup, è necessario rimuovere di nuovo le impostazioni di configurazione e nuovamente @ no__t-0configure. Se è disponibile un backup sarà possibile ripristinare l'oggetto Criteri di gruppo.  
  
**Gestione DirectAccess** visualizza il messaggio di errore seguente: **Impossibile trovare l'oggetto Criteri di gruppo <GPO name>** . Per rimuovere le impostazioni di configurazione eseguire la procedura seguente:  
  
1.  Eseguire il cmdlet di PowerShell **Uninstall @ no__t-1remoteaccess**.  
  
2.  Ri @ no__t-0open **Gestione DirectAccess**.  
  
3.  Verrà visualizzato un messaggio di errore che informa che l'oggetto Criteri di gruppo non è stato trovato. Fare clic su **rimuovere impostazioni di configurazione**. Al termine, il server verrà ripristinato in uno stato di un @ no__t-0configured.  
  
### <a name="BKMK_Links"></a>Passaggio successivo  
  
-   [Passaggio 2: Pianificare la distribuzione di base di DirectAccess](da-basic-plan-s2-deployment.md)  
  
