---
title: Gestire una raccolta di sessioni desktop personali in Servizi Desktop remoto
description: Informazioni su come aggiungere Host sessione Desktop remoto e programmi RemoteApp alla distribuzione di Servizi Desktop remoto.
ms.prod: windows-server
ms.technology: remote-desktop-services
ms.author: elizapo
ms.date: 11/08/2016
ms.topic: article
author: lizap
manager: dongill
ms.openlocfilehash: 7088d164ecdd7211894b004ed580eecb33d1ba60
ms.sourcegitcommit: 3a3d62f938322849f81ee9ec01186b3e7ab90fe0
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 04/23/2020
ms.locfileid: "80861064"
---
# <a name="manage-your-personal-desktop-session-collections"></a>Gestire le raccolte di sessioni desktop personali

Usa le informazioni seguenti per gestire una raccolta di sessioni desktop personali in Servizi Desktop remoto.

## <a name="manually-assign-a-user-to-a-personal-session-host"></a>Assegnare manualmente un utente a un host sessione personale
Usa il cmdlet **Set-RDPersonalSessionDesktopAssignment** per assegnare manualmente un utente a un server host sessione personale nella raccolta. Il cmdlet supporta i parametri seguenti:

-CollectionName \<string\>

-ConnectionBroker \<string\> 

-User \<string\>

-Name \<string\>

- **-CollectionName**: specifica il nome della raccolta di desktop sessione personali. Questo parametro è obbligatorio.
- **-ConnectionBroker**: specifica il server Gestore connessione Desktop remoto per la distribuzione di Desktop remoto. Se non specifichi un valore, il cmdlet usa il nome di dominio completo (FQDN) del computer locale.
- **-User**: specifica l'account utente da associare al desktop sessione personale, nel formato DOMINIO\utente. Questo parametro è obbligatorio.
- **-Name**: specifica il nome del server host sessione. Questo parametro è obbligatorio. L'host sessione identificato qui deve essere un membro della raccolta specificata dal parametro **-CollectionName**.

Il cmdlet **Import-RDPersonalSessionDesktopAssignment** importa le associazioni tra gli account utente e i desktop sessione personali da un file di testo. Il cmdlet supporta i parametri seguenti:

-CollectionName \<string\>

-ConnectionBroker \<string\>

-Path \<string>

**-Path** specifica il percorso e il nome di un file da importare.
 
## <a name="removing-a-user-assignment-from-a-personal-session-host"></a>Rimozione di un'assegnazione utente da un host sessione personale
Usa il cmdlet **Remove-RDPersonalSessionDesktopAssignment** per rimuovere l'associazione tra un utente e un desktop sessione personale. Il cmdlet supporta i parametri seguenti:

-CollectionName \<string\>

-ConnectionBroker \<string\>

-Force

-Name \<string\>

-User \<string\>

**-Force** forza l'esecuzione del comando senza chiedere conferma all'utente.

## <a name="query-user-assignments"></a>Eseguire query sulle assegnazioni utente
Usa il cmdlet **Get-RDPersonalSessionDesktopAssignment** per ottenere un elenco di desktop sessione personali e gli account utente associati. Il cmdlet supporta i parametri seguenti:

-CollectionName \<string\>

-ConnectionBroker \<string\>

-User \<string\>

-Name \<string\>

Puoi eseguire il cmdlet per eseguire query in base al nome della raccolta, al nome utente o al nome del desktop sessione. Se specifichi solo il parametro **-CollectionName**, il cmdlet restituisce un elenco di host sessione e gli utenti associati. Se specifichi anche il parametro **-User**, viene restituito l'host sessione associato a tale utente. Se specifichi il parametro **-Name**, viene restituito l'utente associato a tale host sessione. 


Il cmdlet **Export-RDPersonalPersonalDesktopAssignment** esporta le associazioni correnti tra utenti e desktop virtuali personali in un file di testo. Il cmdlet supporta i parametri seguenti:

-CollectionName \<string\>

-ConnectionBroker \<string\>

-Path \<string\>


Tutti i nuovi cmdlet supportano i parametri comuni -Verbose, -Debug, -ErrorAction, -ErrorVariable, -OutBuffer e -OutVariable. Per altre informazioni, vedi [about_CommonParameters](https://go.microsoft.com/fwlink/p/?LinkID=113216).
