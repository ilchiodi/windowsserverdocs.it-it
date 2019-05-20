---
title: ktpass
description: 'Argomento i comandi di Windows per * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 47087676-311e-41f1-8414-199740d01444
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 7a865b94bbbf2245e8a2e07638064f66de920b90
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59886672"
---
# <a name="ktpass"></a>ktpass

>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Configura il nome dell'entità server per l'host o un servizio in active directory Domain Services (AD DS) e genera un file. keytab contenente la chiave segreta condivisa del servizio. Il file KEYTAB è basato sull'implementazione del protocollo di autenticazione Kerberos del Massachusetts Institute of Technology (MIT). Lo strumento da riga di comando molte consente ai servizi non Windows che supportano l'autenticazione Kerberos per usare le funzionalità di interoperabilità fornite dal servizio Centro distribuzione chiavi (KDC) di Kerberos. In questo argomento si applica alle versioni del sistema operativo designate nel **si applica a** elenco all'inizio dell'argomento.  
  
## <a name="syntax"></a>Sintassi  
```  
ktpass  
[/out <FileName>]   
[/princ <PrincipalName>]   
[/mapuser <UserAccount>]   
[/mapop {add|set}] [{-|+}desonly] [/in <FileName>]  
[/pass {Password|*|{-|+}rndpass}]  
[/minpass]  
[/maxpass]  
[/crypto {DES-CBC-CRC|DES-CBC-MD5|RC4-HMAC-NT|AES256-SHA1|AES128-SHA1|All}]  
[/itercount]  
[/ptype {KRB5_NT_PRINCIPAL|KRB5_NT_SRV_INST|KRB5_NT_SRV_HST}]  
[/kvno <KeyversionNum>]  
[/answer {-|+}]  
[/target]  
[/rawsalt] [{-|+}dumpsalt] [{-|+}setupn] [{-|+}setpass <Password>]  [/?|/h|/help]  
```  
## <a name="parameters"></a>Parametri  
|Parametro|Descrizione|  
|-------|--------|  
|/out <FileName>|Specifica il nome del file Kerberos versione 5 .keytab da generare. **Nota:** Si tratta di file. keytab che trasferire a un computer che non è in esecuzione il sistema operativo Windows, quindi sostituire o unire con il file. keytab esistente, / etc/krb5.keytab.|  
|/princ <PrincipalName>|Specifica il nome dell'entità nel formato host/computer.contoso.com@CONTOSO.COM. **Avviso:** Questo parametro viene fatta distinzione tra maiuscole e minuscole. Visualizzare [osservazioni](#BKMK_remarks) per altre informazioni.|  
|/mapuser <UserAccount>|Mappa il nome dell'entità di Kerberos, è possibile il **princ** parametro per l'account di dominio specificato.|  
|/mapop {aggiungere &#124; set}|Specifica come viene impostato l'attributo di mapping.<br /><br />-   **aggiungere** aggiunge il valore del nome utente locale specificato. Questa è l'impostazione predefinita.<br />-   **Impostare** imposta il valore di Data Encryption Standard (DES)-crittografia solo per il nome utente locale specificato.|  
|{-&#124; +} desonly|La crittografia DES-only viene impostata per impostazione predefinita.<br /><br />-   **+** Imposta un account per la crittografia DES-only.<br />-   **-** Rilascia restrizione su un account per la crittografia DES-only. **IMPORTANTE:** A partire da Windows 7 e Windows Server 2008 R2, Windows non supporta DES per impostazione predefinita.|  
|/in <FileName>|Specifica il file .keytab per leggere da un computer host che non è in esecuzione il sistema operativo Windows.|  
|/ passare {Password &#124; * &#124; { -&#124; +} rndpass}|Specifica una password per il nome dell'entità utente specificato per il **princ** parametro. Utilizzare "*" per richiedere una password.|  
|/minpass|Imposta la lunghezza minima della password casuali a 15 caratteri.|  
|/maxpass|Imposta la lunghezza massima della password casuali a 256 caratteri.|  
|/ crittografia {DES-CBC-CRC &#124; DES-CBC-MD5 &#124; RC4-HMAC-NT &#124; SHA1 AES256 &#124; SHA1 aes128 &#124; All}|Specifica le chiavi che vengono generate nel file keytab:<br /><br />-   **DES-CBC-CRC** per motivi di compatibilità.<br />-   **DES-CBC-MD5** più aderisce all'implementazione MIT e viene usato per la compatibilità.<br />-   **RC4-HMAC-NT** Usa la crittografia a 128 bit.<br />-   **SHA1 AES256** Usa la crittografia AES256-CTS-HMAC-SHA1-96.<br />-   **SHA1 aes128** Usa la crittografia AES128-CTS-HMAC-SHA1-96.<br />-   **Tutti i** stati che tutti i tipi di crittografia supportati possono essere utilizzati. **Nota:** Le impostazioni predefinite sono basate su versioni precedenti di MIT. Di conseguenza, `/crypto` deve sempre essere specificato.|  
|/itercount|Specifica il conteggio delle iterazioni che viene utilizzato per la crittografia AES. Il valore predefinito è che **itercount** viene ignorata per la crittografia AES non e impostare a 4.096 per la crittografia AES.|  
|/ptype {KRB5_NT_PRINCIPAL &#124; KRB5_NT_SRV_INST &#124; KRB5_NT_SRV_HST}|Specifica il tipo di entità.<br /><br />-   **KRB5_NT_PRINCIPAL** è il tipo di entità generale (scelta consigliato).<br />-   **KRB5_NT_SRV_INST** è l'istanza del servizio utente.<br />-   **KRB5_NT_SRV_HST** è l'istanza del servizio host.|  
|/kvno <KeyversionNum>|Specifica il numero di versione della chiave. Il valore predefinito è 1.|  
|/Answer {-&#124; +}|Imposta la modalità di risposta in background:<br /><br />**-** Risposte reimpostare la richiesta della password automaticamente con No.<br /><br />**+** Risposte reimpostare la richiesta della password automaticamente con Sì.|  
|/target|Imposta il controller di dominio da utilizzare. Il valore predefinito è per il controller di dominio essere rilevato, in base al nome dell'entità. Se il nome del controller di dominio non viene risolto, una finestra di dialogo richiederà un controller di dominio valido.|  
|/rawsalt|Utilizzare l'algoritmo rawsalt durante la generazione della chiave molte di forza. Questo parametro non è necessaria.|  
|{-&#124; +} dumpsalt|L'output di questo parametro indica l'algoritmo salt MIT utilizzato per generare la chiave.|  
|{-&#124; +} setupn|Imposta il nome dell'entità utente (UPN) oltre al nome dell'entità servizio (SPN). Per impostazione predefinita viene impostato nel file .keytab.|  
|{-&#124;+}setpass <Password>|Imposta la password dell'utente quando fornito. Se si utilizza rndpass, viene generata invece una password casuale.|  
|/? &#124; /h &#124; /Help|Visualizza la Guida per molte.|  
## <a name="BKMK_remarks"></a>remarks  
Servizi in esecuzione su sistemi che non eseguono il sistema operativo Windows possono essere configurati con gli account istanza del servizio in servizi di dominio active directory. In questo modo qualsiasi client Kerberos per l'autenticazione ai servizi che non eseguono il sistema operativo Windows mediante Windows KDC.  
Il parametro /princ non viene valutato dal molte e viene utilizzato come previsto. Non sono presenti controlli per verificare se il parametro corrisponda esattamente del caso del **userPrincipalName** valore dell'attributo durante la generazione del file Keytab. Maiuscole/minuscole Kerberos distribuzioni che utilizzano questo file Keytab potrebbero avere problemi quando non esiste alcuna corrispondenza esatta case può avere esito negativo durante la pre-autenticazione. Controllare e recuperare i valori corretti **userPrincipalName** valore dell'attributo da un file di esportazione LDifDE. Ad esempio:   
```  
ldifde /f keytab_user.ldf /d "CN=Keytab User,OU=UserAccounts,DC=contoso,DC=corp,DC=microsoft,DC=com" /p base /l samaccountname,userprincipalname  
```  
## <a name="BKMK_examples"></a>Esempi  
Nell'esempio seguente viene illustrato come creare un file di .keytab Kerberos, machine.keytab, nella directory corrente per l'utente Sample1. (Si unirà questo file con il file Krb5.keytab in un computer host che non è in esecuzione il sistema operativo Windows.) Verrà creato il file .keytab Kerberos per tutti i tipi di crittografia supportati per il tipo di entità generale.  
Per generare un file .keytab per un computer host che non è in esecuzione il sistema operativo Windows, utilizzare la procedura seguente per eseguire il mapping di entità per l'account e impostare la password dell'entità dell'host:  
1.  Utilizzo di active directory utente e computer snap-in per creare un account utente per un servizio in un computer che non è in esecuzione il sistema operativo Windows. Ad esempio, creare un account con il nome Sample1.  
2.  Utilizzare molte per impostare un mapping di identità per l'account utente di digitare il comando seguente al prompt:  
    ```  
    ktpass /princ host/Sample1.contoso.com@CONTOSO.COM /mapuser Sample1 /pass MyPas$w0rd /out Sample1.keytab /crypto all /ptype KRB5_NT_PRINCIPAL /mapop set   
    ```  
    
    > [!NOTE]  
    > È Impossibile eseguire il mapping di più istanze del servizio per lo stesso account utente.  
    
3.  Unisce il file .keytab con il file /Etc/Krb5.keytab in un computer host che non è in esecuzione il sistema operativo Windows. 

#### <a name="additional-references"></a>Riferimenti aggiuntivi  
[Chiave sintassi della riga di comando](command-line-syntax-key.md)  
