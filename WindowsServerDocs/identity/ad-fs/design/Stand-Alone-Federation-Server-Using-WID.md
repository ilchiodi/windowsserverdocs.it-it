---
ms.assetid: 33b80a3f-67f3-4da7-ac4a-7fd2232fbd5d
title: Server federativo autonomo che usa Database interno di Windows
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 7253691cff4cc83032e4a345682ca1e43bafddc4
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80858774"
---
# <a name="stand-alone-federation-server-using-wid"></a>Server federativo autonomo che usa Database interno di Windows

Un server federativo autonomo\-in Active Directory Federation Services \(AD FS\) è costituito da un singolo server che ospita un Servizio federativo configurato per l'uso del database interno di Windows \(WID\). Questa topologia ADFS è per i laboratori di test. Non è consigliabile per gli ambienti di produzione perché ha un limite di un solo server federativo e non può essere utilizzato per la scalabilità verticale a più server.  
  
Se si desidera aggiungere altri server federativi a un ambiente di laboratorio, è necessario ricompilare il servizio federativo da zero distribuendo le topologie illustrate più avanti in questa sezione. È pertanto consigliabile utilizzare questa topologia per un laboratorio di test o un modello di prova\-di\-ambiente della rete di test privato in cui un server federativo singolo è adeguato, come illustrato nella figura seguente.  
  
![server con database interno di Windows](media/FedServerWID.gif)  
  
## <a name="test-lab-considerations"></a>Considerazioni sul laboratorio di test  
Questa sezione vengono descritte varie considerazioni sui destinatari, vantaggi e limitazioni di cui è associate a questa topologia per ambienti di laboratorio di test.  
  
### <a name="who-should-use-this-topology"></a>Chi dovrebbe utilizzare questa topologia?  
  
-   IT \(IT\) professionisti o architetti IT che desiderano valutare o sviluppare un modello di prova per questa tecnologia  
  
### <a name="what-are-the-benefits-of-using-this-topology"></a>Quali sono i vantaggi dell'utilizzo di questa topologia?  
  
-   Semplice da configurare in un ambiente di testing  
  
### <a name="what-are-the-limitations-of-using-this-topology"></a>Quali sono le limitazioni dell'utilizzo di questa topologia?  
  
-   Un solo server federativo per Servizio federativo \(nessuna funzionalità per la scalabilità verticale a una farm\)  
  
-   Non ridondante \(è presente una sola istanza del database di configurazione AD FS\)  
  

## <a name="see-also"></a>Vedi anche
[Guida alla progettazione di AD FS in Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
