---
ms.assetid: 6a852428-c1ec-4703-b3b3-a4bfdf8cbb9d
title: Cosa&#39;s novità di servizi di dominio Active Directory in Windows Server 2016
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/07/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: ffdeedfc2d818fe223c033aeecfe29d18365db7a
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59865542"
---
# <a name="whats-new-in-active-directory-domain-services-for-windows-server-2016"></a>Novità di Active Directory Domain Services per Windows Server 2016

>Si applica a: Windows Server 2016

Le seguenti nuove funzionalità in servizi di dominio Active Directory (AD DS) è migliorare la capacità per le organizzazioni di proteggere gli ambienti Active Directory e consentono di eseguire la migrazione a distribuzioni cloud-only e distribuzioni ibride, in alcune applicazioni e servizi sono ospitati nel cloud e ad altri utenti sono ospitati in locale. I miglioramenti includono:  
  
- [Gestione degli accessi con privilegi](https://docs.microsoft.com/microsoft-identity-manager/pam/privileged-identity-management-for-active-directory-domain-services)  
  
- [Estensione delle funzionalità del cloud ai dispositivi Windows 10 tramite Azure Active Directory Join](https://azure.microsoft.com/documentation/articles/active-directory-azureadjoin-overview/)
  
- [Connettere dispositivi aggiunti a un dominio ad Azure AD per Windows 10 esperienze](https://azure.microsoft.com/documentation/articles/active-directory-azureadjoin-devices-group-policy/)
  
- [Abilitare Microsoft Passport for Work nell'organizzazione](https://azure.microsoft.com/documentation/articles/active-directory-azureadjoin-passport-deployment/)
  
- [Deprecazione di livelli di funzionalità del servizio Replica File (FRS) e Windows Server 2003](ad-ds/active-directory-functional-levels.md)  
  
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

### <a name="requirements-for-privileged-access-management"></a>Requisiti per i privilegi di gestione degli accessi
  
- Microsoft Identity Manager  
  
- Livello di funzionalità di Windows Server 2012 R2 o versioni successive di foresta di Active Directory.  
  
## <a name="azure-ad-join"></a>Aggiunta ad Azure AD

Azure Active Directory Join migliora le esperienze di identità per enterprise e business EDU clienti - con funzionalità migliorate per i dispositivi personali e aziendali.  
  
Vantaggi:  
  
- **Disponibilità delle attuali impostazioni** nei dispositivi Windows di proprietà della società. Servizi ossigeno non richiedono un account Microsoft personale: eseguono ora disattivare account aziendali esistenti degli utenti per garantire la conformità. Servizi ossigeno funzionerà su computer che fanno parte di un dominio di Windows in locale, e PC e dispositivi connessi "" per il tenant di Azure AD ("dominio cloud"). Queste impostazioni includono:  

   - Il roaming o personalizzazione, le impostazioni di accessibilità e le credenziali  
   - Backup e ripristino  
   - Accesso a Microsoft Store con account aziendale  
   - I riquadri animati e notifiche  
  
- **Accedere alle risorse aziendali** nei dispositivi mobili (telefoni, phablets) che non possono essere aggiunti a un dominio di Windows, se sono proprietà della società o BYOD  
- **Single-Sign On** a Office 365 e altre applicazioni aziendale, siti Web e risorse.  
- **Sui dispositivi BYOD**, aggiungere un account aziendale (da un dominio locale o Azure AD) in un dispositivo personali e usufruire del servizio SSO per l'uso di risorse, tramite le applicazioni e sul web, in modo che consente di garantire la conformità con nuove funzionalità come l'attestazione condizionale controllo dell'Account e l'integrità del dispositivo.  
- **Integrazione MDM** consente di registrare automaticamente i dispositivi per il MDM (Intune o terze parti)  
- **Impostare la modalità "Continua" e dispositivi condivisi** per più utenti dell'organizzazione  
- **Esperienza dello sviluppatore** consente di creare applicazioni che ciò sia aziendali che personali contesti di uno stack di programmazione condiviso.  
- **Imaging** opzione consente di scegliere tra l'immagine e consentire agli utenti di configurare i dispositivi di proprietà della società direttamente durante la prima esecuzione.  
  
Per altre informazioni, vedere [Introduzione alla gestione dei dispositivi in Azure Active Directory](https://docs.microsoft.com/azure/active-directory/devices/overview).  
  
## <a name="windows-hello-for-business"></a>Windows Hello for Business

Windows Hello for Business è un'autenticazione basata su chiave approccio alle organizzazioni e ai consumatori, che vanno oltre le password. Questo tipo di autenticazione si basa su violazioni, furti e credenziali resistente per attività di phishing.  
  
L'utente accede al dispositivo con un log biometrico o PIN nelle informazioni collegate a un certificato o una coppia di chiavi asimmetrica. I provider di identità (IDP) convalidare l'utente abbinando la chiave pubblica dell'utente per IDLocker e fornisce log in informazioni tramite un tempo Password (OTP), telefono o un diverso meccanismo di notifica.  
  
Per altre informazioni, vedere [Windows Hello for Business](https://docs.microsoft.com/windows/security/identity-protection/hello-for-business/hello-identity-verification)  
  
## <a name="deprecation-of-file-replication-service-frs-and-windows-server-2003-functional-levels"></a>Deprecazione di livelli di funzionalità del servizio Replica File (FRS) e Windows Server 2003

Sebbene il servizio Replica File (FRS) e i livelli di funzionalità di Windows Server 2003 sono stati deprecati nelle versioni precedenti di Windows Server, vale la pena ripetere che il sistema operativo Windows Server 2003 non è più supportato. Di conseguenza, qualsiasi controller di dominio che esegue Windows Server 2003 devono essere rimossi dal dominio. Il livello di funzionalità del dominio e foresta deve essere generato almeno a Windows Server 2008 per evitare che un controller di dominio che esegue una versione precedente di Windows Server venga aggiunto all'ambiente.

In Windows Server 2008 e più livelli funzionali di dominio, replica del file system distribuito (DFS, Distributed File Service) viene utilizzata per la replica di contenuto della cartella SYSVOL tra i controller di dominio. Se si crea un nuovo dominio a livello funzionale di dominio di Windows Server 2008 o versione successiva, replica DFS viene automaticamente utilizzata per replicare SYSVOL. Se è stato creato il dominio a un livello funzionale inferiore, è necessario eseguire la migrazione da utilizza FRS a replica DFS per SYSVOL. Per i passaggi di migrazione, è possibile seguire [questi passaggi](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd640019\(v=ws.10\)) oppure è possibile vedere il [semplificata di set di passaggi nel blog del Team di archiviazione File CAB](http://blogs.technet.com/b/filecab/archive/2014/06/25/streamlined-migration-of-frs-to-dfsr-sysvol.aspx).  
  
I livelli di funzionalità dominio e foresta Windows Server 2003 continuano a essere supportate, ma le organizzazioni devono aumentare il livello di funzionalità a Windows Server 2008 (o versioni successive, se possibile) per garantire la compatibilità di replica di SYSVOL e il supporto in futuro. Inoltre, sono disponibili molti altri vantaggi e funzionalità a più livelli di funzionalità superiore. Per altre informazioni, vedere le risorse seguenti:  

- [La comprensione di Active Directory Domain Services i livelli di funzionalità (AD DS)](ad-ds/active-directory-functional-levels.md)  
- [Aumentare il livello funzionale di dominio](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc753104\(v=ws.11\))  
- [Aumentare il livello funzionale di foresta](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc730985\(v=ws.11\))  
