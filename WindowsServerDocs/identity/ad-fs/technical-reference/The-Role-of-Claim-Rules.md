---
ms.assetid: 65e474b5-3076-4ba3-809d-a09160f7c2bd
title: Ruolo delle regole attestazioni
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 47ad71c9bfab6740873ca50de9d18435c8479853
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71385483"
---
# <a name="the-role-of-claim-rules"></a>Ruolo delle regole attestazioni
La funzione complessiva del servizio federativo in Active Directory Federation Services \(ad FS\) consiste nell'emettere un token che contiene un set di attestazioni. La decisione che riguarda le attestazioni ADFS accetta e quindi invia è disciplinata dalle regole attestazione.  
  
## <a name="what-are-claim-rules"></a>Che cosa sono le regole attestazioni?  
Una regola attestazione rappresenta un'istanza di logica di business che recupera uno o più attestazioni in ingresso, applicarvi condizioni \(Se x y quindi\) e produrre uno o più attestazioni in uscita in base ai parametri di condizione. Per ulteriori informazioni sulle attestazioni in ingresso e in uscita, vedere [il ruolo di attestazioni](The-Role-of-Claims.md).  
  
Le regole attestazioni si usano quando è necessario implementare una logica di business per controllare il flusso di attestazioni nella pipeline delle attestazioni. Mentre la pipeline delle attestazioni è più un concetto logico della fine\-a\-Termina processo per il flusso di attestazioni, le regole sono un elemento amministrativo effettivo che è possibile utilizzare per personalizzare il flusso di attestazioni attraverso il processo di rilascio delle attestazioni di attestazione.  
  
Per ulteriori informazioni sulla pipeline delle attestazioni, vedere [il ruolo del motore di attestazioni](The-Role-of-the-Claims-Engine.md).  
  
Le regole attestazioni offrono i vantaggi seguenti:  
  
-   Fornire un meccanismo per agli amministratori di applicare run\-logica di business di tempo per considerare attendibili attestazioni dai provider di attestazioni  
  
-   Forniscono un meccanismo per consentire agli amministratori di definire le attestazioni rilasciate alle relying party  
  
-   Forniscono attestazioni avanzata\-basato su funzionalità di autorizzazione per gli amministratori che desiderano consentire o negare l'accesso a utenti specifici  
  
### <a name="how-claim-rules-are-processed"></a>Modalità di elaborazione delle regole attestazioni  
Le regole attestazioni vengono elaborate tramite la pipeline delle attestazioni usando il *motore delle attestazioni*. Il motore delle attestazioni è un componente logico del servizio federativo che esamina il set di attestazioni in ingresso fornite da un utente e quindi, in base alla logica di ogni regola, produce un set di attestazioni di output.  
  
Insieme, il motore delle regole attestazioni e il set di regole attestazioni associato a un trust federativo specifico determinano se le attestazioni in ingresso devono essere passate come sono, filtrate in modo da soddisfare i criteri di una condizione specifica o trasformate in un set completamente nuovo di attestazioni prima di essere rilasciate come attestazioni in uscita dal Servizio federativo.  
  
Per ulteriori informazioni su questo processo, vedere [il ruolo del motore di attestazioni](The-Role-of-the-Claims-Engine.md).  
  
## <a name="what-are-claim-rule-templates"></a>Che cosa sono i modelli di regole attestazioni?  
ADFS include un set predefinito di regola attestazione basata su modelli che sono progettati per consentono di selezionare e creano le regole attestazione più appropriate alle esigenze aziendali particolari. I modelli di regole attestazioni vengono usati solo durante il processo di creazione delle regole attestazioni.  
  
Nello snap di gestione di ADFS\-le regole possono essere create solo utilizzando modelli di regola attestazione. Dopo avere utilizzato lo snap\-in per selezionare un modello di regola attestazione, i dati necessari per la logica della regola di input e di salvarlo nel database di configurazione, sarà \(da quel punto in poi\) a cui l'interfaccia Utente di regola attestazione.  
  
### <a name="how-claim-rule-templates-work"></a>Funzionamento dei modelli di regole attestazioni  
A prima vista, modelli di regole sembrano essere moduli di solo input forniti dallo snap di attestazione\-per raccogliere dati e processo logica specifica su attestazioni in ingresso. Tuttavia, a un livello molto più dettagliato, i modelli di regole attestazioni archiviano il framework di lingua delle regole attestazioni che costituisce la logica di base necessaria per creare in modo rapido una regola senza che sia necessario conoscere a fondo la lingua.  
  
