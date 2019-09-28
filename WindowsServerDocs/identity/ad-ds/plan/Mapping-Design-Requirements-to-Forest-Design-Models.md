---
ms.assetid: c0d64566-5530-482e-a332-af029a5fb575
title: Mapping dei requisiti di progettazione ai modelli di progettazione della foresta
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/07/2018
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: d65b03dc255de5523c48c2bb9359530b8e7c3167
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71408767"
---
# <a name="mapping-design-requirements-to-forest-design-models"></a>Mapping dei requisiti di progettazione ai modelli di progettazione della foresta

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

La maggior parte dei gruppi dell'organizzazione può condividere una singola foresta organizzativa gestita da un singolo gruppo IT (Information Technology) e che contiene gli account utente e le risorse per tutti i gruppi che condividono la foresta. Questa foresta condivisa, denominata foresta organizzativa iniziale, costituisce la base del modello di progettazione foresta per l'organizzazione.  

Poiché la foresta organizzativa iniziale può ospitare più gruppi nell'organizzazione, il proprietario della foresta deve stabilire contratti di servizio con ogni gruppo, in modo che tutte le parti conoscano il previsto. Ciò consente di proteggere i singoli gruppi e il proprietario della foresta stabilendo le aspettative di servizio concordate.  

Se non tutti i gruppi dell'organizzazione possono condividere una singola foresta organizzativa, è necessario espandere la struttura della foresta per soddisfare le esigenze dei diversi gruppi. Ciò implica l'identificazione dei requisiti di progettazione che si applicano ai gruppi in base alle proprie esigenze di autonomia e isolamento e a seconda che si disponga o meno di una rete con connettività limitata e che quindi identifichi il modello di foresta che è possibile usare per adattarle requisiti. La tabella seguente elenca gli scenari del modello di progettazione della foresta in base ai fattori di autonomia, isolamento e connettività. Dopo aver identificato lo scenario di progettazione della foresta più adatto alle proprie esigenze, determinare se è necessario prendere decisioni aggiuntive per soddisfare le specifiche di progettazione.  

> [!NOTE]  
> Se un fattore viene elencato come N/A, non è una considerazione, perché altri requisiti si adattano anche a tale fattore.  

