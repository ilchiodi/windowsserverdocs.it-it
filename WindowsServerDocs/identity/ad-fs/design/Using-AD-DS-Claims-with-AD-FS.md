---
ms.assetid: 460792e4-9f1d-4e7b-b6b2-53e057f839df
title: Considerazioni sulla topologia di distribuzione di ADFS
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 881cdc02d06ce5afd3c0706f9c1ea5fa2576799f
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71358924"
---
# <a name="using-ad-ds-claims-with-ad-fs"></a>Utilizzo di attestazioni Active Directory Domain Services con ADFS
  
  
È possibile abilitare il controllo di accesso più completo per le applicazioni federate \(utilizzando Active Directory Domain Services\)servizi di dominio Active Directory\-con attestazioni \(utente e dispositivo con Active Directory Federation Services ad FS \).  
  
## <a name="about-dynamic-access-control"></a>Informazioni sul controllo di accesso dinamico  
In Windows Server® 2012, la funzionalità controllo dinamico degli accessi consente alle organizzazioni di concedere l'accesso ai file in base alle attestazioni utente \(che vengono originate dagli attributi di account utente\) del dispositivo e \(che vengono originate dagli attributi di account computer\) che vengono rilasciati da servizi di dominio Active Directory \(AD DS\). Dominio di Active Directory rilasciate le attestazioni è integrati nell'autenticazione integrata di Windows tramite il protocollo di autenticazione Kerberos.  
  
