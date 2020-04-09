---
ms.assetid: 3ecb6e87-17f1-4d38-97d2-9c4d52b7cf39
title: Pianificazione della capacità per i proxy server federativi
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: ec5b6e39e57539ef3de1c7bd5fed9cc8ed2e64be
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80858764"
---
# <a name="planning-for-federation-server-proxy-capacity"></a>Pianificazione della capacità per i proxy server federativi

La pianificazione della capacità per i proxy server federativi consente di stimare:  
  
-   Requisiti hardware appropriati per ciascun proxy server federativo.  
  
-   Il numero di server federativi e proxy server federativi da inserire in ogni organizzazione.  
  
I proxy server federativi reindirizzano i token di sicurezza da un server federativo protetto nella rete aziendale agli utenti federati. Lo scopo della distribuzione di un proxy server federativo è consentire agli utenti esterni di connettersi a un server federativo. Non firma effettivamente i token o scrive i dati nel database di configurazione AD FS. Pertanto, i requisiti hardware per il proxy server federativo sono in genere inferiori ai requisiti hardware per un server federativo.  
  
Poiché ogni richiesta a un proxy server federativo comporta una richiesta a un server federativo o a un server farm di federazione, la pianificazione della capacità per i server federativi e i proxy server federativi deve essere eseguita in parallelo.  
  
Per stimare il picco del segno\-al secondo per il proxy server federativo, è necessario conoscere i modelli di utilizzo degli utenti federati che effettueranno l'accesso tramite il proxy server federativo. In molte distribuzioni gli utenti federati che eseguono l'accesso tramite il proxy server federativo si trovano su Internet. È possibile stimare il picco del segno\-al secondo osservando i modelli di utilizzo di questi utenti federati sulle applicazioni Web esistenti che verranno protette da AD FS.  
  
> [!NOTE]  
> Per le distribuzioni di produzione, è consigliabile disporre di almeno due proxy server federativi per ogni istanza di Federazione server farm distribuita.  
  
## <a name="estimate-the-number-of-federation-server-proxies-required-for-your-organization"></a>Stimare il numero di proxy server federativi necessari per l'organizzazione  
Prima di poter stimare il numero di AD FS computer proxy server federativi necessari, sarà prima di tutto necessario determinare il numero totale di server federativi da distribuire nell'organizzazione. Per ulteriori informazioni su come eseguire questa operazione, vedere [Planning for Federation Server Capacity](Planning-for-Federation-Server-Capacity.md).  
  
Una volta deciso il numero di server federativi, moltiplicare questo numero di server per la percentuale di richieste di autenticazione federata in ingresso che si prevede verranno effettuate dagli utenti esterni \(situate all'esterno della rete aziendale\). Il valore di questo calcolo fornirà il numero stimato di proxy server federativi che gestiranno le richieste di autenticazione in ingresso per gli utenti esterni.  
  
Se, ad esempio, il numero di server federativi consigliati è 3 e si prevede che il numero totale di richieste di autenticazione che verranno effettuate dagli utenti esterni sarà approssimativamente del 60% del numero totale di richieste di autenticazione federate, il calcolo sarà uguale a 1,8 \(3 X. 60\) che è possibile arrotondare per eccesso a 2.  Pertanto, in questo caso, è necessario distribuire due computer proxy server federativi per gestire il carico delle richieste di autenticazione utente esterno per i tre server federativi.  
  
Nei test eseguiti dal team del prodotto AD FS, l'utilizzo complessivo della CPU in ogni proxy server federativo risultava significativamente inferiore rispetto all'utilizzo della CPU osservato nei server federativi per la stessa farm.  In un unico test, mentre una CPU del server federativo indica che è stata completamente satura, la CPU per un proxy server federativo che fornisce servizi proxy per la stessa farm è stata osservata solo al 20% di utilizzo. Pertanto, i test hanno rivelato che il carico sulla CPU di un proxy server federativo, che utilizza specifiche hardware simili, come illustrato in precedenza in questa sezione, può ragionevolmente gestire il carico di elaborazione per circa tre server federativi.  
  
Tuttavia, per finalità di tolleranza di errore, è consigliabile disporre di almeno due proxy server federativi per ogni Federazione server farm distribuita.  
  
## <a name="see-also"></a>Vedi anche
[Guida alla progettazione di AD FS in Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
