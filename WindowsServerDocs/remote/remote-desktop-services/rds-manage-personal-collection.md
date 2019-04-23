---
title: Gestire un insieme di sessioni di desktop personali in Servizi Desktop remoto
description: Informazioni su come aggiungere e programmi host sessione Desktop remoto e RemoteApp per la distribuzione di servizi desktop remoto.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.author: elizapo
ms.date: 11/08/2016
ms.tgt_pltfrm: na
ms.topic: article
author: lizap
manager: dongill
ms.openlocfilehash: 286c7ba4bd4428669d135c35c825033d22b8f40e
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59865722"
---
## <a name="manage-your-personal-desktop-session-collections"></a>Gestire gli insiemi di sessioni di desktop personali

Usare le informazioni seguenti per gestire un insieme di sessioni di desktop personali in Servizi Desktop remoto.

### <a name="manually-assign-a-user-to-a-personal-session-host"></a>Assegnare manualmente un utente a un host di sessioni personali
Usare la **Set-RDPersonalSessionDesktopAssignment** cmdlet assegnare manualmente un utente a un server host sessione personali nella raccolta. Il cmdlet supporta i parametri seguenti:

-CollectionName \<string\>

-ConnectionBroker \<string\> 

-Utente \<stringa\>

-Nome \<stringa\>

- **– CollectionName** -specifica il nome dell'insieme di desktop personali della sessione. Questo parametro è obbligatorio.
- **– ConnectionBroker** -specifica il server Gestore connessione Desktop remoto (Gestore connessione desktop remoto) per la distribuzione di Desktop remoto. Se non viene specificato un valore, il cmdlet Usa il nome di dominio completo (FQDN) del computer locale.
- **– Utente** -specifica l'account utente da associare con il desktop di sessioni personali, nel formato dominio\utente. Questo parametro è obbligatorio.
- **– Nome** -specifica il nome del server host sessione. Questo parametro è obbligatorio. Host sessione identificato qui deve essere un membro della raccolta che il **- CollectionName** parametro specifica.

Il **Import-RDPersonalSessionDesktopAssignment** cmdlet consente di importare le associazioni tra gli account utente e computer desktop di sessioni personali da un file di testo. Il cmdlet supporta i parametri seguenti:

-CollectionName \<string\>

-ConnectionBroker \<string\>

-Path \<string>

**– Percorso** specifica il percorso e il nome di un file da importare.
 
### <a name="removing-a-user-assignment-from-a-personal-session-host"></a>Rimozione di un'assegnazione di utente da un Host di sessioni personali
Usare la **Remove-RDPersonalSessionDesktopAssignment** cmdlet per rimuovere l'associazione tra un utente e un desktop di sessioni personali. Il cmdlet supporta i parametri seguenti:

-CollectionName \<string\>

-ConnectionBroker \<string\>

-Force

-Nome \<stringa\>

-Utente \<stringa\>

**– Force** forza l'esecuzione senza chiedere conferma all'utente del comando.

### <a name="query-user-assignments"></a>Assegnazioni utente di query
Usare la **Get-RDPersonalSessionDesktopAssignment** per ottenere un elenco di computer desktop di sessioni personali e gli account utente associato. Il cmdlet supporta i parametri seguenti:

-CollectionName \<string\>

-ConnectionBroker \<string\>

-Utente \<stringa\>

-Nome \<stringa\>

È possibile eseguire il cmdlet per eseguire query dal nome della raccolta, nome utente, o dal nome del desktop di sessione. Se si specifica solo le **– CollectionName** , il cmdlet restituisce un elenco di host di sessioni e utenti associati. Se si specifica anche il **– utente** parametro, viene restituito l'host di sessione associato a tale utente. Se si specifica la **– nome** parametro, viene restituito l'utente associato a tale host della sessione. 


Il **Export-RDPersonalPersonalDesktopAssignment** cmdlet consente di esportare le associazioni tra utenti e i desktop virtuali personali correnti in un file di testo. Il cmdlet supporta i parametri seguenti:

-CollectionName \<string\>

-ConnectionBroker \<string\>

-Path \<string\>


Tutti i nuovi cmdlet di supportano i parametri comuni:-Verbose,-Debug, - ErrorAction, - ErrorVariable,-OutBuffer e - OutVariable. Per altre informazioni, vedere [about_CommonParameters](https://go.microsoft.com/fwlink/p/?LinkID=113216).
