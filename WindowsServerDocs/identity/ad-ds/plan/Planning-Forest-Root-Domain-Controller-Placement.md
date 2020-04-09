---
ms.assetid: 2a2f493a-9796-454a-9721-e223b799dfa7
title: Pianificazione del posizionamento del controller di dominio radice della foresta
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: 4bbdbfe294695e068c2ec76a135526febb4b6485
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80822145"
---
# <a name="planning-forest-root-domain-controller-placement"></a>Pianificazione del posizionamento del controller di dominio radice della foresta

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

I controller di dominio radice della foresta sono necessari per creare percorsi di attendibilità per i client che devono accedere alle risorse in domini diversi da quelli propri. Posizionare i controller di dominio radice della foresta nelle posizioni degli hub e nelle posizioni che ospitano i Data Center. Se gli utenti in una determinata posizione devono accedere alle risorse da altri domini nella stessa posizione e la disponibilità di rete tra il Data Center e il percorso utente non è affidabile, è possibile aggiungere un controller di dominio radice della foresta nella posizione o creare un trust di collegamento tra i due domini. È più conveniente creare un trust di collegamento tra i domini a meno che non esistano altri motivi per inserire un controller di dominio radice della foresta in tale posizione.  
  
I trust di collegamento consentono di ottimizzare le richieste di autenticazione effettuate dagli utenti che si trovano in uno dei due domini. Per ulteriori informazioni sui trust di collegamento tra domini, vedere l'articolo [informazioni sulla creazione di un trust di collegamento](https://go.microsoft.com/fwlink/?LinkId=107061).  
  
Per un foglio di lavoro che assiste l'utente nella documentazione del posizionamento del controller di dominio radice della foresta, vedere la pagina [relativa ai supporti per i processi per Windows Server 2003 Deployment Kit](https://go.microsoft.com/fwlink/?LinkID=102558), scaricare Job_Aids_Designing_and_Deploying_Directory_and_Security_Services. zip e aprire "selezione host per controller di dominio" (DSSTOPO_4. doc).  
  
Quando si crea il dominio radice della foresta, sarà necessario fare riferimento a queste informazioni. Per ulteriori informazioni sulla distribuzione di dominio radice della foresta, vedere [la distribuzione di un dominio radice della foresta Windows Server 2008](https://technet.microsoft.com/library/cc731174.aspx).  
