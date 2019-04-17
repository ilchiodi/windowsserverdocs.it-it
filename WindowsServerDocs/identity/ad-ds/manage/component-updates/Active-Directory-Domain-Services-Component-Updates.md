---
title: Aggiornamenti dei componenti di servizi di dominio Active Directory
description: Questo documento vengono descritti gli aggiornamenti dei componenti di dominio Active Directory per Windows Server 2012 R2
author: billmath
ms.author: billmath
manager: femila
ms.date: 09/08/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.assetid: a3a91034-a4da-4ad7-93f8-0cd2ec3e7824
ms.technology: identity-adds
ms.openlocfilehash: 52d3dab542b4250670067e927f42ddf1597fc852
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/12/2017
---
# <a name="active-directory-domain-services-component-updates"></a>Aggiornamenti dei componenti di servizi di dominio Active Directory

>Si applica a: Windows Server 2016, Windows Server 2012 R2

Questo modulo introduce i componenti che hanno ricevuto aggiornamenti secondari in servizi Directory e spazi di identità.  
  
|Informazioni sull'autore|  
|--------------------|  
|**Autore:**|Turner Justin|  
|**Biografia:**|Turner è supporto Senior Escalation Engineer con sede a Irving, in Texas, USA il team di servizi di Directory.  Ha creato o contribuito a molti corsi di formazione e articoli della Knowledge base per la Knowledgebase Microsoft negli ultimi 12 anni. Insegna dipendenti Microsoft e clienti nuova architettura del prodotto, è un membro Microsoft Certified Master (MCM), Microsoft Certified Trainer (MCT) e contiene una laurea magistrale in scienze informatiche e sistemi cognitivi.|  
|**Collaboratori**|Questo modulo di formazione include contributi *Michiko Short*, *Dean Wells*, *Alan Jowett*, *Manu Pushpendran*, *Yashar Bahman*, *Anoosh Saboori*, *Rashmi Jha*, *Justin Hall* e *Herbert Mauerer*|  
|**Revisori**|Si ringraziano ringraziano dedicato i propri tempo revisione e commenti e suggerimenti: *Joey Seifert*, *Justin Hall*|  
  
> [!NOTE]  
> Questo contenuto è stato redatto da un ingegnere del supporto tecnico Microsoft ed è destinato ad amministratori esperti e architetti di sistemi che desiderano per una spiegazione tecnica più approfondita delle funzionalità e le soluzioni in Windows Server 2012 R2 che solitamente disponibili su TechNet. Tuttavia, non è stata sottoposta stessi passaggi redazionali, in modo che il linguaggio potrebbe sembrare che meno accurato della documentazione che cosa si trova in genere su TechNet.  
  
### <a name="what-you-will-learn"></a>Contenuto dell'esercitazione  
Dopo aver completato questo modulo, sarà in grado di:  
  
-   Descrivere gli aggiornamenti dei componenti riguardanti le aree di tecnologia identità e servizi di Directory in Windows Server 2012 R2  
  
    -   [Unicità SPN e UPN](../../../ad-ds/manage/component-updates/SPN-and-UPN-uniqueness.md)  
  
    -   [Accesso automatico riavvio Sign-On & #40; Winlogon & #41;](../../../ad-ds/manage/component-updates/Winlogon-Automatic-Restart-Sign-On--ARSO-.md)  
  
    -   [Attestazione chiave TPM](../../../ad-ds/manage/component-updates/TPM-Key-Attestation.md)  
  
    -   [Cmdlet di Backup della CA e ripristino configurazione di Windows PowerShell](../../../ad-ds/manage/component-updates/CA-Backup-and-Restore-Windows-PowerShell-cmdlets.md)  
  
    -   [Controllo dell'elaborazione della riga di comando](../../../ad-ds/manage/component-updates/Command-line-process-auditing.md)  
  
    -   [Gestione e protezione delle credenziali](https://technet.microsoft.com/library/dn408190.aspx)  
  
    -   [Aggiornamenti dei componenti di servizi directory](../../../ad-ds/manage/component-updates/Directory-Services-component-updates.md)  
  
        -   [Livelli di funzionalità del dominio e foresta](../../../ad-ds/manage/component-updates/../../../ad-ds/manage/component-updates/Directory-Services-component-updates.md#BKMK_FL)  
  
        -   [Elementi deprecati di NTFRS](../../../ad-ds/manage/component-updates/Directory-Services-component-updates.md#BKMK_NTFRS)  
  
        -   [Modifiche a Query Optimizer LDAP](../../../ad-ds/manage/component-updates/../../../ad-ds/manage/component-updates/Directory-Services-component-updates.md#BKMK_LDAPQuery)  
  
        -   [Miglioramenti dell'evento 1644](../../../ad-ds/manage/component-updates/Directory-Services-component-updates.md#BKMK_1644)  
  
        -   [Miglioramento della velocità effettiva della replica di Directory Active](../../../ad-ds/manage/component-updates/../../../ad-ds/manage/component-updates/Directory-Services-component-updates.md#BKMK_ADRepl)  
  


