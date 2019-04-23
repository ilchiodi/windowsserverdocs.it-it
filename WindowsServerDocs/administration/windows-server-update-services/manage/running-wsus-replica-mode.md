---
title: Esecuzione della modalità di replica WSUS
description: 'Argomento di Windows Server Update Service (WSUS) - procedura configurare la modalità di Replica '
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-wsus
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: d218cd6b-3b6b-4429-913b-31d412ce3356
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 3b4139354a3f0f7b1f1a97107d2f6b28db2b02c2
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59878742"
---
# <a name="running-wsus-replica-mode"></a>Esecuzione della modalità di replica WSUS

>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Un server WSUS in esecuzione in modalità di replica eredita le approvazioni degli aggiornamenti e gruppi di computer creati in un server di amministrazione. In uno scenario che usa la modalità di replica, in genere necessario un unico server di amministrazione e uno o più server WSUS di replica secondari dislocati in tutta l'organizzazione, basato su sito o topografia dell'organizzazione. Approvare gli aggiornamenti e creare gruppi di computer nel server di amministrazione, i server in modalità di replica verranno successivamente eseguito il mirroring. I server in modalità di replica possono essere impostati solo durante l'installazione di WSUS e se è implementato in questo scenario, probabilmente è perché è importante nell'organizzazione che approvazioni di aggiornamenti e i gruppi di computer gestiti in modo centralizzato.

Se il server WSUS è in esecuzione in modalità di replica, sarà in grado di eseguire solo le funzionalità di amministrazione limitata nel server, che sarà principalmente costituito da:

-   Aggiunta e rimozione di computer in gruppi di computer. L'appartenenza al gruppo di computer non viene distribuito ai server di replica, solo i computer dei gruppi stessi. Pertanto, in un server in modalità di replica, si ereditano i gruppi di computer creato nel server di amministrazione. Tuttavia, i gruppi di computer sarà vuoti. È quindi necessario assegnare il client di computer che si connettono al server di replica per i gruppi di computer.

-   Impostazione di una pianificazione di sincronizzazione

-   Specificare le impostazioni server proxy

-   Specifica l'origine degli aggiornamenti. Può trattarsi di un server diverso dal server di amministrazione

-   Visualizzazione degli aggiornamenti disponibili

-   Aggiornamento monitoraggio, la sincronizzazione, stato del computer e le impostazioni di WSUS nel server

-   In esecuzione WSUS standard tutti i report disponibili nel server in modalità di replica



