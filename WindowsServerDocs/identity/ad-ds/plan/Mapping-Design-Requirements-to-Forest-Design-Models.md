---
ms.assetid: c0d64566-5530-482e-a332-af029a5fb575
title: Requisiti di progettazione di mapping ai modelli di progettazione di foresta
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/07/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 35d6322f053c7a02dc1df5430b28f771f57a1ad7
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/31/2019
ms.locfileid: "66442575"
---
# <a name="mapping-design-requirements-to-forest-design-models"></a>Requisiti di progettazione di mapping ai modelli di progettazione di foresta

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

La maggior parte dei gruppi nell'organizzazione possono condividere una singola foresta azienda gestito da un unico gruppo technology (IT) e che contiene gli account utente e risorse per tutti i gruppi che condividono la foresta. Questa foresta condivisa, denominata foresta azienda iniziale, è la base del modello di progettazione di foresta per l'organizzazione.  

Poiché la foresta azienda iniziale può ospitare più gruppi nell'organizzazione, il proprietario della foresta deve essere stabilita contratti di servizio con ogni gruppo in modo che tutte le entità comprendere quanto previsto di essi. Ciò consente di proteggere i singoli gruppi sia il proprietario dell'insieme di strutture stabilendo le aspettative di servizio concordato-on.  

Se non tutti i gruppi all'interno dell'organizzazione possono condividere una singola foresta azienda, è necessario espandere la progettazione della foresta per soddisfare le esigenze dei diversi gruppi. Ciò comporta che identifica i requisiti di progettazione che si applicano ai gruppi in base alle proprie esigenze per autonomia e isolamento e se si dispone di una rete di connettività limitata e quindi identificare il modello di foresta che è possibile usare per contenere quelli requisiti. La tabella seguente elenca gli scenari del modello di progettazione dell'insieme di strutture in base l'autonomia, isolamento e connettività fattori. Dopo aver identificato lo scenario di progettazione di foresta più adatto alle proprie esigenze, determinare se è necessario apportare eventuali ulteriori decisioni per soddisfare le specifiche di progettazione.  

> [!NOTE]  
> Se un fattore è elencato come n/d, non è un fattore importante perché altri requisiti di adattare anche a tale fattore.  

