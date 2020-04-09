---
title: 'Guida al Lab di test: dimostrazione di DirectAccess con autenticazione OTP e RSA SecurID'
description: 'Questo argomento fa parte della Guida al Lab di test: dimostrazione di DirectAccess con autenticazione OTP e RSA SecurID per Windows Server 2016'
manager: brianlic
ms.prod: windows-server
ms.technology: networking-da
ms.topic: article
ms.assetid: 10c7a49c-5671-4bec-b562-13fdd67f4629
ms.author: lizross
author: eross-msft
ms.openlocfilehash: ee307804b5a03db5638775cb41d6e0a29f8a4a5d
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80814474"
---
# <a name="test-lab-guide-demonstrate-directaccess-with-otp-authentication-and-rsa-securid"></a>Guida al lab di test: Dimostrazione di DirectAccess con l'autenticazione OTP e SecurID RSA

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016

Accesso remoto è un ruolo del server nel sistema operativo Windows Server 2016, Windows Server 2012 R2 e Windows Server 2012 che consente agli utenti remoti di accedere in modo sicuro alle risorse di rete interne tramite DirectAccess o reti private virtuali (VPN) con il routing e servizio di accesso remoto (RRAS). Questa guida contiene istruzioni dettagliate per estendere la guida al Lab di [test: dimostrazione della configurazione del singolo server DirectAccess con IPv4 misto e IPv6](https://go.microsoft.com/fwlink/p/?LinkId=237004) per dimostrare una configurazione di accesso remoto con password monouso (OTP).  
  
> [!WARNING]  
> La progettazione di questa guida al Lab di test include server di infrastruttura, ad esempio un controller di dominio e un'autorità di certificazione (CA) che eseguono Windows Server 2012 R2 o Windows Server 2012. L'utilizzo di questa guida al Lab di test per configurare i server di infrastruttura che eseguono altri sistemi operativi non è stato testato e le istruzioni per la configurazione di altri sistemi operativi non sono inclusi in questa guida.  
  
## <a name="about-this-guide"></a>Informazioni sulla guida  
Accesso remoto in Windows Server 2016, Windows Server 2012 R2 e Windows Server 2012 aggiunge il supporto per l'autenticazione client con OTP. Ai fini di questo Lab di test viene usato solo RSA SecurID per dimostrare la funzionalità OTP con accesso remoto. Sono supportate anche altre soluzioni OTP basate su RADIUS, ma non rientrano nell'ambito di questo Lab di test. Questa guida contiene istruzioni per configurare e dimostrare Accesso remoto usando sei server e due computer client. Il Lab di test di accesso remoto completato con OTP simula una rete Intranet, Internet e una rete domestica e illustra la funzionalità di accesso remoto in diversi scenari di connessione Internet.  
  
> [!IMPORTANT]  
> Questo lab è un modello di prova che usa il numero minimo di computer. La configurazione descritta in questa guida è unicamente a scopi di lab di test e non deve essere usata in un ambiente di produzione.  
  


