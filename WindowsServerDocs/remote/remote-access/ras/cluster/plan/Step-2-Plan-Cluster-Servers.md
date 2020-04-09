---
title: Passaggio 2 pianificare i server del cluster
description: Questo argomento fa parte della Guida deploy Remote Access in a cluster in Windows Server 2016.
manager: brianlic
ms.prod: windows-server
ms.technology: networking-ras
ms.topic: article
ms.assetid: 673c5bfb-b590-4932-8e54-ca0a466d90cc
ms.author: lizross
author: eross-msft
ms.openlocfilehash: 7ac00278e501115d81d80f55c1ceae33a379cc4a
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80855244"
---
# <a name="step-2-plan-cluster-servers"></a>Passaggio 2 pianificare i server del cluster

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016

Dopo la distribuzione di un singolo server di accesso remoto, pianificare l'aggiunta di altri server al cluster.  
  
|Attività|Descrizione|  
|----|--------|  
|[2,1 installazione dei ruoli e delle funzionalità](#BKMK_Install).|Per ogni server che verrà aggiunto al cluster, pianificare l'installazione del ruolo accesso remoto e della funzionalità NLB di Windows (se necessario), pianificare la topologia, l'indirizzamento IP, il routing e l'inoltro.|  
|[2,2 configurare le impostazioni del server](#BKMK_Config)|Configurare le impostazioni per ogni server che verrà aggiunto al cluster. Si noti che è possibile configurare un cluster con bilanciamento del carico dei server usando macchine virtuali. Per il corretto funzionamento del routing e della connettività, è necessario configurare le macchine virtuali per l'uso dello spoofing degli indirizzi MAC.|  
  
## <a name="21-installing-roles-and-features"></a><a name="BKMK_Install"></a>2,1 installazione dei ruoli e delle funzionalità  
Per ogni server che si desidera aggiungere al cluster, pianificare l'installazione del ruolo accesso remoto. Inoltre, pianificare l'installazione della funzionalità Bilanciamento carico di rete (NLB) se si desidera bilanciare il carico del traffico al cluster utilizzando bilanciamento carico di rete di Windows. Per altre informazioni, vedere [bilanciamento carico di rete](https://technet.microsoft.com/windows-server-docs/networking/technologies/network-load-balancing).  
  
## <a name="22-configure-server-settings"></a><a name="BKMK_Config"></a>2,2 configurare le impostazioni del server  
Per ogni server che verrà aggiunto al cluster, pianificare le impostazioni dell'indirizzo IP e del dominio. Tenere presente quanto segue:  
  
1.  I server nel cluster devono appartenere tutti allo stesso dominio.  
  
2.  I server nel cluster devono trovarsi nella stessa subnet.  
  
3.  Ogni server del cluster deve avere lo stesso numero di schede di rete in uso per la distribuzione di DirectAccess.  
  
Quando si bilancia il carico del cluster usando il bilanciamento del carico di Windows, vengono applicate le seguenti impostazioni di bilanciamento carico di Windows:  
  
1.  Modalità operativa-unicast. Questo può essere modificato in multicast tramite Gestione bilanciamento carico di bilanciamento. Questa impostazione non può essere modificata nella console di gestione accesso remoto.  
  
2.  Fattore di ponderazione del carico definito come uguale, in cui tutti i server del cluster hanno un carico uguale.  
  
3.  Modalità di filtro: il carico del traffico verrà bilanciato tra più host.  
  
4.  Affinità: è definita l'affinità singola.  
  
5.  Protocolli (entrambi)  

