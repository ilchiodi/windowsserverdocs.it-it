---
title: Configurare il server con una quantità sufficiente di indirizzi MAC dinamici
description: Versione online del testo per questa regola di Best Practices Analyzer.
ms.prod: windows-server
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: a2804519-9790-4006-80b6-e990a8f505fe
author: kbdazure
ms.date: 8/16/2016
ms.openlocfilehash: 1fe7b34abc8be9777a491a08016627e7b2a66a2a
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80862044"
---
# <a name="configure-the-server-with-a-sufficient-amount-of-dynamic-mac-addresses"></a>Configurare il server con una quantità sufficiente di indirizzi MAC dinamici

>Si applica a: Windows Server 2016

*Questo argomento è destinato a risolvere un problema specifico identificato da un'analisi Best Practices Analyzer. È consigliabile applicare le informazioni contenute in questo argomento solo ai computer in cui è stata eseguita la Best Practices Analyzer Hyper-V e si è verificato il problema trattato in questo argomento. Per ulteriori informazioni sulle procedure consigliate e le analisi, vedere* [Best Practices Analyzer](https://go.microsoft.com/fwlink/?LinkId=122786).  
  
|Proprietà|Dettagli|  
|-|-|  
|**Sistema operativo**|Windows Server 2016|  
|**Prodotto/funzionalità**|Hyper-V|  
|**Gravità**|Avviso|  
|**Categoria**|Configurazione|  
  
Nelle sezioni seguenti, corsivo indica il testo dell'interfaccia Utente visualizzata nello strumento Analizzatore procedure consigliate per questo problema.  
  
## <a name="issue"></a>Problema  
  
*Il numero di indirizzi MAC dinamici disponibili è basso.*  
  
## <a name="impact"></a>Impatto  
  
*Quando non sono disponibili indirizzi MAC dinamici, non è possibile avviare le macchine virtuali configurate per l'utilizzo di un indirizzo MAC dinamico.*  
  
## <a name="resolution"></a>Risoluzione  
  
*Utilizzare Virtual Switch Manager per visualizzare ed estendere l'intervallo di indirizzi dinamici.*  
  


