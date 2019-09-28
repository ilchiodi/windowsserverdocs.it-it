---
ms.assetid: 87bca912-b912-4bbe-9533-2c34a7abc52d
title: Determinazione del numero di domini necessari
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/08/2018
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: 87214178fdda0cd70c79aed2e46e056deecb6291
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71402606"
---
# <a name="determining-the-number-of-domains-required"></a>Determinazione del numero di domini necessari

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Ogni foresta inizia con un singolo dominio. Il numero massimo di utenti che possono essere contenuti in una foresta a dominio singolo si basa sul collegamento più lento che deve contenere la replica tra i controller di dominio e la larghezza di banda disponibile che si desidera allocare per Active Directory Domain Services (AD DS). La tabella seguente elenca il numero massimo di utenti consigliato che un dominio può contenere in base a una foresta a dominio singolo, la velocità del collegamento più lento e la percentuale di larghezza di banda che si vuole riservare per la replica. Queste informazioni si applicano alle foreste che contengono un massimo di 100.000 utenti e che hanno una connettività di 28,8 kilobit al secondo (Kbps) o superiore. Per indicazioni applicabili alle foreste che contengono più di 100.000 utenti o a una connettività inferiore a 28,8 Kbps, consultare un Active Directory designer esperto. I valori nella tabella seguente sono basati sul traffico di replica generato in un ambiente con le caratteristiche seguenti:  
  
- I nuovi utenti si aggiungono alla foresta a una tariffa del 20% all'anno.  
- Gli utenti lasciano la foresta con una frequenza del 15% all'anno.  
- Ogni utente è membro di cinque gruppi globali e cinque gruppi universali.  
- Il rapporto tra utenti e computer è 1:1.  
- Viene utilizzato Active Directory Integrated Domain Name System (DNS).  
- Viene utilizzato lo scavenging DNS.  

> [!NOTE]  
> Le figure elencate nella tabella seguente sono approssimazioni. La quantità di traffico di replica dipende in gran parte dal numero di modifiche apportate alla directory in un determinato periodo di tempo. Verificare che la rete sia in grado di gestire il traffico di replica testando la quantità stimata e la frequenza delle modifiche apportate alla progettazione in un Lab prima di distribuire i domini.  
  
|Collegamento più lento alla connessione di un controller di dominio (Kbps)|Numero massimo di utenti se è disponibile una larghezza di banda di 1%|Numero massimo di utenti se è disponibile la larghezza di banda del 5%|Numero massimo di utenti se è disponibile una larghezza di banda di 10%|  
| --- | --- | --- | --- |  
|28,8|10,000|25.000|40.000|  
|32|10,000|25.000|50,000|  
|56|10,000|50,000|100.000|  
|64|10,000|50,000|100.000|  
|128|25.000|100.000|100.000|  
|256|50,000|100.000|100.000|  
|512|80.000|100.000|100.000|  
|1,500|100.000|100.000|100.000|  

Per usare questa tabella:  

1. Nel **collegamento più lento che connette una** colonna del controller di dominio, individuare il valore che corrisponde alla velocità del collegamento più lento tra i servizi di dominio Active Directory che vengono replicati nel dominio.  

2. Nella riga che corrisponde alla velocità di collegamento più lenta, individuare la colonna che rappresenta la larghezza di banda percentuale che si desidera allocare ai servizi di dominio Active Directory. Il valore in tale posizione è il numero massimo di utenti che il dominio in una foresta a dominio singolo può contenere.  

Se si determina che il numero totale di utenti nella foresta è inferiore al numero massimo di utenti che il dominio può contenere, è possibile utilizzare un singolo dominio. Assicurarsi di gestire la crescita futura pianificata quando si esegue questa determinazione. Se si determina che il numero totale di utenti nella foresta è maggiore del numero massimo di utenti che il dominio può contenere, è necessario riservare una percentuale più elevata di larghezza di banda per la replica, aumentare la velocità di collegamento o dividere l'organizzazione in domini regionali.  
  
## <a name="dividing-the-organization-into-regional-domains"></a>Suddivisione dell'organizzazione in domini regionali

Se non è possibile gestire tutti gli utenti in un singolo dominio, è necessario selezionare il modello di dominio a livello di area. Suddividere l'organizzazione in aree in modo appropriato per l'organizzazione e la rete esistente. Ad esempio, è possibile creare aree basate su confini continentali.  
  
