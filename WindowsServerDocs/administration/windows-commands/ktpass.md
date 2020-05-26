---
title: ktpass
description: Argomento di riferimento per il comando lo Ktpass, che configura il nome dell'entità server per l'host o il servizio in servizi di dominio Active Directory e genera un file con estensione keytab che contiene la chiave privata condivisa del servizio.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 47087676-311e-41f1-8414-199740d01444
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 432918343ccee70f0c30d294a349fb721f18f705
ms.sourcegitcommit: 4f407b82435afe3111c215510b0ef797863f9cb4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/24/2020
ms.locfileid: "83817231"
---
# <a name="ktpass"></a>ktpass

> Si applica a: Windows Server (canale semestrale), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Configura il nome dell'entità server per l'host o il servizio in servizi di dominio Active Directory (AD DS) e genera un file .keytab che contiene la chiave segreta condivisa del servizio. Il file KEYTAB è basato sull'implementazione del protocollo di autenticazione Kerberos del Massachusetts Institute of Technology (MIT). Lo strumento da riga di comando lo Ktpass consente ai servizi non Windows che supportano l'autenticazione Kerberos di utilizzare le funzionalità di interoperabilità fornite dal servizio Centro distribuzione chiavi Kerberos (KDC).

## <a name="syntax"></a>Sintassi

```
ktpass
[/out <filename>]
[/princ <principalname>]
[/mapuser <useraccount>]
[/mapop {add|set}] [{-|+}desonly] [/in <filename>]
[/pass {password|*|{-|+}rndpass}]
[/minpass]
[/maxpass]
[/crypto {DES-CBC-CRC|DES-CBC-MD5|RC4-HMAC-NT|AES256-SHA1|AES128-SHA1|All}]
[/itercount]
[/ptype {KRB5_NT_PRINCIPAL|KRB5_NT_SRV_INST|KRB5_NT_SRV_HST}]
[/kvno <keyversionnum>]
[/answer {-|+}]
[/target]
[/rawsalt] [{-|+}dumpsalt] [{-|+}setupn] [{-|+}setpass <password>]  [/?|/h|/help]
```

### <a name="parameters"></a>Parametri

| Parametro | Descrizione |
| --------- | ------------|
| /out`<filename>` | Specifica il nome del file Kerberos versione 5 .keytab da generare. **Nota:** Si tratta del file. keytab trasferito a un computer che non esegue il sistema operativo Windows, quindi sostituire o unire con il file con estensione keytab esistente, */etc/krb5.keytab*. |
| /princ`<principalname>` | Specifica il nome dell'entità nel formato host/computer.contoso.com@CONTOSO.COM . **Avviso:** Questo parametro fa distinzione tra maiuscole e minuscole. |
| /mapuser`<useraccount>` | Mappa il nome dell'entità di Kerberos, è possibile il **princ** parametro per l'account di dominio specificato. |
| /mapop`{add|set}` | Specifica come viene impostato l'attributo di mapping.<ul><li>**Aggiungi** : aggiunge il valore del nome utente locale specificato. Questo è il valore predefinito.</li><li>**Set** : imposta il valore per la crittografia DES (Data Encryption Standard) per il nome utente locale specificato.</li></ul> |
| `{-|+}`desonly | La crittografia DES-only viene impostata per impostazione predefinita.<ul><li>**+** Imposta un account per la crittografia DES-only.</li><li>**-** Rilascia la restrizione su un account per la crittografia DES-only. **Importante:** Per impostazione predefinita, Windows non supporta DES.</li></ul> |
| /in`<filename>` | Specifica il file .keytab per leggere da un computer host che non è in esecuzione il sistema operativo Windows. |
| /pass`{password|*|{-|+}rndpass}` | Specifica una password per il nome dell'entità utente specificato per il **princ** parametro. Usare `*` per richiedere una password. |
| /minpass | Imposta la lunghezza minima della password casuali a 15 caratteri. |
| /maxpass | Imposta la lunghezza massima della password casuali a 256 caratteri. |
| /crypto`{DES-CBC-CRC|DES-CBC-MD5|RC4-HMAC-NT|AES256-SHA1|AES128-SHA1|All}` | Specifica le chiavi che vengono generate nel file keytab:<ul><li>**Des-CBC-CRC** : usato per la compatibilità.</li><li>**Des-CBC-MD5** -aderisce più strettamente all'implementazione mit e viene usato per la compatibilità.</li><li>**RC4-HMAC-NT** -impiega la crittografia a 128 bit.</li><li>**AES256-SHA1** -usa la crittografia AES256-CTS-HMAC-SHA1-96.</li><li>   **AES128-SHA1** -usa la crittografia Aes128-CTS-HMAC-SHA1-96.</li><li>**All** : indica che tutti i tipi crittografici supportati possono essere utilizzati.</li></ul><p>**Nota:** Poiché le impostazioni predefinite sono basate su versioni precedenti di MIT, è sempre necessario usare il `/crypto` parametro. |
| /itercount | Specifica il conteggio delle iterazioni che viene utilizzato per la crittografia AES. Il valore predefinito ignora **itercount** per la crittografia non AES e imposta la crittografia AES su 4.096. |
| /ptype`{KRB5_NT_PRINCIPAL|KRB5_NT_SRV_INST|KRB5_NT_SRV_HST}` | Specifica il tipo di entità.<ul><li>**KRB5_NT_PRINCIPAL** : il tipo di entità generale (scelta consigliata).</li><li>**KRB5_NT_SRV_INST** -l'istanza del servizio utente</li><li>  **KRB5_NT_SRV_HST** -l'istanza del servizio host</li></ul> |
| /kvno`<keyversionnum>` | Specifica il numero di versione della chiave. Il valore predefinito è 1. |
| /Answer`{-|+}` | Imposta la modalità di risposta in background:<ul><li>**-** Answers Reimposta automaticamente i prompt della password con **No**.</li><li>**+** Answers Reimposta automaticamente i prompt della password con **Sì**.</li></ul> |
| /target | Imposta il controller di dominio da utilizzare. Il valore predefinito è per il controller di dominio essere rilevato, in base al nome dell'entità. Se il nome del controller di dominio non viene risolto, verrà visualizzata una finestra di dialogo che richiede un controller di dominio valido. |
| /rawsalt | impone a lo Ktpass di usare l'algoritmo rawsalt durante la generazione della chiave. Questo parametro è facoltativo. |
| `{-|+}dumpsalt` | L'output di questo parametro indica l'algoritmo salt MIT utilizzato per generare la chiave. |
| `{-|+}setupn` | Imposta il nome dell'entità utente (UPN) oltre al nome dell'entità servizio (SPN). Per impostazione predefinita viene impostato nel file .keytab. |
| `{-|+}setpass <password>` | Imposta la password dell'utente quando fornito. Se si utilizza rndpass, viene generata invece una password casuale. |
| /? | Visualizza la Guida per questo comando. |

