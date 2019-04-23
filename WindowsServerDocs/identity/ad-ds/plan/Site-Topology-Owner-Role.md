---
ms.assetid: af76ddbe-83a2-4a62-9989-873e3bb1c772
title: Ruolo di proprietario della topologia del sito
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 69443c2fc1af855c7df002e0ac91d43986eff6da
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59832112"
---
# <a name="site-topology-owner-role"></a>Ruolo di proprietario della topologia del sito

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

L'amministratore che gestisce la topologia del sito è noto come il proprietario della topologia del sito. Il proprietario della topologia del sito riconosce le condizioni della rete tra siti e ha l'autorità di modificare le impostazioni in servizi di dominio Active Directory (AD DS) per implementare le modifiche alla topologia del sito. Le modifiche alla topologia del sito influiscono sulle modifiche nella topologia di replica. Le responsabilità del proprietario della topologia del sito includono:  
  
-   Se viene modificata la connettività di rete che controllano le modifiche alla topologia del sito.  
  
-   Come ottenere e gestione di informazioni sulle connessioni di rete e i router dal gruppo di rete. Il proprietario della topologia del sito debba mantenere un elenco di indirizzi di subnet, le subnet mask e la posizione in cui ognuno. Il proprietario della topologia del sito è necessario comprendere anche eventuali problemi che influiscono sulla topologia del sito per impostare in modo efficace i costi per i collegamenti di sito sulla capacità e velocità di rete.  
  
-   Lo spostamento di oggetti di server di Active Directory che rappresentano i controller di dominio tra siti se cambia indirizzo IP del controller di dominio a una subnet diversa in un sito diverso o se la subnet stessa viene assegnata a un altro sito. In entrambi i casi, il proprietario della topologia del sito deve spostare manualmente l'oggetto server di Active Directory del controller di dominio per il nuovo sito.  
  


