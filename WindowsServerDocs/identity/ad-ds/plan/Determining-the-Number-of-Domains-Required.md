---
ms.assetid: 87bca912-b912-4bbe-9533-2c34a7abc52d
title: Determinazione del numero di domini necessari
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/08/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: e5d6369ca91c1d48375c22238d1e706576df7fbf
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59843522"
---
# <a name="determining-the-number-of-domains-required"></a>Determinazione del numero di domini necessari

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Ogni foresta inizia con un singolo dominio. Il numero massimo di utenti che può contenere una foresta a dominio singolo è basato sul collegamento più lento che deve supportare la replica tra controller di dominio e la larghezza di banda disponibile da allocare in Active Directory Domain Services (AD DS). La tabella seguente elenca il numero massimo consigliato di utenti che può contenere un dominio in base a una foresta a dominio singolo, la velocità del collegamento più lento e la percentuale della larghezza di banda che si desidera riservare per la replica. Queste informazioni si applicano a insiemi di strutture che contengono un massimo di 100.000 utenti e che hanno una connettività di 28,8 kilobit al secondo (Kbps) o versione successiva. Per le raccomandazioni applicabili a insiemi di strutture che contengono più di 100.000 utenti o connettività di minore a 28,8 Kbps, consultare un'esperto progettazione di Active Directory. I valori nella tabella seguente sono basati sul traffico di replica generato in un ambiente che presenta le caratteristiche seguenti:  
  
- Nuovi utenti inseriti nella foresta a una tariffa del 20% ogni anno.  
- Gli utenti lasciano l'insieme di strutture con una frequenza di 15% ogni anno.  
- Ogni utente è membro di cinque gruppi globali e cinque gruppi universali.  
- Il rapporto tra utenti e computer è 1:1.  
- Viene utilizzato Active Directory-integrata sistema DNS (Domain Name).  
- Viene usato lo scavenging DNS.  

> [!NOTE]  
> Le figure elencate nella tabella seguente sono approssimazioni. La quantità di traffico di replica dipende in larga misura il numero di modifiche apportate alla directory in un determinato periodo di tempo. Verificare che la rete può supportare il traffico di replica testando la quantità stimata e frequenza delle modifiche nella progettazione di in un laboratorio prima di distribuire i tuoi domini.  
  
|Collegamento più lento con un controller di dominio (Kbps)|Numero massimo di utenti se dell'1% della larghezza di banda disponibile|Numero massimo di utenti se al 5 percento della larghezza di banda disponibile|Numero massimo di utenti se il 10% della larghezza di banda disponibile|  
| --- | --- | --- | --- |  
|28.8|10,000|25,000|40,000|  
|32|10,000|25,000|50,000|  
|56|10,000|50,000|100,000|  
|64|10,000|50,000|100,000|  
|128|25,000|100,000|100,000|  
|256|50,000|100,000|100,000|  
|512|80,000|100,000|100,000|  
|1,500|100,000|100,000|100,000|  

Per usare questa tabella:  

1. Nel **collegamento più lento, ci si connette a un controller di dominio** colonna, individuare il valore che corrisponde alla velocità del collegamento più lento in cui Active Directory Domain Services eseguirà la replica nel dominio.  

2. Nella riga che corrisponde alla velocità di collegamento più lenta, individuare la colonna che rappresenta la percentuale larghezza di banda che si desidera allocare per Active Directory Domain Services. Il valore in tale posizione è il numero massimo di utenti che può contenere il dominio in una foresta a dominio singolo.  

Se si determina che il numero totale di utenti nella foresta è inferiore al numero massimo di utenti che può contenere il dominio, è possibile usare un singolo dominio. Assicurarsi di soddisfare per la crescita futura prevista quando si effettua questa operazione. Se si determina che il numero totale di utenti nella foresta sia maggiore del numero massimo di utenti che può contenere il dominio, ti serve per riservare una percentuale più elevata di larghezza di banda per la replica, aumentare la velocità di collegamento o dividere l'organizzazione in domini regionali.  
  
## <a name="dividing-the-organization-into-regional-domains"></a>Suddivisione dell'organizzazione in domini regionali

Se si non è possibile gestire tutti gli utenti in un singolo dominio, è necessario selezionare il modello di dominio regionale. Dividere l'organizzazione in aree geografiche in modo appropriato per l'organizzazione e la rete esistente. Ad esempio, è possibile creare aree di base ai limiti di continente.  
  
