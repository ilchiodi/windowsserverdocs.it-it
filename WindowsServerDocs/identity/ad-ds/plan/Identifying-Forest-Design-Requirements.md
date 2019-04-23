---
ms.assetid: 7d957ebb-3476-49d8-b00b-6e93b4a94778
title: Identificazione dei requisiti di progettazione della foresta
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/07/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: bf8d9d164bf07151572785cda906be911f97b53e
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59835332"
---
# <a name="identifying-forest-design-requirements"></a>Identificazione dei requisiti di progettazione della foresta

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Per creare una progettazione di foresta per l'organizzazione, è necessario identificare i requisiti di business che deve contenere la struttura di directory. È quindi necessario stabilire quanti autonomia i gruppi in alle esigenze dell'organizzazione per gestire le risorse di rete e se necessarie per ogni gruppo per isolare le risorse di rete da altri gruppi.  
  
Servizi di dominio Active Directory (AD DS) ti permette di progettare un'infrastruttura di directory che supporta più gruppi all'interno di un'organizzazione che hanno requisiti di gestione specifici e per ottenere l'indipendenza dalla struttura e tra i gruppi in base alle esigenze.  
  
Gruppi nell'organizzazione siano presenti alcuni dei tipi seguenti di requisiti:  
  
-   **Requisiti per la struttura organizzativa**. Parti di un'organizzazione potrebbero far parte di un'infrastruttura condivisa per risparmiare sui costi, ma richiedono la possibilità di funzionare in modo indipendente dal resto dell'organizzazione. Ad esempio, un gruppo di ricerca all'interno di un'organizzazione di grandi dimensioni potrebbe essere necessario mantenere il controllo su tutti i propri dati di ricerca.  
  
-   **I requisiti operativi**. Una parte di un'organizzazione potrebbe inserire vincoli univoci per la configurazione del servizio directory, disponibilità o sicurezza oppure usare le applicazioni che consentono di posizionare i vincoli unique nella directory. Singole business unit all'interno di un'organizzazione potrebbe ad esempio, distribuire la directory abilitata le applicazioni che modificano lo schema di directory che non vengono distribuite da altre business unit. Poiché lo schema di directory è condiviso tra tutti i domini nella foresta, la creazione di più foreste è una soluzione per questo scenario. Altri esempi si trovano delle organizzazioni seguenti scenari:  
  
    -   Organizzazioni militari  
  
    -   Scenari di hosting  
  
    -   Organizzazioni che gestiscono una directory in cui è disponibile sia internamente che esternamente (ad esempio quelli che sono pubblicamente accessibili dagli utenti in Internet)  
  
-   **I requisiti legali**. Alcune organizzazioni hanno requisiti legali per operare in modo specifico, ad esempio, limitare l'accesso a determinate informazioni come specificato in un contratto aziendale. Alcune organizzazioni hanno requisiti di sicurezza per il funzionamento in reti isolate interne. Errore a soddisfare questi requisiti possa comportare la perdita del contratto e un'azione legale possibilmente.  
  
Parte di identificare i requisiti di progettazione di foresta implica che identifica il livello a cui i gruppi all'interno dell'organizzazione possono considerare attendibili i potenziali proprietari dell'insieme di strutture e i propri amministratori del servizio e identificare i requisiti di isolamento e autonomia per ogni gruppo di nell'organizzazione.  
  
Il team di progettazione debba documentare i requisiti di isolamento e autonomia per l'amministrazione del servizio e i dati per ogni gruppo di nell'organizzazione che intende utilizzare Active Directory Domain Services. Il team deve tenere inoltre presente le aree di connettività limitata che potrebbero influire sulla distribuzione di Active Directory Domain Services.  
  
Il team di progettazione debba documentare i requisiti di isolamento e autonomia per l'amministrazione del servizio e i dati per ogni gruppo di nell'organizzazione che intende utilizzare Active Directory Domain Services. Il team deve tenere inoltre presente le aree di connettività limitata che potrebbero influire sulla distribuzione di Active Directory Domain Services. Scaricare Job_Aids_Designing_and_Deploying_Directory_and_Security_Services.zip da una cartella di lavoro agevolare così a documentare le aree sono identificati [processo di supporto per Windows Server 2003 Deployment Kit](https://go.microsoft.com/fwlink/?LinkID=102558) e aprire "foresta Requisiti di progettazione"(DSSLOGI_2.doc).  
  
## <a name="in-this-section"></a>Contenuto della sezione  
  
-   [Ambito di amministratore del servizio dell'autorità](../../ad-ds/plan/Service-Administrator-Scope-of-Authority.md)  
  
-   [Visual Studio di autonomia. isolamento](../../ad-ds/plan/Autonomy-vs.-Isolation.md)  
