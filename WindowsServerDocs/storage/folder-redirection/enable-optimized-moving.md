---
title: Abilitare con ottimizzazione per la sposta delle cartelle reindirizzate
description: Come eseguire uno spostamento ottimizzato delle cartelle reindirizzate a una nuova condivisione file.
ms.prod: windows-server-threshold
ms.topic: article
author: JasonGerend
ms.author: jgerend
ms.technology: storage
ms.date: 09/10/2018
ms.localizationpriority: medium
ms.openlocfilehash: 7bdf30a4f721568add4e7902245da2a803b72db1
ms.sourcegitcommit: ed27ddbe316d543b7865bc10590b238290a2a1ad
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/09/2019
ms.locfileid: "65475873"
---
# <a name="enable-optimized-moves-of-redirected-folders"></a>Abilitare con ottimizzazione per la sposta delle cartelle reindirizzate

>Si applica a: Windows 10, Windows 8, Windows 8.1, Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server (canale semestrale)

Questo argomento descrive come eseguire uno spostamento delle cartelle reindirizzate (reindirizzamento) ottimizzato per una nuova condivisione file. Se si abilita questa impostazione di criteri, quando un amministratore si sposta la condivisione di file che contiene le cartelle reindirizzate e aggiorna il percorso di destinazione delle cartelle reindirizzate in Criteri di gruppo, il contenuto memorizzato nella cache viene semplicemente rinominato nella cache dei file Offline locale senza eventuali ritardi o perdita di dati per l'utente.

In precedenza, gli amministratori è stato possibile modificare il percorso di destinazione delle cartelle reindirizzate in Criteri di gruppo e consentire che i computer client copiare i file all'accesso successivo dell'utente interessato, provocando un ritardo di accesso. In alternativa, gli amministratori è stato possibile spostare la condivisione file e aggiornare il percorso di destinazione delle cartelle reindirizzate in Criteri di gruppo. Tuttavia, le modifiche apportate localmente nei computer client tra l'inizio dello spostamento e la prima sincronizzazione dopo lo spostamento andrebbero perse.

## <a name="prerequisites"></a>Prerequisiti

Spostamento ottimizzato ha i requisiti seguenti:

- Il reindirizzamento delle cartelle deve essere configurato. Per altre informazioni, vedere [distribuire reindirizzamento cartelle con i file Offline](deploy-folder-redirection.md).
- I computer client devono eseguire Windows 10, Windows 8.1, Windows 8, Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012 o Windows Server (canale semestrale).

## <a name="step-1-enable-optimized-move-in-group-policy"></a>Passaggio 1: Abilitano lo spostamento ottimizzato in Criteri di gruppo

Per ottimizzare la rilocazione dei dati di reindirizzamento cartelle, usare criteri di gruppo per abilitare la **Enable con ottimizzazione per la spostare il contenuto nella cache dei file Offline in caso di modifica percorso di reindirizzamento cartelle server** impostazione dei criteri per i criteri di gruppo appropriato Oggetto (GPO). Configurare questa impostazione dei criteri per **disabilitati** o **non configurato** fa sì che il client copiare tutto il contenuto di reindirizzamento cartelle nel nuovo percorso e quindi eliminare il contenuto dal percorso precedente se il modifiche del percorso server.

Di seguito viene illustrato come abilitare lo spostamento ottimizzata delle cartelle reindirizzate:

1. In Gestione criteri di gruppo, fare doppio clic su oggetto Criteri di gruppo creato per le impostazioni di reindirizzamento cartelle (ad esempio, **reindirizzamento cartelle e impostazioni dei profili utente mobili**), quindi selezionare **modificare**.
2. Sotto **User Configuration**, passare alla **criteri**, quindi **modelli amministrativi**, quindi **sistema**, quindi  **Il reindirizzamento delle cartelle**.
3. Fare doppio clic su **Enable con ottimizzazione per la spostare il contenuto nella cache dei file Offline in caso di modifica percorso di reindirizzamento cartelle server**, quindi selezionare **modificare**.
4. Selezionare **Enabled**, quindi selezionare **OK**.

