---
title: Abilitare spostamenti ottimizzati delle cartelle reindirizzate
description: Come eseguire un passaggio ottimizzato delle cartelle reindirizzate a una nuova condivisione di file.
ms.prod: windows-server-threshold
ms.topic: article
author: JasonGerend
ms.author: jgerend
ms.technology: storage
ms.date: 09/10/2018
ms.localizationpriority: medium
ms.openlocfilehash: 98fd5d50645ad454204dcf9dabf58e97c246ab1a
ms.sourcegitcommit: 505505b7ec99506e7ff50eddbdd6aa94a602abe6
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/11/2018
ms.locfileid: "3831381"
---
# Abilitare spostamenti ottimizzati delle cartelle reindirizzate

>Si applica a: Windows 10, Windows 8, Windows 8.1, Windows Server 2012, Windows Server 2012 R2, Windows Server 2016

Questo argomento descrive come eseguire un passaggio ottimizzato delle cartelle reindirizzate (reindirizzamento) a una nuova condivisione di file. Se si abilita questa impostazione di criteri, quando un amministratore si sposta la condivisione di file che ospita le cartelle reindirizzate e aggiorna il percorso di destinazione delle cartelle reindirizzate in Criteri di gruppo, il contenuto memorizzato nella cache viene rinominato semplicemente nella cache locale i file Offline senza ritardi o potenziale perdita di dati per l'utente.

In precedenza, gli amministratori modificare il percorso di destinazione delle cartelle reindirizzate in Criteri di gruppo e Consenti ai che computer client copiare i file all'accesso successivo dell'utente interessati, può causare una ritardo di accesso. In alternativa, gli amministratori potrebbero spostare la condivisione di file e aggiorna il percorso di destinazione delle cartelle reindirizzate in Criteri di gruppo. Tuttavia, le modifiche apportate in locale nei computer client tra l'inizio di spostamento e la sincronizzazione prima dopo lo spostamento sarebbe perse.

## Prerequisiti

Spostamento ottimizzato presenta i requisiti seguenti:

- Il reindirizzamento delle cartelle deve essere il programma di installazione. Per altre informazioni, vedi [Distribuire cartelle di reindirizzamento cartelle con i file Offline](deploy-folder-redirection.md).
- I computer client devono eseguire Windows 10, Windows 8.1, Windows 8, Windows Server 2016, Windows Server 2012 R2 o Windows Server 2012.

## Passaggio 1: Abilita ottimizzato movimento in Criteri di gruppo

Per ottimizzare il trasferimento dei dati di cartelle di reindirizzamento cartelle, utilizzare criteri di gruppo per abilitare l'impostazione dei criteri **Abilita ottimizzato sposta il contenuto nella cache i file Offline in Modifica percorso server di cartelle di reindirizzamento cartelle** per oggetto Criteri di gruppo (GPO) appropriato. Configurare questa impostazione dei criteri **disabilitata** o **Non configurata** fa sì che il client per copiare tutti i contenuti di reindirizzamento nella nuova posizione e quindi Elimina il contenuto dalla posizione precedente se cambia il percorso del server.

Ecco come abilitare lo spostamento ottimizzato delle cartelle reindirizzate:

1. In Gestione criteri di gruppo, fai doppio clic l'oggetto Criteri di gruppo, creare per le impostazioni di cartelle di reindirizzamento cartelle (ad esempio, **reindirizzamento cartelle e il Roaming delle impostazioni di profili utente**) e quindi seleziona **Modifica**.
2. **Configurazione utente**, passa a **criteri**, **Modelli amministrativi**, quindi **sistema**e quindi **Il reindirizzamento delle cartelle**.
3. Fai **che abilita ottimizzato sposta il contenuto nella cache i file Offline in Modifica percorso server di cartelle di reindirizzamento cartelle**e quindi seleziona **Modifica**.
4. Seleziona **abilitato**e quindi seleziona **OK**.

## Passaggio 2: Rilocare la condivisione file per le cartelle reindirizzate

Quando lo spostamento alla condivisione di file che contiene degli utenti cartelle reindirizzate, è importazione prendere precauzioni per garantire che le cartelle vengono rilocate correttamente.

>[!IMPORTANT]
>Se i file degli utenti sono in uso o se lo stato completo del file non viene conservato nello spostamento, gli utenti potrebbero riscontrare prestazioni scarse come i file vengono copiati in rete, i conflitti di sincronizzazione generati dal file Offline o persino la perdita di dati.

1. Informare gli utenti in anticipo che il server che ospita le cartelle reindirizzate verrà modificato e consigliamo di eseguire le operazioni seguenti:

      - Sincronizzare il contenuto della cache file Offline e risolvere i conflitti.
      - Apri un prompt dei comandi con privilegi elevati, Immetti **/target: User GpUpdate /Force**e quindi disconnettiti e accedi nuovamente per garantire che le impostazioni di criteri di gruppo più recenti vengano applicate al computer client

        >[!NOTE]
        >Per impostazione predefinita, i computer client aggiornano criteri di gruppo ogni 90 minuti, quindi se Consenti tempo sufficiente per client computer ricevano criterio aggiornato, non dovrai chiedere agli utenti di usare **GpUpdate**.
2. Rimuovere la condivisione file dal server per garantire che i file nella condivisione di file non sono in uso. A tale scopo in Server Manager, nella pagina **condivisioni** di File e i servizi di archiviazione, pulsante destro del mouse nella condivisione file appropriato e quindi seleziona **rimuovere**.

    Gli utenti funzionerà offline usando i file Offline fino a quando lo spostamento è stato completato e ricevono le impostazioni di reindirizzamento aggiornate da criteri di gruppo.

3. Con un account con privilegi di backup, spostare il contenuto della condivisione di file nella nuova posizione con un metodo che mantiene il timestamp di file, ad esempio un backup e ripristino utilità. Per usare i comandi di **Robocopy** , Apri un prompt dei comandi con privilegi elevati e quindi digitare il comando seguente, dove ```<Source>``` è la posizione corrente della condivisione di file, e ```<Destination>``` è la nuova posizione:

    ```PowerShell
    Robocopy /B <Source> <Destination> /Copyall /MIR /EFSRAW
    ```

    >[!NOTE]
    >Il comando **Xcopy** senza mantenere tutte dello stato del file.
4. Modificare le impostazioni di criteri di reindirizzamento cartelle, aggiornando il percorso della cartella di destinazione per ogni cartella reindirizzata che si desidera spostare. Per altre informazioni, vedi il passaggio 4 di [Distribuire cartelle di reindirizzamento cartelle con i file Offline](deploy-folder-redirection.md).
5. Informare gli utenti che il server che ospita le cartelle reindirizzate è cambiato e che dovranno utilizzare il comando **/target: User GpUpdate /Force** e quindi disconnettersi e di nuovo l'accesso per ottenere la configurazione aggiornata e riprendere la sincronizzazione di file appropriato.

    Gli utenti devono accedano a tutti i computer almeno una volta garantire che i dati ottiene rilocati correttamente nella cache ogni file Offline.

## Ulteriori informazioni

* [Distribuire cartelle di reindirizzamento cartelle con i file Offline](deploy-folder-redirection.md)
* [Distribuire i profili utente mobili](deploy-roaming-user-profiles.md)
* [Panoramica di cartelle di reindirizzamento cartelle, file Offline e profili utente mobili](folder-redirection-rup-overview.md)