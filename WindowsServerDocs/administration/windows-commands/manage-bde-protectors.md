---
title: gestire-bde protezioni
description: 'Argomento i comandi di Windows per * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 1f9b22c5-cc93-45df-9165-bedee94998da
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 08/06/2018
ms.openlocfilehash: 78c0b338aa34684cc3c5da5ae4493f3526537ee6
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/31/2019
ms.locfileid: "66437503"
---
# <a name="manage-bde-protectors"></a>manage-bde: protectors

>Si applica a: Windows Server (canale semestrale), Windows Server 2016

Gestisce i metodi di protezione utilizzati per la chiave di crittografia BitLocker. Per esempi di come è possibile utilizzare questo comando, vedere [esempi](#BKMK_Examples).
## <a name="syntax"></a>Sintassi
```
manage-bde -protectors [{-get|-add|-delete|-disable|-enable|-adbackup|-aadbackup}] <Drive> [-computername <Name>] [{-?|/?}] [{-help|-h}]
```
### <a name="parameters"></a>Parametri

|   Parametro   |                                                                                                                                                                                           Descrizione                                                                                                                                                                                            |
|---------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|     -Introduzione      |                                                                                                                                            Visualizza tutti i metodi di protezione con chiave abilitati per l'unità e fornisce il tipo e l'identificatore (ID).                                                                                                                                             |
|     -aggiungere      |                                                                                                                                   Aggiunge metodi di protezione con chiave specificato utilizzando aggiuntivi [aggiungere i parametri](manage-bde-protectors.md#BKMK_addprotectors).                                                                                                                                    |
|    -Elimina    | Elimina i metodi di protezione della chiave utilizzati da BitLocker. Tutte le protezioni con chiave verrà rimossa da un'unità a meno che non facoltativo [-eliminare parametri](manage-bde-protectors.md#BKMK_deleteprotectors) vengono utilizzati per specificare quali programmi di protezione da eliminare. Quando viene eliminata l'ultimo protezione in un'unità, la protezione BitLocker dell'unità è disabilitata per assicurare che l'accesso ai dati non vengano perso accidentalmente. |
|   -Disabilita    |                      Disabilita la protezione, che consentirà a chiunque di accedere ai dati crittografati rendendo disponibile la chiave di crittografia non protette sul disco. Nessuna protezione con chiave vengono rimossi. Protezione verrà ripresa al successivo avvio di Windows a meno che non facoltativo [-disabilitare parametri](manage-bde-protectors.md#BKMK_disableprot) vengono utilizzati per specificare il numero di riavvio.                       |
|    -abilitare    |                                                                                                                             Abilita la protezione rimuovendo la chiave di crittografia non protetta dall'unità. Verranno applicate tutte le protezioni con chiave configurata nell'unità.                                                                                                                             |
|   -adbackup   |                                                                          Esegue il backup di tutte le informazioni di ripristino per l'unità specificata in servizi di dominio Active Directory (AD DS). Per eseguire il backup solo una chiave di ripristino solo di dominio Active Directory, aggiungere il **-id** parametro e specificare l'ID di una chiave di ripristino specifico per eseguire il backup.                                                                           |
|  -aadbackup   |                                                                            Esegue il backup di tutte le informazioni di ripristino per l'unità specificata in Azure Active Directory (Azure Ad). Per eseguire il backup solo una chiave di ripristino solo in Azure AD, aggiungere il **-id** parametro e specificare l'ID di una chiave di ripristino specifici per eseguire il backup.                                                                             |
|    <Drive>    |                                                                                                                                                                          Rappresenta una lettera di unità seguita da due punti.                                                                                                                                                                          |
| -computername |                                                                                                              Specifica che gestire bde.exe verrà usato per modificare la protezione BitLocker su un computer diverso. È inoltre possibile utilizzare **- cn** come una versione abbreviata di questo comando.                                                                                                              |
|    <Name>     |                                                                                                                 Rappresenta il nome del computer in cui si desidera modificare la protezione BitLocker. I valori accettati includono nome NetBIOS del computer e l'indirizzo IP del computer.                                                                                                                  |
|   -? o /?    |                                                                                                                                                                            Consente di visualizzare breve guida al prompt dei comandi.                                                                                                                                                                            |
|  -help o -h  |                                                                                                                                                                          Visualizza la Guida al prompt dei comandi completa.                                                                                                                                                                           |

### <a name="BKMK_addprotectors"></a>-aggiungere la sintassi e parametri
```
manage-bde  protectors  add [<Drive>] [-forceupgrade] [-recoverypassword <NumericalPassword>] [-recoverykey <pathToExternalKeydirectory>]
[-startupkey <pathToExternalKeydirectory>] [-certificate {-cf <pathToCertificateFile>|-ct <CertificateThumbprint>}] [-tpm] [-tpmandpin] 
[-tpmandstartupkey <pathToExternalKeydirectory>] [-tpmandpinandstartupkey <pathToExternalKeydirectory>] [-password][-adaccountorgroup <securityidentifier> [-computername <Name>] 
[{-?|/?}] [{-help|-h}]
```

|          Parametro           |                                                                                                                                                                                   Descrizione                                                                                                                                                                                   |
|------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|           <Drive>            |                                                                                                                                                                 Rappresenta una lettera di unità seguita da due punti.                                                                                                                                                                  |
|      -recoverypassword       |                                                                                                                                    Aggiunge una protezione con password numerica. È inoltre possibile utilizzare **- rp** come una versione abbreviata di questo comando.                                                                                                                                     |
|     <NumericalPassword>      |                                                                                                                                                                        Rappresenta la password di ripristino.                                                                                                                                                                        |
|         -recoverykey         |                                                                                                                                Aggiunge una protezione con chiave esterna per il ripristino. È inoltre possibile utilizzare **- rk** come una versione abbreviata di questo comando.                                                                                                                                 |
| <pathToExternalKeydirectory> |                                                                                                                                                               Rappresenta il percorso della directory per la chiave di ripristino.                                                                                                                                                                |
|         -chiave di avvio          |                                                                                                                                 Aggiunge una protezione con chiave esterna per l'avvio. È inoltre possibile utilizzare **-sk** come una versione abbreviata di questo comando.                                                                                                                                 |
| <pathToExternalKeydirectory> |                                                                                                                                                                Rappresenta il percorso della directory per la chiave di avvio.                                                                                                                                                                |
|         -certificato         |                                                                                                                               Aggiunge una protezione con chiave pubblica per un'unità dati. È inoltre possibile utilizzare **-cert** come una versione abbreviata di questo comando.                                                                                                                               |
|             -cf              |                                                                                                                                              Specifica un file di certificato verrà utilizzato per fornire il certificato chiave pubblica.                                                                                                                                              |
|   <pathToCertificateFile>    |                                                                                                                                                             Rappresenta il percorso della directory del file di certificato.                                                                                                                                                              |
|             -ct              |                                                                                                                                           Specifica un'identificazione personale del certificato verrà utilizzato per identificare il certificato chiave pubblica                                                                                                                                           |
|   <CertificateThumbprint>    |                                                       Specifica il valore della proprietà identificazione personale del certificato da utilizzare. Ad esempio, un valore di identificazione personale certificato di "a9 09 50 2d d8 2a e4 14 33 e6 f8 38 86 b0 0D che 42 77 a3 2a 7b" deve essere specificato come "a909502dd82ae41433e6f83886b00d4277a32a7b".                                                        |
|          -tpmandpin          |                                                                                           Aggiunge un modulo TPM (Trusted Platform) e protezione (PIN) numero di identificazione personale per l'unità del sistema operativo. È inoltre possibile utilizzare **- tp** come una versione abbreviata di questo comando.                                                                                           |
|      -tpmandstartupkey       |                                                                                                                    Aggiunge una protezione con chiave TPM e avvio per l'unità del sistema operativo. È inoltre possibile utilizzare **-tsk** come una versione abbreviata di questo comando.                                                                                                                    |
|   -tpmandpinandstartupkey    |                                                                                                                Aggiunge un TPM, PIN e protezione con chiave di avvio per l'unità del sistema operativo. È inoltre possibile utilizzare **- tpsk** come una versione abbreviata di questo comando.                                                                                                                 |
|          -password           |                                                                                                                              Aggiunge una protezione con chiave password per l'unità dati. È inoltre possibile utilizzare **- pw** come una versione abbreviata di questo comando.                                                                                                                              |
|      -adaccountorgroup       | Aggiunge un ID di sicurezza (SID)-protezione di identità per il volume di base.  È inoltre possibile utilizzare **-sid** come una versione abbreviata di questo comando. **IMPORTANTE:** Per impostazione predefinita, è possibile aggiungere una protezione ADAccountOrGroup in modalità remota tramite WMI o gestire-bde.  Se la distribuzione richiede la possibilità di aggiungere questo tipo di protezione in modalità remota è necessario abilitare la delega vincolata. |
|        -computername         |                                                                                                       Specifica che gestire-bde viene utilizzata per modificare la protezione BitLocker su un computer diverso. È inoltre possibile utilizzare **- cn** come una versione abbreviata di questo comando.                                                                                                       |
|            <Name>            |                                                                                                         Rappresenta il nome del computer in cui si desidera modificare la protezione BitLocker. I valori accettati includono nome NetBIOS del computer e l'indirizzo IP del computer.                                                                                                         |

### <a name="BKMK_deleteprotectors"></a>-eliminare sintassi e parametri
```
manage-bde  protectors  delete <Drive> [-type {recoverypassword|externalkey|certificate|tpm|tpmandstartupkey|tpmandpin|tpmandpinandstartupkey|Password|Identity}] 
[-id <KeyProtectorID>] [-computername <Name>] [{-?|/?}] [{-help|-h}]
```

|       Parametro        |                                                                              Descrizione                                                                               |
|------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|        <Drive>         |                                                             Rappresenta una lettera di unità seguita da due punti.                                                             |
|         -tipo          |                               Identifica la protezione con chiave da eliminare. È inoltre possibile utilizzare **-t** come una versione abbreviata di questo comando.                               |
|    RecoveryPassword    |                                                 Specifica che le protezioni con chiave di password di ripristino deve essere eliminata.                                                 |
|      ExternalKey       |                                        Specifica che le protezioni con chiave esterne associate all'unità devono essere eliminate.                                         |
|      certificato       |                                       Specifica che le protezioni con chiave certificato associati con l'unità devono essere eliminate.                                       |
|          TPM           |                                        Specifica che le protezioni con chiave solo TPM associati con l'unità devono essere eliminate.                                         |
|    TPMAndStartupKey    |                                Specifica che devono essere eliminate qualsiasi TPM e avvio basata su chiave protezioni con chiave associate all'unità.                                |
|       TPMAndPIN        |                                    Specifica che qualsiasi TPM e PIN basati protezioni con chiave associate all'unità devono essere eliminati.                                    |
| tpmandpinandstartupkey |                             Specifica che devono essere eliminate qualsiasi TPM e PIN di avvio basato su chiave protezioni con chiave associate all'unità.                             |
|        password        |                                        Specifica che le protezioni con chiave password associate all'unità devono essere eliminate.                                         |
|        autenticazione        |                                        Specifica che le protezioni con chiave di identità associata all'unità deve essere eliminata.                                         |
|          -id           |                Identifica la protezione con chiave da eliminare utilizzando l'identificatore di chiave. Questo parametro è un'opzione alternativa per il **-tipo** parametro.                 |
|    <KeyProtectorID>    |        Identifica un singolo protezione con chiave sull'unità da eliminare. ID di protezione con chiave possono essere visualizzati utilizzando il **gestire-bde-protezioni-ottenere** comando.         |
|     -computername      | Specifica che gestire bde.exe verrà usato per modificare la protezione BitLocker su un computer diverso. È inoltre possibile utilizzare **- cn** come una versione abbreviata di questo comando. |
|         <Name>         |    Rappresenta il nome del computer in cui si desidera modificare la protezione BitLocker. I valori accettati includono nome NetBIOS del computer e l'indirizzo IP del computer.     |
|        -? o /?        |                                                               Consente di visualizzare breve guida al prompt dei comandi.                                                               |
|      -help o -h       |                                                             Visualizza la Guida al prompt dei comandi completa.                                                              |

### <a name="BKMK_disableprot"></a>-Disabilita la sintassi e parametri
```
manage-bde  protectors  disable <Drive> [-RebootCount <integer 0 - 15>] [-computername <Name>] [{-?|/?}] [{-help|-h}]
```

|   Parametro   |                                                                                                                                                                                                                   Descrizione                                                                                                                                                                                                                    |
|---------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|    <Drive>    |                                                                                                                                                                                                  Rappresenta una lettera di unità seguita da due punti.                                                                                                                                                                                                  |
|  RebootCount  | A partire da Windows 8, specifica che la protezione del volume del sistema operativo è stato sospeso e verrà ripresa dopo Windows è stato riavviato il numero di volte specificato nel parametro RebootCount. Specificare 0 per sospendere la protezione in modo indefinito. Se questo parametro non viene specificato la protezione BitLocker verrà automaticamente ripresa dopo il riavvio di Windows. È inoltre possibile utilizzare **-rc** come una versione abbreviata di questo comando. |
| -computername |                                                                                                                                      Specifica che gestire bde.exe verrà usato per modificare la protezione BitLocker su un computer diverso. È inoltre possibile utilizzare **- cn** come una versione abbreviata di questo comando.                                                                                                                                      |
|    <Name>     |                                                                                                                                         Rappresenta il nome del computer in cui si desidera modificare la protezione BitLocker. I valori accettati includono nome NetBIOS del computer e l'indirizzo IP del computer.                                                                                                                                          |
|   -? o /?    |                                                                                                                                                                                                    Consente di visualizzare breve guida al prompt dei comandi.                                                                                                                                                                                                    |
|  -help o -h  |                                                                                                                                                                                                  Visualizza la Guida al prompt dei comandi completa.                                                                                                                                                                                                   |

## <a name="BKMK_Examples"></a>Esempi
Nell'esempio seguente viene illustrato l'utilizzo di **-protezioni** comando per aggiungere una protezione con chiave certificato identificata da un file di certificato per l'unità E.
```
manage-bde  protectors  add E: -certificate  cf "c:\File Folder\Filename.cer"
```
Nell'esempio seguente viene illustrato l'utilizzo di **-protezioni** comando per aggiungere un **adaccountorgroup** protezione con chiave identificato dal nome di dominio e l'unità E.
```
manage-bde  protectors  add E: -sid DOMAIN\user
```
Nell'esempio seguente viene illustrato l'utilizzo di **protezioni** comando per disabilitare la protezione necessario riavviare il computer dispone di 3 volte.
```
manage-bde  protectors  disable C: -rc 3
```
Nell'esempio seguente viene illustrato l'utilizzo di **-protezioni** comando per eliminare tutte le chiavi TPM e avvio basati protezioni con chiave sull'unità C.
```
manage-bde  protectors  delete C: -type tpmandstartupkey
```
Nell'esempio seguente viene illustrato l'utilizzo di **-protezioni** comando per eseguire il backup di tutte le informazioni di ripristino dell'unità C per AD DS.
```
manage-bde  protectors  adbackup C:
```
## <a name="additional-references"></a>Riferimenti aggiuntivi
-   [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)
-   [manage-bde](manage-bde.md)
