---
title: Tenere traccia delle licenze CAL di Servizi Desktop remoto
description: Informazioni su come tenere traccia delle licenze CAL nella distribuzione di Servizi Desktop remoto.
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 278aa7a2d35aeacfaee8deddcd64e668a320853f
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71387117"
---
# <a name="track-your-remote-desktop-services-client-access-licenses-rds-cals"></a>Tenere traccia delle licenze CAL di Servizi Desktop remoto

>Si applica a: Windows Server (Canale semestrale), Windows Server 2019, Windows Server 2016

Puoi usare lo strumento Gestione licenze Desktop remoto per creare rapporti con cui tenere traccia delle licenze CAL Per Utente di Servizi Desktop remoto emesse da un server licenze Desktop remoto.

> [!NOTE]
>  Se usi Azure AD Domain Services nell'ambiente, lo strumento Gestione licenze Desktop remoto non funzionerà per ottenere licenze CAL Per utente. Dovrai quindi tenere traccia delle licenze manualmente, tramite eventi di accesso, il polling delle connessioni Desktop remoto attive con Gestore connessione o un altro meccanismo adatto alle tue esigenze. 

Segui questa procedura per generare un rapporto relativo alle licenze CAL Per Utente:

1. In Gestione licenze Desktop remoto fai clic con il pulsante destro del mouse sul server licenze, scegli **Crea rapporto** e quindi fai clic su **Utilizzo licenze CAL Per Utente**.
2. Imposta l'ambito del rapporto selezionando una delle opzioni seguenti:
   - Intero dominio: dominio di cui è membro il server licenze.
   - Unità organizzativa: qualsiasi unità organizzativa nel dominio di cui è membro il server licenze.
   - Intero dominio e tutti i domini trusted: può includere i domini di altre foreste. Selezionando questa opzione potrebbe essere necessario più tempo per creare il rapporto.

   L'opzione selezionata determina gli account utente in Active Directory Domain Services in cui vengono cercate le informazioni relative alle licenze CAL Per Utente di Servizi Desktop remoto per generare il rapporto.
3. Fai clic su **Crea rapporto**. Il rapporto viene creato e viene visualizzato un messaggio per confermare la creazione. Fare clic su **OK** per chiudere il messaggio.

Il rapporto creato viene visualizzato nella sezione Rapporti sotto il nodo per il server licenze. Il rapporto fornisce le informazioni seguenti:

- Data e ora di creazione del rapporto
- Ambito del rapporto (ad esempio, dominio, Unità organizzativa=Vendite o Tutti i domini trusted)
- Numero di licenze CAL Per Utente di Servizi Desktop remoto installate nel server licenze
- Numero di licenze CAL Per Utente di Servizi Desktop remoto emesse dal server licenze per l'ambito del rapporto

È anche possibile salvare il rapporto come file CSV in una cartella nel computer. Per il salvataggio, fai clic con il pulsante destro del mouse sul rapporto da salvare, scegli Salva con nome e quindi specifica nome file e percorso per il salvataggio.

I rapporti creati sono elencati nel nodo Rapporti sotto il nodo per il server licenze in Gestione licenze Desktop remoto. Se un rapporto non è più necessario, puoi eliminarlo.
