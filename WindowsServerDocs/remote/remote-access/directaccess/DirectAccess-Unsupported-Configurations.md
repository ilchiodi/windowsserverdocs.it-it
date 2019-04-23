---
title: Configurazioni non supportate da DirectAccess
description: In questo argomento fornisce un elenco di configurazioni non supportate di DirectAccess in Windows Server 2016.
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-da
ms.topic: article
ms.assetid: 23d05e61-95c3-4e70-aa83-b9a8cae92304
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 49fa86883e6a590ac8f7dcf0c724f87af419f88e
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59828852"
---
# <a name="directaccess-unsupported-configurations"></a>Configurazioni non supportate da DirectAccess

>Si applica a: Windows Server (canale semestrale), Windows Server 2016

Esaminare l'elenco seguente di configurazioni non supportate di DirectAccess prima di iniziare la distribuzione per evitare la necessità di avviare nuovamente la distribuzione.  

## <a name="bkmk_frs"></a>Distribuzione del servizio Replica file degli oggetti Criteri di gruppo (repliche SYSVOL)  
Non distribuire DirectAccess in ambienti in cui i controller di dominio sono in esecuzione il servizio Replica File (FRS) per la distribuzione di oggetti Criteri di gruppo (repliche SYSVOL). Distribuzione di DirectAccess non è supportata quando si utilizza FRS.  
  
Si usa il servizio Replica file se si dispone di controller di dominio che eseguono Windows Server 2003 o Windows Server 2003 R2. Inoltre, potrebbe essere in uso FRS se usato in precedenza il controller di dominio di Windows Server 2003 o Windows 2000 Server e si mai eseguita la replica di SYSVOL dal servizio Replica file per Distributed File System Replication (DFS-R).  
  
Se si distribuisce DirectAccess con la replica di FRS SYSVOL, si rischia di eliminazione accidentale di criteri di gruppo DirectAccess oggetti che contengono il server DirectAccess e le informazioni di configurazione client. Se questi oggetti vengono eliminati, la distribuzione di DirectAccess subirà un'interruzione del servizio e i computer client che usano DirectAccess non sarà in grado di connettersi alla rete.  
  
Se si prevede di distribuire DirectAccess, è necessario usare i controller di dominio che eseguono sistemi operativi in un secondo momento rispetto a Windows Server 2003 R2 ed è necessario utilizzare DFS-R.  
  
