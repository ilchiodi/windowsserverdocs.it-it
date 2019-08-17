---
title: Configurazione di account di cluster in Active Directory
ms.date: 11/12/2012
ms.prod: windows-server-threshold
ms.technology: storage-failover-clustering
author: JasonGerend
manager: elizapo
ms.author: jgerend
ms.openlocfilehash: 3cc7449c8fbbad2ed4a3cd27513fcbe74b617e36
ms.sourcegitcommit: 23a6e83b688119c9357262b6815c9402c2965472
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/16/2019
ms.locfileid: "69560519"
---
# <a name="configuring-cluster-accounts-in-active-directory"></a>Configurazione di account di cluster in Active Directory


Si applica a: Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2 e Windows Server 2008

In Windows Server, quando si crea un cluster di failover e si configurano le applicazioni o i servizi del cluster, le procedure guidate del cluster di failover creano gli account computer Active Directory necessari, detti anche oggetti computer, e forniscono loro autorizzazioni specifiche. Le procedure guidate creano un account computer per il cluster stesso (questo account è denominato anche oggetto nome cluster o oggetto nome cluster) e un account computer per la maggior parte dei tipi di servizi e applicazioni in cluster, l'eccezione è una macchina virtuale Hyper-V. Le autorizzazioni per questi account vengono impostate automaticamente dalle procedure guidate del cluster di failover. Se le autorizzazioni vengono modificate, sarà necessario modificarle di nuovo in base ai requisiti del cluster. Questa guida descrive gli account e le autorizzazioni Active Directory, fornisce informazioni di base sui motivi per cui sono importanti e descrive i passaggi per la configurazione e la gestione degli account.
      

## <a name="overview-of-active-directory-accounts-needed-by-a-failover-cluster"></a>Panoramica degli account di Active Directory necessari per un cluster di failover

