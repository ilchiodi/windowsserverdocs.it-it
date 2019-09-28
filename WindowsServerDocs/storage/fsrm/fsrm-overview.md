---
title: Panoramica di Gestione risorse file server
ms.prod: windows-server
ms.author: jgerend
ms.manager: brianlic
ms.technology: storage
ms.topic: article
author: jasongerend
ms.date: 5/14/2018
description: File server Gestione risorse (FSRM) è uno strumento che consente di gestire e classificare i dati in un file server Windows Server.
ms.openlocfilehash: 719176307afc320ad676fd1acfc07ad9d15920cf
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71394173"
---
# <a name="file-server-resource-manager-fsrm-overview"></a>Panoramica di Gestione risorse file server

> Si applica a: Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2, Windows Server (canale semestrale), 

Gestione risorse file server è un servizio ruolo in Windows Server che consente di gestire e classificare i dati archiviati nei file server. È possibile utilizzare Gestione risorse file server per classificare automaticamente i file, eseguire attività in base a tali classificazioni, impostare quote sulle cartelle e creare report che monitorano l'utilizzo dell'archiviazione.

Si tratta di un punto limitato, ma è stata anche [aggiunta la possibilità di disabilitare i journal delle modifiche](#whats-new) in Windows Server, versione 1803.

## <a name="features"></a>Funzionalità

Gestione risorse file server include le funzionalità seguenti:

-   [Gestione quote](quota-management.md) consente di limitare lo spazio consentito per un volume o una cartella e possono essere applicati automaticamente alle nuove cartelle create in un volume. È inoltre possibile definire i modelli di quote applicabili a nuovi volumi o cartelle.  
-   [Infrastruttura di classificazione file](classification-management.md) offre informazioni approfondite sui dati automatizzando i processi di classificazione in modo che sia possibile gestire i dati in modo più efficace. È possibile classificare i file e applicare criteri in base a questa classificazione. Criteri di esempio includono il controllo dell'accesso dinamico per limitare l'accesso ai file, la crittografia dei file e la scadenza dei file. I file possono essere classificati automaticamente tramite le regole di classificazione dei file o manualmente modificando le proprietà di una cartella o un file selezionato.
-   [Attività di gestione file](file-management-tasks.md) consente di applicare criteri o azioni condizionali ai file in base alla relativa classificazione. Le condizioni per un'attività di gestione file includono il percorso del file, le proprietà di classificazione, la data di creazione e la data dell'ultima modifica o dell'ultimo accesso. Le azioni eseguibili da un'attività di gestione file includono l'impostazione della scadenza per i file, la crittografia dei file o l'esecuzione di un comando personalizzato.
-   [Gestione screening dei file](file-screening-management.md) consente di controllare i tipi di file che l'utente può archiviare in un file server. È possibile limitare i file archiviabili nei file condivisi in base all'estensione. Ad esempio, è possibile creare uno screening dei file che non consenta l'archiviazione dei file con estensione MP3 nelle cartelle condivise personali in un file server.
-   I [report di archiviazione](storage-reports-management.md) consentono di identificare le tendenze di utilizzo del disco e il modo in cui i dati vengono classificati. È inoltre possibile monitorare un gruppo selezionato di utenti per individuare i tentativi di salvataggio di file non autorizzati.  
  
Le funzionalità incluse in file server Gestione risorse possono essere configurate e gestite tramite l'app Gestione risorse file server o tramite Windows PowerShell.
  
> [!IMPORTANT]
>  Gestione risorse file server supporta volumi formattati solo con il file system NTFS. Il file system ReFS (Resilient File System) non è supportato.  
  
## <a name="practical-applications"></a>Applicazioni pratiche  
 Alcune applicazioni pratiche di Gestione risorse file server includono:  
  
-   Utilizzare Infrastruttura di classificazione file con lo scenario Controllo di accesso dinamico per create criteri per concedere l'accesso a file e cartelle in base alla classificazione dei file nel file server.  
  
-   Creare una regola di classificazione file per contrassegnare qualsiasi file che contenga almeno 10 codici fiscali come file con informazioni che consentono l'identificazione personale.  
  
-   Impostare come scaduto qualsiasi file non modificato negli ultimi 10 anni.  
  
-   Creare una quota di 200 megabyte per la home directory di ogni utente e inviare una notifica quando usano 180 megabyte.  
  
-   Non consentire l'archiviazione di file musicali nelle cartelle condivise personali.  
  
-   Pianificare un rapporto da eseguire ogni domenica a mezzanotte, che generi un elenco dei file utilizzati più di recente nei due giorni precedenti. Ciò può essere utile per determinare l'attività di archiviazione nei fine settimana e pianificare di conseguenza i periodi di inattività del server.  

## <a name="whats-new"></a>Novità: impedire a FSRM di creare Journal delle modifiche

A partire da Windows Server, versione 1803, è ora possibile impedire al servizio file server Gestione risorse di creare un journal delle modifiche (noto anche come journal USN) sui volumi all'avvio del servizio. Questo consente di conservare un po' di spazio in ogni volume, ma disabilita la classificazione dei file in tempo reale.

Per le nuove funzionalità precedenti, vedere Novità [di file Server Gestione risorse](https://technet.microsoft.com/library/dn383587.aspx).

Per impedire che il file server Gestione risorse la creazione di un journal delle modifiche in alcuni o in tutti i volumi all'avvio del servizio, attenersi alla procedura seguente: 

1. Arrestare il servizio SRMSVC. Ad esempio, aprire una sessione di PowerShell come amministratore e immettere `Stop-Service SrmSvc`.
2. Eliminare il journal USN per i volumi per i quali si vuole conservare spazio usando il comando Fsutil: 

      ```
      fsutil usn deletejournal /d <VolumeName>
      ```
    Ad esempio: `fsutil usn deletejournal /d c:`

3. Aprire l'editor del registro di sistema, ad `regedit` esempio, digitando nella stessa sessione di PowerShell.
4. Passare alla chiave seguente: **HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\SrmSvc\Settings**
5. Per ignorare facoltativamente la creazione del journal delle modifiche per l'intero server (ignorare questo passaggio se si desidera disabilitarlo solo su volumi specifici):
    1. Fare clic con il pulsante destro del mouse sulla chiave **Settings** , quindi scegliere **nuovo** > **valore DWORD (32-bit)** . 
    1. Assegnare un nome `SkipUSNCreationForSystem`al valore.
    1. Impostare il valore su **1** (in esadecimali).
6. Per ignorare facoltativamente la creazione del journal delle modifiche per volumi specifici:
    1. Ottenere i percorsi del volume che si vuole ignorare usando il `fsutil volume list` comando o il comando di PowerShell seguente:
        ```PowerShell
        Get-Volume | Format-Table DriveLetter,FileSystemLabel,Path
        ```
       Di seguito è riportato un esempio di output:

       ```
        DriveLetter FileSystemLabel Path
        ----------- --------------- ----
                    System Reserved \\?\Volume{8d3c9e8a-0000-0000-0000-100000000000}\
        C                           \\?\Volume{8d3c9e8a-0000-0000-0000-501f00000000}\
       ```
    2. Tornare all'editor del registro di sistema, fare clic con il pulsante destro del mouse sulla chiave **HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\SrmSvc\Settings** , quindi scegliere **nuovo** **valore multistringa** > .
    3. Assegnare un nome `SkipUSNCreationForVolumes`al valore.
    4. Immettere il percorso di ogni volume in cui si ignora la creazione di un journal delle modifiche, inserendo ogni percorso in una riga separata. Esempio:

        ```
        \\?\Volume{8d3c9e8a-0000-0000-0000-100000000000}\
        \\?\Volume{8d3c9e8a-0000-0000-0000-501f00000000}\
        ```

        > [!NOTE] 
        > L'editor del registro di sistema potrebbe indicare che sono state rimosse stringhe vuote, visualizzando questo avviso che è possibile ignorare: @no__t 0Apparecchiature per di tipo REG_MULTI_SZ non possono contenere stringhe vuote. Nell'editor del registro di sistema vengono rimosse tutte le stringhe vuote trovate.*

7. Avviare il servizio SRMSVC. Ad esempio, in una sessione di PowerShell `Start-Service SrmSvc`immettere.



## <a name="see-also"></a>Vedere anche

- [Controllo dinamico degli accessi](https://technet.microsoft.com/library/dn408191(v=ws.11).aspx) 