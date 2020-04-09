---
ms.assetid: 85ca191c-0cc7-4453-a72c-42060ddf2ea2
title: Schema riepilogativo
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/08/2018
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: 75b98e3f8078f33098512c8ecd01d3bb49a1c8ed
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80823044"
---
# <a name="executive-summary"></a>Schema riepilogativo

>Si applica a: Windows Server 2012

>[!IMPORTANT] 
>La documentazione seguente è stata scritta in 2013 e viene fornita solo a scopo cronologico.  Attualmente si sta esaminando questa documentazione ed è soggetta a modifiche.  Potrebbe non riflettere le procedure consigliate correnti.

Nessuna organizzazione con un'infrastruttura IT (Information Technology) è immune agli attacchi, ma se sono implementati criteri, processi e controlli appropriati per proteggere i segmenti chiave dell'infrastruttura di elaborazione di un'organizzazione, potrebbe essere possibile impedire a un evento di violazione di crescere fino a una compromissione indesiderata dell'ambiente informatico.  
  
Questo riepilogo può essere utile come documento autonomo in cui viene riepilogato il contenuto del documento, che contiene raccomandazioni che aiuteranno le organizzazioni a migliorare la sicurezza delle installazioni Active Directory. Implementando questi consigli, le organizzazioni saranno in grado di identificare e classificare in ordine di priorità le attività di sicurezza, proteggere i segmenti chiave dell'infrastruttura di elaborazione dell'organizzazione e creare controlli che diminuiscono significativamente la probabilità di attacchi riusciti contro i componenti critici dell'ambiente IT.  
  
Sebbene questo documento discuta gli attacchi più comuni contro Active Directory e contromisure per ridurre la superficie di attacco, contiene anche consigli per il ripristino in caso di compromissione completa. L'unico modo sicuro per eseguire il ripristino in caso di una compromissione completa di Active Directory è prepararsi per la compromissione prima che si verifichi.  
  
Le sezioni principali di questo documento sono:  
  
-   Esposizione a possibili violazioni  
  
-   Riduzione della superficie di attacco di Active Directory  
  
-   Monitoraggio dei segnali di compromissione di Active Directory  
  
-   Pianificazione del compromesso  
  
## <a name="avenues-to-compromise"></a>Esposizione a possibili violazioni  
Questa sezione fornisce informazioni su alcune delle vulnerabilità sfruttate più di frequente usate dagli utenti malintenzionati per compromettere le infrastrutture dei clienti. Contiene categorie generali di vulnerabilità e il modo in cui vengono usate per penetrare inizialmente le infrastrutture dei clienti, propagare i compromessi tra sistemi aggiuntivi e infine indirizzare Active Directory e controller di dominio per ottenere il controllo completo delle foreste delle organizzazioni. Non fornisce indicazioni dettagliate sull'indirizzamento di ogni tipo di vulnerabilità, in particolare nelle aree in cui le vulnerabilità non vengono usate direttamente come destinazione Active Directory. Tuttavia, per ogni tipo di vulnerabilità sono disponibili collegamenti a informazioni aggiuntive da usare per sviluppare contromisure e ridurre la superficie di attacco dell'organizzazione.  
  
Sono inclusi gli argomenti seguenti:  
  
-   **Destinazioni di violazione iniziali** : la maggior parte delle violazioni alla sicurezza delle informazioni inizia con la compromissione di piccoli componenti dell'infrastruttura di un'organizzazione, spesso uno o due sistemi alla volta. Questi eventi iniziali, o punti di ingresso nella rete, sfruttano spesso vulnerabilità che potrebbero essere state corrette, ma non lo sono. Le vulnerabilità più diffuse sono:  
  
    -   Gap nelle distribuzioni antivirus e antimalware  
  
    -   Applicazione di patch incompleta  
  
    -   Applicazioni e sistemi operativi obsoleti  
  
    -   Configurazione  
  
    -   Mancanza di procedure di sviluppo di applicazioni sicure  
  
