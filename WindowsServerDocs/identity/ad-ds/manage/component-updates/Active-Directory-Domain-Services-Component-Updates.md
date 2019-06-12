---
title: Aggiornamenti dei componenti di Servizi di dominio Active Directory
description: Questo documento vengono descritti gli aggiornamenti dei componenti di dominio Active Directory per Windows Server 2012 R2
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 09/08/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.assetid: a3a91034-a4da-4ad7-93f8-0cd2ec3e7824
ms.technology: identity-adds
ms.openlocfilehash: b0bd021863e1e25bd222baf9a633438153fe820b
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/31/2019
ms.locfileid: "66442766"
---
# <a name="active-directory-domain-services-component-updates"></a>Aggiornamenti dei componenti di Servizi di dominio Active Directory

>Si applica a: Windows Server 2016, Windows Server 2012 R2

Questo modulo introduce i componenti che hanno ricevuto aggiornamenti secondari nelle aree Servizi directory e Identità.  


| Informazioni sull'autore |
|------------------|
|   **Autore:**    |
|     **Biografia:**     |
| **Collaboratori** |
|  **Revisori**   |

> [!NOTE]  
> Questo contenuto è stato redatto da un ingegnere del Supporto tecnico Microsoft ed è destinato ad amministratori esperti e architetti di sistemi che desiderano una spiegazione tecnica delle funzionalità e delle soluzioni relative a Windows Server 2012 R2 più approfondita rispetto agli argomenti solitamente disponibili su TechNet. Non è stato tuttavia sottoposto agli stessi passaggi redazionali e, di conseguenza, per alcune lingue potrebbe essere meno accurato della documentazione che si trova in genere su TechNet.  

### <a name="what-you-will-learn"></a>Contenuto dell'esercitazione  
Dopo aver completato questo modulo si sarà in grado di eseguire le operazioni seguenti:  

-   Descrivere gli aggiornamenti dei componenti nelle aree riguardanti le tecnologie Servizi directory e Identità di Windows Server 2012 R2  

    -   [Unicità di SPN e UPN](../../../ad-ds/manage/component-updates/SPN-and-UPN-uniqueness.md)  

    -   [Riavvio automatico di Winlogon Sign-On &#40;ARSO&#41;](../../../ad-ds/manage/component-updates/Winlogon-Automatic-Restart-Sign-On--ARSO-.md)  

    -   [Attestazione chiave TPM](../../../ad-ds/manage/component-updates/TPM-Key-Attestation.md)  

    -   [Cmdlet di Windows PowerShell per il backup e ripristino di una CA](../../../ad-ds/manage/component-updates/CA-Backup-and-Restore-Windows-PowerShell-cmdlets.md)  

    -   [Controllo dell'elaborazione della riga di comando](../../../ad-ds/manage/component-updates/Command-line-process-auditing.md)  

    -   [Gestione e protezione delle credenziali](https://technet.microsoft.com/library/dn408190.aspx)  

    -   [Aggiornamenti dei componenti di Servizi directory](../../../ad-ds/manage/component-updates/Directory-Services-component-updates.md)  

        -   [Livelli di funzionalità di domini e foreste](../../../ad-ds/manage/component-updates/../../../ad-ds/manage/component-updates/Directory-Services-component-updates.md#BKMK_FL)  

        -   [Elementi deprecati di NTFRS](../../../ad-ds/manage/component-updates/Directory-Services-component-updates.md#BKMK_NTFRS)  

        -   [Modifiche a Query Optimizer LDAP](../../../ad-ds/manage/component-updates/../../../ad-ds/manage/component-updates/Directory-Services-component-updates.md#BKMK_LDAPQuery)  

        -   [Miglioramenti dell'evento 1644](../../../ad-ds/manage/component-updates/Directory-Services-component-updates.md#BKMK_1644)  

        -   [Miglioramento della velocità effettiva della replica di Active Directory](../../../ad-ds/manage/component-updates/../../../ad-ds/manage/component-updates/Directory-Services-component-updates.md#BKMK_ADRepl)  



