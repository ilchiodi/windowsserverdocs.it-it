---
ms.assetid: 73542e1c-53ef-4ddb-89b1-bc563b2bfb49
title: Scenario di crittografia basata sulla classificazione dei documenti di Office
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: fa6de270d7d6acf4bf7c99f9a5f9f9457b80b55d
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80861134"
---
# <a name="scenario-classification-based-encryption-for-office-documents"></a>Scenario: Classification-Based Encryption for Office Documents

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

La protezione delle informazioni riservate consiste innanzitutto nel ridurre i rischi per l'organizzazione. Diverse normative di conformità, come Health Insurance Portability and Accountability Act (HIPAA) e Payment Card Industry Data Security Standard (PCI-DSS), richiedono la crittografia delle informazioni e vi sono inoltre numerosi motivi aziendali per crittografare le informazioni aziendali riservate. La crittografia delle informazioni è tuttavia costosa e potrebbe influire negativamente sulla produttività aziendale. Le organizzazioni tendono pertanto ad adottare priorità e approcci diversi per la crittografia delle informazioni.  
  
## <a name="scenario-description"></a><a name="BKMK_OVER"></a>Descrizione dello scenario  
 Windows Server 2012 offre la possibilità di crittografare automaticamente i file riservati di Microsoft Office, in base alla relativa classificazione. Questo risultato viene ottenuto tramite attività di gestione dei file da cui viene richiamata la protezione di AD RMS (Active Directory Rights Management Server) per i documenti riservati pochi secondi dopo che il file è stato identificato come riservato nel file server. Questa operazione è facilitata dalle attività di gestione dei file continue sul file server.  
  
La crittografia AD RMS fornisce un altro livello di protezione per i file. Anche se una persona con accesso a un file riservato lo inviasse accidentalmente tramite posta elettronica, il file sarebbe comunque inaccessibile perché protetto con crittografia AD RMS. Gli utenti che vogliono accedere al file devono prima di tutto autenticarsi su un server AD RMS per ricevere la chiave di decrittografia. Questo processo è illustrato nella figura seguente.  
  
![Guide alle soluzioni](media/Scenario--Classification-Based-Encryption-for-Office-Documents/DynamicAccessControl_RevGuide_6.JPG)  
  
**Figura 6** Protezione di AD RMS basata sulla classificazione  
  
Il supporto per i formati di file non Microsoft è disponibile tramite fornitori non Microsoft. Dopo che un file viene protetto mediante la crittografia di AD RMS, le funzionalità di gestione dei dati, come la classificazione basata sulla ricerca o sul contenuto, non sono più disponibili per quel file.  
  
## <a name="in-this-scenario"></a>In questo scenario  
Per questo scenario sono disponibili le informazioni seguenti:  
  
-   [Considerazioni sulla pianificazione per la crittografia dei documenti di Office](assetId:///14714ba6-d6a2-45e4-aae5-d3318817e52a)  
  
-   [Distribuire la crittografia dei file &#40;di Office procedura dimostrativa&#41;](Deploy-Encryption-of-Office-Files--Demonstration-Steps-.md)  
  
-   [Controllo dinamico degli accessi: Panoramica dello scenario](Dynamic-Access-Control--Scenario-Overview.md)  
  
## <a name="roles-and-features-included-in-this-scenario"></a><a name="BKMK_NEW"></a>Ruoli e funzionalità inclusi in questo scenario  
Nella tabella seguente sono elencati i ruoli e le funzionalità che interessano questo scenario e sono descritte le modalità di supporto.  
  
|Ruolo/funzionalità|Modalità di supporto dello scenario|  
|-----------------|---------------------------------|  
|Ruolo Servizi di dominio Active Directory|Servizi di dominio Active Directory fornisce un database distribuito che consente di archiviare e gestire informazioni sulle risorse di rete e dati specifici delle applicazioni provenienti da applicazioni abilitate all'uso di directory. In questo scenario, Active Directory in Windows Server 2012 offre una piattaforma di autorizzazione basata sulle attestazioni che consente di creare attestazioni utente e richieste diritti da dispositivo, identità composte (attestazioni utente più dispositivo), un nuovo modello di criteri di accesso centrale e l'utilizzo delle informazioni di classificazione dei file nelle decisioni di autorizzazione.|  
|Ruolo Servizi file e archiviazione<p>Gestione risorse file server|Servizi file e archiviazione comprende tecnologie che consentono di configurare e gestire uno o più file server che forniscono posizioni centrali in rete in cui è possibile archiviare file e condividerli con altri utenti. Se gli utenti della rete devono accedere agli stessi file e applicazioni oppure nel caso in cui la centralizzazione del backup e della gestione dei file sia importante nell'organizzazione, è consigliabile configurare uno o più computer come file server aggiungendo il ruolo Servizi file e archiviazione e i servizi ruolo appropriati nei computer. In questo scenario gli amministratori dei file server possono configurare le attività di gestione dei file che richiamano la protezione di AD RMS per i documenti riservati pochi secondi dopo che il file è stato identificato come riservato sul file server (attività di gestione dei file continue sul file server).|  
|Ruolo Active Directory Rights Management Services (AD RMS)|AD RMS consente agli utenti e agli amministratori di specificare tramite criteri di Information Rights Management (IRM) le autorizzazioni di accesso a documenti, cartelle di lavoro e presentazioni, contribuendo quindi a impedire la stampa, l'inoltro o la copia di informazioni sensibili da parte di persone non autorizzate. Quando l'autorizzazione per un file viene limitata tramite IRM, le restrizioni di accesso e utilizzo vengono applicate indipendentemente dalla posizione in cui si trovano le informazioni, poiché l'autorizzazione è archiviata nel file del documento stesso. In questo scenario, la crittografia AD RMS fornisce un altro livello di protezione per i file. Anche se una persona con accesso a un file riservato lo inviasse accidentalmente tramite posta elettronica, il file sarebbe comunque inaccessibile perché protetto con crittografia AD RMS. Gli utenti che vogliono accedere al file devono prima di tutto autenticarsi su un server AD RMS per ricevere la chiave di decrittografia.|  
  


