---
title: Panoramica della tecnologia VPN Always On
description: 'In questa pagina viene illustrata una breve panoramica delle Always On tecnologie VPN con collegamenti a documenti dettagliati. '
ms.prod: windows-server
ms.technology: networking-ras
ms.topic: article
ms.date: 11/05/2018
ms.author: pashort
author: shortpatti
ms.localizationpriority: medium
ms.openlocfilehash: 31d0d5c12760fc627ce93972f4a70e85f61dd178
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71404365"
---
# <a name="always-on-vpn-technology-overview"></a>Panoramica della tecnologia VPN Always On

>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows 10

- [**Precedente** Informazioni sui miglioramenti apportati alla VPN Always On](always-on-vpn-enhancements.md)
- [**Prossimo** Informazioni sulle funzionalità avanzate di Always On VPN](deploy/always-on-vpn-adv-options.md)

Per questa distribuzione, è necessario installare un nuovo server di accesso remoto che esegue Windows Server 2016, nonché modificare parte dell'infrastruttura esistente per la distribuzione.

Nella figura seguente è illustrata l'infrastruttura necessaria per distribuire Always On VPN.

![Infrastruttura VPN Always On](../../../media/Always-On-Vpn/Ao-Vpn-Overview.jpg)

Il processo di connessione illustrato in questa illustrazione è costituito dai passaggi seguenti:

1. Usando i server DNS pubblici, il client VPN di Windows 10 esegue una query di risoluzione dei nomi per l'indirizzo IP del gateway VPN.

2. Usando l'indirizzo IP restituito da DNS, il client VPN invia una richiesta di connessione al gateway VPN.

3. Il gateway VPN viene anche configurato come client di Remote Authentication Dial-In User Service (RADIUS); il client RADIUS VPN Invia la richiesta di connessione al server dei criteri di rete aziendale/aziendale per l'elaborazione della richiesta di connessione.

4. Il server NPS elabora la richiesta di connessione, tra cui l'esecuzione dell'autorizzazione e l'autenticazione, e determina se consentire o rifiutare la richiesta di connessione.

5. Il server NPS invia una risposta di accesso-accettazione o di accesso negato al gateway VPN.

6. La connessione viene avviata o terminata in base alla risposta ricevuta dal server VPN dal server dei criteri di rete.

Per ulteriori informazioni su ogni componente dell'infrastruttura illustrato nella figura precedente, vedere le sezioni seguenti.

>[!NOTE]
>Se sono già state distribuite alcune di queste tecnologie nella rete, è possibile usare le istruzioni disponibili in questa guida alla distribuzione per eseguire ulteriori configurazioni delle tecnologie a questo scopo della distribuzione.

## <a name="domain-name-system-dns"></a>Domain Name System (DNS)

Sono necessarie entrambe le zone di Domain Name System interno ed esterno (DNS), che presuppone che la zona interna sia un sottodominio delegato della zona esterna, ad esempio corp.contoso.com e contoso.com.

Altre informazioni su [Domain Name System (DNS)](../../../../networking/dns/dns-top.md) o la [Guida alla rete core](../../../../networking/core-network-guide/core-network-guide.md).

>[!NOTE]
>Sono inoltre possibili altre progettazioni DNS, ad esempio il DNS split brain (che usa lo stesso nome di dominio internamente ed esternamente in zone DNS separate) o domini interni ed esterni non correlati, ad esempio contoso. local e contoso.com. Per altre informazioni sulla distribuzione del DNS "Split Brain", vedere [usare i criteri DNS per la distribuzione DNS split-brain](../../../../networking/dns/deploy/split-brain-DNS-deployment.md).

## <a name="firewalls"></a>Firewall

Assicurarsi che i firewall consentano il corretto funzionamento del traffico necessario per le comunicazioni VPN e RADIUS.

Per altre informazioni, vedere [configurare i firewall per il traffico RADIUS](../../../../networking/technologies/nps/nps-firewalls-configure.md).

## <a name="remote-access-as-a-ras-gateway-vpn-server"></a>Accesso remoto come server VPN gateway RAS

In Windows Server 2016, il ruolo del server accesso remoto è progettato per essere eseguito correttamente sia come router sia come server di accesso remoto. Pertanto, supporta una vasta gamma di funzionalità. Per questa guida alla distribuzione è necessario solo un piccolo subset di queste funzionalità: supporto per le connessioni VPN IKEv2 e il routing LAN.

