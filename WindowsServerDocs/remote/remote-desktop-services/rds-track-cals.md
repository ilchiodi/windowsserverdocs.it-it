---
title: Tenere traccia delle licenze CAL di Servizi Desktop remoto
description: Informazioni su come tenere traccia delle licenze CAL nella distribuzione di Servizi Desktop remoto.
ms.prod: windows-server
ms.technology: remote-desktop-services
ms.topic: article
ms.assetid: 80d82d30-3ad0-4a8c-9a9b-2773c47eee19
author: lizap
ms.author: elizapo
ms.date: 05/11/2017
manager: dongill
ms.openlocfilehash: 7e5793427b4a294d90c7b9ebeb66bb27578be190
ms.sourcegitcommit: 3a3d62f938322849f81ee9ec01186b3e7ab90fe0
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 04/23/2020
ms.locfileid: "80857334"
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
