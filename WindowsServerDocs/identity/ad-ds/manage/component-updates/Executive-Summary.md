---
ms.assetid: 85ca191c-0cc7-4453-a72c-42060ddf2ea2
title: Schema riepilogativo
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: c218a4d66e8208cf627bc93be50bf11ea2fbf862
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2017
---
# <a name="executive-summary"></a>Schema riepilogativo

>Si applica a: Windows Server 2012

>[!IMPORTANT] 
>La seguente documentazione è stata scritta in 2013 e verrà fornita esclusivamente a scopo cronologici.  Attualmente questa documentazione esame ed è soggetto a modifiche.  Non può riflettere corrente e le procedure consigliate.

Nessuna organizzazione con un'infrastruttura IT (IT) è immune agli attacchi, ma se implementati controlli, processi e criteri appropriati per proteggere i segmenti essenziali dell'infrastruttura di elaborazione di un'organizzazione, potrebbe essere possibile impedire a un evento di violazione di crescita alla compromissione commercio all'ingrosso dell'ambiente di elaborazione.  
  
Questo riepilogo è progettato per essere utili come documento autonomo riepilogano il contenuto del documento, che contiene alcuni suggerimenti che verranno aiutare le organizzazioni di migliorare la protezione delle installazioni Active Directory. Mediante l'implementazione di questi suggerimenti, le organizzazioni sarà in grado di identificare e definire le priorità le attività di protezione, proteggere i segmenti essenziali dell'infrastruttura informatica dell'organizzazione e creare controlli che riducono notevolmente il rischio di attacchi contro i componenti critici dell'ambiente IT.  
  
Anche se gli attacchi contro Active Directory e contromisure per ridurre la superficie di attacco più comuni, descritte in questo documento contiene anche indicazioni per il ripristino in caso di compromissione completa. L'unico modo sicuro per ripristinare in caso di compromissione completa di Active Directory è necessario preparare per la compromissione prima che venga eseguito.  
  
Le principali sezioni di questo documento sono:  
  
-   Esposizione a possibili violazioni  
  
-   Ridurre la superficie di attacco di Active Directory  
  
-   Monitoraggio dei segnali di compromissione di Active Directory  
  
-   Pianificazione del compromesso  
  
## <a name="avenues-to-compromise"></a>Esposizione a possibili violazioni  
In questa sezione fornisce informazioni su alcune delle vulnerabilità più sfruttate usate da utenti malintenzionati per compromettere le infrastrutture dei clienti. Contiene le categorie generali di vulnerabilità e modalità di utilizzo per penetrare inizialmente infrastrutture dei clienti, propagare i compromessi tra altri sistemi e alla fine di destinazione Active Directory e controller di dominio per ottenere il controllo completo delle foreste dell'azienda. Non fornisce indicazioni dettagliate su ogni tipo di vulnerabilità, in particolare nelle aree in cui le vulnerabilità non vengono utilizzate per Active Directory di destinazione direttamente gli indirizzi. Tuttavia, per ogni tipo di vulnerabilità, Microsoft fornisce collegamenti a informazioni aggiuntive da utilizzare per sviluppare contromisure e ridurre la superficie di attacco dell'organizzazione.  
  
Sono inclusi gli argomenti seguenti:  
  
-   **Destinazioni di violazione iniziale** -la maggior parte delle violazioni della sicurezza di informazioni iniziare con la compromissione di piccole parti di un'organizzazione infrastruttura spesso uno o due sistemi alla volta. Questi eventi iniziali o punti di ingresso alla rete, spesso sfruttano vulnerabilità che avrebbero potuto essere riparate, ma non venivano. Vulnerabilità comune sono:  
  
    -   Spazi vuoti nelle distribuzioni antivirus e antimalware.  
  
    -   L'applicazione di patch incompleto  
  
    -   Sistemi operativi e applicazioni non aggiornate  
  
    -   Errore di configurazione  
  
    -   Mancanza di consigliate per lo sviluppo applicazioni protette  
  
