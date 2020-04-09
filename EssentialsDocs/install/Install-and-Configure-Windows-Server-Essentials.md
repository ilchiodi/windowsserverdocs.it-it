---
title: Installazione e configurazione di Windows Server Essentials
description: Viene descritto come usare Windows Server Essentials
ms.date: 06/17/2013
ms.prod: windows-server
ms.topic: article
ms.assetid: e95cf219-46a4-4041-bd81-0c4c2a0622cf
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 65d3f3f30a69e9879e722cbdab260fb9715af545
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80820034"
---
# <a name="install-and-configure-windows-server-essentials"></a>Installazione e configurazione di Windows Server Essentials

>Si applica a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

##  <a name="BKMK_InstallConfigure"></a>   

 In questo documento vengono fornite istruzioni dettagliate per l'installazione e la configurazione di Windows Server Essentials. Prima di iniziare l'installazione, esaminare e completare le attività descritte in prima di [installare Windows Server Essentials](Before-You-Install-Windows-Server-Essentials.md).  

 In questo documento vengono fornite istruzioni dettagliate per l'installazione e la configurazione di Windows Server Essentials. Prima di iniziare l'installazione, esaminare e completare le attività descritte in prima di [installare Windows Server Essentials](../install/Before-You-Install-Windows-Server-Essentials.md).  
  
 Installare e configurare Windows Server Essentials in due passaggi:  
  

