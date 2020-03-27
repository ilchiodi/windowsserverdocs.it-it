---
title: 'Passaggio 3: Aggiungere computer al nuovo server Windows Server Essentials'
description: Viene descritto come usare Windows Server Essentials
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: a0e07d1a-8409-429b-87d7-0f4a7e14d668
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 1ca1e3a031c95f19fb68aadcf203b13fa39d7558
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/26/2020
ms.locfileid: "80318768"
---
# <a name="step-3-join-computers-to-the-new-windows-server-essentials-server"></a>Passaggio 3: Aggiungere computer al nuovo server Windows Server Essentials

>Si applica a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

Il passaggio successivo del processo di migrazione consiste nel connettere i computer client al nuovo server che esegue Windows Server Essentials.  
  
> [!NOTE]
>  È possibile saltare questo passaggio per i computer che eseguono i sistemi operativi Windows XP o Windows Vista. Il software Connettore Windows Server non supporta computer che eseguono Windows XP o Windows Vista.  
  
 Prima di poter aggiungere un computer client al nuovo server di Windows Server Essentials, è necessario disconnetterlo dal server di origine disinstallando il software connettore Windows Server nel computer client.  
  
### <a name="to-uninstall-windows-server-connector-on-a-client-computer"></a>Per disinstallare Connettore Windows Server in un computer client  
  
1.  In un computer client aprire il Pannello di controllo e quindi **Programmi e funzionalità**.  
  
2.  Nell'elenco dei programmi, fare clic con il pulsante destro del mouse sull'applicazione di connessione in esecuzione nel computer.  
  
    > [!NOTE]
    >  L'applicazione Connector può essere un connettore **Windows Small Business Server 2011 Essentials**o un **connettore di Windows Server Essentials**, a seconda della versione di Windows Server Essentials a cui il computer client era connesso.  
  
3.  Fare clic su **Disinstalla**.  
  
### <a name="to-reconnect-a-client-computer-to-the-server"></a>Per riconnettere un computer client al server  
  
1.  Accedere al computer che si vuole connettere al server.  
  
    > [!NOTE]
    >  Se questo computer ha più account utente, effettuare l’accesso con l’account utente per cui si vogliono mantenere i documenti, le immagini e le preferenze personali dopo avere connesso il computer al server.  
  
2.  Aprire un browser Internet, ad esempio Internet Explorer.  
  
3.  Nella barra degli indirizzi digitare **http://< nomeserver\>/Connect**, quindi premere INVIO.  
  
4.  Seguire le istruzioni visualizzate per aggiungere il computer client al nuovo server di Windows Server Essentials.  
  
## <a name="next-steps"></a>Passaggi successivi  
 I computer client sono stati aggiunti al nuovo server che esegue Windows Server Essentials. Andare a [passaggio 4: spostare le impostazioni e i dati nel server di destinazione per la migrazione a Windows Server Essentials](Step-4--Move-settings-and-data-to-the-Destination-Server-for-Windows-Server-Essentials-migration.md).  
  

Per visualizzare tutti i passaggi, vedere [eseguire la migrazione a Windows Server Essentials](Migrate-from-Previous-Versions-to-Windows-Server-Essentials-or-Windows-Server-Essentials-Experience.md).

