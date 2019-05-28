---
title: Server dei criteri di rete
description: In questo argomento viene fornita una panoramica di Server dei criteri di rete in Windows Server 2016 e Windows Server 2019 e include collegamenti a indicazioni aggiuntive dei criteri di rete.
manager: dougkim
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 9c7a67e0-0953-479c-8736-ccb356230bde
ms.author: pashort
author: shortpatti
ms.date: 06/20/2018
ms.openlocfilehash: 0439c0f45a604f6b3ef90369f5fe77a59568d9d7
ms.sourcegitcommit: 8ba2c4de3bafa487a46c13c40e4a488bf95b6c33
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/25/2019
ms.locfileid: "66222581"
---
# <a name="network-policy-server-nps"></a>Server dei criteri di rete

>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2019

È possibile utilizzare questo argomento per una panoramica di Server dei criteri di rete in Windows Server 2016 e Windows Server 2019. Criteri di rete viene installato quando si installa la funzionalità di servizi di accesso (NPAS) e i criteri di rete in Windows Server 2016 e Server 2019.

>[!NOTE]
>Oltre a questo argomento, la documentazione dei criteri di rete seguente è disponibile.
> - [Procedure consigliate di Network Policy Server](nps-best-practices.md)
> - [Introduzione a Server dei criteri di rete](nps-getstart-top.md)
> - [Pianificare il Server dei criteri di rete](nps-plan-top.md)
> - [Distribuire Server dei criteri di rete](nps-deploy.md)
> - [Gestire i Server dei criteri di rete](nps-manage-top.md)
> - [Cmdlet di Server dei criteri in Windows PowerShell di rete](https://technet.microsoft.com/library/jj872739.aspx) per Windows Server 2016 e Windows 10
> - [Cmdlet di Server dei criteri in Windows PowerShell di rete](https://technet.microsoft.com/library/jj872739.aspx) per Windows Server 2012 R2 e Windows 8.1
> - [Cmdlet di criteri di rete in Windows PowerShell](https://technet.microsoft.com/library/jj872739.aspx) per Windows Server 2012 e Windows 8

Server dei criteri di rete consente di creare e imporre criteri di accesso di rete a livello di organizzazione per l'autorizzazione e l'autenticazione delle richieste di connessione.

È anche possibile configurare criteri di rete come proxy RADIUS Remote Authentication Dial-In User Service () per inoltrare le richieste di connessione a un remoto dei criteri di rete o un altro server RADIUS in modo che è possibile inoltrare le richieste di connessione e li inoltrano al dominio corretto per l'autenticazione e l'autorizzazione.

Criteri di rete consente all'utente in modo centralizzato, configurare e gestire l'autenticazione di accesso di rete, autorizzazione e accounting con le funzionalità seguenti:

- **Server RADIUS**. Esegue inoltre centralizzati di autenticazione, autorizzazione e accounting per wireless, l'autenticazione di commutatore, accesso remoto e le connessioni di rete privata virtuale (VPN). Quando si utilizza Server dei criteri di rete come un server RADIUS, si configurano i server di accesso alla rete, ad esempio punti di accesso wireless e server VPN, come client RADIUS in Server dei criteri di rete. È inoltre possibile configurare criteri di rete utilizzati da Server dei criteri di rete per autorizzare richieste di connessione, nonché configurare l'accounting RADIUS in modo che Server dei criteri di rete registri le informazioni di accounting in file di registro sul disco rigido locale oppure in un database di Microsoft SQL Server. Per altre informazioni, vedere [server RADIUS](#radius-server).
- **Proxy RADIUS**. Quando si usa criteri di rete come proxy RADIUS, si configurano criteri di richiesta di connessione che indicano i criteri di rete quali richieste di connessione inoltrare ad altri server RADIUS e server RADIUS che si desidera inoltrare le richieste di connessione. È inoltre possibile configurare Server dei criteri di rete per inoltrare i dati di accounting da registrare da parte di uno o più computer in un gruppo remoto di server RADIUS. Per configurare criteri di rete come server proxy RADIUS, vedere gli argomenti seguenti. Per altre informazioni, vedere [proxy RADIUS](#radius-proxy).
    - [Configurare i criteri di richiesta di connessione](nps-crp-configure.md)
- **Accounting RADIUS**. È possibile configurare criteri di rete per registrare gli eventi in un file di log locale o a un'istanza locale o remota di Microsoft SQL Server. Per altre informazioni, vedere [la registrazione di NPS](#nps-logging).

>[!IMPORTANT]
>Protezione accesso alla rete \(NAP\), Autorità registrazione integrità \(Autorità registrazione integrità\)e Host Credential Authorization Protocol \(HCAP\) deprecate in Windows Server 2012 R2, e non sono disponibili in Windows Server 2016. Se si dispone di una distribuzione di protezione accesso alla rete usando i sistemi operativi precedenti a Windows Server 2016, è possibile eseguire la migrazione della distribuzione di protezione accesso alla rete a Windows Server 2016.

È possibile configurare NPS con qualsiasi combinazione di queste funzionalità. Ad esempio, è possibile configurare uno dei criteri di rete come server RADIUS per connessioni VPN e anche come proxy RADIUS per inoltrare alcune richieste di connessione per i membri di un gruppo di server RADIUS remoto per l'autenticazione e autorizzazione in un altro dominio.

## <a name="windows-server-editions-and-nps"></a>Criteri di rete e le edizioni di Windows Server

Criteri di rete fornisce funzionalità diverse a seconda dell'edizione di Windows Server da installare.

### <a name="windows-server-2016-or-windows-server-2019-standarddatacenter-edition"></a>Windows Server 2016 o Windows Server 2019 Standard o Datacenter Edition

Con criteri di rete in Windows Server 2016 Standard o Datacenter, è possibile configurare un numero illimitato di client RADIUS e gruppi di server RADIUS remoti. È inoltre possibile configurare client RADIUS specificando un intervallo di indirizzi IP.

>[!NOTE]
>La funzionalità Servizi di accesso e criteri di rete di WIndows non è disponibile nei sistemi installati con un'opzione di installazione Server Core.

Le sezioni seguenti forniscono informazioni più dettagliate sui criteri di rete come un proxy e server RADIUS.

## <a name="radius-server-and-proxy"></a>Proxy e server RADIUS

È possibile usare criteri di rete come un server RADIUS, proxy RADIUS o entrambi.

### <a name="radius-server"></a>Server RADIUS

Criteri di rete è l'implementazione Microsoft dello standard RADIUS specificato dalla Internet Engineering Task Force \(IETF\) nelle RFC 2865 e 2866. Come server RADIUS, NPS esegue centralizzate di autenticazione, autorizzazione e accounting per connessioni remote molti tipi di accesso alla rete, inclusi switch wireless, che esegue l'autenticazione e rete privata virtuale \(VPN\) remoto l'accesso e le connessioni da router a router.

>[!NOTE]
>Per informazioni sulla distribuzione dei criteri di rete come server RADIUS, vedere [distribuire Server dei criteri di rete](nps-deploy.md).

Criteri di rete consente l'uso di un set eterogeneo di wireless, switch, accesso remoto o ad apparecchiature VPN. È possibile usare criteri di rete con il servizio di accesso remoto, che è disponibile in Windows Server 2016.

Criteri di rete Usa un Active Directory Domain Services \(Active Directory Domain Services\) dominio o gli account utente di gestione di account di sicurezza (SAM) locale per autenticare le credenziali utente per i tentativi di connessione del database. Quando un server dei criteri di rete è un membro di un dominio di Active Directory Domain Services, criteri di rete Usa il servizio directory come database degli account utente e fa parte di una soluzione single sign-on. Lo stesso insieme di credenziali viene usato per il controllo di accesso di rete \(autenticare e autorizzare l'accesso a una rete\) e accedere a un dominio di Active Directory Domain Services.

>[!NOTE]
>Criteri di rete Usa le proprietà dial-dei criteri di rete e account utente di autorizzare una connessione.

Provider di servizi Internet \(ISP\) e le organizzazioni che gestiscono l'accesso alla rete sono in grado di gestire tutti i tipi di accesso alla rete da un singolo punto dell'amministrazione, indipendentemente dal tipo di accesso alla rete apparecchiature usate. Lo standard RADIUS supporta questa funzionalità negli ambienti sia omogenei ed eterogenei. RADIUS è un protocollo client-server che consente a dispositivi di accesso alla rete (usato come client RADIUS) per inviare richieste di autenticazione e accounting in un server RADIUS.

Un server RADIUS può accedere alle informazioni sull'account utente e possa controllare le credenziali di autenticazione di accesso di rete. Se vengono autenticate le credenziali dell'utente e il tentativo di connessione è autorizzato, il server RADIUS autorizza l'accesso utente in base alle condizioni specificate e quindi registra la connessione di accesso di rete in un log di accounting. L'uso di RADIUS consente l'autenticazione dell'utente di accesso di rete, autorizzazione e i dati di accounting da raccogliere e gestire in una posizione centrale, anziché in ogni server di accesso.

#### <a name="using-nps-as-a-radius-server"></a>Uso dei criteri di rete come server RADIUS

È possibile usare criteri di rete come server RADIUS quando:

- Si usa un dominio di Active Directory Domain Services o il database degli account utente di SAM locale come database degli account utente per i client di accesso. 
- Si usa l'accesso remoto su più server di accesso remoto, i server VPN, o connessione a richiesta router e si vuole centralizzare la configurazione della connessione di registrazione e l'accounting e i criteri di rete. 
- Si Esternalizza la remote, VPN o wireless access per un provider di servizi. I server di accesso usano RADIUS per autenticare e autorizzare le connessioni eseguite dai membri della propria organizzazione.
- Si vuole centralizzare l'autenticazione, autorizzazione e accounting per un set eterogeneo di server di accesso.

La figura seguente mostra i criteri di rete come server RADIUS per un'ampia gamma di client di accesso. 

![NPS come Server RADIUS](../../media/Nps-Server/Nps-Server.jpg)

### <a name="radius-proxy"></a>Proxy RADIUS

Come proxy RADIUS, dei criteri di rete inoltra i messaggi di autenticazione e accounting NPS e altri server RADIUS. È possibile usare criteri di rete come proxy RADIUS per il routing del raggio di messaggi tra i client RADIUS \(server di accesso alla rete è l'acronimo\) e server RADIUS che eseguono l'autenticazione, autorizzazione e accounting per il tentativo di connessione. 

Quando usato come proxy RADIUS, NPS è un passaggio centrale o un punto di routing tramite quale flusso di messaggi accesso e l'accounting RADIUS. Criteri di rete registra le informazioni in un log di accounting sui messaggi che vengono inoltrati.

#### <a name="using-nps-as-a-radius-proxy"></a>Uso dei criteri di rete come proxy RADIUS

È possibile usare criteri di rete come proxy RADIUS quando:

- Sei un provider di servizi che offre servizi di accesso di rete wireless, VPN o connessioni remote in outsourcing a più clienti. Il server NAS inviare richieste di connessione per il proxy RADIUS NPS. In base la parte dell'area di autenticazione del nome utente nella richiesta di connessione, il proxy RADIUS dei criteri di rete inoltra la richiesta di connessione a un server RADIUS è gestito dal cliente e possa autenticare e autorizzare il tentativo di connessione.
- Si vuole fornire l'autenticazione e autorizzazione per gli account utente che non sono membri del dominio in cui i criteri di rete è un membro o un altro dominio che abbia un trust bidirezionale con il dominio in cui i criteri di rete è un membro. Ciò include gli account in domini non attendibili, domini con trust unidirezionali e di altre foreste. Invece di configurare i server di accesso per inviare le richieste di connessione a un server NPS RADIUS, è possibile configurare in modo da inviare le richieste di connessione a un proxy RADIUS NPS. Il proxy RADIUS dei criteri di rete utilizza la parte del nome dell'area di autenticazione del nome utente e inoltra la richiesta a un criteri di rete nel dominio corretto o nella foresta. Tentativi di connessione per l'utente degli account in un dominio o foresta possono essere autenticati per i server NAS in un altro dominio o foresta.
- Si desidera eseguire l'autenticazione e autorizzazione con un database che non è un database di account di Windows. In questo caso, le richieste di connessione che corrisponde al nome specificato dell'area di autenticazione vengono inoltrate a un server RADIUS, che può accedere a un altro database degli account utente e i dati di autorizzazione. Esempi di altri database utente database Structured Query Language (SQL) e Novell Directory Services (NDS).
- Si desidera elaborare un numero elevato di richieste di connessione. In questo caso, invece di configurare i client RADIUS per provare a bilanciare le richieste di connessione e accounting tra più server RADIUS, è possibile configurare in modo da inviare le richieste di accounting e la connessione a un proxy RADIUS NPS. Il proxy RADIUS dei criteri di rete consente di bilanciare il carico della connessione e contabilità delle richieste tra più server RADIUS e aumenta l'elaborazione di un numero elevato di client RADIUS e le autenticazioni al secondo.
- Si vuole fornire l'autenticazione RADIUS e autorizzazione per i provider di servizi in outsourcing e ridurre al minimo la configurazione del firewall intranet. Un firewall intranet è tra la rete perimetrale (la rete tra la rete intranet e Internet) e intranet. Inserendo un criteri di rete nella rete perimetrale, il firewall tra la rete perimetrale e intranet deve consentire il traffico tra i criteri di rete e più controller di dominio. Sostituendo i criteri di rete con un proxy di criteri di rete, il firewall deve consentire solo il traffico RADIUS tra il proxy di criteri di rete e NPSs uno o più all'interno dell'intranet.

La figura seguente mostra i criteri di rete come proxy RADIUS tra i client RADIUS e server RADIUS.

![Criteri di rete come Proxy RADIUS](../../media/Nps-Proxy/Nps-Proxy.jpg)


Con criteri di rete, le organizzazioni possono anche outsourcing dell'infrastruttura di accesso remoto a un provider di servizi, mantenendo il controllo tramite l'autenticazione, autorizzazione e accounting.

È possibile creare configurazioni di criteri di rete per gli scenari seguenti:

- Accesso wireless
- Accesso remoto di connessioni remote o VPN (VPN) dell'organizzazione
- Connessione remota in outsourcing o accesso wireless
- Accesso a Internet
- Accesso autenticato alle risorse di rete extranet per partner commerciali

## <a name="radius-server-and-radius-proxy-configuration-examples"></a>Esempi di configurazione del proxy RADIUS e server RADIUS

Gli esempi di configurazione seguenti illustrano come configurare criteri di rete come server RADIUS e un proxy RADIUS.

**NPS come server RADIUS**. In questo esempio, criteri di rete è configurato come server RADIUS, il criterio di richiesta connessione predefinito è l'unico tipo di criteri configurato e tutte le richieste di connessione vengono elaborate da criteri di rete locale. I criteri di rete possono autenticare e autorizzare gli utenti con account nel dominio di NPS e nei domini trusted.

**Criteri di rete come proxy RADIUS**. In questo esempio, i criteri di rete è configurato come proxy RADIUS che inoltra le richieste di connessione di gruppi di server RADIUS remoti in due domini non trusted. Il criterio di richiesta di connessione predefinita viene eliminato e vengono creati due nuovi criteri di richiesta di connessione per inoltrare le richieste per ognuno dei due domini non attendibili. In questo esempio, criteri di rete non elabora le richieste di connessione nel server locale. 

**NPS come server RADIUS sia come proxy RADIUS**. Oltre a criterio richiesta connessione predefinito, che indica che le richieste di connessione vengono elaborate in locale, viene creato un nuovo criterio richiesta di connessione che inoltra le richieste di connessione a un criteri di rete o un altro server RADIUS in un dominio non trusted. Questo secondo set di criteri è denominato criterio del Proxy. In questo esempio, i criteri Proxy compare per primo nell'elenco ordinato dei criteri. Se la richiesta di connessione corrisponde al criterio del Proxy, la richiesta di connessione viene inoltrata al server RADIUS nel gruppo di server RADIUS remoto. Se la richiesta di connessione non soddisfa il criterio di Proxy, ma corrisponde al criterio di richiesta connessione predefinito, dei criteri di rete elabora la richiesta di connessione nel server locale. Se la richiesta di connessione non corrisponde a dei due criteri, vengono eliminate.

**NPS come server RADIUS con server remoti accounting**. In questo esempio, i criteri di rete locale non è configurato per eseguire l'accounting e il criterio di richiesta connessione predefinito viene modificato in modo che i messaggi di accounting RADIUS vengono inoltrati da un criteri di rete o un altro server RADIUS in un gruppo di server RADIUS remoti. Anche se vengono inoltrati messaggi di accounting, autenticazione e autorizzazione dei messaggi non vengono inoltrati e i criteri di rete locale esegue queste funzioni per il dominio locale e tutti i domini trusted.

**Criteri di rete con server RADIUS remoto a mapping degli utenti Windows**. In questo esempio, criteri di rete funge sia da server RADIUS che come proxy RADIUS per ogni richiesta di connessione da inoltrare la richiesta di autenticazione a un server RADIUS remoto quando si usa un account utente locale di Windows per l'autorizzazione. Questa configurazione viene implementata mediante la configurazione di RADIUS remoto all'attributo di Mapping di utenti di Windows come condizione per il criterio di richiesta di connessione. \(Inoltre, un account utente deve essere creato in locale nel server RADIUS che ha lo stesso nome dell'account utente remoto rispetto al quale l'autenticazione viene eseguita dal server RADIUS remoti.\)

## <a name="configuration"></a>Configurazione

Per configurare criteri di rete come server RADIUS, è possibile usare la configurazione standard o avanzate di configurazione nella console Criteri di rete o in Server Manager. Per configurare criteri di rete come proxy RADIUS, è necessario usare la configurazione avanzata.

### <a name="standard-configuration"></a>Configurazione standard

Con la configurazione standard, sono disponibili procedure guidate che consentono di configurare criteri di rete per gli scenari seguenti:

- Server RADIUS per connessioni remote o VPN
- Server RADIUS per connessioni 802.1X wireless o cablate

Per configurare criteri di rete tramite una procedura guidata, aprire la console Criteri di rete e quindi fare clic sul collegamento che apre la procedura guidata selezionare uno degli scenari precedenti.

### <a name="advanced-configuration"></a>Configurazione avanzata

Quando si usa la configurazione avanzata, configurando manualmente NPS come server RADIUS o proxy RADIUS. 

Per configurare criteri di rete tramite la configurazione avanzata, aprire la console Criteri di rete e quindi fare clic sulla freccia accanto a **configurazione avanzata** per espandere questa sezione.

Vengono forniti i seguenti elementi di configurazione avanzata.

#### <a name="configure-radius-server"></a>Configurare il server RADIUS

Per configurare criteri di rete come server RADIUS, è necessario configurare i client RADIUS, criteri di rete e l'accounting RADIUS.

Per istruzioni su come rendere tali configurazioni, vedere gli argomenti seguenti.

- [Configurare i client RADIUS](nps-radius-clients-configure.md)
- [Configurare i criteri di rete](nps-np-configure.md)
- [Configurare l'Accounting Server dei criteri di rete](nps-accounting-configure.md)

#### <a name="configure-radius-proxy"></a>Configurare il proxy RADIUS

Per configurare criteri di rete come proxy RADIUS, è necessario configurare i client RADIUS, gruppi di server RADIUS remoti e i criteri di richiesta di connessione.

Per istruzioni su come rendere tali configurazioni, vedere gli argomenti seguenti.

- [Configurare i client RADIUS](nps-radius-clients-configure.md)
- [Configurare gruppi di Server RADIUS remoti](nps-crp-rrsg-configure.md)
- [Configurare i criteri di richiesta di connessione](nps-crp-configure.md)

## <a name="nps-logging"></a>Registrazione dei criteri di rete

La registrazione dei criteri di rete è l'acronimo di accounting RADIUS. Configurare la registrazione dei criteri di rete in base ai requisiti se criteri di rete viene utilizzato come server RADIUS, proxy o qualsiasi combinazione di queste configurazioni.

Per configurare la registrazione dei criteri di rete, è necessario configurare gli eventi da registrare e visualizzare il Visualizzatore eventi e quindi determinare le altre informazioni che si desidera registrare. Inoltre, è necessario decidere se si desidera registrare l'autenticazione dell'utente e le informazioni di accounting nei file di log di testo archiviati nel computer locale o a un database di SQL Server in un computer remoto o nel computer locale.

Per altre informazioni, vedere [configurare del Accounting Server dei criteri di rete](nps-accounting-configure.md).