IKEv2 è un protocollo di tunneling VPN descritto in Internet Engineering Task Force Request per i commenti 7296. Il vantaggio principale di IKEv2 è che tollera le interruzioni nella connessione di rete sottostante. Se ad esempio la connessione viene persa temporaneamente o un utente sposta un computer client da una rete a un'altra, IKEv2 ripristina automaticamente la connessione VPN quando viene ristabilita la connessione di rete, senza l'intervento dell'utente.

Tramite il gateway RAS è possibile distribuire connessioni VPN per offrire agli utenti finali accesso remoto alla rete e alle risorse dell'organizzazione. La distribuzione di Always On VPN mantiene una connessione permanente tra i client e la rete dell'organizzazione ogni volta che i computer remoti sono connessi a Internet. Con il gateway RAS è inoltre possibile creare una connessione VPN da sito a sito tra due server in posizioni diverse, ad esempio tra l'ufficio principale e una succursale e utilizzare NAT (Network Address Translation) in modo che gli utenti all'interno della rete possano accedere a External risorse, ad esempio Internet. Il gateway RAS supporta inoltre Border Gateway Protocol (BGP), che fornisce servizi di routing dinamico quando i percorsi di ufficio remoto hanno anche gateway perimetrali che supportano BGP.

È possibile gestire i gateway del servizio di accesso remoto (RAS) usando i comandi di Windows PowerShell e Microsoft Management Console (MMC) di accesso remoto.

## <a name="network-policy-server-nps"></a>Server dei criteri di rete

Server dei criteri di rete consente di creare e applicare criteri di accesso alla rete a livello di organizzazione per l'autenticazione e l'autorizzazione delle richieste di connessione. Quando si usa server dei criteri di rete come server RADIUS (Remote Authentication Dial-In User Service), si configurano i server di accesso alla rete, ad esempio i server VPN, come client RADIUS in NPS.

È inoltre possibile configurare criteri di rete utilizzati da Server dei criteri di rete per autorizzare richieste di connessione, nonché configurare l'accounting RADIUS in modo che Server dei criteri di rete registri le informazioni di accounting in file di registro sul disco rigido locale oppure in un database di Microsoft SQL Server.

Per ulteriori informazioni, vedere [Server dei criteri di rete (NPS)](../../../../networking/technologies/nps/nps-top.md).

## <a name="active-directory-certificate-services"></a>Servizi certificati Active Directory

Il server dell'autorità di certificazione (CA) è un'autorità di certificazione che esegue Active Directory Servizi certificati. La configurazione VPN richiede un'infrastruttura a chiave pubblica (PKI) basata su Active Directory.

Le organizzazioni possono utilizzare Servizi certificati Active Directory per migliorare la sicurezza associando l'identità di una persona, un dispositivo o un servizio a una chiave pubblica corrispondente. Servizi certificati Active Directory include inoltre funzionalità che consentono di gestire la registrazione di certificati e le revoche in una vasta gamma di ambienti scalabili. Per ulteriori informazioni, vedere [Panoramica di servizi di Active Directory certificato](https://technet.microsoft.com/library/hh831740.aspx) e [linee guida alla progettazione di infrastruttura di chiave pubblica](https://social.technet.microsoft.com/wiki/contents/articles/2901.public-key-infrastructure-design-guidance.aspx).

Durante il completamento della distribuzione, si configureranno i modelli di certificato seguenti nell'autorità di certificazione.

- Modello di certificato di autenticazione utente

- Modello di certificato di autenticazione server VPN

- Modello di certificato di autenticazione server NPS

### <a name="certificate-templates"></a>Modelli di certificato

I modelli di certificato possono semplificare notevolmente l'amministrazione di un'autorità di certificazione (CA) consentendo di rilasciare certificati preconfigurati per le attività selezionate. Lo snap-in MMC modelli di certificato consente di eseguire le operazioni seguenti.

- Visualizzare le proprietà per ogni modello di certificato.

- Copiare e modificare i modelli di certificato.

- Controllare quali utenti e computer possono leggere i modelli e registrarsi per i certificati.

- Eseguire altre attività amministrative relative ai modelli di certificato.

I modelli di certificato sono parte integrante di un'autorità di certificazione (CA) dell'organizzazione (Enterprise). Si tratta di un elemento importante del criterio dei certificati per un ambiente, ovvero il set di regole e formati per la registrazione, l'utilizzo e la gestione dei certificati.

