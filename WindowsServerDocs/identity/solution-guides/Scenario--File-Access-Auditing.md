---
ms.assetid: 7be1f2cb-02d5-4209-ba79-edf496a88f47
title: Scenario di File di controllo dell'accesso
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 93d78bbefce38173198f991543fb3a06d145b373
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2017
---
# <a name="scenario-file-access-auditing"></a>Scenario: File di controllo dell'accesso

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

I controlli di sicurezza sono uno degli strumenti più potenti per garantire la protezione di un'azienda. One of the key goals of security audits is regulatory compliance. Standard di settore come Sarbanes Oxley, Health Insurance Portability and Accountability Act (HIPAA) e del settore PCI (Payment Card) sono tenute a seguire una serie di regole relative alla privacy e sicurezza dei dati precise. Controlli di sicurezza consentono stabilire la presenza di tali criteri e confermare la conformità con questi standard. Inoltre, i controlli di sicurezza rilevare comportamenti anomali, identificare e ridurre eventuali lacune nei criteri di sicurezza e scoraggiare confronti di comportamenti irresponsabili tramite la creazione di una traccia delle attività dell'utente che può essere utilizzato per l'analisi forense.  
  
Requisiti dei criteri di controllo sono in genere applicati ai livelli seguenti:  
  
-   **Protezione delle informazioni.** Gli itinerari di controllo di accesso file vengono spesso usati per il rilevamento delle intrusioni e di analisi forense. La possibilità di generare eventi mirati relativi all'accesso alle informazioni di alto valore supponiamo le organizzazioni possono migliorano la precisione di tempo e analisi di risposta.  
  
-   **Criteri dell'organizzazione.** Ad esempio, le organizzazioni regolamentate tramite standard PCI potrebbero avere un criterio centrale per monitorare l'accesso a tutti i file che sono contrassegnati come contenente informazioni della carta di credito e informazioni personali (PII).  
  
-   **Criteri del reparto.** Ad esempio, il reparto finanziario richieda la possibilità di modifica di documenti delicati (ad esempio, un report trimestrale) essere limitato al reparto finanziario, che quindi decidere di monitorare tutti gli altri tentativi di modifica di questi documenti.  
  
-   **Criteri aziendali.** I proprietari aziendali, ad esempio, potrebbero voler monitorare tutti i tentativi non autorizzati di visualizzazione di dati riguardanti i loro progetti.  
  
Inoltre, un reparto conformità potrebbe decidere di monitorare tutte le modifiche ai criteri di autorizzazione centrale e costrutti di criteri, ad esempio utenti, computer e gli attributi delle risorse.  
  
Una delle considerazioni più importanti dei controlli di sicurezza è il costo della raccolta, l'archiviazione e l'analisi degli eventi di controllo. Se i criteri di controllo sono troppo generosi, aumenta il volume di eventi di controllo raccolti e i costi lievitano. Se i criteri di controllo sono troppo limitati, si rischia di trascurare eventi importanti.  
  
Con Windows Server 2012, è possibile creare criteri di controllo tramite attestazioni e proprietà delle risorse. Si presentano pertanto dei criteri di controllo più completo, più mirato e più semplice da gestire. Consente l'orizzonte a scenari finora inimmaginabili o difficilmente realizzabili. Di seguito è riportati esempi dei criteri di controllo che gli amministratori possono creare:  
  
-   Controllare tutti coloro che non dispone di un nulla osta sicurezza elevata e tenta di accedere a un documento ad alto impatto aziendale. Ad esempio, Audit | Tutti gli utenti | All-Access | Businessimpact e User.  
  
-   Controllare tutti i fornitori quando tentano di accedere a documenti correlati ai progetti che non funzionano in. Ad esempio, Audit | Tutti gli utenti | All-Access | User e Project Not_AnyOf Resource.Project.  
  
Questi criteri contribuiscono a definire il volume di eventi di controllo e a limitare tali solo i dati più importanti o gli utenti.  
  
Dopo che gli amministratori hanno creato e applicato i criteri di controllo, il successivo corrispettivo è gleaning informazioni significative dagli eventi di controllo raccolti. Eventi di controllo basati su espressioni consentono di ridurre il volume dei controlli. Tuttavia, gli utenti devono poter eseguire ricerche in questi eventi per trovare informazioni significative e porre domande, ad esempio, "chi sta accedendo ai dati HBI?" O "Era presente un tentativo non autorizzato di accedere a dati sensibili?"  
  
 Windows Server 2012 migliora eventi di accesso dati esistenti con le attestazioni utente, computer e risorse. Questi eventi vengono generati in base al server. Per offrire una visualizzazione completa degli eventi all'interno dell'organizzazione, Microsoft sta lavorando con i partner per fornire la raccolta di eventi e gli strumenti di analisi, ad esempio Audit Collection Services in System Center Operation Manager.  
  
Figura 4 mostra una panoramica di un criterio di controllo centrale.  
  
![Guide alle soluzioni](media/Scenario--File-Access-Auditing/DynamicAccessControl_RevGuide_4.JPG)  
  
**Figura 4** esperienze di controllo centrale  
  
Impostare e utilizzare i controlli di sicurezza in genere prevede i passaggi generali seguenti:  
  
1.  Identificare il corretto insieme di dati e utenti da monitorare  
  
2.  Creare e applicare criteri di controllo appropriati  
  
3.  Raccogliere e analizzare gli eventi di controllo  
  
4.  Gestire e monitorare i criteri che sono stati creati  
  
## <a name="in-this-scenario"></a>In questo scenario  
Gli argomenti seguenti forniscono informazioni aggiuntive per questo scenario:  
  
-   [Pianificare per File di controllo dell'accesso](Plan-for-File-Access-Auditing.md)  
  
-   [Deploy Security Auditing with Central Audit Policies &#40;Demonstration Steps&#41;](Deploy-Security-Auditing-with-Central-Audit-Policies--Demonstration-Steps-.md)  
  
## <a name="BKMK_NEW"></a>Ruoli e funzionalità incluse in questo scenario  
Nella tabella seguente elenca i ruoli e funzionalità che fanno parte di questo scenario e descrive la modalità di supporto.  
  
|Ruolo o funzionalità|Modalità di supporto in questo scenario|  
|-----------------|---------------------------------|  
|Ruolo di Active Directory servizi di dominio|Dominio di Active Directory in Windows Server 2012 offre una piattaforma di autorizzazione basata sulle attestazioni che consente di creare attestazioni utente e richieste diritti da dispositivo, identità composte, (attestazioni utente più dispositivo), nuovo modello di accesso centrale criteri e l'utilizzo delle informazioni di classificazione file nelle decisioni di autorizzazione.|  
|Ruolo Servizi file e archiviazione|File server in Windows Server 2012 forniscono un'interfaccia utente in cui gli amministratori possono visualizzare le autorizzazioni valide per gli utenti per un file o cartella e risolvere i problemi di accesso e concedere l'accesso in base alle esigenze.|  
  