-   **Account attraenti per il furto di credenziali** : gli attacchi con furto di credenziali sono quelli in cui un utente malintenzionato ottiene inizialmente l'accesso con privilegi a un computer in una rete e quindi usa gli strumenti disponibili gratuitamente per estrarre le credenziali dalle sessioni di altri account connessi.   
    In questa sezione sono inclusi i seguenti elementi:  
  
    -   Le **attività che aumentano la probabilità di compromissione** , perché la destinazione del furto di credenziali sono in genere account di dominio con privilegi elevati e account "molto importanti" (VIP), è importante che gli amministratori siano consapevoli delle attività che aumentano la probabilità di successo di un attacco con furto di credenziali. Queste attività sono:  
  
        -   Accesso a computer non protetti con account con privilegi  
  
        -   Esplorazione di Internet con un account con privilegi elevati  
  
        -   Configurazione di account con privilegi locali con le stesse credenziali tra i sistemi  
  
        -   Sovrapopolazione e utilizzo eccessivo dei gruppi di dominio con privilegi  
  
        -   Gestione insufficiente della sicurezza dei controller di dominio.  
  
    -   Gli account, i server e i componenti dell'infrastruttura specifici **per l'elevazione dei privilegi e la propagazione** sono in genere gli obiettivi principali degli attacchi contro Active Directory. Questi account sono:  
  
        -   Account con privilegi permanenti  
  
        -   Account VIP  
  
        -   Account Active Directory "con privilegi collegati"  
  
        -   Controller di dominio  
  
        -   Altri servizi di infrastruttura che influiscono sulle identità, l'accesso e la gestione della configurazione, ad esempio server di infrastruttura a chiave pubblica (PKI) e server di gestione dei sistemi  
  
## <a name="reducing-the-active-directory-attack-surface"></a>Riduzione della superficie di attacco di Active Directory  
Questa sezione è incentrata sui controlli tecnici che consentono di ridurre la superficie di attacco di un'installazione di Active Directory. In questa sezione sono inclusi gli argomenti seguenti:  
  
-   Gli account **e i gruppi con privilegi nella sezione Active Directory** illustrano i gruppi e gli account con privilegi più elevati in Active Directory e i meccanismi con cui vengono protetti gli account con privilegi. All'interno Active Directory tre gruppi predefiniti sono i gruppi di privilegi più elevati nella directory (Enterprise Admins, Domain Admins e Administrators), sebbene sia necessario proteggere anche un numero di gruppi e account aggiuntivi.  
  
-   La sezione **implementazione dei modelli amministrativi con privilegi minimi** è incentrata sull'identificazione del rischio di utilizzo di account con privilegi elevati per l'amministrazione quotidiana, oltre a fornire consigli per ridurre tale rischio.  
  
I privilegi eccessivi non sono disponibili solo in Active Directory in ambienti compromessi. Quando un'organizzazione ha sviluppato l'abitudine di concedere più privilegi rispetto a quanto richiesto, si trova in genere nell'infrastruttura:  
  
-   In Active Directory  
  
-   Sui server membri  
  
-   Sulle workstation  
  
-   In applicazioni  
  
-   In repository di dati  
  
-   Nella sezione **implementazione di host amministrativi protetti** vengono descritti gli host amministrativi protetti, ovvero i computer configurati per supportare l'amministrazione di Active Directory e dei sistemi connessi. Questi host sono dedicati alle funzionalità amministrative e non eseguono software, ad esempio applicazioni di posta elettronica, Web browser o software di produttività, ad esempio Microsoft Office.  
  
In questa sezione sono inclusi i seguenti elementi:  
  
-   **Principi per la creazione di host amministrativi protetti** : i principi generali da tenere presenti sono:  
  
    -   Non amministrare mai un sistema attendibile da un host meno attendibile.  
  
    -   Non fare affidamento su un singolo fattore di autenticazione quando si eseguono attività con privilegi.  
  
    -   Non dimenticare la sicurezza fisica durante la progettazione e l'implementazione di host amministrativi protetti.  
  
-   **Protezione dei controller di dominio da attacchi** : se un utente malintenzionato ottiene l'accesso con privilegi a un controller di dominio, tale utente può modificare, danneggiare ed eliminare definitivamente il database Active Directory e, per estensione, tutti i sistemi e gli account gestiti da Active Directory.  
  
In questa sezione sono inclusi gli argomenti seguenti:  
  
-   **Sicurezza fisica per i controller di dominio** : contiene consigli per fornire la sicurezza fisica per i controller di dominio in data center, succursali e posizioni remote.  
  
