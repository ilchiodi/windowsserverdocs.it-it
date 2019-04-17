---
ms.assetid: 8f994e2e-6c07-43f0-aef4-75f8b2c9a144
title: "Mantenimento di un ambiente più sicuro"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 7d471ee2ba73ef7425464e1aa928c75318f7dd82
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/12/2017
---
# <a name="maintaining-a-more-secure-environment"></a>Mantenimento di un ambiente più sicuro

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

*Legge numero 10: tecnologia non rappresenta la soluzione.* - [10 Immutable Laws of Security Administration](https://technet.microsoft.com/library/cc722488.aspx)  
  
Dopo avere creato un ambiente sicuro e gestibile per gli asset aziendali critiche, la messa a fuoco deve MAIUSC per garantire che venga mantenuta in modo sicuro. Anche se sono stati concessi specifici controlli tecnici per aumentare la sicurezza delle installazioni di dominio Active Directory, la sola tecnologia non protegge un ambiente in cui non funziona in collaborazione con l'azienda per mantenere un'infrastruttura protetta e utilizzabile. I consigli di alto livelli in questa sezione sono concepiti per essere utilizzate come linee guida che è possibile utilizzare per sviluppare non solo sicurezza efficace, ma la gestione del ciclo di vita effettivi.  
  
In alcuni casi, l'organizzazione IT potrebbe essere già presente una stretta collaborazione con business unit, che rende più facile implementazione di questi suggerimenti. Nelle organizzazioni in cui si e business unit non sono strettamente collegate, potrebbe essere necessario ottenere prima l'iniziativa per gli sforzi per creare una relazione tra più vicina IT e business unit. Il [schema riepilogativo](../../../ad-ds/manage/component-updates/Executive-Summary.md) è progettato per essere utili come un documento autonomo per esaminare executive e può essere distribuito per decision maker nell'organizzazione.  
  
## <a name="creating-business-centric-security-practices-for-active-directory"></a>Creazione di procedure di protezione basate su Business per Active Directory  
In passato, all'interno di molte organizzazioni IT è stato visualizzato come una struttura di supporto e un centro di costo. I reparti IT sono stati spesso in gran parte separati da utenti aziendali e interazioni limitate a un modello di richiesta-risposta in cui l'azienda ha richiesto le risorse e IT ha risposto.  
  
Come la tecnologia ha sviluppato e ripensare, la visione di "un computer in ogni desktop" ha effettivamente provenire da passare per gran parte del mondo e anche stato sono per la vasta gamma di tecnologie facilmente accessibile oggi disponibili. Tecnologia di informazioni non è più una funzione di supporto, è una funzione aziendale dei componenti di base. Se l'organizzazione non in grado di continuare a funzionare se tutti i servizi IT non sono disponibili, le attività dell'organizzazione sono almeno in parte, IT.  
  
Per creare la compromissione effettiva piani di ripristino, servizi IT necessario collaborare con business unit nell'organizzazione per identificare non solo i componenti più critici del paesaggio IT, ma le funzioni critiche necessarie per l'azienda. Identificando cosa è importante per l'organizzazione nel suo complesso, è possibile concentrarsi sulla protezione di componenti che hanno il massimo. Non si tratta di un suggerimento per la sicurezza dei dati e sistemi a basso valore shirk. Piuttosto, come si definiscono i livelli di servizio per il tempo di attività di sistema, è necessario considerare la definizione di livelli di protezione controllo e monitoraggio in base alle criticità di asset.  
  
Quando hanno investito nella creazione di un ambiente sicuro e gestibile corrente, è possibile spostare lo stato attivo a gestirlo in modo efficace e accertarsi di disporre di processi di gestione del ciclo di vita efficace che non sono solo per determinate IT, ma dall'azienda. A tale scopo, è necessario non solo per collaborare con l'azienda, ma a investire business in "proprietà" dei dati e i sistemi in Active Directory.  
  
Quando i dati e sistemi vengono introdotti in Active Directory senza proprietari designati, i proprietari aziendali e IT, non esiste alcuna deseleziona catena di responsabilità per il provisioning, gestione, il monitoraggio, aggiornamento e alla fine rimozione delle autorizzazioni di sistema. Di conseguenza infrastrutture in cui l'organizzazione a rischio di esporre sistemi ma non possono essere disattivati perché la proprietà non è chiara. Per gestire in modo efficace il ciclo di vita di utenti, dati, applicazioni e sistemi gestiti dall'installazione di Active Directory, è necessario seguire i principi descritti in questa sezione.  
  
### <a name="assign-a-business-owner-to-active-directory-data"></a>Assegnare un proprietario di Business per dati di Active Directory  
Dati in Active Directory devono avere un proprietario di business identificato, vale a dire un reparto specificato o un utente che è il punto di contatto per le decisioni relative al ciclo di vita dell'attività. In alcuni casi, il proprietario di business di un componente di Active Directory sarà un utente o il reparto IT. Componenti dell'infrastruttura come controller di dominio, server DHCP e DNS e Active Directory verranno probabilmente essere "proprietà" IT. Per i dati che viene aggiunto al dominio di Active Directory per supportare l'azienda (ad esempio, i nuovi dipendenti, le nuove applicazioni e nuovi repository di informazioni), un designato business unit o dall'utente deve essere associata con i dati.  
  
Se si utilizza Active Directory per la proprietà dei record dei dati nella directory o se si implementa un database separato per tenere traccia delle risorse IT, non deve essere creato alcun account utente non deve essere installata alcun server o workstation e nessuna applicazione deve essere distribuita senza un proprietario designato del record. Tentativo di stabilire la proprietà di sistemi dopo la distribuzione nell'ambiente di produzione può essere difficile a ottimali e Impossibile in alcuni casi. Di conseguenza, la proprietà stabilire al momento che i dati sono stato introdotto in Active Directory.  
  
### <a name="implement-business-driven-lifecycle-management"></a>Implementare la gestione del ciclo di vita basato su Business  
Gestione del ciclo di vita deve essere implementati per tutti i dati in Active Directory. Ad esempio, quando in un dominio Active Directory viene introdotto una nuova applicazione, i proprietari aziendali dell'applicazione, a intervalli regolari, prevedibile per attestare l'uso continuativo dell'applicazione. Quando viene rilasciata una nuova versione di un'applicazione, i proprietari aziendali dell'applicazione devono essere informati e devono decidere se e quando viene implementata la nuova versione.  
  
Se un proprietario di business sceglie di non approvare la distribuzione di una nuova versione di un'applicazione, i proprietari aziendali che devono anche essere notificati la data in cui la versione corrente non è più supportata e deve essere responsabile di determinare se l'applicazione verrà rimosse le autorizzazioni o sostituito. Mantenere le applicazioni legacy in esecuzione e non supportati non deve essere un'opzione.  
  
Quando vengono creati gli account utente in Active Directory, i responsabili dei record devono essere una notifica al momento della creazione di oggetti e necessari per attestare la validità dell'account a intervalli regolari. Mediante l'implementazione di un'azienda driven ciclo di vita e regolare attestazione della validità dei dati, le persone che hanno a disposizione migliore identificare eventuali anomalie nei dati sono le persone che esaminare i dati.  
  
Ad esempio, gli utenti malintenzionati potrebbero creare gli account utente che sembrano essere account validi, seguendo le convenzioni di denominazione dell'organizzazione e posizionamento di un oggetto. Per rilevare questi creazioni di account, è possibile implementare un'attività quotidiana che restituisce tutti gli oggetti utente senza un proprietario designato aziendali in modo che è possibile esaminare gli account. Se gli utenti malintenzionati creano gli account e assegnare un proprietario di business, mediante l'implementazione di un'attività che segnala la creazione di nuovi oggetti per il proprietario designato business, il titolare dell'azienda può identificare rapidamente se l'account sia legittimo.  
  
È necessario implementare metodi simili ai gruppi di sicurezza e la distribuzione. Anche se alcuni gruppi possono essere funzionali gruppi creati dall'IT, creando ogni gruppo con un proprietario designato, è possibile recuperare tutti i gruppi appartenenti a un utente designato e richiedere all'utente di attestare la validità di appartenenza. Simile a quella eseguita con la creazione di account utente, è possibile generare report modifiche di gruppo per il proprietario designato business. La routine di ulteriori che diventa un proprietario di business attestare alla validità o all'invalidità dei dati in Active Directory, il più dotati che sei per individuare eventuali anomalie in grado di indicare compromissione effettiva o errori di processo.  
  
### <a name="classify-all-active-directory-data"></a>Classificare tutti i dati Active Directory  
Oltre al momento che viene aggiunto alla directory di registrazione un proprietario di business per tutti i dati di Active Directory, è consigliabile richiedere anche i proprietari di business fornire la classificazione dei dati. Ad esempio, se un'applicazione archivia i dati aziendali cruciali, il titolare dell'azienda l'etichetta di conseguenza, l'applicazione in conformità con l'infrastruttura di classificazione dell'organizzazione.  
  
Alcune organizzazioni implementano criteri di classificazione dei dati che dati etichetta in base al danno che dovrebbe affrontare esposizione dei dati, se sono stato rubato o esposti. Altre organizzazioni implementano classificazione dei dati che i dati etichette da criticità, requisiti di accesso e periodo di mantenimento dati. Indipendentemente dal modello di classificazione di dati in uso nell'organizzazione, è necessario assicurarsi che è possibile applicare la classificazione di dati di Active Directory, non solo a dati "file". Se un account utente è un account VIP, devono essere identificato nel database classificazione asset (se si implementa questo tramite l'utilizzo degli attributi per gli oggetti in Active Directory o se vengono distribuiti i database di classificazione asset separati).  
  
