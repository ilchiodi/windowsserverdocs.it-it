---
title: Considerazioni su LDAP in aggiunge l'ottimizzazione delle prestazioni
description: Considerazioni su LDAP nei carichi di lavoro Active Directory
ms.prod: windows-server
ms.technology: performance-tuning-guide
ms.topic: article
ms.author: TimWi; ChrisRob; HerbertM; KenBrumf;  MLeary; ShawnRab
author: phstee
ms.date: 10/16/2017
ms.openlocfilehash: f6670c8cfd718360518869f0551461c45e5aed27
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71370278"
---
# <a name="ldap-considerations-in-adds-performance-tuning"></a>Considerazioni su LDAP in aggiunge l'ottimizzazione delle prestazioni

> [!IMPORTANT]
> Di seguito è riportato un riepilogo delle indicazioni e delle considerazioni principali per ottimizzare l'hardware del server per i carichi di lavoro Active Directory analizzati in modo più approfondito nella [pianificazione della capacità per Active Directory Domain Services](https://go.microsoft.com/fwlink/?LinkId=324566) articolo. I lettori sono vivamente invitati a esaminare la [pianificazione della capacità per Active Directory Domain Services](https://go.microsoft.com/fwlink/?LinkId=324566) per una maggiore comprensione tecnica e le implicazioni di questi consigli.

## <a name="verify-ldap-queries"></a>Verificare le query LDAP

Verificare che le query LDAP siano conformi alle raccomandazioni per la creazione di query efficienti.

Per informazioni su come scrivere, strutturare e analizzare correttamente le query da utilizzare in Active Directory, è disponibile un'ampia documentazione su MSDN. Per altre informazioni, vedere [creazione di applicazioni più efficienti abilitate per Microsoft Active Directory](https://msdn.microsoft.com/library/ms808539.aspx).

## <a name="optimize-ldap-page-sizes"></a>Ottimizzare le dimensioni delle pagine LDAP

Quando si restituiscono risultati con più oggetti in risposta alle richieste client, il controller di dominio deve archiviare temporaneamente il set di risultati in memoria. L'aumento delle dimensioni di pagina provocherà un maggiore utilizzo della memoria e può influire inutilmente sugli elementi della cache. In questo caso, le impostazioni predefinite sono ottimali. Esistono diversi scenari in cui sono state apportate raccomandazioni per aumentare le impostazioni delle dimensioni della pagina. Si consiglia di utilizzare i valori predefiniti, a meno che non sia specificato in modo non adeguato.

Quando le query hanno molti risultati, è possibile che venga rilevato un limite di query simili eseguite simultaneamente.  Questo problema si verifica perché il server LDAP può esaurire un'area di memoria globale nota come pool di cookie.  Potrebbe essere necessario aumentare le dimensioni del pool come descritto in [come vengono gestiti i cookie del server LDAP](https://technet.microsoft.com/windows-server-docs/identity/ad-ds/manage/how-ldap-server-cookies-are-handled).

