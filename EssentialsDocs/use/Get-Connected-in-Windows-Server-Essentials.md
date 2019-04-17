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
ms.openlocfilehash: 1d7f3c33f1254c8dbe4af8bdf5baa4144c134248
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/12/2017
---
# <a name="get-connected-in-windows-server-essentials"></a>Connessione in Windows Server Essentials

>Si applica a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials
  
 È possibile connettere il computer al server di Windows Server Essentials tramite il software connettore. Il software connettore viene installato quando si connette un computer al server tramite la connessione del Computer per la creazione guidata Server. È possibile avviare questa procedura guidata digitando **http://<servername\>/connect**, dove **< servername\ >** è il nome del server.  
  
 In questo argomento:  
  

-   [Preparare la connessione dei computer al server](Get-Connected-in-Windows-Server-Essentials.md#BKMK_A)  
  
-   [Connettere i computer al server tramite il software connettore](Get-Connected-in-Windows-Server-Essentials.md#BKMK_B)  
  
-   [Utilizzare la finestra di avvio](Get-Connected-in-Windows-Server-Essentials.md#BKMK_C)  

-   [Preparare la connessione dei computer al server](Get-Connected-in-Windows-Server-Essentials.md#BKMK_A)  
  
-   [Connettere i computer al server tramite il software connettore](Get-Connected-in-Windows-Server-Essentials.md#BKMK_B)  
  
-   [Utilizzare la finestra di avvio](Get-Connected-in-Windows-Server-Essentials.md#BKMK_C)  

  
##  <a name="BKMK_A"></a>Preparare la connessione dei computer al server  
 Questa sezione illustra il software connettore, i sistemi operativi supportati da Windows Server Essentials, le attività dei prerequisiti che devono essere completate prima di connettere i computer al server e le modifiche apportate dal server ai computer quando si esegue il software connettore.  
  

-   [Panoramica del software connettore](Get-Connected-in-Windows-Server-Essentials.md#BKMK_1)  
  
-   [Prerequisiti per la connessione di un computer al server](Get-Connected-in-Windows-Server-Essentials.md#BKMK_2)  
  
-   [Prerequisiti per la connessione di un computer Mac alla rete](Get-Connected-in-Windows-Server-Essentials.md#BKMK_3)  
  
-   [Sistemi operativi supportati per i computer client](Get-Connected-in-Windows-Server-Essentials.md#BKMK_4)  
  
-   [Modifiche apportate dal server a un computer client](Get-Connected-in-Windows-Server-Essentials.md#BKMK_5)  
  
-   [Informazioni nome utente e password di rete](Get-Connected-in-Windows-Server-Essentials.md#BKMK_6)  
  
-   [Account amministratore s del server](Get-Connected-in-Windows-Server-Essentials.md#BKMK_7)  
  
-   [Rimuovere un computer da un dominio di Windows](Get-Connected-in-Windows-Server-Essentials.md#BKMK_8)  
  
###  <a name="BKMK_1"></a>Panoramica del software connettore  
 Il software connettore per il sistema operativo Windows Server Essentials connette i computer nella rete al server di Windows Server Essentials. Quando si connettono computer al server, il software connettore consente di eseguire il backup dei computer e monitorarne l'integrità automaticamente. Il software connettore permette anche di configurare e amministrare in remoto i server di Windows Server Essentials. Il software connettore viene installato quando si connette un computer client al server. Per istruzioni dettagliate sulla connessione dei computer client al server di Windows Server Essentials, vedere [connettere i computer al server](Get-Connected-in-Windows-Server-Essentials.md#BKMK_9) più avanti in questo argomento.  

-   [Panoramica del software connettore](Get-Connected-in-Windows-Server-Essentials.md#BKMK_1)  
  
-   [Prerequisiti per la connessione di un computer al server](../use/Get-Connected-in-Windows-Server-Essentials.md#BKMK_2)  
  
-   [Prerequisiti per la connessione di un computer Mac alla rete](Get-Connected-in-Windows-Server-Essentials.md#BKMK_3)  
  
-   [Sistemi operativi supportati per i computer client](Get-Connected-in-Windows-Server-Essentials.md#BKMK_4)  
  
-   [Modifiche apportate dal server a un computer client](Get-Connected-in-Windows-Server-Essentials.md#BKMK_5)  
  
-   [Informazioni nome utente e password di rete](Get-Connected-in-Windows-Server-Essentials.md#BKMK_6)  
  
-   [Account amministratore s del server](Get-Connected-in-Windows-Server-Essentials.md#BKMK_7)  
  
-   [Rimuovere un computer da un dominio di Windows](Get-Connected-in-Windows-Server-Essentials.md#BKMK_8)  
  
###  <a name="BKMK_1"></a>Panoramica del software connettore  
 Il software connettore per il sistema operativo Windows Server Essentials connette i computer nella rete al server di Windows Server Essentials. Quando si connettono computer al server, il software connettore consente di eseguire il backup dei computer e monitorarne l'integrità automaticamente. Il software connettore permette anche di configurare e amministrare in remoto i server di Windows Server Essentials. Il software connettore viene installato quando si connette un computer client al server. Per istruzioni dettagliate sulla connessione dei computer client al server di Windows Server Essentials, vedere [connettere i computer al server](Get-Connected-in-Windows-Server-Essentials.md#BKMK_9) più avanti in questo argomento.  

  
###  <a name="BKMK_2"></a>Prerequisiti per la connessione di un computer al server  
 Prima di connettere un computer alla rete devono essere soddisfatti i seguenti requisiti:  
  
-   L'installazione di Windows Server Essentials è stata completata e il server è in esecuzione. Il software connettore sarà interrotta l'installazione se non è in grado di comunicare con il server.  
  

-   Il computer client è in esecuzione un sistema operativo supportato. Per ulteriori informazioni, vedere [sistemi operativi supportati per i computer client](Get-Connected-in-Windows-Server-Essentials.md#BKMK_4).

  
-   Il computer client deve avere una connessione valida a Internet.  
  
-   Il computer client è nella stessa subnet IP del server che esegue Windows Server Essentials quando il computer client è nella stessa rete del server.  
  
-   Il computer client è installato .NET Framework 4.5. Il software connettore sarà installato automaticamente nel computer.  
  
-   Il client soddisfi i requisiti minimi di sistema seguenti:  
  
    -   Processore 1,4 GHz o superiore  
  
    -   1 GB di RAM o superiore  
  
    -   1 GB di spazio disponibile sul disco rigido (una porzione di questo disco sarà resa disponibile dopo l'installazione)  
  
-   La partizione di avvio (vale a dire la partizione disco in cui è installato il sistema operativo Windows) è formattata con il file system NTFS.  
  
-   Il nome del computer non include più di 15 caratteri.  
  
-   Il nome del computer non include un carattere di sottolineatura (_).  
  
-   Le impostazioni di data e ora del computer s allineano alle impostazioni nel server.  
  
-   Un computer client può essere connesso a un solo server di Windows Server Essentials in un determinato momento.  
  
-   Un computer client che è già stato aggiunto a un altro dominio di Active Directory non può essere aggiunto a un dominio Windows Server Essentials.  
  
> [!NOTE]

>  In una distribuzione client locale per Windows Server Essentials o Windows Server Essentials, è possibile connettere i computer al server senza aggiungerli al dominio di Windows Server Essentials. Questo metodo non è disponibile per tutti i sistemi operativi client supportati e funzionalità quali criteri di gruppo e reti private virtuali (VPN), che è necessario che un computer sia connesso al dominio, non sono disponibili. Per requisiti e istruzioni, vedere [connettere i computer a un server Windows Server Essentials senza aggiungerli al dominio](Get-Connected-in-Windows-Server-Essentials.md#BKMK_10).  
  
 Per istruzioni dettagliate connettere un computer al server che esegue Windows Server Essentials, vedere [connettere i computer al server](Get-Connected-in-Windows-Server-Essentials.md#BKMK_9).  

>  In una distribuzione client locale per Windows Server Essentials o Windows Server Essentials, è possibile connettere i computer al server senza aggiungerli al dominio di Windows Server Essentials. Questo metodo non è disponibile per tutti i sistemi operativi client supportati e funzionalità quali criteri di gruppo e reti private virtuali (VPN), che è necessario che un computer sia connesso al dominio, non sono disponibili. Per requisiti e istruzioni, vedere [connettere i computer a un server Windows Server Essentials senza aggiungerli al dominio](Get-Connected-in-Windows-Server-Essentials.md#BKMK_10).  
  
 Per istruzioni dettagliate connettere un computer al server che esegue Windows Server Essentials, vedere [connettere i computer al server](Get-Connected-in-Windows-Server-Essentials.md#BKMK_9).  

  
###  <a name="BKMK_3"></a>Prerequisiti per la connessione di un computer Mac alla rete  
 Prima di connettere un computer Mac alla rete devono essere soddisfatti i seguenti requisiti:  
  
-   È stata completata l'installazione del sistema operativo del server e il server è in esecuzione. Il software connettore non verrà installato se in grado di comunicare con il server.  
  
-   Il computer è in esecuzione Mac OS X 10.5 (Leopard) o versione successiva.  
  
-   Il computer è nella stessa subnet IP del server.  
  
-   Il computer deve avere una connessione valida a Internet.  
  
-   Assicurarsi che il computer soddisfi i requisiti minimi di sistema seguenti:  
  
    -   Processore 1,4 GHz o superiore  
  
    -   1 GB di RAM o superiore  
  
    -   1 GB di spazio disponibile sul disco rigido (una porzione di questo disco sarà resa disponibile dopo l'installazione)  
  
-   Un computer client può essere connesso a un solo server in un determinato momento.  
  
###  <a name="BKMK_4"></a>Sistemi operativi supportati per i computer client  
 Windows Server Essentials offre lo stesso set di funzionalità per tutti i computer client supportati. Queste funzionalità includono aggiunta a un dominio, finestra di avvio e notifiche sull'integrità lato client.  
  
> [!IMPORTANT]
>  Aggiungere i computer che eseguono le versioni Home, Starter o Media Center di Windows per il dominio non supporta Windows Server Essentials. Inoltre, è possibile usare accesso Web remoto per connettersi a tali computer.  
  
#### <a name="windows-server-essentials"></a>Windows Server Essentials  
  In questa sezione si riferisce a un server che esegue Windows Server Essentials, oppure un server che esegue Windows Server 2012 R2 Standard o Windows Server 2012 R2 Datacenter con il ruolo esperienza Windows Server Essentials installato. Sono supportati i sistemi operativi seguenti:  
  
 **Sistemi operativi Windows 7**  
  
-    Windows 7 Home Basic SP1 (x86 e x64)  
  
-    Windows 7 Home Premium SP1 (x86 e x64)  
  
-    Windows 7 Professional SP1 (x86 e x64)  
  
-    Windows 7 Ultimate SP1 (x86 e x64)  
  
-    Windows 7 Enterprise SP1 (x86 e x64)  
  
-    Windows 7 Starter SP1 (x86)  
  
 **Windows 8 operating systems**  
  
-   Windows 8  
  
-   Windows 8 Professional  
  
-   Windows 8 Enterprise  
  
 **Windows 8.1 operating systems**  
  
-   Windows 8.1  
  
-   Windows 8.1 Professional  
  
-   Windows 8.1 Enterprise  
  
 **Windows 10 operating systems**  
  
-   Windows 10  
  
-   Windows 10 Professional  
  
-   Windows 10 Enterprise  
  
-   Windows 10 Education  
  
 **Computer client Mac**  
  
-   Mac OS X 10.5 Leopard  
  
-   Mac OS X v10.6 neve Leopard  
  
-   Mac OS X v10.7 Lion  
  
-   Mac OS X v10.8 montagna Lion  
  
> [!NOTE]
>  È possibile visualizzare lo stato e lo stato del backup per un computer Mac dal Dashboard di Windows Server Essentials. Tuttavia, Impossibile configurare il backup di computer o avvia un backup dal Dashboard. Inoltre, è possibile usare accesso Web remoto per connettere un computer Mac.  
  
#### <a name="windows-server-essentials"></a>Windows Server Essentials  
  In questa sezione si applica a un server che esegue Windows Server Essentials. Sono supportati i sistemi operativi seguenti:  
  
 **Sistemi operativi Windows 7**  
  
-    Windows 7 Home Basic (x86 and x64)  
  
-    Windows 7 Home Premium (x86 and x64)  
  
-    Windows 7 Professional (x86 and x64)  
  
-    Windows 7 Ultimate (x86 and x64)  
  
-    Windows 7 Enterprise (x86 and x64)  
  
-    Windows 7 Starter (x86)  
  
 **Windows 8 operating systems**  
  
-   Windows 8  
  
-   Windows 8 Professional  
  
-   Windows 8 Enterprise  
  
 **Windows 10 operating systems**  
  
-   Windows 10  
  
-   Windows 10 Professional  
  
-   Windows 10 Enterprise  
  
-   Windows 10 Education  
  
 **Computer client Mac**  
  
-   Mac OS X 10.5 Leopard  
  
-   Mac OS X v10.6 neve Leopard  
  
-   Mac OS X v10.7 Lion  
  
> [!NOTE]
>  È possibile visualizzare lo stato e lo stato del backup per un computer Mac dal Dashboard di Windows Server Essentials. Tuttavia, Impossibile configurare il backup di computer o avvia un backup dal Dashboard. Inoltre, è possibile usare accesso Web remoto per connettere un computer Mac.  
  
###  <a name="BKMK_5"></a>Modifiche apportate dal server a un computer client  
 Quando si connette un computer al server, il software di Windows Server Essentials apporta alcune modifiche al computer in modo che il computer e il server di collaborare.  
  
 Il software esegue le operazioni seguenti:  
  
-   Installa il software connettore nel computer  
  
-   Installa Microsoft .NET Framework 4.5 nel computer, se non è già installato  
  
-   Creazione di collegamenti sul desktop s computer per il Dashboard e finestra di avvio  
  
-   Configura le porte del Firewall di Windows nel computer per consentire le seguenti funzioni:  
  
    -   Funzionalità di base rete  
  
    -   Servizi Desktop remoto  
  
-   Apporta le modifiche seguenti al computer per semplificare i backup:  
  
    -   Crea attività pianificate per l'esecuzione di backup automatici  
  
    -   Installa i servizi che gestiscono le operazioni di backup con il server  
  
    -   Installa un driver di periferica virtuale che viene usato durante i processi di ripristino di file e cartelle  
  
-   Installa l'Agente integrità per rilevare problemi e creare le notifiche di avviso corrispondenti  
  
-   Crea le attività pianificate nel computer per valutazioni ricorrenti dell'integrità e per la sincronizzazione delle definizioni degli avvisi di integrità  
  
-   Aggiunge servizi al computer, che il computer utilizza per comunicare con il server e altre funzionalità di Windows Server Essentials  
  
-   Apertura della porta TCP 3389 nei computer client che eseguono i client Windows per consentire Servizi Desktop remoto per l'esecuzione  
  
-   Distribuzione della VPN nel computer client e fornisce un'esperienza con clic singolo se la funzionalità VPN è abilitata in Windows Server Essentials, o di un'esperienza di connessione automatica in caso di abilitazione della funzionalità VPN in Windows Server Essentials  
  

 Per informazioni sulla connessione del computer al server, vedere [connettere i computer al server](Get-Connected-in-Windows-Server-Essentials.md#BKMK_9).  
  
###  <a name="BKMK_6"></a>Informazioni nome utente e password di rete  
 È possibile ottenere le informazioni di nome e una password utente rete alla persona che gestisce il server. È possibile utilizzare queste credenziali per connettere il computer al server e accedere a informazioni dal server.  
  
###  <a name="BKMK_6"></a>Informazioni nome utente e password di rete  
 È possibile ottenere le informazioni di nome e una password utente rete alla persona che gestisce il server. È possibile utilizzare queste credenziali per connettere il computer al server e accedere a informazioni dal server. 

  
 Se sei un amministratore del server, è possibile creare le credenziali di rete mediante l'aggiunta di un account utente dal **utenti** scheda del Dashboard. Per ulteriori informazioni sugli account utente, vedere [gestire gli account utente tramite il Dashboard](../manage/Manage-User-Accounts-in-Windows-Server-Essentials.md#BKMK_Manage8).  
  
###  <a name="BKMK_7"></a>Account amministratore s del server  
 È necessario essere in grado di fornire un nome di account di amministratore di rete e una password per installare il software connettore. Un account amministratore di rete permette all'utente di gestire la rete locale per l'organizzazione e consente di gestire e mantenere i dispositivi di rete, ad esempio commutatori e router.  
  
 Le attività che possono essere eseguite utilizzando un account amministratore di rete possono includere:  
  
-   L'installazione di applicazioni di rete e altro software  
  
-   Gestione dell'archiviazione nel server di  
  
-   Distribuzione degli aggiornamenti software  
  
-   Esecuzione dei backup periodici  
  
-   Monitoraggio delle attività giornaliere della rete  
  
 In Windows Server Essentials, Windows Server Essentials e Windows Server 2012 R2 con il ruolo esperienza Windows Server Essentials installato, è possibile assegnare l'amministratore di rete livello a qualsiasi account utente di accesso. In questo modo si concede le autorizzazioni necessarie per eseguire attività di amministratore di rete. Quando un utente è assegnato l'amministratore di rete, livello di accesso di **controllo di accesso utente** prompt dei comandi aperto per qualsiasi attività che richiede le autorizzazioni di amministratore.  
  
###  <a name="BKMK_8"></a>Rimuovere un computer da un dominio di Windows  
 Per rimuovere un computer dal relativo dominio, verrà richiesto per il nome utente e la password dell'account di dominio.  
  
##### <a name="to-remove-a-computer-from-a-windows-domain"></a>Per rimuovere un computer da un dominio di Windows  
  
1.  Fare clic su **Start**, fare doppio clic su **Computer**, quindi fare clic su **proprietà**.  
  
2.  In **impostazioni del nome di dominio e gruppo di lavoro Computer**, fare clic su **modificare le impostazioni**.  
  
    > [!NOTE]
    >  Se viene chiesto di immettere una password di amministratore o una conferma, digita la password di dominio o immetti la conferma.  
  
3.  Nel **proprietà del sistema** la finestra di dialogo, fare clic su di **nome Computer** scheda e quindi fare clic su **modifica**.  
  
4.  Nel **cambiamenti dominio/nome Computer** nella finestra di dialogo **appartenente**, fare clic su **gruppo di lavoro**, quindi effettuare una delle operazioni seguenti:  
  
    1.  Per aggiungere un gruppo di lavoro esistente, digitare il nome del gruppo di lavoro che si desidera aggiungere e quindi fare clic su **OK**.  
  
    2.  Per creare un gruppo di lavoro, digitare il nome del gruppo di lavoro che si desidera creare, quindi fare clic su **OK**.  
  
        > [!NOTE]
        >  Il computer verrà rimosso dal dominio e l'account computer del dominio verrà disabilitato.  
  
##  <a name="BKMK_B"></a>Connettere i computer al server tramite il software connettore  
 In questa sezione fornisce l'accesso alle procedure e alle informazioni che consentiranno di installare il software connettore, connettere il computer al server e risolvere i problemi di connessione dei computer al server.  
  

-   [Connettere i computer al server](Get-Connected-in-Windows-Server-Essentials.md#BKMK_9)  
  
-   [Connettere i computer a un server Windows Server Essentials senza aggiungerli al dominio](Get-Connected-in-Windows-Server-Essentials.md#BKMK_10)  
  
-   [Installare il software connettore](Get-Connected-in-Windows-Server-Essentials.md#BKMK_11)  
  
-   [Spostare i dati dei computer e le impostazioni manualmente](Get-Connected-in-Windows-Server-Essentials.md#BKMK_12)  
  
-   [Trasferire più profili utente durante la distribuzione di computer](Get-Connected-in-Windows-Server-Essentials.md#BKMK_Transfer)  
  
-   [Disinstallare il software connettore](Get-Connected-in-Windows-Server-Essentials.md#BKMK_13)  
  
-   [Disconnettere il computer da o riconnettere il computer al server](Get-Connected-in-Windows-Server-Essentials.md#BKMK_14)  
  
-   [Come eseguire il backup con stato di sospensione e ibernazione](Get-Connected-in-Windows-Server-Essentials.md#BKMK_Sleep)  

-   [Connettere i computer al server](Get-Connected-in-Windows-Server-Essentials.md#BKMK_9)  
  
-   [Connettere i computer a un server Windows Server Essentials senza aggiungerli al dominio](Get-Connected-in-Windows-Server-Essentials.md#BKMK_10)  
  
-   [Installare il software connettore](Get-Connected-in-Windows-Server-Essentials.md#BKMK_11)  
  
-   [Spostare i dati dei computer e le impostazioni manualmente](Get-Connected-in-Windows-Server-Essentials.md#BKMK_12)  
  
-   [Trasferire più profili utente durante la distribuzione di computer](Get-Connected-in-Windows-Server-Essentials.md#BKMK_Transfer)  
  
-   [Disinstallare il software connettore](Get-Connected-in-Windows-Server-Essentials.md#BKMK_13)  
  
-   [Disconnettere il computer da o riconnettere il computer al server](Get-Connected-in-Windows-Server-Essentials.md#BKMK_14)  
  
-   [Come eseguire il backup con stato di sospensione e ibernazione](Get-Connected-in-Windows-Server-Essentials.md#BKMK_Sleep)  

  
###  <a name="BKMK_9"></a>Connettere i computer al server  
 Quando si connette un computer a un server che esegue Windows Server Essentials o Windows Server 2012 R2 con il ruolo esperienza Windows Server Essentials installato, verificare che il computer client disponga di una connessione valida a Internet.  
  
 Completare la procedura seguente in tutti i computer client per connetterli al server.  
  
 Per completare la procedura, è necessario le seguenti informazioni:  
  
-   Il nome utente e la password dell'utente che userà il computer sulla rete  
  
-   Il nome utente e la password dell'account amministratore locale del computer s  
  
> [!NOTE]

>  In una distribuzione client locale per Windows Server Essentials o Windows Server Essentials, è possibile connettere i computer al server senza aggiungerli al dominio di Windows Server Essentials. Questo metodo non è disponibile per tutti i sistemi operativi client supportati e funzionalità quali criteri di gruppo e reti private virtuali (VPN), che è necessario che un computer sia connesso al dominio, non sono disponibili. Per requisiti e istruzioni, vedere [connettere i computer a un server Windows Server Essentials senza aggiungerli al dominio](Get-Connected-in-Windows-Server-Essentials.md#BKMK_10).  

>  In una distribuzione client locale per Windows Server Essentials o Windows Server Essentials, è possibile connettere i computer al server senza aggiungerli al dominio di Windows Server Essentials. Questo metodo non è disponibile per tutti i sistemi operativi client supportati e funzionalità quali criteri di gruppo e reti private virtuali (VPN), che è necessario che un computer sia connesso al dominio, non sono disponibili. Per requisiti e istruzioni, vedere [connettere i computer a un server Windows Server Essentials senza aggiungerli al dominio](Get-Connected-in-Windows-Server-Essentials.md#BKMK_10).  

  
##### <a name="to-connect-a-client-computer-to-the-server"></a>Per connettere un computer client al server  
  
1.  Accedere al computer che si desidera connettersi al server.  
  
    > [!NOTE]
    >  Se questo computer ha più account utente, accedere utilizzando l'account utente che dispone di documenti, immagini e le preferenze personali che si desidera mantenere dopo avere connesso il computer al server.  
  
2.  Aprire un browser Internet, ad esempio Internet Explorer.  
  
3.  Nella barra degli indirizzi digitare **http://<servername\>/Connect**, quindi premere INVIO.  
  
    > [!NOTE]
    >  Se il computer si trova in una posizione remota all'esterno della rete di Windows Server Essentials, per eseguire la connessione di un Computer per la procedura guidata Server, digitare **http://<domainname\>/connect** nella barra degli indirizzi del web browser (dove < domain \ > è il nome di dominio dell'organizzazione). È possibile ottenere le informazioni sul nome di dominio dall'amministratore di rete.  
  
4.  Il **connettere il computer al server** verrà visualizzata la pagina. Eseguire una delle operazioni seguenti:  
  
    -   Per un computer che esegue il sistema operativo Windows, fare clic su **Scarica il software per Windows**.  
  
    -   Per un computer che eseguono Mac OS X o versioni successive, fare clic su **Scarica il software per Mac**.  
  
5.  Nel messaggio di avviso sicurezza download file, fare clic su **eseguire**.  
  
6.  Se viene visualizzato il messaggio di controllo dell'Account utente, fare clic su **Sì** oppure digitare il nome utente locale e la password, se richiesto.  
  
7.  Verrà visualizzata Connetti un Computer per la creazione guidata Server. Eseguire il comando seguente per completare la procedura guidata:  
  
    1.  Accettare il contratto di licenza con l'utente finale.  
  
    2.  Nel **Trova server** pagina, rilevare automaticamente il server in reti locali e selezionare il server che si desidera connettersi. In alternativa, se si dispone di informazioni, è possibile specificare manualmente l'indirizzo del server s nome o il dominio.  
  
    3.  Nel **digitare il nuovo nome utente di rete e la password** pagina, eseguire le operazioni seguenti:  
  
        -   Se questo è il primo computer che si connette al server, e se questo è il computer che si utilizzerà per amministrare il server, utilizzare l'account amministratore creato durante l'installazione.  
  
        -   Per tutti gli altri computer, creare innanzitutto un account utente di rete nel server tramite il Dashboard. Creare l'account utente con l'amministratore o Standard privilegi utente, in base alle attività eseguite dalla persona che utilizzerà il computer.  
  
    4.  Se il computer è in esecuzione Windows 8, Windows 8.1 o Windows 10, ignorare questo passaggio. Se il computer è in esecuzione Windows 7 e se si dispone di documenti, immagini o preferenze personali (ad esempio, sfondi del desktop, screen saver o Preferiti di Internet Explorer) che si desidera mantenere dopo aver aggiungere il computer alla nuova rete, nel **scegliere se si desidera spostare i dati esistenti e le impostazioni** pagina della procedura guidata, selezionare il **spostare i dati e le impostazioni per il nuovo account utente di rete**.  
  
    5.  Scegliere se si desidera riattivare automaticamente il computer per creare un backup nel **scegliere se si desidera riattivare il computer per creare la copia di backup** pagina.  
  
    6.  Seguire le istruzioni rimanenti della procedura guidata per aggiungere il computer alla rete.  
  
8.  Dopo avere aggiunto il computer alla rete, utilizzare il nuovo nome utente e la password per accedere al computer.  
  
    > [!NOTE]
    >  Quando si accede a un computer che esegue Windows 8 per la prima volta con l'account di rete, dopo la connessione al server, vengono visualizzate le istruzioni per la migrazione dei file e delle applicazioni dall'account utente precedente. Segui le istruzioni di **come eseguire la migrazione file e delle applicazioni dall'account utente precedente? **pagina per eseguire la migrazione di tutti i file e applicazioni per l'account utente di rete.  
  
9. Dopo che il computer è connesso correttamente al server, i collegamenti all'area di notifica del connettore e al Dashboard del server vengono visualizzati nel menu Start, che può essere utilizzato come indicato di seguito (se il computer è in esecuzione Windows 8, Windows 8.1 o Windows 10, il Dashboard e l'area di notifica del connettore saranno disponibili dalla schermata Start computer s):  
  
    -   Se il computer è in esecuzione Windows 8, Windows 8.1 o Windows 10, il Dashboard e l'area di notifica del connettore sarà possibile eseguire ricerche come un'App.  
  
    -   Dall'area di notifica del connettore, è possibile abilitare o disabilitare il **Mantieni il connessi in remoto** funzionalità. È anche possibile fare doppio clic area di notifica per avviare la finestra di avvio. Dalla finestra di avvio, è possibile accedere al collegamento cartelle condivise, configurare i backup dei computer, risolvere gli avvisi e aprire il sito Web accesso Web remoto.  
  
    -   Dal **Dashboard** collegamento, è possibile amministrare il server.  
  
###  <a name="BKMK_10"></a>Connettere i computer a un server Windows Server Essentials senza aggiungerli al dominio  
 In questo argomento viene descritto come aggiungere un computer Windows 7, Windows 8, Windows 8.1 o Windows 10 a una rete di Windows Server Essentials senza aggiungere il computer al dominio di Windows Server Essentials in una distribuzione client locale. Questo metodo di connessione è supportato in Windows Server Essentials e Windows Server Essentials.  
  
 Questa è un'alternativa al metodo abituale, che richiede l'aggiunta del computer al dominio di Windows Server Essentials. Con questo metodo, se il computer è in un altro dominio, deve essere rimossa dal dominio prima che può essere aggiunto al dominio di Windows Server Essentials.  
  
#### <a name="feature-limits"></a>Limiti della funzionalità  
 Alcune funzionalità sono limitate quando un computer client non viene aggiunto al dominio di Windows Server Essentials:  
  
-   Tutte le funzionalità che richiedono che il computer deve essere aggiunto al dominio? tra cui le credenziali di dominio, criteri di gruppo e reti private virtuali (VPN)? non sono disponibili.  
  
-   Tutti i componenti aggiuntivi di terze parti che richiedono che il computer deve essere aggiunto al dominio non funzionerà correttamente.  
  
-   Questo metodo non può essere utilizzato per connettere un computer remoto al server.  
  
#### <a name="prerequisites"></a>Prerequisiti  
  
-   Il computer deve disporre di una connessione fisica alla rete locale.  
  
-   Nel computer deve essere installato uno dei sistemi operativi seguenti:  
  
    -   Windows 10 Pro, Windows 10 Enterprise  
  
    -    Windows 8.1 Pro, Windows 8.1 Enterprise, Windows 8 Pro, Windows 8 Enterprise  
  
    -    Windows 7 Professional (x86 e x64), Windows 7 Enterprise (x86 e x64), Windows 7 Ultimate (x86 e x64)  
  

-   Il computer deve soddisfare tutti gli altri requisiti per i computer client in Windows Server Essentials. Per ulteriori informazioni, vedere [prerequisiti per la connessione di un computer al server](Get-Connected-in-Windows-Server-Essentials.md#BKMK_2).  

  
-   Per abilitare una connessione senza aggiunta al dominio, è necessario accedere al computer con un account che sia un membro del gruppo Administrators locale.  
  
-   Per connettere il computer al server di Windows Server Essentials, è necessario utilizzare le seguenti informazioni sull'account:  
  
    -   Un account con diritti di amministratore in Windows Server Essentials (account in uso).  
  
    -   Il nome utente e la password per l'account di dominio della persona che userà il computer. L'account di dominio deve inoltre disporre dei diritti di amministratore nel server di Windows Server Essentials.  
  
#### <a name="connect-to-a-windows-server-essentials-network"></a>Connettersi a una rete di Windows Server Essentials  
 Dopo avere verificato che tutti i prerequisiti siano stati soddisfatti, connettere il computer alla rete di Windows Server Essentials.  
  
###### <a name="to-connect-a-computer-in-a-different-domain-to-a-windows-server-essentials-network"></a>Per connettere un computer in un dominio diverso a una rete di Windows Server Essentials  
  
1.  Accedere al computer client con un account che sia un membro del gruppo Administrators locale.  
  
2.  Aprire un prompt dei comandi con diritti di amministratore.  
  
    -   In Windows 10, fare clic su di **Start** pulsante, seleziona **tutte le app** -> **utilità di sistema Windows** -> **prompt dei comandi**, fare doppio clic su prompt dei comandi e quindi fare clic su **Esegui come amministratore**.  
  
    -   In Windows 8 nel **Start** digitare **comando** e quindi premere INVIO. Nei risultati, fare doppio clic su **prompt dei comandi**, quindi fare clic su **Esegui come amministratore**.  
  
    -   In Windows 7 nel **Start** menu, immettere **comando** nella casella di ricerca, fare doppio clic su **prompt dei comandi**e quindi fare clic su **Esegui come amministratore**.  
  
3.  Al prompt dei comandi, digitare il comando seguente e quindi premere INVIO:  
  
    ```  
    reg add "HKLM\SOFTWARE\Microsoft\Windows Server\ClientDeployment" /v SkipDomainJoin /t REG_DWORD /d 1  
    ```  
  

4.  Completare i passaggi in [connettere i computer al server](Get-Connected-in-Windows-Server-Essentials.md#BKMK_9).  

  
####  <a name="BKMK_SecondServer"></a>Aggiungere un secondo server alla rete  
  
###### <a name="to-join-a-second-server-to-the-network"></a>Per aggiungere un secondo server alla rete  
  
1.  Accedere al server che si desidera connettersi alla rete di Windows Server Essentials.  
  
2.  Aprire un browser Internet e nella barra degli indirizzi digitare **http://<servername\>/Connect**, dove *< servername\ >* è il nome del server che esegue Windows Server Essentials e quindi premere INVIO.  
  
3.  Se sicurezza avanzata di Internet Explorer è abilitata sul server che si sta tentando di connettersi alla rete di Windows Server Essentials, completare la procedura seguente. in caso contrario, ignorare questo passaggio.  
  
    1.  Per accettare il messaggio di blocco, fare clic su **Chiudi**.  
  
    2.  Aggiungere il **http://<servername\>/Connect** sito Web per siti Web attendibili, come indicato di seguito:  
  
        1.  Nel riquadro di spostamento del browser, fare clic su **strumenti**, quindi fare clic su **Opzioni Internet**.  
  
        2.  Fare clic su di **sicurezza** scheda e quindi fare clic su **siti attendibili**.  
  
        3.  Fare clic su **siti**.  
  
        4.  Il sito Web dovrebbe essere visualizzato nel **aggiungere il sito Web all'area** campo. Fare clic su **aggiungere**.  
  
        5.  Fare clic su **Chiudi**, quindi fare clic su **OK**.  
  
    3.  Aggiornare la pagina web.  
  

    4.  Per connettere il secondo server a un server che esegue Windows Server Essentials, seguire le istruzioni [connettere i computer al server](Get-Connected-in-Windows-Server-Essentials.md#BKMK_9).  

  
    > [!NOTE]
    >  Connessione di un secondo server a un server che esegue Windows Server Essentials è diversa dalla connessione a un computer client come indicato di seguito:  
    >   
    >  -   Non esiste alcuna migrazione dei profili utente; di conseguenza, il profilo corrente non verrà eseguito.  
    > -   È possibile eseguire il backup del secondo server con backup dei computer client e non è possibile riattivare il computer per il backup.  
  
 Dopo avere aggiunto il secondo server a un server che esegue Windows Server Essentials, server connesso vengono fornite le funzionalità seguenti:  
  
-   Aggiornamento e le funzionalità di stato di avvisi funzionano è uguale a quello degli altri computer client.  
  
-   Il funzionamento delle funzionalità online e Offline uguale a quello degli altri computer client.  
  
-   È possibile connettersi al secondo server tramite accesso Web remoto.  
  
-   Il secondo server verranno incluse nei rapporti di stato perché Windows Server Essentials genererà avvisi correlati a questo server.  
  
 Gestione del secondo server dal server che esegue Windows Server Essentials sarà diverso dalla gestione di altri computer client come indicato di seguito:  
  
-   Non sarà presente alcun punto di ingresso per l'area di notifica, finestra di avvio e il Client Dashboard.  
  
-   Il secondo server è elencato nel **server** gruppo il **dispositivi** scheda.  
  
-   Poiché il backup dei computer client non è supportato per il secondo server, lo stato del backup viene visualizzato come **non supportato**. Inoltre, se si seleziona il secondo server e il pulsante destro del mouse, non esistono alcuna copia di backup e ripristinare le attività correlate visualizzate per il secondo server.  
  
-   Se seleziona il secondo server e quindi fare clic sul **Visualizza proprietà del server** attività, non esiste alcun **Backup** scheda visualizzata nella pagina delle proprietà di server s.  
  
-   Perché non esiste alcun Centro sicurezza PC in un sistema operativo Windows Server, lo stato di protezione server s secondo viene visualizzato come **non applicabile**.  
  
-   Il secondo server s stato criteri di gruppo viene visualizzato come **non applicabile**.  
  
###  <a name="BKMK_11"></a>Installare il software connettore  
 Il software connettore in Windows Server Essentials viene installato quando si connette il computer al server tramite la connessione del Computer per la creazione guidata Server. È possibile avviare questa procedura guidata digitando **http://<ServerName\>/connect** nella barra degli indirizzi del web browser (dove *< ServerName\ >* è il nome del server).  
  
> [!NOTE]
>  Se il computer si trova in una posizione remota, per eseguire la connessione di un Computer per la procedura guidata Server, digitare **http://<domainname\>/connect** nella barra degli indirizzi del web browser (dove *< domain \ >* è il nome di dominio dell'organizzazione). È possibile ottenere le informazioni sul nome di dominio dall'amministratore di rete.  
  
 Il software connettore esegue le operazioni seguenti:  
  
-   Si connette il computer a Windows Server Essentials  
  
-   Esegue automaticamente il backup del computer ogni notte (se si configura il server per creare i backup client)  
  
-   Consente di monitorare l'integrità del computer amministratore  
  
-   Consente di configurare e amministrare in remoto di Windows Server Essentials dal computer di casa  
  

 Per istruzioni dettagliate sulla connessione del computer al server di Windows Server Essentials, vedere [connettere i computer al server](Get-Connected-in-Windows-Server-Essentials.md#BKMK_9).   

  
###  <a name="BKMK_12"></a>Spostare i dati dei computer e le impostazioni manualmente  
  Windows Server Essentials e Windows Server Essentials supportano la migrazione dei profili utente solo per i computer client che eseguono il sistema operativo Windows 7. Quando ci si connette un computer basato su Windows 7 al server, la connessione del Computer per la creazione guidata Server può eseguire automaticamente la migrazione del profilo utente.  
  
 Il profilo utente non potrà essere trasferito automaticamente quando si connette un computer di Windows 10 o Windows 8.1, Windows 8 per il server. Tuttavia, in un computer Windows 8, è possibile utilizzare Trasferimento dati Windows per trasferire dati e impostazioni da parte dell'utente locale originale al computer appartenenti a un dominio. A tale scopo, è necessario essere un amministratore nel computer di origine Windows 8 e il computer di destinazione Windows 8. For information about using Windows Easy Transfer to transfer files and settings, see [article 2735227](https://support.microsoft.com/kb/2735227) in the Microsoft Knowledge Base.  
  
###  <a name="BKMK_Transfer"></a>Trasferire più profili utente durante la distribuzione di computer  
 Prima di connettere un computer che esegue Windows 7 o sistema operativo Windows 7 SP1 al server di Windows Server Essentials, per trasferire più profili utente locali, è innanzitutto necessario creare gli account utente di rete corrispondente nel server. Per ulteriori informazioni sulla creazione di account utente di rete, vedere [aggiungere un account utente](../manage/Manage-User-Accounts-in-Windows-Server-Essentials.md#BKMK_Manage1).  
  
 Migrazione dei profili utente è supportata solo in un computer che esegue Windows 7 (per Windows Server Essentials) o Windows 7 SP1 (per Windows Server Essentials). Quando si connette un computer al server di Windows Server Essentials tramite il Connetti il Computer per la creazione guidata Server, disponibile un'opzione per spostare i dati dell'utente e le impostazioni del vecchio account locale in nuovi account utente di rete. A tale scopo, scegliere il **spostare i dati utente esistenti e le impostazioni** pagina della procedura guidata, eseguire il mapping di account utente di rete per l'account utente locali esistenti nel computer per trasferire più profili utente che si trovano nel computer client.  
  
###  <a name="BKMK_13"></a>Disinstallare il software connettore  
 È possibile disinstallare il software connettore da un computer utilizzando il pannello di controllo. È in genere verrà farlo se esiste un problema con il software connettore o se è necessario installare una versione più recente del software connettore. In qualità di amministratore per completare questa procedura, è necessario essere connessi al computer.  
  
> [!IMPORTANT]
>  Se si aggiorna il sistema operativo in un computer client, il software connettore viene disinstallato automaticamente. È necessario reinstallare il software connettore al termine dell'aggiornamento. Il metodo preferito consiste nel disinstallare il software connettore prima di aggiornare il sistema operativo. Disinstallare il software connettore al termine dell'aggiornamento è comunque accettabile. Tuttavia, potrebbe causare uno stato incoerente per il computer client con il server fino a quando non viene disinstallato e reinstallato il software connettore.  
  
##### <a name="to-uninstall-connector-software-from-a-computer"></a>Per disinstallare il software connettore da un computer  
  
1.  In un computer che esegue Windows 7, Windows 8, Windows 8.1 o Windows 10, aprire **Pannello di controllo**e quindi il **programmi** fare clic su **Visualizza aggiornamenti installati**.  
  
2.  Nell'elenco dei programmi installati, selezionare **connettore Windows Server Essentials**, quindi fare clic su **Disinstalla**.  
  
3.  Nella pagina di avviso, fare clic su **Sì**.  
  
4.  Se il **controllo dell'Account utente** verrà visualizzata la finestra, fare clic su **Consenti**.  
  
5.  In Windows Server Essentials, se la pagina di connettore Windows Server Essentials è suggerita chiudere la finestra di avvio, fare clic su **OK**.  
  
6.  Attendere che il programma disinstallare. Dopo la rimozione di software, **connettore Windows Server Essentials** non viene più visualizzato nell'elenco di programmi o aggiornamenti installati. Inoltre, i collegamenti alla finestra di avvio e il Dashboard non sono più visualizzati sul desktop del computer s.  
  
> [!NOTE]
>  -   La disinstallazione del software connettore non rimuove il computer dall'elenco dei computer in cui vengono visualizzati nel **dispositivi** scheda del Dashboard. Per rimuovere il computer dal Dashboard, vedere [rimuovere un computer dal server](../manage/Manage-Devices-in-Windows-Server-Essentials.md#BKMK_3).  
> -   Quando si disinstalla il software connettore, le cartelle condivise nel computer client che sono state mappate al server non vengono eliminate. È necessario eliminare manualmente le cartelle condivise mappate al server.  

> -   La disinstallazione del software connettore non separa il computer dal dominio originale. È necessario separare manualmente il computer dal dominio. Per istruzioni, vedere [rimuovere un computer da un dominio Windows](Get-Connected-in-Windows-Server-Essentials.md#BKMK_8).  

  
###  <a name="BKMK_14"></a>Disconnettere il computer da o riconnettere il computer al server  
 Per disconnettere un computer dal server, è necessario completare i passaggi seguenti:  
  

1.  Disinstallare il software connettore dal computer utilizzando il pannello di controllo. Per istruzioni dettagliate, vedere [disinstallare il software connettore](Get-Connected-in-Windows-Server-Essentials.md#BKMK_13).   

  
2.  Separare il computer dal dominio di Windows Server Essentials e aggiungerlo al gruppo di lavoro. For step-by-step instructions for joining Windows to a workgroup, [Join or create a workgroup](https://windows.microsoft.com/windows7/Join-or-create-a-workgroup).  
  
3.  Rimuovere il computer dal server tramite il Dashboard. Per istruzioni dettagliate, vedere [rimuovere un computer dal server](../manage/Manage-Devices-in-Windows-Server-Essentials.md#BKMK_3).  
  
 Per riconnettere un computer al server che in precedenza è stato disconnesso dalla rete di server Windows Server Essentials, è necessario completare i passaggi seguenti:  
  

1.  Disinstallare il software connettore dal computer utilizzando il pannello di controllo. Per istruzioni dettagliate, vedere [disinstallare il software connettore](Get-Connected-in-Windows-Server-Essentials.md#BKMK_13).  
  
2.  Separare il computer dal dominio di Windows Server Essentials e aggiungerlo al gruppo di lavoro. For step-by-step instructions for joining Windows to a workgroup, [Join or create a workgroup](https://windows.microsoft.com/windows7/Join-or-create-a-workgroup).  
  
3.  Connettere il computer al server tramite la connessione guidata Computer. Per istruzioni dettagliate, vedere [connettere i computer al server](Get-Connected-in-Windows-Server-Essentials.md#BKMK_9).  
  
###  <a name="BKMK_Sleep"></a>Come eseguire il backup con stato di sospensione e ibernazione  
 Se si seleziona il **riattiva il Computer per il Backup** opzione quando si connette un computer al server, il computer verrà automaticamente riattivarsi dallo stato di sospensione o ibernazione modalità ogni giorno come specificato nella pianificazione del Backup in modo che può eseguire il backup. Al termine dell'operazione di backup, il computer tornerà modalità, in base alle impostazioni di risparmio energia sospensione o ibernazione. Se non si seleziona questa opzione, il server non eseguirà il backup di un computer se il computer è in stato di sospensione o ibernazione. Per ulteriori informazioni, vedere [Gestione Backup Client](../manage/Manage-Client-Computer-Backup-in-Windows-Server-Essentials.md).  
  
##  <a name="BKMK_C"></a>Utilizzare la finestra di avvio  
 È possibile utilizzare la finestra di avvio per accedere alle risorse condivise dal server di Windows Server Essentials, eseguire i backup dei computer e rispondere agli avvisi di integrità di sistema.  
  
-   [Panoramica della finestra di avvio](../manage/Overview-of-the-Launchpad-in-Windows-Server-Essentials.md)  
  
-   [Utilizzare la finestra di avvio in un computer Mac](../manage/Overview-of-the-Launchpad-in-Windows-Server-Essentials.md#BKMK_Mac)  
  
## <a name="see-also"></a>Vedere anche  
  
-   [Risolvere i problemi di connessione dei computer al server](../support/Troubleshoot-connecting-computers-to-the-server-in-Windows-Server-Essentials.md)  
  
-   [Gestire gli account utente](../manage/Manage-User-Accounts-in-Windows-Server-Essentials.md)  
  

-   [Usare le cartelle condivise](Use-Shared-Folders-in-Windows-Server-Essentials.md)  
  
-   [Lavorare in modalità remota](Work-Remotely-in-Windows-Server-Essentials.md)  
  
-   [Riprodurre file multimediali digitali](Play-Digital-Media-in-Windows-Server-Essentials.md)

-   [Usare le cartelle condivise](../use/Use-Shared-Folders-in-Windows-Server-Essentials.md)  
  
-   [Lavorare in modalità remota](../use/Work-Remotely-in-Windows-Server-Essentials.md)  
  
-   [Riprodurre file multimediali digitali](../use/Play-Digital-Media-in-Windows-Server-Essentials.md)

