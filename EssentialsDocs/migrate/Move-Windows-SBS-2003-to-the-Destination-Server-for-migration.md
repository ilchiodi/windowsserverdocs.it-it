---
title: Spostare dati e impostazioni di Windows SBS 2003 nel server di destinazione per la migrazione a Windows Server Essentials
description: Viene descritto come utilizzare Windows Server Essentials
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 67087ccb-d820-4642-8ca2-7d2d38714014
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 9fd9cdfaea641a0aee615befb5d400fa45160d97
ms.sourcegitcommit: 9a4ab3a0d00b06ff16173aed616624c857589459
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 06/11/2019
ms.locfileid: "66828555"
---
# <a name="move-windows-sbs-2003-settings-and-data-to-the-destination-server-for-windows-server-essentials-migration"></a>Spostare dati e impostazioni di Windows SBS 2003 nel server di destinazione per la migrazione a Windows Server Essentials

>Si applica a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

Spostare impostazioni e dati nel server di destinazione nel modo seguente:

1. [Copiare i dati nel Server di destinazione](#copy-data-to-the-destination-server)

2. [Importare account utente di Active Directory al Dashboard di Windows Server Essentials (facoltativo)](#import-active-directory-user-accounts-to-the-windows-server-essentials-dashboard)

3. [Rimuovere i vecchi script di accesso (facoltativo)](#remove-old-logon-scripts)

4. [Rimuovere legacy Active Directory oggetti Criteri di gruppo (facoltativo)](#remove-legacy-active-directory-group-policy-objects) 

5. [Configurare la rete](#configure-the-network) 

6. [Mappare i computer autorizzati agli account utente](#map-permitted-computers-to-user-accounts)

## <a name="copy-data-to-the-destination-server"></a>Copiare i dati nel server di destinazione
Prima di copiare i dati dal server di origine al server di destinazione, eseguire le attività seguenti: 

- Esaminare l'elenco di cartelle condivise sul server di origine, incluse le autorizzazioni per ogni cartella. Creare o personalizzare le cartelle sul server di destinazione in modo che corrispondano alla struttura di cartelle di cui si sta eseguendo la migrazione dal server di origine. 

- Esaminare la dimensione di ogni cartella e verificare che lo spazio di archiviazione sul server di destinazione sia sufficiente. 

- Impostare le cartelle condivise sul server di origine come di sola lettura per tutti gli utenti per impedire operazioni di scrittura nell'unità durante la copia dei file nel server di destinazione. 

#### <a name="to-copy-data-from-the-source-server-to-the-destination-server"></a>Per copiare i dati dal server di origine a quello di destinazione 

1. Accedere al server di destinazione come amministratore di dominio. 

2. Fare clic su **Start**, digitare **cmd** nella casella di ricerca e quindi premere INVIO. 

3. Al prompt dei comandi digitare il comando seguente e quindi premere INVIO: 

    `robocopy \\<SourceServerName> \<SharedSourceFolderName> \\<DestinationServerName> \<SharedDestinationFolderName> /E /B /COPY:DATSOU /LOG:C:\Copyresults.txt` 

Dove:
 - \<SourceServerName\> è il nome del Server di origine
 - \<SharedSourceFolderName\> è il nome della cartella condivisa nel Server di origine
 - \<NomeServerDestinazione\> è il nome del Server di destinazione,
 - \<SharedDestinationFolderName\> è la cartella condivisa nel Server di destinazione in cui verranno copiati i dati. 

4. Ripetere il passaggio precedente per ogni cartella condivisa di cui si deve eseguire la migrazione dal server di origine.

## <a name="import-active-directory-user-accounts-to-the-windows-server-essentials-dashboard"></a>Importare account utente di Active Directory per il Dashboard di Windows Server Essentials
 Per impostazione predefinita, tutti gli account utente creati nel Server di origine vengono automaticamente migrati nel Dashboard in Windows Server Essentials. Tuttavia, la migrazione automatica di un account utente di Active Directory non riuscirà se alcune proprietà non soddisfano i requisiti di migrazione. Per importare gli utenti di Active Directory, è possibile usare il cmdlet Windows PowerShell seguente.

#### <a name="to-import-an-active-directory-user-account-to-the-windows-server-essentials-dashboard"></a>Per importare un account utente di Active Directory per il Dashboard di Windows Server Essentials

1. Accedere al server di destinazione come amministratore di dominio.

2. Aprire Windows PowerShell come amministratore.

3. Eseguire il cmdlet seguente, dove `[AD username]` è il nome dell'account utente di Active Directory da importare:

    `Import-WssUser SamAccountName [AD username]`

## <a name="remove-old-logon-scripts"></a>Rimuovere i vecchi script di accesso
Windows SBS 2003 usa script di accesso per attività come l'installazione di software e la personalizzazione dei desktop. Windows Server Essentials sostituisce gli script di accesso di Windows SBS 2003 con una combinazione di script di accesso e oggetti Criteri di gruppo.

> [!NOTE]
> Se gli script di accesso di Windows SBS 2003 sono stati modificati, è consigliabile rinominare gli script per mantenere le personalizzazioni.
>
> Gli script di accesso di Windows SBS 2003 si applicano solo agli account utente aggiunti con l' **Aggiunta guidata nuovi utenti**.

#### <a name="to-remove-the-windows-sbs-2003-logon-scripts"></a>Per rimuovere gli script di accesso di Windows SBS 2003 

1. Fare clic sul pulsante **Start**, scegliere **Strumenti di amministrazione**, quindi fare clic su **Utenti e computer di Active Directory**.

2. In **Utenti e computer di Active Directory**espandere la rete e quindi fare clic su **Utenti**.

3. Fare clic con il pulsante destro del mouse su un nome utente, scegliere **Proprietà**e fare clic sulla scheda **Profilo** .

4. Eliminare il contenuto della casella di testo **Script di accesso** e quindi fare clic su **OK**.

5. Ripetere i passaggi 3 e 4 per ogni account utente.

## <a name="remove-legacy-active-directory-group-policy-objects"></a>Rimuovere gli oggetti di criteri legacy di Active Directory gruppo
Gli oggetti Criteri di gruppo (GPO) vengono aggiornati per Windows Server Essentials. Sono un soprainsieme degli oggetti Criteri di gruppo di Windows SBS 2003. Per Windows Server Essentials, è necessario eliminare manualmente un numero di filtri di Strumentazione gestione Windows (WMI) e oggetti Criteri di gruppo di Windows SBS 2003 per evitare conflitti con i filtri WMI e oggetti Criteri di gruppo di Windows Server Essentials. 

> [!NOTE]
> Se gli oggetti Criteri di gruppo di Windows SBS 2003 originali sono stati modificati, è consigliabile salvarne le copie in una posizione diversa e quindi eliminarli da Windows SBS 2003.

#### <a name="to-remove-old-group-policy-objects-from-windows-sbs-2003"></a>Per rimuovere i vecchi oggetti Criteri di gruppo da Windows SBS 2003 

1. Accedere al server di origine con un account amministratore. 

2. Fare clic sul pulsante **Start** e quindi scegliere **Gestione server**. 

3. Nel riquadro di spostamento, fare clic su **gestione avanzata**, fare clic su **Gestione criteri di gruppo**, quindi fare clic su **foresta: * * * < NomeDominio\>* . 

4. Fare clic su **domini**, fare clic su *< NomeDominio\>* , quindi fare clic su **oggetti Criteri di gruppo**. 

5. Fare clic con il pulsante destro del mouse su **Criterio controllo Small Business Server**, scegliere **Elimina** e quindi fare clic su **OK**. 

6. Ripetere il passaggio 5 per eliminare i seguenti oggetti Criteri di gruppo che si applicano alla rete: 

 - Computer client di Small Business Server 

 - Criterio password dominio Small Business Server 

È consigliabile che configurare i criteri password in Windows Server Essentials per imporre password complessa. Per configurare i criteri password, usare il dashboard, che scrive la configurazione nei criteri di dominio predefiniti. La configurazione dei criteri password non viene scritta nell'oggetto Criterio password dominio Small Business Server, come in Windows SBS 2003. 

 - Firewall connessione Internet (ICF) Small Business Server 

 - Criterio blocco Small Business Server 

 - Criterio Assistenza remota Small Business Server 

 - Windows Firewall Small Business Server 

 - Criterio computer client servizi di aggiornamento Small Business Server 

 Questo oggetto Criteri di gruppo sarà presente se si esegue la migrazione da Windows SBS 2003 R2. 

 - Criterio impostazioni comuni servizi di aggiornamento Small Business Server 

 Questo oggetto Criteri di gruppo sarà presente se si esegue la migrazione da Windows SBS 2003 R2. 
 
 - Criterio computer server servizi di aggiornamento Small Business Server 
 
 Questo oggetto Criteri di gruppo sarà presente se si esegue la migrazione da Windows SBS 2003 R2.

7. Verificare che tutti gli oggetti Criteri di gruppo siano stati eliminati.

#### <a name="to-remove-wmi-filters-from-windows-sbs-2003"></a>Per rimuovere i filtri WMI da Windows SBS 2003

1. Accedere al server di origine con un account amministratore.

2. Fare clic sul pulsante **Start** e quindi scegliere **Gestione server**.

3. Nel riquadro di spostamento, fare clic su **gestione avanzata**, fare clic su **Gestione criteri di gruppo**, quindi fare clic su **foresta: * * * < YourNetworkDomainName\>*

4. Fare clic su **domini**, fare clic su *< YourNetworkDomainName\>* , quindi fare clic su **filtri WMI**.

5. Fare clic con il pulsante destro del mouse su **PostSP2**, scegliere **Elimina** e quindi fare clic su **Sì**.

6. Fare clic con il pulsante destro del mouse su **PreSP2**, scegliere **Elimina**e quindi fare clic su **Sì**.

7. Verificare che questi tre filtri WMI siano stato eliminati.

## <a name="configure-the-network"></a>Configurare la rete

#### <a name="to-configure-the-network"></a>Per configurare la rete 

1. Nel server di destinazione aprire il dashboard. 

2. Nella pagina **Home** del dashboard fare clic su **CONFIGURA**, fare clic su **Configura Accesso remoto via Internet** e scegliere l'opzione **Fare clic per configurare Accesso remoto via Internet**. 

3. Seguire le istruzioni della procedura guidata **Configura Accesso remoto via Internet** per configurare il router e il nome di dominio.

 Se il router non supporta il framework UPnP o se il framework UPnP è disabilitato, accanto al nome del router verrà visualizzata un'icona di avviso gialla. Verificare che le seguenti porte siano aperte e impostate sull'indirizzo IP del server di destinazione:

- Porta 80: Traffico Web HTTP

- Porta 443: Traffico Web HTTPS

> [!NOTE]
> Se si è installato un Exchange Server locale in un secondo server, è necessario verificare che anche la porta 25 (per SMTP) sia aperta e che venga reindirizzata all'indirizzo IP dell'Exchange Server locale.

## <a name="map-permitted-computers-to-user-accounts"></a>Mappare i computer consentiti agli account utente
 In Windows SBS 2003, se un utente si connette ad Accesso Web remoto, vengono visualizzati tutti i computer della rete, che può includere computer per cui l'utente non ha le autorizzazioni di accesso. In Windows Server Essentials, un utente deve essere assegnato esplicitamente a un computer per poterlo visualizzare in accesso Web remoto. Ogni account utente di cui è stata eseguita la migrazione da Windows SBS 2003 deve essere mappato a uno o più computer. 

#### <a name="to-map-user-accounts-to-computers"></a>Per mappare gli account utente ai computer 

1. Nel Server di destinazione, aprire il Dashboard di Windows Server Essentials. 

2. Sulla barra di spostamento fare clic su **Utenti**. 

3. Nell'elenco di account utente fare clic con il pulsante destro del mouse su un account utente e quindi scegliere **Visualizza proprietà account**. 

4. Fare clic sulla scheda **Accesso remoto via Internet** e quindi fare clic su **Consenti Accesso Web remoto e accedi ai servizi Web**. 

5. Fare clic su **Cartelle condivise**, su **Computer**, su **Home page - Collegamenti** e infine su **Applica**. 

6. Fare clic sulla scheda **Accesso computer** e sul nome del computer a cui si vuole consentire l'accesso. 

7. Ripetere i passaggi 3, 4, 5 e 6 per ogni account utente. 

> [!NOTE]
> Non è necessario cambiare la configurazione del computer client. Viene configurato automaticamente.
>
> Dopo aver completato la migrazione, se si verificano problemi durante la creazione del primo nuovo account utente sul server di destinazione, rimuovere l'account utente aggiunto e quindi ricrearlo.
