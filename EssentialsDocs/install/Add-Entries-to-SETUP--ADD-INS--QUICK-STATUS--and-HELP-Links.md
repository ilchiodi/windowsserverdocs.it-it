---
title: Aggiunta di voci ai collegamenti CONFIGURAZIONE, COMPONENTI AGGIUNTIVI, STATO RAPIDO e GUIDA
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
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59864392"
---
# <a name="add-entries-to-setup-add-ins-quick-status-and-help-links"></a>Aggiunta di voci ai collegamenti CONFIGURAZIONE, COMPONENTI AGGIUNTIVI, STATO RAPIDO e GUIDA

>Si applica a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

È possibile aggiungere delle attività agli elenchi **CONFIGURA**, **COMPONENTI AGGIUNTIVI**, **RIEPILOGO SULLO STATO** e aggiungere collegamenti alla sezione Collegamenti alla comunità nella pagina iniziale del dashboard. Le attività e i collegamenti vengono aggiunti a questi elenchi e a questa sezione collocando un file XML denominato OEMHomePageContent.home o un file delle risorse incorporato denominato OEMHomePageContent.dll in %Programmi%\Windows Server\Bin\Addins\Home. Il file delle risorse predefinito può essere utilizzato per localizzare il testo nelle attività e nei collegamenti aggiunti. Il file .home contiene le definizioni XML delle attività e dei collegamenti.  
  
## <a name="adding-tasks-to-the-setup-add-ins-quick-status-task-lists-and-adding-links-to-help-task"></a>Aggiunta di attività agli elenchi CONFIGURA, COMPONENTI AGGIUNTIVI, RIEPILOGO SULLO STATO e di collegamenti all'attività GUIDA  
 Per aggiungere attività agli elenchi **CONFIGURA**, **COMPONENTI AGGIUNTIVI**, **RIEPILOGO SULLO STATO** e collegamenti all'attività **GUIDA** , definire le attività e i collegamenti utilizzando un file XML oppure, in alternativa, creare un file delle risorse incorporato e installarlo sul server. Se il file XML viene installato sul server senza un file delle risorse, deve chiamarsi OEMHomePageContent.home. Se viene utilizzato un assembly per installare sia il file XML che il file delle risorse, l'assembly deve essere chiamato OEMHomePageContent.dll e deve essere firmato con Authenticode.  
  
### <a name="define-the-tasks-and-links"></a>Definizione delle attività e dei collegamenti  
 Per creare il file .home, utilizzare un editor di testo quale Blocco note, mentre se viene creato anche un file di risorse incorporato, utilizzare Visual Studio 2010 o una versione successiva per definire i file. Nella procedura seguente è descritto l'uso di Visual Studio 2010 o una versione successiva per la creazione di file.  
  
##### <a name="to-define-the-tasks-and-links"></a>Per definire le attività e i collegamenti  
  
1.  Accedere a Visual Studio 2010 o una versione successiva come amministratore facendo clic con il pulsante destro del mouse sul programma nel menu Start, quindi selezionando **Esegui come amministratore**.  
  
2.  Fare clic su **File**, quindi su **Nuovo**e infine su **Progetto**.  
  
3.  Nel riquadro **Modelli** , fare clic su **Libreria di classi**, digitare **OEMHomePageContent** nella casella **Nome** , quindi fare clic su **OK**.  
  
4.  Eliminare il file Class1.cs.  
  
5.  Con il pulsante destro del mouse, fare clic sul nuovo progetto, selezionare **Aggiungi**, quindi **Nuovo elemento**.  
  
6.  Nel riquadro **Modelli** fare clic su **File XML**, digitare **OEMHomePageContent.home** nella casella **Nome** e quindi fare clic su **Aggiungi**.  
  
    > [!NOTE]
    >  Se il file XML viene installato senza un file delle risorse, deve chiamarsi OEMHomePageContent.home. Se è incluso in un assembly, è possibile assegnargli qualsiasi nome con estensione .home.  
  
