---
ms.assetid: 87bca912-b912-4bbe-9533-2c34a7abc52d
title: Determinazione del numero di domini richiesti
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: b65c792929a29c959244d1da83b4882f15fa2935
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/12/2017
---
# <a name="determining-the-number-of-domains-required"></a>Determinazione del numero di domini richiesti

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Ogni insieme di strutture inizia con un singolo dominio. Il numero massimo di utenti che può contenere una foresta con dominio singolo si basa sul collegamento più lento che deve supportare la replica tra controller di dominio e la larghezza di banda disponibile che si desidera allocare a servizi di dominio Active Directory (AD DS). La tabella seguente elenca il numero massimo consigliato di utenti che può contenere un dominio in base a una foresta con dominio singolo, la velocità del collegamento più lenta e la percentuale di larghezza di banda che si desidera riservare per la replica. Queste informazioni si applicano a insiemi di strutture che contengono un massimo di 100.000 utenti e che dispone di una connettività di 28,8 kilobit al secondo (Kbps) o versione successiva. Per consigli che si applicano a insiemi di strutture che contiene più di 100.000 utenti o connettività di meno di 28,8 Kbps, consultare un'esperto progettazione di Active Directory. I valori nella tabella seguente sono basati sul traffico di replica generato in un ambiente che presenta le seguenti caratteristiche:  
  
-   Nuovi utenti inseriti nella foresta a una velocità di 20% ogni anno.  
  
-   Gli utenti lasciano l'insieme di strutture con una frequenza di 15% ogni anno.  
  
-   Ogni utente è membro di cinque gruppi globali e cinque gruppi universali.  
  
-   Il rapporto tra utenti e computer è 1:1.  
  
-   Viene utilizzato Active Directory integrata sistema DNS (Domain Name).  
  
-   Lo scavenging DNS viene utilizzato.  
  
> [!NOTE]  
> Le figure elencate nella tabella seguente sono approssimazioni. La quantità di traffico di replica dipende in gran parte il numero di modifiche apportate alla directory in una determinata quantità di tempo. Verificare che la rete possa contenere il traffico di replica verificando la quantità stimata e la frequenza delle modifiche alla progettazione in un ambiente prima di distribuire i domini.  
  
|Collegamento più lento con un controller di dominio (Kbps)|Numero massimo di utenti se 1% della larghezza di banda disponibile.|Numero massimo di utenti se del 5% della larghezza di banda disponibile.|Numero massimo di utenti, se il 10% della larghezza di banda disponibile.|  
|--------------------------------------------------------|----------------------------------------------------------------|----------------------------------------------------------------|-----------------------------------------------------------------|  
|28.8|10,000|25,000|40,000|  
|32|10,000|25,000|50,000|  
|56|10,000|50,000|100,000|  
|64|10,000|50,000|100,000|  
|128|25,000|100,000|100,000|  
|256|50,000|100,000|100,000|  
|512|80,000|100,000|100,000|  
|1,500|100,000|100,000|100,000|  
  
Per usare questa tabella:  
  
1.  Nel **collegamento più lenta di un controller di dominio** colonna, individuare il valore che corrisponde la velocità del collegamento più lenta tra cui servizi di dominio Active Directory verranno replicate nel dominio.  
  
2.  Nella riga che corrisponde alla velocità di collegamento più lenta, individuare la colonna che rappresenta la larghezza di banda percentuale che si desidera allocare per servizi di dominio Active Directory. Il valore in tale percorso è il numero massimo di utenti che può contenere il dominio in una foresta con dominio singolo.  
  
Se si determina che il numero totale di utenti della foresta è inferiore al numero massimo di utenti che può contenere il dominio, è possibile utilizzare un singolo dominio. Assicurati di supportare la crescita futura pianificata quando si esegue questa operazione. Se si determina che il numero totale di utenti nella foresta sia maggiore del numero massimo di utenti che può contenere il dominio, è necessario riservare maggiore percentuale di larghezza di banda per la replica, aumentare la velocità del collegamento o suddividere l'organizzazione in domini regionali.  
  
## <a name="dividing-the-organization-into-regional-domains"></a>Suddivisione dell'organizzazione in domini regionali  
Se è possibile gestire tutti gli utenti in un singolo dominio, è necessario selezionare il modello di dominio regionale. Suddividere l'organizzazione in aree geografiche in modo che abbia senso per l'organizzazione e la rete esistente. Ad esempio, è possibile creare le aree geografiche in base ai limiti continentali.  
  
