---
title: Aggiungere i computer per il nuovo network1 di Windows Server Essentials
description: Viene descritto come utilizzare Windows Server Essentials
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: d94de050-3300-4323-a5ea-c824cb9cecc9
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: c6abc11ba2ce8a9f1d32c6a884db6332586de78b
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2017
---
# <a name="join-computers-to-the-new-windows-server-essentials-network1"></a>Aggiungere i computer per il nuovo network1 di Windows Server Essentials

>Si applica a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

##  <a name="BKMK_JoinComputers"></a>   
 Il passaggio successivo del processo di migrazione è per aggiungere i computer client alla nuova rete Windows Server Essentials e aggiornare le impostazioni di criteri di gruppo.  
  
### <a name="domain-joined-client-computers"></a>Computer client appartenenti al dominio  
 Passare a **http://***nome-server-destinazione***/ connect** e installare il software connettore Windows Server come se fosse un nuovo computer. Il processo di installazione è lo stesso per i computer client appartenenti a un dominio o non aggiunti al dominio.  
  
> [!NOTE]
>  Il software connettore Windows Server non supporta computer che eseguono Windows XP o Windows Vista. Se si dispone di computer che eseguono Windows XP o Windows Vista che sono già connessi al dominio, è possibile ignorare questo passaggio.  
  
### <a name="non-domain-joined-client-computers"></a>Computer client non aggiunti al dominio  
 Passare a **http://***nome-server-destinazione***/ connect** e installare il software connettore Windows Server come se fosse un nuovo computer. Il processo di installazione è lo stesso per i computer client aggiunti non appartenenti al dominio o di dominio.  
  
> [!NOTE]
>  Il software connettore Windows Server non supporta computer che eseguono Windows XP o Windows Vista. Se si dispone di computer che eseguono Windows XP o Windows Vista che sono già connessi al dominio, è possibile ignorare questo passaggio.  
  
### <a name="ensure-that-group-policy-has-updated"></a>Assicurarsi che sia aggiornato criteri di gruppo  
  
> [!NOTE]
>  Questo passaggio è facoltativo ed è solo necessario se il Server di origine è stato configurato con impostazioni di criteri di gruppo personalizzate, come reindirizzamento cartelle.  
  
 Anche se il Server di origine e il Server di destinazione sono ancora online, è necessario assicurarsi che i criteri di gruppo impostazioni siano state replicate dal Server di destinazione per i computer client. In ogni computer client, procedere come segue:  
  
1.  Aprire una finestra del prompt dei comandi.  
  
2.  Al prompt dei comandi, digitare **GPRESULT /R**, quindi premere INVIO.  
  
3.  Esaminare l'output risultante per la sezione criteri di gruppo applicato da: e verificare che sia elencato il Server di destinazione, ad esempio **DestinationSrv.Domain.local**. Per esempio:  
  
    ```  
    USER SETTINGS  
    --------------  
        CN=User,OU=Users,DC=DOMAIN,DC=Local  
        Last time Group Policy was applied: 1/24/2011 at 1:26:27 PM  
        Group Policy was applied from:      DestinationSrv.Domain.local  
        Group Policy slow link threshold:   500 kbps  
        Domain Name:                        Domain  
        Domain Type:                        Windows 2008  
  
    ```  
  
4.  Se il Server di destinazione non è elencato, al prompt dei comandi, digitare **gpupdate /force**, quindi premere INVIO per aggiornare le impostazioni di criteri di gruppo. Quindi eseguire nuovamente la procedura precedente.  
  
5.  Se il Server di destinazione non è ancora visualizzato, potrebbe trattarsi di un errore nelle impostazioni di criteri di gruppo o un errore nell'applicazione di tali al computer client specifico. Se il Server di destinazione non è visualizzato, eseguire i passaggi seguenti:  
  
    1.  Fare clic su **Start**, fare clic su **eseguire**, tipo **rsop.msc** (gruppo di criteri risultante), quindi premere INVIO.  
  
    2.  Espandere l'albero con la X su di esso finché non viene visualizzato un nodo.  
  
    3.  Pulsante destro del mouse sul nodo e fare clic su **Visualizza errore** per informazioni sui motivi per cui le impostazioni di criteri di gruppo hanno esito negativo nel computer elencato.