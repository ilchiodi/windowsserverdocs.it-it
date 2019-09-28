---
title: Installare licenze CAL di Servizi Desktop remoto
description: Informazioni su come installare le licenze CAL per i client Desktop remoto.
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.tgt_pltfrm: na
ms.topic: article
author: lizap
ms.author: elizapo
ms.date: 09/20/2016
manager: dongill
ms.openlocfilehash: 4ee26a0d9ba5ee3a94a4c569a639454e064d0a90
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71387418"
---
# <a name="install-rds-client-access-licenses-on-the-remote-desktop-license-server"></a>Installare licenze CAL di Servizi Desktop remoto nel server licenze Desktop remoto

>Si applica a: Windows Server (Canale semestrale), Windows Server 2019, Windows Server 2016

Usa le informazioni seguenti per installare licenze CAL di Servizi Desktop remoto nel server licenze. Una volta installate le licenze CAL, il server licenze le emetterà per gli utenti a seconda delle esigenze.

Considera che è necessaria la connettività Internet nel computer che esegue Gestione licenze Desktop remoto, ma non nel computer che esegue il server licenze.

1. Nel server licenze (in genere il primo Gestore connessione Desktop remoto) apri Gestione licenze Desktop remoto.
2. Fai clic con il pulsante destro del mouse sul server licenze e quindi scegli **Installa licenze**.
3. Fai clic su **Avanti** nella pagina iniziale.
4. Seleziona il programma da cui hai acquistato le licenze CAL di Servizi Desktop remoto e quindi fai clic su **Avanti**. Se sei un provider di servizi, seleziona **Service Provider License Agreement** (Contratto di licenza per provider di servizi).
5. Immetti le informazioni per il programma di licenza. Nella maggior parte dei casi si tratta del codice di licenza o del numero di contratto, ma il valore da immettere varia in base al programma di licenza in uso.
6. Fare clic su **Avanti**.
7. Seleziona la versione del prodotto, il tipo di licenza e numero di licenze per l'ambiente e quindi fai clic su **Avanti**. Gestione licenze contatta Microsoft per convalidare e recuperare le licenze.
8.  Fare clic su **Fine** per completare il processo.