Tieni presente che poiché è necessario creare un dominio di Active Directory per ogni area geografica specificata, è consigliabile ridurre al minimo il numero di aree definite per Active Directory. Sebbene sia possibile includere un numero illimitato di domini in una foresta, per la gestibilità è consigliabile che un insieme di strutture include non più di 10 domini. È necessario stabilire il saldo tra ottimizzare la larghezza di banda replica e riducendo al minimo la complessità amministrativa durante la divisione dell'organizzazione in domini regionali appropriato.  
  
Innanzitutto, determinare il numero massimo di utenti che possono ospitare l'insieme di strutture. Base sul collegamento più lento all'interno della foresta in cui verranno replicate quali controller di dominio e la quantità media di larghezza di banda di cui che si desidera allocare per la replica di Active Directory. La tabella seguente elenca il numero di utenti che può contenere un insieme di strutture massimo consigliato. Questo è in base alla velocità del collegamento più lento e la larghezza di banda percentuale che si desidera riservare per la replica. Queste informazioni si applicano a insiemi di strutture che contengono un massimo di 100.000 utenti e che dispone di una connettività di 28,8 Kbps o versione successiva. I valori nella tabella seguente si basano sui seguenti presupposti:  
  
-   Tutti i controller di dominio sono server di catalogo globale.  
  
-   Nuovi utenti inseriti nella foresta a una velocità di 20% ogni anno.  
  
-   Gli utenti lasciano l'insieme di strutture con una frequenza di 15% ogni anno.  
  
-   Gli utenti sono membri dei gruppi globali di cinque e cinque gruppi universali.  
  
-   Il rapporto tra utenti e computer è 1:1.  
  
-   Viene utilizzato DNS di Active Directory integrata.  
  
-   Lo scavenging DNS viene utilizzato.  
  
> [!NOTE]  
> Le figure elencate nella tabella seguente sono approssimazioni. La quantità di traffico di replica dipende in gran parte il numero di modifiche apportate alla directory in una determinata quantità di tempo. Verificare che la rete possa contenere il traffico di replica verificando la quantità stimata e la frequenza delle modifiche alla progettazione in un ambiente prima di distribuire i domini.  
  
|Collegamento più lento con un controller di dominio (Kbps)|Numero massimo di utenti se 1% della larghezza di banda disponibile.|Numero massimo di utenti se del 5% della larghezza di banda disponibile.|Numero massimo di utenti, se il 10% della larghezza di banda disponibile.|  
|--------------------------------------------------------|----------------------------------------------------------------|----------------------------------------------------------------|-----------------------------------------------------------------|  
|28.8|10,000|50,000|75,000|  
|32|10,000|50,000|75,000|  
|56|10,000|75,000|100,000|  
|64|25,000|75,000|100,000|  
|128|50,000|100,000|100,000|  
|256|75,000|100,000|100,000|  
|512|100,000|100,000|100,000|  
|1,500|100,000|100,000|100,000|  
  
Per usare questa tabella:  
  
1.  Nel **collegamento più lenta di un controller di dominio** colonna, individuare il valore che corrisponde la velocità del collegamento più lenta tra cui servizi di dominio Active Directory verranno replicate nella foresta.  
  
2.  Nella riga che corrisponde alla velocità di collegamento più lenta, individuare la colonna che rappresenta la larghezza di banda percentuale che si desidera allocare a servizi di dominio Active Directory. Il valore in tale percorso è il numero massimo di utenti che possono ospitare l'insieme di strutture.  
  
Se il numero massimo di utenti che possono ospitare l'insieme di strutture è maggiore del numero di utenti che è necessario ospitare, una singola foresta funzioneranno per la progettazione. Se è necessario ospitare più utenti rispetto al numero massimo che sono stati individuati, è necessario per aumentare la velocità di collegamento minimo, allocare una maggiore percentuale di larghezza di banda per Active Directory o distribuire foreste aggiuntive.  
  
Se si determina che una singola foresta, verrà eseguito il numero di utenti che è necessario ospitare, il passaggio successivo consiste nel determinare il numero massimo di utenti in grado di supportare ogni area geografica in base al link più lento che si trova in tale area geografica. Suddividere l'insieme di strutture in aree che rendono sensore all'utente. Assicurarsi che basare la decisione a qualcosa che non è soggette a modifica. Ad esempio, è possibile utilizzare continenti invece di aree di vendita. Dopo avere identificato il numero massimo di utenti, queste aree costituiranno la base della struttura di dominio.  
  
