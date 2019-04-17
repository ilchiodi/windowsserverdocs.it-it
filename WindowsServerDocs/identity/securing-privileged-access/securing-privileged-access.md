---
title: Protezione dell'accesso con privilegiato
description: Protezione di Windows Server
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: f5dec0c2-06fe-4c91-9bdc-67cc6a3ede60
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/12/2016
ms.openlocfilehash: eb83903204b00ef6c1eb116554ec54bc2211a399
ms.sourcegitcommit: 7b01b54032ec56432116626e08fbd92508c3a7d5
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/09/2018
---
# <a name="securing-privileged-access"></a>Protezione dell'accesso con privilegiato

>Si applica a: Windows Server 2016

Accesso con privilegi di protezione è il primo importante passaggio per la definizione delle garanzie di sicurezza per i beni aziendali in un'organizzazione moderna. La sicurezza della maggior parte dei beni aziendali in un'organizzazione dipende dall'integrità degli account con privilegi che amministrano e gestiscono i sistemi IT. Informatici sono destinati a questi account e altri elementi dell'accesso con privilegi per accedere rapidamente a dati e sistemi usando attacchi contro il furto di credenziali come [Pass-the-Hash e Pass-the-Ticket](https://www.microsoft.com/pth).

Proteggere l'accesso amministrativo contro determinate avversari è necessario adottare un approccio completo e ponderato per isolare questi sistemi da rischi. Questa figura rappresenta le tre fasi raccomandate per la separazione e protezione dell'amministrazione nella Guida di orientamento:

![Diagramma delle tre fasi raccomandate per la separazione e protezione dell'amministrazione nella Guida di orientamento](../media/securing-privileged-access/PAW_LP_Fig1.JPG)

Obiettivi del piano di azione:

-   **piano da 2-4 settimane**: prevenire rapidamente le tecniche di attacco usate più di frequente

-   **piano da 1-3 mesi**: dare visibilità e controllo delle attività amministrativa

-   **piano di 6 + mesi**: continuare a creare difese per una condizione di sicurezza più proattiva

Si consiglia di che seguire questo piano di azione per proteggere l'accesso con privilegiato da antagonisti. È possibile modificare questo piano di azione per soddisfare le funzionalità esistenti e i requisiti specifici dell'organizzazione.

> [!NOTE]
> L'accesso con privilegi di protezione richiede un'ampia gamma di elementi, inclusi componenti tecnici (difese degli host, protezioni account, la gestione delle identità e così via), nonché le modifiche da elaborare, procedure amministrative e conoscenza.

## <a name="why-is-securing-privileged-access-important"></a>Perché è importante protezione dell'accesso con privilegi?
Nella maggior parte delle organizzazioni, la sicurezza della maggior parte dei beni di business dipende dall'integrità degli account con privilegi che amministrano e gestiscono i sistemi IT. Informatici si stanno concentrando sull'accesso con privilegi a sistemi come Active Directory per accedere rapidamente a tutti di un'organizzazione mirata dei dati.

Sicurezza tradizionali approcci basano sull'uso di punti di ingresso e uscita di una rete di organizzazioni come il perimetro di sicurezza primario, ma l'efficacia della sicurezza di rete è diminuita in modo da due tendenze:

-   Le organizzazioni ospitano dati e risorse all'esterno dei limiti di rete tradizionali in PC aziendali mobili, dispositivi quali telefoni cellulari e Tablet, i servizi cloud e i dispositivi BYOD

-   Gli antagonisti hanno dimostrato una capacità sempre maggiore di ottenere l'accesso alle workstation all'interno del limite di rete tramite anti-phishing e altri attacchi di posta elettronica e web.

L'alternativa naturale per il perimetro di sicurezza di rete in un'azienda moderna complessa è i controlli di autenticazione e autorizzazione nel livello di identità dell'organizzazione. Gli account amministrativi con privilegi detengono effettivamente il controllo di questo nuovo "perimetro di sicurezza" quindi è essenziale per proteggere l'accesso con privilegiato:

![Diagramma che mostra il livello di identità di un'organizzazione](../media/securing-privileged-access/PAW_LP_Fig2.JPG)

Un antagonista che ottiene il controllo di un account amministrativo può usare tali privilegi per perseguire il suo a spese dell'organizzazione di destinazione come illustrato di seguito:

![Diagramma che illustra come un antagonista che ottiene il controllo di un account amministrativo può usare tali privilegi per perseguire il suo a spese dell'organizzazione di destinazione](../media/securing-privileged-access/PAW_LP_Fig3.JPG)

Per ulteriori informazioni sui tipi di attacchi che in genere conducono gli utenti malintenzionati al controllo degli account amministrativi, visitare il [sito web di Pass-The-Hash](https://www.microsoft.com/pth) per white paper informativi, video e altro ancora.

Questa figura rappresenta il "canale" separato per l'amministrazione che il piano di azione stabilisce per isolare le attività di accesso con privilegi dalle attività di utente standard ad alto rischio come web esplorazione e l'accesso alla posta elettronica.

![Diagramma che illustra il "canale" separato per l'amministrazione che il piano di azione stabilisce per isolare le attività di accesso con privilegi dalle attività di utente standard ad alto rischio come web esplorazione e l'accesso alla posta elettronica](../media/securing-privileged-access/PAW_LP_Fig4.JPG)

Poiché l'antagonista può ottenere il controllo di accesso con privilegi usando vari metodi, attenuare questo rischio richiede un approccio tecnico olistico e dettagliato come descritto nella Guida di orientamento. Il piano di azione verrà isolare e rafforzare gli elementi nell'ambiente che abilitano l'accesso con privilegiato tramite creando soluzioni di attenuazione in ogni area della colonna di difesa in questa figura:

![Tabella che mostra le colonne di attacco e difesa](../media/securing-privileged-access/PAW_LP_Fig5.JPG)

## <a name="security-privileged-access-roadmap"></a>Guida di accesso con privilegiata di protezione
Il piano è progettato per massimizzare l'uso di tecnologie che potrebbe già essere distribuite, sfruttare le tecnologie di sicurezza principali correnti e future e integrare qualsiasi 3rd strumenti di protezione di terze parti già potrebbe aver distribuito.

Il piano di azione di suggerimenti di Microsoft è suddiviso in 3 fasi:

-   piano da 2-4 settimane: prevenire rapidamente le tecniche di attacco usate più di frequente

-   piano da 1-3 mesi: dare visibilità e controllo delle attività amministrativa

-   piano di 6 + mesi: continuare a creare difese per una condizione di sicurezza più proattiva

Ogni fase del piano è progettata per aumentare i costi e difficoltà a utenti malintenzionati attaccare l'accesso con privilegi per locale e le risorse cloud. Il piano dà priorità alle più efficaci e le implementazioni più rapida in base alle nostre esperienze con questi attacchi e l'implementazione di soluzioni.

> [!NOTE]
> Le sequenze temporali per il piano di azione sono approssimative e si basano sulla nostra esperienza con implementazioni per i clienti. La durata può variare all'interno dell'organizzazione a seconda della complessità dell'ambiente e i processi di gestione di modifica.

### <a name="security-privileged-access-roadmap-stage-1"></a>Protezione dell'accesso Roadmap con privilegi: Fase 1
Fase 1 del piano è incentrata sulla prevenzione rapidamente le tecniche di attacco usate più di frequente di furto di credenziali e gli abusi. Fase 1 è progettata per essere implementata in circa 2-4 settimane ed è illustrata in questo diagramma:

![Figura che mostra che la fase 1 è progettata per essere implementata in circa 2-4 settimane](../media/securing-privileged-access/PAW_LP_Fig6.JPG)

Fase 1 del piano di sicurezza di accesso con privilegi include i componenti:

**1. account amministrativo separato per attività amministrative**

Per separare i rischi provenienti da internet (attacchi di phishing, navigazione web) dai privilegi amministrativi, creare un account dedicato per tutto il personale con privilegi amministrativi. Informazioni aggiuntive su questo sono incluso in istruzioni di PAW pubblicate [qui](http://Aka.ms/CyberPAW).

**2. privileged Access (workstation) la fase 1: Amministratori di Active Directory**

Per separare i rischi provenienti da internet (attacchi di phishing, navigazione web) dai privilegi amministrativi di dominio, creare l'accesso con privilegi dedicato (workstation) per il personale con privilegi amministrativi di Active Directory. Questo è il primo passaggio di un programma PAW e fase 1 della Guida pubblicata [qui](http://Aka.ms/CyberPAW).

**3. le password di amministratore locale univoche per le workstation**

**4. password di amministratore locale univoche per i server**

Per ridurre il rischio di furto di un hash della password account amministratore locale del database SAM locale e l'abuso di un antagonista per attaccare altri computer, utilizzare lo strumento LAPS per configurare le password casuali univoche in ogni workstation e server e registrare tali password in Active Directory. È possibile ottenere la soluzione di Password di amministratore locale per le workstation e server [qui](http://Aka.ms/LAPS).

Sono disponibili indicazioni aggiuntive per la gestione di un ambiente con soluzioni LAP e workstation Paw [qui](http://aka.ms/securitystandards).

### <a name="security-privileged-access-roadmap-stage-2"></a>Protezione dell'accesso Roadmap con privilegi: Fase 2
Fase 2 si basa sulle attenuazioni della fase 1 ed è progettato per essere implementata in circa 1-3 mesi. I passaggi di questa fase sono rappresentati nel diagramma:

![Diagramma che illustra i passaggi della fase 2](../media/securing-privileged-access/PAW_LP_Fig7.JPG)

**1. PAW fasi 2 e 3: tutti gli amministratori e protezione avanzata aggiuntiva**

Per separare i rischi provenienti da internet da tutti gli account amministrativi con privilegi, continuare con il sistema PAW adottato nella fase 1 e implementare le workstation dedicate per tutto il personale con privilegi di accesso. Questa azione è associata alla fase 2 e 3 della Guida pubblicati [qui](http://Aka.ms/CyberPAW).

**2. privilegi a scadenza (amministratori non permanenti)**

Per ridurre il tempo di esposizione dei privilegi e aumentare la visibilità, fornire privilegi just-in time (JIT) usando una soluzione appropriata, ad esempio quelle indicate di seguito:

-   Per Active Directory Domain Services (AD DS), utilizzare Microsoft Identity Manager (MIM) del [Privileged Access Management (PAM)](https://technet.microsoft.com/en-us/library/mt150258.aspx) funzionalità.

-   Per Azure Active Directory, utilizzare [Azure AD Privileged Identity Management (PIM)](http://aka.ms/AzurePIM) funzionalità.

**3. a più fattori per l'elevazione dei privilegi a scadenza**

Per aumentare il livello di garanzia dell'autenticazione degli amministratori, è consigliabile richiedere l'autenticazione a più fattori prima di concedere privilegi.
A questo scopo PAM di MIM e Azure AD PIM usando Azure multi-factor authentication (MFA).

**4. just Enough Admin (JEA) per la manutenzione di controller di dominio**

Per ridurre la quantità di account con privilegi amministrativi di dominio e l'esposizione ai rischi associati, utilizzare la funzionalità di amministrazione JEA (Just Enough) in PowerShell per eseguire operazioni di manutenzione comuni sui controller di dominio. La tecnologia JEA consente a utenti specifici di eseguire specifiche attività amministrative nel server (ad esempio i controller di dominio) senza concedere loro diritti di amministratore. Scaricare queste linee guida da [TechNet](http://aka.ms/JEA).

**5. inferiore superficie di attacco del dominio e controller di dominio**

Per ridurre la capacità degli utenti malintenzionati di assumere il controllo di una foresta, è necessario ridurre i percorsi di che un utente malintenzionato può richiedere a ottenere il controllo del controller di dominio o gli oggetti di controllo del dominio. Seguire le indicazioni per ridurre il rischio [qui](http://aka.ms/HardenAD).

**6. rilevamento degli attacchi**

Per ottenere la visibilità furto di credenziali attive e identità attacchi in modo da poter rispondere rapidamente agli eventi e contenere i danni, distribuire e configurare [Microsoft Advanced Threat Analitica (ATA)](https://www.microsoft.com/ata).

Prima di installare ATA, è necessario assicurarsi di disporre di un processo in grado di gestire importanti problemi di protezione che ATA può rilevare.

-   Per ulteriori informazioni sulla configurazione di un processo di risposta agli eventi imprevisti, vedere [risposta ai problemi di protezione IT](https://aka.ms/irr) e "Rispondere per attività sospette" e "Recover da una violazione" sezioni di [Mitigating Pass-the-Hash e furto di credenziali altri](https://www.microsoft.com/pth), versione 2.

-   Per ulteriori informazioni sull'impegno dei servizi Microsoft al fine di agevolare la preparazione del processo IR per gli eventi e la distribuzione ATA generati da ATA, contattare il rappresentante Microsoft accedendo [questa pagina](https://www.microsoft.com/en-us/microsoftservices/campaigns/cybersecurity-protection.aspx).

-   Accesso [questa pagina](https://www.microsoft.com/en-us/microsoftservices/campaigns/cybersecurity-protection.aspx) per ulteriori informazioni sull'impegno dei servizi Microsoft al fine di agevolare analisi e il ripristino da un evento imprevisto

-   Per implementare ATA, seguire la Guida di distribuzione disponibile [qui](http://aka.ms/ata).

### <a name="security-privileged-access-roadmap-stage-3"></a>Protezione dell'accesso Roadmap con privilegi: Fase 3
Fase 3 del piano si basa sulle attenuazioni delle fasi 1 e 2 per rafforzare l'intervallo e aggiungere misure di prevenzione in spettro. Fase 3 è illustrata visivamente in questo diagramma:

![Diagramma che illustra la fase 3](../media/securing-privileged-access/PAW_LP_Fig8.JPG)

Queste funzionalità verranno basate sulle soluzioni di attenuazione delle fasi precedenti e sposta le difese in una condizione più proattiva.

**1. rinnovare i ruoli e modello di delega**

Per ridurre il rischio di sicurezza, è consigliabile riprogettare tutti gli aspetti dei ruoli e modello di delega per essere conformi alle regole del modello a livelli, gestire ruoli amministrativi del servizio cloud e incorporare l'utilizzabilità di amministratore come principio fondamentale. Il modello deve usare la funzionalità JIT e JEA distribuite nelle fasi precedenti, nonché di tecnologia di automazione delle attività per raggiungere questi obiettivi.

**2. autenticazione Passport per tutti gli amministratori o della smart card**

Per aumentare il livello di garanzia e la facilità di utilizzo dell'autenticazione degli amministratori, è consigliabile richiedere l'autenticazione avanzata per tutti gli account amministrativi ospitati in Azure Active Directory e in Windows Server Active Directory (inclusi gli account federati ad un servizio cloud).

**3. foresta amministrativa per gli amministratori di Active Directory**

Per fornire la massima protezione per gli amministratori di Active Directory, impostare un ambiente che dispone di alcuna dipendenza di sicurezza in Active Directory di produzione ed è isolata dalle, ma gli attacchi da tutti i sistemi più attendibili nell'ambiente di produzione. Per ulteriori informazioni sull'architettura ESAE visitare [questa pagina](http://aka.ms/esae).

**4. criteri di integrità per i controller di dominio (Server 2016)**

Per limitare il rischio di programmi non autorizzati nei controller di dominio da operazioni di attacco antagoniste ed errori amministrativi accidentali, configurare l'integrità del codice di Windows Server 2016 per kernel (driver) e la modalità utente (applicazioni) per consentire solo file eseguibili autorizzati per l'esecuzione nel computer.

**5. le macchine virtuali schermate per controller di dominio virtuali (Server 2016 Hyper-V Fabric)**

Per proteggere i controller di dominio virtualizzati da vettori di attacco che sfruttano la perdita di sicurezza fisica di una macchina virtuale, utilizzare questa nuova funzionalità Server 2016 Hyper-V per impedire il furto di informazioni riservate di Active Directory dal controller di dominio virtuali. Con questa soluzione, è possibile crittografare macchine virtuali di generazione 2 per proteggere i dati da ispezione, furti e manomissione da parte degli amministratori di rete e archiviazione, nonché protezione avanzata l'accesso alla macchina virtuale da attacchi di amministratori di host Hyper-V.

## <a name="am-i-done"></a>Finito?
Il completamento di questo piano di azione risulterà in protezioni accesso con privilegi per gli attacchi che sono attualmente noti e disponibili agli utenti malintenzionati oggi. Sfortunatamente, i rischi di protezione verranno costantemente evoluzione e spostare, pertanto si consiglia di che considerare la sicurezza come un processo continuo incentrato sull'aumento dei costi e la riduzione dei successi degli antagonisti che puntano l'ambiente.

La protezione con privilegi di accesso è il primo importante passaggio per la definizione delle garanzie di sicurezza per i beni aziendali in un'organizzazione moderna, ma non è l'unico elemento di un programma di sicurezza completo che include invece elementi come criteri, operazioni, protezione delle informazioni, server, applicazioni, PC, dispositivi, infrastruttura cloud e altri componenti forniscono le garanzie di sicurezza che è necessario.

Per ulteriori informazioni sulla creazione di un piano di sicurezza completa, vedere la sezione "responsabilità del cliente e Guida alla consultazione" di Microsoft Cloud Security for documento Enterprise Architects disponibile [qui](http://aka.ms/securecustomer).

Per ulteriori informazioni sull'impegno dei servizi Microsoft per assistenza con uno di questi argomenti, contattare il rappresentante Microsoft o visitare [questa pagina](https://www.microsoft.com/en-us/microsoftservices/campaigns/cybersecurity-protection.aspx).

### <a name="related-topics"></a>Argomenti correlati
[Taste di Premier: come prevenire Pass-the-Hash e altre forme di furto di credenziali](https://channel9.msdn.com/Blogs/Taste-of-Premier/Taste-of-Premier-How-to-Mitigate-Pass-the-Hash-and-Other-Forms-of-Credential-Theft)

[Microsoft Advanced Threat Analitica](http://aka.ms/ata)

[Proteggere le credenziali di dominio derivate con Credential Guard](https://technet.microsoft.com/en-us/library/mt483740%28v=vs.85%29.aspx)

[Panoramica di Device Guard](https://technet.microsoft.com/en-us/library/dn986865(v=vs.85).aspx)

[La protezione delle risorse di valore elevato con secure admin workstation](https://msdn.microsoft.com/en-us/library/mt186538.aspx)

[Modalità utente isolato in Windows 10 con Dave Probert (Channel 9)](https://channel9.msdn.com/Blogs/Seth-Juarez/Isolated-User-Mode-in-Windows-10-with-Dave-Probert)

[Isolamento di processi in modalità utente e delle funzionalità in Windows 10 con Logan Gabriel (Channel 9)](http://channel9.msdn.com/Blogs/Seth-Juarez/Isolated-User-Mode-Processes-and-Features-in-Windows-10-with-Logan-Gabriel)

[Nei processi e funzionalità in modalità utente isolato di Windows 10 con Dave Probert (Channel 9)](https://channel9.msdn.com/Blogs/Seth-Juarez/More-on-Processes-and-Features-in-Windows-10-Isolated-User-Mode-with-Dave-Probert)

[Prevenzione dei furti di credenziali utilizzando il Windows 10 modalità utente isolato (Channel 9)](https://channel9.msdn.com/Blogs/Seth-Juarez/Mitigating-Credential-Theft-using-the-Windows-10-Isolated-User-Mode)

[Consentendo la convalida KDC ristretta in Kerberos di Windows](https://www.microsoft.com/en-us/download/details.aspx?id=6382)

[Novità dell'autenticazione Kerberos per Windows Server 2012](https://technet.microsoft.com/library/hh831747.aspx)

[Verifica del meccanismo di autenticazione di dominio Active Directory nella Guida dettagliata di Windows Server 2008 R2](https://technet.microsoft.com/library/dd378897(v=ws.10).aspx)

[Trusted Platform Module](https://docs.microsoft.com/en-us/windows/device-security/tpm/trusted-platform-module-overview)
