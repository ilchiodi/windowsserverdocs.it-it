---
title: Abilitare la modalità sempre offline per accedere più rapidamente ai file
description: Come usare la modalità sempre offline di File offline per offrire un accesso più rapido ai file memorizzati nella cache e alle cartelle reindirizzate.
ms.prod: windows-server
ms.topic: article
author: JasonGerend
ms.author: jgerend
ms.technology: storage
ms.date: 09/10/2018
ms.localizationpriority: medium
ms.openlocfilehash: 389fdd26a7e1d9824f1eaf0136a544547f08eb05
ms.sourcegitcommit: 3a3d62f938322849f81ee9ec01186b3e7ab90fe0
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 04/23/2020
ms.locfileid: "71401957"
---
# <a name="enable-always-offline-mode-for-faster-access-to-files"></a>Abilitare la modalità sempre offline per accedere più rapidamente ai file

>Si applica a: Windows 10, Windows 8, Windows 8.1, Windows Server 2019, Windows Server 2016, Windows Server 2012, Windows Server 2012 R2 e Windows (canale semestrale)

Questo documento descrive come usare la modalità sempre offline di File offline per offrire un accesso più rapido ai file memorizzati nella cache e alle cartelle reindirizzate. La modalità sempre offline consente anche un minore utilizzo della larghezza di banda in quanto gli utenti sono sempre offline, anche in presenza di una connessione alla rete ad alta velocità.

## <a name="prerequisites"></a>Prerequisiti

Per abilitare la modalità sempre offline, l'ambiente deve soddisfare i prerequisiti seguenti.

- Un dominio Active Directory Domain Services con computer client aggiunti al dominio. Non sono previsti requisiti a livello di funzionalità della foresta o del dominio o requisiti di schema.
- Computer client che eseguono Windows 10, Windows 8.1, Windows 8.1, Windows 8, Windows Server 2016, Windows Server 2012 R2 o Windows Server 2012 (i computer client che eseguono versioni precedenti di Windows potrebbero continuare a passare alla modalità online su connessioni di rete ad alta velocità).
- Un computer in cui è installato lo strumento Gestione Criteri di gruppo.

## <a name="enable-always-offline-mode"></a>Abilitare la modalità sempre offline

Per abilitare la modalità sempre offline, usa Criteri di gruppo per abilitare l'impostazione criterio **Configura modalità collegamento lento** e impostare la latenza su **1** (millisecondi). Con questa operazione i computer client che eseguono Windows 8 o Windows Server 2012 usano automaticamente la modalità sempre offline.

>[!NOTE]
>I computer che eseguono Windows 7, Windows Vista, Windows Server 2008 R2 o Windows Server 2008 potrebbero continuare a passare alla modalità online se la latenza della connessione di rete scende al di sotto di un millisecondo.

1. Apri **Gestione Criteri di gruppo**.
2. Per creare facoltativamente un nuovo oggetto Criteri di gruppo per le impostazioni di File offline, fai clic con il pulsante destro del mouse sul dominio o sull'unità organizzativa appropriata e quindi seleziona **Crea un oggetto Criteri di gruppo in questo dominio e crea qui un collegamento**.
3. Nell'albero della console fai clic con il pulsante destro del mouse sull'oggetto Criteri di gruppo per cui vuoi configurare le impostazioni di File offline e quindi seleziona **Modifica**. Viene visualizzato l'**Editor Gestione Criteri di gruppo**.
4. Nell'albero della console, in **Configurazione computer**, espandi **Criteri**, **Modelli amministrativi**, **Rete** e **File offline**.
5. Fai clic con il pulsante destro del mouse su **Configura modalità collegamento lento** e quindi seleziona **Modifica**. Verrà visualizzata la finestra **Configura modalità collegamento lento**.
6. Seleziona **Attivata**.
7. Nella casella **Opzioni** seleziona **Visualizza**. Verrà visualizzata la finestra **Visualizzazione contenuto**.
8. Nella casella **Nome valore** specifica la condivisione file per la quale vuoi abilitare la modalità sempre offline.
9. Per abilitare la modalità sempre offline in tutte le condivisioni file, immetti **\*** .
10. Nella casella **Valore** immetti **Latenza=1**, per impostare la soglia di latenza su un millisecondo, e quindi seleziona **OK**.

>[!NOTE]
>Per impostazione predefinita, quando è attiva la modalità sempre offline, Windows sincronizza i file nella cache di File offline in background ogni due ore. Per modificare questo valore, usa l'impostazione criterio **Configura sincronizzazione in background**.

## <a name="more-information"></a>Altre informazioni

* [Panoramica di reindirizzamento cartelle, file offline e profili utente mobili](folder-redirection-rup-overview.md)
* [Distribuire il reindirizzamento cartelle con File offline](deploy-folder-redirection.md)