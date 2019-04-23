---
ms.assetid: 3ecb6e87-17f1-4d38-97d2-9c4d52b7cf39
title: Pianificazione della capacità per i proxy server federativi
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 2e57f34b173c10e9e753c7f3b8dcd88d7bf6742c
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59888902"
---
# <a name="planning-for-federation-server-proxy-capacity"></a>Pianificazione della capacità per i proxy server federativi

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Pianificazione della capacità per i server federativi aiuta a stimare:  
  
-   I requisiti di hardware appropriato per ciascun proxy server federativo.  
  
-   Il numero di server federativi e proxy server federativi da posizionare in ogni organizzazione.  
  
Server federativi reindirizzano i token di sicurezza da un server federativo protetti nella rete aziendale per gli utenti federati. Lo scopo della distribuzione di un proxy server federativo è consentire agli utenti esterni di connettersi a un server federativo. Esegue effettivamente non firmare i token o scrivere dati nel database di configurazione di AD FS. Di conseguenza, i requisiti hardware per il proxy server federativo sono in genere inferiori rispetto ai requisiti hardware per un server federativo.  
  
Poiché ogni richiesta per un proxy server federativo comporta una richiesta a un server federativo o una server farm federativa, pianificazione della capacità per i server federativi e proxy server federativo deve essere eseguito in parallelo.  
  
Stimare il segno di picco\-aggiuntivi al secondo per il proxy server federativo è necessario comprendere i modelli di utilizzo degli utenti federati che verranno effettuato l'accesso tramite il proxy server federativo. In molte distribuzioni, gli utenti federati che eseguono l'accesso tramite il proxy server federativo si trovano su Internet. È possibile stimare il segno di picco\-aggiuntivi al secondo, esaminando i modelli di utilizzo di questi utenti per le applicazioni Web esistenti che verranno protetti da AD FS federati.  
  
> [!NOTE]  
> Per le distribuzioni di produzione, si consiglia un minimo di due proxy server federativo per ogni istanza di farm di server federativi distribuita.  
  
## <a name="estimate-the-number-of-federation-server-proxies-required-for-your-organization"></a>Stimare il numero di server federativi necessari per l'organizzazione  
Prima che è possibile stimare il numero di computer proxy server federativo di AD FS necessari, è necessario innanzitutto determinare il numero totale di server federativi che si desidera distribuire nell'organizzazione. Per altre informazioni su come eseguire questa operazione, vedere [pianificazione della capacità del Server federativo](Planning-for-Federation-Server-Capacity.md).  
  
Dopo avere scelto per il numero di server federativi, moltiplica questo numero di server per la percentuale di autenticazione federata in arrivo richiede che si prevede che verranno apportate da utenti esterni \(all'esterno della rete aziendale\). Il valore di questo calcolo riporta il numero stimato di server federativi che gestirà le richieste di autenticazione in ingresso per gli utenti esterni.  
  
Ad esempio, se il numero di server federativi consigliato è 3 e si prevede che il numero totale di richieste di autenticazione che verranno apportate da utenti esterni saranno circa il 60% del numero totale di richieste di autenticazione federata, il equivale al calcolo 1.8 \(3 X.60\) che è possibile eseguire un arrotondamento fino a 2.  Pertanto, in questo caso, è necessario distribuire due macchine di proxy server federativo per adattarlo al carico di richieste di autenticazione utente esterno per i server tre federativi.  
  
Nei test eseguiti dal team del prodotto AD FS, l'utilizzo globale della CPU in ogni server federativo è stato trovato per essere significativamente inferiore rispetto all'utilizzo della CPU che è stato osservato nei server federativi per farm stessa.  In un test, mentre un server federativo della CPU è stato che indica che è stato completamente saturo, è stata rilevata solo 20% dell'utilizzo della CPU per un proxy server federativo che fornisce servizi proxy per la stessa farm. Di conseguenza, i nostri test hanno rivelato che il carico sulla CPU di un proxy server federativo, che usa le specifiche hardware simile come descritto in precedenza in questa sezione, è stato in grado di gestire il carico di elaborazione per circa tre i server federativi.  
  
Tuttavia, per scopi di tolleranza di errore, è consigliabile almeno due proxy server federativo per ogni server farm federativa che la distribuzione.  
  
## <a name="see-also"></a>Vedere anche
[Guida alla progettazione di AD FS in Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
