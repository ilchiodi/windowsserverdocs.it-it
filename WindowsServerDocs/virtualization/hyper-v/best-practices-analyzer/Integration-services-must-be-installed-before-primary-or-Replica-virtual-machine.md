---
title: Servizi di integrazione devono essere installati prima del primario o macchine virtuali di Replica è possibile usare un indirizzo IP alternativo dopo un failover
description: Versione online del testo per questa regola di Best Practices Analyzer, con collegamenti a ulteriori informazioni.
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: a7fdd185-d6c8-4f58-9b58-2df5827bb056
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: 1ff8dbfd71655aee86ba7d0feac87ec2267a2171
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59865512"
---
# <a name="integration-services-must-be-installed-before-primary-or-replica-virtual-machines-can-use-an-alternate-ip-address-after-a-failover"></a>Servizi di integrazione devono essere installati prima del primario o macchine virtuali di Replica è possibile usare un indirizzo IP alternativo dopo un failover

>Si applica a: Windows Server 2016

Per altre informazioni sulle procedure consigliate e sulle analisi, vedere [Eseguire analisi di Best Practice Analyzer e gestire i risultati delle analisi](https://go.microsoft.com/fwlink/p/?LinkID=223177).  
  
|Proprietà|Dettagli|  
|-|-|  
|**Sistema operativo**|Windows Server 2016|  
|**/ Funzionalità del prodotto**|Hyper-V|  
|**Severity**|Errore|  
|**Categoria**|Configurazione|  
  
Nelle sezioni seguenti, corsivo indica il testo dell'interfaccia Utente visualizzata nello strumento Analizzatore procedure consigliate per questo problema.  
  
## <a name="issue"></a>Problema  
*Le macchine virtuali che fanno parte di replica può essere configurato per usare un indirizzo IP specifico in caso di failover, ma solo se sono installati i servizi di integrazione nel sistema operativo guest della macchina virtuale.*  
  
## <a name="impact"></a>Impatto  
*Nel caso di un failover (pianificato, pianificato o di test), la macchina virtuale di Replica verrà portato online tramite lo stesso indirizzo IP della macchina virtuale primaria. Questa configurazione potrebbe causare problemi di connettività. Questo influisce sulle macchine virtuali seguenti:*  
  
\<elenco di macchine virtuali >  
  
## <a name="resolution"></a>Risoluzione  
*Per installare integration services nella macchina virtuale, usare connessione macchina virtuale.*  
  
A partire da Windows Server 2016, servizi di integrazione per le macchine virtuali di Windows vengono forniti tramite Windows Update. Assicurarsi che queste macchine virtuali sono configurate per ricevere gli aggiornamenti di Windows per ottenere la versione più recente di integration services. Il kernel Linux ora include Linux integration services (LIS) e viene aggiornato per le nuove versioni, ma le distribuzioni di Linux basate su kernel precedenti potrebbero non offrire i più recenti miglioramenti o correzioni. Per informazioni dettagliate, vedere [macchine virtuali Linux e FreeBSD supportate per Hyper-V in Windows](../Supported-Linux-and-FreeBSD-virtual-machines-for-Hyper-V-on-Windows.md).