7.  Aggiungere il seguente codice XML al file OEMHomePageContent.home:  
  
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
    |Nome (attività)|Il nome visualizzato per l'attività nell'elenco. Se si crea un file di risorse incorporato, il valore di questo attributo corrisponde alla risorsa di tipo stringa.|  
    |descrizione (attività)|La descrizione dell'attività. Se si crea un file di risorse incorporato, il valore di questo attributo corrisponde alla risorsa di tipo stringa.|  
    |id (attività)|L'identificatore dell'attività. Questo identificatore deve essere un GUID. È necessario creare un nuovo GUID per le attività **exe** , mentre per le attività **globali** occorre utilizzare il GUID creato al momento della definizione dell'attività per il relativo riquadro della sottoscheda. Per altre informazioni sulla creazione di un GUID, vedere [Crea GUID (guidgen.exe)](https://go.microsoft.com/fwlink/?LinkId=116098).|  
    |image|Questo campo verrà ignorato.|  
    |Nome (azione)|Visualizza il nome dell'attività.|  
    |Tipo (azione)|Descrive il tipo di attività. L'attività può essere: **globale**, **exe** o un URL. Un'attività **globale** corrisponde alla medesima attività globale creata durante la definizione delle attività per il relativo riquadro nella sottoscheda. Per altre informazioni sulla creazione di un'attività globale che può essere utilizzata in sia nel riquadro attività della sottoscheda e gli elenchi attività preliminari o attività comuni della home page, vedere œCreating le classi di supporto? in come: Creare una sottoscheda? del [Windows Server Solutions SDK](https://go.microsoft.com/fwlink/?LinkID=248648). Un'attività **exe** può essere utilizzata per eseguire le applicazioni dagli elenchi Attività preliminari o Attività comuni.|  
    |exelocation|Il percorso dell'applicazione associata all'attività. Questo attributo viene utilizzato solo attività di tipo **exe**.|  
    |replaceid|L'identificatore dell'attività che viene sostituito con questa attività.|  
    |assembly|Il nome dell'assembly che fornisce la classe per l'implementazione della query per ottenere un riepilogo delle informazioni sullo stato. L'assembly deve trovarsi in Program Files \ windows Server\Bin.\\.|  
    |classe|Il nome della classe implementa la query per ottenere un riepilogo delle informazioni sullo stato. La classe deve implementare l'interfaccia **ITaskStatusQuery** .|  
    |Titolo (collegamento)|Il testo visualizzato per il collegamento. Se si crea un file di risorse incorporato, il valore di questo attributo corrisponde alla risorsa di tipo stringa.|  
    |Descrizione (collegamento)|La descrizione della destinazione del collegamento. Se si crea un file di risorse incorporato, il valore di questo attributo corrisponde alla risorsa di tipo stringa.|  
    |ShellExecPath|Il percorso dell'applicazione o dell'URL.<br /><br /> **Nota:** Le variabili di ambiente sono supportate nell'attributo ShellExecPath.|  
  
     L'esempio di codice seguente mostra in quale modo definire un collegamento a un'applicazione:  
  
    ```  
    <Links>  
       <Link Title="Calc" Description="Launches Calc" ShellExecPath="%windir%\system32\calc.exe" />  
    </Links>  
    ```  
  
     L'esempio di codice seguente mostra in quale modo definire un collegamento a una pagina Web:  
  
    ```  
    <Links>  
       <Link Title="Browser" Description="Open browser" ShellExecPath="http://www.adventureworks.com/" />  
    </Links>  
    ```  
  
8.  Modificare i valori di attributo per rappresentare l'attività o il collegamento.  
  
9. In **Esplora soluzioni**, fare clic con il pulsante destro del mouse su **OEMHomePageContent.home**e quindi selezionare **Proprietà**.  Nel riquadro **Proprietà**, in **Azione di compilazione**, selezionare **Risorsa incorporata**.  
  
