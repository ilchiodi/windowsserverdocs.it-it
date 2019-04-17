---
ms.assetid: af76ddbe-83a2-4a62-9989-873e3bb1c772
title: Ruolo di proprietario della topologia del sito
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 7c98d313cac28ab07380791a384a87bffacfbda2
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2017
---
# <a name="site-topology-owner-role"></a>Ruolo di proprietario della topologia del sito

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

L'amministratore che gestisce la topologia del sito è noto come il proprietario della topologia del sito. Il proprietario della topologia del sito in grado di riconoscere le condizioni della rete tra siti e autorizzato a modificare le impostazioni in servizi di dominio Active Directory (AD DS) per implementare le modifiche alla topologia del sito. Modifiche alla topologia del sito le modifiche nella topologia di replica. Responsabilità del proprietario della topologia del sito includono:  
  
-   Controllo delle modifiche alla topologia del sito se cambia la connettività di rete.  
  
-   Recupero e la gestione delle informazioni sulle connessioni di rete e router dal gruppo di rete. Il proprietario della topologia del sito è necessario gestire un elenco di indirizzi della subnet, subnet mask e il percorso in cui ogni appartiene. Il proprietario della topologia del sito deve comprendere anche eventuali problemi che influiscono sulla topologia del sito per impostare in modo efficace i costi per i collegamenti di sito sulla velocità di rete e la capacità.  
  
-   Spostare gli oggetti server di Active Directory che rappresenta il controller di dominio tra siti se cambia indirizzo IP del controller di dominio in una subnet diversa in un sito diverso o se la subnet stessa viene assegnata a un sito diverso. In entrambi i casi, è necessario che il proprietario della topologia del sito spostare manualmente l'oggetto server di Active Directory del controller di dominio nel nuovo sito.  
  


