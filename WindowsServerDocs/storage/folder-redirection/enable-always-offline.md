---
title: Abilitare la modalità always offline per accedere più rapidamente ai file
description: Come utilizzare la modalità always offline di File offline per offrire un accesso più rapido ai file memorizzati nella cache e alle cartelle reindirizzate.
ms.prod: windows-server
ms.topic: article
author: JasonGerend
ms.author: jgerend
ms.technology: storage
ms.date: 09/10/2018
ms.localizationpriority: medium
ms.openlocfilehash: 389fdd26a7e1d9824f1eaf0136a544547f08eb05
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71401957"
---
# <a name="enable-always-offline-mode-for-faster-access-to-files"></a>Abilitare la modalità always offline per accedere più rapidamente ai file

>Si applica a: Windows 10, Windows 8, Windows 8.1, Windows Server 2019, Windows Server 2016, Windows Server 2012, Windows Server 2012 R2 e Windows (canale semestrale)

Questo documento descrive come usare la modalità always offline di File offline per offrire un accesso più rapido ai file memorizzati nella cache e alle cartelle reindirizzate. Always offline fornisce anche una minore utilizzo della larghezza di banda, perché gli utenti lavorano sempre offline anche quando sono connessi tramite una connessione di rete ad alta velocità.

## <a name="prerequisites"></a>Prerequisiti

Per abilitare la modalità always offline, l'ambiente deve soddisfare i prerequisiti seguenti.

- Un dominio Active Directory Domain Services (AD DS) con computer client aggiunti al dominio. Non sono previsti requisiti a livello di funzionalità o di foresta o requisiti di schema.
- Computer client che eseguono Windows 10, Windows 8.1, Windows 8, Windows Server 2016, Windows Server 2012 R2 o Windows Server 2012. (I computer client che eseguono versioni precedenti di Windows potrebbero continuare a passare alla modalità online su connessioni di rete ad alta velocità.)
- Un computer in cui è installata la gestione Criteri di gruppo.

## <a name="enable-always-offline-mode"></a>Abilitare la modalità always offline

Per abilitare la modalità always offline, usare Criteri di gruppo per abilitare l'impostazione dei criteri **Configura modalità di collegamento lento** e impostare la latenza su **1** (millisecondo). Questa operazione fa sì che i computer client che eseguono Windows 8 o Windows Server 2012 usino automaticamente la modalità sempre offline.

>[!NOTE]
>I computer che eseguono Windows 7, Windows Vista, Windows Server 2008 R2 o Windows Server 2008 potrebbero continuare a passare alla modalità online se la latenza della connessione di rete scende al di sotto di un millisecondo.

1. Aprire **gestione criteri di gruppo**.
2. Per creare facoltativamente un nuovo oggetto Criteri di gruppo (GPO) per File offline le impostazioni, fare clic con il pulsante destro del mouse sul dominio o sull'unità organizzativa (OU) appropriato, quindi selezionare **Crea un oggetto Criteri di gruppo in questo dominio e collegarlo qui**.
3. Nell'albero della console fare clic con il pulsante destro del mouse sull'oggetto Criteri di gruppo per il quale si desidera configurare le impostazioni di File offline, quindi scegliere **modifica**. Viene visualizzato il **Editor gestione criteri di gruppo** .
4. Nell'albero della console, in **Configurazione computer**, espandere **criteri**, espandere **modelli amministrativi**, espandere **rete**ed espandere **file offline**.
5. Fare clic con il pulsante destro del mouse su **Configura modalità di collegamento lento**, quindi scegliere **modifica**. Verrà visualizzata la finestra **Configura modalità di collegamento lento** .
6. Seleziona **Attivata**.
7. Nella casella **Opzioni** selezionare **Mostra**. Verrà visualizzata la **finestra Mostra contenuti** .
8. Nella casella **nome valore** specificare la condivisione file per la quale si desidera abilitare la modalità always offline.
9. Per abilitare la modalità always offline in tutte le condivisioni file, immettere **\*** .
10. Nella casella **valore** immettere **latenza = 1** per impostare la soglia di latenza su un millisecondo e quindi fare clic su **OK**.

>[!NOTE]
>Per impostazione predefinita, quando si usa la modalità always offline, Windows sincronizza i file nella cache File offline in background ogni due ore. Per modificare questo valore, usare l'impostazione di criteri **configura sincronizzazione in background** .

## <a name="more-information"></a>Altre informazioni

* [Panoramica di Reindirizzamento cartelle, File offline e profili utente mobili](folder-redirection-rup-overview.md)
* [Distribuire il reindirizzamento cartelle con File offline](deploy-folder-redirection.md)