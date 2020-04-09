---
ms.assetid: 4163cf03-3bff-426c-9844-4cc2d7897d52
title: Assegnazione del DNS per il ruolo di proprietario di Active Directory Domain Services
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: 8ef63fd9e53d20c812ff84e601e73d4276b304be
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80825264"
---
# <a name="assigning-the-dns-for-ad-ds-owner-role"></a>Assegnazione del DNS per il ruolo di proprietario di Active Directory Domain Services

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Il proprietario della foresta assegna un Domain Name System (DNS) per il proprietario Active Directory Domain Services (AD DS) per la foresta. Il DNS per il proprietario di servizi di dominio Active Directory della foresta è una persona (o gruppo di persone) responsabile della supervisione della distribuzione del DNS per l'infrastruttura di servizi di dominio Active Directory e per assicurarsi che i nomi di dominio (se necessario) siano registrati con le autorità Internet appropriate.  
  
Il DNS per il proprietario di servizi di dominio Active Directory è responsabile del DNS per la progettazione di servizi di dominio Active Directory per la foresta. Se l'organizzazione sta attualmente operando in un servizio server DNS, la finestra di progettazione DNS per il servizio server DNS esistente funziona con DNS per il proprietario di servizi di dominio Active Directory per delegare il nome DNS radice della foresta ai server DNS in esecuzione nei controller di dominio.  
  
Il DNS per il proprietario di servizi di dominio Active Directory per la foresta mantiene anche il contatto con il gruppo Dynamic Host Configuration Protocol (DHCP) e il gruppo DNS dell'organizzazione e coordina i piani dei singoli proprietari DNS di ogni dominio nella foresta, se presente, con questi gruppi. Il proprietario DNS per la foresta garantisce che i gruppi DHCP e DNS siano interessati dal processo di progettazione DNS per servizi di dominio Active Directory, in modo che ogni gruppo sia a conoscenza del piano di progettazione DNS e possa fornire input anticipato.  
  