#### <a name="remarks"></a>Osservazioni

- I servizi in esecuzione su sistemi che non eseguono il sistema operativo Windows possono essere configurati con gli account dell'istanza del servizio in servizi di dominio Active Directory. In questo modo qualsiasi client Kerberos per l'autenticazione ai servizi che non eseguono il sistema operativo Windows mediante Windows KDC.

- Il parametro **/princ** non viene valutato da lo Ktpass e viene usato come fornito. Non è disponibile alcun controllo per verificare se il parametro corrisponde al caso esatto del valore dell'attributo **userPrincipalName** durante la generazione del file keytab. Le distribuzioni Kerberos con distinzione tra maiuscole e minuscole che usano questo file keytab potrebbero avere problemi in caso di mancata corrispondenza esatta e potrebbero anche non riuscire durante la pre-autenticazione. Per controllare e recuperare il valore dell'attributo **userPrincipalName** corretto da un file di esportazione LDifDE. Ad esempio:

    ```
    ldifde /f keytab_user.ldf /d CN=Keytab User,OU=UserAccounts,DC=contoso,DC=corp,DC=microsoft,DC=com /p base /l samaccountname,userprincipalname
    ````

### <a name="examples"></a>Esempi

Per creare un file con estensione keytab Kerberos per un computer host in cui non è in esecuzione il sistema operativo Windows, è necessario eseguire il mapping dell'entità all'account e impostare la password dell'entità host.

1. Utilizzare lo snap-in **utenti e computer** di Active Directory per creare un account utente per un servizio in un computer in cui non è in esecuzione il sistema operativo Windows. Ad esempio, creare un account con il nome *User1*.

2. Usare il comando **lo Ktpass** per configurare un mapping di identità per l'account utente digitando:

    ```
    ktpass /princ host/User1.contoso.com@CONTOSO.COM /mapuser User1 /pass MyPas$w0rd /out machine.keytab /crypto all /ptype KRB5_NT_PRINCIPAL /mapop set
    ```

    > [!NOTE]
    > È Impossibile eseguire il mapping di più istanze del servizio per lo stesso account utente.

3. Unire il file con estensione keytab con il file */etc/krb5.keytab* in un computer host che non esegue il sistema operativo Windows.

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)
