---
title: Panoramica dello scenario del lab di test
description: 'Questo argomento fa parte della Guida al Lab di test: dimostrazione di DirectAccess con autenticazione OTP e RSA SecurID per Windows Server 2016'
manager: brianlic
ms.prod: windows-server
ms.technology: networking-da
ms.topic: article
ms.assetid: ce584811-b209-48fe-ab2b-4c399bd0bd79
ms.author: lizross
author: eross-msft
ms.openlocfilehash: 39b6212714ebf868b91c1b28f35cc6a5149828ca
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80860324"
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
  


