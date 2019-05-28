---
ms.assetid: 824005ae-c3c1-459b-9baa-1660158918ab
title: Quando creare un server federativo
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 7e61c734780baa1482670af3f24697c10345b292
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/24/2019
ms.locfileid: "66190594"
---
# <a name="when-to-create-a-federation-server"></a>Quando creare un server federativo

Quando si crea un Active Directory Federation Services di serverin federation \(ADFS\), si fornisce un mezzo con cui l'organizzazione può:  
  
-   Interagisci in Web single\-sign\-sul \(SSO\)– basata la comunicazione con un'altra organizzazione \(che ha almeno un server federativo\) e, se necessario, con il i dipendenti dell'organizzazione \(che richiedono l'accesso tramite Internet\).  
  
-   Abilitare i servizi front-end per rappresentare utenti nei servizi di infrastruttura tramite la delega di identità. Per altre informazioni, vedere [When to Use Identity Delegation](When-to-Use-Identity-Delegation.md).  
  
Le sezioni seguenti descrivono alcune delle decisioni chiave per determinare quando e dove creare uno o più server federativi.  
  
## <a name="determine-the-organizational-role-for-the-federation-server"></a>Determinare il ruolo dell'organizzazione per il server federativo  
Per prendere una decisione consapevole su quando creare un nuovo server federativo, è necessario innanzitutto determinare in quale organizzazione risiederà il server. Il ruolo svolto da un server federativo in un'organizzazione dipende dal fatto che è inserire il server federativo nell'organizzazione partner account o nell'organizzazione partner risorse.  
  
Quando un server federativo viene posizionato nella rete aziendale del partner account, il suo ruolo è per autenticare le credenziali utente del browser, servizio Web o client del selettore di identità e invia i token di sicurezza ai client. Per altre informazioni, vedere [Review the Role of the Federation Server in the Account Partner](Review-the-Role-of-the-Federation-Server-in-the-Account-Partner.md).  
  
Quando un server federativo viene posizionato nella rete aziendale del partner risorse, il suo ruolo consiste nell'autenticare gli utenti in base a un token di sicurezza emesso da un server federativo nell'organizzazione partner risorse, o il suo ruolo consiste nel reindirizzare le richieste di token da le applicazioni Web configurate o i servizi Web per l'organizzazione partner account a cui appartiene il client. Per altre informazioni, vedere [Review the Role of the Federation Server in the Resource Partner](Review-the-Role-of-the-Federation-Server-in-the-Resource-Partner.md).  
  
## <a name="determine-which-ad-fs-design-to-deploy"></a>Determinare quale progettazione di AD FS distribuire  
È consigliabile creare i server federativi nell'organizzazione ogni volta che si vuole distribuire uno qualsiasi dei progetti di ADFS seguenti:  
  
-   [Progetto di Web SSO](Web-SSO-Design.md)  
  
-   [Progetto di Web SSO federativo](Federated-Web-SSO-Design.md)  
  
Se necessario, un'organizzazione che distribuisce un progetto SSO Web federativo può configurare un server federativo singolo in modo che operi sia il ruolo di partner account e il ruolo del partner risorse. In questo caso, il server federativo può produrre Security Assertion Markup Language \(SAML\) token, basati sugli account utente nella propria organizzazione, o reindirizzare le richieste di token all'organizzazione, in base in cui si trovano gli account degli utenti .  
  
> [!NOTE]  
> Per la progettazione di Web SSO federativo, deve esistere almeno un server federativo nel partner account e almeno un server federativo nel partner risorse.  
  
## <a name="differences-between-a-federation-server-and-a-federation-server-proxy"></a>Differenze tra un server federativo e un proxy server federativo  
Un server federativo può fornire pagine Web per l'accesso\-in, criteri, l'autenticazione e individuazione nello stesso modo che un proxy server federativo. Le differenze principali tra un server federativo e un proxy server federativo hanno a che fare con le operazioni che un federation server può eseguire che non è possibile eseguire un proxy server federativo.  
  
Di seguito sono le operazioni che possa eseguire solo un server federativo:  
  
-   Il server federativo esegue le operazioni di crittografia che producono il token. Anche se i proxy server federativo non possono produrre token, possono essere utilizzati per instradare o reindirizzare i token per i client e, se necessario, tornare al server federativo. Per altre informazioni sull'uso di server federativi, vedere [questa opzione per creare un Proxy Server federativo](When-to-Create-a-Federation-Server-Proxy.md).  
  
-   I server federativi supportano l'uso dell'autenticazione integrata di Windows per i client della rete aziendale. server federativi non lo sono. Per altre informazioni sull'uso di autenticazione integrata di Windows con server federativo, vedere [questa opzione per creare una Server Farm federativa](When-to-Create-a-Federation-Server-Farm.md).  
  
> [!CAUTION]  
> La comunicazione tra server federativi e database di configurazione di SQL Server, archivi di attributi di SQL Server, controller di dominio e istanze di AD LDS non offre protezione dell'integrità o della riservatezza per impostazione predefinita. Per ridurre questo inconveniente, valutare la possibilità di proteggere il canale di comunicazione tra questi server usando IPSEC o una connessione fisicamente sicura tra tutti questi server. Per la comunicazione tra i server federativi e i server SQL, valutare l'uso della protezione SSL nella stringa di connessione. Per le connessioni tra server federativi e i controller di dominio, valutare la possibilità di attivare la firma e la crittografia Kerberos. Per l'accesso LDAP, LDAP\/S non è supportata per AD LDS\/Active Directory Domain Services.  
  
## <a name="how-to-create-a-federation-server"></a>Come creare un server federativo  
È possibile creare un server federativo utilizzando Configurazione guidata Server federativo AD FS o il comando Fsconfig.exe\-strumento della riga. Quando si usa uno di questi strumenti, è possibile selezionare una qualsiasi delle opzioni seguenti per un server federativo.  
  
-   Creare un supporto\-server federativo da solo  
  
    Per altre informazioni su come configurare un supporto\-server federativo autonomo, vedere [creare un Server federativo autonomo](../../ad-fs/deployment/Create-a-Stand-Alone-Federation-Server.md).  
  
-   Creare il primo server federativo in una server farm federativa  
  
    Per altre informazioni su come configurare il primo server federativo o aggiungere un server federativo a una farm, vedere [Create the First Federation Server in a Federation Server Farm](../../ad-fs/deployment/Create-the-First-Federation-Server-in-a-Federation-Server-Farm.md).  
  
-   Aggiungere un server federativo a una server farm federativa  
  
    Per altre informazioni su come aggiungere un server federativo, vedere [Add a Federation Server to a Federation Server Farm](../../ad-fs/deployment/Add-a-Federation-Server-to-a-Federation-Server-Farm.md).  
  
Per altre informazioni dettagliate su come funziona ciascuna di queste opzioni, vedere [The Role of the AD FS Configuration Database](../../ad-fs/technical-reference/The-Role-of-the-AD-FS-Configuration-Database.md).  
  
Per altre informazioni su come configurare tutti i prerequisiti necessari per distribuire un server federativo, vedere [elenco di controllo: Configurazione di un Server federativo](../../ad-fs/deployment/Checklist--Setting-Up-a-Federation-Server.md).  
  
## <a name="see-also"></a>Vedere anche
[Guida alla progettazione di AD FS in Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)

