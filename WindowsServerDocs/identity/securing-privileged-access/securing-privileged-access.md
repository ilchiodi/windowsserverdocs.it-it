---
title: Protezione dell'accesso con privilegi
description: Approccio per fasi per la protezione dell'accesso con privilegi
ms.prod: windows-server-threshold
ms.topic: conceptual
ms.assetid: f5dec0c2-06fe-4c91-9bdc-67cc6a3ede60
ms.date: 02/25/2019
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: mas
ms.openlocfilehash: 0d54a94d51a4d1e0a1d28f78ec39bf16bc3d9100
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59822012"
---
# <a name="securing-privileged-access"></a>Protezione dell'accesso con privilegi

>Si applica a: Windows Server

La protezione dell'accesso con privilegi è il primo importante passaggio per la definizione delle garanzie di sicurezza per i beni aziendali in un'organizzazione moderna. La sicurezza della maggior parte dei beni aziendali in un'organizzazione IT dipende dall'integrità degli account con privilegi utilizzata per amministrare, gestire e sviluppare. Gli utenti malintenzionati attaccano spesso questi account e altri elementi di accesso con privilegi per ottenere l'accesso ai dati e sistemi usando attacchi con furto di credenziali, ad esempio [Pass-the-Hash e Pass-the-Ticket](https://www.microsoft.com/pth).

La protezione dell'accesso con privilegi da antagonisti è necessario adottare un approccio completo e ponderato per isolare questi sistemi da rischi.

## <a name="what-are-privileged-accounts"></a>Quali sono gli account con privilegi?

Prima di descrivere come proteggerle consente di definire gli account con privilegi.

Account con privilegi, come gli amministratori dei servizi di dominio Active Directory hanno accesso diretto o indiretto alla maggior parte dei beni in un'organizzazione IT, effettua una compromissione degli account di un rischio significativo di business.

## <a name="why-securing-privileged-access-is-important"></a>Il motivo per cui è importante proteggere l'accesso con privilegi?

Stato attivo di attacchi informatici accesso con privilegi ai sistemi, ad esempio Active Directory (AD) per accedere rapidamente a tutti di un'organizzazione di destinazione dei dati. Gli approcci di sicurezza tradizionali si sono concentrati sulle connessioni di rete e i firewall come perimetro di sicurezza primario, ma l'efficacia della sicurezza di rete è diminuita in modo sostanziale di due tendenze:

* Le organizzazioni ospitano dati e le risorse all'esterno dei limiti di rete tradizionale in aziendali per dispositivi mobili, PC, dispositivi come telefoni cellulari e Tablet, servizi cloud e bring your own Device (BYOD)
* Gli antagonisti hanno dimostrato una capacità sempre maggiore di ottenere l'accesso alle workstation all'interno del limite di rete per mezzo del phishing e di altri attacchi alla posta elettronica e al Web.

Questi fattori richiedono la compilazione di un perimetro di sicurezza moderne all'esterno di autenticazione e autorizzazione dei controlli di identità oltre la strategia di perimetro della rete tradizionali. In questo caso un perimetro di sicurezza è definito come un set coerente di controlli tra risorse e le minacce ad essi. Gli account con privilegi sono effettivamente nel controllo di questo nuovo perimetro di sicurezza, pertanto è fondamentale per proteggere l'accesso con privilegi.

