---
title: Abilitare la modalità sempre Offline per un accesso più rapido ai file
description: Come usare la modalità sempre Offline di file Offline per fornire un accesso più rapido per i file memorizzati nella cache e le cartelle reindirizzate.
ms.prod: windows-server-threshold
ms.topic: article
author: JasonGerend
ms.author: jgerend
ms.technology: storage
ms.date: 09/10/2018
ms.localizationpriority: medium
ms.openlocfilehash: bc54b1e33d09e7f2b9eea01e4f09fb83f13dc1af
ms.sourcegitcommit: 505505b7ec99506e7ff50eddbdd6aa94a602abe6
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/11/2018
ms.locfileid: "3831391"
---
# Abilitare la modalità sempre Offline per un accesso più rapido ai file

>Si applica a: Windows 10, Windows 8, Windows 8.1, Windows Server 2012, Windows Server 2012 R2, Windows Server 2016

Questo documento descrive come usare la modalità sempre Offline di file Offline per fornire un accesso più rapido per i file memorizzati nella cache e le cartelle reindirizzate. Sempre Offline fornisce inoltre minore utilizzo della larghezza di banda perché gli utenti sono sempre la modalità offline, anche quando si sono connessi tramite una connessione di rete ad alta velocità.

## Prerequisiti

Per abilitare la modalità sempre Offline, l'ambiente deve soddisfare i prerequisiti seguenti.

- Un dominio di Active Directory Domain Services (AD DS) con i computer client aggiunto al dominio. Non siano presenti i requisiti di livello funzionale di foresta o un dominio o requisiti dello schema.
- Computer client che eseguono Windows 10, Windows 8.1, Windows 8, Windows Server 2016, Windows Server 2012 R2 o Windows Server 2012. (I computer client che eseguono versioni precedenti di Windows potrebbero continuare a transizione alla modalità Online per le connessioni di rete ad alta velocità molto).
- Un computer con installato Gestione criteri di gruppo.

## Abilitare la modalità sempre Offline

Per abilitare la modalità sempre Offline, utilizzare criteri di gruppo per abilitare l'impostazione di criteri di **modalità collegamento lento di configurare** e impostare la latenza su **1** (millisecondi). In questo modo fa sì che i computer client che esegue Windows 8 o Windows Server 2012 utilizzare automaticamente le modalità sempre Offline.

>[!NOTE]
>I computer che eseguono Windows 7, Windows Vista, Windows Server 2008 R2 o Windows Server 2008 potrebbe continuare a transizione alla modalità Online se la latenza della connessione di rete di sotto un millisecondo.

1. Apri **Gestione criteri di gruppo**.
2. Per creare facoltativamente un nuovo oggetto Criteri (gruppo) per le impostazioni di file Offline, destro il dominio appropriato o unità organizzativa (UO) e quindi seleziona **Crea un oggetto Criteri di gruppo in questo dominio e collegarlo qui**.
3. Nell'albero della console fai l'oggetto Criteri di gruppo per cui si desidera configurare le impostazioni di file Offline e quindi seleziona **Modifica**. Viene visualizzata l' **Editor Gestione criteri di gruppo** .
4. Nell'albero della console, in **Configurazione Computer**, espandere **i criteri**, modelli **Amministrativi**, espandere **rete**e **I file Offline**.
5. Fai **modalità collegamento lento di configurare**e quindi seleziona **Modifica**. Verrà visualizzata la finestra di **configurare la modalità collegamento lento** .
6. Seleziona **Attivata**.
7. Nella finestra di **Opzioni** , selezionare **Mostra**. Verrà visualizzata la **finestra di visualizzazione contenuto** .
8. Nella casella **nome del valore** di specificare la condivisione di file per cui vuoi abilitare la modalità sempre Offline.
9. Per abilitare la modalità sempre Offline in tutte le condivisioni di file, immettere **\***.
10. Nella finestra di **valore** , immettere **latenza = 1** per impostare la soglia di latenza a un millisecondo e quindi seleziona **OK**.

>[!NOTE]
>Per impostazione predefinita, quando è in modalità sempre Offline, Windows consente di sincronizzare i file nella cache i file Offline in background ogni due ore. Per modificare questo valore, Usa l'impostazione di criteri di **Configurare la sincronizzazione in Background** .

## Ulteriori informazioni

* [Panoramica di cartelle di reindirizzamento cartelle, file Offline e profili utente mobili](folder-redirection-rup-overview.md)
* [Distribuire cartelle di reindirizzamento cartelle con i file Offline](deploy-folder-redirection.md)