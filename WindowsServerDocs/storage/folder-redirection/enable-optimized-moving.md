---
title: Abilitare spostamenti ottimizzati di cartelle reindirizzate
description: Come eseguire uno spostamento ottimizzato di cartelle reindirizzate in una nuova condivisione file.
ms.prod: windows-server
ms.topic: article
author: JasonGerend
ms.author: jgerend
ms.technology: storage
ms.date: 09/10/2018
ms.localizationpriority: medium
ms.openlocfilehash: edf714bc0d6b39dbe7c5e800e953d7820fe9abc5
ms.sourcegitcommit: 3a3d62f938322849f81ee9ec01186b3e7ab90fe0
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 04/23/2020
ms.locfileid: "75352604"
---
# <a name="enable-optimized-moves-of-redirected-folders"></a>Abilitare spostamenti ottimizzati di cartelle reindirizzate

>Si applica a: Windows 10, Windows 8, Windows 8.1, Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server (Canale semestrale)

In questo argomento viene descritto come eseguire uno spostamento ottimizzato di cartelle reindirizzate (Reindirizzamento cartelle) in una nuova condivisione file. Se abiliti questa impostazione criterio, quando un amministratore sposta la condivisione file che ospita le cartelle reindirizzate e aggiorna il percorso di destinazione delle cartelle reindirizzate in Criteri di gruppo, il contenuto memorizzato nella cache viene semplicemente rinominato nella cache File offline locale senza ritardi o potenziale perdita di dati per l'utente.

In precedenza, gli amministratori potevano modificare in Criteri di gruppo il percorso di destinazione delle cartelle reindirizzate e consentire ai computer client di copiare i file al successivo accesso dell'utente interessato, causando un ritardo nell'accesso. In alternativa, gli amministratori potevano usare Criteri di gruppo per spostare la condivisione file e aggiornare il percorso di destinazione delle cartelle reindirizzate. Tuttavia, tutte le modifiche apportate localmente nei computer client tra l'inizio dello spostamento e la prima sincronizzazione dopo lo spostamento sarebbero andate perse.

## <a name="prerequisites"></a>Prerequisiti

Per uno spostamento ottimizzato devono essere soddisfatti i requisiti seguenti:

- Deve essere configurata la funzionalità Reindirizzamento cartelle. Per altre informazioni, vedi [Distribuire Reindirizzamento cartelle con File offline](deploy-folder-redirection.md).
- I computer client devono eseguire Windows 10, Windows 8.1, Windows 8, Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012 o Windows Server (Canale semestrale).

## <a name="step-1-enable-optimized-move-in-group-policy"></a>Passaggio 1: Abilitare lo spostamento ottimizzato in Criteri di gruppo

Per ottimizzare la rilocazione dei dati di Reindirizzamento cartelle, usa Criteri di gruppo per abilitare l'impostazione **Abilita spostamento ottimizzato del contenuto nella cache dei file offline in caso di modifica del percorso del server di reindirizzamento cartelle** per l'oggetto Criteri di gruppo appropriato. Se configuri questa impostazione criterio su **Disabilitato** o **Non configurato**, il client copia tutto il contenuto di Reindirizzamento cartelle nel nuovo percorso e quindi lo elimina dal percorso precedente se il percorso del server viene modificato.

Di seguito viene illustrato come abilitare lo spostamento ottimizzato delle cartelle reindirizzate:

1. In Gestione Criteri di gruppo fai clic con il pulsante destro del mouse sull'oggetto Criteri di gruppo creato per le impostazioni di Reindirizzamento cartelle (ad esempio **Reindirizzamento cartelle e Profili utente mobili**) e quindi scegli **Modifica**.
2. In **Configurazione utente** passa a **Criteri**, **Modelli amministrativi**, **Sistema** e quindi **Reindirizzamento cartelle**.
3. Fai clic con il pulsante destro del mouse su **Abilita spostamento ottimizzato del contenuto nella cache dei file offline in caso di modifica del percorso del server di reindirizzamento cartelle** e quindi scegli **Modifica**.
4. Seleziona **Abilitato** e quindi fai clic su **OK**.

