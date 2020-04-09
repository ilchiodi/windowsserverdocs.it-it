---
title: Considerazioni sull'account utente
description: Fornisce considerazioni sull'account utente, sul nome utente e sulla password per servizi MultiPoint
ms.prod: windows-server
ms.technology: multipoint-services
ms.topic: article
ms.assetid: e225900b-cee9-48c9-b21c-394dc5e72b78
author: lizap
manager: dongill
ms.author: elizapo
ms.date: 08/04/2016
ms.openlocfilehash: 732100a79adbdd7d9fbe4ade742c43084d6b54f6
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80820574"
---
# <a name="user-account-considerations"></a>Considerazioni sull'account utente
Questo argomento descrive problemi che un utente con privilegi amministrativi deve tenere in considerazione per la creazione e la gestione di account utente. Gestire gli account utente nella scheda utenti gestione MultiPoint. Per altre informazioni, vedere l'argomento [Gestire account utente](Manage-User-Accounts.md).  
  
## <a name="user-account-types"></a>Tipi di account utente  
Un account utente è una raccolta di informazioni che indica a MultiPoint Services i file e le cartelle a cui un utente può accedere, le modifiche che possono apportare al sistema MultiPoint Services e le preferenze di ogni utente, ad esempio lo sfondo del desktop. Ogni persona accede al proprio account utente usando un nome utente e una password univoci. Servizi multiPoint supporta tre tipi di account utente:  
  
-   **Gli account utente con privilegi amministrativi** sono per le persone che verrà utilizzato Gestione MultiPoint per utilizzare e gestire il sistema di servizi MultiPoint. Per altre informazioni, vedere [Creare un account utente amministrativo](Create-an-Administrative-User-Account.md).  
  
-   Gli **account utente standard** riguardano singole persone che accedono regolarmente alle stazioni ma non gestiscono il sistema. Di norma, è necessario creare account utente standard per la maggior parte degli utenti del sistema MultiPoint Services. Per altre informazioni, vedere [Creare un account utente standard](Create-a-Standard-User-Account.md).  
  
-   Gli **account utente di Dashboard MultiPoint** riguardano singole persone che usano Dashboard MultiPoint per gestire le sessioni utente standard e possono accedere da qualsiasi stazione. Per altre informazioni, vedere [Creazione di un account utente di Dashboard MultiPoint](Create-a-MultiPoint-Dashboard-User-Account.md).  
  
## <a name="user-name-and-password-considerations"></a>Considerazioni su nome utente e password  
Gli utenti con privilegi amministrativi possono eseguire attività che riguardano tutti gli altri utenti del sistema MultiPoint Services, ad esempio installazione di software o modifica delle impostazioni di protezione. Per questa ragione, gli utenti con privilegi amministrativi devono avere password e nomi utente univoci noti solo a loro.  
  
Una considerazione importante sugli account utente è che a ogni account utente è allocata una libreria **Documenti** univoca di Esplora risorse che include la cartella **Documenti**. Se gli utenti standard del sistema MultiPoint Services archiviano documenti privati nella libreria **Documenti** in Esplora risorse, dovranno anch'essi accedere al sistema MultiPoint Services usando nome utente e password univoci noti solo a loro. Per altre informazioni sull'archiviazione di documenti in Esplora risorse, vedere l'argomento [Gestire file utente](Manage-User-Files.md).  
  
> [!TIP]  
> Per una maggiore sicurezza del sistema, tutte le password degli utenti devono essere password complesse. Una password complessa non può essere facilmente indovinata o incrinata, è costituita da almeno otto caratteri, non contiene il nome dell'account utente o parte di esso e contiene almeno tre delle quattro categorie di caratteri seguenti: caratteri maiuscoli, caratteri minuscoli, numeri e simboli trovati in una tastiera (ad esempio!, @, #).  
  
## <a name="see-also"></a>Vedi anche  
[Creare un account utente con privilegi amministrativi](Create-an-Administrative-User-Account.md)  
[Creare un account utente standard](Create-a-Standard-User-Account.md)  
[Gestire i file utente](Manage-User-Files.md)
[gestire gli account utente](Manage-User-Accounts.md)