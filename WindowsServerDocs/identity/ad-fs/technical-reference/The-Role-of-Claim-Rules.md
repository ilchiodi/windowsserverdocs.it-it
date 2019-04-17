---
ms.assetid: 65e474b5-3076-4ba3-809d-a09160f7c2bd
title: Il ruolo di regole attestazioni
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: e47dbecdeee620d3a237ad8d8c41a550d3ef069c
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/12/2017
---
>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

# <a name="the-role-of-claim-rules"></a>Il ruolo di regole attestazioni
La funzione generale del servizio federativo in Active Directory Federation Services \(AD FS\) consiste nel rilasciare un token che contiene un set di attestazioni. La decisione che riguarda le attestazioni ADFS accetta e quindi rilascia è governata dalle regole attestazione.  
  
## <a name="what-are-claim-rules"></a>Quali sono le regole attestazioni?  
Una regola attestazioni rappresenta un'istanza della logica di business che recupera uno o più attestazioni in ingresso, applicarvi condizioni \ (se x quindi y\) e produrre uno o più attestazioni in uscita in base ai parametri di condizione. Per ulteriori informazioni sulle attestazioni in ingresso e in uscita, vedere [il ruolo di attestazioni](The-Role-of-Claims.md).  
  
Regole attestazioni si usano quando è necessario implementare la logica di business che consentono di controllare il flusso delle attestazioni nella pipeline delle attestazioni. Mentre la pipeline delle attestazioni è soprattutto un concetto logico del processo end-to-end per il flusso di attestazioni, regole attestazione sono un elemento amministrativo effettivo che è possibile utilizzare per personalizzare il flusso di attestazioni attraverso il processo di rilascio delle attestazioni.  
  
Per ulteriori informazioni sulla pipeline delle attestazioni, vedere [The Role of the Claims Engine](The-Role-of-the-Claims-Engine.md).  
  
Le regole attestazioni offrono i vantaggi seguenti:  
  
-   Forniscono un meccanismo per agli amministratori di applicare la logica di business in fase di debug per considerare attendibili le da provider di attestazioni  
  
-   Forniscono un meccanismo per agli amministratori di definire le attestazioni rilasciate alle relying party  
  
-   Fornire funzionalità complete e dettagliate di autorizzazione basata su claims\ agli amministratori che desiderano consentire o negare l'accesso a utenti specifici  
  
### <a name="how-claim-rules-are-processed"></a>La modalità di elaborazione delle regole attestazione  
Le regole attestazioni vengono elaborate tramite la pipeline di attestazioni utilizzando il *motore delle attestazioni*. Il motore delle attestazioni è un componente del servizio federativo che esamina il set di attestazioni in ingresso presentato da un utente logico e quindi, in base alla logica di ogni regola, produrrà un output di set di attestazioni.  
  
Insieme, le attestazioni regola motore e il set di attestazioni regole associate a un trust federativo specifico determinano se attestazioni in ingresso devono essere passate come sono, filtrate per soddisfare i criteri di una condizione specifica o trasformate in un set completamente nuovo di attestazioni prima di essere rilasciate come attestazioni in uscita dal servizio federativo.  
  
Per ulteriori informazioni su questo processo, vedere [The Role of the Claims Engine](The-Role-of-the-Claims-Engine.md).  
  
## <a name="what-are-claim-rule-templates"></a>Quali sono i modelli di regole attestazioni?  
ADFS include un set predefinito di regola attestazione basata su modelli che sono progettati per consentono di selezionare e creano le regole attestazione più appropriate alle esigenze aziendali particolari. Attestazione regola modelli vengono usati solo durante il processo di creazione di regole attestazione.  
  
Nella gestione di ADFS snap-in, regole possono essere create solo utilizzando modelli di regola attestazione. Dopo avere utilizzato lo snap-in per selezionare un modello di regola attestazione, i dati necessari per la logica della regola di input e salvarlo nel database di configurazione, sarà \ (da tale forward\ punto) a cui l'interfaccia utente di una regola attestazione.  
  
### <a name="how-claim-rule-templates-work"></a>Funzionamento dei modelli di regola attestazioni  
A prima vista, modelli di regole attestazioni sembrano essere moduli di solo input forniti dallo snap-in per raccogliere dati e processo logica specifica su attestazioni in ingresso. Tuttavia, un livello molto più dettagliato, attestazioni archivio modelli di regole attestazioni il framework di linguaggio di regola che costituiscono la logica di base necessarie per creare rapidamente una regola senza che sia necessario conoscere a fondo la lingua.  
  
