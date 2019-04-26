---
title: Software Inventory Logging Aggregator
description: Viene descritto come installare e gestire Software Inventory Logging Aggregator
ms.custom: na
ms.prod: windows-server-threshold
ms.technology: manage-software-inventory-logging
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: e4230a75-6bcd-47d9-ba92-a052a90a6abc
author: brentfor
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 4414f3faca020ffa7169fb006bc1cea2988ea434
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59835462"
---
# <a name="software-inventory-logging-aggregator"></a>Software Inventory Logging Aggregator

>Si applica a: Windows Server 2012 R2

## <a name="what-is-software-inventory-logging-aggregator"></a>Che cos'è Software Inventory Logging Aggregator?
SILA (Software Inventory Logging Aggregator) riceve, aggrega e produce report di base relativi al numero e ai tipi di software aziendale Microsoft installato nei server Windows in un data center.

SILA è un software che si installa in Windows Server ma non è incluso nell’installazione di Windows Server. Per installare il software, scaricarlo gratuitamente dall'area download Windows: [Software Inventory Logging Aggregator 1.0 per Windows Server](https://www.microsoft.com/en-us/download/details.aspx?id=49046)

Il framework Registrazione inventario software è progettato per ridurre i costi operativi delle operazioni di inventario del software Microsoft distribuito in numerosi server in un ambiente IT. Questo framework è costituito da due componenti, questo SIL Aggregator e la funzionalità di Windows Server, introdotta in Windows Server 2012 R2, registrazione inventario Software (SIL). È possibile installare Software Inventory Logging Aggregator 1.0 in un server e ricevere i dati di inventario da qualsiasi server Windows configurato per l’inoltro dei dati tramite Registrazione inventario software. La progettazione consente agli amministratori dei data center di abilitare la funzionalità Registrazione inventario software di Windows Server nelle immagini master di Windows Server destinate alla distribuzione su larga scala all’interno del loro ambiente.  Questo pacchetto software è il punto di destinazione ed è progettato per essere installato presso i clienti per facilitare la registrazione dei dati di inventario nel tempo. Il software consente inoltre di creare periodicamente report di inventario di base in Microsoft Excel. I report di Software Inventory Logging Aggregator 1.0 includono il conteggio delle installazioni di Windows Server, System Center e SQL Server.

> [!IMPORTANT]
> L’uso del software non comporta l’invio di dati a Microsoft.

### <a name="data-sil-collects-over-time"></a>Dati raccolti da Registrazione inventario software nel tempo
Dopo essere stato distribuito correttamente, Software Inventory Logging Aggregator consente di visualizzare i seguenti dati:

-   Installazioni univoche di Windows Server nel data center

-   FQDN

-   GUID di identificazione

-   Numero di processori fisici e core

-   Numero di processori virtuali (per le macchine virtuali)

-   Modello e tipo di processori fisici

-   Abilitazione della tecnologia hyper-threading nei processori fisici

-   Numero di serie dello chassis

-   Conteggio dei limiti massimi e identità delle macchine virtuali Windows Server in esecuzione simultanea (in host che eseguono un hypervisor) in ogni host, nel tempo

-   Numero massimi e nome host, in esecuzione simultanea gestite \(agente di System Center presente\) macchine virtuali di Windows Server in ogni host, nel corso del tempo

-   Nome degli agenti di System Center installati nelle macchine virtuali conteggiate nel elevata gestita\-water mark

-   Conteggio e la posizione delle installazioni di SQL Server nel corso del tempo \(solo SKU ed edizioni che richiedono una licenza\)

-   Elenchi del software installato in Aggiungi\/Installazione applicazioni

### <a name="who-will-use-sil"></a>Chi usa Registrazione inventario software?

-   **Professionisti IT o amministratori di data center** che ricercano un metodo a basso costo per raccogliere utili dati di inventario software, in modo automatico, nel corso del tempo.

-   **CIO e controller finanziari** che necessitano di creare report relativi all’uso del software aziendale Microsoft nelle distribuzioni IT delle proprie organizzazioni.

## <a name="getting-started"></a>Attività iniziali
**Prerequisiti**

Software Inventory Logging Aggregator (SIL Aggregator) in almeno un server per l’aggregazione e i report, in una macchina virtuale o in un hardware fisico:

-   **Windows Server 2012 R2** (Standard o Datacenter Edition)

-   **Il ruolo del server IIS,** con .Net Framework 4.5, Servizi WCF e Attivazione HTTP nella stessa struttura dell' **Aggiunta guidata ruoli e funzionalità**.

-   **SQL Server** 2012 Sp2 Standard Edition o SQL Server 2014 Standard Edition

-   **Microsoft Excel** 2013 a 64 bit (facoltativo per l’installazione ma necessario per la creazione dei report)

-   Facoltativo: **VMware PowerCLI 5.5.0.5836** (necessario in ambienti VMware)

>[!Note]
>Quando si usa il Framework di gestione di Windows, si verifica un problema noto di compatibilità con la versione WMF 5.1, in SIL Aggregator solo.  Non è necessario superare WMF 4.0 nei server con SIL Aggregator installata versione.

Registrazione inventario software è disponibile nelle versioni di Windows Server con i seguenti aggiornamenti installati:

-   **Windows Server 2016**, o versione successiva

-   **Windows Server 2012 R2** (Standard o Datacenter Edition)

    -   Aggiornamento di Windows Server 2012 R2 **KB3000850** (novembre 2014)

    -   Aggiornamento di Windows Server 2012 R2 **KB3060681** (giugno 2015)(potrebbe essere visualizzato come aggiornamento facoltativo in Windows Update)

### <a name="security-and-account-types"></a>Sicurezza e tipi di account
**Requisiti del certificato**

Registrazione inventario software e SIL Aggregator si basano sui certificati SSL per la comunicazione autenticata. L’implementazione prevede in genere l’installazione di SIL Aggregator con un unico certificato (con nome del server e nome del certificato corrispondenti) per l’hosting del servizio Web che riceve i dati di inventario. In seguito, i server Windows di cui deve essere eseguito l’inventario tramite la funzionalità Registrazione inventario software usano un certificato client diverso per l’invio dei dati a SIL Aggregator. È necessario usare un cmdlet di PowerShell (Set-SilAggregator, descritto di seguito) per aggiungere identificazioni personali di certificato all’elenco dei certificati approvati di SIL Aggregator dai quali vengono accettati i dati. SIL Aggregator procede all’elaborazione e all’inserimento nel database dopo l’autenticazione di ogni payload di dati con un certificato. Per altre informazioni, vedere la sezione **Informazioni sui cmdlet di SIL Aggregator** .

### <a name="polling-account-setup"></a>Configurazione dell'account di polling
Quando si aggiungono le credenziali a SIL Aggregator per abilitare le operazioni di polling, è consigliabile usare un account con i privilegi minimi necessari. Inoltre, per garantire una protezione ottimale, è consigliabile non usare le stesse credenziali per tutti o per più host del data center o della distribuzione IT.

