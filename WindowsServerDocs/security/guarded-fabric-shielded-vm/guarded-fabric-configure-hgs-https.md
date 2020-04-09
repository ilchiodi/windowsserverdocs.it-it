---
title: Configurare HGS per le comunicazioni HTTPS
ms.prod: windows-server
ms.topic: article
manager: dongill
author: rpsqrd
ms.author: ryanpu
ms.technology: security-guarded-fabric
ms.date: 08/29/2018
ms.openlocfilehash: de57a4026a33561760ad36fd78d732352b3aa340
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80856844"
---
# <a name="configure-hgs-for-https-communications"></a>Configurare HGS per le comunicazioni HTTPS

>Si applica a: Windows Server 2019, Windows Server (canale semestrale), Windows Server 2016

Per impostazione predefinita, quando si inizializza il server HGS, i siti Web IIS vengono configurati per le comunicazioni solo HTTP.
Tutte le materie sensibili trasmesse da e verso HGS sono sempre crittografate usando la crittografia a livello di messaggio. Tuttavia, se si desidera un livello di sicurezza più elevato, è anche possibile abilitare HTTPS configurando HGS con un certificato SSL.

[!INCLUDE [Configure HTTPS](../../../includes/configure-hgs-for-https.md)] 

