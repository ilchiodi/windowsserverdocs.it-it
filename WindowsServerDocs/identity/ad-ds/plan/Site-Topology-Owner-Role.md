---
ms.assetid: af76ddbe-83a2-4a62-9989-873e3bb1c772
title: Ruolo di proprietario della topologia del sito
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: 1e90c88db6a40de48444e047628abcee6cc3673b
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80821824"
---
# <a name="site-topology-owner-role"></a>Ruolo di proprietario della topologia del sito

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

L'amministratore che gestisce la topologia del sito è noto come proprietario della topologia del sito. Il proprietario della topologia del sito riconosce le condizioni della rete tra siti e ha l'autorità di modificare le impostazioni in Active Directory Domain Services (AD DS) per implementare le modifiche alla topologia del sito. Le modifiche alla topologia del sito influiscono sulle modifiche nella topologia di replica. Le responsabilità del proprietario della topologia del sito includono:  
  
-   Controllo delle modifiche alla topologia del sito in caso di modifica della connettività di rete.  
  
-   Ottenere e gestire le informazioni sulle connessioni di rete e i router dal gruppo di rete. Il proprietario della topologia del sito deve gestire un elenco di indirizzi subnet, subnet mask e il percorso a cui appartengono. Il proprietario della topologia del sito deve anche comprendere eventuali problemi relativi alla velocità e alla capacità della rete che influiscono sulla topologia del sito per impostare efficacemente i costi per i collegamenti di  
  
-   Lo stato di Active Directory oggetti server che rappresentano i controller di dominio tra siti se l'indirizzo IP di un controller di dominio viene modificato in un'altra subnet in un sito diverso o se la subnet stessa viene assegnata a un sito diverso. In entrambi i casi, il proprietario della topologia del sito deve spostare manualmente l'oggetto server Active Directory del controller di dominio nel nuovo sito.  
  


