---
ms.assetid: 460792e4-9f1d-4e7b-b6b2-53e057f839df
title: Considerazioni sulla topologia di distribuzione ADFS
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 46692653ba10558a9236bd321127591bc7c8a275
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2017
---
# <a name="using-ad-ds-claims-with-ad-fs"></a>Con AD DS attestazioni con AD FS
  
>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012
  
È possibile abilitare il controllo di accesso più completo per applicazioni federate tramite servizi di dominio Active Directory \(AD DS\)\-issued utente e richieste diritti da dispositivo insieme \(AD FS\) Active Directory Federation Services.  
  
## <a name="about-dynamic-access-control"></a>Su controllo dinamico degli accessi  
In Windows Server® 2012, la funzionalità controllo dinamico degli accessi consente alle organizzazioni di concedere l'accesso ai file in base alle attestazioni utente \ (che vengono originate dagli attributes\ account utente) e richieste diritti da dispositivo \ (che vengono originate dagli attributes\ account computer) che vengono rilasciati da servizi di dominio Active Directory \(AD DS\). Dominio di Active Directory rilasciate le attestazioni è integrati nell'autenticazione integrata di Windows tramite il protocollo di autenticazione Kerberos.  
  
Per ulteriori informazioni su controllo dinamico degli accessi, vedere [dinamica Guida del contenuto controllo accesso](../../solution-guides/Dynamic-Access-Control--Scenario-Overview.md#BKMK_APP).  
  
### <a name="whats-new-in-ad-fs"></a>What's New in AD FS?  
Come un'estensione allo scenario controllo dinamico degli accessi, ADFS in Windows Server 2012 è ora:  
  
-   Attributi di account accesso computer oltre a attributi di account utente all'interno di dominio Active Directory. Nelle versioni precedenti di ADFS, il servizio federativo Impossibile accedere gli attributi degli account computer affatto da AD DS.  
  
-   Utilizzare Active Directory rilasciate le attestazioni utente o dispositivo che si trovano in un ticket di autenticazione Kerberos. Nelle versioni precedenti di ADFS, il motore delle attestazioni è stato in grado di leggere l'utente e gruppo sicurezza ID \(SIDs\) da Kerberos ma non è in grado di leggere le informazioni contenute all'interno di un ticket Kerberos di attestazioni.  
  
-   Trasformazione di dominio Active Directory emesso utente o dispositivo attestazioni nei token SAML che applicazioni relying party consentono di eseguire il controllo di accesso più completo.  
  
## <a name="benefits-of-using-ad-ds-claims-with-ad-fs"></a>Vantaggi dell'uso di AD DS attestazioni con AD FS  
Queste rilasciate le attestazioni di Active Directory può essere inserita un ticket di autenticazione Kerberos e utilizzata con ADFS per fornire i vantaggi seguenti:  
  
-   Le organizzazioni che richiedono criteri di controllo di accesso più completo possono abilitare l'accesso basato su claims\ alle applicazioni e risorse tramite AD DS rilasciate le attestazioni basate su valori di attributo archiviati in Active Directory per un determinato account utente o computer. Ciò consente agli amministratori di ridurre i costi aggiuntivi associati alla creazione e gestione:  
  
    -   AD DS gruppi di sicurezza in caso contrario, viene utilizzati per controllare l'accesso alle applicazioni e risorse che sono accessibili tramite l'autenticazione integrata di Windows.  
  
    -   Trust tra foreste bidirezionali in caso contrario, viene utilizzati per controllare l'accesso a \(B2B\) Business\-da destra-Business \ / Internet accessibile applicazioni e risorse.  
  
-   Le organizzazioni possono ora impedire accessi non autorizzati alle risorse di rete dal computer client sulla base indica se un account di computer specifico attributo valore archiviato in Active Directory \ (ad esempio, un computer DNS name\) soddisfi i criteri di controllo di accesso della risorsa \ (ad esempio, un file server che è stato consentito in modo con claims\) o criteri di relying party \ (ad esempio, un application\ Web compatibile con claims\). Ciò consente agli amministratori di impostare criteri di controllo di accesso più preciso per le risorse o le applicazioni:  
  
    -   Accessibile solo tramite l'autenticazione integrata di Windows.  
  
    -   Internet accessibili tramite meccanismi di autenticazione ADFS. ADFS può essere utilizzata per trasformare di dominio Active Directory rilasciate le attestazioni dispositivo in attestazioni ADFS che possono essere incapsulate in token SAML che può essere utilizzato da una risorsa accessibile mediante Internet o l'applicazione relying party.  
  
## <a name="differences-between-ad-ds-and-ad-fs-issued-claims"></a>Differenze tra Active Directory e ADFS rilasciate le attestazioni  
Esistono due fattori discriminanti importanti comprendere le attestazioni inviate da AD DS vs. AD FS. Queste differenze includono:  
  
-   Servizi di dominio Active Directory può rilasciare solo le attestazioni vengono incapsulate i ticket Kerberos, non i token SAML. Per ulteriori informazioni sulla modalità di dominio Active Directory emette attestazioni, vedere [dinamica Guida del contenuto controllo accesso](../../solution-guides/Dynamic-Access-Control--Scenario-Overview.md#BKMK_APP).  
  
-   ADFS può rilasciare solo le attestazioni vengono incapsulate nei token SAML, non i ticket Kerberos. Per ulteriori informazioni su come ADFS emette attestazioni, vedere [The Role of the Claims Engine](../../ad-fs/technical-reference/The-Role-of-the-Claims-Engine.md).  
  
## <a name="how-ad-ds-issued-claims-work-with-ad-fs"></a>Come di dominio Active Directory rilasciate le attestazioni di lavoro con AD FS  
Rilasciate le attestazioni di Active Directory può essere utilizzata con ADFS per accedere attestazioni utente e dispositivo direttamente dal contesto di autenticazione dell'utente, anziché effettuare una chiamata LDAP ad Active Directory. La figura seguente e i passaggi corrispondenti vengono illustrati il funzionamento del processo in maggiore dettaglio per abilitare il controllo di accesso basato su claims\ per lo scenario controllo dinamico degli accessi.  
  
![uso delle attestazioni](media/UsingADDSClaimswithADFS.gif)  
  
1.  Un amministratore di dominio Active Directory utilizza la console di centro di amministrazione di Active Directory o i cmdlet PowerShell per gli oggetti di tipo attestazione specifico consente nello schema di Active Directory.  
  
2.  Un amministratore di AD FS utilizza la console di gestione di ADFS per creare e configurare il provider di attestazioni e relying party relazioni di trust con uno pass\-through o di regole attestazione di trasformazione.  
  
3.  Un client Windows tenta di accedere alla rete. Come parte del processo di autenticazione Kerberos, il client presenta l'utente e computer ticket\ di concessione ticket \(TGT\) che non esiste ancora contengono le attestazioni, al controller di dominio. Il controller di dominio in Active Directory cerca tipi di attestazione abilitato e include tutte le attestazioni risultante del ticket Kerberos restituito.  
  
4.  Quando dall'utente/client tenta di accedere a una risorsa di file che è consentito in modo da richiedere le attestazioni, possono accedere alla risorsa perché l'ID composta che venivano esposte da Kerberos ha tali attestazioni.  
  
5.  Quando lo stesso client tenta di accedere a una sito Web o un'applicazione Web che è configurata per l'autenticazione di ADFS, l'utente viene reindirizzato a un server federativo ADFS è configurato per l'autenticazione integrata di Windows. Il client invia una richiesta al controller di dominio con Kerberos. Il controller di dominio invia un ticket Kerberos contenente le attestazioni richieste che il client può quindi presentare al server federativo.  
  
6.  In base al modo in cui che sono state configurate le regole attestazioni nel provider di attestazioni e attendibilità che l'amministratore configurata in precedenza, ADFS legge le attestazioni dal ticket Kerberos e inclusi in un token SAML emessi per il client.  
  
7.  Il client riceve il token SAML contenente le attestazioni corrette e quindi viene reindirizzato al sito Web.  
  
Per ulteriori informazioni su come creare le regole di attestazione necessarie per AD DS emesso attestazioni per lavorare con ADFS, vedere [creare una regola per trasformare un'attestazione in ingresso](../../ad-fs/operations/Create-a-Rule-to-Transform-an-Incoming-Claim.md).  
  
## <a name="see-also"></a>Vedere anche
[Guida alla progettazione di ADFS in Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
