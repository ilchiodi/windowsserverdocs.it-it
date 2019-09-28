---
ms.assetid: 7d957ebb-3476-49d8-b00b-6e93b4a94778
title: Identificazione dei requisiti di progettazione della foresta
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/07/2018
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: 41caeca82819eaea3d86d5f1eb4883ab8bbf53cc
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71408793"
---
# <a name="identifying-forest-design-requirements"></a>Identificazione dei requisiti di progettazione della foresta

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Per creare una progettazione della foresta per l'organizzazione, è necessario identificare i requisiti aziendali che devono essere inclusi nella struttura di directory. Ciò comporta la determinazione dell'autonomia dei gruppi dell'organizzazione per la gestione delle risorse di rete e dell'eventuale necessità di isolare le risorse in rete da altri gruppi da parte di ogni gruppo.  
  
Active Directory Domain Services (AD DS) consente di progettare un'infrastruttura di directory in grado di supportare più gruppi all'interno di un'organizzazione che hanno requisiti di gestione univoci e di ottenere l'indipendenza strutturale e operativa tra gruppi Se necessario.  
  
I gruppi dell'organizzazione potrebbero avere alcuni dei seguenti tipi di requisiti:  
  
-   **Requisiti della struttura organizzativa**. Parti di un'organizzazione possono partecipare a un'infrastruttura condivisa per ridurre i costi, ma richiedono la possibilità di operare in modo indipendente dal resto dell'organizzazione. Ad esempio, un gruppo di ricerca all'interno di un'organizzazione di grandi dimensioni potrebbe dover mantenere il controllo su tutti i propri dati di ricerca.  
  
-   **Requisiti operativi**. Una parte di un'organizzazione può inserire vincoli univoci sulla configurazione, sulla disponibilità o sulla sicurezza del servizio directory oppure usare applicazioni che inseriscono vincoli univoci sulla directory. Ad esempio, singole unità aziendali all'interno di un'organizzazione possono distribuire applicazioni abilitate alla directory che modificano lo schema di directory che non sono distribuite da altre business unit. Poiché lo schema di directory è condiviso tra tutti i domini della foresta, la creazione di più foreste è una soluzione per tale scenario. Altri esempi sono disponibili nelle organizzazioni e negli scenari seguenti:  
  
    -   Organizzazioni militari  
  
    -   Scenari di hosting  
  
    -   Organizzazioni che gestiscono una directory disponibile sia internamente che esternamente (ad esempio quelle accessibili pubblicamente dagli utenti su Internet)  
  
-   **Requisiti legali**. Alcune organizzazioni hanno requisiti legali per operare in modo specifico, ad esempio, limitando l'accesso a determinate informazioni come specificato in un contratto aziendale. Alcune organizzazioni hanno requisiti di sicurezza per operare su reti interne isolate. La mancata ottemperanza di questi requisiti può comportare la perdita del contratto e di eventuali azioni legali.  
  
Parte dell'identificazione dei requisiti di progettazione della foresta implica l'identificazione del grado di attendibilità dei gruppi dell'organizzazione per i potenziali proprietari della foresta e degli amministratori dei servizi e l'identificazione dei requisiti di autonomia e isolamento per ogni gruppo nell'organizzazione.  
  
Il team di progettazione deve documentare i requisiti di isolamento e autonomia per l'amministrazione dei servizi e dei dati per ogni gruppo dell'organizzazione che intende utilizzare servizi di dominio Active Directory. Il team deve inoltre prendere nota di eventuali aree di connettività limitata che potrebbero influire sulla distribuzione di servizi di dominio Active Directory.  
  
Il team di progettazione deve documentare i requisiti di isolamento e autonomia per l'amministrazione dei servizi e dei dati per ogni gruppo dell'organizzazione che intende utilizzare servizi di dominio Active Directory. Il team deve inoltre prendere nota di eventuali aree di connettività limitata che potrebbero influire sulla distribuzione di servizi di dominio Active Directory. Per un foglio di lavoro che consente di documentare le aree identificate, scaricare Job_Aids_Designing_and_Deploying_Directory_and_Security_Services. zip da [Job Aids per Windows Server 2003 Deployment Kit](https://go.microsoft.com/fwlink/?LinkID=102558) e aprire "Forest Design requirements" ( DSSLOGI_2. doc).  
  
## <a name="in-this-section"></a>Contenuto della sezione  
  
-   [Ambito di autorità dell'amministratore dei servizi](../../ad-ds/plan/Service-Administrator-Scope-of-Authority.md)  
  
-   [Confronto tra autonomia e isolamento](../../ad-ds/plan/Autonomy-vs.-Isolation.md)  