All'interno del modello di classificazione dei dati, è necessario includere una classificazione per i dati di dominio Active Directory, ad esempio le operazioni seguenti.  
  
### <a name="systems"></a>Sistemi  
È necessario non solo classificare i dati, ma anche le relative popolazioni di server. Per ogni server, è consigliabile conoscere è installato il sistema operativo, quali ruoli generali fornisce il server, quali applicazioni vengono eseguiti nel server, il proprietario IT del record e i proprietari aziendali dei record, se applicabile. Per tutti i dati o le applicazioni in esecuzione sul server, è consigliabile richiedere la classificazione e il server deve essere protetto in base ai requisiti per i carichi di lavoro che supporta e le classificazioni applicate al sistema e i dati. È inoltre possibile raggruppare i server per la classificazione di carichi di lavoro, che consente di identificare rapidamente i server che devono corrispondere al meglio monitorato e più rigorosamente configurata.  
  
### <a name="applications"></a>Applicazioni  
È necessario classificare per funzionalità (le operazioni eseguite) base di utenti (che utilizza le applicazioni) e il sistema operativo in cui vengono eseguite le applicazioni. È necessario mantenere i record che contengono informazioni sulla versione, lo stato delle patch e altre informazioni. È anche necessario classificare le applicazioni per i tipi di dati che gestiscono, come descritto in precedenza.  
  