-   **Account interessanti per il furto di credenziali** -attacchi contro il furto di credenziali sono quelle in cui un utente malintenzionato ottiene inizialmente l'accesso con privilegiato a un computer in una rete e quindi Usa gli strumenti disponibili gratuitamente per estrarre le credenziali dalle sessioni di altri account connesso.   
    In questa sezione sono inclusi le operazioni seguenti:  
  
    -   **Le attività che aumenta la probabilità di compromissione**, perché la destinazione di furto di credenziali è in genere gli account di dominio con privilegi elevati e "persona molto importante" account (VIP), è importante per gli amministratori di tenere conto delle attività che aumentano la probabilità di successo di un attacco di furto di credenziali. Queste attività sono:  
  
        -   L'accesso a un computer non protetti con gli account con privilegi  
  
        -   Esplorazione di Internet con un account con privilegiato elevati  
  
        -   Configurazione dell'account con privilegi locali con le stesse credenziali tra sistemi  
  
        -   Overpopulation e utilizzo eccessivo di gruppi di dominio con privilegi  
  
        -   Gestione insufficiente di sicurezza dei controller di dominio.  
  
    -   **Privilegio l'elevazione dei privilegi e propagazione** -specifici account, server e componenti dell'infrastruttura sono in genere le destinazioni di Active Directory attacchi di primario. Questi account sono:  
  
        -   Account con privilegi in modo permanente  
  
        -   Account VIP  
  
        -   "Collegato privilegio" account di Active Directory  
  
        -   Controller di dominio  
  
        -   Altri servizi di infrastruttura che influiscono sulla gestione delle identità, accesso e configurazione, ad esempio server di infrastruttura a chiave pubblica (PKI) e il server di gestione di sistemi  
  
## <a name="reducing-the-active-directory-attack-surface"></a>Ridurre la superficie di attacco di Active Directory  
Questa sezione vengono illustrati i controlli tecnici per ridurre la superficie di attacco di un'installazione di Active Directory. In questa sezione sono inclusi gli argomenti seguenti:  
  
-   Il **account con privilegi e gruppi in Active Directory** sezione illustra gli account con privilegi più elevato e i gruppi in Active Directory e i meccanismi per cui gli account con privilegi sono protetti. All'interno di Active Directory, i tre gruppi predefiniti sono i gruppi con privilegi di livello più alti nella directory (Enterprise Admins, Domain Admins e gli amministratori), anche se un numero di altri gruppi e account deve inoltre essere protetto.  
  
-   Il **implementazione di modelli amministrativi con privilegi minimi** ha come obiettivo sezione l'identificazione del rischio che l'utilizzo di privilegi elevati account per i regali amministrazione quotidiana, inoltre a fornire suggerimenti per ridurre tale rischio.  
  
Privilegi eccessivi non solo trovare in Active Directory negli ambienti di compromessi. Quando un'organizzazione ha sviluppato l'abitudine di concessione di più privilegi è necessaria, in genere risulta in tutta l'infrastruttura:  
  
-   In Active Directory  
  
-   Sui server membri  
  
-   Sulle workstation  
  
-   Nelle applicazioni  
  
-   In archivi dati  
  
-   Il **implementazione host amministrativi protetti** sezione viene descritto host amministrativi protetti, che sono computer che sono configurati per supportare l'amministrazione di Active Directory e sistemi connessi. Questi host sono dedicati alle funzionalità di amministrazione e non eseguono il software, ad esempio software per la produttività, web browser o applicazioni di posta elettronica (ad esempio Microsoft Office).  
  
In questa sezione sono inclusi le operazioni seguenti:  
  
-   **Principi per la creazione di host amministrativi Secure** -principi generali da tenere presenti sono:  
  
    -   Non amministrare mai un sistema attendibile da un host meno attendibile.  
  
    -   Non si basano su un singolo fattore di autenticazione durante l'esecuzione di attività con privilegi.  
  
    -   Non dimenticare sicurezza fisica per la progettazione e implementazione host amministrativi protetti.  
  
-   **Protezione dei controller di dominio dagli attacchi** -se un utente malintenzionato ottiene accesso con privilegi per un controller di dominio, tale utente può modificare danneggiato ed eliminare il database di Active Directory e, di conseguenza, tutti i sistemi e gli account che sono gestiti da Active Directory.  
  
In questa sezione sono inclusi gli argomenti seguenti:  
  
-   **Sicurezza fisica per i controller di dominio** -contiene alcuni suggerimenti per fornire la sicurezza fisica per i controller di dominio in centri dati, le succursali e sedi remote.  
  