Ogni modello che viene fornito nell'interfaccia utente \(UI\) rappresenta una sintassi regola attestazione prepopolato, in base alle attività amministrative più comunemente necessari. Esiste tuttavia un modello di regola, che è l'eccezione. Questo modello è detto modello di regola personalizzata. Con questo modello non è prepopolata alcuna sintassi. È invece necessario modificare direttamente la sintassi di linguaggio di regola attestazione nel corpo del modulo del modello di regola attestazione utilizzando la sintassi di linguaggio di regola attestazione.  
  
Per ulteriori informazioni su come usare la sintassi di linguaggio di regola attestazione, vedere [il ruolo del linguaggio di regola attestazione di](The-Role-of-the-Claim-Rule-Language.md) nella Guida alla distribuzione di ADFS.  
  
> [!TIP]  
> È possibile visualizzare il linguaggio delle regole attestazioni associato a una regola in qualsiasi momento facendo il **Visualizza lingua delle regole** pulsante delle proprietà di una regola attestazione.  
  
### <a name="how-to-create-a-claim-rule"></a>Come creare una regola attestazione  
Le regole attestazioni vengono create separatamente per ogni relazione di trust federativa nel servizio federativo e non vengono condivise tra più trust. È possibile creare una regola da un modello di regola attestazione, iniziare da zero creando la regola usando il linguaggio di regola attestazioni o utilizzare Windows PowerShell per personalizzare una regola.  
  
