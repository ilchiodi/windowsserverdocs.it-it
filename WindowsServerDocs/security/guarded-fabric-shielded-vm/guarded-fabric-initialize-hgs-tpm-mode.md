---
title: Inizializzare HGS usando l'attestazione TPM
ms.custom: na
ms.prod: windows-server-threshold
ms.topic: article
manager: dongill
author: rpsqrd
ms.technology: security-guarded-fabric
ms.date: 08/29/2018
ms.openlocfilehash: 672054462437b615236dfc4f00c8b20c946f11a7
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59882332"
---
# <a name="initialize-hgs-using-tpm-trusted-attestation"></a>Inizializzare HGS usando l'attestazione TPM

>Si applica a: Windows Server 2019, Windows Server (canale semestrale), Windows Server 2016

Questi passaggi variano a seconda del fatto che si sta inizializzando HGS in una nuova foresta o una foresta bastion esistente:

1. [Inizializzare il cluster del servizio HGS in una nuova foresta (impostazione predefinita)](guarded-fabric-initialize-hgs-tpm-mode-default.md)

   -Oppure-

   [Inizializzare il cluster del servizio HGS in una foresta bastion esistente](guarded-fabric-initialize-hgs-tpm-mode-bastion.md)

2. [Installare i certificati radice TPM attendibili](guarded-fabric-install-trusted-tpm-root-certificates.md)   
3. [Configurare l'infrastruttura DNS](guarded-fabric-configuring-fabric-dns.md)