-   **Sistemi operativi dei Controller di dominio** -contiene alcuni suggerimenti per la protezione di sistemi operativi dei controller di dominio.  
  
-   **Configurazione del controller di dominio protetti** -impostazioni e strumenti nativi e disponibile gratuitamente configurazione possono essere utilizzate per creare configurazioni di base per i controller di dominio che possono essere applicati in seguito dagli oggetti Criteri di gruppo (GPO) di sicurezza.  
  
## <a name="monitoring-active-directory-for-signs-of-compromise"></a>Monitoraggio dei segnali di compromissione di Active Directory  
In questa sezione fornisce informazioni sul controllo legacy categorie e sottocategorie di criteri di controllo (che sono stati introdotti in Windows Vista e Windows Server 2008) e criteri di controllo avanzati (che è stata introdotta in Windows Server 2008 R2). Inoltre fornite informazioni sugli eventi e gli oggetti da monitorare che possono indicare tentativi di compromettere l'ambiente e alcuni riferimenti aggiuntivi che possono essere utilizzati per creare un criterio di controllo completo per Active Directory.  
  
In questa sezione sono inclusi gli argomenti seguenti:  
  
-   **Criteri di controllo Windows** - nei registri eventi di sicurezza di Windows sono le categorie e sottocategorie che determinano gli eventi di protezione sono registrate e registrate.  
  
-   **Suggerimenti per i criteri di controllo** -in questa sezione vengono descritte le impostazioni di criteri di controllo predefinito di Windows, controlla le impostazioni di criteri che sono consigliate da Microsoft e consigli più aggressive per le organizzazioni di utilizzare per controllare le workstation e server di importanza strategica.  
  
## <a name="planning-for-compromise"></a>Pianificazione del compromesso  
Questa sezione contiene indicazioni che consentono alle organizzazioni di preparare per la compromissione prima che venga eseguito, implementare i controlli che possono rilevare un evento di compromissione prima di una violazione completo e fornire linee guida per la risposta e ripristino per i casi in cui una compromissione completa della directory risultato viene ottenuta da utenti malintenzionati. In questa sezione sono inclusi gli argomenti seguenti:  
  
-   **L'approccio riflessioni** -contiene i principi e linee guida per creare ambienti sicuri in cui un'organizzazione può collocare le proprie risorse più critiche. Queste linee guida sono i seguenti:  
  
    -   Identificazione dei principi per separare e proteggere gli asset critici  
  
    -   Definizione di un piano di migrazione limitato, basati sui rischi  
  
    -   Sfruttando le migrazioni "nonmigratory" in caso di necessità  
  
    -   Implementazione "distruzione creativa"  
  
    -   Isolamento delle applicazioni e sistemi legacy  
  
    -   La semplificazione della protezione per gli utenti finali  
  
-   **Gestione di un ambiente più sicuro** -contiene consigli generali concepite per essere utilizzate come linee guida da utilizzare per lo sviluppo non solo sicurezza efficace, ma la gestione del ciclo di vita effettivi. In questa sezione sono inclusi gli argomenti seguenti:  
  
    -   **Creazione di procedure di protezione basate su Business per Active Directory** : se si desidera gestire il ciclo di vita degli utenti, dati, applicazioni e dei sistemi gestiti da Active Directory in modo efficace, seguire questi principi.  
  
        -   **Assegnare un proprietario di Business per dati di Active Directory** -assegna la proprietà dei componenti di infrastruttura IT; per i dati che viene aggiunto a servizi Active Directory dominio (AD DS) per supportare le attività aziendali, ad esempio, i nuovi dipendenti, le nuove applicazioni e nuovi repository di informazioni, un reparto designato o un utente deve essere associata con i dati.  
  
        -   **Implementare la gestione del ciclo di vita Business-Driven** -gestione del ciclo di vita deve essere implementati per i dati in Active Directory.  
  
        -   **Classificare i dati di Active Directory tutti** -imprenditori devono fornire classificazione per i dati in Active Directory. All'interno del modello di classificazione dei dati, deve essere inclusa classificazione per i dati di Active Directory seguenti:  
  
            -   **Sistemi** -classificare popolazioni di server, il sistema operativo proprio ruolo, le applicazioni in esecuzione e IT e i proprietari aziendali dei record.  
  
            -   **Applicazioni** -classificare le applicazioni da funzionalità di base di utenti e il sistema operativo.  
  
            -   **Gli utenti** -gli account nelle installazioni di Active Directory che più probabilmente essere indirizzati da utenti malintenzionati devono essere contrassegnati e monitorati.  
  