## <a name="step-2-relocate-the-file-share-for-redirected-folders"></a>Passaggio 2: Rilocare la condivisione file per le cartelle reindirizzate

Quando sposti la condivisione file che contiene le cartelle reindirizzate degli utenti, è importante prendere precauzioni in modo che le cartelle vengano rilocate correttamente.

>[!IMPORTANT]
>Se i file degli utenti sono in uso o se lo stato completo dei file non viene mantenuto nello spostamento, è possibile che gli utenti riscontrino prestazioni ridotte durante la copia dei file in rete, conflitti di sincronizzazione generati da File offline o anche perdita di dati.

1. Informa gli utenti in anticipo che il server che ospita le cartelle reindirizzate verrà modificato e consiglia di eseguire queste operazioni:

      - Sincronizzare il contenuto della cache File offline e risolvere eventuali conflitti.
      - Aprire un prompt dei comandi con privilegi elevati, immettere **GpUpdate /Target:User /Force** e quindi disconnettersi e accedere di nuovo per assicurarsi che vengano applicate al computer client le impostazioni di Criteri di gruppo più recenti.

        >[!NOTE]
        >Per impostazione predefinita, i computer client aggiornano Criteri di gruppo ogni 90 minuti, quindi se concedi un tempo sufficiente per la ricezione dei criteri aggiornati da parte dei computer client, non dovrai chiedere agli utenti di usare **GpUpdate**.
2. Rimuovi la condivisione file dal server per garantire che non vi siano file in uso nella condivisione file. A tale scopo, in Server Manager, nella pagina **Condivisioni** di Servizi file e archiviazione fai clic con il pulsante destro del mouse sulla condivisione file appropriata e quindi scegli **Rimuovi**.

    Gli utenti lavoreranno offline usando File offline fino al completamento dello spostamento e riceveranno da Criteri di gruppo le impostazioni di Reindirizzamento cartelle aggiornate.

3. Usando un account con privilegi di backup, sposta il contenuto della condivisione file nel nuovo percorso usando un metodo che mantenga i timestamp dei file, ad esempio un'utilità di backup e ripristino. Per usare il comando **Robocopy**, apri un prompt dei comandi con privilegi elevati e quindi digita il comando seguente, dove ```<Source>``` è il percorso corrente della condivisione file e ```<Destination>``` è il nuovo percorso:

    ```PowerShell
    Robocopy /B <Source> <Destination> /Copyall /MIR /EFSRAW
    ```

    >[!NOTE]
    >Il comando **Xcopy** non mantiene lo stato completo dei file.
4. Modifica le impostazioni criterio di Reindirizzamento cartelle, aggiornando il percorso delle cartelle di destinazione per ogni cartella reindirizzata che vuoi rilocare. Per altre informazioni, vedi il passaggio 4 in [Distribuire Reindirizzamento cartelle con File offline](deploy-folder-redirection.md).
5. Informa gli utenti che il server che ospita le cartelle reindirizzate è stato modificato e che devono usare il comando **GpUpdate /Target:User /Force** e quindi disconnettersi e accedere di nuovo per ottenere la configurazione aggiornata e riprendere la sincronizzazione dei file corretta.

    Gli utenti devono accedere a tutti i computer almeno una volta per assicurarsi che i dati vengano rilocati correttamente in ogni cache File offline.

## <a name="more-information"></a>Altre informazioni

* [Distribuire Reindirizzamento cartelle con File offline](deploy-folder-redirection.md)
* [Distribuire profili utente mobili](deploy-roaming-user-profiles.md)
* [Panoramica di Reindirizzamento cartelle, File offline e Profili utente mobili](folder-redirection-rup-overview.md)