---
ms.assetid: 9a06cd41-426f-4cb9-89cf-f5be730e0b79
title: "Cosa & #39; s novità di servizi di dominio Active Directory"
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.service: 
ms.suite: na
ms.technology: active-directory-domain-services
ms.tgt_pltfrm: na
ms.topic: article
author: Femila
ms.author: billmath
ms.date: 05/31/2017
ms.openlocfilehash: 56716e2e0d633a19e9c6e02bc829874bbe863833
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/12/2017
---
# <a name="what39s-new-in-active-directory-domain-services"></a>Cosa & #39; s novità di servizi di dominio Active Directory 

>Si applica a: Windows Server 2016

Le nuove funzionalità seguenti in servizi di dominio Active Directory (AD DS) migliorano la capacità per le organizzazioni di proteggere gli ambienti Active Directory e consentono di eseguire la migrazione a distribuzioni cloud-only e distribuzioni ibride, in alcune applicazioni e servizi sono ospitati nel cloud e ad altri utenti sono ospitati in locale. I miglioramenti includono:  
  
-   [Gestione accessi con privilegi](https://technet.microsoft.com/library/mt150258.aspx   
)  
  
- [Estensione delle funzionalità cloud per i dispositivi Windows 10 tramite aggiunta Azure Active Directory](https://azure.microsoft.com/en-us/documentation/articles/active-directory-azureadjoin-overview/)   
  
- [Connessione dei dispositivi appartenenti a un dominio ad Azure AD per Windows 10 esperienze](https://azure.microsoft.com/en-us/documentation/articles/active-directory-azureadjoin-devices-group-policy/)   
  
- [Abilitare Microsoft Passport for Work all'interno dell'organizzazione](https://azure.microsoft.com/en-us/documentation/articles/active-directory-azureadjoin-passport-deployment/)    
  
-  [Deprecazione di livelli di funzionalità del servizio Replica File (FRS) e Windows Server 2003](ad-ds/active-directory-functional-levels.md)  
  
  
## <a name="BKMK_PAM"></a>Gestione accessi con privilegi  
Gestione accesso con privilegi (PAM) riduce i rischi di sicurezza per gli ambienti Active Directory che sono causati da problemi di credenziali tecniche furto tali pass-the-hash, spear phishing e tipi di attacchi simili. Fornisce una nuova soluzione di accesso amministrativo che viene configurata utilizzando Microsoft Identity Manager (MIM). Sono stati introdotti PAM:  
  
-   Bastione una nuova foresta di Active Directory, che viene eseguito il provisioning da MIM. La foresta bastione ha una relazione di trust con una foresta esistente PAM speciali. Fornisce un nuovo ambiente Active Directory che è privi di qualsiasi attività dannose e isolamento da una foresta esistente per l'utilizzo di account con privilegi.  
  
-   Nuovi processi in MIM per richiedere privilegi amministrativi, insieme ai nuovi flussi di lavoro in base l'approvazione delle richieste.  
  
-   Nuovo shadow entità di sicurezza (gruppi) che viene effettuato il provisioning nella foresta bastione da MIM in risposta alle richieste di privilegi amministrativi. Le entità di sicurezza shadow hanno un attributo che fa riferimento il SID di un gruppo amministrativo in una foresta esistente. In questo modo il gruppo shadow per accedere alle risorse in una foresta esistente senza modificare qualsiasi elenchi di controllo di accesso (ACL).  
  
-   Una funzionalità collegamenti in scadenza, che consente a scadenza appartenenza a un gruppo di ombreggiatura. Un utente può essere aggiunto al gruppo sufficiente tempo necessario per eseguire un'attività amministrativa. L'appartenenza a un limite di tempo è espresso da un valore di (durata TTL) time-to-live che viene propagato a una durata del ticket Kerberos.  
  
    > [!NOTE]  
    > Scadenza collegamenti sono disponibili in tutti gli attributi collegati. Ma l'attributo collegato membro/memberOf relazione tra un gruppo e un utente è riportato l'esempio solo in una soluzione completa, ad esempio PAM è preconfigurata per utilizzare la funzionalità di collegamenti che sta per scadere.  
  
-   Miglioramenti KDC sono incorporati controller di dominio Active Directory per limitare durata del ticket Kerberos sul valore più basso possibile time-to-live (TTL) nei casi in cui un utente dispone di più le appartenenze associate alla durata in gruppi amministrativi. Ad esempio, se si viene aggiunti a un gruppo di limite di tempo, quando si accede, durata Kerberos ticket di concessione ticket (TGT) è uguale all'ora di ciò che resta nel gruppo A. Se sono anche un membro di un altro limite di tempo gruppo B, che ha una durata inferiore rispetto a un gruppo, la durata TGT è uguale a tempo che rimanente nel gruppo B.  
  
-   Nuove funzionalità di monitoraggio per consentono di identificare che ha richiesto l'accesso, il tipo di accesso è stato concesso e quali attività sono state eseguite.  
  
**Requisiti**  
  
-   Microsoft Identity Manager  
  
-   Livello di funzionalità di Windows Server 2012 R2 o versioni successive di foresta di Active Directory.  
  
## <a name="BKMK_AzureADJoin"></a>Appartenenza ad Azure AD  
Azure Active Directory Join migliora le esperienze di identità per enterprise e business EDU clienti - con funzionalità migliorate per i dispositivi personali e aziendali.  
  
Vantaggi:  
  
-   **Disponibilità delle attuali impostazioni** nei dispositivi Windows di proprietà della società. Servizi ossigeno non richiedono un account Microsoft personale: eseguono ora disattivare account aziendali esistenti degli utenti per garantire la conformità. Servizi ossigeno funzionerà su computer che fanno parte di un dominio di Windows locale, e PC e dispositivi connessi "" a tenant di Azure AD ("dominio cloud"). Queste impostazioni includono:  
  
    -   Il roaming o personalizzazione, le impostazioni di accessibilità e le credenziali  
  
    -   Backup e ripristino  
  
    -   Accesso a Microsoft Store con account aziendale  
  
    -   Riquadri animati e notifiche  
  
-   **Accedere alle risorse aziendale** su dispositivi mobili (telefoni, phablets) che non possono essere aggiunti a un dominio di Windows, se sono proprietà della società o BYOD  
  
-   **Single-Sign On** a Office 365 e altre applicazioni aziendale, siti Web e risorse.  
  
-   **Nei dispositivi BYOD**, aggiungere un account aziendale (da un dominio locale o Azure AD) in un dispositivo personali e usufruire del servizio SSO per l'uso di risorse, tramite le applicazioni e sul web, in modo che consente di garantire la conformità con nuove funzionalità come l'attestazione condizionale controllo dell'Account e l'integrità del dispositivo.  
  
-   **Integrazione con MDM** consente di registrare automaticamente i dispositivi per il MDM (Intune o terze parti)  
  
-   **Impostare la modalità "Continua" e dispositivi condivisi** per più utenti nell'organizzazione  
  
-   **Esperienza di sviluppo** consente di creare applicazioni che ciò sia aziendali che personali contesti di uno stack di programmazione condiviso.  
  
-   **Immagine** opzione consente di scegliere tra l'immagine e consentire agli utenti di configurare i dispositivi di proprietà della società direttamente durante l'esperienza di prima esecuzione.  
  
Per ulteriori informazioni, vedere [Windows 10 per l'azienda: modalità di utilizzo di dispositivi per lavoro](https://azure.microsoft.com/en-us/documentation/articles/active-directory-azureadjoin-windows10-devices-overview/?rnd=1).  
  
## <a name="BKMK_IDLocker"></a>Microsoft Passport  
Microsoft Passport è un'autenticazione basata su chiave nuovo approccio alle organizzazioni e ai consumatori, che vanno oltre le password. Questa forma di autenticazione si basa su violazioni, furti e credenziali resistente per attività di phishing.  
  
L'utente accede al dispositivo con un log biometrico o PIN su informazioni che sono collegate a un certificato o una coppia di chiavi asimmetrica. I provider di identità (IDP) convalidare l'utente abbinando la chiave pubblica dell'utente per IDLocker e fornisce log in informazioni tramite un tempo Password (OTP), Phonefactor o un diverso meccanismo di notifica.  
  
Per ulteriori informazioni, vedere [autenticazione delle identità senza password con Microsoft Passport](https://azure.microsoft.com/en-us/documentation/articles/active-directory-azureadjoin-passport/)  
  
## <a name="BKMK_FRSDeprecation"></a>Deprecazione di livelli di funzionalità del servizio Replica File (FRS) e Windows Server 2003  
Sebbene il servizio Replica File (FRS) e i livelli di funzionalità di Windows Server 2003 sono stati deprecati nelle versioni precedenti di Windows Server, vale la pena ripetere che il sistema operativo Windows Server 2003 non è più supportato. Di conseguenza, qualsiasi controller di dominio che esegue Windows Server 2003 devono essere rimossi dal dominio. Il livello di funzionalità del dominio e foresta deve essere generato almeno Windows Server 2008 per evitare che un controller di dominio che esegue una versione precedente di Windows Server venga aggiunto all'ambiente.  
  
In Windows Server 2008 e più livelli di funzionalità dominio, replica del file system distribuito (DFS, Distributed File Service) viene utilizzata per replicare i contenuti della cartella SYSVOL tra i controller di dominio. Se si crea un nuovo dominio a livello funzionale di dominio di Windows Server 2008 o versione successiva, replica DFS viene automaticamente utilizzata per replicare SYSVOL. Se hai creato il dominio a un livello funzionale inferiore, è necessario eseguire la migrazione da utilizza FRS a replica DFS per SYSVOL. Per la procedura di migrazione, è possibile seguire la [procedure su TechNet](https://technet.microsoft.com/library/dd640019(v=WS.10).aspx) o è possibile fare riferimento al [semplificata di set di passaggi nel blog sui File CAB del Team di archiviazione](http://blogs.technet.com/b/filecab/archive/2014/06/25/streamlined-migration-of-frs-to-dfsr-sysvol.aspx).  
  
I livelli di funzionalità dominio e foresta Windows Server 2003 continuano a essere supportato, ma le organizzazioni devono aumentare il livello di funzionalità a Windows Server 2008 (o versione successiva, se possibile) per garantire la compatibilità di replica di SYSVOL e il supporto in futuro. Inoltre, sono disponibili molti altri vantaggi e funzionalità a più livelli di funzionalità superiore. Le risorse seguenti per ulteriori informazioni, vedere:  
  
-   [Informazioni di dominio Active Directory Services livelli di funzionalità (AD DS)](ad-ds/active-directory-functional-levels.md)  
  
-   [Aumentare il livello funzionale di dominio](https://technet.microsoft.com/library/cc753104.aspx)  
  
-   [Aumentare il livello funzionale di foresta](https://technet.microsoft.com/library/cc730985.aspx)  
  
