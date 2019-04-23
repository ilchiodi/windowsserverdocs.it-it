---
ms.assetid: c81b8291-fba5-4b30-a43d-7feb2f4b66be
title: Guida alla progettazione di AD FS in Windows Server 2012 R2
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: f618add4c4803142b3bd7278908834a412f30999
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59862572"
---
# <a name="identify-your-ad-fs-deployment-goals"></a>Identificare gli obiettivi di distribuzione di ADFS

>Si applica a: Windows Server 2016, Windows Server 2012 R2

Identificazione corretta di Active Directory Federation Services \(ADFS\) degli obiettivi di distribuzione è essenziale per il successo del progetto di progettazione di AD FS. Definire le priorità e, se possibile, combinare gli obiettivi di distribuzione in modo che sia possibile progettare e distribuire AD FS con un approccio iterativo. È possibile sfruttare i vantaggi esistenti, documentati e predefiniti degli obiettivi di distribuzione di AD FS che sono rilevanti per le progettazioni di AD FS e sviluppano una soluzione appropriata per la situazione specifica.  
  
Le versioni precedenti di AD FS sono state distribuite più comunemente per ottenere quanto segue:  
  
-   Fornire ai dipendenti o i clienti con un sito web\-base, esperienza SSO quando l'accesso alle attestazioni\-basato su applicazioni all'interno dell'azienda.  
  
-   Fornire ai dipendenti o i clienti con un web\-esperienza basata su, l'accesso SSO per accedere alle risorse in qualsiasi organizzazione partner federativo.  
  
-   Fornire ai dipendenti o i clienti con un Web\-basato, esperienza SSO internamente l'accesso remoto ospitato siti Web o servizi.  
  
-   Fornire ai dipendenti o i clienti con un web\-basato, esperienza SSO quando si accede a risorse o servizi nel cloud.  
  
Oltre a queste, ADFS in Windows Server® 2012 R2 consente di aggiungere funzionalità che consentono di ottenere quanto segue:  
  
-   Aggiunta dei dispositivi all'area di lavoro per l'accesso SSO e l'autenticazione a due fattori trasparente. In questo modo le organizzazioni possono consentire l'accesso dai dispositivi personali dell'utente e gestire i rischi correlati quando si fornisce l'accesso.  
  
-   Gestione dei rischi con più\-fattore di controllo di accesso. AD FS fornisce un elevato livello di autorizzazioni per controllare a chi viene concesso l'accesso e per quali applicazioni. Ciò può essere basata su attributi utente \(UPN, posta elettronica, l'appartenenza al gruppo di sicurezza, livello di autenticazione e così via\), gli attributi del dispositivo \(se il dispositivo è aggiunto all'area di lavoro\) o richiedere gli attributi \(percorso di rete, indirizzo IP o agente utente\).  
  
-   Gestione dei rischi con più aggiuntive\-factor authentication per le applicazioni sensibili. ADFS consente di controllare i criteri per richiedere multi\-fattore di autenticazione a livello globale o in ogni singola applicazione. Inoltre, ADFS fornisce punti di estendibilità per qualsiasi multi\-fornitore factor integrare profondamente per una più sicuro e trasparente\-fattore per gli utenti finali.  
  
-   Fornendo funzionalità di autenticazione e autorizzazione per accedere alle risorse web dalla rete extranet protette da Proxy applicazione Web.  
  
Per riepilogare, è possibile distribuire ADFS in Windows Server 2012 R2 per realizzare gli obiettivi seguenti all'interno dell'organizzazione:  
  
### <a name="enable-your-users-to-access-resources-on-their-personal-devices-from-anywhere"></a>Abilitare gli utenti per accedere alle risorse dai dispositivi personali da qualsiasi posizione  
  
-   Aggiunta all'area di lavoro che consente agli utenti di aggiungere i propri dispositivi personali ad Active Directory dell'azienda e, di conseguenza, ottenere l'accesso e un'esperienza trasparente quando accedono alle risorse aziendali da tali dispositivi.  
  
-   Pre\-autenticazione delle risorse all'interno della rete azienda che sono protette da proxy applicazione Web e accessibili da internet.  
  
-   Modifica della password per consentire agli utenti di modificare la password alla scadenza da qualsiasi dispositivo aggiunto all'area di lavoro così da poter continuare ad accedere alle risorse.  
  
### <a name="enhance-your-access-control-risk-management-tools"></a>Migliorare gli strumenti di gestione dei rischi di controllo di accesso  
La gestione dei rischi è un aspetto importante della governance e della conformità di ogni organizzazione IT. Sono disponibili numerosi accesso controllo rischio miglioramenti di gestione in ADFS in Windows Server® 2012 R2, inclusi i seguenti:  
  
-   Controlli flessibili in basano alla posizione di rete per definire la modalità di autenticazione di un utente per accedere a un'istanza di ADFS\-applicazione protetta.  
  
-   Criteri flessibili per determinare se un utente deve eseguire multi\-l'autenticazione a fattore basato su dati dell'utente, i dati del dispositivo e percorso di rete.  
  
-   Per ogni\-controllo delle applicazioni di ignorare SSO e richiedere all'utente di fornire credenziali ogni volta che accedono a un'applicazione riservata.  
  
-   Flessibile per\-criteri di accesso delle applicazioni basano sui dati dell'utente, i dati del dispositivo o il percorso di rete.  
  
-   Il blocco della Extranet di AD FS consente agli amministratori di proteggere gli account di Active Directory da attacchi di forza bruta da Internet.  
  
-   Revoca dell'accesso per qualsiasi dispositivo aggiunto all'area di lavoro disabilitato o eliminato in Active Directory.  
  
### <a name="use-ad-fs-to-enhance-the-sign-in-experience"></a>Usare AD FS per migliorare il segno\-nell'esperienza  
Di seguito sono nuove funzionalità di AD FS in Windows Server® 2012 R2 che consentono di personalizzare e migliorare l'accesso amministratore\-nell'esperienza:  
  
-   Personalizzazione unificata del servizio AD FS, in cui le modifiche vengono apportate una sola volta, quindi propagate automaticamente al resto dei server federativi AD FS in una specifica farm.  
  
-   Aggiornato sign\-nelle pagine che aspetto moderno che soddisfano automaticamente a diversi fattori di forma.  
  
-   Supporto per il fallback automatico a un form\-basato su autenticazione per i dispositivi non aggiunti al dominio aziendale, ma sono ancora usati generano richieste di accesso dall'interno della rete aziendale \(intranet\).  
  
-   Controlli semplici per personalizzare il logo dell'azienda, l'immagine di illustrazione, i collegamenti standard per il supporto IT, la home page, la privacy e così via.  
  
-   Personalizzazione della descrizione dei messaggi di accesso\-nelle pagine.  
  
-   Personalizzazione dei temi Web.  
  
-   Home Realm Discovery \(HRD\) basata sul suffisso aziendale dell'utente per una privacy avanzata dei partner aziendali.  
  
-   Filtro HRD in una per ogni\-base all'applicazione per individuare automaticamente un'area di autenticazione in base all'applicazione.  
  
-   Uno\-fare clic su segnalazione errori per più facilmente IT la risoluzione dei problemi.  
  
-   Messaggi di errore personalizzabili.  
  
-   Scelta dell'autenticazione utente quando sono disponibili più provider di autenticazione.  
  
## <a name="see-also"></a>Vedere anche  
[Guida alla progettazione di AD FS in Windows Server 2012 R2](../../ad-fs/design/AD-FS-Design-Guide-in-Windows-Server-2012-R2.md)  
  

