---
ms.assetid: 7f285c9f-c3e8-4aae-9ff4-a9123815114e
title: Criteri di accesso centrale dello scenario
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 1ec4165209b726609b1f9b2caeab02fb5072c756
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2017
---
# <a name="scenario-central-access-policy"></a>Scenario: Criteri di accesso centrale

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Criteri di accesso centrale per i file consentono alle organizzazioni di distribuire e gestire criteri di autorizzazione che includono espressioni condizionali che usano gruppi di utenti, attestazioni utente, richieste diritti da dispositivo e proprietà delle risorse a livello centrale. (Le attestazioni sono asserzioni sugli attributi dell'oggetto a cui sono associati). Ad esempio, per accedere ai dati di alto impatto aziendale (HBI), un utente deve essere un dipendente a tempo pieno, ottenga l'accesso da un dispositivo gestito e accedere con una smart card. Questi criteri sono definiti e ospitati in servizi di dominio Active Directory (AD DS).  
  
Criteri di accesso aziendale sono determinati da requisiti normativi aziendali e di conformità. Ad esempio, se un'organizzazione ha come requisito aziendale per limitare l'accesso alle informazioni personali (PII) nei file al solo il proprietario del file e i membri del reparto risorse umane (HR) che sono autorizzati a visualizzare informazioni personali, questo criterio si applica a tutti i file ovunque si trovino nei file server dell'organizzazione. In questo esempio, è necessario essere in grado di:  
  
-   Identificare e contrassegnare i file che contengono informazioni personali.  
  
-   Identificare il gruppo dei membri del reparto risorse Umane autorizzati a visualizzare informazioni personali.  
  
-   Creare un criterio di accesso centrale che si applica a tutti i file che contengono informazioni personali, ovunque si trovino nei file server dell'organizzazione.  
  
L'iniziativa per distribuire e applicare un criterio di autorizzazione può essere originata da diversi motivi e si applicano a più livelli dell'organizzazione. Di seguito sono indicati alcuni tipi di criteri di esempio:  
  
-   **Criteri di autorizzazione a livello di organizzazione.** In genere avviato dall'ufficio sicurezza informazioni, questi criteri di autorizzazione sono finalizzati al conformità o un requisiti aziendali di alto livello ed è pertinente all'interno dell'organizzazione. Ad esempio, i file ad alto impatto aziendale sono accessibili a solo dipendenti a tempo pieno.  
  
-   **Criteri di autorizzazione relativi al reparto.** Ogni reparto di un'organizzazione ha alcuni requisiti speciali di gestione dei dati che si desidera applicare. Ad esempio, potrebbe essere il reparto finanziario limitare l'accesso ai server corrispondenti ai soli dipendenti del reparto.  
  
-   **Criteri di gestione dei dati specifici.** Questo criterio in genere fa riferimento a requisiti aziendali e di conformità e sono finalizzati alla protezione dell'accesso corretto alle informazioni gestite. Ad esempio istituti finanziari potrebbero implementare pareti informazioni in modo che gli analisti non possano accedere alle informazioni e Broker non possono accedere a informazioni di analisi.  
  
-   **Necessità di sapere criteri.** Questo tipo di criteri di autorizzazione viene in genere utilizzato in combinazione con i tipi di criteri precedenti. Ad esempio, i fornitori devono essere in grado di accedere e modificare solo i file relativi al progetto su che cui stanno lavorando.  
  
Gli ambienti reali insegnano anche di che tutti i criteri di autorizzazione devono avere eccezioni, in modo che le organizzazioni possono rispondere rapidamente nel caso esigenze aziendali importanti. Ad esempio, dirigenti che non possono trovare le smart card e devono accedere rapidamente alle informazioni ad alto impatto aziendale possono chiamare il supporto tecnico per ottenere un'eccezione temporanea per accedere a tali informazioni.  
  
Central access policies act as security umbrellas that an organization applies across its servers. Questi criteri migliorano, ma non sostituiscono i criteri di accesso locale o elenchi di controllo di accesso discrezionale (DACL) che vengono applicati ai file e cartelle. Ad esempio, se un DACL su un file consente l'accesso a un utente specifico, ma un criterio centrale che viene applicato al file limita l'accesso allo stesso utente, l'utente non potrà ottenere l'accesso al file. Se il criterio di accesso centrale consente l'accesso, ma il DACL non consente l'accesso, l'utente non potrà ottenere l'accesso al file.  
  
Una regola dei criteri di accesso centrale è le parti logiche seguenti:  
  
-   **Applicabilità.** Una condizione che definisce i dati a cui il criterio si applica, ad esempio businessimpact = High.  
  
-   **Condizioni di accesso.** Un elenco di uno o più voci controllo di accesso (ACE) che definiscono chi può accedere ai dati, ad esempio Allow | Controllo completo | EmployeeType = FTE.  
  
-   **Eccezioni.** Elenco aggiuntivo di uno o più ACE che definiscono un'eccezione per i criteri, ad esempio memberOf (hbiexceptiongroup).  
  
Nelle due figure seguenti mostrano il flusso di lavoro di accesso centrale e criteri di controllo.  
  
![Guide alle soluzioni](media/Scenario--Central-Access-Policy/DynamicAccessControl_RevGuide.JPG)  
  
**Figura 1** concetti di criteri di accesso e controllo centrale  
  
![Guide alle soluzioni](media/Scenario--Central-Access-Policy/DynamicAccessControl_RevGuide_2.JPG)  
  
**Figura 2** flusso di lavoro criterio di accesso centrale  
  
Il criterio di autorizzazione centrale combina i componenti seguenti:  
  
-   Un elenco di regole di accesso definito a livello centrale destinate a tipi specifici di informazioni, ad esempio ad alto impatto aziendale o informazioni personali.  
  
-   Criteri definiti a livello centrale che contiene un elenco di regole.  
  
-   Un identificatore di criteri che è assegnato a ogni file nei file server in modo che punti a un criterio di accesso centrale specifici che deve essere applicato durante l'autorizzazione di accesso.  
  
La figura seguente illustra come è possibile combinare i criteri in elenchi di criteri per controllare a livello centralizzato l'accesso ai file.  
  
![Guide alle soluzioni](media/Scenario--Central-Access-Policy/DynamicAccessControl_RevGuide3.JPG)  
  
**Figura 3** combinazione di criteri  
  
## <a name="in-this-scenario"></a>In questo scenario  
Sono disponibile per i criteri di accesso centrale le indicazioni seguenti:  
  
-   [Pianificare una distribuzione di criteri di accesso centrale](assetId:///0311a76d-d66c-4ddb-ade6-af586a2ad82f)  
  
-   [Deploy a Central Access Policy &#40;Demonstration Steps&#41;](Deploy-a-Central-Access-Policy--Demonstration-Steps-.md)  
  
-   [Controllo dinamico degli accessi: Panoramica dello Scenario](Dynamic-Access-Control--Scenario-Overview.md)  
  
## <a name="BKMK_NEW"></a>Ruoli e funzionalità incluse in questo scenario  
Nella tabella seguente elenca i ruoli e funzionalità che fanno parte di questo scenario e descrive la modalità di supporto.  
  
|Ruolo o funzionalità|Modalità di supporto in questo scenario|  
|-----------------|---------------------------------|  
|Ruolo di Active Directory Domain Services|Dominio di Active Directory in Windows Server 2012 offre una piattaforma di autorizzazione basata sulle attestazioni che consente di creare attestazioni utente e richieste diritti da dispositivo, identità composte, (attestazioni utente più dispositivo), nuovi modelli di criteri di accesso centrale e l'utilizzo delle informazioni di classificazione file nelle decisioni di autorizzazione.|  
|Ruolo file server e Server di servizi di archiviazione|Servizi file e archiviazione offre tecnologie che consentono di configurare e gestire uno o più file server che forniscono posizioni centrali in rete in cui è possibile archiviare i file e condividerli con altri utenti. Se gli utenti della rete devono accedere agli stessi file e applicazioni oppure se la gestione centralizzata di backup e i file è importante per l'organizzazione, è necessario impostare uno o più computer come un file server aggiungendo il ruolo Servizi File e archiviazione e i servizi ruolo appropriati per i computer.|  
|Computer client Windows|Gli utenti possono accedere a file e cartelle in rete tramite il computer client.|  
  