-   **Sistemi operativi dei controller di dominio** : contiene raccomandazioni per la protezione dei sistemi operativi dei controller di dominio.  
  
-   **Configurazione sicura dei controller di dominio** : gli strumenti e le impostazioni di configurazione native e disponibili possono essere usati per creare linee di base di configurazione della sicurezza per i controller di dominio che possono essere applicati in seguito da oggetti Criteri di gruppo (GPO).  
  
## <a name="monitoring-active-directory-for-signs-of-compromise"></a>Monitoraggio dei segnali di compromissione di Active Directory  
In questa sezione vengono fornite informazioni sulle categorie di controllo legacy e sulle sottocategorie dei criteri di controllo (introdotte in Windows Vista e Windows Server 2008) e sui criteri di controllo avanzati introdotti in Windows Server 2008 R2. Sono inoltre disponibili informazioni sugli eventi e sugli oggetti da monitorare che possono indicare i tentativi di compromissione dell'ambiente e alcuni riferimenti aggiuntivi che possono essere utilizzati per costruire criteri di controllo completi per Active Directory.  
  
In questa sezione sono inclusi gli argomenti seguenti:  
  
-   **Criteri di controllo di Windows** : i registri eventi di sicurezza di Windows includono categorie e sottocategorie che determinano quali eventi di sicurezza vengono rilevati e registrati.  
  
-   **Raccomandazioni sui criteri di controllo** : questa sezione descrive le impostazioni dei criteri di controllo predefiniti di Windows, le impostazioni dei criteri di controllo consigliate da Microsoft e consigli più aggressivi per le organizzazioni da usare per controllare i server e le workstation critiche.  
  
## <a name="planning-for-compromise"></a>Pianificazione del compromesso  
Questa sezione contiene raccomandazioni che consentono alle organizzazioni di prepararsi per un compromesso prima che avvenga, implementare i controlli in grado di rilevare un evento di compromissione prima che si verifichi una violazione completa e fornire linee guida per la risposta e il ripristino per i casi in cui un compromesso completo della directory viene raggiunto dagli utenti malintenzionati. In questa sezione sono inclusi gli argomenti seguenti:  
  
-   **Ripensare all'approccio** : contiene principi e linee guida per creare ambienti sicuri in cui un'organizzazione può inserire le risorse più importanti. Queste linee guida sono le seguenti:  
  
    -   Identificazione dei principi per la separazione e la protezione di asset critici  
  
    -   Definizione di un piano di migrazione limitato e basato sul rischio  
  
    -   Sfruttando le migrazioni "non migrate" laddove necessario  
  
    -   Implementazione della "distruzione creativa"  
  
    -   Isolamento di applicazioni e sistemi legacy  
  
    -   Semplificazione della sicurezza per gli utenti finali  
  
-   Gestione di **un ambiente più sicuro** : contiene raccomandazioni di alto livello da usare come linee guida per lo sviluppo di una sicurezza efficace, ma una gestione efficace del ciclo di vita. In questa sezione sono inclusi gli argomenti seguenti:  
  
    -   La **creazione di procedure di sicurezza incentrate sull'azienda per Active Directory** per gestire efficacemente il ciclo di vita di utenti, dati, applicazioni e sistemi gestiti da Active Directory, attenersi a questi principi.  
  
        -   **Assegnare una proprietà dell'azienda a Active Directory dati** : assegnare alla proprietà i componenti dell'infrastruttura; per i dati aggiunti a Active Directory Domain Services (AD DS) per il supporto dell'azienda, ad esempio nuovi dipendenti, nuove applicazioni e nuovi repository di informazioni, un'unità aziendale o un utente designato deve essere associato ai dati.  
  
        -   **Implementare** la gestione del ciclo di vita basata sull'azienda-la gestione del ciclo di vita deve essere implementata per i dati in Active Directory  
  
        -   **Classificare tutti i Active Directory** i proprietari di dati aziendali devono fornire la classificazione dei dati in Active Directory. All'interno del modello di classificazione dei dati, è necessario includere la classificazione per i dati di Active Directory seguenti:  
  
            -   **Sistemi** : classificare i popolamenti dei server, il sistema operativo, il loro ruolo, le applicazioni in esecuzione su di essi e i proprietari it e aziendali del record.  
  
            -   **Applicazioni** : classificare le applicazioni in base alle funzionalità, alla base utente e al sistema operativo.  
  
            -   **Utenti** : gli account nelle installazioni di Active Directory che più probabilmente verranno assegnati a utenti malintenzionati devono essere contrassegnati e monitorati.  
  
