---
ms.assetid: 7f285c9f-c3e8-4aae-9ff4-a9123815114e
title: Criteri di accesso centrale dello scenario
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: a22592e5c8af9fa23725de90a14a9a8a46c286d7
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80861154"
---
# <a name="scenario-central-access-policy"></a>Scenario: Criteri di accesso centrale

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

I criteri di accesso centrale per i file consentono alle organizzazioni di distribuire e gestire, da una posizione centrale, criteri di autorizzazione che includono espressioni condizionali che usano gruppi di utenti, attestazioni utente, richieste diritti da dispositivo e proprietà delle risorse. Le attestazioni sono asserzioni relative agli attributi dell'oggetto a cui sono associate. Ad esempio, per accedere ai dati ad alto impatto aziendale, un utente deve essere un dipendente a tempo pieno, ottenere l'accesso da un dispositivo gestito e accedere con una smart card. Questi criteri sono definiti e ospitati in Servizi di dominio Active Directory.  
  
I criteri di accesso aziendale sono determinati da requisiti normativi aziendali e di conformità. Se ad esempio un'organizzazione ha come requisito aziendale la limitazione dell'accesso alle informazioni personali contenute nei file al solo proprietario dei file e ai soli dipendenti del reparto risorse umane autorizzati a visualizzare informazioni personali, questi criteri si applicano a tutti i file di informazioni personali, indipendentemente dalla relativa posizione nei vari file server dell'organizzazione. In questo esempio è necessario poter:  
  
-   Identificare e contrassegnare i file che contengono informazioni personali.  
  
-   Identificare il gruppo di utenti del reparto risorse umane ai quali è consentita la visualizzazione di informazioni personali.  
  
-   Creare criteri di accesso centrale applicabili a tutti i file che contengono informazioni personali, ovunque si trovino nei file server dell'organizzazione.  
  
L'iniziativa per la distribuzione e l'applicazione dei criteri di autorizzazione può essere originata da diversi motivi ed essere applicabile a più livelli dell'organizzazione. Di seguito sono elencati alcuni tipi di criteri di esempio:  
  
-   **Criteri di autorizzazione a livello di organizzazione.** Applicati in genere su iniziativa del reparto responsabile della sicurezza delle informazioni, questi criteri di autorizzazione sono finalizzati al rispetto di requisiti di conformità generale o di alto livello dell'organizzazione e sono rilevanti per l'intera organizzazione. Ad esempio, i file ad alto impatto aziendale sono accessibili solo ai dipendenti a tempo pieno.  
  
-   **Criteri di autorizzazione relativi al reparto.** Ogni reparto di un'organizzazione definisce requisiti speciali da implementare per la gestione dei dati. Ad esempio, è possibile che il reparto finanziario voglia permettere l'accesso ai server corrispondenti ai soli dipendenti del reparto.  
  
-   **Criteri specifici per la gestione dei dati.** Questi criteri sono in genere correlati al rispetto dei requisiti di conformità generale e aziendali e sono finalizzati alla protezione dell'accesso corretto alle informazioni gestite. Ad esempio, gli istituti finanziari possono implementare la separazione delle informazioni, in modo che gli analisti non possano accedere alle informazioni usate dai broker e viceversa.  
  
-   **Criteri solo quando necessario.** Questo tipo di criteri di autorizzazione viene in genere usato in combinazione con i tipi di criteri precedenti. Ad esempio, i fornitori devono poter accedere ai file e modificare solo quelli relativi al progetto su cui stanno lavorando.  
  
Gli ambienti reali insegnano anche che tutti i criteri di autorizzazione devono avere eccezioni, per consentire alle organizzazioni di rispondere rapidamente nel caso di esigenze aziendali importanti. Ad esempio, dirigenti che non riescono a trovare le proprie smart card, e hanno l'esigenza di accedere rapidamente alle informazioni ad alto impatto aziendale, possono chiamare l'help desk per ottenere un'eccezione temporanea per accedere a tali informazioni.  
  
I criteri di accesso centrale sono una sorta di ombrello di protezione che un'organizzazione predispone per i propri server. Questi criteri migliorano, ma non sostituiscono, i criteri di accesso locale o gli elenchi di controllo di accesso discrezionale (DACL) applicati ai file e alle cartelle. Se, ad esempio, un DACL su un file consente l'accesso a un utente specifico, ma i criteri di accesso centrale applicati al file limitano l'accesso allo stesso utente, quest'ultimo non potrà ottenere l'accesso al file. Se i criteri di accesso centrale consentono l'accesso, ma il DACL non lo consente, l'utente non potrà ottenere l'accesso al file.  
  
