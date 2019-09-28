---
ms.assetid: e34622ff-b2d0-4f81-8d00-dacd5d6c215e
title: Riferimento tecnico per Windows Time Service
description: ''
author: shortpatti
ms.author: pashort
manager: dougkim
ms.date: 05/08/2018
ms.topic: article
ms.prod: windows-server
ms.technology: networking
ms.openlocfilehash: 31c7c53a5dd28813076fcaa745093050808b5755
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71405230"
---
# <a name="windows-time-service"></a>Ora di Windows

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows 10 o versione successiva


**Contenuto della Guida**  
  
* Dove trovare le informazioni di configurazione del servizio ora di Windows  
* Che cos'è il servizio ora di Windows?  
* Importanza dei protocolli temporali  
* Funzionamento del servizio Ora di Windows   
* Strumenti e impostazioni del servizio Ora di Windows  
  
> [!NOTE]  
> In Windows Server 2003 e Microsoft Windows 2000 Server, il servizio directory è denominato servizio directory Active Directory. In Windows Server 2008 R2 e Windows Server 2008, il servizio directory è denominato Active Directory Domain Services (AD DS). Nella parte restante di questo argomento si fa riferimento a servizi di dominio Active Directory, ma le informazioni sono applicabili anche a Active Directory Domain Services in Windows Server 2016.  
  
Il servizio ora di Windows, noto anche come W32Time, sincronizza la data e l'ora per tutti i computer in esecuzione in un dominio di servizi di dominio Active Directory. La sincronizzazione dell'ora è essenziale per il corretto funzionamento di molti servizi Windows e applicazioni line-of-business. Il servizio ora di Windows usa il protocollo NTP (Network Time Protocol) per sincronizzare gli orologi dei computer in rete, in modo che sia possibile assegnare un valore di clock accurato o un timestamp alle richieste di convalida della rete e di accesso alle risorse. Il servizio integra i provider NTP e Time, rendendolo un servizio di tempo affidabile e scalabile per gli amministratori dell'organizzazione.
  
> [!IMPORTANT]  
> Prima di Windows Server 2016, il servizio W32Time non è stato progettato per soddisfare le esigenze dell'applicazione in termini di tempo.  Tuttavia, gli aggiornamenti a Windows Server 2016 consentono ora di implementare una soluzione per l'accuratezza del 1 MS nel dominio.  Per ulteriori informazioni, vedere l' [ora esatta](accurate-time.md) e [il limite del supporto di Windows 2016 per configurare il servizio ora di Windows per ambienti ad alta precisione](support-boundary.md) .  
  
## <a name="BKMK_Config"></a>Dove trovare le informazioni di configurazione del servizio ora di Windows  
In questa guida **non** viene illustrata la configurazione del servizio ora di Windows. In Microsoft TechNet e nella Microsoft Knowledge base sono disponibili diversi argomenti che illustrano le procedure per la configurazione del servizio ora di Windows. Se sono necessarie informazioni di configurazione, gli argomenti seguenti consentono di individuare le informazioni appropriate.  
  