Determinare il numero di utenti che devono essere ospitato in ogni area geografica e quindi verificare che non superano il massimo consentito in base alla velocità del collegamento più lenta e la larghezza di banda allocata a dominio Active Directory in tale area. La tabella seguente elenca il numero di utenti che può contenere un dominio regionale massimo consigliato. Si basa sulla velocità del collegamento più lenta e la percentuale di larghezza di banda da riservare per la replica. Queste informazioni si applicano a insiemi di strutture che contengono un massimo di 100.000 utenti e che dispone di una connettività di 28,8 Kbps o versione successiva. I valori nella tabella seguente si basano sui seguenti presupposti:  
  
-   Tutti i controller di dominio sono server di catalogo globale.  
  
-   Nuovi utenti inseriti nella foresta a una velocità di 20% ogni anno.  
  
-   Gli utenti lasciano l'insieme di strutture con una frequenza di 15% ogni anno.  
  
-   Gli utenti sono membri dei gruppi globali di cinque e cinque gruppi universali.  
  
-   Il rapporto tra utenti e computer è 1:1.  
  
-   Viene utilizzato DNS di Active Directory integrata.  
  
-   Lo scavenging DNS viene utilizzato.  
  
> [!NOTE]  
> Le figure elencate nella tabella seguente sono approssimazioni. La quantità di traffico di replica dipende in gran parte il numero di modifiche apportate alla directory in una determinata quantità di tempo. Verificare che la rete possa contenere il traffico di replica verificando la quantità stimata e la frequenza delle modifiche alla progettazione in un ambiente prima di distribuire i domini.  
  
|Collegamento più lento con un controller di dominio (Kbps)|Numero massimo di utenti se 1% della larghezza di banda disponibile.|Numero massimo di utenti se del 5% della larghezza di banda disponibile.|Numero massimo di utenti, se il 10% della larghezza di banda disponibile.|  
|--------------------------------------------------------|----------------------------------------------------------------|----------------------------------------------------------------|-----------------------------------------------------------------|  
|28.8|10,000|18,000|40,000|  
|32|10,000|20,000|50,000|  
|56|10,000|40,000|100,000|  
|64|10,000|50,000|100,000|  
|128|15,000|100,000|100,000|  
|256|30,000|100,000|100,000|  
|512|80,000|100,000|100,000|  
|1,500|100,000|100,000|100,000|  
  
Per usare questa tabella:  
  
1.  Nel **collegamento più lenta di un controller di dominio** colonna, individuare il valore che corrisponde la velocità del collegamento più lenta tra cui servizi di dominio Active Directory verranno replicate nella tua area geografica.  
  
2.  Nella riga che corrisponde alla velocità di collegamento più lenta, individuare la colonna che rappresenta la larghezza di banda percentuale che si desidera allocare a servizi di dominio Active Directory. Questo valore rappresenta il numero massimo di utenti che può ospitare l'area geografica.  
  
Valutare ogni regione proposta e determinare se il numero massimo di utenti in ogni regione è inferiore al numero massimo consigliato di utenti che può contenere un dominio. Se si determina che l'area geografica può ospitare il numero di utenti necessari, è possibile creare un dominio per quell'area. Determinare se che non è possibile ospitare che molti utenti, è consigliabile dividere la progettazione in aree geografiche più piccole e ricalcolo il numero massimo di utenti che possono essere ospitati in ogni regione. Le altre alternative sono per allocare più larghezza di banda o aumentare la velocità del collegamento.  
  
Anche se il numero totale di utenti che è possibile inserire in un dominio in una foresta con più domini è minore del numero di utenti nel dominio in una foresta con dominio singolo, il numero complessivo di utenti nella foresta multidominio può essere superiore. Il numero minore di utenti per ogni dominio in una foresta con più domini supporta l'overhead di replica aggiuntivo creato tramite la gestione del catalogo globale in tale ambiente. Per consigli che si applicano a insiemi di strutture che contiene più di 100.000 utenti o connettività di meno di 28,8 Kbps, consultare un'esperto progettazione di Active Directory.  
  
## <a name="documenting-the-regions-identified"></a>Documentare le aree identificate  
Dopo che l'organizzazione si divide in domini regionali, documento le aree che si desidera rappresentate e il numero di utenti che saranno disponibili in ogni regione. Si noti inoltre la velocità di collegamento più lenta in ogni regione che verrà utilizzato per la replica di Active Directory. Queste informazioni viene utilizzate per determinare se sono necessari altri domini o foreste.  
  
Per un foglio di lavoro agevolare la documentazione regioni identificati, scaricare Job_Aids_Designing_and_Deploying_Directory_and_Security_Services.zip).  
  