Una regola dei criteri di accesso centrale include le parti logiche seguenti:  
  
-   **Applicabilità.** Condizione che definisce i dati a cui sono applicati i criteri, ad esempio Resource.BusinessImpact=High.  
  
-   **Condizioni di accesso.** Elenco di una o più voci controllo di accesso (ACE) che definiscono chi può accedere ai dati, ad esempio Allow | Full Control | User.EmployeeType=FTE.  
  
-   **Eccezioni.** Elenco aggiuntivo di una o più ACE che definiscono un'eccezione per i criteri, ad esempio MemberOf(HBIExceptionGroup).  
  
Le due figure seguenti mostrano il flusso di lavoro dei criteri di accesso centrale e dei criteri di controllo.  
  
![Guide alle soluzioni](media/Scenario--Central-Access-Policy/DynamicAccessControl_RevGuide.JPG)  
  
**Figura 1** Concetti relativi ai criteri di accesso centrale e ai criteri di controllo  
  
![Guide alle soluzioni](media/Scenario--Central-Access-Policy/DynamicAccessControl_RevGuide_2.JPG)  
  
**Figura 2** Flusso di lavoro dei criteri di accesso centrale  
  
I criteri di autorizzazione centrale si combinano con i componenti seguenti:  
  
-   Elenco di regole di accesso definito a livello centrale destinate a tipi specifici di informazioni, ad esempio ad alto impatto aziendale o informazioni personali.  
  
-   Criteri definiti a livello centrale contenenti un elenco di regole.  
  
-   Identificatore dei criteri assegnato a ogni file nei file server, in modo che punti a criteri di accesso centrale specifici da applicare durante l'autorizzazione di accesso.  
  
La figura seguente illustra come è possibile combinare i criteri in elenchi di criteri per controllare l'accesso ai file a livello centrale.  
  
![Guide alle soluzioni](media/Scenario--Central-Access-Policy/DynamicAccessControl_RevGuide3.JPG)  
  
**Figura 3** Combinazione di criteri  
  
## <a name="in-this-scenario"></a>In questo scenario  
Per i criteri di accesso centrale sono disponibili le indicazioni seguenti:  
  
-   [Pianificare una distribuzione di criteri di accesso centrale](assetId:///0311a76d-d66c-4ddb-ade6-af586a2ad82f)  
  
-   [Distribuire una procedura dimostrativa per &#40;i criteri di accesso centrale&#41;](Deploy-a-Central-Access-Policy--Demonstration-Steps-.md)  
  
-   [Controllo dinamico degli accessi: Panoramica dello scenario](Dynamic-Access-Control--Scenario-Overview.md)  
  
## <a name="roles-and-features-included-in-this-scenario"></a><a name="BKMK_NEW"></a>Ruoli e funzionalità inclusi in questo scenario  
Nella tabella seguente sono elencati i ruoli e le funzionalità che interessano questo scenario e sono descritte le modalità di supporto.  
  
|Ruolo/funzionalità|Modalità di supporto dello scenario|  
|-----------------|---------------------------------|  
|Ruolo Servizi di dominio Active Directory|Dominio di Active Directory in Windows Server 2012 offre una piattaforma di autorizzazione basata sulle attestazioni che consente di creare attestazioni utente e richieste diritti da dispositivo, identità composte, (attestazioni utente più dispositivo), nuovi modelli di criteri di connessioni di accesso centrale e l'utilizzo delle informazioni di classificazione dei file nelle decisioni di autorizzazione.|  
|Ruolo del server Servizi file e archiviazione|Servizi file e archiviazione comprende tecnologie per configurare e gestire uno o più file server che forniscono posizioni centrali in rete dove è possibile archiviare file e condividerli con altri utenti. Se gli utenti della rete devono accedere agli stessi file e applicazioni oppure nel caso in cui la centralizzazione del backup e della gestione dei file sia importante nell'organizzazione, è consigliabile configurare uno o più computer come file server aggiungendo il ruolo Servizi file e archiviazione e i servizi ruolo appropriati nei computer.|  
|Computer client Windows|Gli utenti possono accedere a file e cartelle in rete tramite il computer client.|  
  


