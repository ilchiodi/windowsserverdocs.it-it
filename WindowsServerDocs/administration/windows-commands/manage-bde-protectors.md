---
title: protezione da Manage-bde
description: Argomento di riferimento per il comando Manage-bde protectors, che consente di gestire i metodi di protezione utilizzati per la chiave di crittografia di BitLocker.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 1f9b22c5-cc93-45df-9165-bedee94998da
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 08/06/2018
ms.openlocfilehash: 999c92fd9f2bfedad92a9c68c1528ee66836f315
ms.sourcegitcommit: 29bc8740e5a8b1ba8f73b10ba4d08afdf07438b0
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/30/2020
ms.locfileid: "84222621"
---
# <a name="manage-bde-protectors"></a>protezione da Manage-bde

> Si applica a: Windows Server (Canale semestrale), Windows Server 2019, Windows Server 2016

Gestisce i metodi di protezione utilizzati per la chiave di crittografia BitLocker.

## <a name="syntax"></a>Sintassi

```
manage-bde -protectors [{-get|-add|-delete|-disable|-enable|-adbackup|-aadbackup}] <drive> [-computername <name>] [{-?|/?}] [{-help|-h}]
```

### <a name="parameters"></a>Parametri

| Parametro | Descrizione |
| ----------- | ----------- |
| -Introduzione | Visualizza tutti i metodi di protezione con chiave abilitati per l'unità e fornisce il tipo e l'identificatore (ID). |
| -aggiungere | Aggiunge i metodi di protezione delle chiavi come specificato **utilizzando parametri aggiuntivi aggiuntivi.** |
| -Elimina | Elimina i metodi di protezione della chiave utilizzati da BitLocker. Tutte le protezioni con chiave verranno rimosse da un'unità a meno che non vengano usati i parametri facoltativi **-Delete** per specificare quali protezioni eliminare. Quando viene eliminata l'ultimo protezione in un'unità, la protezione BitLocker dell'unità è disabilitata per assicurare che l'accesso ai dati non vengano perso accidentalmente. |
| -Disabilita | Disabilita la protezione, che consentirà a chiunque di accedere ai dati crittografati rendendo disponibile la chiave di crittografia non protette sul disco. Nessuna protezione con chiave vengono rimossi. La protezione verrà ripresa al successivo avvio di Windows, a meno che non vengano usati i parametri facoltativi **Disable** per specificare il numero di riavvii. |
| -abilitare | Abilita la protezione rimuovendo la chiave di crittografia non protetta dall'unità. Verranno applicate tutte le protezioni con chiave configurata nell'unità. |
| -adbackup | Esegue il backup di tutte le informazioni di ripristino per l'unità specificata in servizi di dominio Active Directory (AD DS). Per eseguire il backup solo una chiave di ripristino solo di dominio Active Directory, aggiungere il **-id** parametro e specificare l'ID di una chiave di ripristino specifico per eseguire il backup. |
| -aadbackup | Esegue il backup di tutte le informazioni di ripristino per l'unità specificata Azure Active Directory (Azure AD). Per eseguire il backup di una sola chiave di ripristino per Azure AD, aggiungere il parametro **-ID** e specificare l'ID di una chiave di ripristino specifica di cui eseguire il backup. |
| `<drive>` | Rappresenta una lettera di unità seguita da due punti. |
| -computername | Specifica che manage-bde. exe verrà utilizzato per modificare la protezione BitLocker in un computer diverso. È inoltre possibile utilizzare **- cn** come una versione abbreviata di questo comando. |
| `<name>` | Rappresenta il nome del computer in cui si desidera modificare la protezione BitLocker. I valori accettati includono nome NetBIOS del computer e l'indirizzo IP del computer. |
| -? o /? | Visualizza una breve guida al prompt dei comandi. |
| -Help o-h | Visualizza la guida completa al prompt dei comandi. |

#### <a name="additional--add-parameters"></a>Aggiuntivi-Aggiungi parametri

Il parametro-Add può anche usare questi parametri aggiuntivi validi.

```
manage-bde -protectors -add [<drive>] [-forceupgrade] [-recoverypassword <numericalpassword>] [-recoverykey <pathtoexternalkeydirectory>]
[-startupkey <pathtoexternalkeydirectory>] [-certificate {-cf <pathtocertificatefile>|-ct <certificatethumbprint>}] [-tpm] [-tpmandpin]
[-tpmandstartupkey <pathtoexternalkeydirectory>] [-tpmandpinandstartupkey <pathtoexternalkeydirectory>] [-password][-adaccountorgroup <securityidentifier> [-computername <name>]
[{-?|/?}] [{-help|-h}]
```

