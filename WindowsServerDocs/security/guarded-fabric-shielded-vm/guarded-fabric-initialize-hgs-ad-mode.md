---
title: Inizializzare HGS usando l'attestazione attendibile dell'amministratore
ms.custom: na
ms.prod: windows-server
ms.topic: article
manager: dongill
author: rpsqrd
ms.technology: security-guarded-fabric
ms.date: 08/29/2018
ms.openlocfilehash: 491754e8bcaad4524084604b78c7c6ed0fdee295
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71402360"
---
# <a name="initialize-hgs-using-admin-trusted-attestation"></a>Inizializzare HGS usando l'attestazione attendibile dell'amministratore

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016

>[!IMPORTANT]
>L'attestazione amministratore (modalità AD) è deprecata a partire da Windows Server 2019. Per gli ambienti in cui non è possibile attestazione TPM, configurare l' [attestazione chiave host](guarded-fabric-initialize-hgs-key-mode.md). L'attestazione chiave host offre una garanzia simile alla modalità AD ed è più semplice da configurare. 


Questa procedura varia a seconda che si stia inizializzando HGS in una nuova foresta o in una foresta Bastion esistente:

1. [Inizializzare il cluster HGS in una nuova foresta (impostazione predefinita)](guarded-fabric-initialize-hgs-ad-mode-default.md)

   -Oppure-

   [Inizializzare il cluster HGS in una foresta Bastion esistente](guarded-fabric-initialize-hgs-ad-mode-bastion.md)

2. [Configurare l'invio DNS nel dominio dell'infrastruttura](guarded-fabric-configuring-fabric-dns.md)

3. [Configurare l'invio DNS e un trust unidirezionale nel dominio HGS](guarded-fabric-configure-dns-forwarding-and-trust.md)



