---
ms.assetid: 9ee8a6cb-7550-46e2-9c11-78d0545c3a97
title: Panoramica del controllo dinamico degli accessi
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: 2374e2c8a1efb204dbae1ee633bc5ee41d049d57
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80861174"
---
# <a name="dynamic-access-control-overview"></a>Panoramica del controllo dinamico degli accessi

>Si applica a: Windows Server 2012 R2, Windows Server 2012

Questa panoramica, dedicata ai professionisti IT, descrive il controllo dinamico degli accessi e gli elementi associati, introdotti in Windows Server 2012 e Windows 8.  
  
Il controllo dinamico degli accessi basato sul dominio consente agli amministratori di applicare autorizzazioni e limitazioni al controllo degli accessi, seguendo regole ben definite che possono includere sensibilità delle risorse, mansione o ruolo dell'utente e configurazione del dispositivo utilizzato per accedere alle risorse.  
  
È possibile, ad esempio, concedere a un utente autorizzazioni diverse a seconda che acceda a una risorsa dal computer dell'ufficio oppure da un computer portatile tramite una rete privata virtuale. È anche possibile consentire l'accesso solo se il dispositivo usato è conforme ai requisiti di sicurezza definiti dagli amministratori della rete. Quando si usa il controllo dinamico degli accessi, le autorizzazioni di un utente cambiano in modo dinamico senza alcun intervento da parte dell'amministratore se il processo o il ruolo dell'utente cambia (con conseguente modifica degli attributi dell'account dell'utente in servizi di dominio Active Directory).  
  
Il controllo dinamico degli accessi non è supportato nei sistemi operativi Windows precedenti a Windows Server 2012 e Windows 8. Se il controllo dinamico degli accessi è configurato all'interno di ambienti che comprendono sia versioni supportate che non supportate di Windows, le modifiche verranno implementate solo per le versioni supportate.  
  
Le funzionalità e i concetti associati al controllo dinamico degli accessi includono:  
  
