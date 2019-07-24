---
title: Abilitare la modalità sempre Offline per un accesso più rapido ai file
description: Come usare la modalità sempre Offline dei file Offline per fornire accesso più rapido ai file memorizzati nella cache e le cartelle reindirizzate.
ms.prod: windows-server-threshold
ms.topic: article
author: JasonGerend
ms.author: jgerend
ms.technology: storage
ms.date: 09/10/2018
ms.localizationpriority: medium
ms.openlocfilehash: ddf6a816e417c2eddff090df8dba841a894a3255
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/31/2019
ms.locfileid: "66447669"
---
# <a name="enable-always-offline-mode-for-faster-access-to-files"></a>Abilitare la modalità sempre Offline per un accesso più rapido ai file

>Si applica a: Windows 10, Windows 8, Windows 8.1, Windows Server 2019, Windows Server 2016, Windows Server 2012, Windows Server 2012 R2 e Windows (canale semestrale)

Questo documento descrive come usare la modalità sempre Offline dei file Offline per fornire accesso più rapido ai file memorizzati nella cache e le cartelle reindirizzate. Sempre Offline offre anche minore utilizzo della larghezza di banda perché gli utenti sempre attiva la modalità offline, anche quando sono connessi tramite una connessione di rete ad alta velocità.

## <a name="prerequisites"></a>Prerequisiti

Per abilitare la modalità sempre Offline, l'ambiente deve soddisfare i prerequisiti seguenti.

- Un dominio di Active Directory Domain Services (AD DS) con i computer client aggiunti al dominio. Non esistono requisiti a livello funzionale di foresta o dominio o requisiti dello schema.
- Computer client che eseguono Windows 10, Windows 8.1, Windows 8, Windows Server 2016, Windows Server 2012 R2 o Windows Server 2012. (Computer client che eseguono versioni precedenti di Windows potrebbe continuare per eseguire la transizione alla modalità Online per le connessioni di rete ad alta velocità molto).
- Un computer di gestione di criteri di gruppo installata.

## <a name="enable-always-offline-mode"></a>Abilitare la modalità sempre Offline

Per abilitare la modalità sempre Offline, usare criteri di gruppo per abilitare la **Configura modalità di collegamento lento** criteri di impostazione e impostare la latenza **1** (in millisecondi). In questo modo, i computer client che eseguono Windows 8 o Windows Server 2012 per usare automaticamente la modalità sempre Offline.

>[!NOTE]
>Computer che eseguono Windows 7, Windows Vista, Windows Server 2008 R2 o Windows Server 2008 potrebbe continuare per eseguire la transizione alla modalità Online se la latenza della connessione di rete scende di sotto di un millisecondo.

1. Aprire **Gestione criteri di gruppo**.
2. Per creare, facoltativamente, un nuovo oggetto (criteri di gruppo) per le impostazioni di file Offline, il dominio appropriato o unità organizzativa (OU) e quindi scegliere **crea un oggetto Criteri di gruppo in questo dominio e crea qui un collegamento**.
3. Nell'albero della console, fare doppio clic su oggetto Criteri di gruppo per cui si desidera configurare le impostazioni di file Offline e quindi selezionare **modifica**. Il **Editor Gestione criteri di gruppo** viene visualizzata.
4. Nell'albero della console, sotto **configurazione Computer**, espandere **criteri**, espandere **modelli amministrativi**, espandere **rete**, Espandere **file Offline**.
5. Fare doppio clic su **Configura modalità di collegamento lento**, quindi selezionare **modificare**. Il **Configura modalità di collegamento ridotta** verrà visualizzata la finestra.
6. Seleziona **Attivata**.
7. Nel **le opzioni** , quindi selezionare **mostrano**. Il **finestra Mostra contenuti** verranno visualizzati.
8. Nel **nome del valore** , specificare la condivisione di file per cui si desidera abilitare la modalità sempre Offline.
9. Per abilitare la modalità sempre Offline in tutte le condivisioni di file, immettere **\\** *.
10. Nel **valore** casella, immettere **latenza ad=1** per impostare la soglia di latenza a un millisecondo e quindi selezionare **OK**.

>[!NOTE]
>Per impostazione predefinita, quando è in modalità sempre Offline, Windows sincronizza i file nella cache dei file Offline in background ogni due ore. Per modificare questo valore, usare il **Configura sincronizzazione in Background** impostazione dei criteri.

## <a name="more-information"></a>Altre informazioni

* [Panoramica di reindirizzamento cartelle, file Offline e profili utente mobili](folder-redirection-rup-overview.md)
* [Distribuire reindirizzamento cartelle con i file Offline](deploy-folder-redirection.md)