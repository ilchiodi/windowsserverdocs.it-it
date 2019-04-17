---
ms.assetid: 4cc9c16c-1928-4dce-a3a8-6229be28eb65
title: Concetti di replica di Active Directory
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 45753c048f9bb1cb174daade1d408b88b3686b4b
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/12/2017
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
  
-   [Caching appartenenza a gruppo universale](#BKMK_10)  
  
## <a name="BKMK_1"></a>Oggetto di connessione  
Un oggetto di connessione è un oggetto di Active Directory che rappresenta una connessione di replica da un controller di dominio di origine a un controller di dominio di destinazione. Un controller di dominio è un membro di un singolo sito ed è rappresentato da un oggetto server in servizi di dominio Active Directory (AD DS) nel sito. Ogni oggetto server dispone di un elemento figlio dell'oggetto impostazioni NTDS che rappresenta il controller di dominio di replica nel sito.  
  
L'oggetto di connessione è un elemento figlio dell'oggetto impostazioni NTDS nel server di destinazione. Per la replica tra due controller di dominio, l'oggetto server di uno deve disporre di un oggetto di connessione che rappresenta la replica in ingresso da altro. Tutte le connessioni di replica per un controller di dominio vengono archiviate come oggetti connessione sotto l'oggetto impostazioni NTDS. L'oggetto connessione identifica il server di origine di replica, contiene una pianificazione di replica e specifica un trasporto di replica.  
  
Il controllo di coerenza informazioni (KCC) crea automaticamente gli oggetti connessione, ma possono anche essere creati manualmente. Gli oggetti connessione creati da KCC vengono visualizzati nei siti di Active Directory e snap-in servizi come **<automatically generated>**e sono considerate sufficienti in condizioni operative normali. Gli oggetti connessione creati da un amministratore manualmente vengono creati oggetti connessione. Un oggetto di connessione creati manualmente viene identificato dal nome assegnato dall'amministratore quando è stata creata. Quando si modifica un **<automatically generated>**oggetto di connessione, convertirlo in un oggetto connessione modificate dall'amministratore e l'oggetto viene visualizzato sotto forma di un GUID. Il controllo di coerenza informazioni non apportare modifiche agli oggetti connessione manuale o modificati.  
  
## <a name="BKMK_2"></a>KCC  
Il controllo di coerenza informazioni è un processo eseguito in tutti i controller di dominio che genera la topologia di replica per la foresta di Active Directory. KCC crea topologie di replica separati a seconda se la replica viene eseguita all'interno di un sito (all'interno del sito) o tra siti (ISM). Il controllo di coerenza informazioni regola dinamicamente la topologia per supportare l'aggiunta di nuovi controller di dominio, la rimozione di controller di dominio esistenti, il movimento di controller di dominio a e da siti, modifica i costi e le pianificazioni e i controller di dominio che non sono temporaneamente disponibili o in uno stato di errore.  
  