-   [Regole di accesso centrale](#BKMK_Rules)  
  
-   [Criteri di accesso centrale](#BKMK_Policies)  
  
-   [Sostiene](#BKMK_Claims)  
  
-   [Espressioni](#BKMK_Expressions2)  
  
-   [Autorizzazioni proposte](#BKMK_Permissions2)  
  
### <a name="central-access-rules"></a><a name="BKMK_Rules"></a>Regole di accesso centrale  
Una regola di accesso centrale è un'espressione delle regole di autorizzazione, che possono includere una o più condizioni riguardanti gruppi di utenti, attestazioni di utenti e dispositivi e proprietà delle risorse. Combinando più regole di accesso centrale è possibile definire un criterio di accesso centrale.  
  
Se per un dominio sono state definite una o più regole di accesso centrale, gli amministratori di condivisioni file possono associare regole specifiche a risorse e requisiti aziendali specifici.  
  
### <a name="central-access-policies"></a><a name="BKMK_Policies"></a>Criteri di accesso centrale  
I criteri di accesso centrale sono criteri di autorizzazione che includono espressioni condizionali. Supponiamo, ad esempio, che un'organizzazione abbia un requisito aziendale per limitare l'accesso alle informazioni personali nei file solo al proprietario del file e ai membri del reparto risorse umane che possono visualizzare le informazioni personali. Questo costituisce un criterio a livello dell'organizzazione valido per i file contenenti informazioni personali, ovunque si trovino i file server dell'organizzazione che contengono tali file. Per applicare questo criterio, un'organizzazione deve essere in grado di:  
  
-   Identificare e contrassegnare i file che contengono le informazioni personali.  
  
-   Identificare il gruppo di utenti del reparto risorse umane ai quali è consentita la visualizzazione delle informazioni personali.  
  
-   Aggiungere i criteri di accesso centrale a una regola di accesso centrale e applicare quest'ultima a tutti i file che contengono informazioni personali, qualunque sia la posizione di questi file nei vari file server all'interno dell'organizzazione.  
  
I criteri di accesso centrale sono una sorta di ombrello di protezione che un'organizzazione predispone per i propri server. Questi criteri non intendono sostituire, ma vanno a sommarsi ai criteri di accesso locale o agli elenchi di controllo di accesso discrezionale (DACL) che vengono applicati ai file e alle cartelle.  
  
### <a name="claims"></a><a name="BKMK_Claims"></a>Sostiene  
Un'attestazione è un'informazione univoca riguardante un utente, un dispositivo o una risorsa che viene pubblicata da un controller di dominio. Il titolo dell'utente, la classificazione del reparto di un file o lo stato di integrità di un computer sono esempi validi di un'attestazione. Per un'entità possono esistere più attestazioni ed è possibile utilizzare qualsiasi combinazione di attestazioni per autorizzare l'accesso alle risorse. Nelle versioni supportate di Windows sono disponibili i tipi di attestazioni seguenti:  
  
-   **Attestazioni utente** Attributi Active Directory associati a un utente specifico.  
  
-   **Attestazioni dispositivo** Attributi Active Directory associati a un oggetto computer specifico.  
  
-   **Attributi risorsa** Proprietà globali delle risorse, destinate all'uso per le decisioni relative alle autorizzazioni e pubblicate in Active Directory.  
  
Le attestazioni consentono agli amministratori di fare affermazioni precise, nell'ambito dell'organizzazione o dell'impresa, per quanto riguarda gli utenti, i dispositivi e le risorse che possono essere incorporati in espressioni, regole e criteri.  
  
### <a name="expressions"></a><a name="BKMK_Expressions2"></a>Espressioni  
Le espressioni condizionali rappresentano un progresso nella gestione del controllo dell'accesso, in quanto consentono di concedere o negare l'accesso alle risorse in presenza di determinate condizioni, ad esempio l'appartenenza a un gruppo, il percorso o lo stato di sicurezza del dispositivo. Le espressioni possono essere gestite mediante la finestra di dialogo Impostazioni di protezione avanzate dell'Editor ACL o mediante l'Editor delle regole di accesso centrale in Centro di amministrazione di Active Directory.  
  
Le espressioni consentono agli amministratori di gestire l'accesso alle risorse sensibili con condizioni flessibili in ambienti aziendali sempre più complessi.  
  
### <a name="proposed-permissions"></a><a name="BKMK_Permissions2"></a>Autorizzazioni proposte  
Le autorizzazioni proposte consentono agli amministratori di analizzare in modo più accurato l'impatto dei potenziali cambiamenti delle impostazioni di controllo dell'accesso senza cambiare realmente le impostazioni.  
  
La possibilità di prevedere di poter effettivamente accedere a una risorsa consente di pianificare e configurare le autorizzazioni per le risorse prima di implementare concretamente tali modifiche.  
  
## <a name="additional-changes"></a>Ulteriori modifiche  
Altri miglioramenti delle versioni di Windows supportate che supportano il controllo dinamico degli accessi:  
  
### <a name="support-in-the-kerberos-authentication-protocol-to-reliably-provide-user-claims-device-claims-and-device-groups"></a>Supporto del protocollo di autenticazione Kerberos per fornire in modo affidabile attestazioni di utenti, dispositivi e gruppi di dispositivi.  
Per impostazione predefinita, i dispositivi che eseguono una delle versioni di Windows supportate sono in grado di elaborare i ticket Kerberos correlati al controllo dinamico degli accessi, che contengono i dati necessari per l'autenticazione composta. I controller di dominio sono in grado di emettere ticket Kerberos e di rispondere ad essi con informazioni correlate all'autenticazione composta. Quando un dominio è configurato in modo da riconoscere il controllo dinamico degli accessi, i dispositivi ricevono le attestazioni dai controller di dominio nella fase di autenticazione iniziale e ricevono i ticket di autenticazione composta durante l'invio delle richieste di ticket di servizio. L'autenticazione composta genera un token di accesso, che include l'identità dell'utente e il dispositivo, nelle risorse che riconoscono il controllo dinamico degli accessi.  
  
### <a name="support-for-using-the-key-distribution-center-kdc-group-policy-setting-to-enable-dynamic-access-control-for-a-domain"></a>Supporto dell'impostazione Criterio di gruppo del Centro distribuzione chiavi (KDC) per consentire il controllo dinamico degli accessi per un dominio.  
Ogni controller di dominio deve avere la stessa impostazione dei criteri Modelli amministrativi corrispondente a **Configurazione computer\Criteri\Modelli amministrativi\Sistema\KDC\Supporta controllo dinamico degli accessi e blindatura Kerberos**.  
  
### <a name="support-for-using-the-key-distribution-center-kdc-group-policy-setting-to-enable-dynamic-access-control-for-a-domain"></a>Supporto dell'impostazione Criterio di gruppo del Centro distribuzione chiavi (KDC) per consentire il controllo dinamico degli accessi per un dominio.  
Ogni controller di dominio deve avere la stessa impostazione dei criteri Modelli amministrativi corrispondente a **Configurazione computer\Criteri\Modelli amministrativi\Sistema\KDC\Supporta controllo dinamico degli accessi e blindatura Kerberos**.  
  
### <a name="support-in-active-directory-to-store-user-and-device-claims-resource-properties-and-central-access-policy-objects"></a>Supporto di Active Directory per l'archiviazione di attestazioni utente e dispositivo, proprietà risorse e oggetti criteri di accesso centrale.  
  
### <a name="support-for-using-group-policy-to-deploy-central-access-policy-objects"></a>Supporto dell'uso dei criteri di gruppo per la distribuzione degli oggetti criteri di accesso centrale.  
L'impostazione Criteri di gruppo **Configurazione computer\Criteri\Impostazioni di Windows\Impostazioni di sicurezza\File System\Criteri di accesso centrale** consente di distribuire gli oggetti criteri di accesso centrale ai file server dell'organizzazione.  
  
### <a name="support-for-claims-based-file-authorization-and-auditing-for-file-systems-by-using-group-policy-and-global-object-access-auditing"></a>Supporto di autorizzazione e controllo dei file basato sulle attestazioni per i file system tramite Criteri di gruppo e Controllo di accesso agli oggetti globale  
È necessario abilitare il controllo dei criteri di accesso centrale con installazione di appoggio allo scopo di verificare l'effettivo accesso dei criteri di accesso centrale utilizzando le autorizzazioni proposte. È possibile configurare questa impostazione per il computer in **Configurazione avanzata dei criteri di controllo** nelle **Impostazioni di protezione** di un oggetto Criteri di gruppo. Dopo aver configurato l'impostazione di protezione nell'oggetto Criteri di gruppo, è possibile distribuire l'oggetto Criteri di gruppo nei computer della rete.  
  
### <a name="support-for-transforming-or-filtering-claim-policy-objects-that-traverse-active-directory-forest-trusts"></a>Supporto per la trasformazione o il filtraggio degli oggetti criteri di attestazione che attraversano i trust tra foreste Active Directory.  
È possibile filtrare o trasformare le attestazioni in entrata o in uscita che attraversano un trust tra foreste. Gli scenari di base per filtrare e trasformare le attestazioni sono tre:  
  
-   **Filtraggio basato su valore** I filtri usati si basano sul valore dell'attestazione. In questo modo la foresta trusted può impedire l'invio di attestazioni con determinati valori alla foresta trusting. I controller di dominio nelle foreste trusting possono usare il filtraggio basato su valore per difendersi dagli attacchi di elevazione dei privilegi, semplicemente filtrando le attestazioni in entrata in base a valori specifici dalla foresta trusted.  
  
-   **Filtraggio basato sul tipo di attestazione**I filtri usati si basano sul tipo dell'attestazione e non sul valore di questa. Il tipo dell'attestazione è identificabile in base al nome dell'attestazione stessa. Il filtraggio basato sul tipo di attestazione viene usato nella foresta trusted e impedisce a Windows di inviare attestazioni che rivelano informazioni alla foresta trusting.  
  
-   **Trasformazione basata sul tipo di attestazione**Manipola un'attestazione prima di inviarla alla destinazione prevista. È possibile utilizzare la trasformazione basata sul tipo di attestazione nella foresta trusted per generalizzare un'attestazione nota che contiene informazioni specifiche. Le trasformazioni possono essere utilizzate per generalizzare il tipo e/o il valore dell'attestazione.  
  
## <a name="software-requirements"></a>Requisiti software  
Dal momento che le attestazioni e l'autenticazione composta per il controllo dinamico degli accessi richiedono le estensioni di autenticazione Kerberos, qualsiasi dominio che supporta il controllo dinamico degli accessi deve necessariamente avere un numero sufficiente di controller che eseguono le versioni di Windows supportate per supportare l'autenticazione dai client Kerberos compatibili con il controllo dinamico degli accessi. Per impostazione predefinita, i dispositivi devono usare controller di dominio in altri siti. Se non sono disponibili tali controller di dominio, l'autenticazione non verrà portata a termine correttamente. Di conseguenza deve verificarsi una delle condizioni seguenti:  
  
-   Ogni dominio che supporta il controllo dinamico degli accessi deve avere un numero sufficiente di controller di dominio che eseguono le versioni di Windows Server supportate per supportare l'autenticazione da tutti i dispositivi che eseguono le versioni di Windows o Windows Server supportate.  
  
-   I dispositivi che eseguono le versioni di Windows supportate e che non proteggono le risorse mediante attestazioni o identità composta dovranno disabilitare il supporto del protocollo Kerberos per il controllo dinamico degli accessi.  
  
Per i domini che supportano le attestazioni utente, è necessario configurare tutti i controller di dominio che eseguono le versioni di Windows Server supportate con l'impostazione appropriata per supportare le attestazioni e l'autenticazione composta e assicurare la blindatura Kerberos. Configurare le impostazioni nei criteri Modelli amministrativi KDC nel modo seguente:  
  
-   **Fornisci sempre attestazioni** Usare questa impostazione se tutti i controller di dominio eseguono le versioni di Windows Server supportate. Il livello di funzionalità del dominio,poi, deve essere impostato su Windows Server 2012 o versione successiva.  
  
-   **Supportati**Quando si usa questa impostazione, è necessario monitorare i controller di dominio per assicurarsi che il numero di controller di dominio che eseguono le versioni di Windows Server supportate sia sufficiente in rapporto al numero di computer client che devono accedere alle risorse protette tramite il controllo dinamico degli accessi.  
  
Se il dominio utente e il dominio di file server si trovano in foreste diverse, tutti i controller di dominio nella radice della foresta del file server devono essere impostati sul livello di funzionalità di Windows Server 2012 o versione successiva.  
  
Se i client non riconoscono il controllo dinamico degli accessi, deve esistere una relazione di trust bidirezionale tra le due foreste.  
  
Se le attestazioni vengono trasformate quando lasciano una foresta, tutti i controller di dominio nella radice della foresta dell'utente devono essere impostati sul livello di funzionalità di Windows Server 2012 o versione successiva.  
  
Un file server che esegue Windows Server 2012 o Windows Server 2012 R2 deve avere un'impostazione dei Criteri di gruppo che stabilisca se è necessario ottenere le attestazioni utente per i token utente che non hanno attestazioni. L'impostazione predefinita è **Automatico**, pertanto l'impostazione dei Criteri di gruppo viene **attivata** se esiste un criterio centrale che contiene le attestazioni utente o le attestazioni dispositivo per tale file server. Se il file server contiene elenchi di controllo di accesso discrezionali che includono attestazioni utente, è necessario rendere **attiva** questa impostazione dei Criteri di gruppo in modo tale che il server richieda le attestazioni per conto degli utenti che forniscono attestazioni quando accedono al server.  
  
## <a name="additional-resource"></a>Risorsa aggiuntiva  
Per informazioni sull'implementazione di soluzioni basate su questa tecnologia, vedere [controllo dinamico degli accessi: Panoramica dello scenario](Dynamic-Access-Control--Scenario-Overview.md).  
  


