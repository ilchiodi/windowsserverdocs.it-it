---
title: Aggiungere i server a Server Manager
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
ms.localizationpriority: high
ms.date: 02/01/2018
ms.openlocfilehash: 007279c8a60e5e762d1e59b1519c449484cfc167
ms.sourcegitcommit: 5101bd19a4dce9ed9d3d7836c927e2a5745dcb7e
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 06/06/2018
ms.locfileid: "1985271"
---
# <a name="add-servers-to-server-manager"></a>Aggiungere i server a Server Manager

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

In Windows Server puoi gestire più server remoti con un'unica console di Server Manager. I server da gestire con Server Manager possono eseguire Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2 o Windows Server 2008. Non puoi gestire una versione più recente di Windows Server con una versione precedente di Server Manager.

Questo argomento descrive come aggiungere server al pool di server di Server Manager.

> [!NOTE]
> Secondo i nostri test, Server Manager in Windows Server 2012 e versioni successive di Windows Server può essere usato per gestire fino a 100 server configurati con un carico di lavoro tipico. Il numero di server che può essere gestito con una sola console di Server Manager può variare a seconda della quantità di dati richiesti ai server gestiti e alle risorse hardware e di rete disponibili nel computer che esegue Server Manager. Quando la quantità di dati da visualizzare si avvicina alla capacità delle risorse del computer, possono verificarsi rallentamenti nelle risposte di Server Manager e ritardi nel completamento degli aggiornamenti. Per aumentare il numero di server gestibile con Server Manager, si consiglia di limitare i dati dell'evento che Server Manager ottiene dai server gestiti usando le impostazioni nella finestra di dialogo **Configura dati evento**. La finestra di dialogo Configura dati evento può essere aperta dal menu **Attività** nel riquadro **Eventi**. Per gestire un numero elevato di server nell'organizzazione, è consigliabile valutare i prodotti di [Microsoft System Center](https://go.microsoft.com/fwlink/p/?LinkId=239437).
>
> Server Manager può ricevere solo lo stato online o offline dai server che eseguono Windows Server 2003. Sebbene sia possibile usare Server Manager per eseguire attività di gestione nei server che eseguono Windows Server 2008 R2 o Windows Server 2008, non si possono aggiungere ruoli e funzionalità ai server che eseguono Windows Server 2008 R2, Windows Server 2008 o Windows Server 2003.

> Server Manager non può essere usato per gestire una versione più recente del sistema operativo di Windows Server. Server Manager in esecuzione in Windows Server 2012 R2, Windows Server 2012, Windows 8.1 o Windows 8 non può essere usato per gestire i server che eseguono Windows Server 2016.

Questo argomento comprende le sezioni seguenti.

-   [Aggiungere i server da gestire](#BKMK_add)

-   [Fornire le credenziali con il comando Account di gestione](#BKMK_creds)

## <a name="BKMK_creds"></a>Fornire le credenziali con il comando Account di gestione
Quando si aggiungono server remoti a Server Manager, alcuni di questi server potrebbero richiedere credenziali diverse per l'account utente per accedervi o gestirli. Per specificare le credenziali per un server gestito, diverse da quelle usate per accedere al computer in cui è in esecuzione Server Manager, usa il comando **Account di gestione** dopo aver aggiunto un server a Server Manager, accessibile facendo clic con il pulsante destro del mouse sulla voce del server gestito nel riquadro **Server** della home page di un ruolo o gruppo. Facendo clic su **Account di gestione** viene aperta la finestra di dialogo **Sicurezza di Windows** in cui è possibile fornire un nome utente con diritti di accesso nel server gestito, in uno dei formati seguenti.

-   *Nome utente*

-   *Nome utente*@example.domain.com

-   *Dominio*\\*Nome utente*

La finestra di dialogo **Sicurezza di Windows** aperta dal comando **Account di gestione** non può accettare credenziali per smart card. L'uso delle credenziali per smart card in Server Manager non è supportato. Le credenziali fornite per un server gestito con il comando **Account di gestione** vengono memorizzate nella cache e mantenute a condizione che il server venga gestito con lo stesso computer in cui è attualmente in esecuzione Server Manager oppure a condizione che non vengano sovrascritte specificando credenziali vuote o diverse nello stesso server. Se si esportano le impostazioni di Server Manager in altri computer o si configura il profilo di dominio con il roaming per consentire l'uso delle impostazioni di Server Manager in altri computer, le credenziali di **Account di gestione** per i server nel pool di server non vengono archiviate nel profilo di roaming. Gli utenti di Server Manager devono aggiungerle in ogni computer da cui eseguiranno la gestione.

Dopo avere aggiunto i server da gestire seguendo le procedure descritte in questo argomento, ma prima di usare il comando **Account di gestione** per specificare le credenziali alternative che potrebbero essere necessarie per gestire un server aggiunto, è possibile che vengano visualizzati errori relativi allo stato gestibilità del server:

-   Errore di risoluzione della destinazione Kerberos

-   Errore di autenticazione Kerberos

-   Online - Accesso negato

> [!NOTE]
> Ruoli e funzionalità che non supportano il comando **Account di gestione** includono il server Remote Desktop Services (RDS) e Gestione indirizzi IP (IPAM). Se non puoi gestire il server RDS o IPAM remoto con le stesse credenziali in uso nel computer con Server Manager, prova ad aggiungere l'account che in genere usi per gestire questi server remoti al gruppo Administrators nel computer con Server Manager. Quindi, accedi al computer in cui è in esecuzione Server Manager con l'account usato per gestire il server remoto con RDS o IPAM.

## <a name="BKMK_add"></a>Aggiungere i server da gestire
Puoi aggiungere server da gestire a Server Manager usando uno dei tre metodi nella finestra di dialogo **Aggiungi server**.

-   **Active Directory Domain Services** aggiunge i server da gestire trovati da Active Directory nello stesso dominio del computer locale.

-   **Voce DNS (Domain Name System)** cerca i server da gestire in base al nome del computer o all'indirizzo IP.

-   **Importa più server** specifica più server da importare in un file contenente i server elencati in base al nome del computer o all'indirizzo IP.

#### <a name="to-add-servers-to-the-server-pool"></a>Per aggiungere i server al pool di server

1.  Se Server Manager è già aperto, vai al passaggio successivo. Se Server Manager non è già aperto, aprilo in uno di questi modi.

    -   Nel desktop di Windows avvia Server Manager facendo clic su **Server Manager** nella barra delle applicazioni di Windows.

    -   Nella schermata **start** di Windows, seleziona il riquadro Server Manager.

2.  Nel menu **Gestisci** fai clic su **Aggiungi server**.

3.  Effettua una delle operazioni seguenti.

    -   Nella scheda **active directory** seleziona i server presenti nel dominio corrente. Premi **CTRL** mentre selezioni più server. Fai clic sul pulsante freccia destra per spostare i server selezionati nell'elenco **selezionato**.

    -   Nella scheda **DNS** digita i primi caratteri del nome di un computer o un indirizzo IP, quindi premi **INVIO** o fai clic su **Cerca**. seleziona i server da aggiungere e quindi fai clic sul pulsante freccia destra.

    -   Nella scheda **Importa** individua un file di testo contenente i nomi DNS o gli indirizzi IP dei computer da aggiungere, usando un solo nome o indirizzo IP per riga.

4.  Dopo aver aggiunto i server, fai clic su **OK**.

### <a name="add-and-manage-servers-in-workgroups"></a>Aggiungere e gestire i server nei gruppi di lavoro
Anche se l'aggiunta di server contenuti in gruppi di lavoro a Server Manager può riuscire, dopo l'aggiunta la colonna **Gestibilità** del riquadro **Server** in una pagina di un ruolo o gruppo che include un server del gruppo di lavoro può visualizzare errori di tipo **credenziali non valide** che si verificano quando si prova a connettersi o a raccogliere dati da un server del gruppo di lavoro remoto.

Questi o errori simili possono verificarsi nelle condizioni seguenti.

-   Il server gestito è nello stesso gruppo di lavoro del computer in cui è in esecuzione Server Manager.

-   Il server gestito è in un gruppo di lavoro diverso dal computer in cui è in esecuzione Server Manager.

-   Uno dei computer è in un gruppo di lavoro, mentre l'altro in un dominio.

-   Il computer che esegue Server Manager è in un gruppo di lavoro mentre i server gestiti remoti si trovano in una subnet diversa.

-   Entrambi i computer si trovano in domini, ma non esiste alcuna relazione di trust tra i due domini.

-   Entrambi i computer si trovano in domini, ma c'è solo una relazione di trust unidirezionale tra i due domini.

-   Il server da gestire è stato aggiunto usando il relativo indirizzo IP.

##### <a name="to-add-remote-workgroup-servers-to-server-manager"></a>Per aggiungere server del gruppo di lavoro remoto a Server Manager

1.  Nel computer che esegue Server Manager, aggiungi il nome del server del gruppo di lavoro nell'elenco **TrustedHosts**. Questo è un requisito di autenticazione NTLM. Per aggiungere un nome del computer a un elenco di host attendibili esistente, aggiungi il parametro `Concatenate` al comando. Ad esempio, per aggiungere il computer `Server01` a un elenco esistente di host attendibili, usa il comando seguente.

    ```
    Set-Item wsman:\localhost\Client\TrustedHosts Server01 -Concatenate -force
    ```

2.  Determina se il server del gruppo di lavoro da gestire si trova nella stessa subnet del computer in cui è in esecuzione Server Manager.

    Se i due computer si trovano nella stessa subnet o se il profilo di rete del server del gruppo di lavoro è impostato su **Privato** in **Centro connessioni di rete e condivisione**, vai al passaggio successivo.

    Se non si trovano nella stessa subnet o se il profilo di rete del server del gruppo di lavoro non è impostato su **Privato**, sul server del gruppo di lavoro modifica l'impostazione in ingresso **Gestione remota Windows (HTTP-In)** in Windows Firewall per consentire in modo esplicito le connessioni da computer remoti aggiungendo i nomi dei computer nella scheda **Computer** della finestra di dialogo **Proprietà** dell'impostazione.

3.  > [!IMPORTANT]
    > L'esecuzione del cmdlet in questo passaggio ha priorità sulle misure di Controllo dell'account utente (UAC) che impediscono l'esecuzione di processi con privilegi elevati nei computer del gruppo di lavoro a meno che l'account predefinito Administrator o l'account di sistema non esegua i processi. Il cmdlet consente ai membri del gruppo Administrators di gestire i server del gruppo di lavoro senza effettuare l'accesso con l'account predefinito Administrator. Consentire ad altri utenti di gestire il server del gruppo di lavoro può ridurne la sicurezza. Tuttavia, è una procedura più sicura rispetto a fornire le credenziali dell'account predefinito Administrator a tutte le persone che gestiscono il server del gruppo di lavoro.

    Per ignorare le restrizioni di Controllo dell'account utente relative all'esecuzione di processi con privilegi elevati su computer del gruppo di lavoro, crea una voce del Registro di sistema denominata **LocalAccountTokenFilterPolicy** sul server del gruppo di lavoro eseguendo il cmdlet riportato di seguito.

    ```
    New-ItemProperty -Name LocalAccountTokenFilterPolicy -path HKLM:\SOFTWARE\Microsoft\Windows\Currentversion\Policies\System -propertytype DWord -value 1
    ```

4.  Nel computer in cui è in esecuzione Server Manager, apri la pagina **Tutti i server**.

5.  Se il computer che esegue Server Manager e il server del gruppo di lavoro di destinazione si trovano nello stesso gruppo di lavoro, vai all'ultimo passaggio. Se i due computer non si trovano nello stesso gruppo di lavoro, fai clic con il pulsante destro del mouse sul server del gruppo di lavoro di destinazione nel riquadro **Server** e quindi scegli **Account di gestione**.

6.  Accedi al server del gruppo di lavoro con l'account predefinito Administrator per il server del gruppo di lavoro.

7.  Verifica che Server Manager sia in grado di connettersi e raccogliere dati dal server del gruppo di lavoro aggiornando la pagina **Tutti i server** e visualizzando lo stato di gestibilità del server del gruppo di lavoro.

##### <a name="to-add-remote-servers-when-server-manager-is-running-on-a-workgroup-computer"></a>Per aggiungere server remoti quando Server Manager è in esecuzione in un computer del gruppo di lavoro

1.  Nel computer che esegue Server Manager aggiungi server remoti all'elenco **TrustedHosts** del computer locale in una sessione di Windows PowerShell. Per aggiungere un nome del computer a un elenco di host attendibili esistente, aggiungi il parametro `Concatenate` al comando. Ad esempio, per aggiungere il computer `Server01` a un elenco esistente di host attendibili, usa il comando seguente.

    ```
    Set-Item wsman:\localhost\Client\TrustedHosts Server01 -Concatenate -force
    ```

2.  Determina se il server da gestire si trova nella stessa subnet del computer del gruppo di lavoro in cui è in esecuzione Server Manager.

    Se i due computer si trovano nella stessa subnet o se il profilo di rete del computer del gruppo di lavoro è impostato su **Privato** in **Centro connessioni di rete e condivisione**, vai al passaggio successivo.

    Se non si trovano nella stessa subnet o se il profilo di rete del computer del gruppo di lavoro non è impostato su **Privato**, sul computer del gruppo di lavoro che esegue Server Manager modifica l'impostazione in ingresso **Gestione remota Windows (HTTP-In)** in Windows Firewall per consentire in modo esplicito le connessioni da computer remoti aggiungendo i nomi dei computer nella scheda **Computer** della finestra di dialogo **Proprietà** dell'impostazione.

3.  Nel computer in cui è in esecuzione Server Manager, apri la pagina **Tutti i server**.

4.  Verifica che Server Manager sia in grado di connettersi e raccogliere dati dai server remoti aggiornando la pagina **Tutti i server** e visualizzando lo stato di gestione per il server remoto. Se nel riquadro **Server** è ancora visualizzato un errore di gestibilità per il server remoto, vai al passaggio successivo.

5.  Disconnettiti dal computer in cui è in esecuzione Server Manager e quindi ripeti l'accesso con l'account predefinito Administrator. Ripeti il passaggio precedente per verificare che Server Manager sia in grado di connettersi e raccogliere dati dal server remoto.

se hai seguito le procedure descritte in questa sezione, ma i problemi di gestione dei computer del gruppo di lavoro o di gestione dei computer dai computer del gruppo di lavoro persistono, vedi [about_remote_Troubleshooting](https://technet.microsoft.com/library/dd347642.aspx) nel sito Web Microsoft.

### <a name="add-and-manage-servers-in-clusters"></a>Aggiungere e gestire i server in cluster
Puoi usare Server Manager per gestire i server presenti nel cluster di failover (denominati anche cluster di server o MSCS). I server inclusi nei cluster di failover (se i nodi del cluster sono fisici o virtuali) hanno comportamenti particolari e limitazioni di gestione in Server Manager.

-   I server fisici e virtuali nei cluster vengono aggiunti automaticamente a Server Manager quando un server del cluster viene aggiunto a Server Manager. Analogamente, quando rimuovi un server di cluster da Server Manager, viene chiesto di rimuovere anche gli altri server del cluster.

-   Server Manager non visualizza i dati per i server di cluster virtuali perché i dati sono dinamici e identici ai dati per il server in cui è ospitato il nodo del cluster virtuale. È possibile selezionare il server che ospita il server virtuale per visualizzarne i dati.

-   Se aggiungi un server a Server Manager usando il nome dell'oggetto cluster virtuale del server, il nome dell'oggetto virtuale viene visualizzato in Server Manager al posto del nome del server fisico (previsto).

-   Non puoi installare ruoli e funzionalità in un server di cluster virtuale.

## <a name="see-also"></a>Vedi anche
[Server Manager](server-manager.md)
[crea e gestisci gruppi di server](create-and-manage-server-groups.md)
