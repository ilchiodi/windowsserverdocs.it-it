---
ms.assetid: e34622ff-b2d0-4f81-8d00-dacd5d6c215e
title: Servizio ora di Windows
description: 
author: shortpatti
ms.author: pashort
manager: brianlic
ms.date: 02/01/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: c3c53db3839b5f33a87fe3789f2f7958f0bc42c2
ms.sourcegitcommit: 556361fe7c73c75d6cdc46a67dc88679fbe89c61
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/08/2018
---
# <a name="windows-time-service"></a>Servizio ora di Windows

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012
 
  
## <a name="w2k3tr_times_intro"></a>Documentazione tecnica su Windows ora servizio  
**In questa Guida**  
  
* Dove trovare informazioni di configurazione del servizio ora di Windows  
* Che cos'è il servizio ora di Windows?  
* Importanza dei protocolli ora  
* Come funziona il servizio ora di Windows   
* Impostazioni e strumenti servizio ora di Windows  
  
> [!NOTE]  
> In Windows Server 2003 e Microsoft Windows 2000 Server, il servizio directory è denominato servizio directory Active Directory. In Windows Server 2008 R2 e Windows Server 2008, il servizio directory è denominato servizi di dominio Active Directory (AD DS). Il resto di questo argomento fa riferimento a servizi di dominio Active Directory, ma le informazioni sono applicabili anche a servizi di dominio Active Directory in Windows Server 2016.  
  
Il servizio ora di Windows, noto anche come W32Time, sincronizza la data e ora per tutti i computer in esecuzione in un dominio di Active Directory. Sincronizzazione dell'ora è fondamentale per il corretto funzionamento dei molti servizi e applicazioni line-of-business. Il servizio ora di Windows utilizza il protocollo NTP (Network Time) per sincronizzare gli orologi dei computer nella rete in modo che un valore di orologio accurato o timestamp, può essere assegnato a risorse e convalida le richieste di accesso di rete. Il servizio si integra NTP e provider servizi orari, rendendolo un servizio ora affidabile e scalabile per gli amministratori dell'organizzazione.  
  
> [!IMPORTANT]  
> Prima di Windows Server 2016, il servizio W32Time non è stato progettato per soddisfare le esigenze dell'applicazione di scadenza.  Tuttavia, gli aggiornamenti a Windows Server 2016 ora consentono di implementare una soluzione per 1 ms accuratezza nel dominio.  Vedere [ora di Windows 2016 accurata](accurate-time.md) e [limiti di supporto per configurare il servizio ora di Windows per gli ambienti ad alta precisione](https://go.microsoft.com/fwlink/?LinkID=179459) per ulteriori informazioni.  
  
## <a name="BKMK_Config"></a>Dove trovare informazioni di configurazione del servizio ora di Windows  
Questa guida viene **non** illustrare la configurazione del servizio ora di Windows. Esistono diversi argomenti su Microsoft TechNet e della Microsoft Knowledge Base che illustrano le procedure per la configurazione del servizio ora di Windows. Se si richiedono informazioni di configurazione, gli argomenti seguenti ti aiuterà a individuare le informazioni appropriate.  
  
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
  
## <a name="BKMK_WTS"></a>Che cos'è il servizio ora di Windows?  
Il servizio ora di Windows (W32Time) fornisce la sincronizzazione dell'orologio di rete per i computer senza la necessità di configurazione completa.  
  
Il servizio ora di Windows è essenziale per il corretto funzionamento di autenticazione Kerberos versione 5 e, pertanto, per l'autenticazione basata su directory Active Directory. Qualsiasi applicazione compatibile con Kerberos, tra cui la maggior parte dei servizi di sicurezza, si basa sulla sincronizzazione dell'ora tra i computer che fanno parte nella richiesta di autenticazione. Controller di dominio Active Directory è necessario inoltre sincronizzare gli orologi per garantire la replica di dati accurati.  
  
Il servizio ora di Windows è implementato in una libreria di collegamento dinamico denominata W32Time.dll. W32Time.dll viene installato per impostazione predefinita nel **%Systemroot%\System32** cartella durante l'installazione del sistema operativo e l'installazione.  
  
W32Time.dll è stato originariamente sviluppato per Windows 2000 Server supportare una specifica dal protocollo di autenticazione Kerberos V5 che richiedevano orologi in una rete da sincronizzare. A partire da Windows Server 2003, W32Time.dll fornito una maggiore precisione nella sincronizzazione dell'orologio di rete del sistema operativo Windows 2000 Server e, inoltre, è supportata un'ampia gamma di dispositivi hardware e i protocolli di rete ora tramite provider servizi orari. Sebbene originariamente progettato per consentire la sincronizzazione dell'orologio per l'autenticazione Kerberos, molte applicazioni corrente utilizzano timestamp per garantire la coerenza transazionale, per registrare l'ora di eventi importanti e altre informazioni aziendali cruciali, scadenza. Queste applicazioni trarre vantaggio dalla sincronizzazione dell'ora tra i computer che viene fornito dal servizio ora di Windows.  
  
## <a name="BKMK_TimeProtocols"></a>Importanza dei protocolli ora  
Protocolli ora la comunicazione tra due computer per scambiare le informazioni sul tempo e quindi usare tali informazioni per sincronizzare gli orologi. Con il protocollo di tempo del servizio ora di Windows, un client richiede informazioni sull'ora da un server e consente di sincronizzare l'orologio in base alle informazioni che sono state ricevute.  
  
Il servizio ora di Windows utilizza NTP per sincronizzare l'ora in una rete. NTP è un protocollo di tempo Internet che include gli algoritmi di disciplina necessari per la sincronizzazione degli orologi. NTP è un protocollo di tempo più accurato rispetto il Simple Network Time Protocol (SNTP) che viene utilizzato in alcune versioni di Windows. tuttavia W32Time continua a supportare SNTP per abilitare la compatibilità con computer che eseguono servizi basati su SNTP tempo, ad esempio Windows 2000.  
  
## <a name="see-also"></a>Vedere anche  
[Come funziona il servizio ora di Windows](How-the-Windows-Time-Service-Works.md)  
[Impostazioni e strumenti servizio ora di Windows](Windows-Time-Service-Tools-and-Settings.md)  
[Articolo della Microsoft Knowledge Base 902229](https://go.microsoft.com/fwlink/?LinkId=186066)