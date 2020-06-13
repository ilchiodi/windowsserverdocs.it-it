---
title: nfsadmin
description: Argomento di riferimento per il comando nfsadmin, che gestisce sia server per NFS che client per NFS.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 7375b2cf-c6b8-45b5-abf6-6c10e462defd
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: c122577758dd28d11d25445ca9dc98ed03024c77
ms.sourcegitcommit: 99d548141428c964facf666c10b6709d80fbb215
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 06/12/2020
ms.locfileid: "84721534"
---
# <a name="nfsadmin"></a>nfsadmin

> Si applica a: Windows Server (canale semestrale), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Utilità da riga di comando che amministra server per NFS o client per NFS nel computer locale o remoto che esegue i servizi Microsoft per NFS (Network File System). Usato senza parametri, il server nfsadmin Visualizza le impostazioni di configurazione del server corrente per NFS e il client nfsadmin Visualizza le impostazioni di configurazione del client corrente per NFS.

## <a name="syntax"></a>Sintassi

```
nfsadmin server [computername] [-u Username [-p Password]] -l
nfsadmin server [computername] [-u Username [-p Password]] -r {client | all}
nfsadmin server [computername] [-u Username [-p Password]] {start | stop}
nfsadmin server [computername] [-u Username [-p Password]] config option[...]
nfsadmin server [computername] [-u Username [-p Password]] creategroup <name>
nfsadmin server [computername] [-u Username [-p Password]] listgroups
nfsadmin server [computername] [-u Username [-p Password]] deletegroup <name>
nfsadmin server [computername] [-u Username [-p Password]] renamegroup <oldname> <newname>
nfsadmin server [computername] [-u Username [-p Password]] addmembers <hostname>[...]
nfsadmin server [computername] [-u Username [-p Password]] listmembers
nfsadmin server [computername] [-u Username [-p Password]] deletemembers <hostname><groupname>[...]
nfsadmin client [computername] [-u Username [-p Password]] {start | stop}
nfsadmin client [computername] [-u Username [-p Password]] config option[...]
```

### <a name="general-parameters"></a>Parametri generali

| Parametro | Descrizione |
| --------- | ----------- |
| nomecomputer | Specifica il computer remoto che si desidera amministrare. È possibile specificare il computer utilizzando un nome WINS (Windows Internet Name Service) o un nome di Domain Name System (DNS) o un indirizzo IP (Internet Protocol). |
| -u nomeutente | Specifica il nome utente dell'utente di cui si desidera utilizzare le credenziali. Potrebbe essere necessario aggiungere il nome di dominio al nome utente nel formato *DOMINIO\nomeutente*. |
| -p password | Specifica la password dell'utente specificato utilizzando l'opzione **-u** . Se si specifica l'opzione **-u** omettendo l'opzione **-p** , verrà richiesta la password dell'utente. |

### <a name="server-for-nfs-related-parameters"></a>Parametri correlati al server per NFS