| Parametro | Descrizione |
| --------- | ----------- |
| `<drive>` | Rappresenta una lettera di unità seguita da due punti. |
| -recoverypassword | Aggiunge una protezione con password numerica. È inoltre possibile utilizzare **- rp** come una versione abbreviata di questo comando. |
| `<numericalpassword>` | Rappresenta la password di ripristino. |
| -recoverykey | Aggiunge una protezione con chiave esterna per il ripristino. È inoltre possibile utilizzare **- rk** come una versione abbreviata di questo comando. |
| `<pathtoexternalkeydirectory>` | Rappresenta il percorso della directory per la chiave di ripristino. |
| -chiave di avvio | Aggiunge una protezione con chiave esterna per l'avvio. È inoltre possibile utilizzare **-sk** come una versione abbreviata di questo comando. |
| `<pathtoexternalkeydirectory>` | Rappresenta il percorso della directory per la chiave di avvio. |
| -certificato | Aggiunge una protezione con chiave pubblica per un'unità di dati. È inoltre possibile utilizzare **-cert** come una versione abbreviata di questo comando. |
| -cf | Specifica un file di certificato verrà utilizzato per fornire il certificato chiave pubblica. |
| <pathtocertificatefile> | Rappresenta il percorso della directory del file di certificato. |
| -ct | Specifica un'identificazione personale del certificato verrà utilizzato per identificare il certificato chiave pubblica |
| `<certificatethumbprint>` | Specifica il valore della proprietà identificazione personale del certificato da utilizzare. Ad esempio, un valore di identificazione personale del certificato a9 09 50 2D D8 2a E4 14 33 E6 F8 38 86 B0 0D 42 77 a3 2a 7B deve essere specificato come a909502dd82ae41433e6f83886b00d4277a32a7b. |
| -tpmandpin | Aggiunge un modulo TPM (Trusted Platform) e protezione (PIN) numero di identificazione personale per l'unità del sistema operativo. È inoltre possibile utilizzare **- tp** come una versione abbreviata di questo comando. |
| -tpmandstartupkey | Aggiunge una protezione con chiave TPM e avvio per l'unità del sistema operativo. È inoltre possibile utilizzare **-tsk** come una versione abbreviata di questo comando. |
| -tpmandpinandstartupkey | Aggiunge un TPM, PIN e protezione con chiave di avvio per l'unità del sistema operativo. È inoltre possibile utilizzare **- tpsk** come una versione abbreviata di questo comando. |
| -password | Aggiunge una protezione con chiave password per l'unità dati. È inoltre possibile utilizzare **- pw** come una versione abbreviata di questo comando. |
| -adaccountorgroup | Aggiunge un ID di sicurezza (SID)-protezione di identità per il volume di base. È inoltre possibile utilizzare **-sid** come una versione abbreviata di questo comando. **Importante:** Per impostazione predefinita, non è possibile aggiungere una protezione ADaccountorgroup in modalità remota tramite WMI o Manage-bde. Se la distribuzione richiede la possibilità di aggiungere questa protezione in modalità remota, è necessario abilitare la delega vincolata. |
| -computername | Specifica che manage-bde viene utilizzato per modificare la protezione BitLocker in un computer diverso. È inoltre possibile utilizzare **- cn** come una versione abbreviata di questo comando. |
| `<name>` | Rappresenta il nome del computer in cui si desidera modificare la protezione BitLocker. I valori accettati includono nome NetBIOS del computer e l'indirizzo IP del computer. |
| -? o /? | Visualizza una breve guida al prompt dei comandi. |
| -Help o-h | Visualizza la guida completa al prompt dei comandi. |

#### <a name="additional--delete-parameters"></a>Parametri aggiuntivi per l'eliminazione

```
manage-bde -protectors -delete <drive> [-type {recoverypassword|externalkey|certificate|tpm|tpmandstartupkey|tpmandpin|tpmandpinandstartupkey|Password|Identity}]
[-id <keyprotectorID>] [-computername <name>] [{-?|/?}] [{-help|-h}]
```

