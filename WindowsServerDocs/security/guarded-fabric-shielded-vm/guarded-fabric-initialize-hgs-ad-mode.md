---
title: Inizializzazione tramite attestazione amministratore HGS
ms.custom: na
ms.prod: windows-server-threshold
ms.topic: article
manager: dongill
author: rpsqrd
ms.technology: security-guarded-fabric
ms.date: 08/29/2018
ms.openlocfilehash: 664404cb72981e162bca016df14847e684d987c1
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59821832"
---
# <a name="initialize-hgs-using-admin-trusted-attestation"></a>Inizializzazione tramite attestazione amministratore HGS

>Si applica a: Windows Server (canale semestrale), Windows Server 2016

>[!IMPORTANT]
>Attestazione amministratore (modalità AD) è deprecato a partire da Windows Server 2019. Per gli ambienti in cui l'attestazione TPM non è possibile, configurare [ospitare l'attestazione chiave](guarded-fabric-initialize-hgs-key-mode.md). Attestazione chiave host fornisce una garanzia analoga alla modalità di Active Directory ed è più semplice da configurare. 


Questi passaggi variano a seconda del fatto che si sta inizializzando HGS in una nuova foresta o una foresta bastion esistente:

1. [Inizializzare il cluster del servizio HGS in una nuova foresta (impostazione predefinita)](guarded-fabric-initialize-hgs-ad-mode-default.md)

   -Oppure-

   [Inizializzare il cluster del servizio HGS in una foresta bastion esistente](guarded-fabric-initialize-hgs-ad-mode-bastion.md)

2. [Configurare l'inoltro DNS nel dominio dell'infrastruttura](guarded-fabric-configuring-fabric-dns.md)

3. [Configurare l'inoltro di DNS e una relazione di trust unidirezionali nel dominio HGS](guarded-fabric-configure-dns-forwarding-and-trust.md)