| Parametro | Descrizione |
| --------- | ----------- |
| -l | Elenca tutti i blocchi utilizzati dai client. |
| -r`{client|all}` | Rilascia i blocchi mantenuti da un client o, se viene specificato all, da tutti i client. |
| start | Avvia il server per il servizio NFS. |
| stop | Arresta il servizio Server per NFS. |
| config | Specifica le impostazioni generali per server per NFS. Con l'argomento del comando **config** è necessario specificare almeno una delle opzioni seguenti:<ul><li>**mapsvr = `<server>` ** -Imposta il server come server di mapping dei nomi utente per server per NFS. Anche se questa opzione continua a essere supportata per la compatibilità con le versioni precedenti, è consigliabile usare invece l'utilità sfuadmin.</li><li>**auditlocation = `{eventlog|file|both|none}` ** : Specifica se gli eventi verranno controllati e verranno registrati gli eventi. È necessario uno degli argomenti seguenti:<ul><li>**EventLog** : specifica che gli eventi controllati verranno registrati solo nel registro applicazioni Visualizzatore eventi.</li><li>**file** : specifica che gli eventi controllati verranno registrati solo nel file specificato da `config fname` .</li><li>**both** : specifica che gli eventi controllati verranno registrati nel registro applicazioni di Visualizzatore eventi e nel file specificato da `config fname` .</li><li>**None** : specifica che gli eventi non vengono controllati.</li></ul><li>**fname = `<file>` ** : Imposta il file specificato da file come file di controllo. Il valore predefinito è **%SFUDIR%\log \\ NfsSvr. log**.</li><li>**fsize = `<size>` ** -Imposta Size come dimensione massima in megabyte del file di controllo. La dimensione massima predefinita è **7 MB**.</li><li>**`audit=[+|-]mount [+|-]read [+|-]write [+|-]create [+|-]delete [+|-]locking [+|-]all`**: Specifica gli eventi da registrare. Per avviare la registrazione di un evento, digitare un segno più ( **+** ) prima del nome dell'evento. per arrestare la registrazione di un evento, digitare un segno meno ( **-** ) prima del nome dell'evento. Se il segno viene omesso, **+** viene utilizzato il segno. Non usare **tutti** con altri nomi di evento.</li><li>**lockperiod = `<seconds>` ** -Specifica il numero di secondi di attesa da parte di server per NFS per recuperare i blocchi dopo che una connessione al server per NFS è stata persa e quindi ristabilita o dopo il riavvio del servizio Server per NFS.</li><li>**portmapprotocol = `{TCP|UDP|TCP+UDP}` ** : Specifica i protocolli di trasporto supportati da portmap. L'impostazione predefinita è **TCP + UDP**.</li><li>**mountprotocol = `{TCP|UDP|TCP+UDP}` ** : Specifica i protocolli di trasporto supportati dal montaggio. L'impostazione predefinita è **TCP + UDP**.</li><li>**nfsprotocol = `{TCP|UDP|TCP+UDP}` ** : Specifica i protocolli di trasporto supportati da NFS (Network File System). L'impostazione predefinita è **TCP + UDP**</li><li>**nlmprotocol = `{TCP|UDP|TCP+UDP}` ** : Specifica il protocollo di trasporto supportato da Gestione blocchi di rete (NLM). L'impostazione predefinita è **TCP + UDP**.</li><li>**nsmprotocol = `{TCP|UDP|TCP+UDP}` ** : Specifica quale protocollo di trasporto supporta il gestore dello stato di rete (NSM). L'impostazione predefinita è **TCP + UDP**.</li><li>**enableV3 = `{yes|no}` ** : Specifica se saranno supportati i protocolli NFS versione 3. L'impostazione predefinita è **Sì**.</li><li>**renewauth = `{yes|no}` ** : Specifica se le connessioni client dovranno essere riautenticate dopo il periodo specificato da config renewauthinterval. L'impostazione predefinita è **No**.</li><li>**renewauthinterval = `<seconds>` ** : Specifica il numero di secondi che devono trascorrere prima che venga forzata la riautenticazione di un client se `config renewauth` è impostato su **Sì**. Il valore predefinito è **600 secondi**.</li><li>**dircache = `<size>` ** : Specifica le dimensioni in kilobyte della cache di directory. Il numero specificato come dimensione deve essere un multiplo di 4 compreso tra 4 e 128. La dimensione predefinita della cache della directory è **128 KB**.</li><li>**translationfile = `<file>` ** : Specifica un file contenente le informazioni di mapping per la sostituzione di caratteri nei nomi dei file quando vengono spostati da file System basati su Windows in UNIX. Se il file non è specificato, la conversione di caratteri del nome file è disabilitata. Se il valore di **translationfile** viene modificato, è necessario riavviare il server per rendere effettive le modifiche.</li><li>**dotfileshidden = `{yes|no}` ** : Specifica se i file con nomi che iniziano con un punto (.) sono contrassegnati come nascosti nel file system di Windows e, di conseguenza, nascosti ai client NFS. L'impostazione predefinita è **No**.</li><li>**casesensitivelookups = `{yes|no}` ** : Specifica se le ricerche nelle directory fanno distinzione tra maiuscole e minuscole (richiede la corrispondenza esatta del case di caratteri).<p>È inoltre necessario disabilitare la distinzione tra maiuscole e minuscole del kernel di Windows per supportare i nomi di file con distinzione tra maiuscole Per supportare la distinzione tra maiuscole e minuscole, modificare il valore **DWORD** della chiave del registro di sistema, `HKLM\SYSTEM\CurrentControlSet\Control\Session Manager\kernel` , in **0**.</li><li>**ntfscase = `{lower|upper|preserve}` ** : Specifica se la maiuscola/minuscola dei caratteri nei nomi dei file nella file system NTFS verrà restituita in lettere minuscole, maiuscole o nel formato archiviato nella directory. L'impostazione predefinita è **Preserve**. Questa impostazione non può essere modificata se **casesensitivelookups** è impostata su **Sì**.</li></ul> |
| CreateGroup`<name>` | Crea un nuovo gruppo di client, assegnando il nome specificato. |
| listgroups | Visualizza i nomi di tutti i gruppi di client. |
| Deletegroup`<name>` | Rimuove il gruppo di client specificato in base al nome. |
| RENAMEGROUP `<oldname>``<newname>` | Modifica il nome del gruppo di client specificato da *OldName* in *newname*. |
| AddMembers`<hostname>[...]` | Aggiunge un *host* al gruppo di client specificato in base al *nome*. |
| ListMembers`<name>` | Elenca i computer host nel gruppo client specificato in base al *nome*. |
| deletemembers`<hostname><groupname>[...]` | Rimuove il client specificato dall' *host* dal gruppo di client specificato dal *gruppo*. |