## <a name="summary-of-best-practices-for-securing-active-directory-domain-services"></a>Riepilogo delle procedure consigliate per la protezione di servizi di dominio Active Directory  
Nella tabella seguente fornisce un riepilogo dei consigli forniti in questo documento per la protezione di un'installazione di Active Directory. Le procedure consigliate sono di natura strategiche e richiedono pianificazione completa e progetti di implementazione. altri sono di tipo tattico e sono specifici per determinati componenti di Active Directory e infrastruttura correlata.  
  
Procedure consigliate sono elencate all'incirca l'ordine di priorità, tale com'è., i numeri più bassi indicano priorità più alta. Dove applicabile, suggerimenti sono identificate come preventative o detective natura. Tutti questi consigli forniti devono essere accuratamente testati e modificati in base alle esigenze per le caratteristiche e requisiti dell'organizzazione.  
  
  
|-|-|-|-|  
||**Consigliata**|**tattico o strategico**|**Preventative o Detective**|  
| 1 | Applicazioni di patch. | Tattico | Preventative |  
| 2 | Sistemi operativi di patch. | Tattico | Preventative |  
| 3 | Distribuire e aggiornare tempestivamente software antivirus e antimalware in tutti i sistemi e monitoraggio dei tentativi per rimuovere o disattivarlo. | Tattico | Entrambi |  
| 4 | Monitorare gli oggetti di Active Directory riservati per i tentativi di modifica e Windows per gli eventi che potrebbero indicare compromissione tentata. | Tattico | Rilevamento |  
| 5 | Proteggere e monitorare gli account per gli utenti che hanno accesso a dati sensibili | Tattico | Entrambi |  
| 6 | Impedire l'account potenti in sistemi non autorizzati. | Tattico | Preventative |  
| 7 | Eliminare l'appartenenza permanente a gruppi con privilegi elevati. | Tattico | Preventative |  
| 8 | Implementare controlli per concedere l'appartenenza a gruppi con privilegi quando necessario. | Tattico | Preventative |  
| 9 | Implementazione host amministrativi sicuri. | Tattico | Preventative |  
& 10 | Utilizzare whitelist applicazione controller di dominio, host amministrativi e altri sistemi sensibili. | Tattico | Preventative |  
& 11 | Asset critici, identificare e definire le priorità la sicurezza e monitoraggio. | Tattico | Entrambi |  
& 12 | Implementare i controlli di accesso con privilegi minimi, in base al ruolo per l'amministrazione dei sistemi appartenenti a un dominio, la directory e dell'infrastruttura di supporto. | Strategico | Preventative |  
| 13 | Isolare le applicazioni e sistemi legacy. | Tattico | Preventative |  
& 14 | Rimuovere le autorizzazioni di applicazioni e sistemi legacy. | Strategico | Preventative |  
& 15 | Implementare i programmi del ciclo di vita per applicazioni personalizzate di sviluppo sicure. | Strategico | Preventative |  
& 16 | Implementare la gestione della configurazione, verificare la conformità regolarmente e valutare le impostazioni con ogni nuova versione del software o hardware. | Strategico | Preventative |  
& 17 | Eseguire la migrazione di asset critici a insiemi di strutture invariati con i requisiti di monitoraggio e della sicurezza restrittivi. | Strategico | Entrambi |  
& 18 | Semplificare la protezione per gli utenti finali. | Strategico | Preventative |  
& 19 | Utilizzare per controllare e proteggere le comunicazioni per i firewall basato su host. | Tattico | Preventative |  
| 20 | I dispositivi di patch. | Tattico | Preventative |  
| 21 | Implementare la gestione del ciclo di vita basate su business per le risorse IT. | Strategico | N/D |  
& 22 & Creare o aggiornare i piani di ripristino agli eventi imprevisti. | Strategico | N/D |  
  


