---
title: 'Guida al Lab di test: dimostrare una distribuzione multisito di DirectAccess'
description: 'Questo argomento fa parte della Guida al Lab di Test: dimostrare una distribuzione multisito DirectAccess per Windows Server 2016'
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-da
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 3c98106c-67cc-406a-810e-f2e09f7e2c5e
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 87c1f629d96d247d273d48066797795b9fe724e6
ms.sourcegitcommit: afb0602767de64a76aaf9ce6a60d2f0e78efb78b
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 06/20/2019
ms.locfileid: "67281385"
---
# <a name="test-lab-guide-demonstrate-a-directaccess-multisite-deployment"></a>Guida dell'ambiente di prova: Dimostrare una distribuzione multisito di DirectAccess

>Si applica a: Windows Server (canale semestrale), Windows Server 2016

Accesso remoto è un ruolo del server nei sistemi operativi Windows Server 2016, Windows Server 2012 R2 e Windows Server 2012 che consente agli utenti remoti di accedere in modo sicuro le risorse di rete interna usando DirectAccess o VPN RRAS. Questa guida contiene istruzioni dettagliate per estendere il [Guida al Lab di Test: Dimostrazione del singolo Server DirectAccess con ambiente misto IPv4 e IPv6](https://go.microsoft.com/fwlink/p/?LinkId=237004) per dimostrare l'accesso remoto in uno scenario multisito.  
  
Distribuzione di accesso remoto in uno scenario multisito consente di configurare i server di accesso remoto in località geografiche diverse. In precedenza, gli utenti remoti dovevano sempre la connessione alla rete aziendale tramite un determinato server DirectAccess. Con Windows Server 2016, Windows Server 2012 R2 o Windows Server 2012 e Windows 10 o Windows 8, è possibile configurare punti di ingresso per ogni area geografica della distribuzione. Ogni punto di ingresso può essere un singolo server di accesso remoto o un cluster di server di accesso remoto. Gli utenti remoti hanno la possibilità di connettersi a uno qualsiasi dei punti di ingresso di accesso remoto dell'organizzazione. Ad esempio, se un utente remoto in genere si connette al punto di ingresso di accesso remoto che si trova in Asia, ma continua spiegando un viaggio in Europa, il computer client si connette automaticamente al punto di ingresso più vicino di accesso remoto.  
  
## <a name="about-this-guide"></a>Informazioni sulla guida  
Questa guida contiene istruzioni per configurare e dimostrare accesso remoto con nove server e tre i computer client. Il lab di test multisito di accesso remoto completato simula una intranet, Internet e una rete domestica e viene illustrata la funzionalità di accesso remoto in diversi scenari di connessione Internet.  
  
> [!IMPORTANT]  
> Questo lab è un modello di prova che usa il numero minimo di computer. La configurazione descritta in questa guida è unicamente a scopi di lab di test e non deve essere usata in un ambiente di produzione.  
  


