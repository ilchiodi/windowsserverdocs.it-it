---
title: Protezione dell'accesso con privilegi
description: Approccio in più fasi alla protezione dell'accesso con privilegi
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
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71357696"
---
# <a name="securing-privileged-access"></a>Protezione dell'accesso con privilegi

>Si applica a: Windows Server

La protezione dell'accesso con privilegi è il primo importante passaggio per la definizione delle garanzie di sicurezza per i beni aziendali in un'organizzazione moderna. La sicurezza della maggior parte delle risorse aziendali di un'organizzazione IT dipende dall'integrità degli account con privilegi usati per le attività di amministrazione, gestione e sviluppo. I pirati informatici prendono spesso di mira questi account, e altri elementi che richiedono l'accesso con privilegi, per accedere a dati e sistemi usando attacchi finalizzati al furto di credenziali come [Pass-the-Hash e Pass-the-Ticket](https://www.microsoft.com/pth).

Per proteggere l'accesso con privilegi da antagonisti specifici è necessario adottare un approccio completo e ponderato per isolare questi sistemi da eventuali rischi.

## <a name="what-are-privileged-accounts"></a>Che cosa sono gli account con privilegi?

Prima di descrivere come proteggere gli account con privilegi è opportuno spiegare che cosa sono.

Gli account con privilegi, come gli amministratori di Active Directory Domain Services, hanno accesso diretto o indiretto alla maggior parte o alla totalità delle risorse di un'organizzazione IT. Di conseguenza, la compromissione di questi account rappresenta un rischio aziendale significativo.

## <a name="why-securing-privileged-access-is-important"></a>Perché è importante proteggere l'accesso con privilegi?

I pirati informatici si concentrano sull'accesso con privilegi a sistemi come Active Directory (AD) per accedere rapidamente a tutti i dati di un'organizzazione. Gli approcci alla sicurezza tradizionali si basano sulla rete e sui firewall come perimetro di sicurezza primario, ma l'efficacia della sicurezza di rete è diminuita in modo sostanziale a causa di due tendenze:

* Le organizzazioni ospitano dati e risorse all'esterno dei limiti di rete tradizionali, ovvero in PC aziendali mobili, su dispositivi quali telefoni cellulari e tablet, su servizi cloud e nei dispositivi di tipo BYOD (Bring Your Own Device).
* Gli antagonisti hanno dimostrato una capacità sempre maggiore di ottenere l'accesso alle workstation all'interno del limite di rete per mezzo del phishing e di altri attacchi alla posta elettronica e al Web.

Questi fattori richiedono la creazione di un perimetro di sicurezza moderno con controlli di autenticazione e autorizzazione delle identità, in aggiunta alla strategia tradizionale basata sul perimetro della rete. In questo contesto, un perimetro di sicurezza è definito come un set coerente di controlli tra le risorse e le minacce che queste possono subire. Gli account con privilegi detengono, di fatto, il controllo di questo nuovo perimetro di sicurezza ed è pertanto fondamentale proteggerne l'accesso.

