---
ms.assetid: 8f994e2e-6c07-43f0-aef4-75f8b2c9a144
title: Mantenimento di un ambiente più protetto
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 554f841473d79edbac19ce4c53e99f5a43fa2b23
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59821702"
---
# <a name="maintaining-a-more-secure-environment"></a>Mantenimento di un ambiente più protetto

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

*Legge numero 10: Tecnologia non è una panacea.* - [10 leggi immutabili della protezione amministrazione](https://technet.microsoft.com/library/cc722488.aspx)  
  
Quando è stato creato un ambiente sicuro e gestibile per gli asset aziendali critiche, deve spostare lo stato attivo per garantire che venga mantenuto in modo sicuro. Anche se è stato ottenuto specifici controlli tecnici per aumentare la sicurezza delle installazioni Active Directory Domain Services, la sola tecnologia non proteggerà un ambiente in cui si non funziona in collaborazione con all'azienda di gestire un'infrastruttura protetta e utilizzabile. Le indicazioni di alto livello in questa sezione sono concepite per essere utilizzate come linee guida che è possibile usare per lo sviluppo di sicurezza valide non solo, ma gestione efficace del ciclo di vita.  
  
In alcuni casi, l'organizzazione IT dispone già di una stretta collaborazione con business unit, che semplificherà l'implementazione di queste indicazioni. Nelle organizzazioni in cui si e unità aziendali non sono strettamente correlate, potrebbe essere necessario prima ottenere responsabili della sponsorizzazione per sforzi difficilmente potrebbe riprodurre una relazione più stretta tra IT e le business unit. Il [sunto](../../../ad-ds/manage/component-updates/Executive-Summary.md) è destinato a essere utile come un documento autonomo per la revisione del dirigente che può essere distribuito per decision maker all'interno dell'organizzazione.  
  
## <a name="creating-business-centric-security-practices-for-active-directory"></a>Creazione di procedure consigliate di sicurezza spendere per Active Directory  
In passato, in molte organizzazioni IT è stata visualizzata come una struttura di supporto e un centro di costo. I reparti IT spesso in gran parte sono stati separati dagli utenti di business e interazioni limitate a un modello di richiesta-risposta in cui le risorse richieste all'azienda e IT ha risposto.  
  
Come tecnologia ha sviluppato e ripensare, la visione di "un computer a casa" ha effettivamente provengono da passare per la maggior parte del mondo e persino stati tempo eclissata da un'ampia gamma di tecnologie facilmente accessibile attualmente disponibili. Tecnologia informatica è più una funzione di supporto, è una funzione aziendale principale. Se l'organizzazione non è stato possibile continuare a funzionare anche se non sono disponibili tutti i servizi IT, aziendali della tua organizzazione sono, almeno in parte, IT.  
  
Per creare piani di ripristino buon compromesso, services IT deve operano a stretto contatto con business unit dell'organizzazione per identificare non solo i componenti principali del panorama applicativo IT, ma le funzioni critiche necessarie per l'azienda. Identificando che cosa è importante per l'organizzazione nel suo complesso, è possibile concentrarsi sulla protezione dei componenti che il valore. Questo non è una raccomandazione per shirk la sicurezza dei dati e sistemi di valore inferiore. Piuttosto, ad esempio si definiscono i livelli di servizio per il tempo di attività di sistema, è necessario considerare che definisce i livelli di controllo di sicurezza e il monitoraggio basato su criticità di asset.  
  
Quando si è investito nella creazione di un ambiente corrente, sicuro e gestibile, è possibile spostare lo stato attivo a gestirla in modo efficace e di avere installato i processi di gestione del ciclo di vita efficace che non sono solo da determinati IT, ma per l'azienda. A tale scopo, è necessario non solo per le partnership con l'azienda, ma a investire il business in "proprietà" dei dati e i sistemi in Active Directory.  
  
Quando i dati e i sistemi vengono introdotti nell'Active Directory senza proprietari designati, i proprietari aziendali e IT, non si verifica alcuna catena chiaro di responsabilità per la gestione del provisioning, monitoraggio, l'aggiornamento e alla fine rimozione delle autorizzazioni del sistema. Di conseguenza le infrastrutture in cui i sistemi dell'organizzazione a rischio di esporre ma non possono essere messo fuori servizio perché la proprietà non è chiara. Per gestire in modo efficace il ciclo di vita di utenti, dati, applicazioni e i sistemi gestiti tramite l'installazione di Active Directory, è consigliabile seguire i principi descritti in questa sezione.  
  
### <a name="assign-a-business-owner-to-active-directory-data"></a>Assegnare un proprietario di Business ai dati di Active Directory  
I dati in Active Directory devono avere un proprietario identificato business, vale a dire, un reparto specificato o un utente che è il punto di contatto per prendere decisioni sul ciclo di vita dell'asset. In alcuni casi, il titolare dell'organizzazione di un componente di Active Directory sarà un utente o un reparto IT. Componenti dell'infrastruttura, ad esempio i controller di dominio, server DHCP e DNS e Active Directory verranno molto probabilmente sono "proprietà" da IT. Per i dati che viene aggiunto all'Active Directory Domain Services per supportare l'azienda (ad esempio nuovi dipendenti nuove applicazioni e nuovi repository di informazioni), un'azienda designata unità o l'utente deve essere associata ai dati.  
  
Se si usano Active Directory per la proprietà dei record dei dati nella directory, o se si implementa un database separato per tenere traccia delle risorse IT, non deve essere creato alcun account utente, non deve essere installato alcun server o workstation e nessuna applicazione deve essere distribuita senza un proprietario designato del record. Tentativo di stabilire la proprietà dei sistemi dopo la distribuzione nell'ambiente di produzione può essere difficile ottimali e impossibili in alcuni casi. Pertanto, la proprietà deve essere stabilita al momento che i dati sono stato introdotto in Active Directory.  
  
### <a name="implement-business-driven-lifecycle-management"></a>Implementare la gestione del ciclo di vita ispirate alle necessità aziendali  
Gestione del ciclo di vita deve essere implementati per tutti i dati in Active Directory. Ad esempio, quando viene introdotta una nuova applicazione in un dominio di Active Directory, titolare dell'organizzazione dell'applicazione, a intervalli regolari, prevedibile per attestare l'uso continuativo dell'applicazione. Quando viene rilasciata una nuova versione di un'applicazione, titolare dell'organizzazione dell'applicazione, è consigliabile informare e stabilire se e quando verrà implementata la nuova versione.  
  
Se un titolare dell'organizzazione decide di non approvare la distribuzione di una nuova versione di un'applicazione, il titolare dell'organizzazione deve anche una notifica della data quando la versione corrente non è più supportata e deve essere responsabile di determinare se l'applicazione verrà essere ritirati o sostituiti. Mantenere le applicazioni legacy in esecuzione e non supportate non deve essere un'opzione.  
  
Quando vengono creati gli account utente in Active Directory, responsabili di record devono essere una notifica al momento della creazione di oggetti e necessari per attestare la validità dell'account a intervalli regolari. Mediante l'implementazione di un'azienda driven ciclo di vita e l'attestazione regolare della validità dei dati, le persone che hanno a disposizione migliore identificare le anomalie nei dati sono le persone che esaminare i dati.  
  
Ad esempio, gli utenti malintenzionati potrebbero creare account utente che sembra essere account validi, seguendo le convenzioni di denominazione e posizionamento di un oggetto della propria organizzazione. Per rilevare le creazioni di questi account, è possibile implementare un'attività giornaliera che restituisce tutti gli oggetti utente senza un titolare dell'organizzazione designata in modo da poter individuare gli account. Se gli utenti malintenzionati creano gli account e assegnare un proprietario di business, mediante l'implementazione di un'attività che segnala la creazione di nuovi oggetti al titolare dell'azienda designato, il titolare dell'organizzazione possa identificare rapidamente se l'account sia legittima.  
  
È consigliabile implementare gli approcci simili ai gruppi di sicurezza e distribuzione. Anche se alcuni gruppi possono essere gruppi funzionali creati dal reparto IT, creando ogni gruppo con un proprietario designato, è possibile recuperare tutti i gruppi appartenenti a un utente designato e richiedere all'utente di attestare la validità delle proprie appartenenze a. Come per l'approccio adottato con la creazione dell'account utente, è possibile attivare reporting modifiche ai gruppi per il titolare dell'organizzazione designata. La routine più che diventa proprietario di un business attestare la validità o non validità dei dati in Active Directory, l'addetto più che sei per identificare eventuali anomalie che possono indicare compromissione effettiva o gli errori nei processi.  
  
### <a name="classify-all-active-directory-data"></a>Classificare tutti i dati di Active Directory  
Oltre a registrare un titolare dell'organizzazione per tutti i dati di Active Directory al momento che viene aggiunto alla directory, è anche consigliabile richiedere ai proprietari di business fornire la classificazione per i dati. Ad esempio, se un'applicazione archivia i dati aziendali cruciali, il titolare dell'organizzazione deve assegnare un'etichetta di conseguenza, l'applicazione in base alla funzionalità infrastruttura di classificazione dell'organizzazione.  
  
Alcune organizzazioni implementano criteri di classificazione dei dati di tale etichetta ai dati in base al danno che comporta l'esposizione dei dati se sono stati rubato o esposti. Altre organizzazioni implementano la classificazione dei dati che i dati delle etichette dalla criticità, dai requisiti di accesso e dalla conservazione. Indipendentemente dal modello di classificazione dei dati in uso nell'organizzazione, è necessario assicurarsi che si è in grado di applicare la classificazione di dati di Active Directory, non solo ai dati "file". Se un account utente è un account VIP, deve essere identificato nel database di classificazione di asset (se si implementa questo tramite l'utilizzo di attributi per gli oggetti di Active Directory Domain Services, o se si distribuisce il database di classificazione asset separato).  
  
All'interno del modello di classificazione dei dati, è consigliabile includere la classificazione per dati di Active Directory Domain Services, ad esempio il seguente.  
  
### <a name="systems"></a>-  
Si deve non solo classificare i dati, ma anche relativa popolazione di server. Per ogni server, è consigliabile conoscere quale sistema operativo è installato, i ruoli generali forniti dal server, quali applicazioni sono in esecuzione nel server, il proprietario IT di record e i proprietari aziendali dei record, ove applicabile. Per tutti i dati o le applicazioni in esecuzione nel server, è consigliabile richiedere la classificazione e il server deve essere protetti in base ai requisiti per i carichi di lavoro che supporta e le classificazioni applicate al sistema e dati. È anche possibile raggruppare i server per la classificazione di carichi di lavoro, che consente di identificare rapidamente i server che devono essere il più strettamente monitorati e configurati più rigoroso.  
  
### <a name="applications"></a>Applicazioni  
È consigliabile classificare per funzionalità (operazioni), base di utenti (che usa le applicazioni) e il sistema operativo in cui vengono eseguite le applicazioni. È necessario gestire i record che contengono informazioni sulla versione, lo stato delle patch e qualsiasi altra informazione. È anche consigliabile classificare le applicazioni per i tipi di dati che gestiscono, come descritto in precedenza.  
  
### <a name="users"></a>Utenti  
Chiamarli utenti "VIP", gli account critici, o usare un'etichetta diversa, gli account nelle installazioni Active Directory che possono più essere attaccate da utenti malintenzionati devono essere contrassegnati e monitorati. Nella maggior parte delle organizzazioni, semplicemente non è fattibile per monitorare tutte le attività di tutti gli utenti. Tuttavia, se si è in grado di identificare gli account critici nell'installazione di Active Directory, è possibile monitorare gli account per le modifiche come descritto in precedenza in questo documento.  
  
È anche possibile iniziare a compilare un database di "comportamenti previsti" per questi account quando si controllano gli account. Ad esempio, se si ritiene che un determinato dirigente Usa sua workstation protetti per accedere ai dati aziendali critici dall'ufficio e da casa sua, ma raramente da altre posizioni, se si tenta di accedere ai dati usando l'account da un computer non autorizzato o un oggetto percorso a metà di tutto il mondo in cui si conosce l'executive non è attualmente posizionato, è possibile identificare e ricercare questo comportamento anomalo più rapidamente.  
  
Grazie all'integrazione di informazioni aziendali con l'infrastruttura, è possibile usare tali informazioni di business che consentono di identificare i falsi positivi. Se, ad esempio, viaggi viene registrato in un calendario che sia accessibile per il personale IT responsabile del monitoraggio dell'ambiente, è possibile correlare i tentativi di connessione con percorsi noti dei dirigenti.  
  
Si supponga Executive oggetto si trova in genere a Chicago e Usa una workstation sicura per accedere ai dati aziendali critici dalla propria scrivania e un evento viene attivato da un tentativo non riuscito per accedere ai dati da una workstation non protetta che si trova ad Atlanta. Se si è in grado di verificare che il dirigente è attualmente disponibile in Atlanta, è possibile risolvere l'evento contattando il dirigente o Assistente del dirigente per determinare se l'errore di accesso sia il risultato del dirigente se si dimentica di usare le workstation protette per accedere ai dati. Creando un programma che usa gli approcci descritti in [pianificazione del compromesso](../../../ad-ds/plan/security-best-practices/Planning-for-Compromise.md), è possibile iniziare a creare un database di comportamenti previsti per gli account più "important" nell'installazione di Active Directory che può essere potenzialmente consentono più rapidamente di individuare e rispondere agli attacchi.  
  