Si noti che poiché è necessario creare un dominio di Active Directory per ogni area stabilita, è consigliabile ridurre al minimo il numero di aree definite per servizi di dominio Active Directory. Sebbene sia possibile includere un numero illimitato di domini in una foresta, per gestibilità è consigliabile che una foresta includa non più di 10 domini. È necessario stabilire il giusto equilibrio tra l'ottimizzazione della larghezza di banda di replica e la riduzione della complessità amministrativa quando si divide l'organizzazione in domini regionali.  
  
Per prima cosa, determinare il numero massimo di utenti che la foresta può ospitare. Basare questa funzione sul collegamento più lento nella foresta in cui vengono replicati i controller di dominio e la quantità media di larghezza di banda che si desidera allocare per Active Directory la replica. La tabella seguente elenca il numero massimo di utenti consigliato che possono essere contenuti in una foresta. Questa operazione si basa sulla velocità del collegamento più lento e sulla larghezza di banda che si vuole riservare per la replica. Queste informazioni si applicano a foreste contenenti un massimo di 100.000 utenti e con una connettività di 28,8 Kbps o superiore. I valori nella tabella seguente sono basati sui presupposti seguenti:  

- Tutti i controller di dominio sono server di catalogo globale.  
- I nuovi utenti si aggiungono alla foresta a una tariffa del 20% all'anno.  
- Gli utenti lasciano la foresta con una frequenza del 15% all'anno.  
- Gli utenti sono membri di cinque gruppi globali e cinque gruppi universali.  
- Il rapporto tra utenti e computer è 1:1.  
- Viene usato il DNS integrato in Active Directory.  
- Viene utilizzato lo scavenging DNS.  

> [!NOTE]  
> Le figure elencate nella tabella seguente sono approssimazioni. La quantità di traffico di replica dipende in gran parte dal numero di modifiche apportate alla directory in un determinato periodo di tempo. Verificare che la rete sia in grado di gestire il traffico di replica testando la quantità stimata e la frequenza delle modifiche apportate alla progettazione in un Lab prima di distribuire i domini.  
  
|Collegamento più lento alla connessione di un controller di dominio (Kbps)|Numero massimo di utenti se è disponibile una larghezza di banda di 1%|Numero massimo di utenti se è disponibile la larghezza di banda del 5%|Numero massimo di utenti se è disponibile una larghezza di banda di 10%|  
| --- | --- | --- | --- |  
|28,8|10,000|50,000|75.000|  
|32|10,000|50,000|75.000|  
|56|10,000|75.000|100.000|  
|64|25.000|75.000|100.000|  
|128|50,000|100.000|100.000|  
|256|75.000|100.000|100.000|  
|512|100.000|100.000|100.000|  
|1,500|100.000|100.000|100.000|  

Per usare questa tabella:  

1. Nel **collegamento più lento che connette una** colonna del controller di dominio individuare il valore che corrisponde alla velocità del collegamento più lento tra i servizi di dominio Active Directory che vengono replicati nella foresta.  

2. Nella riga che corrisponde alla velocità di collegamento più lenta, individuare la colonna che rappresenta la larghezza di banda percentuale che si desidera allocare ai servizi di dominio Active Directory. Il valore in tale posizione è il numero massimo di utenti che la foresta può ospitare.  

Se il numero massimo di utenti che la foresta può ospitare è maggiore del numero di utenti che è necessario ospitare, una singola foresta funzionerà per la progettazione. Se è necessario ospitare più utenti rispetto al numero massimo identificato, è necessario aumentare la velocità minima del collegamento, allocare una maggiore percentuale di larghezza di banda per servizi di dominio Active Directory o distribuire foreste aggiuntive.  

Se si determina che una singola foresta includerà il numero di utenti che è necessario ospitare, il passaggio successivo consiste nel determinare il numero massimo di utenti che ciascuna area può supportare in base al collegamento più lento presente in tale area. Dividere la foresta in aree che hanno senso. Assicurarsi di basare la decisione su un elemento che probabilmente non cambierà. Usare, ad esempio, continenti anziché aree di vendita. Queste aree saranno la base della struttura del dominio dopo aver identificato il numero massimo di utenti.  

