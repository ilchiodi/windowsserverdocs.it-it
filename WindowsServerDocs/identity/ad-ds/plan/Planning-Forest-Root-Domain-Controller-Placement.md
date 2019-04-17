---
ms.assetid: 2a2f493a-9796-454a-9721-e223b799dfa7
title: Pianificazione posizionamento del Controller di dominio radice foresta
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 374ce31c61c666302e2b00a8f365c289cb8799f8
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/12/2017
---
# <a name="planning-forest-root-domain-controller-placement"></a>Pianificazione posizionamento del Controller di dominio radice foresta

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Controller di dominio radice foresta sono necessari per creare percorsi di trust per i client che devono accedere alle risorse in domini diversi dai propri. Posizionare il controller di dominio radice foresta in sedi hub e nei percorsi che ospitano i centri dati. Se gli utenti in un determinato percorso devono accedere alle risorse di altri domini nella stessa posizione e la disponibilità di rete tra il Data Center e la posizione dell'utente non è affidabile, è possibile aggiungere un controller di dominio radice della foresta nel percorso o creare un trust di collegamento tra i due domini. È più una riduzione dei costi per creare un trust di collegamento tra i domini a meno che non hai altri motivi per inserire un controller di dominio radice della foresta in tale percorso.  
  
Trust di collegamento Guida per ottimizzare le richieste di autenticazione eseguite dagli utenti che si trova in uno dei due domini. Per ulteriori informazioni sui trust di collegamento tra domini, vedere informazioni sulla creazione di un Trust di collegamento ([https://go.microsoft.com/fwlink/?LinkId=107061](https://go.microsoft.com/fwlink/?LinkId=107061)).  
  
Per un foglio di lavoro agevolare la documentazione del posizionamento del controller dominio radice foresta, vedere processo Aid per Windows Server 2003 Deployment Kit ([https://go.microsoft.com/fwlink/?LinkID=102558](https://go.microsoft.com/fwlink/?LinkID=102558)), scaricare Job_Aids_Designing_and_Deploying_Directory_and_Security_Services.zip, e Aprire (DSSTOPO_4.doc) "Posizionamento del Controller di dominio".  
  
È necessario fare riferimento a queste informazioni quando si crea il dominio radice della foresta. Per ulteriori informazioni sulla distribuzione di dominio radice della foresta, vedere [la distribuzione di un dominio radice della foresta Windows Server 2008](https://technet.microsoft.com/library/cc731174.aspx).  
  


