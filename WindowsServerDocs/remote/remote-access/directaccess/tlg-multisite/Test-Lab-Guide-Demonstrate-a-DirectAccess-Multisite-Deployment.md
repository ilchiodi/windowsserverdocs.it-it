---
title: Guida al Lab di test-dimostrazione di una distribuzione multisito di DirectAccess
description: 'Questo argomento fa parte della Guida al Lab di test: illustra una distribuzione multisito di DirectAccess per Windows Server 2016'
manager: brianlic
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-da
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 3c98106c-67cc-406a-810e-f2e09f7e2c5e
ms.author: pashort
author: shortpatti
ms.openlocfilehash: bb4d69f914331e8fa7a06c6dd77ea342d5eefc6a
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71388217"
---
# <a name="test-lab-guide-demonstrate-a-directaccess-multisite-deployment"></a>Guida dell'ambiente di prova: Dimostrazione di una distribuzione multisito di DirectAccess

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016

Accesso remoto è un ruolo server nei sistemi operativi Windows Server 2016, Windows Server 2012 R2 e Windows Server 2012 che consente agli utenti remoti di accedere in modo sicuro alle risorse di rete interne tramite DirectAccess o VPN RRAS. Questa guida contiene istruzioni dettagliate per estendere la guida al Lab [Test: Dimostrazione della configurazione del singolo server DirectAccess con mixed IPv4 e IPv6 @ no__t-0 per dimostrare l'accesso remoto in uno scenario multisito.  
  
La distribuzione di accesso remoto in uno scenario multisito consente di configurare i server di accesso remoto in posizioni geografiche diverse. In precedenza era necessario che gli utenti remoti si connettano sempre alla rete aziendale tramite un server DirectAccess specifico. Con Windows Server 2016, Windows Server 2012 R2 o Windows Server 2012 e Windows 10 o Windows 8, è possibile configurare punti di ingresso per ogni posizione geografica nella distribuzione. Ogni punto di ingresso può essere un singolo server di accesso remoto o un cluster di server di accesso remoto. Gli utenti remoti hanno la possibilità di connettersi a uno dei punti di ingresso di accesso remoto dell'organizzazione. Se, ad esempio, un utente remoto si connette di solito al punto di ingresso di accesso remoto situato in Asia, ma passa a un viaggio aziendale in Europa, il computer client si connette automaticamente al punto di ingresso di accesso remoto più vicino.  
  
## <a name="about-this-guide"></a>Informazioni sulla guida  
Questa guida contiene le istruzioni per configurare e dimostrare l'accesso remoto usando nove server e tre computer client. Il Lab di test multisito di accesso remoto completato simula una rete Intranet, Internet e una rete domestica e illustra la funzionalità di accesso remoto in diversi scenari di connessione Internet.  
  
> [!IMPORTANT]  
> Questo lab è un modello di prova che usa il numero minimo di computer. La configurazione descritta in questa guida è unicamente a scopi di lab di test e non deve essere usata in un ambiente di produzione.  
  


