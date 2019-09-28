---
title: Configurare HGS per le comunicazioni HTTPS
ms.custom: na
ms.prod: windows-server
ms.topic: article
manager: dongill
author: rpsqrd
ms.technology: security-guarded-fabric
ms.date: 08/29/2018
ms.openlocfilehash: f0cbf6a6dc1970499758a6a48bfaadb95c464ec1
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71403672"
---
# <a name="configure-hgs-for-https-communications"></a>Configurare HGS per le comunicazioni HTTPS

>Si applica a: Windows Server 2019, Windows Server (canale semestrale), Windows Server 2016

Per impostazione predefinita, quando si inizializza il server HGS, i siti Web IIS vengono configurati per le comunicazioni solo HTTP.
Tutte le materie sensibili trasmesse da e verso HGS sono sempre crittografate usando la crittografia a livello di messaggio. Tuttavia, se si desidera un livello di sicurezza più elevato, è anche possibile abilitare HTTPS configurando HGS con un certificato SSL.

[!INCLUDE [Configure HTTPS](../../../includes/configure-hgs-for-https.md)] 

