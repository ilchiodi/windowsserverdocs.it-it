---
author: shortpatti
ms.author: pashort
ms.date: 10/02/2018
ms.prod: windows-server-threshold
ms:topic: include
ms.openlocfilehash: 4e8e3234b89630bf16148eef644f0c6607ad38bd
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59852342"
---
## <a name="importance-of-time-protocols"></a>Importanza dei protocolli ora
Protocolli ora le comunicazioni tra due computer per lo scambio di informazioni sull'ora e quindi usare tali informazioni per sincronizzare gli orologi. Con il protocollo ora servizio ora di Windows, un client richiede informazioni sull'ora da un server e consente di sincronizzare l'orologio sulla base delle informazioni che viene ricevute.
  
Il servizio ora di Windows utilizza NTP per sincronizzare l'ora in una rete. NTP è un protocollo di tempo Internet che include gli algoritmi di disciplina necessari per la sincronizzazione degli orologi. NTP è un protocollo di tempo più accurato rispetto il Simple Network Time Protocol (SNTP) che viene utilizzato in alcune versioni di Windows; tuttavia W32Time continua a supportare SNTP per abilitare la compatibilità con computer che eseguono servizi basati su SNTP tempo, ad esempio Windows 2000.

Esistono molte ragioni diverse, che potrebbe essere necessario ora esatta.  Il caso tipico per Windows è Kerberos, che richiede 5 minuti di accuratezza tra il client e server.  Tuttavia, esistono molte altre aree che possono essere influenzati dal tempo accuratezza inclusi:


- Normative come:
    - accuratezza 50 ms per FINRA negli Stati Uniti
    - 1 ms ESMA (MiFID II) dell'Unione europea.
- Algoritmi di crittografia
- Sistemi distribuiti come Cluster/SQL/Exchange e di database di documenti
- Blockchain framework per le transazioni bitcoin
- I log distribuiti e l'analisi delle minacce 
- Replica di Active Directory
- PCI (Payment Card settore), attualmente 1 secondo accuratezza