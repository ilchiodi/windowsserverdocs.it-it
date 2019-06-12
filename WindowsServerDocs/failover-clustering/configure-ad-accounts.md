---
Title: Configurazione di account di cluster in Active Directory
ms.date: 11/12/2012
ms.prod: windows-server-threshold
ms.technology: storage-failover-clustering
author: JasonGerend
manager: elizapo
ms.author: jgerend
ms.openlocfilehash: c15c33e31bf0bf7261097fbea110f2a0a788dab2
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/31/2019
ms.locfileid: "66439754"
---
# <a name="configuring-cluster-accounts-in-active-directory"></a>Configurazione di account di cluster in Active Directory


Si applica a: Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2 e Windows Server 2008

In Windows Server, quando si crea un cluster di failover e configurare servizi del cluster o le applicazioni, le procedure guidate del cluster di failover creano gli account computer Active Directory necessari (detti anche oggetti computer) e concedere loro autorizzazioni specifiche. Le procedure guidate di creare un account computer per il cluster stesso (questo account viene anche chiamato l'oggetto nome cluster o CNO) e un account computer per la maggior parte delle applicazioni e servizi cluster, l'eccezione in fase di una macchina virtuale Hyper-V. Le autorizzazioni per questi account vengono impostate automaticamente tramite le procedure guidate del cluster di failover. Se vengono modificate le autorizzazioni, devono essere modificati in modo da corrispondere i requisiti di un cluster. Questa guida vengono descritti questi account di Active Directory e autorizzazioni, vengono fornite informazioni sui motivi per cui sono importanti e vengono descritti i passaggi per configurare e gestire gli account.
      

## <a name="overview-of-active-directory-accounts-needed-by-a-failover-cluster"></a>Panoramica degli account di Active Directory necessari per un cluster di failover

Questa sezione descrive l'account computer Active Directory (detti anche oggetti computer di Active Directory) che sono importanti per un cluster di failover. Questi account sono i seguenti:

  - **L'account utente usato per creare il cluster.** Si tratta dell'account utente usato per avviare la creazione guidata Cluster. L'account è importante perché costituisce la base da cui viene creato un account computer per il cluster stesso.  
      
  - **L'account del nome del cluster.** (l'account computer del cluster stesso, denominato anche oggetto nome cluster o CNO). Questo account viene creato automaticamente da Creazione guidata Cluster e ha lo stesso nome del cluster. L'account del nome del cluster è molto importante, perché tramite tale account, gli altri account viene creato automaticamente durante la configurazione nuovi servizi e applicazioni nel cluster. Se l'account del nome del cluster viene eliminato o le autorizzazioni vengono eseguite dal controllo, non è possibile creare altri account come richiesto dal cluster, fino a quando non viene ripristinato l'account del nome cluster o vengono ripristinate le autorizzazioni corrette.  
      
    Ad esempio, se si crea un cluster denominato Cluster1 e quindi provare a configurare un server di stampa in cluster denominato PrintServer1 nel cluster, l'account Cluster1 in Active Directory sarà necessario mantenere le autorizzazioni corrette in modo che può essere utilizzato per creare un computer account denominato PrintServer1.  
      
    Viene creato l'account del nome cluster nel contenitore predefinito per gli account computer in Active Directory. Per impostazione predefinita si tratta del contenitore "Computer", ma l'amministratore di dominio è possibile scegliere di reindirizzarlo a un altro contenitore o unità organizzativa (OU).  
      
  - **L'account computer (oggetti computer) di un'applicazione o servizio del cluster.** Questi account vengono creati automaticamente dalla procedura guidata disponibilità elevata come parte del processo di creazione di molti tipi di servizi del cluster o dell'applicazione, l'eccezione in fase di una macchina virtuale Hyper-V. L'account del nome cluster verrà concesse le autorizzazioni necessarie per controllare tali account.  
      
    Ad esempio, se si dispone di un cluster denominato Cluster1 e quindi si crea un file server in cluster denominato FileServer1, la configurazione guidata disponibilità elevata consente di creare un account computer di Active Directory denominato FileServer1. La configurazione guidata disponibilità elevata offre anche l'account Cluster1 le autorizzazioni necessarie per controllare l'account FileServer1.  
      

La tabella seguente descrive le autorizzazioni necessarie per questi account.


