---
title: Considerazioni sulla LDAP in ottimizzazione delle prestazioni di ADDS
description: Considerazioni su LDAP nei carichi di lavoro di Active Directory
ms.prod: windows-server-threshold
ms.technology: performance-tuning-guide
ms.topic: article
ms.author: TimWi; ChrisRob; HerbertM; KenBrumf;  MLeary; ShawnRab
author: phstee
ms.date: 10/16/2017
ms.openlocfilehash: 7ac9453159fe97dc15ecbb2ab858214664a2a197
ms.sourcegitcommit: 6ef4986391607bb28593852d06cc6645e548a4b3
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 06/07/2019
ms.locfileid: "66811529"
---
# <a name="ldap-considerations-in-adds-performance-tuning"></a>Considerazioni sulla LDAP in ottimizzazione delle prestazioni di ADDS

> [!IMPORTANT]
> Di seguito è riportato un riepilogo dei principali requisiti e considerazioni per ottimizzare l'hardware del server per i carichi di lavoro di Active Directory trattati in modo più approfondito nel [pianificazione della capacità per Active Directory Domain Services](https://go.microsoft.com/fwlink/?LinkId=324566) articolo. I lettori sono altamente consigliabile rivedere [pianificazione della capacità per Active Directory Domain Services](https://go.microsoft.com/fwlink/?LinkId=324566) per una maggiore conoscenza tecnica e le implicazioni di queste indicazioni.

## <a name="verify-ldap-queries"></a>Verificare le query LDAP

Verificare che le query LDAP siano conformi alle raccomandazioni creazione delle query efficienti.

È la documentazione completa su come scrivere, struttura, in modo corretto e analizzare le query per l'utilizzo con Active Directory in MSDN. Per altre informazioni, vedi [Creating More Efficient Microsoft Active Directory-Enabled applicazioni](https://msdn.microsoft.com/library/ms808539.aspx).

## <a name="optimize-ldap-page-sizes"></a>Ottimizzare le dimensioni di pagina LDAP

La restituzione di risultati con più oggetti in risposta alle richieste dei client, il controller di dominio deve archiviare temporaneamente il set di risultati in memoria. Aumento delle dimensioni di pagina causano maggiore utilizzo di memoria e possono scadere inutilmente gli elementi dalla cache. In questo caso, le impostazioni predefinite sono ottimale. Esistono diversi scenari in cui le raccomandazioni sono state apportate per aumentare le impostazioni delle dimensioni di pagina. È consigliabile usare i valori predefiniti, a meno che specificamente identificato come non adeguate.

Quando le query hanno un numero di risultati, un limite di query simile eseguita simultaneamente può essere rilevato.  Ciò si verifica quando il server LDAP può portare un'area di memoria globale nota come il pool di cookie.  Potrebbe essere necessario aumentare le dimensioni del pool come descritto nella [come LDAP Server vengono gestiti i cookie](https://technet.microsoft.com/windows-server-docs/identity/ad-ds/manage/how-ldap-server-cookies-are-handled).

Per l'ottimizzazione di queste impostazioni, vedere [controller di dominio 2008 e versioni successive di Windows Server restituisce solo 5000 valori in una risposta LDAP](https://support.microsoft.com/kb/2009267).

## <a name="determine-whether-to-add-indices"></a>Determinare se si desidera aggiungere indici

L'indicizzazione degli attributi è utile quando si cercano oggetti con il nome dell'attributo in un filtro. L'indicizzazione può ridurre il numero di oggetti che devono essere visitati durante la valutazione del filtro. Tuttavia, ciò riduce le prestazioni delle operazioni di scrittura perché l'indice deve essere aggiornato quando l'attributo corrispondente viene modificata o aggiunta. Aumenta anche la dimensione del database di directory, anche se i vantaggi superano spesso il costo dell'archiviazione. La registrazione è utilizzabile per individuare le query costosa e inefficiente. Una volta identificati, prendere in considerazione l'indicizzazione di alcuni attributi che vengono utilizzati nelle query corrispondente per migliorare le prestazioni della ricerca. Per altre informazioni sul funzionamento delle ricerche di Active Directory, vedere [Active Directory ricerche funzionamento](https://technet.microsoft.com/library/cc755809.aspx).

### <a name="scenarios-that-benefit-in-adding-indices"></a>Scenari che traggono vantaggio nell'aggiunta di indici

-   Caricamento di client nella richiesta di dati genera significativa dell'utilizzo della CPU e il comportamento di query client non può essere modificato o con ottimizzazione per la. Per un carico significativo, prendere in considerazione che viene visualizzato se stesso in un elenco reati primi 10 Server Performance Advisor o incorporata Active Directory insieme di raccolta dati e Usa più di 1% della CPU.

-   Il carico di lavoro è la generazione dei / o disco significativo in un server a causa di un attributo non indicizzato e il comportamento di query client non può essere modificato o ottimizzato.

-   Una query sta impiegando molto tempo e non è stata completata in un intervallo di tempo accettabile per il client a causa di mancanza di indici di copertura.

