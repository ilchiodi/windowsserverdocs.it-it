---
title: Usare desktop sessione personali con Servizi Desktop remoto
description: Informazioni su come condividere tramite Servizi Desktop remoto desktop personalizzati assegnati.
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.tgt_pltfrm: na
ms.topic: article
author: lizap
ms.author: elizapo
ms.date: 10/22/2019
manager: dongill
ms.openlocfilehash: 9386733911ca81ad60d038854bd68e5603aae4cf
ms.sourcegitcommit: 3262c5c7cece9f2adf2b56f06b7ead38754a451c
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 10/23/2019
ms.locfileid: "72812279"
---
# <a name="use-personal-session-desktops-with-remote-desktop-services"></a>Usare desktop sessione personali con Servizi Desktop remoto

>Si applica a: Windows Server (Canale semestrale), Windows Server 2019, Windows Server 2016

Puoi distribuire desktop personali basati su server in un ambiente di cloud computing usando desktop sessione personali.  Un ambiente di cloud computing applica una separazione tra i server Hyper-V dell'infrastruttura e le macchine virtuali guest, ad esempio il servizio cloud di Microsoft Azure o la piattaforma cloud Microsoft. La funzionalità desktop sessione personali estende lo scenario di distribuzione di desktop basati su sessione in Servizi Desktop remoto per creare un nuovo tipo di raccolta di sessioni in cui ogni utente è assegnato al proprio host sessione personale con diritti amministrativi. 

Usa le informazioni seguenti per creare e gestire una raccolta di desktop sessione personali.

## <a name="create-a-personal-session-desktop-collection"></a>Creare una raccolta di desktop sessione personali

Usa il cmdlet New-RDSessionCollection per creare una raccolta di desktop sessione personali. I tre parametri seguenti offrono le informazioni di configurazione necessarie per i desktop sessione personali:

- **-PersonalUnmanaged**: specifica il tipo di raccolta di sessioni che consente di assegnare utenti a un server host sessione personale. Se non specifichi questo parametro, la raccolta verrà creata come una tradizionale raccolta di host sessione Desktop remoto, in cui gli utenti vengono assegnati al successivo host sessione disponibile al momento dell'accesso.
- **-GrantAdministrativePrivilege**: se usi **-PersonalUnmanaged**, questo parametro specifica che all'utente assegnato all'host sessione devono essere concessi privilegi amministrativi. Se non usi questo parametro, agli utenti verranno concessi solo i privilegi utente standard.
- **-AutoAssignUser**: se usi **-PersonalUnmanaged**, questo parametro specifica che i nuovi utenti che si connettono tramite Gestore connessione Desktop remoto devono essere automaticamente assegnati a un host sessione non assegnato. Se non sono presenti host sessione non assegnati nella raccolta, l'utente vedrà un messaggio di errore. Se non usi questo parametro, dovrai [assegnare manualmente gli utenti a un host sessione](#manually-assign-a-user-to-a-personal-session-host) prima che accedano.

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


Tutti i nuovi cmdlet supportano i parametri comuni -Verbose, -Debug, -ErrorAction, -ErrorVariable, -OutBuffer e -OutVariable. Per altre informazioni, vedere [about_CommonParameters](https://go.microsoft.com/fwlink/p/?LinkID=113216).
