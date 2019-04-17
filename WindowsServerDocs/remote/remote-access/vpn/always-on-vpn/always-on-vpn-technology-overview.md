---
title: Always On Panoramica della tecnologia VPN
description: 'Questa pagina provies una breve panoramica delle tecnologie di VPN Always On con collegamenti a documenti dettagliati. '
ms.prod: windows-server-threshold
ms.technology: networking-ras
ms.topic: article
ms.date: 11/05/2018
ms.author: pashort
author: shortpatti
ms.localizationpriority: medium
ms.openlocfilehash: fd3f7c6ca8555e270aabf04bbee6800ed284080c
ms.sourcegitcommit: 4147e092d77d9458696e6686bccefe3788c90508
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/28/2019
ms.locfileid: "9031325"
---
# Panoramica della tecnologia di VPN Always On
>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows 10

& #171;  [ **Precedente:** conoscere i miglioramenti di VPN Always On](always-on-vpn-enhancements.md)<br>
& #187;  [ **Successivo:** ulteriori informazioni sulle funzionalità avanzate di VPN Always On](deploy/always-on-vpn-adv-options.md)

Per la distribuzione, è necessario installare un nuovo server di accesso remoto che esegue Windows Server 2016, nonché modificare alcuni dell'infrastruttura esistente per la distribuzione.

La figura seguente mostra l'infrastruttura necessaria per distribuire VPN Always On.

![Always On infrastruttura VPN](../../../media/Always-On-Vpn/Ao-Vpn-Overview.jpg)

Il processo di connessione rappresentato in questa figura è composto dai passaggi seguenti:

1. Utilizzando il server DNS pubblici, il client VPN di Windows 10 esegue una query di risoluzione dei nomi per l'indirizzo IP del gateway VPN.

2. Usando l'indirizzo IP restituito da DNS, il client VPN invia una richiesta di connessione al gateway VPN.

3. Il gateway VPN è configurato anche come un \(RADIUS\) Remote Authentication Dial-In User Service Client. il Client RADIUS VPN invia la richiesta di connessione al server dei criteri di rete aziendale o dell'organizzazione per l'elaborazione richiesta di connessione.

4. Il server dei criteri di rete elabora la richiesta di connessione, inclusa l'esecuzione di autenticazione e l'autorizzazione e determina se consentire o negare la richiesta di connessione.

5. Il server dei criteri di rete inoltra una risposta di accettazione o negare l'accesso al gateway VPN.

6. La connessione viene avviata o terminata in base alla risposta che il server VPN ricevuto dal server dei criteri di rete.

Per altre informazioni su ogni componente di infrastruttura illustrato nella figura precedente, vedi le sezioni seguenti.

>[!NOTE]
>Se hai già alcune di queste tecnologie distribuite nella rete, è possibile utilizzare le istruzioni in questa Guida alla distribuzione per eseguire la configurazione aggiuntive delle tecnologie per questo scopo di distribuzione.

## Domain Name System (DNS)

Interni ed esterni aree Domain Name System (DNS) sono obbligatori, che presuppone che l'area interno è un sottodominio privilegi di amministratore delegato della zona esterno (ad esempio, corp.contoso.com e contoso.com).

Altre informazioni sulle [Domain Name System (DNS)](../../../../networking/dns/dns-top.md) o [Guida alla rete Core](../../../../networking/core-network-guide/core-network-guide.md).




>[!NOTE] 
>Altri DNS progettazioni, ad esempio DNS "split Brain" (con lo stesso nome di dominio internamente ed esternamente in zone DNS separate) o riguardano interna e i domini esterni (ad esempio, contoso.com e contoso.local) sono inoltre disponibili. Per altre informazioni sulla distribuzione DNS "split Brain", vedi [Usare i criteri DNS per la distribuzione DNS /-cervello doppia](../../../../networking/dns/deploy/split-brain-DNS-deployment.md).

## Firewall

Assicurati che i firewall consentano il traffico che è necessario per le comunicazioni con VPN sia raggio funzionare correttamente.

Per altre informazioni, vedi [Configurare Firewall per il traffico RADIUS](../../../../networking/technologies/nps/nps-firewalls-configure.md).

## Accesso remoto come Server VPN Gateway RAS

