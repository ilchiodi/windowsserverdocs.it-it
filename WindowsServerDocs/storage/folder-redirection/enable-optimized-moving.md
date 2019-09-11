---
title: Abilita spostamento ottimizzato di cartelle reindirizzate
description: Come eseguire uno spostamento ottimizzato delle cartelle reindirizzate in una nuova condivisione file.
ms.prod: windows-server-threshold
ms.topic: article
author: JasonGerend
ms.author: jgerend
ms.technology: storage
ms.date: 09/10/2018
ms.localizationpriority: medium
ms.openlocfilehash: edf6596f7daaa2f496b8b4da36e98ee72b05dfcd
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/10/2019
ms.locfileid: "70867254"
---
# <a name="enable-optimized-moves-of-redirected-folders"></a>Abilita spostamento ottimizzato di cartelle reindirizzate

>Si applica a Windows 10, Windows 8, Windows 8.1, Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server (canale semestrale)

In questo argomento viene descritto come eseguire uno spostamento ottimizzato di cartelle reindirizzate (Reindirizzamento cartelle) in una nuova condivisione file. Se si abilita questa impostazione di criteri, quando un amministratore sposta la condivisione file che ospita le cartelle reindirizzate e aggiorna il percorso di destinazione delle cartelle reindirizzate in Criteri di gruppo, il contenuto memorizzato nella cache viene semplicemente rinominato nella cache File offline locale senza ritardi o potenziale perdita di dati per l'utente.

In precedenza, gli amministratori potevano modificare il percorso di destinazione delle cartelle reindirizzate in Criteri di gruppo e consentire ai computer client di copiare i file al successivo accesso dell'utente interessato, causando un accesso ritardato. In alternativa, gli amministratori possono spostare la condivisione file e aggiornare il percorso di destinazione delle cartelle reindirizzate in Criteri di gruppo. Tuttavia, tutte le modifiche apportate localmente sui computer client tra l'inizio dello spostamento e la prima sincronizzazione dopo lo spostamento andranno perse.

## <a name="prerequisites"></a>Prerequisiti

Lo spostamento ottimizzato prevede i requisiti seguenti:

- Il reindirizzamento cartelle deve essere configurato. Per ulteriori informazioni, vedere la pagina relativa alla [distribuzione del reindirizzamento cartelle con file offline](deploy-folder-redirection.md).
- I computer client devono eseguire Windows 10, Windows 8.1, Windows 8, Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012 o Windows Server (canale semestrale).

## <a name="step-1-enable-optimized-move-in-group-policy"></a>Passaggio 1: Abilita spostamento ottimizzato in Criteri di gruppo

Per ottimizzare la rilocazione dei dati di Reindirizzamento cartelle, utilizzare Criteri di gruppo per abilitare l'impostazione dei criteri di **modifica del percorso del server** per il reindirizzamento delle cartelle per l'oggetto Criteri di gruppo appropriato (GPO) nella cache file offline. La configurazione di questa impostazione di criteri su **disabilitato** o **non configurato** fa in modo che il client copi tutto il contenuto di Reindirizzamento cartelle nel nuovo percorso e quindi elimini il contenuto dal percorso precedente se il percorso del server viene modificato.

Di seguito viene illustrato come abilitare lo stato di trasferimento ottimizzato delle cartelle reindirizzate:

1. In Gestione Criteri di gruppo fare clic con il pulsante destro del mouse sull'oggetto Criteri di gruppo creato per le impostazioni di Reindirizzamento cartelle (ad esempio, **Reindirizzamento cartelle e impostazioni profili utente mobili**) e quindi selezionare **modifica**.
2. In **Configurazione utente**passare a **criteri**, quindi **modelli amministrativi**, **sistema**, quindi **Reindirizzamento cartelle**.
3. Fare clic con **il pulsante destro del mouse su Abilita spostamento ottimizzato del contenuto nella cache file offline in modifica percorso server Reindirizzamento cartelle**, quindi scegliere **modifica**.
4. Selezionare **abilitato**e quindi fare clic su **OK**.

