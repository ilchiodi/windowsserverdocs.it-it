---
ms.assetid: ac727bd1-a892-47ed-a7ba-439b34187d4e
title: Descrizioni delle pagine delle procedure guidate di installazione e rimozione di Servizi di dominio Active Directory
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: 3563c30e86c53435c10cafc840a71c7b8c526943
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71391197"
---
# <a name="ad-ds-installation-and-removal-wizard-page-descriptions"></a>Descrizioni delle pagine delle procedure guidate di installazione e rimozione di Servizi di dominio Active Directory

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

In questo argomento vengono descritte le opzioni presenti nelle pagine seguenti delle procedure guidate, incluse le procedure di installazione e rimozione del ruolo server Servizi di dominio Active Directory in Server Manager.  
  
-   [Configurazione della distribuzione](../../ad-ds/deploy/AD-DS-Installation-and-Removal-Wizard-Page-Descriptions.md#BKMK_DepConfigPage)  
  
-   [Opzioni del controller di dominio](../../ad-ds/deploy/AD-DS-Installation-and-Removal-Wizard-Page-Descriptions.md#BKMK_DCOptionsPage)  
  
-   [Opzioni DNS](../../ad-ds/deploy/AD-DS-Installation-and-Removal-Wizard-Page-Descriptions.md#BKMK_DNSOptionsPage)  
  
-   [Opzioni RODC](../../ad-ds/deploy/AD-DS-Installation-and-Removal-Wizard-Page-Descriptions.md#BKMK_RODCOptionsPage)  
  
-   [Opzioni aggiuntive](../../ad-ds/deploy/AD-DS-Installation-and-Removal-Wizard-Page-Descriptions.md#BKMK_AdditionalOptionsPage)  
  
-   [Percorsi](../../ad-ds/deploy/AD-DS-Installation-and-Removal-Wizard-Page-Descriptions.md#BKMK_Paths)  
  
-   [Opzioni di preparazione](../../ad-ds/deploy/AD-DS-Installation-and-Removal-Wizard-Page-Descriptions.md#BKMK_AdprepCreds)  
  
-   [Opzioni di Revisione](../../ad-ds/deploy/AD-DS-Installation-and-Removal-Wizard-Page-Descriptions.md#BKMK_ViewInstallOptionsPage)  
  
-   [Controllo dei prerequisiti](../../ad-ds/deploy/AD-DS-Installation-and-Removal-Wizard-Page-Descriptions.md#BKMK_PrerqCheckPage)  
  
-   [Risultati](../../ad-ds/deploy/AD-DS-Installation-and-Removal-Wizard-Page-Descriptions.md#BKMK_Results)  
  
-   [Credenziali di rimozione ruolo](../../ad-ds/deploy/AD-DS-Installation-and-Removal-Wizard-Page-Descriptions.md#BKMK_RemovalCredsPage)  
  
-   [Opzioni e avvisi di rimozione di servizi di dominio Active Directory](../../ad-ds/deploy/AD-DS-Installation-and-Removal-Wizard-Page-Descriptions.md#BKMK_RemovalOptionsPage)  
  
-   [Nuova password amministratore](../../ad-ds/deploy/AD-DS-Installation-and-Removal-Wizard-Page-Descriptions.md#BKMK_NewAdminPwdPage)  
  
-   [Conferma selezioni per la rimozione del ruolo](../../ad-ds/deploy/AD-DS-Installation-and-Removal-Wizard-Page-Descriptions.md#BKMK_ConfirmRoleRemovalPage)  
  
## <a name="BKMK_DepConfigPage"></a>Configurazione della distribuzione  
Server Manager inizia l'installazione di ogni controller di dominio con il **configurazione della distribuzione** pagina. Le opzioni restanti e i campi obbligatori in questa pagina e nella pagine successive sono diversi a seconda dell'operazione di distribuzione selezionata. Ad esempio, se si crea una nuova foresta, il **Opzioni di preparazione** pagina non viene visualizzata, ma non se si installa il primo controller di dominio che esegue Windows Server 2012 in un dominio o foresta esistente.  
  
Alcuni test di convalida vengono eseguiti in questa pagina, mentre altri vengono effettuati successivamente nell'ambito del controllo dei prerequisiti. Ad esempio, se si tenta di installare il primo controller di dominio Windows Server 2012 in una foresta con livello di funzionalità di Windows 2000, in questa pagina viene visualizzato un errore.  
  
Quando viene creata una nuova foresta, vengono visualizzate le opzioni seguenti.  
  
![Installazione di AD DS](media/AD-DS-Installation-and-Removal-Wizard-Page-Descriptions/ADDS_SMI_DeploymentConfiguration_Forest.gif)  
  
-   Quando viene creata una nuova foresta, è necessario specificare un nome per il dominio radice della foresta. Non può essere il nome dominio radice della foresta con etichetta singola (ad esempio, deve essere "contoso.com" invece di "contoso"). È necessario usare le convenzioni di denominazione dei domini DNS consentite. È possibile specificare un nome IDN. Per ulteriori informazioni sulle convenzioni di denominazione dei domini DNS, vedere [KB 909264](https://support.microsoft.com/kb/909264).  
  
-   Non creare nuove foreste Active Directory con lo stesso nome del DNS esterno. Ad esempio, se l'URL DNS Internet è http: \//contoso. com, è necessario scegliere un nome diverso per la foresta interna per evitare problemi di compatibilità futuri. Il nome deve essere univoco e non scontato per il traffico Web, ad esempio corp.contoso.com.  
  
-   È necessario essere membro del gruppo Administrators nel server in cui si intende creare una nuova foresta.  
  
Per ulteriori informazioni su come creare un insieme di strutture, vedere [Installa una nuova foresta Windows Server 2012 Active Directory & #40; Livello 200 & #41;](../../ad-ds/deploy/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-.md).  
  
Quando viene creato un nuovo dominio, vengono visualizzate le opzioni seguenti.  
  
![Installazione di AD DS](media/AD-DS-Installation-and-Removal-Wizard-Page-Descriptions/ADDS_SMI_DeploymentConfiguration_ChildDomain.gif)  
  
> [!NOTE]  
> Se si crea un nuovo dominio albero, è necessario specificare il nome del dominio radice della foresta invece del dominio padre. Le restanti pagine e opzioni delle procedure guidate rimangono le stesse.  
  
-   Fare clic su **Seleziona** per selezionare il dominio padre o l'albero Active Directory, oppure specificare un dominio padre o un nome di albero valido. Digitare il nome del nuovo dominio in **nuovo nome di dominio**.  
  
-   Dominio albero: specificare un nome valido e completo per il dominio radice. Il nome non può avere un'etichetta singola e deve rispettare i requisiti DNS per i nomi di dominio.  
  
-   Dominio figlio: specificare un nome di dominio figlio con etichetta singola valido. Il nome deve rispettare i requisiti DNS per i nomi di dominio.  
  
-   La Configurazione guidata Servizi di dominio Active Directory richiede l'immissione delle credenziali per il dominio se le proprie attuali credenziali non appartengono al dominio. Fare clic su **Modifica** per fornire le credenziali di dominio.  
  
Per ulteriori informazioni su come creare un dominio, vedere [Installa un nuovo Windows Server 2012 figlio di Active Directory o dominio albero & #40; Livello 200 & #41;](../../ad-ds/deploy/Install-a-New-Windows-Server-2012-Active-Directory-Child-or-Tree-Domain--Level-200-.md).  
  
Quando si aggiunge un nuovo controller di dominio a un dominio esistente, vengono visualizzate le opzioni seguenti.  
  
![Installazione di AD DS](./media/AD-DS-Installation-and-Removal-Wizard-Page-Descriptions/ADDS_SMI_DeploymentConfiguration_Replica.gif)  
  
-   Fare clic su **selezionare** per selezionare il dominio o digitare un nome di dominio valido.  
  
-   Server Manager richiede di specificare delle credenziali valide, se necessario. Per installare un ulteriore controller di dominio, è necessario appartenere al gruppo Domain Admins.  
  
    Inoltre, l'installazione del primo controller di dominio che esegue Windows Server 2012 in una foresta richiede le credenziali che includano l'appartenenza a gruppi sia Enterprise Admins e Schema Admins. La Configurazione guidata Servizi di dominio Active Directory le richiede successivamente se le credenziali correnti non dispongono delle necessarie autorizzazioni o appartenenze ai gruppi.  
  
Per ulteriori informazioni su come aggiungere un controller di dominio a un dominio esistente, vedere [installare un Controller di dominio di Replica di Windows Server 2012 in un dominio esistente e 40 #; Livello 200 & #41;](../../ad-ds/deploy/Install-a-Replica-Windows-Server-2012-Domain-Controller-in-an-Existing-Domain--Level-200-.md).  
  
## <a name="BKMK_DCOptionsPage"></a>Opzioni del controller di dominio  
Se si crea una nuova foresta, nella pagina Opzioni controller di dominio sono disponibili le opzioni seguenti:  
  
![Installazione di AD DS](media/AD-DS-Installation-and-Removal-Wizard-Page-Descriptions/ADDS_SMI_DCOptions_Forest.gif)  
  
-   I livelli di funzionalità foresta e nel dominio a Windows Server 2012 impostazione predefinita.  
  
    È disponibile una nuova funzionalità a livello funzionale di dominio di Windows Server 2012: il supporto per controllo dinamico degli accessi e blindatura criteri modelli amministrativi KDC Kerberos ha due impostazioni (sempre forniscono attestazioni e Rifiuta richieste di autenticazione non blindate) che richiedono il livello funzionale del dominio di Windows Server 2012. Per ulteriori informazioni, vedere "Supporto di attestazioni, autenticazione composta e blindatura Kerberos" in [Novità dell'autenticazione Kerberos](https://technet.microsoft.com/library/hh831747.aspx).    
    Il livello di funzionalità foresta Windows Server 2012 non fornisce tutte le nuove funzionalità, ma garantisce che ogni nuovo dominio creato nella foresta venga automaticamente utilizzato a livello funzionale di dominio di Windows Server 2012. Il livello funzionale del dominio Windows Server 2012 non fornisce nuove caratteristiche oltre al supporto per il controllo dinamico degli accessi e blindatura Kerberos, ma garantisce che qualsiasi controller di dominio nel dominio esegue Windows Server 2012. Per altre informazioni sulle altre caratteristiche disponibili a livelli di funzionalità diversi, vedere [Informazioni sui livelli di funzionalità di Servizi di dominio Active Directory](../active-directory-functional-levels.md).  
  
    Oltre a livelli di funzionalità, un controller di dominio che esegue Windows Server 2012 fornisce funzionalità aggiuntive che non sono disponibili in un controller di dominio che esegue una versione precedente di Windows Server. Ad esempio, un controller di dominio che esegue Windows Server 2012 utilizzabile per la clonazione del controller di dominio virtuale, mentre non è un controller di dominio che esegue una versione precedente di Windows Server.  
  
-   Il server DNS viene selezionato per impostazione predefinita quando si crea una nuova foresta. Il primo controller di dominio nella foresta deve essere un server di catalogo globale e non può essere un controller di dominio di sola lettura.  
  
-   È necessario specificare la password per la modalità ripristino servizi directory (DSRM, Directory Service Restore Mode) per accedere ad un controller di dominio in cui Servizi di dominio Active Directory non è in esecuzione. La password specificata deve soddisfare i criteri password applicati al server, che per impostazione predefinita non richiedono una password complessa, ma semplicemente una password non vuota. Scegliere sempre una password complessa e di difficile individuazione o preferibilmente una passphrase. Per informazioni su come sincronizzare la password DSRM con la password dell'account utente di dominio, vedere [KB 961320](https://support.microsoft.com/kb/961320).  
  
Per ulteriori informazioni su come creare un insieme di strutture, vedere [Installa una nuova foresta Windows Server 2012 Active Directory & #40; Livello 200 & #41;](../../ad-ds/deploy/../../ad-ds/deploy/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-.md).  
  
Se si crea una dominio figlio, nella pagina Opzioni controller di dominio sono disponibili le opzioni seguenti:  
  
![Installazione di AD DS](media/AD-DS-Installation-and-Removal-Wizard-Page-Descriptions/ADDS_SMI_DCOptions_Child.gif)  
  
-   Il livello funzionale di dominio è impostato a Windows Server 2012 per impostazione predefinita. È possibile specificare qualunque altro valore almeno pari al valore del livello di funzionalità della foresta o superiore.  
  
-   Le opzioni configurabili per il controller di dominio includono **Server DNS** e **Catalogo globale**. Non è possibile configurare un controller di dominio di sola lettura come primo controller in un nuovo dominio.  
  
    È consigliabile che tutti i controller di dominio forniscano servizi DNS e di catalogo globale per garantire un'elevata disponibilità negli ambienti distribuiti, ragione per cui queste opzioni sono attivate per impostazione predefinita nella procedura guidata quando si crea un nuovo dominio.  
  
-   Nella pagina **Opzioni controller di dominio** è anche possibile scegliere dalla configurazione della foresta un **nome del sito** di Active Directory logico e appropriato. Per impostazione predefinita, viene selezionato il sito con la subnet più corretta. Se è presente un solo sito, il sito viene selezionato automaticamente.  
  
    > [!IMPORTANT]  
    > Se il server non fa parte di una subnet di Active Directory ed è presente più di un sito, viene selezionato nulla e **Avanti** pulsante non è disponibile fino a quando non si seleziona un sito dall'elenco.  
  
Per ulteriori informazioni su come creare un dominio, vedere [Installa un nuovo Windows Server 2012 figlio di Active Directory o dominio albero & #40; Livello 200 & #41;](../../ad-ds/deploy/../../ad-ds/deploy/../../ad-ds/deploy/Install-a-New-Windows-Server-2012-Active-Directory-Child-or-Tree-Domain--Level-200-.md).  
  
Se si aggiunge un controller di dominio a un dominio, nella pagina Opzioni controller di dominio sono disponibili le opzioni seguenti:  
  
![Installazione di AD DS](media/AD-DS-Installation-and-Removal-Wizard-Page-Descriptions/ADDS_SMI_DCOptions_Replica.gif)  
  
-   Le opzioni configurabili per il controller di dominio includono **Server DNS**, **Catalogo globale** e **Controller di dominio di sola lettura**.  
  
    È consigliabile che tutti i controller di dominio forniscano servizi DNS e di catalogo globale per garantire un'elevata disponibilità negli ambienti distribuiti, ragione per cui queste opzioni sono attivate per impostazione predefinita nella procedura guidata. Per ulteriori informazioni sulla distribuzione di controller, vedere [Guida alla distribuzione e pianificazione del Controller di dominio di sola lettura](https://technet.microsoft.com/library/cc771744(v=WS.10).aspx).  
  
Per ulteriori informazioni su come aggiungere un controller di dominio a un dominio esistente, vedere [installare un Controller di dominio di Replica di Windows Server 2012 in un dominio esistente e 40 #; Livello 200 & #41;](../../ad-ds/deploy/../../ad-ds/deploy/Install-a-Replica-Windows-Server-2012-Domain-Controller-in-an-Existing-Domain--Level-200-.md).  
  
## <a name="BKMK_DNSOptionsPage"></a>Opzioni DNS  
Se si installa il server DNS seguente **Opzioni DNS** verrà visualizzata la pagina:  
  
![Installazione di AD DS](media/AD-DS-Installation-and-Removal-Wizard-Page-Descriptions/ADDS_SMI_DNSOptions_Replica.gif)  
  
Quando si installa un server DNS, i record di delega che fanno riferimento al server DNS come autorevoli per la zona devono essere creati nella zona padre DNS. I record di delega trasferiscono l'autorità per la risoluzione dei nomi e forniscono un riferimento corretto agli altri server e client DNS dei nuovi server che diventano autorevoli per la nuova zona. Tali record di risorse sono i seguenti:  
  
-   Un record di risorse server dei nomi (NS) per effettuare la delega. Questo record di risorse annuncia che il server denominato ns1.na.example.microsoft.com è un server autorevole per il sottodominio delegato.  
  
-   Per risolvere il nome del server specificato nel record di risorse del server dei nomi (NS) nell'indirizzo IP, è necessario che sia presente un record di risorse host (A o AAAA) noto anche come record colla. Il processo di risoluzione del nome host di questo record di risorse nel server DNS delegato del record di risorse server dei nomi (NS) a volte viene indicato come glue chasing.  
  
È possibile fare in modo che la Configurazione guidata Servizi di dominio Active Directory li crei automaticamente. La procedura guidata verifica che il presenza dei record appropriati nella zona DNS padre dopo aver fatto clic **Avanti** sul **Opzioni Controller di dominio** pagina. Se non riesce a verificare la presenza dei record nel dominio padre, la procedura guidata dà la possibilità di creare una nuova delega DNS per un nuovo dominio (o di aggiornare una delega esistente) automaticamente e di continuare con l'installazione del nuovo controller di dominio.  
  
In alternativa, è possibile creare questi record di delega DNS prima di installare il server DNS. Per creare una delega di zona, aprire **gestore DNS**, pulsante destro del mouse sul dominio padre e quindi fare clic su **nuova delega**. Eseguire la procedura per la creazione della delega nell'Aggiunta guidata nuova delega.  
  
Il processo di installazione tenta di creare la delega per garantire che i computer in altri domini siano in grado di risolvere query DNS per gli host, inclusi i controller di dominio e i computer membro, nel sottodominio DNS. Si osservi che i record di delega possono essere creati automaticamente solo su server Microsoft DNS. Se la zona del dominio DNS padre si trova su server DNS di terze parti quali BIND, nella pagina Controllo dei prerequisiti viene visualizzato un avviso per segnalare che la creazione dei record di delega DNS ha avuto esito negativo. Per ulteriori informazioni sull'avviso, vedere [problemi noti per l'installazione di AD DS](https://technet.microsoft.com/library/cc754463(v=WS.10).aspx).  
  
Le deleghe tra il dominio padre e il sottodominio da promuovere possono essere create e convalidate prima o dopo l'installazione. Non vi è motivo di rimandare l'installazione di un nuovo controller di dominio a causa dell'impossibilità di creare o aggiornare la delega DNS.  
  
Per ulteriori informazioni sulla delega, vedere [informazioni sulla delega delle zone](https://go.microsoft.com/fwlink/?LinkId=164773) (https://go.microsoft.com/fwlink/?LinkId=164773). Se la delega della zona non è possibile nella situazione esistente, è possibile prendere in considerazione altri metodi per fornire la risoluzione dei nomi dagli altri domini agli host nel dominio. Ad esempio, l'amministratore DNS di un altro dominio potrebbe configurare l'inoltro condizionale, zone dello stub o zone secondarie per risolvere i nomi nel dominio. Per altre informazioni, vedere i seguenti argomenti:  
  
-   [Informazioni sui tipi di zona](https://go.microsoft.com/fwlink/?LinkID=157399) (https://go.microsoft.com/fwlink/?LinkID=157399)  
  
-   [Informazioni sulle zone Stub](https://go.microsoft.com/fwlink/?LinkId=164776) (https://go.microsoft.com/fwlink/?LinkId=164776)  
  
-   [Informazioni sui server d'inoltri](https://go.microsoft.com/fwlink/?LinkId=164778) (https://go.microsoft.com/fwlink/?LinkId=164778)  
  
## <a name="BKMK_RODCOptionsPage"></a>Opzioni RODC  
Quando viene creato un nuovo controller di dominio di sola lettura, vengono visualizzate le opzioni seguenti.  
  
![Installazione di AD DS](media/AD-DS-Installation-and-Removal-Wizard-Page-Descriptions/ADDS_SMI_RODCOptions.gif)  
  
-   Gli account amministratore con delega ottengono autorizzazioni amministrative locali per il controller di dominio di sola lettura. Questi utenti possono operare con privilegi equivalenti al gruppo Administrators del computer locale. Non sono membri del gruppo Domain Admins o del gruppo incorporato Administrators del dominio. Questa opzione risulta utile per delegare l'amministrazione in una succursale senza dover assegnare autorizzazioni amministrative per il dominio. La configurazione delle deleghe di amministrazione non è necessaria. Per ulteriori informazioni, vedere [la separazione dei ruoli di amministratore](https://technet.microsoft.com/library/cc753170(v=WS.10).aspx).  
  
-   Criteri di replica password funge da elenco di controllo di accesso (ACL) e determina se consentire al controller di dominio di sola lettura di archiviare una password nella cache. Quando riceve una richiesta di accesso da parte di un utente o di un computer autenticato, il controller di dominio di sola lettura fa riferimento a Criteri di replica password per stabilire se la password dell'account deve essere archiviata nella cache. Lo stesso account potrà successivamente eseguire gli accessi in modo più efficiente.  
  
    Criteri di replica password elenca gli account per i quali le password possono essere archiviate nella cache e gli account per i quali la archiviazione è espressamente negata. La presenza nell'elenco degli account di utenti e computer che è possibile archiviare nella cache non implica che il controller di dominio di sola lettura abbia necessariamente già archiviato nella cache le relative password. Un amministratore può ad esempio indicare in anticipo tutti gli account che il controller di dominio di sola lettura dovrà archiviare nella cache. In questo modo il controller di dominio di sola lettura può autenticare tali account, anche quando il collegamento WAN al sito hub è offline.  
  
    Gli utenti e i computer a cui l'autorizzazione non viene concessa (anche implicitamente) o viene negata non possono archiviare le password nella cache. Se tali utenti o computer non hanno accesso a un controller di dominio scrivibile, non possono accedere alle risorse e funzionalità fornite da Servizi di dominio Active Directory. Per ulteriori informazioni sui criteri di replica Password, vedere [Criteri replica Password](https://technet.microsoft.com/library/cc730883(v=ws.10).aspx). Per ulteriori informazioni sulla gestione dei criteri di replica Password, vedere [amministrazione dei criteri di replica Password](https://technet.microsoft.com/library/rodc-guidance-for-administering-the-password-replication-policy(v=ws.10).aspx).  
  
Per ulteriori informazioni sull'installazione di controller, vedere [Installa un Windows Server 2012 Active Directory un Controller di dominio & #40; RODC & #41; & #40; Livello 200 & #41;](../../ad-ds/deploy/RODC/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-.md).  
  
## <a name="BKMK_AdditionalOptionsPage"></a>Opzioni aggiuntive  
Quando si crea un nuovo dominio, nella pagina **Opzioni aggiuntive** vengono visualizzate le opzioni seguenti:  
  
![Installazione di AD DS](media/AD-DS-Installation-and-Removal-Wizard-Page-Descriptions/ADDS_SMI_AdditionalOptions_Child.gif)  
  
Vengono visualizzate le opzioni seguenti nel **Opzioni aggiuntive** pagina se si installa un controller di dominio aggiuntivo in un dominio esistente:  
  
![Installazione di AD DS](media/AD-DS-Installation-and-Removal-Wizard-Page-Descriptions/ADDS_SMI_AdditionalOptions_Replica.gif)  
  
-   È possibile specificare un controller di dominio come origine di replica o consentire alla procedura guidata di scegliere un qualsiasi controller di dominio come origine di replica.  
  
-   È inoltre possibile decidere di installare il controller di dominio usando i supporti di backup tramite l'opzione Installazione da supporto. Se il supporto di installazione è conservato localmente, l'opzione **Installa da supporto** consente di selezionare il percorso del file. L'opzione di selezione non è disponibile per le installazioni remote. È possibile fare clic su **Verifica** per assicurare che il percorso specificato si riferisca ad un supporto valido. Supporti utilizzati dall'opzione di installazione da Supporto devono essere creati con Windows Server Backup o Ntdsutil.exe da un altro esistente computer Windows Server 2012. è possibile utilizzare un Windows Server 2008 R2 o un sistema operativo precedente per creare un supporto per un controller di dominio di Windows Server 2012. Se il supporto è protetto da SYSKEY, Server Manager richiede la password dell'immagine durante la verifica.  
  
Per ulteriori informazioni su come creare un dominio, vedere [Installa un nuovo Windows Server 2012 figlio di Active Directory o dominio albero & #40; Livello 200 & #41;](../../ad-ds/deploy/../../ad-ds/deploy/../../ad-ds/deploy/Install-a-New-Windows-Server-2012-Active-Directory-Child-or-Tree-Domain--Level-200-.md). Per ulteriori informazioni su come aggiungere un controller di dominio a un dominio esistente, vedere [installare un Controller di dominio di Replica di Windows Server 2012 in un dominio esistente e 40 #; Livello 200 & #41;](../../ad-ds/deploy/../../ad-ds/deploy/Install-a-Replica-Windows-Server-2012-Domain-Controller-in-an-Existing-Domain--Level-200-.md).  
  
## <a name="BKMK_Paths"></a>Percorsi  
Vengono visualizzate le opzioni seguenti nel **percorsi** pagina.  
  
![Installazione di AD DS](media/AD-DS-Installation-and-Removal-Wizard-Page-Descriptions/ADDS_SMI_Paths.gif)  
  
-   Nella pagina **Percorsi** è possibile sostituire i percorsi predefiniti delle cartelle per il database di Servizi di dominio Active Directory, i registri delle transazioni del database e la condivisione SYSVOL. I percorsi predefiniti sono sempre impostati in %systemroot%.  
  
Specificare il percorso per il database di Servizi di dominio Active Directory (NTDS.DIT), i file di log e SYSVOL. Per le installazioni locali è possibile selezionare il percorso in cui si desidera archiviare i file.  
  
## <a name="BKMK_AdprepCreds"></a>Opzioni di preparazione  
![Installazione di AD DS](media/AD-DS-Installation-and-Removal-Wizard-Page-Descriptions/ADDS_SMI_PreparationOptions.gif)  
  
Se si è effettuato l'accesso con credenziali insufficienti per eseguire i comandi adprep.exe ed è necessario eseguire adprep per completare l'installazione di Servizi di dominio Active Directory, verrà richiesto di fornire le credenziali per eseguire adprep.exe. Adprep è necessario eseguire per aggiungere il primo controller di dominio che esegue Windows Server 2012 a un dominio o foresta esistente. In particolare:  
  
-   Per aggiungere il primo controller di dominio che esegue Windows Server 2012 a una foresta esistente, è necessario eseguire adprep /forestprep. Per eseguire questo comando è necessario essere membro dei gruppi Enterprise Admins, Schema Admins e Domain Admins per il dominio che ospita il master schema. Perché il comando abbia esito positivo, il computer in cui il comando viene eseguito deve essere connesso al master schema della foresta.  
  
-   Per aggiungere il primo controller di dominio che esegue Windows Server 2012 a un dominio esistente, è necessario eseguire adprep /domainprep. Questo comando deve essere eseguito da un membro del gruppo Domain Admins del dominio in cui si installa il controller di dominio che esegue Windows Server 2012. Perché il comando abbia esito positivo, il computer in cui il comando viene eseguito deve essere connesso al master infrastrutture del dominio.  
  
-   Adprep /rodcprep è il comando necessario per aggiungere il primo controller di dominio di sola lettura a una foresta esistente. Per eseguire questo comando è necessario essere membro del gruppo Enterprise Admins. Perché il comando abbia esito positivo, il computer in cui il comando viene eseguito deve essere connesso al master infrastrutture per ogni partizione di directory applicative nella foresta.  
  
Per ulteriori informazioni su Adprep.exe, vedere [Adprep.exe integrazione](../../ad-ds/deploy/What-s-New-in-Active-Directory-Domain-Services-Installation-and-Removal.md#BKMK_NewAdprep) e [in esecuzione Adprep.exe](https://technet.microsoft.com/library/dd464018(WS.10).aspx).  
  
## <a name="BKMK_ViewInstallOptionsPage"></a>Opzioni di Revisione  
![Installazione di AD DS](media/AD-DS-Installation-and-Removal-Wizard-Page-Descriptions/ADDS_SMI_ReviewOptions.gif)  
  
-   Nella pagina **Verifica opzioni** è possibile convalidare le impostazioni e accertarsi se soddisfano i requisiti prima di iniziare l'installazione. Questa non è l'ultima possibilità per interrompere l'installazione utilizzando Server Manager. La pagina consente semplicemente di rivedere e confermare le impostazioni prima di proseguire con la configurazione.  
  
-   La pagina **Verifica opzioni** di Server Manager include inoltre un pulsante opzionale **Visualizza script** , che consente di creare un file di testo Unicode contenente la configurazione ADDSDeployment corrente come singolo script di Windows PowerShell. In questo modo è possibile utilizzare l'interfaccia grafica di Server Manager come strumento di distribuzione di Windows PowerShell. Utilizzare la Configurazione guidata Servizi di dominio Active Directory per configurare le opzioni, esportare la configurazione e annullare la procedura guidata. Questo processo crea un esempio valido e sintatticamente corretto che può essere utilizzato direttamente o successivamente modificato.  
  
## <a name="BKMK_PrerqCheckPage"></a>Controllo dei prerequisiti  
![Installazione di AD DS](media/AD-DS-Installation-and-Removal-Wizard-Page-Descriptions/ADDS_SMI_PrerequisitesCheck.gif)  
  
Alcuni degli avvisi visualizzati in questa pagina includono:  
  
-   Controller di dominio che eseguono Windows Server 2008 o versioni successive dispongono di un'impostazione "Consenti algoritmi di crittografia compatibili con Windows NT 4" che impedisce gli algoritmi di crittografia vulnerabili per stabilire sessioni su canale sicuro. Per ulteriori informazioni sull'impatto potenziale e risolvere il problema, vedere l'articolo KB [942564](https://support.microsoft.com/kb/942564).  
  
-   Impossibile creare o aggiornare la delega DNS. Per ulteriori informazioni, vedere [Opzioni DNS](../../ad-ds/deploy/../../ad-ds/deploy/../../ad-ds/deploy/../../ad-ds/deploy/../../ad-ds/deploy/../../ad-ds/deploy/../../ad-ds/deploy/../../ad-ds/deploy/../../ad-ds/deploy/../../ad-ds/deploy/../../ad-ds/deploy/../../ad-ds/deploy/../../ad-ds/deploy/../../ad-ds/deploy/../../ad-ds/deploy/AD-DS-Installation-and-Removal-Wizard-Page-Descriptions.md#BKMK_DNSOptionsPage).  
  
-   Il controllo dei prerequisiti richiede chiamate WMI. Le chiamate possono avere esito negativo se sono bloccate dalle regole del firewall e restituire un errore di server RPC non disponibile.  
  
Per ulteriori informazioni sui controlli dei prerequisiti specifici eseguiti per l'installazione di Servizi di dominio Active Directory, vedere l'[articolo relativo ai test dei prerequisiti](../../ad-ds/manage/AD-DS-Simplified-Administration.md#BKMK_ADDSInstallPrerequisiteTests).  
  
## <a name="BKMK_Results"></a>Risultati  
![Installazione di AD DS](media/AD-DS-Installation-and-Removal-Wizard-Page-Descriptions/ADDS_SMI_SMResultsBeta.gif)  
  
In questa pagina è possibile rivedere i risultati dell'installazione.  
  
È inoltre possibile scegliere di riavviare il server di destinazione al termine della procedura guidata. Nel caso in cui l'installazione abbia esito positivo, il server verrà comunque riavviato indipendentemente dalla selezione di questa opzione. Talvolta, quando la procedura viene completata su un server di destinazione che non era stato aggiunto al dominio prima dell'installazione, lo stato del sistema del server di destinazione può rendere il server non raggiungibile nella rete o impedire all'utente di ottenere le autorizzazioni per gestire il server in remoto.  
  
In questo caso se il server di destinazione non viene riavviato, è necessario riavviarlo manualmente. Il server non può essere riavviato utilizzando funzionalità quali shutdown.exe o Windows PowerShell È possibile utilizzare Servizi Desktop remoto per eseguire l'accesso al server di destinazione ed arrestarlo in remoto.  
  
## <a name="BKMK_RemovalCredsPage"></a>Credenziali di rimozione ruolo  
![Installazione di AD DS](media/AD-DS-Installation-and-Removal-Wizard-Page-Descriptions/ADDS_RRW_Credentials.gif)  
  
Nella pagina **Credenziali** è possibile configurare le opzioni di abbassamento di livello. Fornire le credenziali necessarie per eseguire l'abbassamento di livello attenendosi all'elenco seguente:  
  
-   L'abbassamento di livello di un controller di dominio aggiuntivo richiede le credenziali Domain Admin. Se si seleziona **forza rimozione del controller di** dominio, il controller di dominio viene abbassato di pari senza rimuovere i metadati dell'oggetto controller di dominio da Active Directory.  
  
    > [!IMPORTANT]  
    > Non selezionare questa opzione se il controller di dominio non riesce a contattare altri controller di dominio e non vi è alcuna *soluzione ragionevole* per risolvere il problema di rete. L'abbassamento di livello forzato lascia orfani i metadati di Active Directory nei restanti controller di dominio della foresta. Inoltre tutte le modifiche non replicate nel rispettivo controller di dominio, come password o nuovi account utente, andranno perse. I metadati orfani sono la causa principale di una elevata percentuale di casi riportati al supporto tecnico Microsoft riguardanti i Servizi di dominio Active Directory, Exchange, SQL e altre applicazioni software. Quando si esegue l'abbassamento di livello forzato di un controller di dominio, è *necessario* effettuare immediatamente la pulizia dei metadati. Per i passaggi, esaminare [pulizia dei metadati del Server](https://technet.microsoft.com/library/cc816907(WS.10).aspx).  
  
-   Per abbassare il livello dell'ultimo controller di dominio in un dominio è necessario appartenere al gruppo Enterprise Admins, in quanto viene rimosso il dominio stesso (se questo è l'ultimo dominio nella foresta, viene rimossa anche la foresta). Server Manager segnala che il controller di dominio corrente è l'ultimo controller nel dominio. Selezionare **ultimo controller di dominio nel dominio** per verificare il dominio controller è l'ultimo controller di dominio nel dominio.  
  
Per ulteriori informazioni sulla rimozione di dominio Active Directory, vedere [rimuovere Active Directory Domain Services (livello 100)](assetId:///99b97af0-aa7e-41ed-8c81-4eee6c03eb4c) e [l'abbassamento di livello controller di dominio e domini & #40; Livello 200 & #41;](Demoting-Domain-Controllers-and-Domains--Level-200-.md).  
  
## <a name="BKMK_RemovalOptionsPage"></a>Opzioni e avvisi di rimozione di servizi di dominio Active Directory  
Per informazioni sulla pagina Verifica opzioni, vedere Verifica opzioni.  
  
Se il controller di dominio ospita ruoli aggiuntivi, come il ruolo Server DNS o il server di catalogo globale, viene visualizzata la pagina di avviso seguente:  
  
![Installazione di AD DS](media/AD-DS-Installation-and-Removal-Wizard-Page-Descriptions/ADDS_RRW_Warnings.gif)  
  
È necessario fare clic su **Procedi alla rimozione** per confermare che i ruoli aggiuntivi non saranno disponibili prima di scegliere **Avanti** per continuare.  
  
Se si forza la rimozione di un controller di dominio, andranno perse tutte le modifiche agli oggetti di Active Directory non replicate negli altri controller di dominio nel dominio. Se inoltre il controller di dominio ospita ruoli di master operazioni, il catalogo globale o il ruolo Server DNS, le operazioni critiche nel dominio e nella foresta possono essere influenzate come descritto di seguito. Prima di rimuovere un controller di dominio che ospita un ruolo di master operazioni, provare a trasferire il ruolo in un altro controller di dominio. Se non è possibile trasferire il ruolo, rimuovere Servizi di dominio Active Directory dal computer, quindi utilizzare Ntdsutil.exe per assegnare il ruolo. Utilizzare Ntdsutil sul controller di dominio a cui si intende assegnare il ruolo; se possibile, utilizzare un partner di replica recente appartenente allo stesso sito del controller di dominio. Per ulteriori informazioni sul trasferimento e requisizione dei ruoli di master operazioni, vedere [articolo 255504](https://go.microsoft.com/fwlink/?LinkId=80395) della Microsoft Knowledge Base. Se la procedura guidata non riesce a stabilire se il controller di dominio ospiti un ruolo di master operazioni, eseguire il comando netdom.exe per determinare se il controller di dominio esegua ruoli di master operazioni.  
  
-   Catalogo globale: È possibile che gli utenti abbiano problemi di accesso ai domini della foresta. Prima di rimuovere un server di catalogo globale, assicurarsi che vi sia un numero sufficiente di server di catalogo globale nella foresta e nel sito rispetto agli accessi utente. Se necessario, specificare un altro server di catalogo globale e aggiornare i client e le applicazioni con le nuove informazioni.  
  
-   Server DNS: Tutti i dati DNS archiviati in zone integrate Active Directory andranno perduti. Dopo avere rimosso Servizi di dominio Active Directory, questo server DNS non potrà eseguire la risoluzione dei nomi per le zone DNS integrate in Active Directory. È consigliabile pertanto aggiornare la configurazione DNS di tutti i computer che fanno riferimento all'indirizzo IP del server DNS per la risoluzione dei nomi con l'indirizzo IP del nuovo server DNS.  
  
-   Master infrastrutture: i client nel dominio potrebbero avere problemi di localizzazione degli oggetti negli altri domini. Prima di continuare, trasferire il ruolo master infrastrutture in un controller di dominio che non sia un server di catalogo globale.  
  
-   Master RID: potrebbero verificarsi problemi nella creazione di nuovi account utente, account di computer e gruppi di sicurezza. Prima di continuare, trasferire il ruolo di master RID in un controller di dominio nello stesso dominio del controller in uso.  
  
-   Emulatore PDC: non funzionano correttamente le operazioni eseguite dall'emulatore PDC, ad esempio gli aggiornamenti dei Criteri di gruppo e le reimpostazioni delle password per gli account diversi da Servizi di dominio Active Directory. Prima di continuare, trasferire il ruolo di master emulatore PDC in un controller di dominio nello stesso dominio del controller in uso.  
  
-   Master schema: non sarà più possibile modificare lo schema della foresta. Prima di continuare, trasferire il ruolo di master schema in un controller di dominio nel dominio radice della foresta.  
  
-   Master per la denominazione dei domini: non sarà più possibile aggiungere domini alla foresta o rimuovere i domini dalla foresta. Prima di continuare, trasferire il ruolo di master per la denominazione dei domini in un controller di dominio nel dominio radice della foresta.  
  
-   Tutte le partizioni di directory applicative in questo controller di dominio Active Directory verranno rimosse. Se un controller di dominio ospita l'ultima replica di una o più partizioni di directory applicative, al termine dell'operazione di rimozione tali partizioni non esisteranno più.  
  
Osservare che il dominio non esisterà più dopo la disinstallazione dei Servizi di dominio Active Directory dall'ultimo controller di dominio nel dominio.  
  
Se il controller di dominio è un server DNS delegato ad ospitare la zona DNS, nella pagina seguente si avrà la possibilità di rimuovere il server DNS dalla delega della zona DNS.  
  
![Installazione di AD DS](media/AD-DS-Installation-and-Removal-Wizard-Page-Descriptions/ADDS_RRW_RemovalOptions.gif)  
  
Per ulteriori informazioni sulla rimozione di dominio Active Directory, vedere [rimuovere Active Directory Domain Services (livello 100)](assetId:///99b97af0-aa7e-41ed-8c81-4eee6c03eb4c) e [l'abbassamento di livello controller di dominio e domini & #40; Livello 200 & #41;](Demoting-Domain-Controllers-and-Domains--Level-200-.md).  
  
## <a name="BKMK_NewAdminPwdPage"></a>Nuova password amministratore  
Il **Nuova Password amministratore** pagina è necessario fornire una password per account di amministratore del computer locale predefinito, una volta completata l'abbassamento di livello e il computer diventa un server membro del dominio o gruppo di lavoro.  
  
![Installazione di AD DS](media/AD-DS-Installation-and-Removal-Wizard-Page-Descriptions/ADDS_RRW_NewAdminPwd.gif)  
  
Per ulteriori informazioni sulla rimozione di dominio Active Directory, vedere [rimuovere Active Directory Domain Services (livello 100)](assetId:///99b97af0-aa7e-41ed-8c81-4eee6c03eb4c) e [l'abbassamento di livello controller di dominio e domini & #40; Livello 200 & #41;](Demoting-Domain-Controllers-and-Domains--Level-200-.md).  
  
## <a name="BKMK_ConfirmRoleRemovalPage"></a>Opzioni di Revisione  
Il **Verifica opzioni** pagina offre la possibilità di esportare le impostazioni di configurazione dell'abbassamento di livello a uno script di Windows PowerShell in modo che è possibile automatizzare l'abbassamento di livello aggiuntive. Fare clic su **Abbassa di livello** per rimuovere Servizi di dominio Active Directory.  
  
![Installazione di AD DS](media/AD-DS-Installation-and-Removal-Wizard-Page-Descriptions/ADDS_RRW_ReviewOptions.gif)  
  


