---
title: Aggiungere i computer al nuovo Server1 di Windows Server Essentials
description: Viene descritto come usare Windows Server Essentials
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: cdfa9504-9881-4265-b308-c7ee8721bfaa
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 6f7d20e2d74c311a34b98de7c33c755b5981fcef
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/26/2020
ms.locfileid: "80318970"
---
# <a name="join-computers-to-the-new-windows-server-essentials-server1"></a>Aggiungere i computer al nuovo Server1 di Windows Server Essentials

>Si applica a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

##  <a name="BKMK_JoinComputers"></a>   
 Il passaggio successivo del processo di migrazione consiste nell'aggiungere i computer client alla nuova rete di Windows Server Essentials e aggiornare le impostazioni Criteri di gruppo.  
  
> [!NOTE]
>  Se un computer client è già stato aggiunto al server di origine, è necessario disinstallare il software Connettere nel computer client prima di poter connettere il computer al server di destinazione.  
  
 Il processo per connettere un computer client al server è lo stesso per i computer aggiunti a un dominio o non aggiunti a un dominio:  
  
- Passare a **http://** <em>nome-server-destinazione</em> **/connect** e installare il software Connettore di Windows Server come se fosse un nuovo computer.  
  
> [!NOTE]
>  Il software Connettore Windows Server non supporta computer che eseguono Windows XP o Windows Vista. Se al dominio sono già stati aggiunti computer che eseguono Windows XP o Windows Vista, è possibile saltare questo passaggio.  
  
### <a name="ensure-that-group-policy-has-updated"></a>Verificare che Criteri di gruppo sia stato aggiornato  
  
> [!NOTE]
>  È un passaggio facoltativo, necessario solo se il server di origine è stato configurato con impostazioni di Criteri di gruppo personalizzate, come Reindirizzamento cartelle.  
  
 Mentre il server di origine e il server di destinazione sono ancora online, è consigliabile verificare che le impostazioni di Criteri di gruppo siano state replicate dal server di destinazione ai computer client. Eseguire i passaggi seguenti in ogni computer client:  
  
1.  Aprire una finestra del prompt dei comandi.  
  
2.  Al prompt dei comandi digitare **GPRESULT /R** e quindi premere INVIO.  
  
3.  Esaminare l'output risultante per la sezione Criteri di gruppo è stato applicato da: e verificare che sia elencato il server di destinazione, ad esempio **DestinationSrv. Domain. local**. Ad esempio,  
  
    ```  
    USER SETTINGS  
    --------------  
        CN=User,OU=Users,DC=DOMAIN,DC=Local  
        Last time Group Policy was applied: 1/24/2011 at 1:26:27 PM  
        Group Policy was applied from:      DestinationSrv.Domain.local  
        Group Policy slow link threshold:   500 kbps  
        Domain Name:                        Domain  
        Domain Type:                        Windows 2011  
  
    ```  
  
4.  Se il server di destinazione non è nell'elenco, al prompt dei comandi digitare **gpupdate /force** e quindi premere INVIO per aggiornare le impostazioni di Criteri di gruppo. Quindi eseguire di nuovo la procedura precedente.  
  
5.  Se il server di destinazione non è ancora visualizzato, potrebbe esserci un errore nelle impostazioni di Criteri di gruppo o nell'applicazione di tali impostazioni a questo specifico computer client. Se il server di destinazione non viene visualizzato, eseguire i passaggi seguenti:  
  
    1.  Fare clic su **Start**, scegliere **Esegui**, digitare **rsop.msc** (Gruppo di criteri risultante) e quindi premere INVIO.  
  
    2.  Espandere l'albero con la X fino a raggiungere un nodo.  
  
    3.  Fare clic con il pulsante destro del mouse sul nodo e scegliere **Visualizza errore** per conoscere il motivo per cui le impostazioni di Criteri di gruppo hanno esito negativo nel computer elencato.