Ogni modello che viene fornito nell'interfaccia utente \(dell'interfaccia Utente\) rappresenta una sintassi regola attestazione prepopolato, in base alle attività amministrative più comunemente necessari. C'è tuttavia un modello di regola che rappresenta un'eccezione. Questo modello è detto modello di regola personalizzata. Con questo modello non c'è una sintassi prepopolata. È invece necessario modificare direttamente la sintassi della lingua delle regole attestazioni nel corpo del modello di regola attestazioni usando la sintassi stessa della lingua delle regole attestazioni.  
  
Per ulteriori informazioni su come utilizzare la sintassi del linguaggio di regola attestazione, vedere [il ruolo del linguaggio di regola attestazione](The-Role-of-the-Claim-Rule-Language.md) nella Guida alla distribuzione di ADFS.  
  
> [!TIP]  
> È possibile visualizzare la lingua delle regole attestazioni associata a una regola in qualsiasi momento facendo clic sul pulsante **Visualizza lingua delle regole** nelle proprietà di una regola attestazioni.  
  
### <a name="how-to-create-a-claim-rule"></a>Come creare una regola attestazioni  
Le regole attestazioni vengono create separatamente per ogni relazione di trust federativa nel servizio federativo e non vengono condivise tra più trust. È possibile creare una regola da un modello di regola attestazioni, iniziare da zero creando la regola con la lingua delle regole attestazioni oppure usare Windows PowerShell per personalizzare una regola.  
  