In questa sezione vengono descritti gli account computer Active Directory, detti anche Active Directory oggetti computer, importanti per un cluster di failover. Questi account sono i seguenti:

  - **Account utente usato per creare il cluster.** Si tratta dell'account utente utilizzato per avviare la creazione guidata cluster. L'account è importante perché fornisce la base da cui viene creato un account computer per il cluster stesso.  
      
  - **Account del nome del cluster.** (l'account computer del cluster stesso, denominato anche oggetto nome cluster o oggetto nome cluster). Questo account viene creato automaticamente dalla creazione guidata cluster e ha lo stesso nome del cluster. L'account del nome del cluster è molto importante, perché tramite questo account vengono creati automaticamente altri account durante la configurazione di nuovi servizi e applicazioni nel cluster. Se l'account del nome del cluster viene eliminato o le autorizzazioni vengono sottratte, non è possibile creare altri account come richiesto dal cluster, fino a quando non viene ripristinato l'account del nome del cluster o quando vengono ripristinate le autorizzazioni corrette.  
      
    Ad esempio, se si crea un cluster denominato cluster1 e quindi si prova a configurare un server di stampa in cluster denominato PrintServer1 nel cluster, l'account CLUSTER1 in Active Directory dovrà mantenere le autorizzazioni corrette in modo che possa essere usato per creare un computer account denominato PrintServer1.  
      
    L'account del nome del cluster viene creato nel contenitore predefinito per gli account computer in Active Directory. Per impostazione predefinita, si tratta del contenitore "computer", ma l'amministratore di dominio può scegliere di reindirizzarlo a un altro contenitore o unità organizzativa (OU).  
      
  - **Account computer (oggetto computer) di un servizio o un'applicazione in cluster.** Questi account vengono creati automaticamente dalla configurazione guidata disponibilità elevata come parte del processo di creazione della maggior parte dei tipi di servizi o applicazioni in cluster, l'eccezione è una macchina virtuale Hyper-V. All'account del nome del cluster vengono concesse le autorizzazioni necessarie per controllare questi account.  
      
    Se, ad esempio, si dispone di un cluster denominato cluster1 e si crea un file server cluster denominato FileServer1, la configurazione guidata disponibilità elevata creerà un account computer Active Directory denominato FileServer1. La configurazione guidata disponibilità elevata fornisce anche all'account CLUSTER1 le autorizzazioni necessarie per controllare l'account FileServer1.  
      

Nella tabella seguente vengono descritte le autorizzazioni necessarie per questi account.


<table>
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<thead>
<tr class="header">
<th>Account</th>
<th>Dettagli sulle autorizzazioni</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>Account usato per creare il cluster</p></td>
<td><p>Richiede autorizzazioni amministrative per i server che diventeranno nodi del cluster. Sono inoltre necessarie le autorizzazioni <strong>Crea oggetti computer</strong> e <strong>Leggi tutte le proprietà</strong> nel contenitore usato per gli account computer nel dominio.</p></td>
</tr>
<tr class="even">
<td><p>Account nome cluster (account computer del cluster stesso)</p></td>
<td><p>Quando si esegue la creazione guidata cluster, viene creato l'account del nome del cluster nel contenitore predefinito usato per gli account computer nel dominio. Per impostazione predefinita, l'account del nome del cluster (ad esempio altri account computer) può creare fino a dieci account computer nel dominio.</p>
<p>Se si crea l'account del nome del cluster (oggetto nome cluster) prima di creare il cluster, ovvero pre-installare l'account, è necessario assegnargli le autorizzazioni <strong>Crea oggetti computer</strong> e <strong>Leggi tutte le proprietà</strong> nel contenitore usato per il computer account nel dominio. È anche necessario disabilitare l'account e assegnargli il <strong>controllo completo</strong> all'account che verrà usato dall'amministratore che installa il cluster. Per ulteriori informazioni, vedere <a href="#steps-for-prestaging-the-cluster-name-account" data-raw-source="[Steps for prestaging the cluster name account](#steps-for-prestaging-the-cluster-name-account)">passaggi per la pre-installazione dell'account del nome cluster</a>, più avanti in questa guida.</p></td>
</tr>
<tr class="odd">
<td><p>Account computer di un servizio o un'applicazione in cluster</p></td>
<td><p>Quando viene eseguita la configurazione guidata disponibilità elevata (per creare un nuovo servizio o applicazione in cluster), nella maggior parte dei casi viene creato un account computer per il servizio o l'applicazione in cluster in Active Directory. All'account del nome del cluster vengono concesse le autorizzazioni necessarie per controllare l'account. L'eccezione è una macchina virtuale Hyper-V in cluster: non viene creato alcun account computer.</p>
<p>Se si pre-installa l'account computer per un servizio o un'applicazione in cluster, è necessario configurarlo con le autorizzazioni necessarie. Per ulteriori informazioni, vedere la pagina <a href="#steps-for-prestaging-an-account-for-a-clustered-service-or-application" data-raw-source="[Steps for prestaging an account for a clustered service or application](#steps-for-prestaging-an-account-for-a-clustered-service-or-application)">relativa alla procedura di pre-installazione di un account per un servizio o un'applicazione in cluster</a>, più avanti in questa guida.</p></td>
</tr>
</tbody>
</table>


> [!NOTE]
> Nelle versioni precedenti di Windows Server era disponibile un account per la Servizio cluster. Poiché Windows Server 2008, tuttavia, il Servizio cluster viene eseguito automaticamente in un contesto speciale che fornisce le autorizzazioni e i privilegi specifici necessari per il servizio, in modo analogo al contesto del sistema locale, ma con privilegi ridotti. Gli altri account sono tuttavia necessari, come descritto in questa guida. 
<br>


### <a name="how-accounts-are-created-through-wizards-in-failover-clustering"></a>Modalità di creazione degli account tramite procedure guidate in clustering di failover

Nel diagramma seguente vengono illustrati l'utilizzo e la creazione di account computer (oggetti Active Directory) descritti nella sottosezione precedente. Questi account entrano in gioco quando un amministratore esegue la creazione guidata cluster e quindi esegue la configurazione guidata disponibilità elevata (per configurare un servizio o un'applicazione in cluster).

![](media/configure-ad-accounts/Cc731002.e8a7686c-9ba8-4ddf-87b1-175b7b51f65d(WS.10).gif)

Si noti che il diagramma precedente mostra un singolo amministratore che esegue la creazione guidata cluster e la configurazione guidata disponibilità elevata. Tuttavia, potrebbe trattarsi di due amministratori diversi con due account utente diversi, se entrambi gli account dispongono di autorizzazioni sufficienti. Le autorizzazioni sono descritte in modo più dettagliato in requisiti correlati a cluster di failover, domini Active Directory e account, più avanti in questa guida.

### <a name="how-problems-can-result-if-accounts-needed-by-the-cluster-are-changed"></a>Come possono verificarsi problemi se gli account necessari per il cluster sono stati modificati

Il diagramma seguente illustra il modo in cui i problemi possono verificarsi se l'account del nome del cluster (uno degli account richiesti dal cluster) viene modificato dopo che è stato creato automaticamente dalla creazione guidata cluster.

![](media/configure-ad-accounts/Cc731002.beecc4f7-049c-4945-8fad-2cceafd6a4a5(WS.10).gif)

