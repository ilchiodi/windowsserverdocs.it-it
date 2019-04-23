---
title: L'interfaccia team associato a un commutatore virtuale deve essere in modalità predefinita
description: Versione online del testo per questa regola di Best Practices Analyzer.
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 8c118e1e-865f-4cff-acdc-7c35e45d5da9
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: 22e5ad0eed6e6ea07a83150762b76163442f2c5c
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59872162"
---
# <a name="the-team-interface-bound-to-a-virtual-switch-should-be-in-default-mode"></a>L'interfaccia team associato a un commutatore virtuale deve essere in modalità predefinita

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
*Alcuni switch virtuali sono associate a un'interfaccia del team, ma l'interfaccia del team non passa il traffico in tutte le VLAN per i commutatori virtuali.*  
  
## <a name="impact"></a>**Impact**  
*I commutatori virtuali seguenti non possono avere accesso a tutte le VLAN: \n{0}*  
  
## <a name="resolution"></a>**Soluzione**  
*Usare Server Manager o il cmdlet di Windows PowerShell Set-NetLbfoTeamNic per reimpostare l'interfaccia del team per la modalità predefinita.*  
  


