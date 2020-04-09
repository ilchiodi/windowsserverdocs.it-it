---
title: L'interfaccia team associato a un commutatore virtuale deve essere in modalità predefinita
description: Versione online del testo per questa regola di Best Practices Analyzer.
ms.prod: windows-server
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 8c118e1e-865f-4cff-acdc-7c35e45d5da9
author: kbdazure
ms.date: 8/16/2016
ms.openlocfilehash: fe19de5dd380d08c01c917da9d4e2ef9465de042
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80854604"
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
*I commutatori virtuali seguenti non possono accedere a tutte le VLAN: \n{0}*  
  
## <a name="resolution"></a>**Soluzione**  
*Usare Server Manager o il cmdlet di Windows PowerShell set-NetLbfoTeamNic per reimpostare l'interfaccia del team sulla modalità predefinita.*  
  