## <a name="step-2-relocate-the-file-share-for-redirected-folders"></a>Passaggio 2: Spostare la condivisione file per le cartelle reindirizzate

Quando si trasferisce la condivisione file che contiene le cartelle reindirizzate degli utenti, viene importato per adottare le precauzioni necessarie per assicurarsi che le cartelle vengano rilocate correttamente.

>[!IMPORTANT]
>Se i file degli utenti sono in uso o se lo stato completo del file non viene mantenuto nello spostamento, gli utenti potrebbero subire prestazioni ridotte durante la copia dei file in rete, i conflitti di sincronizzazione generati da File offline o persino la perdita di dati.

1. Informare gli utenti in anticipo che il server che ospita le cartelle reindirizzate cambierà e consiglierà di eseguire le azioni seguenti:

      - Sincronizzare il contenuto della cache File offline e risolvere eventuali conflitti.
      - Aprire un prompt dei comandi con privilegi elevati, immettere **gpupdate/target: User/Force**e quindi disconnettersi e accedere di nuovo per assicurarsi che le impostazioni di criteri di gruppo più recenti vengano applicate al computer client.

        >[!NOTE]
        >Per impostazione predefinita, i computer client aggiornano Criteri di gruppo ogni 90 minuti. Pertanto, se si concede un tempo sufficiente per la ricezione dei criteri aggiornati da parte dei computer client, non è necessario chiedere agli utenti di usare **gpupdate**.
2. Rimuovere la condivisione file dal server per assicurarsi che non siano in uso file nella condivisione file. A tale scopo, in Server Manager, nella pagina **condivisioni** di servizi file e archiviazione, fare clic con il pulsante destro del mouse sulla condivisione file appropriata, quindi scegliere **Rimuovi**.

    Gli utenti funzioneranno offline usando File offline fino al completamento dello spostamento e riceveranno le impostazioni di Reindirizzamento cartelle aggiornate da Criteri di gruppo.

3. Usando un account con privilegi di backup, spostare il contenuto della condivisione file nel nuovo percorso usando un metodo che conserva i timestamp dei file, ad esempio un'utilità di backup e ripristino. Per utilizzare il comando **Robocopy** , aprire un prompt dei comandi con privilegi elevati, quindi digitare il comando seguente ```<Source>``` , dove è il percorso corrente della condivisione file e ```<Destination>``` è il nuovo percorso:

    ```PowerShell
    Robocopy /B <Source> <Destination> /Copyall /MIR /EFSRAW
    ```

    >[!NOTE]
    >Il comando **xcopy** non mantiene tutto lo stato del file.
4. Modificare le impostazioni dei criteri di Reindirizzamento cartelle, aggiornando il percorso della cartella di destinazione per ogni cartella reindirizzata che si desidera spostare. Per ulteriori informazioni, vedere il passaggio 4 relativo alla [distribuzione del reindirizzamento cartelle con file offline](deploy-folder-redirection.md).
5. Notificare agli utenti che il server che ospita le cartelle reindirizzate è stato modificato e che è necessario usare il comando **gpupdate/target: User/Force** e quindi disconnettersi e accedere di nuovo per ottenere la configurazione aggiornata e riprendere la sincronizzazione dei file corretta.

    Gli utenti devono accedere a tutti i computer almeno una volta per assicurarsi che i dati vengano spostati correttamente in ogni cache File offline.

## <a name="more-information"></a>Altre informazioni

* [Distribuire il reindirizzamento cartelle con File offline](deploy-folder-redirection.md)
* [Distribuire profili utente mobili](deploy-roaming-user-profiles.md)
* [Panoramica di Reindirizzamento cartelle, File offline e profili utente mobili](folder-redirection-rup-overview.md)