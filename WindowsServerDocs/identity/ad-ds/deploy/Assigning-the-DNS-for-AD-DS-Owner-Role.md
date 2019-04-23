---
ms.assetid: 4163cf03-3bff-426c-9844-4cc2d7897d52
title: Assegnazione del DNS per il ruolo di proprietario di Active Directory Domain Services
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: dde9ed6035b30ba5b5b96b7132d25a1f8a1c0b8c
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59884022"
---
# <a name="assigning-the-dns-for-ad-ds-owner-role"></a>Assegnazione del DNS per il ruolo di proprietario di Active Directory Domain Services

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Il proprietario della foresta viene assegnato un sistema DNS (Domain Name) per il proprietario di Active Directory Domain Services (AD DS) per la foresta. Il DNS per il proprietario di dominio Active Directory della foresta è una persona o gruppo di persone, chi è responsabile della supervisione della distribuzione del DNS per l'infrastruttura di Active Directory Domain Services e di assicurarsi che, se necessario, i nomi di dominio registrati con l'autorità Internet appropriate.  
  
Il DNS per il proprietario di Active Directory Domain Services è responsabile per il DNS per la progettazione di Active Directory Domain Services per la foresta. Se l'organizzazione è attualmente in esecuzione un servizio Server DNS, la finestra di progettazione DNS per il servizio Server DNS esistente funziona con il DNS per il proprietario di Active Directory Domain Services delegare il nome DNS radice della foresta a server DNS eseguiti nei controller di dominio.  
  
Il DNS per il proprietario di Active Directory Domain Services per la foresta, inoltre, mantiene contatto con il gruppo di Dynamic Host Configuration Protocol (DHCP) e il gruppo di DNS dell'organizzazione e coordina i piani dei singoli proprietari di ogni dominio nella foresta DNS (se presente) con tali gruppi. Il proprietario DNS per la foresta assicura che i gruppi di server DHCP e DNS sono coinvolti nel DNS per il processo di progettazione di Active Directory Domain Services in modo che ogni gruppo è a conoscenza del piano di progettazione di DNS e può fornire input all'inizio.  
  


