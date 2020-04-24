---
title: Proteggere la distribuzione di Servizi Desktop remoto - Ripristino di emergenza
description: Informazioni sulle opzioni di ripristino di emergenza per Servizi Desktop remoto
ms.prod: windows-server
ms.technology: remote-desktop-services
ms.topic: article
ms.assetid: 9ff6a3b0-ea14-424e-9524-209252e9f1a8
author: lizap
ms.author: elizapo
ms.date: 06/12/2017
ms.openlocfilehash: 5c3257be5b3e9a53b8aa2400777f0519f8891843
ms.sourcegitcommit: 3a3d62f938322849f81ee9ec01186b3e7ab90fe0
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 04/23/2020
ms.locfileid: "80861294"
---
# <a name="configure-disaster-recovery-for-remote-desktop-services"></a>Configurare il ripristino di emergenza per Servizi Desktop remoto

Quando si distribuisce Servizi Desktop remoto nell'ambiente, diventa una parte essenziale dell'infrastruttura, in particolare le app e le risorse che si condividono con gli utenti. Se la distribuzione di Servizi Desktop remoto si arresta per qualsiasi motivo, ad esempio un errore di rete o una calamità naturale, gli utenti non possono accedere alle app e alle risorse e le attività rallentano. Per evitare questo problema, è possibile configurare una soluzione di ripristino di emergenza che consenta di eseguire il failover della distribuzione. Se la distribuzione di Servizi Desktop remoto non è disponibile per qualunque motivo, è disponibile un backup da utilizzare automaticamente.

Per mantenere la distribuzione di Servizi Desktop remoto in esecuzione in caso di malfunzionamento di un singolo componente o computer, si consiglia di configurare la disponibilità elevata nella distribuzione di Servizi Desktop remoto. È possibile farlo configurando una [farm di host sessione Desktop remoto](rds-scale-rdsh-farm.md) e verificando che i [gestori connessione siano raggruppati per la disponibilità elevata](rds-connection-broker-cluster.md). 

Le soluzioni di ripristino di emergenza che consigliamo qui servono a proteggere la distribuzione da situazioni critiche, problemi che interrompono l'intera distribuzione di Servizi Desktop remoto (inclusi ruoli ridondanti configurati per la disponibilità elevata). Quando si verificano tali situazioni critiche, disporre di una soluzione di ripristino di emergenza nella distribuzione permette di eseguire il failover dell'intera distribuzione e di ripristinare rapidamente app e risorse in modo che siano disponibili per gli utenti.

Usare le informazioni seguenti per distribuire le soluzioni di ripristino di emergenza in Servizi Desktop remoto:

- [Sfruttare più data center di Azure in modo che gli utenti possano accedere alla distribuzione di Servizi Desktop remoto anche se un data center di Azure diventa inattivo (ridondanza geografica)](rds-multi-datacenter-deployment.md)
- [Distribuire Azure Site Recovery per mettere a disposizione il failover per i componenti di Servizi Desktop remoto in failover da sito a sito o da sito ad Azure](rds-disaster-recovery-with-azure.md)