## <a name="summary-of-best-practices-for-securing-active-directory-domain-services"></a>Riepilogo delle procedure consigliate per la protezione di Active Directory Domain Services  
La tabella seguente fornisce un riepilogo delle indicazioni fornite in questo documento per la protezione di un'installazione di servizi di dominio Active Directory. Alcune procedure consigliate sono di natura strategica e richiedono progetti di pianificazione e implementazione completi; altri sono tattici e sono incentrati su componenti specifici di Active Directory e sull'infrastruttura correlata.  
  
Le procedure sono elencate in ordine di priorità approssimativo, ovvero. i numeri più bassi indicano una priorità più alta. Laddove applicabile, le procedure consigliate vengono identificate come preventive o investigative di natura. Tutti questi consigli devono essere testati e modificati accuratamente in base alle esigenze delle caratteristiche e dei requisiti dell'organizzazione.  
  
  
||**Procedura consigliata**|**Tattica o strategica**|**Preventivo o detective**|  
|-|-|-|-|  
|1|Applicazione patch.|Tattico|Prevenzione|  
|2|Applicare patch ai sistemi operativi.|Tattico|Prevenzione|  
|3|Distribuisci e aggiorna tempestivamente il software antivirus e antimalware in tutti i sistemi e monitora i tentativi di rimozione o disabilitazione.|Tattico|Entrambe|  
|4|Monitorare gli oggetti Active Directory sensibili per i tentativi di modifica e Windows per gli eventi che potrebbero indicare un tentativo di compromissione.|Tattico|Rilevamento|  
|5|Proteggi e monitora gli account per gli utenti che hanno accesso ai dati sensibili|Tattico|Entrambe|  
|6|Impedire l'utilizzo di account avanzati nei sistemi non autorizzati.|Tattico|Prevenzione|  
|7|Eliminare l'appartenenza permanente a gruppi con privilegi elevati.|Tattico|Prevenzione|  
|8|Implementare i controlli per concedere l'appartenenza temporanea a gruppi con privilegi quando necessario.|Tattico|Prevenzione|  
|9|Implementare host amministrativi protetti.|Tattico|Prevenzione|  
|10|Utilizzare l'elenco elementi consentiti dell'applicazione nei controller di dominio, negli host amministrativi e in altri sistemi sensibili.|Tattico|Prevenzione|  
|11|Identificare le risorse critiche e classificare in ordine di priorità la sicurezza e il monitoraggio.|Tattico|Entrambe|  
|12|Implementare i controlli di accesso con privilegi minimi e basati sui ruoli per l'amministrazione della directory, l'infrastruttura di supporto e i sistemi aggiunti a un dominio.|Strategico|Prevenzione|  
|13|Isolare i sistemi e le applicazioni legacy.|Tattico|Prevenzione|  
|14|Rimuovere le autorizzazioni di sistemi e applicazioni legacy.|Strategico|Prevenzione|  
|15|Implementare programmi del ciclo di vita dello sviluppo sicuro per applicazioni personalizzate.|Strategico|Prevenzione|  
|16|Implementare la gestione della configurazione, verificare regolarmente la conformità e valutare le impostazioni con ogni nuova versione hardware o software.|Strategico|Prevenzione|  
|17|Esegui la migrazione di asset critici a foreste incontaminate con requisiti di sicurezza e monitoraggio rigorosi.|Strategico|Entrambe|  
|18|Semplifica la sicurezza per gli utenti finali.|Strategico|Prevenzione|  
|19|Usare firewall basati su host per controllare e proteggere le comunicazioni.|Tattico|Prevenzione|  
|20|Dispositivi patch.|Tattico|Prevenzione|  
|21|Implementare la gestione del ciclo di vita incentrata sull'azienda per le risorse IT.|Strategico|N/D|  
|22|Crea o aggiorna i piani di ripristino degli eventi imprevisti.|Strategico|N/D|  
  


