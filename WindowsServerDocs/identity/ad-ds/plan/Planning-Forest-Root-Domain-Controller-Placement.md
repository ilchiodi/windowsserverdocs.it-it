---
ms.assetid: 2a2f493a-9796-454a-9721-e223b799dfa7
title: Pianificazione del posizionamento del controller di dominio radice della foresta
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 57eafcc884a827d98c249e2da0c0af6888abc5b9
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59823642"
---
# <a name="planning-forest-root-domain-controller-placement"></a>Pianificazione del posizionamento del controller di dominio radice della foresta

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Controller di dominio radice della foresta sono necessari per creare percorsi di trust per i client che devono accedere alle risorse in domini diversi da quello proprio. Inserire il controller di dominio radice foresta nei percorsi di hub e in posizioni che ospitano i Data Center. Se gli utenti in una determinata località devono accedere alle risorse da altri domini nella stessa posizione e la disponibilità di rete tra il Data Center e la posizione dell'utente non è affidabile, è possibile aggiungere un controller di dominio radice della foresta nel percorso o creare un trust di collegamento tra i due domini. È più conveniente per creare un trust di collegamento tra i domini a meno che non si dispone di altri motivi per inserire un controller di dominio radice della foresta in tale percorso.  
  
Guida per ottimizzare le richieste di autenticazione eseguite dagli utenti che si trova in uno dei due domini di trust di collegamento. Per altre informazioni sui trust di collegamento tra i domini, vedere l'articolo [informazioni su quando creare un Trust di collegamento](https://go.microsoft.com/fwlink/?LinkId=107061).  
  
Per un foglio di lavoro agevolare così a documentare il posizionamento dei controller di dominio radice foresta, vedere [processo di supporto per Windows Server 2003 Deployment Kit](https://go.microsoft.com/fwlink/?LinkID=102558), scaricare Job_Aids_Designing_and_Deploying_Directory_and_Security_Services.zip e aprire "Posizionamento Controller di dominio" (DSSTOPO_4.doc).  
  
Dovrai usare queste informazioni quando si crea il dominio radice della foresta. Per ulteriori informazioni sulla distribuzione di dominio radice della foresta, vedere [la distribuzione di un dominio radice della foresta Windows Server 2008](https://technet.microsoft.com/library/cc731174.aspx).  