Nell’host Windows Server che si desidera configurare per il polling tramite SIL Aggregator e per evitare di usare un utente del gruppo di amministratori, eseguire i passaggi seguenti per assegnare il livello di accesso sufficiente a un account utente:

##### <a name="to-setup-a-polling-account"></a>Per configurare un account di polling:

1.  Nell’host Windows Server Hyper-V di cui si desidera eseguire il polling da SIL Aggregator, creare un account utente locale usando **Gestione computer** in Windows (assicurarsi di deselezionare la casella che forza una modifica della password al primo accesso).

2.  Aggiungere l’utente al gruppo **Utenti gestione remota**.

3.  Aggiungere l’utente al gruppo **Amministratori Hyper-V**.

4.  Aprire **WMIMgmt.msc** con **Start**->**Esegui**.

5.  Fare clic su **Altre azioni** nella sezione **Azioni** e selezionare **Proprietà**.

6.  Fare clic su **Sicurezza**.

7.  Selezionare lo **spazio dei nomi cimv2** nel controllo TreeView **Spazio dei nomi**.

8.  Fare clic sul pulsante **Sicurezza**.

9. Aggiungere il gruppo **Utenti gestione remota** nel formato **nomecomputer\nome gruppo**

10. Fare clic su **OK**.

11. Nella finestra Sicurezza di **root\cimv2**, selezionare **Utenti gestione remota**.

12. Nella sezione delle autorizzazioni nella parte inferiore, assicurarsi che il **Abilita remoto** sia selezionata.

13. Fare clic su **Applica** e quindi su **OK**.

14. Fare clic su **OK** nella finestra **Proprietà** .

### <a name="installing-sil-aggregator"></a>Installazione di SIL Aggregator
Prima di procedere all’installazione di SIL Aggregator in un server Windows è necessario verificare quanto segue:

-   **Disponibilità di un certificato SSL valido** da usare per l’hosting del servizio Web del software.

    -   Il certificato deve essere nel formato **.pfx**

    -   Il nome del server Windows e del certificato devono corrispondere.

-   **Installazione di SQL Server Standard Edition** o installazione nel server remoto che si prevede di usare con il software.

    -   SIL Aggregator può essere usato con SQL Server 2012 sp2 e SQL Server 2014. Non è stata effettuata alcuna selezione particolare durante l’installazione di SQL Server.

    -   L’account usato per l’installazione di SIL Aggregator deve avere il ruolo sysadmin in SQL per poter creare il database durante l’installazione.

    -   L’account usato per installare SIL Aggregator deve essere aggiunto come amministratore in SQL Analysis Services prima di installare SIL Aggregator.

    -   Una volta installato, SQL Server Agent deve essere configurato per l’esecuzione automatica.

-   **Il ruolo del server IIS è aggiunto** con .Net Framework 4.5, Servizi WCF e Attivazione HTTP nella stessa struttura dell’**Aggiunta guidata ruoli e funzionalità**.

-   **Accesso al server con un account con privilegi amministrativi** nel server.

-   **Accesso al server con un account con privilegi sysadmin in SQL Server**, se è richiesta l’autenticazione di Windows

    O

    Se si desidera usare l’autenticazione SQL, **disponibilità della password per un account con privilegi amministrativi per SQL**.

##### <a name="to-install-software-inventory-logging-aggregator"></a>Per installare Software Inventory Logging Aggregator

1.  Fare doppio clic su **Setup.exe** per avviare l’installazione.

2.  Fare clic su **Avanti** nella finestra iniziale.

3.  Se si accetta il contratto di licenza, selezionare la casella per l’accettazione del contratto e fare clic su **Avanti**.

4.  In **Choose Features**, selezionare **Install Software Inventory Logging Aggregator e Reporting Module**, quindi fare clic su **Next**.

    Per altre informazioni sull’installazione del solo modulo dei report, vedere `Publish-SilReport` nella sezione **Informazioni sui cmdlet di SIL Aggregator**.

5.  Dopo aver verificato tutti i prerequisiti, fare clic su **Next**.