1.  [Passaggio 1: installare il sistema operativo Windows Server Essentials](Install-and-Configure-Windows-Server-Essentials.md#BKMK_ManualInstallation) In questo passaggio si installa il sistema operativo nel server.  
  
2.  [Passaggio 2: configurare il sistema operativo Windows Server Essentials](Install-and-Configure-Windows-Server-Essentials.md#BKMK_Step2Configure) In questo passaggio si completa l'installazione fornendo informazioni sulla società, sulle impostazioni di dominio e sull'amministratore di rete. Queste informazioni vengono utilizzate per preparare il server per l'utilizzo.  

1.  [Passaggio 1: installare il sistema operativo Windows Server Essentials](../install/Install-and-Configure-Windows-Server-Essentials.md#BKMK_ManualInstallation) In questo passaggio si installa il sistema operativo nel server.  
  
2.  [Passaggio 2: configurare il sistema operativo Windows Server Essentials](../install/Install-and-Configure-Windows-Server-Essentials.md#BKMK_Step2Configure) In questo passaggio si completa l'installazione fornendo informazioni sulla società, sulle impostazioni di dominio e sull'amministratore di rete. Queste informazioni vengono utilizzate per preparare il server per l'utilizzo.  

  
###  <a name="step-1-install-the-windows-server-essentials-operating-system"></a><a name="BKMK_ManualInstallation"></a>Passaggio 1: installare il sistema operativo Windows Server Essentials  
  
> [!IMPORTANT]
>  Dopo l'installazione del sistema operativo, non personalizzare il server fino al completamento del [passaggio 2: configurare il sistema operativo Windows Server Essentials](#BKMK_Step2Configure).  
  
 **Tempo previsto di completamento:** circa 30 minuti.  
  
> [!NOTE]
>  Il tempo di completamento previsto per il completamento di questa procedura è basato sui requisiti hardware minimi.  
  
##### <a name="to-install-the-operating-system"></a>Per installare il sistema operativo  
  
1. Connettere il computer alla rete utilizzando un cavo di rete.  
  
   > [!IMPORTANT]
   >  Non disconnettere il computer dalla rete durante la procedura di installazione per evitare che l'installazione non venga completata correttamente.  
  
2. Accendere il computer, quindi inserire il DVD di Windows Server Essentials nell'unità DVD.  
  
    Se si esegue un'installazione automatica, connettere il supporto rimovibile (ad esempio, un floppy disk o un'unità flash USB) che contiene i file di risposte. In base al contenuto del file di risposte, è possibile che le schermate di installazione non vengano visualizzate o ne venga visualizzata solo una parte.  
  
3. Riavviare il computer. Quando viene visualizzato il messaggio **Premere un tasto per avviare da CD o DVD**, premere un qualsiasi tasto.  
  
   > [!NOTE]
   >  Se il computer non è in grado di avviarsi da DVD, verificare che l'unità CD-ROM sia elencata per prima nella sequenza di avvio del BIOS. Per ulteriori informazioni sulla sequenza di avvio del BIOS, consultare la documentazione fornita dal produttore del computer.  
  
4. Selezionare i valori desiderati per **Lingua**, **Formato ora e valuta** e **Layout di tastiera o metodo di input**, quindi fare clic su **Avanti**.  
  
5. Fare clic su **Installa ora**.  
  
6. In **Immettere il codice "Product Key"** , digitare il codice Product Key.  
  
7. Leggere le **Condizioni di licenza**. Se le si accetta, selezionare la casella di controllo **Accetto le condizioni di licenza**, quindi fare clic su **Avanti**.  
  
   > [!NOTE]
   >  Se si sceglie di non accettarle, l'installazione viene interrotta.  
  
8. In **Scegliere il tipo di installazione da eseguire**, fare clic su **Personalizzata: installa solo Windows (opzione avanzata)**  
  
9. In **Specificare il percorso in cui installare Windows**, selezionare l'unità disco rigido su cui installare il sistema operativo Windows. Verificare che tutte le unità disco rigido interne siano disponibili per l'installazione.  
  
    > [!IMPORTANT]
    >   Windows Server Essentials deve essere installato come volume C: e la dimensione del volume deve essere almeno di 60 GB. È consigliabile creare due partizioni sul disco del sistema operativo ed è preferibile non utilizzare la partizione di sistema C: per archiviare i dati aziendali.  
  
    > [!NOTE]
    >  Se un'unità disco rigido non compare nell'elenco (ad esempio, un'unità disco rigido SATA), è necessario caricare i driver del dispositivo per quel disco rigido. Richiedere il driver del dispositivo al produttore e salvarlo su un dispositivo mobile (ad esempio, un floppy disk o un'unità flash USB). Collegare il dispositivo mobile al computer, quindi fare clic su **Carica driver**.  
  
     Se è necessario eliminare e/o creare delle partizioni, vedere le seguenti procedure:  
  
    1.  Per eliminare una partizione, selezionarla, quindi fare clic su **Opzioni unità (avanzate)** e infine su **Elimina**. Dopo aver eliminato la partizione di sistema, crearne una nuova seguendo le istruzioni nel passaggio **b** o **c**.  
  
        > [!NOTE]
        >  Dopo aver fatto clic su **Opzioni unità (avanzate)** , quella opzione non verrà più visualizzata. In questo caso, ignorare la parte del passaggio che riguarda le opzioni dell'unità.  
  
    2.  Per creare una partizione da uno spazio non partizionato, fare clic sul disco rigido che si desidera partizionare, selezionare **Opzioni unità (avanzate)** , quindi **Nuovo** e infine, nella casella di testo **Dimensione**, digitare la dimensione per la partizione che si desidera creare. Ad esempio, per utilizzare il valore consigliato per la partizione pari a 120 gigabyte (GB), digitare **122880**, quindi fare clic su **Applica**. Dopo aver creato la partizione, fare clic su **Avanti**. La partizione viene formattata prima che l'installazione prosegua.  
  
    3.  Per creare una partizione che utilizza tutto lo spazio non partizionato, fare clic sul disco rigido che si desidera partizionare, selezionare **Opzioni unità (avanzate)** , quindi fare clic su **Nuovo** e infine su **Applica** per accettare la dimensione predefinita. Dopo aver creato la partizione, fare clic su **Avanti**. La partizione viene formattata prima che l'installazione prosegua.  
  
        > [!IMPORTANT]
        >  Non è possibile spostare il sistema operativo su un'altra partizione dopo aver completato questo passaggio.  
  
   Durante l'installazione, i file temporanei vengono copiati in una cartella di installazione sul computer e l'operazione può richiedere circa 30 minuti. Dopo l'installazione del sistema operativo Windows Server Essentials, il computer viene riavviato. A questo punto è possibile configurare il sistema operativo Windows Server Essentials.  
  
###  <a name="step-2-configure-the-windows-server-essentials-operating-system"></a><a name="BKMK_Step2Configure"></a>Passaggio 2: configurare il sistema operativo Windows Server Essentials  
  
> [!IMPORTANT]
>  Se si esegue la migrazione da una versione precedente di Windows Small Business Server a Windows Server Essentials, è necessario seguire un processo diverso. Per informazioni sulle installazioni con migrazione, vedere:  
> 
> - [Eseguire la migrazione da Windows SBS 2003](../migrate/Migrate-Windows-Small-Business-Server-2003-to-Windows-Server-Essentials.md)  
>   -   [Eseguire la migrazione da Windows SBS 2008](../migrate/Migrate-Windows-Small-Business-Server-2008-to-Windows-Server-Essentials.md)  
  
 Durante questa fase dell'installazione, l'utente viene invitato a rispondere ad alcune domande sulla sua organizzazione. Queste informazioni vengono utilizzate per configurare il sistema operativo.  
  
> [!IMPORTANT]
>  Prima di iniziare questo passaggio, verificare che la scheda di rete locale sia connessa a un router o a uno switch acceso e funzionante.  
  
 **Tempo previsto di completamento:** circa 30 minuti  
  
##### <a name="to-configure-the-operating-system"></a>Per configurare il sistema operativo  
  
1.  Nella pagina **Verificare le impostazioni di data e ora**, fare clic su **Modifica le impostazioni di data e ora del sistema** per selezionare le impostazioni di data, ora e fuso orario per il server. Al termine dell'operazione scegliere **Avanti**.  
  
    > [!IMPORTANT]
    >  Se si installa Windows Server Essentials in una macchina virtuale, assicurarsi di scegliere le stesse impostazioni di fuso orario utilizzate dal sistema operativo host. Se le impostazioni del fuso orario sono diverse, l'installazione del server potrebbe non avere esito positivo.  
  
2.  Nella pagina **Scegli modalità di installazione server**, effettuare una delle seguenti operazioni:  
  
    1.  Scegliere **Pulisci installazione** per configurare una nuova installazione completa del software server di Windows Server Essentials.  
  
    2.  Scegliere **migrazione del server** per installare Windows Server Essentials e aggiungere il server a un dominio Windows esistente.  
  
3.  Nella pagina **Personalizza il server in uso**, immettere il nome dell'organizzazione, un nome di dominio interno e il nome del server.  
  
    > [!IMPORTANT]
    >  Il nome del server deve essere univoco sulla rete. Una volta completato questo passaggio, non sarà possibile modificare il nome del server o il nome del dominio interno.  
  
4.  Fare clic su **Avanti**.  
  
5.  Nella pagina **Fornire le informazioni sull'account amministratore**, digitare le informazioni per un nuovo account amministratore.  
  
    > [!CAUTION]
    >  Non denominare l'amministratore dell'account amministratore di rete o l'amministratore di rete. perché questi nomi sono riservati per il sistema.  
  
6.  Nella pagina **Fornire le informazioni sull'account utente standard**, digitare le informazioni per un nuovo account utente standard, quindi fare clic su **Avanti**.  
  
7.  Nella pagina **Mantieni automaticamente aggiornato il server in uso**, selezionare la modalità desiderata per il ricevimento degli aggiornamenti di Windows per il server, quindi fare clic su **Avanti**.  
  
8.  Nella pagina **Aggiornamento e preparazione del server in corso** viene visualizzato l'avanzamento del processo di installazione. Questa operazione richiede un certo tempo e il computer verrà riavviato un paio di volte.  
  
9. Dopo l'ultimo riavvio del server, verrà visualizzata la finestra **È ora possibile utilizzare il server**. Fare clic su **Chiudi**.  
  
10. Fare clic sul riquadro Dashboard nella schermata **Start**, quindi su Dashboard, completare le attività **Configura server** nella pagina **Home**. È necessario completare queste attività subito dopo il completamento dell'installazione di Windows Server Essentials.  
  
> [!NOTE]
>  Una volta completata l'installazione, l'utente accede automaticamente al server con il nuovo account amministratore aggiunto durante l'installazione. La password dell'account Administrator predefinito è uguale alla password del nuovo account amministratore e l'account Administrator predefinito viene disabilitato.  
  
 È possibile utilizzare il server appena installato per un intervallo di tempo limitato (noto come periodo di valutazione) senza immettere un codice Product Key. Una volta terminato il periodo di valutazione, è necessario immettere un codice Product Key per attivare il server oppure prolungare il periodo di valutazione. Il periodo di valutazione può essere prolungato al massimo per due volte. Quando si raggiunge il numero massimo di giorni consentiti per il periodo di valutazione, è necessario attivare il server con un codice Product Key.  
  
### <a name="customize-windows-server-essentials"></a>Personalizzare Windows Server Essentials  
 La **Home** page del dashboard di Windows Server Essentials è collegata alle attività di **configurazione** che è necessario completare immediatamente dopo l'installazione del server. Eseguendo queste attività, è possibile proteggere le informazioni archiviate nel server e abilitare le funzionalità disponibili in Windows Server Essentials.  
  
 Se l'utente sceglie di non effettuare queste attività, potrebbe rischiare di non accedere ad alcune funzionalità di rete. Per tornare a queste attività in un secondo momento, tornare alla **Home** page del dashboard di Windows Server Essentials.  
  
 La seguente tabella definisce gli elementi che possono essere inclusi nell'elenco delle attività di configurazione.  
  
|Attività|Descrizione
|----------|-----------------|  
|Ottenere aggiornamenti per altri prodotti Microsoft|Fare clic su questa attività per accedere a un collegamento che esegue uno strumento che consente di specificare se si desidera utilizzare Microsoft Update per ottenere automaticamente gli aggiornamenti per Windows Server Essentials e altri prodotti Microsoft, ad esempio Office.  
|Aggiungere gli account utente|Fare clic su questa attività per visualizzare brevi istruzioni su come aggiungere gli account utente. Viene anche fornito un collegamento per eseguire la procedura guidata**Aggiungi account utente**. Per altre informazioni, vedere [Aggiungere un account utente](../manage/Manage-User-Accounts-in-Windows-Server-Essentials.md#BKMK_Manage1).  
|Aggiunta di cartelle al server|Fare clic su questa attività per visualizzare brevi istruzioni su come aggiungere cartelle al server. Viene fornito un collegamento per eseguire la procedura guidata **Aggiungi cartella**. Viene anche fornito un collegamento a un argomento online sull'utilizzo delle cartelle del server. Per altre informazioni, vedere [Aggiungere o spostare una cartella del server](../manage/Manage-Server-Folders-in-Windows-Server-Essentials.md#BKMK_5). 
|Impostazione del backup del server|Fare su questa attività per visualizzare brevi istruzioni sull'utilizzo del backup del server per salvaguardare i propri dati. Viene fornito un collegamento per eseguire la procedura guidata **Configura backup server**. Per altre informazioni, vedere [Configurare o personalizzare un backup del server](../manage/Manage-Server-Backup-in-Windows-Server-Essentials.md#BKMK_1). 
|Configurare Accesso remoto via Internet|Fare clic su questa attività per visualizzare brevi informazioni sulla funzionalità accesso remoto via Internet in Windows Server Essentials. Viene fornito una collegamento alla pagina **Impostazioni di Accesso remoto via Internet**. Per altre informazioni, vedere [gestire l'accesso remoto via Internet](../manage/Manage-Anywhere-Access-in-Windows-Server-Essentials.md). 
|Impostazione della notifica degli avvisi di posta elettronica|Fare clic su questa attività per visualizzare brevi istruzioni sulla notifica degli avvisi di posta elettronica. Viene fornito un collegamento per eseguire la procedura guidata **Configura notifiche di posta elettronica per gli avvisi**. Per altre informazioni, vedere [Configurare le notifiche di posta elettronica per gli avvisi](../manage/Manage-System-Health-in-Windows-Server-Essentials.md#BKMK_Email).  
|Configurazione del server dei contenuti multimediali|Fare clic su questa attività per visualizzare brevi istruzioni su come utilizzare il server dei contenuti multimediali per condividere musica, video e immagini. Viene fornito un collegamento alla pagina  **Impostazioni multimediali** . Viene anche fornito un collegamento a un argomento online per approfondire la conoscenza del server dei contenuti multimediali. Per altre informazioni, vedere [gestire i supporti digitali](../manage/Manage-Digital-Media-in-Windows-Server-Essentials.md). 
|Connessione dei computer|Fare clic su questa attività per visualizzare brevi istruzioni su come connettere un computer di rete al server. Per ulteriori informazioni, vedere [Come connettere i computer al server](../use/Get-Connected-in-Windows-Server-Essentials.md#BKMK_9)
  
## <a name="see-also"></a>Vedere anche  
  
-   [Installare Windows Server Essentials](Install-Windows-Server-Essentials.md)