Tutte queste opzioni di coesistere per offrire la flessibilità di scegliere il metodo appropriato per un determinato scenario. Per ulteriori informazioni su come creare una regola attestazioni, vedere [configurazione di regole attestazione](https://technet.microsoft.com/library/ee913571.aspx) nella Guida alla FSDeployment.  
  
#### <a name="using-claim-rule-templates"></a>Utilizzando i modelli di regola attestazione  
Attestazione regola modelli vengono usati solo durante il processo di creazione di regole attestazione. Per creare una regola attestazione, è possibile utilizzare uno qualsiasi dei modelli seguenti:  
  
-   Pass-Through o filtrare un'attestazione in ingresso  
  
-   Trasformare un'attestazione in ingresso  
  
-   Inviare attributi LDAP come attestazioni  
  
-   Inviare l'appartenenza al gruppo come attestazione  
  
-   Inviare attestazioni mediante una regola personalizzata  
  
-   Consentire o negare agli utenti in base a un'attestazione in ingresso  
  
-   Consentire tutti gli utenti  
  
Per ulteriori informazioni su ognuno di questi modelli di regola attestazioni, vedere [determinare il tipo di modello di regola attestazione da utilizzare](Determine-the-Type-of-Claim-Rule-Template-to-Use.md).  
  
#### <a name="using-the-claim-rule-language"></a>Utilizzando il linguaggio di regola attestazione  
Per le regole di business che non rientrano nell'ambito dei modelli di regole attestazioni standard, è possibile utilizzare un modello di regola personalizzata per esprimere una serie di condizioni logiche complesse usando il linguaggio delle regole attestazione. Per ulteriori informazioni sull'utilizzo di una regola personalizzata, vedere [quando usare una regola attestazione personalizzata](When-to-Use-a-Custom-Claim-Rule.md).  
  
#### <a name="using-windows-powershell"></a>Utilizzo di Windows PowerShell  
È inoltre possibile utilizzare l'oggetto cmdlet ADFSClaimRuleSet con Windows PowerShell per creare o gestire le regole in ADFS. Per ulteriori informazioni sull'utilizzo di Windows PowerShell con questo cmdlet, vedere [amministrazione di ADFS con Windows PowerShell](https://go.microsoft.com/fwlink/?LinkID=179634).  
  
## <a name="what-is-a-claim-rule-set"></a>Che cos'è un set di regole attestazioni?  
Come illustrato nella figura seguente, un set di regole attestazioni è un raggruppamento di uno o più regole per un trust federativo specifico che definisce come verranno elaborate attestazioni dal motore delle regole attestazioni. Quando il servizio federativo riceve un'attestazione in ingresso il motore delle regole attestazioni applica la logica specificata dal set di regole attestazione appropriato. È la somma della logica di ogni regola nel set che determina come verranno rilasciate le attestazioni per un trust specifico nella sua interezza.  
  
![Ruoli di AD FS](media/adfs2_claimruleset.gif)  
  
Le regole attestazioni vengono elaborate dal motore delle attestazioni in ordine cronologico entro un determinato set di regole. Questo ordine è importante, perché l'output di una regola può essere utilizzato come input per la regola successiva nel set.  
  
## <a name="what-are-claim-rule-set-types"></a>Tipi di set di quali sono regole attestazioni?  
Tipo di impostare una regola attestazioni è un segmento logico di un trust federativo che identifica in modo categorico se il set di regole attestazioni associato al trust verrà utilizzato per il rilascio di attestazioni, l'autorizzazione o accettazione. Ogni trust federativo può avere uno o più regole attestazione Imposta tipi associati, a seconda del tipo di trust che viene utilizzato.  
  
Nella tabella seguente vengono descritti i diversi tipi di set di regole attestazione e la loro relazione con un trust del provider di attestazioni o trust della relying party.  
  
|Tipo di set di regole attestazioni|Descrizione|Usato in|  
|-----------------------|---------------|-----------|  
|Set di regole di trasformazione accettazione|Un set di regole attestazioni usato in un particolare attestazioni attendibilità del provider per specificare le attestazioni in ingresso che verranno accettate dall'organizzazione del provider di attestazioni e le attestazioni in uscita che verranno inviate al trust della relying party.<br /><br />Le attestazioni in ingresso che verranno utilizzate per l'origine il set di regole saranno le attestazioni di output dalla regola di trasformazione rilascio impostata come specificato nell'organizzazione del provider di attestazioni.<br /><br />Per impostazione predefinita, il nodo di attendibilità provider di attestazioni contiene un trust del provider di attestazioni denominato **Active Directory** che viene utilizzato per rappresentare l'archivio di attributi di origine per il set di regole di trasformazione accettazione. Questo oggetto trust viene utilizzato per rappresentare la connessione dal servizio federativo a un database di Active Directory nella rete. Questo trust predefinito è elabora le attestazioni per gli utenti che sono stati autenticati da Active Directory e non può essere eliminato.|Provider di attestazioni|  
|Set di regole di trasformazione rilascio|Un set di regole attestazione che usi in un trust della relying party per specificare le attestazioni che verranno rilasciate alla relying party.<br /><br />Le attestazioni in ingresso che verranno utilizzate per l'origine il set di regole saranno inizialmente le attestazioni di output di regole di trasformazione accettazione.|Trust della relying party|  
|Set di regole di autorizzazione rilascio|Un set di regole attestazione che usi in un trust della relying party per specificare gli utenti che potranno ricevere un token per la relying party.<br /><br />Queste regole determinano se un utente può ricevere attestazioni per una relying party e, pertanto, accesso alla relying party.<br /><br />Se non si specifica una regola di autorizzazione rilascio, tutti gli utenti verranno negati l'accesso per impostazione predefinita.|Trust della relying party|  
|Set di regole di autorizzazione di delega|Un set di regole attestazione che usi in un trust della relying party per specificare gli utenti che potranno fungere da delegati per altri utenti alla relying party.<br /><br />Queste regole determinano se il richiedente può rappresentare un utente identificando comunque il richiedente nel token inviato alla relying party.<br /><br />Se non si specifica una regola di autorizzazione rilascio, nessun utente possono fungere da delegati per impostazione predefinita.|Trust della relying party|  
|Set di regole di autorizzazione di rappresentazione|Un set di regole configurate utilizzando Windows PowerShell per determinare se un utente possono completamente attestazione rappresentare un altro utente per la relying party.<br /><br />Queste regole determinano se il richiedente può rappresentare un utente senza identificare il richiedente nel token inviato alla relying party.<br /><br />La rappresentazione di un altro utente in questo modo è una funzionalità molto potente, perché la relying party non riconoscerà che l'utente è rappresentato.|Trust della relying party|  
  
Per ulteriori informazioni su come selezionare le regole attestazione appropriato da utilizzare nell'organizzazione, vedere [determinare il tipo di modello di regola attestazione da utilizzare](Determine-the-Type-of-Claim-Rule-Template-to-Use.md).  
  

