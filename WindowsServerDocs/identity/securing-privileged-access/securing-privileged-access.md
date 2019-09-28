---
title: Protezione dell'accesso con privilegi
description: Approccio in più fasi alla sicurezza dell'accesso con privilegi
ms.prod: windows-server
ms.topic: conceptual
ms.assetid: f5dec0c2-06fe-4c91-9bdc-67cc6a3ede60
ms.date: 02/25/2019
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: mas
ms.openlocfilehash: e6ff22d0563fa11aa633004966b2cd2648ba5877
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71357696"
---
# <a name="securing-privileged-access"></a>Protezione dell'accesso con privilegi

>Si applica a: Windows Server

La protezione dell'accesso con privilegi è il primo importante passaggio per la definizione delle garanzie di sicurezza per i beni aziendali in un'organizzazione moderna. La protezione della maggior parte o di tutti gli asset aziendali in un'organizzazione IT dipende dall'integrità degli account con privilegi usati per amministrare, gestire e sviluppare. Gli utenti malintenzionati informatici spesso si riferiscono a questi account e ad altri elementi di accesso con privilegi per ottenere l'accesso ai dati e ai sistemi usando attacchi di furto delle credenziali come [pass-the-hash e pass-the-ticket](https://www.microsoft.com/pth).

La protezione dell'accesso con privilegi da avversari determinati richiede di adottare un approccio completo e ponderato per isolare questi sistemi dai rischi.

## <a name="what-are-privileged-accounts"></a>Che cosa sono gli account con privilegi?

Prima di descrivere come proteggerli, è possibile definire account con privilegi.

Gli account con privilegi come gli amministratori di Active Directory Domain Services hanno accesso diretto o indiretto alla maggior parte o a tutti gli asset di un'organizzazione IT, rendendo un compromesso di questi account un rischio aziendale significativo.

## <a name="why-securing-privileged-access-is-important"></a>Perché la protezione dell'accesso con privilegi è importante?

Gli autori di attacchi informatici si concentrano sull'accesso con privilegi a sistemi come Active Directory (AD) per ottenere rapidamente l'accesso a tutti i dati di destinazione di un'organizzazione. Gli approcci di sicurezza tradizionali si sono concentrati sulla rete e sui firewall come perimetro di sicurezza primario, ma l'efficacia della sicurezza della rete è stata notevolmente ridotta di due tendenze:

* Le organizzazioni ospitano i dati e le risorse al di fuori del limite di rete tradizionale nei PC portatili aziendali, dispositivi come telefoni cellulari e tablet, servizi cloud e BYOD (Bring Your Own Devices)
* Gli antagonisti hanno dimostrato una capacità sempre maggiore di ottenere l'accesso alle workstation all'interno del limite di rete per mezzo del phishing e di altri attacchi alla posta elettronica e al Web.

Questi fattori richiedono la creazione di un perimetro di sicurezza moderno rispetto ai controlli di identità di autenticazione e autorizzazione, oltre alla strategia tradizionale perimetrale di rete. Un perimetro di sicurezza viene definito come un set coerente di controlli tra asset e le relative minacce. Gli account con privilegi sono in grado di controllare questo nuovo perimetro di sicurezza, quindi è fondamentale proteggere l'accesso con privilegi.

