---
title: L'aggiornamento di Host sessione Desktop remoto a Windows Server 2016
description: In questo articolo viene descritto come aggiornare le distribuzioni di Servizi Desktop remoto esistente a Windows Server 2016.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.author: spatnaik
ms.date: 08/01/2016
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 5c9b98b8-4eca-4a39-b10b-2bac729f7f44
author: spatnaik
manager: scottman
ms.openlocfilehash: 0cf5af29d610ba64d045e10241fd39b01d3f7024
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59856062"
---
# <a name="upgrading-your-remote-desktop-session-host-to-windows-server-2016"></a>L'aggiornamento di Host sessione Desktop remoto a Windows Server 2016

>Si applica a: Windows Server (canale semestrale), Windows Server 2016

> [!IMPORTANT]
> Tutte le applicazioni devono essere disinstallate prima dell'aggiornamento e reinstallate dopo l'aggiornamento per evitare eventuali problemi di compatibilità delle app che possono aumentare a causa dell'aggiornamento.

## <a name="supported-os-upgrades-with-rds-role-installed"></a>Aggiornamenti del sistema operativo è supportato con ruolo Servizi Desktop REMOTO
Gli aggiornamenti a Windows Server 2016 sono supportati solo da Windows Server 2012 R2 e Windows Server 2016 TP5.

## <a name="upgrading-a-rds-session-based-collection"></a>Aggiornamento di una raccolta basata sulla sessione di servizi desktop remoto
Per ridurre i tempi di inattività al minimo, è consigliabile seguire i passaggi seguenti durante l'aggiornamento di una raccolta basata sulla sessione di servizi desktop remoto:

1. Identificare i server da aggiornare, ad esempio, mezz'i server nell'insieme.
2. Evitare di nuove connessioni ai server seguenti impostando **consentire nuove connessioni** su false.
3. Disconnettere tutte le sessioni in tali server. 
4. Rimuovere questi server dalla raccolta.
5. Aggiornare i server a Windows Server 2016.
6. Impostare **consentire nuove connessioni** su "false" negli altri server nella raccolta.
7. Aggiungere i server aggiornati al insiemi corrispondenti.
8. Rimuovere il rimanente set di server per l'aggiornamento dalla raccolta.
9. Impostare **consentire nuove connessioni** su "true" nei server aggiornati nella raccolta.
10. Ora è possibile aggiornare i server rimanenti nella distribuzione seguendo i passaggi 3 e 9 precedenti.

## <a name="upgrading-a-standalone-rd-session-host-server"></a>Aggiornamento di un server Host sessione Desktop remoto autonomi
Un server Host sessione Desktop remoto autonomi può essere aggiornato in qualsiasi momento.