<table>
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<thead>
<tr class="header">
<th>Account</th>
<th>Informazioni dettagliate sulle autorizzazioni</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>Account utilizzato per creare il cluster</p></td>
<td><p>Richiede autorizzazioni amministrative sui server che diventeranno i nodi del cluster. Richiede inoltre <strong>crea oggetti Computer</strong> e <strong>Leggi tutte le proprietà</strong> autorizzazioni nel contenitore che viene usato per gli account computer nel dominio.</p></td>
</tr>
<tr class="even">
<td><p>Account del nome cluster (account computer del cluster)</p></td>
<td><p>Quando viene eseguita la creazione guidata Cluster, viene creato l'account del nome cluster nel contenitore predefinito che viene usato per gli account computer nel dominio. Per impostazione predefinita, l'account del nome cluster (ad esempio altri account computer) è possibile creare fino a dieci account computer nel dominio.</p>
<p>Se si crea l'account del nome cluster (oggetto nome cluster) prima di creare il cluster, ovvero, ovvero pre-installare l'account, è necessario specificare il <strong>crea oggetti Computer</strong> e <strong>Leggi tutte le proprietà</strong> autorizzazioni nel contenitore che viene usato per gli account computer nel dominio. È inoltre necessario disabilitare l'account e concedere <strong>controllo completo</strong> parte per l'account che verrà utilizzato dall'amministratore che installa il cluster. Per altre informazioni, vedere <a href="#steps-for-prestaging-the-cluster-name-account" data-raw-source="[Steps for prestaging the cluster name account](#steps-for-prestaging-the-cluster-name-account)">passaggi per la pre-installazione del cluster del nome account</a>, più avanti in questa Guida.</p></td>
</tr>
<tr class="odd">
<td><p>Account del computer di un'applicazione o servizio cluster</p></td>
<td><p>Quando la configurazione guidata disponibilità elevata è esecuzione (per creare un nuovo servizio o applicazione cluster), nella maggior parte dei casi, un account computer per il servizio cluster o dell'applicazione viene creata in Active Directory. L'account del nome cluster verrà concesse le autorizzazioni necessarie per controllare questo account. L'eccezione è una macchina virtuale Hyper-V in cluster: non viene creato alcun account computer per questo oggetto.</p>
<p>Se si pre-installare l'account computer per un servizio o applicazione cluster, è necessario configurarlo con le autorizzazioni necessarie. Per altre informazioni, vedere <a href="#steps-for-prestaging-an-account-for-a-clustered-service-or-application" data-raw-source="[Steps for prestaging an account for a clustered service or application](#steps-for-prestaging-an-account-for-a-clustered-service-or-application)">passaggi per la configurazione pre-installazione di un account per un servizio o applicazione cluster</a>, più avanti in questa Guida.</p></td>
</tr>
</tbody>
</table>


> [!NOTE]
> Nelle versioni precedenti di Windows Server, si è verificato un account per il servizio Cluster. A partire da Windows Server 2008, tuttavia, il servizio Cluster viene eseguita automaticamente in un contesto speciale che fornisce le autorizzazioni specifiche e i privilegi necessari per il servizio (simile nel contesto di sistema locale, ma con privilegi ridotti). È necessari altri account, tuttavia, come descritto in questa Guida. 
<br>


### <a name="how-accounts-are-created-through-wizards-in-failover-clustering"></a>Come gli account vengono creati attraverso procedure guidate incluse nel clustering di failover

Il diagramma seguente illustra l'uso e la creazione di account del computer (oggetti di Active Directory) che sono descritti nella sezione precedente. Questi account entrano in gioco quando un amministratore esegue la procedura guidata di creazione del Cluster e quindi esegue la configurazione guidata disponibilità elevata (per configurare un servizio o applicazione cluster).

![](media/configure-ad-accounts/Cc731002.e8a7686c-9ba8-4ddf-87b1-175b7b51f65d(WS.10).gif)

Si noti che il diagramma precedente mostra un unico amministratore che esegue sia la creazione guidata Cluster e la configurazione guidata disponibilità elevata. Tuttavia, potrebbe trattarsi di due amministratori diversi usando due account utente diverso, se entrambi gli account dispone di autorizzazioni sufficienti. Le autorizzazioni sono descritte in dettaglio nei requisiti relativi al cluster di failover, i domini di Active Directory e gli account, più avanti in questa Guida.

### <a name="how-problems-can-result-if-accounts-needed-by-the-cluster-are-changed"></a>Come possono causare problemi se si modificano gli account necessari per il cluster

Il diagramma seguente illustra come possono causare problemi se l'account del nome cluster (uno degli account necessari per il cluster) viene modificata dopo che viene creato automaticamente per la creazione guidata Cluster.

![](media/configure-ad-accounts/Cc731002.beecc4f7-049c-4945-8fad-2cceafd6a4a5(WS.10).gif)

