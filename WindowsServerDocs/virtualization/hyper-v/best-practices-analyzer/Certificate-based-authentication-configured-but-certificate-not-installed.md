---
title: È configurata l'autenticazione basata su certificato, ma il certificato specificato non è installato nel server di replica o nei nodi del cluster di failover
description: Versione online del testo per questa regola di Best Practices Analyzer.
ms.prod: windows-server
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 4cabbce3-9367-4ddc-a108-1e5e1ab2bcff
author: kbdazure
ms.date: 8/16/2016
ms.openlocfilehash: cc89aa201de4b25e4c221b770e6f88908785c859
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80857684"
---
# <a name="certificate-based-authentication-is-configured-but-the-specified-certificate-is-not-installed-on-the-replica-server-or-failover-cluster-nodes"></a>È configurata l'autenticazione basata su certificato, ma il certificato specificato non è installato nel server di replica o nei nodi del cluster di failover

>Si applica a: Windows Server 2016


  
*Per ulteriori informazioni sulle procedure consigliate e le analisi, vedere* [Best Practices Analyzer](https://go.microsoft.com/fwlink/?LinkId=122786).  
  
|Proprietà|Dettagli|  
|-|-|  
|**Sistema operativo**|Windows Server 2016|  
|**Prodotto/funzionalità**|Hyper-V|  
|**Gravità**|Errore|  
|**Categoria**|Configurazione|  

Nelle sezioni seguenti, corsivo indica il testo dell'interfaccia Utente visualizzata nello strumento Analizzatore procedure consigliate per questo problema.

## <a name="issue"></a>Problema  
  
*Il certificato di sicurezza configurato per la replica Hyper-V per fornire la replica basata su certificato non è installato nel server di replica (o in qualsiasi nodo del cluster di failover).*  
  
## <a name="impact"></a>Impatto  
  
*In caso di failover di un cluster o di spostamento in un altro nodo, la replica Hyper-V viene sospesa se nel nuovo nodo non è installato anche il certificato appropriato. Ciò influisca sui nodi seguenti:*  
  
\<elenco dei nodi >  
  
## <a name="resolution"></a>Risoluzione  
  
*Installare il certificato configurato nel server di replica (e in tutti i nodi associati nel cluster di failover, se presente).*  
  


