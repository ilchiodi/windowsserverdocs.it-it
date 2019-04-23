---
ms.assetid: e34622ff-b2d0-4f81-8d00-dacd5d6c215e
title: Riferimento tecnico per Windows Time Service
description: Il servizio W32Time fornisce la sincronizzazione degli orologi dei computer senza la necessità di una configurazione complessa. Il servizio W32Time è essenziale per il corretto funzionamento dell'autenticazione Kerberos V5 e, pertanto, per l'autenticazione basata su dominio Active Directory AD.
author: shortpatti
ms.author: dacuo
ms.date: 05/08/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: networking
ms.openlocfilehash: 904a53797d22d2e06e0cd2f7572b99fc386dae16
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59845122"
---
# <a name="windows-time-service-technical-reference"></a>Riferimento tecnico per Windows Time Service
>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows 10 o versione successiva

Il servizio W32Time fornisce la sincronizzazione degli orologi dei computer senza la necessità di una configurazione complessa. Il servizio W32Time è essenziale per il corretto funzionamento dell'autenticazione Kerberos V5 e, pertanto, per l'autenticazione basata su dominio Active Directory AD. Qualsiasi applicazione compatibile con Kerberos, tra cui la maggior parte dei servizi di sicurezza, si basa sulla sincronizzazione dell'ora tra i computer che fanno parte della richiesta di autenticazione. I controller di dominio di Active Directory Domain Services devono anche avere sincronizzato orologi per contribuire a garantire la replica dei dati accurati.

> [!NOTE]  
> In Windows Server 2003 e Microsoft Windows 2000 Server, il servizio directory è denominato servizio directory Active Directory. In Windows Server 2008 R2 e Windows Server 2008, il servizio directory è denominato Active Directory Domain Services (AD DS). Il resto di questo argomento fa riferimento ad AD DS, ma le informazioni sono applicabile anche a servizi di dominio Active Directory in Windows Server 2016.

Il servizio W32Time viene implementato in una libreria a collegamento dinamico denominata W32Time. dll, che viene installato per impostazione predefinita in **%Systemroot%\System32**. W32Time. dll è stato originariamente sviluppato per Windows 2000 Server supportare una specifica dal protocollo di autenticazione Kerberos V5 che richiedevano orologi in una rete da sincronizzare. A partire da Windows Server 2003, W32Time. dll fornito una maggiore accuratezza in sincronizzazione dell'orologio rete tramite il sistema operativo Windows Server 2000. In Windows Server 2003, inoltre, W32Time. dll supportata un'ampia gamma di dispositivi hardware e protocolli di rete ora usando provider servizi orari.

Anche se originariamente progettato per consentire la sincronizzazione dell'orologio per l'autenticazione Kerberos, molte applicazioni usano i timestamp per garantire la coerenza delle transazioni, registrare l'ora di eventi importanti e altri business-critical, scadenza informazioni.  Queste applicazioni trarre vantaggio dalla sincronizzazione dell'ora tra i computer che sono forniti dal servizio ora di Windows.

## <a name="importance-of-time-protocols"></a>Importanza dei protocolli ora
Protocolli ora le comunicazioni tra due computer per lo scambio di informazioni sull'ora e quindi usare tali informazioni per sincronizzare gli orologi. Con il protocollo ora servizio ora di Windows, un client richiede informazioni sull'ora da un server e consente di sincronizzare l'orologio sulla base delle informazioni che viene ricevute.
  