Se si verifica il tipo di problema illustrato nel diagramma, un determinato evento (1193, 1194, 1206 o 1207) viene registrato nel Visualizzatore eventi. Per altre informazioni su questi eventi, vedere [ http://go.microsoft.com/fwlink/?LinkId=118271 ](http://go.microsoft.com/fwlink/?linkid=118271).

Si noti che un problema simile con la creazione di un account per un servizio o applicazione cluster può verificarsi se è stata raggiunta la quota a livello di dominio per la creazione di oggetti computer (impostazione predefinita, 10). In questo caso, potrebbe essere opportuno rivolgersi all'amministratore di dominio sull'aumento della quota, anche se questa è un'impostazione a livello di dominio e deve essere modificata solo dopo un'attenta valutazione e solo dopo aver confermato che il diagramma precedente non è Descrivere la situazione. Per altre informazioni, vedere [passaggi per la risoluzione dei problemi causati dalle modifiche apportate negli account di Active Directory correlati ai cluster](#steps-for-troubleshooting-problems-caused-by-changes-in-cluster-related-active-directory-accounts), più avanti in questa Guida.

## <a name="requirements-related-to-failover-clusters-active-directory-domains-and-accounts"></a>Requisiti correlati a cluster di failover, i domini di Active Directory e account

Come descritto nelle tre sezioni precedenti, prima di servizi del cluster devono essere soddisfatti determinati requisiti e le applicazioni possono essere configurate correttamente in un cluster di failover. I requisiti di base riguardano la posizione dei nodi del cluster (all'interno di un singolo dominio) e il livello di autorizzazioni dell'account della persona che installa il cluster. Se vengono soddisfatti questi requisiti, gli altri account necessari dal cluster possono essere creati automaticamente tramite le procedure guidate del cluster di failover. Nell'elenco seguente fornisce informazioni dettagliate su questi requisiti di base.

  - **nodi:** Tutti i nodi devono trovarsi nello stesso dominio Active Directory. (Il dominio non può essere basato su Windows NT 4.0, che non include Active Directory).  
      
  - **Account della persona che installa il cluster:** La persona che installa il cluster deve usare un account con le caratteristiche seguenti:  
      
      - L'account deve essere un account di dominio. Che è necessario essere un account di amministratore di dominio. Può trattarsi di un account utente di dominio se soddisfa gli altri requisiti di questo elenco.  
          
      - L'account deve disporre delle autorizzazioni amministrative sui server che diventeranno i nodi del cluster. Il modo più semplice per fornire ciò è creare un account utente di dominio, e quindi aggiungere tale account al gruppo Administrators locale in ogni server che diventeranno i nodi del cluster. Per altre informazioni, vedere [passaggi per la configurazione dell'account per chi installa il cluster](#steps-for-configuring-the-account-for-the-person-who-installs-the-cluster), più avanti in questa Guida.  
          
      - L'account (o il gruppo che l'account è membro di) è necessario assegnare il **crea oggetti Computer** e **Leggi tutte le proprietà** autorizzazioni nel contenitore che viene usato per gli account computer nel dominio. In alternativa è possibile verificare l'account di un account di amministratore di dominio. Per altre informazioni, vedere [passaggi per la configurazione dell'account per chi installa il cluster](#steps-for-configuring-the-account-for-the-person-who-installs-the-cluster), più avanti in questa Guida.  
          
      - Se l'organizzazione sceglie di pre-installare l'account del nome cluster (un account computer con lo stesso nome del cluster), l'account del nome cluster pre-installazione deve concedere l'autorizzazione di "Controllo completo" per l'account della persona che installa il cluster. Per altri dettagli importanti su come pre-installare l'account del nome cluster, vedere [passaggi per la pre-installazione del cluster del nome account](#steps-for-prestaging-the-cluster-name-account), più avanti in questa Guida.  
          

### <a name="planning-ahead-for-password-resets-and-other-account-maintenance"></a>La pianificazione per la reimpostazione della password e altre operazioni di manutenzione di account

Gli amministratori di cluster di failover in alcuni casi potrebbe essere necessario reimpostare la password dell'account del nome del cluster. Questa azione richiede un'autorizzazione specifica, il **la reimpostazione della password** l'autorizzazione. Pertanto, è consigliabile modificare le autorizzazioni dell'account del nome cluster (tramite lo snap-in Active Directory Users and Computers) per assegnare gli amministratori del cluster di **la reimpostazione della password** dell'autorizzazione per il cluster account del nome. Per altre informazioni, vedere [passaggi per la risoluzione dei problemi delle password con il cluster del nome account](#steps-for-troubleshooting-password-problems-with-the-cluster-name-account), più avanti in questa Guida.

## <a name="steps-for-configuring-the-account-for-the-person-who-installs-the-cluster"></a>Passaggi per la configurazione dell'account per chi installa il cluster

L'account della persona che installa il cluster è importante perché costituisce la base da cui viene creato un account computer per il cluster stesso.

L'appartenenza al gruppo minimo necessario per completare la procedura seguente dipende dal fatto che si creano l'account di dominio e assegnazione delle autorizzazioni necessarie nel dominio o se solo si intende collocare l'account (creato da un altro utente) nel locale **gli amministratori** gruppo nei server che costituiranno i nodi del cluster di failover. Se l'appartenenza al precedente, in **Account Operators** oppure **Domain Admins**, o equivalente è il requisito minimo necessario per completare questa procedura. Se il secondo caso, l'appartenenza al gruppo **gli amministratori** nei server che costituiranno i nodi del cluster di failover il gruppo o equivalente, è tutto ciò che è obbligatorio. Informazioni dettagliate sull'utilizzo degli account appropriati e appartenenze [ http://go.microsoft.com/fwlink/?LinkId=83477 ](http://go.microsoft.com/fwlink/?linkid=83477).

#### <a name="to-configure-the-account-for-the-person-who-installs-the-cluster"></a>Per configurare l'account per chi installa il cluster

1.  Creare o ottenere un account di dominio per la persona che installa il cluster. Questo account può essere un account utente di dominio o un account di amministratore di dominio (in **Domain Admins** o un gruppo equivalente).

2.  Se l'account che è stata creata oppure ottenuta nel passaggio 1 è un account utente di dominio o gli account di amministratore di dominio nel dominio non sono inclusi automaticamente in locale **gli amministratori** Raggruppa in base a computer nel dominio, aggiungere il account locale **gli amministratori** gruppo nei server che costituiranno i nodi del cluster di failover:
    
    1.  Fare clic sul pulsante **Start**, scegliere **Strumenti di amministrazione** e quindi **Server Manager**.  
          
    2.  Nell'albero della console, espandere **Configuration**, espandere **utenti e gruppi locali**, quindi espandere **gruppi**.  
          
    3.  Nel riquadro centrale, fare doppio clic su **Administrators**, fare clic su **aggiungere al gruppo**, quindi fare clic su **Add**.  
          
    4.  Sotto **immettere i nomi degli oggetti da selezionare**, digitare il nome dell'account utente che era stata creata oppure ottenuta nel passaggio 1. Se richiesto, immettere un nome account e password con autorizzazioni sufficienti per questa azione. Fare quindi clic su **OK**.  
          
    5.  Ripetere questi passaggi in ogni server che fungerà da un nodo del cluster di failover.  
          
    

> [!IMPORTANT]
> Questi passaggi devono essere ripetuti in tutti i server che costituiranno i nodi del cluster. 
<br>


3. Se l'account che è stata creata oppure ottenuta nel passaggio 1 è un account di amministratore di dominio, ignorare il resto di questa procedura. In caso contrario, assegnare all'account il **crea oggetti Computer** e **Leggi tutte le proprietà** autorizzazioni nel contenitore che viene usato per gli account computer nel dominio:
    
   1.  In un controller di dominio, fare clic su **avviare**, fare clic su **strumenti di amministrazione**, quindi fare clic su **Active Directory Users and Computers**. Se viene visualizzata la finestra di dialogo **Controllo account utente**, confermare che l'azione visualizzata è quella desiderata e scegliere **Continua**.  
          
   2.  Nel **vista** menu, assicurarsi che **funzionalità avanzate** sia selezionata.  
          
       Quando **Advanced Features** è selezionata, è possibile visualizzare i **Security** scheda nelle proprietà dell'account (oggetti) in **Active Directory Users and Computers**.  
          
   3.  Fare doppio clic il valore predefinito **computer** contenitore o il contenitore predefinito nel computer a cui vengono creati nel dominio, account e quindi fare clic su **proprietà**. **I computer** si trova in <b>Active Directory Users and Computers /</b><i>nodo dominio</i><b>/Computers</b>.  
          
   4.  Nella scheda **Sicurezza** fare clic su **Avanzate**.  
          
   5.  Fare clic su **Add**, digitare il nome dell'account che era stata creata oppure ottenuta nel passaggio 1 e quindi fare clic su **OK**.  
          
   6.  Nel **voci di autorizzazione per * * * contenitore* finestra di dialogo individuare il **crea oggetti Computer** e **Leggi tutte le proprietà** autorizzazioni e assicurarsi che il **Consenti** casella di controllo è selezionata per ognuno di essi.  
          
       ![](media/configure-ad-accounts/Cc731002.0a863ac5-2024-4f9f-8a4d-a419aff32fa0(WS.10).gif)

## <a name="steps-for-prestaging-the-cluster-name-account"></a>Passaggi per la configurazione pre-installazione l'account del nome cluster

È in genere più semplice se si non pre-installare l'account del nome cluster, ma consentire invece l'account venga creato e configurato automaticamente quando si esegue la creazione guidata Cluster. Tuttavia, se è necessario pre-installare l'account del nome del cluster a causa dei requisiti dell'organizzazione, usare la procedura seguente.

L'appartenenza al gruppo **Domain Admins** oppure a un gruppo equivalente è il requisito minimo per completare la procedura. Informazioni dettagliate sull'utilizzo degli account appropriati e appartenenze [ http://go.microsoft.com/fwlink/?LinkId=83477 ](http://go.microsoft.com/fwlink/?linkid=83477). Si noti che è possibile usare lo stesso account per questa procedura perché verrà usata durante la creazione del cluster.

#### <a name="to-prestage-a-cluster-name-account"></a>Per pre-installare un account del nome cluster

1.  Assicurarsi di conoscere il nome che avrà il cluster e il nome dell'account utente che verrà utilizzato dalla persona che crea il cluster. Si noti che è possibile usare tale account per eseguire questa procedura.

2.  In un controller di dominio, fare clic su **avviare**, fare clic su **strumenti di amministrazione**, quindi fare clic su **Active Directory Users and Computers**. Se viene visualizzata la finestra di dialogo **Controllo account utente**, confermare che l'azione visualizzata è quella desiderata e scegliere **Continua**.

3.  Nell'albero della console, fare doppio clic su **computer** o il contenitore predefinito nel computer a cui vengono creati account nel dominio. **I computer** si trova in <b>Active Directory Users and Computers /</b><i>nodo dominio</i><b>/Computers</b>.

4.  Fare clic su **New** e quindi fare clic su **Computer**.

5.  Digitare il nome che verrà utilizzato per il cluster di failover, in altre parole, il nome del cluster che verrà specificato in Creazione guidata Cluster e quindi fare clic su **OK**.

6.  Fare doppio clic sull'account appena creato e quindi fare clic su **Disabilita Account**. Se viene richiesto di confermare la scelta, fare clic su **Sì**.
    
    L'account deve essere disabilitato in modo che quando la **crea Cluster** esecuzione della procedura guidata, è possibile verificare che l'account verrà utilizzato per il cluster non sia attualmente in uso da un computer esistente o un cluster nel dominio.

7.  Nel **vista** menu, assicurarsi che **funzionalità avanzate** sia selezionata.
    
    Quando **Advanced Features** è selezionata, è possibile visualizzare i **Security** scheda nelle proprietà dell'account (oggetti) in **Active Directory Users and Computers**.

8.  Fare clic sulla cartella selezionata nel passaggio 3 e quindi fare clic su **proprietà**.

9.  Nella scheda **Sicurezza** fare clic su **Avanzate**.

10. Fare clic su **Add**, fare clic su **tipi di oggetti** e verificare che **computer** sia selezionata e quindi fare clic su **OK**. Quindi, sotto **immettere il nome dell'oggetto da selezionare**, digitare il nome dell'account computer appena creato e quindi fare clic su **OK**. Se viene visualizzato un messaggio che informa che sta tentando di aggiungere un oggetto disabilitato, fare clic su **OK**.

11. Nel **voce di autorizzazione** finestra di dialogo individuare il **crea oggetti Computer** e **Leggi tutte le proprietà** autorizzazioni e assicurarsi che il **Consenti** casella di controllo è selezionata per ognuno di essi.
    
    ![](media/configure-ad-accounts/Cc731002.f5977c4d-a62e-4b17-81e3-8c19ddca2078(WS.10).gif)

12. Fare clic su **OK** fino a quando non viene di nuovo il **Active Directory Users and Computers** snap-in.

13. Se si usa lo stesso account per eseguire questa procedura verrà usato per creare il cluster, ignorare i passaggi rimanenti. In caso contrario, è necessario configurare le autorizzazioni in modo che l'account utente che verrà utilizzato per creare il cluster dispone del controllo completo dell'account computer che appena creato:
    
    1.  Nel **vista** menu, assicurarsi che **funzionalità avanzate** sia selezionata.  
          
    2.  Fare doppio clic sull'account computer appena creato e quindi fare clic su **proprietà**.  
          
    3.  Nella scheda **Sicurezza** fare clic su **Aggiungi**. Se viene visualizzata la finestra di dialogo **Controllo account utente**, confermare che l'azione visualizzata è quella desiderata e scegliere **Continua**.  
          
    4.  Usare la **Seleziona utenti, computer o gruppi** finestra di dialogo per specificare l'account utente che verrà utilizzato durante la creazione del cluster. Fare quindi clic su **OK**.  
          
    5.  Assicurarsi che sia selezionato l'account utente che appena aggiunto, quindi, accanto a **controllo completo**, selezionare la **Consenti** casella di controllo.  
          
        ![](media/configure-ad-accounts/Cc731002.fffaafe2-a494-498b-974c-8f9d70f7103b(WS.10).gif)

## <a name="steps-for-prestaging-an-account-for-a-clustered-service-or-application"></a>Passaggi per la configurazione pre-installazione di un account per un servizio o applicazione cluster

È in genere più semplice se si non pre-installare l'account computer per un servizio o applicazione cluster, ma consentire invece l'account venga creato e configurato automaticamente quando si esegue la configurazione guidata disponibilità elevata. Tuttavia, se è necessario pre-installare gli account a causa dei requisiti dell'organizzazione, usare la procedura seguente.

Appartenenza al gruppo il **Account Operators** gruppo o equivalente è il requisito minimo necessario per completare questa procedura. Informazioni dettagliate sull'utilizzo degli account appropriati e appartenenze [ http://go.microsoft.com/fwlink/?LinkId=83477 ](http://go.microsoft.com/fwlink/?linkid=83477).

#### <a name="to-prestage-an-account-for-a-clustered-service-or-application"></a>Per pre-installare un account per un servizio o applicazione cluster

1.  Assicurarsi di conoscere il nome del cluster e il nome che sarà necessario il servizio cluster o l'applicazione.

2.  In un controller di dominio, fare clic su **avviare**, fare clic su **strumenti di amministrazione**, quindi fare clic su **Active Directory Users and Computers**. Se viene visualizzata la finestra di dialogo **Controllo account utente**, confermare che l'azione visualizzata è quella desiderata e scegliere **Continua**.

3.  Nell'albero della console, fare doppio clic su **computer** o il contenitore predefinito nel computer a cui vengono creati account nel dominio. **I computer** si trova in <b>Active Directory Users and Computers /</b><i>nodo dominio</i><b>/Computers</b>.

4.  Fare clic su **New** e quindi fare clic su **Computer**.

5.  Digitare il nome verrà utilizzare per il servizio o applicazione cluster e quindi fare clic su **OK**.

6.  Nel **vista** menu, assicurarsi che **funzionalità avanzate** sia selezionata.
    
    Quando **Advanced Features** è selezionata, è possibile visualizzare i **Security** scheda nelle proprietà dell'account (oggetti) in **Active Directory Users and Computers**.

7.  Fare doppio clic sull'account computer appena creato e quindi fare clic su **proprietà**.

8.  Nella scheda **Sicurezza** fare clic su **Aggiungi**.

9.  Fare clic su **tipi di oggetti** e assicurarsi che **computer** sia selezionata e quindi fare clic su **OK**. Quindi, sotto **immettere il nome dell'oggetto da selezionare**, digitare l'account del nome cluster e quindi fare clic su **OK**. Se viene visualizzato un messaggio che informa che sta tentando di aggiungere un oggetto disabilitato, fare clic su **OK**.

10. Assicurarsi che sia selezionato l'account del nome cluster, quindi, accanto a **controllo completo**, selezionare la **Consenti** casella di controllo.
    
    ![](media/configure-ad-accounts/Cc731002.2e614376-87a6-453a-81ba-90ff7ebc3fa2(WS.10).gif)

## <a name="steps-for-troubleshooting-problems-related-to-accounts-used-by-the-cluster"></a>Passaggi per la risoluzione dei problemi relativi agli account usati dal cluster

Come descritto in precedenza in questa Guida, quando si crea un cluster di failover e configurare applicazioni o servizi del cluster, le procedure guidate del cluster di failover creano gli account di Active Directory necessari e assegnare loro le autorizzazioni corrette. Se viene eliminato un account necessario oppure si modificano le autorizzazioni necessarie, possono causare problemi. Le sottosezioni seguenti forniscono i passaggi per la risoluzione di questi problemi.

  - [Passaggi per la risoluzione dei problemi delle password con l'account del nome cluster](#steps-for-troubleshooting-password-problems-with-the-cluster-name-account)
      
  - [Passaggi per la risoluzione dei problemi causati dalle modifiche apportate negli account di Active Directory legati al cluster](#steps-for-troubleshooting-problems-caused-by-changes-in-cluster-related-active-directory-accounts)
      

### <a name="steps-for-troubleshooting-password-problems-with-the-cluster-name-account"></a>Passaggi per la risoluzione dei problemi delle password con l'account del nome cluster

Utilizzare questa procedura se è presente un messaggio di evento sugli oggetti computer o sull'identità del cluster che include il testo seguente. Si noti che questo testo sarà all'interno del messaggio di evento, non all'inizio del messaggio dell'evento:

`Logon failure: unknown user name or bad password.`

I messaggi di evento che si adatta alla descrizione precedente indicano che la password per l'account del nome cluster e la password corrispondente memorizzate dal software di clustering non corrispondono.

Per informazioni su come assicurare che gli amministratori di cluster abbiano le autorizzazioni corrette per eseguire la procedura seguente in base alle esigenze, vedere pianificazione-ahead per la reimpostazione della password e altre operazioni di manutenzione di account, più indietro in questa Guida.

Appartenenza al gruppo locale **amministratori** o gruppo equivalente, è il requisito minimo necessario per completare questa procedura. Inoltre, l'account deve disporre **Reimposta password** l'autorizzazione per l'account del nome cluster (a meno che l'account è un **Domain Admins** account o si è il proprietario dell'account del nome cluster). L'account che è stato utilizzato dalla persona che ha installato il cluster può essere utilizzato per questa procedura. Informazioni dettagliate sull'utilizzo degli account appropriati e appartenenze [ http://go.microsoft.com/fwlink/?LinkId=83477 ](http://go.microsoft.com/fwlink/?linkid=83477).

#### <a name="to-troubleshoot-password-problems-with-the-cluster-name-account"></a>Risoluzione dei problemi di password con l'account del nome cluster

1.  Per aprire lo snap-in cluster di failover, fare clic sul pulsante **Start**, scegliere **Strumenti di amministrazione**, quindi fare clic su **Gestione cluster di failover**. (Se il **User Account Control** verrà visualizzata la finestra di dialogo, verificare che l'azione visualizzata sia quella desiderato e quindi fare clic su **continua**.)

2.  Nello snap-in Gestione cluster di failover, se il cluster che si desidera configurare non viene visualizzato, nell'albero della console fare clic con il pulsante destro del mouse su **Gestione cluster di failover**, fare clic su **Gestisci un cluster** e scegliere o specificare il cluster desiderato.

3.  Nel riquadro centrale, espandere **risorse principali del Cluster**.

4.  Sotto **nome Cluster**, fare doppio clic sui **nome** elemento, scegliere **altre azioni**e quindi fare clic su **oggetto Ripristina Active Directory**.

### <a name="steps-for-troubleshooting-problems-caused-by-changes-in-cluster-related-active-directory-accounts"></a>Passaggi per la risoluzione dei problemi causati dalle modifiche apportate negli account di Active Directory legati al cluster

Se l'account del nome del cluster viene eliminato o se sono state eseguite dal controllo delle autorizzazioni, si verificheranno problemi quando si prova a configurare un nuovo servizio o applicazione cluster. Per risolvere un problema in cui ciò potrebbe essere la causa, utilizzare Active Directory Users e snap-in computer per visualizzare o modificare i cluster nome account e altri account correlati. Per informazioni sugli eventi che vengono registrati quando vedere (evento 1193, 1194, 1206 o 1207), si verifica questo tipo di problema [ http://go.microsoft.com/fwlink/?LinkId=118271 ](http://go.microsoft.com/fwlink/?linkid=118271).

L'appartenenza al gruppo **Domain Admins** oppure a un gruppo equivalente è il requisito minimo per completare la procedura. Informazioni dettagliate sull'utilizzo degli account appropriati e appartenenze [ http://go.microsoft.com/fwlink/?LinkId=83477 ](http://go.microsoft.com/fwlink/?linkid=83477).

#### <a name="to-troubleshoot-problems-caused-by-changes-in-cluster-related-active-directory-accounts"></a>Per risolvere i problemi causati dalle modifiche apportate negli account di Active Directory legati al cluster

1.  In un controller di dominio, fare clic su **avviare**, fare clic su **strumenti di amministrazione**, quindi fare clic su **Active Directory Users and Computers**. Se viene visualizzata la finestra di dialogo **Controllo account utente**, confermare che l'azione visualizzata è quella desiderata e scegliere **Continua**.

2.  Espandere il valore predefinito **computer** contenitore o la cartella in cui si trova l'account del nome cluster (l'account computer per il cluster). **I computer** si trova in <b>Active Directory Users and Computers /</b><i>nodo dominio</i><b>/Computers</b>.

3.  Esaminare l'icona per l'account del nome del cluster. Non deve avere una freccia verso il basso su di esso, vale a dire l'account non deve essere disattivata. Se viene visualizzato deve essere disabilitata, pulsante destro del mouse e cercare il comando **Abilita Account**. Se viene visualizzato il comando, fare clic.

4.  Nel **vista** menu, assicurarsi che **funzionalità avanzate** sia selezionata.
    
    Quando **Advanced Features** è selezionata, è possibile visualizzare i **Security** scheda nelle proprietà dell'account (oggetti) in **Active Directory Users and Computers**.

5.  Fare doppio clic il valore predefinito **computer** contenitore o la cartella in cui si trova l'account del nome del cluster.

6.  Scegliere **Proprietà**.

7.  Nella scheda **Sicurezza** fare clic su **Avanzate**.

8.  Nell'elenco degli account con autorizzazioni, scegliere l'account del nome cluster e quindi fare clic su **modifica**.
    

> [!NOTE]
> Se l'account del nome del cluster non è elencato, fare clic su <STRONG>Add</STRONG> e aggiungerlo all'elenco. 
<br>


9. Per l'account del nome cluster (detto anche oggetto nome cluster o CNO), assicurarsi che **Allow** sia selezionata per il **crea oggetti Computer** e **Leggi tutte le proprietà** autorizzazioni.
    
   ![](media/configure-ad-accounts/Cc731002.f5977c4d-a62e-4b17-81e3-8c19ddca2078(WS.10).gif)

10. Fare clic su **OK** fino a quando non viene di nuovo il **Active Directory Users and Computers** snap-in.

11. Rivedere i criteri di dominio (società di consulenza con un amministratore di dominio, se applicabile) correlate alla creazione di account computer (oggetti). Assicurarsi che l'account del nome del cluster può creare un account del computer ogni volta che si configura un servizio o applicazione cluster. Ad esempio, se l'amministratore di dominio ha configurato le impostazioni che determinano tutti i nuovi account computer deve essere creato in un contenitore specifico anziché il valore predefinito **computer** contenitore, assicurarsi che queste impostazioni consentono di account del nome cluster per creare nuovi account computer nel contenitore anche.

12. Espandere il valore predefinito **computer** contenitore o il contenitore in cui si trova l'account computer per uno dei servizi del cluster o delle applicazioni.

13. Fare doppio clic su account computer per uno dei servizi del cluster o delle applicazioni e quindi fare clic su **proprietà**.

14. Nel **sicurezza** scheda, verificare che l'account del nome cluster sia elencato tra gli account che dispongano delle autorizzazioni e selezionarla. Verificare che l'account del nome cluster disponga **controllo completo** autorizzazione (il **Consenti** casella di controllo è selezionata). In caso contrario, aggiungere l'account del nome cluster all'elenco e assegnargli **controllo completo** l'autorizzazione.
    
   ![](media/configure-ad-accounts/Cc731002.2e614376-87a6-453a-81ba-90ff7ebc3fa2(WS.10).gif)

15. Ripetere i passaggi da 13 a 14 per ogni servizio del cluster e un'applicazione configurata nel cluster.

16. Verificare che la quota a livello di dominio per la creazione di oggetti computer (impostazione predefinita, 10) non è stata raggiunta (consulting con un amministratore di dominio se applicabile). Se gli elementi precedenti in questa procedura tutte stati verificati e corretti e se è stata raggiunta la quota, provare ad aumentare la quota. Per modificare la quota:
    
   1.  Aprire un prompt dei comandi come amministratore ed eseguire **Adsiedit. msc**.  
          
   2.  Fare doppio clic su **ADSI Edit**, fare clic su **connettersi**, quindi fare clic su **OK**. Il **contesto dei nomi predefinito** viene aggiunto alla struttura della console.  
          
   3.  Fare doppio clic su **contesto dei nomi predefinito**, fare doppio clic su oggetto di dominio sotto di esso e quindi fare clic su **proprietà**.  
          
   4.  Scorrere fino alla **ms-DS-MachineAccountQuota**, selezionarlo, fare clic su **Edit**, modificare il valore e quindi fare clic su **OK**.

