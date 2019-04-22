---
title: Sempre nella panoramica della tecnologia VPN
description: 'Questa pagina fornisce un una breve panoramica delle tecnologie VPN Always On con collegamenti a documenti dettagliati. '
ms.prod: windows-server-threshold
ms.technology: networking-ras
ms.topic: article
ms.date: 11/05/2018
ms.author: pashort
author: shortpatti
ms.localizationpriority: medium
ms.openlocfilehash: fd3f7c6ca8555e270aabf04bbee6800ed284080c
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59821262"
---
# <a name="always-on-vpn-technology-overview"></a>Panoramica della tecnologia VPN Always On
>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows 10

&#171;  [**Precedente:** Scopri i miglioramenti di VPN Always On](always-on-vpn-enhancements.md)<br>
&#187;  [**Next:** Informazioni sulle funzionalità avanzate di VPN Always On](deploy/always-on-vpn-adv-options.md)

Per questa distribuzione, è necessario installare un nuovo server di accesso remoto che esegue Windows Server 2016, nonché modificare alcuni dell'infrastruttura esistente per la distribuzione.

La figura seguente mostra l'infrastruttura necessaria per la distribuzione VPN Always On.

![Always On infrastruttura VPN](../../../media/Always-On-Vpn/Ao-Vpn-Overview.jpg)

Il processo di connessione illustrato in questa illustrazione è costituito da questa procedura:

1. Usano i server DNS pubblici, il client VPN di Windows 10 esegue una query di risoluzione dei nomi per l'indirizzo IP del gateway VPN.

2. Usando l'indirizzo IP restituito dal DNS, il client VPN invia una richiesta di connessione al gateway VPN.

3. Il gateway VPN è inoltre configurato come un Remote Authentication Dial-In User Service \(RADIUS\) Client; il Client RADIUS VPN invia la richiesta di connessione al server dei criteri di rete aziendali / dell'organizzazione per l'elaborazione richiesta di connessione.

4. Il server dei criteri di rete elabora la richiesta di connessione, inclusa l'esecuzione di autorizzazione e autenticazione e determina se consentire o negare la richiesta di connessione.

5. Il server dei criteri di rete inoltra una risposta di autorizzazione di accesso o nega l'accesso al gateway VPN.

6. Viene avviata o interruzione della connessione in base alla risposta ricevuti dal server VPN dal server dei criteri di rete.

Per altre informazioni su ogni componente dell'infrastruttura illustrata nella figura sopra riportata, vedere le sezioni seguenti.

>[!NOTE]
>Se si dispone già di alcune di queste tecnologie distribuite nella rete, è possibile utilizzare le istruzioni riportate in questa Guida alla distribuzione per eseguire un'ulteriore configurazione delle tecnologie per questo scopo della distribuzione.

## <a name="domain-name-system-dns"></a>Domain Name System (DNS)

Le zone di sistema DNS (Domain Name) interne ed esterne sono necessari, che presuppone che l'area interna è un sottodominio delegato della zona esterno (ad esempio, corp.contoso.com e contoso.com).

Altre informazioni sulle [sistema DNS (Domain Name)](../../../../networking/dns/dns-top.md) oppure [Guida alla rete Core](../../../../networking/core-network-guide/core-network-guide.md).




>[!NOTE] 
>Altri servizi DNS progetta, ad esempio DNS "split Brain" (usando lo stesso nome di dominio internamente ed esternamente in zone DNS separate) o non correlata interni e sono possibili anche i domini esterni (ad esempio, contoso. Local e contoso.com). Per altre informazioni sulla distribuzione di DNS "split Brain", vedere [usare i criteri DNS per la distribuzione di Split-Brain/DNS](../../../../networking/dns/deploy/split-brain-DNS-deployment.md).

## <a name="firewalls"></a>Firewall

Verificare che i firewall consentano il traffico che è necessario per le comunicazioni di VPN sia RADIUS funzionare correttamente.

Per altre informazioni, vedere [configurare i firewall per il traffico RADIUS](../../../../networking/technologies/nps/nps-firewalls-configure.md).

## <a name="remote-access-as-a-ras-gateway-vpn-server"></a>Accesso remoto come Server VPN Gateway RAS

In Windows Server 2016, il ruolo server Accesso remoto è progettato per essere eseguita anche da un router sia un server di accesso remoto; Pertanto, supporta un'ampia gamma di funzionalità. Per questa Guida alla distribuzione, è necessario solo un piccolo subset di queste funzionalità: supporto per le connessioni VPN IKEv2 e routing LAN.