10. Salvare il file OEMHomePageContent.home.  
  
 Per istruzioni su come implementare una query per ottenere un riepilogo delle informazioni sullo stato, vedere la documentazione e gli esempi [Windows Server Solutions SDK](https://go.microsoft.com/fwlink/?LinkID=248648).  
  
#### <a name="change-the-status-of-a-setupadd-ins-task"></a>Modifica dello stato di un'attività CONFIGURA/COMPONENTI AGGIUNTIVI  
 Le attività incluse negli elenchi CONFIGURA e COMPONENTI AGGIUNTIVI possono essere registrate con lo stato di completate (configurate per i componenti aggiuntivi) e non completate (non configurate per i componenti aggiuntivi).  
  
 Quando si definisce l'applicazione associata alla nuova attività, è possibile utilizzare il metodo SetTaskStatus dello spazio dei nomi Microsoft.WindowsServerSolutions.Administration.ObjectModel.TaskStatusHelper (incluso, ma non documentato in Windows Server Solutions SDK) per modificare lo stato dell'attività. Ad esempio, è possibile cambiare il segno di spunta da grigio a verde richiamando il metodo SetTaskStatus con il valore di enumerazione TaskStatus.Complete (SetTaskStatus(id, TaskStatus.Complete), dove **id** è l'identificatore dell'attività). I valori di enumerazione che è possibile utilizzare sono TaskStatus.Complete, TaskStatus.Incomplete o TaskStatus.Hidden.  
  
##### <a name="replace-tasks"></a>Sostituzione delle attività  
 È possibile sostituire le attività predefinite presenti negli elenchi Azioni preliminari o Azioni comuni aggiungendo il GUID dell'attività all'attributo sostituzioneid nella definizione delle attività. Nella tabella seguente sono elencate le attività e gli identificatori corrispondenti che è possibile sostituire nel dashboard:  
  
|Nome attività|Identificatore|  
|---------------|----------------|  
|Ottenere aggiornamenti per altri prodotti Microsoft|8412D35A-13EE-4112-AE0B-F7DBC83EA83D|  
|Impostazione del backup del server|F68B3F3F-19DE-499D-9ACB-4BB41B8FF420|  
|Impostazione di Accesso remoto via Internet|8991302D-676A-4A7C-B244-D1E08AE0EFEA|  
|Impostazione della notifica degli avvisi|DE6F2B36-F19C-4FAF-998B-9772300E3530|  
|Aggiunta di un account utente|6D5B5D5F-2EC7-4B1F-9580-4DB084B278B1|  
|Aggiunta di cartelle al server|03F1F438-D94E-439B-A9F7-0C817C37D625|  
|Accesso remoto via Internet - Riepilogo sullo stato|6093B462-1F04-4212-8804-9BC823070FAD|  
|Backup server - Riepilogo sullo stato|156947D8-21DC-45FE-A9A8-2F80AE304191|  
|Cartelle del server - Riepilogo sullo stato|C657E8AB-AC1F-4AA1-8E80-5AF6BB27C314|  
|Account utente attivi - Riepilogo sullo stato|68BCB125-CE8F-467F-B65B-56AD45A614B5|  
|Notifica degli avvisi di posta elettronica - Riepilogo sullo stato|75AB06E7-A679-4D62-A5EC-65362FE4F9DB|  
|Computer - Riepilogo sullo stato|7966A974-D52D-4F5D-B37F-05C1B73CEEF3|  
  
##### <a name="optional-create-the-resource-file"></a>(Facoltativo) Creare il file di risorse  
 Per localizzare il testo nelle attività da aggiungere a Attività preliminari, Attività comuni e Collegamenti alla comunità, è necessario creare un assembly che contenga il file .home e un file .home.resx che definisca le stringhe di testo.  
  
###### <a name="to-create-the-resource-file"></a>Per creare il file di risorse  
  
1.  Con il pulsante destro del mouse, fare clic sul progetto creato per le attività, selezionare **Aggiungi**, quindi **Nuovo elemento**.  
  
2.  Nel riquadro **Modelli**, fare clic su **File di risorse**, digitare **OEMHomePageContent.home.resx** nella casella **Nome**, quindi fare clic su **Aggiungi**.  
  
    > [!NOTE]
    >  Al file di risorse è possibile assegnare un nome qualsiasi, purché abbia l'estensione .home.resx.  
  
3.  Per ogni attività o collegamento che si desidera aggiungere, occorre aggiungere anche stringhe e valori al file OEMHomePageContent.home.resx che corrispondano agli elementi Attività e Collegamento definiti nel file OEMHomePageContent.home. Nel seguente esempio di codice è riportato un esempio di file Tasks.xml strutturato per il file di risorse:  
  
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
  
4.  Aggiungere i nomi di risorsa MyTaskDescription, MyActionName e IconForAction al file .resx con i valori appropriati.  
  
5.  Salvare il file OEMHomePageContent.home.resx, quindi generare la soluzione.  
  
#####  <a name="BKMK_SignAssembly"></a> Firmare l'assembly con una firma Authenticode  
 Per poter utilizzare l'assembly nel sistema operativo, è necessario applicarvi una firma Authenticode. Per altre informazioni sulla firma dell'assembly, vedere [Firma e verifica del codice con Authenticode](https://msdn.microsoft.com/library/ms537364\(VS.85\).aspx#SignCode).  
  
##### <a name="install-the-task-files"></a>Installazione dei file delle attività  
 Una volta creati i file .home e .home.resx, occorre installarli sul server.  
  
###### <a name="to-install-the-task-files"></a>Per installare i file delle attività  
  
1.  Accertarsi che la soluzione venga generata senza errori.  
  
2.  Se non è stato creato un file di risorse incorporato, copiare il file OEMHomePageContent.home in **%Programmi%\Windows Server\Bin\Addins\Home** nel server. Se è stato creato un file di risorse incorporato, copiare il file OEMHomePageContent.dll in **%Programmi%\Windows Server\Bin\Addins\Home** nel server.  
  
## <a name="see-also"></a>Vedere anche  
 [Creazione e personalizzazione dell'immagine](Creating-and-Customizing-the-Image.md)   
 [Personalizzazioni aggiuntive](Additional-Customizations.md)   
 [Preparazione dell'immagine per la distribuzione](Preparing-the-Image-for-Deployment.md)   
 [Testare l'esperienza dei clienti](Testing-the-Customer-Experience.md)