## <a name="step-2-relocate-the-file-share-for-redirected-folders"></a>Passaggio 2: Spostare la condivisione file per le cartelle reindirizzate

Quando lo spostamento della condivisione di file che contiene degli utenti di cartelle reindirizzate, è importante adottare delle precauzioni per garantire che le cartelle vengono rilocate correttamente.

>[!IMPORTANT]
>Se i file degli utenti sono uso o se lo stato completo del file non viene mantenuto durante lo spostamento, gli utenti potrebbero riscontrare una riduzione delle prestazioni come i file vengono copiati in rete, conflitti di sincronizzazione generati da file Offline, o addirittura una perdita di dati.

1. Notificare in anticipo agli utenti che cambia il server che ospita le cartelle reindirizzate e consiglia che eseguono le azioni seguenti:

      - Sincronizzare il contenuto della cache dei file Offline e risolvere eventuali conflitti.
      - Aprire un prompt dei comandi con privilegi elevati, immettere **GpUpdate /Target:User /Force**e quindi disconnettersi e accedere nuovamente per assicurarsi che le impostazioni di criteri di gruppo più recenti vengono applicate al computer client

        >[!NOTE]
        >Per impostazione predefinita, aggiornare i computer client Criteri di gruppo ogni 90 minuti, pertanto se consentire tempo sufficiente per i client ai computer di ricevere i criteri aggiornati, è necessario richiedere agli utenti di usare **GpUpdate**.
2. Rimuovere la condivisione file dal server per assicurarsi che siano presenti file nella condivisione file in uso. A tale scopo in Server Manager, scegliere il **condivisioni** pagina di servizi File e archiviazione, fare doppio clic su condivisione file appropriata, quindi selezionare **rimuovere**.

    Gli utenti funzionerà in modalità offline mediante i file Offline fino al completamento dello spostamento e ricevono le impostazioni di reindirizzamento cartelle aggiornate da criteri di gruppo.

3. Utilizzando un account con privilegi di backup, spostare il contenuto della condivisione file nella nuova posizione utilizzando un metodo che consente di mantenere i timestamp dei file, ad esempio un backup e ripristino di utilità. Usare la **Robocopy** comando, aprire un prompt dei comandi con privilegi elevati e quindi digitare il comando seguente, dove ```<Source>``` è la posizione corrente della condivisione file, e ```<Destination>``` è la nuova posizione:

    ```PowerShell
    Robocopy /B <Source> <Destination> /Copyall /MIR /EFSRAW
    ```

    >[!NOTE]
    >Il **Xcopy** tutto lo stato di file non vengono mantenuti nel comando.
4. Modificare le impostazioni dei criteri di reindirizzamento cartelle, aggiornare il percorso della cartella di destinazione per ogni cartella reindirizzata che si desidera spostare. Per altre informazioni, vedere il passaggio 4 del [distribuire reindirizzamento cartelle con i file Offline](deploy-folder-redirection.md).
5. Inviare notifiche agli utenti che è stato modificato il server che ospita le cartelle reindirizzate e che devono usare la **GpUpdate /Target:User /Force** comando e quindi disconnettersi e accedere nuovamente per ottenere la configurazione aggiornata e riprendere la corretta file sincronizzazione.

    Gli utenti devono accedere a tutte le macchine almeno una volta per assicurarsi che i dati ottiene trasferiti correttamente in ogni cache dei file Offline.

## <a name="more-information"></a>Altre informazioni

* [Distribuire reindirizzamento cartelle con i file Offline](deploy-folder-redirection.md)
* [Distribuire profili utente mobili](deploy-roaming-user-profiles.md)
* [Panoramica di reindirizzamento cartelle, file Offline e profili utente mobili](folder-redirection-rup-overview.md)