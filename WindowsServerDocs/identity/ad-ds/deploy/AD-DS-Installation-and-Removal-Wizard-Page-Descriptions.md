---
ms.assetid: ac727bd1-a892-47ed-a7ba-439b34187d4e
title: Installazione di Active Directory e le descrizioni delle pagine di rimozione guidata
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: fa023398822e79ca8c3e93d44bb1e87fc9190cee
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/12/2017
---
# <a name="ad-ds-installation-and-removal-wizard-page-descriptions"></a>Installazione di Active Directory e le descrizioni delle pagine di rimozione guidata

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

In questo argomento vengono descritte le opzioni nelle pagine seguenti della procedura guidata che costituiscono l'installazione del ruolo server Servizi di dominio Active Directory e la rimozione di Server Manager.  
  
-   [Configurazione della distribuzione](../../ad-ds/deploy/AD-DS-Installation-and-Removal-Wizard-Page-Descriptions.md#BKMK_DepConfigPage)  
  
-   [Opzioni Controller di dominio](../../ad-ds/deploy/AD-DS-Installation-and-Removal-Wizard-Page-Descriptions.md#BKMK_DCOptionsPage)  
  
-   [Opzioni DNS](../../ad-ds/deploy/AD-DS-Installation-and-Removal-Wizard-Page-Descriptions.md#BKMK_DNSOptionsPage)  
  
-   [Opzioni controller di dominio](../../ad-ds/deploy/AD-DS-Installation-and-Removal-Wizard-Page-Descriptions.md#BKMK_RODCOptionsPage)  
  
-   [Opzioni aggiuntive](../../ad-ds/deploy/AD-DS-Installation-and-Removal-Wizard-Page-Descriptions.md#BKMK_AdditionalOptionsPage)  
  
-   [Percorsi](../../ad-ds/deploy/AD-DS-Installation-and-Removal-Wizard-Page-Descriptions.md#BKMK_Paths)  
  
-   [Opzioni di preparazione](../../ad-ds/deploy/AD-DS-Installation-and-Removal-Wizard-Page-Descriptions.md#BKMK_AdprepCreds)  
  
-   [Verifica opzioni](../../ad-ds/deploy/AD-DS-Installation-and-Removal-Wizard-Page-Descriptions.md#BKMK_ViewInstallOptionsPage)  
  
-   [Controllo dei prerequisiti](../../ad-ds/deploy/AD-DS-Installation-and-Removal-Wizard-Page-Descriptions.md#BKMK_PrerqCheckPage)  
  
-   [Risultati](../../ad-ds/deploy/AD-DS-Installation-and-Removal-Wizard-Page-Descriptions.md#BKMK_Results)  
  
-   [Credenziali di rimozione ruolo](../../ad-ds/deploy/AD-DS-Installation-and-Removal-Wizard-Page-Descriptions.md#BKMK_RemovalCredsPage)  
  
-   [Avvisi e opzioni di rimozione di Active Directory](../../ad-ds/deploy/AD-DS-Installation-and-Removal-Wizard-Page-Descriptions.md#BKMK_RemovalOptionsPage)  
  
-   [Nuova Password amministratore](../../ad-ds/deploy/AD-DS-Installation-and-Removal-Wizard-Page-Descriptions.md#BKMK_NewAdminPwdPage)  
  
-   [Confermare le selezioni di rimozione ruolo](../../ad-ds/deploy/AD-DS-Installation-and-Removal-Wizard-Page-Descriptions.md#BKMK_ConfirmRoleRemovalPage)  
  
## <a name="BKMK_DepConfigPage"></a>Configurazione della distribuzione  
Server Manager inizia l'installazione di ogni controller di dominio con il **configurazione distribuzione** pagina. Le opzioni restanti e i campi obbligatori modificare in questa pagina e le pagine successive, a seconda operazione di distribuzione selezionato. Ad esempio, se si crea una nuova foresta, il **opzioni di preparazione** pagina non viene visualizzata, ma non se si installa il primo controller di dominio che esegue Windows Server 2012 in un dominio o foresta esistente.  
  
In questa pagina e successivamente nell'ambito del controllo dei prerequisiti, vengono eseguiti alcuni test di convalida. Ad esempio, se si tenta di installare il primo controller di dominio di Windows Server 2012 in una foresta con livello di funzionalità di Windows 2000, in questa pagina viene visualizzato un errore.  
  
Quando si crea una nuova foresta, vengono visualizzate le opzioni seguenti.  
  
![Installazione di AD DS](media/AD-DS-Installation-and-Removal-Wizard-Page-Descriptions/ADDS_SMI_DeploymentConfiguration_Forest.gif)  
  
-   Quando si crea una nuova foresta, è necessario specificare un nome per il dominio radice della foresta. Non può essere il nome dominio radice della foresta con etichetta singola (ad esempio, deve essere "contoso.com" invece di "contoso"). È necessario usare consentite convenzioni di denominazione dei domini DNS. È possibile specificare un nome di dominio IDN (Internationalized). For more information about DNS domain naming conventions, see [KB 909264](https://support.microsoft.com/kb/909264).  
  
-   Non creare nuove foreste Active Directory con lo stesso nome del nome DNS esterno. Ad esempio, se l'URL del DNS Internet è http://contoso.com, è necessario scegliere un nome diverso per la foresta interna evitare successivi problemi di compatibilità. Tale nome deve essere univoco e non scontato per il traffico web, ad esempio corp.contoso.com.  
  
-   È necessario essere un membro del gruppo Administrators nel server in cui si desidera creare una nuova foresta.  
  
Per ulteriori informazioni su come creare un insieme di strutture, vedere [installare una nuova foresta Windows Server 2012 Active Directory & #40; Livello 200 & #41;](../../ad-ds/deploy/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-.md).  
  
Quando si crea un nuovo dominio, vengono visualizzate le opzioni seguenti.  
  
![Installazione di AD DS](media/AD-DS-Installation-and-Removal-Wizard-Page-Descriptions/ADDS_SMI_DeploymentConfiguration_ChildDomain.gif)  
  
> [!NOTE]  
> Se si crea un nuovo dominio albero, è necessario specificare il nome del dominio radice della foresta invece del dominio padre, ma le pagine rimanenti della procedura guidata e le opzioni sono gli stessi.  
  
-   Fare clic su **selezionare** per individuare la struttura di Active Directory o il dominio padre o digitare un nome di dominio o di albero valido padre. Digitare il nome del nuovo dominio in **nuovo nome di dominio**.  
  
-   Dominio albero: specificare un nome di dominio radice valido e completo. il nome non può essere con etichetta singola e deve rispettare requisiti dei nomi di dominio DNS.  
  
-   Dominio figlio: specificare un figlio etichetta singola valido nome di dominio. il nome deve rispettare i requisiti dei nomi di dominio DNS.  
  
-   La configurazione guidata servizi di dominio Active Directory richiede l'immissione delle credenziali di dominio se le credenziali correnti non dal dominio. Fare clic su **modifica** per fornire le credenziali di dominio.  
  
Per ulteriori informazioni su come creare un dominio, vedere [installare un nuovo Windows Server 2012 figlio di Active Directory o dominio albero & #40; Livello 200 & #41;](../../ad-ds/deploy/Install-a-New-Windows-Server-2012-Active-Directory-Child-or-Tree-Domain--Level-200-.md).  
  
Quando si aggiunge un nuovo controller di dominio a un dominio esistente, vengono visualizzate le opzioni seguenti.  
  
![Installazione di AD DS](./media/AD-DS-Installation-and-Removal-Wizard-Page-Descriptions/ADDS_SMI_DeploymentConfiguration_Replica.gif)  
  
-   Fare clic su **selezionare** per selezionare il dominio o digitare un nome di dominio valido.  
  
-   Server Manager richiede le credenziali valide se necessario. L'installazione di un controller di dominio aggiuntivo richiede l'appartenenza al gruppo Domain Admins.  
  
    Inoltre, l'installazione del primo controller di dominio che esegue Windows Server 2012 in una foresta richiede le credenziali che includano l'appartenenza di gruppi Enterprise Admins e Schema Admins. La configurazione guidata servizi di dominio Active Directory richiede successivamente se le credenziali correnti non dispongono di autorizzazioni adeguate o appartenenza al gruppo.  
  
Per ulteriori informazioni su come aggiungere un controller di dominio a un dominio esistente, vedere [installare un Controller di dominio di Replica Windows Server 2012 in un dominio esistente e 40 #; Livello 200 & #41;](../../ad-ds/deploy/Install-a-Replica-Windows-Server-2012-Domain-Controller-in-an-Existing-Domain--Level-200-.md).  
  
## <a name="BKMK_DCOptionsPage"></a>Opzioni Controller di dominio  
Se si sta creando una nuova foresta, la pagina Opzioni Controller di dominio è disponibili le opzioni:  
  
![Installazione di AD DS](media/AD-DS-Installation-and-Removal-Wizard-Page-Descriptions/ADDS_SMI_DCOptions_Forest.gif)  
  
-   I livelli di funzionalità foresta e del dominio a Windows Server 2012 impostazione predefinita.  
  
    È disponibile una nuova funzionalità a livello funzionale di dominio di Windows Server 2012: il supporto per controllo dinamico degli accessi e blindatura criteri modelli amministrativi KDC Kerberos ha due impostazioni (sempre forniscono attestazioni e Rifiuta richieste di autenticazione non blindate) che richiedono il livello di funzionalità dominio Windows Server 2012. For more information, see "Support for claims, compound authentication and Kerberos armoring" in [What's new in Kerberos Authentication](https://technet.microsoft.com/library/hh831747.aspx).    
    Il livello di funzionalità foresta Windows Server 2012 non fornisce nuove funzionalità, ma garantisce che ogni nuovo dominio creato nella foresta venga automaticamente utilizzato a livello funzionale di dominio di Windows Server 2012. Il livello di funzionalità del dominio Windows Server 2012 non fornisce nuove caratteristiche oltre al supporto per controllo dinamico degli accessi e blindatura Kerberos, ma garantisce che qualsiasi controller di dominio nel dominio esegue Windows Server 2012. Per ulteriori informazioni sulle altre funzionalità che sono disponibili a diversi livelli funzionali, vedere [livelli di funzionalità di servizi di dominio Active Directory informazioni (AD DS)](../active-directory-functional-levels.md).  
  
    Oltre a livelli di funzionalità, un controller di dominio che esegue Windows Server 2012 fornisce funzionalità aggiuntive che non sono disponibili in un controller di dominio che esegue una versione precedente di Windows Server. Ad esempio, un controller di dominio che esegue Windows Server 2012 utilizzabile per la clonazione del controller di dominio virtuale, operazione Impossibile con un controller di dominio che esegue una versione precedente di Windows Server.  
  
-   Server DNS è selezionato per impostazione predefinita quando si crea una nuova foresta. Il primo controller di dominio nella foresta deve essere un server di catalogo globale (GC) e non può essere un controller di dominio solo lettura (RODC).  
  
-   La password modalità ripristino servizi Directory (DSRM) è necessaria per accedere a un controller di dominio in cui servizi di dominio Active Directory non è in esecuzione. La password specificata deve soddisfare i criteri password applicati al server, che per impostazione predefinita non richiedono una password complessa; solo una password non vuota. Scegliere sempre una password complessa o preferibilmente una passphrase. For information about how to synchronize the DSRM password with the password of a domain user account, see [KB 961320](https://support.microsoft.com/kb/961320).  
  
Per ulteriori informazioni su come creare un insieme di strutture, vedere [installare una nuova foresta Windows Server 2012 Active Directory & #40; Livello 200 & #41;](../../ad-ds/deploy/../../ad-ds/deploy/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-.md).  
  
Se si sta creando un dominio figlio, la pagina Opzioni Controller di dominio è disponibili le opzioni:  
  
![Installazione di AD DS](media/AD-DS-Installation-and-Removal-Wizard-Page-Descriptions/ADDS_SMI_DCOptions_Child.gif)  
  
-   Il livello funzionale di dominio è impostato a Windows Server 2012 per impostazione predefinita. È possibile specificare qualsiasi altro valore almeno il valore del livello di funzionalità foresta o superiore.  
  
-   Le opzioni controller di dominio configurabili includono **server DNS** e **catalogo globale**; è possibile configurare i controller di dominio di sola lettura come primo controller di dominio in un nuovo dominio.  
  
    È consigliabile che tutti i controller di dominio forniscano servizi DNS e catalogo globale per garantire un'elevata disponibilità negli ambienti distribuiti, ragione per cui queste opzioni sono attivate la procedura guidata per impostazione predefinita quando si crea un nuovo dominio.  
  
-   Il **opzioni Controller di dominio** pagina è inoltre possibile scegliere di Active Directory appropriato logica **nome sito** dalla configurazione della foresta. Per impostazione predefinita, viene selezionato il sito con la subnet più corretta. Se è presente un solo sito, il sito viene selezionato automaticamente.  
  
    > [!IMPORTANT]  
    > Se il server non fa parte di una subnet di Active Directory ed è presente più di un sito, viene selezionato nulla e il **Avanti** pulsante non è disponibile fino a quando non viene scelto un sito dall'elenco.  
  
Per ulteriori informazioni su come creare un dominio, vedere [installare un nuovo Windows Server 2012 figlio di Active Directory o dominio albero & #40; Livello 200 & #41;](../../ad-ds/deploy/../../ad-ds/deploy/../../ad-ds/deploy/Install-a-New-Windows-Server-2012-Active-Directory-Child-or-Tree-Domain--Level-200-.md).  
  
Se vuoi aggiungere un controller di dominio a un dominio, la pagina Opzioni Controller di dominio è disponibili le opzioni:  
  
![Installazione di AD DS](media/AD-DS-Installation-and-Removal-Wizard-Page-Descriptions/ADDS_SMI_DCOptions_Replica.gif)  
  
-   Le opzioni controller di dominio configurabili includono **server DNS** e **catalogo globale**, e **controller di dominio di sola lettura**.  
  
    È consigliabile che tutti i controller di dominio forniscano servizi DNS e catalogo globale per garantire un'elevata disponibilità negli ambienti distribuiti, ragione per cui la procedura guidata di queste opzioni sono attivate per impostazione predefinita. For more information about deploying RODCs, see [Read-Only Domain Controller Planning and Deployment Guide](https://technet.microsoft.com/library/cc771744(v=WS.10).aspx).  
  
Per ulteriori informazioni su come aggiungere un controller di dominio a un dominio esistente, vedere [installare un Controller di dominio di Replica Windows Server 2012 in un dominio esistente e 40 #; Livello 200 & #41;](../../ad-ds/deploy/../../ad-ds/deploy/Install-a-Replica-Windows-Server-2012-Domain-Controller-in-an-Existing-Domain--Level-200-.md).  
  
## <a name="BKMK_DNSOptionsPage"></a>Opzioni DNS  
Se si installa un server DNS, il comando seguente **opzioni DNS** verrà visualizzata la pagina:  
  
![Installazione di AD DS](media/AD-DS-Installation-and-Removal-Wizard-Page-Descriptions/ADDS_SMI_DNSOptions_Replica.gif)  
  
Quando si installa il server DNS, record di delega che fanno riferimento al server DNS come autorevoli per la zona deve essere creato nella zona padre sistema DNS (Domain Name). Record di delega trasferire autorità di risoluzione dei nomi e forniscono un riferimento corretto agli altri server DNS e i client dei nuovi server che diventano autorevoli per la nuova zona. Questi record di risorse includono quanto segue:  
  
-   Un record di risorse nome server (NS) per effettuare la delega. Questo record di risorse annuncia che il server denominato ns1.na.example.microsoft.com è un server autorevole per il sottodominio delegato.  
  
-   Un record di risorse host (A o AAAA) noto anche come glue record deve essere presente per risolvere il nome del server specificato nel record di risorse nome server (NS) nel relativo indirizzo IP. Il processo di risoluzione del nome host di questo record di risorse per il server DNS delegato nel record di risorse nome server (NS) viene talvolta definito come "glue chasing."  
  
Si può avere la configurazione guidata servizi di dominio Active Directory li crei automaticamente. La procedura guidata verifica che il presenza dei record appropriati nella zona DNS padre dopo aver fatto clic **Avanti** nel **opzioni Controller di dominio** pagina. Se la procedura guidata è possibile verificare la presenza dei record nel dominio padre, la procedura guidata fornisce automaticamente con l'opzione per creare una nuova delega DNS per un nuovo dominio (o aggiornare una delega esistente) e continuare con la nuova installazione di controller di dominio.  
  
In alternativa, è possibile creare questi record di delega DNS prima di installare il server DNS. Per creare una delega di zona, aprire **gestore DNS**, pulsante destro del mouse sul dominio padre e quindi fare clic su **nuova delega**. Seguire i passaggi nella procedura guidata nuova delega per creare la delega.  
  
Il processo di installazione tenta di creare la delega per garantire che i computer in altri domini possono risolvere query DNS per gli host, inclusi i controller di dominio e i computer membro, nel sottodominio DNS. Si noti che il record di delega possono essere creati automaticamente solo su server DNS Microsoft. Se la zona del dominio DNS padre si trova nel server DNS di terze parti quali BIND, nella pagina controllo dei prerequisiti viene visualizzato un avviso sull'errore per creare record di delega DNS. For more information about the warning, see [Known issues for installing AD DS](https://technet.microsoft.com/library/cc754463(v=WS.10).aspx).  
  
Le deleghe tra il dominio padre e il sottodominio da promuovere possono essere create e convalidate prima o dopo l'installazione. Non c'è alcun motivo per ritardare l'installazione di un nuovo controller di dominio perché non è possibile creare o aggiornare la delega DNS.  
  
For more information about delegation, see [Understanding Zone Delegation](https://go.microsoft.com/fwlink/?LinkId=164773) (https://go.microsoft.com/fwlink/?LinkId=164773). Se la delega di zona non è possibile nella situazione esistente, è possibile altri metodi per fornire la risoluzione dei nomi di altri domini agli host nel dominio. Ad esempio, l'amministratore DNS di un altro dominio potrebbe configurare inoltro condizionale, zone dello stub o secondaria zone per risolvere i nomi del dominio. Per ulteriori informazioni, vedere gli argomenti seguenti:  
  
-   [Understanding zone types](https://go.microsoft.com/fwlink/?LinkID=157399) (https://go.microsoft.com/fwlink/?LinkID=157399)  
  
-   [Understanding stub zones](https://go.microsoft.com/fwlink/?LinkId=164776) (https://go.microsoft.com/fwlink/?LinkId=164776)  
  
-   [Understanding forwarders](https://go.microsoft.com/fwlink/?LinkId=164778) (https://go.microsoft.com/fwlink/?LinkId=164778)  
  
## <a name="BKMK_RODCOptionsPage"></a>Opzioni controller di dominio  
Quando si installa un controller di dominio di sola lettura (RODC), vengono visualizzate le opzioni seguenti.  
  
![Installazione di AD DS](media/AD-DS-Installation-and-Removal-Wizard-Page-Descriptions/ADDS_SMI_RODCOptions.gif)  
  
-   Gli account di amministratore delegato ottengono autorizzazioni amministrative locali per tale controller di dominio. Questi utenti possono operare con privilegi equivalenti al gruppo Administrators del computer locale. Non sono membri del gruppo Domain Admins o il gruppo incorporato Administrators del dominio. Questa opzione è utile per delegare l'amministrazione della succursale senza concedere le autorizzazioni amministrative di dominio. Configurare la delega dell'amministrazione non è obbligatorio. For more information, see [Administrator Role Separation](https://technet.microsoft.com/library/cc753170(v=WS.10).aspx).  
  
-   Criteri di replica Password funge da un elenco di controllo di accesso (ACL). Determina se un controller di dominio di memorizzare nella cache di una password. Dopo che tale controller di dominio riceve una richiesta di accesso utente o computer autenticata, fa riferimento a criteri di replica Password per determinare se la password per l'account deve essere memorizzate nella cache. Lo stesso account quindi possibile eseguire in modo più efficiente gli accessi successivi.  
  
    Le Password dei criteri di replica sono elencati gli account le cui password possono essere memorizzate nella cache e gli account le cui password viene negata esplicitamente dalla memorizzazione nella cache. L'elenco di account utente e computer che possono essere memorizzate nella cache non implica che tale controller di dominio è necessariamente memorizzato nella cache le password per tali account. Un amministratore può, ad esempio, specificare in anticipo tutti gli account che memorizza nella cache un RODC. In questo modo, la sola lettura può autenticare tali account, anche se il collegamento WAN al sito hub è offline.  
  
    Gli utenti o computer che non sono consentiti (anche implicitamente) o negato l'accesso non memorizzare nella cache le password. Se tali utenti o computer non hanno accesso a un controller di dominio scrivibile, non possono accedere a risorse di AD DS-condizione o funzionalità. For more information about the PRP, see [Password Replication Policy](https://technet.microsoft.com/library/cc730883(v=ws.10).aspx). For more information about managing the PRP, see [Administering the Password Replication Policy](https://technet.microsoft.com/library/rodc-guidance-for-administering-the-password-replication-policy(v=ws.10).aspx).  
  
Per ulteriori informazioni sull'installazione di controller, vedere [installa un Windows Server 2012 Active Directory Read-Only Controller di dominio & #40; RODC & #41; & #40; Livello 200 & #41;](../../ad-ds/deploy/RODC/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-.md).  
  
## <a name="BKMK_AdditionalOptionsPage"></a>Opzioni aggiuntive  
L'opzione seguente viene visualizzato nel **opzioni aggiuntive** pagina se si sta creando un nuovo dominio:  
  
![Installazione di AD DS](media/AD-DS-Installation-and-Removal-Wizard-Page-Descriptions/ADDS_SMI_AdditionalOptions_Child.gif)  
  
Vengono visualizzate le opzioni seguenti nel **opzioni aggiuntive** pagina se si installa un controller di dominio aggiuntivo in un dominio esistente:  
  
![Installazione di AD DS](media/AD-DS-Installation-and-Removal-Wizard-Page-Descriptions/ADDS_SMI_AdditionalOptions_Replica.gif)  
  
-   È possibile specificare un controller di dominio come origine della replica o consentire alla procedura guidata scegliere qualsiasi controller di dominio come origine della replica.  
  
-   È inoltre possibile installare il controller di dominio usando supporti utilizzando l'installazione dall'opzione di supporto di backup. Se il supporto di installazione è archiviato in locale, il **installa da supporto** opzione consente di passare al percorso del file. L'opzione di selezione non è disponibile per un'installazione remota. È possibile fare clic su **verifica** per verificare il percorso specificato sia un supporto valido. Supporti utilizzati dall'opzione di installazione da supporto devono essere creati con Windows Server Backup o Ntdsutil.exe da un altro esistente computer Windows Server 2012. è possibile utilizzare un Windows Server 2008 R2 o un sistema operativo precedente per creare il supporto per un controller di dominio Windows Server 2012. Se il supporto è protetto da SYSKEY, Server Manager richiede la password dell'immagine durante la verifica.  
  
Per ulteriori informazioni su come creare un dominio, vedere [installare un nuovo Windows Server 2012 figlio di Active Directory o dominio albero & #40; Livello 200 & #41;](../../ad-ds/deploy/../../ad-ds/deploy/../../ad-ds/deploy/Install-a-New-Windows-Server-2012-Active-Directory-Child-or-Tree-Domain--Level-200-.md). Per ulteriori informazioni su come aggiungere un controller di dominio a un dominio esistente, vedere [installare un Controller di dominio di Replica Windows Server 2012 in un dominio esistente e 40 #; Livello 200 & #41;](../../ad-ds/deploy/../../ad-ds/deploy/Install-a-Replica-Windows-Server-2012-Domain-Controller-in-an-Existing-Domain--Level-200-.md).  
  
## <a name="BKMK_Paths"></a>Percorsi  
Vengono visualizzate le opzioni seguenti nel **percorsi** pagina.  
  
![Installazione di AD DS](media/AD-DS-Installation-and-Removal-Wizard-Page-Descriptions/ADDS_SMI_Paths.gif)  
  
-   Il **percorsi** pagina consente di sostituire i percorsi predefiniti del database di Active Directory, i registri delle transazioni di database, e la condivisione SYSVOL. I percorsi predefiniti sono sempre in % systemroot %.  
  
Specificare il percorso per il database di Active Directory (NTDS.DIT), file di registro e SYSVOL. Per un'installazione locale, è possibile selezionare il percorso in cui si desidera archiviare i file.  
  
## <a name="BKMK_AdprepCreds"></a>Opzioni di preparazione  
![Installazione di AD DS](media/AD-DS-Installation-and-Removal-Wizard-Page-Descriptions/ADDS_SMI_PreparationOptions.gif)  
  
Se si non è attualmente connessi con credenziali insufficienti per eseguire i comandi adprep.exe e adprep è necessario eseguire per completare l'installazione di servizi di dominio Active Directory, viene chiesto di fornire le credenziali per eseguire adprep.exe. Adprep è necessario eseguire per aggiungere il primo controller di dominio che esegue Windows Server 2012 a un dominio o foresta esistente. In particolare:  
  
-   Adprep /forestprep deve essere eseguito per aggiungere il primo controller di dominio che esegue Windows Server 2012 a una foresta esistente. Questo comando deve essere eseguito da un membro del gruppo Enterprise Admins, gruppo Schema Admins e il gruppo Domain Admins del dominio che ospita il master schema. Per questo comando abbia completato correttamente, deve essere la connettività tra il computer in cui si esegue il comando e il master schema della foresta.  
  
-   Adprep /domainprep deve essere eseguito per aggiungere il primo controller di dominio che esegue Windows Server 2012 a un dominio esistente. Questo comando deve essere eseguito da un membro del gruppo Domain Admins del dominio in cui si installa il controller di dominio che esegue Windows Server 2012. Per questo comando abbia completato correttamente, deve essere la connettività tra il computer in cui si esegue il comando e il master infrastrutture per il dominio.  
  
-   Adprep /rodcprep deve essere eseguito per aggiungere il primo controller di dominio a una foresta esistente. Questo comando deve essere eseguito da un membro del gruppo Enterprise Admins. Per questo comando abbia completato correttamente, deve essere la connettività tra il computer in cui si esegue il comando e il master infrastrutture per ogni partizione di directory applicativa nella foresta.  
  
For more information about Adprep.exe, see [Adprep.exe integration](../../ad-ds/deploy/What-s-New-in-Active-Directory-Domain-Services-Installation-and-Removal.md#BKMK_NewAdprep) and see [Running Adprep.exe](https://technet.microsoft.com/library/dd464018(WS.10).aspx).  
  
## <a name="BKMK_ViewInstallOptionsPage"></a>Verifica opzioni  
![Installazione di AD DS](media/AD-DS-Installation-and-Removal-Wizard-Page-Descriptions/ADDS_SMI_ReviewOptions.gif)  
  
-   Il **verifica opzioni** pagina consente di convalidare le impostazioni e accertarsi se soddisfano i requisiti prima di iniziare l'installazione. Questo non è l'ultima possibilità per interrompere l'installazione utilizzando Server Manager. Questa pagina consente semplicemente di rivedere e confermare le impostazioni prima di proseguire con la configurazione.  
  
-   Il **verifica opzioni** pagina in Server Manager offre anche un facoltativo **Visualizza Script** pulsante per creare un file di testo Unicode contenente la configurazione ADDSDeployment corrente come singolo script di Windows PowerShell. In questo modo è possibile utilizzare l'interfaccia grafica di Server Manager come un strumento di distribuzione di Windows PowerShell. Utilizzare la configurazione guidata servizi di dominio Active Directory per configurare le opzioni, esportare la configurazione e quindi annullare la procedura guidata. Questo processo crea un esempio valido e sintatticamente corretto utilizzato direttamente o successivamente modificato.  
  
## <a name="BKMK_PrerqCheckPage"></a>Controllo dei prerequisiti  
![Installazione di AD DS](media/AD-DS-Installation-and-Removal-Wizard-Page-Descriptions/ADDS_SMI_PrerequisitesCheck.gif)  
  
Gli avvisi visualizzati in questa pagina includono:  
  
-   Controller di dominio che eseguono Windows Server 2008 o versioni successive dispongono di un'impostazione "Consenti algoritmi di crittografia compatibili con Windows NT 4" che impedisce gli algoritmi di crittografia vulnerabili per stabilire sessioni su canale sicuro. For more information about the potential impact and a workaround, see KB article [942564](https://support.microsoft.com/kb/942564).  
  
-   Impossibile creare o aggiornare la delega DNS. Per ulteriori informazioni, vedere [opzioni DNS](../../ad-ds/deploy/../../ad-ds/deploy/../../ad-ds/deploy/../../ad-ds/deploy/../../ad-ds/deploy/../../ad-ds/deploy/../../ad-ds/deploy/../../ad-ds/deploy/../../ad-ds/deploy/../../ad-ds/deploy/../../ad-ds/deploy/../../ad-ds/deploy/../../ad-ds/deploy/../../ad-ds/deploy/../../ad-ds/deploy/AD-DS-Installation-and-Removal-Wizard-Page-Descriptions.md#BKMK_DNSOptionsPage).  
  
-   Il controllo dei prerequisiti richiede chiamate WMI. Può avere esito negativo se sono blocco regole firewall bloccati e restituire un server RPC non disponibile.  
  
Per ulteriori informazioni sui controlli dei prerequisiti specifici eseguiti per l'installazione di Active Directory, vedere [test dei prerequisiti](../../ad-ds/manage/AD-DS-Simplified-Administration.md#BKMK_ADDSInstallPrerequisiteTests).  
  
## <a name="BKMK_Results"></a>Risultati  
![Installazione di AD DS](media/AD-DS-Installation-and-Removal-Wizard-Page-Descriptions/ADDS_SMI_SMResultsBeta.gif)  
  
In questa pagina, è possibile esaminare i risultati dell'installazione.  
  
È anche possibile scegliere di riavviare il server di destinazione dopo aver completato la procedura guidata, ma se l'installazione ha esito positivo, il server verrà comunque riavviato indipendentemente dal fatto che si seleziona questa opzione. In alcuni casi dopo aver completato la procedura guidata in un server di destinazione che non è stato aggiunto al dominio prima dell'installazione, lo stato del sistema del server di destinazione può rendere il server non raggiungibile in rete o lo stato del sistema può impedire che dispongono delle autorizzazioni per gestire il server remoto.  
  
Se il server di destinazione non viene riavviato in questo caso, è necessario riavviarlo manualmente. Strumenti quali shutdown.exe o Windows PowerShell non riavviarlo. È possibile utilizzare Servizi Desktop remoto per accedere e spegnere in modalità remota il server di destinazione.  
  
## <a name="BKMK_RemovalCredsPage"></a>Credenziali di rimozione ruolo  
![Installazione di AD DS](media/AD-DS-Installation-and-Removal-Wizard-Page-Descriptions/ADDS_RRW_Credentials.gif)  
  
Configurare le opzioni di abbassamento di livello nel **credenziali** pagina. Fornire le credenziali necessarie per eseguire l'abbassamento di livello dall'elenco seguente:  
  
-   L'abbassamento di livello un controller di dominio aggiuntivo richiede le credenziali di amministratore di dominio. Selezione **forzare la rimozione del controller di dominio** viene abbassato di livello il controller di dominio senza rimuovere i metadati dell'oggetto controller di dominio da Active Directory.  
  
    > [!IMPORTANT]  
    > Non selezionare questa opzione a meno che il controller di dominio non riesce a contattare altri controller di dominio e non esiste *in alcun modo ragionevole* per risolvere il problema di rete. Abbassamento di livello forzato lascia orfani i metadati in Active Directory nei restanti controller di dominio nella foresta. Inoltre, tutte le modifiche non replicate nel controller di dominio, ad esempio le password o nuovi account utente, andranno perse. I metadati orfani sono la causa principale di un'elevata percentuale di casi di supporto tecnico clienti Microsoft per servizi di dominio Active Directory, Exchange, SQL e altro software. Se si forzatamente abbassare di livello un controller di dominio è *deve* manualmente eseguire immediatamente la pulizia dei metadati. For steps, review [Clean Up Server Metadata](https://technet.microsoft.com/library/cc816907(WS.10).aspx).  
  
-   Abbassare di livello l'ultimo controller di dominio in un dominio è necessario appartenere al gruppo Enterprise Admins, quanto viene rimosso il dominio stesso (se questo è l'ultimo dominio nella foresta, questo criterio rimuove la foresta). Server Manager segnala che il controller di dominio corrente è l'ultimo controller di dominio nel dominio. Selezionare **ultimo controller di dominio nel dominio** per verificare il dominio controller è l'ultimo controller di dominio nel dominio.  
  
Per ulteriori informazioni sulla rimozione di dominio Active Directory, vedere [rimuovere Active Directory Domain Services (livello 100)](assetId:///99b97af0-aa7e-41ed-8c81-4eee6c03eb4c) e [abbassamento di livello controller di dominio e domini & #40; Livello 200 & #41;](Demoting-Domain-Controllers-and-Domains--Level-200-.md).  
  
## <a name="BKMK_RemovalOptionsPage"></a>Avvisi e opzioni di rimozione di Active Directory  
Se hai bisogno di assistenza per la pagina Verifica opzioni, vedere Verifica opzioni.  
  
Se il controller di dominio ospita ruoli aggiuntivi, ad esempio il ruolo server DNS o il server di catalogo globale, viene visualizzata la pagina avviso seguente:  
  
![Installazione di AD DS](media/AD-DS-Installation-and-Removal-Wizard-Page-Descriptions/ADDS_RRW_Warnings.gif)  
  
È necessario fare clic **Procedi alla rimozione** per confermare che i ruoli aggiuntivi non saranno più disponibili prima di scegliere **Avanti** per continuare.  
  
Se si forza la rimozione di un controller di dominio, le modifiche di oggetti Active Directory che non sono state replicate in altri controller di dominio nel dominio andranno perse. Inoltre, se il controller di dominio ospita ruoli di master operazioni, il catalogo globale o il ruolo server DNS, le operazioni critiche nel dominio e foresta possono essere influenzate come indicato di seguito. Prima di rimuovere un controller di dominio che ospita un ruolo di master operazioni, provare a trasferire il ruolo in un altro controller di dominio. Se non è possibile trasferire il ruolo, prima di tutto rimuovere servizi di dominio Active Directory dal computer e quindi utilizzare Ntdsutil.exe per assegnare il ruolo. Utilizzare Ntdsutil sul controller di dominio che si intende assegnare il ruolo. Se possibile, utilizzare un partner di replica recenti nello stesso sito come controller di dominio. For more information about transferring and seizing operations master roles, see [article 255504](https://go.microsoft.com/fwlink/?LinkId=80395) in the Microsoft Knowledge Base. Se la procedura guidata è possibile determinare se il controller di dominio ospita un ruolo di master operazioni, eseguire il comando netdom.exe per determinare se il controller di dominio esegue i ruoli di master operazioni.  
  
-   Catalogo globale: gli utenti potrebbero avere problemi di accesso ai domini nella foresta. Prima di rimuovere un server di catalogo globale, assicurarsi che il server di catalogo globale sufficiente siano nella foresta e nel sito agli accessi utente. Se necessario, specificare un altro server di catalogo globale e aggiornare i client e le applicazioni con le nuove informazioni.  
  
-   Server DNS: tutti i dati DNS archiviati in zone integrate in Active Directory andranno persi. Dopo la rimozione di dominio Active Directory, questo server DNS non sarà in grado di eseguire la risoluzione dei nomi per le zone DNS che sono state integrate in Active Directory. Pertanto, è consigliabile aggiornare la configurazione DNS di tutti i computer che fanno riferimento all'indirizzo IP del server DNS per la risoluzione dei nomi con l'indirizzo IP di un nuovo server DNS.  
  
-   Master infrastrutture: client del dominio potrebbero avere difficoltà di individuazione degli oggetti negli altri domini. Prima di continuare, trasferire il ruolo di master infrastrutture per un controller di dominio che non è un server di catalogo globale.  
  
-   Master RID: potrebbero verificarsi problemi nella creazione di nuovi account utente, account computer e gruppi di sicurezza. Prima di continuare, trasferire il ruolo di master RID in un controller di dominio nello stesso dominio controller di dominio.  
  
-   Emulatore di dominio primario (PDC) controller: le operazioni eseguite dall'emulatore PDC, ad esempio gli aggiornamenti di criteri di gruppo e i ripristini delle password per gli account non AD DS non funzionerà correttamente. Prima di continuare, trasferire il ruolo di master emulatore PDC a un controller di dominio che si trova in questo controller di dominio nello stesso dominio.  
  
-   Master schema: non sarà in grado di modificare lo schema della foresta. Prima di continuare, trasferire il ruolo di master schema a un controller di dominio nel dominio radice della foresta.  
  
-   Master denominazione domini: non sarà in grado di aggiungere o rimuovere i domini dalla foresta. Prima di continuare, trasferire il ruolo di master per un controller di dominio nel dominio radice della foresta di denominazione dei domini.  
  
-   Verranno rimosse tutte le partizioni di directory applicative nel controller di dominio Active Directory. Se un controller di dominio ospita l'ultima replica di uno o più partizioni di directory applicative, al termine dell'operazione di rimozione, tali partizioni non esisterà più.  
  
Tenere presente che il dominio non esisterà più dopo la disinstallazione di servizi di dominio Active Directory dall'ultimo controller di dominio nel dominio.  
  
Se il controller di dominio è un server DNS delegato ad ospitare la zona DNS, la pagina seguente fornirà l'opzione per rimuovere il server DNS dalla delega della zona DNS.  
  
![Installazione di AD DS](media/AD-DS-Installation-and-Removal-Wizard-Page-Descriptions/ADDS_RRW_RemovalOptions.gif)  
  
Per ulteriori informazioni sulla rimozione di dominio Active Directory, vedere [rimuovere Active Directory Domain Services (livello 100)](assetId:///99b97af0-aa7e-41ed-8c81-4eee6c03eb4c) e [abbassamento di livello controller di dominio e domini & #40; Livello 200 & #41;](Demoting-Domain-Controllers-and-Domains--Level-200-.md).  
  
## <a name="BKMK_NewAdminPwdPage"></a>Nuova Password amministratore  
Il **nuova Password amministratore** pagina è necessario fornire una password per account di amministratore del computer locale predefinito, una volta completata l'abbassamento di livello e il computer diventa un server membro di dominio o gruppo di lavoro.  
  
![Installazione di AD DS](media/AD-DS-Installation-and-Removal-Wizard-Page-Descriptions/ADDS_RRW_NewAdminPwd.gif)  
  
Per ulteriori informazioni sulla rimozione di dominio Active Directory, vedere [rimuovere Active Directory Domain Services (livello 100)](assetId:///99b97af0-aa7e-41ed-8c81-4eee6c03eb4c) e [abbassamento di livello controller di dominio e domini & #40; Livello 200 & #41;](Demoting-Domain-Controllers-and-Domains--Level-200-.md).  
  
## <a name="BKMK_ConfirmRoleRemovalPage"></a>Verifica opzioni  
Il **verifica opzioni** pagina offre la possibilità di esportare le impostazioni di configurazione per l'abbassamento di livello in uno script di Windows PowerShell in modo che è possibile automatizzare l'abbassamento di livello aggiuntive. Fare clic su **Abbassa di livello** per rimuovere servizi di dominio Active Directory.  
  
![Installazione di AD DS](media/AD-DS-Installation-and-Removal-Wizard-Page-Descriptions/ADDS_RRW_ReviewOptions.gif)  
  


