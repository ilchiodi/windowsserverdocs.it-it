---
title: MultiPoint Services-attività post-migrazione
description: Informazioni su come convalidare e chiudere la migrazione a MultiPoint Services
ms.custom: na
ms.date: 07/29/2016
ms.prod: windows-server
ms.technology: multipoint-services
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 1497cae0-071e-467d-89b8-a7050815d7de
author: lizap
manager: dongill
ms.openlocfilehash: 3102a442b4668856050f603f30f57f6bbed20654
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71389025"
---
# <a name="multipoint-services---post-migration-tasks"></a>MultiPoint Services-attività post-migrazione

>Si applica a: Windows Server 2016

Dopo aver eseguito la migrazione a MultiPoint Services in Windows Server 2016, usare le informazioni seguenti per convalidare la migrazione ed eseguire i passaggi di pulizia.

## <a name="validate-the-migration-by-running-a-pilot-program"></a>Convalidare la migrazione eseguendo un programma pilota

È possibile convalidare la migrazione di MultiPoint Services creando un progetto pilota nell'ambiente di produzione. Eseguire il progetto pilota sui server prima di inserire i servizi ruolo migrati in produzione per verificare che la distribuzione funzioni come previsto. Provare a limitare il numero di connessioni inizialmente, aumentando lentamente il numero di utenti che accedono a MultiPoint Services.

> [!NOTE] 
> Usare sempre gli account di test per testare la migrazione. Usare un account con privilegi amministrativi e un account per un utente valido.

## <a name="retire-the-source-server"></a>Disattivare il server di origine
Dopo aver convalidato la migrazione, è possibile arrestare o disconnettere il server di origine dalla rete. Se il server è stato aggiunto a un dominio, rimuoverlo dal dominio prima di disconnetterlo.

