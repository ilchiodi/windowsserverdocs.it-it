---
title: Configurare il servizio HGS per le comunicazioni Https
ms.custom: na
ms.prod: windows-server-threshold
ms.topic: article
manager: dongill
author: rpsqrd
ms.technology: security-guarded-fabric
ms.date: 08/29/2018
ms.openlocfilehash: 83529a5bdb4547b9881bb307a8a4cd526552d02c
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59829002"
---
# <a name="configure-hgs-for-https-communications"></a>Configurare il servizio HGS per le comunicazioni HTTPS

>Si applica a: Windows Server 2019, Windows Server (canale semestrale), Windows Server 2016

Per impostazione predefinita, quando si inizializza il server HGS verrà configurata i siti web IIS per le comunicazioni solo HTTP.
Tutto il materiale sensibile trasmesso da e verso HGS vengono sempre crittografate tramite crittografia a livello di messaggio, tuttavia, se lo si desidera un maggiore livello di sicurezza è anche possibile abilitare HTTPS tramite la configurazione di HGS con un certificato SSL.

[!INCLUDE [Configure HTTPS](../../../includes/configure-hgs-for-https.md)] 

