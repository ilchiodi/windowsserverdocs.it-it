---
ms.assetid: 85ca191c-0cc7-4453-a72c-42060ddf2ea2
title: Schema riepilogativo
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/08/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: a3eecec9d47f91bb6a9ba549abc3bf62482b2f49
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59863282"
---
# <a name="executive-summary"></a>Schema riepilogativo

>Si applica a: Windows Server 2012

>[!IMPORTANT] 
>La documentazione seguente è stato scritto nel 2013 e viene fornita solo a fini cronologici.  Attualmente è esame della presente documentazione ed è soggetta a modifiche.  Potrebbe non riflettere corrente e le procedure consigliate.

Nessuna organizzazione con un'infrastruttura degli information technology (IT) è immune agli attacchi, ma se i controlli, processi e i criteri appropriati vengono implementati per proteggere i segmenti essenziali dell'infrastruttura informatica dell'organizzazione, potrebbe essere possibile impedire l'aumento delle dimensioni per una vendita all'ingrosso compromissione dell'ambiente di elaborazione di un evento di violazione.  
  
Questo sunto è destinato a essere utile come un documento autonomo che contenga il contenuto del documento, che contiene le indicazioni di supporto delle organizzazioni nel migliorare la sicurezza delle installazioni Active Directory. Implementando questi suggerimenti, le organizzazioni saranno in grado di identificare e classificare le attività di sicurezza, proteggere i segmenti essenziali dell'infrastruttura informatica della propria organizzazione e creare i controlli che diminuire in modo significativo la probabilità di esito positivo attacchi contro i componenti critici dell'ambiente IT.  
  
Sebbene questo documento illustra gli attacchi più comuni Active Directory e contromisure per ridurre la superficie di attacco, include anche le raccomandazioni per il ripristino in caso di compromissione completa. L'unico modo sicuro per il ripristino in caso di compromissione completa di Active Directory deve essere preparato per la compromissione prima che venga eseguito.  
  
Le sezioni principali di questo documento sono:  
  
-   Esposizione a possibili violazioni  
  
-   Riduzione della superficie di attacco di Active Directory  
  
-   Monitoraggio dei segnali di compromissione di Active Directory  
  
-   Pianificazione del compromesso  
  
## <a name="avenues-to-compromise"></a>Esposizione a possibili violazioni  
In questa sezione fornisce informazioni su alcune delle vulnerabilità sfruttate più comunemente usata dagli utenti malintenzionati per compromettere le infrastrutture dei clienti. Include le categorie generali di vulnerabilità e modo in cui sono abituati a penetrare inizialmente infrastrutture dei clienti, propagare i compromessi tra altri sistemi e infine destinata Active Directory e controller di dominio per ottenere completo controllo delle foreste di organizzazioni. Non fornisce indicazioni dettagliate sulla risoluzione di ogni tipo di vulnerabilità, in particolare nelle aree in cui le vulnerabilità non vengono utilizzate per Active Directory hanno come destinazione diretta. Per ogni tipo di vulnerabilità, tuttavia, Microsoft fornisce collegamenti a informazioni aggiuntive da usare per sviluppare contromisure e ridurre la superficie di attacco dell'organizzazione.  
  
Sono inclusi gli argomenti seguenti:  
  
-   **Violazione destinazioni iniziali** -la maggior parte delle violazioni della sicurezza informazioni iniziano con la compromissione di piccole parti di un'organizzazione infrastruttura spesso uno o due sistemi alla volta. Questi eventi iniziali o punti di ingresso alla rete, spesso sfruttano le vulnerabilità che è stato possibile sono stati corretti, ma non sono stati. Le vulnerabilità comuni sono:  
  
    -   Gap nelle distribuzioni di antivirus e antimalware  
  
    -   L'applicazione di patch incompleto  
  
    -   Sistemi operativi e applicazioni obsolete  
  
    -   Errore di configurazione  
  
    -   Mancanza di procedure consigliate per lo sviluppo di applicazioni sicure  
  
