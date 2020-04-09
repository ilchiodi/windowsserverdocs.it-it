---
ms.assetid: 6a852428-c1ec-4703-b3b3-a4bfdf8cbb9d
title: Novità di Active Directory Domain Services in Windows Server 2016
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/07/2018
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: a82f45772e5e35afffc632de2b40c02c75b5e5e4
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80856284"
---
# <a name="whats-new-in-active-directory-domain-services-for-windows-server-2016"></a>Novità di Active Directory Domain Services per Windows Server 2016

>Si applica a: Windows Server 2016

Le seguenti nuove funzionalità in servizi di dominio Active Directory (AD DS) è migliorare la capacità per le organizzazioni di proteggere gli ambienti Active Directory e consentono di eseguire la migrazione a distribuzioni cloud-only e distribuzioni ibride, in alcune applicazioni e servizi sono ospitati nel cloud e ad altri utenti sono ospitati in locale. I miglioramenti includono:  
  
- [Privileged Access Management](https://docs.microsoft.com/microsoft-identity-manager/pam/privileged-identity-management-for-active-directory-domain-services)  
  
- [Estensione delle funzionalità del cloud ai dispositivi Windows 10 tramite l'aggiunta di Azure Active Directory](https://azure.microsoft.com/documentation/articles/active-directory-azureadjoin-overview/)
  
- [Connessione di dispositivi aggiunti a un dominio a Azure AD per le esperienze di Windows 10](https://azure.microsoft.com/documentation/articles/active-directory-azureadjoin-devices-group-policy/)
  
- [Abilitare Microsoft Passport for Work nell'organizzazione](https://azure.microsoft.com/documentation/articles/active-directory-azureadjoin-passport-deployment/)
  
- [Deprecazione del servizio Replica file (FRS) e dei livelli di funzionalità di Windows Server 2003](ad-ds/active-directory-functional-levels.md)  
  
## <a name="privileged-access-management"></a>Gestione dell'accesso con privilegi

Gestione accesso con privilegi (PAM) riduce i rischi di sicurezza per gli ambienti di Active Directory che sono causati da problemi di credenziali tecniche furto tali pass-the-hash, spear phishing e tipi di attacchi simili. Fornisce una nuova soluzione di accesso amministrativo che viene configurata tramite Microsoft Identity Manager (MIM). Sono stati introdotti PAM:  
  
- Bastione una nuova foresta di Active Directory, che viene eseguito il provisioning da MIM. La foresta bastione ha una relazione di trust con una foresta esistente PAM speciali. Fornisce un nuovo ambiente Active Directory che sono privi di qualsiasi attività dannose e isolamento da una foresta esistente per l'utilizzo di account privilegiati.  
  
- Nuovi processi in MIM per richiedere privilegi amministrativi, insieme ai nuovi flussi di lavoro in base l'approvazione delle richieste.  
  
- Nuovo shadow entità di sicurezza (gruppi) che viene eseguito il provisioning nella foresta bastione da MIM in risposta alle richieste di privilegi amministrativi. Le entità di sicurezza shadow hanno un attributo che fa riferimento il SID di un gruppo amministrativo in una foresta esistente. In questo modo il gruppo shadow per accedere alle risorse in una foresta esistente senza modificare qualsiasi elenchi di controllo di accesso (ACL).  
  
- Una funzionalità collegamenti in scadenza, che consente di scadenza appartenenza a un gruppo di ombreggiatura. Un utente può essere aggiunto al gruppo sufficiente tempo necessario per eseguire un'attività amministrativa. L'appartenenza a un limite di tempo è espresso da un valore di (durata TTL) time-to-live che viene propagato a una durata del ticket Kerberos.  
  
    > [!NOTE]  
    > Scadenza collegamenti sono disponibili in tutti gli attributi collegati. Mentre l'attributo collegato membro/memberOf relazione tra un gruppo e un utente è riportato l'esempio solo in una soluzione completa, ad esempio PAM è preconfigurata per utilizzare la funzionalità di collegamenti che sta per scadere.  
  
- Miglioramenti KDC sono incorporati controller di dominio Active Directory per limitare la durata ticket Kerberos sul valore più basso possibile time-to-live (TTL) nei casi in cui un utente dispone di più le appartenenze associate alla durata in gruppi amministrativi. Ad esempio, se si viene aggiunti a un gruppo di limite di tempo, quando si accede, durata Kerberos ticket di concessione ticket (TGT) è uguale all'ora di ciò che resta nel gruppo A. Se si è anche un membro di un altro limite di tempo gruppo B, che ha una DURATA inferiore rispetto a un gruppo, la durata TGT è uguale a tempo rimanente nel gruppo B.  
  
- Nuove funzionalità di monitoraggio consentono di identificare che ha richiesto l'accesso, il tipo di accesso è stato concesso e quali attività sono state eseguite.  

### <a name="requirements-for-privileged-access-management"></a>Requisiti per Privileged Access Management
  
- Microsoft Identity Manager  
  
- Livello di funzionalità di Windows Server 2012 R2 o versioni successive di foresta di Active Directory.  
  
## <a name="azure-ad-join"></a>Aggiunta ad Azure AD

Azure Active Directory Join migliora le esperienze di identità per enterprise e business EDU clienti - con funzionalità migliorate per i dispositivi personali e aziendali.  
  
Vantaggi  
  
- **Disponibilità delle attuali impostazioni** nei dispositivi Windows di proprietà della società. I servizi di ossigeno non richiedono più un account Microsoft personale: ora eseguono gli account di lavoro esistenti degli utenti per garantire la conformità. I servizi di ossigeno funzioneranno nei PC che fanno parte di un dominio Windows locale e i PC e i dispositivi aggiunti al tenant di Azure AD ("dominio cloud"). Queste impostazioni includono:  

   - Il roaming o personalizzazione, le impostazioni di accessibilità e le credenziali  
   - Backup e ripristino  
   - Accesso a Microsoft Store con account di lavoro  
   - I riquadri animati e notifiche  
  
- **Accedere alle risorse aziendali** nei dispositivi mobili (telefoni, phablet) che non possono essere aggiunti a un dominio di Windows, indipendentemente dal fatto che siano di proprietà di Corp o BYOD  
- **Single-Sign On** a Office 365 e altre applicazioni aziendale, siti Web e risorse.  
- **Sui dispositivi BYOD**, aggiungere un account aziendale (da un dominio locale o Azure AD) in un dispositivo personali e usufruire del servizio SSO per l'uso di risorse, tramite le applicazioni e sul web, in modo che consente di garantire la conformità con nuove funzionalità come l'attestazione condizionale controllo dell'Account e l'integrità del dispositivo.  
- **Integrazione MDM** consente di registrare automaticamente i dispositivi per il MDM (Intune o terze parti)  
- **Impostare la modalità "Continua" e dispositivi condivisi** per più utenti dell'organizzazione  
- **Esperienza dello sviluppatore** consente di creare applicazioni che ciò sia aziendali che personali contesti di uno stack di programmazione condiviso.  
- **Imaging** opzione consente di scegliere tra l'immagine e consentire agli utenti di configurare i dispositivi di proprietà della società direttamente durante la prima esecuzione.  
  
Per ulteriori informazioni, vedere [Introduzione alla gestione dei dispositivi in Azure Active Directory](https://docs.microsoft.com/azure/active-directory/devices/overview).  
  
## <a name="windows-hello-for-business"></a>Windows Hello for Business

Windows Hello for business è un approccio di autenticazione basato su chiavi per organizzazioni e consumer che va oltre le password. Questo tipo di autenticazione si basa su violazioni, furti e credenziali resistente per attività di phishing.  
  
L'utente accede al dispositivo con un log biometrico o PIN nelle informazioni collegate a un certificato o una coppia di chiavi asimmetrica. I provider di identità (IDP) convalidano l'utente eseguendo il mapping della chiave pubblica dell'utente a IDLocker e fornisce informazioni di accesso tramite password monouso (OTP), telefono o un meccanismo di notifica diverso.  
  
Per ulteriori informazioni, vedere [Windows Hello for business](https://docs.microsoft.com/windows/security/identity-protection/hello-for-business/hello-identity-verification)  
  
## <a name="deprecation-of-file-replication-service-frs-and-windows-server-2003-functional-levels"></a>Deprecazione di livelli di funzionalità del servizio Replica File (FRS) e Windows Server 2003

Sebbene il servizio Replica File (FRS) e i livelli di funzionalità di Windows Server 2003 sono stati deprecati nelle versioni precedenti di Windows Server, vale la pena ripetere che il sistema operativo Windows Server 2003 non è più supportato. Di conseguenza, qualsiasi controller di dominio che esegue Windows Server 2003 devono essere rimossi dal dominio. Il livello di funzionalità del dominio e foresta deve essere generato almeno a Windows Server 2008 per evitare che un controller di dominio che esegue una versione precedente di Windows Server venga aggiunto all'ambiente.

In Windows Server 2008 e più livelli funzionali di dominio, replica del file system distribuito (DFS, Distributed File Service) viene utilizzata per la replica di contenuto della cartella SYSVOL tra i controller di dominio. Se si crea un nuovo dominio a livello funzionale di dominio di Windows Server 2008 o versione successiva, replica DFS viene automaticamente utilizzata per replicare SYSVOL. Se è stato creato il dominio a un livello funzionale inferiore, è necessario eseguire la migrazione da utilizza FRS a replica DFS per SYSVOL. Per la procedura di migrazione, è possibile seguire [questa procedura](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd640019\(v=ws.10\)) oppure è possibile fare riferimento al [set di passaggi semplificato nel Blog del file CAB del team di archiviazione](https://blogs.technet.com/b/filecab/archive/2014/06/25/streamlined-migration-of-frs-to-dfsr-sysvol.aspx).  
  
I livelli di funzionalità dominio e foresta Windows Server 2003 continuano a essere supportate, ma le organizzazioni devono aumentare il livello di funzionalità a Windows Server 2008 (o versioni successive, se possibile) per garantire la compatibilità di replica di SYSVOL e il supporto in futuro. Inoltre, sono disponibili molti altri vantaggi e funzionalità a più livelli di funzionalità superiore. Per altre informazioni, vedere le risorse seguenti:  

- [Informazioni sui livelli di funzionalità di Active Directory Domain Services (AD DS)](ad-ds/active-directory-functional-levels.md)  
- [Aumentare il livello di funzionalità del dominio](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc753104\(v=ws.11\))  
- [Aumentare il livello di funzionalità della foresta](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc730985\(v=ws.11\))  
