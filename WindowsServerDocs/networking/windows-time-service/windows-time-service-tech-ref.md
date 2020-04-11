---
ms.assetid: e34622ff-b2d0-4f81-8d00-dacd5d6c215e
title: Riferimento tecnico per Windows Time Service
description: Il servizio W32Time fornisce la sincronizzazione dei clock di rete per i computer senza la necessità di una configurazione complessa. Il servizio W32Time è essenziale per il corretto funzionamento dell'autenticazione Kerberos V5 e, pertanto, per l'autenticazione basata su Active Directory Domain Services.
author: dcuomo
ms.author: dacuo
ms.date: 05/08/2018
ms.topic: article
ms.prod: windows-server
ms.technology: networking
ms.openlocfilehash: ec141fe8a5e2b1acf0a9f6627dd2d394382424bb
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80859824"
---
# <a name="windows-time-service-technical-reference"></a>Riferimento tecnico per Windows Time Service
>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows 10 o versioni successive

Il servizio W32Time fornisce la sincronizzazione dei clock di rete per i computer senza la necessità di una configurazione complessa. Il servizio W32Time è essenziale per il corretto funzionamento dell'autenticazione Kerberos V5 e, pertanto, per l'autenticazione basata su Active Directory Domain Services. Qualsiasi applicazione in grado di riconoscere Kerberos, inclusa la maggior parte dei servizi di sicurezza, si basa sulla sincronizzazione dell'ora tra i computer che partecipano alla richiesta di autenticazione. Anche i controller di dominio di Active Directory Domain Services devono avere clock sincronizzati per garantire l'accuratezza della replica dei dati.

> [!NOTE]  
> In Windows Server 2003 e Microsoft Windows 2000 Server, il servizio directory è denominato servizio directory Active Directory. In Windows Server 2008 R2 e Windows Server 2008 il servizio directory è denominato Active Directory Domain Services. Nel resto di questo argomento si fa riferimento ad Active Directory Domain Services, ma le informazioni sono applicabili anche al servizio directory di Windows Server 2016.

Il servizio W32Time viene implementato in una libreria di collegamento dinamico denominata W32Time.dll, che viene installata per impostazione predefinita in **%SystemRoot%\System32**. La libreria W32Time.dll è stata sviluppata in origine per Windows 2000 Server per supportare una specifica del protocollo di autenticazione Kerberos V5 che richiede la sincronizzazione dei clock in una rete. A partire da Windows Server 2003, W32Time.dll ha fornito una maggiore accuratezza nella sincronizzazione dei clock di rete sul sistema operativo Windows Server 2000. Inoltre, in Windows Server 2003 W32Time.dll supporta un'ampia gamma di dispositivi hardware e protocolli NTP che usano provider servizi orari.

Sebbene originariamente progettate per fornire la sincronizzazione dei clock per l'autenticazione Kerberos, molte applicazioni correnti usano timestamp per garantire la coerenza delle transazioni, registrare l'ora di eventi importanti e fornire altre informazioni basate sull'ora importanti per l'azienda.  Queste applicazioni usufruiscono della sincronizzazione dell'ora tra computer con il servizio Ora di Windows.

## <a name="importance-of-time-protocols"></a>Importanza dei protocolli di tempo
I protocolli di tempo comunicano tra due computer per scambiare informazioni sull'ora e quindi usano tali informazioni per sincronizzare i clock. Con il protocollo di tempo del servizio Ora di Windows, un client richiede informazioni sull'ora a un server e sincronizza il clock in base alle informazioni ricevute.
  