Determinare il numero di utenti che devono essere ospitati in ogni area, quindi verificare che non superino il valore massimo consentito in base alla velocità di collegamento più lenta e alla larghezza di banda allocata per servizi di dominio Active Directory in tale area. La tabella seguente elenca il numero massimo di utenti consigliato che un dominio regionale può contenere. Si basa sulla velocità del collegamento più lento e sulla percentuale di larghezza di banda che si vuole riservare per la replica. Queste informazioni si applicano a foreste contenenti un massimo di 100.000 utenti e con una connettività di 28,8 Kbps o superiore. I valori nella tabella seguente sono basati sui presupposti seguenti:  

- Tutti i controller di dominio sono server di catalogo globale.  
- I nuovi utenti si aggiungono alla foresta a una tariffa del 20% all'anno.  
- Gli utenti lasciano la foresta con una frequenza del 15% all'anno.  
- Gli utenti sono membri di cinque gruppi globali e cinque gruppi universali.  
- Il rapporto tra utenti e computer è 1:1.  
- Viene usato il DNS integrato in Active Directory.  
- Viene utilizzato lo scavenging DNS.  
  
> [!NOTE]  
> Le figure elencate nella tabella seguente sono approssimazioni. La quantità di traffico di replica dipende in gran parte dal numero di modifiche apportate alla directory in un determinato periodo di tempo. Verificare che la rete sia in grado di gestire il traffico di replica testando la quantità stimata e la frequenza delle modifiche apportate alla progettazione in un Lab prima di distribuire i domini.  
  
|Collegamento più lento alla connessione di un controller di dominio (Kbps)|Numero massimo di utenti se è disponibile una larghezza di banda di 1%|Numero massimo di utenti se è disponibile la larghezza di banda del 5%|Numero massimo di utenti se è disponibile una larghezza di banda di 10%|  
| --- | --- | --- | --- |  
|28,8|10,000|18.000|40.000|  
|32|10,000|20.000|50,000|  
|56|10,000|40.000|100.000|  
|64|10,000|50,000|100.000|  
|128|15.000|100.000|100.000|  
|256|30.000|100.000|100.000|  
|512|80.000|100.000|100.000|  
|1,500|100.000|100.000|100.000|  

Per usare questa tabella:  

1. Nel **collegamento più lento che connette una** colonna del controller di dominio individuare il valore che corrisponde alla velocità del collegamento più lento tra i servizi di dominio Active Directory e la replica nella propria area.  

2. Nella riga che corrisponde alla velocità di collegamento più lenta, individuare la colonna che rappresenta la larghezza di banda percentuale che si desidera allocare ai servizi di dominio Active Directory. Tale valore rappresenta il numero massimo di utenti che l'area può ospitare.  

Valutare ogni area proposta e determinare se il numero massimo di utenti in ogni area è inferiore al numero massimo consigliato di utenti che un dominio può contenere. Se si determina che l'area può ospitare il numero di utenti necessari, è possibile creare un dominio per tale area. Se si stabilisce che non è possibile ospitare molti utenti, è consigliabile suddividere la progettazione in aree più piccole e ricalcolare il numero massimo di utenti che possono essere ospitati in ogni area. Le altre alternative consentono di allocare una maggiore larghezza di banda o di aumentare la velocità di collegamento.  

Anche se il numero totale di utenti che è possibile inserire in un dominio in una foresta con più domini è inferiore al numero di utenti nel dominio in una foresta a dominio singolo, il numero complessivo di utenti nella foresta con più domini può essere superiore. Il minor numero di utenti per dominio in una foresta con più domini si adatta al sovraccarico di replica aggiuntivo creato gestendo il catalogo globale in tale ambiente. Per indicazioni applicabili alle foreste che contengono più di 100.000 utenti o a una connettività inferiore a 28,8 Kbps, consultare un Active Directory designer esperto.  
  
## <a name="documenting-the-regions-identified"></a>Documentazione delle aree identificate

Dopo aver suddiviso l'organizzazione in domini regionali, documentare le aree da rappresentare e il numero di utenti che dovranno esistere in ogni area. Si noti inoltre la velocità dei collegamenti più lenti in ogni area che si utilizzerà per la replica Active Directory. Queste informazioni vengono utilizzate per determinare se sono necessari domini o foreste aggiuntivi.  

Per un foglio di lavoro che assiste l'utente nella documentazione delle aree identificate, scaricare Job_Aids_Designing_and_Deploying_Directory_and_Security_Services. zip da [Job Aids per Windows Server 2003 Deployment Kit](https://go.microsoft.com/fwlink/?LinkID=102558) e aprire "Identification regions" (DSSLOGI _4. doc).  
