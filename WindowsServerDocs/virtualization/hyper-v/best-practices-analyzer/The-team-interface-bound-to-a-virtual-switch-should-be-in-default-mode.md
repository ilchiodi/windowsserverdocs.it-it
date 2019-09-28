---
title: L'interfaccia team associato a un commutatore virtuale deve essere in modalità predefinita
description: Versione online del testo per questa regola di Best Practices Analyzer.
ms.prod: windows-server
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 8c118e1e-865f-4cff-acdc-7c35e45d5da9
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: 9bfd0c98e865a0faae8dd70e97696e2c2682531b
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71393432"
---
# <a name="the-team-interface-bound-to-a-virtual-switch-should-be-in-default-mode"></a>L'interfaccia team associato a un commutatore virtuale deve essere in modalità predefinita

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
*Alcuni commutatori virtuali sono associati a un'interfaccia del team, ma l'interfaccia del team non passa il traffico su tutte le VLAN ai commutatori virtuali.*  
  
## <a name="impact"></a>**Impatto**  
*I commutatori virtuali seguenti non possono accedere a tutte le VLAN: \n @ no__t-1*  
  
## <a name="resolution"></a>**Soluzione**  
*Usare Server Manager o il cmdlet di Windows PowerShell set-NetLbfoTeamNic per reimpostare l'interfaccia del team sulla modalità predefinita.*  
  


