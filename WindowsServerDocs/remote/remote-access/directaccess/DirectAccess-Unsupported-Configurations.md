---
title: Configurazioni non supportate da DirectAccess
description: In questo argomento viene fornito un elenco di configurazioni DirectAccess non supportate in Windows Server 2016.
manager: brianlic
ms.prod: windows-server
ms.technology: networking-da
ms.topic: article
ms.assetid: 23d05e61-95c3-4e70-aa83-b9a8cae92304
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 5e652083d4accf90b542a16d51e314299303954f
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71388836"
---
# <a name="directaccess-unsupported-configurations"></a>Configurazioni non supportate da DirectAccess

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016

Prima di iniziare la distribuzione, esaminare l'elenco seguente di configurazioni DirectAccess non supportate per evitare di dover avviare di nuovo la distribuzione.  

## <a name="bkmk_frs"></a>Distribuzione del servizio Replica file (FRS) di oggetti Criteri di gruppo (repliche SYSVOL)  
Non distribuire DirectAccess negli ambienti in cui i controller di dominio eseguono il servizio Replica file (FRS) per la distribuzione di oggetti Criteri di gruppo (repliche SYSVOL). La distribuzione di DirectAccess non è supportata quando si usa il servizio Replica file.  
  
Si utilizza FRS se si dispone di controller di dominio che eseguono Windows Server 2003 o Windows Server 2003 R2. Inoltre, è possibile che si stia utilizzando FRS se in precedenza sono stati utilizzati i controller di dominio Windows 2000 Server o Windows Server 2003 e non è mai stata eseguita la migrazione della replica SYSVOL da FRS a file system distribuito replica (DFS-R).  
  
Se si distribuisce DirectAccess con la replica SYSVOL di FRS, si rischia l'eliminazione non intenzionale di oggetti Criteri di gruppo DirectAccess che contengono le informazioni di configurazione del server e del client DirectAccess. Se questi oggetti vengono eliminati, la distribuzione di DirectAccess subirà un'interruzione e i computer client che utilizzano DirectAccess non saranno in grado di connettersi alla rete.  
  
Se si prevede di distribuire DirectAccess, è necessario utilizzare controller di dominio che eseguono sistemi operativi successivi a Windows Server 2003 R2 ed è necessario utilizzare DFS-R.  
  
