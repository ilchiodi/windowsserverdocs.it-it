---
title: Inizializzare HGS
ms.prod: windows-server
ms.topic: article
manager: dongill
author: rpsqrd
ms.author: ryanpu
ms.technology: security-guarded-fabric
ms.date: 08/29/2018
ms.openlocfilehash: 42f76dabbe150f229027909f8b58b843f31ae4e1
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80856594"
---
# <a name="initialize-the-host-guardian-service-hgs"></a>Inizializzare il servizio sorveglianza host (HGS)

>Si applica a: Windows Server 2019, Windows Server (canale semestrale), Windows Server 2016

Quando si inizializza HGS, si specifica la modalità che HGS utilizzerà per misurare l'integrità degli host sorvegliati. Sono disponibili due opzioni che si escludono a vicenda. Per informazioni complementari sulla modalità di scelta, vedere l'argomento relativo all' [infrastruttura sorvegliata e la guida alla pianificazione delle VM schermate per i](guarded-fabric-planning-for-hosters.md)provider di hosting.

Negli argomenti seguenti vengono illustrati i passaggi di distribuzione per ogni modalità:

- [Attestazione TPM attendibile (modalità TPM)](guarded-fabric-initialize-hgs-tpm-mode.md)
- [Attestazione chiave host (modalità chiave)](guarded-fabric-initialize-hgs-key-mode.md)
- [Attestazione amministratore (modalità AD)](guarded-fabric-initialize-hgs-ad-mode.md)

È necessario eseguire questi passaggi in un server fisico.