---
title: Spostare dati e impostazioni di Windows Server 2008 Foundation nel server di destinazione per la migrazione a Windows Server Essentials
description: Viene descritto come usare Windows Server Essentials
ms.date: 10/03/2016
ms.prod: windows-server
ms.topic: article
ms.assetid: 3ff7d040-ebd1-421c-80db-765deacedd4c
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 86aff1f03cc5c70789f63d2acee4b925c3ca058c
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80852444"
---
# <a name="move-windows-server-2008-foundation-settings-and-data-to-the-destination-server-for-windows-server-essentials-migration"></a>Spostare dati e impostazioni di Windows Server 2008 Foundation nel server di destinazione per la migrazione a Windows Server Essentials

>Si applica a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

Spostare impostazioni e dati nel server di destinazione nel modo seguente:

1. [Copiare i dati nel server di destinazione (facoltativo)](#copy-data-to-the-destination-server)

2. [Importare Active Directory account utente nel dashboard di Windows Server Essentials (facoltativo)](#import-active-directory-user-accounts-to-the-windows-server-essentials-dashboard)

3. [Spostare il ruolo server DHCP dal server di origine al router](#move-the-dhcp-server-role-from-the-source-server-to-the-router)

4. [Configurare la rete](#configure-the-network) 

5. [Mappare i computer consentiti agli account utente](#map-permitted-computers-to-user-accounts)
  
## <a name="copy-data-to-the-destination-server"></a>Copiare i dati nel server di destinazione
 Prima di copiare i dati dal server di origine al server di destinazione, eseguire le attività seguenti:  
  
- Esaminare l'elenco di cartelle condivise sul server di origine, incluse le autorizzazioni per ogni cartella. Creare o personalizzare le cartelle sul server di destinazione in modo che corrispondano alla struttura di cartelle di cui si sta eseguendo la migrazione dal server di origine.  
  
- Esaminare la dimensione di ogni cartella e verificare che lo spazio di archiviazione sul server di destinazione sia sufficiente.  
  
- Impostare le cartelle condivise sul server di origine come di sola lettura per tutti gli utenti per impedire operazioni di scrittura nell'unità durante la copia dei file nel server di destinazione.  
  
#### <a name="to-copy-data-from-the-source-server-to-the-destination-server"></a>Per copiare i dati dal server di origine a quello di destinazione  
  
1.  Accedere al server di destinazione come amministratore di dominio e quindi aprire una finestra di comando.  
  
2.  Al prompt dei comandi digitare il comando seguente, quindi premere INVIO:  
  
    `robocopy \\<SourceServerName> \<SharedSourceFolderName> \\<DestinationServerName> \<SharedDestinationFolderName> /E /B /COPY:DATSOU /LOG:C:\Copyresults.txt`  
  
     Dove:
     - \<SourceServerName\> è il nome del server di origine
     - \<SharedSourceFolderName\> è il nome della cartella condivisa nel server di origine
     - \<NomeServerDestinazione\> è il nome del server di destinazione,
     - \<SharedDestinationFolderName\> è la cartella condivisa nel server di destinazione in cui verranno copiati i dati.  
  
3.  Ripetere il passaggio precedente per ogni cartella condivisa di cui si deve eseguire la migrazione dal server di origine.  
  
## <a name="import-active-directory-user-accounts-to-the-windows-server-essentials-dashboard"></a>Importare Active Directory account utente nel dashboard di Windows Server Essentials
 Per impostazione predefinita, tutti gli account utente creati nel server di origine vengono automaticamente migrati nel dashboard in Windows Server Essentials. Tuttavia, la migrazione automatica di un account utente di Active Directory non riuscirà se tutte le proprietà non soddisfano i requisiti di migrazione. Per importare gli utenti di Active Directory, è possibile usare il cmdlet Windows PowerShell seguente.  
  
#### <a name="to-import-an-active-directory-user-account-to-the-windows-server-essentials-dashboard"></a>Per importare un account utente Active Directory nel dashboard di Windows Server Essentials
  
1.  Accedere al server di destinazione come amministratore di dominio.  
  
2.  Aprire Windows PowerShell come amministratore.  
  
3.  Eseguire il cmdlet seguente, dove `[AD username]` è il nome dell'account utente di Active Directory da importare:  
  
     `Import-WssUser  SamAccountName [AD username]`  
  
## <a name="move-the-dhcp-server-role-from-the-source-server-to-the-router"></a>Spostare il ruolo server DHCP dal server di origine al router
 Se il server di origine esegue il ruolo DHCP, eseguire i passaggi seguenti per spostare il ruolo DHCP nel router.  
  
#### <a name="to-move-the-dhcp-role-from-the-source-server-to-the-router"></a>Per spostare il ruolo DHCP dal server di origine al router  
  
1.  Disattivare il servizio DHCP sul server di origine, nel modo seguente:  
  
    1.  Nel server di origine fare clic su **Start**, scegliere **Strumenti di amministrazione** e quindi fare clic su **Servizi**.  
  
    2.  Nell'elenco di servizi attualmente in esecuzione fare clic con il pulsante destro del mouse su **Server DHCP** e quindi scegliere **Proprietà**.  
  
    3.  Per **Tipo di avvio**, selezionare **Disabilitato**.  
  
    4.  Arrestare il servizio.  
  
2.  Attivare il ruolo DHCP sul router.  
  
    1.  Seguire le istruzioni della documentazione del router per attivare il ruolo DHCP sul router.  
  
    2.  Per essere certi che gli indirizzi IP rilasciati dal server di origine rimangano gli stessi, seguire le istruzioni della documentazione del router per configurare un intervallo DHCP sul router uguale a quello sul server di origine.  
  
        > [!IMPORTANT]
        >  Se non è stato configurato un IP statico o prenotazioni DHCP sul router per il server di destinazione e l'intervallo DHCP non è uguale a quello del server di origine, è possibile che il router rilascerà un nuovo indirizzo IP per il server di destinazione. In questo caso, reimpostare le regole di port forwarding del router per l'inoltro al nuovo indirizzo IP del server di destinazione.  
  
## <a name="configure-the-network"></a>Configurare la rete
 Dopo aver spostato il ruolo DHCP nel router, configurare le impostazioni di rete sul server di destinazione.  
  
#### <a name="to-configure-the-network"></a>Per configurare la rete  
  
1. Nel server di destinazione aprire il dashboard.  
  
2. Nella pagina **Home** del dashboard fare clic su **CONFIGURA**, fare clic su **Configura Accesso remoto via Internet** e scegliere l'opzione **Fare clic per configurare Accesso remoto via Internet**.  
  
3. Completare le istruzioni della procedura guidata per configurare il router e i nomi di dominio.  
  
   Se il router non supporta il framework UPnP o se il framework UPnP è disabilitato, accanto al nome del router verrà visualizzata un'icona di avviso gialla. Verificare che le seguenti porte siano aperte e impostate sull'indirizzo IP del server di destinazione:  
  
-   Porta 80: Traffico Web HTTP  
  
-   Porta 443: Traffico Web HTTPS  
  
## <a name="map-permitted-computers-to-user-accounts"></a>Mappare i computer consentiti agli account utente  
 In Windows Server Essentials, un utente deve essere assegnato in modo esplicito a un computer per poterlo visualizzare in Accesso Web remoto. Ogni account utente di cui è stata eseguita la migrazione da Windows Server 2008 Foundation deve essere mappato a uno o più computer.  
  
#### <a name="to-map-user-accounts-to-computers"></a>Per mappare gli account utente ai computer  
  
1.  Aprire il dashboard di Windows Server Essentials.  
  
2.  Sulla barra di spostamento fare clic su **Utenti**.  
  
3.  Nell'elenco di account utente fare clic con il pulsante destro del mouse su un account utente e quindi scegliere **Visualizza proprietà account**.  
  
4.  Fare clic sulla scheda **Accesso remoto via Internet** e quindi fare clic su **Consenti Accesso Web remoto e accedi ai servizi Web**.  
  
5.  Selezionare **Cartelle condivise**, selezionare **Computer**, selezionare **Home page - Collegamenti** e quindi fare clic su **Applica**.  
  
6.  Fare clic sulla scheda **Accesso computer** e quindi fare clic sul nome del computer a cui si vuole consentire l'accesso.  
  
7.  Ripetere i passaggi 3, 4, 5 e 6 per ogni account utente.  
  
> [!NOTE]
> Non è necessario cambiare la configurazione del computer client. Viene configurato automaticamente.  
>
> Dopo aver completato la migrazione, se si verificano problemi durante la creazione del primo nuovo account utente sul server di destinazione, rimuovere l'account utente aggiunto e quindi ricrearlo.