Per informazioni sulla migrazione da FRS a DFS-R, vedere la [Guida alla migrazione della replica di SYSVOL: FRS to replica DFS](https://technet.microsoft.com/library/dd640019(v=ws.10).aspx).  
  
## <a name="bkmk_nap"></a>Protezione accesso alla rete per i client DirectAccess  
Protezione accesso alla rete (NAP) viene usato per determinare se i computer client remoti soddisfano i criteri IT prima di concedere l'accesso alla rete aziendale. NAP è stato deprecato in Windows Server 2012 R2 e non è incluso in Windows Server 2016. Per questo motivo, non è consigliabile avviare una nuova distribuzione di DirectAccess con protezione accesso alla rete. È consigliabile usare un metodo diverso per il controllo di endpoint per la sicurezza dei client DirectAccess.  
  
## <a name="bkmk_multi"></a>Supporto multisito per i client Windows 7  
Quando DirectAccess è configurato in una distribuzione multisito, i client Windows 10&reg;, Windows&reg; 8,1 e Windows&reg; 8 hanno la possibilità di connettersi al sito più vicino.  Windows 7&reg; i computer client non hanno la stessa funzionalità. La selezione del sito per i client Windows 7 è impostata su un sito specifico al momento della configurazione dei criteri e questi client si connetteranno sempre al sito designato, indipendentemente dalla loro posizione.  
  
## <a name="bkmk_user"></a>Controllo degli accessi in base all'utente  
I criteri DirectAccess sono basati su computer, non sulla base dell'utente. La specifica dei criteri utente DirectAccess per controllare l'accesso alla rete aziendale non è supportata.  
  
## <a name="bkmk_policy"></a>Personalizzazione dei criteri DirectAccess  
DirectAccess può essere configurato tramite la configurazione guidata DirectAccess, la console di gestione accesso remoto o i cmdlet di Windows PowerShell per accesso remoto. Non è supportato l'utilizzo di un metodo diverso dalla configurazione guidata DirectAccess per configurare DirectAccess, ad esempio la modifica di oggetti DirectAccess Criteri di gruppo direttamente o la modifica manuale delle impostazioni dei criteri predefinite nel server o nel client. Queste modifiche possono causare una configurazione inutilizzabile.  
  
## <a name="bkmk_kerb"></a>Autenticazione KerbProxy  
Quando si configura un server DirectAccess con la procedura guidata Introduzione, il server DirectAccess viene configurato automaticamente per usare l'autenticazione KerbProxy per l'autenticazione del computer e dell'utente. Per questo motivo, è consigliabile usare la procedura guidata Introduzione solo per le distribuzioni a sito singolo in cui vengono distribuiti solo i client Windows 10&reg;, Windows 8.1 o Windows 8.  
  
Inoltre, le funzionalità seguenti non devono essere usate con l'autenticazione di KerbProxy:  
  
-   Bilanciamento del carico usando un servizio di bilanciamento del carico esterno o un carico di Windows   
    Equilibratrice  
  
-   Autenticazione a due fattori in cui sono richieste smart card o una password monouso (OTP)  
  
Se si Abilita l'autenticazione KerbProxy, i piani di distribuzione seguenti non sono supportati:  
  
-   Multisito.  
  
-   Supporto di DirectAccess per i client Windows 7.  
  
-   Imposizione del tunneling. Per assicurarsi che l'autenticazione KerbProxy non sia abilitata quando si usa il tunneling forzato, configurare gli elementi seguenti durante l'esecuzione della procedura guidata:  
  
    -   Abilitare il tunneling forzato  
  
    -   Abilita DirectAccess per client Windows 7  
  
> [!NOTE]  
> Per le distribuzioni precedenti, è necessario usare la configurazione guidata avanzata, che usa una configurazione a due tunnel con un'autenticazione utente e un computer basato su certificati. Per ulteriori informazioni, vedere la pagina relativa alla [distribuzione di un server DirectAccess singolo con impostazioni avanzate](../../remote-access/directaccess/single-server-advanced/Deploy-a-Single-DirectAccess-Server-with-Advanced-Settings.md).  
  
## <a name="bkmk_isa"></a>Uso di ISATAP  
ISATAP è una tecnologia di transizione che offre connettività IPv6 in reti aziendali solo IPv4. È limitato alle organizzazioni di piccole e medie dimensioni con una singola distribuzione del server DirectAccess e consente la gestione remota dei client DirectAccess. Se ISATAP viene distribuito in un ambiente multidominio, di bilanciamento del carico o multidominio, è necessario rimuoverlo o spostarlo in una distribuzione IPv6 nativa prima di configurare DirectAccess.  
  
## <a name="bkmk_iphttps"></a>Configurazione dell'endpoint IPHTTPS e una password monouso (OTP)  
Quando si usa IPHTTPS, la connessione IPHTTPS deve terminare sul server DirectAccess, non su qualsiasi altro dispositivo, ad esempio un servizio di bilanciamento del carico. Analogamente, la connessione Secure Sockets Layer fuori banda (SSL) creata durante l'autenticazione di una password monouso (OTP) deve terminare nel server DirectAccess. Tutti i dispositivi tra gli endpoint di queste connessioni devono essere configurati in modalità pass-through.  
  
## <a name="bkmk_ft"></a>Imponi tunnel con autenticazione OTP  
Non distribuire un server DirectAccess con autenticazione a due fattori con OTP e il tunneling forzato. in caso contrario, l'autenticazione OTP avrà esito negativo. È necessaria una connessione di Secure Sockets Layer (SSL) fuori banda tra il server DirectAccess e il client DirectAccess. Questa connessione richiede un'esenzione per l'invio del traffico all'esterno del tunnel DirectAccess. In una configurazione del tunneling forzato, tutto il traffico deve passare attraverso un tunnel DirectAccess e non è consentita alcuna esenzione dopo che è stato stabilito il tunnel. Per questo motivo, non è supportata l'autenticazione OTP in una configurazione con tunneling forzato.  
  
## <a name="bkmk_rodc"></a>Distribuzione di DirectAccess con un controller di dominio di sola lettura  
I server DirectAccess devono avere accesso a un controller di dominio di lettura/scrittura e non funzionano correttamente con un controller di dominio di sola lettura (RODC).  
  
Per diversi motivi è necessario un controller di dominio di lettura/scrittura, inclusi i seguenti:  
  
-   Nel server DirectAccess è necessario un controller di dominio di lettura/scrittura per aprire Microsoft Management Console (MMC) di accesso remoto.  
  
-   Il server DirectAccess deve sia in lettura che in scrittura nel client DirectAccess e nel server DirectAccess Criteri di gruppo oggetti (GPO).  
  
-   Il server DirectAccess legge e scrive nell'oggetto Criteri di gruppo del client specifico dall'emulatore del controller di dominio primario (emulatore PDC).  
  
A causa di questi requisiti, non distribuire DirectAccess con un RODC.  
  


