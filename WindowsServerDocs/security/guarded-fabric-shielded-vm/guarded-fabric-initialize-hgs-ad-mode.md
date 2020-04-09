---
title: Inizializzare HGS usando l'attestazione attendibile dell'amministratore
ms.prod: windows-server
ms.topic: article
manager: dongill
author: rpsqrd
ms.author: ryanpu
ms.technology: security-guarded-fabric
ms.date: 08/29/2018
ms.openlocfilehash: b7c0b88071a28953ddda8abb57a805ef119511e0
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80856674"
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



