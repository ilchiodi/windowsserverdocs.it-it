---
ms.assetid: d04dd17e-a843-46fd-8711-0039918f92d9
title: Implementazione del piano di progettazione di AD FS
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: ee822ef94e2723a4ce20e456a5507f572f038a93
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80855414"
---
# <a name="implementing-your-ad-fs-design-plan"></a>Implementazione del piano di progettazione di AD FS

I requisiti e le condizioni ambientali seguenti sono fattori importanti nell'implementazione della Active Directory Federation Services \(AD FS piano di progettazione\):  
  
-   **Partner supportati:** In genere si usa AD FS per collaborare con le organizzazioni partner. Per stabilire la Federazione delle identità, determinare le organizzazioni con cui si desidera creare una relazione. Dopo la distribuzione di una linea di base AD FS, l'utilizzo dei partner comporta l'aggiunta di partner, l'eliminazione di partner e l'aggiornamento delle informazioni sui partner. Le modifiche alle relazioni possono verificarsi per diversi motivi. Ad esempio, la distribuzione di AD FS potrebbe richiedere aggiornamenti della partnership se il partner cambia significativamente l'azienda, l'organizzazione diventa parte di un'organizzazione più grande o di una Federazione di organizzazioni oppure l'organizzazione viene acquisita da un'altra società. In uno scenario in cui si esegue la Federazione delle identità da più domini, sarà necessario conoscerne i domini \(\) attualmente supportati e tutti i domini aggiuntivi che rappresentano i potenziali partner.  
  
-   **Tipi di servizi e applicazioni supportati:** Per alcune applicazioni e servizi è necessario l'accesso alle risorse del sistema operativo, mentre altre sono in grado di riconoscere le attestazioni. È importante comprendere i tipi di applicazioni e servizi che AD FS supporta per poter formulare i requisiti amministrativi.  
  
-   **Diagrammi dell'architettura logica e fisica o topologia di distribuzione:** È necessario tenere presente quanto segue:  
  
    -   Indica se i server federativi funzioneranno in un set di server farm o in un singolo server.  
  
    -   Dove la rete distribuisce firewall e proxy.  
  
    -   La posizione delle risorse e la possibilità per gli utenti di accedere alle risorse all'interno dell'organizzazione, all'esterno dell'organizzazione o a entrambe.  
  
## <a name="how-to-implement-your-ad-fs-design-using-this-guide"></a>Come implementare la progettazione di AD FS usando questa guida  
Il passaggio successivo dell'implementazione della progettazione consiste nel determinare in quale ordine deve essere eseguita ogni attività di distribuzione. Questa guida usa elenchi di controllo per fornire indicazioni dettagliate sulle diverse attività di distribuzione di server e applicazioni necessarie per implementare il piano di progettazione. Gli elenchi di controllo padre e figlio vengono utilizzati in base alle esigenze per rappresentare l'ordine in cui è necessario elaborare le attività per una progettazione di AD FS specifica.  
  
Utilizzare i seguenti elenchi di controllo padre in questa sezione della Guida per acquisire familiarità con le attività di distribuzione per l'implementazione della progettazione AD FS preferita dell'organizzazione:  
  
-   [Elenco di controllo: implementazione di un progetto Web SSO](Checklist--Implementing-a-Web-SSO-Design.md)  
  
-   [Elenco di controllo: implementazione di un progetto Web SSO federato](Checklist--Implementing-a-Federated-Web-SSO-Design.md)  