![Diagramma che mostra il livello di identità di un'organizzazione](../media/securing-privileged-access/PAW_LP_Fig2.JPG)

Un utente malintenzionato che deve assumere il controllo di un account amministrativo può usare tali privilegi per aumentare l'impatto dell'organizzazione di destinazione, come illustrato di seguito:

![Diagramma che illustra in che modo un antagonista che ottiene il controllo di un account amministrativo può usare tali privilegi per perseguire il suo obiettivo a spese dell'organizzazione di destinazione](../media/securing-privileged-access/PAW_LP_Fig3.JPG)

L'illustrazione seguente mostra due percorsi:

* Un percorso "blue" in cui viene usato un account di utente standard per l'accesso senza privilegi a risorse quali la posta elettronica e l'esplorazione del web e quotidianamente vengono completate.

   > [!NOTE]
   > Gli elementi di percorso blu descritti in un secondo momento indicano ampie funzionalità di protezione dell'ambiente che si estendono oltre gli account amministrativi.

* Un "red" percorso in cui si verifica l'accesso con privilegi in un dispositivo con protezione avanzato per ridurre il rischio di phishing e altri attacchi di posta elettronica e web.

![Diagramma che mostra il "percorso" separato per l'amministrazione che la Guida di orientamento stabilisce per isolare le attività di accesso con privilegi dalle attività di utente standard ad alto rischio, ad esempio navigazione web e accedere alla posta elettronica](../media/securing-privileged-access/PAW_LP_Fig4.JPG)

## <a name="securing-privileged-access-roadmap"></a>Guida di orientamento alla protezione dell'accesso con privilegi

La Guida di orientamento è progettato per massimizzare l'utilizzo delle tecnologie Microsoft che sono già stati distribuiti, Ottieni i vantaggi delle tecnologie cloud per migliorare la sicurezza e integrare eventuali strumenti di sicurezza di terze parti 3rd che eventualmente già distribuiti.

Roadmap di consigli di Microsoft è suddiviso in 3 fasi:

* [Fase 1: Primi 30 giorni]()
   * Wins rapido con un impatto significativo positivo.
* [Fase 2: 90 giorni]()
   * Significativi miglioramenti incrementali.
* [Fase 3: Ongoing]()
   * Miglioramento della protezione e sustainment.

Il piano dà priorità alle implementazioni più efficaci e rapide in base alle nostre esperienze con questi attacchi e l'implementazione di soluzioni. 

Si consiglia di seguire questo piano di azione per proteggere l'accesso con privilegi da antagonisti specifici. È possibile modificare questo piano di azione per soddisfare le funzionalità esistenti e i requisiti specifici dell'organizzazione.

> [!NOTE]
> La protezione dell'accesso con privilegi richiede una vasta gamma di elementi, inclusi componenti tecnici (difese degli host, protezioni account, gestione delle identità e così via), modifiche da elaborare, procedure amministrative e conoscenza. Le sequenze temporali per il piano di azione sono approssimative e si basano sulla nostra esperienza con le implementazioni per i clienti. La durata può variare all'interno dell'organizzazione a seconda della complessità dell'ambiente e dai processi di gestione delle modifiche.

## <a name="phase-1-quick-wins-with-minimal-operational-complexity"></a>Fase 1: Rapido wins con minime complessità operativa

Fase 1 della roadmap è incentrata sull'attenuazione rapidamente le tecniche di attacco usate più di frequente di furto di credenziali e l'abuso. Fase 1 è progettata per essere implementata in circa 30 giorni ed è illustrata nella figura seguente:

![Diagramma di fase 1: 1. Separata account amministratore e utente, 2. Just-in-password di amministratore locale ora, 3. Admin workstation fase 1, 4. Rilevamento di attacchi di identità](../media/securing-privileged-access/PAW_LP_Fig6.JPG)

### <a name="1-separate-accounts"></a>1. Account separati

Per separare i rischi di internet (attacchi di phishing, navigazione web) da con privilegi degli account di accesso, creare un account dedicato per tutto il personale con privilegi di accesso. Gli amministratori devono essere esplorazione non web, controllo della posta elettronica e si eseguono attività quotidiane per la produttività con gli account con privilegi elevati. Altre informazioni sono reperibili nella sezione [separare gli account amministrativi](securing-privileged-access-reference-material.md#separate-administrative-accounts) del documento di riferimento.

Seguire le indicazioni fornite nell'articolo [gestire gli account di accesso di emergenza in Azure AD](/azure/active-directory/users-groups-roles/directory-emergency-access) per creare accesso di emergenza almeno due account, con diritti di amministratore assegnati in modo permanente, in entrambi tra sito locale AD Azure e AD ambienti . Questi account sono utilizzabili solo quando gli account amministratore tradizionale è possibile eseguire un'operazione necessaria ad esempio in un caso di emergenza.

### <a name="2-just-in-time-local-admin-passwords"></a>2. Just-in-password di amministratore locale ora

Per ridurre il rischio di furto di un hash della password account amministratore locale dal database SAM locale e l'abuso, un malintenzionato per attaccare altri computer, le organizzazioni devono assicurarsi che ogni computer ha una password di amministratore locale univoco. Lo strumento di locale amministratore Password soluzione (LAP) è possibile configurare le password casuali univoche in ogni workstation e server archiviarli in Active Directory (AD) protetti da un elenco ACL. Solo gli utenti autorizzati idonei possono leggere o richiedere la reimpostazione di queste password account amministratore locale. È possibile ottenere il LAPS per l'uso nelle workstation e server dal [Microsoft Download Center](http://Aka.ms/LAPS).

Altro materiale sussidiario per il funzionamento di un ambiente con soluzioni LAP e sistemi Paw sono reperibili nella sezione [standard operativi basati sul principio di origine pulita](securing-privileged-access-reference-material.md#operational-standards-based-on-clean-source-principle).

### <a name="3-administrative-workstations"></a>3. Workstation amministrative

Come misura di sicurezza iniziali per gli utenti con Azure Active Directory e privilegi di amministratore di Active Directory locale tradizionale, assicurarsi che utilizzano i dispositivi Windows 10 configurati con la [standard per una sicurezza elevata Windows dispositivo 10](/windows-hardware/design/device-experiences/oem-highly-secure). 

### <a name="4-identity-attack-detection"></a>4. Rilevamento di attacchi di identità

[Azure Advanced Threat Protection (ATP)](/azure-advanced-threat-protection/what-is-atp) è una soluzione di sicurezza basato sul cloud che identifica, rileva e consente di analizzare le minacce avanzate, le identità compromesse e azioni dipendente malintenzionato mirate attive in locale Ambiente di directory.

## <a name="phase-2-significant-incremental-improvements"></a>Fase 2: Significativi miglioramenti incrementali

Fase 2 si basa sul lavoro svolto nella fase 1 ed è progettato per essere completato in circa 90 giorni. I passaggi di questa fase sono rappresentati nel diagramma:

![Diagramma di fase 2: 1. Windows Hello for Business / autenticazione a più fattori, 2. Implementazione di workstation PAW, 3. Just-in-privilegi ora, 4. Credential Guard, 5. Credenziali perse, 6. Rilevamento delle vulnerabilità di spostamento laterale](../media/securing-privileged-access/PAW_LP_Fig7.JPG)

### <a name="1-require-windows-hello-for-business-and-mfa"></a>1. Richiede Windows Hello for Business e autenticazione a più fattori

Gli amministratori possono sfruttare la semplicità d'uso associato con Windows Hello for Business. Gli amministratori possono sostituire le password complesse con l'autenticazione a due fattori avanzata nei PC dei singoli. Un utente malintenzionato deve avere il dispositivo e il info biometrico o PIN, è molto più difficile ottenere l'accesso senza una conoscenza del dipendente. Altre informazioni su Windows Hello for Business e il percorso di rollout sono reperibili nell'articolo [Windows Hello for Business Panoramica](/windows/security/identity-protection/hello-for-business/hello-overview)

Abilitare multi-factor authentication (MFA) per gli account di amministratore in Azure AD tramite Azure MFA. Alla minima Abilita il [criteri di accesso condizionale di protezione della linea di base](/azure/active-directory/conditional-access/baseline-protection#require-mfa-for-admins) ulteriori informazioni su Azure multi-Factor Authentication sono reperibili nell'articolo [distribuire Azure multi-Factor Authentication basato sul cloud](/azure/active-directory/authentication/howto-mfa-getstarted)

### <a name="2-deploy-paw-to-all-privileged-identity-access-account-holders"></a>2. Distribuire workstation PAW a tutti i titolari di account accesso di identità con privilegi

Se si continua il processo di separazione gli account con privilegi da minacce rilevate nel messaggio di posta elettronica, esplorazione del web e altre attività non amministrative, è necessario implementare dedicato Privileged Access workstation per tutto il personale con privilegi di accesso per il sistemi informativi dell'organizzazione. Indicazioni aggiuntive per la distribuzione PAW sono reperibili nell'articolo [workstation con accesso con privilegi](privileged-access-workstations.md#paw-phased-implementation).

### <a name="3-just-in-time-privileges"></a>3. Just-in privilegi di tempo

Per ridurre il tempo di esposizione dei privilegi e aumentare la visibilità sul loro uso, fornire privilegi Just-in-time (JIT) tramite una soluzione appropriata, ad esempio quelle indicate di seguito o altre soluzioni di terze parti:

* Per Active Directory Domain Services (AD DS), usare la funzionalità [Privileged Access Manager (PAM)](/microsoft-identity-manager/pam/privileged-identity-management-for-active-directory-domain-services) di Microsoft Identity Manager (MIM).
* Per Azure Active Directory, usare la funzionalità [Azure AD Privileged Identity Management (PIM)](/azure/active-directory/privileged-identity-management/pim-deployment-plan).

### <a name="4-enable-windows-defender-credential-guard"></a>4. Abilitare Windows Defender Credential Guard

Abilitazione di Credential Guard consente di proteggere gli hash delle password NTLM, Kerberos Ticket Granting Tickets e le credenziali archiviate da applicazioni come credenziali di dominio. Questa funzionalità consente di impedire attacchi di furto delle credenziali, ad esempio Pass-the-Hash o Pass-The-Ticket, aumentando la difficoltà della trasformazione tramite pivot nell'ambiente con credenziali rubate. Informazioni sul funzionamento di Credential Guard e la modalità di distribuzione sono disponibili nell'articolo [Proteggi derivati delle credenziali di dominio con Windows Defender Credential Guard](/windows/security/identity-protection/credential-guard/credential-guard).

### <a name="5-leaked-credentials-reporting"></a>5. Creazione di report sulle credenziali perse

"Ogni giorno, Microsoft analizza i segnali oltre 6.5 trilioni allo scopo di identificare le minacce emergenti e proteggere i clienti" - [Microsoft da numeri](https://news.microsoft.com/bythenumbers/cyber-attacks)

Abilitare Microsoft Azure AD Identity Protection segnalare agli utenti con credenziali perse, in modo che è possibile monitorarle e aggiornarle. [Azure AD Identity Protection](/azure/active-directory/identity-protection/index) può essere sfruttata per consentire alle organizzazioni di proteggere gli ambienti cloud e ibride da minacce esterne.

### <a name="6-azure-atp-lateral-movement-paths"></a>6. Percorsi di spostamento laterale di Azure ATP

Verificare con privilegi di accesso account titolari Usa le workstation PAW per l'amministrazione solo in modo che un account senza privilegi compromessi non è possibile ottenere l'accesso a un account con privilegi tramite attacchi di furto delle credenziali, ad esempio Pass-the-Hash o Pass-The-Ticket. [Azure ATP laterale dei percorsi di spostamento (LMPs)](/azure-advanced-threat-protection/use-case-lateral-movement-path) fornisce un modo facile per informazioni sui report per identificare dove è possibile Apri per compromettere gli account con privilegi.

## <a name="phase-3-security-improvement-and-sustainment"></a>Fase 3: Sustainment e miglioramento della protezione

Fase 3 della roadmap si basa sulle operazioni effettuate in fasi 1 e 2 migliora il tuo comportamento di sicurezza. Fase 3 è illustrata visivamente in questo diagramma:

![Fase 3: 1. Esaminare RBAC, 2. Ridurre le superfici di attacco, 3. Integrare i log con SEIM, 4. Automazione credenziali perse](../media/securing-privileged-access/PAW_LP_Fig8.JPG)

Queste funzionalità verranno compilare sui passaggi delle fasi precedenti e spostare le difese in una condizione più proattiva. Questa fase non è alcuna sequenza temporale specifica e potrebbe richiedere più tempo per implementare base delle singole organizzazioni.

### <a name="1-review-role-based-access-control"></a>1. Esaminare il controllo di accesso basato sui ruoli

Usando il modello a livelli tre descritta nell'articolo [modello di livello amministrativo di Active Directory](securing-privileged-access-reference-material.md)rivedere e verificare che gli amministratori di livello inferiore non hanno accesso amministrativo alle risorse di livello superiore (appartenenza a gruppi, gli ACL nei gli account utente e così via...).

### <a name="2-reduce-attack-surfaces"></a>2. Ridurre le superfici di attacco

Rafforzare la protezione dei carichi di lavoro di identità tra domini, i controller di dominio, ad FS e Azure AD Connect come uno di questi sistemi compromettere può comportare compromissioni di altri sistemi all'interno dell'organizzazione. Gli articoli [riducendo la superficie di attacco Active Directory](../ad-ds/plan/security-best-practices/reducing-the-active-directory-attack-surface.md) e [cinque passaggi per la protezione dell'infrastruttura di identità](/azure/security/azure-ad-secure-steps) forniscono indicazioni per la protezione delle applicazioni in locale e ibrida gli ambienti di identità.

### <a name="3-integrate-logs-with-siem"></a>3. Integrare i log con SIEM

L'integrazione di registrazione in uno strumento SIEM centralizzato può aiutare l'azienda da analizzare, rilevare e rispondere agli eventi di sicurezza. Gli articoli [monitoraggio di Active Directory per i segni di compromissione](../ad-ds/plan/security-best-practices/monitoring-active-directory-for-signs-of-compromise.md) e [appendice l: Eventi da monitorare](../ad-ds/plan/appendix-l--events-to-monitor.md) fornire indicazioni sugli eventi che devono essere monitorati nell'ambiente in uso.

Questo fa parte di oltre il piano perché l'aggregazione, la creazione e l'ottimizzazione degli avvisi in un evento e informazioni gestione della sicurezza (SIEM) richiede gli analisti esperti (a differenza di Azure ATP nel piano di 30 giorni che include dalla casella avvisi)

### <a name="4-leaked-credentials---force-password-reset"></a>4. La reimpostazione della password Force - sulle credenziali perse

Continuare a migliorare le condizioni di sicurezza, consentendo di Azure AD Identity Protection alla forza automaticamente la reimpostazione della password quando le password sono sospetta di compromissione. Le indicazioni disponibili nell'articolo [usare gli eventi di rischio su trigger multi-Factor Authentication e le modifiche delle password](/azure/active-directory/authentication/tutorial-risk-based-sspr-mfa) spiega come abilitarla usando criteri di accesso condizionale.

## <a name="am-i-done"></a>Finito?

La risposta breve è nessuna.

Malintenzionati mai interrompere, pertanto nessuna di esse è possibile. Questa Guida di orientamento possibile proteggere l'organizzazione attualmente note minacce come gli utenti malintenzionati verranno evolvere costantemente and -shift. È consigliabile che considerare la sicurezza come un processo continuo incentrato sull'aumento dei costi e ridurre la frequenza di esito positivo di antagonisti che puntano l'ambiente.

Anche se non è l'unica parte del programma di sicurezza dell'organizzazione la protezione dell'accesso con privilegi è un componente fondamentale della strategia di sicurezza.
