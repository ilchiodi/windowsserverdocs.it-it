---
title: Add Servers to Server Manager
description: Server Manager
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-server-manager
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: aab895f2-fe4d-4408-b66b-cdeadbd8969e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.localizationpriority: medium
ms.date: 02/01/2018
ms.openlocfilehash: a47ecbc0c7359438ed60ed34c94adf0096b14967
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/31/2019
ms.locfileid: "66435457"
---
# <a name="add-servers-to-server-manager"></a>Add Servers to Server Manager

>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

In Windows Server è possibile gestire più server remoti tramite un'unica console di Server Manager. I server da gestire con Server Manager possono eseguire Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2 o Windows Server 2008. Si noti che non è possibile gestire una versione più recente di Windows Server con una versione precedente di Server Manager.

In questo argomento viene descritto come aggiungere server al pool di server di gestione Server.

> [!NOTE]
> Secondo i nostri test, Server Manager in Windows Server 2012 e versioni successive di Windows Server può essere usato per gestire fino a 100 server configurati con un carico di lavoro tipico. Il numero di server che è possibile gestire tramite un'unica console di Server Manager può variare a seconda della quantità di dati richiesti dai server gestiti e le risorse hardware e di rete disponibili nel computer che esegue Server Manager. Quando la quantità di dati da visualizzare si avvicina alla capacità delle risorse del computer, possono verificarsi rallentamenti nelle risposte di Server Manager e ritardi nel completamento degli aggiornamenti. Per aumentare il numero di server gestibile con Server Manager, si consiglia di limitare i dati dell'evento che Server Manager ottiene dai server gestiti usando le impostazioni nella finestra di dialogo **Configura dati evento**. La finestra di dialogo Configura dati evento può essere aperta dal menu **Attività** nel riquadro **Eventi** . Se è necessario gestire un numero di livello aziendale di server nell'organizzazione, è consigliabile valutare i prodotti di [suite di Microsoft System Center](https://go.microsoft.com/fwlink/p/?LinkId=239437).
> 
> Server Manager può ricevere lo stato solo online o offline dai server che eseguono Windows Server 2003. Sebbene sia possibile utilizzare Server Manager per eseguire attività di gestione su server che eseguono Windows Server 2008 R2 o Windows Server 2008, è possibile aggiungere ruoli e funzionalità per i server che eseguono Windows Server 2008 R2, Windows Server 2008 o Windows Server 2003.
> 
> Server Manager può essere utilizzata per gestire una versione più recente del sistema operativo Windows Server. Server Manager in esecuzione in Windows Server 2012 R2, Windows Server 2012, Windows 8.1 o Windows 8 non può essere usato per gestire i server che eseguono Windows Server 2016.

In questo argomento sono incluse le sezioni seguenti.