Per ottimizzare queste impostazioni, vedere [Windows Server 2008 e il controller di dominio più recente restituisce solo 5000 valori in una risposta LDAP](https://support.microsoft.com/kb/2009267).

## <a name="determine-whether-to-add-indices"></a>Determinare se aggiungere indici

Gli attributi di indicizzazione sono utili per la ricerca di oggetti con il nome dell'attributo in un filtro. L'indicizzazione può ridurre il numero di oggetti che devono essere visitati durante la valutazione del filtro. Tuttavia, ciò riduce le prestazioni delle operazioni di scrittura perché l'indice deve essere aggiornato quando viene modificato o aggiunto l'attributo corrispondente. Aumenta anche la dimensione del database di directory, sebbene i vantaggi spesso superino il costo di archiviazione. La registrazione può essere usata per trovare le query costose e inefficienti. Una volta identificati, provare a indicizzare alcuni attributi usati nelle query corrispondenti per migliorare le prestazioni di ricerca. Per ulteriori informazioni sul funzionamento delle ricerche Active Directory, vedere la pagina relativa al funzionamento delle [ricerche Active Directory](https://technet.microsoft.com/library/cc755809.aspx).

### <a name="scenarios-that-benefit-in-adding-indices"></a>Scenari che traggono vantaggio dall'aggiunta di indici

-   Il carico del client in cui vengono richiesti i dati sta generando un utilizzo significativo della CPU e non è possibile modificare o ottimizzare il comportamento della query client. Per carico significativo, tenere presente che viene visualizzato in un elenco dei primi 10 trasgressori in Server Performance Advisor o nel set di agenti di raccolta dati Active Directory incorporato e che utilizza più dell'1% di CPU.

-   Il carico client sta generando un I/O su disco significativo in un server a causa di un attributo non indicizzato e non è possibile modificare o ottimizzare il comportamento della query client.

-   Una query richiede molto tempo e non viene completata in un intervallo di tempo accettabile per il client a causa della mancanza di copertura degli indici.

- Grandi volumi di query con durata elevata causano il consumo e l'esaurimento dei thread LDAP ATQ. Monitorare i contatori delle prestazioni seguenti:

    - **NTDS @ no__t-1Request latenza** : questo è soggetto al tempo necessario per l'elaborazione della richiesta. Active Directory timeout delle richieste dopo 120 secondi (impostazione predefinita), tuttavia, la maggior parte dovrebbe essere eseguita molto più velocemente e le query con esecuzione estremamente lunga dovrebbero essere nascoste nei numeri complessivi. Cercare le modifiche in questa linea di base, anziché le soglie assolute.

        > [!NOTE]
        > I valori elevati possono inoltre essere indicatori di ritardi nelle richieste di "inoltro" ad altri domini e controlli CRL.

    - **NTDS @ no__t-1Estimated Queue Delay** : questo dovrebbe essere idealmente prossimo a 0 per ottenere prestazioni ottimali, in quanto ciò significa che le richieste non passano tempo in attesa di essere gestite.

Questi scenari possono essere rilevati utilizzando uno o più degli approcci seguenti:

-   [Determinazione del tempo di esecuzione delle query con il controllo Statistics](https://msdn.microsoft.com/library/ms808539.aspx)

-   [Rilevamento di ricerche costose e inefficienti](https://msdn.microsoft.com/library/ms808539.aspx)

-   Active Directory insieme agenti di raccolta dati di diagnostica in Performance Monitor ([Son di SPA: Insiemi agenti di raccolta dati AD in Win2008 e oltre @ no__t-0)

-   [Microsoft Server Performance Advisor](../../../server-performance-advisor/microsoft-server-performance-advisor.md) Pacchetto di Active Directory Advisor

-   Esegue la ricerca utilizzando un filtro oltre a "(objectClass = \*)" che utilizzano l'indice dei predecessori.

### <a name="other-index-considerations"></a>Altre considerazioni sugli indici

-   Assicurarsi che la creazione dell'indice sia la soluzione corretta al problema dopo che l'ottimizzazione della query è stata esaurita come opzione. Il dimensionamento corretto dell'hardware è molto importante. Gli indici devono essere aggiunti solo quando la correzione corretta prevede l'indicizzazione dell'attributo e non un tentativo di offuscare i problemi hardware.

-   Gli indici aumentano le dimensioni del database di almeno la dimensione totale dell'attributo indicizzato. Una stima della crescita del database può quindi essere valutata prendendo le dimensioni medie dei dati nell'attributo e moltiplicando per il numero di oggetti in cui verrà popolato l'attributo. In genere si tratta di un aumento del 1% delle dimensioni del database. Per altre informazioni, vedere funzionamento [dell'archivio dati](https://technet.microsoft.com/library/cc772829.aspx).

-   Se il comportamento di ricerca viene eseguito prevalentemente a livello di unità organizzativa, prendere in considerazione l'indicizzazione per le ricerche in contenitori.

-   Gli indici di tupla sono più grandi degli indici normali, ma è molto più difficile stimare la dimensione. Usare le stime delle dimensioni normali degli indici come il piano per la crescita, con un massimo del 20%. Per altre informazioni, vedere funzionamento [dell'archivio dati](https://technet.microsoft.com/library/cc772829.aspx).

-   Se il comportamento di ricerca viene eseguito prevalentemente a livello di unità organizzativa, prendere in considerazione l'indicizzazione per le ricerche in contenitori.

-   Gli indici di tupla sono necessari per supportare le stringhe di ricerca mediale e le stringhe di ricerca finale. Gli indici di tupla non sono necessari per le stringhe di ricerca iniziali.

    -   Stringa di ricerca iniziale – (samAccountName = MYPC @ no__t-0)

    -   Stringa di ricerca mediana-(samAccountName = \*MYPC @ no__t-1)

    -   Stringa di ricerca finale – (samAccountName = \*MYPC $)

-   La creazione di un indice genererà l'I/O del disco durante la compilazione dell'indice. Questa operazione viene eseguita in un thread in background con priorità più bassa e le richieste in ingresso verranno classificate in ordine di priorità sulla compilazione dell'indice. Se la pianificazione della capacità per l'ambiente è stata eseguita correttamente, questa operazione dovrebbe essere trasparente. Tuttavia, gli scenari con gravi Scritture o un ambiente in cui il carico sull'archiviazione del controller di dominio è sconosciuto potrebbero compromettere l'esperienza del client ed essere eseguiti fuori orario.

-   L'effetto sul traffico di replica è minimo perché la compilazione degli indici avviene localmente.

Per ulteriori informazioni, vedere gli argomenti seguenti:

-   [Creazione di applicazioni più efficienti abilitate per Active Directory Microsoft](https://msdn.microsoft.com/library/ms808539.aspx)

-   [Ricerca in Active Directory Domain Services](https://msdn.microsoft.com/library/aa746427.aspx)

-   [Attributi indicizzati](https://msdn.microsoft.com/library/windows/desktop/ms677112.aspx)

## <a name="see-also"></a>Vedere anche

- [Ottimizzazione delle prestazioni di Active Directory Server](index.md)
- [Considerazioni relative ai requisiti hardware](hardware-considerations.md)
- [Esatto posizionamento dei controller di dominio e considerazioni sul sito](site-definition-considerations.md)
- [Risoluzione dei problemi delle prestazioni di Active Directory Domain Services](troubleshoot.md) 
- [Pianificazione della capacità per Active Directory Domain Services](https://go.microsoft.com/fwlink/?LinkId=324566)