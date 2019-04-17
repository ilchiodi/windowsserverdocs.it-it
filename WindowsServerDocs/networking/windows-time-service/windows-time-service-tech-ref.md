---
ms.assetid: e34622ff-b2d0-4f81-8d00-dacd5d6c215e
title: Documentazione tecnica su Windows ora servizio
description: ''
author: shortpatti
ms.author: pashort
manager: brianlic
ms.date: 02/01/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: networking
ms.openlocfilehash: 916ac654aeb1ca56e0283f9a18e011ef67734776
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/28/2018
---
# <a name="windows-time-service-technical-reference"></a>Documentazione tecnica su Windows ora servizio
Il servizio W32Time fornisce la sincronizzazione dell'orologio di rete per i computer senza la necessità di configurazione completa. Il servizio W32Time è essenziale per il corretto funzionamento dell'autenticazione Kerberos V5 e, pertanto, per l'autenticazione basata su directory Active Directory. Qualsiasi applicazione compatibile con Kerberos, tra cui la maggior parte dei servizi di sicurezza, si basa sulla sincronizzazione dell'ora tra i computer che fanno parte nella richiesta di autenticazione. Controller di dominio Active Directory è necessario inoltre sincronizzare gli orologi per garantire la replica di dati accurati.

> [!NOTE]  
> In Windows Server 2003 e Microsoft Windows 2000 Server, il servizio directory è denominato servizio directory Active Directory. In Windows Server 2008 R2 e Windows Server 2008, il servizio directory è denominato servizi di dominio Active Directory (AD DS). Il resto di questo argomento fa riferimento a servizi di dominio Active Directory, ma le informazioni sono applicabili anche a servizi di dominio Active Directory in Windows Server 2016.

Il servizio W32Time è implementato in una libreria di collegamento dinamico denominata W32Time.dll, che viene installato per impostazione predefinita in **%Systemroot%\System32**. W32Time.dll è stato originariamente sviluppato per Windows 2000 Server supportare una specifica dal protocollo di autenticazione Kerberos V5 che richiedevano orologi in una rete da sincronizzare. A partire da Windows Server 2003, W32Time.dll fornito una maggiore precisione nella sincronizzazione dell'orologio di rete del sistema operativo Windows Server 2000. Inoltre, in Windows Server 2003, W32Time.dll supportata un'ampia gamma di dispositivi hardware e i protocolli di rete tempo utilizzando provider servizi orari.

Sebbene originariamente progettato per consentire la sincronizzazione dell'orologio per l'autenticazione Kerberos, molte applicazioni corrente utilizzano timestamp per garantire la coerenza delle transazioni, registrare il tempo di eventi importanti e altre business-critical, scadenza informazioni.  Queste applicazioni trarre vantaggio dalla sincronizzazione dell'ora tra i computer che vengono forniti dal servizio ora di Windows.

## <a name="importance-of-time-protocols"></a>Importanza dei protocolli ora
Protocolli ora la comunicazione tra due computer per scambiare le informazioni sul tempo e quindi usare tali informazioni per sincronizzare gli orologi. Con il protocollo di tempo del servizio ora di Windows, un client richiede informazioni sull'ora da un server e consente di sincronizzare l'orologio in base alle informazioni che sono state ricevute.
  