Tutte queste opzioni coesistono per offrire flessibilità di scelta del metodo appropriato per uno scenario specifico. Per ulteriori informazioni su come creare una regola attestazione, vedere [configurazione di regole attestazione](https://technet.microsoft.com/library/ee913571.aspx) nella Guida alla FSDeployment.  
  
#### <a name="using-claim-rule-templates"></a>Uso di modelli di regole attestazioni  
I modelli di regole attestazioni vengono usati solo durante il processo di creazione delle regole attestazioni. Per creare una regola attestazioni, è possibile usare uno dei modelli seguenti:  
  
-   Pass-through o filtro di un'attestazione in ingresso  
  
-   Trasformare un'attestazione in ingresso  
  
-   Inviare attributi LDAP come attestazioni  
  
-   Inviare l'appartenenza a un gruppo come attestazione  
  
-   Inviare attestazioni mediante una regola personalizzata  
  
-   Consentire o negare l'accesso agli utenti in base a un'attestazione in ingresso  
  
-   Consentire l'accesso a tutti gli utenti  
  
Per ulteriori informazioni che descrivono ognuno di questi modelli di regole di attestazioni, vedere [determinare il tipo di modello di regola da utilizzare](Determine-the-Type-of-Claim-Rule-Template-to-Use.md).  
  
#### <a name="using-the-claim-rule-language"></a>Uso del linguaggio delle regole attestazioni  
Per le regole di business che non rientrano nell'ambito dei modelli di regole attestazioni standard, è possibile usare un modello di regola personalizzata per esprimere una serie di condizioni logiche complesse usando la lingua delle regole attestazioni. Per ulteriori informazioni sull'utilizzo di una regola personalizzata, vedere [quando utilizzare una regola attestazione personalizzata](When-to-Use-a-Custom-Claim-Rule.md).  
  
#### <a name="using-windowspowershell"></a>Uso di Windows PowerShell  
È anche possibile usare l'oggetto cmdlet ADFSClaimRuleSet con Windows PowerShell per creare o amministrare regole in AD FS. Per altre informazioni su come usare Windows PowerShell con questo cmdlet, vedere [Amministrazione di AD FS con Windows PowerShell](https://go.microsoft.com/fwlink/?LinkID=179634).  
  
## <a name="what-is-a-claim-rule-set"></a>Che cos'è un set di regole attestazioni?  
Come illustrato nella figura seguente, un set di regole attestazioni è un raggruppamento di una o più regole per un trust federativo specifico che definisce come verranno elaborate le attestazioni dal motore delle regole attestazioni. Quando il servizio federativo riceve un'attestazione in ingresso, il motore delle regole attestazioni applica la logica specificata dal set di regole attestazioni appropriato. È la somma della logica di ogni regola nel set che determina come verranno rilasciate le attestazioni per un trust specifico.  
  
![Ruoli di AD FS](media/adfs2_claimruleset.gif)  
  
Le regole attestazioni vengono elaborate dal motore delle attestazioni in ordine cronologico all'interno di un determinato set di regole. Questo ordine è importante perché l'output di una regola può essere usato come input per la regola successiva nel set.  
  
## <a name="what-are-claim-rule-set-types"></a>Che cosa sono i tipi di set di regole attestazioni?  
Un tipo di set di regole attestazioni è un segmento logico di un trust federativo che identifica in modo categorico se il set di regole attestazioni associato al trust verrà usato per il rilascio, l'autorizzazione o l'accettazione delle attestazioni. Ogni trust federativo può avere uno o più tipi di set di regole attestazioni associati, a seconda del tipo di trust usato.  
  
La tabella seguente descrive i vari tipi di set di regole attestazioni e spiega la loro relazione con un trust del provider di attestazioni o della relying party.  
  
|Tipo di set di regole attestazioni|Descrizione|Usato in|  
|-----------------------|---------------|-----------|  
|Set di regole di trasformazione accettazione|Set di regole attestazioni usato in un trust del provider di attestazioni specifico per indicare le attestazioni in ingresso che verranno accettate dall'organizzazione del provider di attestazioni e le attestazioni in uscita che verranno inviate al trust della relying party.<br /><br />Le attestazioni in ingresso che verranno usate come origine per il set di regole saranno le attestazioni di output del set di regole di trasformazione rilascio come specificato nell'organizzazione del provider di attestazioni.<br /><br />Per impostazione predefinita, il nodo di attendibilità provider di attestazioni contiene un'attendibilità del provider di attestazioni denominata **Active Directory** che viene utilizzata per rappresentare l'archivio di attributi di origine per il set di regole di trasformazione accettazione. Questo oggetto trust viene usato per rappresentare la connessione dal servizio federativo a un database di Active Directory nella rete. Questo trust predefinito elabora le attestazioni per gli utenti che sono stati autenticati da Active Directory e non può essere eliminato.|Trust del provider di attestazioni|  
|Set di regole di trasformazione rilascio|Set di regole attestazioni usato in un trust della relying party per specificare le attestazioni che verranno rilasciate alla relying party.<br /><br />Le attestazioni in ingresso che verranno usate come origine per il set di regole saranno inizialmente le attestazioni di output delle regole di trasformazione accettazione.|Trust della relying party|  
|Set di regole di autorizzazione di rilascio|Set di regole attestazioni usato in un trust della relying party per specificare gli utenti che potranno ricevere un token per la relying party.<br /><br />Queste regole determinano se un utente può ricevere attestazioni per una relying party e perciò accedere alla relying party.<br /><br />A meno che non si specifichi una regola di autorizzazione rilascio, a tutti gli utenti viene negato l'accesso per impostazione predefinita.|Trust della relying party|  
|Set di regole di autorizzazione di delega|Set di regole attestazioni usato in un trust della relying party per specificare gli utenti che potranno fungere da delegati per altri utenti nella relying party.<br /><br />Queste regole determinano se il richiedente può rappresentare un utente identificando comunque il richiedente nel token inviato alla relying party.<br /><br />A meno che non si specifichi una regola di autorizzazione rilascio, nessun utente può fungere da delegato per impostazione predefinita.|Trust della relying party|  
|Set di regole di autorizzazione di rappresentazione|Set di regole attestazioni configurato usando Windows PowerShell per determinare se un utente può rappresentare completamente un altro utente nella relying party.<br /><br />Queste regole determinano se il richiedente può rappresentare un utente senza identificare il richiedente nel token inviato alla relying party.<br /><br />La rappresentazione di un altro utente in questo modo è una funzionalità molto potente, perché la relying party non riconoscerà che l'utente è rappresentato.|Trust della relying party|  
  
Per ulteriori informazioni su, selezionare le regole attestazione appropriato da utilizzare nell'organizzazione, vedere [determinare il tipo di modello di regola da utilizzare](Determine-the-Type-of-Claim-Rule-Template-to-Use.md).  
  

