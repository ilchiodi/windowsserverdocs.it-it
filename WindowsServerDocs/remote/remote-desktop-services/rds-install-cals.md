---
title: Installare licenze CAL Servizi Desktop remoto
description: Informazioni su come installare le licenze CAL per i client desktop remoto.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.tgt_pltfrm: na
ms.topic: article
author: lizap
ms.author: elizapo
ms.date: 09/20/2016
manager: dongill
ms.openlocfilehash: 2f283b51acc869704a52f09bebc228660cdfbc38
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59870572"
---
# <a name="install-rds-client-access-licenses-on-the-remote-desktop-license-server"></a>Installare licenze CAL Servizi Desktop remoto nel server licenze Desktop remoto

>Si applica a: Windows Server (canale semestrale), Windows Server 2016

Usare le informazioni seguenti per installare le licenze di accesso client di Servizi Desktop remoto (CAL) nel server licenze. Una volta installate le licenze CAL, il server licenze emetterà li agli utenti come appropriato.

Tenere presente che è necessaria la connettività Internet nel computer che esegue Gestione licenze Desktop remoto, ma non nel computer che esegue il server licenze.

1. Nel server licenze (in genere il primo gestore connessione desktop remoto), aprire Gestione licenze Desktop remoto.
2. Fare clic sul server licenze e quindi fare clic su **installare le licenze**.
3. Fare clic su **successivo** nella pagina di benvenuto.
4. Selezionare il programma è stato acquistato le licenze CAL Servizi Desktop remoto da e quindi fare clic su **successivo**. Se sei un provider di servizi, selezionare **Service Provider License Agreement**.
5. Immettere le informazioni per il programma di licenza. Nella maggior parte dei casi, questo sarà il codice di licenza o un numero di contratto, ma il valore varia in base al programma di licenza in uso.
6. Fare clic su **Avanti**.
7. Selezionare la versione del prodotto, il tipo di licenza e numero di licenze per l'ambiente e quindi fare clic su **successivo**. Gestione licenze contatta Microsoft Clearinghouse per convalidare e recuperare le licenze.
8.  Fare clic su **Fine** per completare il processo.