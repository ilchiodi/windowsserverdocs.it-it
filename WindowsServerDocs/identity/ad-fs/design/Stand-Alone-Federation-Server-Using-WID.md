---
ms.assetid: 33b80a3f-67f3-4da7-ac4a-7fd2232fbd5d
title: Server federativo autonomo che usa Database interno di Windows
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 31e2e1b04383adc8bec12e7290a7acec80e0402f
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/24/2019
ms.locfileid: "66190795"
---
# <a name="stand-alone-federation-server-using-wid"></a>Server federativo autonomo che usa Database interno di Windows

Un supporto\-server federativo solo in Active Directory Federation Services \(ADFS\) costituito da un singolo server che ospita un servizio federativo configurato per utilizzare Database interno di Windows \(WID\). Questa topologia ADFS è per i laboratori di test. Non è consigliabile per gli ambienti di produzione perché ha un limite di un solo server federativo e non può essere utilizzato per la scalabilità verticale a più server.  
  
Se si desidera aggiungere altri server federativi a un ambiente di laboratorio, è necessario ricompilare il servizio federativo da zero distribuendo le topologie illustrate più avanti in questa sezione. È pertanto consigliabile utilizzare questa topologia per un laboratorio di test o un modello di prova\-di\-ambiente della rete di test privato in cui un server federativo singolo è adeguato, come illustrato nella figura seguente.  
  
![server con database interno di Windows](media/FedServerWID.gif)  
  
## <a name="test-lab-considerations"></a>Considerazioni sul laboratorio di test  
Questa sezione vengono descritte varie considerazioni sui destinatari, vantaggi e limitazioni di cui è associate a questa topologia per ambienti di laboratorio di test.  
  
### <a name="who-should-use-this-topology"></a>Chi dovrebbe utilizzare questa topologia?  
  
-   IT \(IT\) professionisti o architetti IT che desiderano valutare o sviluppare un modello di prova per questa tecnologia  
  
### <a name="what-are-the-benefits-of-using-this-topology"></a>Quali sono i vantaggi dell'utilizzo di questa topologia?  
  
-   Semplice da configurare in un ambiente di testing  
  
### <a name="what-are-the-limitations-of-using-this-topology"></a>Quali sono le limitazioni dell'utilizzo di questa topologia?  
  
-   Un solo server federativo al servizio federativo \(alcuna funzionalità per la scalabilità verticale a una farm\)  
  
-   Non ridondante \(esiste una sola istanza del database di configurazione di AD FS\)  
  

## <a name="see-also"></a>Vedere anche
[Guida alla progettazione di AD FS in Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