-   **Account interessanti per il furto di credenziali** -attacchi con furto di credenziali sono quelle in cui un utente malintenzionato ottiene inizialmente l'accesso con privilegi a un computer in una rete e quindi Usa gli strumenti disponibili gratuitamente per estrarre le credenziali dalle sessioni di altri account connessi.   
    Inclusi in questa sezione sono i seguenti:  
  
    -   **Le attività che aumentano la probabilità di compromissione** - perché la destinazione di furto di credenziali è in genere gli account di dominio con privilegi elevati e "molto importante person" account (VIP), è importante per gli amministratori di essere consapevole di attività che aumentano la probabilità di successo di un attacco di furto di credenziali. Queste attività sono:  
  
        -   L'accesso a computer non protetto con gli account con privilegi  
  
        -   Esplorazione di Internet con un account con privilegiato elevati  
  
        -   Configurazione di account con privilegi locali con le stesse credenziali tra sistemi  
  
        -   Overpopulation e uso eccessivo di gruppi di dominio con privilegi  
  
        -   Insufficiente gestione della sicurezza del controller di dominio.  
  
    -   **Con privilegi più elevati e propagazione** -account specifici, server e i componenti dell'infrastruttura sono in genere le destinazioni principali di attacchi contro Active Directory. Questi account sono:  
  
        -   Account con privilegi in modo permanente  
  
        -   Account VIP  
  
        -   Account di Active Directory "Associati con privilegi"  
  
        -   Controller di dominio  
  
        -   Altri servizi di infrastruttura che influiscono sulla gestione di identità, l'accesso e configurazione, ad esempio i server di infrastruttura a chiave pubblica (PKI) e il server di gestione di sistemi  
  
## <a name="reducing-the-active-directory-attack-surface"></a>Riduzione della superficie di attacco di Active Directory  
Questa sezione è incentrata sui controlli tecnici per ridurre la superficie di attacco di un'installazione di Active Directory. In questa sezione sono inclusi gli argomenti seguenti:  
  
-   Il **gli account con privilegi e gruppi in Active Directory** sezione vengono illustrati gli account con privilegi più elevato e i gruppi in Active Directory e i meccanismi per cui sono protetti gli account con privilegi. All'interno di Active Directory, tre gruppi predefiniti sono i gruppi con privilegi più elevati nella directory (Enterprise Admins, Domain Admins e amministratori), anche se un numero di altri gruppi e gli account deve anche essere protetti.  
  
-   Il **implementazione di modelli amministrativi con privilegi minimi** sezione è incentrata sui che identifica il rischio che presenta l'uso di account con privilegi elevati per attività di ordinaria amministrazione, oltre a fornire suggerimenti a ridurre tale rischio.  
  
Privilegi eccessivi non solo trovare in Active Directory negli ambienti di compromessi. Quando un'organizzazione ha sviluppato l'abitudine di concessione di più privilegi rispetto a quelli necessari, in genere risulta in tutta l'infrastruttura:  
  
-   In Active Directory  
  
-   Sui server membri  
  
-   Sulle workstation  
  
-   Nelle applicazioni  
  
-   Nel repository dei dati  
  
-   Il **implementazione host amministrativi protetti** sezione descrive host amministrativi protetti, costituiti da computer configurati per supportare l'amministrazione di Active Directory e i sistemi connessi. Gli host dedicati alla funzionalità amministrative e non vengono eseguite applicazioni software come applicazioni di posta elettronica, browser web o software di produttività (ad esempio Microsoft Office).  
  
Inclusi in questa sezione sono i seguenti:  
  
-   **Principi per la creazione di host amministrativi Secure** -sono i principi generali da tenere presenti:  
  
    -   Non amministrare mai un sistema attendibile da un host considerato meno attendibile.  
  
    -   Non fare affidamento su un singolo fattore di autenticazione durante l'esecuzione di attività con privilegi.  
  
    -   Non dimenticare la protezione fisica durante la progettazione e implementazione host amministrativi protetti.  
  
-   **Protezione dei controller di dominio contro attacco** -se un utente malintenzionato ottiene accesso con privilegi a un controller di dominio, tale utente può modificare, danneggiati ed elimina il database di Active Directory e di conseguenza, tutti i sistemi e gli account gestito da Active Directory.  
  
In questa sezione sono inclusi gli argomenti seguenti:  
  
-   **Sicurezza fisica per i controller di dominio** -contiene le indicazioni per fornire la sicurezza fisica per i controller di dominio nel Data Center, succursali e sedi remote.  
  
