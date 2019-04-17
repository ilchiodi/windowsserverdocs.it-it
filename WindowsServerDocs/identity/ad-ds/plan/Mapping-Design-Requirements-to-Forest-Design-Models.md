---
ms.assetid: c0d64566-5530-482e-a332-af029a5fb575
title: Mapping dei requisiti di progettazione ai modelli di progettazione di foresta
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 48a2b03c6e29afcca565e861383a831050ef4233
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/12/2017
---
# <a name="mapping-design-requirements-to-forest-design-models"></a>Mapping dei requisiti di progettazione ai modelli di progettazione di foresta

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

La maggior parte dei gruppi all'interno dell'organizzazione possono condividere una singola foresta azienda che è gestito da un unico gruppo reparti IT e che contiene gli account utente e per tutti i gruppi che condividono l'insieme di strutture. Questa foresta condivisa, denominata foresta azienda iniziale, è la base del modello di progettazione di foresta per l'organizzazione.  
  
Perché la foresta aziendale iniziale può ospitare più gruppi nell'organizzazione, il proprietario della foresta deve stabilire contratti di servizio con ogni gruppo in modo che tutte le parti comprendere previsto di essi. Consente di proteggere sia i singoli gruppi e il proprietario della foresta mediante la definizione di servizio concordato on aspettative.  
  
Se non tutti i gruppi all'interno dell'organizzazione possono condividere una singola foresta azienda, è necessario espandere la progettazione della foresta per soddisfare le esigenze dei diversi gruppi. Questo implica l'identificazione dei requisiti di progettazione che si applicano ai gruppi in base alle proprie esigenze per autonomia e isolamento e o meno dispongono di una rete di connettività limitata e quindi individua il modello di foresta che è possibile utilizzare per supportare tali requisiti. La tabella seguente elenca gli scenari del modello di progettazione dell'insieme di strutture in base al autonomia, isolamento e fattori di connettività. Dopo aver identificato lo scenario di progettazione di foresta più adatto alle esigenze, determinare se è necessario prendere le decisioni di qualsiasi aggiuntive per soddisfare le specifiche di progettazione.  
  
> [!NOTE]  
> Se un fattore è elencato come n/d, non è un fattore importante perché altri requisiti supportare anche che il fattore.  
  
