---
title: 'Passaggio 2: Installare Windows Server Essentials come nuovo controller di dominio di replica'
description: Viene descritto come usare Windows Server Essentials
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: c7ccfc34-63fd-436b-a1cd-e05810f60bfe
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 5968db77c091dbca1eb7d38f5e924e5f449052ce
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/26/2020
ms.locfileid: "80318770"
---
# <a name="step-2-install-windows-server-essentials-as-a-new-replica-domain-controller"></a>Passaggio 2: Installare Windows Server Essentials come nuovo controller di dominio di replica

>Si applica a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

Questa sezione descrive come installare Windows Server Essentials e Windows Server 2012 R2 Standard (con il ruolo esperienza Windows Server Essentials abilitato) come controller di dominio.  
  
 Per gli ambienti con un massimo di 25 utenti e 50 dispositivi, è possibile seguire i passaggi descritti in questa guida per eseguire la migrazione da versioni precedenti di Windows SBS a Windows Server Essentials. Per gli ambienti con un massimo di 100 utenti e 200 dispositivi, è possibile seguire le stesse linee guida per eseguire la migrazione alle edizioni standard e Datacenter di Windows Server 2012 R2 con il ruolo esperienza Windows Server Essentials installato. In questa documentazione sono illustrati entrambi gli scenari.  
  
> [!IMPORTANT]
>  Se si esegue la migrazione a Windows Server Essentials, il messaggio di errore seguente viene aggiunto al registro eventi ogni giorno durante il periodo di tolleranza di 21 giorni finché non si rimuove il server di origine dalla rete. Dopo il periodo di prova di 21 giorni, il server di origine verrà arrestato. <br> **Il controllo del ruolo FSMO ha rilevato una condizione nell'ambiente che non è conforme ai criteri di licenza. Il server di gestione deve disporre dei ruoli Active Directory principale del controller di dominio e della denominazione dei domini. Spostare ora i ruoli di Active Directory nel server di gestione. Questo server verrà automaticamente arrestato se il problema non viene risolto entro 21 giorni dal momento in cui questa condizione è stata rilevata per la prima volta**.   
  
#### <a name="install-windows-server-essentials-or-windows-server-2012-r2-standard-on-the-destination-server"></a>Installare Windows Server Essentials o Windows Server 2012 R2 Standard nel server di destinazione  
  
1.  Installare Windows Server Essentials o Windows Server 2012 R2 Standard con il ruolo esperienza Windows Server Essentials abilitato seguendo le istruzioni riportate in [installare e configurare Windows Server Essentials](../install/Install-and-Configure-Windows-Server-Essentials-or-Windows-Server-Essentials-Experience.md).  
  
    > [!NOTE]
    >  Se viene avviata la procedura guidata Configurazione di Windows Server Essentials, annullarla.  
  
2.  Trasferire i ruoli FSMO dal server di origine.  
  
    > [!NOTE]
    >  Se Windows Server Essentials è l'unico controller di dominio nel dominio, il ruolo FSMO viene spostato automaticamente nel server che esegue Windows Server Essentials quando si abbassa di un server di origine.  
  
3.  Aprire Server Manager ed eseguire l'Aggiunta guidata ruoli e funzionalità.  
  
4.  Se non è installato, aggiungere il ruolo Esperienza Windows Server Essentials.  
  
5.  Dopo aver installato il ruolo Esperienza Windows Server Essentials, l'attività di configurazione di Windows Server Essentials viene visualizzata nell'area di notifica. Fare clic sull'attività per avviare la procedura guidata Configurazione di Windows Server Essentials.  
  
6.  Seguire le istruzioni per completare la configurazione di Windows Server Essentials. Prima di eseguire la procedura guidata, eseguire le operazioni seguenti:  
  
    -   Modificare il nome del server se necessario, perché non è possibile modificare il nome dopo aver completato la procedura guidata Configurazione di Windows Server Essentials.  
  
    -   Verificare che l'ora e le impostazioni del server siano corrette.  
  
7.  Verificare l'installazione nel modo seguente:  
  
    1.  Aprire il Dashboard.  
  
    2.  Fare clic sulla scheda **Utenti** e verificare che siano elencati gli account utente presenti in Active Directory.  
  
### <a name="transfer-the-operations-master-roles"></a>Trasferire i ruoli di master operazioni  
 I ruoli master operazioni, detti anche FSMO (Flexible Single Master Operation), devono essere trasferiti dal server di origine al server di destinazione entro 21 giorni dall'installazione di Windows Server Essentials nel server di destinazione.  
  
##### <a name="to-transfer-the-operations-master-roles"></a>Per trasferire i ruoli di master operazioni  
  
1.  Nel server di destinazione aprire una finestra del prompt dei comandi come amministratore. Vedere [Per aprire una finestra Prompt dei comandi come amministratore](https://technet.microsoft.com/library/cc947813\(v=WS.10\).aspx).  
  
2.  Al prompt dei comandi digitare **NETDOM QUERY FSMO**, quindi premere INVIO.  
  
3.  Al prompt dei comandi digitare **ntdsutil**, quindi premere INVIO.  
  
4.  Al prompt dei comandi **ntdsutil** immettere i comandi seguenti:  
  
    1.  Digitare **activate instance NTDS** e quindi premere INVIO.  
  
    2.  Digitare **roles**, quindi premere INVIO.  
  
    3.  Digitare **connections**, quindi premere INVIO.  
  
    4.  Digitare **Connetti al server** *< nomeserver\>* (dove *< nomeserver\>* è il nome del server di destinazione), quindi premere INVIO.  
  
    5.  Al prompt dei comandi digitare **q**, quindi premere INVIO.  
  
        1.  Digitare **transfer PDC**, premere INVIO e quindi fare clic su **Sì** nella **Finestra di conferma Trasferimento ruoli**.  
  
        2.  Digitare **transfer infrastructure master**, premere INVIO e quindi fare clic su **Sì** nella **Finestra di conferma Trasferimento ruoli**.  
  
        3.  Digitare **transfer naming master**, premere INVIO e quindi fare clic su **Sì** nella **Finestra di conferma Trasferimento ruoli**.  
  
        4.  Digitare **transfer RID master**, premere INVIO e quindi fare clic su **Sì** nella **Finestra di conferma Trasferimento ruoli**.  
  
        5.  Digitare **transfer schema master**, premere INVIO e quindi fare clic su **Sì** nella **Finestra di conferma Trasferimento ruoli**.  
  
    6.  Digitare **q**, quindi premere INVIO fino a tornare al prompt dei comandi.  
  
> [!NOTE]
>  Da qualsiasi server in rete, è possibile verificare che i ruoli di master operazioni siano stati trasferiti al server di destinazione. Aprire una finestra del prompt dei comandi come amministratore (per altre informazioni, vedere [Per aprire una finestra Prompt dei comandi come amministratore](https://technet.microsoft.com/library/cc947813\(v=WS.10\).aspx)). Digitare **netdom query fsmo**, quindi premere INVIO.  
  
## <a name="next-steps"></a>Passaggi successivi  
 Windows Server Essentials è stato installato come nuovo controller di dominio di replica. Passare quindi a [passaggio 3: aggiungere i computer al nuovo server Windows Server Essentials](Step-3--Join-computers-to-the-new-Windows-Server-Essentials-server.md).  
  
Per visualizzare tutti i passaggi, vedere [eseguire la migrazione a Windows Server Essentials](Migrate-from-Previous-Versions-to-Windows-Server-Essentials-or-Windows-Server-Essentials-Experience.md).

