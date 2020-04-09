---
title: Evitare di installare RemoteFX in un computer configurato come controller di dominio Active Directory
description: Versione online del testo per questa regola di Best Practices Analyzer.
ms.prod: windows-server
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: da58694e-91f6-45d8-a599-18966db165f4
author: kbdazure
ms.date: 8/16/2016
ms.openlocfilehash: 597dd26d0a317ca026cd94ab29138ce679d7c3b9
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80857764"
---
# <a name="avoid-installing-remotefx-on-a-computer-that-is-configured-as-an-active-directory-domain-controller"></a>Evitare di installare RemoteFX in un computer configurato come controller di dominio Active Directory

>Si applica a: Windows Server 2016

Per altre informazioni sulle procedure consigliate e sulle analisi, vedere [Eseguire analisi di Best Practice Analyzer e gestire i risultati delle analisi](https://go.microsoft.com/fwlink/p/?LinkID=223177).  
  
|Proprietà|Dettagli|  
|-|-|  
|**Sistema operativo**|Windows Server 2016|  
|**Prodotto/funzionalità**|Hyper-V|  
|**Gravità**|Errore|  
|**Categoria**|Configurazione|  
  
Nelle sezioni seguenti, corsivo indica il testo dell'interfaccia Utente visualizzata nello strumento Analizzatore procedure consigliate per questo problema.  
  
## <a name="issue"></a>**Problema**  
*RemoteFX è installato in un controller di dominio.*  
  
## <a name="impact"></a>**Impatto**  
*I computer virtuali configurati per RemoteFX non possono essere usati in questi computer.*  
  
## <a name="resolution"></a>**Soluzione**  
*Decidere se si desidera che il server sia configurato con RemoteFX per Hyper-V o come controller di Dominio di Active Directory, quindi riconfigurare il server in caso di necessità.*  
  