Per ulteriori informazioni, vedere [modelli di certificato](https://technet.microsoft.com/library/cc730705.aspx).

### <a name="digital-server-certificates"></a>Certificati server digitali

In questa guida alla distribuzione vengono fornite istruzioni per l'utilizzo di Active Directory Servizi certificati (AD CS) per registrare e registrare automaticamente i certificati per l'accesso remoto e i server di infrastruttura NPS. Servizi certificati Active Directory consente di compilare un'infrastruttura a chiave pubblica (PKI) e fornire funzionalità di firma digitale, certificati digitali e crittografia a chiave pubblica dell'organizzazione.

Quando si utilizzano i certificati digitali server per l'autenticazione tra computer della rete, forniscano i certificati:

1. Riservatezza tramite crittografia.

2. Integrità tramite firme digitali.

3. Autenticazione mediante l'associazione di chiavi del certificato a un computer, un utente o un account di dispositivo in una rete di computer.

Per ulteriori informazioni, vedere [la guida dettagliata di Servizi certificati Active Directory: Distribuzione](https://social.technet.microsoft.com/wiki/contents/articles/15037.ad-cs-step-by-step-guide-two-tier-pki-hierarchy-deployment.aspx)della gerarchia PKI a due livelli.

## <a name="active-directory-domain-services-ad-ds"></a>Servizi di dominio Active Directory

Servizi di dominio Active Directory fornisce un database distribuito che consente di archiviare e gestire informazioni sulle risorse di rete e dati specifici delle applicazioni provenienti da applicazioni abilitate all'uso di directory. Gli amministratori possono utilizzare Servizi di dominio Active Directory per organizzare gli elementi di una rete, ad esempio utenti, computer e altri dispositivi, in una struttura di contenimento gerarchica. Tale struttura include la foresta di Active Directory, i domini della foresta e le unità organizzative (OU, Organizational Unit) di ogni dominio. Un server che esegue Servizi di dominio Active Directory viene definito controller di dominio.

Servizi di dominio Active Directory contiene gli account utente, gli account computer e le proprietà dell'account richiesti da PEAP (Protected Extensible Authentication Protocol) per autenticare le credenziali utente e per valutare l'autorizzazione per le richieste di connessione VPN. Per informazioni sulla distribuzione di servizi di dominio Active Directory, vedere la [Guida alla rete core](../../../../networking/core-network-guide/Core-Network-Guide.md)di Windows Server 2016.

Durante il completamento dei passaggi di questa distribuzione, si configureranno gli elementi seguenti nel controller di dominio.

- Abilitare la registrazione automatica dei certificati in Criteri di gruppo per computer e utenti

- Creare il gruppo utenti VPN

- Creare il gruppo di server VPN

- Creare il gruppo server NPS

### <a name="active-directory-users-and-computers"></a>Utenti e computer di Active Directory

Active Directory Users and Computers è un componente di dominio Active Directory che contiene gli account che rappresentano entità fisiche, ad esempio un computer, una persona o un gruppo di sicurezza. Un gruppo di sicurezza è una raccolta di account utente o computer che gli amministratori possono gestire come una singola unità. Gli account utente e computer che appartengono a un determinato gruppo sono detti membri del gruppo.

Gli account utente in Active Directory utenti e computer dispongono di proprietà di connessione remota che i server dei criteri di rete valutano durante il processo di autorizzazione, a meno che la proprietà **autorizzazioni di accesso alla rete** dell'account utente non sia impostata per **controllare l'accesso tramite criteri di rete NPS** . Questa è l'impostazione predefinita per tutti gli account utente. In alcuni casi, tuttavia, questa impostazione potrebbe avere una configurazione diversa che impedisce all'utente di connettersi usando la VPN. Per evitare questa possibilità, è possibile configurare il server NPS per ignorare le proprietà di connessione remota dell'account utente.

Per ulteriori informazioni, vedere [configurare NPS per ignorare le proprietà di connessione remota dell'account utente](../../../../networking/technologies/nps/nps-np-configure.md#configure-nps-to-ignore-user-account-dial-in-properties).

### <a name="group-policy-management"></a>Gestione Criteri di gruppo

Criteri di gruppo Management consente la gestione delle modifiche e della configurazione basata su directory delle impostazioni utente e computer, incluse le informazioni sulla sicurezza e sugli utenti. Usare Criteri di gruppo per definire le configurazioni per gruppi di utenti e computer.

Con Criteri di gruppo è possibile specificare le impostazioni per le voci del registro di sistema, sicurezza, installazione software, script, Reindirizzamento cartelle, servizi di installazione remota e manutenzione di Internet Explorer. Le impostazioni Criteri di gruppo create sono contenute in un oggetto Criteri di gruppo (GPO). Associando un oggetto Criteri di gruppo di contenitori di sistema di Active Directory selezionati, siti, domini e unità organizzative, è possibile applicare le impostazioni dell'oggetto Criteri di gruppo a utenti e computer presenti in tali contenitori Active Directory. Per gestire Criteri di gruppo oggetti in un'organizzazione, è possibile utilizzare la Editor Gestione Criteri di gruppo Microsoft Management Console (MMC).

## <a name="windows-10-vpn-clients"></a>Client VPN di Windows 10

Oltre ai componenti server, assicurarsi che i computer client configurati per l'uso della VPN eseguano l'aggiornamento dell'anniversario di Windows 10 (versione 1607). I client VPN di Windows 10 devono essere aggiunti a un dominio Active Directory dominio.


Il client VPN di Windows 10 è altamente configurabile e offre numerose opzioni. Per illustrare meglio le funzionalità specifiche usate in questo scenario, nella tabella 1 vengono identificate le categorie di funzionalità VPN e le configurazioni specifiche a cui fa riferimento questa distribuzione. Si configureranno le singole impostazioni per queste funzionalità usando il provider del servizio di configurazione VPNv2 (CSP) descritto più avanti in questa distribuzione. 

Tabella 1. Funzionalità e configurazioni VPN descritte in questa distribuzione

| Funzionalità VPN     |     Configurazione dello scenario di distribuzione         |
|-----------------|-----------------------------------------------|
| Tipo di connessione |                 IKEv2 nativo                  |
|     Routing     |                Split tunneling                |
| Risoluzione dei nomi |  Elenco di informazioni sul nome di dominio e suffisso DNS  |
|   Attivazione    |    Rilevamento della rete Always On e attendibile    |
| Authentication  | PEAP-TLS con TPM-certificati utente protetti |

>[!NOTE]
>PEAP-TLS e TPM sono rispettivamente "Protected Extensible Authentication Protocol with Transport Layer Security" e "Trusted Platform Module".

### <a name="vpnv2-csp-nodes"></a>Nodi CSP VPNv2

In questa distribuzione si usa il nodo CSP ProfileXML VPNv2 per creare il profilo VPN che viene recapitato ai computer client Windows 10. I provider di servizi di configurazione (CSP) sono interfacce che espongono diverse funzionalità di gestione all'interno del client Windows; concettualmente, i CSP funzionano in modo analogo al funzionamento Criteri di gruppo. Ogni CSP dispone di nodi di configurazione che rappresentano singole impostazioni. Inoltre, come Criteri di gruppo impostazioni, è possibile collegare le impostazioni CSP alle chiavi del registro di sistema, ai file, alle autorizzazioni e così via. Analogamente a come si usa il Editor Gestione Criteri di gruppo per configurare gli oggetti di Criteri di gruppo (GPO), si configurano i nodi CSP usando una soluzione di gestione di dispositivi mobili (MDM), ad esempio Microsoft Intune. I prodotti MDM come Intune offrono un'opzione di configurazione intuitiva che configura il CSP nel sistema operativo.

![Gestione dei dispositivi mobili per la configurazione CSP](../../../media/Always-On-Vpn/Vpn-Mdm.jpg)

Tuttavia, non è possibile configurare alcuni nodi CSP direttamente tramite un'interfaccia utente come la console di amministrazione di Intune. In questi casi, è necessario configurare manualmente le impostazioni Open Mobile Alliance Uniform Resource Identifier (OMA-URI). Per configurare gli URI OMA è possibile usare il protocollo OMA-DM (OMA Device Management Protocol), una specifica di gestione universale dei dispositivi supportata dalla maggior parte dei dispositivi Apple, Android e Windows moderni. Fino a quando si rispetta la specifica OMA-DM, tutti i prodotti MDM dovrebbero interagire con questi sistemi operativi nello stesso modo.

Windows 10 offre molti CSP, ma questa distribuzione è incentrata sull'uso del CSP VPNv2 per configurare il client VPN. Il CSP VPNv2 consente la configurazione di ogni impostazione del profilo VPN in Windows 10 tramite un nodo CSP univoco. Contenuto anche in VPNv2 CSP è un nodo denominato *ProfileXML*, che consente di configurare tutte le impostazioni in un nodo anziché singolarmente. Per ulteriori informazioni su ProfileXML, vedere la sezione "Panoramica di ProfileXML" più avanti in questa distribuzione. Per informazioni dettagliate su ogni nodo VPNv2 CSP, vedere [VPNV2 CSP](https://msdn.microsoft.com/windows/hardware/commercialize/customize/mdm/vpnv2-csp).

## <a name="next-steps"></a>Passaggi successivi

- [Informazioni su alcune delle funzionalità avanzate di Always On VPN](deploy/always-on-vpn-adv-options.md)

- [Inizia a pianificare la distribuzione di Always On VPN](deploy/always-on-vpn-deploy-deployment.md)

## <a name="related-topics"></a>Argomenti correlati

- [Supporto del software server Microsoft per Microsoft Azure macchine virtuali](https://support.microsoft.com/help/2721672/microsoft-server-software-support-for-microsoft-azure-virtual-machines): Questo articolo illustra i criteri di supporto per l'esecuzione del software server Microsoft nell'ambiente di Microsoft Azure macchina virtuale (infrastruttura distribuita come servizio).

- [Accesso remoto](../../Remote-Access.md): Questo argomento fornisce una panoramica del ruolo del server accesso remoto in Windows Server 2016.

- [Guida tecnica VPN di Windows 10](https://docs.microsoft.com/windows/access-protection/vpn/vpn-guide): Questa guida illustra le decisioni che si faranno per i client Windows 10 nella soluzione VPN aziendale e come configurare la distribuzione. Questa guida fa riferimento al provider di servizi di configurazione VPNv2 (CSP) e fornisce istruzioni di configurazione per la gestione di dispositivi mobili (MDM) usando Microsoft Intune e il modello di profilo VPN per Windows 10.

- [Guida alla rete core](../../../../networking/core-network-guide/Core-Network-Guide.md): In questa guida vengono fornite istruzioni su come pianificare e distribuire i componenti di base necessari per una rete completamente funzionante e un nuovo dominio di Active Directory in una nuova foresta.

- [Domain Name System (DNS)](../../../../networking/dns/dns-top.md): In questo argomento viene fornita una panoramica di DNS (Domain Name System). In Windows Server 2016, DNS è un ruolo del server che è possibile installare usando Server Manager o i comandi di Windows PowerShell. Se si installa una nuova foresta Active Directory e un dominio, il DNS viene installato automaticamente con Active Directory come server di catalogo globale per la foresta e il dominio.

- [Panoramica di Servizi certificati Active Directory](https://technet.microsoft.com/library/hh831740.aspx): In questo documento viene fornita una panoramica di Active Directory Servizi certificati (AD CS) in Windows Server® 2012. Servizi certificati Active Directory è il ruolo server che consente di creare un'infrastruttura a chiave pubblica (PKI) e fornire crittografia a chiave pubblica, certificati digitali e funzionalità di firma digitale per l'organizzazione.

- [Linee guida per la progettazione dell'infrastruttura a chiave pubblica](https://social.technet.microsoft.com/wiki/contents/articles/2901.public-key-infrastructure-design-guidance.aspx):  Questo wiki fornisce indicazioni sulla progettazione di infrastrutture a chiave pubblica (PKI). Prima di configurare un'infrastruttura a chiave pubblica (PKI) e una gerarchia di autorità di certificazione (CA), è necessario conoscere i criteri di sicurezza dell'organizzazione e l'istruzione di pratica del certificato (CPS).

- [Guida dettagliata di Servizi certificati Active Directory: Distribuzione](https://social.technet.microsoft.com/wiki/contents/articles/15037.ad-cs-step-by-step-guide-two-tier-pki-hierarchy-deployment.aspx)della gerarchia PKI a due livelli: Questa guida dettagliata descrive i passaggi necessari per configurare una configurazione di base di Active Directory® Servizi certificati (AD CS) in un ambiente lab. Servizi certificati Active Directory in Windows Server® 2008 R2 offre servizi personalizzabili per la creazione e la gestione di certificati di chiave pubblica usati nei sistemi di sicurezza software che usano tecnologie a chiave pubblica.

- [Server dei criteri di rete](../../../../networking/technologies/nps/nps-top.md): Questo argomento fornisce una panoramica di server dei criteri di rete in Windows Server 2016. Server dei criteri di rete consente di creare e imporre criteri di accesso di rete a livello di organizzazione per l'autorizzazione e l'autenticazione delle richieste di connessione.
