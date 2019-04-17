---
title: Aggiunta di voci ai configura, componenti aggiuntivi, riepilogo sullo stato e i collegamenti della Guida
description: Viene descritto come utilizzare Windows Server Essentials
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: c0a8f10d-fd85-4c8d-b9bb-176cb1db1f46
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 6d3303f2c6d84932ad9d5dee8a547cd478447732
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/12/2017
---
# <a name="add-entries-to-setup-add-ins-quick-status-and-help-links"></a>Aggiunta di voci ai configura, componenti aggiuntivi, riepilogo sullo stato e i collegamenti della Guida

>Si applica a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

È possibile aggiungere attività per il **installazione**, **componenti aggiuntivi**, **riepilogo sullo stato** elenchi ed è possibile aggiungere collegamenti alla sezione collegamenti alla Comunità nella home page del Dashboard. Attività e i collegamenti vengono aggiunti a questi elenchi e sezione collocando un file XML denominato OEMHomePageContent.home file o un file di risorse incorporato denominato OEMHomePageContent.dll in %ProgramFiles%\Windows Server\Bin\Addins\Home. Il file di risorse incorporato utilizzabile per localizzare il testo nelle attività e nei collegamenti aggiunti. Il file. Home contiene le definizioni XML delle attività e dei collegamenti.  
  
## <a name="adding-tasks-to-the-setup-add-ins-quick-status-task-lists-and-adding-links-to-help-task"></a>Aggiunta di attività per la configurazione, componenti aggiuntivi, gli elenchi di attività di riepilogo sullo stato e aggiungendo collegamenti all'attività Guida  
 È possibile aggiungere attività per il **il programma di installazione**, **componenti aggiuntivi**, **riepilogo sullo stato** di attività e collegamenti a elenchi il **Guida** attività tramite la definizione di attività e dei collegamenti con un file XML, facoltativamente la creazione del file di risorse incorporato e installando il file nel server. Se il file XML viene installato sul server senza un file di risorse, deve essere denominato OEMHomePageContent.home. Se viene utilizzato un assembly per installare un file XML e un file di risorse, deve essere denominato OEMHomePageContent.dll e deve essere firmato con Authenticode.  
  
### <a name="define-the-tasks-and-links"></a>Definire le attività e collegamenti  
 È possibile utilizzare un editor di testo come blocco note per creare il file. Home o se viene creato anche un file di risorse incorporato, è possibile utilizzare Visual Studio 2010 o versione successiva per definire i file. La procedura seguente viene illustrato come utilizzare Visual Studio 2010 o versione successiva per creare i file.  
  
##### <a name="to-define-the-tasks-and-links"></a>Per definire le attività e collegamenti  
  
1.  Apri Visual Studio 2010 o versione successiva come amministratore facendo il programma nel menu Start **Esegui come amministratore**.  
  
2.  Fare clic su **File**, fare clic su **New**, quindi fare clic su **progetto**.  
  
3.  Nel **modelli** riquadro, fare clic su **libreria di classi**, tipo **OEMHomePageContent** nel **nome** casella e quindi fare clic su **OK**.  
  
4.  Eliminare il file Class1.cs.  
  
5.  Fare clic su Nuovo progetto, fare clic su **Aggiungi**, quindi fare clic su **nuovo elemento**.  
  
6.  Nel **modelli** riquadro, fare clic su **File XML**, tipo **Oemhomepagecontent** nel **nome** casella e quindi fare clic su **Aggiungi**.  
  
    > [!NOTE]
    >  Se il file XML viene installato senza un file di risorse, deve essere denominato OEMHomePageContent.home. Se è incluso in un assembly, è possibile assegnargli qualsiasi nome, purché con un'estensione. Home.  
  