- Grandi volumi di query con tempi di esecuzione lunghi causano il consumo e all'esaurimento dei thread ATQ LDAP. Monitorare i contatori delle prestazioni seguenti:

    - **NTDS\\latenza richiesta** – si tratta di soggetti a quanto tempo impiegato per elaborare la richiesta. Active Directory verifica il timeout delle richieste dopo 120 secondi (impostazione predefinita), tuttavia, la maggior parte deve essere eseguito più velocemente e le query con esecuzione estremamente prolungata deve ottenere nascosto nei numeri di complessivo. Cercare le modifiche apportate in questa linea di base, anziché le soglie assolute.

        > [!NOTE]
        > Valori elevati qui possono essere anche gli indicatori di ritardi nelle richieste di "inoltro" ad altri domini e i controlli di CRL.

    - **NTDS\\stimato ritardo coda** – ciò dovrebbe essere idealmente vicini a 0 per ottenere prestazioni ottimali perché ciò significa che le richieste di impiegare alcun tempo di attesa di essere gestite.

Questi scenari possono essere rilevati tramite uno o più degli approcci seguenti:

-   [Determinazione della durata delle Query con il controllo delle statistiche](https://msdn.microsoft.com/library/ms808539.aspx)

-   [Rilevamento delle ricerche costose e inefficienti](https://msdn.microsoft.com/library/ms808539.aspx)

-   Agente di raccolta dati di Active Directory diagnostica impostato in Performance Monitor ([Son di SPA: Agente di raccolta dati di Active Directory imposta in Win2008 e oltre](http://blogs.technet.com/b/askds/archive/2010/06/08/son-of-spa-ad-data-collector-sets-in-win2008-and-beyond.aspx))

-   [Microsoft Server Performance Advisor](../../../server-performance-advisor/microsoft-server-performance-advisor.md) Pack Advisor per Active Directory

-   Esegue la ricerca usando qualsiasi filtro oltre a "(objectClass =\*)" che utilizzano l'indice di predecessori.

### <a name="other-index-considerations"></a>Altre considerazioni sull'indice

-   Assicurarsi che la creazione dell'indice è la soluzione adatta al problema dopo l'ottimizzazione delle query è stata esaurita come opzione. Ridimensionamento dell'hardware in modo corretto è molto importante. Gli indici devono essere aggiunti solo quando la correzione corretta per l'attributo di indice e non un tentativo di nascondere problemi hardware.

-   Gli indici di aumentare le dimensioni del database da un minimo delle dimensioni totali dell'attributo in corso l'indicizzazione. Una stima della crescita del database può pertanto essere valutata considerando le dimensioni medie dei dati dell'attributo e moltiplicando il numero di oggetti che avrà il valore di attributo. Si tratta in genere un aumento di 1% della dimensione del database. Per altre informazioni, vedi [come funziona il Data Store](https://technet.microsoft.com/library/cc772829.aspx).

-   Se il comportamento di ricerca viene eseguito prevalentemente a livello di unità di organizzazione, prendere in considerazione l'indicizzazione per le ricerche nei contenitori.

-   Gli indici di tupla sono maggiori degli indici normali, ma è molto più difficile da stimare le dimensioni. Usare gli indici normali dimensioni stimano come il tetto minimo per tale crescita, con un massimo di 20%. Per altre informazioni, vedi [come funziona il Data Store](https://technet.microsoft.com/library/cc772829.aspx).

-   Se il comportamento di ricerca viene eseguito prevalentemente a livello di unità di organizzazione, prendere in considerazione l'indicizzazione per le ricerche nei contenitori.

-   Gli indici di tupla sono necessari per supportare le stringhe di ricerca alfabeto e le stringhe di ricerca finale. Gli indici di tupla non sono necessari per le stringhe di ricerca iniziale.

    -   Iniziale: stringa di ricerca (samAccountName = MYPC\*)

    -   Stringa di ricerca ed - (samAccountName =\*MYPC\*)

    -   Stringa di ricerca finale: (samAccountName =\*MYPC$)

-   Creazione di un indice genererà i/o disco mentre viene compilato l'indice. Questa operazione viene eseguita in un thread in background con priorità più bassa e le richieste in ingresso saranno valutate tramite la compilazione dell'indice. Pianificazione della capacità per l'ambiente sia stata eseguita correttamente, deve essere trasparente. Tuttavia, gli scenari con intensa attività di scrittura o in un ambiente in cui il carico di archiviazione del controller di dominio è sconosciuto può ridurre le prestazioni del client e deve essere eseguita orari di minore attività.

-   Effetti per il traffico di replica è minimo perché la creazione degli indici avviene in locale.

Per altre informazioni, vedere gli argomenti seguenti:

-   [Creazione di applicazioni abilitate alla Directory Microsoft Active più efficiente](https://msdn.microsoft.com/library/ms808539.aspx)

-   [La ricerca in Active Directory Domain Services](https://msdn.microsoft.com/library/aa746427.aspx)

-   [Attributi indicizzati](https://msdn.microsoft.com/library/windows/desktop/ms677112.aspx)

## <a name="see-also"></a>Vedere anche

- [Server Active Directory l'ottimizzazione delle prestazioni](index.md)
- [Considerazioni relative ai requisiti hardware](hardware-considerations.md)
- [Esatto posizionamento dei controller di dominio e considerazioni sul sito](site-definition-considerations.md)
- [Risoluzione dei problemi delle prestazioni di Active Directory Domain Services](troubleshoot.md) 
- [Pianificazione della capacità per Active Directory Domain Services](https://go.microsoft.com/fwlink/?LinkId=324566)