---
ms.assetid: d04dd17e-a843-46fd-8711-0039918f92d9
title: ADFS di implementazione del piano di progettazione
description: 
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 31c2e048c3c125d0ea60610b049501151d7aa823
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2017
---
# <a name="implementing-your-ad-fs-design-plan"></a>ADFS di implementazione del piano di progettazione

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Le seguenti condizioni ambientali e i requisiti sono fattori importanti nell'implementazione del piano di progettazione \(AD FS\) Active Directory Federation Services:  
  
-   **I partner è supportato:** AD FS in genere utilizzato per lavorare con le organizzazioni partner. Per stabilire la federazione delle identità, determinare le organizzazioni che si desidera utilizzare per formare una relazione. Dopo una distribuzione di ADFS in base AD è attiva, funziona con i partner implica aggiunta partner, partner di eliminazione e aggiornamento delle informazioni per i partner. Modifiche alle collaborazioni possono verificarsi per vari motivi. Ad esempio, la distribuzione di ADFS potrebbe richiedere aggiornamenti relazione se il partner cambia la propria attività in modo significativo, l'organizzazione diventa parte di un'organizzazione più grande o una federazione di organizzazioni o organizzazione verrà acquisita da un'altra società. In qualsiasi scenario in cui effettuare la federazione delle identità di più domini, è necessario conoscere il \(partners\) domini che sono attualmente supportati e tutti i domini aggiuntivi che rappresentano i potenziali partner.  
  
-   **Applicazioni e tipi di servizi supportati:** alcuni servizi e applicazioni richiedono l'accesso alle risorse del sistema operativo, mentre altri sono "grado di riconoscere attestazioni." È importante comprendere i tipi di applicazioni e servizi che supportano ADFS, in modo che è possibile formulare i requisiti di amministrazione.  
  
-   **Diagrammi dell'architettura logici e fisici o topologia di distribuzione:** è necessario conoscere:  
  
    -   Se i server federativi funzionare in un set di server farm o in un singolo server.  
  
    -   La rete in distribuisce firewall e proxy.  
  
    -   Il percorso delle risorse e indica se gli utenti accedono alle risorse all'interno dell'organizzazione, all'esterno dell'organizzazione, o entrambi.  
  
## <a name="how-to-implement-your-ad-fs-design-using-this-guide"></a>Come implementare la progettazione di ADFS usando questa Guida  
Il passaggio successivo dell'implementazione della progettazione consiste nel determinare in quale ordine da eseguire ogni attività di distribuzione. Questa Guida usa elenchi di controllo per aiutarti a scorrere le varie attività di distribuzione di server e dell'applicazione necessari per implementare il piano di progettazione. Elenchi di controllo padre e figlio vengono utilizzati per rappresentare l'ordine in cui attività per una specifica ADFS deve essere elaborata progettazione.  
  
Utilizzare i seguenti elenchi di controllo padre in questa sezione della Guida per acquisire familiarità con le attività di distribuzione per l'implementazione di progettazione di AD FS preferito dell'organizzazione:  
  
-   [Elenco di controllo: Implementazione di un progetto Web SSO](Checklist--Implementing-a-Web-SSO-Design.md)  
  
-   [Elenco di controllo: Implementazione di un progetto Web SSO federativo](Checklist--Implementing-a-Federated-Web-SSO-Design.md)  
