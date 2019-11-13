---
title: Integration Services deve essere installato prima che le macchine virtuali primarie o di replica possano usare un indirizzo IP alternativo dopo un failover
description: Versione online del testo per questa regola di Best Practices Analyzer, con collegamenti ad altre informazioni.
ms.prod: windows-server
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: a7fdd185-d6c8-4f58-9b58-2df5827bb056
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: 58e744c182fb2013e55e91f58140c6ba14181f9f
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71393585"
---
# <a name="integration-services-must-be-installed-before-primary-or-replica-virtual-machines-can-use-an-alternate-ip-address-after-a-failover"></a>Integration Services deve essere installato prima che le macchine virtuali primarie o di replica possano usare un indirizzo IP alternativo dopo un failover

>Si applica a: Windows Server 2016

Per altre informazioni sulle procedure consigliate e sulle analisi, vedere [Eseguire analisi di Best Practice Analyzer e gestire i risultati delle analisi](https://go.microsoft.com/fwlink/p/?LinkID=223177).  
  
|Proprietà|Dettagli|  
|-|-|  
|**Sistema operativo**|Windows Server 2016|  
|**Prodotto/funzionalità**|Hyper-V|  
|**Gravità**|Error|  
|**Categoria**|Configurazione|  
  
Nelle sezioni seguenti, corsivo indica il testo dell'interfaccia Utente visualizzata nello strumento Analizzatore procedure consigliate per questo problema.  
  
## <a name="issue"></a>Problema  
*Le macchine virtuali che partecipano alla replica possono essere configurate per l'utilizzo di un indirizzo IP specifico in caso di failover, ma solo se i servizi di integrazione sono installati nel sistema operativo guest della macchina virtuale.*  
  
## <a name="impact"></a>Impatto  
*In caso di failover (pianificato, non pianificato o di test), la macchina virtuale di replica verrà resa online utilizzando lo stesso indirizzo IP della macchina virtuale primaria. Questa configurazione potrebbe causare problemi di connettività. Ciò influisca sulle macchine virtuali seguenti:*  
  
\<elenco di macchine virtuali >  
  
## <a name="resolution"></a>Risoluzione  
*Usare Virtual Machine Connection per installare Integration Services nella macchina virtuale.*  
  
A partire da Windows Server 2016, i servizi di integrazione per le macchine virtuali Windows vengono distribuiti tramite Windows Update. Verificare che le macchine virtuali siano configurate in modo da ricevere gli aggiornamenti di Windows per ottenere la versione più recente di Integration Services. Il kernel Linux ora include Linux Integration Services (LIS) e viene aggiornato per le nuove versioni, ma le distribuzioni Linux basate su kernel precedenti potrebbero non avere i miglioramenti o le correzioni più recenti. Per informazioni dettagliate, vedere [macchine virtuali Linux e FreeBSD supportate per Hyper-V in Windows](../Supported-Linux-and-FreeBSD-virtual-machines-for-Hyper-V-on-Windows.md).