Si noti che poiché è necessario creare un dominio di Active Directory per ogni area in cui viene stabilita, è consigliabile ridurre al minimo il numero di aree geografiche definite per Active Directory Domain Services. Sebbene sia possibile includere un numero illimitato di domini in una foresta, per una migliore gestibilità è consigliabile che una foresta Includi domini non più di 10. È necessario stabilire l'equilibrio ottimale tra ottimizzazione della larghezza di banda della replica e riducendo al minimo la complessità amministrativa quando si divide l'organizzazione in domini regionali.  
  
In primo luogo, determinare il numero massimo di utenti che possono ospitare l'insieme di strutture. Basare sul collegamento più lento dell'insieme di strutture in replicherà quali controller di dominio e la quantità media di larghezza di banda che si desidera allocare per la replica di Active Directory. La tabella seguente elenca il numero massimo consigliato di utenti che possono contenere un insieme di strutture. Si basa sulla velocità di collegamento più lento e la larghezza di banda di percentuale che si desidera riservare per la replica. Queste informazioni si applicano a insiemi di strutture che contengono un massimo di 100.000 utenti e che hanno una connettività di a 28,8 Kbps o versione successiva. I valori nella tabella riportata di seguito si basano sui seguenti presupposti:  

- Tutti i controller di dominio sono i server di catalogo globale.  
- Nuovi utenti inseriti nella foresta a una tariffa del 20% ogni anno.  
- Gli utenti lasciano l'insieme di strutture con una frequenza di 15% ogni anno.  
- Gli utenti sono membri di gruppi globali di cinque e cinque gruppi universali.  
- Il rapporto tra utenti e computer è 1:1.  
- DNS di Active Directory-integrata viene usato.  
- Viene usato lo scavenging DNS.  

> [!NOTE]  
> Le figure elencate nella tabella seguente sono approssimazioni. La quantità di traffico di replica dipende in larga misura il numero di modifiche apportate alla directory in un determinato periodo di tempo. Verificare che la rete può supportare il traffico di replica testando la quantità stimata e frequenza delle modifiche nella progettazione di in un laboratorio prima di distribuire i tuoi domini.  
  
|Collegamento più lento con un controller di dominio (Kbps)|Numero massimo di utenti se dell'1% della larghezza di banda disponibile|Numero massimo di utenti se al 5 percento della larghezza di banda disponibile|Numero massimo di utenti se il 10% della larghezza di banda disponibile|  
| --- | --- | --- | --- |  
|28.8|10,000|50,000|75,000|  
|32|10,000|50,000|75,000|  
|56|10,000|75,000|100,000|  
|64|25,000|75,000|100,000|  
|128|50,000|100,000|100,000|  
|256|75,000|100,000|100,000|  
|512|100,000|100,000|100,000|  
|1,500|100,000|100,000|100,000|  

Per usare questa tabella:  

1. Nel **collegamento più lento, ci si connette a un controller di dominio** colonna, individuare il valore che corrisponde alla velocità del collegamento più lento in cui Active Directory Domain Services eseguirà la replica dell'insieme di strutture.  

2. Nella riga che corrisponde alla velocità di collegamento più lenta, individuare la colonna che rappresenta la larghezza di banda di percentuale che vuoi allocare per Active Directory Domain Services. Il valore in tale posizione è il numero massimo di utenti che possono ospitare l'insieme di strutture.  

Se il numero massimo di utenti che possono ospitare l'insieme di strutture è maggiore del numero di utenti che è necessario ospitare, una singola foresta funzionerà per la progettazione. Se è necessario per l'hosting di più utenti che il numero massimo che è identificato, è necessario per aumentare la velocità di collegamento minimo, allocare una percentuale maggiore di larghezza di banda per Active Directory Domain Services oppure distribuire altre foreste.  

Se si determina che una singola foresta permetteranno il numero di utenti che è necessario ospitare, il passaggio successivo è per determinare il numero massimo di utenti in grado di supportare ogni area in base al collegamento più lento che si trova in tale area. Dividere la foresta in aree che hanno un significato all'utente. Assicurarsi di basare la decisione su un elemento che non è possibile che cambino. Ad esempio, usare continenti invece di aree di vendita. Queste aree verrà utilizzato come base della struttura di dominio dopo aver identificato il numero massimo di utenti.  