Per ulteriori informazioni su controllo dinamico degli accessi, vedere [dinamica Guida del contenuto controllo accesso](../../solution-guides/Dynamic-Access-Control--Scenario-Overview.md#BKMK_APP).  
  
### <a name="whats-new-in-ad-fs"></a>Novità di AD FS  
Come un'estensione allo scenario controllo dinamico degli accessi, ADFS in Windows Server 2012 è ora:  
  
-   Attributi di account accesso computer oltre a attributi di account utente all'interno di Active Directory. Nelle versioni precedenti di ADFS, il servizio federativo Impossibile accedere gli attributi degli account computer del tutto da AD DS.  
  
-   Utilizzare Active Directory rilasciate le attestazioni utente o dispositivo che si trovano in un ticket di autenticazione Kerberos. Nelle versioni precedenti di ADFS, il motore di attestazioni è stato in grado di leggere l'ID di sicurezza utente e gruppo \(SID\) da Kerberos ma non è in grado di leggere le informazioni contenute all'interno di un ticket Kerberos di attestazioni.  
  
-   Trasformazione di dominio Active Directory emesso utente o dispositivo attestazioni nei token SAML che applicazioni relying party consentono di eseguire il controllo di accesso più completo.  
  
## <a name="benefits-of-using-ad-ds-claims-with-ad-fs"></a>Vantaggi dell'utilizzo di Active Directory le attestazioni con AD FS  
Queste rilasciate le attestazioni di Active Directory può essere inserito un ticket di autenticazione Kerberos e utilizzato con ADFS per fornire i seguenti vantaggi:  
  
-   Le organizzazioni che richiedono criteri di controllo di accesso più completi possano abilitare le attestazioni\-accesso in base alle applicazioni e risorse tramite AD DS rilasciate le attestazioni che sono in base ai valori di attributo archiviati in Active Directory per un determinato account utente o computer. Ciò consente agli amministratori di ridurre i costi aggiuntivi associati alla creazione e gestione:  
  
    -   AD DS gruppi di sicurezza, in caso contrario, viene utilizzati per controllare l'accesso alle applicazioni e risorse che sono accessibili tramite l'autenticazione integrata di Windows.  
  
    -   Trust tra foreste che verrebbero altrimenti usati per controllare l'\-accesso alle\- \(applicazioni e alle risorse\) internet B2B \/ accessibili per le aziende.  
  
-   Le organizzazioni possono ora impedire l'accesso non autorizzato alle risorse di rete dai computer client a seconda che un valore di attributo dell'account computer specifico \(archiviato in servizi di dominio Active Directory,\) ad esempio, il nome DNS di un computer corrisponda al controllo di accesso \(criteri della risorsa, ad esempio, un file server che è stato consentito con attestazioni \(\) o i criteri di relying party, ad esempio\-, un'applicazione\)Web in grado di riconoscere attestazioni. Ciò consente agli amministratori di impostare criteri di controllo di accesso più preciso per le risorse o le applicazioni:  
  
    -   Accessibile solo tramite l'autenticazione integrata di Windows.  
  
    -   Internet accessibili tramite meccanismi di autenticazione ADFS. ADFS può essere usato per trasformare rilasciate le attestazioni dispositivo attestazioni ADFS che possono essere incapsulate in token SAML che possono essere utilizzati da una risorsa accessibile mediante Internet o l'applicazione relying party in Active Directory.  
  
## <a name="differences-between-ad-ds-and-ad-fs-issued-claims"></a>Differenze tra Active Directory e ADFS rilasciate le attestazioni  
Esistono due fattori discriminanti importanti comprendere le attestazioni inviate da Visual Studio di dominio Active Directory. AD FS. Queste differenze includono:  
  
-   Active Directory può rilasciare solo le attestazioni che sono incapsulate i ticket Kerberos, non i token SAML. Per ulteriori informazioni sulla modalità di dominio Active Directory emette attestazioni, vedere [dinamica Guida del contenuto controllo accesso](../../solution-guides/Dynamic-Access-Control--Scenario-Overview.md#BKMK_APP).  
  
-   ADFS può rilasciare solo le attestazioni vengono incapsulate nei token SAML, non i ticket Kerberos. Per ulteriori informazioni su come ADFS emette attestazioni, vedere [il ruolo del motore di attestazioni](../../ad-fs/technical-reference/The-Role-of-the-Claims-Engine.md).  
  
## <a name="how-ad-ds-issued-claims-work-with-ad-fs"></a>Come funzionano le attestazioni rilasciate da Servizi di dominio Active Directory con ADFS  
Le attestazioni rilasciate da servizi di dominio Active Directory possono essere utilizzate con AD FS per accedere alle attestazioni utente e dispositivo direttamente dal contesto di autenticazione dell'utente, invece di effettuare una chiamata LDAP separata a Active Directory. La figura riportata di seguito e i passaggi corrispondenti vengono illustrati il funzionamento del processo in modo più dettagliato per abilitare le attestazioni\-controllo di accesso per lo scenario controllo dinamico degli accessi basato.  
  
![uso delle attestazioni](media/UsingADDSClaimswithADFS.gif)  
  
1.  Un amministratore di dominio Active Directory utilizza la console di centro di amministrazione di Active Directory o i cmdlet PowerShell per gli oggetti di tipo attestazione specifico consente nello schema di Active Directory.  
  
2.  Un amministratore di AD FS utilizza la console di gestione di ADFS per creare e configurare il provider di attestazioni e relying party relazioni di trust con passaggio\-tramite o le regole di attestazione di trasformazione.  
  
3.  Un client Windows tenta di accedere alla rete. Come parte del processo di autenticazione Kerberos, il client presenta il ticket utente e computer\-concessione ticket \(TGT\) che non contiene ancora le attestazioni, al controller di dominio. Il controller di dominio in Active Directory cerca tipi di attestazione abilitato e include tutte le attestazioni risultante del ticket Kerberos restituito.  
  
4.  Quando l'utente\/client tenta di accedere a una risorsa di file che è consentito in modo da richiedere le attestazioni, possono accedere alla risorsa perché l'ID composta che venivano esposte da Kerberos ha tali attestazioni.  
  
5.  Quando lo stesso client tenta di accedere a una sito Web o un'applicazione Web è configurata per l'autenticazione di ADFS, l'utente viene reindirizzato a un server federativo ADFS è configurato per l'autenticazione integrata di Windows. Il client invia una richiesta al controller di dominio con Kerberos. Il controller di dominio invia un ticket Kerberos contenente le attestazioni richieste che il client può quindi presentare al server federativo.  
  
6.  In base al modo in cui che le regole delle attestazioni sono state configurate nel provider di attestazioni e attendibilità del componente che l'amministratore configurata in precedenza, ADFS legge le attestazioni dal ticket Kerberos e vengono inclusi in un token SAML emessi per il client.  
  
7.  Il client riceve il token SAML contenente le attestazioni corrette e quindi viene reindirizzato al sito Web.  
  
Per ulteriori informazioni su come creare le regole di attestazione necessarie per AD DS emesso attestazioni per lavorare con ADFS, vedere [creare una regola per trasformare un'attestazione in ingresso](../../ad-fs/operations/Create-a-Rule-to-Transform-an-Incoming-Claim.md).  
  
## <a name="see-also"></a>Vedere anche
[Guida alla progettazione di AD FS in Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
