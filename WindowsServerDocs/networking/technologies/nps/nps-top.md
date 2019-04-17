---
title: Server dei criteri di rete (NPS)
description: In questo argomento viene fornita una panoramica di Server dei criteri di rete in Windows Server 2016 e include collegamenti a indicazioni aggiuntive sui criteri di rete.
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 9c7a67e0-0953-479c-8736-ccb356230bde
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 8f48cbdaec82e9e60f734a4a8949082187b13166
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/28/2018
---
# <a name="network-policy-server-nps"></a>Server dei criteri di rete (NPS)

>Si applica a: Windows Server (canale annuale e virgola), Windows Server 2016

È possibile utilizzare questo argomento per una panoramica di Server dei criteri di rete in Windows Server 2016.

>[!NOTE]
>Oltre a questo argomento, è disponibile la seguente documentazione dei criteri di rete.
> - [Rete criteri procedure consigliate per Server](nps-best-practices.md)
> - [Introduzione a Server dei criteri di rete](nps-getstart-top.md)
> - [Pianificare i Server dei criteri di rete](nps-plan-top.md)
> - [Distribuire Server dei criteri di rete](nps-deploy.md)
> - [Gestire Server dei criteri di rete](nps-manage-top.md)
> - [Cmdlet di Server dei criteri di criteri di rete](https://technet.microsoft.com/library/jj872739.aspx) per Windows Server 2016 e Windows 10
> - [Cmdlet di Server dei criteri di criteri in Windows PowerShell di rete](https://technet.microsoft.com/library/jj872739.aspx) per Windows Server 2012 R2 e Windows 8.1
> - [Cmdlet di criteri di rete in Windows PowerShell](https://technet.microsoft.com/library/jj872739.aspx) per Windows Server 2012 e Windows 8

Server dei criteri di rete (NPS) consente di creare e imporre criteri di accesso di rete a livello di organizzazione per l'autenticazione e autorizzazione.

È inoltre possibile configurare criteri di rete come proxy RADIUS Remote Authentication Dial-In User Service () per inoltrare le richieste di connessione a un server NPS remoto o un altro server RADIUS in modo che è possibile caricare bilanciare le richieste di connessione e li trasmette al dominio corretto per l'autenticazione e autorizzazione.

Criteri di rete consente di configurare e gestire centralmente le autenticazione di accesso di rete, autorizzazione e accounting con le funzionalità seguenti:

- **Server RADIUS**. Criteri di rete esegue centralizzato l'autenticazione, autorizzazione e accounting per wireless, l'autenticazione di commutatore, accesso remoto e connessioni di rete privata virtuale (VPN). Quando si utilizza criteri di rete come server RADIUS, si configurano server di accesso di rete, ad esempio punti di accesso wireless e server VPN, come client RADIUS in Criteri di rete. È inoltre configurare criteri di rete che utilizza criteri di rete per autorizzare le richieste di connessione ed è possibile configurare l'accounting RADIUS in modo che i criteri di rete registri le informazioni di accounting per registrare i file sul disco rigido locale o in un database Microsoft SQL Server. Per ulteriori informazioni, vedere [server RADIUS](#bkmk_server).
- **Proxy RADIUS**. Quando si utilizza criteri di rete come proxy RADIUS, si configurano i criteri di richiesta di connessione che indicano il server dei criteri di rete quali richieste di connessione inoltrare ad altri server RADIUS e server RADIUS che si desidera inoltrare le richieste di connessione. È inoltre possibile configurare criteri di rete per l'inoltro dei dati di accounting da registrare da uno o più computer in un gruppo di server RADIUS remoto. Per configurare criteri di rete come un server proxy RADIUS, vedere gli argomenti seguenti. Per ulteriori informazioni, vedere [proxy RADIUS](#bkmk_proxy).
    - [Configurare i criteri di richiesta di connessione](nps-crp-configure.md)
- **Accounting RADIUS**. È possibile configurare criteri di rete per registrare gli eventi in un file di registro locale o a un'istanza locale o remota di Microsoft SQL Server. Per ulteriori informazioni, vedere [la registrazione dei criteri di rete](#bkmk_logging).

>[!IMPORTANT]
>\(NAP\) protezione accesso alla rete, Autorità registrazione integrità \(HRA\) e Host Credential autorizzazione Protocol \(HCAP\) sono state deprecate in Windows Server 2012 R2 e non sono disponibili in Windows Server 2016. Se si dispone di una distribuzione di protezione accesso alla rete con i sistemi operativi precedenti a Windows Server 2016, è possibile eseguire la migrazione della distribuzione di protezione accesso alla rete a Windows Server 2016.

È possibile configurare criteri di rete con qualsiasi combinazione di queste funzionalità. Ad esempio, è possibile configurare un server dei criteri di rete come server RADIUS per connessioni VPN e anche come proxy RADIUS per inoltrare alcune richieste di connessione per i membri di un gruppo di server RADIUS remoto per autenticazione e autorizzazione in un altro dominio.

## <a name="windows-server-editions-and-nps"></a>Criteri di rete e le edizioni di Windows Server

Criteri di rete offre funzionalità diverse a seconda dell'edizione di Windows Server che si installa.

### <a name="windows-server-2016-datacenter-edition"></a>Windows Server 2016 Datacenter Edition

Con criteri di rete in Windows Server 2016 Datacenter, è possibile configurare un numero illimitato di client RADIUS e gruppi di server RADIUS remoti. Inoltre, è possibile configurare client RADIUS specificando un intervallo di indirizzi IP.

### <a name="windows-server-2016-standard-edition"></a>Windows Server 2016 Standard Edition

Con criteri di rete in Windows Server 2016 Standard, è possibile configurare un massimo di 50 client RADIUS e un massimo di 2 gruppi di server RADIUS remoti. È possibile definire un client RADIUS utilizzando un nome di dominio completo o un indirizzo IP, ma non è possibile definire gruppi di client RADIUS specificando un intervallo di indirizzi IP. Se viene risolto il nome di dominio completo di un client RADIUS in più indirizzi IP, il server dei criteri di rete utilizza il primo indirizzo IP restituito nella query sistema DNS (Domain Name).

Le sezioni seguenti forniscono informazioni più dettagliate sui criteri di rete come un server e proxy RADIUS.

## <a name="radius-server-and-proxy"></a>Proxy e server RADIUS

È possibile utilizzare criteri di rete come un server RADIUS, proxy RADIUS o entrambi.

### <a name="bkmk_server"></a>Server RADIUS

Criteri di rete è l'implementazione Microsoft dello standard RADIUS specificato da \(IETF\) la Internet Engineering Task Force in RFC 2865 e 2866. Come server RADIUS, dei criteri di rete esegue l'autenticazione, autorizzazione e accounting per molti tipi di accesso alla rete, tra cui wireless, l'autenticazione di commutatore, accesso remoto e accesso remoto \(VPN\) rete privata virtuale e le connessioni da router a router.

>[!NOTE]
>Per informazioni sulla distribuzione dei criteri di rete come server RADIUS, vedere [distribuire Server dei criteri di rete](nps-deploy.md).

Criteri di rete consente l'uso di un set eterogenei di wireless, commutatore, accesso remoto o attrezzature VPN. È possibile utilizzare criteri di rete con il servizio Accesso remoto, che è disponibile in Windows Server 2016.

Criteri di rete utilizza un dominio di Active Directory Domain Services \(AD DS\) o tenta di database degli account utente gli account di protezione (SAM) locale per autenticare le credenziali utente per la connessione. Quando un server dei criteri di rete è un membro di un dominio di Active Directory, dei criteri di rete utilizza il servizio directory come database degli account utente e fa parte di una soluzione single sign-on. Lo stesso set di credenziali viene utilizzato per il controllo di accesso di rete \ (autenticazione e autorizzazione dell'accesso a un isolamento di rete) e accedere al dominio di Active Directory.

>[!NOTE]
>Criteri di rete Usa le proprietà dial-dei criteri di rete e account utente di autorizzare una connessione.

Internet service provider \(ISPs\) e le organizzazioni che dispongono di accesso alla rete sono in grado di gestire tutti i tipi di accesso alla rete da un singolo punto di amministrazione, indipendentemente dal tipo di dispositivi di rete accesso utilizzate. Lo standard RADIUS supporta questa funzionalità in ambienti eterogenei e omogenei. RADIUS è un protocollo client / server che consente di dispositivi di accesso alla rete (utilizzato come client RADIUS) per inviare richieste di autenticazione e accounting a un server RADIUS.

Un server RADIUS ha accesso a informazioni sull'account utente e può controllare le credenziali di autenticazione di accesso di rete. Se vengono autenticate le credenziali dell'utente e il tentativo di connessione è autorizzato, il server RADIUS autorizza l'accesso utente in base alle condizioni specificate e quindi registra la connessione di accesso di rete in un log di accounting. L'utilizzo di RADIUS consente l'autenticazione dell'utente di accesso di rete, autorizzazione e i dati di accounting per raccogliere e gestire in una posizione centrale invece che in ogni server di accesso.

#### <a name="using-nps-as-a-radius-server"></a>Utilizzo dei criteri di rete come server RADIUS

È possibile utilizzare criteri di rete come server RADIUS quando:

- Si utilizza un dominio di Active Directory o database degli account utente SAM locale come database degli account utente per i client di accesso. 
- Usi in più server di accesso remoto, server VPN, accesso remoto o connessione a richiesta router e si vuole centralizzare la configurazione di connessione, registrazione e l'accounting e criteri di rete. 
- Si affidano remote o VPN, accesso wireless a un provider di servizi. Server di accesso utilizzano RADIUS per autenticare e autorizzare le connessioni eseguite dai membri della propria organizzazione.
- Si vuole centralizzare l'autenticazione, autorizzazione e accounting per un set di server di accesso eterogeneo.

La figura seguente mostra i criteri di rete come server RADIUS per un'ampia gamma di client di accesso. 

![Criteri di rete come Server RADIUS](../../media/Nps-Server/Nps-Server.jpg)

### <a name="bkmk_proxy"></a>Proxy RADIUS

Come proxy RADIUS, dei criteri di rete inoltra i messaggi di autenticazione e accounting per criteri di rete e altri server RADIUS. È possibile utilizzare criteri di rete come proxy RADIUS per il routing del raggio di messaggi tra i client RADIUS \ (detto anche servers\ di accesso di rete) e server RADIUS che eseguono l'autenticazione, autorizzazione e accounting per il tentativo di connessione. 

Se usato come proxy RADIUS, dei criteri di rete è un passaggio centrale o un punto di routing tramite il flusso di messaggi accesso e l'accounting RADIUS. Criteri di rete registra le informazioni in un log di accounting sui messaggi che vengono inoltrati.

#### <a name="using-nps-as-a-radius-proxy"></a>Utilizzo dei criteri di rete come proxy RADIUS

È possibile utilizzare criteri di rete come proxy RADIUS quando:

- Sei un provider del servizio che offre una connessione remota in outsourcing, VPN o servizi di accesso di rete wireless per più clienti. Il server NAS inviare le richieste di connessione al proxy RADIUS dei criteri di rete. In base la parte dell'area di autenticazione del nome utente nella richiesta di connessione, il proxy RADIUS dei criteri di rete inoltra la richiesta di connessione a un server RADIUS che viene mantenuta dal cliente e può autenticare e autorizzare il tentativo di connessione.
- Si desidera fornire l'autenticazione e autorizzazione per gli account utente che non sono membri del dominio in cui il server dei criteri di rete è un membro o un altro dominio che abbia un trust bidirezionale con il dominio in cui il server dei criteri di rete è un membro. Questo include gli account in domini non attendibili, i domini trusted unidirezionali e altre foreste. Anziché configurare i server di accesso per l'invio delle richieste di connessione a un server RADIUS NPS, è possibile configurare in modo da inviare le richieste di connessione a un proxy RADIUS dei criteri di rete. Il proxy RADIUS dei criteri di rete utilizza la parte del nome dell'area di autenticazione del nome utente e inoltra la richiesta a un server dei criteri di rete nel dominio corretto o nella foresta. Tentativi di connessione per gli account in un dominio o foresta possono essere autenticati per NAS in un altro dominio o foresta utente.
- Si desidera eseguire l'autenticazione e autorizzazione utilizzando un database che non è un database di account di Windows. In questo caso, le richieste di connessione che corrispondano al nome dell'area di autenticazione specificato vengono inoltrate a un server RADIUS, che ha accesso a un altro database degli account utente e i dati di autorizzazione. Esempi di altri database database Novell Directory Services (NDS) e Structured Query Language (SQL).
- Si desidera elaborare un numero elevato di richieste di connessione. Anziché configurare i client RADIUS per tentare di bilanciare le richieste di connessione e accounting tra più server RADIUS, in questo caso, è possibile configurare in modo da inviare le richieste di accounting e la connessione a un proxy RADIUS dei criteri di rete. Il proxy RADIUS NPS in modo dinamico bilancia il carico di connessione e accounting richiede tra più server RADIUS e aumenta l'elaborazione di un numero elevato di client RADIUS e le autenticazioni al secondo.
- Si desidera fornire l'autenticazione RADIUS e l'autorizzazione per i provider di servizi in outsourcing e ridurre al minimo la configurazione del firewall intranet. Un firewall intranet è compreso tra la rete perimetrale (la rete tra la rete intranet e Internet) e intranet. Inserimento di un server dei criteri di rete nella rete perimetrale, il firewall tra la rete perimetrale e la intranet deve consentire il traffico tra il server dei criteri di rete e più controller di dominio. Se si sostituisce il server dei criteri di rete con un proxy dei criteri di rete, il firewall deve consentire solo il traffico RADIUS tra il proxy di criteri di rete e uno o più server dei criteri di rete all'interno della intranet.

La figura seguente mostra i criteri di rete come proxy RADIUS tra i client RADIUS e server RADIUS.

![Criteri di rete come Proxy RADIUS](../../media/Nps-Proxy/Nps-Proxy.jpg)


Con criteri di rete, le organizzazioni possono anche dare in outsource infrastruttura di accesso remoto a un provider di servizi mantenendo il controllo tramite l'autenticazione, autorizzazione e accounting.

È possibile creare configurazioni di criteri di rete per gli scenari seguenti:

- Accesso wireless
- Organizzazione l'accesso remoto remote o VPN (VPN)
- Connessione remota in outsourcing o accesso wireless
- Accesso a Internet
- Accesso autenticato alle risorse extranet per partner commerciali

## <a name="radius-server-and-radius-proxy-configuration-examples"></a>Esempi di configurazione proxy RADIUS e di server RADIUS

Gli esempi di configurazione seguenti illustrano come è possibile configurare criteri di rete come proxy RADIUS e un server RADIUS.

**Criteri di rete come server RADIUS**. In questo esempio, dei criteri di rete è configurato come server RADIUS, il criterio di richiesta di connessione predefinito è l'unico criterio configurato e tutte le richieste di connessione vengono elaborate dal server dei criteri di rete locale. Il server dei criteri di rete può autenticare e autorizzare gli utenti con account nel dominio del server dei criteri di rete e in domini trusted.

**Criteri di rete come proxy RADIUS**. In questo esempio, il server dei criteri di rete è configurato come proxy RADIUS che inoltra le richieste di connessione a gruppi di server RADIUS remoti in due domini non attendibili. Il criterio di richiesta di connessione predefinito viene eliminato e vengono creati due nuovi criteri di richiesta di connessione per inoltrare le richieste per ognuno dei due domini non attendibili. In questo esempio, dei criteri di rete non elabora le richieste di connessione nel server locale. 

**Criteri di rete come proxy RADIUS e server RADIUS**. Oltre il criterio di richiesta di connessione predefinito, che indica che le richieste di connessione vengono elaborate in locale, viene creato un nuovo criterio di richiesta di connessione che inoltra le richieste di connessione a dei criteri di rete o un altro server RADIUS in un dominio non trusted. Il secondo criterio è denominato criterio il Proxy. In questo esempio, il criterio Proxy visualizzata per primo nell'elenco ordinato dei criteri. Se la richiesta di connessione soddisfa i criteri di Proxy, viene inoltrata la richiesta di connessione al server RADIUS nel gruppo di server RADIUS remoti. Se la richiesta di connessione non corrispondono al criterio di Proxy ma soddisfa i criteri di richiesta di connessione predefinito, dei criteri di rete elabora la richiesta di connessione nel server locale. Se la richiesta di connessione non corrisponde a uno di questi criteri, viene ignorata.

**Criteri di rete come server RADIUS con i server remoti di accounting**. In questo esempio, il server dei criteri di rete locale non è configurato per eseguire l'accounting e il criterio di richiesta di connessione predefinito viene modificato in modo che i messaggi di accounting RADIUS vengono inoltrati a un server dei criteri di rete o un altro server RADIUS in un gruppo di server RADIUS remoto. Anche se i messaggi di accounting vengono inoltrati, messaggi di autenticazione e autorizzazione, non vengono inoltrati e il server dei criteri di rete locale esegue queste funzioni per il dominio locale e tutti i domini trusted.

**Criteri di rete con server RADIUS remoto a mapping utente Windows**. In questo esempio, dei criteri di rete funge sia da server RADIUS che come proxy RADIUS per ogni richiesta di connessione per inoltrare le richieste di autenticazione a un server RADIUS remoto durante l'uso di un account utente locale di Windows per l'autorizzazione. Questa configurazione viene implementata tramite la configurazione RADIUS Remote all'attributo Mapping utente Windows come condizione per il criterio di richiesta di connessione. \ (Inoltre, un account utente deve essere creato in locale sul server RADIUS che ha lo stesso nome dell'account utente remoto rispetto al quale l'autenticazione viene eseguita dal server RADIUS remoti. \)

## <a name="configuration"></a>Configurazione

Per configurare criteri di rete come server RADIUS, è possibile utilizzare la configurazione standard o configurazione avanzata nella console di criteri di rete o in Server Manager. Per configurare criteri di rete come proxy RADIUS, è necessario utilizzare la configurazione avanzata.

### <a name="standard-configuration"></a>Configurazione standard

Con la configurazione standard, sono disponibili procedure guidate per la configurazione dei criteri di rete per gli scenari seguenti:

- Server RADIUS per connessioni remote o VPN
- Server RADIUS per connessioni wireless o cablate 802.1 X

Per configurare criteri di rete utilizzando una procedura guidata, aprire la console dei criteri di rete, selezionare uno dei precedenti scenari e quindi fare clic sul collegamento che consente di aprire la procedura guidata.

### <a name="advanced-configuration"></a>Configurazione avanzata

Quando si utilizza configurazione avanzata, configurare manualmente dei criteri di rete come server RADIUS o come proxy RADIUS. 

Per configurare criteri di rete tramite la configurazione avanzata, aprire la console dei criteri di rete e quindi fare clic sulla freccia accanto a **configurazione avanzata** per espandere la sezione.

Sono disponibili i seguenti elementi di configurazione avanzata.

#### <a name="configure-radius-server"></a>Configurare il server RADIUS

Per configurare criteri di rete come server RADIUS, è necessario configurare client RADIUS, criteri di rete e l'accounting RADIUS.

Per istruzioni su come rendere tali configurazioni, vedere gli argomenti seguenti.

- [Configurare i client RADIUS](nps-radius-clients-configure.md)
- [Configurare i criteri di rete](nps-np-configure.md)
- [Configurare l'Accounting Server dei criteri di rete](nps-accounting-configure.md)

#### <a name="configure-radius-proxy"></a>Configurare proxy RADIUS

Per configurare criteri di rete come proxy RADIUS, è necessario configurare client RADIUS, gruppi di server RADIUS remoti e criteri di richiesta di connessione.

Per istruzioni su come rendere tali configurazioni, vedere gli argomenti seguenti.

- [Configurare i client RADIUS](nps-radius-clients-configure.md)
- [Configurare gruppi di Server RADIUS remoti](nps-crp-rrsg-configure.md)
- [Configurare i criteri di richiesta di connessione](nps-crp-configure.md)

## <a name="bkmk_logging"></a>Registrazione dei criteri di rete

Registrazione dei criteri di rete è l'acronimo di accounting RADIUS. Configurare la registrazione dei criteri di rete in base ai requisiti dei criteri di rete viene utilizzato come un server RADIUS, proxy o qualsiasi combinazione di queste configurazioni.

Per configurare la registrazione dei criteri di rete, è necessario configurare gli eventi da registrare e visualizzare con il Visualizzatore eventi e quindi determinare le altre informazioni che si desidera accedere. Inoltre, è necessario decidere se si desidera registrare l'autenticazione utente e le informazioni di accounting un file di testo memorizzati nel computer locale o a un database di SQL Server in un computer locale o un computer remoto.

Per ulteriori informazioni, vedere [configurare Network Policy Server Accounting](nps-accounting-configure.md).
