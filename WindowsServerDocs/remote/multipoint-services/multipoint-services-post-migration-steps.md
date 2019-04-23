---
title: MultiPoint Services - attività di post-migrazione
description: Informazioni su come convalidare e chiudere la pianificazione della migrazione ai servizi MultiPoint
ms.custom: na
ms.date: 07/29/2016
ms.prod: windows-server-threshold
ms.technology: multipoint-services
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 1497cae0-071e-467d-89b8-a7050815d7de
author: lizap
manager: dongill
ms.openlocfilehash: e3fa3c812355a14289ea4eeff3ab1e7e92e00d97
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59863502"
---
# <a name="multipoint-services---post-migration-tasks"></a>MultiPoint Services - attività di post-migrazione

>Si applica a: Windows Server 2016

Dopo la migrazione ai servizi MultiPoint in Windows Server 2016, usare le informazioni seguenti per convalidare la migrazione ed eseguire operazioni di pulizia.

## <a name="validate-the-migration-by-running-a-pilot-program"></a>Convalidare la migrazione eseguendo un programma pilota

È possibile convalidare la migrazione di servizi MultiPoint creando un progetto pilota nell'ambiente di produzione. Eseguire il progetto pilota nei server prima di sospendere i servizi ruolo migrati nell'ambiente di produzione per verificare che la distribuzione funzioni come previsto. È consigliabile limitare il numero di connessioni all'inizio, lenta aumentando il numero di utenti che accedono ai servizi MultiPoint.

> [!NOTE] 
> Usare sempre gli account di prova per testare la migrazione. Usare un account con privilegi amministrativi e di un account per un utente valido.

## <a name="retire-the-source-server"></a>Disattivare il server di origine
Dopo aver convalidato la migrazione, è possibile arrestare o disconnettere il server di origine dalla rete. Se il server è stato aggiunto al dominio, rimuoverlo dal dominio prima di disconnetterla.

