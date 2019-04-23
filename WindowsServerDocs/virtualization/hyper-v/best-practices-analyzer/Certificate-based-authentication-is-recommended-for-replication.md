---
title: L'autenticazione basata su certificato è consigliato per la replica
description: Versione online del testo per questa regola di Best Practices Analyzer.
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: d931cc57-414f-4bdf-9ebd-08fd5e22b19d
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: 5bd131b48b009b43b6379f72f370b4179dea5a03
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59873062"
---
# <a name="certificate-based-authentication-is-recommended-for-replication"></a>L'autenticazione basata su certificato è consigliato per la replica

>Si applica a: Windows Server 2016

Per altre informazioni sulle procedure consigliate e sulle analisi, vedere [Eseguire analisi di Best Practice Analyzer e gestire i risultati delle analisi](https://go.microsoft.com/fwlink/p/?LinkID=223177).  
  
|Proprietà|Dettagli|  
|-|-|  
|**Sistema operativo**|Windows Server 2016|  
|**/ Funzionalità del prodotto**|Hyper-V|  
|**Severity**|Avviso|  
|**Categoria**|Configurazione|  
  
Nelle sezioni seguenti, corsivo indica il testo dell'interfaccia Utente visualizzata nello strumento Analizzatore procedure consigliate per questo problema.  
  
## <a name="issue"></a>**Problema**  
*Uno o più macchine virtuali selezionate per la replica sono configurate per l'autenticazione Kerberos.*  
  
## <a name="impact"></a>**Impact**  
*Il traffico di rete di replica dal server primario al server di replica non crittografato. Questo influisce sulle macchine virtuali seguenti:*  
  
\<elenco di macchine virtuali >  
  
## <a name="resolution"></a>**Soluzione**  
*Se un altro metodo viene utilizzato per eseguire la crittografia, è possibile ignorarlo. In caso contrario, modificare le impostazioni della macchina virtuale per scegliere l'autenticazione basata su certificato.*  
  


