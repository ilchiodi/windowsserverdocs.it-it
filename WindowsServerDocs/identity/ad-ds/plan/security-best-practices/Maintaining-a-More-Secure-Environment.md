---
ms.assetid: 8f994e2e-6c07-43f0-aef4-75f8b2c9a144
title: Mantenimento di un ambiente più protetto
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: b058fb084b0c46010ba03a11a45e840aa902c7b5
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71367678"
---
# <a name="maintaining-a-more-secure-environment"></a>Mantenimento di un ambiente più protetto

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

*Law numero dieci: La tecnologia non è un toccasana.* - [10 leggi immutabili di amministrazione della sicurezza](https://technet.microsoft.com/library/cc722488.aspx)  
  
Quando è stato creato un ambiente gestibile e sicuro per gli asset aziendali critici, l'attenzione dovrebbe essere spostata in modo da garantire che venga mantenuta in modo sicuro. Sebbene siano stati forniti controlli tecnici specifici per aumentare la protezione delle installazioni di servizi di dominio Active Directory, la tecnologia da sola non proteggerà un ambiente in cui non collabora con l'azienda per mantenere un'infrastruttura sicura e utilizzabile. Le indicazioni generali contenute in questa sezione sono destinate a essere usate come linee guida che è possibile usare per sviluppare non solo una sicurezza efficace, ma una gestione efficace del ciclo di vita.  
  
In alcuni casi, è possibile che l'organizzazione IT abbia già una relazione operativa con le business unit, semplificando l'implementazione di questi consigli. Nelle organizzazioni in cui l'IT e le business unit non sono strettamente collegati, potrebbe essere necessario ottenere prima la sponsorizzazione esecutiva per poter creare una relazione più stretta tra IT e business unit. Il [Riepilogo generale](../../../ad-ds/manage/component-updates/Executive-Summary.md) è concepito per essere utile come documento autonomo per la revisione esecutiva e può essere divulgato ai decision maker della propria organizzazione.  
  
## <a name="creating-business-centric-security-practices-for-active-directory"></a>Creazione di procedure di sicurezza incentrate sull'azienda per Active Directory  
In passato, Information Technology in molte organizzazioni veniva considerata come una struttura di supporto e un centro di costo. I reparti IT sono stati spesso ampiamente isolati dagli utenti aziendali e le interazioni sono limitate a un modello di richiesta-risposta in cui l'azienda ha richiesto risorse e ha risposto.  
  
Man mano che la tecnologia si è evoluta e proliferata, la visione di "un computer su ogni desktop" ha effettivamente superato la maggior parte del mondo ed è stata anche eclissata dall'ampia gamma di tecnologie facilmente accessibili attualmente disponibili. Information Technology non è più una funzione di supporto. si tratta di una funzione aziendale principale. Se l'organizzazione non è stata in grado di continuare a funzionare se tutti i servizi IT non erano disponibili, l'azienda è, almeno in parte, Information Technology.  
  
Per creare piani di ripristino efficaci per i compromessi, i servizi IT devono collaborare a stretto contatto con le Business Unit dell'organizzazione per identificare non solo i componenti più importanti del panorama IT, ma le funzioni critiche richieste dall'azienda. Grazie all'identificazione di cosa è importante per l'organizzazione nel suo complesso, è possibile concentrarsi sulla protezione dei componenti che hanno il maggior valore. Questo non è un Consiglio per sottrarsi alla sicurezza dei sistemi e dei dati a basso valore. Piuttosto, come si definiscono i livelli di servizio per il tempo di attività del sistema, è consigliabile definire i livelli di controllo di sicurezza e il monitoraggio in base alla criticità degli asset.  
  
Una volta investito nella creazione di un ambiente di gestione corrente e sicuro, è possibile spostare lo stato attivo per gestirlo in modo efficace e garantire che siano presenti processi di gestione del ciclo di vita effettivi che non sono determinati solo dal settore IT, ma dall'azienda. A tale scopo, è necessario non solo collaborare con l'azienda, ma per investire l'azienda nella "proprietà" dei dati e dei sistemi in Active Directory.  
  
Quando i dati e i sistemi vengono introdotti in Active Directory senza proprietari designati, proprietari di aziende e proprietari IT, non esiste una chiara catena di responsabilità per il provisioning, la gestione, il monitoraggio, l'aggiornamento e infine la rimozione delle autorizzazioni del sistema. In questo modo, le infrastrutture in cui i sistemi espongono l'organizzazione rischiano, ma non possono essere rimosse, perché la proprietà non è chiara. Per gestire efficacemente il ciclo di vita di utenti, dati, applicazioni e sistemi gestiti dall'installazione di Active Directory, è necessario attenersi ai principi descritti in questa sezione.  
  
### <a name="assign-a-business-owner-to-active-directory-data"></a>Assegnare un proprietario aziendale a Active Directory dati  
I dati in Active Directory devono avere un proprietario aziendale identificato, ovvero un reparto o un utente specifico che rappresenta il punto di contatto per le decisioni relative al ciclo di vita dell'asset. In alcuni casi, il proprietario dell'azienda di un componente di Active Directory sarà un reparto IT o un utente. I componenti dell'infrastruttura, ad esempio controller di dominio, server DHCP e DNS, e Active Directory saranno probabilmente "di proprietà". Per i dati aggiunti a servizi di dominio Active Directory per il supporto dell'azienda, ad esempio nuovi dipendenti, nuove applicazioni e nuovi repository di informazioni, è necessario associare ai dati un'unità aziendale o un utente designato.  
  
Se si usa Active Directory per registrare la proprietà dei dati nella directory o se si implementa un database separato per tenere traccia degli asset IT, non è necessario creare un account utente, non è necessario installare un server o una workstation e non è necessario distribuire alcuna applicazione senza un proprietario designato di record. Il tentativo di stabilire la proprietà dei sistemi dopo che sono stati distribuiti nell'ambiente di produzione può risultare difficoltoso nel migliore dei casi e non è possibile. Di conseguenza, la proprietà deve essere stabilita nel momento in cui i dati vengono introdotti in Active Directory.  
  
### <a name="implement-business-driven-lifecycle-management"></a>Implementare la gestione del ciclo di vita basata sull'azienda  
La gestione del ciclo di vita deve essere implementata per tutti i dati in Active Directory. Ad esempio, quando una nuova applicazione viene introdotta in un dominio Active Directory, il proprietario dell'applicazione deve, a intervalli regolari, prevedere di attestare l'uso continuo dell'applicazione. Quando viene rilasciata una nuova versione di un'applicazione, il proprietario aziendale dell'applicazione deve essere informato e deve decidere se e quando verrà implementata la nuova versione.  
  
Se il proprietario di un business sceglie di non approvare la distribuzione di una nuova versione di un'applicazione, il proprietario deve ricevere una notifica anche della data in cui la versione corrente non sarà più supportata e deve essere responsabile di determinare se l'applicazione verrà rimuovere le autorizzazioni o sostituirle. Non è possibile mantenere le applicazioni legacy in esecuzione e non supportate.  
  
Quando gli account utente vengono creati in Active Directory, i responsabili del record devono ricevere una notifica al momento della creazione dell'oggetto e devono attestare la validità dell'account a intervalli regolari. Implementando un ciclo di vita guidato dall'azienda e l'attestazione regolare della validità dei dati, le persone che sono più adatte per identificare le anomalie nei dati sono le persone che esaminano i dati.  
  
Ad esempio, gli utenti malintenzionati potrebbero creare account utente che sembrano essere account validi, in seguito alle convenzioni di denominazione dell'organizzazione e al posizionamento degli oggetti. Per rilevare queste creazioni di account, è possibile implementare un'attività giornaliera che restituisce tutti gli oggetti utente senza un proprietario aziendale designato, in modo che sia possibile esaminare gli account. Se gli utenti malintenzionati creano account e assegnano un proprietario aziendale, implementando un'attività che segnala la creazione di nuovi oggetti al proprietario aziendale designato, il proprietario dell'azienda può identificare rapidamente se l'account è legittimo.  
  
È necessario implementare approcci simili ai gruppi di sicurezza e di distribuzione. Anche se alcuni gruppi possono essere gruppi funzionali creati dall'IT, creando ogni gruppo con un proprietario designato, è possibile recuperare tutti i gruppi di proprietà di un utente designato e richiedere all'utente di attestare la validità delle proprie appartenenze. Analogamente all'approccio adottato con la creazione dell'account utente, è possibile attivare le modifiche ai gruppi di report per il proprietario aziendale designato. Maggiore è la routine per un proprietario aziendale che attesta la validità o l'invalidità dei dati in Active Directory, più è adatto per identificare le anomalie che possono indicare errori di processo o compromessi effettivi.  
  
### <a name="classify-all-active-directory-data"></a>Classificare tutti i dati di Active Directory  
Oltre a registrare un proprietario aziendale per tutti i dati Active Directory nel momento in cui viene aggiunto alla directory, è necessario richiedere anche ai proprietari dell'azienda di fornire la classificazione per i dati. Se, ad esempio, in un'applicazione vengono archiviati dati aziendali critici, il proprietario dell'azienda deve etichettare l'applicazione in base all'infrastruttura di classificazione dell'organizzazione.  
  
Alcune organizzazioni implementano criteri di classificazione dei dati che etichettano i dati in base al danno causato dall'esposizione dei dati in caso di furto o esposizione. Altre organizzazioni implementano la classificazione dei dati che consente di etichettare i dati in base alla criticità, ai requisiti di accesso e alla conservazione. Indipendentemente dal modello di classificazione dei dati in uso nell'organizzazione, è necessario assicurarsi di poter applicare la classificazione ai dati Active Directory, non solo ai dati "file". Se un account utente è un account VIP, deve essere identificato nel database di classificazione asset (indipendentemente dal fatto che venga implementato tramite l'utilizzo di attributi sugli oggetti in servizi di dominio Active Directory o se si distribuiscono database di classificazione asset distinti).  
  
All'interno del modello di classificazione dei dati, è necessario includere la classificazione per i dati di servizi di dominio Active Directory, come il seguente.  
  
### <a name="systems"></a>-  
Non solo è consigliabile classificare i dati, ma anche i relativi popolamenti del server. Per ogni server è necessario conoscere il sistema operativo installato, i ruoli generali forniti dal server, le applicazioni in esecuzione nel server, il proprietario del record e il proprietario dell'azienda del record, ove applicabile. Per tutti i dati o le applicazioni in esecuzione nel server, è necessario disporre della classificazione e il server deve essere protetto in base ai requisiti per i carichi di lavoro supportati e alle classificazioni applicate al sistema e ai dati. È anche possibile raggruppare i server in base alla classificazione dei propri carichi di lavoro, consentendo di identificare rapidamente i server che devono essere monitorati più accuratamente e configurati in modo più rigoroso.  
  
### <a name="applications"></a>Applicazioni  
È consigliabile classificare le applicazioni in base alle funzionalità (le operazioni che eseguono), alla base utente (che usa le applicazioni) e al sistema operativo in cui vengono eseguite. È necessario mantenere i record che contengono informazioni sulla versione, lo stato delle patch e altre informazioni pertinenti. È inoltre consigliabile classificare le applicazioni in base ai tipi di dati gestiti, come descritto in precedenza.  
  
### <a name="users"></a>Utenti  
Se si chiamano gli utenti "VIP", gli account critici o si usa un'etichetta diversa, gli account nelle installazioni Active Directory che più probabilmente verranno assegnati agli utenti malintenzionati devono essere contrassegnati e monitorati. Nella maggior parte delle organizzazioni, non è possibile eseguire il monitoraggio di tutte le attività di tutti gli utenti. Tuttavia, se si è in grado di identificare gli account critici nell'installazione di Active Directory, è possibile monitorare tali account per le modifiche, come descritto in precedenza in questo documento.  
  
È anche possibile iniziare a creare un database di "comportamenti previsti" per questi account quando si controllano gli account. Se, ad esempio, un determinato dirigente USA la propria workstation protetta per accedere ai dati cruciali per l'azienda dal proprio ufficio e dalla propria abitazione, ma raramente da altre località, se vengono visualizzati tentativi di accesso ai dati tramite il suo account da un computer non autorizzato o un posizione a metà del pianeta in cui si sa che il dirigente non è attualmente disponibile, è possibile identificare e analizzare in modo più rapido questo comportamento anomalo.  
  
Integrando le informazioni aziendali con l'infrastruttura di, è possibile utilizzare le informazioni aziendali per identificare i falsi positivi. Se, ad esempio, il viaggio esecutivo viene registrato in un calendario accessibile al personale IT responsabile del monitoraggio dell'ambiente, è possibile correlare i tentativi di connessione con le posizioni note dei dirigenti.  
  
Supponiamo che Executive A si trovi in genere a Chicago e usi una workstation protetta per accedere ai dati cruciali per l'azienda dalla propria scrivania e che un evento venga attivato da un tentativo non riuscito di accedere ai dati da una workstation non protetta situata in Atlanta. Se si è in grado di verificare che il dirigente si trovi attualmente in Atlanta, è possibile risolvere l'evento contattando il dirigente o l'assistente del dirigente per determinare se l'errore di accesso è stato causato dalla dimenticanza da parte del dirigente di usare la workstation protetta per accedere ai dati. Grazie alla costruzione di un programma che usa gli approcci descritti in [pianificazione della compromissione](../../../ad-ds/plan/security-best-practices/Planning-for-Compromise.md), è possibile iniziare a creare un database dei comportamenti previsti per gli account più importanti nell'installazione di Active Directory che possano potenzialmente aiutare a migliorare individuare e rispondere rapidamente agli attacchi.  
  


