---
title: Passaggi per la migrazione di servizi MultiPoint
description: Illustra i passaggi per eseguire la migrazione ai servizi MultiPoint in Windows Server 2016
ms.custom: na
ms.date: 07/29/2016
ms.prod: windows-server-threshold
ms.technology: multipoint-services
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 3ee77efa-7cc5-4ddf-aaff-b5634a717014
author: lizap
manager: dongill
ms.author: elizapo
ms.openlocfilehash: b63ef4bf63ce990aa0b0ba7624905ba8f14dde98
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59854812"
---
# <a name="migrate-to--multipoint-services-in-windows-server-2016"></a>Eseguire la migrazione ai servizi MultiPoint in Windows Server 2016

>Si applica a: Windows Server 2016

Usare la procedura seguente e le informazioni che raccolte del foglio di lavoro di pianificazione della migrazione per eseguire la migrazione ai servizi MultiPoint in Windows Server 2016.

## <a name="transfer-server-settings"></a>Trasferire le impostazioni del server
Nel server di destinazione, aprire Gestione MultiPoint. Fare clic su **modificare le impostazioni del server**. Applicare le impostazioni in base al foglio di lavoro Pianificazione della migrazione.

> [!NOTE]
> Se è necessario abilitare la protezione disco nel server di destinazione, attendere fino a quando si configurano i servizi MultiPoint.

## <a name="transfer-station-settings"></a>Trasferire le impostazioni della stazione
Assicurarsi che le stazioni sono collegate al server di destinazione e tutti i mapping prima di applicare le impostazioni della stazione. Le stazioni verranno rilevate automaticamente. Seguire le istruzioni sullo schermo ogni stazione per definire il server mapping delle stazioni di utenti e dispositivi USB collegati. Applicare le impostazioni di stazione preferito, come descritto nel foglio di lavoro Pianificazione della migrazione.

## <a name="migrate-the-vdi-template"></a>Eseguire la migrazione del modello VDI

Prima di importare il modello VDI dal server di origine, tramite Gestione MultiPoint abilitare desktop virtuali nel server di destinazione:

1. Andare alla **desktop virtuali** scheda selezionare Gestione MultiPoint.
2. Fare clic su **abilitato desktop virtuali**. Il server verrà installato il ruolo Hyper-V e quindi riavviare.
3. Aprire Gestione MultiPoint e tornare alla **desktop virtuali**.
4. Fare clic su **modello di desktop virtuale importazione**. Seguire le istruzioni per importare il modello dal server di origine.

> [!NOTE]
> Quando si importa un modello di desktop virtuale, verranno ripristinate le personalizzazioni applicate al modello. 

## <a name="next-step"></a>Passaggio successivo
[Convalidare la nuova distribuzione di MultiPoint Services.](multipoint-services-post-migration-steps.md)