|Scenario|Connettività limitata|Isolamento di dati|Autonomia dei dati|Isolamento dei servizi|Autonomia dei servizi|  
|------------|------------------------|------------------|-----------------|---------------------|--------------------|  
|[Scenario 1: Aggiungere una foresta esistente per l'autonomia dei dati](#BKMK_1)|No|No|Sì|No|No|  
|[Scenario 2: Utilizzare un insieme di strutture organizzative o un dominio per autonomia dei servizi](#BKMK_2)|No|No|N/D|No|Sì|  
|[Scenario 3: Utilizzare un insieme di strutture organizzative o foresta delle risorse per l'isolamento dei servizi](#BKMK_3)|No|No|N/D|Sì|N/D|  
|[Scenario 4: Utilizzano un insieme di strutture organizzative o foresta ad accesso limitato per l'isolamento di dati](#BKMK_4)|N/D|Sì|N/D|N/D|N/D|  
|[Scenario 5: Utilizzare un insieme di strutture organizzative o riconfigurare il firewall per la connettività limitata](#BKMK_5)|Sì|No|N/D|No|No|  
|[Scenario 6: Utilizzare un insieme di strutture organizzative o un dominio e riconfigurare il firewall per autonomia con connettività limitata](#BKMK_6)|Sì|No|N/D|No|Sì|  
|[Scenario 7: Usare una foresta di risorse e riconfigurare il firewall per l'isolamento dei servizi con connettività limitata](#BKMK_7)|Sì|No|N/D|Sì|N/D|  
  
## <a name="BKMK_1"></a>Scenario 1: Aggiungere una foresta esistente per l'autonomia dei dati  
È possibile soddisfare un requisito per autonomia dei dati facendo semplicemente che ospita il gruppo nelle unità organizzative (OU) in una foresta esistente dell'organizzazione. Delegare il controllo tramite le unità organizzative per gli amministratori di dati da tale gruppo per ottenere l'autonomia dei dati. Per ulteriori informazioni sulla delega controllo utilizzando le unità organizzative, vedere [creazione di una struttura di unità organizzativa](../../ad-ds/plan/Creating-an-Organizational-Unit-Design.md).  
  
## <a name="BKMK_2"></a>Scenario 2: Utilizzare un insieme di strutture organizzative o un dominio per autonomia dei servizi  
Se un gruppo nell'organizzazione identifica autonomia come requisito, è consigliabile che si riconsiderare prima di tutto questo requisito. Per ottenere autonomia crea ulteriori interventi di gestione e dei costi aggiuntivi per l'organizzazione. Assicurarsi che il requisito per autonomia dei servizi non è sufficiente per comodità e che è possibile giustificare i costi legati soddisfare questo requisito.  
  
È possibile soddisfare un requisito per autonomia effettuando una delle operazioni seguenti:  
  
-   Creazione di un insieme di strutture organizzative. Inserire utenti, gruppi e computer per il gruppo che richiede l'autonomia dei servizi in una foresta separata dell'organizzazione. Assegnare un singolo da quel gruppo di essere il proprietario della foresta. Se il gruppo di risorse di accesso o la condivisione con altre foreste nell'organizzazione, è possibile creare un trust tra la foresta azienda e le altre foreste.  
  
-   Utilizzo di domini aziendali. Inserire utenti, gruppi e computer in un dominio separato in una foresta esistente dell'organizzazione. Questo modello consente solo autonomia dei servizi a livello di dominio e non per completo autonomia, service isolamento o isolamento dei dati.  
  
Per ulteriori informazioni sull'utilizzo di domini organizzativi, vedere [utilizzando il modello di foresta aziendale dominio](../../ad-ds/plan/../../ad-ds/plan/Using-the-Organizational-Domain-Forest-Model.md).  
  
## <a name="BKMK_3"></a>Scenario 3: Utilizzare un insieme di strutture organizzative o foresta delle risorse per l'isolamento dei servizi  
È possibile soddisfare un requisito per l'isolamento dei servizi, effettuare una delle operazioni seguenti:  
  
-   Utilizzo di un insieme di strutture organizzative. Inserire utenti, gruppi e computer per il gruppo che richiede l'isolamento dei servizi in una foresta separata dell'organizzazione. Assegnare un singolo da quel gruppo di essere il proprietario della foresta. Se il gruppo di risorse di accesso o la condivisione con altre foreste nell'organizzazione, è possibile creare un trust tra la foresta azienda e le altre foreste. Tuttavia, si sconsiglia questo approccio perché l'accesso alle risorse tramite i gruppi universali è molto limitato in scenari di attendibilità dell'insieme di strutture.  
  
-   Utilizzo di una foresta di risorse. Inserire le risorse e gli account del servizio in una foresta di risorse distinto, mantenere gli account utente in una foresta esistente dell'organizzazione. Se necessario, è possibile creare account alternativi nella foresta delle risorse per accedere alle risorse nella foresta di risorse, se la foresta azienda è più disponibile. Gli account alternativi devono disporre dell'autorizzazione necessaria per accedere alla foresta delle risorse e mantenere il controllo delle risorse fino a quando la foresta azienda è tornata in linea.  
  
    Stabilire una relazione di trust tra la risorsa e insiemi di strutture organizzative, in modo che gli utenti possono accedere alle risorse nella foresta durante l'uso di account utente standard. Questa configurazione consente la gestione centralizzata di account utente, consentendo agli utenti di eseguire il fallback a account alternativi nella foresta di risorse se la foresta azienda è più disponibile.  
  
Considerazioni per l'isolamento di servizio includono quanto segue:  
  
-   Le foreste create per l'isolamento dei servizi possono ritenere attendibili i domini di altre foreste, ma non devono includere gli utenti di altre foreste loro gruppi di amministratori del servizio. Se gli utenti di altre foreste sono inclusi in gruppi amministrativi nella foresta isolato, la sicurezza della foresta isolata potenzialmente può essere compromessi perché gli amministratori del servizio nell'insieme di strutture non hanno il controllo esclusivo.  
  
-   Come controller di dominio siano accessibili in una rete, sono soggetti ad attacchi di (ad esempio attacchi denial of service) da software dannoso in tale rete. È possibile eseguire le operazioni seguenti per la protezione contro la possibilità di un attacco:  
  
    -   Controller di dominio host solo su reti che sono considerati sicuri.  
  
    -   Limitare l'accesso alla rete o a reti che ospita i controller di dominio.  
  
-   Isolamento dei servizi richiede la creazione di un insieme di strutture. Valutare se il costo di manutenzione dell'infrastruttura per supportare l'insieme di strutture ulteriori compensa i costi associati alla perdita dell'accesso alle risorse a causa di una foresta azienda non disponibile.  
  
## <a name="BKMK_4"></a>Scenario 4: Utilizzano un insieme di strutture organizzative o foresta ad accesso limitato per l'isolamento di dati  
È possibile ottenere l'isolamento di dati eseguendo una delle operazioni seguenti:  
  
-   Utilizzo di un insieme di strutture organizzative. Inserire utenti, gruppi e computer per il gruppo che richiede l'isolamento di dati in una foresta separata dell'organizzazione. Assegnare un singolo da quel gruppo di essere il proprietario della foresta. Se il gruppo di risorse di accesso o la condivisione con altre foreste nell'organizzazione, creare un trust tra la foresta azienda e le altre foreste. Solo gli utenti che richiedono l'accesso a informazioni riservate sono nella nuova foresta azienda. Gli utenti hanno un account che utilizzano per dati di accesso classificato nella propria foresta e non classificato in altre foreste tramite le relazioni di trust.  
  
-   Utilizzo di una foresta ad accesso limitato. Si tratta di una foresta separata che contiene dati riservati e gli account utente che vengono utilizzati per accedere a tali dati. Gli account utente separato vengono mantenuti nelle foreste esistenti dell'organizzazione che vengono utilizzate per accedere alle risorse della rete senza restrizioni. Non vengono creati alcun trust tra la foresta ad accesso limitato e altre foreste nell'organizzazione. È possibile limitare ulteriormente la foresta distribuendo l'insieme di strutture in una rete fisica separata, in modo che non può connettersi alle altre foreste. Se si distribuisce la foresta in una rete separata, gli utenti devono disporre di due workstation: uno per l'accesso dell'insieme di strutture con restrizioni e uno per l'accesso alle aree nonrestricted della rete.  
  
Considerazioni per la creazione di foreste per l'isolamento di dati includono quanto segue:  
  
-   Gli insiemi di strutture organizzative creati per l'isolamento di dati attendibile domini di altre foreste, ma gli utenti di altre foreste non devono essere incluso in una qualsiasi delle operazioni seguenti:  
  
    -   Gruppi responsabili della gestione dei servizi o i gruppi che possono gestire l'appartenenza di gruppi di amministratori del servizio  
  
    -   Gruppi che hanno il controllo amministrativo su computer in cui archiviare i dati protetti.  
  
    -   Gruppi che possono accedere ai dati protetti o i gruppi che sono responsabili della gestione di oggetti utente o gruppo che possono accedere ai dati protetti  
  
    Se gli utenti da un'altra foresta sono inclusi in uno di questi gruppi, la violazione di altra foresta potrebbe portare alla compromissione dell'insieme di strutture isolato e la divulgazione dei dati protetti.  
  
-   Altre foreste possono essere configurati per considerare attendibile la foresta azienda creata per l'isolamento di dati in modo che gli utenti nella foresta isolato possono accedere alle risorse in altre foreste. Tuttavia, gli utenti dall'insieme di strutture isolato devono mai accedere in modo interattivo alle workstation nella foresta trusting. Il computer nella foresta trusting può essere potenzialmente compromessi dal software dannoso e può essere usato per acquisire le credenziali di accesso dell'utente.  
  
    > [!NOTE]  
    > Per impedire che i server in una foresta trusting rappresentare gli utenti dall'insieme di strutture isolato e quindi l'accesso alle risorse nella foresta isolata, il proprietario della foresta possibile disabilitare l'autenticazione delegata o usare la funzionalità di delega vincolata. Per ulteriori informazioni sull'autenticazione delegata e la delega vincolata, vedere la delega dell'autenticazione ([https://go.microsoft.com/fwlink/?LinkId=106614](https://go.microsoft.com/fwlink/?LinkId=106614)).  
  
-   Potrebbe essere necessario stabilire un firewall tra la foresta azienda e le altre foreste nell'organizzazione per limitare l'accesso a informazioni di fuori dell'insieme di strutture.  
  
-   Anche se la creazione di una foresta separata consente l'isolamento dei dati, come i controller di dominio nella foresta isolato e i computer che informazioni sull'host protetti sono accessibili in una rete, sono soggetti ad attacchi provenienti dai computer nella rete. Le organizzazioni che decide che il rischio di attacchi è troppo elevato o che la conseguenza di una violazione di protezione o di attacco è troppo grande necessario limitare l'accesso alla rete o a reti che ospitano i controller di dominio e i computer che ospitano i dati protetti. Limitare l'accesso può essere eseguita mediante tecnologie quali i firewall e Internet Protocol security (IPsec). In casi estremi, le organizzazioni potrebbero scegliere di mantenere i dati protetti in una rete indipendente che non esiste una connessione fisica a un'altra rete nell'organizzazione.  
  
    > [!NOTE]  
    > Se qualsiasi connettività di rete esiste tra un insieme di strutture con restrizioni di accesso e un'altra rete, esiste la possibilità per i dati nell'area con restrizioni devono essere trasmessi all'altra rete.  
  
## <a name="BKMK_5"></a>Scenario 5: Utilizzare un insieme di strutture organizzative o riconfigurare il firewall per la connettività limitata  
Per soddisfare un requisito di connettività limitata, è possibile eseguire una delle operazioni seguenti:  
  
-   Inserire gli utenti in una foresta esistente dell'organizzazione e quindi aprire il firewall sufficiente per consentire il traffico di Active Directory per pass-through.  
  
-   Utilizzare un insieme di strutture organizzative. Inserire utenti, gruppi e computer per il gruppo per cui la connettività è limitata in una foresta separata dell'organizzazione. Assegnare un singolo da quel gruppo di essere il proprietario della foresta. Insieme di strutture organizzative fornisce un ambiente separato su altro lato del firewall. La foresta include gli account utente e risorse che sono gestite all'interno della foresta, in modo che gli utenti non è necessario passare attraverso il firewall per eseguire le attività giornaliere. Utenti specifici o le applicazioni potrebbero con esigenze speciali che richiedono la possibilità di passare attraverso il firewall per contattare altri insiemi di strutture. È possibile soddisfare queste esigenze singolarmente aprendo le interfacce appropriate nel firewall, comprese quelle necessarie per tutti i trust di funzionare.  
  
Per ulteriori informazioni sulla configurazione dei firewall per l'utilizzo con servizi di dominio Active Directory (AD DS), vedere Active Directory in reti segmentate tramite firewall ([https://go.microsoft.com/fwlink/?LinkId=37928](https://go.microsoft.com/fwlink/?LinkId=37928)).  
  
## <a name="BKMK_6"></a>Scenario 6: Utilizzare un insieme di strutture organizzative o un dominio e riconfigurare il firewall per autonomia con connettività limitata  
Se un gruppo nell'organizzazione identifica autonomia come requisito, è consigliabile che si riconsiderare prima di tutto questo requisito. Per ottenere autonomia crea ulteriori interventi di gestione e dei costi aggiuntivi per l'organizzazione. Assicurarsi che il requisito per autonomia dei servizi non è sufficiente per comodità e che è possibile giustificare i costi legati soddisfare questo requisito.  
  
Connettività limitata è un problema, se si dispone di un requisito per autonomia dei servizi, è possibile eseguire una delle operazioni seguenti:  
  
-   Utilizzare un insieme di strutture organizzative. Inserire utenti, gruppi e computer per il gruppo che richiede l'autonomia dei servizi in una foresta separata dell'organizzazione. Assegnare un singolo da quel gruppo di essere il proprietario della foresta. Insieme di strutture organizzative fornisce un ambiente separato su altro lato del firewall. La foresta include gli account utente e risorse che sono gestite all'interno della foresta, in modo che gli utenti non è necessario passare attraverso il firewall per eseguire le attività giornaliere. Utenti specifici o le applicazioni potrebbero con esigenze speciali che richiedono la possibilità di passare attraverso il firewall per contattare altri insiemi di strutture. È possibile soddisfare queste esigenze singolarmente aprendo le interfacce appropriate nel firewall, comprese quelle necessarie per tutti i trust di funzionare.  
  
-   Inserire utenti, gruppi e computer in un dominio separato in una foresta esistente dell'organizzazione. Questo modello consente solo autonomia dei servizi a livello di dominio e non per completo autonomia, service isolamento o isolamento dei dati. Altri gruppi nella foresta devono considerare attendibile gli amministratori del servizio del nuovo dominio per lo stesso livello che si considera attendibile il proprietario della foresta. Per questo motivo, si sconsiglia questo approccio. Per ulteriori informazioni sull'utilizzo di domini organizzativi, vedere [utilizzando il modello di foresta aziendale dominio](../../ad-ds/plan/../../ad-ds/plan/Using-the-Organizational-Domain-Forest-Model.md).  
  
È necessario anche aprire il firewall sufficiente per consentire il traffico di Active Directory per pass-through. Per ulteriori informazioni sulla configurazione dei firewall per l'uso di dominio Active Directory, vedere Active Directory in reti segmentate tramite firewall ([https://go.microsoft.com/fwlink/?LinkId=37928](https://go.microsoft.com/fwlink/?LinkId=37928)).  
  
## <a name="BKMK_7"></a>Scenario 7: Usare una foresta di risorse e riconfigurare il firewall per l'isolamento dei servizi con connettività limitata  
Connettività limitata è un problema, se si dispone di un requisito per l'isolamento dei servizi, è possibile eseguire una delle operazioni seguenti:  
  
-   Utilizzare un insieme di strutture organizzative. Inserire utenti, gruppi e computer per il gruppo che richiede l'isolamento dei servizi in una foresta separata dell'organizzazione. Assegnare un singolo da quel gruppo di essere il proprietario della foresta. Insieme di strutture organizzative fornisce un ambiente separato su altro lato del firewall. La foresta include gli account utente e risorse che sono gestite all'interno della foresta, in modo che gli utenti non è necessario passare attraverso il firewall per eseguire le attività giornaliere. Utenti specifici o le applicazioni potrebbero con esigenze speciali che richiedono la possibilità di passare attraverso il firewall per contattare altri insiemi di strutture. È possibile soddisfare queste esigenze singolarmente aprendo le interfacce appropriate nel firewall, comprese quelle necessarie per tutti i trust di funzionare.  
  
-   Utilizzare una foresta di risorse. Inserire le risorse e gli account del servizio in una foresta di risorse distinto, mantenere gli account utente in una foresta esistente dell'organizzazione. Potrebbe essere necessario creare alcuni account utente alternativo nella foresta delle risorse per mantenere l'accesso alla foresta delle risorse se la foresta azienda è più disponibile. Gli account alternativi devono disporre dell'autorizzazione necessaria per accedere alla foresta delle risorse e mantenere il controllo delle risorse fino a quando la foresta azienda è tornata in linea.  
  
    Stabilire una relazione di trust tra la risorsa e insiemi di strutture organizzative, in modo che gli utenti possono accedere alle risorse nella foresta durante l'uso di account utente standard. Questa configurazione consente la gestione centralizzata di account utente, consentendo agli utenti di eseguire il fallback a account alternativi nella foresta di risorse se la foresta azienda è più disponibile.  
  
Considerazioni per l'isolamento di servizio includono quanto segue:  
  
-   Le foreste create per l'isolamento dei servizi possono ritenere attendibili i domini di altre foreste, ma non devono includere gli utenti di altre foreste loro gruppi di amministratori del servizio. Se gli utenti di altre foreste sono inclusi in gruppi amministrativi nella foresta isolato, la sicurezza della foresta isolata potenzialmente può essere compromessi perché gli amministratori del servizio nell'insieme di strutture non hanno il controllo esclusivo.  
  
-   Come controller di dominio siano accessibili in una rete, sono soggetti ad attacchi di (ad esempio attacchi denial of service) dal computer in rete. È possibile eseguire le operazioni seguenti per la protezione contro la possibilità di un attacco:  
  
    -   Controller di dominio host solo su reti che sono considerati sicuri.  
  
    -   Limitare l'accesso alla rete o a reti che ospita i controller di dominio.  
  
-   Isolamento dei servizi richiede la creazione di un insieme di strutture. Valutare se il costo di manutenzione dell'infrastruttura per supportare l'insieme di strutture ulteriori compensa i costi associati alla perdita dell'accesso alle risorse a causa di una foresta azienda non disponibile.  
  
    Utenti specifici o le applicazioni potrebbero con esigenze speciali che richiedono la possibilità di passare attraverso il firewall per contattare altri insiemi di strutture. È possibile soddisfare queste esigenze singolarmente aprendo le interfacce appropriate nel firewall, comprese quelle necessarie per tutti i trust di funzionare.  
  
Per ulteriori informazioni sulla configurazione dei firewall per l'uso di dominio Active Directory, vedere Active Directory in reti segmentate tramite firewall ([https://go.microsoft.com/fwlink/?LinkId=37928](https://go.microsoft.com/fwlink/?LinkId=37928)).  
  