-   Per configurare il servizio ora di Windows per l'emulatore del controller di dominio primario (PDC) radice della foresta, vedere:  
  
    -   [Configurare il servizio ora di Windows nell'emulatore PDC nel dominio radice della foresta](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc731191%28v=ws.10%29) 
  
    -   [Configurazione di un'origine dell'ora per la foresta](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-r2-and-2008/cc794823%28v%3dws.10%29) 
  
    -   Articolo della Microsoft Knowledge base 816042, [come configurare un server di ora autorevole in Windows Server](https://go.microsoft.com/fwlink/?LinkID=60402), che descrive le impostazioni di configurazione per i computer che eseguono windows Server 2008 R2, windows Server 2008, windows Server 2003 e Windows Server 2003 R2.  
  
-   Per configurare il servizio ora di Windows in qualsiasi client o server membro del dominio o anche per i controller di dominio non configurati come emulatore PDC radice della foresta, vedere [configurare un computer client per la sincronizzazione automatica dell'ora di dominio](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-r2-and-2008/cc816884%28v%3dws.10%29).  
  
    > [!WARNING]  
    > Per alcune applicazioni potrebbe essere necessario che i computer dispongano di servizi temporali con accuratezza elevata. In tal caso, è possibile scegliere di configurare un'origine dell'ora manuale, ma è necessario tenere presente che il servizio ora di Windows non è stato progettato per funzionare come origine dell'ora molto accurata. Assicurarsi di essere a conoscenza delle limitazioni del supporto per gli ambienti con accuratezza elevata, come descritto nell'articolo 939322 della Microsoft Knowledge base, supporto dei limiti [per configurare il servizio ora di Windows per ambienti ad alta precisione](support-boundary.md).  
  
-   Per configurare il servizio ora di Windows in tutti i computer client o server basati su Windows configurati come membri del gruppo di lavoro anziché come membri del dominio, vedere [configurare un'origine dell'ora manuale per un computer client selezionato](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-r2-and-2008/cc816656%28v%3dws.10%29).  
  
-   Per configurare il servizio ora di Windows in un computer host in cui viene eseguito un ambiente virtuale, vedere l'articolo 816042 della Microsoft Knowledge base, [come configurare un server di ora autorevole in Windows Server](https://go.microsoft.com/fwlink/?LinkID=60402). Se si utilizza un prodotto di virtualizzazione non Microsoft, assicurarsi di consultare la documentazione del fornitore del prodotto.  
  
-   Per configurare il servizio ora di Windows in un controller di dominio in esecuzione in una macchina virtuale, è consigliabile disabilitare parzialmente la sincronizzazione dell'ora tra il sistema host e il sistema operativo guest che funge da controller di dominio. In questo modo, il controller di dominio Guest può sincronizzare l'ora per la gerarchia di domini, ma protegge dalla presenza di uno sfasamento dell'ora se viene ripristinato da uno stato salvato. Per ulteriori informazioni, vedere l'articolo 976924 della Microsoft Knowledge base, [che riceve gli ID evento del servizio ora di Windows 24, 29 e 38 in un controller di dominio virtualizzato in esecuzione su un server host basato su Windows server 2008 con Hyper-V](https://go.microsoft.com/fwlink/?LinkID=192236) e [distribuzione Considerazioni per i controller di dominio virtualizzati](https://go.microsoft.com/fwlink/?LinkID=192235).  
  
-   Per configurare il servizio ora di Windows in un controller di dominio che funge da emulatore PDC radice della foresta eseguito anche in un computer virtuale, seguire le stesse istruzioni per un computer fisico come descritto in [configurare il servizio ora di Windows nel PDC emulatore nel dominio radice della foresta](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc731191%28v=ws.10%29).  
  
-   Per configurare il servizio ora di Windows in un server membro che esegue come computer virtuale, utilizzare la gerarchia temporale del dominio come descritto in ([configurare un computer client per la sincronizzazione automatica dell'ora di dominio](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-r2-and-2008/cc816884%28v%3dws.10%29).  
  
## <a name="BKMK_WTS"></a>Che cos'è il servizio ora di Windows?  
Il servizio ora di Windows (W32Time) fornisce la sincronizzazione dell'orologio di rete per i computer senza la necessità di una configurazione completa.  
  
Il servizio ora di Windows è essenziale per il corretto funzionamento dell'autenticazione Kerberos versione 5 e, pertanto, per l'autenticazione basata su servizi di dominio Active Directory. Qualsiasi applicazione in grado di riconoscere Kerberos, inclusa la maggior parte dei servizi di sicurezza, si basa sulla sincronizzazione dell'ora tra i computer che partecipano alla richiesta di autenticazione. I controller di dominio di servizi di dominio Active Directory devono avere anche orologi sincronizzati per garantire la corretta replica dei dati.  
  
Il servizio ora di Windows viene implementato in una libreria a collegamento dinamico denominata W32Time. dll. W32Time. dll viene installato per impostazione predefinita nella cartella **%SystemRoot%\System32.** durante l'installazione e l'installazione del sistema operativo.  
  
W32Time. dll è stato sviluppato originariamente per Windows 2000 Server per supportare una specifica dal protocollo di autenticazione Kerberos V5 che richiedeva la sincronizzazione degli orologi in una rete. A partire da Windows Server 2003, W32Time. dll ha fornito una maggiore precisione nella sincronizzazione dell'orologio di rete sul sistema operativo Windows 2000 Server e, inoltre, supportava un'ampia gamma di dispositivi hardware e protocolli di tempo di rete per mezzo del tempo provider. Sebbene originariamente progettato per fornire la sincronizzazione dell'orologio per l'autenticazione Kerberos, molte applicazioni correnti utilizzano timestamp per garantire la coerenza delle transazioni, registrare l'ora di eventi importanti e altre attività critiche per il tempo informazioni. Queste applicazioni traggono vantaggio dalla sincronizzazione dell'ora tra i computer forniti dal servizio ora di Windows.  
  
## <a name="BKMK_TimeProtocols"></a>Importanza dei protocolli temporali  
I protocolli temporali comunicano tra due computer per scambiare informazioni sull'ora e quindi utilizzano tali informazioni per sincronizzare gli orologi. Con il protocollo ora servizio ora di Windows, un client richiede informazioni sull'ora da un server e sincronizza l'orologio in base alle informazioni ricevute.  
  
Il servizio ora di Windows usa NTP per facilitare la sincronizzazione dell'ora in una rete. NTP è un protocollo di tempo Internet che include gli algoritmi di disciplina necessari per la sincronizzazione degli orologi. NTP è un protocollo di tempo più preciso rispetto al protocollo SNTP (Simple Network Time Protocol) utilizzato in alcune versioni di Windows. Tuttavia, W32Time continua a supportare SNTP per consentire la compatibilità con le versioni precedenti dei computer che eseguono servizi ora basati su SNTP come Windows 2000.  
  
## <a name="see-also"></a>Vedere anche  
[Funzionamento del servizio ora di Windows](How-the-Windows-Time-Service-Works.md)  
[Strumenti e impostazioni del servizio Ora di Windows](Windows-Time-Service-Tools-and-Settings.md)  
[Articolo 902229 della Microsoft Knowledge base](https://go.microsoft.com/fwlink/?LinkId=186066)