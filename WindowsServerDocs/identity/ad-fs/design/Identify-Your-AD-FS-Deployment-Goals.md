---
ms.assetid: c81b8291-fba5-4b30-a43d-7feb2f4b66be
title: Guida alla progettazione di AD FS in Windows Server 2012 R2
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: d79bfa571f420b23b1716cd1bceca33fdc96e038
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71359107"
---
# <a name="identify-your-ad-fs-deployment-goals"></a>Identificare gli obiettivi di distribuzione di ADFS

L'identificazione corretta degli \(obiettivi\) di distribuzione di Active Directory Federation Services ad FS è essenziale per il successo del progetto di progettazione ad FS. Classificare in ordine di priorità e, possibilmente, combinare gli obiettivi di distribuzione in modo che sia possibile progettare e distribuire AD FS usando un approccio iterativo. È possibile sfruttare i vantaggi degli obiettivi di distribuzione AD FS esistenti, documentati e predefiniti che sono rilevanti per la AD FS progetta e sviluppano una soluzione funzionante per la situazione specifica.  
  
Le versioni precedenti di AD FS sono state distribuite più di frequente per ottenere quanto segue:  
  
-   Offrire ai dipendenti o ai clienti un'esperienza\-SSO basata sul Web per l'accesso alle\-applicazioni basate su attestazioni all'interno dell'azienda.  
  
-   Offrire ai dipendenti o ai clienti un'esperienza\-SSO basata sul Web per accedere alle risorse in qualsiasi organizzazione partner federativo.  
  
-   Offrire ai dipendenti o ai clienti un'esperienza\-SSO basata sul Web quando si accede in remoto a siti o servizi Web ospitati internamente.  
  
-   Offrire ai dipendenti o ai clienti un'esperienza\-SSO basata sul Web per l'accesso alle risorse o ai servizi nel cloud.  
  
Oltre a queste, AD FS in Windows Server® 2012 R2 aggiunge funzionalità che consentono di ottenere i risultati seguenti:  
  
-   Aggiunta dei dispositivi all'area di lavoro per l'accesso SSO e l'autenticazione a due fattori trasparente. In questo modo le organizzazioni possono consentire l'accesso dai dispositivi personali dell'utente e gestire i rischi quando forniscono questo accesso.  
  
-   Gestione dei rischi con\-il controllo degli accessi a più fattori. AD FS fornisce un elevato livello di autorizzazioni per controllare a chi viene concesso l'accesso e per quali applicazioni. Questa operazione può essere basata sugli attributi \(utente UPN, posta elettronica, appartenenza ai gruppi di sicurezza, livello\)di autenticazione e \(così via, attributi del dispositivo\) se il dispositivo è aggiunto all'area di lavoro o attributi \(dirichiestapercorso di rete, indirizzo IP o agente\)utente.  
  
-   Gestione dei rischi con l'\-autenticazione a più fattori aggiuntiva per le applicazioni riservate. AD FS consente di controllare i criteri per potenzialmente richiedere\-l'autenticazione a più fattori a livello globale o in base alle singole applicazioni. Inoltre, ad FS fornisce punti di estendibilità per tutti\-i fornitori di più fattori per integrarsi in modo approfondito per un'esperienza sicura e\-trasparente per gli utenti finali.  
  
-   Fornire funzionalità di autenticazione e autorizzazione per l'accesso alle risorse Web dalla rete extranet protette dal proxy dell'applicazione Web.  
  
Per riepilogare, è possibile distribuire AD FS in Windows Server 2012 R2 per ottenere gli obiettivi seguenti nell'organizzazione:  
  
### <a name="enable-your-users-to-access-resources-on-their-personal-devices-from-anywhere"></a>Consenti agli utenti di accedere alle risorse sui dispositivi personali ovunque ci si trovi  
  