Per informazioni sulla migrazione da FRS a DFS-R, vedere il [Guida alla migrazione di replica di SYSVOL: Replica da FRS a DFS](https://technet.microsoft.com/library/dd640019(v=ws.10).aspx).  
  
## <a name="bkmk_nap"></a>Protezione accesso alla rete per i client DirectAccess  
Protezione accesso rete (NAP) viene utilizzato per determinare se i computer client remoti soddisfano i criteri IT prima concesso l'accesso alla rete aziendale. Protezione accesso alla rete è stata deprecata in Windows Server 2012 R2 e non è incluso in Windows Server 2016. Per questo motivo, non è consigliabile avviare una nuova distribuzione di DirectAccess con protezione accesso alla rete. È consigliabile un altro metodo di controllo del punto di fine per la sicurezza dei client DirectAccess.  
  
## <a name="bkmk_multi"></a>Supporto multisito per i client Windows 7  
Quando DirectAccess è configurato in una distribuzione multisito, Windows 10&reg;, Windows&reg; 8.1 e Windows&reg; 8 client sono in grado di connettersi al sito più vicino.  Windows 7&reg; computer client non hanno le stesse funzionalità. La selezione del sito per i client Windows 7 è impostata su un particolare sito al momento della configurazione dei criteri e questi client si connetteranno sempre a tale sito designato, indipendentemente dalla loro posizione.  
  
## <a name="bkmk_user"></a>Controllo degli accessi basato su utente  
I criteri DirectAccess sono computer di base, non dell'utente in base. Specifica i criteri utente di DirectAccess per controllare l'accesso alla rete aziendale non è supportata.  
  
## <a name="bkmk_policy"></a>Personalizzazione dei criteri DirectAccess  
DirectAccess può essere configurato tramite configurazione guidata DirectAccess, la console Gestione accesso remoto o i cmdlet di PowerShell di Windows di accesso remoto. Uso di strumenti diversi da Configurazione guidata DirectAccess per configurare DirectAccess, ad esempio modificando direttamente gli oggetti Criteri di gruppo DirectAccess o modificare manualmente le impostazioni di criteri predefinite nel server o nel client, non è supportato. Queste modifiche possono comportare una configurazione inutilizzabile.  
  
## <a name="bkmk_kerb"></a>Autenticazione KerbProxy  
Quando si configura un server DirectAccess con attività iniziali guidate, il server DirectAccess viene configurato automaticamente per usare l'autenticazione KerbProxy per computer e l'autenticazione dell'utente. Per questo motivo, è necessario utilizzare solo attività iniziali guidate per distribuzioni a singolo sito solo Windows 10&reg;, Windows 8.1 o Windows 8 client vengono distribuiti.  
  
Inoltre, le funzionalità seguenti non usare con l'autenticazione KerbProxy:  
  
-   Il bilanciamento del carico usando un servizio di bilanciamento del carico esterno o carico di Windows   
    Servizio di bilanciamento  
  
-   Autenticazione a due fattori in cui sono necessarie le smart card o una password monouso (OTP)  
  
I piani di distribuzione seguenti non sono supportati se si abilita l'autenticazione KerbProxy:  
  
-   Distribuzione multisito.  
  
-   DirectAccess supporta per i client Windows 7.  
  
-   Tunneling forzato. Per garantire che l'autenticazione KerbProxy non è abilitato quando si usa il tunneling forzato, configurare gli elementi seguenti durante l'esecuzione della procedura guidata:  
  
    -   Abilitare il tunneling forzato  
  
    -   Abilitare i client DirectAccess per Windows 7  
  
> [!NOTE]  
> Per le distribuzioni precedenti, utilizzare la configurazione avanzata guidata, che usa una configurazione di due tunnel con un certificato computer e utente l'autenticazione basata su. Per altre informazioni, vedere [distribuire un DirectAccess Server singolo con impostazioni avanzate](../../remote-access/directaccess/single-server-advanced/Deploy-a-Single-DirectAccess-Server-with-Advanced-Settings.md).  
  
## <a name="bkmk_isa"></a>Usa la tecnologia ISATAP  
ISATAP è una tecnologia di transizione che fornisce connettività IPv6 in reti aziendali solo IPv4. È limitato alle organizzazioni di piccole e medie dimensioni con una distribuzione del server DirectAccess singolo e consente la gestione remota dei client DirectAccess. Se ISATAP è deployedin un distribuzione multisito, il bilanciamento del carico o all'ambiente con più domini, è necessario rimuoverlo o spostarlo in una distribuzione di IPv6 nativa prima di configurare DirectAccess.  
  
## <a name="bkmk_iphttps"></a>Configurazione di endpoint di One Time password (OTP) e IPHTTPS  
Quando si usa IPHTTPS, è necessario terminare la connessione IPHTTPS nel server DirectAccess, non su qualsiasi dispositivo, ad esempio un servizio di bilanciamento del carico. Analogamente, la connessione di out-of-band Secure Sockets Layer (SSL) che viene creata durante l'autenticazione One Time password (OTP) deve terminare nel server DirectAccess. Tutti i dispositivi tra gli endpoint di queste connessioni devono essere configurati in modalità pass-through.  
  
## <a name="bkmk_ft"></a>Eseguire il tunneling forzato con l'autenticazione OTP  
Non distribuire un server DirectAccess con autenticazione a due fattori con OTP e il Tunneling forzato, o l'autenticazione OTP avrà esito negativo. Una connessione Secure Sockets Layer (SSL) out-of-band è necessaria tra il server DirectAccess e il client DirectAccess. Questa connessione richiede un'esenzione per inviare il traffico di fuori del tunnel di DirectAccess. In una configurazione con tunneling forzato, tutto il traffico deve passare attraverso un tunnel di DirectAccess e Nessuna esenzione è consentita dopo aver stabilito il tunnel. Per questo motivo, non è in conflitto con l'autenticazione OTP in una configurazione con tunneling forzato.  
  
## <a name="bkmk_rodc"></a>Distribuzione di DirectAccess con un Controller di dominio di sola lettura  
I server DirectAccess devono avere accesso a un controller di dominio di lettura / scrittura e non funzionano correttamente con un Controller di dominio di sola lettura (RODC).  
  
Un controller di dominio di lettura / scrittura è necessario per diversi motivi, inclusi i seguenti:  
  
-   Nel server DirectAccess, per aprire il Microsoft Management Console (MMC) l'accesso remoto è necessario un controller di dominio di lettura / scrittura.  
  
-   Il server DirectAccess deve sia di lettura e scrittura per il client DirectAccess e server DirectAccess oggetti Criteri di gruppo (GPO).  
  
-   Il server DirectAccess legge e scrive al client oggetto Criteri di gruppo in particolare dall'emulatore del Controller di dominio primario (emulatore PDC).  
  
A causa di questi requisiti, non distribuire DirectAccess con un RODC.  
  