![Diagramma che mostra il livello di identità di un'organizzazione](../media/securing-privileged-access/PAW_LP_Fig2.JPG)

Un utente malintenzionato che acquisisce il controllo di un account amministrativo può usare tali privilegi per aumentare il loro effetto nell'organizzazione di destinazione, come illustrato di seguito:

![Diagramma che illustra in che modo un antagonista che ottiene il controllo di un account amministrativo può usare tali privilegi per perseguire il suo obiettivo a spese dell'organizzazione di destinazione](../media/securing-privileged-access/PAW_LP_Fig3.JPG)

La figura seguente illustra due percorsi:

* Un percorso "blu" in cui viene usato un account utente standard per l'accesso senza privilegi a risorse quali la posta elettronica e l'esplorazione Web e il lavoro giornaliero completato.

   > [!NOTE]
   > Gli elementi del percorso blu descritti in un secondo momento indicano protezioni ambientali estese che si estendono oltre gli account amministrativi.

* Un percorso "rosso" in cui si verifica l'accesso con privilegi in un dispositivo con protezione avanzata per ridurre il rischio di phishing e altri attacchi Web e di posta elettronica.

![Diagramma che mostra il "percorso" separato per l'amministrazione che la roadmap stabilisce per isolare le attività di accesso con privilegi dalle attività utente standard ad alto rischio, come l'esplorazione Web e l'accesso alla posta elettronica](../media/securing-privileged-access/PAW_LP_Fig4.JPG)

## <a name="securing-privileged-access-roadmap"></a>Sicurezza della roadmap di accesso con privilegi

La roadmap è progettata per ottimizzare l'utilizzo delle tecnologie Microsoft già distribuite, sfruttare le tecnologie cloud per migliorare la sicurezza e integrare gli strumenti di sicurezza di terze parti che potrebbero essere già stati distribuiti.

La roadmap delle raccomandazioni Microsoft è suddivisa in tre fasi:

* [Fase 1: Primi 30 giorni]()
   * WINS rapido con un impatto positivo significativo.
* [Fase 2: 90 giorni]()
   * Miglioramenti significativi incrementali.
* [Fase 3: In corso]()
   * Miglioramento e miglioramento della sicurezza.

Il piano dà priorità alle implementazioni più efficaci e rapide in base alle nostre esperienze con questi attacchi e l'implementazione di soluzioni. 

Si consiglia di seguire questo piano di azione per proteggere l'accesso con privilegi da antagonisti specifici. È possibile modificare questo piano di azione per soddisfare le funzionalità esistenti e i requisiti specifici dell'organizzazione.

> [!NOTE]
> La protezione dell'accesso con privilegi richiede una vasta gamma di elementi, inclusi componenti tecnici (difese degli host, protezioni account, gestione delle identità e così via), modifiche da elaborare, procedure amministrative e conoscenza. Le sequenze temporali per il piano di azione sono approssimative e si basano sulla nostra esperienza con le implementazioni per i clienti. La durata può variare all'interno dell'organizzazione a seconda della complessità dell'ambiente e dai processi di gestione delle modifiche.

## <a name="phase-1-quick-wins-with-minimal-operational-complexity"></a>Fase 1: WINS rapido con complessità operativa minima

La fase 1 della roadmap è incentrata sulla riduzione rapida delle tecniche di attacco usate più di frequente per il furto e l'abuso di credenziali. La fase 1 è progettata per essere implementata in circa 30 giorni e viene illustrata in questo diagramma:

![Diagramma fase 1: 1. Separare l'account amministratore e l'account utente, 2. Password di amministratore locale Just-in-time, 3. Workstation di amministrazione-fase 1, 4. Rilevamento degli attacchi di identità](../media/securing-privileged-access/PAW_LP_Fig6.JPG)

### <a name="1-separate-accounts"></a>1. Account distinti

Per separare i rischi per Internet (attacchi di phishing, esplorazione Web) dagli account di accesso con privilegi, creare un account dedicato per tutto il personale con accesso con privilegi. Gli amministratori non dovrebbero esplorare il Web, controllare la posta elettronica e svolgere attività di produttività quotidiane con account con privilegi elevati. Altre informazioni su questo argomento sono disponibili nella sezione [account amministrativi distinti](securing-privileged-access-reference-material.md#separate-administrative-accounts) del documento di riferimento.

Seguire le istruzioni riportate nell'articolo [gestire gli account di accesso di emergenza in Azure ad](/azure/active-directory/users-groups-roles/directory-emergency-access) per creare almeno due account di accesso di emergenza, con diritti di amministratore assegnati in modo permanente, sia nell'ambiente ad locale che in quello di Azure ad. Questi account vengono utilizzati solo quando gli account amministratore tradizionali non sono in grado di eseguire un'attività richiesta, ad esempio in caso di emergenza.

### <a name="2-just-in-time-local-admin-passwords"></a>2. Password di amministratore locale Just-in-Time

Per attenuare il rischio che un utente malintenzionato rubi un hash della password dell'account amministratore locale dal database SAM locale e lo abusi per attaccare altri computer, le organizzazioni devono assicurarsi che ogni computer disponga di una password di amministratore locale univoca. Lo strumento di soluzione per la password dell'amministratore locale può configurare password casuali univoche in ogni workstation e server archiviarle nel Active Directory (AD) protetto da un ACL. Solo gli utenti autorizzati idonei possono leggere o richiedere la reimpostazione delle password dell'account amministratore locale. È possibile ottenere i giri da usare nelle workstation e nei server dall' [area download Microsoft](http://Aka.ms/LAPS).

Altre informazioni aggiuntive per il funzionamento di un ambiente con giri e paw sono disponibili nella sezione [standard operativi basati sul principio di origine pulita](securing-privileged-access-reference-material.md#operational-standards-based-on-clean-source-principle).

### <a name="3-administrative-workstations"></a>3. Workstation amministrative

Come misura di sicurezza iniziale per gli utenti con Azure Active Directory e con privilegi amministrativi Active Directory locali tradizionali, assicurarsi che utilizzino dispositivi Windows 10 configurati con gli [standard per un dispositivo Windows 10 altamente sicuro ](/windows-hardware/design/device-experiences/oem-highly-secure). Gli account amministratore con privilegi non devono essere membri del gruppo Administrators locale delle workstation amministrative.  L'elevazione dei privilegi tramite il controllo di accesso utente può essere usata quando è necessario apportare modifiche alla configurazione per le workstation.  Inoltre, è necessario applicare la linea di base di sicurezza di Windows 10 alle workstation per rafforzare ulteriormente il dispositivo.

### <a name="4-identity-attack-detection"></a>4. Rilevamento degli attacchi di identità

[Azure Advanced Threat Protection (ATP)](/azure-advanced-threat-protection/what-is-atp) è una soluzione di sicurezza basata sul cloud che consente di identificare, rilevare e individuare minacce avanzate, identità compromesse e azioni Insider dannose dirette all'Active Directory locale ambiente.

## <a name="phase-2-significant-incremental-improvements"></a>Fase 2: Miglioramenti significativi incrementali

La fase 2 si basa sul lavoro svolto nella fase 1 ed è progettata per essere completata in circa 90 giorni. I passaggi di questa fase sono rappresentati nel diagramma:

![Diagramma fase 2: 1. Windows Hello for business/multi-factor authentication, 2. Implementazione PAW, 3. Privilegi just-in-time, 4. Credential Guard, 5. Credenziali perse, 6. Rilevamento della vulnerabilità di spostamento laterale](../media/securing-privileged-access/PAW_LP_Fig7.JPG)

### <a name="1-require-windows-hello-for-business-and-mfa"></a>1. Richiedi Windows Hello for business ed autenticazione a più fattori

Gli amministratori possono trarre vantaggio dalla facilità d'uso associata a Windows Hello for business. Gli amministratori possono sostituire le password complesse con l'autenticazione a due fattori avanzata nei PC. Un utente malintenzionato deve avere sia il dispositivo che il PIN o le informazioni biometriche, è molto più difficile ottenere l'accesso senza la conoscenza del dipendente. Altre informazioni su Windows Hello for business e il percorso per la distribuzione sono disponibili nell'articolo [Panoramica di Windows Hello for business](/windows/security/identity-protection/hello-for-business/hello-overview)

Abilitare l'autenticazione a più fattori per gli account amministratore in Azure AD usando l'autenticazione a più fattori di Azure. Abilitare almeno i [criteri di accesso condizionale](/azure/active-directory/conditional-access/baseline-protection#require-mfa-for-admins) per la protezione di base ulteriori informazioni su Azure multi-factor authentication sono disponibili nell'articolo [distribuire Azure multi-factor authentication basato sul cloud](/azure/active-directory/authentication/howto-mfa-getstarted)

### <a name="2-deploy-paw-to-all-privileged-identity-access-account-holders"></a>2. Distribuire PAW a tutti i titolari dell'account di accesso di identità con privilegi

Continuando il processo di separazione degli account con privilegi da minacce rilevate in posta elettronica, esplorazione Web e altre attività non amministrative, è necessario implementare workstation con accesso con privilegi dedicati (PAW) per tutto il personale con accesso con privilegi al sistemi informativi dell'organizzazione. Altre informazioni aggiuntive per la distribuzione di PAW sono disponibili nell'articolo [workstation con accesso con privilegi](privileged-access-workstations.md#paw-phased-implementation).

### <a name="3-just-in-time-privileges"></a>3. Privilegi just-in-Time

Per ridurre il tempo di esposizione dei privilegi e aumentare la visibilità sul loro utilizzo, fornire i privilegi JIT (just-in-Time) usando una soluzione appropriata, ad esempio quelli riportati di seguito o altre soluzioni di terze parti:

* Per Active Directory Domain Services (AD DS), usare la funzionalità [Privileged Access Manager (PAM)](/microsoft-identity-manager/pam/privileged-identity-management-for-active-directory-domain-services) di Microsoft Identity Manager (MIM).
* Per Azure Active Directory, usare la funzionalità [Azure AD Privileged Identity Management (PIM)](/azure/active-directory/privileged-identity-management/pim-deployment-plan).

### <a name="4-enable-windows-defender-credential-guard"></a>4. Abilitare Windows Defender Credential Guard

L'abilitazione di Credential Guard consente di proteggere gli hash delle password NTLM, i ticket di concessione dei ticket Kerberos e le credenziali archiviate dalle applicazioni come credenziali di dominio. Questa funzionalità consente di evitare gli attacchi di furto di credenziali, ad esempio pass-the-hash o pass-the-ticket, aumentando la difficoltà di pivot nell'ambiente usando le credenziali rubate. Le informazioni sul funzionamento di Credential Guard e su come eseguire la distribuzione sono disponibili nell'articolo [proteggere le credenziali di dominio derivato con Windows Defender Credential Guard](/windows/security/identity-protection/credential-guard/credential-guard).

### <a name="5-leaked-credentials-reporting"></a>5. Segnalazione di credenziali perse

"Ogni giorno Microsoft analizza oltre 6,5 mila miliardi segnali per identificare le minacce emergenti e proteggere i clienti"- [Microsoft in base ai numeri](https://news.microsoft.com/bythenumbers/cyber-attacks)

Abilitare Microsoft Azure AD Identity Protection per segnalare agli utenti con credenziali perse, in modo da poterli correggere. È possibile sfruttare [Azure ad Identity Protection](/azure/active-directory/identity-protection/index) per aiutare l'organizzazione a proteggere gli ambienti cloud e ibridi dalle minacce.

### <a name="6-azure-atp-lateral-movement-paths"></a>6. Percorsi di spostamento laterale di Azure ATP

Assicurarsi che i titolari dell'account di accesso con privilegi stiano usando la propria PAW per l'amministrazione, in modo che un account senza privilegi compromessi non possa accedere a un account con privilegi tramite attacchi di furto delle credenziali, ad esempio pass-the-hash o pass-the-ticket. I [percorsi di spostamento laterale di Azure ATP (LMPS)](/azure-advanced-threat-protection/use-case-lateral-movement-path) consentono di comprendere facilmente la creazione di report per identificare la posizione in cui gli account con privilegi possono essere compromessi.

## <a name="phase-3-security-improvement-and-sustainment"></a>Fase 3: Miglioramento della sicurezza e sustain

La fase 3 della roadmap si basa sui passaggi eseguiti nelle fasi 1 e 2 per rafforzare il comportamento di sicurezza. La fase 3 viene illustrata visivamente in questo diagramma:

![Fase 3: 1. Esaminare RBAC, 2. Ridurre le superfici di attacco, 3. Integrare i log con SEIM, 4. Automazione delle credenziali perse](../media/securing-privileged-access/PAW_LP_Fig8.JPG)

Queste funzionalità si basano sui passaggi delle fasi precedenti e spostano le difese in una postura più proattiva. Questa fase non ha una sequenza temporale specifica e potrebbe richiedere più tempo per l'implementazione in base alla propria organizzazione.

### <a name="1-review-role-based-access-control"></a>1. Esaminare il controllo degli accessi in base al ruolo

Usando il modello a tre livelli descritto nell'articolo [Active Directory modello di livello amministrativo](securing-privileged-access-reference-material.md), esaminare e verificare che gli amministratori di livello inferiore non dispongano dell'accesso amministrativo alle risorse di livello superiore (appartenenze a gruppi, ACL per gli account utente e così via).

### <a name="2-reduce-attack-surfaces"></a>2. Riduzione delle superfici di attacco

Per rafforzare i carichi di lavoro di identità, inclusi i domini, i controller di dominio, ADFS e Azure AD Connect come compromettere uno di questi sistemi, è possibile che si verifichi un compromesso tra gli altri sistemi dell'organizzazione. Gli articoli che [riducono la superficie di attacco Active Directory](../ad-ds/plan/security-best-practices/reducing-the-active-directory-attack-surface.md) e [cinque passaggi per la sicurezza dell'infrastruttura di identità](/azure/security/azure-ad-secure-steps) forniscono indicazioni per la protezione degli ambienti locali e ibridi di identità.

### <a name="3-integrate-logs-with-siem"></a>3. Integrare i log con SIEM

L'integrazione dell'accesso a uno strumento SIEM centralizzato può consentire all'organizzazione di analizzare, rilevare e rispondere agli eventi di sicurezza. Gli articoli che [monitorano Active Directory per i segnali di compromissione](../ad-ds/plan/security-best-practices/monitoring-active-directory-for-signs-of-compromise.md) e [l'appendice L: Gli eventi da](../ad-ds/plan/appendix-l--events-to-monitor.md) monitorare forniscono indicazioni sugli eventi che devono essere monitorati nell'ambiente in uso.

Questo è parte del piano oltre perché l'aggregazione, la creazione e l'ottimizzazione degli avvisi in una gestione di informazioni ed eventi di sicurezza (SIEM) richiede analisti qualificati (a differenza di Azure ATP nel piano di 30 giorni che include gli avvisi predefiniti)

### <a name="4-leaked-credentials---force-password-reset"></a>4. Credenziali perse: forzare la reimpostazione della password

Continuare a migliorare il comportamento di sicurezza abilitando Azure AD Identity Protection per forzare automaticamente le reimpostazioni della password quando si sospetta la compromissione delle password. Le linee guida disponibili nell'articolo [usare gli eventi di rischio per attivare multi-factor authentication e le modifiche della password](/azure/active-directory/authentication/tutorial-risk-based-sspr-mfa) spiegano come abilitarlo usando un criterio di accesso condizionale.

## <a name="am-i-done"></a>Finito?

La risposta breve è no.

I cattivi non si arrestano mai, quindi non è possibile. Questa guida di orientamento consente all'organizzazione di proteggersi dalle minacce attualmente note, perché gli utenti malintenzionati si evolvono e cambiano continuamente. Si consiglia di visualizzare la sicurezza come un processo continuo mirato a elevare i costi e a ridurre la percentuale di successo degli avversari destinati all'ambiente.

Anche se non è l'unica parte del programma di sicurezza dell'organizzazione che protegge l'accesso con privilegi è un componente fondamentale della strategia di sicurezza.
