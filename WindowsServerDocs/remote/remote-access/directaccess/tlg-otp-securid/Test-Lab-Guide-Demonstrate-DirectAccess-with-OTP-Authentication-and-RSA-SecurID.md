---
title: 'Guida al Lab di test: dimostrazione di DirectAccess con autenticazione OTP e SecurID RSA'
description: 'Questo argomento fa parte della Guida al Lab di Test: dimostrare DirectAccess con autenticazione OTP e SecurID RSA per Windows Server 2016'
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology:
- networking-da
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 10c7a49c-5671-4bec-b562-13fdd67f4629
ms.author: pashort
author: shortpatti
ms.openlocfilehash: a6e4b03ceb331899ac622bffe44061a7d92f5a09
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59866232"
---
# <a name="test-lab-guide-demonstrate-directaccess-with-otp-authentication-and-rsa-securid"></a>Guida dell'ambiente di prova: Dimostrazione di DirectAccess con autenticazione OTP e SecurID RSA

>Si applica a: Windows Server (canale semestrale), Windows Server 2016

Accesso remoto è un ruolo del server nel sistema operativo Windows Server 2016, Windows Server 2012 R2 e Windows Server 2012 che consente agli utenti remoti di accedere in modo sicuro le risorse di rete interna usando DirectAccess o reti private virtuali (VPN) con il Routing e il servizio di accesso remoto (RRAS). Questa guida contiene istruzioni dettagliate per estendere il [Guida al Lab di Test: Dimostrazione del singolo Server DirectAccess con ambiente misto IPv4 e IPv6](https://go.microsoft.com/fwlink/p/?LinkId=237004) per dimostrare la configurazione di una password monouso (OTP) di accesso remoto.  
  
> [!WARNING]  
> La progettazione di questa Guida al lab di test include i server di infrastruttura, ad esempio un controller di dominio e un'autorità di certificazione (CA) che eseguono Windows Server 2012 R2 o Windows Server 2012. Uso di questa Guida al lab di test per configurare i server di infrastruttura che eseguono altri sistemi operativi non è stato testato e istruzioni per la configurazione di altri sistemi operativi non sono inclusi in questa Guida.  
  
## <a name="about-this-guide"></a>Informazioni sulla guida  
Accesso remoto in Windows Server 2016, Windows Server 2012 R2 e Windows Server 2012 aggiunge il supporto per l'autenticazione client con OTP. Ai fini di questo lab di test solo RSA SecurID viene usato per illustrare la funzionalità OTP con accesso remoto. Altri RADIUS OTP di base sono supportate anche soluzioni, ma non rientrano nell'ambito di questo lab di test. Questa guida contiene istruzioni per configurare e dimostrare Accesso remoto usando sei server e due computer client. L'accesso remoto completato con il lab di test OTP simula una intranet, Internet e una rete domestica e viene illustrata la funzionalità di accesso remoto in diversi scenari di connessione Internet.  
  
> [!IMPORTANT]  
> Questo lab è un modello di prova che usa il numero minimo di computer. La configurazione descritta in questa guida è unicamente a scopi di lab di test e non deve essere usata in un ambiente di produzione.  
  


