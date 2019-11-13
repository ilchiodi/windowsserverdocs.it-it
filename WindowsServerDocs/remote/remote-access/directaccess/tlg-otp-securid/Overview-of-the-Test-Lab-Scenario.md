---
title: Panoramica dello scenario del lab di test
description: 'Questo argomento fa parte della Guida al Lab di test: dimostrazione di DirectAccess con autenticazione OTP e RSA SecurID per Windows Server 2016'
manager: brianlic
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-da
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: ce584811-b209-48fe-ab2b-4c399bd0bd79
ms.author: pashort
author: shortpatti
ms.openlocfilehash: ba3981c79d1441797f21c8ed8051ace6ac490cab
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71404734"
---
# <a name="overview-of-the-test-lab-scenario"></a>Panoramica dello scenario del lab di test

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016

Accesso remoto è un ruolo del server nei sistemi operativi Windows Server 2016, Windows Server 2012 R2 e Windows Server 2012 che consente agli utenti remoti di accedere in modo sicuro alle risorse di rete interne tramite DirectAccess o reti private virtuali (VPN) con il Servizio Routing e accesso remoto (RRAS). Questa guida contiene istruzioni dettagliate per estendere la guida al Lab di [test: dimostrazione della configurazione del singolo server DirectAccess con IPv4 misto e IPv6](https://go.microsoft.com/fwlink/p/?LinkId=237004) per dimostrare una configurazione di accesso remoto con password monouso (OTP).  
  
> [!WARNING]  
> La progettazione di questa guida al Lab di test include server di infrastruttura, ad esempio un controller di dominio e un'autorità di certificazione (CA) che eseguono Windows Server 2016, Windows Server 2012 R2 o Windows Server 2012. L'utilizzo di questa guida al Lab di test per configurare i server di infrastruttura che eseguono altri sistemi operativi non è stato testato e le istruzioni per la configurazione di altri sistemi operativi non sono inclusi in questa guida.  
  
## <a name="about-this-guide"></a>Informazioni sulla guida  
Accesso remoto in Windows Server 2016, Windows Server 2012 R2 e Windows Server 2012 aggiunge il supporto per l'autenticazione client con OTP. Ai fini di questo Lab di test viene usato solo RSA SecurID per dimostrare la funzionalità OTP con accesso remoto. Sono supportate anche altre soluzioni OTP basate su RADIUS, ma non rientrano nell'ambito di questo Lab di test. Questa guida contiene istruzioni per configurare e dimostrare Accesso remoto usando sei server e due computer client. Il Lab di test di accesso remoto completato con OTP simula una rete Intranet, Internet e una rete domestica e illustra la funzionalità di accesso remoto in diversi scenari di connessione Internet.  
  
> [!IMPORTANT]  
> Questo lab è un modello di prova che usa il numero minimo di computer. La configurazione descritta in questa guida è unicamente a scopi di lab di test e non deve essere usata in un ambiente di produzione.  
  