All'interno di un sito, le connessioni tra controller di dominio scrivibili vengono sempre disposti in un circuito bidirezionale, con le connessioni di scelta rapida aggiuntive per ridurre la latenza di siti di grandi dimensioni. D'altra parte, la topologia tra siti è una sovrapposizione di spanning alberi, ovvero una connessione tra siti tra i due siti per ogni partizione di directory e in genere non contiene le connessioni di scelta rapida. Per ulteriori informazioni sullo spanning strutture ad albero e topologia di replica di Active Directory, vedere Active Directory replica topologia Technical Reference ([https://go.microsoft.com/fwlink/?LinkID=93578](https://go.microsoft.com/fwlink/?LinkID=93578)).  
  
In ogni controller di dominio, il controllo di coerenza informazioni crea le route di replica mediante la creazione di oggetti di connessione in ingresso unidirezionale che definiscono le connessioni da altri controller di dominio. Per i controller di dominio nello stesso sito, KCC crea gli oggetti connessione automaticamente senza l'intervento dell'amministratore. Quando si dispone di più di un sito, configurare i collegamenti di sito tra siti e un singolo controllo di coerenza informazioni in ogni sito crea automaticamente le connessioni tra i siti anche.  
  
### <a name="kcc-improvements-for-windows-server-2008-rodcs"></a>Miglioramenti di controllo di coerenza informazioni per la lettura di Windows Server 2008  

Esistono numerosi miglioramenti KCC per supportare il controller di dominio appena disponibile di sola lettura (RODC) in Windows Server 2008. Uno scenario di distribuzione tipica per RODC è della succursale. La topologia di replica di Active Directory in genere distribuita in questo scenario si basa su una progettazione di hub e spoke, in cui eseguire la replica controller di dominio di succursale in più siti con un numero ridotto di server testa di ponte in un sito hub.  
  
Uno dei vantaggi della distribuzione RODC in questo scenario è la replica unidirezionale. Server testa di ponte non sono necessari per la replica dal controller di dominio, che riduce l'amministrazione e l'utilizzo della rete.  
  
Tuttavia, una delle sfide amministrativa evidenziata per la topologia hub-spoke in versioni precedenti del sistema operativo Windows Server è che dopo aver aggiunto un nuovo controller di dominio testa di ponte nell'hub, non esiste alcun meccanismo per ridistribuire le connessioni di replica tra i controller di dominio di succursale e i controller di dominio hub di sfruttare il nuovo controller di dominio hub automatica.  
  
Per i controller di dominio Windows Server 2003, è possibile ribilanciare il carico di lavoro utilizzando uno strumento come Adlb.exe da Windows Server 2003 Branch Office Deployment Guide ([https://go.microsoft.com/fwlink/?LinkID=28523](https://go.microsoft.com/fwlink/?LinkID=28523)).  
  
Per Windows Server 2008 RODC, normale funzionamento di KCC fornisce alcuni ribilanciamento, che elimina la necessità di usare uno strumento aggiuntivo, ad esempio Adlb.exe. La nuova funzionalità è abilitata per impostazione predefinita. È possibile disabilitarlo aggiungendo la seguente chiave del Registro di sistema impostare tale controller di dominio:  
  
**HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\NTDS\Parameters.**  
  
**"Orario di ufficio casuale Loadbalancing consentito"**  
**1 = attivata (impostazione predefinita), 0 = disattivata**  
  
Per ulteriori informazioni sul funzionano di questi miglioramenti di controllo di coerenza informazioni, vedere pianificazione e distribuzione dei servizi di dominio Active Directory per le succursali ([https://go.microsoft.com/fwlink/?LinkId=107114](https://go.microsoft.com/fwlink/?LinkId=107114)).  
  
## <a name="BKMK_3"></a>Funzionalità di failover  
Siti di assicurarsi che la replica viene indirizzata a errori di rete e controller di dominio non in linea. KCC eseguito a intervalli specificati per regolare la topologia di replica per le modifiche apportate in Active Directory, ad esempio quando vengono aggiunti nuovi controller di dominio e nuovi siti vengono creati. KCC esamina lo stato della replica delle connessioni esistenti per determinare se tutte le connessioni non funzionano. Se non funziona una connessione a causa di un controller di dominio non riuscita, il controllo di coerenza informazioni crea automaticamente connessioni temporanee per altri partner di replica (se disponibile) per garantire che la replica si verifica. Se tutti i controller di dominio in un sito non sono disponibili, KCC crea automaticamente le connessioni di replica tra controller di dominio da un altro sito.  
  
## <a name="BKMK_4"></a>Subnet  
Una subnet è un segmento di rete TCP/IP a cui sono assegnati un set di indirizzi IP logici. Subnet raggruppano i computer in modo che la prossimità fisica sulla rete. Gli oggetti subnet in Active Directory identificano gli indirizzi di rete utilizzate per mappare i computer ai siti.  
  
## <a name="BKMK_5"></a>Sito  
I siti sono oggetti di Active Directory che rappresentano una o più subnet TCP/IP con connessioni di rete affidabili e veloce. Informazioni sul sito consente agli amministratori di configurare l'accesso ad Active Directory e la replica per ottimizzare l'utilizzo della rete fisica. Gli oggetti del sito sono associati a un set di subnet e ogni controller di dominio in una foresta è associata a un sito di Active Directory in base al relativo indirizzo IP. I siti possono ospitare controller di dominio da più di un dominio e un dominio può essere rappresentato in più di un sito.  
  
## <a name="BKMK_6"></a>Collegamento di sito  
I collegamenti di sito sono oggetti di Active Directory che rappresentano i percorsi logici che usa il controllo di coerenza informazioni per stabilire una connessione per la replica di Active Directory. Un oggetto collegamento di sito rappresenta un set di siti in grado di comunicare tra loro mediante un trasporto tra siti specificato.  
  
Tutti i siti contenuti nel collegamento di sito sono considerati deve essere connessa tramite lo stesso tipo di rete. I siti devono essere collegati manualmente ad altri siti utilizzando i collegamenti di sito in modo che i controller di dominio in un sito possono replicare le modifiche di directory dal controller di dominio in un altro sito. Poiché i collegamenti di sito non corrispondono al percorso effettivo seguito dai pacchetti di rete sulla rete fisica durante la replica, non è necessario creare collegamenti di sito ridondanti per migliorare l'efficienza della replica di Active Directory.  
  
Quando due siti sono connessi tramite un collegamento di sito, il sistema di replica crea automaticamente le connessioni tra controller di dominio specifico in ogni sito sono denominati server testa di ponte. In Windows Server 2008, tutti i controller di dominio in un sito che ospitano stessa partizione di directory sono candidati per la selezione come server testa di ponte. Le connessioni di replica create da KCC vengono distribuite in modo casuale tra tutti i server testa di ponte candidato in un sito di condividere il carico di lavoro di replica. Per impostazione predefinita, la selezione casuale processo avviene una sola volta, quando gli oggetti connessione vengono aggiunti prima di tutto al sito.  
  
## <a name="BKMK_7"></a>Ponte di collegamenti di sito  
Un ponte di collegamenti di sito è un oggetto di Active Directory che rappresenta un set di collegamenti di sito, tutti i siti del quale può comunicare con un tipo di trasporto comune. Ponti di collegamenti di sito abilitare i controller di dominio che non sono direttamente connessi tramite un collegamento di comunicazione per la replica tra loro. In genere, un ponte di collegamenti di sito corrisponde a un router (o un set di router) su una rete IP.  
  
Per impostazione predefinita, il controllo di coerenza informazioni possa formare una route transitiva attraverso tutti i collegamenti che hanno in comune alcuni siti. Se questo comportamento è disabilitato, ciascun collegamento di sito rappresenta la propria rete distinti e isolato. Set di collegamenti di sito che possono essere considerati come una singola route sono espressi tramite un ponte di collegamenti di sito. Ogni bridge rappresenta un ambiente isolato comunicazione per il traffico di rete.  
  
Ponti di collegamenti di sito sono un meccanismo per rappresentare logicamente connettività fisica transitiva tra siti. Un ponte di collegamenti di sito consente di KCC utilizzare qualsiasi combinazione di collegamenti di sito incluso per determinare la route meno costosa per interconnettere componenti partizioni di directory contenute in tali siti. Il ponte di collegamenti di sito non fornisce la connettività effettiva nei controller di dominio. Se il ponte di collegamenti di sito viene rimosso, la replica su collegamenti di sito combinata continuerà fino a quando non KCC rimuove i collegamenti.  
  
Ponti di collegamenti di sito sono necessari solo se un sito contiene un controller di dominio che ospita una partizione di directory che non è ospitata anche in un controller di dominio in un sito adiacente, ma un controller di dominio che ospita tale partizione di directory si trova in uno o più siti nella foresta. Nei siti adiacenti sono definiti come due o più siti inclusi in un singolo collegamento di sito.  
  
Un ponte di collegamenti di sito crea una connessione logica tra due collegamenti di sito, fornendo un percorso transitivo tra due siti non connessi tramite un sito intermedio. Per quanto riguarda il generatore della topologia tra siti (ISTG), il bridge implica connettività fisica tramite il sito intermedio. Il bridge non implica che un controller di dominio nel sito intermedio fornirà il percorso di replica. Tuttavia, questo sarebbe valere se il sito provvisorio conteneva un controller di dominio che ospita la partizione di directory da replicare, nel qual caso un ponte di collegamenti di sito non è necessario.  
  
Viene aggiunto il costo di ciascun collegamento di sito, la creazione di un costo totale del percorso risultante. Il ponte di collegamenti di sito viene utilizzato se il sito intermedio non contiene un controller di dominio che ospita la partizione di directory e un collegamento di costo inferiore non esiste. Se il sito provvisorio conteneva un controller di dominio che ospita la partizione di directory, due siti disconnessi sarebbero configurare le connessioni di replica al controller di dominio provvisorie e non usano il bridge.  
  
## <a name="BKMK_8"></a>Transitività dei collegamenti di sito  
Per impostazione predefinita i collegamenti di sito sono transitive o "bridging". Quando sono collegati tramite bridge i collegamenti di sito e le pianificazioni si sovrappongono, KCC crea connessioni di replica che determinano il partner di replica di controller di dominio tra siti, in cui i siti non connessi direttamente dai collegamenti di sito ma sono connessi in modo transitivo tramite un set di siti comuni. Ciò significa che è possibile connettersi qualsiasi sito per qualsiasi altro sito tramite una combinazione di collegamenti di sito.  
  
In generale, per una rete completamente instradata, non devi creare qualsiasi sito ponti di collegamenti a meno che non si desidera controllare il flusso delle modifiche di replica. Se la rete non è completamente indirizzata, ponti di collegamenti di sito devono essere creati per evitare la replica Impossibile tentativi. I collegamenti di sito per un determinato trasporto in modo implicito appartengano a un ponte di collegamenti di sito per tale tipo di trasporto. Il bridging predefinito per i collegamenti di sito viene automaticamente, e nessun oggetto di Active Directory rappresenta tale bridge. Il **Bridge i collegamenti di sito** impostazione nelle proprietà del Simple Mail Transfer Protocol (SMTP) sia IP contenitori trasporto tra siti, implementa il bridging collegamenti di sito automatica.  
  
> [!NOTE]  
> La replica SMTP non sarà supportata nelle versioni future di servizi di dominio Active Directory; di conseguenza, la creazione di oggetti di collegamenti di sito nel contenitore SMTP non è consigliata.  
  
## <a name="BKMK_9"></a>Server di catalogo globale  
Un server di catalogo globale è un controller di dominio che contiene informazioni su tutti gli oggetti della foresta, in modo che le applicazioni possono eseguire una ricerca di dominio Active Directory senza fare riferimento ai controller di dominio specifico che archiviano i dati richiesti. Come tutti i controller di dominio, server di catalogo globale archivia repliche completa e scrivibile delle partizioni di directory dello schema e la configurazione e una replica completa e scrivibile della partizione di directory dominio per il dominio di hosting. Inoltre, un server di catalogo globale archivia una replica parziale di sola lettura di ogni altro dominio nella foresta. Le repliche di dominio parziali di sola lettura contengono tutti gli oggetti nel dominio ma solo un sottoinsieme degli attributi (tali attributi più comunemente utilizzati per la ricerca dell'oggetto).  
  
## <a name="BKMK_10"></a>Caching appartenenza a gruppo universale  
Caching appartenenza a gruppo universale consente il controller di dominio per informazioni sull'appartenenza a gruppi universali di cache per gli utenti. È possibile abilitare i controller di dominio che eseguono Windows Server 2008 per l'appartenenza a gruppi universali di cache utilizzando i siti di Active Directory e snap-in servizi.  
  
Abilitare la memorizzazione nella cache di appartenenza a gruppo universale Elimina la necessità di un server di catalogo globale in ogni sito in un dominio, riducendo al minimo l'utilizzo della larghezza di banda di rete perché un controller di dominio non è necessario replicare tutti gli oggetti che si trovano nella foresta. Riduce inoltre tempi di accesso perché i controller di dominio che esegue l'autenticazione non è sempre necessario accedere a un catalogo globale per ottenere informazioni sull'appartenenza a gruppi universali. Per ulteriori informazioni su quando usare la memorizzazione nella cache di appartenenza a gruppi universali, vedere [pianificazione posizionamento del Server di catalogo globale](../../../ad-ds/plan/Planning-Global-Catalog-Server-Placement.md).  
  