-   **Sistemi operativi dei Controller di dominio** -contiene le indicazioni per la protezione di sistemi operativi dei controller di dominio.  
  
-   **Configurazione di controller di dominio protetti** -nativo e disponibile gratuitamente gli strumenti e configurazione delle impostazioni consente di creare linee di base di configurazione per i controller di dominio che successivamente possono essere applicati dalla (oggetti Criteri di gruppo di sicurezza Oggetti Criteri di gruppo).  
  
## <a name="monitoring-active-directory-for-signs-of-compromise"></a>Monitoraggio dei segnali di compromissione di Active Directory  
Questa sezione vengono fornite informazioni sulle categorie di controllo legacy e le sottocategorie dei criteri di controllo (che sono stati introdotti in Windows Vista e Windows Server 2008) e criteri di controllo avanzati (che è stata introdotta in Windows Server 2008 R2). Anche fornito informazioni sugli eventi e gli oggetti per il monitoraggio che possono indicare tentativi di violazione dell'ambiente e alcuni riferimenti aggiuntivi che possono essere utilizzati per costruire un criterio di controllo completo per Active Directory.  
  
In questa sezione sono inclusi gli argomenti seguenti:  
  
-   **Criteri di controllo di Windows** : i registri eventi di sicurezza di Windows dispongono delle categorie e sottocategorie che determinano gli eventi di sicurezza vengono monitorate e registrate.  
  
-   **Suggerimenti per i criteri di controllo** -in questa sezione descrive le impostazioni di Criteri controllo predefinito di Windows, controllare le impostazioni dei criteri consigliati da Microsoft e consigli più aggressive per le organizzazioni a per un controllo server critici e workstation.  
  
## <a name="planning-for-compromise"></a>Pianificazione del compromesso  
Questa sezione vengono fornite indicazioni che possono favorire le organizzazioni di prepararsi a una compromissione prima che venga eseguito, implementare i controlli che è possono rilevare un evento di compromissione prima che si è verificata una violazione completa e fornire linee guida per la risposta e il ripristino per i casi in cui gli utenti malintenzionati viene ottenuta una compromissione completa della directory. In questa sezione sono inclusi gli argomenti seguenti:  
  
-   **L'approccio rethinking** -contiene i principi e le linee guida per creare ambienti protetti, in cui un'organizzazione può inserire le proprie risorse più critiche. Queste linee guida sono i seguenti:  
  
    -   Che identifica i principi per adeguatamente e protezione di asset critici  
  
    -   Definizione di un piano di migrazione limitata, in base al rischio  
  
    -   Sfruttando le migrazioni "nonmigratory" dove necessario  
  
    -   Implementazione di "eliminazione creative"  
  
    -   Isolamento delle applicazioni e sistemi legacy  
  
    -   Semplificazione della protezione per gli utenti finali  
  
-   **Manutenzione di un ambiente più sicuro** -contiene le indicazioni di alto livello deve essere usata come linee guida da utilizzare nello sviluppo di sicurezza valide non solo, ma gestione efficace del ciclo di vita. In questa sezione sono inclusi gli argomenti seguenti:  
  
    -   **Creazione di spendere procedure di sicurezza per Active Directory** : per gestire il ciclo di vita di utenti, dati, applicazioni e i sistemi gestiti da Active Directory in modo efficace, seguire questi principi.  
  
        -   **Assegnare una proprietà di Business a dati di Active Directory** -assegnare la proprietà dei componenti dell'infrastruttura IT; per i dati che viene aggiunto in Active Directory Domain Services (AD DS) per supportare l'azienda, ad esempio nuovi dipendenti, le nuove applicazioni, e nuovi repository di informazioni, un'unità aziendale designato o l'utente deve essere associata ai dati.  
  
        -   **Implementare la gestione del ciclo di vita Business-Driven** -gestione del ciclo di vita deve essere implementato per i dati in Active Directory.  
  
        -   **Classificare tutti i dati di Active Directory** -impresa deve fornire la classificazione per i dati in Active Directory. All'interno del modello di classificazione dei dati, è necessario inclusa la classificazione per i dati di Active Directory seguenti:  
  
            -   **I sistemi** -classificare popolazioni di server, il sistema operativo proprio ruolo, le applicazioni in esecuzione su di essi e l'IT e i proprietari aziendali dei record.  
  
            -   **Le applicazioni** -classificazione delle applicazioni dalla funzionalità di base di utenti e il sistema operativo.  
  
            -   **Gli utenti** -devono essere contrassegnati e monitorati gli account nelle installazioni di Active Directory che possono più essere attaccate da utenti malintenzionati.  
  
