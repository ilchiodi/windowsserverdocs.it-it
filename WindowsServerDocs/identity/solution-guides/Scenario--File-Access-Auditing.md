---
ms.assetid: 7be1f2cb-02d5-4209-ba79-edf496a88f47
title: Scenario di File di controllo dell'accesso
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: 37ea1cf28e42f74a18cd57b8fa9def5f9d1646be
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80861124"
---
# <a name="scenario-file-access-auditing"></a>Scenario: File Access Auditing

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

I controlli di sicurezza sono tra gli strumenti più potenti per garantire la sicurezza di un'organizzazione. Uno degli obiettivi principali dei controlli di sicurezza è la conformità ai requisiti. In base a standard di settore come Sarbanes Oxley, HIPAA (Health Insurance Portability and Accountability Act) e PCI (Payment Card Industry), le organizzazioni sono tenute a seguire una serie di regole precise in materia di sicurezza dei dati e privacy. I controlli di sicurezza consentono di verificare l'esistenza di tali criteri e confermare la conformità agli standard. I controlli di sicurezza contribuiscono a intercettare comportamenti anomali e a identificare e ridurre eventuali lacune nei criteri di sicurezza, inoltre svolgono un'azione deterrente nei confronti di comportamenti irresponsabili tramite la creazione di una traccia delle attività dell'utente che può essere utilizzata in un'analisi forense.  
  
I requisiti dei criteri di controllo sono in genere applicati ai livelli seguenti:  
  
-   **Sicurezza delle informazioni.** Gli audit trail di accesso ai file vengono usati spesso per l'analisi forense e il rilevamento delle intrusioni. Essendo in grado di generare eventi mirati relativi all'accesso a informazioni di alto valore, le organizzazioni possono migliorare il tempo di risposta e l'accuratezza dell'analisi in modo significativo.  
  
-   **Criteri dell'organizzazione.** Ad esempio, le organizzazioni regolamentate tramite standard PCI possono avere un criterio centrale per monitorare l'accesso a tutti i file contenenti informazioni sulle carte di credito e le informazioni personali.  
  
-   **Criteri del reparto.** Un reparto Finanze potrebbe decidere, ad esempio, di consentire la modifica di determinati documenti, come i rendiconti trimestrali degli utili, soltanto ai membri del reparto stesso e richiedere quindi che vengano monitorati tutti gli altri tentativi di modifica di questi documenti.  
  
-   **Criteri aziendali.** I proprietari di un'azienda potrebbero decidere, ad esempio, di monitorare tutti i tentativi non autorizzati di visualizzazione di dati riguardanti i loro progetti.  
  
Un reparto conformità potrebbe inoltre decidere di monitorare tutte le modifiche dei criteri centrali di autorizzazione e le strutture dei criteri, ad esempio gli attributi di utente, computer e risorse.  
  
Una delle considerazioni più importanti per quanto riguarda i controlli di sicurezza è la valutazione dei costi connessi alla raccolta, all'archiviazione e all'analisi degli eventi di controllo. Se i criteri di controllo sono troppo generosi, il volume di eventi di controllo da raccogliere aumenta e i costi lievitano. Se i criteri di controllo sono troppo limitati, il rischio è quello di trascurare eventi importanti.  
  
Con Windows Server 2012, è possibile creare criteri di controllo utilizzando le attestazioni e le proprietà delle risorse. In tal modo è possibile definire criteri di controllo più mirati e facili da gestire. Si apre così l'orizzonte a scenari finora inimmaginabili o difficilmente realizzabili. Di seguito sono riportati esempi dei criteri di controllo che gli amministratori possono creare:  
  
-   Controllare tutti coloro che non hanno un Nulla Osta Sicurezza di livello elevato e tentano di accedere a un documento ad alto impatto aziendale. Ad esempio, Audit | Everyone | All-Access | Resource.BusinessImpact=HBI AND User.SecurityClearance!=High.  
  
-   Controllare tutti i fornitori quando tentano di accedere a documenti di progetti nei quali non sono coinvolti. Ad esempio, Audit | Everyone | All-Access | User.EmploymentStatus=Vendor AND User.Project Not_AnyOf Resource.Project.  
  
Questi criteri contribuiscono a definire il volume degli eventi di controllo e a limitare tali eventi ai soli dati o utenti significativi.  
  
Dopo avere creato e applicato i criteri di controllo, gli amministratori devono riflettere su come ottenere informazioni significative dagli eventi di controllo raccolti. Gli eventi di controllo basati sulle espressioni sono utili per ridurre il volume dei controlli. Tuttavia, gli utenti hanno bisogno di eseguire query su questi eventi per trovare informazioni significative e porre domande, ad esempio, "chi accede a dati HBI?" o "Era presente un tentativo non autorizzato di accedere ai dati sensibili?"  
  
 Windows Server 2012 migliora eventi di accesso ai dati esistenti con le attestazioni utente, computer e risorse. Questi eventi vengono generati per i singoli server. Per offrire una visualizzazione completa degli eventi di tutta l'organizzazione, Microsoft e i relativi partner sono impegnati nella creazione di strumenti di raccolta e analisi degli eventi, ad esempio i servizi Raccolta controlli in System Center Operation Manager.  
  
Nella figura 4 è illustrata una panoramica di un criterio di controllo centrale.  
  
![Guide alle soluzioni](media/Scenario--File-Access-Auditing/DynamicAccessControl_RevGuide_4.JPG)  
  
**Figura 4** Esperienze di controllo centrale  
  
Per impostare e utilizzare i controlli di sicurezza è in genere necessario:  
  
1.  Identificare il corretto insieme di dati e utenti da monitorare  
  
2.  Creare e applicare i criteri di controllo appropriati  
  
3.  Raccogliere e analizzare gli eventi di controllo  
  
4.  Gestire e monitorare i criteri creati  
  
## <a name="in-this-scenario"></a>In questo scenario  
Negli argomenti seguenti sono disponibili ulteriori informazioni su questo scenario:  
  
-   [Pianificare il controllo dell'accesso ai file](Plan-for-File-Access-Auditing.md)  
  
-   [Distribuire il controllo di sicurezza con i criteri &#40;di controllo centrali procedura dimostrativa&#41;](Deploy-Security-Auditing-with-Central-Audit-Policies--Demonstration-Steps-.md)  
  
## <a name="roles-and-features-included-in-this-scenario"></a><a name="BKMK_NEW"></a>Ruoli e funzionalità inclusi in questo scenario  
Nella tabella seguente sono elencati i ruoli e le funzionalità che interessano questo scenario e sono descritte le modalità di supporto.  
  
|Ruolo/funzionalità|Modalità di supporto dello scenario|  
|-----------------|---------------------------------|  
|Ruolo Servizi di dominio Active Directory|Dominio di Active Directory in Windows Server 2012 offre una piattaforma di autorizzazione basata sulle attestazioni che consente la creazione di attestazioni utente e richieste diritti da dispositivo, identità composte, (attestazioni utente più dispositivo), nuovo modello di accesso centrale criteri e l'utilizzo delle informazioni di classificazione file nelle decisioni di autorizzazione.|  
|Ruolo Servizi file e archiviazione|File server in Windows Server 2012 forniscono un'interfaccia utente in cui gli amministratori possono visualizzare le autorizzazioni valide per gli utenti per un file o cartella e risolvere i problemi di accesso e concedere l'accesso in base alle esigenze.|  
  


