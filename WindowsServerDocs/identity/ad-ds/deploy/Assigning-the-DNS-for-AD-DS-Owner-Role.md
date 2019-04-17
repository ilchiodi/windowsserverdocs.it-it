---
ms.assetid: 4163cf03-3bff-426c-9844-4cc2d7897d52
title: L'assegnazione del DNS per il ruolo di proprietario di directory Active Directory
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: e393cbf32aa5a13ff22044eabb8c575508baaf79
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2017
---
# <a name="assigning-the-dns-for-ad-ds-owner-role"></a>L'assegnazione del DNS per il ruolo di proprietario di directory Active Directory

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Il proprietario della foresta assegna un sistema DNS (Domain Name) per il proprietario di servizi di dominio Active Directory (AD DS) per la foresta. Il DNS per il proprietario di dominio Active Directory della foresta è una persona o gruppo di utenti, che è responsabile della supervisione la distribuzione di DNS per l'infrastruttura di dominio Active Directory e di assicurarsi che, se necessario, i nomi di dominio vengono registrati con le autorità Internet appropriate.  
  
Il DNS per il proprietario di dominio Active Directory è responsabile per il DNS per la progettazione di Active Directory per la foresta. Se l'organizzazione è attualmente in esecuzione un servizio Server DNS, la progettazione DNS per il servizio Server DNS esistente funziona con il DNS per il proprietario di dominio Active Directory delegare il nome DNS radice della foresta ai server DNS in esecuzione sul controller di dominio.  
  
Il DNS per il proprietario di dominio Active Directory per la foresta, inoltre, mantiene contatto con il gruppo di Dynamic Host Configuration Protocol (DHCP) e il gruppo DNS dell'organizzazione e coordina i piani di singoli proprietari DNS di ogni dominio della foresta (se presente) a tali gruppi. Il proprietario DNS per la foresta assicura che i gruppi di server DHCP e DNS sono coinvolti nel DNS per il processo di progettazione di dominio Active Directory in modo che è a conoscenza del piano di progettazione DNS e può fornire input all'inizio di ogni gruppo.  
  


