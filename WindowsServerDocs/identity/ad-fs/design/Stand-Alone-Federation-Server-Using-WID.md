---
ms.assetid: 33b80a3f-67f3-4da7-ac4a-7fd2232fbd5d
title: Server federativo autonomo con database interno di Windows
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 9ec4150a7d3adfaac786219d253e1d0898c18204
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2017
---
# <a name="stand-alone-federation-server-using-wid"></a>Server federativo autonomo con database interno di Windows

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Un server federativo autonoma in Active Directory Federation Services \(AD FS\) è costituito da un singolo server che ospita un servizio federativo è configurato per l'utilizzo di \(WID\) Database interno di Windows. Questa topologia ADFS è per i laboratori di test. Non è consigliabile per gli ambienti di produzione perché ha un limite di un solo server federativo e non può essere utilizzato per applicare la scalabilità verticale a più server.  
  
Se si desidera aggiungere altri server federativi a un ambiente di laboratorio, è necessario ricreare il servizio federativo da zero distribuendo le topologie illustrate più avanti in questa sezione. È pertanto consigliabile utilizzare questa topologia per un ambiente di prova o di un ambiente concetto di of\ proof\ in rete di test privato in cui un server federativo singolo è adeguato, come illustrato nella figura seguente.  
  
![server con database interno di Windows](media/FedServerWID.gif)  
  
## <a name="test-lab-considerations"></a>Considerazioni sul laboratorio di test  
In questa sezione vengono descritte varie considerazioni sui destinatari, vantaggi e limitazioni di cui è associate a questa topologia per ambienti di laboratorio di test.  
  
### <a name="who-should-use-this-topology"></a>Chi dovrebbe utilizzare questa topologia?  
  
-   \(IT\) professionisti o architetti IT che desiderano valutare o sviluppare un modello di prova per questa tecnologia  
  
### <a name="what-are-the-benefits-of-using-this-topology"></a>Quali sono i vantaggi dell'uso di questa topologia?  
  
-   Semplice da configurare in un ambiente di testing  
  
### <a name="what-are-the-limitations-of-using-this-topology"></a>Quali sono i limiti di utilizzo di questa topologia?  
  
-   Un solo server federativo al servizio federativo \ (alcuna funzionalità di scalabilità verticale a un farm\)  
  
-   Non ridondanti \ (solo una singola istanza del exists\ database di configurazione AD FS)  
  

## <a name="see-also"></a>Vedere anche
[Guida alla progettazione di ADFS in Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
