---
title: Panoramica di Gestione risorse file server
ms.prod: windows-server-threshold
ms.author: jgerend
ms.manager: brianlic
ms.technology: storage
ms.topic: article
author: jasongerend
ms.date: 5/14/2018
description: Gestione risorse file Server (FSRM) è uno strumento che consente di gestire e classificare i dati in un file server Windows Server.
ms.openlocfilehash: 107d08f247fc56720ccc3d11a3db88c77377257c
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59870722"
---
# <a name="file-server-resource-manager-fsrm-overview"></a>Panoramica di Gestione risorse file server

> Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2

Gestione risorse file server è un servizio ruolo in Windows Server che consente di gestire e classificare i dati archiviati nei file server. È possibile utilizzare Gestione risorse File Server a automaticamente classificare i file, eseguire le attività in base a queste classificazioni, impostare le quote nelle cartelle e creare report di monitoraggio dell'utilizzo di archiviazione.

È anche un punto di piccole dimensioni, ma si è eseguito [stata aggiunta la possibilità di disabilitare il journal delle modifiche](#whats-new) in Windows Server, versione 1803.

## <a name="features"></a>Funzionalità

Gestione risorse file server include le funzionalità seguenti:

-   [Gestione quote](quota-management.md) consente un limite allo spazio consentito per un volume o una cartella e possono essere applicate automaticamente alle nuove cartelle create in un volume. È inoltre possibile definire i modelli di quote applicabili a nuovi volumi o cartelle.  
-   [Infrastruttura di classificazione file](classification-management.md) fornisce approfondite sui dati automatizzando il processo di classificazione in modo che è possibile gestire i dati in modo più efficace. È possibile classificare i file e applicare criteri in base a questa classificazione. Criteri di esempio includono il controllo dell'accesso dinamico per limitare l'accesso ai file, la crittografia dei file e la scadenza dei file. I file possono essere classificati automaticamente tramite le regole di classificazione dei file o manualmente modificando le proprietà di una cartella o un file selezionato.
-   [Attività di gestione file](file-management-tasks.md) consente di applicare un criteri o azioni condizionali ai file in base alla classificazione. Le condizioni per un'attività di gestione file includono il percorso del file, le proprietà di classificazione, la data di creazione e la data dell'ultima modifica o dell'ultimo accesso. Le azioni eseguibili da un'attività di gestione file includono l'impostazione della scadenza per i file, la crittografia dei file o l'esecuzione di un comando personalizzato.
-   [Gestione di screening dei file](file-screening-management.md) consente di controllare i tipi di file che gli utenti possono archiviare in un file server. È possibile limitare i file archiviabili nei file condivisi in base all'estensione. Ad esempio, è possibile creare uno screening dei file che non consenta l'archiviazione dei file con estensione MP3 nelle cartelle condivise personali in un file server.
-   [I rapporti di archiviazione](storage-reports-management.md) consentono di identificare le tendenze di utilizzo del disco e come vengono classificati i dati. È inoltre possibile monitorare un gruppo selezionato di utenti per individuare i tentativi di salvataggio di file non autorizzati.  
  
Le funzionalità incluse con Gestione risorse File Server possono essere configurate e gestite tramite l'app Gestione risorse File Server o Windows PowerShell.
  
> [!IMPORTANT]
>  Gestione risorse file server supporta volumi formattati solo con il file system NTFS. Il file system ReFS (Resilient File System) non è supportato.  
  
## <a name="practical-applications"></a>Applicazioni pratiche  
 Alcune applicazioni pratiche di Gestione risorse file server includono:  
  
-   Utilizzare Infrastruttura di classificazione file con lo scenario Controllo di accesso dinamico per create criteri per concedere l'accesso a file e cartelle in base alla classificazione dei file nel file server.  
  
-   Creare una regola di classificazione file per contrassegnare qualsiasi file che contenga almeno 10 codici fiscali come file con informazioni che consentono l'identificazione personale.  
  
-   Impostare come scaduto qualsiasi file non modificato negli ultimi 10 anni.  
  
-   Creare una quota di 200 megabyte per la home directory di ogni utente e inviare notifica quando l'utilizzo raggiunge i 180 megabyte.  
  
-   Non consentire l'archiviazione di file musicali nelle cartelle condivise personali.  
  
-   Pianificare un rapporto da eseguire ogni domenica a mezzanotte, che generi un elenco dei file utilizzati più di recente nei due giorni precedenti. Ciò può essere utile per determinare l'attività di archiviazione nei fine settimana e pianificare di conseguenza i periodi di inattività del server.  

## <a name="whats-new"></a>Novità: impedire la creazione di registrazioni modifica Gestione risorse file server

A partire da Windows Server, versione 1803, è possibile impedire a questo punto il servizio di gestione risorse File Server di creare un journal delle modifiche (noto anche come un journal USN) nei volumi all'avvio del servizio. Ciò consente di conservare un po' di spazio su ciascun volume, ma disabiliterà la classificazione dei file in tempo reale.

Per le nuove funzionalità meno recenti, vedere [What ' s New in Gestione risorse File Server](https://technet.microsoft.com/library/dn383587.aspx).

Per impedire la creazione di un journal delle modifiche in alcuni o tutti i volumi all'avvio del servizio di gestione risorse File Server, procedere come segue: 

1. Arrestare il servizio SRMSVC. Ad esempio, aprire una sessione di PowerShell come amministratore e immettere `Stop-Service SrmSvc`.
2. Eliminare il journal USN per i volumi che si desidera risparmiare spazio nell'usare il comando fsutil: 

      ```
      fsutil usn deletejournal /d <VolumeName>
      ```
    Ad esempio: `fsutil usn deletejournal /d c:`

3. Aprire Editor del Registro di sistema, ad esempio, digitando `regedit` nella stessa sessione di PowerShell.
4. Passare alla chiave seguente: **HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\SrmSvc\Settings**
5. Al facoltativamente skip modificare la creazione di registrazioni per l'intero server (ignorare questo passaggio se si vuole disabilitare questa solo su volumi specifici):
    1. Fare doppio clic il **le impostazioni** chiave e quindi selezionare **New** > **valore DWORD (32 bit)**. 
    1. Nome valore `SkipUSNCreationForSystem`.
    1. Impostare il valore su **1** (in esadecimale).
6. Se lo si desidera ignorare la creazione di journal delle modifiche per volumi specifici:
    1. Ottenere i percorsi del volume che si desidera ignorare usando il `fsutil volume list` comando o il comando PowerShell seguente:
        ```PowerShell
        Get-Volume | Format-Table DriveLetter,FileSystemLabel,Path
        ```
       Ecco un esempio di output:

       ```
        DriveLetter FileSystemLabel Path
        ----------- --------------- ----
                    System Reserved \\?\Volume{8d3c9e8a-0000-0000-0000-100000000000}\
        C                           \\?\Volume{8d3c9e8a-0000-0000-0000-501f00000000}\
       ```
    2. Fare doppio clic su Indietro nell'Editor del registro, il **HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\SrmSvc\Settings** chiave e quindi selezionare **New** > **multistringa Valore**.
    3. Nome valore `SkipUSNCreationForVolumes`.
    4. Immettere il percorso di ciascun volume in cui si ignora la creazione di un journal delle modifiche, l'inserimento di ogni percorso in una riga separata. Ad esempio: 

        ```
        \\?\Volume{8d3c9e8a-0000-0000-0000-100000000000}\
        \\?\Volume{8d3c9e8a-0000-0000-0000-501f00000000}\
        ```

        > [!NOTE] 
        > Editor del Registro di sistema potrebbe indicare che sia rimosso le stringhe vuote, visualizzare questo avviso che è possibile ignorare tranquillamente: *I dati di tipo REG_MULTI_SZ non possono contenere le stringhe vuote. L'Editor del Registro di sistema rimuoverà tutte le stringhe vuote trovate.*

7. Avviare il servizio SRMSVC. Ad esempio, in una sessione di PowerShell immettere `Start-Service SrmSvc`.



## <a name="see-also"></a>Vedere anche

- [Controllo dinamico degli accessi](https://technet.microsoft.com/library/dn408191(v=ws.11).aspx) 