---
title: Inizializzare HGS usando un'attestazione Trusted TPM
ms.prod: windows-server
ms.topic: article
manager: dongill
author: rpsqrd
ms.author: ryanpu
ms.technology: security-guarded-fabric
ms.date: 08/29/2018
ms.openlocfilehash: b8ceebe63a586ec95b502dfea12f99d174549448
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80856614"
---
# <a name="initialize-hgs-using-tpm-trusted-attestation"></a>Inizializzare HGS usando un'attestazione Trusted TPM

>Si applica a: Windows Server 2019, Windows Server (canale semestrale), Windows Server 2016

Questa procedura varia a seconda che si stia inizializzando HGS in una nuova foresta o in una foresta Bastion esistente:

1. [Inizializzare il cluster HGS in una nuova foresta (impostazione predefinita)](guarded-fabric-initialize-hgs-tpm-mode-default.md)

   -Oppure-

   [Inizializzare il cluster HGS in una foresta Bastion esistente](guarded-fabric-initialize-hgs-tpm-mode-bastion.md)

2. [Installare i certificati radice TPM attendibili](guarded-fabric-install-trusted-tpm-root-certificates.md)   
3. [Configurare il DNS dell'infrastruttura](guarded-fabric-configuring-fabric-dns.md)

