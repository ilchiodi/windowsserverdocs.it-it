---
ms.assetid: 50bd2566-e03c-4884-b5c4-895c8aab80aa
title: Identificazione dei partecipanti al progetto di distribuzione
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: efb240fabc4272a4dbef4cb7e86d7058cedb9584
ms.sourcegitcommit: 11421f4005f9f3a3f6c0db95b1836d0f765a9fa3
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2020
ms.locfileid: "81624169"
---
# <a name="identifying-the-deployment-project-participants"></a>Identificazione dei partecipanti al progetto di distribuzione

> Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Il primo passaggio nella creazione di un progetto di distribuzione per Dominio di Active Directory servizio (AD DS) consiste nel definire i team del progetto di progettazione e distribuzione che saranno responsabili della gestione della fase di progettazione e della fase di distribuzione del ciclo del progetto Active Directory. Inoltre, è necessario identificare gli utenti e i gruppi che saranno responsabili del proprietario e della gestione della directory al termine della distribuzione.

- [Definizione di ruoli specifici del progetto](#BKMK_1)

- [Creazione di proprietari e amministratori](#BKMK_2)

- [Compilazione di team di progetto](#BKMK_3)

## <a name="defining-project-specific-roles"></a><a name="BKMK_1"></a>Definizione di ruoli specifici del progetto
Un passaggio importante per stabilire i team del progetto consiste nell'identificare gli individui che devono mantenere i ruoli specifici del progetto. Questi includono lo sponsor esecutivo, il progettista del progetto e il project manager. Questi utenti sono responsabili dell'esecuzione del progetto di distribuzione Active Directory.

Dopo aver nominato Project Architect e project manager, questi individui stabiliscono canali di comunicazione nell'intera organizzazione, compilano pianificazioni di progetti e identificano gli individui che saranno membri dei team del progetto, a partire dai diversi proprietari.

### <a name="executive-sponsor"></a>SPONSOR Executive
La distribuzione di un'infrastruttura come AD DS può avere un notevole effetto su un'organizzazione. Per questo motivo, è importante avere uno sponsor esecutivo che comprende il valore aziendale della distribuzione, supporta il progetto a livello esecutivo e può contribuire a risolvere i conflitti nell'intera organizzazione.

### <a name="project-architect"></a>Progettista progetto
Ogni progetto di distribuzione di Active Directory richiede un progettista di progetto per gestire il processo decisionale di progettazione e distribuzione di Active Directory. Il progettista fornisce competenze tecniche per supportare il processo di progettazione e distribuzione di servizi di dominio Active Directory.

> [!NOTE]
> Se nessun personale esistente dell'organizzazione dispone di un'esperienza di progettazione della directory, potrebbe essere necessario assumere un consulente esterno esperto in Active Directory progettazione e distribuzione.

Di seguito sono riportate le responsabilità di Active Directory Project Architect:

- Proprietario della progettazione Active Directory

- Informazioni e registrazione della logica per le decisioni di progettazione principali

- Garantire che la progettazione soddisfi le esigenze aziendali dell'organizzazione

- Determinazione del consenso tra i team di progettazione, distribuzione e operazioni

- Informazioni sulle esigenze di applicazioni integrate in servizi di dominio Active Directory

Il progetto di Active Directory finale deve riflettere una combinazione di obiettivi aziendali e decisioni tecniche. Pertanto, il progettista del progetto deve esaminare le decisioni di progettazione per assicurarsi che siano allineate agli obiettivi aziendali.

### <a name="project-manager"></a>Project manager
Il project manager facilita la collaborazione tra le business unit e tra i gruppi di gestione tecnologici. Idealmente, la distribuzione di Active Directory project manager è un utente all'interno dell'organizzazione che ha familiarità con i criteri operativi del gruppo IT e con i requisiti di progettazione per i gruppi che si preparano alla distribuzione di servizi di dominio Active Directory. Il project manager controlla l'intero progetto di distribuzione, a partire dalla progettazione e continuando attraverso l'implementazione, e assicura che il progetto rimanga in base alla pianificazione e all'interno del budget. Di seguito sono riportate le responsabilità del project manager:

- Pianificazione del progetto di base, ad esempio pianificazione e budget

- Guida dello stato di avanzamento nel progetto di progettazione e distribuzione Active Directory

- Verificare che i singoli utenti appropriati siano interessati a ogni parte del processo di progettazione

- Fungere da singolo punto di contatto per il progetto di distribuzione Active Directory

- Creazione della comunicazione tra i team di progettazione, distribuzione e operazioni

- Creazione e gestione della comunicazione con lo sponsor Executive nell'intero progetto di distribuzione

## <a name="establishing-owners-and-administrators"></a><a name="BKMK_2"></a>Creazione di proprietari e amministratori
In un progetto di distribuzione di Active Directory, i singoli proprietari sono tenuti responsabili dalla gestione per assicurarsi che le attività di distribuzione siano completate e che Active Directory specifiche di progettazione soddisfino le esigenze dell'organizzazione. I proprietari non possono necessariamente accedere o modificare direttamente l'infrastruttura di directory. Gli amministratori sono gli utenti responsabili del completamento delle attività di distribuzione richieste. Gli amministratori dispongono dell'accesso alla rete e delle autorizzazioni necessarie per modificare la directory e la relativa infrastruttura.

Il ruolo del proprietario è strategico e gestionale. I proprietari sono responsabili della comunicazione con gli amministratori delle attività necessarie per l'implementazione del Active Directory progettazione, ad esempio la creazione di nuovi controller di dominio all'interno della foresta. Gli amministratori sono responsabili dell'implementazione della progettazione sulla rete in base alle specifiche di progettazione.

Nelle organizzazioni di grandi dimensioni, persone diverse riempiono i ruoli di proprietario e amministratore; Tuttavia, in alcune organizzazioni di piccole dimensioni, lo stesso individuo può fungere sia da proprietario che da amministratore.

### <a name="service-and-data-owners"></a>Proprietari di servizi e dati
La gestione di servizi di dominio Active Directory su base giornaliera implica due tipi di proprietari:

- Proprietari del servizio responsabili della pianificazione e della manutenzione a lungo termine dell'infrastruttura Active Directory e per garantire che la directory continui a funzionare e che vengano mantenuti gli obiettivi stabiliti nei contratti di servizio.
- Proprietari di dati responsabili della manutenzione delle informazioni archiviate nella directory. Sono inclusi la gestione e la gestione di account utente e computer per le risorse locali, ad esempio i server membri e le workstation.

È importante identificare tempestivamente il servizio Active Directory e i proprietari dei dati, in modo che possano partecipare alla maggior parte del processo di progettazione possibile. Poiché il servizio e i proprietari dei dati sono responsabili della manutenzione a lungo termine della directory dopo il completamento del progetto di distribuzione, è importante che tali utenti forniscano l'input in merito alle esigenze organizzative e conoscano come e perché vengono prese determinate decisioni di progettazione. I proprietari del servizio includono il proprietario della foresta, il proprietario DNS (Dominio di Active Directory Naming System) e il proprietario della topologia del sito. I proprietari dei dati includono le unità organizzative (OU).

### <a name="service-and-data-administrators"></a>Amministratori di servizi e dati
Il funzionamento di servizi di dominio Active Directory prevede due tipi di amministratori: amministratori del servizio e amministratori dei dati. Gli amministratori del servizio implementano le decisioni relative ai criteri prese dai proprietari dei servizi e gestiscono le attività quotidiane associate alla gestione del servizio directory e dell'infrastruttura. Ciò include la gestione dei controller di dominio che ospitano il servizio directory, la gestione di altri servizi di rete, ad esempio DNS, necessari per servizi di dominio Active Directory, il controllo della configurazione delle impostazioni a livello di foresta e la garanzia che la directory sia sempre disponibile.

Gli amministratori del servizio sono inoltre responsabili del completamento delle attività di distribuzione Active Directory necessarie dopo il completamento del processo di distribuzione Active Directory iniziale di Windows Server 2008. Ad esempio, con l'aumentare delle richieste di directory, gli amministratori del servizio creano controller di dominio aggiuntivi e stabiliscono o rimuovono i trust tra i domini, in base alle esigenze. Per questo motivo, il team di distribuzione di Active Directory deve includere gli amministratori del servizio.

È necessario prestare attenzione a assegnare i ruoli di amministratore del servizio solo a utenti attendibili nell'organizzazione. Poiché tali utenti hanno la possibilità di modificare i file di sistema nei controller di dominio, possono modificare il comportamento di servizi di dominio Active Directory. È necessario assicurarsi che gli amministratori del servizio all'interno dell'organizzazione siano utenti che hanno familiarità con i criteri operativi e di sicurezza presenti nella rete e che conoscono la necessità di applicare tali criteri.

Gli amministratori di dati sono utenti all'interno di un dominio responsabile sia per la gestione dei dati archiviati in servizi di dominio Active Directory, ad esempio gli account utente e di gruppo, sia per la gestione dei computer membri del proprio dominio. Gli amministratori dei dati controllano i subset di oggetti all'interno della directory e non hanno alcun controllo sull'installazione o la configurazione del servizio directory.

Gli account amministratore dati non vengono forniti per impostazione predefinita. Dopo che il team di progettazione ha determinato la modalità di gestione delle risorse per l'organizzazione, i proprietari del dominio devono creare gli account amministratore dati e delegarli le autorizzazioni appropriate in base al set di oggetti per cui gli amministratori devono essere responsabili.

È consigliabile limitare il numero di amministratori del servizio nell'organizzazione al numero minimo necessario per garantire il funzionamento dell'infrastruttura. La maggior parte del lavoro amministrativo può essere completata dagli amministratori dei dati. Gli amministratori del servizio richiedono un set di competenze molto più ampio perché sono responsabili della gestione della directory e dell'infrastruttura che la supporta. Gli amministratori di dati richiedono solo le competenze necessarie per gestire la propria parte della directory. Suddividere le assegnazioni di lavoro in questo modo comporta un risparmio sui costi per l'organizzazione, perché solo un numero ridotto di amministratori deve essere sottoposto a training per operare e gestire l'intera directory e la relativa infrastruttura.

Ad esempio, un amministratore del servizio deve comprendere come aggiungere un dominio a una foresta. Questo include la modalità di installazione del software per convertire un server in un controller di dominio e come modificare l'ambiente DNS in modo che il controller di dominio possa essere unito facilmente all'ambiente Active Directory. Un amministratore dei dati deve solo essere in grado di gestire i dati specifici di cui sono responsabili, ad esempio la creazione di nuovi account utente per i nuovi dipendenti del reparto.

La distribuzione di servizi di dominio Active Directory richiede il coordinamento e la comunicazione tra molti gruppi diversi implicati nel funzionamento dell'infrastruttura di rete. Questi gruppi devono designare i proprietari di servizi e dati responsabili della rappresentazione dei diversi gruppi durante il processo di progettazione e distribuzione.

Una volta completato il progetto di distribuzione, questi proprietari di servizi e dati continuano a essere responsabili della parte dell'infrastruttura gestita dal gruppo. In un ambiente di Active Directory, questi proprietari sono il proprietario della foresta, il DNS per il proprietario di servizi di dominio Active Directory, il proprietario della topologia del sito e il proprietario dell'unità organizzativa. Le sezioni seguenti illustrano i ruoli di questi proprietari di servizi e dati.

#### <a name="forest-owner"></a>Proprietario foresta
Il proprietario della foresta è in genere un responsabile IT (Senior Information Technology) nell'organizzazione responsabile del processo di distribuzione Active Directory e che è infine responsabile della gestione della distribuzione dei servizi nella foresta al termine della distribuzione. Il proprietario della foresta assegna a singoli utenti la possibilità di compilare gli altri ruoli di proprietà identificando il personale principale nell'organizzazione che è in grado di fornire le informazioni necessarie sull'infrastruttura di rete e le esigenze amministrative. Il proprietario della foresta è responsabile per gli elementi seguenti:

- Distribuzione del dominio radice della foresta per creare la foresta

- Distribuzione del primo controller di dominio in ogni dominio per creare i domini necessari per la foresta

- Appartenenze dei gruppi di amministratori del servizio in tutti i domini della foresta

- Creazione della progettazione della struttura dell'unità organizzativa per ogni dominio nella foresta

- Delega dell'autorità amministrativa ai proprietari delle unità organizzative

- Modifiche allo schema

- Modifiche alle impostazioni di configurazione a livello di foresta

- Implementazione di alcune impostazioni dei criteri di Criteri di gruppo, inclusi i criteri di account utente di dominio, ad esempio i criteri granulari per le password e il blocco degli account

- Impostazioni dei criteri di business applicabili ai controller di dominio

- Tutte le altre impostazioni di Criteri di gruppo applicate a livello di dominio

Il proprietario della foresta dispone di autorità per l'intera foresta. È responsabilità del proprietario della foresta impostare Criteri di gruppo e criteri aziendali e selezionare gli utenti che sono amministratori del servizio. Il proprietario della foresta è un proprietario del servizio.

#### <a name="dns-for-ad-ds-owner"></a>DNS per il proprietario di servizi di dominio Active Directory
Il DNS per il proprietario di servizi di dominio Active Directory è un utente che ha una conoscenza approfondita dell'infrastruttura DNS esistente e dello spazio dei nomi esistente dell'organizzazione.

Il DNS per il proprietario di servizi di dominio Active Directory è responsabile dei seguenti elementi:

- Fungere da collegamento tra il team di progettazione e il gruppo IT attualmente proprietario dell'infrastruttura DNS

- Fornire le informazioni sullo spazio dei nomi DNS esistente dell'organizzazione per semplificare la creazione del nuovo spazio dei nomi Active Directory

- Collaborare con il team di distribuzione per assicurarsi che la nuova infrastruttura DNS venga distribuita in base alle specifiche del team di progettazione e che funzioni correttamente

- Gestione del DNS per l'infrastruttura di servizi di dominio Active Directory, inclusi il servizio server DNS e i dati DNS

Il DNS per il proprietario di servizi di dominio Active Directory è un proprietario del servizio.

#### <a name="site-topology-owner"></a>Proprietario della topologia del sito
Il proprietario della topologia del sito ha familiarità con la struttura fisica della rete aziendale, incluso il mapping di singole subnet, router e aree di rete connesse tramite collegamenti lenti. Il proprietario della topologia del sito è responsabile dei seguenti elementi:

- Informazioni sulla topologia di rete fisica e su come influiscono su servizi di dominio Active Directory

- Informazioni sul modo in cui la distribuzione del Active Directory avrà un effetto sulla rete

- Determinazione del Active Directory i siti logici che devono essere creati

- Aggiornamento degli oggetti del sito per i controller di dominio quando una subnet viene aggiunta, modificata o rimossa

- Creazione di collegamenti di sito, ponti di collegamenti di sito e oggetti di connessione manuali

Il proprietario della topologia del sito è un proprietario del servizio.

#### <a name="ou-owner"></a>Proprietario unità organizzativa
Il proprietario dell'unità organizzativa è responsabile della gestione dei dati archiviati nella directory. Questo utente deve avere familiarità con i criteri operativi e di sicurezza presenti nella rete. I proprietari delle unità organizzative possono eseguire solo le attività che sono state delegate dagli amministratori del servizio e possono eseguire solo le attività nelle unità organizzative a cui sono state assegnate. Di seguito sono riportate le attività che possono essere assegnate al proprietario dell'unità organizzativa:

- Esecuzione di tutte le attività di gestione degli account nell'unità organizzativa assegnata

- Gestione delle workstation e dei server membro membri dell'unità organizzativa assegnata

- Delega dell'autorità agli amministratori locali all'interno dell'unità organizzativa assegnata

Il proprietario dell'unità organizzativa è un proprietario di dati.

## <a name="building-project-teams"></a><a name="BKMK_3"></a>Compilazione di team di progetto
Active Directory i team di progetto sono gruppi temporanei responsabili del completamento delle attività di progettazione e distribuzione di Active Directory. Al termine del Active Directory progetto di distribuzione, i proprietari assumono la responsabilità della directory e i team del progetto possono dissociare.

Le dimensioni dei team del progetto variano in base alle dimensioni dell'organizzazione. Nelle organizzazioni di piccole dimensioni, un singolo utente può coprire più aree di responsabilità di un team di progetto e essere interessato da più fasi della distribuzione. Le organizzazioni di grandi dimensioni potrebbero richiedere team di grandi dimensioni con persone diverse o persino team diversi che coprono le diverse aree di responsabilità. Le dimensioni dei team non sono importanti a condizione che vengano assegnate tutte le aree di responsabilità e che vengano soddisfatti gli obiettivi di progettazione dell'organizzazione.

### <a name="identifying-potential-forest-owners"></a>Identificazione di potenziali proprietari della foresta
Identificare i gruppi all'interno dell'organizzazione che possiedono e controllano le risorse necessarie per fornire servizi directory agli utenti in rete. Questi gruppi sono considerati potenziali proprietari della foresta.

La separazione tra l'amministrazione dei servizi e i dati in servizi di dominio Active Directory consente all'infrastruttura di gruppo (o gruppi) di un'organizzazione di gestire il servizio directory, mentre gli amministratori locali di ogni gruppo gestiscono i dati appartenenti ai propri gruppi. I proprietari di foreste potenziali hanno l'autorità necessaria sull'infrastruttura di rete per distribuire e supportare servizi di dominio Active Directory.

Per le organizzazioni che dispongono di un gruppo IT centralizzato di infrastruttura, il gruppo IT è in genere il proprietario della foresta e, di conseguenza, il potenziale proprietario della foresta per le distribuzioni future. Le organizzazioni che includono diversi gruppi IT di infrastruttura indipendente hanno un certo numero di proprietari di foreste potenziali. Se l'organizzazione dispone già di un'infrastruttura Active Directory, tutti i proprietari di foreste correnti sono anche proprietari di foreste potenziali per le nuove distribuzioni.

Selezionare uno dei proprietari della foresta potenziali da usare come proprietario della foresta per ogni foresta che si sta considerando per la distribuzione. Questi potenziali proprietari della foresta sono responsabili dell'utilizzo del team di progettazione per determinare se la loro foresta verrà effettivamente distribuita o se un'azione alternativa, ad esempio l'aggiunta a un'altra foresta esistente, è un uso migliore delle risorse disponibili e soddisfa comunque le proprie esigenze. Il proprietario o i proprietari della foresta nell'organizzazione sono membri del team di progettazione di Active Directory.

### <a name="establishing-a-design-team"></a>Creazione di un team di progettazione
Il team di progettazione Active Directory è responsabile della raccolta di tutte le informazioni necessarie per prendere decisioni relative alla progettazione della struttura logica di Active Directory.

Di seguito sono riportate le responsabilità del team di progettazione:

- Determinare il numero di foreste e domini richiesti e le relazioni tra foreste e domini

- Collaborazione con i proprietari dei dati per garantire che la progettazione soddisfi i requisiti di sicurezza e amministrativi

- Collaborare con gli amministratori di rete correnti per garantire che l'infrastruttura di rete corrente supporti la progettazione e che la progettazione non influirà negativamente sulle applicazioni esistenti distribuite nella rete

- Collaborare con i rappresentanti del gruppo di sicurezza dell'organizzazione per assicurarsi che la progettazione soddisfi i criteri di sicurezza stabiliti

- Progettazione di strutture OU che consentono livelli appropriati di protezione e la delega appropriata dell'autorità ai proprietari di dati

- Collaborare con il team di distribuzione per testare la progettazione in un ambiente lab per assicurarsi che funzioni come pianificate e modifichi la progettazione, in base alle esigenze, per risolvere eventuali problemi che si verificano

- Creazione di una progettazione della topologia del sito che soddisfi i requisiti di replica della foresta, evitando l'overload della larghezza di banda disponibile. Per ulteriori informazioni sulla progettazione della topologia del sito, vedere [progettazione della topologia del sito per servizi di dominio Active Directory di Windows Server 2008](https://technet.microsoft.com/library/cc772013.aspx).

- Collaborare con il team di distribuzione per assicurarsi che la progettazione sia implementata correttamente

Il team di progettazione include i membri seguenti:

- Proprietari di foreste potenziali

- Progettista progetto

- Project manager

- Utenti responsabili della creazione e della gestione dei criteri di sicurezza sulla rete

Durante il processo di progettazione della struttura logica, il team di progettazione identifica gli altri proprietari. Questi utenti devono iniziare a partecipare al processo di progettazione non appena vengono identificati. Dopo che il progetto di distribuzione è stato consegnato al team di distribuzione, il team di progettazione è responsabile della supervisione del processo di distribuzione per garantire che la progettazione venga implementata correttamente. Il team di progettazione apporta anche modifiche alla progettazione in base ai commenti e suggerimenti dei test.

### <a name="establishing-a-deployment-team"></a>Creazione di un team di distribuzione
Il team di distribuzione Active Directory è responsabile del test e dell'implementazione del Active Directory progettazione della struttura logica. Sono incluse le attività seguenti:

- Creazione di un ambiente di test che emula sufficientemente l'ambiente di produzione

- Test della progettazione implementando la foresta e la struttura di dominio proposte in un ambiente lab per verificare che soddisfi gli obiettivi di ogni proprietario del ruolo

- Sviluppo e test di qualsiasi scenario di migrazione proposto dalla progettazione in un ambiente lab

- Verificare che ogni proprietario si connetta al processo di test per assicurarsi che vengano testate le funzionalità di progettazione corrette

- Test dell'operazione di distribuzione in un ambiente pilota

Al termine delle attività di progettazione e test, il team di distribuzione esegue le attività seguenti:

- Crea le foreste e i domini in base al Active Directory progettazione della struttura logica

- Crea i siti e gli oggetti di collegamento di sito come necessario in base alla progettazione della topologia del sito

- Garantisce che l'infrastruttura DNS sia configurata per supportare servizi di dominio Active Directory e che tutti i nuovi spazi dei nomi siano integrati nello spazio dei nomi esistente dell'organizzazione

Il team di distribuzione di Active Directory include i membri seguenti:

- Proprietario foresta

- DNS per il proprietario di servizi di dominio Active Directory

- Proprietario della topologia del sito

- Proprietari OU

Il team di distribuzione collabora con il servizio e gli amministratori dei dati durante la fase di distribuzione per garantire che i membri del team operativo abbiano familiarità con la nuova progettazione. Ciò consente di garantire una transizione uniforme della proprietà al termine dell'operazione di distribuzione. Al termine del processo di distribuzione, la responsabilità della gestione del nuovo ambiente di Active Directory passa al team operativo.

### <a name="documenting-the-design-and-deployment-teams"></a>Documentazione dei team di progettazione e distribuzione
Documentare i nomi e le informazioni di contatto per gli utenti che parteciperanno alla progettazione e alla distribuzione di servizi di dominio Active Directory. Identificare chi sarà responsabile di ogni ruolo nei team di progettazione e distribuzione. Inizialmente, questo elenco include i proprietari delle foreste potenziali, il project manager e l'architetto del progetto. Quando si determina il numero di foreste che si intende distribuire, potrebbe essere necessario creare nuovi team di progettazione per foreste aggiuntive. Si noti che è necessario aggiornare la documentazione quando le appartenenze ai team cambiano e quando si identificano i vari proprietari Active Directory durante il processo di progettazione. Per un foglio di lavoro in cui è possibile documentare i team di progettazione e distribuzione per ogni foresta, scaricare Job_Aids_Designing_and_Deploying_Directory_and_Security_Services. zip da supporto [per i processi per Windows Server 2003 Deployment Kit](https://microsoft.com/download/details.aspx?id=9608) e aprire "informazioni sul team di progettazione e distribuzione" (DSSLOGI_1. doc).
