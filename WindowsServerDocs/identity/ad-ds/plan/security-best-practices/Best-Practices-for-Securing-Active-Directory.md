---
ms.assetid: e2651dc8-4b31-4cd8-a400-3b8123890210
title: Procedure consigliate per la protezione di Active Directory
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 972def668634e794908a3ff2933d038ae38be5d6
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59817082"
---
# <a name="best-practices-for-securing-active-directory"></a>Procedure consigliate per la protezione di Active Directory

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Questo documento offre una prospettiva di un professionista e contiene un set di tecniche pratiche che consentono ai responsabili IT di proteggere un ambiente Active Directory aziendale. Active Directory svolge un ruolo fondamentale nell'infrastruttura IT e garantisce l'armonia e la sicurezza di risorse di rete diverse in un ambiente globale interconnesso. I metodi descritti sono basati principalmente sull'esperienza dell'organizzazione Microsoft Information Security e gestione dei rischi (ISRM), che è responsabile di proteggere le risorse di Microsoft IT e altre divisioni aziendali di Microsoft, oltre a segnalare una numero selezionato di clienti Microsoft Global 500.  
  
-   [Schema riepilogativo](../../../ad-ds/manage/component-updates/Executive-Summary.md)  
  
-   [Introduzione](../../../ad-ds/manage/component-updates/Introduction.md)  
  
-   [Esposizione a possibili violazioni](../../../ad-ds/plan/security-best-practices/Avenues-to-Compromise.md)  
  
-   [Account interessanti per il furto di credenziali](../../../ad-ds/plan/security-best-practices/Attractive-Accounts-for-Credential-Theft.md)  
  
-   [La riduzione della superficie di attacco di Active Directory](../../../ad-ds/plan/security-best-practices/Reducing-the-Active-Directory-Attack-Surface.md)  
  
-   [Implementazione di modelli amministrativi con privilegi minimi](../../../ad-ds/plan/security-best-practices/Implementing-Least-Privilege-Administrative-Models.md)  
  
-   [Implementazione host amministrativi protetti](../../../ad-ds/plan/security-best-practices/Implementing-Secure-Administrative-Hosts.md)  
  
-   [Proteggere i controller di dominio da eventuali attacchi](../../../ad-ds/plan/security-best-practices/Securing-Domain-Controllers-Against-Attack.md)  
  
-   [Monitoraggio dei segnali di compromissione di Active Directory](../../../ad-ds/plan/security-best-practices/Monitoring-Active-Directory-for-Signs-of-Compromise.md)  
  
-   [Suggerimenti per i criteri di controllo](../../../ad-ds/plan/security-best-practices/Audit-Policy-Recommendations.md)  
  
-   [Pianificazione del compromesso](../../../ad-ds/plan/security-best-practices/Planning-for-Compromise.md)  
  
-   [Manutenzione di un ambiente più sicuro](../../../ad-ds/plan/security-best-practices/Maintaining-a-More-Secure-Environment.md)  
  
-   [Appendici](../../../ad-ds/plan/security-best-practices/Appendices.md)  
   
-   [Appendice b: Gli account con privilegi e gruppi in Active Directory](../../../ad-ds/plan/security-best-practices/Appendix-B--Privileged-Accounts-and-Groups-in-Active-Directory.md)  
  
-   [Appendice C: Gli account protetti e gruppi in Active Directory](../../../ad-ds/plan/security-best-practices/Appendix-C--Protected-Accounts-and-Groups-in-Active-Directory.md)  
  
-   [Appendice d: Protezione predefiniti Administrator account in Active Directory](../../../ad-ds/plan/security-best-practices/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory.md)  
  
-   [Appendice e: Protezione dei gruppi Enterprise Admins in Active Directory](../../../ad-ds/plan/security-best-practices/Appendix-E--Securing-Enterprise-Admins-Groups-in-Active-Directory.md)  
  
-   [Appendice f: Protezione Domain Admins gruppi in Active Directory](../../../ad-ds/plan/security-best-practices/Appendix-F--Securing-Domain-Admins-Groups-in-Active-Directory.md)  
  
-   [Appendice g: Protezione dei gruppi Administrators in Active Directory](../../../ad-ds/plan/security-best-practices/Appendix-G--Securing-Administrators-Groups-in-Active-Directory.md)  
  
-   [Appendice h: Protezione dell'account di amministratore locale e i gruppi](../../../ad-ds/plan/security-best-practices/Appendix-H--Securing-Local-Administrator-Accounts-and-Groups.md)  
  
-   [Appendice i: Creazione di gestione di account per gli account protetti e gruppi in Active Directory](../../../ad-ds/manage/component-updates/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory.md)   
  
-   [Appendice l: Eventi da monitorare](../../../ad-ds/plan/Appendix-L--Events-to-Monitor.md)  
  
-   [Appendice m: Collegamenti a documenti e letture consigliate](../../../ad-ds/manage/Appendix-M--Document-Links-and-Recommended-Reading.md)  
  


