---
ms.assetid: e2651dc8-4b31-4cd8-a400-3b8123890210
title: Procedure consigliate per la protezione di Active Directory
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: ecef0c173677d379524189b1769d4721ab0774a8
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2017
---
# <a name="best-practices-for-securing-active-directory"></a>Procedure consigliate per la protezione di Active Directory

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Questo documento contiene prospettiva di un professionista e contiene un set di tecniche pratiche che consentono ai responsabili IT di proteggere un ambiente Active Directory aziendale. Active Directory svolge un ruolo fondamentale nell'infrastruttura IT e garantisce l'armonia e la protezione delle risorse di rete diverso in un ambiente globale interconnesso. I metodi descritti sono basati principalmente sull'esperienza dell'organizzazione Microsoft Information Security and Risk Management (ISRM), che ha il compito di proteggere le risorse di Microsoft IT e altre divisioni aziendali Microsoft, oltre a fornire consulenze a un numero selezionato di clienti Microsoft Global 500.  
  
-   [Premessa](https://technet.microsoft.com/library/dn487451.aspx)  
  
-   [Riconoscimenti](https://technet.microsoft.com/library/dn487445.aspx)  
  
-   [Schema riepilogativo](../../../ad-ds/manage/component-updates/Executive-Summary.md)  
  
-   [Introduzione](../../../ad-ds/manage/component-updates/Introduction.md)  
  
-   [Esposizione a possibili violazioni](../../../ad-ds/plan/security-best-practices/Avenues-to-Compromise.md)  
  
-   [Account interessanti per il furto di credenziali](../../../ad-ds/plan/security-best-practices/Attractive-Accounts-for-Credential-Theft.md)  
  
-   [Ridurre la superficie di attacco di Active Directory](../../../ad-ds/plan/security-best-practices/Reducing-the-Active-Directory-Attack-Surface.md)  
  
-   [Implementazione dei modelli amministrativi con privilegi minimi](../../../ad-ds/plan/security-best-practices/Implementing-Least-Privilege-Administrative-Models.md)  
  
-   [Implementazione host amministrativi protetti](../../../ad-ds/plan/security-best-practices/Implementing-Secure-Administrative-Hosts.md)  
  
-   [Protezione dei controller di dominio dagli attacchi](../../../ad-ds/plan/security-best-practices/Securing-Domain-Controllers-Against-Attack.md)  
  
-   [Monitoraggio dei segnali di compromissione di Active Directory](../../../ad-ds/plan/security-best-practices/Monitoring-Active-Directory-for-Signs-of-Compromise.md)  
  
-   [Suggerimenti per i criteri di controllo](../../../ad-ds/plan/security-best-practices/Audit-Policy-Recommendations.md)  
  
-   [Pianificazione del compromesso](../../../ad-ds/plan/security-best-practices/Planning-for-Compromise.md)  
  
-   [Mantenimento di un ambiente più sicuro](../../../ad-ds/plan/security-best-practices/Maintaining-a-More-Secure-Environment.md)  
  
-   [Appendici](../../../ad-ds/plan/security-best-practices/Appendices.md)  
   
-   [Appendice b: account con privilegi e gruppi in Active Directory](../../../ad-ds/plan/security-best-practices/Appendix-B--Privileged-Accounts-and-Groups-in-Active-Directory.md)  
  
-   [Appendice c: account protetti e gruppi in Active Directory](../../../ad-ds/plan/security-best-practices/Appendix-C--Protected-Accounts-and-Groups-in-Active-Directory.md)  
  
-   [Appendice d: protezione predefiniti Administrator account in Active Directory](../../../ad-ds/plan/security-best-practices/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory.md)  
  
-   [Appendice e: protezione dei gruppi Enterprise Admins in Active Directory](../../../ad-ds/plan/security-best-practices/Appendix-E--Securing-Enterprise-Admins-Groups-in-Active-Directory.md)  
  
-   [Appendice f: protezione Domain Admins gruppi in Active Directory](../../../ad-ds/plan/security-best-practices/Appendix-F--Securing-Domain-Admins-Groups-in-Active-Directory.md)  
  
-   [Appendice g: protezione dei gruppi Administrators in Active Directory](../../../ad-ds/plan/security-best-practices/Appendix-G--Securing-Administrators-Groups-in-Active-Directory.md)  
  
-   [Appendice h: protezione Local Administrator account e gruppi](../../../ad-ds/plan/security-best-practices/Appendix-H--Securing-Local-Administrator-Accounts-and-Groups.md)  
  
-   [Appendice i: creazione di gestione degli account per gli account protetti e gruppi in Active Directory](../../../ad-ds/manage/component-updates/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory.md)   
  
-   [Appendice l: eventi da monitorare](../../../ad-ds/plan/Appendix-L--Events-to-Monitor.md)  
  
-   [Appendice m: collegamenti a documenti e letture consigliate](../../../ad-ds/manage/Appendix-M--Document-Links-and-Recommended-Reading.md)  
  