Il servizio Ora di Windows usa il protocollo NTP per sincronizzare l'ora in una rete. NTP è un protocollo di tempo Internet che include gli algoritmi di disciplina necessari per la sincronizzazione degli orologi. NTP è un protocollo di tempo più accurato rispetto al protocollo SNTP (Simple Network Time Protocol) che viene usato in alcune versioni di Windows. W32Time tuttavia continua a supportare SNTP per garantire la compatibilità con le versioni precedenti in computer che eseguono servizi ora basati su SNTP, ad esempio Windows 2000.
## <a name="where-to-find-windows-time-service-configuration-related-information"></a>Dove trovare informazioni relative alla configurazione del servizio Ora di Windows  
Questa guida **non** illustra la configurazione del servizio Ora di Windows. In Microsoft TechNet e nella Microsoft Knowledge Base sono disponibili diversi argomenti che illustrano le procedure per la configurazione di tale servizio. Se necessiti di informazioni sulla configurazione, gli argomenti seguenti consentono di individuare le informazioni appropriate.  
-   Per configurare il servizio Ora di Windows per l'emulatore del controller di dominio primario (PDC) radice della foresta, vedi:
  
    -   [Configurare il servizio Ora di Windows sull'emulatore PDC nel dominio radice della foresta](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc731191%28v=ws.10%29) 
  
    -   [Configurazione di un'origine ora per la foresta](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-r2-and-2008/cc794823%28v%3dws.10%29) 
  
    -   L'articolo 816042 della Microsoft Knowledge Base, [Come configurare un server di riferimento ora autorevole in Windows Server](https://go.microsoft.com/fwlink/?LinkID=60402), che descrive le impostazioni di configurazione per i computer che eseguono Windows Server 2008 R2, Windows Server 2008, Windows Server 2003 e Windows Server 2003 R2.  
  
-   Per configurare il servizio Ora di Windows in qualsiasi server o client membro del dominio o anche nei controller di dominio non configurati come emulatore PDC radice della foresta, vedi [Configurare un computer client per la sincronizzazione automatica dell'ora del dominio](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-r2-and-2008/cc816884%28v%3dws.10%29).  
  
    > [!WARNING]  
    > Per alcune applicazioni potrebbe essere necessario che i computer dispongano di servizi ora con accuratezza elevata. In tal caso, puoi scegliere di configurare un'origine ora manuale, ma tieni presente che il servizio Ora di Windows non è stato progettato per funzionare come origine ora con accuratezza elevata. Assicurati di essere a conoscenza delle limitazioni del supporto per gli ambienti con accuratezza elevata, come descritto nell'articolo 939322 della Microsoft Knowledge Base, [Limiti di supporto per configurare il servizio Ora di Windows per gli ambienti con accuratezza elevata](support-boundary.md).  
  
-   Per configurare il servizio Ora di Windows in tutti i computer server o client basati su Windows configurati come membri di un gruppo di lavoro anziché come membri di un dominio, vedi [Configurare un'origine ora manuale per un computer client selezionato](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-r2-and-2008/cc816656%28v%3dws.10%29).  
  
-   Per configurare il servizio Ora di Windows in un computer host in cui viene eseguito un ambiente virtuale, vedi l'articolo 816042 della Microsoft Knowledge Base, [Come configurare un server di riferimento ora autorevole in Windows Server](https://go.microsoft.com/fwlink/?LinkID=60402). Se usi un prodotto di virtualizzazione non Microsoft, consulta la documentazione del fornitore del prodotto.  
  
-   Per configurare il servizio Ora di Windows in un controller di dominio in esecuzione in una macchina virtuale, è consigliabile disabilitare parzialmente la sincronizzazione dell'ora tra il sistema host e il sistema operativo guest che funge da controller di dominio. In questo modo, il controller di dominio guest può sincronizzare l'ora per la gerarchia di domini, evitando che si verifichi uno sfasamento dell'ora in caso di ripristino da uno stato salvato. Per altre informazioni, vedi l'articolo 976924 della Microsoft Knowledge Base, [Vengono visualizzati gli ID evento del servizio Ora di Windows 24, 29 e 38 in un controller di dominio virtualizzato in esecuzione su un server host basato su Windows Server 2008 con Hyper-V](https://go.microsoft.com/fwlink/?LinkID=192236), e [Considerazioni sulla distribuzione per i controller di dominio virtualizzati](https://go.microsoft.com/fwlink/?LinkID=192235).  
  
-   Per configurare il servizio Ora di Windows in un controller di dominio che funge da emulatore PDC radice della foresta eseguito anche in un computer virtuale, segui le stesse istruzioni relative a un computer fisico come descritto in [Configurare il servizio Ora di Windows nell'emulatore PDC nel dominio radice della foresta](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc731191%28v=ws.10%29).  
  
-   Per configurare il servizio Ora di Windows in un server membro eseguito come computer virtuale, usa la gerarchia temporale del dominio come descritto in [Configurare un computer client per la sincronizzazione automatica dell'ora del dominio](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-r2-and-2008/cc816884%28v%3dws.10%29).


> [!IMPORTANT]  
> Nelle versioni antecedenti a Windows Server 2016 il servizio W32Time non è progettato per soddisfare le esigenze delle applicazioni dipendenti dall'ora.  Tuttavia, gli aggiornamenti a Windows Server 2016 consentono ora di implementare una soluzione per l'accuratezza di 1 ms nel dominio.  Per altre informazioni, vedi [Ora esatta Windows 2016](accurate-time.md) e [Limiti di supporto per configurare il servizio Ora di Windows per gli ambienti con accuratezza elevata](support-boundary.md).

## <a name="related-topics"></a>Argomenti correlati
- [Ora esatta Windows 2016](accurate-time.md)
- [Miglioramenti dell'accuratezza dell'ora per Windows Server 2016](windows-server-2016-improvements.md)  
- [Funzionamento del servizio Ora di Windows](How-the-Windows-Time-Service-Works.md)  
- [Strumenti e impostazioni del servizio Ora di Windows](Windows-Time-Service-Tools-and-Settings.md)  
- [Limiti di supporto per configurare il servizio Ora di Windows per gli ambienti con accuratezza elevata](support-boundary.md)
- [Articolo 902229 della Microsoft Knowledge Base](https://go.microsoft.com/fwlink/?LinkId=186066)