IKEv2 è una protocollo descritto in Internet Engineering Task Force richiesta per i commenti 7296 di tunneling VPN. Il vantaggio principale di IKEv2 è che tollera interruzioni della connessione di rete sottostante. Ad esempio, se la connessione viene temporaneamente persa o se un utente si sposta un computer client da una rete a un altro, IKEv2 Ripristina automaticamente la connessione VPN quando viene ristabilita la connessione di rete, ovvero senza l'intervento dell'utente.

Tramite il Gateway RAS, è possibile distribuire le connessioni VPN per fornire agli utenti finali con accesso remoto alla rete e le risorse dell'organizzazione. Distribuire VPN Always On mantiene una connessione permanente tra i client e rete dell'organizzazione ogni volta che i computer remoti connessi a Internet. Con il Gateway RAS, è anche possibile creare una connessione VPN site-to-site tra due server in posizioni diverse, ad esempio tra la sede principale e una succursale e Usa Network Address Translation \(NAT\) in modo che gli utenti all'interno di rete possa accedere alle risorse esterne, ad esempio Internet. Inoltre, Gateway RAS supporta protocollo BGP (Border Gateway), che fornisce servizi di routing dinamici quando le sedi remote dispone anche di gateway edge che supportano il protocollo BGP.

È possibile gestire i gateway di servizi di accesso remoto (RAS) usando i comandi di Windows PowerShell e il Microsoft Management Console (MMC) l'accesso remoto.



## <a name="network-policy-server-nps"></a>Server dei criteri di rete

Criteri di rete consente di creare e applicare criteri di accesso di rete a livello di organizzazione per la connessione richiesta autenticazione e autorizzazione. Quando si usa criteri di rete come server RADIUS Remote Authentication Dial-In User Service (), configurare i server di accesso di rete, ad esempio i server VPN, come client RADIUS in Criteri di rete.

È inoltre possibile configurare criteri di rete utilizzati da Server dei criteri di rete per autorizzare richieste di connessione, nonché configurare l'accounting RADIUS in modo che Server dei criteri di rete registri le informazioni di accounting in file di registro sul disco rigido locale oppure in un database di Microsoft SQL Server.

Per altre informazioni, vedere [Strumentazione gestione Windows (NPS, Network Policy Server)](../../../../networking/technologies/nps/nps-top.md).


## <a name="active-directory-certificate-services"></a>Servizi certificati Active Directory

Il Server di autorità di certificazione (CA) è un'autorità di certificazione che esegue Servizi certificati Active Directory. La configurazione VPN richiede una basata su Active Directory dall'infrastruttura a chiave pubblica (PKI).

Le organizzazioni possono utilizzare Servizi certificati Active Directory per migliorare la sicurezza associando l'identità di una persona, un dispositivo o un servizio a una chiave pubblica corrispondente. Servizi certificati Active Directory include inoltre funzionalità che consentono di gestire la registrazione di certificati e le revoche in una vasta gamma di ambienti scalabili. Per ulteriori informazioni, vedere [Panoramica di servizi di Active Directory certificato](https://technet.microsoft.com/library/hh831740.aspx) e [linee guida alla progettazione di infrastruttura di chiave pubblica](https://social.technet.microsoft.com/wiki/contents/articles/2901.public-key-infrastructure-design-guidance.aspx).

Durante il completamento della distribuzione, si configurerà i seguenti modelli di certificato nell'autorità di certificazione.

-   Il modello di certificato di autenticazione utente

-   Il modello di certificato di autenticazione Server VPN

-   Il modello di certificato di autenticazione del Server dei criteri di rete

### <a name="certificate-templates"></a>Modelli di certificato

Modelli di certificato possono semplificare notevolmente l'attività di amministrazione di un'autorità di certificazione (CA) offrendo la possibilità di rilasciare certificati che sono preconfigurati per le attività selezionate. Lo snap-in MMC di modelli di certificato consente di eseguire le attività seguenti.

-   Visualizzare le proprietà per ogni modello di certificato.

-   Copiare e modificare i modelli di certificato.

-   Controllare quali utenti e computer possono leggere modelli e si registra per i certificati.

-   Eseguire altre attività amministrative relative ai modelli di certificato.

Modelli di certificato sono parte integrante di un'autorità di certificazione (CA). Sono un elemento importante dei criteri del certificato per un ambiente, ovvero il set di regole e i formati per la registrazione del certificato, l'uso e gestione.

