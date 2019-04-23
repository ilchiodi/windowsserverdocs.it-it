---
title: Usare desktop di sessioni personali con Servizi Desktop remoto
description: Informazioni su come condividere personalizzate, assegnato desktop tramite Servizi Desktop remoto.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.tgt_pltfrm: na
ms.topic: article
author: lizap
ms.author: elizapo
ms.date: 09/16/2016
manager: dongill
ms.openlocfilehash: 41f6a28c1b754a5277a0966a87dae08a6aa34e08
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59875952"
---
# <a name="use-personal-session-desktops-with-remote-desktop-services"></a>Usare desktop di sessioni personali con Servizi Desktop remoto

>Si applica a: Windows Server (canale semestrale), Windows Server 2016

È possibile distribuire in base al server di desktop personali in un ambiente di cloud computing con desktop di sessioni personali.  (Un ambiente di cloud computing ha una separazione tra i server dell'infrastruttura Hyper-V e le macchine virtuali guest, ad esempio Cloud di Microsoft Azure o la piattaforma Cloud Microsoft). La funzionalità di desktop personali sessione consente di estendere lo scenario di distribuzione desktop basati su sessione in Servizi Desktop remoto per creare un nuovo tipo di insieme di sessioni in cui ogni utente è assegnato al proprio host sessione personali con diritti amministrativi. 

Usare le informazioni seguenti per creare e gestire un insieme di desktop personali della sessione.

## <a name="create-a-personal-session-desktop-collection"></a>Creare un insieme di desktop di sessioni personali

Usare il cmdlet New-RDSessionCollection per creare un insieme di desktop personali della sessione. I tre parametri seguenti forniscono le informazioni di configurazione necessarie per i desktop di sessioni personali:

- **-PersonalUnmanaged** -specifica il tipo di insieme di sessioni che consente di assegnare gli utenti a un server host sessione personali. Se non si specifica questo parametro, viene creata la raccolta come una raccolta di Host sessione Desktop remoto tradizionali, in cui gli utenti vengono assegnati quando effettuano l'accesso all'host sessione disponibile successivo.
- **-GrantAdministrativePrivilege** : se si usa **- PersonalUnmanaged**, specifica che l'utente assegnato all'host della sessione da ha privilegi amministrativi. Se non si usa questo parametro, gli utenti vengono concessi solo i privilegi di utente standard.
- **-AutoAssignUser** : se si usa **- PersonalUnmanaged**, specifica i nuovi utenti che si connettono tramite il Gestore connessione desktop remoto vengono assegnati automaticamente a un host di sessione non assegnati. Se non sono presenti host sessione non assegnati nella raccolta, l'utente verrà visualizzato un messaggio di errore. Se non si usa questo parametro, devi [assegnare manualmente gli utenti a un host sessione](#manually-assign-a-user-to-a-personal-session-host) prima che effettuano l'accesso.

## <a name="manually-assign-a-user-to-a-personal-session-host"></a>Assegnare manualmente un utente a un host di sessioni personali
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
 
## <a name="removing-a-user-assignment-from-a-personal-session-host"></a>Rimozione di un'assegnazione di utente da un Host di sessioni personali
Usare la **Remove-RDPersonalSessionDesktopAssignment** cmdlet per rimuovere l'associazione tra un utente e un desktop di sessioni personali. Il cmdlet supporta i parametri seguenti:

-CollectionName \<string\>

-ConnectionBroker \<string\>

-Force

-Nome \<stringa\>

-Utente \<stringa\>

**– Force** forza l'esecuzione senza chiedere conferma all'utente del comando.

## <a name="query-user-assignments"></a>Assegnazioni utente di query
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

## <a name="hardware-accelerated-graphics"></a>Grafica con accelerazione hardware
Windows Server 2016 estende la tecnologia RemoteFX presentata 3D Graphics adapter (vGPU) per supportare OpenGL e supporta le macchine virtuali guest di Windows Server 2016 di utente singolo. È possibile combinare i desktop di sessioni personali con le nuove funzionalità vGPU per fornire supporto per le applicazioni ospitate che richiedono grafica accelerata. In alternativa, è possibile combinare i desktop personali sessione con la nuova funzionalità di assegnazione dispositivo discreti (DDA) a garantiscono inoltre supporto per le applicazioni ospitate che richiedono grafica accelerata.