Determinare il numero di utenti che devono essere ospitati in ogni area e quindi verificare che non superano il massimo consentito di base della velocità di collegamento più lenta e la larghezza di banda allocata a Active Directory Domain Services in tale area. La tabella seguente elenca il numero massimo consigliato di utenti che può contenere un dominio a livello di area. Si basa la velocità del collegamento più lento e la percentuale di larghezza di banda da riservare per la replica. Queste informazioni si applicano a insiemi di strutture che contengono un massimo di 100.000 utenti e che hanno una connettività di a 28,8 Kbps o versione successiva. I valori nella tabella riportata di seguito si basano sui seguenti presupposti:  

- Tutti i controller di dominio sono i server di catalogo globale.  
- Nuovi utenti inseriti nella foresta a una tariffa del 20% ogni anno.  
- Gli utenti lasciano l'insieme di strutture con una frequenza di 15% ogni anno.  
- Gli utenti sono membri di gruppi globali di cinque e cinque gruppi universali.  
- Il rapporto tra utenti e computer è 1:1.  
- DNS di Active Directory-integrata viene usato.  
- Viene usato lo scavenging DNS.  
  
> [!NOTE]  
> Le figure elencate nella tabella seguente sono approssimazioni. La quantità di traffico di replica dipende in larga misura il numero di modifiche apportate alla directory in un determinato periodo di tempo. Verificare che la rete può supportare il traffico di replica testando la quantità stimata e frequenza delle modifiche nella progettazione di in un laboratorio prima di distribuire i tuoi domini.  
  
|Collegamento più lento con un controller di dominio (Kbps)|Numero massimo di utenti se dell'1% della larghezza di banda disponibile|Numero massimo di utenti se al 5 percento della larghezza di banda disponibile|Numero massimo di utenti se il 10% della larghezza di banda disponibile|  
| --- | --- | --- | --- |  
|28.8|10,000|18,000|40,000|  
|32|10,000|20,000|50,000|  
|56|10,000|40,000|100,000|  
|64|10,000|50,000|100,000|  
|128|15,000|100,000|100,000|  
|256|30,000|100,000|100,000|  
|512|80,000|100,000|100,000|  
|1,500|100,000|100,000|100,000|  

Per usare questa tabella:  

1. Nel **collegamento più lento, ci si connette a un controller di dominio** colonna, individuare il valore corrispondente la velocità del collegamento più lento in cui Active Directory Domain Services eseguirà la replica nell'area dell'utente.  

2. Nella riga che corrisponde alla velocità di collegamento più lenta, individuare la colonna che rappresenta la larghezza di banda di percentuale che vuoi allocare per Active Directory Domain Services. Tale valore rappresenta il numero massimo di utenti che può ospitare l'area.  

Valutare ogni area proposto e determinare se il numero massimo di utenti in ogni area è inferiore al numero massimo consigliato di utenti che può contenere un dominio. Se si determina che l'area può ospitare il numero di utenti necessari, è possibile creare un dominio per tale area. Se si determina che non è possibile ospitare che molti utenti, è consigliabile dividere la progettazione in aree di dimensioni ridotte e ricalcolare il numero massimo di utenti che possono essere ospitati in ogni area. Le altre alternative sono per allocare larghezza di banda superiore o aumentare la velocità del collegamento.  

Anche se è inferiore al numero di utenti nel dominio in una foresta a dominio singolo il numero totale di utenti che è possibile inserire in un dominio in una foresta con più domini, il numero complessivo di utenti nella foresta con più domini può essere superiore. Il più piccolo numero di utenti per ogni dominio in una foresta con più domini alcune variazioni per il sovraccarico di replica aggiuntivo creato tramite la gestione di catalogo globale in tale ambiente. Per le raccomandazioni applicabili a insiemi di strutture che contengono più di 100.000 utenti o connettività di minore a 28,8 Kbps, consultare un'esperto progettazione di Active Directory.  
  
## <a name="documenting-the-regions-identified"></a>Documentare le aree geografiche identificati

Dopo che l'organizzazione viene suddiviso in domini regionali, documento rappresentate le aree desiderate e il numero di utenti che saranno disponibili in ogni area. Si noti, inoltre, la velocità dei collegamenti più lente in ogni area che si utilizzerà per la replica di Active Directory. Queste informazioni consente di determinare se sono necessari altri domini o foreste.  

Scaricare Job_Aids_Designing_and_Deploying_Directory_and_Security_Services.zip da una cartella di lavoro agevolare così a documentare le aree sono identificati [processo di supporto per Windows Server 2003 Deployment Kit](https://go.microsoft.com/fwlink/?LinkID=102558) e aprire " Identificazione delle regioni"(DSSLOGI_4.doc).  