### <a name="users"></a>Utenti  
Se si chiamarli utenti "VIP", gli account critici, o Usa un'etichetta diversa, gli account in installazioni di Active Directory che più probabilmente essere indirizzati da utenti malintenzionati devono essere contrassegnati e monitorati. Nella maggior parte delle organizzazioni, non è semplicemente fattibile per monitorare tutte le attività di tutti gli utenti. Tuttavia, se è possibile identificare gli account critici nell'installazione di Active Directory, è possibile monitorare questi account per le modifiche come descritto in precedenza in questo documento.  
  
È anche possibile iniziare a creare un database di "comportamenti previsti" per questi account quando si controlla l'account. Ad esempio, se si rileva che un determinato executive Usa le workstation protette per accedere ai dati aziendali critici dal suo ufficio e da sua, ma raramente da altri percorsi, se si tenta di accedere ai dati usando il suo account da un computer non autorizzato o un percorso a metà intorno al mondo in cui si conosce il executive non attualmente, puoi più rapidamente identificare e ricercare questo comportamento anomalo.  
  
Integrando le informazioni aziendali con l'infrastruttura, è possibile utilizzare le informazioni aziendali in modo da identificare falsi positivi. Ad esempio, se viaggi sono registrato in un calendario che sia accessibile per il personale IT responsabile per il monitoraggio dell'ambiente, è possibile correlare tentativi di connessione con percorsi noti dei dirigenti.  
  
Si supponga che Executive A si trova in genere a Chicago e utilizza una workstation protetta per accedere ai dati aziendali critici dalla propria scrivania e un evento viene attivato da un tentativo non riuscito per accedere ai dati da una workstation protetta che si trova nella Atlanta. Se è possibile verificare che il executive è attualmente Atlanta, è possibile risolvere l'evento contattando il executive o Assistente della persona per determinare se l'errore di accesso è il risultato dell'esecutivo dimenticare di utilizzare la workstation protetta per accedere ai dati. Creando un programma che utilizza gli approcci descritti in [pianificazione del compromesso](../../../ad-ds/plan/security-best-practices/Planning-for-Compromise.md), è possibile iniziare a creare un database di tipi di comportamento previsti per l'account più "importante" nell'installazione di Active Directory che potenzialmente consentono più rapidamente di individuare e rispondere agli attacchi.  
  


