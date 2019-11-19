---
ms.assetid: 4cc9c16c-1928-4dce-a3a8-6229be28eb65
title: Concetti di replica di Active Directory
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: ccc1801ba325fec4d7273b503ccb799966122b99
ms.sourcegitcommit: 9a6a692a7b2a93f52bb9e2de549753e81d758d28
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 10/18/2019
ms.locfileid: "72591076"
---
# <a name="active-directory-replication-concepts"></a>Concetti di replica di Active Directory

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Prima di progettare la topologia del sito, acquisire familiarità con alcuni Active Directory concetti di replica.  
  
-   [Oggetto Connection](#BKMK_1)  
  
-   [KCC](#BKMK_2)  
  
-   [Funzionalità di failover](#BKMK_3)  
  
-   [Subnet](#BKMK_4)  
  
-   [Sito](#BKMK_5)  
  
-   [Collegamento di sito](#BKMK_6)  
  
-   [Bridge di collegamenti di sito](#BKMK_7)  
  
-   [Transitività del collegamento di sito](#BKMK_8)  
  
-   [Server di catalogo globale](#BKMK_9)  
  
-   [Memorizzazione nella cache dell'appartenenza a gruppi universali](#BKMK_10)  
  
## <a name="BKMK_1"></a>Oggetto Connection  
Un oggetto connessione è un oggetto Active Directory che rappresenta una connessione di replica da un controller di dominio di origine a un controller di dominio di destinazione. Un controller di dominio è un membro di un singolo sito e viene rappresentato nel sito da un oggetto server in Active Directory Domain Services (AD DS). Ogni oggetto server dispone di un oggetto Impostazioni NTDS figlio che rappresenta il controller di dominio di replica nel sito.  
  
L'oggetto Connection è un elemento figlio dell'oggetto Impostazioni NTDS nel server di destinazione. Per eseguire la replica tra due controller di dominio, l'oggetto server di uno deve disporre di un oggetto connessione che rappresenta la replica in ingresso dell'altro. Tutte le connessioni di replica per un controller di dominio vengono archiviate come oggetti connessione nell'oggetto Impostazioni NTDS. L'oggetto Connection identifica il server di origine della replica, contiene una pianificazione di replica e specifica un trasporto di replica.  
  
Il controllo di coerenza informazioni (KCC) crea automaticamente oggetti connessione, ma è possibile crearli anche manualmente. Gli oggetti connessione creati da KCC vengono visualizzati nello snap-in siti e servizi Active Directory come **<automatically generated>** e sono considerati adeguati in condizioni operative normali. Gli oggetti connessione creati da un amministratore vengono creati manualmente. Un oggetto connessione creato manualmente viene identificato dal nome assegnato dall'amministratore al momento della creazione. Quando si modifica un oggetto **<automatically generated>** connessione, questo viene convertito in un oggetto di connessione modificato in modo amministrativo e l'oggetto viene visualizzato sotto forma di GUID. Il KCC non consente di apportare modifiche agli oggetti connessione manuali o modificati.  
  
## <a name="BKMK_2"></a>KCC  
KCC è un processo incorporato che viene eseguito su tutti i controller di dominio e genera la topologia di replica per la foresta Active Directory. KCC crea topologie di replica separate a seconda che si verifichi la replica all'interno di un sito (intrasito) o tra siti (tra siti). Il KCC consente inoltre di modificare dinamicamente la topologia per supportare l'aggiunta di nuovi controller di dominio, la rimozione di controller di dominio esistenti, lo spostamento dei controller di dominio da e verso i siti, la modifica di costi e pianificazioni e i controller di dominio temporaneamente non disponibile o in stato di errore.  
  
All'interno di un sito, le connessioni tra i controller di dominio scrivibili sono sempre disposte in un anello bidirezionale, con connessioni di collegamento aggiuntive per ridurre la latenza nei siti di grandi dimensioni. D'altra parte, la topologia tra siti è una sovrapposizione di alberi con spanning, ovvero esiste una connessione tra siti tra due siti per ogni partizione di directory e in genere non contiene connessioni di collegamento. Per ulteriori informazioni sugli alberi con spanning e Active Directory topologia di replica, vedere riferimento tecnico per la topologia di replica Active Directory ([https://go.microsoft.com/fwlink/?LinkID=93578](https://go.microsoft.com/fwlink/?LinkID=93578)).  
  
In ogni controller di dominio, KCC crea route di replica creando oggetti connessione in ingresso unidirezionali che definiscono le connessioni da altri controller di dominio. Per i controller di dominio nello stesso sito, KCC crea automaticamente oggetti connessione senza intervento amministrativo. Quando si dispone di più di un sito, si configurano i collegamenti di sito tra siti e un singolo KCC in ogni sito crea automaticamente anche le connessioni tra siti.  
  
### <a name="kcc-improvements-for-windows-server-2008-rodcs"></a>Miglioramenti di KCC per Windows Server 2008 RODC  

Il controller di dominio di sola lettura (RODC) disponibile in Windows Server 2008 include diversi miglioramenti apportati a KCC. Uno scenario di distribuzione tipico per il RODC è la succursale. La topologia di replica Active Directory più comunemente distribuita in questo scenario si basa su una progettazione hub e spoke, in cui i controller di dominio ramo in più siti vengono replicati con un numero ridotto di server testa di ponte in un sito hub.  
  
Uno dei vantaggi della distribuzione di RODC in questo scenario è la replica unidirezionale. Non è necessario che i server testa di ponte vengano replicati dal RODC, riducendo l'amministrazione e l'utilizzo della rete.  
  
Tuttavia, una sfida amministrativa evidenziata dalla topologia hub-spoke nelle versioni precedenti del sistema operativo Windows Server è che dopo l'aggiunta di un nuovo controller di dominio testa di ponte nell'hub, non esiste alcun meccanismo automatico per ridistribuire connessioni di replica tra i controller di dominio ramo e i controller di dominio hub per sfruttare i vantaggi del nuovo controller di dominio hub.  
  
Per i controller di dominio Windows Server 2003, è possibile ribilanciare il carico di lavoro usando uno strumento come Adlb. exe nella Guida alla distribuzione di una succursale di Windows Server 2003 ([https://go.microsoft.com/fwlink/?LinkID=28523](https://go.microsoft.com/fwlink/?LinkID=28523)).  
  
Per Windows Server 2008 RODC, il funzionamento normale di KCC fornisce un nuovo bilanciamento, che elimina la necessità di usare uno strumento aggiuntivo, ad esempio Adlb. exe. La nuova funzionalità è abilitata per impostazione predefinita. È possibile disabilitare questa impostazione aggiungendo la seguente chiave del registro di sistema nel controller di sola lettura:  
  
**HKEY_LOCAL_MACHINE \SYSTEM\CurrentControlSet\Services\NTDS\Parameters**  
  
**"Loadbalancing BH casuale consentito"**  
**1 = abilitato (impostazione predefinita), 0 = disabilitato**  
  
Per ulteriori informazioni sul funzionamento di questi miglioramenti di KCC, vedere Planning and Deploying Active Directory Domain Services for Branch Succursal ([https://go.microsoft.com/fwlink/?LinkId=107114](https://go.microsoft.com/fwlink/?LinkId=107114)).  
  
## <a name="BKMK_3"></a>Funzionalità di failover  
I siti garantiscono che la replica venga instradata in caso di errori di rete e controller di dominio offline. Il KCC viene eseguito a intervalli specificati per modificare la topologia di replica per le modifiche che si verificano in servizi di dominio Active Directory, ad esempio quando vengono aggiunti nuovi controller di dominio e vengono creati nuovi siti. KCC esamina lo stato della replica delle connessioni esistenti per determinare se le connessioni non funzionano. Se una connessione non funziona a causa di un controller di dominio che ha avuto esito negativo, KCC compila automaticamente le connessioni temporanee ad altri partner di replica (se disponibili) per assicurarsi che la replica venga eseguita. Se non sono disponibili tutti i controller di dominio in un sito, KCC crea automaticamente le connessioni di replica tra i controller di dominio di un altro sito.  
  
## <a name="BKMK_4"></a>Subnet  
Una subnet è un segmento di una rete TCP/IP a cui sono assegnati un set di indirizzi IP logici. Le subnet raggruppano i computer in modo che identifichino la prossimità fisica sulla rete. Gli oggetti subnet in servizi di dominio Active Directory identificano gli indirizzi di rete usati per eseguire il mapping dei computer ai siti.  
  
## <a name="BKMK_5"></a>Sito  
I siti sono Active Directory oggetti che rappresentano una o più subnet TCP/IP con connessioni di rete estremamente affidabili e veloci. Le informazioni sul sito consentono agli amministratori di configurare Active Directory l'accesso e la replica per ottimizzare l'utilizzo della rete fisica. Gli oggetti del sito sono associati a un set di subnet e ogni controller di dominio in una foresta è associato a un sito Active Directory in base al relativo indirizzo IP. I siti possono ospitare controller di dominio da più di un dominio e un dominio può essere rappresentato in più di un sito.  
  
## <a name="BKMK_6"></a>Collegamento di sito  
I collegamenti di sito sono Active Directory oggetti che rappresentano i percorsi logici utilizzati da KCC per stabilire una connessione per Active Directory la replica. Un oggetto collegamento di sito rappresenta un set di siti che possono comunicare a costo uniforme tramite un trasporto tra siti specificato.  
  
Tutti i siti contenuti all'interno del collegamento di sito sono considerati connessi tramite lo stesso tipo di rete. I siti devono essere collegati manualmente ad altri siti usando collegamenti di sito, in modo che i controller di dominio in un sito possano replicare le modifiche della directory dai controller di dominio in un altro sito. Poiché i collegamenti di sito non corrispondono al percorso effettivo effettuato dai pacchetti di rete nella rete fisica durante la replica, non è necessario creare collegamenti di sito ridondanti per migliorare l'efficienza della replica Active Directory.  
  
Quando due siti sono connessi da un collegamento di sito, il sistema di replica crea automaticamente le connessioni tra controller di dominio specifici in ogni sito denominato server testa di ponte. In Windows Server 2008, tutti i controller di dominio in un sito che ospitano la stessa partizione di directory sono candidati per essere selezionati come server testa di ponte. Le connessioni di replica create da KCC vengono distribuite in modo casuale tra tutti i server testa di ponte candidato in un sito per condividere il carico di lavoro di replica. Per impostazione predefinita, il processo di selezione casuale viene eseguito una sola volta, quando gli oggetti di connessione vengono prima aggiunti al sito.  
  
## <a name="BKMK_7"></a>Bridge di collegamenti di sito  
Un Bridge di collegamenti di sito è un oggetto Active Directory che rappresenta un set di collegamenti di sito, tutti i cui siti possono comunicare utilizzando un trasporto comune. I Bridge di collegamenti di sito consentono i controller di dominio che non sono direttamente connessi tramite un collegamento di comunicazione per la replica reciproca. In genere, un Bridge di collegamenti di sito corrisponde a un router (o a un set di router) su una rete IP.  
  
Per impostazione predefinita, il KCC può formare una route transitiva attraverso tutti i collegamenti di sito con alcuni siti comuni. Se questo comportamento è disabilitato, ciascun collegamento di sito rappresenta una propria rete distinta e isolata. I set di collegamenti di sito che possono essere considerati come una singola route vengono espressi tramite un Bridge di collegamenti di sito. Ogni Bridge rappresenta un ambiente di comunicazione isolato per il traffico di rete.  
  
I Bridge di collegamenti di sito sono un meccanismo per rappresentare logicamente la connettività fisica transitiva tra siti. Un Bridge di collegamenti di sito consente all'KCC di utilizzare qualsiasi combinazione dei collegamenti di sito inclusi per determinare la route meno costosa per l'interconnessione delle partizioni di directory contenute in questi siti. Il Bridge di collegamenti di sito non fornisce la connettività effettiva ai controller di dominio. Se il Bridge di collegamenti di sito viene rimosso, la replica sui collegamenti di sito combinati continuerà fino a quando il KCC non rimuove i collegamenti.  
  
I Bridge di collegamenti di sito sono necessari solo se un sito contiene un controller di dominio che ospita una partizione di directory non ospitata anche in un controller di dominio in un sito adiacente, ma un controller di dominio che ospita la partizione di directory si trova in uno o più siti di insieme di strutture. I siti adiacenti sono definiti come due o più siti inclusi in un singolo collegamento di sito.  
  
Un Bridge di collegamenti di sito crea una connessione logica tra due collegamenti di sito, fornendo un percorso transitivo tra due siti disconnessi tramite un sito provvisorio. Ai fini del generatore di topologia tra siti (ISTG), il Bridge implica la connettività fisica tramite il sito provvisorio. Il Bridge non implica che un controller di dominio nel sito provvisorio fornirà il percorso di replica. Tuttavia, questa situazione si verifica se il sito provvisorio conteneva un controller di dominio che ospitava la partizione di directory da replicare, nel qual caso non è necessario un Bridge di collegamenti di sito.  
  
Viene aggiunto il costo di ogni collegamento di sito, creando un costo sommato per il percorso risultante. Il Bridge di collegamenti di sito verrebbe utilizzato se il sito provvisorio non contiene un controller di dominio che ospita la partizione di directory e non esiste un collegamento di costo inferiore. Se il sito provvisorio contiene un controller di dominio che ospita la partizione di directory, due siti disconnessi configurano le connessioni di replica al controller di dominio provvisorio e non usano il Bridge.  
  
## <a name="BKMK_8"></a>Transitività del collegamento di sito  
Per impostazione predefinita, tutti i collegamenti di sito sono transitivi o "bridged". Quando i collegamenti di sito sono collegati a Bridge e le pianificazioni si sovrappongono, KCC crea connessioni di replica che determinano i partner di replica del controller di dominio tra siti, in cui i siti non sono connessi direttamente dai collegamenti di sito, ma sono connessi in modo transitivo tramite un set di siti comuni. Ciò significa che è possibile connettere qualsiasi sito a qualsiasi altro sito tramite una combinazione di collegamenti di sito.  
  
In generale, per una rete completamente indirizzata, non è necessario creare ponti di collegamenti di sito a meno che non si desideri controllare il flusso delle modifiche della replica. Se la rete non viene instradata completamente, è necessario creare ponti di collegamenti di sito per evitare possibili tentativi di replica. Tutti i collegamenti di sito per un trasporto specifico appartengono in modo implicito a un unico Bridge di collegamenti di sito per il trasporto. Il bridging predefinito per i collegamenti di sito viene eseguito automaticamente e nessun oggetto Active Directory rappresenta tale Bridge. L'impostazione **Bridge tutti i collegamenti di sito** , disponibile nelle proprietà dei contenitori di trasporto tra siti IP e Simple Mail Transfer Protocol (SMTP), implementa il bridging automatico del collegamento di sito.  
  
> [!NOTE]  
> La replica SMTP non sarà supportata nelle versioni future di servizi di dominio Active Directory. Pertanto, non è consigliabile creare oggetti collegamenti di sito nel contenitore SMTP.  
  
## <a name="BKMK_9"></a>Server di catalogo globale  
Un server di catalogo globale è un controller di dominio che archivia le informazioni su tutti gli oggetti nella foresta, in modo che le applicazioni possano eseguire ricerche in servizi di dominio Active Directory senza fare riferimento a controller di dominio specifici che archiviano i dati richiesti. Come tutti i controller di dominio, un server di catalogo globale archivia le repliche scrivibili e complete delle partizioni di directory dello schema e della configurazione e una replica scrivibile completa della partizione di directory del dominio per il dominio che ospita. Inoltre, un server di catalogo globale archivia una replica parziale di sola lettura di ogni altro dominio nella foresta. Le repliche di dominio parziali di sola lettura contengono tutti gli oggetti nel dominio, ma solo un subset degli attributi (gli attributi usati più di frequente per la ricerca nell'oggetto).  
  
## <a name="BKMK_10"></a>Memorizzazione nella cache dell'appartenenza a gruppi universali  
La memorizzazione nella cache dell'appartenenza a gruppi universali consente al controller di dominio di memorizzare nella cache le informazioni sull'appartenenza a gruppi universali È possibile abilitare i controller di dominio che eseguono Windows Server 2008 per memorizzare nella cache le appartenenze a gruppi universali usando lo snap-in siti e servizi Active Directory.  
  
L'abilitazione della memorizzazione nella cache dell'appartenenza a gruppi universali Elimina la necessità di un server di catalogo globale in ogni sito in un dominio, riducendo al minimo l'utilizzo della larghezza di banda di rete perché un controller di dominio non deve replicare tutti gli oggetti presenti nella foresta. Consente inoltre di ridurre i tempi di accesso perché i controller di dominio di autenticazione non devono sempre accedere a un catalogo globale per ottenere informazioni sull'appartenenza a gruppi universali. Per ulteriori informazioni su quando utilizzare la memorizzazione nella cache dell'appartenenza a gruppi universali, vedere [pianificazione del posizionamento del server di catalogo globale](../../../ad-ds/plan/Planning-Global-Catalog-Server-Placement.md).  
  


