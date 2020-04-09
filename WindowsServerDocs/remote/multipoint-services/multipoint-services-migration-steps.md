---
title: Passaggi per la migrazione di servizi MultiPoint
description: Vengono illustrati i passaggi per eseguire la migrazione a MultiPoint Services in Windows Server 2016
ms.date: 07/29/2016
ms.prod: windows-server
ms.technology: multipoint-services
ms.topic: article
ms.assetid: 3ee77efa-7cc5-4ddf-aaff-b5634a717014
author: lizap
manager: dongill
ms.author: elizapo
ms.openlocfilehash: f2e293fafb8d6f5d84e9ea5a4ad8ef3b7fe2ba7d
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80858694"
---
# <a name="migrate-to--multipoint-services-in-windows-server-2016"></a>Eseguire la migrazione a MultiPoint Services in Windows Server 2016

>Si applica a: Windows Server 2016

Usare la procedura seguente più le informazioni raccolte nel foglio di foglio di pianificazione della migrazione per eseguire la migrazione a MultiPoint Services in Windows Server 2016.

## <a name="transfer-server-settings"></a>Impostazioni del server di trasferimento
Nel server di destinazione aprire Gestione MultiPoint. Fare clic su **Modifica impostazioni server**. Applicare le impostazioni in base al foglio di progettazione per la pianificazione della migrazione.

> [!NOTE]
> Se è necessario abilitare la protezione del disco nel server di destinazione, attendere che siano stati configurati i servizi MultiPoint.

## <a name="transfer-station-settings"></a>Impostazioni della stazione di trasferimento
Verificare che le stazioni siano connesse al server di destinazione e che tutte siano mappate prima di applicare le impostazioni della stazione. Le stazioni verranno rilevate automaticamente. Seguire le istruzioni visualizzate nella schermata di ogni stazione per definire il mapping del server delle stazioni utente e dei dispositivi USB connessi. Applicare le impostazioni della stazione preferita come indicato nel foglio di lavoro per la pianificazione della migrazione.

## <a name="migrate-the-vdi-template"></a>Eseguire la migrazione del modello VDI

Prima di poter importare il modello VDI dal server di origine, abilitare i desktop virtuali nel server di destinazione usando Gestione MultiPoint:

1. Passare alla scheda **desktop virtuali** in Gestione MultiPoint.
2. Fare clic su **desktop virtuali abilitati**. Il server installerà il ruolo Hyper-V e quindi verrà riavviato.
3. Aprire Gestione MultiPoint e tornare a **desktop virtuali**.
4. Fare clic su **Importa modello di desktop virtuale**. Seguire le istruzioni per importare il modello dal server di origine.

> [!NOTE]
> Quando si importa un modello di desktop virtuale, qualsiasi personalizzazione applicata al modello verrà reimpostata. 

## <a name="next-step"></a>Passaggio successivo
[Convalidare la nuova distribuzione di MultiPoint Services.](multipoint-services-post-migration-steps.md)