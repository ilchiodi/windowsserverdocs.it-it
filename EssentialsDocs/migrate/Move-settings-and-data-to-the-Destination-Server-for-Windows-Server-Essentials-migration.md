---
title: Spostare dati e impostazioni nel server di destinazione per la migrazione a Windows Server Essentials
description: Viene descritto come usare Windows Server Essentials
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 2b882e87-347a-4010-b7fd-9599d61198dd
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 4f4ba08c17429f70ef754b0861553e38ba116e5d
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/26/2020
ms.locfileid: "80318834"
---
# <a name="move-settings-and-data-to-the-destination-server-for-windows-server-essentials-migration"></a>Spostare dati e impostazioni nel server di destinazione per la migrazione a Windows Server Essentials

>Si applica a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

Spostare impostazioni e dati nel server di destinazione nel modo seguente:

1. [Copiare i dati nel server di destinazione](#copy-data-to-the-destination-server)

2. [Configurare la rete](#configure-the-network) 

3. [Mappare i computer consentiti agli account utente](#map-permitted-computers-to-user-accounts)
 
## <a name="copy-data-to-the-destination-server"></a>Copiare i dati nel server di destinazione
 Prima di copiare i dati dal server di origine al server di destinazione, eseguire le attività seguenti: 
 
- Esaminare l'elenco di cartelle condivise sul server di origine, incluse le autorizzazioni per ogni cartella. Creare o personalizzare le cartelle sul server di destinazione in modo che corrispondano alla struttura di cartelle di cui si sta eseguendo la migrazione dal server di origine. 
 
- Esaminare la dimensione di ogni cartella e verificare che lo spazio di archiviazione sul server di destinazione sia sufficiente. 
 
- Impostare le cartelle condivise sul server di origine come di sola lettura per tutti gli utenti per impedire operazioni di scrittura nell'unità durante la copia dei file nel server di destinazione. 
 
#### <a name="to-copy-data-from-the-source-server-to-the-destination-server"></a>Per copiare i dati dal server di origine a quello di destinazione 
 
1. Accedere al server di destinazione come amministratore di dominio e quindi aprire una finestra di comando. 
 
2. Al prompt dei comandi digitare il comando seguente, quindi premere INVIO: 
 
 `robocopy \\<SourceServerName> \<SharedSourceFolderName> \\<DestinationServerName> \<SharedDestinationFolderName> /E /B /COPY:DATSOU /LOG:C:\Copyresults.txt` 
 
 Dove:
 - \<SourceServerName\> è il nome del server di origine
 - \<SharedSourceFolderName\> è il nome della cartella condivisa nel server di origine
 - \<NomeServerDestinazione\> è il nome del server di destinazione,
 - \<SharedDestinationFolderName\> è la cartella condivisa nel server di destinazione in cui verranno copiati i dati. 
 
3. Ripetere il passaggio precedente per ogni cartella condivisa di cui si deve eseguire la migrazione dal server di origine. 
 
## <a name="configure-the-network"></a>Configurare la rete
 Dopo aver spostato il ruolo DHCP nel router, configurare le impostazioni di rete sul server di destinazione. 
 
#### <a name="to-configure-the-network"></a>Per configurare la rete 
 
1. Nel server di destinazione aprire il dashboard. 
 
2. Nella pagina **Home** del dashboard fare clic su **CONFIGURA**, fare clic su **Configura Accesso remoto via Internet** e scegliere l'opzione **Fare clic per configurare Accesso remoto via Internet**. 
 
3. Completare le istruzioni della procedura guidata per configurare il router e i nomi di dominio. 
 
 Se il router non supporta il framework UPnP o se il framework UPnP è disabilitato, accanto al nome del router verrà visualizzata un'icona di avviso gialla. Verificare che le seguenti porte siano aperte e impostate sull'indirizzo IP del server di destinazione: 
 
- Porta 80: Traffico Web HTTP 
 
- Porta 443: Traffico Web HTTPS 
 
## <a name="map-permitted-computers-to-user-accounts"></a>Mappare i computer consentiti agli account utente
 Ogni account utente di cui è stata eseguita la migrazione dal server di origine deve essere mappato a uno o più computer. 
 
#### <a name="to-map-user-accounts-to-computers"></a>Per mappare gli account utente ai computer
 
1. Aprire il dashboard di Windows Server Essentials. 
 
2. Sulla barra di spostamento fare clic su **Utenti**. 
 
3. Nell'elenco di account utente fare clic con il pulsante destro del mouse su un account utente e quindi scegliere **Visualizza proprietà account**. 
 
4. Fare clic sulla scheda **Accesso remoto via Internet** e quindi fare clic su **Consenti Accesso Web remoto e accedi ai servizi Web**. 
 
5. Selezionare **Cartelle condivise**, selezionare **Computer**, selezionare **Home page - Collegamenti** e quindi fare clic su **Applica**. 
 
6. Fare clic sulla scheda **Accesso computer** e quindi fare clic sul nome del computer a cui si vuole consentire l'accesso. 
 
7. Ripetere i passaggi 3, 4, 5 e 6 per ogni account utente. 
 
> [!NOTE]
> Non è necessario cambiare la configurazione del computer client. Viene configurato automaticamente. 
>
> Dopo aver completato la migrazione, se si verificano problemi durante la creazione del primo nuovo account utente sul server di destinazione, rimuovere l'account utente aggiunto e quindi ricrearlo.
