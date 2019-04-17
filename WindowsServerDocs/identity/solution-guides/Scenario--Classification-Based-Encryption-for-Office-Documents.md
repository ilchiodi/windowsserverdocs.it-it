---
ms.assetid: 73542e1c-53ef-4ddb-89b1-bc563b2bfb49
title: Scenario di crittografia basata sulla classificazione dei documenti di Office
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 38e058f36522ba6a2c81694cb883d0946b04adda
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2017
---
# <a name="scenario-classification-based-encryption-for-office-documents"></a>Scenario: La crittografia basata sulla classificazione dei documenti di Office

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Protection of sensitive information is mainly about mitigating risk for the organization. Diverse normative di conformità, come Health Insurance Portability and Accountability Act (HIPAA) e Payment Card Industry Data Security Standard (PCI-DSS), richiedono la crittografia delle informazioni e vi sono inoltre numerosi motivi aziendali per crittografare le informazioni aziendali riservate. However, encrypting information is expensive, and it might impair business productivity. Thus, organizations tend to have different approaches and priorities for encrypting their information.  
  
## <a name="BKMK_OVER"></a>Descrizione dello scenario  
 Windows Server 2012 offre la possibilità di crittografare automaticamente i file riservati di Microsoft Office, in base alla relativa classificazione. Questo risultato viene ottenuto tramite attività di gestione file che richiamano la protezione di Active Directory Rights Management Services (AD RMS) per i documenti riservati pochi secondi dopo che il file è stato identificato come un file riservato nel file server. Questa operazione è facilitata dalle attività di gestione file continue sul file server.  
  
La crittografia AD RMS fornisce un ulteriore livello di protezione per i file. Anche se una persona con accesso a un file riservato lo inviasse accidentalmente tramite posta elettronica, il file viene protetto con crittografia AD RMS. Gli utenti che vogliono accedere al file devono prima autenticarsi su un server AD RMS per ricevere la chiave di decrittografia. La figura seguente illustra questo processo.  
  
![Guide alle soluzioni](media/Scenario--Classification-Based-Encryption-for-Office-Documents/DynamicAccessControl_RevGuide_6.JPG)  
  
**Figura 6** protezione RMS basata sulla classificazione  
  
Supporto per i formati di file non Microsoft è disponibile tramite fornitori non Microsoft. Dopo che un file è stato protetto con crittografia AD RMS, funzionalità di gestione dei dati come la classificazione di ricerca - o in base al contenuto non sono più disponibili per quel file.  
  
## <a name="in-this-scenario"></a>In questo scenario  
Di seguito è disponibile per questo scenario, le linee guida:  
  
-   [Considerazioni sulla pianificazione per la crittografia dei documenti di Office](assetId:///14714ba6-d6a2-45e4-aae5-d3318817e52a)  
  
-   [Deploy Encryption of Office Files &#40;Demonstration Steps&#41;](Deploy-Encryption-of-Office-Files--Demonstration-Steps-.md)  
  
-   [Controllo dinamico degli accessi: Panoramica dello Scenario](Dynamic-Access-Control--Scenario-Overview.md)  
  
## <a name="BKMK_NEW"></a>Ruoli e funzionalità incluse in questo scenario  
Nella tabella seguente elenca i ruoli e funzionalità che fanno parte di questo scenario e descrive la modalità di supporto.  
  
|Ruolo o funzionalità|Modalità di supporto in questo scenario|  
|-----------------|---------------------------------|  
|Ruolo Active Directory Domain Services (AD DS)|Servizi di dominio Active Directory fornisce un database distribuito che archivia e gestisce le informazioni sulle risorse di rete e dati specifici dell'applicazione da applicazioni abilitate alla directory. In questo scenario, servizi di dominio Active Directory in Windows Server 2012 offre una piattaforma di autorizzazione basata sulle attestazioni che consente di creare attestazioni utente e richieste diritti da dispositivo, identità composte (attestazioni utente più dispositivo), un nuovo modello di criteri di accesso centrale e l'utilizzo delle informazioni di classificazione file nelle decisioni di autorizzazione.|  
|Ruolo Servizi file e archiviazione<br /><br />Gestione risorse file Server|Servizi file e archiviazione comprende tecnologie che consentono configurare e gestire uno o più file server che forniscono posizioni centrali in rete in cui è possibile archiviare i file e condividerli con altri utenti. Se gli utenti della rete devono accedere agli stessi file e applicazioni oppure se la gestione centralizzata di backup e i file è importante per l'organizzazione, è necessario impostare uno o più computer come un file server aggiungendo il ruolo Servizi File e archiviazione e i servizi ruolo appropriati per i computer. In questo scenario, gli amministratori dei file server possono configurare attività di gestione file che richiamano la protezione di AD RMS per i documenti riservati pochi secondi dopo che il file è stato identificato come un file riservato nel file server (attività di gestione file continue sul file server).|  
|Ruolo di Active Directory Rights Management Services (AD RMS)|AD RMS consente agli utenti e amministratori (tramite criteri di Information Rights Management (IRM)) per specificare le autorizzazioni di accesso a documenti, cartelle di lavoro e presentazioni. Questo consente di evitare informazioni riservate vengano stampati, inoltrati o copiati da utenti non autorizzati. Dopo l'autorizzazione per un file viene limitata tramite IRM, le restrizioni di accesso e utilizzo vengono applicate indipendentemente in cui le informazioni, perché l'autorizzazione a un file viene archiviata nel file del documento stesso. In questo scenario, la crittografia AD RMS fornisce un ulteriore livello di protezione per i file. Anche se una persona con accesso a un file riservato lo inviasse accidentalmente tramite posta elettronica, il file viene protetto con crittografia AD RMS. Gli utenti che vogliono accedere al file devono prima autenticarsi su un server AD RMS per ricevere la chiave di decrittografia.|  
  