Se il tipo di problema illustrato nel diagramma si verifica, viene registrato un determinato evento (1193, 1194, 1206 o 1207) in Visualizzatore eventi. Per ulteriori informazioni su questi eventi, vedere [http://go.microsoft.com/fwlink/?LinkId=118271](http://go.microsoft.com/fwlink/?linkid=118271).

Si noti che un problema simile alla creazione di un account per un servizio o un'applicazione in cluster può verificarsi se è stata raggiunta la quota a livello di dominio per la creazione di oggetti computer (per impostazione predefinita, 10). In caso affermativo, potrebbe essere opportuno rivolgersi all'amministratore di dominio per aumentare la quota, anche se si tratta di un'impostazione a livello di dominio e deve essere modificata solo dopo un'attenta considerazione e solo dopo la conferma che il diagramma precedente non descrivere la situazione. Per ulteriori informazioni, vedere la [sezione relativa ai passaggi per la risoluzione dei problemi causati da modifiche negli account di Active Directory correlati ai cluster](#steps-for-troubleshooting-problems-caused-by-changes-in-cluster-related-active-directory-accounts), più avanti in questa guida.

## <a name="requirements-related-to-failover-clusters-active-directory-domains-and-accounts"></a>Requisiti correlati a cluster di failover, domini Active Directory e account

Come descritto nelle tre sezioni precedenti, è necessario soddisfare alcuni requisiti prima che i servizi e le applicazioni del cluster possano essere configurati correttamente in un cluster di failover. I requisiti più semplici riguardano la posizione dei nodi del cluster (all'interno di un singolo dominio) e il livello di autorizzazioni dell'account della persona che installa il cluster. Se vengono soddisfatti questi requisiti, gli altri account richiesti dal cluster possono essere creati automaticamente dalle procedure guidate del cluster di failover. Nell'elenco seguente vengono fornite informazioni dettagliate su questi requisiti di base.

  - **Nodi** Tutti i nodi devono trovarsi nello stesso dominio Active Directory. Il dominio non può essere basato su Windows NT 4,0, che non include Active Directory.  
      
  - **Account della persona che installa il cluster:** La persona che installa il cluster deve usare un account con le caratteristiche seguenti:  
      
      - L'account deve essere un account di dominio. Non è necessario che sia un account amministratore di dominio. Può essere un account utente di dominio se soddisfa gli altri requisiti in questo elenco.  
          
      - L'account deve disporre di autorizzazioni amministrative per i server che diventeranno nodi del cluster. Il modo più semplice per fornire questo problema consiste nel creare un account utente di dominio, quindi aggiungere tale account al gruppo Administrators locale in ognuno dei server che diventeranno nodi del cluster. Per ulteriori informazioni, vedere la pagina relativa alla [procedura per la configurazione dell'account per la persona che installa il cluster](#steps-for-configuring-the-account-for-the-person-who-installs-the-cluster), più avanti in questa guida.  
          
      - All'account o al gruppo di cui l'account è membro è necessario assegnare le autorizzazioni **Crea oggetti computer** e **Leggi tutte le proprietà** nel contenitore utilizzato per gli account computer nel dominio. Un'altra alternativa consiste nel rendere l'account un account amministratore di dominio. Per ulteriori informazioni, vedere la pagina relativa alla [procedura per la configurazione dell'account per la persona che installa il cluster](#steps-for-configuring-the-account-for-the-person-who-installs-the-cluster), più avanti in questa guida.  
          
      - Se l'organizzazione sceglie di pre-installare l'account del nome del cluster (un account computer con lo stesso nome del cluster), l'account del nome del cluster pre-installato deve concedere l'autorizzazione controllo completo all'account della persona che installa il cluster. Per altri dettagli importanti su come pre-installare l'account del nome del cluster, vedere [passaggi per la pre-installazione dell'account del nome cluster](#steps-for-prestaging-the-cluster-name-account), più avanti in questa guida.  
          

### <a name="planning-ahead-for-password-resets-and-other-account-maintenance"></a>Pianificazione in anticipo per la reimpostazione della password e altre operazioni di manutenzione dell'account

Gli amministratori dei cluster di failover potrebbero talvolta dover reimpostare la password dell'account del nome del cluster. Questa azione richiede un'autorizzazione specifica, ovvero l'autorizzazione **Reimposta password** . È pertanto consigliabile modificare le autorizzazioni dell'account nome cluster usando lo snap-in utenti e computer Active Directory per concedere agli amministratori del cluster l'autorizzazione **Reimposta password** per l'account del nome del cluster. Per ulteriori informazioni, vedere [la sezione relativa ai passaggi per la risoluzione dei problemi relativi alle password con l'account nome cluster](#steps-for-troubleshooting-password-problems-with-the-cluster-name-account), più avanti in questa guida.

## <a name="steps-for-configuring-the-account-for-the-person-who-installs-the-cluster"></a>Passaggi per la configurazione dell'account per la persona che installa il cluster

L'account della persona che installa il cluster è importante perché fornisce la base da cui viene creato un account computer per il cluster stesso.

L'appartenenza al gruppo minima richiesta per completare la procedura seguente varia a seconda che si crei l'account di dominio e si assegni le autorizzazioni necessarie nel dominio o che si inserisca solo l'account (creato da un altro utente) nel gruppo **Administrators** locale sui server che saranno nodi del cluster di failover. Se il primo, l'appartenenza al gruppo **account Operators** o **Domain Admins**o a un gruppo equivalente è il requisito minimo necessario per completare questa procedura. Se il secondo, l'appartenenza al gruppo **Administrators** locale sui server che saranno nodi nel cluster di failover, o equivalente, è tutto ciò che è necessario. Esaminare i dettagli relativi all'utilizzo degli account appropriati e delle appartenenze ai gruppi all'indirizzo [http://go.microsoft.com/fwlink/?LinkId=83477](http://go.microsoft.com/fwlink/?linkid=83477).

#### <a name="to-configure-the-account-for-the-person-who-installs-the-cluster"></a>Per configurare l'account per la persona che installa il cluster

1.  Creare o ottenere un account di dominio per la persona che installa il cluster. Questo account può essere un account utente di dominio o un account amministratore di dominio (in **Domain Admins** o un gruppo equivalente).

2.  Se l'account creato o ottenuto nel passaggio 1 è un account utente di dominio o se gli account di amministratore di dominio nel dominio non vengono inclusi automaticamente nel gruppo **Administrators** locale nei computer del dominio, aggiungere l'account al locale  **Gruppo Administrators** sui server che saranno nodi del cluster di failover:
    
    1.  Fare clic sul pulsante **Start**, scegliere **Strumenti di amministrazione** e quindi **Server Manager**.  
          
    2.  Nell'albero della console espandere **configurazione**, espandere **utenti e gruppi locali**, quindi espandere **gruppi**.  
          
    3.  Nel riquadro centrale fare clic con il pulsante destro del mouse su **Administrators**, scegliere **Aggiungi al gruppo**e quindi fare clic su **Aggiungi**.  
          
    4.  In **immettere i nomi degli oggetti da selezionare**Digitare il nome dell'account utente creato o ottenuto nel passaggio 1. Se richiesto, immettere un nome account e una password con autorizzazioni sufficienti per questa azione. Fare quindi clic su **OK**.  
          
    5.  Ripetere questi passaggi in ogni server che sarà un nodo nel cluster di failover.  
          
    

> [!IMPORTANT]
> Questi passaggi devono essere ripetuti in tutti i server che saranno nodi del cluster. 
<br>


3. Se l'account creato o ottenuto nel passaggio 1 è un account di amministratore di dominio, ignorare il resto di questa procedura. In caso contrario, assegnare all'account le autorizzazioni **Crea oggetti computer** e **Leggi tutte le proprietà** nel contenitore usato per gli account computer nel dominio:
    
   1.  In un controller di dominio fare clic sul pulsante **Start**, scegliere **strumenti di amministrazione**e quindi fare clic su **Active Directory utenti e computer**. Se viene visualizzata la finestra di dialogo **Controllo account utente**, confermare che l'azione visualizzata è quella desiderata e scegliere **Continua**.  
          
   2.  Nel menu **Visualizza** verificare che sia selezionata l'opzione **funzionalità avanzate** .  
          
       Quando si seleziona **funzionalità avanzate** , è possibile visualizzare la scheda **sicurezza** nelle proprietà degli account (oggetti) in **Active Directory utenti e computer**.  
          
   3.  Fare clic con il pulsante destro del mouse sul contenitore **computer** predefinito o sul contenitore predefinito in cui vengono creati gli account computer nel dominio, quindi fare clic su **Proprietà**. I **computer** si trovano in <b>Active Directory utenti e computer/</b><i>nodo di dominio</i><b>/Computers</b>.  
          
   4.  Nella scheda **Sicurezza** fare clic su **Avanzate**.  
          
   5.  Fare clic su **Aggiungi**, digitare il nome dell'account creato o ottenuto nel passaggio 1, quindi fare clic su **OK**.  
          
   6.  Nella finestra di dialogo **voce di autorizzazione per il contenitore * * ** individuare le autorizzazioni **Crea oggetti computer** e **Leggi tutte le proprietà** e assicurarsi che la casella di controllo **Consenti** sia selezionata per ciascuna di esse.  
          
       ![](media/configure-ad-accounts/Cc731002.0a863ac5-2024-4f9f-8a4d-a419aff32fa0(WS.10).gif)

## <a name="steps-for-prestaging-the-cluster-name-account"></a>Passaggi per la configurazione pre-installazione dell'account del nome cluster

È in genere più semplice se non si esegue la pre-installazione dell'account del nome cluster, ma si consente invece di creare e configurare automaticamente l'account quando si esegue la creazione guidata cluster. Tuttavia, se è necessario pre-installare l'account del nome del cluster a causa dei requisiti dell'organizzazione, attenersi alla procedura riportata di seguito.

L'appartenenza al gruppo **Domain Admins** oppure a un gruppo equivalente è il requisito minimo per completare la procedura. Esaminare i dettagli relativi all'utilizzo degli account appropriati e delle appartenenze ai gruppi all'indirizzo [http://go.microsoft.com/fwlink/?LinkId=83477](http://go.microsoft.com/fwlink/?linkid=83477). Si noti che è possibile usare lo stesso account per questa procedura, che verrà usato durante la creazione del cluster.

#### <a name="to-prestage-a-cluster-name-account"></a>Per pre-installare un account nome cluster

1.  Assicurarsi di conoscere il nome che avrà il cluster e il nome dell'account utente che verrà usato dalla persona che ha creato il cluster. Si noti che è possibile utilizzare tale account per eseguire questa procedura.

2.  In un controller di dominio fare clic sul pulsante **Start**, scegliere **strumenti di amministrazione**e quindi fare clic su **Active Directory utenti e computer**. Se viene visualizzata la finestra di dialogo **Controllo account utente**, confermare che l'azione visualizzata è quella desiderata e scegliere **Continua**.

3.  Nell'albero della console fare clic con il pulsante destro del mouse su **computer** o sul contenitore predefinito in cui vengono creati gli account computer nel dominio. I **computer** si trovano in <b>Active Directory utenti e computer/</b><i>nodo di dominio</i><b>/Computers</b>.

4.  Fare clic su **nuovo** e quindi su **computer**.

5.  Digitare il nome che verrà utilizzato per il cluster di failover, ovvero il nome del cluster che verrà specificato nella creazione guidata cluster, quindi fare clic su **OK**.

6.  Fare clic con il pulsante destro del mouse sull'account appena creato, quindi scegliere **Disabilita account**. Se viene richiesto di confermare la scelta, fare clic su **Sì**.
    
    È necessario disabilitare l'account in modo che, quando viene eseguita la creazione guidata **cluster** , possa verificare che l'account che verrà utilizzato per il cluster non sia attualmente utilizzato da un computer o un cluster esistente nel dominio.

7.  Nel menu **Visualizza** verificare che sia selezionata l'opzione **funzionalità avanzate** .
    
    Quando si seleziona **funzionalità avanzate** , è possibile visualizzare la scheda **sicurezza** nelle proprietà degli account (oggetti) in **Active Directory utenti e computer**.

8.  Fare clic con il pulsante destro del mouse sulla cartella a cui si è fatto clic con il pulsante destro del mouse nel passaggio 3, quindi scegliere **Proprietà**.

9.  Nella scheda **Sicurezza** fare clic su **Avanzate**.

10. Fare clic su **Aggiungi**, fare clic su **tipi di oggetto** e verificare che **computer** sia selezionato, quindi fare clic su **OK**. Quindi, in **immettere il nome dell'oggetto da selezionare**, digitare il nome dell'account computer appena creato e quindi fare clic su **OK**. Se viene visualizzato un messaggio che informa che sta per essere aggiunto un oggetto disabilitato, fare clic su **OK**.

11. Nella finestra di dialogo **voce autorizzazione** individuare le autorizzazioni **Crea oggetti computer** e **Leggi tutte le proprietà** e assicurarsi che la casella di controllo **Consenti** sia selezionata per ciascuna di esse.
    
    ![](media/configure-ad-accounts/Cc731002.f5977c4d-a62e-4b17-81e3-8c19ddca2078(WS.10).gif)

12. Fare clic su **OK** fino a tornare allo snap-in **Active Directory utenti e computer** .

13. Se si usa lo stesso account per eseguire questa procedura come verrà usato per creare il cluster, ignorare i passaggi rimanenti. In caso contrario, è necessario configurare le autorizzazioni in modo che l'account utente che verrà usato per creare il cluster abbia il controllo completo dell'account computer appena creato:
    
    1.  Nel menu **Visualizza** verificare che sia selezionata l'opzione **funzionalità avanzate** .  
          
    2.  Fare clic con il pulsante destro del mouse sull'account computer appena creato e quindi scegliere **Proprietà**.  
          
    3.  Nella scheda **Sicurezza** fare clic su **Aggiungi**. Se viene visualizzata la finestra di dialogo **Controllo account utente**, confermare che l'azione visualizzata è quella desiderata e scegliere **Continua**.  
          
    4.  Utilizzare la finestra di dialogo **Seleziona utenti, computer o gruppi** per specificare l'account utente che verrà utilizzato durante la creazione del cluster. Fare quindi clic su **OK**.  
          
    5.  Assicurarsi che l'account utente appena aggiunto sia selezionato, quindi selezionare la casella di controllo **Consenti** accanto a **controllo completo**.  
          
        ![](media/configure-ad-accounts/Cc731002.fffaafe2-a494-498b-974c-8f9d70f7103b(WS.10).gif)

## <a name="steps-for-prestaging-an-account-for-a-clustered-service-or-application"></a>Passaggi per la configurazione pre-installazione di un account per un servizio o un'applicazione in cluster

È in genere più semplice se non si esegue la pre-installazione dell'account computer per un servizio o un'applicazione in cluster, ma si consente invece di creare e configurare automaticamente l'account quando si esegue la configurazione guidata disponibilità elevata. Tuttavia, se è necessario pre-installare gli account a causa dei requisiti dell'organizzazione, attenersi alla procedura riportata di seguito.

Per completare questa procedura, è necessaria almeno l'appartenenza al gruppo **account Operators** o a un gruppo equivalente. Esaminare i dettagli relativi all'utilizzo degli account appropriati e delle appartenenze ai gruppi all'indirizzo [http://go.microsoft.com/fwlink/?LinkId=83477](http://go.microsoft.com/fwlink/?linkid=83477).

#### <a name="to-prestage-an-account-for-a-clustered-service-or-application"></a>Per pre-installare un account per un servizio o un'applicazione in cluster

1.  Assicurarsi di aver conosciuto il nome del cluster e il nome del servizio o dell'applicazione in cluster.

2.  In un controller di dominio fare clic sul pulsante **Start**, scegliere **strumenti di amministrazione**e quindi fare clic su **Active Directory utenti e computer**. Se viene visualizzata la finestra di dialogo **Controllo account utente**, confermare che l'azione visualizzata è quella desiderata e scegliere **Continua**.

3.  Nell'albero della console fare clic con il pulsante destro del mouse su **computer** o sul contenitore predefinito in cui vengono creati gli account computer nel dominio. I **computer** si trovano in <b>Active Directory utenti e computer/</b><i>nodo di dominio</i><b>/Computers</b>.

4.  Fare clic su **nuovo** e quindi su **computer**.

5.  Digitare il nome che verrà utilizzato per il servizio o l'applicazione in cluster, quindi fare clic su **OK**.

6.  Nel menu **Visualizza** verificare che sia selezionata l'opzione **funzionalità avanzate** .
    
    Quando si seleziona **funzionalità avanzate** , è possibile visualizzare la scheda **sicurezza** nelle proprietà degli account (oggetti) in **Active Directory utenti e computer**.

7.  Fare clic con il pulsante destro del mouse sull'account computer appena creato e quindi scegliere **Proprietà**.

8.  Nella scheda **Sicurezza** fare clic su **Aggiungi**.

9.  Fare clic su **tipi di oggetto** e verificare che **computer** sia selezionato, quindi fare clic su **OK**. Quindi, in **immettere il nome dell'oggetto da selezionare**, digitare l'account del nome del cluster e quindi fare clic su **OK**. Se viene visualizzato un messaggio che informa che sta per essere aggiunto un oggetto disabilitato, fare clic su **OK**.

10. Verificare che l'account nome cluster sia selezionato, quindi selezionare la casella di controllo **Consenti** accanto a **controllo completo**.
    
    ![](media/configure-ad-accounts/Cc731002.2e614376-87a6-453a-81ba-90ff7ebc3fa2(WS.10).gif)

## <a name="steps-for-troubleshooting-problems-related-to-accounts-used-by-the-cluster"></a>Passaggi per la risoluzione dei problemi relativi agli account usati dal cluster

Come descritto in precedenza in questa guida, quando si crea un cluster di failover e si configurano le applicazioni o i servizi del cluster, le procedure guidate del cluster di failover creano gli account Active Directory necessari e assegnano loro le autorizzazioni corrette. Se viene eliminato un account necessario o se vengono modificate le autorizzazioni necessarie, possono verificarsi problemi. Nelle sottosezioni seguenti vengono illustrati i passaggi per la risoluzione di questi problemi.

  - [Passaggi per la risoluzione dei problemi relativi alle password con l'account del nome del cluster](#steps-for-troubleshooting-password-problems-with-the-cluster-name-account)
      
  - [Passaggi per la risoluzione dei problemi causati da modifiche negli account di Active Directory correlati al cluster](#steps-for-troubleshooting-problems-caused-by-changes-in-cluster-related-active-directory-accounts)
      

### <a name="steps-for-troubleshooting-password-problems-with-the-cluster-name-account"></a>Passaggi per la risoluzione dei problemi relativi alle password con l'account del nome del cluster

Utilizzare questa procedura se è presente un messaggio di evento sugli oggetti computer o sull'identità del cluster che include il testo seguente. Si noti che questo testo sarà incluso nel messaggio dell'evento, non all'inizio del messaggio di evento:

`Logon failure: unknown user name or bad password.`

I messaggi di evento che soddisfano la descrizione precedente indicano che la password per l'account del nome del cluster e la password corrispondente archiviata dal software di clustering non corrispondono più.

Per informazioni su come verificare che gli amministratori del cluster dispongano delle autorizzazioni corrette per eseguire la procedura seguente, in base alle esigenze, vedere Pianificazione in anticipo per la reimpostazione della password e altre operazioni di manutenzione degli account più indietro in questa guida.

Appartenenza al gruppo locale **amministratori** o gruppo equivalente, è il requisito minimo necessario per completare questa procedura. Inoltre, all'account deve essere assegnata l'autorizzazione **Reimposta password** per l'account del nome del cluster, a meno che l'account non sia un account **Domain Admins** o il proprietario dell'autore dell'account del nome del cluster. L'account usato dalla persona che ha installato il cluster può essere usato per questa procedura. Esaminare i dettagli relativi all'utilizzo degli account appropriati e delle appartenenze ai gruppi all'indirizzo [http://go.microsoft.com/fwlink/?LinkId=83477](http://go.microsoft.com/fwlink/?linkid=83477).

#### <a name="to-troubleshoot-password-problems-with-the-cluster-name-account"></a>Per risolvere i problemi relativi alle password con l'account nome cluster

1.  Per aprire lo snap-in cluster di failover, fare clic sul pulsante **Start**, scegliere **Strumenti di amministrazione**, quindi fare clic su **Gestione cluster di failover**. Se viene visualizzata la finestra di dialogo **controllo account utente** , verificare che l'azione visualizzata sia quella desiderata, quindi fare clic su **continua**.

2.  Nello snap-in Gestione cluster di failover, se il cluster che si desidera configurare non viene visualizzato, nell'albero della console fare clic con il pulsante destro del mouse su **Gestione cluster di failover**, fare clic su **Gestisci un cluster** e scegliere o specificare il cluster desiderato.

3.  Nel riquadro centrale espandere **risorse principali del cluster**.

4.  In **nome cluster**fare clic con il pulsante destro del mouse sull'elemento **nome** , scegliere **altre azioni**e quindi fare clic su **Ripristina Active Directory oggetto**.

### <a name="steps-for-troubleshooting-problems-caused-by-changes-in-cluster-related-active-directory-accounts"></a>Passaggi per la risoluzione dei problemi causati da modifiche negli account di Active Directory correlati al cluster

Se l'account del nome del cluster viene eliminato o se le autorizzazioni vengono sottratte, si verificheranno problemi quando si tenterà di configurare un nuovo servizio o un'applicazione in cluster. Per risolvere un problema che potrebbe essere causato, utilizzare lo snap-in Active Directory utenti e computer per visualizzare o modificare l'account del nome del cluster e altri account correlati. Per informazioni sugli eventi che vengono registrati quando si verifica questo tipo di problema (evento 1193, 1194, 1206 o 1207), vedere [http://go.microsoft.com/fwlink/?LinkId=118271](http://go.microsoft.com/fwlink/?linkid=118271).

L'appartenenza al gruppo **Domain Admins** oppure a un gruppo equivalente è il requisito minimo per completare la procedura. Esaminare i dettagli relativi all'utilizzo degli account appropriati e delle appartenenze ai gruppi all'indirizzo [http://go.microsoft.com/fwlink/?LinkId=83477](http://go.microsoft.com/fwlink/?linkid=83477).

#### <a name="to-troubleshoot-problems-caused-by-changes-in-cluster-related-active-directory-accounts"></a>Per risolvere i problemi causati da modifiche negli account di Active Directory correlati al cluster

1.  In un controller di dominio fare clic sul pulsante **Start**, scegliere **strumenti di amministrazione**e quindi fare clic su **Active Directory utenti e computer**. Se viene visualizzata la finestra di dialogo **Controllo account utente**, confermare che l'azione visualizzata è quella desiderata e scegliere **Continua**.

2.  Espandere il contenitore **computer** predefinito o la cartella in cui si trova l'account del nome del cluster (l'account computer per il cluster). I **computer** si trovano in <b>Active Directory utenti e computer/</b><i>nodo di dominio</i><b>/Computers</b>.

3.  Esaminare l'icona per l'account del nome del cluster. Non deve avere una freccia verso il basso, ovvero l'account non deve essere disabilitato. Se risulta disabilitato, fare clic con il pulsante destro del mouse su di esso e cercare l' **account Enable**. Se viene visualizzato il comando, fare clic su di esso.

4.  Nel menu **Visualizza** verificare che sia selezionata l'opzione **funzionalità avanzate** .
    
    Quando si seleziona **funzionalità avanzate** , è possibile visualizzare la scheda **sicurezza** nelle proprietà degli account (oggetti) in **Active Directory utenti e computer**.

5.  Fare clic con il pulsante destro del mouse sul contenitore **computer** predefinito o sulla cartella in cui si trova l'account del nome del cluster.

6.  Scegliere **Proprietà**.

7.  Nella scheda **Sicurezza** fare clic su **Avanzate**.

8.  Nell'elenco di account con le autorizzazioni, fare clic sull'account nome cluster, quindi fare clic su **modifica**.
    

> [!NOTE]
> Se l'account del nome del cluster non è elencato, fare clic su <STRONG>Aggiungi</STRONG> e aggiungerlo all'elenco. 
<br>


9. Per l'account del nome del cluster (noto anche come oggetto nome cluster o oggetto nome cluster), assicurarsi che sia selezionata l'opzione **Consenti** per le autorizzazioni **Crea oggetti computer** e **Leggi tutte le proprietà** .
    
   ![](media/configure-ad-accounts/Cc731002.f5977c4d-a62e-4b17-81e3-8c19ddca2078(WS.10).gif)

10. Fare clic su **OK** fino a tornare allo snap-in **Active Directory utenti e computer** .

11. Esaminare i criteri di dominio (se applicabile, consultare un amministratore di dominio) correlati alla creazione di account computer (oggetti). Assicurarsi che l'account del nome del cluster sia in grado di creare un account computer ogni volta che viene configurata un'applicazione o un servizio del cluster. Se, ad esempio, l'amministratore di dominio ha configurato impostazioni che comportano la creazione di tutti i nuovi account computer in un contenitore specializzato invece che nel contenitore **computer** predefinito, assicurarsi che queste impostazioni consentano all'account del nome del cluster di creare anche nuovi account computer in tale contenitore.

12. Espandere il contenitore **computer** predefinito o il contenitore in cui si trova l'account computer di una delle applicazioni o dei servizi del cluster.

13. Fare clic con il pulsante destro del mouse sull'account computer di una delle applicazioni o dei servizi del cluster, quindi scegliere **Proprietà**.

14. Nella scheda **sicurezza** , verificare che l'account nome cluster sia elencato tra gli account che dispongono di autorizzazioni e selezionarlo. Verificare che l'account del nome del cluster disponga dell'autorizzazione **controllo completo** (la casella di controllo **Consenti** è selezionata). In caso contrario, aggiungere l'account del nome del cluster all'elenco e assegnargli l'autorizzazione **controllo completo** .
    
   ![](media/configure-ad-accounts/Cc731002.2e614376-87a6-453a-81ba-90ff7ebc3fa2(WS.10).gif)

15. Ripetere i passaggi 13-14 per ogni servizio e applicazione in cluster configurati nel cluster.

16. Verificare che la quota a livello di dominio per la creazione di oggetti computer (per impostazione predefinita, 10) non sia stata raggiunta (se applicabile, consultando un amministratore di dominio). Se gli elementi precedenti di questa procedura sono stati rivisti e corretti e se è stata raggiunta la quota, provare ad aumentare la quota. Per modificare la quota:
    
   1.  Aprire un prompt dei comandi come amministratore ed eseguire **ADSIEdit. msc**.  
          
   2.  Fare clic con il pulsante destro del mouse su **ADSI Edit**, scegliere **Connetti a**, quindi fare clic su **OK**. Il **contesto dei nomi predefinito** viene aggiunto all'albero della console.  
          
   3.  Fare doppio clic su **contesto dei nomi predefinito**, fare clic con il pulsante destro del mouse sull'oggetto dominio sottostante, quindi scegliere **Proprietà**.  
          
   4.  Scorrere fino a **ms-DS-MachineAccountQuota**, selezionarlo, fare clic su **modifica**, modificare il valore e quindi fare clic su **OK**.

