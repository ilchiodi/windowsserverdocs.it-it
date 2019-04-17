---
ms.assetid: 3ecb6e87-17f1-4d38-97d2-9c4d52b7cf39
title: "Pianificazione della capacità di Proxy Server federativo"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 2e57f34b173c10e9e753c7f3b8dcd88d7bf6742c
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2017
---
# <a name="planning-for-federation-server-proxy-capacity"></a>Pianificazione della capacità di Proxy Server federativo

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Pianificazione della capacità per i proxy server federativi consente di stimare:  
  
-   I requisiti hardware appropriato per ogni proxy server federativo.  
  
-   Il numero di server federativi e proxy server federativi per inserire in ogni organizzazione.  
  
Server federativi reindirizzano i token di sicurezza da un server federativo protetto nella rete aziendale per gli utenti federati. Lo scopo della distribuzione di un proxy server federativo è consentire agli utenti esterni di connettersi a un server federativo. Caso non è effettivamente i token di accesso o scrivere dati nel database di configurazione di ADFS. Di conseguenza, i requisiti hardware per il proxy server federativo sono in genere inferiori rispetto ai requisiti hardware per un server federativo.  
  
Poiché ogni richiesta per un proxy server federativo in una richiesta a un server federativo o una server farm federativa, pianificazione della capacità per i server federativi e proxy server federativi devono essere eseguite in parallelo.  
  
Stima il picco accesso aggiuntivo al secondo per il proxy server federativo è necessario individuare schemi di utilizzo degli utenti federati ai quali verranno accesso tramite il proxy server federativo. In molte distribuzioni, gli utenti federati l'accesso utilizzando il proxy server federativo si trovano su Internet. Esaminando i modelli di utilizzo di questi utenti federati nelle applicazioni Web esistenti che verranno protetti da ADFS, è possibile stimare il picco accesso aggiuntivo al secondo.  
  
> [!NOTE]  
> Per le distribuzioni di produzione, si consiglia un minimo di due proxy server federativo per ogni istanza di farm di server federativo che si distribuisce.  
  
## <a name="estimate-the-number-of-federation-server-proxies-required-for-your-organization"></a>Stimare il numero di server federativi necessari per l'organizzazione  
Prima di è possibile stimare il numero di computer proxy server federativo di ADFS necessari, è necessario determinare il numero totale di server federativi che sarà distribuito nell'organizzazione. Per ulteriori informazioni su come eseguire questa operazione, vedere [pianificazione della capacità del Server federativo](Planning-for-Federation-Server-Capacity.md).  
  
Dopo avere scelto per il numero di server federativi, più volte il numero di server per la percentuale di autenticazione federata in ingresso richiede che si prevede che saranno da utenti esterni \ (si trovano di fuori di isolamento di rete aziendale). Il valore di questo calcolo fornirà con il numero stimato di proxy server federativi che consente di gestire le richieste di autenticazione in ingresso per gli utenti esterni.  
  
Ad esempio, se il numero di server federativi consigliata è di 3 e si prevede che il numero totale di richieste di autenticazione effettuati da utenti esterni sarà di circa il 60% del numero totale di richieste di autenticazione federata, il calcolo daranno 1.8 \ (3 X. 60\) che puoi arrotondare fino a 2.  Pertanto, in questo caso, sarà necessario distribuire due macchine di proxy server federativo per gestire il carico delle richieste di autenticazione utente esterno per i server tre federativi.  
  
Nei test eseguiti dal team del prodotto AD FS, l'utilizzo globale della CPU in ogni server federativo è stato trovato per essere notevolmente inferiore l'utilizzo della CPU che è stato rilevato nei server federativi della stessa farm.  In un test, mentre un server federativo della CPU è stato che indica che è stato completamente saturo, è stata rilevata solo 20% dell'utilizzo della CPU per un proxy server federativo che forniscono servizi proxy per la stessa farm. Di conseguenza, i test è emerso che il carico della CPU di un proxy server federativo, che utilizza le specifiche hardware simili come descritto in precedenza in questa sezione, stato in grado di gestire il carico di elaborazione per circa tre i server federativi.  
  
Tuttavia, per motivi di tolleranza di errore, si consiglia un minimo di due proxy server federativo per ogni server farm federativa che la distribuzione.  
  
## <a name="see-also"></a>Vedere anche
[Guida alla progettazione di ADFS in Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