![Diagramma che mostra il livello di identità di un'organizzazione](../media/securing-privileged-access/PAW_LP_Fig2.JPG)

Un utente malintenzionato che ottiene il controllo di un account amministrativo può usare i relativi privilegi per rendere più efficace il suo impatto sull'organizzazione di destinazione, come illustrato di seguito:

![Diagramma che illustra in che modo un antagonista che ottiene il controllo di un account amministrativo può usare tali privilegi per perseguire il suo obiettivo a spese dell'organizzazione di destinazione](../media/securing-privileged-access/PAW_LP_Fig3.JPG)

Nella figura sono illustrati due percorsi:

* Un percorso "blu", in cui viene usato un account utente standard per l'accesso senza privilegi a risorse quali la posta elettronica e l'esplorazione del Web e in cui vengono svolte le attività giornaliere.

   > [!NOTE]
   > Gli elementi del percorso blu descritti più avanti indicano protezioni a livello di ambiente che si estendono oltre gli account amministrativi.

* Un percorso "rosso", in cui viene usato l'accesso con privilegi in un dispositivo con protezione avanzata per ridurre il rischio di phishing e altri attacchi all'esplorazione del Web e alla posta elettronica.

![Diagramma che rappresenta il percorso separato per l'amministrazione definito dal piano di azione per isolare le attività di accesso con privilegi dalle attività di un utente standard ad alto rischio, come l'esplorazione del Web e l'accesso alla posta elettronica](../media/securing-privileged-access/PAW_LP_Fig4.JPG)

## <a name="securing-privileged-access-roadmap"></a>Piano di azione per la protezione dell'accesso con privilegi

Il piano è stato progettato per sfruttare al massimo l'uso di tecnologie Microsoft già distribuite, trarre vantaggio delle tecnologie cloud per migliorare la sicurezza e integrare eventuali strumenti di protezione di terze parti già distribuiti.

Il piano di azione consigliato da Microsoft è suddiviso in tre fasi:

* [Fase 1: primi 30 giorni]()
   * Vantaggi rapidi con un impatto di notevole efficacia.
* [Fase 2: 90 giorni]()
   * Miglioramenti incrementali significativi.
* [Fase 3: continuativa]()
   * Miglioramento e manutenzione delle soluzioni per la sicurezza.

Il piano dà priorità alle implementazioni più efficaci e rapide in base alle nostre esperienze con questi attacchi e l'implementazione di soluzioni. 

Si consiglia di seguire questo piano di azione per proteggere l'accesso con privilegi da antagonisti specifici. È possibile modificare questo piano di azione per soddisfare le funzionalità esistenti e i requisiti specifici dell'organizzazione.

> [!NOTE]
> La protezione dell'accesso con privilegi richiede una vasta gamma di elementi, inclusi componenti tecnici (difese degli host, protezioni account, gestione delle identità e così via), modifiche da elaborare, procedure amministrative e conoscenza. Le sequenze temporali per il piano di azione sono approssimative e si basano sulla nostra esperienza con le implementazioni per i clienti. La durata può variare all'interno dell'organizzazione a seconda della complessità dell'ambiente e dai processi di gestione delle modifiche.

## <a name="phase-1-quick-wins-with-minimal-operational-complexity"></a>Fase 1: vantaggi rapidi con complessità operativa minima

La fase 1 del piano di azione è incentrata sulla mitigazione rapida del rischio rappresentato dalle tecniche di attacco usate più di frequente per il furto di credenziali e gli abusi. La fase 1 è stata progettata in modo da essere implementata in circa 30 giorni ed è illustrata nel diagramma seguente:

![Diagramma della fase 1: 1. Account separati per amministratori e utenti, 2. Password di amministratore locale Just-In-Time, 3. Workstation amministrative della fase 1, 4. Rilevamento degli attacchi alle identità](../media/securing-privileged-access/PAW_LP_Fig6.JPG)

### <a name="1-separate-accounts"></a>1. Account separati

Per separare i rischi provenienti da Internet (attacchi di phishing, esplorazione del Web) dagli account con accesso privilegiato, puoi creare un account dedicato per tutto il personale che richiede questo tipo di accesso. Gli amministratori non dovrebbero esplorare il Web, controllare la posta elettronica e svolgere attività di produttività quotidiane usando account con privilegi elevati. Altre informazioni su questo argomento sono disponibili nella sezione [Account amministrativi separati](securing-privileged-access-reference-material.md#separate-administrative-accounts) del documento di riferimento.

Segui le istruzioni riportate nell'articolo [Gestire gli account di accesso di emergenza in Azure AD](/azure/active-directory/users-groups-roles/directory-emergency-access) per creare almeno due account di accesso di emergenza, con diritti di amministratore assegnati in modo permanente, sia nell'ambiente AD locale sia in Azure AD. Questi account vengono usati solo quando gli account amministratore tradizionali non sono in grado di eseguire un'attività necessaria, ad esempio in caso di emergenza.

### <a name="2-just-in-time-local-admin-passwords"></a>2. Password di amministratore locale Just-In-Time

Per mitigare il rischio che un utente malintenzionato sottragga l'hash di una password di account amministratore locale dal database SAM locale e ne faccia un uso improprio per attaccare altri computer, le organizzazioni devono assicurarsi che ogni computer disponga di una password di amministratore locale univoca. Lo strumento Soluzione password dell'amministratore locale (LAPS) può configurare password casuali univoche in ogni workstation e server e archiviarle in un'istanza di Active Directory (AD) protetta da un elenco di controllo di accesso. Solo gli utenti autorizzati idonei possono leggere o richiedere la reimpostazione di queste password di account amministratore locale. Puoi ottenere lo strumento LAPS da usare su workstation e server dall'[Area download Microsoft](http://Aka.ms/LAPS).

Altre indicazioni per il funzionamento di un ambiente con LAPS e workstation PAW sono disponibili nella sezione [Standard operativi basati sul principio di origine pulita](securing-privileged-access-reference-material.md#operational-standards-based-on-clean-source-principle).

### <a name="3-administrative-workstations"></a>3. Workstation amministrative

Come misura di sicurezza iniziale per gli utenti con Azure Active Directory e con i privilegi amministrativi tradizionali dell'ambiente Active Directory locale, assicurarsi che tali utenti usino dispositivi Windows 10 configurati con gli [standard per un dispositivo Windows 10 con protezione elevata](/windows-hardware/design/device-experiences/oem-highly-secure). Gli account amministratore con privilegi non devono essere membri del gruppo Administrators locale delle workstation amministrative.  Quando è necessario apportare modifiche alla configurazione delle workstation, può essere usata l'elevazione dei privilegi tramite Controllo di accesso utente.  Può inoltre essere opportuno applicare alle workstation gli elementi di base della sicurezza di Windows 10 per rafforzare ulteriormente il dispositivo.

### <a name="4-identity-attack-detection"></a>4. Rilevamento degli attacchi alle identità

[Azure Advanced Threat Protection (ATP)](/azure-advanced-threat-protection/what-is-atp) è una soluzione di sicurezza basata sul cloud che consente di identificare, rilevare e analizzare le minacce avanzate, le identità compromesse e le attività svolte da utenti interni malintenzionati ai danni dell'ambiente Active Directory locale.

## <a name="phase-2-significant-incremental-improvements"></a>Fase 2: miglioramenti incrementali significativi

La fase 2 si basa sul lavoro svolto nella fase 1 ed è stata progettata in modo da essere completata in circa 90 giorni. I passaggi di questa fase sono rappresentati nel diagramma:

![Diagramma della fase 2: 1. Windows Hello for Business / MFA, 2. Implementazione di PAW, 3. Privilegi Just-In-Time, 4. Credential Guard, 5. Perdita di credenziali, 6. Rilevamento della vulnerabilità degli spostamenti laterali](../media/securing-privileged-access/PAW_LP_Fig7.JPG)

### <a name="1-require-windows-hello-for-business-and-mfa"></a>1. Imporre l'uso di Windows Hello for Business e dell'autenticazione a più fattori

Gli amministratori possono trarre vantaggio dalla facilità d'uso offerta da Windows Hello for Business. Gli amministratori possono sostituire le proprie password complesse con l'autenticazione a due fattori avanzata sui PC. In questo modo, l'autore dell'attacco deve ottenere sia il dispositivo sia i dati biometrici o il PIN ed è molto più difficile che riesca ad accedere senza che il titolare dell'account se ne accorga. Altre informazioni su Windows Hello for Business e sul percorso per la distribuzione sono disponibili nell'articolo [Panoramica di Windows Hello for Business](/windows/security/identity-protection/hello-for-business/hello-overview).

Puoi abilitare l'autenticazione a più fattori per gli account amministratore in Azure AD usando Azure Multi-Factor Authentication (Azure MFA). Devono essere abilitati almeno i [criteri di accesso condizionale per la protezione di base](/azure/active-directory/conditional-access/baseline-protection#require-mfa-for-admins). Altre informazioni su Azure MFA sono disponibili nell'articolo [Distribuire il servizio Azure Multi-Factor Authentication basato sul cloud](/azure/active-directory/authentication/howto-mfa-getstarted).

### <a name="2-deploy-paw-to-all-privileged-identity-access-account-holders"></a>2. Distribuire PAW a tutti i titolari di account di accesso di identità con privilegi

Proseguendo lungo la linea del processo di separazione degli account con privilegi dalle minacce rilevate nella posta elettronica, nell'esplorazione del Web e in altre attività non amministrative, è opportuno implementare workstation con accesso con privilegi (PAW) dedicate per tutto il personale che dispone di accesso con privilegi ai sistemi informativi dell'organizzazione. Altre informazioni per la distribuzione di PAW sono disponibili nell'articolo [Workstation con accesso con privilegi](privileged-access-workstations.md#paw-phased-implementation).

### <a name="3-just-in-time-privileges"></a>3. Assegnare privilegi Just-In-Time

Per ridurre il tempo di esposizione dei privilegi e aumentarne la visibilità, puoi assegnare privilegi JIT (Just-In-Time) usando una soluzione appropriata, ad esempio quelle indicate di seguito o altre soluzioni di terze parti:

* Per Active Directory Domain Services (AD DS), usare la funzionalità [Privileged Access Manager (PAM)](/microsoft-identity-manager/pam/privileged-identity-management-for-active-directory-domain-services) di Microsoft Identity Manager (MIM).
* Per Azure Active Directory, usare la funzionalità [Azure AD Privileged Identity Management (PIM)](/azure/active-directory/privileged-identity-management/pim-deployment-plan).

### <a name="4-enable-windows-defender-credential-guard"></a>4. Abilitare Windows Defender Credential Guard

L'abilitazione di Credential Guard consente di proteggere gli hash delle password NTLM, i ticket di concessione Ticket Kerberos e le credenziali archiviate dalle applicazioni come credenziali di dominio. Usando questa funzionalità, puoi evitare il furto di credenziali, ad esempio Pass-the-Hash o Pass-The-Ticket, aumentando la difficoltà di accedere all'ambiente con credenziali rubate. Le informazioni sulla modalità di funzionamento di Credential Guard e su come eseguire la distribuzione sono disponibili nell'articolo [Proteggere le credenziali di dominio derivate con Windows Defender Credential Guard](/windows/security/identity-protection/credential-guard/credential-guard).

### <a name="5-leaked-credentials-reporting"></a>5. Segnalazione della perdita di credenziali

"Ogni giorno Microsoft analizza oltre 6500 miliardi di segnali per identificare le minacce emergenti e proteggere i clienti" - [Microsoft By the Numbers](https://news.microsoft.com/bythenumbers/cyber-attacks)

Abilita Microsoft Azure AD Identity Protection per inviare segnalazioni relative alla perdita di credenziali degli utenti, in modo da adottare le opportune misure correttive. Il servizio [Azure AD Identity Protection](/azure/active-directory/identity-protection/index) può essere utilizzato per aiutare le organizzazioni a proteggere dalle minacce gli ambienti cloud e ibridi.

### <a name="6-azure-atp-lateral-movement-paths"></a>6. Percorsi di spostamento laterale di Azure ATP

Verifica che i titolari di account di accesso con privilegi stiano usando una specifica workstation PAW per l'amministrazione, evitando così che un account compromesso senza privilegi possa accedere a un account con privilegi tramite attacchi finalizzati al furto di credenziali, ad esempio Pass-the-Hash o Pass-The-Ticket. [Percorsi di spostamento laterale di Azure ATP](/azure-advanced-threat-protection/use-case-lateral-movement-path) offre semplici funzionalità per la generazione di report che consentono di identificare i casi in cui gli account con privilegi possono essere soggetti ad attacchi.

## <a name="phase-3-security-improvement-and-sustainment"></a>Fase 3: Miglioramento e manutenzione delle soluzioni per la sicurezza

La fase 3 del piano di azione si basa sui passaggi eseguiti nelle fasi 1 e 2 per rafforzare il comportamento relativo alla sicurezza. La fase 3 è illustrata graficamente in questo diagramma:

![Fase 3: 1. Rivedere il controllo degli accessi in base al ruolo, 2. Ridurre le superfici di attacco, 3. Integrare i log con SIEM, 4. Automazione della gestione della perdita di credenziali](../media/securing-privileged-access/PAW_LP_Fig8.JPG)

Queste funzionalità sono basate sulle soluzioni di mitigazione delle fasi precedenti e consentono di adottare un comportamento di difesa più proattivo. Questa fase non prevede una sequenza temporale specifica e la sua implementazione può richiedere più tempo a seconda delle caratteristiche dell'organizzazione.

### <a name="1-review-role-based-access-control"></a>1. Rivedere il controllo degli accessi in base al ruolo

Usando il modello a tre livelli descritto nell'articolo [Modello a livelli dell'amministrazione di Active Directory](securing-privileged-access-reference-material.md), verificare che gli amministratori di livello inferiore non dispongano di diritti di accesso amministrativo a risorse di livello superiore (appartenenze a gruppi, ACL per account utente e così via).

### <a name="2-reduce-attack-surfaces"></a>2. Ridurre le superfici di attacco

Rafforza la protezione dei carichi di lavoro delle identità, inclusi i domini, i controller di dominio, ADFS e Azure AD Connect, poiché la compromissione di uno di questi sistemi può ripercuotersi su altri sistemi dell'organizzazione. Gli articoli [Riduzione della superficie di attacco di Active Directory](../ad-ds/plan/security-best-practices/reducing-the-active-directory-attack-surface.md) e [Cinque passaggi per proteggere l'infrastruttura di identità](/azure/security/azure-ad-secure-steps) forniscono indicazioni per la protezione degli ambienti di identità locali e ibride.

### <a name="3-integrate-logs-with-siem"></a>3. Integrare i log con SIEM

L'integrazione dell'accesso in uno strumento SIEM centralizzato può consentire all'organizzazione di analizzare, rilevare e contrastare gli eventi associati alla sicurezza. Negli articoli [Monitoraggio di Active Directory per l'individuazione di segnali di compromissione](../ad-ds/plan/security-best-practices/monitoring-active-directory-for-signs-of-compromise.md) e [Appendice L: Eventi da monitorare](../ad-ds/plan/appendix-l--events-to-monitor.md) puoi trovare indicazioni sugli eventi che devono essere monitorati nel tuo ambiente.

Questa è un'attività che esula dal piano di azione illustrato perché l'aggregazione, la creazione e l'ottimizzazione degli avvisi in una soluzione SIEM (Security Information and Event Management) richiede l'intervento di analisti qualificati, a differenza dell'uso di Azure ATP nel piano di 30 giorni, che include una funzionalità predefinita per gli avvisi.

### <a name="4-leaked-credentials---force-password-reset"></a>4. Perdita di credenziali: forzare la reimpostazione della password

Continua a migliorare il comportamento relativo alla sicurezza della tua organizzazione abilitando Azure AD Identity Protection in modo da forzare automaticamente la reimpostazione della password quando esiste il sospetto che sia compromessa. L'articolo [Usare i rilevamenti di eventi di rischio per attivare Multi-Factor Authentication e modifiche delle password](/azure/active-directory/authentication/tutorial-risk-based-sspr-mfa) contiene indicazioni su come abilitare l'autenticazione a più fattori usando criteri di accesso condizionale.

## <a name="am-i-done"></a>Finito?

La risposta è no.

I cattivi non si fermano mai, quindi nemmeno tu puoi farlo. Questo piano di azione può aiutare la tua organizzazione a proteggersi dalle minacce attualmente note, ma nel frattempo le modalità di attacco sono in continua evoluzione. Ti consigliamo pertanto di considerare la sicurezza come un processo continuo incentrato sull'aumento dei costi e sulla riduzione dei successi degli utenti malintenzionati che prendono di mira il tuo ambiente.

Anche se questa non è l'unica parte del programma di sicurezza della tua organizzazione, la protezione dell'accesso con privilegi è un componente fondamentale della tua strategia di sicurezza.
