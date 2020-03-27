---
title: Server dei criteri di rete
description: In questo argomento viene fornita una panoramica di server dei criteri di rete in Windows Server 2016 e Windows Server 2019 e sono inclusi collegamenti a indicazioni aggiuntive su NPS.
manager: dougkim
ms.prod: windows-server
ms.technology: networking
ms.topic: article
ms.assetid: 9c7a67e0-0953-479c-8736-ccb356230bde
ms.author: lizross
author: eross-msft
ms.date: 06/20/2018
ms.openlocfilehash: b509bb14757fab6ebd32490d0146b2801bc8e9f1
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/26/2020
ms.locfileid: "80315619"
---
# <a name="network-policy-server-nps"></a>Server dei criteri di rete

>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2019

È possibile utilizzare questo argomento per una panoramica di server dei criteri di rete in Windows Server 2016 e Windows Server 2019. NPS viene installato quando si installa la funzionalità Servizi di accesso e criteri di rete (NPAS) in Windows Server 2016 e server 2019.

> [!NOTE]
> Oltre a questo argomento, è disponibile la seguente documentazione di NPS.
> - [Procedure consigliate per il server dei criteri di rete](nps-best-practices.md)
> - [Introduzione con server dei criteri di rete](nps-getstart-top.md)
> - [Pianificare il server dei criteri di rete](nps-plan-top.md)
> - [Distribuire il server dei criteri di rete](nps-deploy.md)
> - [Gestire il server dei criteri di rete](nps-manage-top.md)
> - [Cmdlet di server dei criteri di rete in Windows PowerShell](https://technet.microsoft.com/library/jj872739.aspx) per windows server 2016 e Windows 10
> - [Cmdlet di server dei criteri di rete in Windows PowerShell](https://technet.microsoft.com/library/jj872739.aspx) per windows Server 2012 R2 e Windows 8.1
> - [Cmdlet NPS in Windows PowerShell](https://technet.microsoft.com/library/jj872739.aspx) per windows Server 2012 e Windows 8

Server dei criteri di rete consente di creare e imporre criteri di accesso di rete a livello di organizzazione per l'autorizzazione e l'autenticazione delle richieste di connessione.

È anche possibile configurare NPS come proxy di Remote Authentication Dial-In User Service (RADIUS) per l'inoltro delle richieste di connessione a un server dei criteri di accesso remoto o a un altro server RADIUS in modo che sia possibile eseguire il bilanciamento del carico delle richieste di connessione e di inoltro al dominio corretto per l'autenticazione e autorizzazione.

Server dei criteri di rete consente di configurare e gestire centralmente l'autenticazione, l'autorizzazione e l'accounting di accesso alla rete con le seguenti funzionalità:

- **Server RADIUS**. Server dei criteri di rete esegue l'autenticazione, l'autorizzazione e la contabilità centralizzate per la connessione wireless, l'opzione di autenticazione, l'accesso remoto e le connessioni VPN (Virtual Private Network). Quando si utilizza Server dei criteri di rete come un server RADIUS, si configurano i server di accesso alla rete, ad esempio punti di accesso wireless e server VPN, come client RADIUS in Server dei criteri di rete. È inoltre possibile configurare criteri di rete utilizzati da Server dei criteri di rete per autorizzare richieste di connessione, nonché configurare l'accounting RADIUS in modo che Server dei criteri di rete registri le informazioni di accounting in file di registro sul disco rigido locale oppure in un database di Microsoft SQL Server. Per ulteriori informazioni, vedere [server RADIUS](#radius-server).
- **Proxy RADIUS**. Quando si usa NPS come proxy RADIUS, si configurano i criteri di richiesta di connessione che indicano al server dei criteri di accesso quali richieste di connessione inviare ad altri server RADIUS e a quali server RADIUS si vuole inviare le richieste di connessione. È inoltre possibile configurare Server dei criteri di rete per inoltrare i dati di accounting da registrare da parte di uno o più computer in un gruppo remoto di server RADIUS. Per configurare NPS come server proxy RADIUS, vedere gli argomenti seguenti. Per altre informazioni, vedere [proxy RADIUS](#radius-proxy).
    - [Configurare i criteri di richiesta di connessione](nps-crp-configure.md)
- **Accounting RADIUS**. È possibile configurare NPS per registrare gli eventi in un file di log locale o in un'istanza locale o remota di Microsoft SQL Server. Per ulteriori informazioni, vedere la pagina relativa alla [registrazione di NPS](#nps-logging).

> [!IMPORTANT]
> Protezione accesso alla rete \(NAP\), Autorità registrazione integrità \(Autorità registrazione integrità\)e protocollo Host Credential Authorization Protocol \(HCAP\) sono stati deprecati in Windows Server 2012 R2 e non sono disponibili in Windows Server 2016. Se si dispone di una distribuzione di protezione accesso alla rete con sistemi operativi precedenti a Windows Server 2016, non è possibile eseguire la migrazione della distribuzione NAP a Windows Server 2016.

È possibile configurare NPS con qualsiasi combinazione di queste funzionalità. È ad esempio possibile configurare un server dei criteri di rete come server RADIUS per le connessioni VPN e anche come proxy RADIUS per l'inoltro di alcune richieste di connessione ai membri di un gruppo di server RADIUS remoti per l'autenticazione e l'autorizzazione in un altro dominio.

## <a name="windows-server-editions-and-nps"></a>Edizioni di Windows Server e NPS

NPS offre funzionalità diverse a seconda dell'edizione di Windows Server installata.

### <a name="windows-server-2016-or-windows-server-2019-standarddatacenter-edition"></a>Windows Server 2016 o Windows Server 2019 standard/Datacenter Edition

Con server dei criteri di gruppo in Windows Server 2016 standard o Datacenter, è possibile configurare un numero illimitato di client RADIUS e di gruppi di server RADIUS remoti. È possibile inoltre configurare client RADIUS specificando un intervallo di indirizzi IP.

> [!NOTE]
> La funzionalità Servizi di accesso e criteri di rete di WIndows non è disponibile nei sistemi installati con un'opzione di installazione dei componenti di base del server.

Nelle sezioni seguenti vengono fornite informazioni più dettagliate su NPS come server e proxy RADIUS.

## <a name="radius-server-and-proxy"></a>Server e proxy RADIUS

È possibile utilizzare NPS come server RADIUS, proxy RADIUS o entrambi.

### <a name="radius-server"></a>Server RADIUS

Server dei criteri di rete è l'implementazione Microsoft dello standard RADIUS specificato da Internet Engineering Task Force \(IETF\) in RFC 2865 e 2866. Come server RADIUS, server dei criteri di rete esegue l'autenticazione, l'autorizzazione e la contabilità centralizzata della connessione per molti tipi di accesso alla rete, tra cui wireless, commutatore di autenticazione, connessione remota e rete privata virtuale \(VPN\) accesso remoto e connessioni da router a router.

> [!NOTE]
> Per informazioni sulla distribuzione di server dei criteri di rete come server RADIUS, vedere [distribuire un server dei criteri di rete](nps-deploy.md).

NPS consente l'uso di un set eterogeneo di apparecchiature wireless, Switch, Remote Access o VPN. È possibile usare NPS con il servizio di accesso remoto, disponibile in Windows Server 2016.

NPS utilizza un Active Directory Domain Services \(dominio AD DS\) o il database degli account utente di gestione degli account di protezione (SAM) locale per autenticare le credenziali utente per i tentativi di connessione. Quando un server che esegue NPS è membro di un dominio di servizi di dominio Active Directory, NPS usa il servizio directory come database degli account utente e fa parte di una soluzione Single Sign-On. Lo stesso set di credenziali viene utilizzato per il controllo di accesso alla rete \(l'autenticazione e l'autorizzazione dell'accesso a una rete\) e per accedere a un dominio di servizi di dominio Active Directory.

> [!NOTE]
> NPS usa le proprietà di connessione remota dell'account utente e dei criteri di rete per autorizzare una connessione.

I provider di servizi Internet \(ISP\) e le organizzazioni che gestiscono l'accesso alla rete hanno la maggiore sfida di gestire tutti i tipi di accesso alla rete da un singolo punto di amministrazione, indipendentemente dal tipo di apparecchiature di accesso alla rete usate. Lo standard RADIUS supporta questa funzionalità in ambienti omogenei ed eterogenei. RADIUS è un protocollo client-server che consente alle apparecchiature di accesso alla rete, utilizzate come client RADIUS, di inviare richieste di autenticazione e accounting a un server RADIUS.

Un server RADIUS ha accesso alle informazioni sull'account utente e può controllare le credenziali di autenticazione dell'accesso alla rete. Se le credenziali dell'utente vengono autenticate e il tentativo di connessione è autorizzato, il server RADIUS autorizza l'accesso utente in base alle condizioni specificate e quindi registra la connessione di accesso alla rete in un log contabilità. L'uso di RADIUS consente ai dati di autenticazione, autorizzazione e accounting di accesso alla rete di essere raccolti e mantenuti in una posizione centrale, anziché in ogni server di accesso.

#### <a name="using-nps-as-a-radius-server"></a>Uso di NPS come server RADIUS

È possibile utilizzare NPS come server RADIUS nei casi seguenti:

- Si sta usando un dominio di servizi di dominio Active Directory o il database degli account utente SAM locale come database dell'account utente per i client di accesso. 
- Si usa l'accesso remoto su più server di connessione remota, server VPN o router di connessione a richiesta e si desidera centralizzare sia la configurazione dei criteri di rete che la registrazione e l'accounting della connessione. 
- Si sta esternalizzando l'accesso remoto, VPN o wireless a un provider di servizi. I server di accesso usano RADIUS per autenticare e autorizzare le connessioni effettuate dai membri dell'organizzazione.
- Si desidera centralizzare l'autenticazione, l'autorizzazione e l'accounting per un set eterogeneo di server di accesso.

Nella figura seguente viene illustrato il server dei criteri di accesso come server RADIUS per un'ampia gamma di client Access. 

![Server dei criteri di server come server RADIUS](../../media/Nps-Server/Nps-Server.jpg)

### <a name="radius-proxy"></a>Proxy RADIUS

Come proxy RADIUS, NPS invia i messaggi di autenticazione e accounting ai server dei criteri di server e ad altri server RADIUS. È possibile utilizzare server dei criteri di rete come proxy RADIUS per fornire il routing dei messaggi RADIUS tra i client RADIUS \(anche detti server di accesso alla rete\) e i server RADIUS che eseguono l'autenticazione, l'autorizzazione e l'accounting per il tentativo di connessione. 

Quando viene usato come proxy RADIUS, server dei criteri di servizio è un punto di routing o di routing centrale attraverso il quale vengono inviati i messaggi di accesso e accounting RADIUS. NPS registra le informazioni in un log di contabilità sui messaggi che vengono inviati.

#### <a name="using-nps-as-a-radius-proxy"></a>Uso di NPS come proxy RADIUS

È possibile utilizzare NPS come proxy RADIUS nei casi seguenti:

- Si è un provider di servizi che offre servizi di accesso remoto in outsourcing, VPN o accesso alla rete wireless a più clienti. I server NAS inviano richieste di connessione al proxy RADIUS NPS. In base alla parte dell'area di autenticazione del nome utente nella richiesta di connessione, il proxy RADIUS NPS Invia la richiesta di connessione a un server RADIUS gestito dal cliente ed è in grado di autenticare e autorizzare il tentativo di connessione.
- Si desidera fornire l'autenticazione e l'autorizzazione per gli account utente che non sono membri del dominio in cui NPS è un membro o un altro dominio con una relazione di trust bidirezionale con il dominio in cui il server dei criteri di dominio è membro. Sono inclusi gli account in domini non trusted, domini trusted unidirezionali e altre foreste. Anziché configurare i server di accesso per l'invio delle richieste di connessione a un server RADIUS NPS, è possibile configurarli per l'invio delle richieste di connessione a un proxy RADIUS NPS. Il proxy RADIUS NPS usa la parte relativa al nome dell'area di autenticazione del nome utente e la invia a un server dei criteri di dominio nel dominio o nella foresta corretta. I tentativi di connessione per gli account utente in un dominio o in una foresta possono essere autenticati per i NAS in un altro dominio o insieme di strutture.
- Si desidera eseguire l'autenticazione e l'autorizzazione utilizzando un database che non è un database di account di Windows. In questo caso, le richieste di connessione che corrispondono a un nome dell'area di autenticazione specificato vengono inviate a un server RADIUS, che ha accesso a un database diverso di account utente e dati di autorizzazione. Esempi di altri database utente includono i database di Novell Directory Services (NDS) e Structured Query Language (SQL).
- Si desidera elaborare un numero elevato di richieste di connessione. In questo caso, invece di configurare i client RADIUS per tentare di bilanciare le richieste di connessione e accounting tra più server RADIUS, è possibile configurarli per l'invio delle richieste di connessione e accounting a un proxy RADIUS NPS. Il proxy RADIUS NPS bilancia dinamicamente il carico delle richieste di connessione e accounting tra più server RADIUS e aumenta l'elaborazione di un numero elevato di client RADIUS e di autenticazioni al secondo.
- Si desidera fornire l'autenticazione e l'autorizzazione RADIUS per i provider di servizi in outsourcing e ridurre al minimo la configurazione del firewall Intranet. Un firewall Intranet è tra la rete perimetrale (la rete tra Intranet e Internet) e la Intranet. Inserendo un server dei criteri di rete nella rete perimetrale, il firewall tra la rete perimetrale e la Intranet deve consentire il flusso del traffico tra il server dei criteri di rete e più controller di dominio. Sostituendo il server dei criteri di rete con un proxy NPS, il firewall deve consentire solo il flusso del traffico RADIUS tra il proxy server dei criteri di rete e uno o più NPSs all'interno della Intranet.

La figura seguente mostra NPS come un proxy RADIUS tra i client RADIUS e i server RADIUS.

![Server dei criteri di server come proxy RADIUS](../../media/Nps-Proxy/Nps-Proxy.jpg)

Con NPS, le organizzazioni possono anche esternalizzare l'infrastruttura di accesso remoto a un provider di servizi, mantenendo al tempo stesso il controllo sull'autenticazione, l'autorizzazione e l'accounting degli utenti.

È possibile creare configurazioni NPS per gli scenari seguenti:

- Accesso wireless
- Accesso remoto dell'organizzazione o VPN (Virtual Private Network)
- Accesso remoto o wireless in outsourcing
- Accesso a Internet
- Accesso autenticato alle risorse Extranet per partner commerciali

## <a name="radius-server-and-radius-proxy-configuration-examples"></a>Esempi di configurazione del server RADIUS e del proxy RADIUS

Gli esempi di configurazione seguenti illustrano come è possibile configurare NPS come un server RADIUS e un proxy RADIUS.

**NPS come server RADIUS**. In questo esempio, NPS è configurato come server RADIUS, i criteri di richiesta di connessione predefiniti sono gli unici criteri configurati e tutte le richieste di connessione vengono elaborate dal server dei criteri di accesso locale. NPS è in grado di autenticare e autorizzare gli utenti i cui account si trovano nel dominio del server dei criteri di dominio e in domini trusted.

**NPS come proxy RADIUS**. In questo esempio, il server dei criteri di gruppo è configurato come proxy RADIUS che consente di inviare le richieste di connessione ai gruppi di server RADIUS remoti in due domini non trusted. Il criterio di richiesta di connessione predefinito viene eliminato e vengono creati due nuovi criteri di richiesta di connessione per l'invio delle richieste a ognuno dei due domini non trusted. In questo esempio, NPS non elabora le richieste di connessione nel server locale. 

**NPS come server RADIUS e proxy RADIUS**. Oltre ai criteri di richiesta di connessione predefiniti, che indicano che le richieste di connessione vengono elaborate localmente, viene creato un nuovo criterio di richiesta di connessione che inoltra le richieste di connessione a un server dei criteri di dominio o un altro server RADIUS in un dominio non trusted. Il secondo criterio è denominato criterio proxy. In questo esempio il criterio proxy viene visualizzato per primo nell'elenco ordinato dei criteri. Se la richiesta di connessione corrisponde al criterio proxy, la richiesta di connessione viene trasmessa al server RADIUS nel gruppo di server RADIUS remoti. Se la richiesta di connessione non corrisponde ai criteri del proxy ma corrisponde ai criteri di richiesta di connessione predefiniti, NPS elabora la richiesta di connessione nel server locale. Se la richiesta di connessione non corrisponde a uno dei due criteri, viene ignorata.

**NPS come server RADIUS con server di contabilità remoto**. In questo esempio, il server dei criteri di gruppo locale non è configurato per eseguire l'accounting e i criteri di richiesta di connessione predefiniti vengono rivisti in modo che i messaggi di accounting RADIUS vengano inviati a un server dei criteri di gruppo o un altro server RADIUS in un gruppo di server RADIUS remoto Sebbene i messaggi di contabilità vengano inviati, i messaggi di autenticazione e autorizzazione non vengono inviati e il server dei criteri di dominio locale esegue queste funzioni per il dominio locale e per tutti i domini trusted.

**NPS con RADIUS remoto al mapping degli utenti di Windows**. In questo esempio, NPS funge sia da server RADIUS sia come proxy RADIUS per ogni singola richiesta di connessione tramite l'inoltro della richiesta di autenticazione a un server RADIUS remoto utilizzando un account utente di Windows locale per l'autorizzazione. Questa configurazione viene implementata configurando il raggio remoto in attributo mapping utenti Windows come condizione dei criteri di richiesta di connessione. \(inoltre, è necessario creare un account utente localmente nel server RADIUS con lo stesso nome dell'account utente remoto sul quale viene eseguita l'autenticazione da parte del server RADIUS remoto.\)

## <a name="configuration"></a>Configurazione

Per configurare NPS come server RADIUS, è possibile usare la configurazione standard o la configurazione avanzata nella console NPS o in Server Manager. Per configurare NPS come proxy RADIUS, è necessario utilizzare la configurazione avanzata.

### <a name="standard-configuration"></a>Configurazione standard

Con la configurazione standard, vengono fornite procedure guidate che consentono di configurare NPS per gli scenari seguenti:

- Server RADIUS per connessioni remote o VPN
- Server RADIUS per connessioni 802.1X wireless o cablate

Per configurare NPS mediante una procedura guidata, aprire la console NPS, selezionare uno degli scenari precedenti, quindi fare clic sul collegamento che consente di aprire la procedura guidata.

### <a name="advanced-configuration"></a>Configurazione avanzata

Quando si usa la configurazione avanzata, è necessario configurare manualmente NPS come server RADIUS o proxy RADIUS. 

Per configurare NPS utilizzando la configurazione avanzata, aprire la console NPS, quindi fare clic sulla freccia accanto a **configurazione avanzata** per espandere questa sezione.

Vengono forniti gli elementi di configurazione avanzati seguenti.

#### <a name="configure-radius-server"></a>Configurare il server RADIUS

Per configurare server dei criteri di rete come server RADIUS, è necessario configurare i client RADIUS, i criteri di rete e l'accounting RADIUS.

Per istruzioni su come eseguire queste configurazioni, vedere gli argomenti seguenti.

- [Configurare i client RADIUS](nps-radius-clients-configure.md)
- [Configurare i criteri di rete](nps-np-configure.md)
- [Configurare l'accounting del server dei criteri di rete](nps-accounting-configure.md)

#### <a name="configure-radius-proxy"></a>Configurare il proxy RADIUS

Per configurare NPS come proxy RADIUS, è necessario configurare i client RADIUS, i gruppi di server RADIUS remoti e i criteri di richiesta di connessione.

Per istruzioni su come eseguire queste configurazioni, vedere gli argomenti seguenti.

- [Configurare i client RADIUS](nps-radius-clients-configure.md)
- [Configurare gruppi di server RADIUS remoti](nps-crp-rrsg-configure.md)
- [Configurare i criteri di richiesta di connessione](nps-crp-configure.md)

## <a name="nps-logging"></a>Registrazione NPS

La registrazione NPS è detta anche accounting RADIUS. Configurare il server dei criteri di accesso ai propri requisiti se NPS viene usato come server RADIUS, proxy o qualsiasi combinazione di queste configurazioni.

Per configurare la registrazione NPS, è necessario configurare gli eventi che si desidera registrare e visualizzare con Visualizzatore eventi, quindi determinare le altre informazioni che si desidera registrare. Inoltre, è necessario decidere se si desidera registrare le informazioni di autenticazione e di accounting degli utenti nei file di log di testo archiviati nel computer locale o in un database SQL Server nel computer locale o in un computer remoto.

Per ulteriori informazioni, vedere [configurare l'accounting del server dei criteri di rete](nps-accounting-configure.md).
