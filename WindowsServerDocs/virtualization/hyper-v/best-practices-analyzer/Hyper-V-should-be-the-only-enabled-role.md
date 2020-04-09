---
title: Hyper-V deve essere il solo ruolo abilitato
description: Vengono fornite istruzioni per risolvere il problema segnalato da questa regola di Best Practices Analyzer.
ms.prod: windows-server
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 5a0ed176-048f-40b1-b56c-8391b805fd37
author: kbdazure
ms.date: 8/16/2016
ms.openlocfilehash: 40dae4baad782d3be4fc6ca8ba2bc4e506131dbf
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80861914"
---
# <a name="hyper-v-should-be-the-only-enabled-role"></a>Hyper-V deve essere il solo ruolo abilitato

>Si applica a: Windows Server 2016

Per altre informazioni sulle procedure consigliate e le analisi, vedere [Best Practices Analyzer](https://go.microsoft.com/fwlink/?LinkId=122786).  
  
|Proprietà|Dettagli|  
|-|-|  
|**Sistema operativo**|Windows Server 2016|  
|**Prodotto/funzionalità**|Hyper-V|  
|**Gravità**|Avviso|  
|**Categoria**|Configurazione|  
  
Nelle sezioni seguenti, corsivo indica il testo dell'interfaccia Utente visualizzata nello strumento Analizzatore procedure consigliate per questo problema.  
  
## <a name="issue"></a>Problema  
  
*I ruoli diversi da Hyper-V sono abilitati in questo server.*  
  
Nella maggior parte dei casi, non è consigliabile installare altri ruoli in un server che esegue il ruolo Hyper-V. Servizio ruolo Host di virtualizzazione Desktop remoto è un'eccezione, perché fa parte del ruolo Servizi Desktop remoto e richiede Hyper-V venga installato nello stesso server.  
  
## <a name="impact"></a>Impatto  
  
*Il ruolo Hyper-V deve essere l'unico ruolo abilitato in un server.*  
  
Questa procedura consigliata consente di mantenere il sistema operativo host privo di ruoli, funzionalità e le applicazioni che non sono necessari per eseguire Hyper-V. Seguendo questa procedura consigliata e in esecuzione Hyper-V nel Server di Nano consente di ridurre il numero di aggiornamenti che è necessario perché solo Nano Server, i componenti di servizio Hyper-V e Windows hypervisor sarebbe soggetto agli aggiornamenti software.  
  
## <a name="resolution"></a>Risoluzione  
  
*Utilizzare Server Manager per rimuovere tutti i ruoli ad eccezione di Hyper-V.*  
  
Server Manager include la rimozione guidata ruoli. Questa procedura guidata consente di rimuovere più ruoli contemporaneamente. Prima di rimuovere ruoli, la rimozione guidata ruoli controlla le dipendenze, per ridurre il rischio di rimozione di software da cui dipendono altri ruoli. Se sono state trovate dipendenze, la procedura guidata viene richiesto di approvare la rimozione di altri ruoli, servizi ruolo o software necessari per i ruoli installati.   
  
Per utilizzare Server Manager, è necessario essere connessi al computer come amministratore.  
  
#### <a name="to-remove-a-role"></a>Per rimuovere un ruolo  
  
1.  Aprire Server Manager utilizzando i tasti di scelta rapida nel **avviare** dal menu della barra delle applicazioni Windows o in strumenti di amministrazione.  
2.   Nel **Riepilogo ruoli** area della finestra principale di Server Manager, fare clic su **Rimuovi ruoli**. Seguire le istruzioni della procedura guidata per rimuovere il ruolo.   
  
  
  