|Scenario|Connettività limitata|Isolamento dei dati|Autonomia dei dati|Isolamento del servizio|Autonomia del servizio|  
|------------|------------------------|------------------|-----------------|---------------------|--------------------|  
|[Scenario 1: Unisciti a una foresta esistente per l'autonomia dei dati @ no__t-0|No|No|Yes|No|No|  
|[Scenario 2: Usare una foresta o un dominio aziendale per l'autonomia del servizio @ no__t-0|No|No|N/D|No|Yes|  
|[Scenario 3: Usare una foresta aziendale o una foresta di risorse per l'isolamento dei servizi @ no__t-0|No|No|N/D|Yes|N/D|  
|[Scenario 4: Usare una foresta organizzativa o una foresta con accesso limitato per l'isolamento dei dati @ no__t-0|N/D|Yes|N/D|N/D|N/D|  
|[Scenario 5: Usare una foresta organizzativa o riconfigurare il firewall per la connettività limitata @ no__t-0|Yes|No|N/D|No|No|  
|[Scenario 6: Usare una foresta o un dominio aziendale e riconfigurare il firewall per l'autonomia del servizio con connettività limitata @ no__t-0|Yes|No|N/D|No|Yes|  
|[Scenario 7: Usare una foresta di risorse e riconfigurare il firewall per l'isolamento dei servizi con connettività limitata @ no__t-0|Yes|No|N/D|Yes|N/D|  

## <a name="BKMK_1"></a>Scenario 1: Unisciti a una foresta esistente per l'autonomia dei dati  

È possibile soddisfare un requisito per l'autonomia dei dati semplicemente ospitando il gruppo in unità organizzative (OU) in una foresta organizzativa esistente. Delegare il controllo sulle unità organizzative agli amministratori di dati di tale gruppo per ottenere l'autonomia dei dati. Per ulteriori informazioni sulla delega del controllo tramite le unità organizzative, vedere [creazione di un'unità organizzativa progettazione](../../ad-ds/plan/Creating-an-Organizational-Unit-Design.md).  
  
## <a name="BKMK_2"></a>Scenario 2: Usare una foresta o un dominio aziendale per l'autonomia del servizio  

Se un gruppo dell'organizzazione identifica l'autonomia del servizio come requisito, è consigliabile riconsiderare prima questo requisito. Il raggiungimento dell'autonomia dei servizi consente di creare un sovraccarico di gestione e costi aggiuntivi per l'organizzazione. Verificare che il requisito per l'autonomia del servizio non sia semplicemente per praticità e che sia possibile giustificare i costi associati a questo requisito.  
  
È possibile soddisfare un requisito per l'autonomia del servizio effettuando una delle operazioni seguenti:  

- Creazione di una foresta organizzativa. Inserire gli utenti, i gruppi e i computer per il gruppo che richiede l'autonomia del servizio in una foresta organizzativa separata. Assegnare a un utente singolo da tale gruppo il proprietario della foresta. Se il gruppo deve accedere alle risorse o condividerle con altre foreste nell'organizzazione, può stabilire una relazione di trust tra la foresta organizzativa e le altre foreste.  

- Uso di domini aziendali. Posizionare gli utenti, i gruppi e i computer in un dominio separato in una foresta organizzativa esistente. Questo modello fornisce solo l'autonomia dei servizi a livello di dominio e non per l'autonomia completa, l'isolamento dei servizi o l'isolamento dei dati.  

Per ulteriori informazioni sull'utilizzo di domini aziendali, vedere [utilizzo del modello di foresta di domini aziendali](../../ad-ds/plan/../../ad-ds/plan/Using-the-Organizational-Domain-Forest-Model.md).  

## <a name="BKMK_3"></a>Scenario 3: Usare una foresta aziendale o una foresta di risorse per l'isolamento dei servizi  

È possibile soddisfare un requisito per l'isolamento dei servizi effettuando una delle operazioni seguenti:  

- Utilizzo di una foresta organizzativa. Inserire gli utenti, i gruppi e i computer per il gruppo che richiede l'isolamento del servizio in una foresta organizzativa separata. Assegnare a un utente singolo da tale gruppo il proprietario della foresta. Se il gruppo deve accedere alle risorse o condividerle con altre foreste nell'organizzazione, può stabilire una relazione di trust tra la foresta organizzativa e le altre foreste. Tuttavia, questo approccio non è consigliato perché l'accesso alle risorse tramite i gruppi universali è strettamente limitato negli scenari di trust tra foreste.  

- Utilizzo di una foresta di risorse. Posizionare le risorse e gli account di servizio in una foresta di risorse separata, mantenendo gli account utente in una foresta organizzativa esistente. Se necessario, è possibile creare account alternativi nella foresta delle risorse per accedere alle risorse nella foresta delle risorse se la foresta organizzativa diventa non disponibile. Gli account alternativi devono disporre dell'autorità necessaria per accedere alla foresta delle risorse e mantenere il controllo delle risorse finché la foresta organizzativa non è di nuovo online.  

   Stabilire una relazione di trust tra la risorsa e le foreste aziendali, in modo che gli utenti possano accedere alle risorse nella foresta con gli account utente normali. Questa configurazione consente la gestione centralizzata degli account utente, consentendo agli utenti di eseguire il fallback a account alternativi nella foresta delle risorse se la foresta organizzativa diventa non disponibile.  

Di seguito sono riportate alcune considerazioni sull'isolamento dei servizi:

- Le foreste create per l'isolamento del servizio possono considerare attendibili i domini di altre foreste, ma non devono includere utenti di altre foreste nei gruppi di amministratori del servizio. Se gli utenti di altre foreste sono inclusi in gruppi amministrativi nella foresta isolata, è potenzialmente possibile che la sicurezza della foresta isolata possa essere compromessa perché gli amministratori del servizio nella foresta non dispongono di controllo esclusivo.  

- Fino a quando i controller di dominio sono accessibili in una rete, sono soggetti ad attacchi, ad esempio attacchi Denial of Service, da software dannoso sulla rete. Per evitare la possibilità di un attacco, è possibile eseguire le operazioni seguenti:  

   - Ospitare i controller di dominio solo nelle reti considerate sicure.  

   - Limitare l'accesso alla rete o alle reti che ospitano i controller di dominio.  

- L'isolamento dei servizi richiede la creazione di una foresta aggiuntiva. Valutare se il costo di gestione dell'infrastruttura per il supporto della foresta aggiuntiva supera i costi associati alla perdita di accesso alle risorse a causa di una foresta organizzativa non disponibile.  

## <a name="BKMK_4"></a>Scenario 4: Usare una foresta organizzativa o una foresta con accesso limitato per l'isolamento dei dati  

È possibile ottenere l'isolamento dei dati eseguendo una delle operazioni seguenti:  

- Utilizzo di una foresta organizzativa. Inserire gli utenti, i gruppi e i computer per il gruppo che richiede l'isolamento dei dati in una foresta organizzativa separata. Assegnare a un utente singolo da tale gruppo il proprietario della foresta. Se il gruppo deve accedere alle risorse o condividerle con altre foreste dell'organizzazione, stabilire una relazione di trust tra la foresta organizzativa e le altre foreste. Nella nuova foresta organizzativa sono presenti solo gli utenti che richiedono l'accesso alle informazioni classificate. Gli utenti hanno un account che usano per accedere ai dati classificati nella propria foresta e ai dati non classificati in altre foreste per mezzo di relazioni di trust.  

- Utilizzo di una foresta con accesso limitato. Si tratta di una foresta separata che contiene i dati limitati e gli account utente usati per accedere a tali dati. Gli account utente distinti vengono mantenuti nelle foreste organizzative esistenti utilizzate per accedere alle risorse senza restrizioni sulla rete. Non vengono creati trust tra la foresta con accesso limitato e altre foreste dell'organizzazione. È possibile limitare ulteriormente la foresta distribuendo la foresta in una rete fisica distinta, in modo che non possa connettersi ad altre foreste. Se si distribuisce la foresta in una rete separata, gli utenti devono disporre di due workstation: una per l'accesso alla foresta con restrizioni e una per l'accesso alle aree senza restrizioni della rete.  

Di seguito sono riportate alcune considerazioni sulla creazione di foreste per l'isolamento dei dati:  

- Le foreste organizzative create per l'isolamento dei dati possono considerare attendibili i domini di altre foreste, ma gli utenti di altre foreste non devono essere inclusi in uno dei seguenti elementi:  

  - Gruppi responsabili della gestione dei servizi o dei gruppi che possono gestire l'appartenenza dei gruppi di amministratori del servizio  

  - Gruppi con controllo amministrativo sui computer che archiviano dati protetti  

  - Gruppi che hanno accesso a dati protetti o a gruppi responsabili della gestione degli oggetti utente o del gruppo che hanno accesso ai dati protetti  

    Se gli utenti di un'altra foresta sono inclusi in uno di questi gruppi, una compromissione dell'altra foresta può comportare una compromissione della foresta isolata e della divulgazione dei dati protetti.  

- È possibile configurare altre foreste in modo da considerare attendibile la foresta organizzativa creata per l'isolamento dei dati, in modo che gli utenti della foresta isolata possano accedere alle risorse in altre foreste. Tuttavia, gli utenti della foresta isolata non devono mai accedere in modo interattivo alle workstation nella foresta trusting. Il computer nella foresta trusting può essere potenzialmente compromesso da software dannoso e può essere usato per acquisire le credenziali di accesso dell'utente.  

   > [!NOTE]
   > Per impedire che i server in una foresta trusting possano rappresentare gli utenti dalla foresta isolata e quindi accedere alle risorse nella foresta isolata, il proprietario della foresta può disabilitare l'autenticazione delegata o utilizzare la funzionalità di delega vincolata. Per ulteriori informazioni sull'autenticazione delegata e la delega vincolata, vedere [delega dell'autenticazione](https://go.microsoft.com/fwlink/?LinkId=106614).  

- Potrebbe essere necessario stabilire un firewall tra la foresta organizzativa e le altre foreste dell'organizzazione per limitare l'accesso degli utenti alle informazioni esterne alla foresta.  

- Sebbene la creazione di una foresta separata consenta l'isolamento dei dati, purché i controller di dominio nella foresta isolata e i computer che ospitano informazioni protette siano accessibili in una rete, sono soggetti ad attacchi avviati dai computer della rete. Le organizzazioni che decidono che il rischio di attacco è troppo elevato o che la conseguenza di un attacco o di una violazione della sicurezza è troppo grande per limitare l'accesso alla rete o alle reti che ospitano i controller di dominio e ai computer che ospitano i dati protetti . La limitazione dell'accesso può essere eseguita utilizzando tecnologie quali firewall e Internet Protocol Security (IPsec). In casi estremi, le organizzazioni possono scegliere di mantenere i dati protetti in una rete indipendente senza connessione fisica ad altre reti dell'organizzazione.  

   > [!NOTE]  
   > Se esiste una connettività di rete tra una foresta con accesso limitato e un'altra rete, esiste la possibilità che i dati nell'area con restrizioni vengano trasmessi all'altra rete.  

## <a name="BKMK_5"></a>Scenario 5: Usare una foresta organizzativa o riconfigurare il firewall per la connettività limitata  

Per soddisfare i requisiti di connettività limitati, è possibile eseguire una delle operazioni seguenti:  

- Inserire gli utenti in una foresta organizzativa esistente e quindi aprire il firewall abbastanza per consentire il passaggio del traffico Active Directory.  

- Usare una foresta organizzativa. Inserire gli utenti, i gruppi e i computer per il gruppo per cui la connettività è limitata a una foresta organizzativa separata. Assegnare a un utente singolo da tale gruppo il proprietario della foresta. La foresta organizzativa fornisce un ambiente separato sull'altro lato del firewall. La foresta include account utente e risorse gestiti all'interno della foresta, in modo che gli utenti non debbano attraversare il firewall per completare le attività quotidiane. Utenti o applicazioni specifiche potrebbero avere esigenze speciali che richiedono la possibilità di passare attraverso il firewall per contattare altre foreste. È possibile soddisfare queste esigenze singolarmente aprendo le interfacce appropriate nel firewall, incluse quelle necessarie per il funzionamento di tutti i trust.  

Per ulteriori informazioni sulla configurazione dei firewall per l'utilizzo con Active Directory Domain Services (AD DS), vedere [Active Directory in reti segmentate in base ai firewall](https://go.microsoft.com/fwlink/?LinkId=37928).  

## <a name="BKMK_6"></a>Scenario 6: Usare una foresta o un dominio aziendale e riconfigurare il firewall per l'autonomia del servizio con connettività limitata  

Se un gruppo dell'organizzazione identifica l'autonomia del servizio come requisito, è consigliabile riconsiderare prima questo requisito. Il raggiungimento dell'autonomia dei servizi consente di creare un sovraccarico di gestione e costi aggiuntivi per l'organizzazione. Verificare che il requisito per l'autonomia del servizio non sia semplicemente per praticità e che sia possibile giustificare i costi associati a questo requisito.  

Se la connettività limitata è un problema e si dispone di un requisito per l'autonomia dei servizi, è possibile eseguire una delle operazioni seguenti:  

- Usare una foresta organizzativa. Inserire gli utenti, i gruppi e i computer per il gruppo che richiede l'autonomia del servizio in una foresta organizzativa separata. Assegnare a un utente singolo da tale gruppo il proprietario della foresta. La foresta organizzativa fornisce un ambiente separato sull'altro lato del firewall. La foresta include account utente e risorse gestiti all'interno della foresta, in modo che gli utenti non debbano attraversare il firewall per completare le attività quotidiane. Utenti o applicazioni specifiche potrebbero avere esigenze speciali che richiedono la possibilità di passare attraverso il firewall per contattare altre foreste. È possibile soddisfare queste esigenze singolarmente aprendo le interfacce appropriate nel firewall, incluse quelle necessarie per il funzionamento di tutti i trust.  

- Posizionare gli utenti, i gruppi e i computer in un dominio separato in una foresta organizzativa esistente. Questo modello fornisce solo l'autonomia dei servizi a livello di dominio e non per l'autonomia completa, l'isolamento dei servizi o l'isolamento dei dati. Gli altri gruppi della foresta devono considerare attendibili gli amministratori del nuovo dominio con lo stesso livello di attendibilità del proprietario della foresta. Per questo motivo, questo approccio non è consigliato. Per ulteriori informazioni sull'utilizzo di domini aziendali, vedere [utilizzo del modello di foresta di domini aziendali](../../ad-ds/plan/../../ad-ds/plan/Using-the-Organizational-Domain-Forest-Model.md).  

È anche necessario aprire il firewall abbastanza per consentire il passaggio del traffico Active Directory. Per ulteriori informazioni sulla configurazione dei firewall per l'utilizzo con servizi di dominio Active Directory, vedere [Active Directory in reti segmentate in base ai firewall](https://go.microsoft.com/fwlink/?LinkId=37928).  

## <a name="BKMK_7"></a>Scenario 7: Usare una foresta di risorse e riconfigurare il firewall per l'isolamento dei servizi con connettività limitata  

Se la connettività limitata è un problema e si dispone di un requisito per l'isolamento dei servizi, è possibile eseguire una delle operazioni seguenti:  

- Usare una foresta organizzativa. Inserire gli utenti, i gruppi e i computer per il gruppo che richiede l'isolamento del servizio in una foresta organizzativa separata. Assegnare a un utente singolo da tale gruppo il proprietario della foresta. La foresta organizzativa fornisce un ambiente separato sull'altro lato del firewall. La foresta include account utente e risorse gestiti all'interno della foresta, in modo che gli utenti non debbano attraversare il firewall per completare le attività quotidiane. Utenti o applicazioni specifiche potrebbero avere esigenze speciali che richiedono la possibilità di passare attraverso il firewall per contattare altre foreste. È possibile soddisfare queste esigenze singolarmente aprendo le interfacce appropriate nel firewall, incluse quelle necessarie per il funzionamento di tutti i trust.  

- Usare una foresta di risorse. Posizionare le risorse e gli account di servizio in una foresta di risorse separata, mantenendo gli account utente in una foresta organizzativa esistente. Potrebbe essere necessario creare alcuni account utente alternativi nella foresta delle risorse per mantenere l'accesso alla foresta delle risorse se la foresta organizzativa diventa non disponibile. Gli account alternativi devono disporre dell'autorità necessaria per accedere alla foresta delle risorse e mantenere il controllo delle risorse finché la foresta organizzativa non è di nuovo online.  

   Stabilire una relazione di trust tra la risorsa e le foreste aziendali, in modo che gli utenti possano accedere alle risorse nella foresta con gli account utente normali. Questa configurazione consente la gestione centralizzata degli account utente, consentendo agli utenti di eseguire il fallback a account alternativi nella foresta delle risorse se la foresta organizzativa diventa non disponibile.  

Di seguito sono riportate alcune considerazioni sull'isolamento dei servizi:  

- Le foreste create per l'isolamento del servizio possono considerare attendibili i domini di altre foreste, ma non devono includere utenti di altre foreste nei gruppi di amministratori del servizio. Se gli utenti di altre foreste sono inclusi in gruppi amministrativi nella foresta isolata, è potenzialmente possibile che la sicurezza della foresta isolata possa essere compromessa perché gli amministratori del servizio nella foresta non dispongono di controllo esclusivo.  

- Fino a quando i controller di dominio sono accessibili in una rete, sono soggetti ad attacchi, ad esempio attacchi Denial of Service, da computer in tale rete. Per evitare la possibilità di un attacco, è possibile eseguire le operazioni seguenti:  

   - Ospitare i controller di dominio solo nelle reti considerate sicure.  

   - Limitare l'accesso alla rete o alle reti che ospitano i controller di dominio.  

- L'isolamento dei servizi richiede la creazione di una foresta aggiuntiva. Valutare se il costo di gestione dell'infrastruttura per il supporto della foresta aggiuntiva supera i costi associati alla perdita di accesso alle risorse a causa di una foresta organizzativa non disponibile.  

   Utenti o applicazioni specifiche potrebbero avere esigenze speciali che richiedono la possibilità di passare attraverso il firewall per contattare altre foreste. È possibile soddisfare queste esigenze singolarmente aprendo le interfacce appropriate nel firewall, incluse quelle necessarie per il funzionamento di tutti i trust.  

Per ulteriori informazioni sulla configurazione dei firewall per l'utilizzo con servizi di dominio Active Directory, vedere [Active Directory in reti segmentate in base ai firewall](https://go.microsoft.com/fwlink/?LinkId=37928).  
