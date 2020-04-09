---
title: Per la replica è consigliabile usare l'autenticazione basata su certificati
description: Versione online del testo per questa regola di Best Practices Analyzer.
ms.prod: windows-server
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: d931cc57-414f-4bdf-9ebd-08fd5e22b19d
author: kbdazure
ms.date: 8/16/2016
ms.openlocfilehash: c5e356792ba93d21d9ce130a46e1d60336c99844
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80857674"
---
# <a name="certificate-based-authentication-is-recommended-for-replication"></a>Per la replica è consigliabile usare l'autenticazione basata su certificati

>Si applica a: Windows Server 2016

Per altre informazioni sulle procedure consigliate e sulle analisi, vedere [Eseguire analisi di Best Practice Analyzer e gestire i risultati delle analisi](https://go.microsoft.com/fwlink/p/?LinkID=223177).  
  
|Proprietà|Dettagli|  
|-|-|  
|**Sistema operativo**|Windows Server 2016|  
|**Prodotto/funzionalità**|Hyper-V|  
|**Gravità**|Avviso|  
|**Categoria**|Configurazione|  
  
Nelle sezioni seguenti, corsivo indica il testo dell'interfaccia Utente visualizzata nello strumento Analizzatore procedure consigliate per questo problema.  
  
## <a name="issue"></a>**Problema**  
*Una o più macchine virtuali selezionate per la replica sono configurate per l'autenticazione Kerberos.*  
  
## <a name="impact"></a>**Impatto**  
*Il traffico di rete di replica dal server primario al server di replica non è crittografato. Ciò influisca sulle macchine virtuali seguenti:*  
  
\<elenco di macchine virtuali >  
  
## <a name="resolution"></a>**Soluzione**  
*Se viene usato un altro metodo per eseguire la crittografia, è possibile ignorare questa operazione. In caso contrario, modificare le impostazioni della macchina virtuale per scegliere l'autenticazione basata su certificati.*  
  


