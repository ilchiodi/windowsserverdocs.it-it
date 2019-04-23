---
title: Inizializzare HGS
ms.custom: na
ms.prod: windows-server-threshold
ms.topic: article
manager: dongill
author: rpsqrd
ms.technology: security-guarded-fabric
ms.date: 08/29/2018
ms.openlocfilehash: c689a1d5f69731db0cb85a884f5af2ee0230e7be
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59865452"
---
# <a name="initialize-the-host-guardian-service-hgs"></a>Inizializzare il servizio sorveglianza Host (HGS)

>Si applica a: Windows Server 2019, Windows Server (canale semestrale), Windows Server 2016

Durante l'inizializzazione HGS, specificare la modalità di HGS verrà utilizzato per misurare l'integrità degli host sorvegliati. Sono disponibili due opzioni si escludono a vicenda. Per informazioni generali sulla modalità da scegliere, vedere [infrastruttura protetta e schermate della macchina virtuale Guida alla pianificazione di servizi di hosting](guarded-fabric-planning-for-hosters.md).

Gli argomenti seguenti illustrano i passaggi di distribuzione per ogni modalità:

- [Attestazione TPM (modalità TPM)](guarded-fabric-initialize-hgs-tpm-mode.md)
- [Attestazione chiave host (modalità chiave)](guarded-fabric-initialize-hgs-key-mode.md)
- [Attestazione amministratore (modalità Active Directory)](guarded-fabric-initialize-hgs-ad-mode.md)

È consigliabile eseguire questi passaggi in un server fisico.