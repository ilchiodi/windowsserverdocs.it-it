---
ms.assetid: 50bd2566-e03c-4884-b5c4-895c8aab80aa
title: Identificazione dei partecipanti al progetto di distribuzione
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 962825494c613dfef808ce54dfba22727abf6878
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59834652"
---
# <a name="identifying-the-deployment-project-participants"></a>Identificazione dei partecipanti al progetto di distribuzione

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

È il primo passaggio nella definizione di un progetto di distribuzione per servizi di dominio Active Directory (AD DS) per stabilire la progettazione e del ciclo di team di progetto di distribuzione che sarà responsabile della gestione la fase di progettazione e distribuzione del progetto di Active Directory. Inoltre, è necessario identificare gli utenti singoli e i gruppi che saranno responsabili per proprietario e la gestione della directory dopo aver completato la distribuzione.  
  
-   [Definizione dei ruoli specifici del progetto](#BKMK_1)  
  
-   [Tentativo di stabilire i proprietari e gli amministratori](#BKMK_2)  
  
-   [Team di progetto predefiniti](#BKMK_3)  
  
## <a name="BKMK_1"></a>Definizione dei ruoli specifici del progetto  
Un passaggio importante per stabilire il team di progetto consiste nell'identificare gli utenti che devono contenere ruoli specifici del progetto. Sono inclusi lo sponsor direttivo, il progettista di progetto e il project manager. Questi utenti sono responsabili per l'esecuzione del progetto di distribuzione di Active Directory.  
  
Dopo che l'utente autorizza l'architetto di project e project manager, questi singoli stabilire canali di comunicazione all'interno dell'organizzazione, creare pianificazioni di progetto e identificare gli utenti che saranno membri del team di progetto, a partire dal proprietari diversi.  
  
### <a name="executive-sponsor"></a>Sponsor direttivo  
Distribuzione di un'infrastruttura, ad esempio Active Directory Domain Services può avere un impatto ampio in un'organizzazione. Per questo motivo, è importante disporre di un dirigente che riconosca il valore aziendale della distribuzione, supporta il progetto a livello esecutivo e può consentire di risolvere i conflitti nell'intera organizzazione.  
  
### <a name="project-architect"></a>Architetto di progetto  
Ogni progetto di distribuzione di Active Directory richiede un progettista di progetto gestire il processo decisionale Active Directory progettazione e la distribuzione. L'architetto di servizi offre assistenza tecnica per agevolare il processo di progettazione e distribuzione di Active Directory Domain Services.  
  
> [!NOTE]  
> Se nessun membro del personale esistente all'interno dell'organizzazione hanno progettazione directory esperienza, si potrebbe voler assumere un consulente esterno che è un esperto nella progettazione di Active Directory e la distribuzione.  
  
Le responsabilità del progettista Active Directory progetto includono quanto segue:  
  
-   Proprietario la progettazione di Active Directory  
  
-   La comprensione e registrando le motivazioni principali decisioni di progettazione  
  
-   Verificare che la progettazione soddisfi le esigenze aziendali dell'organizzazione  
  
-   Tentativo di stabilire un consenso tra Progettazione, distribuzione e i team operativi  
  
-   Comprendere le esigenze delle applicazioni integrate AD  
  
Il progetto finale di Active Directory deve riflettere una combinazione di obiettivi aziendali e le decisioni tecniche. Pertanto, l'architetto deve esaminare le decisioni di progettazione per garantire che essi siano allineate agli obiettivi aziendali.  
  
### <a name="project-manager"></a>Gestione del progetto  
Il project manager facilita la collaborazione tra business unit e tra i gruppi di gestione di tecnologia. Idealmente, il responsabile di progetto di distribuzione di Active Directory è un utente all'interno dell'organizzazione che hanno familiarità con entrambi i criteri operativi del gruppo IT e i requisiti di progettazione per i gruppi che sono preparazione alla distribuzione di Active Directory Domain Services. Il project manager controlla l'intero progetto di distribuzione, che iniziano con la progettazione e continuare tramite l'implementazione e assicura che il progetto resti su pianificazione e nel rispetto del budget. Le responsabilità del responsabile di progetto includono quanto segue:  
  
-   Che fornisce pianificazione, ad esempio la pianificazione e determinazione del budget del progetto base  
  
-   L'avanzamento del progetto di progettazione e distribuzione di Active Directory  
  
-   Assicurarsi che gli utenti interessati siano interessati da ogni parte del processo di progettazione  
  
-   Che funge da singolo punto di contatto per il progetto di distribuzione di Active Directory  
  
-   La comunicazione tra i team di progettazione, distribuzione e operazioni  
  
-   Stabilire e mantenere la comunicazione con lo sponsor direttivo in tutto il progetto di distribuzione  
  
## <a name="BKMK_2"></a>Tentativo di stabilire i proprietari e gli amministratori  
In un progetto di distribuzione di Active Directory, persone che sono i proprietari sono ritenuti responsabili per la gestione per assicurarsi che la distribuzione sono state completate le attività e la progettazione di Active Directory specifiche di soddisfano le esigenze dell'organizzazione. I proprietari non necessariamente avere accesso a o modificare direttamente l'infrastruttura di directory. Gli amministratori sono responsabili per completare le attività di distribuzione necessarie. Gli amministratori hanno l'accesso alla rete e delle autorizzazioni necessarie per modificare la directory e dell'infrastruttura.  
  
Il ruolo del proprietario è strategica e gestionale. I proprietari sono responsabili per la comunicazione per gli amministratori le attività necessarie per l'implementazione della progettazione, ad esempio la creazione di nuovi controller di dominio all'interno della foresta Active Directory. Gli amministratori sono responsabili dell'implementazione di progettazione in rete in base alle specifiche di progettazione.  
  
Nelle organizzazioni di grandi dimensioni, i vari utenti è riempire i ruoli proprietario e amministratore; Tuttavia, in alcune organizzazioni di piccole dimensioni, la stessa persona potrebbe fungere sia il proprietario e l'amministratore.  
  
### <a name="service-and-data-owners"></a>Proprietari del servizio e dati  
La gestione di Active Directory Domain Services su base giornaliera prevede due tipi di proprietari di:  
  
-   Proprietari del servizio responsabili per la manutenzione a lungo termine e della pianificazione dell'infrastruttura di Active Directory e per garantire che la directory continui a funzionare e che vengono mantenuti gli obiettivi stabiliti in contratti di servizio  
  
-   Proprietari di dati che sono responsabili per la manutenzione delle informazioni archiviate nella directory. Ciò include l'utente e la gestione degli account computer e la gestione delle risorse locali, ad esempio i server membri e workstation.  
  
È importante identificare tempestivamente i proprietari del servizio e i dati di Active Directory in modo che possono partecipare in gran parte del processo di progettazione possibile. Poiché i proprietari del servizio e i dati sono responsabili del mantenimento a lungo termine della directory dopo aver completato il progetto di distribuzione, è importante che queste persone per fornire informazioni relative alle esigenze dell'organizzazione e avere familiarità con come e perché vengono apportate determinate decisioni di progettazione. I proprietari del servizio includono il proprietario della foresta, il proprietario di Active Directory del sistema DNS (Domain Naming) e il proprietario della topologia del sito. I proprietari di dati includono i proprietari di un'unità organizzativa (OU).  
  
### <a name="service-and-data-administrators"></a>Amministratori del servizio e i dati  
L'operazione di dominio Active Directory prevede due tipi di amministratore: gli amministratori e gli amministratori dei dati del servizio. Gli amministratori del servizio implementano le decisioni prese dai proprietari e gestiscono le attività quotidiane associate alla manutenzione dell'infrastruttura e il servizio directory. Ciò include la gestione di controller di dominio che sono host del servizio directory, la gestione di altri servizi di rete, ad esempio DNS necessarie per Active Directory Domain Services, il controllo della configurazione delle impostazioni a livello di foresta e assicurando che la directory è sempre è disponibile.  
  
Gli amministratori del servizio sono anche responsabili del completamento in corso attività di distribuzione di Active Directory che sono necessari al termine del processo di distribuzione iniziale di Windows Server 2008 Active Directory. Ad esempio, come richieste per l'aumento di directory, gli amministratori del servizio creano controller di dominio aggiuntivi e stabiliscano o rimuovere trust tra domini, in base alle esigenze. Per questo motivo, il team di distribuzione di Active Directory deve includere gli amministratori del servizio.  
  
È necessario prestare attenzione assegnare ruoli di amministratore del servizio solo agli utenti attendibili all'interno dell'organizzazione. Poiché queste persone hanno la possibilità di modificare i file di sistema sui controller di dominio, è possibile modificare il comportamento di Active Directory Domain Services. È necessario assicurarsi che gli amministratori del servizio nell'organizzazione sono persone che hanno familiarità con operativo e i criteri di sicurezza che sono presenti nella rete e che comprendano la necessità di applicare tali criteri.  
  
Gli amministratori dei dati sono gli utenti all'interno di un dominio che hanno la responsabili sia per la gestione dei dati archiviati in Active Directory Domain Services, ad esempio gli account utente e gruppo e di mantenere i computer che sono membri del dominio. Gli amministratori dei dati controllano subset degli oggetti all'interno della directory e non può essere controllata tramite l'installazione o configurazione del servizio directory.  
  
Gli account di amministratore dei dati non disponibili per impostazione predefinita. Dopo che il team di progettazione determina come devono essere gestiti per l'organizzazione delle risorse, i proprietari del dominio è necessario creare gli account di amministratore dei dati e li delegare le autorizzazioni appropriate basate sul set di oggetti per cui gli amministratori sono responsabili .  
  
È consigliabile limitare il numero di amministratori del servizio nell'organizzazione per il numero minimo necessario per assicurare che l'infrastruttura continuerà a funzionare. La maggior parte delle attività amministrative può essere completata da parte degli amministratori dei dati. Gli amministratori del servizio richiedono una molto più ampia set di competenze perché sono responsabili della gestione della directory e l'infrastruttura che lo supporta. Gli amministratori dei dati richiedono soltanto le competenze necessarie per gestire la propria parte di directory. Divisione di assegnazioni di lavoro in questo modo comporta risparmi sui costi per l'organizzazione perché solo un numero ridotto degli amministratori desidera essere addestrati a usare e gestire l'intera directory e all'infrastruttura.  
  
Ad esempio, un amministratore del servizio deve comprendere come aggiungere un dominio a un insieme di strutture. Sono inclusi come installare il software per convertire un server in un controller di dominio e come modificare l'ambiente di DNS in modo che il controller di dominio può essere unito con facilità nell'ambiente di Active Directory. Un amministratore dei dati deve solo sapere come gestire i dati specifici che si occupano di, ad esempio la creazione di nuovi account utente per i nuovi dipendenti nel dipartimento.  
  
La distribuzione di Active Directory Domain Services richiede il coordinamento e la comunicazione tra i molti gruppi differenti coinvolti nell'operazione dell'infrastruttura di rete. Questi gruppi devono designare i proprietari del servizio e i dati che sono responsabili che rappresentano i vari gruppi durante il processo di progettazione e la distribuzione.  
  
Una volta completato il progetto di distribuzione, i proprietari del servizio e i dati continuano a essere responsabile per la parte dell'infrastruttura gestita dal relativo gruppo. In un ambiente Active Directory, i proprietari sono il proprietario della foresta, il DNS per il proprietario di Active Directory Domain Services, il proprietario della topologia del sito e il proprietario dell'unità Organizzativa. I ruoli di questi proprietari del servizio e i dati sono illustrati nelle sezioni seguenti.  
  
#### <a name="forest-owner"></a>Proprietario della foresta  
Il proprietario della foresta è in genere un senior information technology (IT) manager nell'organizzazione chi è responsabile per il processo di distribuzione di Active Directory e chi è responsabile per la gestione di erogazione del servizio all'interno della foresta dopo il la distribuzione è stata completata. Il proprietario della foresta viene assegnato agli utenti di compilare gli altri ruoli di proprietà grazie all'identificazione personale chiave all'interno dell'organizzazione che sono in grado di contribuire con le informazioni necessarie sulle esigenze amministrative e dell'infrastruttura di rete. Il proprietario della foresta è responsabile per le operazioni seguenti:  
  
-   Distribuzione del dominio radice della foresta per creare l'insieme di strutture  
  
-   Distribuzione del primo controller di dominio in ogni dominio per creare i domini richiesti per la foresta  
  
-   Appartenenze dei gruppi di amministratore del servizio in tutti i domini della foresta  
  
-   Creazione della progettazione della struttura di OU per ogni dominio della foresta  
  
-   Delega dell'autorità amministrativa ai proprietari di unità Organizzativa  
  
-   Modifiche dello schema  
  
-   Modifiche alle impostazioni di configurazione a livello di foresta  
  
-   Implementazione di alcune impostazioni dei criteri di criteri di gruppo, inclusi i criteri di account utente di dominio, ad esempio criteri di blocco degli account e password con granularità fine  
  
-   Impostazioni dei criteri di business che si applicano ai controller di dominio  
  
-   Eventuali altre impostazioni di criteri di gruppo che vengono applicate a livello di dominio  
  
Il proprietario della foresta ha autorità su nell'intera foresta. È responsabilità del proprietario della foresta per impostare criteri di gruppo e criteri di business e per selezionare gli utenti che sono amministratori del servizio. Il proprietario della foresta è un proprietario del servizio.  
  
#### <a name="dns-for-ad-ds-owner"></a>DNS per il proprietario di Active Directory Domain Services  
Il DNS per il proprietario di Active Directory Domain Services è una persona che ha una conoscenza approfondita dell'infrastruttura DNS esistente e lo spazio dei nomi esistente dell'organizzazione.  
  
Il DNS per il proprietario di Active Directory Domain Services è responsabile per le operazioni seguenti:  
  
-   Che funge da un anello di congiunzione tra il team di progettazione e il gruppo IT che attualmente possiede l'infrastruttura DNS  
  
-   Fornire le informazioni sui nomi DNS esistenti dell'organizzazione per facilitare la creazione di nuovi nomi di Active Directory  
  
-   Collabora con il team di distribuzione per assicurarsi che la nuova infrastruttura DNS viene distribuita in base alle specifiche del team di progettazione e che funzioni correttamente  
  
-   Gestisce il DNS per l'infrastruttura Active Directory Domain Services, tra cui il servizio Server DNS e i dati DNS  
  
Il DNS per il proprietario di Active Directory Domain Services è un proprietario del servizio.  
  
#### <a name="site-topology-owner"></a>Proprietario della topologia del sito  
Il proprietario della topologia del sito abbia familiarità con la struttura fisica della rete dell'organizzazione, incluso il mapping di singole subnet, i router e le aree di rete che sono connessi tramite collegamenti lenti. Il proprietario della topologia del sito è responsabile per le operazioni seguenti:  
  
-   Informazioni sulla topologia di rete fisica e sull'influenza Active Directory Domain Services  
  
-   Comprendere come la distribuzione di Active Directory influirà sulla rete  
  
-   Determinare i siti di Active Directory logici che dovranno essere creati  
  
-   Aggiornare gli oggetti del sito per i controller di dominio quando viene aggiunta una subnet, modificato o rimosso  
  
-   Creazione di collegamenti di sito, ponti di collegamenti di sito e gli oggetti di connessione manuale  
  
Il proprietario di topologia del sito è un proprietario del servizio.  
  
#### <a name="ou-owner"></a>Proprietario dell'unità Organizzativa  
Il proprietario dell'unità Organizzativa è responsabile della gestione dei dati archiviati nella directory. Questa persona dovrà essere che hanno familiarità con operativo e i criteri di sicurezza che sono presenti nella rete. I proprietari di unità Organizzativa possono eseguire solo le attività che sono state delegate ad essi dagli amministratori del servizio e possono eseguire solo le attività su unità organizzative a cui sono assegnate. Attività che è possibile assegnare al proprietario dell'unità Organizzativa includono quanto segue:  
  
-   Esecuzione di tutte le attività di gestione di account all'interno della propria unità amministrativa assegnato  
  
-   La gestione delle workstation e server membro che sono membri della OU assegnato  
  
-   Delega dell'autorità agli amministratori locali all'interno della propria unità amministrativa assegnato  
  
Il proprietario dell'unità Organizzativa è proprietario dei dati.  
  
## <a name="BKMK_3"></a>Team di progetto predefiniti  
I team di progetto di Active Directory sono gruppi temporanei che sono responsabili per il completamento delle attività di progettazione e distribuzione di Active Directory. Una volta completato il progetto di distribuzione di Active Directory, i proprietari di assumono la responsabilità della directory e il team di progetto può Elimina.  
  
Le dimensioni dei team di progetto variano in base alle dimensioni dell'organizzazione. Nelle organizzazioni di piccole dimensioni, una singola persona può coprono più aree di responsabilità in un progetto team ed essere coinvolto nel più di una fase della distribuzione. Le organizzazioni di grandi dimensioni potrebbero richiedere i team più grandi con persone diverse o persino diversi team che coprono le diverse aree di responsabilità. Le dimensioni dei team non sono importante, purché tutte le aree di responsabilità assegnate e gli obiettivi di progettazione dell'organizzazione siano soddisfatti.  
  
### <a name="identifying-potential-forest-owners"></a>Identificazione di potenziali proprietari dell'insieme di strutture  
Identificare i gruppi all'interno dell'organizzazione che possiede e controllano le risorse necessarie per fornire servizi di directory per gli utenti della rete. Questi gruppi sono considerati proprietari dell'insieme di strutture.  
  
La separazione di amministrazione del servizio e i dati in Active Directory Domain Services rende possibile per l'infrastruttura IT gruppo (o gruppi) di un'organizzazione per gestire il servizio directory, mentre gli amministratori locali in ogni gruppo di gestiscono dei dati che appartiene ai propri gruppi. Potenziali proprietari dell'insieme di strutture hanno le autorizzazioni richieste tramite l'infrastruttura di rete per distribuire e supportare Active Directory Domain Services.  
  
Per le organizzazioni che hanno un'infrastruttura centralizzata gruppo IT, il gruppo IT è in genere il proprietario della foresta e, pertanto, il proprietario della foresta potenziali per le distribuzioni future. Le organizzazioni che includono un numero di infrastrutture indipendenti IT gruppi hanno un numero di possibili proprietari dell'insieme di strutture. Se l'organizzazione ha già un'infrastruttura Active Directory posto, tutti i proprietari dell'insieme di strutture corrente sono anche potenziali proprietari dell'insieme di strutture per le nuove distribuzioni.  
  
Selezionare uno dei potenziali proprietari dell'insieme di strutture di agire come proprietario della foresta per ogni foresta che stanno prendendo in considerazione per la distribuzione. I proprietari di foresta potenziali sono responsabili per l'uso con il team di progettazione per stabilire o meno cui relativa foresta sarà effettivamente distribuito o se un corso alternativo dell'azione (ad esempio l'aggiunta di un'altra foresta esistente) è un uso migliore delle risorse disponibili e ancora soddisfa le proprie esigenze. Il proprietario della foresta (o i proprietari) nell'organizzazione sono membri del team di progettazione di Active Directory.  
  
### <a name="establishing-a-design-team"></a>La definizione di un team di progettazione  
Il team di progettazione di Active Directory è responsabile per la raccolta di tutte le informazioni necessarie per prendere decisioni sulla progettazione della struttura logica di Active Directory.  
  
Le responsabilità del team di progettazione seguenti:  
  
-   Per determinare il numero di foreste e domini sono necessari e quali sono le relazioni tra le foreste e domini  
  
-   Lavorare con i proprietari di dati per assicurarsi che la progettazione soddisfi i requisiti di sicurezza e amministrazione  
  
-   Collaborazione con amministratori di rete corrente per garantire che l'infrastruttura di rete corrente supporta la progettazione e che la progettazione non influiranno negativamente le applicazioni esistenti distribuite nella rete  
  
-   Lavorare con i rappresentanti del gruppo di sicurezza dell'organizzazione per garantire che la progettazione soddisfi i criteri di sicurezza stabilito  
  
-   Progettazione di strutture di unità Organizzative che consentono a livelli appropriati di protezione e la delega dell'autorità appropriata per i proprietari dei dati  
  
-   Collabora con il team di distribuzione per la progettazione di test in un laboratorio di ambiente per assicurarsi che funzioni come previsto e modificare la progettazione come necessari per risolvere eventuali problemi che si verificano  
  
-   Creazione di una progettazione di topologia del sito che soddisfa i requisiti di replica dell'insieme di strutture, impedendo l'overload di larghezza di banda disponibile. Per altre informazioni sulla progettazione della topologia dei siti, vedere [progettazione sito topologia per Windows Server 2008 AD DS](https://technet.microsoft.com/library/cc772013.aspx).  
  
-   Collabora con il team di distribuzione per garantire che la progettazione sia implementata correttamente  
  
Il team di progettazione include i membri seguenti:  
  
-   Potenziali proprietari dell'insieme di strutture  
  
-   Architetto di progetto  
  
-   Gestione del progetto  
  
-   Utenti che sono responsabili della definizione e la gestione dei criteri di sicurezza di rete  
  
Durante il processo di progettazione logica struttura, il team di progettazione identifica gli altri proprietari. Queste persone devono iniziare che fanno parte del processo di progettazione, non appena vengono identificati. Dopo che il progetto di distribuzione viene passato al team di distribuzione, il team di progettazione è responsabile della supervisione il processo di distribuzione per verificare che la progettazione sia implementata correttamente. Il team di progettazione inoltre apporta le modifiche alla progettazione basata su commenti e suggerimenti dai test.  
  
### <a name="establishing-a-deployment-team"></a>La definizione di un team di distribuzione  
Il team di distribuzione di Active Directory è responsabile per il test e implementazione della progettazione della struttura logica di Active Directory. Ciò include le attività seguenti:  
  
-   Definizione di un ambiente di test che sufficientemente emula l'ambiente di produzione  
  
-   La verifica della progettazione implementando la struttura proposta di foreste e domini in un ambiente lab per verificare che soddisfi gli obiettivi di ogni proprietario del ruolo  
  
-   Sviluppare e testare tutti gli scenari di migrazione suggerito per impostazione predefinita in un ambiente lab  
  
-   Assicurarsi che ogni proprietario conclude il processo di test per garantire che vengano testate le caratteristiche di progettazione corretta  
  
-   Test dell'operazione di distribuzione in un ambiente pilota  
  
Dopo aver completata la progettazione e attività di test, il team di distribuzione esegue le attività seguenti:  
  
-   Creazione di insiemi di strutture e domini in base alla progettazione della struttura logica di Active Directory  
  
-   Crea i siti e gli oggetti collegamento di sito come necessario in base alla progettazione di topologia del sito  
  
-   Assicura che l'infrastruttura DNS è configurato per supportare Active Directory Domain Services e che eventuali nuovi spazi dei nomi sono integrate nella spazio dei nomi esistente dell'organizzazione  
  
Il team di distribuzione di Active Directory include i membri seguenti:  
  
-   Proprietario della foresta  
  
-   DNS per il proprietario di Active Directory Domain Services  
  
-   Proprietario della topologia del sito  
  
-   Proprietari di unità Organizzativa  
  
Il team di distribuzione funziona con gli amministratori del servizio e i dati durante la fase di distribuzione per garantire che i membri del team di operazioni abbiano familiari con la nuova progettazione. Ciò contribuisce a garantire una transizione senza problemi di proprietà quando viene completata l'operazione di distribuzione. Al termine del processo di distribuzione, la responsabilità di mantenere il nuovo ambiente Active Directory passa dal team di gestione.  
  
### <a name="documenting-the-design-and-deployment-teams"></a>Documentare i team di progettazione e distribuzione  
Documentare i nomi e le informazioni di contatto per le persone che farà parte della progettazione e distribuzione di Active Directory Domain Services. Identificare chi sarà responsabile per ogni ruolo nel team di progettazione e distribuzione. Inizialmente, questo elenco include i possibili proprietari di foresta, il project manager e l'architetto di progetto. Quando si determinano il numero di foreste che si desidera distribuire, si potrebbe essere necessario creare nuovi team di progettazione per le foreste aggiuntive. Si noti che è necessario aggiornare la documentazione quando cambiano le iscrizioni al team e non appena si identificano i proprietari di Active Directory diversi durante il processo di progettazione. Per un foglio di lavoro agevolare così a documentare i team di progettazione e distribuzione per ogni foresta scaricare Job_Aids_Designing_and_Deploying_Directory_and_Security_Services.zip dal processo di supporto per Windows Server 2003 Deployment Kit ([ https://go.microsoft.com/fwlink/?LinkID=102558 ](https://go.microsoft.com/fwlink/?LinkID=102558)) e aprire "Progettazione e distribuzione del Team informazioni" (DSSLOGI_1.doc).  
  