-   [Aggiungi server da gestire](#BKMK_add)

-   [Fornire le credenziali con il comando account di gestione](#BKMK_creds)

## <a name="BKMK_creds"></a>Fornire le credenziali con il comando account di gestione
Quando si aggiungono server remoti a Server Manager, alcuni dei server che aggiungono potrebbero richiedere le credenziali dell'account utente diverso per accedere o gestirli. Per specificare le credenziali per un server gestito che sono diverse da quelli utilizzati per accedere al computer in cui è in esecuzione Server Manager, utilizzare il **Gestione** comando dopo aver aggiunto un server a Server Manager, che è accessibile facendo clic con il movimento di un server gestito nel **server** sezioni della home page di un ruolo o gruppo. Facendo clic su **Account di gestione** viene aperta la finestra di dialogo **Sicurezza di Windows** in cui è possibile fornire un nome utente con diritti di accesso nel server gestito, in uno dei formati seguenti.

-   *Nome utente*

-   *Nome utente*@example.domain.com

-   *Dominio*\\*Nome utente*

Il **la protezione di Windows** la finestra di dialogo che viene aperta la **Gestione** comando non può accettare credenziali della smart card, fornendo le credenziali della smart card tramite Server Manager non è supportato. Le credenziali fornite per un server gestito utilizzando il **Gestione** comando vengono memorizzati nella cache e mantenute fino a quando si utilizza lo stesso computer in cui è attualmente in esecuzione Server Manager gestisce il server o, purché non vengano sovrascritti specificando credenziali vuote o diverso nello stesso server. Se si esporta le impostazioni di Server Manager ad altri computer, o configurare il profilo di dominio in comune per consentire le impostazioni di gestione di Server da utilizzare in altri computer, **Gestione** le credenziali per i pool di server non vengono archiviati nel profilo comune. Gli utenti di Server Manager è necessario aggiungerli in ogni computer da cui si desidera gestire.

Dopo avere aggiunto i server da gestire seguendo le procedure descritte in questo argomento, ma prima di usare il comando **Account di gestione** per specificare le credenziali alternative che potrebbero essere necessarie per gestire un server aggiunto, è possibile che vengano visualizzati errori relativi allo stato gestibilità del server:

-   Errore di risoluzione della destinazione Kerberos

-   Errore dell'autenticazione Kerberos

-   Online - Accesso negato

> [!NOTE]
> Ruoli e funzionalità che non supportano il comando **Account di gestione** includono il server Remote Desktop Services (RDS) e Gestione indirizzi IP (IPAM). Se è possibile gestire il server di servizi desktop REMOTO o gestione indirizzi IP remoto usando le stesse credenziali in uso nel computer in cui è in esecuzione Server Manager, provare ad aggiungere l'account in genere utilizzato per gestire tali server remoto al gruppo Administrators nel computer che esegue Server Manager. Quindi, accedi al computer in cui è in esecuzione Server Manager con l'account usato per gestire il server remoto con RDS o IPAM.

## <a name="BKMK_add"></a>Aggiungi server da gestire
Puoi aggiungere server da gestire a Server Manager usando uno dei tre metodi nella finestra di dialogo **Aggiungi server**.

-   **Active Directory Domain Services** aggiunge i server da gestire trovati da Active Directory nello stesso dominio del computer locale.

-   **Voce DNS (Domain Name System)** Cercare i server da gestire in base a nome computer o indirizzo IP.

-   **Importa più server** specifica più server da importare in un file contenente i server elencati in base al nome del computer o all'indirizzo IP.

#### <a name="to-add-servers-to-the-server-pool"></a>Per aggiungere server al pool di server

1.  Se Server Manager è già aperto, andare al passaggio successivo. Se Server Manager non è aperto, aprirlo in uno dei modi seguenti.

    -   Nel desktop di Windows avviare Server Manager facendo clic su **Server Manager** nella barra delle applicazioni di Windows.

    -   Nella schermata **start** di Windows, seleziona il riquadro Server Manager.

2.  Nel menu **Gestisci** fai clic su **Aggiungi server**.

3.  Effettuare una delle operazioni seguenti.

    -   Nella scheda **active directory** seleziona i server presenti nel dominio corrente. Premere **CTRL** mentre si selezionano più server Fai clic sul pulsante freccia destra per spostare i server selezionati nell'elenco **selezionato**.

    -   Nella scheda **DNS** digitare i primi caratteri di un nome computer o di un indirizzo IP e quindi premere **INVIO** o fare clic su **Cerca**. seleziona i server da aggiungere e quindi fai clic sul pulsante freccia destra.

    -   Nella scheda **Importa** individua un file di testo contenente i nomi DNS o gli indirizzi IP dei computer da aggiungere, usando un solo nome o indirizzo IP per riga.

4.  Dopo aver aggiunto i server, fare clic su **OK**.

### <a name="add-and-manage-servers-in-workgroups"></a>Aggiungere e gestire server in gruppi di lavoro
Aggiungendo i server appartenenti a gruppi di lavoro a Server Manager potrebbero essere eseguita correttamente, dopo l'aggiunta, la **gestibilità** colonna del **server** sezioni in una pagina ruolo o gruppo che include un server del gruppo di lavoro è possono visualizzare **credenziali non valide** errori che si verificano durante il tentativo di connettersi o raccogliere i dati dal server remoto, gruppo di lavoro.

Nelle condizioni seguenti possono verificarsi questi errori o simili.

-   Il server gestito è stesso gruppo di lavoro del computer che esegue Server Manager.

-   Il server gestito è un gruppo di lavoro diverso dal computer che esegue Server Manager.

-   Uno dei computer è un gruppo di lavoro, mentre l'altro in un dominio.

-   Il computer che esegue Server Manager è un gruppo di lavoro e server remoto, gestiti si trovano in una subnet diversa.

-   Entrambi i computer si trovano in domini, ma non esiste alcuna relazione di trust tra i due domini.

-   Entrambi i computer si trovano in domini, ma esiste una relazione di trust unidirezionale tra i due domini.

-   Il server che si vuole gestire è stato aggiunto usando il relativo indirizzo IP.

##### <a name="to-add-remote-workgroup-servers-to-server-manager"></a>Per aggiungere server di gruppi di lavoro a Server Manager

1.  Sul computer che esegue Server Manager, aggiungere il nome del server del gruppo di lavoro per il **TrustedHosts** elenco. Si tratta di un requisito dell'autenticazione NTLM. Per aggiungere un nome computer a un elenco di host attendibili esistente, aggiungere il parametro `Concatenate` al comando. Ad esempio, per aggiungere il computer `Server01` a un elenco esistente di host attendibili, utilizzare il comando seguente.

    ```
    Set-Item wsman:\localhost\Client\TrustedHosts Server01 -Concatenate -force
    ```

2.  Determinare se il server del gruppo di lavoro che si desidera gestire è nella stessa subnet del computer in cui è in esecuzione Server Manager.

    Se i due computer si trovano nella stessa subnet o se il profilo di rete del server del gruppo di lavoro è impostato su **privata** nel **Centro rete e condivisione**, andare al passaggio successivo.

    Se non si trovano nella stessa subnet o se il profilo di rete del server del gruppo di lavoro non è impostato su **Privato**, sul server del gruppo di lavoro modifica l'impostazione in ingresso **Gestione remota Windows (HTTP-In)** in Windows Firewall per consentire in modo esplicito le connessioni da computer remoti aggiungendo i nomi dei computer nella scheda **Computer** della finestra di dialogo **Proprietà** dell'impostazione.

3.  > [!IMPORTANT]
    > L'esecuzione del cmdlet in questo passaggio sostituisce le misure di Controllo dell'account utente che impediscono l'esecuzione di processi con privilegi elevati sui computer del gruppo di lavoro, a meno che i processi vengano eseguiti con l'account Administrator predefinito o con l'account di sistema. Il cmdlet consente ai membri del gruppo Administrators di gestire il server del gruppo di lavoro senza accedere con l'account Administrator predefinito. La possibilità di consentire ad altri utenti di gestire il server del gruppo di lavoro può ridurne la sicurezza, tuttavia questo approccio è più sicuro che fornire le credenziali dell'account Administrator predefinito a più persone che potrebbero dover gestire il server del gruppo di lavoro.

    Per ignorare le restrizioni di Controllo dell'account utente relative all'esecuzione di processi con privilegi elevati su computer del gruppo di lavoro, creare una voce del Registro di sistema denominata **LocalAccountTokenFilterPolicy** sul server del gruppo di lavoro eseguendo il cmdlet riportato di seguito.

    ```
    New-ItemProperty -Name LocalAccountTokenFilterPolicy -path HKLM:\SOFTWARE\Microsoft\Windows\Currentversion\Policies\System -propertytype DWord -value 1
    ```

4.  Nel computer in cui è in esecuzione Server Manager, aprire il **tutti i server** pagina.

5.  Se il computer che esegue Server Manager e il server del gruppo di lavoro di destinazione sono nello stesso gruppo di lavoro, ignorare l'ultimo passaggio. Se i due computer non si trovano nello stesso gruppo di lavoro, fare clic con il pulsante destro del mouse sul server del gruppo di lavoro di destinazione nel riquadro **Server** e quindi scegliere **Account di gestione**.

6.  Accedere al computer del gruppo di lavoro con l'account utente Administrator predefinito per il server del gruppo di lavoro.

7.  Verificare che Server Manager è in grado di connettersi a e raccogliere dati dal server del gruppo di lavoro aggiornando il **tutti i server** pagina e quindi visualizzando lo stato di gestibilità per il server del gruppo di lavoro.

##### <a name="to-add-remote-servers-when-server-manager-is-running-on-a-workgroup-computer"></a>Per aggiungere server remoti quando Server Manager viene eseguito su un computer del gruppo di lavoro

1.  Nel computer che esegue Server Manager, aggiungere server remoti nel computer locale **TrustedHosts** elenco in una sessione di Windows PowerShell. Per aggiungere un nome computer a un elenco di host attendibili esistente, aggiungere il parametro `Concatenate` al comando. Ad esempio, per aggiungere il computer `Server01` a un elenco esistente di host attendibili, utilizzare il comando seguente.

    ```
    Set-Item wsman:\localhost\Client\TrustedHosts Server01 -Concatenate -force
    ```

2.  Determinare se il server che si desidera gestire è nella stessa subnet del computer del gruppo di lavoro in cui è in esecuzione Server Manager.

    Se i due computer si trovano nella stessa subnet o se il profilo di rete del computer del gruppo di lavoro è impostato su **privata** nel **Centro rete e condivisione**, andare al passaggio successivo.

    Se non si trovano nella stessa subnet o se il profilo di rete del computer del gruppo di lavoro non è impostato su **Privato**, sul computer del gruppo di lavoro che esegue Server Manager modifica l'impostazione in ingresso **Gestione remota Windows (HTTP-In)** in Windows Firewall per consentire in modo esplicito le connessioni da computer remoti aggiungendo i nomi dei computer nella scheda **Computer** della finestra di dialogo **Proprietà** dell'impostazione.

3.  Nel computer in cui è in esecuzione Server Manager, aprire il **tutti i server** pagina.

4.  Verifica che Server Manager sia in grado di connettersi e raccogliere dati dai server remoti aggiornando la pagina **Tutti i server** e visualizzando lo stato di gestione per il server remoto. Se nel riquadro **Server** è ancora visualizzato un errore di gestibilità per il server remoto, andare al passaggio successivo.

5.  Disconnettersi dal computer in cui è in esecuzione Server Manager e quindi accedere di nuovo utilizzando l'account Administrator predefinito. Ripetere il passaggio precedente, per verificare che Server Manager è in grado di connettersi a e raccogliere dati dal server remoto.

se hai seguito le procedure descritte in questa sezione, ma i problemi di gestione dei computer del gruppo di lavoro o di gestione dei computer dai computer del gruppo di lavoro persistono, vedi [about_remote_Troubleshooting](https://technet.microsoft.com/library/dd347642.aspx) nel sito Web Microsoft.

### <a name="add-and-manage-servers-in-clusters"></a>Aggiungere e gestire server in cluster
È possibile utilizzare Server Manager per gestire i server in cluster di failover (denominato anche cluster di server o su MSCS). Server in cluster di failover (se i nodi del cluster sono fisico o virtuale) hanno alcuni comportamenti univoci e limitazioni alla gestione in Server Manager.

-   I server fisici e virtuali in cluster vengono aggiunti automaticamente a Server Manager quando un server del cluster viene aggiunto a Server Manager. Analogamente, quando si rimuove un server del cluster di Server Manager, richiesto per rimuovere gli altri server del cluster.

-   Server Manager non vengono visualizzati dati per i server cluster virtuale, in quanto i dati sono dinamici ed sono identici per i dati per il server in cui è ospitato sul nodo cluster virtuale. È possibile selezionare il server che ospita il server virtuale per visualizzarne i dati.

-   Se si aggiunge un server a Server Manager utilizzando il nome del server cluster virtuale oggetto, il nome dell'oggetto virtuale viene visualizzato in Server Manager anziché il nome del server fisico (previsto).

-   Non è possibile installare ruoli e funzionalità in un server virtuale di cluster.

## <a name="see-also"></a>Vedere anche
[Server Manager](server-manager.md)
[crea e gestisci gruppi di server](create-and-manage-server-groups.md)