Il servizio ora di Windows utilizza NTP per sincronizzare l'ora in una rete. NTP è un protocollo di tempo Internet che include gli algoritmi di disciplina necessari per la sincronizzazione degli orologi. NTP è un protocollo di tempo più accurato rispetto il Simple Network Time Protocol (SNTP) che viene utilizzato in alcune versioni di Windows; tuttavia W32Time continua a supportare SNTP per abilitare la compatibilità con computer che eseguono servizi basati su SNTP tempo, ad esempio Windows 2000.
<!-- maybe this should be its own topic under the Tech Ref section -->
## <a name="where-to-find-windows-time-service-configuration-related-information"></a>Dove trovare informazioni relative alla configurazione del servizio ora di Windows  
Questa guida viene **non** illustrano la configurazione del servizio ora di Windows. Esistono diversi argomenti su Microsoft TechNet e della Microsoft Knowledge Base che illustrano le procedure per la configurazione del servizio ora di Windows. Se si richiedono informazioni di configurazione, gli argomenti seguenti devono consentono di individuare le informazioni appropriate.  
<!-- should this be an if/then table -->
-   Per configurare il servizio ora di Windows per l'emulatore (PDC) controller di dominio primario radice foresta, vedere:  
  
    -   [Configurare il servizio ora di Windows sull'emulatore PDC nel dominio radice della foresta](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc731191%28v=ws.10%29) 
  
    -   [Configurazione di un'origine dell'ora per la foresta](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-r2-and-2008/cc794823%28v%3dws.10%29) 
  
    -   Articolo della Microsoft Knowledge Base, 816042 [come configurare un server di riferimento ora autorevole in Windows Server](https://go.microsoft.com/fwlink/?LinkID=60402), che descrive le impostazioni di configurazione per i computer che esegue Windows Server 2008 R2, Windows Server 2008, Windows Server 2003 e Windows Server 2003 R2.  
  
-   Per configurare il servizio ora di Windows su qualsiasi client membro di dominio o server o anche controller di dominio che non sono configurati come emulatore del PDC radice foresta, vedere [configurare un computer client per la sincronizzazione dell'ora di dominio automatici](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-r2-and-2008/cc816884%28v%3dws.10%29).  
  
    > [!WARNING]  
    > Alcune applicazioni possono richiedere ai servizi in fase di elevata precisione loro computer. In tal caso, è possibile scegliere di configurare un'origine dell'ora manuale, ma tenere presente che il servizio ora di Windows non è stato progettato per funzionare come un'origine ora estremamente precisi. Verificare che sia compatibile con le limitazioni di supporto per gli ambienti ad alta precisione time come descritto nella Microsoft Knowledge Base 939322, articolo [limiti di supporto per configurare il servizio ora di Windows per gli ambienti ad alta precisione](support-boundary.md).  
  
-   Per configurare il servizio ora di Windows su qualsiasi Windows computer basati su client o server configurati come membri del team anziché i membri del dominio notare [configurare un'origine dell'ora manuale per un computer client selezionati](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-r2-and-2008/cc816656%28v%3dws.10%29).  
  
-   Per configurare il servizio ora di Windows in un computer host che esegue un ambiente virtuale, vedere l'articolo della Microsoft Knowledge Base, 816042 [come configurare un server di riferimento ora autorevole in Windows Server](https://go.microsoft.com/fwlink/?LinkID=60402). Se si lavora con un prodotto di virtualizzazione non Microsoft, assicurarsi di consultare la documentazione del fornitore del prodotto.  
  
-   Per configurare il servizio ora di Windows in un controller di dominio che è in esecuzione in una macchina virtuale, si consiglia di disattivare parzialmente la sincronizzazione dell'ora tra il sistema e il guest sistema operativo host che agisce come un controller di dominio. Ciò consente il controller di dominio guest sincronizzare l'ora per la gerarchia dei domini, ma impedisce lo sfasamento dell'ora una se viene ripristinato da uno stato salvato. Per altre informazioni, vedere l'articolo della Microsoft Knowledge Base, 976924 [viene visualizzato servizio ora di Windows gli ID evento 24, 29 e 38 in un controller di dominio virtualizzati in esecuzione in un server host basato su Windows Server 2008 con Hyper-V](https://go.microsoft.com/fwlink/?LinkID=192236) e [considerazioni sulla distribuzione di controller di dominio virtualizzati](https://go.microsoft.com/fwlink/?LinkID=192235).  
  
-   Per configurare il servizio ora di Windows in un controller di dominio che agisce come l'emulatore PDC radice foresta che è in esecuzione anche in un computer virtuale, seguire le stesse istruzioni per un computer fisico, come descritto in [configurare il servizio ora di Windows in l'emulatore PDC nel dominio radice della foresta](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc731191%28v=ws.10%29).  
  
-   Per configurare il servizio ora di Windows in un server membro con un computer virtuale, usare la gerarchia temporale di dominio come descritto in [configurare un computer client per la sincronizzazione dell'ora automatica dominio](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-r2-and-2008/cc816884%28v%3dws.10%29).


> [!IMPORTANT]  
> Prima di Windows Server 2016, il servizio W32Time non è stato progettato per soddisfare le esigenze dell'applicazione di scadenza.  Tuttavia, gli aggiornamenti a Windows Server 2016 ora consentono di implementare una soluzione per 1 ms accuratezza nel dominio.  Per altre informazioni, vedere [ora di Windows 2016 accurata](accurate-time.md) e [limiti di supporto per configurare il servizio ora di Windows per gli ambienti ad alta precisione](support-boundary.md) per altre informazioni.

## <a name="related-topics"></a>Argomenti correlati
- [Ora esatta Windows 2016](accurate-time.md)
- [Miglioramenti di accuratezza di tempo per Windows Server 2016](windows-server-2016-improvements.md)  
- [Come funziona il servizio ora di Windows](How-the-Windows-Time-Service-Works.md)  
- [Windows ora servizio strumenti e impostazioni](Windows-Time-Service-Tools-and-Settings.md)  
- [Limiti di supporto per configurare il servizio ora di Windows per gli ambienti ad alta precisione](support-boundary.md)
- [Articolo della Microsoft Knowledge Base 902229](https://go.microsoft.com/fwlink/?LinkId=186066)