In Windows Server 2016, il ruolo del server Accesso remoto è progettato per eseguire correttamente come un router sia un server di accesso remoto. di conseguenza, supporta un'ampia gamma di funzionalità. Per questa Guida alla distribuzione, richiedere solo un piccolo sottoinsieme di queste funzionalità: supporto per le connessioni VPN IKEv2 e il routing LAN.

IKEv2 è una connessione VPN descritto in Internet Engineering Task Force richiedere per commenti 7296 protocollo di tunneling. Il vantaggio principale di IKEv2 è che è tollerata interruzioni nella connessione di rete sottostante. Ad esempio, se la connessione è temporaneamente persa o se un utente si sposta un computer client da una rete a un altro, IKEv2 Ripristina automaticamente la connessione VPN quando viene ripristinata la connessione di rete, tutto senza l'intervento dell'utente.

Usando Gateway RAS, puoi distribuire le connessioni VPN per fornire agli utenti finali con accesso remoto alla rete dell'organizzazione e le risorse. Distribuzione di VPN Always On consente di mantenere una connessione permanente tra i client e di rete dell'organizzazione, ogni volta che i computer remoti connessi a Internet. Con Gateway RAS, è anche possibile creare una connessione VPN da sito a sito tra due server in posizioni diverse, ad esempio tra ufficio principale e una filiale e utilizzare \(NAT\) Network Address Translation in modo che gli utenti all'interno della rete possono accedere esterno risorse, ad esempio Internet. Inoltre, Gateway RAS supporta protocollo BGP (Border Gateway), che fornisce servizi di routing dinamici quando le sedi remote dispone anche di gateway edge che supportano il protocollo BGP.

È possibile gestire i gateway di servizio di accesso remoto (RAS) usando i comandi di Windows PowerShell e la Microsoft Management Console (MMC) l'accesso remoto.



## Server dei criteri di rete

Dei criteri di rete consente di creare e applicare i criteri di accesso di rete a livello di organizzazione per la connessione richiesta autenticazione e autorizzazione. Quando usi dei criteri di rete come server RADIUS Remote Authentication Dial-In User Service (), configurare server di accesso di rete, ad esempio server VPN, come client RADIUS in Criteri di rete.

Anche configurare i criteri di rete che usa dei criteri di rete di autorizzare le richieste di connessione ed è possibile configurare l'accounting RADIUS in modo che dei criteri di rete registra le informazioni di contabilità per registrare i file sul disco rigido locale o in un database Microsoft SQL Server.

Per altre informazioni, vedi [Il Server di criteri di rete (NPS)](../../../../networking/technologies/nps/nps-top.md).


## Servizi certificati Active Directory

Il Server di autorità di certificazione (CA) è un'autorità di certificazione che è in esecuzione Servizi certificati Active Directory. La configurazione di reti VPN richiede una basata su Active Directory infrastruttura a chiave pubblica (PKI).

