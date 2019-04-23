---
title: L'autenticazione basata su certificati viene configurata, ma il certificato specificato non è installato nel server di Replica o nodi cluster di failover
description: Versione online del testo per questa regola di Best Practices Analyzer.
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 4cabbce3-9367-4ddc-a108-1e5e1ab2bcff
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: feef30d2f798057e4bd8e53ebab240af6b1d25b8
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59832942"
---
# <a name="certificate-based-authentication-is-configured-but-the-specified-certificate-is-not-installed-on-the-replica-server-or-failover-cluster-nodes"></a>L'autenticazione basata su certificati viene configurata, ma il certificato specificato non è installato nel server di Replica o nodi cluster di failover

>Si applica a: Windows Server 2016


  
*Per altre informazioni sulle procedure consigliate e le analisi, vedere* [Best Practices Analyzer](https://go.microsoft.com/fwlink/?LinkId=122786).  
  
|Proprietà|Dettagli|  
|-|-|  
|**Sistema operativo**|Windows Server 2016|  
|**/ Funzionalità del prodotto**|Hyper-V|  
|**Severity**|Errore|  
|**Categoria**|Configurazione|  

Nelle sezioni seguenti, corsivo indica il testo dell'interfaccia Utente visualizzata nello strumento Analizzatore procedure consigliate per questo problema.

## <a name="issue"></a>Problema  
  
*Il certificato di sicurezza che la Replica Hyper-V sia stata configurata per l'uso per fornire la replica basata su certificato non è installata nel server di Replica (o qualsiasi nodi del cluster di failover).*  
  
## <a name="impact"></a>Impatto  
  
*In caso di failover del cluster o spostare in un altro nodo, la replica Hyper-V sospenderà se il nuovo nodo non ha anche installato il certificato appropriato. Questo influisce sulle seguenti nodi:*  
  
\<elenco di nodi >  
  
## <a name="resolution"></a>Risoluzione  
  
*Installare il certificato configurato nel server Replica (e tutti i relativi nodi del cluster di failover, se presente).*  
  