## <a name="summary-of-best-practices-for-securing-active-directory-domain-services"></a>Riepilogo delle procedure consigliate per la protezione di Active Directory Domain Services  
Nella tabella seguente fornisce un riepilogo dei consigli forniti in questo documento per la protezione di un'installazione di Active Directory Domain Services. Alcune procedure consigliate sono di natura strategiche e richiedono pianificazione completa e i progetti di implementazione. altri sono tattica e di focalizzare l'attenzione su componenti specifici di Active Directory e relativa infrastruttura.  
  
Procedure consigliate sono elencate in ordine approssimativo di priorità, ovvero, i numeri più bassi indicano priorità più alta. Dove applicabile sulle procedure consigliate identificate come preventive o detective di natura. Tutte queste indicazioni deve essere accuratamente testate e modificate in base alle necessità per le caratteristiche e requisiti della propria organizzazione.  
  
  
||**Procedure consigliate**|**Tattico o strategico**|**Preventive o Detective**|  
|-|-|-|-|  
|1|Applicazioni di patch.|Tattico|Preventive|  
|2|Sistemi operativi di patch.|Tattico|Preventive|  
|3|Distribuire e aggiornare immediatamente un software antivirus e antimalware in tutti i sistemi e monitoraggio dei tentativi per rimuovere o disabilitarlo.|Tattico|Entrambi|  
|4|Monitoraggio oggetti Active Directory riservati per i tentativi di modifica e Windows per gli eventi che potrebbero indicare compromissione effettuata è fallita.|Tattico|Detective|  
|5|Proteggere e monitorare gli account per utenti che hanno accesso ai dati sensibili|Tattico|Entrambi|  
|6|Impedire l'account di potenti nei sistemi non autorizzati.|Tattico|Preventive|  
|7|Eliminare l'appartenenza permanente a gruppi con privilegi elevati.|Tattico|Preventive|  
|8|Implementare controlli per concedere l'appartenenza a gruppi con privilegi quando necessario.|Tattico|Preventive|  
|9|Implementazione host amministrativi protetti.|Tattico|Preventive|  
|10|Usare nell'elenco elementi consentiti dell'applicazione sul controller di dominio, gli host amministrativi e altri sistemi riservati.|Tattico|Preventive|  
|11|Identificare le risorse critiche e definire la priorità relativa protezione e il monitoraggio.|Tattico|Entrambi|  
|12|Implementare i controlli di accesso basato su ruolo con privilegi minimi per l'amministrazione della directory relativa infrastruttura di supporto e i sistemi appartenenti a un dominio.|Strategico|Preventive|  
|13|Isolare le applicazioni e sistemi legacy.|Tattico|Preventive|  
|14|Rimozione delle autorizzazioni delle applicazioni e sistemi legacy.|Strategico|Preventive|  
|15|Implementare i programmi del ciclo di vita di sviluppo sicuro per le applicazioni personalizzate.|Strategico|Preventive|  
|16|Implementare la gestione della configurazione, esaminare regolarmente la conformità e valutare le impostazioni con ogni nuova versione di hardware o software.|Strategico|Preventive|  
|17|Eseguire la migrazione di asset critici alle foreste originario con rigorosi di sicurezza e monitoraggio dei requisiti.|Strategico|Entrambi|  
|18|Semplifica la sicurezza per gli utenti finali.|Strategico|Preventive|  
|19|Usare firewall basati su host per le comunicazioni sicure e controllo.|Tattico|Preventive|  
|20|Dispositivi di patch.|Tattico|Preventive|  
|21|Implementare la gestione del ciclo di vita spendere per risorse IT.|Strategico|N/D|  
|22|Creare o aggiornare i piani di ripristino degli eventi imprevisti.|Strategico|N/D|  
  


