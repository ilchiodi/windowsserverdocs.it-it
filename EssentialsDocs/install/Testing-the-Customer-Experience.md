---
title: Test di Analisi utilizzo software
description: Viene descritto come usare Windows Server Essentials
ms.date: 10/03/2016
ms.prod: windows-server
ms.topic: article
ms.assetid: 1b1a2040-4cfd-48bf-8d04-3ffde9c26b9b
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 938b64d96111a3ed128ec184acde56f20cb1abda
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80819784"
---
# <a name="testing-the-customer-experience"></a>Test di Analisi utilizzo software

>Si applica a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

Per analizzare l'uso da parte del cliente e controllare le personalizzazioni del partner, eseguire la configurazione iniziale su un computer di destinazione. Si consiglia di completare la configurazione iniziale almeno una volta manualmente, per analizzare l'uso da parte del cliente. Se il dashboard è stato personalizzato, è necessario completare la configurazione iniziale per verificare la personalizzazione. Se il sito di Accesso Web remoto è stato personalizzato, è necessario accedere a http://< nomeserver\> per verificare la personalizzazione (< nomeserver\> è il nome del server). È possibile utilizzare la sezione di configurazione iniziale del file cfg.ini per automatizzare l'analisi dell'uso da parte del cliente. Per ulteriori informazioni sulla creazione di questa sezione nel file cfg.ini, vedere [Creazione del file Cfg.ini](Create-the-Cfg.ini-File.md).  
  
> [!IMPORTANT]
>  Per preparare l'immagine per la distribuzione, è necessario eseguire il comando Sysprep.exe prima di analizzare l'uso della configurazione iniziale. Per ulteriori informazioni sull'esecuzione di Sysprep.exe, vedere [Preparazione dell'immagine per la distribuzione](Preparing-the-Image-for-Deployment.md).  
  
> [!IMPORTANT]
>  Per verificare la configurazione iniziale, è necessaria una connessione di rete. DHCP non è configurato o installato sul server e consente la verifica della rete senza interferenze.  
  
 Per verificare le informazioni di supporto del partner, nel dashboard, fare clic sulla freccia giù accanto al pulsante ?.  
  
## <a name="see-also"></a>Vedi anche  
 [Creazione e personalizzazione dell'immagine](Creating-and-Customizing-the-Image.md)   
 [Personalizzazioni aggiuntive](Additional-Customizations.md)   
 [Preparazione dell'immagine per la distribuzione](Preparing-the-Image-for-Deployment.md)