Le organizzazioni possono utilizzare Servizi certificati Active Directory per migliorare la sicurezza eseguendo il binding l'identità di una persona, un dispositivo o un servizio a una chiave pubblica corrispondente. Servizi certificati Active Directory include anche funzionalità che consentono di gestire la registrazione certificato e revoca in un'ampia gamma di ambienti scalabile. Per altre informazioni, vedere [Panoramica di servizi di Active Directory certificati](https://technet.microsoft.com/library/hh831740.aspx) e [Le indicazioni di progettazione infrastruttura a chiave pubblica](https://social.technet.microsoft.com/wiki/contents/articles/2901.public-key-infrastructure-design-guidance.aspx).

Durante il completamento della distribuzione, configurerai i seguenti modelli di certificato nella CA.

-   Il modello di certificato di autenticazione utente

-   Il modello di certificato di autenticazione Server VPN

-   Il modello di certificato di autenticazione Server dei criteri di rete

### Modelli di certificato

Modelli di certificato possono semplificare notevolmente l'attività di amministrazione di un'autorità di certificazione (CA), consentendo a rilasciare i certificati che sono preconfigurati per le attività selezionate. Lo snap-in MMC Modelli di certificato consente di eseguire le attività seguenti.

-   Visualizzare le proprietà per ogni modello di certificato.

-   Copiare e modificare modelli di certificato.

-   Controllare quali utenti e computer possono leggere modelli e registrazione dei certificati.

-   Eseguire altre attività amministrative relative ai modelli di certificato.

Modelli di certificato sono parte integrante di un'autorità di certificazione (CA). Sono un elemento importante dei criteri del certificato per un ambiente, ovvero il set di regole e formati per la registrazione dei certificati, dell'uso e gestione.

Per altre informazioni, vedi [I modelli di certificato](https://technet.microsoft.com/library/cc730705.aspx).

### Certificati digitali Server

Questa Guida alla distribuzione fornisce istruzioni per l'uso di servizi certificati Active Directory (AD CS) per eseguire la registrazione e registrazione automatica dei certificati per l'accesso remoto e server di infrastruttura dei criteri di rete. Servizi certificati Active Directory consente di creare un'infrastruttura a chiave pubblica (PKI) e di fornire crittografia a chiave pubblica, i certificati digitali e funzionalità di firma digitale per la tua organizzazione.

Quando si usano i certificati digitali server per l'autenticazione tra computer della rete, i certificati di forniscano:

1.  Riservatezza tramite la crittografia.

2.  Integrità attraverso le firme digitali.

3.  Autenticazione associando le chiavi dei certificati con un account computer, utente o un dispositivo su una rete di computer.

Per altre informazioni, vedi [Guida dettagliata di AD CS: distribuzione di due livelli PKI gerarchia](https://social.technet.microsoft.com/wiki/contents/articles/15037.ad-cs-step-by-step-guide-two-tier-pki-hierarchy-deployment.aspx).

## Servizi di dominio Active Directory

AD DS fornisce un database distribuito che archivia e gestisce le informazioni sulle risorse di rete e i dati specifici dell'applicazione da applicazioni basate su directory. Gli amministratori possono utilizzare Active Directory Domain Services per organizzare gli elementi di una rete, ad esempio gli utenti, computer e altri dispositivi, in una struttura gerarchica contenimento. La struttura gerarchica contenimento include foresta di Active Directory, domini nella foresta e unità organizzative (UO) in ogni dominio. Un server che esegue Active Directory Domain Services viene chiamato un controller di dominio.

Active Directory Domain Services contiene gli account utente, gli account dei computer e proprietà dell'account che sono necessari per PEAP Protected Extensible Authentication Protocol () per autenticare le credenziali dell'utente e per valutare l'autorizzazione per le richieste di connessione VPN. Per informazioni sulla distribuzione di Active Directory Domain Services, vedi la [Guida alla rete Core](../../../../networking/core-network-guide/Core-Network-Guide.md)di Windows Server 2016.



Durante il completamento delle operazioni di in questa distribuzione, configurerai gli elementi seguenti nel controller di dominio.

-   Abilitare la registrazione automatica del certificato in Criteri di gruppo per utenti e computer

-   Creare il gruppo di utenti VPN

-   Creare il gruppo di server VPN

-   Creare il gruppo di server dei criteri di rete

### Computer e utenti di active Directory

Active Directory gli utenti e computer è un componente di Active Directory Domain Services che contiene gli account che rappresentano le entità fisiche, ad esempio un computer, una persona o un gruppo di sicurezza. Un gruppo di sicurezza è una raccolta degli account utente o computer che gli amministratori possono gestire come una singola unità. Account utente e computer appartenenti a un determinato gruppo vengono definiti membri del gruppo.

Gli account utente in Active Directory utenti e computer hanno proprietà dial-in che valuta i criteri di rete durante il processo di autorizzazione - a meno che non è impostata la proprietà di **Autorizzazione di accesso alla rete** dell'account utente per controllare l'accesso tramite criteri di rete dei criteri di rete ** **. Questo è l'impostazione predefinita per tutti gli account utente. In alcuni casi, tuttavia, questa impostazione potrebbe essere una diversa configurazione che impedisce all'utente di connessione tramite VPN. Per evitare questa eventualità, è possibile configurare il server dei criteri di rete per ignorare dial-in di proprietà dell'account utente.

Per altre informazioni, vedi [Configurazione dei criteri di rete alle proprietà Dial-in di ignorare Account utente](../../../../networking/technologies/nps/nps-np-configure.md#configure-nps-to-ignore-user-account-dial-in-properties).



### Gestione Criteri di gruppo

Gestione criteri di gruppo consente di directory gestione basata su modifiche e della configurazione delle impostazioni utente e computer, incluse le informazioni di sicurezza e l'utente. Usare criteri di gruppo per definire le configurazioni per gruppi di utenti e computer.

Con criteri di gruppo, è possibile specificare le impostazioni per le voci del Registro di sistema, sicurezza, installazione software, script, reindirizzamento cartelle, servizi di installazione remota e la manutenzione di Internet Explorer. Le impostazioni di criteri di gruppo che crei sono contenute in un oggetto Criteri di gruppo (GPO). Associando un oggetto Criteri di gruppo selezionati contenitori del sistema Active Directory, ovvero siti, domini e unità organizzative, è possibile applicare le impostazioni dell'oggetto Criteri di gruppo agli utenti e computer in tali contenitori di Active Directory. Per gestire gli oggetti Criteri di gruppo in un'organizzazione, è possibile utilizzare gruppo di criteri di gestione Editor Microsoft Management Console (MMC).


## Client VPN di Windows 10

Oltre a componenti del server, assicurati che i computer client configurare per l'uso di VPN eseguono aggiornamento dell'anniversario di Windows 10 (version1607). I client VPN di Windows 10 devono essere aggiunto al dominio per il dominio di Active Directory.


Il client VPN di Windows 10 è configurabile e offre molte opzioni. Per illustrare meglio le funzionalità specifiche di che questo scenario Usa, Table1 identifica le categorie di funzionalità VPN e configurazioni specifiche che fa riferimento a questa distribuzione. Configurare le singole impostazioni di queste funzionalità con il provider di servizi di configurazione (CSP) VPNv2 descritti più avanti in questa distribuzione. 

Tabella 1. Funzionalità VPN e le configurazioni descritte in questa distribuzione

| **Funzionalità VPN** | **Configurazione di uno scenario di distribuzione**         |
|-----------------|-----------------------------------------------|
| Tipo di connessione | IKEv2 nativo                                  |
| Routing         | Dividere tunneling                               |
| Risoluzione dei nomi | Suffisso DNS e Domain Name Information List   |
| Attivazione      | Rilevamento della rete sempre On e attendibili       |
| Authentication  | PEAP-TLS con i certificati utente protetta da TPM. |
---

>[!NOTE] 
>PEAP-TLS e TPM sono rispettivamente "Protected Extensible Authentication Protocol con Transport Layer Security" e "Trusted Platform Module".

### Nodi CSP VPNv2

In questa distribuzione, utilizzare il nodo del CSP VPNv2 ProfileXML per creare il profilo VPN che viene inviato al computer client Windows 10. Provider di servizi di configurazione (CSP) sono le interfacce che espongono funzionalità di gestione di varie all'interno del client di Windows. Concettualmente, CSP funzionano in modo analogo funzionamento di criteri di gruppo. Ciascun CSP ha i nodi di configurazione che rappresentano le singole impostazioni. Inoltre, come le impostazioni di criteri di gruppo, è possibile collegare impostazioni CSP le chiavi del Registro di sistema, file, autorizzazioni e così via. Simile a come usare l'Editor Gestione criteri di gruppo per configurare gli oggetti Criteri di gruppo (GPO), configurare nodi CSP usando una soluzione di gestione di dispositivi mobili come Microsoft Intune. I prodotti MDM come Intune offrono un'opzione di configurazione descrittivi che consente di configurare il provider CSP nel sistema operativo.

![Gestione dei dispositivi mobili per la configurazione CSP](../../../media/Always-On-Vpn/Vpn-Mdm.jpg)

Tuttavia, è possibile configurare alcuni nodi CSP direttamente tramite un'interfaccia utente (UI), come la Console di amministrazione di Intune. In questi casi, è necessario configurare le impostazioni Open Mobile Alliance Uniform Resource Identifier (URI OMA) manualmente. Configurare OMA-URI tramite il protocollo di gestione dei dispositivi OMA OMA-DM (), una specifica di gestione di dispositivi universali che supportano i dispositivi più moderni Apple, Android e Windows. A condizione conforme alla specifica OMA-DM, tutti i prodotti MDM devono interagire con questi sistemi operativi nello stesso modo.

Windows 10 offre molte CSP, ma questa distribuzione è incentrata sull'utilizzo del CSP VPNv2 per configurare il client VPN. CSP VPNv2 consente la configurazione di ogni impostazione del profilo VPN in Windows 10 tramite un nodo CSP univoco. Contenuti anche nel CSP VPNv2 è un nodo denominato *ProfileXML*, che consente di configurare tutte le impostazioni in un nodo anziché singolarmente. Per altre informazioni su ProfileXML, vedi la sezione "Panoramica ProfileXML" più avanti in questa distribuzione. Per informazioni dettagliate su ogni nodo del CSP VPNv2, vedi il [CSP VPNv2](https://msdn.microsoft.com/windows/hardware/commercialize/customize/mdm/vpnv2-csp).



## Passaggi successivi

- [Informazioni su alcune delle funzionalità avanzate di VPN Always On](deploy/always-on-vpn-adv-options.md)

- [Iniziare a pianificare la distribuzione di VPN Always On](deploy/always-on-vpn-deploy-deployment.md)


---

## Argomenti correlati
- [Supporto di software server Microsoft per le macchine virtuali di Microsoft Azure](https://support.microsoft.com/help/2721672/microsoft-server-software-support-for-microsoft-azure-virtual-machines): questo articolo illustra i criteri di supporto per l'esecuzione di software server Microsoft nell'ambiente di macchina virtuale Microsoft Azure (infrastructure-as-a-service).

- [Accesso remoto](../../Remote-Access.md): in questo argomento viene fornita una panoramica del ruolo del server Accesso remoto in Windows Server 2016.

- [Guida tecnica alle reti VPN Windows 10](https://docs.microsoft.com/windows/access-protection/vpn/vpn-guide): questa guida illustra le decisioni da prendere per i client Windows 10 nella tua soluzione VPN aziendale e come configurare la distribuzione. Questa guida fa riferimento al provider di servizi di configurazione (CSP) VPNv2 e fornisce istruzioni per la configurazione di gestione dei dispositivi mobili (MDM) con Microsoft Intune e il modello di profilo VPN per Windows10.

- [Guida alla rete core](../../../../networking/core-network-guide/Core-Network-Guide.md): questa guida fornisce istruzioni su come pianificare e distribuire i componenti di base necessari per una rete completamente funziona e un nuovo dominio di Active Directory in una nuova foresta.

- [Domain Name System (DNS)](../../../../networking/dns/dns-top.md): questo argomento offre una panoramica dei sistemi DNS (Domain Name). In Windows Server 2016, DNS è un ruolo del server che puoi installare tramite i comandi di Server Manager o Windows PowerShell. Se installi una nuova foresta di Active Directory e dominio, DNS viene installato automaticamente con Active Directory come server di catalogo globale per la foresta e dominio. 

- [Panoramica di servizi di Active Directory certificati](https://technet.microsoft.com/library/hh831740.aspx): questo documento viene fornita una panoramica di servizi certificati Active Directory (AD CS) in Windows Server 2012. Servizi certificati Active Directory è il ruolo del Server che consente di creare un'infrastruttura a chiave pubblica (PKI) e di fornire crittografia a chiave pubblica, i certificati digitali e funzionalità di firma digitale per la tua organizzazione.

- [Le indicazioni di progettazione infrastruttura a chiave pubblica](https://social.technet.microsoft.com/wiki/contents/articles/2901.public-key-infrastructure-design-guidance.aspx): questo wiki vengono fornite indicazioni sulla progettazione di infrastrutture a chiave pubblica (PKI). Prima di configurare una gerarchia di autorità di certificazione PKI e certificazione, prestare attenzione criteri e certificato pratica informativa security dell'organizzazione (CPS).

- [Guida dettagliata di AD CS: distribuzione di due livelli PKI gerarchia](https://social.technet.microsoft.com/wiki/contents/articles/15037.ad-cs-step-by-step-guide-two-tier-pki-hierarchy-deployment.aspx): questa procedura dettagliata descrive i passaggi necessari per configurare una configurazione di base di servizi certificati Active Directory® (AD CS) in un ambiente lab. Servizi certificati Active Directory in Windows Server® 2008 R2 offre servizi personalizzabili per la creazione e la gestione dei certificati di chiave pubblici utilizzati nei sistemi di protezione software Usa tecnologie a chiave pubblica.

- [Server dei criteri di rete (NPS)](../../../../networking/technologies/nps/nps-top.md): questo argomento viene fornita una panoramica di Server dei criteri di rete in Windows Server 2016. Server dei criteri di rete consente di creare e imporre criteri di accesso di rete a livello di organizzazione per l'autorizzazione e l'autenticazione delle richieste di connessione. 

---