| Parametro | Descrizione |
| --------- | ----------- |
| `<drive>` | Rappresenta una lettera di unità seguita da due punti. |
| -tipo | Identifica la protezione con chiave da eliminare. È inoltre possibile utilizzare **-t** come una versione abbreviata di questo comando. |
| RecoveryPassword | Specifica che le protezioni con chiave di password di ripristino deve essere eliminata. |
| ExternalKey | Specifica che le protezioni con chiave esterne associate all'unità devono essere eliminate. |
| certificato | Specifica che le protezioni con chiave certificato associati con l'unità devono essere eliminate. |
| TPM | Specifica che le protezioni con chiave solo TPM associati con l'unità devono essere eliminate. |
| TPMAndStartupKey | Specifica che devono essere eliminate tutte le protezioni con chiave basata su TPM e chiave di avvio associate all'unità. |
| TPMAndPIN | Specifica che le protezioni con chiave basata su TPM e PIN associate all'unità devono essere eliminate. |
| tpmandpinandstartupkey | Specifica che le protezioni con chiave TPM, PIN e chiave di avvio associate all'unità devono essere eliminate. |
| password | Specifica che le protezioni con chiave password associate all'unità devono essere eliminate. |
| identity | Specifica che le protezioni con chiave di identità associata all'unità deve essere eliminata. |
| -ID | Identifica la protezione con chiave da eliminare utilizzando l'identificatore di chiave. Questo parametro è un'opzione alternativa per il **-tipo** parametro. |
| `<keyprotectorID>` | Identifica un singolo protezione con chiave sull'unità da eliminare. ID di protezione con chiave possono essere visualizzati utilizzando il **gestire-bde-protezioni-ottenere** comando. |
| -computername | Specifica che manage-bde. exe verrà utilizzato per modificare la protezione BitLocker in un computer diverso. È inoltre possibile utilizzare **- cn** come una versione abbreviata di questo comando. |
| `<name>` | Rappresenta il nome del computer in cui si desidera modificare la protezione BitLocker. I valori accettati includono nome NetBIOS del computer e l'indirizzo IP del computer. |
| -? o /? | Visualizza una breve guida al prompt dei comandi. |
| -Help o-h | Visualizza la guida completa al prompt dei comandi. |

#### <a name="additional--disable-parameters"></a>Aggiuntivi-Disabilita parametri

```
manage-bde -protectors -disable <drive> [-rebootcount <integer 0 - 15>] [-computername <name>] [{-?|/?}] [{-help|-h}]
```

| Parametro | Descrizione |
| --------- | ----------- |
| `<drive>` | Rappresenta una lettera di unità seguita da due punti. |
| rebootcount | Specifica che la protezione del volume del sistema operativo è stata sospesa e riprenderà dopo il riavvio di Windows per il numero di volte specificato nel parametro **rebootcount** . Specificare **0** per sospendere la protezione per un periodo illimitato. Se questo parametro non è specificato, la protezione BitLocker riprende automaticamente dopo il riavvio di Windows. È inoltre possibile utilizzare **-rc** come una versione abbreviata di questo comando. |
| -computername | Specifica che manage-bde. exe verrà utilizzato per modificare la protezione BitLocker in un computer diverso. È inoltre possibile utilizzare **- cn** come una versione abbreviata di questo comando. |
| `<name>` | Rappresenta il nome del computer in cui si desidera modificare la protezione BitLocker. I valori accettati includono nome NetBIOS del computer e l'indirizzo IP del computer. |
| -? o /? | Visualizza una breve guida al prompt dei comandi. |
| -Help o-h | Visualizza la guida completa al prompt dei comandi. |

### <a name="examples"></a>Esempi

Per aggiungere una protezione con chiave del certificato, identificata da un file di certificato, all'unità E, digitare:

```
manage-bde -protectors -add E: -certificate -cf c:\File Folder\Filename.cer
```

Per aggiungere una protezione con chiave **adaccountorgroup** , identificata dal dominio e dal nome utente, all'unità E, digitare:

```
manage-bde -protectors -add E: -sid DOMAIN\user
```

Per disabilitare la protezione finché il computer non è stato riavviato tre volte, digitare:

```
manage-bde -protectors -disable C: -rc 3
```

Per eliminare tutte le protezioni con chiave basate su TPM e chiavi di avvio nell'unità C, digitare:

```
manage-bde -protectors -delete C: -type tpmandstartupkey
```

Per eseguire il backup di tutte le informazioni di ripristino per l'unità C in servizi di dominio Active Directory, digitare:

```
manage-bde -protectors -adbackup C:
```

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)

- [comando Manage-bde](manage-bde.md)
