---
title: Esecuzione della modalità di replica WSUS
description: Argomento di Windows Server Update Service (WSUS)-come configurare la modalità di replica
ms.prod: windows-server
ms.technology: manage-wsus
ms.topic: article
ms.assetid: d218cd6b-3b6b-4429-913b-31d412ce3356
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 0680cba35066d0fb752a714424729eed7f47211a
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80828614"
---
# <a name="running-wsus-replica-mode"></a>Esecuzione della modalità di replica WSUS

>Si applica a: Windows Server (canale semestrale), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Un server WSUS in esecuzione in modalità di replica eredita le approvazioni degli aggiornamenti e i gruppi di computer creati in un server di amministrazione. In uno scenario in cui viene utilizzata la modalità di replica, in genere si dispone di un singolo server di amministrazione e uno o più server WSUS di replica subordinata vengono distribuiti nell'intera organizzazione, in base alla topografia del sito o dell'organizzazione. È possibile approvare gli aggiornamenti e creare gruppi di computer nel server di amministrazione, che verrà quindi rispecchiato dai server in modalità di replica. I server in modalità di replica possono essere configurati solo durante la configurazione di WSUS e, se è stato implementato questo scenario, è probabile che sia importante per l'organizzazione che le approvazioni degli aggiornamenti e i gruppi di computer siano gestiti a livello centralizzato.

Se il server WSUS è in esecuzione in modalità di replica, sarà possibile eseguire solo le funzionalità di amministrazione limitate sul server, che sono costituite principalmente da:

-   Aggiunta e rimozione di computer da gruppi di computer. L'appartenenza a un gruppo di computer non viene distribuita ai server di replica, bensì solo ai gruppi di computer. Pertanto, in un server in modalità di replica, i gruppi di computer creati nel server di amministrazione vengono ereditati. Tuttavia, i gruppi di computer saranno vuoti. È quindi necessario assegnare i computer client che si connettono al server di replica ai gruppi di computer.

-   Impostazione di una pianificazione di sincronizzazione

-   Specifica delle impostazioni del server proxy

-   Specifica dell'origine degli aggiornamenti. Può trattarsi di un server diverso dal server di amministrazione

-   Visualizzazione degli aggiornamenti disponibili

-   Monitoraggio delle impostazioni di aggiornamento, sincronizzazione, stato del computer e WSUS sul server

-   Esecuzione di tutti i report standard di WSUS disponibili sui server in modalità di replica



