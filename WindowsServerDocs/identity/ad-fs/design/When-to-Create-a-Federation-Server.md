---
ms.assetid: 824005ae-c3c1-459b-9baa-1660158918ab
title: Quando creare un Server federativo
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 8013764b88a1061cfcaa3a507466c111bfd59aad
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2017
---
# <a name="when-to-create-a-federation-server"></a>Quando creare un Server federativo

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Quando si crea una federazione serverin Active Directory Federation Services \(AD FS\), si fornisce un mezzo con cui l'organizzazione può:  
  
-   Puoi interagire in Web apartment accesso in \ (SSO\) – comunicazioni con un'altra organizzazione basate su \ (che ha almeno un server federativo) e, se necessario, con i dipendenti dell'organizzazione \ (che richiedono l'accesso tramite l'Internet).  
  
-   Abilitare i servizi front-end rappresentare gli utenti ai servizi di infrastruttura tramite la delega dell'identità. Per ulteriori informazioni, vedere [When to Use Identity Delegation](When-to-Use-Identity-Delegation.md).  
  
Le sezioni seguenti vengono descritte alcune delle decisioni chiave per determinare quando e dove creare uno o più server federativi.  
  
## <a name="determine-the-organizational-role-for-the-federation-server"></a>Determinare il ruolo dell'organizzazione per il server federativo  
Per rendere una decisione consapevole su quando creare un nuovo server federativo, è necessario determinare in quale organizzazione risiederà il server. Il ruolo svolto da un server federativo in un'organizzazione dipende se inserire il server federativo nell'organizzazione partner account o nell'organizzazione partner risorse.  
  
Quando un server federativo viene posizionato nella rete aziendale del partner account, il ruolo è per autenticare le credenziali utente del browser, del servizio Web o client del selettore di identità e inviare i token di sicurezza ai client. Per ulteriori informazioni, vedere [rivedere il ruolo del Server federativo nel Partner Account](Review-the-Role-of-the-Federation-Server-in-the-Account-Partner.md).  
  
Quando un server federativo viene posizionato nella rete aziendale del partner risorse, il ruolo è per autenticare gli utenti, in base a un token di sicurezza emesso da un server federativo nell'organizzazione partner risorse, o il ruolo è per reindirizzare le richieste di token da applicazioni Web configurate o servizi Web per l'organizzazione partner account che il client appartiene. Per ulteriori informazioni, vedere [rivedere il ruolo del Server federativo nel Partner risorse](Review-the-Role-of-the-Federation-Server-in-the-Resource-Partner.md).  
  
## <a name="determine-which-ad-fs-design-to-deploy"></a>Determinare quale progettazione di AD FS distribuire  
È consigliabile creare i server federativi nell'organizzazione ogni volta che si desidera distribuire uno qualsiasi dei progetti di ADFS seguenti:  
  
-   [Progettazione di Web SSO](Web-SSO-Design.md)  
  
-   [Progetto Web SSO federativo](Federated-Web-SSO-Design.md)  
  
Se necessario, un'organizzazione che distribuisce un progetto SSO Web federativo può configurare un server federativo singolo in modo che operi sia il ruolo di partner account e il ruolo di partner risorse. In questo caso, il server federativo può produrre token \(SAML\) Security Assertion Markup Language, basati sugli account utente nella propria organizzazione, o i token di reindirizzare le richieste di organizzazione, in base a dove si trovano gli account degli utenti.  
  
> [!NOTE]  
> Per la progettazione di Web SSO federativo, deve essere presente almeno un server federativo nel partner account e almeno un server federativo nel partner risorse.  
  
## <a name="differences-between-a-federation-server-and-a-federation-server-proxy"></a>Differenze tra un server federativo e un proxy server federativo  
Un server federativo può fornire pagine Web per l'accesso in, criteri di autenticazione e individuazione nello stesso modo che un proxy server federativo. Le principali differenze tra un server federativo e un proxy server federativo sono necessario eseguire le operazioni una federazione server può eseguire che non può eseguire un proxy server federativo.  
  
Di seguito sono le operazioni che possa eseguire solo un server federativo:  
  
-   Il server federativo esegue le operazioni di crittografia che producono il token. Sebbene i proxy server federativi non possono produrre token, possono essere utilizzati per inviare o reindirizzare i token ai client e, se necessario, al server federativo. Per ulteriori informazioni sull'utilizzo di server federativi, vedere [quando creare un Proxy Server federativo](When-to-Create-a-Federation-Server-Proxy.md).  
  
-   Server federativi supportano l'uso dell'autenticazione integrata di Windows per i client della rete aziendale. i proxy server federativi non. Per ulteriori informazioni sull'utilizzo di autenticazione integrata di Windows con server federativo, vedere [sulla creazione di una Server Farm federativa](When-to-Create-a-Federation-Server-Farm.md).  
  
> [!CAUTION]  
> La comunicazione tra server federativi e database di configurazione di SQL Server, archivi di attributi di SQL Server, i controller di dominio e istanze di AD LDS non integrità o della riservatezza protetti per impostazione predefinita. Per ridurre questo inconveniente, è consigliabile proteggere il canale di comunicazione tra questi server usando IPSEC o una connessione fisicamente sicura tra tutti i server. Per la comunicazione tra server federativi e istanze di SQL Server, è consigliabile utilizzare la protezione SSL nella stringa di connessione. Per le connessioni tra server federativi e i controller di dominio, è possibile attivare Kerberos firma e crittografia. Per LDAP, LDAP\/S non è supportato per AD LDS \ / Active Directory.  
  
## <a name="how-to-create-a-federation-server"></a>Come creare un server federativo  
È possibile creare un server federativo utilizzando Configurazione guidata Server federativo AD FS o lo strumento della riga di comando Fsconfig.exe. Quando si usa uno di questi strumenti, è possibile selezionare una delle opzioni seguenti per creare un server federativo.  
  
-   Creare un server federativo autonoma  
  
    Per ulteriori informazioni su come configurare un server federativo autonoma, vedere [creare un Server federativo autonomo](../../ad-fs/deployment/Create-a-Stand-Alone-Federation-Server.md).  
  
-   Creare il primo server federativo in una server farm federativa  
  
    Per ulteriori informazioni su come configurare il primo server federativo o aggiungere un server federativo a una farm, vedere [creare il primo Server federativo in una Server Farm federativa](../../ad-fs/deployment/Create-the-First-Federation-Server-in-a-Federation-Server-Farm.md).  
  
-   Aggiungere un server federativo a una server farm federativa  
  
    Per ulteriori informazioni su come aggiungere un server federativo a una farm, vedere [aggiunge un Server federativo a una Server Farm federativa](../../ad-fs/deployment/Add-a-Federation-Server-to-a-Federation-Server-Farm.md).  
  
Per ulteriori informazioni su come ognuna di queste soluzioni funziona, vedere [il ruolo del Database di configurazione AD FS](../../ad-fs/technical-reference/The-Role-of-the-AD-FS-Configuration-Database.md).  
  
Per ulteriori informazioni su come configurare tutti i prerequisiti necessari per distribuire un server federativo, vedere [Checklist: Setting Up a Federation Server](../../ad-fs/deployment/Checklist--Setting-Up-a-Federation-Server.md).  
  
## <a name="see-also"></a>Vedere anche
[Guida alla progettazione di ADFS in Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)

