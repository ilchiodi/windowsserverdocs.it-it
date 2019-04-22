---
title: Configurazione PVLAN in un commutatore virtuale deve essere coerenti
description: Versione online del testo per questa regola di Best Practices Analyzer.
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 4db63bcc-7a54-4f19-98a6-c274c3956d51
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: b7d6d068027aa9497b00138bd1d889ea86aa3308
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59813252"
---
# <a name="pvlan-configuration-on-a-virtual-switch-must-be-consistent"></a>Configurazione PVLAN in un commutatore virtuale deve essere coerenti

>Si applica a: Windows Server 2016

Per altre informazioni sulle procedure consigliate e sulle analisi, vedere [Eseguire analisi di Best Practice Analyzer e gestire i risultati delle analisi](https://go.microsoft.com/fwlink/p/?LinkID=223177).  
  
|Proprietà|Dettagli|  
|-|-|  
|**Sistema operativo**|Windows Server 2016| 
|**/ Funzionalità del prodotto**|Hyper-V|  
|**Severity**|Errore|  
|**Categoria**|Configurazione|  
  
Nelle sezioni seguenti, corsivo indica il testo dell'interfaccia Utente visualizzata nello strumento Analizzatore procedure consigliate per questo problema.
  
## <a name="issue"></a>**Problema**  
*Privato Virtual Local Area Network (PVLAN) non è configurato correttamente in uno o più schede di rete virtuale.*  
  
## <a name="impact"></a>**Impact**  
*PVLAN può isolare il traffico di rete tra le macchine virtuali correttamente.*  
  
## <a name="resolution"></a>**Soluzione**  
*Usare il cmdlet di Windows PowerShell Set-VMNetworkAdapterVlan, alla configurazione corretta PVLAN.*  
  


