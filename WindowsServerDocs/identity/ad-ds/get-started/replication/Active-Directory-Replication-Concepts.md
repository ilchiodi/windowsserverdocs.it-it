---
ms.assetid: 4cc9c16c-1928-4dce-a3a8-6229be28eb65
title: Concetti di replica di Active Directory
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 087dee7c914b81d97c6cf3a2ea985d809cb57fe6
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59812352"
---
# <a name="active-directory-replication-concepts"></a>Concetti di replica di Active Directory

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Prima di progettare la topologia del sito, acquisire familiarità con alcuni concetti di replica di Active Directory.  
  
-   [Oggetto di connessione](#BKMK_1)  
  
-   [KCC](#BKMK_2)  
  
-   [Funzionalità di failover](#BKMK_3)  
  
-   [Subnet](#BKMK_4)  
  
-   [Sito](#BKMK_5)  
  
-   [Collegamento di sito](#BKMK_6)  
  
-   [Ponte di collegamenti di sito](#BKMK_7)  
  
-   [Transitività dei collegamenti di sito](#BKMK_8)  
  
-   [Server di catalogo globale](#BKMK_9)  
  
-   [Caching dell'appartenenza a gruppo universale](#BKMK_10)  
  
## <a name="BKMK_1"></a>Oggetto di connessione  
Un oggetto di connessione è un oggetto di Active Directory che rappresenta una connessione di replica da un controller di dominio di origine a un controller di dominio di destinazione. Un controller di dominio è un membro di un singolo sito e viene rappresentato nel sito da un oggetto server in Active Directory Domain Services (AD DS). Ogni oggetto server dispone di un elemento figlio dell'oggetto impostazioni NTDS che rappresenta il controller di dominio di replica nel sito.  
  
L'oggetto di connessione è un figlio dell'oggetto impostazioni NTDS nel server di destinazione. Per la replica tra due controller di dominio, l'oggetto server uno deve avere un oggetto di connessione che rappresenta la replica in ingresso da un altro. Tutte le connessioni di replica per un controller di dominio vengono archiviate come oggetti di connessione sotto l'oggetto impostazioni NTDS. L'oggetto di connessione identifica il server di origine di replica, include la pianificazione della replica e specifica un trasporto di replica.  
  
Il controllo di coerenza informazioni (KCC) crea automaticamente gli oggetti di connessione, ma possono anche essere creati manualmente. Gli oggetti connessione creati dal controllo di coerenza informazioni vengono visualizzate nei siti di Active Directory e snap-in servizi come **<automatically generated>** e sono considerati adeguati in condizioni operative normali. Gli oggetti connessione creati da un amministratore vengono creati manualmente gli oggetti connessione. Un oggetto di connessione create manualmente è identificato dal nome assegnato dall'amministratore al momento della creazione. Quando si modifica un' **<automatically generated>** dell'oggetto connessione, convertirlo in un oggetto di connessione modificata a livello amministrativo e l'oggetto viene visualizzato sotto forma di un GUID. Controllo di coerenza informazioni non apportare modifiche agli oggetti connessione manuale o modificati.  
  
## <a name="BKMK_2"></a>KCC  
KCC è incorporato in un processo viene eseguito in tutti i controller di dominio e genera la topologia di replica per la foresta di Active Directory. Controllo di coerenza informazioni consente di creare topologie di replica separati a seconda del fatto che la replica venga eseguita all'interno di un sito (tra siti) o tra siti (tra siti). KCC regola dinamicamente la topologia per supportare l'aggiunta di nuovi controller di dominio, la rimozione di controller di dominio esistenti, lo spostamento dei controller di dominio a e da siti, la modifica delle pianificazioni e i costi e controller di dominio temporaneamente non disponibile o in uno stato di errore.  
  
All'interno di un sito, le connessioni tra controller di dominio scrivibili vengono sempre disposte in un anello bidirezionali, con le connessioni di scelta rapida aggiuntivi per ridurre la latenza nei siti di grandi dimensioni. D'altra parte, la topologia tra siti è una sovrapposizione di relativi alberi, pertanto una connessione tra siti non esiste tra qualsiasi due siti per ogni partizione di directory e a livello generale, non contiene connessioni di scelta rapida. Per altre informazioni sullo spanning di strutture ad albero e topologia di replica di Active Directory, vedere Active Directory replica topologia Technical Reference ([https://go.microsoft.com/fwlink/?LinkID=93578](https://go.microsoft.com/fwlink/?LinkID=93578)).  
  
In ogni controller di dominio KCC crea route di replica tramite la creazione di oggetti di connessione in ingresso unidirezionale che definiscono le connessioni da altri controller di dominio. Per i controller di dominio nello stesso sito, KCC creati oggetti connessione automaticamente senza l'intervento amministrativo. Quando si dispone di più di un sito, è configurare i collegamenti di sito tra siti e un singolo controllo di coerenza informazioni in ogni sito crea automaticamente le connessioni tra i siti anche.  
  
### <a name="kcc-improvements-for-windows-server-2008-rodcs"></a>Miglioramenti di controllo di coerenza informazioni per Windows Server 2008 RODC  

Esistono una serie di miglioramenti di controllo di coerenza informazioni per supportare il controller di dominio appena disponibili di sola lettura (RODC) in Windows Server 2008. Uno scenario di distribuzione tipici per RODC è della succursale. La topologia di replica di Active Directory in genere distribuita in questo scenario si basa su una progettazione hub-spoke, in cui i controller di dominio di succursale in più siti di replicano con un numero ridotto di server testa di ponte in un sito hub.  
  
Uno dei vantaggi della distribuzione RODC in questo scenario è rappresentato dalla replica unidirezionale. Server testa di ponte non sono necessari per la replica dal RODC, che riduce l'amministrazione e utilizzo della rete.  
  
Tuttavia, una sfida amministrativa evidenziata dalla topologia hub-spoke in versioni precedenti del sistema operativo Windows Server non è che dopo aver aggiunto un nuovo controller di dominio di testa di ponte nell'hub, esiste alcun meccanismo automatico per ridistribuire il connessioni di replica tra i controller di dominio di succursale e i controller di dominio hub per sfruttare i vantaggi del nuovo controller di dominio hub.  
  
Per i controller di dominio di Windows Server 2003, è possibile ribilanciare il carico di lavoro usando uno strumento come Adlb.exe dalla Guida alla distribuzione di Windows Server 2003 Branch Office ([https://go.microsoft.com/fwlink/?LinkID=28523](https://go.microsoft.com/fwlink/?LinkID=28523)).  
  
Per Windows Server 2008 RODC, normale funzionamento di KCC fornisce un ribilanciamento, che elimina la necessità di usare uno strumento aggiuntivo, ad esempio Adlb.exe. La nuova funzionalità è abilitata per impostazione predefinita. È possibile disabilitarla aggiungendo la seguente chiave del Registro di sistema impostata nel RODC:  
  
**HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\NTDS\Parameters**  
  
**"BH casuale il bilanciamento del carico consentito"**  
**1 = attivata (impostazione predefinita), 0 = disabilitato**  
  
Per altre informazioni sul funzionano di questi miglioramenti di controllo di coerenza informazioni, vedere pianificazione e distribuzione di Active Directory Domain Services per le succursali ([https://go.microsoft.com/fwlink/?LinkId=107114](https://go.microsoft.com/fwlink/?LinkId=107114)).  
  
## <a name="BKMK_3"></a>Funzionalità di failover  
Siti assicurano che la replica viene indirizzata a errori di rete e controller di dominio offline. Controllo di coerenza informazioni viene eseguito a intervalli specificati per regolare la topologia di replica per le modifiche apportate in Active Directory Domain Services, vengono creati, ad esempio quando vengono aggiunti nuovi controller di dominio e nuovi siti. KCC esamina lo stato di replica delle connessioni esistenti per determinare se tutte le connessioni non funzionano. Se una connessione non funziona a causa di un controller di dominio non riuscita, KCC compili automaticamente connessioni temporanee per gli altri partner di replica, se disponibile, per garantire che la replica venga eseguita. Se non sono disponibili tutti i controller di dominio in un sito, KCC crea automaticamente le connessioni di replica tra controller di dominio da un altro sito.  
  
## <a name="BKMK_4"></a>Subnet  
Una subnet è un segmento di una rete TCP/IP a cui sono assegnati un set di indirizzi IP logici. Subnet raggruppare i computer in modo da loro prossimità fisica sulla rete. Oggetti subnet di Active Directory Domain Services identificano gli indirizzi di rete che consentono di mappare i computer ai siti.  
  
## <a name="BKMK_5"></a>Sito  
I siti sono oggetti di Active Directory che rappresentano una o più subnet TCP/IP con le connessioni di rete altamente affidabile e veloce. Informazioni sul sito consente agli amministratori di configurare l'accesso ad Active Directory e la replica per ottimizzare l'utilizzo della rete fisica. Gli oggetti del sito sono associati a un set di subnet e ogni controller di dominio in una foresta è associato a un sito di Active Directory in base al relativo indirizzo IP. Siti possono ospitare controller di dominio da più di un dominio e un dominio può essere rappresentato in più di un sito.  
  
## <a name="BKMK_6"></a>Collegamento di sito  
I collegamenti di sito sono oggetti di Active Directory che rappresentano percorsi logici che utilizza il controllo di coerenza informazioni per stabilire una connessione per la replica di Active Directory. Un oggetto collegamento sito rappresenta un set di siti che possono comunicare tra loro tramite un trasporto tra siti specificato.  
  
Tutti i siti contenuti all'interno di collegamento di sito sono considerati siano connessi tramite lo stesso tipo di rete. Siti devono essere collegati manualmente ad altri siti utilizzando i collegamenti di sito in modo che i controller di dominio in un sito possono replicare le modifiche directory dal controller di dominio in un altro sito. Poiché i collegamenti di sito non corrispondono per il percorso effettivo impiegato dai pacchetti di rete nella rete fisica durante la replica, non devi creare collegamenti di sito ridondanti per migliorare l'efficienza della replica di Active Directory.  
  
Quando due siti sono connesse tramite un collegamento di sito, il sistema di replica consente di creare automaticamente le connessioni tra i controller di dominio specifico in ogni sito che vengono chiamati server testa di ponte. In Windows Server 2008, tutti i controller di dominio in un sito che ospitano la stessa partizione di directory sono candidati per la selezione come server testa di ponte. Le connessioni di replica create da KCC vengono distribuite in modo casuale tra tutti i server testa di ponte candidato in un sito di condividere il carico di lavoro di replica. Per impostazione predefinita, la selezione casuale processo avviene una sola volta, quando gli oggetti connessione vengono aggiunti prima al sito.  
  
## <a name="BKMK_7"></a>Ponte di collegamenti di sito  
Un ponte di collegamenti di sito è un oggetto di Active Directory che rappresenta un set di collegamenti di sito, tutti i siti di cui può comunicare tramite un trasporto comune. Ponti di collegamenti di sito abilitare i controller di dominio che non sono direttamente connessi tramite un collegamento di comunicazione per la replica tra loro. In genere, un ponte di collegamenti di sito corrisponde a un router (o un set di router) in una rete IP.  
  
Per impostazione predefinita, KCC può formare una route transitiva attraverso i collegamenti di sito tutti con alcuni siti in comune. Se questo comportamento è disabilitato, ogni collegamento di sito rappresenta una propria rete distinto e isolato. Set di collegamenti di sito che possono essere considerati come una singola route sono espressi tramite un ponte di collegamenti di sito. Ogni bridge rappresenta un ambiente di comunicazione di tipo isolato il traffico di rete.  
  
Ponti di collegamenti di sito sono un meccanismo per rappresentare in modo logico la connettività fisica transitiva tra siti. Un ponte di collegamenti di sito consente a KCC di utilizzare qualsiasi combinazione dei collegamenti di sito inclusi per determinare la route meno costosa per interconnettere partizioni di directory contenute in tali siti. Il ponte di collegamenti di sito non fornisce effettivamente connettersi ai controller di dominio. Se viene rimosso il ponte di collegamenti di sito, la replica tramite i collegamenti di sito combinato continuerà fino a quando KCC rimuove i collegamenti.  
  
Ponti di collegamenti di sito sono necessari solo se un sito contiene un controller di dominio che ospita una partizione di directory che non è ospitata anche in un controller di dominio in un sito adiacente, ma un controller di dominio che ospita la partizione di directory si trova in uno o più siti di la foresta. Nei siti adiacenti vengono definiti come due o più siti inclusi in un singolo collegamento di sito.  
  
Un ponte di collegamenti di sito crea una connessione logica tra due collegamenti di sito, che fornisce un percorso transitivo tra due siti non connessi tramite un sito provvisorio. Ai fini del generatore di topologia tra siti (ISTG), il bridge implica connettività fisica utilizzando il sito provvisorio. Il bridge non implica che un controller di dominio nel sito intermedio fornirà il percorso di replica. Tuttavia, ciò potrebbe valere se il sito provvisorio contenuti un controller di dominio che ospita la partizione di directory devono essere replicate, nel qual caso un ponte di collegamenti di sito non è necessario.  
  
Viene aggiunto il costo di ogni collegamento di sito, la creazione di un costo totale per il percorso risulta. Il ponte di collegamenti di sito verrà utilizzato qualora il sito intermedio non contiene un controller di dominio che ospita la partizione di directory e un collegamento di costo inferiore non esiste. Se il sito provvisorio conteneva un controller di dominio che ospita la partizione di directory, due siti non connessi sarebbero configurare connessioni di replica al controller di dominio provvisorio e non usare il bridge.  
  
## <a name="BKMK_8"></a>Transitività dei collegamenti di sito  
Per impostazione predefinita tutti i collegamenti di sito sono transitive, o "bridging". Quando sono collegati tramite bridge i collegamenti di sito e le pianificazioni si sovrappongono, il controllo di coerenza informazioni consente di creare connessioni di replica che determinano i partner di replica controller di dominio tra siti, in cui i siti non sono direttamente connessi da collegamenti di sito ma sono connessi in modo transitivo attraverso un set di siti comuni. Ciò significa che sia possibile connettersi qualsiasi sito a un altro sito tramite una combinazione di collegamenti di sito.  
  
In generale, per una rete completamente instradata, non occorre creare i ponti di collegamenti di qualsiasi sito a meno che non si desidera controllare il flusso delle modifiche della replica. Se la rete non viene instradata completamente, ponti di collegamenti di sito devono essere creati per evitare tentativi di replica impossibili. Tutti i collegamenti di sito per un determinato trasporto in modo implicito appartengano a un ponte di collegamenti di sito singolo di tale tipo di trasporto. Il bridging predefinito per i collegamenti di sito non avviene automaticamente e nessun oggetto Active Directory rappresenta questo bridge. Il **Bridge i collegamenti di sito** impostazione, disponibile nelle proprietà dei contenitori trasporto tra siti IP sia Simple Mail Transfer Protocol (SMTP), implementa il bridging di collegamenti di sito automatica.  
  
> [!NOTE]  
> La replica SMTP non sarà supportata nelle versioni future di Active Directory Domain Services; Pertanto, non è consigliabile creare gli oggetti di collegamenti di sito nel contenitore SMTP.  
  
## <a name="BKMK_9"></a>Server di catalogo globale  
Un server di catalogo globale è un controller di dominio che archivia le informazioni su tutti gli oggetti nella foresta, in modo che le applicazioni possono eseguire la ricerca Active Directory Domain Services senza fare riferimento a specifici controller di dominio che archiviano i dati richiesti. Come tutti i controller di dominio, server di catalogo globale archivia repliche complete, accessibile in scrittura delle partizioni di directory dello schema e la configurazione e una replica completa e scrivibile della partizione di directory dominio per il dominio che lo ospita. Inoltre, un server di catalogo globale archiviata una replica parziale di sola lettura di tutti gli altri domini nella foresta. Le repliche di dominio parziali, di sola lettura contengono tutti gli oggetti nel dominio ma solo un subset degli attributi (gli attributi che sono più comunemente utilizzati per la ricerca dell'oggetto).  
  
## <a name="BKMK_10"></a>Caching dell'appartenenza a gruppo universale  
Caching dell'appartenenza a gruppo universale consente al controller di dominio di informazioni sull'appartenenza a gruppi universali di cache per gli utenti. È possibile abilitare i controller di dominio che eseguono Windows Server 2008 per le appartenenze ai gruppi universali cache con i siti di Active Directory e snap-in servizi.  
  
Abilitazione di caching dell'appartenenza a gruppo universale Elimina la necessità di un server di catalogo globale in ogni sito in un dominio, riducendo al minimo dell'utilizzo della larghezza di banda di rete perché un controller di dominio non è necessario replicare tutti gli oggetti che si trova nella foresta. Riduce inoltre tempi di accesso perché i controller di dominio che esegue l'autenticazione non sono sempre necessario accedere a un catalogo globale per ottenere informazioni sull'appartenenza a gruppi universali. Per altre informazioni sull'uso di caching dell'appartenenza a gruppi universali, vedere [pianificazione posizionamento del Server di catalogo globale](../../../ad-ds/plan/Planning-Global-Catalog-Server-Placement.md).  
  


