---
title: Ripristino della foresta Active Directory - ripristino di Windows Server 2003
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 07/07/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.assetid: 5a291f65-794e-4fc3-996e-094c5845a383
ms.technology: identity-adfs
ms.openlocfilehash: daebbc65b9864554fde839c3865f507ab787e6b6
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/12/2017
---
# <a name="ad-forest-recovery---windows-server-2003-recovery"></a>Ripristino della foresta Active Directory - ripristino di Windows Server 2003

>Si applica a: Windows Server 2003

Questo argomento include le procedure di ripristino di foreste per i controller di dominio (DC) che eseguono Windows Server 2003. Il processo generale di ripristino della foresta è non diverso con i controller di dominio di Windows Server 2003, ma procedure specifiche possono essere diverse a causa di diversi strumenti. Ad esempio, è possibile utilizzare Ntdsutil.exe per eseguire il backup e ripristinare i controller di dominio che eseguono Windows Server 2003 i controller di dominio, mentre Windows Server Backup o Wbadmin.exe è utilizzato per i controller di dominio che eseguono Windows Server 2008 o versione successiva.  
  
-   [Eseguire il backup dello stato del sistema](#Backing-up-the-System-State-data)  
  
-   [Eseguire un ripristino non autorevole](#Performing-a-nonauthoritative restore)  
  
-   [Installare e configurare il servizio Server DNS](#Install-and-configure-the-DNS-Server-service)  
  
  
## <a name="backing-up-the-system-state-data"></a>Eseguire il backup dello stato del sistema  
 Utilizzare la procedura seguente per eseguire il backup dello stato del sistema, insieme a tutti gli altri dati selezionati per l'operazione di backup corrente, di un controller di dominio che esegue Windows Server 2003. Windows Server 2003 include lo strumento Ntbackup, che è possibile utilizzare per eseguire il backup dei dati dello stato del sistema.  
  
 Appartenenza al gruppo **amministratori** o **Backup Operators**, o equivalente è il requisito minimo necessario per eseguire il backup di file e cartelle.   
  
 Se esegue il backup dello stato del sistema su un nastro e il programma di Backup indica che non è disponibile alcun supporto inutilizzato, potrebbe essere necessario utilizzare archivi rimovibili. Aggiunge il nastro di pool di supporti liberi, in modo che il Backup è possibile utilizzarlo.  
  
 È possibile eseguire il backup solo dello stato del sistema in un computer locale. È possibile eseguirne il backup in un computer remoto.  
  
### <a name="to-back-up-the-system-state-data-on-a-domain-controller-that-runs-windows-server-2003"></a>Per eseguire il backup dello stato del sistema in un controller di dominio che esegue Windows Server 2003  
  
1.  Fare clic su **Start**, scegliere **tutti i programmi**, scegliere **Accessori**, scegliere **utilità di sistema**, quindi fare clic su **Backup**.  
  
2.  Nel **iniziale** pagina, fare clic su **modalità avanzata**.  
  
3.  Nel **Backup** scheda, selezionare la casella di controllo per qualsiasi unità, cartelle o file che si desidera eseguire il backup.  
  
4.  Selezionare il **dello stato del sistema** casella di controllo.  
  
5.  Fare clic su **avviare Backup**.  
  
  
## <a name="performing-a-nonauthoritative-restore"></a>Eseguire un ripristino non autorevole  
 Utilizzare la procedura seguente per eseguire un ripristino non autorevole di un controller di dominio che esegue Windows Server 2003. Per eseguire un ripristino non autorevole in Active Directory in Windows Server 2003, eseguire automaticamente un ripristino non autorevole di SYSVOL. Ulteriori operazioni non sono necessarie.  
  
> [!NOTE]
>  Se inoltre si intende reinstallare il sistema operativo Windows Server 2003, si potrebbe o non potrebbe aggiungere il computer al dominio, è possibile assegnare qualsiasi nome al computer durante l'installazione del sistema operativo. Non installare Active Directory. Dopo aver reinstallato il sistema operativo, andare direttamente al passaggio 4.  
  
 Nei controller di dominio Windows Server 2003 in cui è stato ripristinato solo i dati di stato del sistema, è necessario reinstallare anche tutte le applicazioni software che erano in esecuzione sul controller di dominio prima del ripristino. Il ripristino di dominio Active Directory nel primo controller di dominio nel dominio inoltre consente di ripristinare il Registro di sistema poiché sono parte dei dati dello stato del sistema. Tienilo a mente se si dispone di tutte le applicazioni in esecuzione su questi controller di dominio e se avessero tutte le informazioni archiviate nel Registro di sistema.  
  
 Per risparmiare tempo necessario per installare nuovamente il software, determinare se le applicazioni che devono essere installati nel controller di dominio sono compatibili con la clonazione di controller di dominio virtuale. Tali applicazioni possono essere installate sul controller di dominio di origine prima della clonazione per salvare il tempo e fatica necessario installarli sul controller di dominio virtuale clonato.  
  
#### <a name="to-perform-a-nonauthoritative-restore"></a>Per eseguire un ripristino non autorevole  
  
1.  Dopo avere avviato il controller di dominio, premere F8 per riavviare il computer in servizi di modalità ripristino Directory (DSRM).  
  
2.  Selezionare **modalità ripristino servizi Directory (solo controller di dominio Windows)**.  
  
3.  Selezionare il sistema operativo che si desidera avviare in modalità di ripristino.  
  
4.  Accedere come amministratore (è possibile solo utilizzare un account computer locale, nessuna opzione di accesso di dominio è disponibile).  
  
5.  Al prompt dei comandi, digitare **ntbackup**, quindi premere INVIO.  
  
6.  Nel **iniziale** pagina, fare clic su **modalità avanzata**e quindi seleziona il **Ripristina e gestisci supporti** scheda (non selezionare **ripristino guidato**.)  
  
7.  Selezionare il file di backup appropriato per eseguire il ripristino da e assicurarsi che il **disco di sistema** e **dello stato del sistema** siano selezionate le caselle di controllo.  
  
8.  Fare clic su **Avvia ripristino**.  
  
9. Una volta completata l'operazione di ripristino, riavviare il computer.  
  
 Utilizzare la procedura seguente per eseguire un ripristino autorevole (noto anche come primario) della cartella SYSVOL su un controller di dominio che esegue Windows Server 2003. Eseguire questa procedura solo nel primo Windows Server 2003 controller di dominio che viene ripristinato nel dominio.  
  
### <a name="to-perform-an-authoritative-restore-of-sysvol"></a>Per eseguire un ripristino autorevole di SYSVOL  
  
1.  Eseguire i passaggi da 1 a 8 della procedura precedente.  
  
2.  Nel **Conferma ripristino** la finestra di dialogo, fare clic su **avanzate**.  
  
3.  Per eseguire un ripristino autorevole di SYSVOL, selezionare la casella di controllo **durante il ripristino replicati set di dati, contrassegnare i dati ripristinati come i dati primari per tutte le repliche**.  
  
    > [!NOTE]
    >  Contrassegnare i dati ripristinati, come i dati nel Backup primari sono equivalenti all'impostazione di **BurFlags** voce D4 nella sottochiave del Registro di sistema seguente:  
    >   
    >  **HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\NtFrs\Parameters\Cumulative Replica Sets\\***GUID*  
  
4.  Una volta completata l'operazione di ripristino, riavviare il computer.  
  
 
## <a name="install-and-configure-the-dns-server-service"></a>Installare e configurare il servizio Server DNS  
 Se il controller di dominio che è stato ripristinato da backup è in esecuzione Windows Server 2003, è possibile installare server DNS senza eseguire la connessione al controller di dominio a qualsiasi rete.  
  
### <a name="to-install-and-configure-the-dns-server-service"></a>Per installare e configurare il servizio Server DNS  
  
1.  Aprire Aggiunta guidata componenti di Windows. Per aprire la procedura guidata:  
  
    -   Fare clic su **Start**, fare clic su **Pannello di controllo**, quindi fare clic su **Aggiungi / Rimuovi programmi**.  
  
    -   Fare clic su **Installazione componenti di Windows**.  
  
2.  In **componenti**, selezionare il **servizi di rete** casella di controllo, quindi fare clic su **dettagli**.  
  
3.  In **sottocomponenti di servizi di rete**, selezionare il **sistema DNS (Domain Name)** casella di controllo, fare clic su **OK**, quindi fare clic su **Avanti**.  
  
4.  Se viene richiesto, in **copiare i file da**, digitare il percorso completo dei file di distribuzione e quindi fare clic su **OK**.  
  
     Dopo l'installazione, completare i passaggi seguenti per configurare il server DNS.  
  
5.  Fare clic su **Start**, scegliere **tutti i programmi**, scegliere **strumenti di amministrazione**, quindi fare clic su **DNS**.  
  
6.  Creare le zone DNS per gli stessi nomi di dominio DNS che sono stati ospitati nei server DNS prima il malfunzionamento critico. Per ulteriori informazioni, vedere Aggiunta di una zona di ricerca diretta ([https://go.microsoft.com/fwlink/?LinkId=74574](https://go.microsoft.com/fwlink/?LinkId=74574)).  
  
7.  Configurare i dati DNS esistente prima il malfunzionamento critico. Per esempio:  
  
    -   Configurare le zone DNS a essere archiviati in Active Directory. Per ulteriori informazioni, vedere Modifica il tipo di zona ([https://go.microsoft.com/fwlink/?LinkId=74579](https://go.microsoft.com/fwlink/?LinkId=74579)).  
  
    -   Configurare la zona DNS che è autorevole per record di risorse localizzatore controller di dominio di controller di dominio consentire l'aggiornamento dinamico sicuro. Per ulteriori informazioni, vedere consentire solo aggiornamenti dinamici protetti ([https://go.microsoft.com/fwlink/?LinkId=74580](https://go.microsoft.com/fwlink/?LinkId=74580)).  
  
8.  Assicurarsi che la zona DNS padre contiene delega record di risorse (nome server (NS) e associa host (A) resource record) per la zona figlio che è ospitato su questo server DNS. Per ulteriori informazioni, vedere come creare una delega di Zone ([https://go.microsoft.com/fwlink/?LinkId=74562](https://go.microsoft.com/fwlink/?LinkId=74562)).  
  
9. Dopo la configurazione DNS, al prompt dei comandi, digitare il comando seguente e quindi premere INVIO:  
  
     **Net stop netlogon**  
  
10. Digitare il comando seguente e quindi premere INVIO:  
  
     **Net start netlogon**  
  
    > [!NOTE]
    >  Accesso rete registrerà i record di risorse del localizzatore di controller di dominio in DNS per questo controller di dominio. Se si installa il servizio Server DNS in un server del dominio figlio, il controller di dominio non sarà in grado di registrare i propri record immediatamente. In questo modo è attualmente isolato come parte del processo di ripristino e il server DNS primario è server DNS radice della foresta. Configurare il computer con lo stesso indirizzo IP che aveva prima dell'errore per evitare errori durante la ricerca servizio controller di dominio.

## <a name="next-steps"></a>Passaggi successivi
-   [Ripristino della foresta Active Directory - prerequisiti](AD-Forest-Recovery-Prerequisties.md)  
-   [Ripristino della foresta Active Directory - sviluppo di un piano di ripristino personalizzato foresta](AD-Forest-Recovery-Devising-a-Plan.md)  
- [Ripristino della foresta Active Directory - identificare il problema](AD-Forest-Recovery-Identify-the-Problem.md)
-   [Ripristino della foresta Active Directory - determinare la modalità di ripristino](AD-Forest-Recovery-Determine-how-to-Recover.md)
-   [Ripristino della foresta Active Directory - eseguire il ripristino iniziale](AD-Forest-Recovery-Perform-initial-recovery.md)  
-   [Ripristino della foresta Active Directory - procedure](AD-Forest-Recovery-Procedures.md)  
-   [Ripristino della foresta Active Directory - domande frequenti](AD-Forest-Recovery-FAQ.md)  
-   [Ripristino della foresta Active Directory - ripristino di un singolo dominio all'interno di un insieme di strutture Multidomain](AD-Forest-Recovery-Single-Domain-in-Multidomain-Recovery.md)  
-   [Ripristino della foresta Active Directory - ripristino della foresta con i controller di dominio di Windows Server 2003](AD-Forest-Recovery-Windows-Server-2003.md) 
