---
title: Connessione in Windows Server Essentials
description: Viene descritto come utilizzare Windows Server Essentials
ms.custom: na
ms.date: 05/07/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 149a5d34-43b7-4b9e-99e7-9f2294ab9ddb
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 98292e0715ea662712e596d89646a735e22ba3f9
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/31/2019
ms.locfileid: "66435971"
---
# <a name="get-connected-in-windows-server-essentials"></a>Connessione in Windows Server Essentials

>Si applica a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

 È possibile connettere i computer al server di Windows Server Essentials tramite il software Connettore. Il software Connettore viene installato quando si connette un computer al server tramite la procedura guidata Connetti il computer al server. È possibile avviare questa procedura guidata digitando **http://<servername\>/Connect**, dove **< servername\>**  è il nome del server.  

 In questo argomento  


-   [Preparare la connessione di computer al server](Get-Connected-in-Windows-Server-Essentials.md#BKMK_A)  

-   [Connettere i computer al server tramite il software connettore](Get-Connected-in-Windows-Server-Essentials.md#BKMK_B)  

-   [Utilizzare la finestra di avvio](Get-Connected-in-Windows-Server-Essentials.md#BKMK_C)  

-   [Preparare la connessione di computer al server](Get-Connected-in-Windows-Server-Essentials.md#BKMK_A)  

-   [Connettere i computer al server tramite il software connettore](Get-Connected-in-Windows-Server-Essentials.md#BKMK_B)  

-   [Utilizzare la finestra di avvio](Get-Connected-in-Windows-Server-Essentials.md#BKMK_C)  


##  <a name="BKMK_A"></a> Preparare la connessione di computer al server  
 Questa sezione illustra il software Connettore, i sistemi operativi supportati da Windows Server Essentials, le attività da completare prima della connessione dei computer al server e le modifiche apportate dal server ai computer quando si esegue il software Connettore.  


-   [Panoramica del software connettore](Get-Connected-in-Windows-Server-Essentials.md#BKMK_1)  

-   [Prerequisiti per la connessione di un computer al server](Get-Connected-in-Windows-Server-Essentials.md#BKMK_2)  

-   [Prerequisiti per la connessione di un computer Mac alla rete](Get-Connected-in-Windows-Server-Essentials.md#BKMK_3)  

-   [Sistemi operativi supportati per i computer client](Get-Connected-in-Windows-Server-Essentials.md#BKMK_4)  

-   [Modifiche apportate dal server a un computer client](Get-Connected-in-Windows-Server-Essentials.md#BKMK_5)  

-   [Informazioni nome utente e password di rete](Get-Connected-in-Windows-Server-Essentials.md#BKMK_6)  

-   [Account dell'amministratore del server](Get-Connected-in-Windows-Server-Essentials.md#BKMK_7)  

-   [Rimuovere un computer da un dominio di Windows](Get-Connected-in-Windows-Server-Essentials.md#BKMK_8)  

###  <a name="BKMK_1"></a> Panoramica del software connettore  
 Il software Connettore per il sistema operativo Windows Server Essentials connette i computer nella rete al server di Windows Server Essentials. Quando si connettono computer al server, il software Connettore permette di eseguire automaticamente il backup dei computer e di monitorarne l'integrità. Il software Connettore permette anche di configurare e amministrare in modalità remota il server di Windows Server Essentials. Il software Connettore viene installato quando si connette un computer client al server. Per istruzioni dettagliate sulla connessione dei computer client al server di Windows Server Essentials, vedere [Connettere i computer al server](Get-Connected-in-Windows-Server-Essentials.md#BKMK_9) più avanti in questo argomento.  

-   [Panoramica del software connettore](Get-Connected-in-Windows-Server-Essentials.md#BKMK_1)  

-   [Prerequisiti per la connessione di un computer al server](../use/Get-Connected-in-Windows-Server-Essentials.md#BKMK_2)  

-   [Prerequisiti per la connessione di un computer Mac alla rete](Get-Connected-in-Windows-Server-Essentials.md#BKMK_3)  

-   [Sistemi operativi supportati per i computer client](Get-Connected-in-Windows-Server-Essentials.md#BKMK_4)  

-   [Modifiche apportate dal server a un computer client](Get-Connected-in-Windows-Server-Essentials.md#BKMK_5)  

-   [Informazioni nome utente e password di rete](Get-Connected-in-Windows-Server-Essentials.md#BKMK_6)  

-   [Account dell'amministratore del server](Get-Connected-in-Windows-Server-Essentials.md#BKMK_7)  

-   [Rimuovere un computer da un dominio di Windows](Get-Connected-in-Windows-Server-Essentials.md#BKMK_8)  

###  <a name="BKMK_1"></a> Panoramica del software connettore  
 Il software Connettore per il sistema operativo Windows Server Essentials connette i computer nella rete al server di Windows Server Essentials. Quando si connettono computer al server, il software Connettore permette di eseguire automaticamente il backup dei computer e di monitorarne l'integrità. Il software Connettore permette anche di configurare e amministrare in modalità remota il server di Windows Server Essentials. Il software Connettore viene installato quando si connette un computer client al server. Per istruzioni dettagliate sulla connessione dei computer client al server di Windows Server Essentials, vedere [Connettere i computer al server](Get-Connected-in-Windows-Server-Essentials.md#BKMK_9) più avanti in questo argomento.  


###  <a name="BKMK_2"></a> Prerequisiti per la connessione di un computer al server  
 Prima di connettere un computer alla rete, è necessario soddisfare i requisiti seguenti:  

-   L'installazione di Windows Server Essentials è stata completata e il server è in esecuzione. Se non è possibile comunicare con il server, l'installazione del software Connettore sarà interrotta.  


-   Il sistema operativo in esecuzione nel computer client è supportato. Per altre informazioni, vedere [Supported operating systems for client computers](Get-Connected-in-Windows-Server-Essentials.md#BKMK_4).


-   Il computer client deve avere una connessione valida a Internet.  

-   Il computer client si trova nella stessa subnet IP del server che esegue Windows Server Essentials quando il computer client si trova nella stessa rete del server.  

-   Nel computer client è installato .NET Framework 4.5. Il software Connettore sarà installato automaticamente nel computer.  

-   Il computer client soddisfa i requisiti di sistema minimi seguenti:  

    -   Processore da 1,4 GHz o superiore  

    -   1 GB di RAM o superiore  

    -   1 GB di spazio disponibile sull'unità disco rigido (una porzione di questo disco sarà resa disponibile dopo l'installazione)  

-   Partizione di avvio, ovvero la partizione del disco in cui è installato il sistema operativo Windows, formattata con il file system NTFS.  

-   Il nome del computer non include più di 15 caratteri.  

-   Il nome del computer non include un carattere di sottolineatura (_).  

-   Le impostazioni per data e ora del computer sono allineate alle impostazioni del server.  

-   Un computer client può essere connesso solo a un server di Windows Server Essentials in un determinato momento.  

-   Un computer client già aggiunto a un altro dominio di Active Directory non potrà essere aggiunto a un dominio di Windows Server Essentials.  

> [!NOTE]
> 
>  In una distribuzione client locale per Windows Server Essentials o Windows Server Essentials, è possibile connettere i computer al server senza aggiungerli al dominio di Windows Server Essentials. Questo metodo non è disponibile per tutti i sistemi operativi client supportati e le funzionalità quali Criteri di gruppo e le reti private virtuali (VPN, Virtual Private Network), che necessitano della connessione di un computer al dominio, non saranno disponibili. Per informazioni sui requisiti e istruzioni, vedere [Connettere i computer a un server di Windows Server Essentials senza aggiungerli al dominio](Get-Connected-in-Windows-Server-Essentials.md#BKMK_10).  

 Per istruzioni dettagliate per la connessione di un computer al server che esegue Windows Server Essentials, vedere [Connect computers to the server](Get-Connected-in-Windows-Server-Essentials.md#BKMK_9).  

>  In una distribuzione client locale per Windows Server Essentials o Windows Server Essentials, è possibile connettere i computer al server senza aggiungerli al dominio di Windows Server Essentials. Questo metodo non è disponibile per tutti i sistemi operativi client supportati e le funzionalità quali Criteri di gruppo e le reti private virtuali (VPN, Virtual Private Network), che necessitano della connessione di un computer al dominio, non saranno disponibili. Per informazioni sui requisiti e istruzioni, vedere [Connettere i computer a un server di Windows Server Essentials senza aggiungerli al dominio](Get-Connected-in-Windows-Server-Essentials.md#BKMK_10).  

 Per istruzioni dettagliate per la connessione di un computer al server che esegue Windows Server Essentials, vedere [Connect computers to the server](Get-Connected-in-Windows-Server-Essentials.md#BKMK_9).  


###  <a name="BKMK_3"></a> Prerequisiti per la connessione di un computer Mac alla rete  
 Prima di connettere un computer Mac alla rete, è necessario soddisfare i requisiti seguenti:  

-   L'installazione del sistema operativo è stata completata e il server è in esecuzione. Se non è possibile comunicare con il server, l'installazione del software Connettore non sarà eseguita.  

-   Nel computer è in esecuzione Mac OS X 10.5 (Leopard) o versione successiva.  

-   Il computer si trova nella stessa subnet IP in cui si trova il server.  

-   Il computer deve avere una connessione valida a Internet.  

-   Verificare che il computer soddisfi i requisiti minimi di sistema seguenti:  

    -   Processore da 1,4 GHz o superiore  

    -   1 GB di RAM o superiore  

    -   1 GB di spazio disponibile sull'unità disco rigido (una porzione di questo disco sarà resa disponibile dopo l'installazione)  

-   Un computer client può essere connesso a un solo server in un determinato momento.  

###  <a name="BKMK_4"></a> Sistemi operativi supportati per i computer client  
 Windows Server Essentials offre lo stesso insieme di funzionalità a tutti i computer client supportati. Queste funzionalità includono Associazione del dominio, finestra di avvio e notifiche sull'integrità lato client.  

> [!IMPORTANT]
>  Windows Server Essentials non supporta l'aggiunta al dominio di computer che eseguono le versioni Home, Starter o Media Center di Windows. Non è inoltre possibile usare Accesso Web remoto per la connessione a questi computer.  

#### <a name="windows-server-essentials"></a>Windows Server Essentials  
  In questa sezione si riferisce a un server che esegue Windows Server Essentials oppure un server che esegue Windows Server 2012 R2 Standard o Windows Server 2012 R2 Datacenter con il ruolo esperienza Windows Server Essentials installato. Sono supportati i sistemi operativi per computer seguenti:  

 **Sistemi operativi Windows 7**  

- Windows 7 Home Basic SP1 (x86 e x64)  

- Windows 7 Home Premium SP1 (x86 e x64)  

- Windows 7 Professional SP1 (x86 e x64)  

- Windows 7 Ultimate SP1 (x86 e x64)  

- Windows 7 Enterprise SP1 (x86 e x64)  

- Windows 7 Starter SP1 (x86)  

  **Sistemi operativi Windows 8**  

- Windows 8  

- Windows 8 Pro  

- Windows 8 Enterprise  

  **Sistemi operativi Windows 8.1**  

- Windows 8.1  

- Windows 8.1 Pro  

- Windows 8.1 Enterprise  

  **Sistemi operativi Windows 10**  

- Windows 10  

- Windows 10 Pro  

- Windows 10 Enterprise  

- Windows 10 Education  

  **Computer client Mac**  

- Mac OS X v10.5 Leopard  

- Mac OS X v10.6 Snow Leopard  

- Mac OS X v10.7 Lion  

- Mac OS X v10.8 Mountain Lion  

> [!NOTE]
>  È possibile visualizzare l'integrità e lo stato del backup per un computer Mac dal Dashboard di Windows Server Essentials. Non è tuttavia possibile configurare o avviare il backup di un computer dal dashboard. Non si può inoltre usare Accesso Web remoto per la connessione a un computer Mac.  

#### <a name="windows-server-essentials"></a>Windows Server Essentials  
  In questa sezione si applica a un server che esegue Windows Server Essentials. Sono supportati i sistemi operativi per computer seguenti:  

 **Sistemi operativi Windows 7**  

- Windows 7 Home Basic (x86 e x64)  

- Windows 7 Home Premium (x86 e x64)  

- Windows 7 Professional (x86 e x64)  

- Windows 7 Ultimate (x86 e x64)  

- Windows 7 Enterprise (x86 e x64)  

- Windows 7 Starter (x86)  

  **Sistemi operativi Windows 8**  

- Windows 8  

- Windows 8 Pro  

- Windows 8 Enterprise  

  **Sistemi operativi Windows 10**  

- Windows 10  

- Windows 10 Pro  

- Windows 10 Enterprise  

- Windows 10 Education  

  **Computer client Mac**  

- Mac OS X v10.5 Leopard  

- Mac OS X v10.6 Snow Leopard  

- Mac OS X v10.7 Lion  

> [!NOTE]
>  È possibile visualizzare lo stato dell'integrità e del backup per un computer Mac dal dashboard di Windows Server Essentials. Non è tuttavia possibile configurare o avviare il backup di un computer dal dashboard. Non si può inoltre usare Accesso Web remoto per la connessione a un computer Mac.  

###  <a name="BKMK_5"></a> Modifiche apportate dal server a un computer client  
 Quando si connette un computer al server, il software di Windows Server Essentials apporta alcune modifiche al computer, in modo da permettere l'interazione tra computer e server.  

 Il software esegue le operazioni seguenti:  

-   Installazione del software Connettore nel computer.  

-   Installazione di Microsoft .NET Framework 4.5 nel computer, se non è già installato.  

-   Creazione di collegamenti al dashboard e alla finestra di avvio sul desktop del computer.  

-   Configurazione delle porte per Windows Firewall nel computer, in modo da permettere il funzionamento delle funzionalità seguenti:  

    -   Funzionalità di base rete  

    -   Servizi Desktop remoto  

-   Le modifiche seguenti sono apportate al computer per semplificare i backup:  

    -   Creazione di attività pianificate per l'esecuzione di backup automatici.  

    -   Installazione di servizi per la gestione delle operazioni di backup con il server.  

    -   Installazione di un driver di dispositivo virtuale, usato durante i processi di ripristino di file e cartelle.  

-   Installazione dell'Agente integrità sistema per rilevare problemi e creare le notifiche di avviso corrispondenti.  

-   Creazione di attività pianificate nel computer per valutazioni ricorrenti dell'integrità e per la sincronizzazione delle definizioni degli avvisi sull'integrità.  

-   Aggiunta al computer di servizi usati dal computer per comunicare con il server e con altre funzionalità di Windows Server Essentials.  

-   Apertura della porta TCP 3389 nei computer client che eseguono i client di Windows, per permettere l'esecuzione dei Servizi Desktop remoto.  

-   Distribuzione della VPN nel computer client e fornisce un'esperienza con clic singolo se la funzionalità VPN è abilitata in Windows Server Essentials, o offre un'esperienza con connessione automatica se è abilitata la funzionalità VPN in Windows Server Essentials  


 Per informazioni sulla connessione del computer al server, vedere [Connect computers to the server](Get-Connected-in-Windows-Server-Essentials.md#BKMK_9).  

###  <a name="BKMK_6"></a> Informazioni nome utente e password di rete  
 È possibile ottenere informazioni su nome utente e password di rete dall'utente che gestisce il server. Queste credenziali possono essere usate per la connessione del computer al server e per accedere a informazioni dal server.  

###  <a name="BKMK_6"></a> Informazioni nome utente e password di rete  
 È possibile ottenere informazioni su nome utente e password di rete dall'utente che gestisce il server. Queste credenziali possono essere usate per la connessione del computer al server e per accedere a informazioni dal server. 


 Se si è l'amministratore del server, è possibile creare le credenziali di rete mediante l'aggiunta di un account utente dalla scheda **Utenti** del dashboard. Per altre informazioni sull'account utente, vedere [Gestire gli account utente tramite il dashboard](../manage/Manage-User-Accounts-in-Windows-Server-Essentials.md#BKMK_Manage8).  

###  <a name="BKMK_7"></a> Account dell'amministratore del server  
 Per installare il software Connettore, occorre essere in grado di fornire un nome account amministratore e una password. Un account amministratore di rete permette all'utente di gestire la rete locale (LAN, Local Area Network) per l'organizzazione e semplifica la gestione e la manutenzione dei dispositivi di rete, ad esempio router e commutatori.  

 Le attività che possono essere eseguite tramite un account amministratore di rete includono le seguenti:  

- Installazione delle applicazioni in rete e di altro software  

- Gestione dell'archiviazione sul server  

- Distribuzione degli aggiornamenti software  

- Esecuzione dei backup periodici  

- Monitoraggio delle attività giornaliere della rete  

  In Windows Server Essentials, Windows Server Essentials e Windows Server 2012 R2 con il ruolo esperienza Windows Server Essentials installato, è possibile assegnare all'amministratore di rete livello a qualsiasi account utente di accesso. in modo da concedere le autorizzazioni necessarie per eseguire le attività di amministratore di rete. Quando si assegna un livello di accesso di amministratore di rete, il prompt **Controllo di accesso utente** sarà visualizzato per qualsiasi attività che richiede autorizzazioni di amministratore.  

###  <a name="BKMK_8"></a> Rimuovere un computer da un dominio di Windows  
 Per rimuovere un computer dal dominio, saranno richiesti il nome utente e la password per l'account di dominio.  

##### <a name="to-remove-a-computer-from-a-windows-domain"></a>Per rimuovere un computer da un dominio di Windows  

1.  Fare clic sul pulsante **Start**, fare clic con il pulsante destro del mouse su **Computer**, quindi scegliere **Proprietà**.  

2.  In **Impostazioni relative a nome computer, dominio e gruppo di lavoro**fare clic su **Cambia impostazioni**.  

    > [!NOTE]
    >  Se è richiesta una password di amministratore o una conferma, digitare la password del dominio oppure fornire una conferma.  

3.  Nella finestra di dialogo **Proprietà del sistema** , fare clic sulla scheda **Nome computer** , quindi su **Cambia**.  

4.  Nella finestra di dialogo **Cambiamenti dominio/nome computer** in **Membro di** fare clic su **Gruppo di lavoro**, quindi eseguire una delle operazioni seguenti:  

    1.  Per aggiungere un gruppo di lavoro esistente, digitare il nome del gruppo di lavoro da aggiungere e quindi fare clic su **OK**.  

    2.  Per creare un gruppo di lavoro, digitare il nome del gruppo di lavoro da creare, quindi fare clic su **OK**.  

        > [!NOTE]
        >  Il computer sarà rimosso dal dominio e l'account computer del dominio sarà disabilitato.  

##  <a name="BKMK_B"></a> Connettere i computer al server tramite il software connettore  
 Questa sezione offre l'accesso alle procedure e alle informazioni utili per installare il software Connettore, connettere il computer al server e risolvere i problemi della connessione dei computer al server.  


-   [Connettere i computer al server](Get-Connected-in-Windows-Server-Essentials.md#BKMK_9)  

-   [Connettere computer a un server Windows Server Essentials senza aggiungerli al dominio](Get-Connected-in-Windows-Server-Essentials.md#BKMK_10)  

-   [Installare il software connettore](Get-Connected-in-Windows-Server-Essentials.md#BKMK_11)  

-   [Spostare i dati del computer e le impostazioni manualmente](Get-Connected-in-Windows-Server-Essentials.md#BKMK_12)  

-   [Trasferire più profili utente durante la distribuzione di computer](Get-Connected-in-Windows-Server-Essentials.md#BKMK_Transfer)  

-   [Disinstallare il software connettore](Get-Connected-in-Windows-Server-Essentials.md#BKMK_13)  

-   [Disconnettere il computer da o riconnettere il computer al server](Get-Connected-in-Windows-Server-Essentials.md#BKMK_14)  

-   [Come funziona con la sospensione di backup e le modalità di ibernazione](Get-Connected-in-Windows-Server-Essentials.md#BKMK_Sleep)  

-   [Connettere i computer al server](Get-Connected-in-Windows-Server-Essentials.md#BKMK_9)  

-   [Connettere computer a un server Windows Server Essentials senza aggiungerli al dominio](Get-Connected-in-Windows-Server-Essentials.md#BKMK_10)  

-   [Installare il software connettore](Get-Connected-in-Windows-Server-Essentials.md#BKMK_11)  

-   [Spostare i dati del computer e le impostazioni manualmente](Get-Connected-in-Windows-Server-Essentials.md#BKMK_12)  

-   [Trasferire più profili utente durante la distribuzione di computer](Get-Connected-in-Windows-Server-Essentials.md#BKMK_Transfer)  

-   [Disinstallare il software connettore](Get-Connected-in-Windows-Server-Essentials.md#BKMK_13)  

-   [Disconnettere il computer da o riconnettere il computer al server](Get-Connected-in-Windows-Server-Essentials.md#BKMK_14)  

-   [Come funziona con la sospensione di backup e le modalità di ibernazione](Get-Connected-in-Windows-Server-Essentials.md#BKMK_Sleep)  


###  <a name="BKMK_9"></a> Connettere i computer al server  
 Quando ci si connette un computer a un server che esegue Windows Server Essentials o Windows Server 2012 R2 con il ruolo esperienza Windows Server Essentials installato, assicurarsi che il computer client abbia una connessione valida a Internet.  

 Completare la procedura seguente in tutti i computer client per connetterli al server.  

 Per completare la procedura sono necessarie le informazioni seguenti:  

-   Nome utente e password dell'utente che userà il computer nella nuova rete  

-   Nome utente e password dell'account amministratore locale del computer  

> [!NOTE]
> 
>  In una distribuzione client locale per Windows Server Essentials o Windows Server Essentials, è possibile connettere i computer al server senza aggiungerli al dominio di Windows Server Essentials. Questo metodo non è disponibile per tutti i sistemi operativi client supportati e le funzionalità quali Criteri di gruppo e le reti private virtuali (VPN, Virtual Private Network), che necessitano della connessione di un computer al dominio, non saranno disponibili. Per informazioni sui requisiti e istruzioni, vedere [Connettere i computer a un server di Windows Server Essentials senza aggiungerli al dominio](Get-Connected-in-Windows-Server-Essentials.md#BKMK_10).  
> 
>  In una distribuzione client locale per Windows Server Essentials o Windows Server Essentials, è possibile connettere i computer al server senza aggiungerli al dominio di Windows Server Essentials. Questo metodo non è disponibile per tutti i sistemi operativi client supportati e le funzionalità quali Criteri di gruppo e le reti private virtuali (VPN, Virtual Private Network), che necessitano della connessione di un computer al dominio, non saranno disponibili. Per informazioni sui requisiti e istruzioni, vedere [Connettere i computer a un server di Windows Server Essentials senza aggiungerli al dominio](Get-Connected-in-Windows-Server-Essentials.md#BKMK_10).  


##### <a name="to-connect-a-client-computer-to-the-server"></a>Per connettere un computer client al server  

1.  Accedere al computer che si desidera connettere al server.  

    > [!NOTE]
    >  Se nel computer sono disponibili più account utente, accedere usando l'account utente associato a documenti, immagini e preferenze personali da conservare dopo la connessione del computer al server.  

2.  Aprire un browser Internet, ad esempio Internet Explorer.  

3.  Nella barra degli indirizzi, digitare **http://<servername\>/Connect**, quindi premere INVIO.  

    > [!NOTE]
    >  Se il computer si trova in una posizione remota all'esterno della rete di Windows Server Essentials, per eseguire la connessione di un Computer per la procedura guidata Server, digitare **http://<domainname\>/ connect** nella barra degli indirizzi di (il browser web dove < domain\> è il nome di dominio dell'organizzazione). È possibile ottenere le informazioni sul nome di dominio dall'amministratore di rete.  

4.  Verrà visualizzata la pagina **Connetti il computer al server** . Effettua una delle seguenti operazioni:  

    -   Per i computer che eseguono il sistema operativo Windows fare clic su **Scarica il software per Windows**.  

    -   Per i computer che eseguono Mac OS X o versione successiva, fare clic su **Scarica il software per Mac**.  

5.  Nell’avviso di protezione relativo al download del file, fare clic su **Esegui**.  

6.  Se viene visualizzato il messaggio relativo al Controllo dell’account utente, fare clic su **Sì** oppure digitare il nome utente e la password locali, se richiesto.  

7.  Verrà visualizzata la procedura guidata Connessione del computer al server. Effettuare le seguenti operazioni per completare la procedura guidata:  

    1.  Accettare il Contratto di Licenza con l'utente finale.  

    2.  Nella pagina **Trova server** eseguire il rilevamento automatico del server nelle reti locali, quindi selezionare il server a cui ci si vuole connettere. In alternativa, se le informazioni sono disponibili, è possibile specificare manualmente il nome o l'indirizzo di dominio del server.  

    3.  Nella pagina **Immettere il nome utente e la password di rete nuovi**, fare quanto segue:  

        -   Se è il primo computer che si connette al server, e se questo è il computer che si utilizzerà per amministrare il server, utilizzare l'account amministratore creato durante l'installazione.  

        -   Per tutti gli altri computer, creare innanzitutto un account utente di rete nel server mediante il dashboard. Creare l'account utente con i privilegi utente amministratore o standard, in base alle attività eseguite dalla persona che utilizzerà il computer.  

    4.  Se il computer esegue Windows 8, Windows 8.1 o Windows 10, ignorare questo passaggio. Se il computer esegue Windows 7 e si desidera mantenere documenti, immagini o preferenze personali (ad esempio, sfondi del desktop, screen saver o Preferiti di Internet Explorer) dopo aver aggiunto il computer alla nuova rete, nella pagina **Scegliere se spostare i dati e le impostazioni personali esistenti** della procedura guidata, selezionare **Sposta i dati e le impostazioni personali sul nuovo account utente di rete**.  

    5.  Nella pagina **Scegliere se riattivare il computer per creare una copia di backup**, specificare se si desidera riattivare automaticamente il computer per creare una copia di backup.  

    6.  Seguire le istruzioni rimanenti della procedura guidata per aggiungere il computer alla rete.  

8.  Dopo avere aggiunto il computer alla rete, utilizzare il nome utente e la password nuovi per accedere al computer.  

    > [!NOTE]
    >  Quando si effettua il primo accesso a un computer che esegue Windows 8 utilizzando il proprio account di rete, dopo che il computer si è connesso al server verranno visualizzate le istruzioni per la migrazione dei file e delle applicazioni dall'account utente precedente. Seguire le istruzioni riportate nella pagina **Migrazione di file e applicazioni da un account utente precedente** per migrare tutti i file e le applicazioni all'account utente di rete.  

9. Dopo che il computer è connesso correttamente al server, i collegamenti all'area di notifica del connettore e al Dashboard del server vengono visualizzati nel menu Start, che può essere utilizzato come segue (se il computer esegue Windows 8, Windows 8.1 o Windows 10, il Dashboard e un connettore Area di notifica saranno disponibile dalla schermata Start del computer):  

    -   Se il computer esegue Windows 8, Windows 8.1 o Windows 10, il Dashboard e l'area di notifica del connettore saranno disponibili per la ricerca come un'App.  

    -   Nell'area di notifica di Connettore è possibile abilitare o disabilitare la funzionalità **Mantieni la connessione in modalità remota** . È anche possibile fare doppio clic sull'area di notifica per avviare la finestra di avvio. Dalla finestra di avvio è possibile accedere al collegamento alle cartelle condivise, configurare le operazioni di backup del computer, risolvere gli avvisi e accedere al sito Accesso Web remoto.  

    -   Il collegamento **Dashboard** permette di amministrare il server.  

###  <a name="BKMK_10"></a> Connettere computer a un server Windows Server Essentials senza aggiungerli al dominio  
 Questo argomento descrive come aggiungere un computer Windows 7, Windows 8, Windows 8.1 o Windows 10 a una rete di Windows Server Essentials senza aggiungere il computer al dominio di Windows Server Essentials in una distribuzione client locale. Questo metodo di connessione è supportato in Windows Server Essentials e Windows Server Essentials.  

 Si tratta di un'alternativa al metodo abituale, che richiede l'aggiunta del computer al dominio di Windows Server Essentials. Con il metodo normale, se il computer si trova in un altro dominio sarà necessario rimuoverlo da quel dominio prima di aggiungerlo al dominio di Windows Server Essentials.  

#### <a name="feature-limits"></a>Limiti della funzionalità  
 Alcune funzionalità sono limitate quando un computer client non è aggiunto al dominio di Windows Server Essentials:  

-   Tutte le funzionalità che richiedono che il computer deve essere aggiunti al dominio? tra cui reti private virtuali (VPN), le credenziali di dominio e criteri di gruppo? non sono disponibili.  

-   Eventuali componenti aggiuntivi di terze parti che richiedono l'aggiunta del computer al dominio non funzioneranno correttamente.  

-   Non è possibile usare questo metodo per la connessione di un computer remoto.  

#### <a name="prerequisites"></a>Prerequisiti  

-   Il computer deve avere una connessione fisica alla rete locale.  

-   Uno dei sistemi operativi seguenti deve essere installato nel computer:  

    -   Windows 10 Pro, Windows 10 Enterprise  

    -    Windows 8.1 Pro,  Windows 8.1 Enterprise,  Windows 8 Pro,  Windows 8 Enterprise  

    -    Windows 7 Professional (x86 e x64), Windows 7 Enterprise (x86 e x64), Windows 7 Ultimate (x86 e x64)  


-   Il computer deve soddisfare tutti gli altri requisiti per i computer client in Windows Server Essentials. Per altre informazioni, vedere [Prerequisites for connecting a computer to the server](Get-Connected-in-Windows-Server-Essentials.md#BKMK_2).  


-   Per abilitare una connessione senza aggiunta al dominio, è necessario accedere al computer con un account appartenente al gruppo Administrators locale.  

-   Per la connessione del computer al server di Windows Server Essentials saranno necessarie le informazioni seguenti sull'account:  

    -   Account con diritti di amministratore in Windows Server Essentials (account in uso).  

    -   Nome utente e password per l'account di dominio dell'utente che userà il computer. L'account di dominio deve disporre anche di diritti di amministratore nel server di Windows Server Essentials.  

#### <a name="connect-to-a-windows-server-essentials-network"></a>Connessione a una rete di Windows Server Essentials  
 Dopo avere verificato che tutti i prerequisiti siano stati soddisfatti, connettere il computer alla rete di Windows Server Essentials.  

###### <a name="to-connect-a-computer-in-a-different-domain-to-a-windows-server-essentials-network"></a>Per connettere un computer di un dominio diverso a una rete di Windows Server Essentials  

1.  Accedere al computer client con un account appartenente al gruppo Administrators.  

2.  Aprire un prompt dei comandi con diritti di amministratore.  

    -   In Windows 10, fare clic sui **avviare** pulsante, seleziona **tutte le app** -> **utilità di sistema di Windows** -> **prompt dei comandi**, fare doppio clic su prompt dei comandi e quindi fare clic su **Esegui come amministratore**.  

    -   In Windows 8 sul **avviare** , digitare **comando** e quindi premere INVIO. Nei risultati fare clic con il pulsante destro del mouse su **Prompt dei comandi**, quindi scegliere **Esegui come amministratore**.  

    -   In Windows 7 sul **avviare** menu, immettere **comando** nella casella di ricerca, fare doppio clic su **prompt dei comandi**e quindi fare clic su **Esegui come amministratore**.  

3.  Al prompt dei comandi digitare il comando seguente e quindi premere INVIO:  

    ```  
    reg add "HKLM\SOFTWARE\Microsoft\Windows Server\ClientDeployment" /v SkipDomainJoin /t REG_DWORD /d 1  
    ```  


4.  Completare i passaggi descritti in [Connettere i computer al server](Get-Connected-in-Windows-Server-Essentials.md#BKMK_9).  


####  <a name="BKMK_SecondServer"></a> Aggiungere un secondo server alla rete  

###### <a name="to-join-a-second-server-to-the-network"></a>Per aggiungere un secondo server alla rete  

1.  Accedere al server da connettere alla rete di Windows Server Essentials.  

2.  Aprire un browser Internet e nella barra degli indirizzi, digitare **http://<servername\>/Connect**, dove *< servername\>*  è il nome del server che esegue Windows Server Essentials e quindi premere INVIO.  

3.  Se la Sicurezza avanzata di Internet Explorer è abilitata sul server che si vuole connettere alla rete di Windows Server Essentials, completare la procedura seguente. In caso contrario, ignorare questo passaggio.  

    1.  Per accettare il messaggio di blocco, fare clic su **Chiudi**.  

    2.  Aggiungere il **http://<servername\>/Connect** sito Web per siti Web attendibili come indicato di seguito:  

        1.  Nel riquadro di spostamento del browser fare clic su **Strumenti**, quindi su **Opzioni Internet**.  

        2.  Fare clic sulla scheda **Sicurezza**, quindi su **Siti attendibili**.  

        3.  Fare clic su **Siti**.  

        4.  Il sito Web dovrebbe essere visualizzato nel campo **Aggiungi il sito Web all'area** . Fai clic su **Aggiungi**.  

        5.  Fare clic su **Chiudi**e quindi su **OK**.  

    3.  Aggiornare la pagina Web.  


    4.  Per connettere il secondo server a un server che esegue Windows Server Essentials, seguire le istruzioni disponibili in [Connect computers to the server](Get-Connected-in-Windows-Server-Essentials.md#BKMK_9).  


~~~
> [!NOTE]
>  Connecting a second server to a server running Windows Server Essentials differs from connecting to a client computer as follows:  
>   
>  -   There is no user profile migration; therefore, the current profile will not be migrated.  
> -   You cannot back up the second server by using client computer backup, and there is no option to wake up the computer for backup.  
~~~

 Dopo l'aggiunta del secondo server a un server che esegue Windows Server Essentials, nel server connesso saranno disponibili le funzionalità seguenti:  

- Il funzionamento delle funzionalità relative allo stato di aggiornamenti e avvisi è uguale a quello degli altri computer client.  

- Il funzionamento delle funzionalità online e offline è uguale a quello degli altri computer client.  

- È possibile connettersi al secondo server tramite Accesso Web remoto.  

- Il secondo server sarà incluso nei Rapporti di stato, poiché Windows Server Essentials genererà avvisi correlati a questo server.  

  La gestione del secondo server dal server che esegue Windows Server Essentials presenterà le differenze seguenti rispetto alla gestione degli altri computer client:  

- Non sarà disponibile alcun punto di ingresso per l'area di notifica, la finestra di avvio e il client dashboard.  

- Il secondo server è elencato nel gruppo **Server** della scheda **Dispositivi** .  

- Poiché il backup del computer client non è supportato per il secondo server, lo stato del backup sarà **Non supportato**. Se inoltre si seleziona il secondo server e si fa clic con il pulsante destro del mouse, non saranno visualizzate attività di backup e ripristino correlate per il secondo server.  

- Se si seleziona il secondo server e quindi si fa clic sull'attività **Visualizza proprietà del server** , nella pagina delle proprietà del server non sarà visualizzata la scheda **Backup** .  

- Poiché il sistema operativo Windows Server non include alcun Centro sicurezza PC, lo stato relativo alla sicurezza del secondo server sarà **Non applicabile**.  

- Lo stato di Criteri di gruppo del secondo server sarà **Non applicabile**.  

###  <a name="BKMK_11"></a> Installare il software connettore  
 Il software Connettore in Windows Server Essentials sarà installato quando si connette il computer al server tramite la procedura guidata Connetti il computer al server. È possibile avviare questa procedura guidata digitando **http://<ServerName\>/Connect** nella barra degli indirizzi del web browser (dove *< ServerName\>*  è il nome del server).  

> [!NOTE]
>  Se il computer si trova in una posizione remota, per eseguire la connessione di un Computer per la procedura guidata Server, digitare **http://<domainname\>/Connect** nella barra degli indirizzi del web browser (in cui *< dominio\>*  è il nome di dominio dell'organizzazione). È possibile ottenere le informazioni sul nome di dominio dall'amministratore di rete.  

 Il software Connettore esegue le operazioni seguenti:  

-   Connessione del computer a Windows Server Essentials.  

-   Backup automatico del computer ogni notte, se si configurare il server per la creazione di backup client.  

-   Semplificazione del monitoraggio dell'integrità del computer da parte dell'amministratore.  

-   Abilitazione della possibilità di configurare e amministrare in modalità remota di Windows Server Essentials dal computer di casa.  


 Per istruzioni dettagliate sulla connessione del computer al server di Windows Server Essentials, vedere [Connect computers to the server](Get-Connected-in-Windows-Server-Essentials.md#BKMK_9).   


###  <a name="BKMK_12"></a> Spostare i dati del computer e le impostazioni manualmente  
  Windows Server Essentials e Windows Server Essentials supportano la migrazione dei profili utente solo per i computer client che eseguono il sistema operativo Windows 7. Quando si connette un computer basato su Windows 7 al server, la procedura guidata Connetti il computer al server potrà eseguire automaticamente la migrazione del profilo utente.  

 Il profilo utente non può essere trasferito automaticamente quando ci si connette un computer Windows 10 o Windows 8.1, Windows 8 nel server. In un computer Windows 8 è tuttavia possibile usare Trasferimento dati Windows per il trasferimento di dati e impostazioni dall'utente locale originale al computer aggiunto al dominio. Per eseguire questa operazione, è necessario essere amministratore nel computer di origine Windows 8 e nel computer di destinazione Windows 8. Per informazioni sull'uso di Trasferimento dati Windows per il trasferimento di file e impostazioni, vedere l' [articolo 2735227](https://support.microsoft.com/kb/2735227) della Microsoft Knowledge Base.  

###  <a name="BKMK_Transfer"></a> Trasferire più profili utente durante la distribuzione di computer  
 Prima di connettere un computer che esegue il sistema operativo Windows 7 o Windows 7 SP1 al server di Windows Server Essentials, per trasferire più profili utente locali sarà necessario creare prima di tutto gli account utente di rete corrispondenti sul server. Per altre informazioni sulla creazione di account utente di rete, vedere [Aggiungere un account utente](../manage/Manage-User-Accounts-in-Windows-Server-Essentials.md#BKMK_Manage1).  

 Migrazione del profilo utente è supportata solo in un computer che esegue Windows 7 (per Windows Server Essentials) o Windows 7 SP1 (per Windows Server Essentials). Quando si connette un computer al server di Windows Server Essentials usando la procedura guidata Connetti il computer al server, sarà disponibile un'opzione per lo spostamento dei dati e delle impostazioni degli utenti degli account locali precedenti nei nuovi account utente di rete. A tale scopo, nella pagina **Sposta impostazioni e dati utente esistenti** della procedura guidata mappare gli account utente di rete agli account utente locali esistenti nel computer per trasferire più profili utente presenti nel computer client.  

###  <a name="BKMK_13"></a> Disinstallare il software connettore  
 È possibile disinstallare il software Connettore da un computer mediante il Pannello di controllo. La disinstallazione si esegue in genere in caso di problemi con il software Connettore o se è necessario installare una versione più recente del software Connettore. Per completare questa procedura, è necessario accedere al computer come amministratore.  

> [!IMPORTANT]
>  Se si aggiorna il sistema operativo in un computer client, il software Connettore sarà in genere disinstallato automaticamente. Sarà necessario reinstallare il software Connettore al termine dell'aggiornamento. Il metodo preferito consiste nel disinstallare il software Connettore prima dell'aggiornamento del sistema operativo. La disinstallazione del software Connettore al termine dell'aggiornamento è comunque accettabile, ma potrebbe causare uno stato incoerente per il computer client rispetto al server, fino alla disinstallazione e alla reinstallazione del software Connettore.  

##### <a name="to-uninstall-connector-software-from-a-computer"></a>Per disinstallare il software Connettore da un computer  

1.  In un computer che esegue Windows 7, Windows 8, Windows 8.1 o Windows 10, aprire **Pannello di controllo**e quindi la **programmi** fare clic su **Visualizza aggiornamenti installati**.  

2.  Dall'elenco di programmi installati selezionare **Connettore Windows Server Essentials**, quindi fare clic su **Disinstalla**.  

3.  Nella pagina di avviso fare clic su **Sì**.  

4.  Se si apre la finestra **Controllo account utente** , fare clic su **Consenti**.  

5.  In Windows Server Essentials, se la pagina di connettore Windows Server Essentials è suggerita per chiudere la finestra di avvio, fare clic su **OK**.  

6.  Attendere la disinstallazione del programma. Dopo la rimozione del software, il **Connettore Windows Server Essentials** non sarà più incluso nell'elenco di programmi o aggiornamenti installati. I collegamenti alla finestra di avvio e al dashboard, inoltre, non saranno più visualizzati sul desktop del computer.  

> [!NOTE]
> - La disinstallazione del software Connettore non rimuove il computer dall'elenco di computer visualizzati nella scheda **DISPOSITIVI** del dashboard. Per rimuovere il computer dal dashboard, vedere [Remove a computer from the server](../manage/Manage-Devices-in-Windows-Server-Essentials.md#BKMK_3).  
>   -   Quando si disinstalla il software Connettore, le cartelle condivise nel computer client mappate al server non saranno eliminate. È necessario eliminare manualmente le cartelle condivise mappate al server.  
> 
> -   La disinstallazione del software Connettore non separa il computer dal dominio originale. È necessario separare manualmente il computer da dominio. Per ulteriori informazioni, vedere [Remove a computer from a Windows domain](Get-Connected-in-Windows-Server-Essentials.md#BKMK_8).  


###  <a name="BKMK_14"></a> Disconnettere il computer da o riconnettere il computer al server  
 Per disconnettere un computer dal server, è necessario completare i passaggi seguenti:  


1. Disinstallare il software Connettore dal computer usando il Pannello di controllo. Per istruzioni dettagliate, vedere [Disinstallare il software Connettore](Get-Connected-in-Windows-Server-Essentials.md#BKMK_13).   


2. Separare il computer dal dominio di Windows Server Essentials e aggiungerlo al gruppo di lavoro. Per istruzioni dettagliate sull'aggiunta di Windows a un gruppo di lavoro, [Creare un gruppo di lavoro o aggiungervi un computer](https://windows.microsoft.com/windows7/Join-or-create-a-workgroup).  

3. Rimuovere il computer dal server tramite il dashboard. Per le istruzioni dettagliate, vedere [Remove a computer from the server](../manage/Manage-Devices-in-Windows-Server-Essentials.md#BKMK_3).  

   Per riconnettere al server un computer disconnesso in precedenza dalla rete di server di Windows Server Essentials, è necessario completare i passaggi seguenti:  


4. Disinstallare il software Connettore dal computer usando il Pannello di controllo. Per istruzioni dettagliate, vedere [Disinstallare il software Connettore](Get-Connected-in-Windows-Server-Essentials.md#BKMK_13).  

5. Separare il computer dal dominio di Windows Server Essentials e aggiungerlo al gruppo di lavoro. Per istruzioni dettagliate sull'aggiunta di Windows a un gruppo di lavoro, [Creare un gruppo di lavoro o aggiungervi un computer](https://windows.microsoft.com/windows7/Join-or-create-a-workgroup).  

6. Connettere il computer al server tramite la procedura guidata Connessione del computer. Per le istruzioni dettagliate, vedere [Connect computers to the server](Get-Connected-in-Windows-Server-Essentials.md#BKMK_9).  

###  <a name="BKMK_Sleep"></a> Come funziona con la sospensione di backup e le modalità di ibernazione  
 Se si seleziona l'opzione **Riattiva il computer per il backup** quando si connette un computer al server, il computer sarà riattivato automaticamente ogni giorno dalla modalità sospensione o ibernazione, in base a quanto specificato nella pianificazione del backup, in modo da permetterne il backup. Al termine del backup, il computer tornerà in modalità sospensione o ibernazione, in base alle rispettive impostazioni di risparmio energia. Se non si seleziona questa opzione, il server non eseguirà il backup del computer che si trova in modalità sospensione o ibernazione. Per altre informazioni, vedere [Manage Backup Client](../manage/Manage-Client-Computer-Backup-in-Windows-Server-Essentials.md).  

##  <a name="BKMK_C"></a> Utilizzare la finestra di avvio  
 È possibile usare la finestra di avvio per accedere alle risorse condivise dal server di Windows Server Essentials, eseguire backup dei computer e rispondere agli avvisi relativi all'integrità del sistema.  

-   [Panoramica di LaunchPad](../manage/Overview-of-the-Launchpad-in-Windows-Server-Essentials.md)  

-   [Usare la finestra di avvio con un computer Mac](../manage/Overview-of-the-Launchpad-in-Windows-Server-Essentials.md#BKMK_Mac)  

## <a name="see-also"></a>Vedere anche  

-   [Risolvere i problemi dei computer che esegue la connessione al server](../support/Troubleshoot-connecting-computers-to-the-server-in-Windows-Server-Essentials.md)  

-   [Gestire gli account utente](../manage/Manage-User-Accounts-in-Windows-Server-Essentials.md)  


-   [Usare le cartelle condivise](Use-Shared-Folders-in-Windows-Server-Essentials.md)  

-   [Lavorare in modalità remota](Work-Remotely-in-Windows-Server-Essentials.md)  

-   [Riprodurre file multimediali digitali](Play-Digital-Media-in-Windows-Server-Essentials.md)

-   [Usare le cartelle condivise](../use/Use-Shared-Folders-in-Windows-Server-Essentials.md)  

-   [Lavorare in modalità remota](../use/Work-Remotely-in-Windows-Server-Essentials.md)  

-   [Riprodurre file multimediali digitali](../use/Play-Digital-Media-in-Windows-Server-Essentials.md)

