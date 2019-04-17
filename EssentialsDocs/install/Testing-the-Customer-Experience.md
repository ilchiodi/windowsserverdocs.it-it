---
title: Test di analisi utilizzo software
description: Viene descritto come utilizzare Windows Server Essentials
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 1b1a2040-4cfd-48bf-8d04-3ffde9c26b9b
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 223b0e1be3a53e9a7d198dc005fc8725e421db58
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2017
---
# <a name="testing-the-customer-experience"></a>Test di analisi utilizzo software

>Si applica a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

Per verificare l'esperienza del cliente e controllare le personalizzazioni del partner, eseguire la configurazione iniziale del computer di destinazione. È consigliabile completare la configurazione iniziale almeno una volta manualmente per analizzare l'esperienza dei clienti. Se è stato personalizzato il Dashboard è necessario completare la configurazione iniziale per verificare la personalizzazione. Se è stato personalizzato il sito accesso Web remoto, è necessario accedere http://<servername\> per verificare la personalizzazione (< servername\ > è il nome del server). È possibile utilizzare la sezione di configurazione iniziale del file cfg.ini per automatizzare i test dell'esperienza cliente. Per ulteriori informazioni sulla creazione di questa sezione nel file cfg.ini vedere [creazione del Cfg.ini File](Create-the-Cfg.ini-File.md).  
  
> [!IMPORTANT]
>  È necessario eseguire il comando Sysprep.exe per preparare l'immagine per la distribuzione prima di testare l'esperienza di configurazione iniziale. Per ulteriori informazioni sull'esecuzione di Sysprep.exe, vedere [preparazione dell'immagine per la distribuzione](Preparing-the-Image-for-Deployment.md).  
  
> [!IMPORTANT]
>  Per testare la configurazione iniziale, è necessaria una connessione di rete. DHCP non è configurato o installato sul server, che consente di verifica della rete senza interferenze.  
  
 Per verificare le informazioni di supporto, il partner nel Dashboard, fare clic sulla freccia accanto al pulsante?.  
  
## <a name="see-also"></a>Vedere anche  
 [Creazione e personalizzazione dell'immagine](Creating-and-Customizing-the-Image.md)   
 [Ulteriori personalizzazioni](Additional-Customizations.md)   
 [Preparazione dell'immagine per la distribuzione](Preparing-the-Image-for-Deployment.md)