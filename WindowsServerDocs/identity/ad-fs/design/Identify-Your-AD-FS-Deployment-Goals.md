---
ms.assetid: c81b8291-fba5-4b30-a43d-7feb2f4b66be
title: Guida alla progettazione di ADFS in Windows Server 2012 R2
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: f618add4c4803142b3bd7278908834a412f30999
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2017
---
# <a name="identify-your-ad-fs-deployment-goals"></a>Identificare gli obiettivi di distribuzione AD FS

>Si applica a: Windows Server 2016, Windows Server 2012 R2

Identificazione corretta degli obiettivi di distribuzione di Active Directory Federation Services \(AD FS\) è essenziale per il successo del tuo progetto di progettazione di ADFS. Definire la priorità e, se possibile, combinare gli obiettivi di distribuzione in modo che sia possibile progettare e distribuire ADFS utilizzando un approccio iterativo. Possono sfruttare esistenti, documentati e predefiniti AD FS gli obiettivi di distribuzione sono rilevanti per i progetti di ADFS e sviluppano una soluzione appropriata per la situazione specifica.  
  
Le versioni precedenti di ADFS sono state distribuite più comunemente per ottenere quanto segue:  
  
-   Fornire ai dipendenti o ai clienti con una basata sul Web, esperienza SSO quando accedono alle applicazioni basate su claims\ all'interno dell'azienda.  
  
-   Fornire ai dipendenti o ai clienti un'esperienza SSO basata sul Web, per accedere alle risorse in qualsiasi organizzazione partner federativo.  
  
-   Fornire ai dipendenti o ai clienti con una basata sul Web, esperienza SSO quando internamente l'accesso remoto ospitato siti Web o servizi.  
  
-   Fornire ai dipendenti o ai clienti un'esperienza SSO basata sul Web, quando si accede a risorse o ai servizi nel cloud.  
  
Oltre a questi elementi, AD FS in Windows Server® 2012 R2 aggiunge funzionalità che consentono di ottenere quanto segue:  
  
-   Aggiunta alla rete aziendale di dispositivo per SSO e trasparente secondo autenticazione fattori. In questo modo le organizzazioni consentire l'accesso dai dispositivi personali dell'utente e gestire i rischi quando viene fornito l'accesso.  
  
-   Gestione dei rischi con il controllo di accesso più fattori. AD FS fornisce un elevato livello di autorizzazione che controlla chi ha accesso a quali applicazioni. Questo può essere basato su attributi di utente \ (UPN, posta elettronica, l'appartenenza al gruppo di sicurezza, complessità dell'autenticazione e così via \), gli attributi dei dispositivi \ (se il dispositivo è joined\ all'area di lavoro) o richiedere gli attributi \ (rete percorso, l'indirizzo IP o agent\. utente).  
  
-   Gestione dei rischi con l'autenticazione a più fattori aggiuntiva per le applicazioni sensibili. ADFS consente di controllare i criteri per richiedere l'autenticazione a più fattori a livello globale o in ogni applicazione. Inoltre, AD FS fornisce dei punti di estendibilità per qualsiasi fornitore a più fattori per l'integrazione in profondità per un'esperienza di fattori più sicuro e trasparente per gli utenti finali.  
  
-   Fornire funzionalità di autenticazione e autorizzazione per accedere alle risorse web dalla rete extranet protette da Proxy applicazione Web.  
  
Per riassumere, è possibile distribuire ADFS in Windows Server 2012 R2 per raggiungere gli obiettivi seguenti nell'organizzazione:  
  
### <a name="enable-your-users-to-access-resources-on-their-personal-devices-from-anywhere"></a>Consentire agli utenti di accedere alle risorse nei loro dispositivi personali da qualsiasi posizione  
  
-   Aggiunta alla rete aziendale che consente agli utenti di aggiungere i dispositivi personali ad Active Directory aziendale e ottenere di conseguenza l'accesso e un'esperienza trasparente quando accedono alle risorse aziendali da tali dispositivi.  
  
-   Pre-autenticazione delle risorse all'interno della rete azienda protette da proxy applicazione Web e accessibile da internet.  
  
-   Modifica della password per consentire agli utenti di modificare la password da qualsiasi dispositivo aggiunto all'area di lavoro quando la password è scaduto in modo che possono continuare ad accedere alle risorse.  
  
### <a name="enhance-your-access-control-risk-management-tools"></a>Migliorare gli strumenti di gestione dei rischi di controllo di accesso  
Gestione dei rischi è un aspetto importante della governance e conformità di ogni organizzazione IT. Sono disponibili numerosi il controllo degli accessi rischio gestione miglioramenti in ADFS in Windows Server® 2012 R2, inclusi i seguenti:  
  
-   Controlli flessibili in base al percorso di rete per definire la modalità di autenticazione dell'utente per accedere a un'applicazione protetta FS\ Active Directory.  
  
-   Criteri flessibili per determinare se un utente deve eseguire l'autenticazione a più fattori in base ai dati dell'utente, dati sul dispositivo e percorso di rete.  
  
-   Controllo delle applicazioni di Per\ per ignorare SSO e richiedere all'utente di fornire le credenziali ogni volta che accede a un'applicazione riservata.  
  
-   Criteri di accesso flessibile per applicazione per\ in base ai dati utente, dati del dispositivo o percorso di rete.  
  
-   AD FS Extranet Lockout, che consente agli amministratori di proteggere gli account di Active Directory da attacchi di forza bruta da internet.  
  
-   Revoca dell'accesso per qualsiasi dispositivo aggiunto all'area di lavoro disabilitato o eliminato in Active Directory.  
  
### <a name="use-ad-fs-to-enhance-the-sign-in-experience"></a>Utilizzo di ADFS per migliorare l'esperienza di accesso  
Di seguito sono nuove funzionalità di ADFS in Windows Server® 2012 R2 che consentono all'amministratore di personalizzare e migliorare l'esperienza di accesso:  
  
-   Personalizzazione unificata del servizio ADFS, in cui le modifiche sono apportate una sola volta e quindi propagate automaticamente al resto dei server federativi AD FS in una specifica farm.  
  
-   Accesso pagine aggiornate che aspetto moderno che soddisfano automaticamente a diversi fattori di forma.  
  
-   Supporto per il fallback automatico per l'autenticazione basata su forms\ per i dispositivi che non appartengono al dominio aziendale, ma ancora usati generare richieste di accesso dall'interno di \(intranet\) la rete aziendale.  
  
-   Controlli semplici per personalizzare il logo della società, l'immagine di illustrazione, i collegamenti standard per il supporto IT, pagina iniziale, privacy e così via.  
  
-   Personalizzazione dei messaggi di descrizione delle pagine di accesso.  
  
-   Personalizzazione dei temi web.  
  
-   Home Realm Discovery \(HRD\) basata sul suffisso aziendale dell'utente per una privacy avanzata dei partner aziendali.  
  
-   Filtro HRD in una singola applicazione per\ per individuare automaticamente un'area di autenticazione in base all'applicazione.  
  
-   Fare clic su base segnalazione errori per semplici IT la risoluzione dei problemi.  
  
-   Messaggi di errore personalizzabili.  
  
-   Scelta dell'autenticazione utente quando è disponibile più di un provider di autenticazione.  
  
## <a name="see-also"></a>Vedere anche  
[Guida alla progettazione di ADFS in Windows Server 2012 R2](../../ad-fs/design/AD-FS-Design-Guide-in-Windows-Server-2012-R2.md)  
  