Il servizio ora di Windows utilizza NTP per sincronizzare l'ora in una rete. NTP è un protocollo di tempo Internet che include gli algoritmi di disciplina necessari per la sincronizzazione degli orologi. NTP è un protocollo di tempo più accurato rispetto il Simple Network Time Protocol (SNTP) che viene utilizzato in alcune versioni di Windows. tuttavia W32Time continua a supportare SNTP per abilitare la compatibilità con computer che eseguono servizi basati su SNTP tempo, ad esempio Windows 2000.
<!-- maybe this should be its own topic under the Tech Ref section -->
## <a name="BKMK_Config"></a>Dove trovare informazioni relative alla configurazione del servizio ora di Windows  
Questa guida viene **non** illustrare la configurazione del servizio ora di Windows. Esistono diversi argomenti su Microsoft TechNet e della Microsoft Knowledge Base che illustrano le procedure per la configurazione del servizio ora di Windows. Se si richiedono informazioni di configurazione, gli argomenti seguenti ti aiuterà a individuare le informazioni appropriate.  
<!-- should this be an if/then table -->
-   Per configurare il servizio ora di Windows per l'emulatore (PDC) controller di dominio primario radice foresta, vedere:  
  
    -   [Configurare il servizio ora di Windows sull'emulatore PDC nel dominio radice della foresta](https://docs.microsoft.com/en-us/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc731191%28v=ws.10%29) 
  
    -   [Configurazione di un'origine dell'ora per la foresta](https://docs.microsoft.com/en-us/previous-versions/windows/it-pro/windows-server-2008-r2-and-2008/cc794823%28v%3dws.10%29) 
  
    -   Articolo della Microsoft Knowledge Base 816042, [come configurare un server di riferimento ora autorevole in Windows Server](https://go.microsoft.com/fwlink/?LinkID=60402), che descrive le impostazioni di configurazione per i computer che eseguono Windows Server 2008 R2, Windows Server 2008, Windows Server 2003, e Windows Server 2003 R2.  
  
-   Per configurare il servizio ora di Windows in qualsiasi client membri del dominio o un server o un controller di dominio anche che non sono configurati come l'emulatore PDC radice foresta, vedere [configurare un computer client per la sincronizzazione dell'ora dominio automatica](https://docs.microsoft.com/en-us/previous-versions/windows/it-pro/windows-server-2008-r2-and-2008/cc816884%28v%3dws.10%29).  
  
    > [!WARNING]  
    > Alcune applicazioni possono richiedere i computer di servizi di tempo ad alta precisione. In tal caso, è possibile configurare un'origine dell'ora manuale, ma Tieni presente che il servizio ora di Windows non è stato progettato per funzionare come origine dell'ora estremamente precisa. Assicurarsi di essere compatibile con le limitazioni di supporto per gli ambienti ad alta precisione ora come descritto nell'articolo della Microsoft Knowledge Base 939322, [limiti di supporto per configurare il servizio ora di Windows per gli ambienti ad alta precisione](https://go.microsoft.com/fwlink/?LinkID=179459).  
  
-   Per configurare il servizio ora di Windows in basati su Windows server o client computer che sono configurati come membri del gruppo di lavoro anziché i membri del dominio vedono [configurare un'origine dell'ora manuale per un computer client selezionati](https://docs.microsoft.com/en-us/previous-versions/windows/it-pro/windows-server-2008-r2-and-2008/cc816656%28v%3dws.10%29).  
  
-   Per configurare il servizio ora di Windows in un computer host che esegue un ambiente virtuale, vedere l'articolo della Microsoft Knowledge Base 816042, [come configurare un server di riferimento ora autorevole in Windows Server](https://go.microsoft.com/fwlink/?LinkID=60402). Se si lavora con un prodotto di virtualizzazione non Microsoft, assicurarsi di consultare la documentazione del fornitore per il prodotto.  
  
-   Per configurare il servizio ora di Windows in un controller di dominio che è in esecuzione in una macchina virtuale, è consigliabile disabilitare parzialmente sincronizzazione dell'ora tra il sistema e guest sistema operativo host funge da un controller di dominio. Questo consente il controller di dominio guest di sincronizzare l'ora per la gerarchia dei domini, ma protegge dagli con un tempo di inclinazione se viene ripristinato da uno stato salvato. Per ulteriori informazioni, vedere l'articolo della Microsoft Knowledge Base 976924, [Ricevi servizio ora di Windows gli ID evento 24, 29 e 38 in un controller di dominio virtualizzati che è in esecuzione su un server host basato su Windows Server 2008 con Hyper-V](https://go.microsoft.com/fwlink/?LinkID=192236) e [Considerazioni sulla distribuzione del controller di dominio virtualizzati](https://go.microsoft.com/fwlink/?LinkID=192235).  
  
-   Per configurare il servizio ora di Windows in un controller di dominio che agisce come l'emulatore PDC radice foresta che è in esecuzione anche in un computer virtuale, seguire le stesse istruzioni per un computer fisico come descritto in [configurare il servizio ora di Windows nel controller di dominio primario emulatore nel dominio radice della foresta](https://docs.microsoft.com/en-us/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc731191%28v=ws.10%29).  
  
-   Per configurare il servizio ora di Windows in un server membro in esecuzione come un computer virtuale, utilizzare la gerarchia temporale dominio come descritto in ([configurare un computer client per la sincronizzazione dell'ora dominio automatica](https://docs.microsoft.com/en-us/previous-versions/windows/it-pro/windows-server-2008-r2-and-2008/cc816884%28v%3dws.10%29).


> [!IMPORTANT]  
> Prima di Windows Server 2016, il servizio W32Time non è stato progettato per soddisfare le esigenze dell'applicazione di scadenza.  Tuttavia, gli aggiornamenti a Windows Server 2016 ora consentono di implementare una soluzione per 1 ms accuratezza nel dominio.  Vedere [ora di Windows 2016 accurata](accurate-time.md) e [limiti di supporto per configurare il servizio ora di Windows per gli ambienti ad alta precisione](https://go.microsoft.com/fwlink/?LinkID=179459) per ulteriori informazioni.

## <a name="related-topics"></a>Argomenti correlati  
[Come funziona il servizio ora di Windows](How-the-Windows-Time-Service-Works.md)  
[Impostazioni e strumenti servizio ora di Windows](Windows-Time-Service-Tools-and-Settings.md)  
[Articolo della Microsoft Knowledge Base 902229](https://go.microsoft.com/fwlink/?LinkId=186066)