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
ms.openlocfilehash: 62f31f859ed3fd0f77baf37d3467d4702b24ad95
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/31/2019
ms.locfileid: "66432910"
---
# <a name="join-computers-to-the-new-windows-server-essentials-network1"></a>Aggiungere i computer per il nuovo network1 di Windows Server Essentials

>Si applica a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

##  <a name="BKMK_JoinComputers"></a>   
 Il passaggio successivo del processo di migrazione è aggiungere i computer client alla nuova rete Windows Server Essentials e aggiornare le impostazioni di criteri di gruppo.  
  
### <a name="domain-joined-client-computers"></a>Computer client aggiunti a un dominio  
 Passare a **http://** <em>nome-server-destinazione</em> **/connect** e installare il software Connettore di Windows Server come se fosse un nuovo computer. Il processo di installazione è lo stesso per i computer client aggiunti a un dominio o non aggiunti a un dominio.  
  
> [!NOTE]
>  Il software Connettore Windows Server non supporta computer che eseguono Windows XP o Windows Vista. Se al dominio sono già stati aggiunti computer che eseguono Windows XP o Windows Vista, è possibile saltare questo passaggio.  
  
### <a name="non-domain-joined-client-computers"></a>Computer client non aggiunti a un dominio  
 Passare a **http://** <em>nome-server-destinazione</em> **/connect** e installare il software Connettore di Windows Server come se fosse un nuovo computer. Il processo di installazione è lo stesso per i computer client aggiunti a un dominio o non aggiunti a un dominio.  
  
> [!NOTE]
>  Il software Connettore Windows Server non supporta computer che eseguono Windows XP o Windows Vista. Se al dominio sono già stati aggiunti computer che eseguono Windows XP o Windows Vista, è possibile saltare questo passaggio.  
  
### <a name="ensure-that-group-policy-has-updated"></a>Verificare che Criteri di gruppo sia stato aggiornato  
  
> [!NOTE]
>  È un passaggio facoltativo, necessario solo se il server di origine è stato configurato con impostazioni di Criteri di gruppo personalizzate, come Reindirizzamento cartelle.  
  
 Mentre il server di origine e il server di destinazione sono ancora online, è consigliabile verificare che le impostazioni di Criteri di gruppo siano state replicate dal server di destinazione ai computer client. Eseguire i passaggi seguenti in ogni computer client:  
  
1.  Aprire una finestra del prompt dei comandi.  
  
2.  Al prompt dei comandi digitare **GPRESULT /R** e quindi premere INVIO.  
  
3.  Esaminare l'output risultante per la sezione criteri di gruppo applicato da: e verificare che sia elencato il Server di destinazione, ad esempio **Destinationsrv**. Ad esempio:  
  
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
  
4.  Se il server di destinazione non è nell'elenco, al prompt dei comandi digitare **gpupdate /force**e quindi premere INVIO per aggiornare le impostazioni di Criteri di gruppo. Quindi eseguire di nuovo la procedura precedente.  
  
5.  Se il server di destinazione non è ancora visualizzato, potrebbe esserci un errore nelle impostazioni di Criteri di gruppo o nell'applicazione di tali impostazioni a questo specifico computer client. Se il server di destinazione non viene visualizzato, eseguire i passaggi seguenti:  
  
    1.  Fare clic su **Start**, scegliere **Esegui**, digitare **rsop.msc** (Gruppo di criteri risultante) e quindi premere INVIO.  
  
    2.  Espandere l'albero con la X su di esso fino ad arrivare a un nodo.  
  
    3.  Fare clic con il pulsante destro del mouse sul nodo e scegliere **Visualizza errore** per conoscere il motivo per cui le impostazioni di Criteri di gruppo hanno esito negativo nel computer elencato.