-   Aggiunta all'area di lavoro che consente agli utenti di aggiungere i propri dispositivi personali ad Active Directory dell'azienda e, di conseguenza, ottenere l'accesso e un'esperienza trasparente quando accedono alle risorse aziendali da tali dispositivi.  
  
-   Pre\--autenticazione delle risorse all'interno della rete aziendale protette dal proxy dell'applicazione Web e a cui si accede da Internet.  
  
-   Modifica della password per consentire agli utenti di modificare la password alla scadenza da qualsiasi dispositivo aggiunto all'area di lavoro così da poter continuare ad accedere alle risorse.  
  
### <a name="enhance-your-access-control-risk-management-tools"></a>Migliorare gli strumenti di gestione dei rischi per il controllo di accesso  
La gestione dei rischi è un aspetto importante della governance e della conformità di ogni organizzazione IT. Sono disponibili numerosi miglioramenti della gestione dei rischi di controllo degli accessi in AD FS in Windows Server® 2012 R2, inclusi i seguenti:  
  
-   Controlli flessibili in base al percorso di rete per definire il modo in cui un utente esegue\-l'autenticazione per accedere a un'applicazione protetta ad FS.  
  
-   Criteri flessibili per determinare se un utente deve eseguire\-l'autenticazione a più fattori in base ai dati dell'utente, ai dati del dispositivo e al percorso di rete.  
  
-   Per\-controllo applicazione per ignorare SSO e forzare l'utente a fornire le credenziali ogni volta che accedono a un'applicazione sensibile.  
  
-   Criteri flessibili\-per l'accesso alle applicazioni in base ai dati utente, ai dati del dispositivo o al percorso di rete.  
  
-   Il blocco della Extranet di AD FS consente agli amministratori di proteggere gli account di Active Directory da attacchi di forza bruta da Internet.  
  
-   Revoca dell'accesso per qualsiasi dispositivo aggiunto all'area di lavoro disabilitato o eliminato in Active Directory.  
  
### <a name="use-ad-fs-to-enhance-the-sign-in-experience"></a>Usare ad FS per migliorare l'esperienza\-di accesso  
Di seguito sono riportate le nuove funzionalità ad FS di Windows Server® 2012 R2 che consentono all'amministratore di\-personalizzare e migliorare l'esperienza di accesso:  
  
-   Personalizzazione unificata del servizio AD FS, in cui le modifiche vengono apportate una sola volta, quindi propagate automaticamente al resto dei server federativi AD FS in una specifica farm.  
  
-   Pagine di\-accesso aggiornate che sembrano moderne e soddisfano automaticamente i diversi fattori di forma.  
  
-   Supporto per il fallback automatico all'\-autenticazione basata su form per i dispositivi non aggiunti al dominio aziendale, ma che vengono ancora usati per generare richieste di accesso dall' \(interno\)della Intranet della rete aziendale.  
  
-   Controlli semplici per personalizzare il logo dell'azienda, l'immagine di illustrazione, i collegamenti standard per il supporto IT, la home page, la privacy e così via.  
  
-   Personalizzazione dei messaggi di descrizione nelle pagine\-di accesso.  
  
-   Personalizzazione dei temi Web.  
  
-   \(Individuazione\) dell'area di autenticazione principale in base al suffisso aziendale dell'utente per una maggiore privacy dei partner aziendali.  
  
-   Filtro HRD in\-base all'applicazione per selezionare automaticamente un'area di autenticazione in base all'applicazione.  
  
-   Segnalazione\-errori con un clic per semplificare la risoluzione dei problemi.  
  
-   Messaggi di errore personalizzabili.  
  
-   Scelta dell'autenticazione utente quando sono disponibili più provider di autenticazione.  
  
## <a name="see-also"></a>Vedere anche  
[Guida alla progettazione di AD FS in Windows Server 2012 R2](../../ad-fs/design/AD-FS-Design-Guide-in-Windows-Server-2012-R2.md)  
  

