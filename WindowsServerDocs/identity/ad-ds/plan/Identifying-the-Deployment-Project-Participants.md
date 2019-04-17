---
ms.assetid: 50bd2566-e03c-4884-b5c4-895c8aab80aa
title: Identificazione dei partecipanti al progetto di distribuzione
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 57c24c2b2b48c712445ef7dca453de586d33a130
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/12/2017
---
# <a name="identifying-the-deployment-project-participants"></a>Identificazione dei partecipanti al progetto di distribuzione

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

È il primo passaggio nella definizione di un progetto di distribuzione per servizi di dominio Active Directory (AD DS) per stabilire la progettazione e ciclo team di progetto di distribuzione che sarà responsabile per la gestione la fase di progettazione e distribuzione del progetto di Active Directory. Inoltre, è necessario identificare gli utenti e i gruppi che saranno responsabili per proprietario e la gestione della directory dopo il completamento della distribuzione.  
  
-   [Definizione di ruoli specifici del progetto](#BKMK_1)  
  
-   [Stabilire i proprietari e gli amministratori](#BKMK_2)  
  
-   [Team di progetto di creazione](#BKMK_3)  
  
## <a name="BKMK_1"></a>Definizione di ruoli specifici del progetto  
Un passaggio importante per stabilire i team di progetto consiste nell'identificare gli utenti che devono contenere ruoli specifici del progetto. Queste includono lo sponsor executive, l'architetto e il responsabile del progetto. Queste persone sono responsabili per l'esecuzione del progetto di distribuzione di Active Directory.  
  
Dopo designare architetto che del progetto, queste persone stabiliscono i canali di comunicazione in tutta l'organizzazione, creare le pianificazioni di progetto e identificano gli utenti che saranno membri del team di progetto, a partire dai proprietari diversi.  
  
### <a name="executive-sponsor"></a>Sponsor Executive  
Distribuzione di un'infrastruttura, ad esempio servizi di dominio Active Directory può avere un impatto ampio in un'organizzazione. Per questo motivo, è importante disporre di un responsabile in grado di riconoscere il valore di business della distribuzione, supporta il progetto a livello esecutivo e può consentire di risolvere i conflitti nell'intera organizzazione.  
  
### <a name="project-architect"></a>Architetto  
Ogni progetto di distribuzione di Active Directory richiede un architetto di gestire il processo decisionale Active Directory progettazione e distribuzione. Il progettista fornisce assistenza tecnica per agevolare il processo di progettazione e distribuzione di Active Directory.  
  
> [!NOTE]  
> Se non personale esistente nell'organizzazione dispone di progettazione della directory esperienza, puoi incaricare un consulente esterno che è un esperto nella progettazione di Active Directory e la distribuzione.  
  
Di seguito sono elencate le responsabilità di architetto di Active Directory:  
  
-   Proprietario della progettazione di Active Directory  
  
-   Comprensione e la registrazione le motivazioni principali decisioni di progettazione  
  
-   Assicurarsi che la progettazione soddisfi le esigenze dell'organizzazione  
  
-   Definizione di consenso tra team di progettazione, distribuzione e operazioni  
  
-   Comprendere le esigenze di applicazioni AD DS-integrato  
  
Progettazione di Active Directory finale deve riflettere una combinazione di obiettivi aziendali e le decisioni tecniche. Di conseguenza, l'architetto deve esaminare decisioni di progettazione per assicurarsi che siano allineate con obiettivi aziendali.  
  
### <a name="project-manager"></a>Gestione di progetto  
Il responsabile del progetto facilita collaborazione tra business unit e tra gruppi di gestione di tecnologia. In teoria, il gestore di progetto di distribuzione di Active Directory è un utente di all'interno dell'organizzazione che abbia familiarità con entrambi i criteri operativi del gruppo IT e i requisiti di progettazione per i gruppi sono preparazione alla distribuzione di servizi di dominio Active Directory. Il responsabile del progetto sovrintende l'intero progetto di distribuzione, a partire da progettazione e continuando con l'implementazione e assicura che il progetto rimane sulla pianificazione e all'interno di budget. Di seguito sono elencate le responsabilità del manager del progetto:  
  
-   Fornisce la pianificazione, ad esempio la pianificazione e budget progetto di base  
  
-   L'avanzamento del progetto di progettazione e distribuzione di Active Directory  
  
-   Assicurarsi che gli utenti interessati sono coinvolti in ogni parte del processo di progettazione  
  
-   Che funge da singolo punto di contatto per il progetto di distribuzione di Active Directory  
  
-   La comunicazione tra i team di progettazione, distribuzione e operazioni  
  
-   Stabilire e mantenere la comunicazione con la direzione in tutto il progetto di distribuzione  
  
## <a name="BKMK_2"></a>Stabilire i proprietari e gli amministratori  
In un progetto di distribuzione di Active Directory, persone che sono proprietari sono ritenuti responsabili dalla gestione per assicurarsi che sono state completate le attività di distribuzione e la progettazione di Active Directory specifiche di soddisfano le esigenze dell'organizzazione. I proprietari non necessariamente non hanno accesso a o modificare direttamente l'infrastruttura di directory. Gli amministratori sono responsabili per il completamento di attività di distribuzione necessari. Gli amministratori hanno l'accesso alla rete e delle autorizzazioni necessarie per modificare la directory e dell'infrastruttura.  
  
Il ruolo di proprietario è strategica e disponibile. Proprietari sono responsabili per la comunicazione agli amministratori le attività necessarie per l'implementazione della progettazione di Active Directory, ad esempio la creazione di nuovi controller di dominio all'interno della foresta. Gli amministratori sono responsabili per implementare la progettazione della rete in base alle specifiche di progettazione.  
  
In organizzazioni di grandi dimensioni, persone diverse fill ruoli proprietario e l'amministratore. Tuttavia, in alcune organizzazioni di piccole dimensioni, la stessa persona potrebbe fungere sia il proprietario e l'amministratore.  
  
### <a name="service-and-data-owners"></a>Proprietari dei servizi e dati  
La gestione dei servizi di dominio Active Directory su base giornaliera prevede due tipi di proprietari:  
  
-   Proprietari del servizio responsabili per la manutenzione a lungo termine e pianificazione dell'infrastruttura di Active Directory e per garantire che la directory continui a funzionare e che vengono gestiti gli obiettivi stabiliti contratti di servizio  
  
-   Proprietari di dati che sono responsabili per la manutenzione delle informazioni archiviate nella directory. Ciò include l'utente e la gestione delle risorse locali, ad esempio server membri e workstation di gestione degli account computer.  
  
È importante identificare i proprietari di dati e del servizio Active Directory in anticipo in modo che possono partecipare in gran parte del processo di progettazione possibile. Poiché i proprietari di servizi e dati sono responsabili della manutenzione a lungo termine della directory dopo aver terminato il progetto di distribuzione, è importante per questi utenti per fornire informazioni relative alle esigenze dell'organizzazione e per acquisire familiarità con come e perché vengono apportate alcune decisioni di progettazione. I proprietari di servizio includono il proprietario della foresta, il proprietario di Active Directory del sistema DNS (Domain Naming) e il proprietario della topologia del sito. Proprietari di dati includono i proprietari di unità organizzativa (OU).  
  
### <a name="service-and-data-administrators"></a>Amministratori di servizi e dati  
L'operazione di dominio Active Directory include due tipi di amministratori: gli amministratori e gli amministratori dei dati del servizio. Gli amministratori del servizio implementano le decisioni prese dai proprietari e gestiscono le attività quotidiane associate alla gestione dell'infrastruttura e il servizio directory. Ciò include la gestione di controller di dominio che ospitano il servizio directory, la gestione di altri servizi di rete, ad esempio DNS necessari per Active Directory, controllare la configurazione delle impostazioni a livello di foresta e assicurare che la directory è sempre disponibile.  
  
Gli amministratori del servizio sono anche responsabili per il completamento di attività di distribuzione di Active Directory in corso che sono necessari al termine del processo di distribuzione iniziale di Windows Server 2008 Active Directory. Ad esempio, come l'aumento della directory delle richieste, gli amministratori del servizio creano controller di dominio aggiuntivi e stabiliscono o rimuovere trust tra domini, in base alle esigenze. Per questo motivo, il team di distribuzione di Active Directory deve includere gli amministratori del servizio.  
  
È necessario prestare attenzione assegnare ruoli di amministratore del servizio solo a persone attendibili nell'organizzazione. Poiché queste persone hanno la possibilità di modificare i file di sistema nei controller di dominio, è possibile modificare il comportamento di dominio Active Directory. È necessario assicurarsi che gli amministratori del servizio nell'organizzazione siano singoli utenti che hanno familiari con operative e criteri di sicurezza che sono presenti nella rete e che comprendere la necessità di applicare tali criteri.  
  
Gli amministratori di dati sono utenti all'interno di un dominio responsabili sia per i dati archiviati in Active Directory, ad esempio account utente e gruppo di gestione e di mantenere i computer che sono membri del dominio di. Gli amministratori di dati controllano sottoinsiemi di oggetti all'interno della directory e dispongono di alcun controllo sull'installazione o configurazione del servizio directory.  
  
Gli account amministratore di dati non vengono forniti per impostazione predefinita. Dopo che il team di progettazione determina come risorse devono essere gestiti per l'organizzazione, i proprietari del dominio devono creare gli account amministratore di dati e li delegare le autorizzazioni appropriate in base all'insieme di oggetti per cui gli amministratori devono essere responsabile.  
  
È consigliabile limitare il numero di amministratori del servizio nell'organizzazione per il numero minimo necessario per assicurarsi che l'infrastruttura continui a funzionare. La maggior parte delle attività amministrative può essere completata dagli amministratori di dati. Gli amministratori del servizio richiedono una quantità più ampia competenze sono responsabili per la gestione di directory e dell'infrastruttura che la supporta. Gli amministratori dei dati richiedono solo le competenze necessarie per gestire la parte della directory. Dividendo le assegnazioni di lavoro in questo modo ridurre i costi per l'organizzazione perché solo un numero ridotto degli amministratori desidera essere addestrati per utilizzare e gestire l'intera directory e all'infrastruttura.  
  
Ad esempio, un amministratore del servizio deve comprendere come aggiungere un dominio a un insieme di strutture. Sono inclusi come installare il software per convertire un server in un controller di dominio e come modificare l'ambiente DNS in modo che il controller di dominio può essere unito senza problemi in ambiente Active Directory. Un amministratore di dati solo deve sapere come gestire i dati specifici che sono responsabili per, ad esempio la creazione di nuovi account utente per i nuovi dipendenti nelle reparto.  
  
Distribuzione di Active Directory richiede coordinamento e la comunicazione tra molti gruppi diversi coinvolti nell'operazione dell'infrastruttura di rete. Questi gruppi dovrebbero designare i proprietari dei servizi e dati responsabili che rappresenta i vari gruppi durante il processo di progettazione e distribuzione.  
  
Una volta completato il progetto di distribuzione, i proprietari di dati e continuano a essere responsabile per la parte dell'infrastruttura gestito dal relativo gruppo. In un ambiente Active Directory, i proprietari sono il proprietario della foresta, il DNS per il proprietario di dominio Active Directory, il proprietario della topologia del sito e il proprietario dell'unità Organizzativa. I ruoli dei proprietari servizi e dati sono illustrati nelle sezioni seguenti.  
  
#### <a name="forest-owner"></a>Proprietario della foresta  
Il proprietario della foresta è in genere un gestore di reparti IT senior informazioni nell'organizzazione responsabile per il processo di distribuzione di Active Directory e che è responsabile per la gestione di fornitura di servizi all'interno della foresta al termine della distribuzione. Il proprietario della foresta assegna individui per compilare gli altri ruoli di proprietà mediante l'identificazione personale chiave all'interno dell'organizzazione che sono in grado di contribuire le informazioni necessarie sulle esigenze amministrative e di infrastruttura di rete. Il proprietario della foresta è responsabile per le operazioni seguenti:  
  
-   Distribuzione del dominio radice della foresta per creare l'insieme di strutture  
  
-   Distribuzione del primo controller di dominio in ogni dominio per creare i domini richiesti per la foresta  
  
-   Delle appartenenze ai gruppi di amministratore del servizio in tutti i domini della foresta  
  
-   Creazione della progettazione della struttura di unità Organizzativa in ogni dominio nella foresta  
  
-   Delega dell'autorità amministrativa ai proprietari di unità Organizzativa  
  
-   Modifiche allo schema  
  
-   Modifiche alle impostazioni di configurazione a livello di foresta  
  
-   Implementazione di determinate impostazioni dei criteri di criteri di gruppo, inclusi i criteri di account utente di dominio come criterio di blocco account e password granulari per le  
  
-   Impostazioni dei criteri aziendali che si applicano ai controller di dominio  
  
-   Le altre impostazioni di criteri di gruppo che vengono applicati a livello di dominio  
  
Il proprietario della foresta dispone autorità nell'intera foresta. È responsabilità del proprietario della foresta per impostare criteri di gruppo e i criteri aziendali e per selezionare gli utenti che sono amministratori del servizio. Il proprietario della foresta è proprietario di un servizio.  
  
#### <a name="dns-for-ad-ds-owner"></a>DNS per il proprietario di dominio Active Directory  
Il DNS per il proprietario di dominio Active Directory è una persona che ha una conoscenza approfondita dell'infrastruttura DNS esistente e lo spazio dei nomi esistente dell'organizzazione.  
  
Il DNS per il proprietario di dominio Active Directory è responsabile per le operazioni seguenti:  
  
-   Che funge da collegamento tra il team di progettazione e il gruppo IT attualmente proprietario dell'infrastruttura DNS  
  
-   Fornire le informazioni sui nomi DNS esistente dell'organizzazione per facilitare la creazione di nuovi nomi di Active Directory  
  
-   Funziona con il team di distribuzione per assicurarsi che la nuova infrastruttura DNS viene distribuita in base alle specifiche del team di progettazione e che funzioni correttamente  
  
-   La gestione di DNS per l'infrastruttura di dominio Active Directory, tra cui il servizio Server DNS e i dati DNS  
  
Il DNS per il proprietario di dominio Active Directory è il proprietario di un servizio.  
  
#### <a name="site-topology-owner"></a>Proprietario della topologia del sito  
Il proprietario della topologia del sito abbia familiarità con la struttura fisica della rete dell'organizzazione, incluso il mapping di singole subnet, router e le aree di rete che sono connessi tramite collegamenti lenti. Il proprietario della topologia del sito è responsabile per le operazioni seguenti:  
  
-   Informazioni sulla topologia di rete fisica e il relativo impatto sulla di dominio Active Directory  
  
-   Informazioni sulla distribuzione di Active Directory verrà impatto di rete  
  
-   Determinare i siti di Active Directory logici che deve essere creato  
  
-   Aggiornare gli oggetti del sito per i controller di dominio quando viene aggiunto una subnet, modificato o rimosso  
  
-   Creazione di collegamenti di sito, ponti di collegamenti di sito e gli oggetti connessione manuale  
  
Il proprietario della topologia del sito è il proprietario di un servizio.  
  
#### <a name="ou-owner"></a>Proprietario dell'unità Organizzativa  
Il proprietario dell'unità Organizzativa è responsabile della gestione dei dati memorizzati nella directory. Questa persona deve essere familiarità con operative e criteri di sicurezza sono presenti nella rete. Proprietari di unità Organizzative è possono eseguire solo le attività che sono state delegate loro dagli amministratori del servizio e possono eseguire solo le attività sulle unità organizzative in cui vengono assegnati. Attività che è possibile assegnare al proprietario della OU includono quanto segue:  
  
-   Esecuzione di tutte le attività di gestione di account all'interno di OU assegnato  
  
-   La gestione delle workstation e server membri che sono membri della OU assegnato  
  
-   Delega dell'autorità per gli amministratori locali all'interno di OU assegnato  
  
Il proprietario dell'unità Organizzativa è un proprietario dei dati.  
  
## <a name="BKMK_3"></a>Team di progetto di creazione  
I team di progetto di Active Directory sono gruppi temporanei che sono responsabili per il completamento delle attività di progettazione e distribuzione di Active Directory. Una volta completato il progetto di distribuzione di Active Directory, i proprietari assumono la responsabilità per la directory e i team di progetto possono Elimina.  
  
Le dimensioni dei team di progetto variano in base alle dimensioni dell'organizzazione. Nelle organizzazioni di piccole dimensioni, una sola persona può riguardano più aree di responsabilità in un team di progetto e coinvolti in più fasi della distribuzione. Le organizzazioni di grandi dimensioni potrebbero richiedere grandi team con persone diverse o team diversi per diverse aree di responsabilità. La dimensione dei team non è importante, purché non vengono assegnate tutte le aree di responsabilità e gli obiettivi di progettazione dell'organizzazione siano soddisfatti.  
  
### <a name="identifying-potential-forest-owners"></a>Identificazione dei potenziali proprietari di foreste  
Identificare i gruppi all'interno dell'organizzazione che possiedono e controllano le risorse necessarie per fornire servizi di directory per gli utenti della rete. Questi gruppi sono considerati potenziali proprietari di foreste.  
  
La separazione dei dati e servizio Amministrazione di dominio Active Directory rende possibile per l'infrastruttura IT gruppo (o gruppi) di un'organizzazione per gestire il servizio directory, mentre gli amministratori locali in ogni gruppo di gestiscono i dati appartenenti ai gruppi personalizzati. Possibili proprietari di foresta dispongono dell'autorità richiesta tramite l'infrastruttura di rete per distribuire e il supporto di dominio Active Directory.  
  
Per le organizzazioni che dispongono di un gruppo IT di infrastruttura centralizzata, il gruppo IT è in genere il proprietario della foresta e, pertanto, il proprietario della foresta potenziali per le distribuzioni future. Le organizzazioni che includono un numero di indipendente infrastruttura IT gruppi hanno un numero di potenziali proprietari di foreste. Se l'organizzazione ha già un'infrastruttura di Active Directory sul posto, tutti i proprietari dei foresta corrente sono potenziali proprietari di foreste per nuove distribuzioni.  
  
Selezionare uno dei proprietari di foresta potenziale di agire come proprietario della foresta per ogni foresta che si intende per la distribuzione. Questi potenziali proprietari di foresta sono responsabili per l'utilizzo con il team di progettazione per stabilire o meno dell'insieme di strutture effettivamente essere distribuita o se un corso alternativo di azione (ad esempio l'aggiunta di un'altra foresta esistente) è un migliore utilizzo delle risorse disponibili e ancora le loro esigenze. Il proprietario della foresta (o proprietari) nell'organizzazione sono membri del team di progettazione di Active Directory.  
  
### <a name="establishing-a-design-team"></a>Creazione di un team di progettazione  
Il team di progettazione di Active Directory è responsabile per la raccolta di tutte le informazioni necessarie per prendere decisioni sulla progettazione della struttura logica di Active Directory.  
  
Di seguito sono elencate le responsabilità del team di progettazione:  
  
-   Determinare il numero di foreste e domini sono necessari e quali sono le relazioni tra le foreste e domini  
  
-   Utilizzo di proprietari di dati per verificare che la progettazione soddisfi i requisiti di sicurezza e amministrazione  
  
-   Lavorare con gli amministratori di rete corrente per assicurarsi che l'infrastruttura di rete corrente supporta il progetto e che la progettazione non influirà negativamente le applicazioni esistenti distribuite nella rete  
  
-   Funziona con i rappresentanti del gruppo di sicurezza dell'organizzazione per verificare che la progettazione soddisfi i criteri di sicurezza stabiliti  
  
-   Progettazione di strutture di unità Organizzative che consentono di livelli appropriati di protezione e la delega dell'autorità appropriata per i proprietari di dati  
  
-   Funziona con il team di distribuzione per la progettazione di test in un ambiente di ambiente per assicurarsi che funzioni come previsto e modificare la struttura come necessari per risolvere i problemi che si verificano  
  
-   Creazione di una progettazione di topologia del sito che soddisfi i requisiti di replica dell'insieme di strutture, impedendo l'overload di larghezza di banda disponibile. Per ulteriori informazioni sulla progettazione della topologia dei siti, vedere [progettazione sito topologia per Windows Server 2008 Active Directory](https://technet.microsoft.com/library/cc772013.aspx).  
  
-   Funziona con il team di distribuzione per assicurarsi che la progettazione è implementata correttamente  
  
Il team di progettazione include i membri seguenti:  
  
-   Proprietari di foresta  
  
-   Architetto  
  
-   Gestione di progetto  
  
-   Utenti che hanno la responsabili di stabilire e gestire criteri di sicurezza della rete  
  
Durante il processo di progettazione logica struttura, il team di progettazione identifica i proprietari. Necessario avviare queste persone che fanno parte del processo di progettazione non appena vengono identificati. Dopo il progetto di distribuzione viene trasferito al team di distribuzione, il team di progettazione è responsabile per il processo di distribuzione per assicurarsi che il progetto è implementato correttamente di supervisione. Il team di progettazione consente inoltre di rendere le modifiche per il progetto in base al feedback dei test.  
  
### <a name="establishing-a-deployment-team"></a>Creazione di un team di distribuzione  
Il team di distribuzione di Active Directory è responsabile per il testing e implementazione di progettazione della struttura logica di Active Directory. Ciò include le attività seguenti:  
  
-   Creazione di un ambiente di test che emula sufficientemente dell'ambiente di produzione  
  
-   La verifica della progettazione implementando la struttura proposta foresta e del dominio in un ambiente di testing per verificare che soddisfi gli obiettivi di ogni proprietario del ruolo  
  
-   Sviluppo e test tutti gli scenari di migrazione proposte per la progettazione in un ambiente di testing  
  
-   Assicurarsi che ogni proprietario di disconnessione sul processo di test per assicurarsi che le funzionalità di progettazione corretta sono stata individuate  
  
-   Test dell'operazione di distribuzione in un ambiente pilota  
  
Dopo aver completata la progettazione e attività di test, il team di distribuzione esegue le attività seguenti:  
  
-   Crea gli insiemi di strutture e domini in base alla progettazione della struttura logica di Active Directory  
  
-   Crea i siti e oggetti collegamento di sito in base alle esigenze basato sulla progettazione della topologia del sito  
  
-   Assicura che l'infrastruttura DNS è configurato per supportare di dominio Active Directory e che qualsiasi nuovi spazi dei nomi sono integrate nello spazio dei nomi esistente dell'organizzazione  
  
Il team di distribuzione di Active Directory include i membri seguenti:  
  
-   Proprietario della foresta  
  
-   DNS per il proprietario di dominio Active Directory  
  
-   Proprietario della topologia del sito  
  
-   Proprietari di unità Organizzative  
  
Il team di distribuzione funziona con gli amministratori del servizio e i dati durante la fase di distribuzione per garantire che i membri del team di operazioni abbiano familiari con la nuova progettazione. Ciò permette di garantire una transizione uniforme di proprietà al termine dell'operazione di distribuzione. Al termine del processo di distribuzione, la responsabilità di mantenere il nuovo ambiente Active Directory passa al team di operazioni.  
  
### <a name="documenting-the-design-and-deployment-teams"></a>Documentare i team di progettazione e distribuzione  
Documentare i nomi e le informazioni di contatto per gli utenti che faranno parte della progettazione e distribuzione di dominio Active Directory. Identificare chi sarà responsabile di ogni ruolo il team di progettazione e distribuzione. Inizialmente, questo elenco include i potenziali proprietari di foresta, il responsabile del progetto e l'architetto. Quando si determina il numero di foreste che sarà distribuito, potresti dover creare nuovo team di progettazione per le foreste aggiuntive. Si noti che è necessario aggiornare la documentazione come modificare le appartenenze di gruppo e non appena si identificano i vari proprietari di Active Directory durante il processo di progettazione. Per un foglio di lavoro agevolare la documentazione i team di progettazione e distribuzione per ogni insieme di strutture, scaricare Job_Aids_Designing_and_Deploying_Directory_and_Security_Services.zip dal processo Aid per Windows Server 2003 Deployment Kit ([https://go.microsoft.com/fwlink/?LinkID=102558](https://go.microsoft.com/fwlink/?LinkID=102558)) e aprire "Informazioni di Team di progettazione e distribuzione" (DSSLOGI_1.doc).  
  