|Scenario|Connettività limitata|Isolamento dei dati|Autonomia dei dati|Isolamento dei servizi|Autonomia dei servizi|  
|------------|------------------------|------------------|-----------------|---------------------|--------------------|  
|[Scenario 1: Aggiungere una foresta esistente per l'autonomia dei dati](#BKMK_1)|No|No|Yes|No|No|  
|[Scenario 2: Usare un dominio o foresta azienda per autonomia dei servizi](#BKMK_2)|No|No|N/D|No|Yes|  
|[Scenario 3: Usare una foresta dell'organizzazione o foresta di risorse per l'isolamento dei servizi](#BKMK_3)|No|No|N/D|Yes|N/D|  
|[Scenario 4: Usare un insieme di strutture dell'organizzazione o foresta ad accesso limitato per l'isolamento dei dati](#BKMK_4)|N/D|Yes|N/D|N/D|N/D|  
|[Scenario 5: Usare una foresta azienda oppure riconfigurare il firewall per la connettività limitata](#BKMK_5)|Yes|No|N/D|No|No|  
|[Scenario 6: Usare un dominio o foresta azienda e riconfigurare il firewall per l'autonomia del servizio con connettività limitata](#BKMK_6)|Yes|No|N/D|No|Yes|  
|[Scenario 7: Usare una foresta di risorse e riconfigurare il firewall per l'isolamento del servizio con connettività limitata](#BKMK_7)|Yes|No|N/D|Yes|N/D|  

## <a name="BKMK_1"></a>Scenario 1: Aggiungere una foresta esistente per l'autonomia dei dati  

È possibile soddisfare un requisito per autonomia dei dati è sufficiente ospitando il gruppo di nell'unità organizzativa (OU) in una foresta esistente dell'organizzazione. Delegare il controllo tramite le unità organizzative per gli amministratori dei dati da tale gruppo per ottenere l'autonomia dei dati. Per altre informazioni sulla delega del controllo tramite le unità organizzative, vedere [creazione di una struttura di unità organizzative](../../ad-ds/plan/Creating-an-Organizational-Unit-Design.md).  
  
## <a name="BKMK_2"></a>Scenario 2: Usare un dominio o foresta azienda per autonomia dei servizi  

Se un gruppo di nell'organizzazione identifica autonomia dei servizi come requisito, è consigliabile che questo requisito prima riconsiderate. Raggiungimento di autonomia dei servizi vengono creati ulteriori overhead di gestione e costi aggiuntivi per l'organizzazione. Assicurarsi che il requisito di autonomia dei servizi non è sufficiente per motivi di praticità e che è possibile giustificare i costi coinvolti nel soddisfare questo requisito.  
  
È possibile soddisfare un requisito per l'autonomia del servizio eseguendo una delle operazioni seguenti:  

- Creazione di una foresta azienda. Inserire utenti, gruppi e computer per il gruppo che richiede l'autonomia dei servizi in una foresta separata dell'organizzazione. Assegnare un singolo da tale gruppo sia il proprietario della foresta. Se il gruppo necessita per le risorse di condivisione o accesso con altre foreste dell'organizzazione, è possibile stabilire una relazione di trust tra le foreste dell'organizzazione e le altre foreste.  

- Uso dei domini aziendali. Inserire utenti, gruppi e computer in un dominio separato in una foresta esistente dell'organizzazione. Questo modello consente solo l'autonomia del servizio a livello di dominio e non per completa autonomia, servizio isolamento o isolamento dei dati.  

Per altre informazioni sull'uso di domini aziendali, vedere [usando il modello di foresta aziendale dominio](../../ad-ds/plan/../../ad-ds/plan/Using-the-Organizational-Domain-Forest-Model.md).  

## <a name="BKMK_3"></a>Scenario 3: Usare una foresta dell'organizzazione o foresta di risorse per l'isolamento dei servizi  

È possibile soddisfare un requisito per l'isolamento del servizio eseguendo una delle operazioni seguenti:  

- Utilizzo di una foresta azienda. Inserire utenti, gruppi e computer per il gruppo che richiede l'isolamento dei servizi in una foresta separata dell'organizzazione. Assegnare un singolo da tale gruppo sia il proprietario della foresta. Se il gruppo necessita per le risorse di condivisione o accesso con altre foreste dell'organizzazione, è possibile stabilire una relazione di trust tra le foreste dell'organizzazione e le altre foreste. Tuttavia, si sconsiglia questo approccio perché l'accesso alle risorse tramite i gruppi universali è molto limitato in scenari di trust tra insiemi di strutture.  

- Utilizzo di una foresta di risorse. Inserire le risorse e gli account del servizio in una foresta di risorse distinto, mantenendo gli account utente in una foresta esistente dell'organizzazione. Se necessario, è possibile creare account alternativi nella foresta di risorse per accedere alle risorse nella foresta di risorse se la foresta azienda non è più disponibile. L'account alternativo deve disporre dell'autorizzazione necessaria per accedere alla foresta di risorse e mantenere il controllo delle risorse fino a quando la foresta azienda è tornata in linea.  

   Stabilire una relazione di trust tra le risorse e foreste dell'organizzazione, in modo che gli utenti possono accedere alle risorse nella foresta quando si usa i propri account utente normale. Questa configurazione consente una gestione centralizzata degli account utente, consentendo agli utenti di tornare all'account alternativi nella foresta di risorse, se la foresta azienda non è più disponibile.  

Considerazioni per l'isolamento dei servizi seguenti:

- Le foreste create per l'isolamento dei servizi possono considerare attendibili i domini da altri insiemi di strutture ma non devono includere gli utenti di altre foreste nei propri gruppi di amministratori del servizio. Se gli utenti di altre foreste sono inclusi in gruppi amministrativi nella foresta di tipo isolato, la sicurezza della foresta isolata potenzialmente può essere compromessa perché gli amministratori del servizio nella foresta non hanno il controllo esclusivo.  

- Fino a quando i controller di dominio siano accessibili in una rete, sono soggetti ad attacchi (come gli attacchi denial of service) da software dannoso in tale rete. È possibile eseguire il comando seguente per evitare la possibilità di attacchi:  

   - Controller di dominio host solo in reti sono considerati sicuri.  

   - Limitare l'accesso alla rete o reti che ospita il controller di dominio.  

- Isolamento dei servizi richiede la creazione di un insieme di strutture. Valutare se i costi di manutenzione dell'infrastruttura per supportare l'insieme di strutture aggiuntivo supera i costi associati alla perdita dell'accesso alle risorse a causa di una foresta azienda non disponibile.  

## <a name="BKMK_4"></a>Scenario 4: Usare un insieme di strutture dell'organizzazione o foresta ad accesso limitato per l'isolamento dei dati  

È possibile ottenere l'isolamento dei dati eseguendo una delle operazioni seguenti:  

- Utilizzo di una foresta azienda. Inserire utenti, gruppi e computer per il gruppo che richiede l'isolamento dei dati in una foresta separata dell'organizzazione. Assegnare un singolo da tale gruppo sia il proprietario della foresta. Se il gruppo necessita per le risorse di condivisione o accesso con altre foreste dell'organizzazione, stabilire una relazione di trust tra la foresta azienda e le altre foreste. Solo gli utenti che richiedono l'accesso alle informazioni classificate esistono nella nuova foresta azienda. Gli utenti hanno un account da usare per i dati di accesso sia classificato nel proprio insieme di strutture e i dati non classificati nelle altre foreste tramite relazioni di trust.  

- Usando una foresta ad accesso limitato. Si tratta di una foresta separata che contiene dati riservati e gli account utente che consentono di accedere a tali dati. Separare gli account utente vengono mantenuti nelle foreste esistenti dell'organizzazione che consentono di accedere alle risorse nella rete senza restrizioni. Non vengono creati alcun trust tra la foresta ad accesso limitato e altre foreste dell'organizzazione. È possibile limitare ulteriormente l'insieme di strutture tramite la distribuzione della foresta in una rete fisica separata, in modo che non è possibile connettersi alle altre foreste. Se si distribuisce la foresta in una rete separata, gli utenti devono disporre di due workstation: uno per l'accesso dell'insieme di strutture con restrizioni e uno per l'accesso alle aree nonrestricted della rete.  

Considerazioni per la creazione di insiemi di strutture di isolamento dei dati includono quanto segue:  

- Gli insiemi di strutture organizzative create per l'isolamento dei dati può considerare attendibili i domini da altre foreste, ma gli utenti di altre foreste non devono essere incluso in una qualsiasi delle operazioni seguenti:  

  - Gruppi responsabili per la gestione dei servizi o i gruppi che possono gestire l'appartenenza dei gruppi di amministratori del servizio  

  - Gruppi che hanno controllo amministrativo su computer in cui archiviare i dati protetti  

  - I gruppi che hanno accesso a dati protetti o i gruppi che sono responsabili per la gestione degli oggetti utente o gli oggetti gruppo che hanno accesso a dati protetti  

    Se in uno di questi gruppi sono inclusi gli utenti da un'altra foresta, la violazione di altra foresta potrà portare alla compromissione dell'insieme di strutture di tipo isolato e la divulgazione dei dati protetti.  

- Altre foreste possono essere configurati per considerare attendibile la foresta azienda creata per l'isolamento dei dati in modo che gli utenti nella foresta di tipo isolato possono accedere alle risorse in altre foreste. Tuttavia, gli utenti nella foresta di tipo isolato devono mai accedere in modo interattivo alle workstation nella foresta trusting. Il computer nella foresta trusting può potenzialmente essere compromesse da software dannoso e può essere usato per acquisire le credenziali di accesso dell'utente.  

   > [!NOTE]
   > Per evitare che i server in una foresta trusting rappresentare gli utenti nella foresta di tipo isolato e quindi l'accesso alle risorse nella foresta di tipo isolata, il proprietario della foresta può disabilitare l'autenticazione delegata oppure usare la funzionalità di delega vincolata. Per altre informazioni sull'autenticazione delegata e la delega vincolata, vedere [delega dell'autenticazione](https://go.microsoft.com/fwlink/?LinkId=106614).  

- Potrebbe essere necessario stabilire un firewall tra la foresta azienda e le altre foreste dell'organizzazione di limitare l'accesso a informazioni di fuori dell'insieme di strutture.  

- Sebbene la creazione di una foresta separata consente l'isolamento dei dati, purché i controller di dominio nella foresta isolata e nel computer che risiedono le informazioni protette sono accessibili in una rete, sono soggetti ad attacchi avviati dai computer nella rete. Le organizzazioni che decide che il rischio di attacco è troppo elevato o la conseguenza di una violazione di sicurezza o di attacco è troppo elevata necessario limitare l'accesso alla rete o reti che ospitano i controller di dominio e i computer che ospitano i dati protetti . Limitare l'accesso è possibile tramite le tecnologie, ad esempio i firewall e Internet Protocol security (IPsec). In casi estremi, le organizzazioni potrebbero scegliere di mantenere i dati protetti su una rete indipendente che è disponibile alcuna connessione fisica a un'altra rete dell'organizzazione.  

   > [!NOTE]  
   > Se non esiste connettività di rete tra una foresta ad accesso limitato e un'altra rete, esiste la possibilità per i dati nell'area con restrizioni deve essere trasmesso a altra rete.  

## <a name="BKMK_5"></a>Scenario 5: Usare una foresta azienda oppure riconfigurare il firewall per la connettività limitata  

Per soddisfare un requisito di connettività limitata, è possibile eseguire una delle operazioni seguenti:  

- Inserire gli utenti in una foresta azienda esistente e quindi aprire il firewall sufficiente per consentire il traffico di Active Directory per pass-through.  

- Usare un insieme di strutture dell'organizzazione. Inserire in una foresta separata dell'organizzazione di utenti, gruppi e computer per il gruppo per cui la connettività è limitata. Assegnare un singolo da tale gruppo sia il proprietario della foresta. Foresta azienda fornisce un ambiente separato su altro lato del firewall. La foresta include account utente e risorse sono gestite all'interno della foresta, in modo che gli utenti non sono necessario passare attraverso il firewall per eseguire le attività giornaliere. Utenti specifici o applicazioni potrebbero essere particolari esigenze che richiedono la possibilità di passare attraverso il firewall di contattare altre foreste. È possibile soddisfare queste esigenze singolarmente aprendo le interfacce appropriate nel firewall, comprese quelle necessarie per tutti i trust a funzionare.  

Per altre informazioni sulla configurazione dei firewall per l'utilizzo con Active Directory Domain Services (AD DS), vedere [Active Directory in reti segmentate tramite firewall](https://go.microsoft.com/fwlink/?LinkId=37928).  

## <a name="BKMK_6"></a>Scenario 6: Usare un dominio o foresta azienda e riconfigurare il firewall per l'autonomia del servizio con connettività limitata  

Se un gruppo di nell'organizzazione identifica autonomia dei servizi come requisito, è consigliabile che questo requisito prima riconsiderate. Raggiungimento di autonomia dei servizi vengono creati ulteriori overhead di gestione e costi aggiuntivi per l'organizzazione. Assicurarsi che il requisito di autonomia dei servizi non è sufficiente per motivi di praticità e che è possibile giustificare i costi coinvolti nel soddisfare questo requisito.  

Connettività limitata è un problema, se si dispone di un requisito per autonomia dei servizi, è possibile eseguire una delle operazioni seguenti:  

- Usare un insieme di strutture dell'organizzazione. Inserire utenti, gruppi e computer per il gruppo che richiede l'autonomia dei servizi in una foresta separata dell'organizzazione. Assegnare un singolo da tale gruppo sia il proprietario della foresta. Foresta azienda fornisce un ambiente separato su altro lato del firewall. La foresta include account utente e risorse sono gestite all'interno della foresta, in modo che gli utenti non sono necessario passare attraverso il firewall per eseguire le attività giornaliere. Utenti specifici o applicazioni potrebbero essere particolari esigenze che richiedono la possibilità di passare attraverso il firewall di contattare altre foreste. È possibile soddisfare queste esigenze singolarmente aprendo le interfacce appropriate nel firewall, comprese quelle necessarie per tutti i trust a funzionare.  

- Inserire utenti, gruppi e computer in un dominio separato in una foresta esistente dell'organizzazione. Questo modello consente solo l'autonomia del servizio a livello di dominio e non per completa autonomia, servizio isolamento o isolamento dei dati. Altri gruppi nella foresta devono considerare attendibile i amministratori del servizio del nuovo dominio allo stesso livello attendibilità proprietario della foresta. Per questo motivo, si sconsiglia questo approccio. Per altre informazioni sull'uso di domini aziendali, vedere [usando il modello di foresta aziendale dominio](../../ad-ds/plan/../../ad-ds/plan/Using-the-Organizational-Domain-Forest-Model.md).  

È anche necessario aprire il firewall sufficiente per consentire il traffico di Active Directory per pass-through. Per altre informazioni sulla configurazione dei firewall per l'utilizzo con Active Directory Domain Services, vedere [Active Directory in reti segmentate tramite firewall](https://go.microsoft.com/fwlink/?LinkId=37928).  

## <a name="BKMK_7"></a>Scenario 7: Usare una foresta di risorse e riconfigurare il firewall per l'isolamento del servizio con connettività limitata  

Connettività limitata è un problema, se si dispone di un requisito per l'isolamento dei servizi, è possibile eseguire una delle operazioni seguenti:  

- Usare un insieme di strutture dell'organizzazione. Inserire utenti, gruppi e computer per il gruppo che richiede l'isolamento dei servizi in una foresta separata dell'organizzazione. Assegnare un singolo da tale gruppo sia il proprietario della foresta. Foresta azienda fornisce un ambiente separato su altro lato del firewall. La foresta include account utente e risorse sono gestite all'interno della foresta, in modo che gli utenti non sono necessario passare attraverso il firewall per eseguire le attività giornaliere. Utenti specifici o applicazioni potrebbero essere particolari esigenze che richiedono la possibilità di passare attraverso il firewall di contattare altre foreste. È possibile soddisfare queste esigenze singolarmente aprendo le interfacce appropriate nel firewall, comprese quelle necessarie per tutti i trust a funzionare.  

- Usare una foresta di risorse. Inserire le risorse e gli account del servizio in una foresta di risorse distinto, mantenendo gli account utente in una foresta esistente dell'organizzazione. Potrebbe essere necessario creare alcuni account utente alternativo nella foresta di risorse per mantenere l'accesso alla foresta di risorse se la foresta azienda non è più disponibile. L'account alternativo deve disporre dell'autorizzazione necessaria per accedere alla foresta di risorse e mantenere il controllo delle risorse fino a quando la foresta azienda è tornata in linea.  

   Stabilire una relazione di trust tra le risorse e foreste dell'organizzazione, in modo che gli utenti possono accedere alle risorse nella foresta quando si usa i propri account utente normale. Questa configurazione consente una gestione centralizzata degli account utente, consentendo agli utenti di tornare all'account alternativi nella foresta di risorse, se la foresta azienda non è più disponibile.  

Considerazioni per l'isolamento dei servizi seguenti:  

- Le foreste create per l'isolamento dei servizi possono considerare attendibili i domini da altri insiemi di strutture ma non devono includere gli utenti di altre foreste nei propri gruppi di amministratori del servizio. Se gli utenti di altre foreste sono inclusi in gruppi amministrativi nella foresta di tipo isolato, la sicurezza della foresta isolata potenzialmente può essere compromessa perché gli amministratori del servizio nella foresta non hanno il controllo esclusivo.  

- Fino a quando i controller di dominio siano accessibili in una rete, sono soggetti ad attacchi (come gli attacchi denial of service) dai computer nella rete. È possibile eseguire il comando seguente per evitare la possibilità di attacchi:  

   - Controller di dominio host solo in reti sono considerati sicuri.  

   - Limitare l'accesso alla rete o reti che ospita il controller di dominio.  

- Isolamento dei servizi richiede la creazione di un insieme di strutture. Valutare se i costi di manutenzione dell'infrastruttura per supportare l'insieme di strutture aggiuntivo supera i costi associati alla perdita dell'accesso alle risorse a causa di una foresta azienda non disponibile.  

   Utenti specifici o applicazioni potrebbero essere particolari esigenze che richiedono la possibilità di passare attraverso il firewall di contattare altre foreste. È possibile soddisfare queste esigenze singolarmente aprendo le interfacce appropriate nel firewall, comprese quelle necessarie per tutti i trust a funzionare.  

Per altre informazioni sulla configurazione dei firewall per l'utilizzo con Active Directory Domain Services, vedere [Active Directory in reti segmentate tramite firewall](https://go.microsoft.com/fwlink/?LinkId=37928).  