Per altre informazioni, vedere [modelli di certificato](https://technet.microsoft.com/library/cc730705.aspx).

### <a name="digital-server-certificates"></a>Certificati digitali Server

Questa Guida alla distribuzione vengono fornite istruzioni per l'uso di servizi certificati Active Directory (AD CS) per registrare sia registrare automaticamente certificati per accesso remoto e server dell'infrastruttura Criteri di rete. Servizi certificati Active Directory consente di compilare un'infrastruttura a chiave pubblica (PKI) e fornire funzionalità di firma digitale, certificati digitali e crittografia a chiave pubblica dell'organizzazione.

Quando si utilizzano i certificati digitali server per l'autenticazione tra computer della rete, forniscano i certificati:

1.  Riservatezza tramite crittografia.

2.  Integrità tramite firme digitali.

3.  Autenticazione di associazione di chiavi del certificato con un account computer, utente o dispositivo in una rete di computer.

Per altre informazioni, vedere [Guida dettagliata di AD CS: A due livelli di distribuzione di gerarchia infrastruttura a chiave pubblica](https://social.technet.microsoft.com/wiki/contents/articles/15037.ad-cs-step-by-step-guide-two-tier-pki-hierarchy-deployment.aspx).

## <a name="active-directory-domain-services-ad-ds"></a>Servizi di dominio Active Directory

Servizi di dominio Active Directory fornisce un database distribuito che consente di archiviare e gestire informazioni sulle risorse di rete e dati specifici delle applicazioni provenienti da applicazioni abilitate all'uso di directory. Gli amministratori possono utilizzare Servizi di dominio Active Directory per organizzare gli elementi di una rete, ad esempio utenti, computer e altri dispositivi, in una struttura di contenimento gerarchica. Tale struttura include la foresta di Active Directory, i domini della foresta e le unità organizzative (OU, Organizational Unit) di ogni dominio. Un server che esegue Servizi di dominio Active Directory viene definito controller di dominio.

AD DS contiene gli account utente, gli account computer e proprietà dell'account necessari da PEAP Protected Extensible Authentication Protocol () per autenticare le credenziali dell'utente e per valutare l'autorizzazione per le richieste di connessione VPN. Per informazioni sulla distribuzione di Active Directory Domain Services, vedere Windows Server 2016 [Guida alla rete Core](../../../../networking/core-network-guide/Core-Network-Guide.md).



Durante il completamento dei passaggi di questa distribuzione, si configurerà gli elementi seguenti nel controller di dominio.

-   Abilitare la registrazione automatica dei certificati in Criteri di gruppo per computer e utenti

-   Creare il gruppo di utenti VPN

-   Creare il gruppo di server VPN

-   Creare il gruppo di server dei criteri di rete

### <a name="active-directory-users-and-computers"></a>Utenti e computer di Active Directory

Active Directory Users and Computers è un componente di dominio Active Directory che contiene gli account che rappresentano entità fisiche, ad esempio un computer, una persona o un gruppo di sicurezza. Un gruppo di sicurezza è una raccolta di account utente o computer che gli amministratori possono gestire come singola unità. Account utente e computer appartenenti a un determinato gruppo sono indicati come membri del gruppo.

Gli account utente in Active Directory Users and Computers hanno proprietà di connessione remota che valuta dei criteri di rete durante il processo di autorizzazione -, a meno che il **l'autorizzazione di accesso di rete** dell'account utente è impostata su **controllo accesso tramite criteri di rete NPS**. Si tratta dell'impostazione predefinita per tutti gli account utente. In alcuni casi, tuttavia, questa impostazione potrebbe essere una configurazione diversa che impedisce all'utente di connettersi tramite VPN. Per evitare questa eventualità, è possibile configurare il server NPS per ignorare la proprietà di connessione remota account utente.

Per altre informazioni, vedere [configurare criteri di rete per le proprietà di connessione a Account utente di ignorare](../../../../networking/technologies/nps/nps-np-configure.md#configure-nps-to-ignore-user-account-dial-in-properties).



### <a name="group-policy-management"></a>Gestione Criteri di gruppo

Gestione criteri di gruppo Abilita directory gestione basate su modifiche e della configurazione delle impostazioni utente e computer, incluse le informazioni di sicurezza e utente. Si usano criteri di gruppo per definire le configurazioni per i gruppi di utenti e computer.

Con criteri di gruppo, è possibile specificare le impostazioni per le voci del Registro di sistema, sicurezza, installazione del software, script, reindirizzamento cartelle, servizi di installazione remota e la manutenzione di Internet Explorer. Sono contenute le impostazioni di criteri di gruppo creati in un oggetto Criteri di gruppo (GPO). Associando un oggetto Criteri di gruppo di contenitori di sistema di Active Directory selezionati, siti, domini e unità organizzative, è possibile applicare le impostazioni dell'oggetto Criteri di gruppo a utenti e computer presenti in tali contenitori Active Directory. Per gestire gli oggetti Criteri di gruppo in un'organizzazione, è possibile usare gruppo di criteri di gestione Editor Microsoft Management Console (MMC).


## <a name="windows-10-vpn-clients"></a>Client VPN di Windows 10

Oltre ai componenti server, assicurarsi che i computer client configurati per l'uso di VPN siano in esecuzione Windows 10 Anniversary Update (versione 1607). I client VPN di Windows 10 devono essere aggiunto al dominio nel dominio di Active Directory.


Il client VPN di Windows 10 è ampiamente configurabile e offre numerose opzioni. Per illustrare meglio questo scenario Usa la funzionalità specifiche, nella tabella 1 identifica le categorie di funzionalità VPN e le configurazioni specifiche che fa riferimento a questa distribuzione. Si configurerà le singole impostazioni per queste funzionalità usando il provider di servizi di configurazione VPNv2 (CSP) illustrata più avanti in questa distribuzione. 

Tabella 1. Funzionalità VPN e configurazioni descritte in questa distribuzione

| **Funzionalità VPN** | **Configurazione di uno scenario di distribuzione**         |
|-----------------|-----------------------------------------------|
| Tipo di connessione | Native IKEv2                                  |
| Routing         | Lo split tunneling                               |
| Risoluzione dei nomi | Suffisso DNS e l'elenco di informazioni sul nome di dominio   |
| Attivazione      | Rilevamento di rete Always On e attendibile       |
| Autenticazione  | PEAP-TLS con i certificati utente protetto da TPM: |
---

>[!NOTE] 
>PEAP-TLS e TPM sono rispettivamente "Protected Extensible Authentication Protocol con Transport Layer Security" e "Trusted Platform Module".

### <a name="vpnv2-csp-nodes"></a>Nodi di VPNv2 CSP

In questa distribuzione, utilizzare il nodo ProfileXML VPNv2 CSP per creare il profilo VPN che viene recapitato al computer client Windows 10. I provider di servizi di configurazione (CSP) sono le interfacce che espongono diverse funzionalità di gestione all'interno del client di Windows. Concettualmente, CSP funzionano in modo analogo al funzionamento di criteri di gruppo. Nodo di configurazione che rappresentano le singole impostazioni ogni CSP. Inoltre, ad esempio le impostazioni di criteri di gruppo, è possibile collegare le impostazioni di CSP per le chiavi del Registro di sistema, file, autorizzazioni e così via. Analogamente a come utilizzare Editor Gestione criteri di gruppo per configurare gli oggetti Criteri di gruppo (GPO), si configurare nodi CSP usando una soluzione di gestione (MDM) di dispositivi mobili, ad esempio Microsoft Intune. I prodotti MDM come Intune offrono un'opzione di configurazione intuitiva che consente di configurare il provider CSP nel sistema operativo.

![Gestione dei dispositivi mobili alla configurazione CSP](../../../media/Always-On-Vpn/Vpn-Mdm.jpg)

Tuttavia, è possibile configurare alcuni nodi CSP direttamente tramite un'interfaccia utente (UI), ad esempio la Console di amministrazione di Intune. In questi casi, è necessario configurare le impostazioni di Open Mobile Alliance Uniform Resource Identifier (URI OMA) manualmente. Per configurare OMA-URI, utilizzando il protocollo OMA Device Management (OMA-DM), una specifica di gestione dispositivo universale che supportano i dispositivi di Apple, Android e Windows più recenti. Finché sono conformi alla specifica OMA-DM, tutti i prodotti MDM devono interagire con questi sistemi operativi nello stesso modo.

Windows 10 offre molte CSP, ma questa distribuzione è incentrato sull'uso di VPNv2 CSP per configurare il client VPN. VPNv2 CSP consente la configurazione di ogni impostazione del profilo VPN in Windows 10 tramite un nodo CSP univoco. Contenuti anche nel VPNv2 CSP è un nodo chiamato *ProfileXML*, che consente di configurare tutte le impostazioni in un nodo anziché singolarmente. Per altre informazioni sulle ProfileXML, vedere la sezione "Panoramica ProfileXML" più avanti in questa distribuzione. Per informazioni dettagliate su ogni nodo VPNv2 CSP, vedere la [VPNv2 CSP](https://msdn.microsoft.com/windows/hardware/commercialize/customize/mdm/vpnv2-csp).



## <a name="next-steps"></a>Passaggi successivi

- [Informazioni su alcune delle funzionalità avanzate VPN Always On](deploy/always-on-vpn-adv-options.md)

- [Iniziare a pianificare la distribuzione VPN Always On](deploy/always-on-vpn-deploy-deployment.md)


---

## <a name="related-topics"></a>Argomenti correlati
- [Supporto del software server Microsoft per le macchine virtuali di Microsoft Azure](https://support.microsoft.com/help/2721672/microsoft-server-software-support-for-microsoft-azure-virtual-machines): Questo articolo illustra i criteri di supporto per l'esecuzione di software server Microsoft nell'ambiente di macchina virtuale di Microsoft Azure (infrastructure-as-a-service).

- [Accesso remoto](../../Remote-Access.md): In questo argomento viene fornita una panoramica del ruolo del server Accesso remoto in Windows Server 2016.

- [Guida tecnica di Windows 10 VPN](https://docs.microsoft.com/windows/access-protection/vpn/vpn-guide): Questa guida illustra le scelte effettuate per i client Windows 10 nell'azienda soluzione VPN e come configurare la distribuzione. Questa guida fa riferimento a VPNv2 Configuration Service Provider (CSP) e fornisce la gestione dei dispositivi mobili le istruzioni di configurazione (MDM) con Microsoft Intune e il modello di profilo VPN per Windows 10.

- [Guida alla rete core](../../../../networking/core-network-guide/Core-Network-Guide.md): In questa guida vengono fornite istruzioni su come pianificare e distribuire i componenti di base necessari per una rete completamente funzionante e un nuovo dominio di Active Directory in una nuova foresta.

- [Domain Name System (DNS)](../../../../networking/dns/dns-top.md): In questo argomento offre una panoramica dei sistemi DNS (Domain Name). In Windows Server 2016, DNS è un ruolo del server che è possibile installare usando i comandi di Server Manager o Windows PowerShell. Se si sta installando un nuovo insieme di strutture Active Directory e un dominio, DNS viene installato automaticamente con Active Directory come server di catalogo globale per la foresta e del dominio. 

- [Panoramica di servizi certificati Active Directory](https://technet.microsoft.com/library/hh831740.aspx): Questo documento viene fornita una panoramica di servizi certificati Active Directory (AD CS) in Windows Server® 2012. Servizi certificati Active Directory è il ruolo server che consente di creare un'infrastruttura a chiave pubblica (PKI) e fornire crittografia a chiave pubblica, certificati digitali e funzionalità di firma digitale per l'organizzazione.

- [Linee guida alla progettazione di infrastruttura a chiave pubblica](https://social.technet.microsoft.com/wiki/contents/articles/2901.public-key-infrastructure-design-guidance.aspx):  In questo wiki fornisce materiale sussidiario sulla progettazione di infrastrutture a chiave pubblica (PKI). Prima di configurare una gerarchia infrastruttura a chiave pubblica e certificazione di autorità (CA), è consigliabile tenere dei criteri e il certificato pratica informativa security dell'organizzazione (CPS).

- [Guida dettagliata di AD CS: A due livelli di distribuzione di gerarchia infrastruttura a chiave pubblica](https://social.technet.microsoft.com/wiki/contents/articles/15037.ad-cs-step-by-step-guide-two-tier-pki-hierarchy-deployment.aspx): In questa Guida dettagliata descrive i passaggi necessari per configurare una configurazione di base di servizi certificati di Active Directory® (AD CS) in un ambiente lab. Servizi certificati Active Directory in Windows Server® 2008 R2 offre servizi personalizzabili per la creazione e gestione dei certificati di chiave pubblica utilizzati in sistemi di sicurezza software che utilizza tecnologie a chiave pubblica.

- [Server dei criteri di (rete NPS) di rete](../../../../networking/technologies/nps/nps-top.md): In questo argomento viene fornita una panoramica di Server dei criteri di rete in Windows Server 2016. Server dei criteri di rete consente di creare e imporre criteri di accesso di rete a livello di organizzazione per l'autorizzazione e l'autenticazione delle richieste di connessione. 

---
