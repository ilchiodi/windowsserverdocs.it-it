---
title: Tenere traccia delle licenze di accesso client (CAL) di Servizi Desktop remoto
description: Informazioni su come tenere traccia delle licenze CAL nella distribuzione di servizi desktop remoto.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 80d82d30-3ad0-4a8c-9a9b-2773c47eee19
author: lizap
ms.author: elizapo
ms.date: 05/11/2017
manager: dongill
ms.openlocfilehash: ed7490b2b61eb516d43b280b67fcefaeedb3e225
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59890622"
---
# <a name="track-your-remote-desktop-services-client-access-licenses-rds-cals"></a>Tenere traccia delle licenze di accesso client (CAL) di Servizi Desktop remoto

>Si applica a: Windows Server (canale semestrale), Windows Server 2016

È possibile utilizzare lo strumento Gestione licenze Desktop remoto per creare report per monitorare le CAL Per utente che sono stati rilasciati da un server licenze Desktop remoto.

> [!NOTE]
>  Se si usa Azure AD Domain Services nell'ambiente in uso, lo strumento Gestione licenze Desktop remoto non funzionerà per ottenere licenze CAL Per utente. In alternativa, è necessario tenere traccia delle licenze manualmente, tramite gli eventi di accesso, il polling le connessioni Desktop remoto attive tramite il gestore di connessione o un altro meccanismo che funziona per l'utente. 

Usare la procedura seguente per generare un report di licenze CAL utente per:

1. In Gestione licenze Desktop remoto fare clic sul server licenze, fare clic su **Crea rapporto**, quindi fare clic su **utilizzo di licenze CAL per ogni utente**.
2. Impostare l'ambito per il report: selezionare uno dei seguenti:
   - Intero dominio - quello in cui il server licenze è un membro.
   - Unità organizzativa - qualsiasi unità Organizzativa all'interno del dominio in cui il server licenze è un membro.
   - Intero dominio e tutti i domini trusted - possono includere domini nelle altre foreste. Se si seleziona questa opzione, è possibile aumentare il tempo necessario per creare il report.

   La selezione effettuata determina gli account utente in Active Directory Domain Services vengono cercati le informazioni di CAL Servizi Desktop remoto per ogni utente generare il report.
3. Fare clic su **creazione di Report**. Il report viene creato e viene visualizzato un messaggio per confermare che il report è stato creato correttamente. Fare clic su **OK** per chiudere il messaggio.

Il report creato verrà visualizzato nella sezione report sotto il nodo per il server licenze. Il report fornisce le informazioni seguenti:

- Data e ora di creazione report
- L'ambito del report (ad esempio, dominio, OU = Sales, o tutti i domini trusted)
- Il numero di CAL Per utente che vengono installati nel server licenze
- Il numero di CAL Per utente che sono stati rilasciati dal server licenze in base all'ambito del report

È anche possibile salvare il report come file CSV in una cartella nel computer. Per salvare il report, fare clic sul report che si desidera salvare, fare clic su Salva con nome e quindi specificare il nome file e percorso in cui salvare il report.

I report creati sono elencati nel nodo report sotto il nodo per il server licenze di gestione licenze Desktop remoto. Se non è più necessario un report, è possibile eliminarlo.