7.  Aggiungi il codice XML seguente nel file OEMHomePageContent.home:  
  
    ```  
  
    <Tasks version=?2.0? xmlns=?https://schemas.microsoft.com/WindowsServerSolutions/2010/01/Dashboard>  
       <SetupMyServerTasks>  
          <Task name="MyTask"  
             description="MyTaskDescription"  
             id="GUID">  
                  <Action   
                  name=?MyAction1Name?   
                  image=?IconForAction1?  
                  type=?TaskType?  
                  exelocation=?ActionExeLocation? />  
                  <Action   
                  name=?MyAction2Name?   
                  image=?IconForAction2?  
                  type=?TaskType?  
                  exelocation=?ActionExeLocation? />  
                   ¦  
           </Task>  
                   ¦  
        </SetupMyServerTasks>  
    <MailServiceTasks>  
         <!-- Same schema as in œSetupMyServerTasks? but the tasks are shown in œConnect to Email Service? category. -->  
    </MailServiceTasks>  
    <LineOfBusinessTasks>  
         <!-- Same schema as in œSetupMyServerTasks? but the tasks are shown in œAdd-ins? category. -->  
  
    <GetQuickStatusTasks>  
          <Task name="MyQuickStatusTask1"  
             description="MyQuickStatusTask1Desc   "  
             id="GUID"  
             assembly="AssemblyName of quick status query implementation"  
             class="ClassName of quick status query implementation"           
             replaceid="GUID"/>  
               <!--  Same schema as Actions in œSetupMyServerTasks? -->   
             </Task>  
    </GetQuickStatusTasks>  
       <Links>  
          <Link  
             ID=?GUID?  
             Title="Displayed text of the link"  
             Description="A very short description"  
             ShellExecPath="Path to the application or URL"/>  
       </Links>  
    </Tasks>  
    ```  
  
     Dove:  
  
    |Attributo|Descrizione|  
    |---------------|-----------------|  
    |Nome (attività)|Il nome visualizzato per l'attività nell'elenco. Se si crea un file di risorse incorporato, il valore di questo attributo è la risorsa stringa.|  
    |Descrizione (attività)|La descrizione dell'attività. Se si crea un file di risorse incorporato, il valore di questo attributo è la risorsa stringa.|  
    |ID (attività)|L'identificatore dell'attività. Questo identificatore deve essere un GUID. Creare un nuovo GUID per un **exe** attività, ma per un **globale** attività, utilizzare il GUID creato al momento della definizione dell'attività per il riquadro attività della sottoscheda. Per ulteriori informazioni sulla creazione di un GUID, vedere [crea Guid (guidgen.exe)](https://go.microsoft.com/fwlink/?LinkId=116098).|  
    |immagine|Questo campo verrà ignorato.|  
    |Nome (azione)|Visualizza il nome dell'attività.|  
    |Tipo (azione)|Descrive il tipo di attività. L'attività può uno dei seguenti:- **globale** attività, **exe**, o un url. Un **globale** attività è la medesima attività globale creata durante la definizione di attività per il riquadro nella sottoscheda. Per ulteriori informazioni sulla creazione di un'attività globale che può essere utilizzata in entrambi riquadro attività della sottoscheda e negli elenchi attività iniziali e attività comuni della home page, vedere œCreating delle classi di supporto? in œHow a: creare una sottoscheda? del [Windows Server Solutions SDK](https://go.microsoft.com/fwlink/?LinkID=248648). Un **exe** attività può essere utilizzata per eseguire le applicazioni da attività iniziali o elenchi di attività comuni.|  
    |exelocation|Il percorso dell'applicazione che viene associato all'attività. Questo attributo viene utilizzato solo per **exe** attività.|  
    |sostituzioneid|L'identificatore dell'attività che viene sostituito con questa attività.|  
    |assembly|Il nome dell'assembly che fornisce la classe per implementare una query di riepilogo sullo stato. L'assembly deve trovarsi in programma Files \ windows server\bin\\.|  
    |classe|Il nome della classe implementa la query di riepilogo sullo stato. La classe deve implementare **ITaskStatusQuery** interfaccia.|  
    |Titolo (collegamento)|Il testo visualizzato per il collegamento. Se si crea un file di risorse incorporato, il valore di questo attributo è la risorsa stringa.|  
    |Descrizione (collegamento)|La descrizione della destinazione del collegamento. Se si crea un file di risorse incorporato, il valore di questo attributo è la risorsa stringa.|  
    |ShellExecPath|Il percorso dell'applicazione o l'URL.<br /><br /> **Nota:** le variabili di ambiente sono supportate nell'attributo ShellExecPath.|  
  
     Esempio di codice seguente viene illustrato come definire un collegamento a un'applicazione:  
  
    ```  
    <Links>  
       <Link Title="Calc" Description="Launches Calc" ShellExecPath="%windir%\system32\calc.exe" />  
    </Links>  
    ```  
  
     Esempio di codice seguente viene illustrato come definire un collegamento a una pagina Web:  
  
    ```  
    <Links>  
       <Link Title="Browser" Description="Open browser" ShellExecPath="http://www.adventureworks.com/" />  
    </Links>  
    ```  
  
8.  Modificare i valori di attributo per rappresentare l'attività o il collegamento.  
  
9. In **Esplora**, fare doppio clic su **Oemhomepagecontent**, quindi fare clic su **proprietà**.  Nel **proprietà** riquadro, in **azione di compilazione**selezionare **risorse incorporato**.  
  
10. Salvare il file OEMHomePageContent.home.  
  
 Per informazioni su come implementare una query di riepilogo sullo stato, fare riferimento a documenti e agli esempi di [Windows Server Solutions SDK](https://go.microsoft.com/fwlink/?LinkID=248648).  
  
#### <a name="change-the-status-of-a-setupadd-ins-task"></a>Modificare lo stato di un'attività di installazione componenti aggiuntivi  
 Le attività elencate nel programma di installazione e componenti aggiuntivi possono essere attivate negli Stati di completamento (configurato per Add-ins) e non completate (non configurate per Add-ins).  
  
 Quando si definisce l'applicazione che è associato alla nuova attività, è possibile utilizzare il metodo SetTaskStatus dello spazio dei nomi Microsoft.WindowsServerSolutions.Administration.ObjectModel.TaskStatusHelper (incluso, ma non documentato in Windows Server Solutions SDK) per modificare lo stato dell'attività. Ad esempio, è possibile modificare il segno di spunta da grigio a verde chiamando il metodo SetTaskStatus con il valore di enumerazione TaskStatus (SetTaskStatus (id, TaskStatus), in cui **id** è l'identificatore dell'attività). I valori di enumerazione che è possono utilizzare sono TaskStatus, TaskStatus.Incomplete o TaskStatus.  
  
##### <a name="replace-tasks"></a>Sostituire le attività  
 È possibile sostituire le attività che sono predefinite in attività preliminari o gli elenchi di attività comuni aggiungendo il GUID per l'attività all'attributo sostituzioneid nella definizione delle attività. Nella tabella seguente sono elencate le attività e gli identificatori corrispondenti che è possibile sostituire nel Dashboard:  
  
|Nome attività|Identificatore|  
|---------------|----------------|  
|Ottenere gli aggiornamenti per altri prodotti Microsoft|8412D35A-13EE-4112-AE0B-F7DBC83EA83D|  
|Configurare il Backup di Server|F68B3F3F-19DE-499D-9ACB-4BB41B8FF420|  
|Configurare accesso remoto via Internet|8991302D-676A-4A7C-B244-D1E08AE0EFEA|  
|Installazione di notifica degli avvisi|DE6F2B36-F19C-4FAF-998B-9772300E3530|  
|Aggiungere un account utente|6D5B5D5F-2EC7-4B1F-9580-4DB084B278B1|  
|Aggiungere le cartelle del server|03F1F438-D94E-439B-A9F7-0C817C37D625|  
|Accesso remoto via Internet - riepilogo sullo stato|6093B462-1F04-4212-8804-9BC823070FAD|  
|Backup del server - riepilogo sullo stato|156947D8-21DC-45FE-A9A8-2F80AE304191|  
|Cartelle del server - riepilogo sullo stato|C657E8AB-AC1F-4AA1-8E80-5AF6BB27C314|  
|Account utente attivi - riepilogo sullo stato|68BCB125-CE8F-467F-B65B-56AD45A614B5|  
|Posta elettronica di notifica degli avvisi - riepilogo sullo stato|75AB06E7-A679-4D62-A5EC-65362FE4F9DB|  
|Computer - riepilogo sullo stato|7966A974-D52D-4F5D-B37F-05C1B73CEEF3|  
  
##### <a name="optional-create-the-resource-file"></a>(Facoltativo) Creare il file di risorse  
 Se vuoi localizzare il testo nelle attività da aggiungere collegamenti alla Comunità, attività iniziali e attività comuni, è necessario creare un assembly che contiene il file. Home e un oggetto. file resx che definisce le stringhe di testo.  
  
###### <a name="to-create-the-resource-file"></a>Per creare il file di risorse  
  
1.  Fare clic sul progetto creato per le tue attività, fare clic su **Aggiungi**, quindi fare clic su **nuovo elemento**.  
  
2.  Nel **modelli** riquadro, fare clic su **File di risorse**, tipo **Oemhomepagecontent** nel **nome** casella e quindi fare clic su **Aggiungi**.  
  
    > [!NOTE]
    >  Il file di risorse è possibile assegnare un nome qualsiasi, purché ha un. estensione resx.  
  
3.  Per ogni attività o collegamento che aggiungere, è necessario aggiungere al file che corrispondono agli elementi attività e collegamento definiti nel file OEMHomePageContent.home OEMHomePageContent.home.resx stringhe e valori. L'esempio di codice seguente mostra un esempio di un file Tasks.xml strutturato per il file di risorse:  
  
    ```  
  
    <Tasks version=?2.0? xmlns=?https://schemas.microsoft.com/WindowsServerSolutions/2010/01/Dashboard>  
       <SetupMyServerTasks>  
          <Task name="MyTask"  
             description="MyDescription"  
             id="GUID">  
             <Action  
             name="MyActionname"  
             image="IconForAction"  
             type="TaskType"  
             exelocation="ActionExeLocation" />  
          </Task>  
       </SetupMyServerTasks>  
    </Tasks>  
  
    ```  
  
    > [!NOTE]
    >  Gli identificatori di attributi utilizzati per la localizzazione non possono contenere spazi.  
  
4.  Aggiungere i nomi delle risorse nomi, MyTaskDescription, MyActionName e IconForAction al file. resx con i valori appropriati.  
  
5.  Salvare il file OEMHomePageContent.home.resx e quindi compilare la soluzione.  
  
#####  <a name="BKMK_SignAssembly"></a>Una firma Authenticode all'assembly  
 È necessario applicarvi una firma Authenticode all'assembly per poter essere utilizzato nel sistema operativo. Per ulteriori informazioni sulla firma dell'assembly, vedere [firma e il controllo del codice con Authenticode](https://msdn.microsoft.com/library/ms537364\(VS.85\).aspx#SignCode).  
  
##### <a name="install-the-task-files"></a>Installare i file delle attività  
 Dopo aver creato il. Home e. resx file, è necessario installarli sul server.  
  
###### <a name="to-install-the-task-files"></a>Per installare i file di attività  
  
1.  Assicurarsi che la soluzione venga generata senza errori.  
  
2.  Se non è stato creato un file di risorse incorporato, copiare il file OEMHomePageContent.home **%ProgramFiles%\Windows Server\Bin\Addins\Home** sul server. Se hai creato un file di risorse incorporato, copiare il file OEMHomePageContent.dll in **%ProgramFiles%\Windows Server\Bin\Addins\Home** sul server.  
  
## <a name="see-also"></a>Vedere anche  
 [Creazione e personalizzazione dell'immagine](Creating-and-Customizing-the-Image.md)   
 [Ulteriori personalizzazioni](Additional-Customizations.md)   
 [Preparazione dell'immagine per la distribuzione](Preparing-the-Image-for-Deployment.md)   
 [Test di analisi utilizzo software](Testing-the-Customer-Experience.md)