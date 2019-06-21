---
title: Passaggio 2 piano Cluster Server
description: Questo argomento fa parte della Guida alla distribuzione di accesso remoto in un Cluster in Windows Server 2016.
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-ras
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 673c5bfb-b590-4932-8e54-ca0a466d90cc
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 0dcb14a03f02f931d59743f1b1b8b24b84ba8351
ms.sourcegitcommit: afb0602767de64a76aaf9ce6a60d2f0e78efb78b
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 06/20/2019
ms.locfileid: "67282864"
---
# <a name="step-2-plan-cluster-servers"></a>Passaggio 2 piano Cluster Server

>Si applica a: Windows Server (canale semestrale), Windows Server 2016

Dopo aver distribuito un singolo server di accesso remoto, prevede di aggiungere altri server nel cluster.  
  
|Attività|Descrizione|  
|----|--------|  
|[2.1 installano ruoli e funzionalità](#BKMK_Install).|Per ogni server che verranno aggiunti al cluster, pianificare per l'installazione del ruolo Accesso remoto e la funzionalità Bilanciamento carico di rete di Windows (se necessario), pianificare la topologia, gli indirizzi IP, routing e inoltro.|  
|[2.2 configurare le impostazioni del server](#BKMK_Config)|Configurare le impostazioni per ogni server che verranno aggiunti al cluster. Si noti che è possibile configurare un cluster con bilanciamento del carico dei server con macchine virtuali. Affinché il routing e connettività a funzionare correttamente, è necessario configurare le macchine virtuali per l'utilizzo di spoofing degli indirizzi MAC.|  
  
## <a name="BKMK_Install"></a>2.1 installano ruoli e funzionalità  
Per ogni server che si desidera aggiungere al cluster, prevede di installare il ruolo Accesso remoto. Inoltre prevede di installare la funzionalità di carico bilanciamento rete (NLB) se si desidera caricare bilanciare il traffico al cluster usando bilanciamento carico di rete di Windows. Per altre informazioni, vedere [bilanciamento carico di rete](https://technet.microsoft.com/windows-server-docs/networking/technologies/network-load-balancing).  
  
## <a name="BKMK_Config"></a>2.2 configurare le impostazioni del server  
Per ogni server che verranno aggiunti al cluster, pianificare le impostazioni di dominio e indirizzo IP. Tenere presente quanto segue:  
  
1.  Tutti i server del cluster devono appartenere allo stesso dominio.  
  
2.  Server del cluster devono trovarsi nella stessa subnet.  
  
3.  Ogni server del cluster deve avere lo stesso numero di schede di rete in uso per la distribuzione di DirectAccess.  
  
Quando si carica bilanciare il cluster usando bilanciamento carico di rete di Windows vengono applicate le impostazioni di bilanciamento carico di rete di Windows seguenti:  
  
1.  Operazione in modalità Unicast. Ciò può essere modificato per il multicast utilizzando Gestione bilanciamento carico di rete. Questa impostazione non può essere modificata nella console di gestione accesso remoto.  
  
2.  Peso definito a fattori come uguali, in cui tutti i server del cluster hanno carico equa del carico.  
  
3.  Filtraggio del traffico di modalità sarà con carico bilanciato tra più host.  
  
4.  Un singolo affinità affinità è definito.  
  
5.  Entrambi i protocolli  