6.  In **Choose an Account Type**, selezionare l’ **utente locale** o **gMSA**, a seconda delle proprie preferenze.

    Se si sceglie l’utente locale, viene creato un utente locale con una password complessa generata automaticamente. Questo account viene usato per tutti i servizi di SIL Aggregator e le operazioni su attività nel server locale.  Si consiglia di scegliere gMSA (Group Managed Service Accounts) se SIL Aggregator appartiene a un dominio Active Directory (Windows Server 2012 e versioni successive). Per altre informazioni su gMSA, vedere: [Panoramica degli account del servizio gestito del gruppo](https://technet.microsoft.com/library/hh831782.aspx)

    -   Se si prevede di eseguire il database SQL Server in un server separato da SIL Aggregator è necessario scegliere l’opzione di account gMSA.

    -   Non dimenticare di riavviare il server dopo aver aggiunto l’account del computer al gruppo di protezione abilitato per gMSA in Active Directory.

7.  In **Choose a SQL Server**immettere il server SQL in cui è installata l’istanza SQL oppure **localhost**se l’istanza è installata nel server locale.

    È supportato un solo SIL Aggregator per ogni istanza di SQL.

8.  Selezionare il tipo di autenticazione e fare clic su **Verify SQL**.

9. Fare clic su **Next** e quindi in **Internet Information Services Server Details**, selezionare un numero di porta o confermare il valore predefinito.

10. Individuare il percorso del file con estensione **pfx**, digitare la password per il file PFX e fare clic su **Next**.

11. Nella schermata finale viene visualizzato lo stato dell’installazione. Al termine dell’installazione, fare clic su **Finish**.

### <a name="uninstalling-sil-aggregator"></a>Disinstallazione di SIL Aggregator

##### <a name="to-uninstall-software-inventory-logging-aggregator"></a>Per disinstallare Software Inventory Logging Aggregator

1.  Aprire **PowerShell** come amministratore e digitare `Stop-SilAggregator`. Il ritorno al prompt indica che SIL Aggregator è stato arrestato.

    Per impostazione predefinita, SIL Aggregator procede all’elaborazione dei file dopo 20 minuti o dopo la ricezione di 100 file.  In ambienti su larga scala questa situazione non si verifica mai. In alcuni casi, tuttavia è possibile che alcuni file rimangano da elaborare prima che SIL Aggregator possa essere arrestato. Usare il parametro `–Force` quando non è necessario mantenere i file e i dati.

2.  Passare al **Pannello di controllo**, fare clic su **Programmi e funzionalità**, quindi su **Disinstallare programmi**, **Software Inventory Logging Aggregator**e infine su **Disinstalla**.

    Software Inventory Logging Aggregator apre una finestra in cui viene richiesto di scegliere se eliminare tutti i dati nel database o mantenerli tutti. La selezione predefinita prevede di mantenerli. In caso di reinstallazione, è possibile collegarsi al database esistente per riprendere dal momento della disinstallazione di SIL Aggregator.

3.  Selezionare **Keep** o **Delete**e quindi fare clic su **Next**.

4.  Al termine dell’avanzamento sulla barra, fare clic su **Finish**.

### <a name="start-using-sil-and-the-sil-aggregator"></a>Iniziare a usare Registrazione inventario software e SIL Aggregator

#### <a name="introduction-to-sil-aggregator-powershell-cmdlets"></a>Introduzione ai cmdlet di PowerShell di SIL Aggregator
I comandi seguenti possono essere eseguiti dalla console di Windows PowerShell come amministratore.

|Cmdlet di Windows PowerShell|Funzione|
|-----------------------------|------------|
|`Start-SilAggregator`|Avvia tutti i servizi e le attività di Software Inventory Logging Aggregator. Questa operazione è necessaria per consentire a SIL Aggregator di ricevere dati tramite HTTPS dai server in cui è stata avviata la funzionalità Registrazione inventario software.|
|`Stop-SilAggregator`|Interrompe l’esecuzione di tutti i servizi e le attività di Software Inventory Logging Aggregator. Per le attività e i servizi in esecuzione potrebbe verificarsi un ritardo nell’esecuzione di questo comando.|
|`Set-SilAggregator`|Consente all’amministratore di apportare modifiche alla configurazione di Software Inventory Logging Aggregator.|
|`Add-SilVmHost`|Consente di aggiungere nomi host specifici o una matrice di nomi host di cui eseguire il polling a intervalli regolari \(intervalli di un'ora il valore predefinito è\).|
|`Remove-SilVmHost`|Consente di rimuovere nomi host specifici o una matrice di nomi host di cui eseguire il polling a intervalli regolari.|
|`Get-SilVMHost`|Consente di recuperare l’elenco degli host fisici il cui polling è configurato in Software Inventory Logging Aggregator per i dati relativi allo stato di esecuzione corrente delle macchine virtuali.|
|`Get-SILAggregatorData`|Consente di recuperare i dati del database nella console PowerShell.|
|`Publish-SilReport`|Usare per creare report dal database dei dati di Registrazione inventario software. **Nota:** L’elaborazione dei cubi in SIL Aggregator viene eseguita una volta al giorno. Per questa ragione i dati acquisiti in SIL Aggregator non vengono visualizzati nei report fino al giorno successivo.|

#### <a name="suggested-order-to-start"></a>Ordine consigliato per iniziare
Dopo aver installato Software Inventory Logging Aggregator nel server, aprire PowerShell come amministratore.

-   In SIL Aggregator:

    -   Eseguire `Start-SilAggregator`

        Questa operazione è necessaria per consentire a SIL Aggregator di ricevere i dati inoltrati tramite HTTPS dai server che sono o che verranno configurati per l’inventario. Si noti che anche se i server sono stati abilitati per la precedenza di inoltro a SIL Aggregator, non si verifica alcun problema poiché i payload dei dati verranno memorizzati nella cache in locale per un massimo di 30 giorni. Una volta Sil Aggregator, le "targeturi? è attivo e in esecuzione, tutti i dati memorizzati nella cache verranno inoltrati in una sola volta a Sil Aggregator e tutti i dati verranno elaborati.

    -   Eseguire `Add-SilVMHost`

        Esempio: `add-silvmhost –vmhostname contoso1 –hostcredential get-credential`

        -   Nell’esempio, **contoso1** è il nome di rete o indirizzo IP del server host fisico di cui si desidera eseguire il polling tramite SIL Aggregator per gli aggiornamenti regolari relativi alle macchine virtuali in esecuzione sul server per tenere traccia dei dati nel tempo. Get-Credential richiede all’utente che ha eseguito l’accesso di specificare l’account da usare per eseguire il polling dell’host da quel punto in avanti. L’esecuzione dello stesso comando nello stesso host consente di aggiornare l’account usato in qualsiasi momento. Fare attenzione alle modifiche e alle scadenze delle password degli account. Se le credenziali vengono modificate o scadono, non viene eseguito il polling nell’host.

        -   Per impostazione predefinita, il polling viene avviato ogni ora, a partire dall’ora successiva all’esecuzione di `Start-SilAggregator` o un’ora dopo l’aggiunta di un host all’elenco di polling.  È possibile modificare l’intervallo di polling usando `Set-SilAggregator cmdlet`.

        -   Questo cmdlet rileva automaticamente da un elenco di opzioni predefinite (vedere la sezione **Informazioni sui cmdlet di SIL Aggregator**) HostType e HyperVisorType corretti per l’host da aggiungere. Se il rilevamento non è possibile oppure le credenziali specificate non sono corrette, viene visualizzato un prompt. Se si accetta immettendo **Y**, l’host viene aggiunto e visualizzato come **Unknown** ma non ne viene eseguito il polling.

    -   Eseguire `Set-SilAggregator –AddCertificateThumbprint` "identificazione personale del certificato client?

        Questa operazione è necessaria per poter ricevere i dati tramite HTTPS da server Windows in cui è abilitata la funzionalità Registrazione inventario software. L’identificazione personale viene aggiunta all’elenco di identificazioni personali da cui SIL Aggregator accetta i dati. SIL Aggregator è progettato per accettare certificati di autenticazione client aziendali validi. Il certificato usato dovrà essere installato nel  **\\archivi localmachine\MY (Computer locale -> personale**) archiviato nel server di inoltro dei dati.

-   Nei server Windows di cui deve essere eseguito l’inventario, aprire PowerShell come amministratore e eseguire i comandi seguenti:

    -   Eseguire `Set-SilLogging –TargetUri “https://contososilaggregator�? –CertificateThumbprint “your client certificate’s thumbprint�?`

        -   In questo modo vengono indicati a Registrazione inventario software in Windows Server il percorso a cui inviare i dati di inventario e il certificato da usare per l'autenticazione.

            > [!IMPORTANT]
            > Assicurarsi che sia specificato “https://’ nel valore TargetUri.

        -   Installare il certificato client dell'organizzazione con l'identificazione personale in **\localmachine\MY** o usare **certmgr.msc** per installare il certificato in **Computer locale -> Personale**.

            > [!IMPORTANT]
            > Se i valori non sono corretti o il certificato non è installato nel percorso corretto o non è valido, gli inoltri alla destinazione non verranno eseguiti quando viene avviata la funzionalità Registrazione inventario software. I dati rimangono memorizzati nella cache in locale per un massimo di 30 giorni.

    -   Eseguire `Start-SilLogging`

        Viene avviata la funzionalità Registrazione inventario software. Ogni ora, a intervalli casuali all’interno dell’ora, Registrazione inventario software inoltra i dati di inventario al software SIL Aggregator specificato con il parametro `–targeturi` . Il primo inoltro è costituito da un set di dati completo. Ogni inoltro successivo è costituito da un "heartbeat? identificare solo i dati che è stato modificato nulla. Se non è stata apportata alcuna modifica al set di dati, viene inoltrato un altro set di dati completo.

    -   Eseguire `Publish-SilData`

        -   Questo passaggio è facoltativo la prima volta che viene abilitata la funzionalità Registrazione inventario software per la registrazione.

        -   Si tratta di un unico inoltro manuale di un set di dati completo.

        -   Se Registrazione inventario software è stata avviata per un periodo di tempo limitato ed è stato impostato un nuovo SIL Aggregator con `Set-SilLogging`, è necessario eseguire questo cmdlet una sola volta per inviare un set di dati completo al nuovo SIL Aggregator.

Dopo aver eseguito questi passaggi per l’aggiunta di host fisici in cui sono in esecuzione macchine virtuali Windows Server E aver abilitato la funzionalità Registrazione inventario software in questi server Windows, sarà possibile eseguire `Publish-SilReport –OpenReport` in qualsiasi momento in SIL Aggregator (è necessario aver installato Excel 2013). Si noti tuttavia che poiché il cubo di SQL Server Analysis Services esegue l’elaborazione una sola volta al giorno, i dati non sono disponibili nei report nella stessa giornata.

## <a name="architectural-overview"></a>Panoramica dell’architettura
Registrazione inventario software funziona in modalità push e pull e include due componenti in parallelo: La funzionalità registrazione inventario Software (SIL) in Windows Server e il file MSI scaricabile di Software Inventory Logging Aggregator (SILA). I server da includere nell’inventario usando Registrazione inventario software eseguono il push dei dati dell’inventario in SIL Aggregator (ogni ora in momenti casuali all’interno di ogni ora). SIL Aggregator esegue a sua volta il polling o una query negli host hypervisor per eseguire il pull dei dati di inventario hardware ogni ora Per garantire la completa funzionalità di Registrazione inventario software è necessario configurare correttamente l’esecuzione del push e del pull. La configurazione può essere eseguita in qualsiasi ordine. Tuttavia, poiché l’elaborazione dei cubi in SIL Aggregator viene eseguita una volta al giorno, i dati acquisiti in SIL Aggregator tramite push o pull vengono visualizzati nei report il giorno successivo.

![](../media/software-inventory-logging/SILA_Architecture.png)

> [!IMPORTANT]
> L’uso del software non comporta l’invio di dati a Microsoft.

## <a name="enable-sil-on-multiple-servers"></a>Abilitazione di Registrazione inventario software in più server
In un’infrastruttura di server distribuiti, la funzionalità Registrazione inventario software può essere abilitata in diversi modi, ad esempio in un cloud privato di macchine virtuali.  Di seguito è riportato un esempio di configurazione delle immagini di Windows Server per l’invio automatico dei dati di inventario a SIL Aggregator quando vengono avviate per la prima volta nella rete.

Eseguire i cmdlet seguenti nella console PowerShell come amministratore in ogni VM o computer fisico/dispositivo in esecuzione in cui è installato Windows Server. Vedere la sezione **Prerequisiti** :

Per eseguire questa procedura, sarà necessario un certificato SSL client valido in formato PFX.  L'identificazione personale del certificato dovrà essere aggiunta all'aggregatore SIL mediante il cmdlet `Set-SILAggregator –AddCertificateThumbprint`. Non è necessario che questo certificato client corrisponda al nome dell'aggregatore SIL.

-   `$secpasswd = ConvertTo-SecureString "`**<password for the account with permissions to the network location holding your client pfx file>**`" -AsPlainText –Force`

-   `$mycreds = New-Object System.Management.Automation.PSCredential ("`**<user account with permissions to the network location holding your client  pfx file>**`", $secpasswd)`

-   `$driveLetters = ([int][char]'C')..([int][char]'Z') | % {[char]$_}`

-   `$occupiedDriveLetters = Get-Volume | % DriveLetter`

-   `$availableDriveLetters = $driveLetters | ? {$occupiedDriveLetters -notcontains $_}`

-   `$firstAvailableDriveLetter = $availableDriveLetters[0]`

-   `New-PSDrive -Name $firstAvailableDriveLetter -PSProvider filesystem -root` **<\\server\path condividere che contiene il file di certificato pfx >** `-credential $mycreds`

-   `Copy-Item ${firstAvailableDriveLetter}:\`**c: < file nomecertificato. pfx nella directory della nuova unità >\<posizione di propria scelta >**

-   `Remove-PSDrive –Name $firstAvailableDriveLetter`

-   `$mypwd = ConvertTo-SecureString -String "`**<password for the certificate pfx file>**`" -Force –AsPlainText`

-   `Import-PfxCertificate -FilePath c:\`**<location\\certificatename.pfx>** `cert:\localMachine\my -Password $mypwd`

-   `Set-sillogging –targeturi “https://`**<machinename of your SIL Aggregator>** `–certificatethumbprint`

> [!NOTE] 
> Usare l'identificazione personale del certificato dal file pfx client e aggiunta all'aggregatore SIL usando il **Set-SilAggregator '-AddCertificateThumbprint** cmdlet.

-   `Start-sillogging`

Quando SIL Aggregator non è raggiungibile, i dati di inventario di Registrazione inventario software rimangono memorizzati nella cache in locale nei server Windows per un massimo di 30 giorni. Una volta completato il push in SIL Aggregator, tutti i dati memorizzati nella cache vengono inoltrati.

Aggiungere `Publish-SilData` all’elenco precedente se viene eseguito il push dei dati di Registrazione inventario software a un nuovo SIL Aggregator dopo il completamento dei push a un precedente SIL Aggregator. In questo modo viene inviato un complemento completo dei dati di Registrazione inventario software necessario al nuovo SIL Aggregator per il computer.

## <a name="software-inventory-logging-aggregator-reports"></a>Report di Software Inventory Logging Aggregator
![](../media/software-inventory-logging/SILA_Report.png)

### <a name="cube-processing"></a>Elaborazione cubi
In Software Inventory Logging Aggregator, il cubo di SQL Server Analysis Services viene elaborato una volta al giorno alle 3.00 dell’ora di sistema locale. I report prendono in considerazioni i dati fino a quell’ora e non i dati successivi del giorno stesso.

### <a name="high-water-mark"></a>Limite massimo
Un aspetto fondamentale dei report Software Inventory Logging Aggregator è l'acquisizione di ciò che è noto come un "limite massimo? di contemporaneamente in esecuzione Windows Server. Il limite si applica ai totali di Windows Server e System Center nei report. Per Windows Server, ogni host fisico dispone di un momento specifico, indipendentemente dal tipo di sistema operativo dell’host, nel corso di un mese in cui la maggior parte delle macchine virtuali Windows Server viene eseguita simultaneamente. Questo momento corrisponde al limite massimo del mese. Per System Center, esiste un momento specifico del mese in cui la maggior parte dei server Windows gestiti vengono eseguiti simultaneamente per singolo host fisico. Il server gestito viene identificato quando sono presenti uno o più agenti System Center. Nel report viene visualizzato soltanto il limite massimo più recente di ogni host fisico. I dati successivi al limite massimo non vengono visualizzati. Per questa ragione si può presupporre che il numero di macchine virtuali Windows Server (schede WS) o di macchine virtuali Windows Server gestite (schede SC) sia inferiore al limite massimo dopo quel momento. Questa modalità di registrazione e rappresentazione dell’uso è finalizzata alla pianificazione delle capacità e all’allineamento ai modelli di licenza di questi prodotti.

In SQL relative schede nel report, le installazioni vengono conteggiate in modo cumulativo; SQL Server non dalla hig limite. I totali corrispondono a un conteggio parziale delle installazioni di SQL Server.

> [!NOTE]
> L’uso di Registrazione inventario software non sostituisce l’obbligo di creare report precisi sull’uso del software Microsoft in base alle condizioni di licenza applicabili.

### <a name="poll-date-time"></a>Data e ora di polling
Quando si usa Software Inventory Logging Aggregator è importante tenere presente che l’aggregazione per i totali del limite massimo è eseguita in base al polling. In altre parole, un limite massimo può essere acquisito solo attraverso il polling dell'host fisico sottostante. In questo modo i conteggi di valore del limite massimo sono direttamente associati con una corrispondente "Poll data ora.? Mentre l’intervallo di polling è regolabile, la fedeltà dei limiti massimi acquisiti subisce gli effetti dell’uso di un valore di intervallo maggiore. Maggiore è l'intervallo, meno rappresentativi saranno i dati di uso effettivo.

### <a name="reports-are-month-by-month"></a>Rappresentazione dei report mese per mese
Tutti i report, anche i report annuali, sono rappresentati mese per mese. I limiti massimi, i totali e i dati del computer vengono reimpostati all’inizio di ogni mese.

I dati dei report vengono influenzati dal passaggio a un nuovo mese nel modo seguente:

-   Tutti i limiti massimi di tutti gli host vengono reimpostati all’inizio di un nuovo mese.

-   Se SIL Aggregator riceve tramite HTTPS almeno un payload completo da una macchina virtuale e non riceve più alcun heartbeat, nei polling dell’host sottostante eseguiti nel mese verrà presupposta l’associazione tra host, macchina virtuale e dati della macchina virtuale in esecuzione o arrestata nel mese. All’inizio del nuovo mese, l’associazione viene cancellata fino a quando non viene ricevuto un payload completo o un heartbeat inviato dalla macchina virtuale.

### <a name="additional-notes-on-report-behavior"></a>Note aggiuntive sul funzionamento dei report

-   Le schede di riepilogo sono elenchi di riferimento rapido dell’inventario. Gli host e le macchine virtuali sono elencati nella stessa colonna.

-   Ignorare tutti i valori di colore grigio o che appaiono disattivati. Questi valori sono elementi della creazione di report dal cubo SSAS.

-   Se una macchina virtuale sia elencata con "sistema operativo sconosciuto?, significa che Sil Aggregator non ha ricevuto un payload di dati completo dalla macchina virtuale attraverso registrazione inventario software tramite HTTPS.

-   Macchine virtuali elencate in "Host sconosciuto? correttamente l'inoltro dei dati di inventario tramite HTTPS a Sil Aggregator e Sil Aggregator non attivamente o correttamente le VM Windows Server eseguono il polling di host sottostante per la macchina virtuale. I totali di questi valori risultano azzerati poiché l’host sottostante è sconosciuto. Usare il cmdlet `Add-SilVMHost` con le credenziali corrette per aggiungere l’host specifico o tutti gli host a SIL Aggregator per l’esecuzione del polling. Una volta completato il polling, i dati della macchina virtuale e dell’host vengono associati nei report successivi.

-   Tutte le date e ore sono locali rispetto all’ora e alle impostazioni internazionali di sistema di SIL Aggregator. Sono inclusi i dati di inventario ricevuti tramite HTTPS dai sistemi in cui è abilitata la funzionalità Registrazione inventario software. Quando i file vengono elaborati, entro 20 minuti dalla ricezione, i dati vengono inseriti nel database con l’ora di sistema locale.

-   "SIL Aggregator? viene rilevato su qualsiasi computer server in cui è installato Software Inventory Logging Aggregator.

-   Se in un host fisico il numero di processori o la quantità di memoria fisica viene modificata, nel report viene visualizzata una nuova riga insieme alla riga precedente. Gli aggiornamenti del polling vengono interrotti nella riga precedente e proseguono nella nuova riga come se fosse stato aggiunto un nuovo host.

-   Nelle schede **Summary** e **Detail** i totali delle colonne relativi ai server Windows con esecuzione simultanea o ai server Windows gestiti indicano un totale di tutti i limiti massimi per tutti gli host sottostanti. Sono inclusi server Windows che non sono host hypervisor e macchine virtuali e i server che possono avere macchine virtuali in esecuzione ma sono "sconosciuti? come dati non viene ricevuti dall'interno della macchina virtuale attraverso registrazione inventario software tramite HTTPS. I server sono inclusi nei totali per ragioni di praticità.

-   Nella sezione **SQL Server** della scheda **Dashboard** il totale delle installazioni di SQL Server è un riepilogo dei totali della versione del dashboard.  Questo può causare una discrepanza con il totale visualizzato nella scheda **SQL Detail** nei casi in cui sono installate più versioni di SQL in un singolo server.  A differenza della scheda **Detail**, nel dashboard il totale è separato per ogni server.  Le diverse versioni di SQL installate in un unico Windows Server vengono conteggiate come un solo elemento,  conformemente alle condizioni di licenza.

-   Nella sezione **Windows Server** della scheda **Dashboard** le righe **Other Hypervisor Hosts** e **Total Hypervisor Hosts** includono gli host fisici Windows Server che eseguono o NON eseguono Hyper-V.

### <a name="column-descriptions"></a>Descrizioni delle colonne
Di seguito sono riportate le descrizioni delle singole colonne della scheda **Windows Server Detail** del report Excel creato da SIL Aggregator. Le altre schede di dati corrispondono o includono un sottoinsieme di queste colonne. L'unica eccezione può essere "Install Count? delle schede SQL Server (vedere **massimi** sezione).

|Intestazione di colonna|Descrizione|
|-----------------|---------------|
|Calendar Month|I dati nei report sono raggruppati per mese, con il più recente per primo. I dati all’interno del mese non sono elencati in un ordine specifico.|
|Nome host|Nome di rete o nome di dominio completo dell'host fisico di cui SIL Aggregator esegue il polling.<br /><br />Usare il cmdlet Get-SilVMHost per individuare gli host che sono stati aggiunti ma di cui non viene eseguito il polling. Viene visualizzato l'ultimo polling eseguito.|
|Host Type|Produttore del sistema operativo dell'host fisico.|
|Hypervisor Type|Produttore di hypervisor per l'host fisico.|
|Processor Manufacturer|Produttore dei processori dell'host fisico.|
|Processor Model|Modello dei processori dell'host fisico.|
|Is Hyper Threading Enabled?|Visualizza il valore True o False in base all’abilitazione della tecnologia hyper-threading nei processori dell'host fisico.|
|Nome VM|Nome di rete o nome di dominio completo della macchina virtuale Windows Server. Se SIL Aggregator non ha ricevuto dati dalla macchina tramite HTTPS, viene visualizzato il nome descrittivo della macchina virtuale in hypervisor.|
|Simultaneously Running Windows Server VMs by host|Totale delle macchine virtuali Windows Server in esecuzione simultanea nell’host. Il numero maggiore del mese relativo all’host corrisponde al limite massimo visualizzato e acquisito in quel momento specifico.<br /><br />Vedere la sezione **Limite massimo** .<br /><br />Gli host fisici in cui è installato Windows Server o in cui è installato Windows Server e non sono in esecuzione macchine virtuali Windows Server note hanno sempre un totale pari a uno. Se almeno una macchina virtuale di Windows Server nota è in esecuzione nell’host e Windows Server è in esecuzione nell’host, il sistema operativo dell’host non viene conteggiato.|
|Physical Processor Count|Numero di processori fisici installati nell'host fisico.|
|Physical Core Count|Numero di core dei processori fisici installati nell'host fisico.|
|Virtual Processor Count|Numero di processori virtuali riconosciuti all'interno della macchina virtuale. Questo valore proviene solo da dati inoltrati tramite HTTPS usando Registrazione inventario software in Windows Server.|
|Data e ora di polling|Data e ora del momento più recente di limite massimo delle macchine virtuali Windows Server in esecuzione simultanea nell’host fisico.<br /><br />Vedere la sezione **Data e ora di polling** .|
|VM Last Seen Date Time|Data e ora dell’ultima ricezione da parte di SIL Aggregator dei dati di inventario inviati tramite HTTPS dalla macchina virtuale Windows Server.|
|Host Last Seen Date Time|Data e ora dell’ultima ricezione da parte di SIL Aggregator dei dati di inventario inviati tramite HTTPS dall’host fisico Windows Server.<br /><br />È possibile usare host fisici che eseguono Windows Server e HyperV per l’abilitazione di Registrazione inventario software e l’inoltro dei dati di inventario tramite HTTPS a SIL Aggregator.|

## <a name="sil-aggregator-cmdlets-detail"></a>Informazioni sui cmdlet di SIL Aggregator
Di seguito sono fornite informazioni dettagliate sui cmdlet di SIL Aggregator. Per informazioni sui cmdlet completo, vedere: [Cmdlet di PowerShell di SIL Aggregator](https://technet.microsoft.com/library/mt548455.aspx)

### <a name="publish-silreport"></a>Publish-SilReport

-   Questo cmdlet, usato così com’è, crea un report di Registrazione inventario software e lo inserisce nella registrazione nella cartella Documenti dell’utente. È necessario aver installato Excel 2013 nel computer in cui viene eseguito il cmdlet.

-   Usato con il parametro `–OpenReport` , il cmdlet crea il report e lo apre in Excel per la visualizzazione.

-   Durante l’installazione di SIL Aggregator è possibile notare la disponibilità dell’opzione che consente di installare soltanto il modulo dei report. È possibile installare il modulo dei report nel sistema operativo di un client Windows, ad esempio Windows 8.1 o Windows 10. Ciò consente a un thin client, ad esempio un laptop o un tablet, di connettersi a un server di database di SIL Aggregator per la pubblicazione diretta dei report di Registrazione inventario software.

    -   L’esempio seguente richiede l’immissione delle credenziali da usare, si connette al server di database di SIL Aggregator denominato SILContoso e crea e apre un report di Registrazione inventario software nel computer locale.

        `Publish-SilReport -DBServerName "SILContoso" -DBServerCredential Get-Credential –OpenReport`

    -   Nella maggior parte dei casi, alla prima connessione è necessario aprire una porta nel firewall nel server di database di SIL Aggregator per consentire le connessioni. I professionisti IT desiderano in genere eseguire questa configurazione in anticipo per consentire ai controller finanziari o a altri responsabili di inventario di accedere per creare i report desiderati. Per istruzioni sui passaggi da eseguire, vedere il collegamento riportato di seguito. Una porta predefinita tipica per SQL Server Analysis Services è la porta 2383.

### <a name="add-silvmhost"></a>Add-SilVMHost
Quando si usa il cmdlet `Add-SilVMHost` sono supportati i seguenti tipi di host e le seguenti versioni di hypervisor. Si noti che non è necessario specificarli. Il cmdlet `Add-SilVMHost` rileva automaticamente la combinazione supportata. Se il rilevamento non è possibile oppure le credenziali specificate non sono corrette, viene visualizzato un prompt. Se l'utente accetta con una "Y? voce, verrà aggiunto l'host, ma lo sarà non viene eseguito il polling. Verrà aggiunto come "sconosciuto?.

|Versione Hypervisor|Valore HostType di SIL Aggregator|Valore HypervisorType di SIL Aggregator|
|----------------------|-----------------------------------------|---------------------------------------|
|Windows Server, 2012 R2|Windows|HyperV|
|VMware 5.5|VMware|Esxi|
|Xen 4.x|Ubuntu, OpenSuse o CentOS|Xen|
|XenServer 6.2|Citrix|XenServer|
|KVM|Ubuntu, OpenSuse o CentOS|KVM|

È possibile che funzionino anche altre versioni di queste piattaforme e di questi tipi hypervisor.  SIL Aggregator viene fornito con la versione sshnet indicata di seguito.  Questa versione viene usata per la comunicazione con le piattaforme di virtualizzazione Linux.

<pre>sshnet 2014.4.6-beta1
https://sshnet.codeplex.com/releases/view/120504
Copyright (c) 2010, RENCI</pre>

### <a name="get-silaggregator"></a>Get-SilAggregator
`Get-SilAggregator` fornisce le informazioni di configurazione per l’applicazione Software Inventory Logging Aggregator. Nell’output dell’esempio riportato di seguito è possibile osservare quanto segue:

-   L’applicazione è in esecuzione

-   L’intervallo di polling è ogni ora. L’intervallo può essere modificato con incrementi di un’ora.

-   L’ora di avvio del polling

-   L’URI di destinazione che deve essere impostato negli altri computer per l’inoltro dei dati a SIL Aggregator

-   Le identificazioni personali di certificato da cui SIL Aggregator accetta i dati di Registrazione inventario software

-   Il tipo di account specificato al momento dell’installazione

    `PS C:\Windows\system32> Get-SilAggregator`

    ``

    `State          : Running HostPollIntervalInHours : Every 1 Hour(s)`

    `PollStartTime      : 8/24/2015 5:07:33 AM`

    `TargetURI        : https://SilContoso`

    `CertificateThumbprint  : 3efc6b8ce7d5eefba5107ede9d1caca550417452, 2dc4ea8bfb64b1246a8c1ffa1b701cd1042a3412`

    `UserProfile       : Local`

### <a name="set-silaggregator"></a>Set-SilAggregator
Il cmdlet `Set-SilAggregator` consente di:

-   Modificare l’intervallo orario di esecuzione del polling.

-   Modificare la data e ora di avvio del polling per avviare il polling nell’intervallo specificato.

-   Aggiungere o rimuovere identificazioni personali di certificato da cui SIL Aggregator accetta i dati o a cui è associato.

### <a name="get-aggregatordata"></a>Get-AggregatorData

-   Usato da solo, questo cmdlet visualizza il contenuto della scheda Dettagli di Windows Server di un report Excel di SIL Aggregator.

-   Usato con i parametri, questo cmdlet recupera i dati direttamente dal database progettato per gli usi personalizzati della soluzione Registrazione inventario software.

-   Si noti che i parametri `–StartTime` e `–Endtime` visualizzano i dati del report a partire dal primo giorno del mese della data di inizio e all’ultimo giorno del mese della data di fine.

![](../media/software-inventory-logging/SILA_Get-SILAggregator.png)

### <a name="get-silvmhost"></a>Get-SilVMHost

-   Questo cmdlet visualizza l’elenco degli host fisici il cui polling è configurato in SIL Aggregator, la data e l’ora dell’ultimo polling eseguito, il valore HostType (o del produttore del sistema operativo) e il valore HypervisorType (produttore di hypervisor). Per altre informazioni sui valori HostType e HypervisorType, vedere la descrizione di Add-SilVMHost.

    Se in un host non sono disponibili la data e l’ora del polling e i valori HostType e HypervisorType sono supportati, significa che il polling non è stato ancora avviato o completato.

-   Questo cmdlet visualizza anche tutti i nomi host aggiunti tramite i dati provenienti dalle macchine virtuali, a condizione che sia disponibile dalla macchina virtuale. I nomi host sono inclusi nell’elenco senza alcun valore HostType o HypervisorType. Questi dati possono essere utili per associare le macchine virtuali e gli host non configurati per il polling.

-   Usare i parametri `–StartTime` e`–EndTime` per individuare quando gli host sono stati aggiunti per la prima volta o quando è stato eseguito l’ultimo polling.

### <a name="remove-silvmhost"></a>Remove-SilVMHost

-   Questo cmdlet rimuove qualsiasi host dall’elenco degli host di cui eseguire il polling. Se un host viene rimosso, è possibile che l’host venga aggiunto nuovamente all’elenco da una macchina virtuale nell’host ma il polling non verrà eseguito con le credenziali corrette specificate con il cmdlet `Add-SilVMHost` .

-   Se un host viene rimosso, viene rimosso dal polling ma non dai report. Poiché l’esecuzione del polling viene interrotta, l’host non sarà incluso nei report dei mesi successivi.

-   Usare i parametri `–StartTime` e`–EndTime` separatamente per la rimozione di gruppi di host di cui è stato eseguito il polling fino a una data specifica o a partire da una data specifica.

## <a name="avoid-these-errors-and-issues-with-sil-and-sil-aggregator-troubleshooting-guide"></a>Errori e problemi da evitare con Registrazione inventario software e SIL Aggregator (Guida alla risoluzione dei problemi)

-   Elementi da controllare se il cmdlet `SilLogging` o `Publish-Sildata` non viene eseguito o si verifica un errore:

    -   Assicurarsi che sia specificato **https://** nell’ **URI di destinazione** .

    -   Verificare che siano stati installati tutti gli aggiornamenti di Windows Server (vedere i prerequisiti per Registrazione inventario software).  Per verificare in modo rapido viene possibile ricercarli usando il cmdlet seguente:   `Get-SilWindowsUpdate *3060*, *3000*`

    -   Assicurarsi che il certificato usato per l’autenticazione in SIL Aggregator sia installato nel percorso corretto sul server locale di cui eseguire l’inventario con Registrazione inventario software (vedere la sezione Guida introduttiva).

    -   In SIL Aggregator, assicurarsi che l’identificazione personale del certificato usato per l’autenticazione in SIL Aggregator sia aggiunta all’elenco usando il cmdlet `Set-SilAggregator –AddCertificateThumbprint` (vedere la sezione Guida introduttiva).

    -   Se si usano certificati dell’organizzazione, assicurarsi che il server in cui è stata abilitata la funzionalità Registrazione inventario software appartenga al dominio per il quale è stato creato il certificato o sia in altro modo verificabile tramite un’autorità radice. Se un certificato non è attendibile nel computer locale che sta tentando di eseguire l’inoltro o il push dei dati a SIL Aggregator, l’azione viene interrotta con un errore.

    -   Se tutti gli elementi indicati sono stati verificati, controllare che il certificato usato per l’installazione di SIL Aggregator sia valido e che il nome del certificato corrisponda al nome del server di SIL Aggregator. Quest’ultimo passaggio non è necessario se altri computer inoltrano correttamente i dati allo stesso SIL Aggregator.

    -   È possibile controllare il percorso per i file di registrazione inventario software memorizzati nella cache sul server che tenta di eseguire l'inoltro/push, \Windows\System32\\Logfiles\\registrazione inventario software. Se `SilLogging` è stato avviato ed è in esecuzione da più di un’ora oppure `Publish-SilData` è stato eseguito di recente e la cartella non contiene file, l’accesso a SIL Aggregator è stato eseguito correttamente.

-   Assicurarsi che l’utente che ha eseguito l’accesso possa accedere al database SQL e a Analysis Services.

    -   Questa operazione è necessaria durante l’installazione di SIL Aggregator.

    -   Ciò è necessario quando si usa PowerShell in modalità remota per la gestione di SIL Aggregator.

-   Per pubblicare i report di SIL Aggregator dal sistema operativo di un client desktop:

    -   Usare l’opzione di installazione del solo modulo dei rapporti nel client Windows (8.1/10).

    -   Se si verificano problemi durante la creazione di un report quando PowerShell viene usato in remoto, è possibile sia necessario aprire una porta del firewall nel software SIL Aggregator a cui si sta tentando di connettersi (vedere la descrizione del cmdlet `Publish-SilReport` nella sezione Informazioni sui cmdlet di SIL Aggregator).

-   Quando si usa l’opzione gMSA:

    -   Non dimenticare di riavviare il server dopo averlo aggiunto al gruppo di computer gMSA in Active Directory.

    -   Durante il processo di installazione, non usare il nome completo di dominio quando si specifica dominio\utente. Ad esempio, usare **miodominio\accountgmsa**. Non immettere **mydomain.<i> </i>com\gmsaaccount**.

-   Quando si usa il Framework di gestione di Windows nell'ambiente in uso:

    -   Assicurarsi che i server con SILA installato non è installato WMF 5.1.  È possibile ha restituito un errore nel registro eventi relative alle DLL **'mpunits.dll'**.  Ciò impedirà il corretto funzionamento.  SILA richiede solo WMF 4.0.

## <a name="managing-sil-over-time"></a>Gestione di Registrazione inventario software nel tempo

### <a name="uninstallreinstall-sil-aggregator"></a>Disinstallazione e reinstallazione di SIL Aggregator
Se necessario, è possibile disinstallare e reinstallare SIL Aggregator senza perdere i dati di inventario esistenti e cronologici. Eseguire la disinstallazione seguendo le istruzioni fornite nella presente documentazione e selezionare l’opzione che consente di mantenere il database di Registrazione inventario software. Reinstallare SIL Aggregator seguendo le istruzioni fornite nella presente documentazione e selezionare l’opzione di connessione a un database esistente.

Dopo aver eseguito questa operazione è necessario aggiornare le credenziali usando il cmdlet `Add-SilVMHost` in tutti gli host di cui SIL Aggregator ha eseguito il polling, a condizione che si desideri continuare la raccolta dei dati da questi host. Inoltre, per evitare che i report contengano voci duplicate per lo stesso host, è necessario aggiungere nuovamente gli host di cui eseguire il polling usando lo stesso indirizzo di rete con cui sono stati precedentemente aggiunti. Per aggiungere un host eseguendo il cmdlet `Add-SilVMHost` è possibile usare i seguenti tre tipi di valori vmhostname supportati:

-   Indirizzo IP

-   Nome di dominio completo

-   Nome NetBIOS

### <a name="changing-sil-aggregators"></a>Modifica di SIL Aggregator
Quando si desidera iniziare a eseguire l’inventario dei server nel proprio ambiente con un diverso SIL Aggregator, è sufficiente usare il cmdlet `Set-SilLogging –TargetUri` di Registrazione inventario software per modificare l’URI di destinazione e, se necessario, l’identificazione personale di certificato. Tenere presente che dopo aver eseguito questa operazione è necessario usare il cmdlet `Publish-SilData` almeno una volta per inoltrare un inventario completo al nuovo SIL Aggregator.

### <a name="changing-or-updating-certificates"></a>Modifica o aggiornamento dei certificati
**PASSAGGI IMPORTANTI PER EVITARE LA PERDITA DI DATI:** Se è necessario modificare il certificato che utilizzano i server per inoltrare i dati a SIL Aggregator, ma la destinazione di Sil Aggregator subirà modifiche, usare questi passaggi per evitare una potenziale perdita di dati in transito a Sil Aggregator:

-   In SIL Aggregator usare il cmdlet `Set-SilAggregator –AddCertificateThumbprint` per aggiungere la nuova identificazione personale a SIL Aggregator.

-   In TUTTI i server che eseguono l’inoltro dei dati, installare il nuovo certificato in **\LOCALMACHINE\MY** usando il metodo preferito.

-   In TUTTI i server che eseguono l’inoltro dei dati, usare il cmdlet `Set-SilLogging –CertificateThumbprint` per eseguire l’aggiornamento all’identificazione personale del nuovo certificato.

-   **CRITICO: Solo dopo che sono stati aggiornati tutti i server di inoltro dei dati, rimuovere l'identificazione personale del vecchio** da SIL Aggregator usando `Set-SilAggregator –RemoveCertificateThumbprint` cmdlet. Se un server continua a eseguire l’inoltro usando un certificato rimosso da SIL Aggregator, **i dati vengono persi** senza essere inseriti nel database di SIL Aggregator. Ciò si verifica soltanto nelle situazioni in cui un server ha precedentemente eseguito l’inoltro dei dati a SIL Aggregator ed è stato successivamente rimosso il certificato dall’elenco delle identificazioni personali da cui accettare i dati di SIL Aggregator.

## <a name="release-notes"></a>Note sulla versione

-   A causa di un problema noto, l'aggregatore SIL non elaborerà e non segnalerà la presenza di installazioni di SQL Server Standard Edition.  Per risolvere il problema, seguire questa procedura:

    1.  Aprire SQL Server Management Studio nell'aggregatore SIL.

    2.  Connettersi al motore di database.

    3.  Espandere il database SoftwareInventoryLogging quindi espandere Tables nella struttura per la selezione.

    4.  Fare clic con il pulsante destro del mouse su **dbo.SqlServerEdition**, quindi scegliere '**Modifica le prime 200 righe**'.

    5.  Modificare il valore PropertyNumValue accanto a "Standard Edition? per **2760240536** (da -1534726760).

    6.  Chiudere la query per salvare le modifiche.

    7.  Per qualsiasi server che esegue SIL e che ha già registrato dati nell'aggregatore potrebbe essere necessario eseguire una volta il cmdlet `Publish-SilData` per l'aggregatore, in modo da elaborare correttamente la presenza di SQL Server Standard Edition.

-   Nei report di Registrazione inventario software, se la tecnologia hyper-threading è abilitata nel server fisico, tutti i totali dei core dei processori includono il numero di thread.  Per ottenere il totale effettivo dei core fisici nei server con la tecnologia hyper-threading abilitata è necessario ridurre i totali della metà.

-   I totali nelle righe (in **Dashboard** scheda) e le colonne (sul **Summary e Detail** schede) con etichetta "**Simultaneously Running**... ?, per Windows Server e System Center non corrispondono tra le due posizioni. Nel **Dashboard** scheda, è necessario aggiungere "**Windows Server Devices (con nessun known VMs**)? valore di "**Simultaneously Running**... ? per ottenere il numero indicato nelle schede **Summary e Detail**.

-   Durante la modifica o l’aggiornamento dei certificati, tenere presente le **OPERAZIONI IMPORTANTI PER EVITARE LA PERDITA DI DATI** descritte nella sezione **Gestione di Registrazione inventario software nel tempo** della presente documentazione.

-   Sebbene sia possibile aggiungere host Windows Server 2008 R2 e Windows Server 2012 all’elenco degli host di cui eseguire il polling, la presente versione 1.0 di SIL Aggregator supporta soltanto il polling di host Windows Server 2012 R2 per Windows/Hyper-V per i quali viene garantito il funzionamento corretto di tutte le funzionalità.  In particolare, è noto che durante l’esecuzione del polling di host Windows Server 2008 R2, è possibile che le macchine virtuali e gli host non corrispondano nei report di SIL Aggregator.

## <a name="see-also"></a>Vedere anche
[Software Inventory Logging Aggregator 1.0 per Windows Server](https://www.microsoft.com/en-us/download/details.aspx?id=49046)<br>
[Cmdlet di PowerShell di SIL Aggregator](https://technet.microsoft.com/library/mt548455.aspx)<br>
[Cmdlet di PowerShell di registrazione inventario software](https://technet.microsoft.com/library/dn283390.aspx)<br>
[Una panoramica di registrazione inventario software](https://technet.microsoft.com/library/dn268301.aspx)<br>
[Gestire registrazione inventario software](https://technet.microsoft.com/library/dn383584.aspx)