### <a name="client-for-nfs-related-parameters"></a>Parametri correlati a client per NFS

| Parametro | Descrizione |
| --------- | ----------- |
| start | Avvia il client per il servizio NFS. |
| stop | Arresta il client per il servizio NFS. |
| config | Specifica le impostazioni generali per client per NFS. Con l'argomento del comando **config** è necessario specificare almeno una delle opzioni seguenti:<ul><li>**FileAccess = `<mode>` ** : Specifica la modalità di autorizzazione predefinita per i file creati nei server NFS (Network File System). L'argomento **mode** è costituito da un numero a tre cifre, compreso tra 0 e 7 (inclusi), che rappresentano le autorizzazioni predefinite concesse all'utente, al gruppo e ad altre. Le cifre vengono convertite in autorizzazioni di tipo UNIX come segue: *0 = nessuna*, *1 = x (esecuzione)*, *2 = w (solo scrittura)*, *3 = WX (scrittura ed esecuzione)*, *4 = r (sola lettura)*, *5 = RX (lettura ed esecuzione)*, *6 = RW (lettura e scrittura)* e *7 = rwx (lettura, scrittura ed esecuzione*). Ad esempio, `fileaccess=750` concede le autorizzazioni di lettura, scrittura ed esecuzione al proprietario, le autorizzazioni di lettura ed esecuzione per il gruppo e nessuna autorizzazione di accesso ad altri utenti.</li><li>**mapsvr = `<server>` ** : Imposta il server come server di mapping dei nomi utente per client per NFS. Anche se questa opzione continua a essere supportata per la compatibilità con le versioni precedenti, è consigliabile usare invece l'utilità sfuadmin.</li><li>**mtype = `{hard|soft}` ** -Specifica il tipo di montaggio predefinito. Per un hard mount, il client per NFS continua a ritentare una RPC non riuscita fino a quando non ha esito positivo. Per un montaggio soft, client per NFS restituisce un errore all'applicazione chiamante dopo aver ritentato la chiamata il numero di volte specificato dall'opzione di ripetizione dei tentativi.</li><li>**nuovo tentativo `<number>` =** : Specifica il numero di tentativi di connessione per un montaggio soft. Questo valore deve essere compreso tra 1 e 10, inclusi. L'impostazione predefinita è **1**.</li><li>**timeout = `<seconds>` ** -Specifica il numero di secondi di attesa di una connessione (chiamata di procedura remota). Questo valore deve essere *0,8*, *0,9*o un numero intero compreso tra *1 e 60*inclusi. Il valore predefinito è **0,8**.</li><li>**protocollo = `{TCP|UDP|TCP+UDP}` ** : Specifica i protocolli di trasporto supportati dal client. L'impostazione predefinita è **TCP + UDP**.</li><li>**rsize. = `<size>` ** : Specifica la dimensione, espressa in kilobyte, del buffer di lettura. Questo valore può essere *0,5, 1, 2, 4, 8, 16* o *32*. Il valore predefinito è **32**.</li><li>**wsize = `<size>` ** : Specifica la dimensione, espressa in kilobyte, del buffer di scrittura. Questo valore può essere *0,5, 1, 2, 4, 8, 16* o *32*. Il valore predefinito è **32**.</li><li>**Perf = default** : Ripristina le impostazioni delle prestazioni seguenti in valori predefiniti, *mtype*, *retry*, *timeout*, *rsize.* o *wsize*. |

### <a name="examples"></a>Esempio

Per arrestare il server per NFS o il client per NFS, digitare:

```
nfsadmin server stop
nfsadmin client stop
```

Per avviare Server per NFS o client per NFS, digitare:

```
nfsadmin server start
nfsadmin client start
```

Per impostare server per NFS in modo che non venga fatta distinzione tra maiuscole e minuscole, digitare:

```
nfsadmin server config casesensitive=no
```

Per impostare client per NFS affinché venga fatta distinzione tra maiuscole e minuscole, digitare:

```
nfsadmin client config casesensitive=yes
```

Per visualizzare tutte le opzioni del server corrente per NFS o client per NFS, digitare:

```
nfsadmin server config
nfsadmin client config
```

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)

- [Riferimenti per i cmdlet NFS](https://docs.microsoft.com/powershell/module/nfs)
