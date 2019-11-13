---
title: nfsadmin
description: 'Argomento dei comandi di Windows per * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 7375b2cf-c6b8-45b5-abf6-6c10e462defd
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 2658cf610e4328d382b9224f4230d68a022d1cc3
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71373227"
---
# <a name="nfsadmin"></a>nfsadmin

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

È possibile utilizzare **nfsadmin** per gestire server per NFS e client per NFS.  
  
## <a name="syntax"></a>Sintassi  
**Server nfsadmin** `[`*nomecomputer*`] [`\-u *username*`[`\-p *password*`]]` \-l  
  
**Server nfsadmin** `[`*nomecomputer*`] [`\-u *UserName* `[`\-p *password*`]]` \-r `{`*client* `|` tutti`}`  
  
**Server nfsadmin** `[`*nomecomputer*`] [`\-u *UserName* `[`\-p *password*`]] {`avvia `|` arresta`}`  
  
**nfsadmin server** `[`*nomecomputer*`] [`\-u *UserName* `[`\-p *password*`]]` *opzione* config`[...]`  
  
**Server nfsadmin** `[`*nomecomputer*`] [`\-u *UserName* `[`\-p *password*`]]` *nome* CreateGroup  
  
**Server nfsadmin** `[`*nomecomputer*`] [`\-u *UserName* `[`\-p *password*`]]` listgroups  
  
**Server nfsadmin** `[`*nomecomputer*`] [`\-u *UserName* `[`\-p *password*`]]` *nome* DeleteGroup  
  
**Server nfsadmin** `[`*nomecomputer*`] [`\-u *UserName* `[`\-p *password*`]]` RENAMEGROUP *OldName NewName*  
  
**Server nfsadmin** `[`*nomecomputer*`] [`\-u *UserName* `[`\-p *password*`]]` AddMembers *nome host*`[...]`  
  
**Server nfsadmin** `[`*nomecomputer*`] [`\-u *UserName* `[`\-p *password*`]]` ListMembers  
  
**Server nfsadmin** `[`*nomecomputer*`] [`\-u *UserName* `[`\-p *password*`]]` host del *gruppo* deletemembers`[...]`  
  
**nfsadmin client** `[`*nomecomputer*`] [`\-u *UserName* `[`\-p *password*`]] {`avvia `|` arresta`}`  
  
**nfsadmin client** `[`*computername*`] [`\-u *UserName* `[`\-p *password*`]]` *opzione* config`[...]`  
  
## <a name="description"></a>Descrizione  
Il comando **nfsadmin**\-line utility amministra server per NFS o client for NFS nel computer locale o remoto che esegue i servizi Microsoft per il file System di rete \(NFS\). Se si è connessi con un account che non dispone dei privilegi necessari, è possibile specificare un nome utente e una password di un account che lo esegue. L'azione eseguita da **nfsadmin** dipende dagli argomenti del comando forniti.  
  
Oltre al servizio\-gli argomenti e le opzioni di comando specifici, **nfsadmin** accetta quanto segue:  
  
*computerName*  
Specifica il computer remoto che si desidera amministrare. È possibile specificare il computer utilizzando un servizio Windows Internet Name Service \(WINS\) nome o un Domain Name System \(nome DNS\) oppure tramite protocollo Internet \(indirizzo IP\).  
  
*nome utente* **\-u**  
Specifica il nome utente dell'utente di cui si desidera utilizzare le credenziali. Potrebbe essere necessario aggiungere il nome di dominio al nome utente nel formato <em>dominio</em> **\\** <em>nome utente</em>  
  
*Password* **\-p**  
Specifica la password dell'utente specificato utilizzando l'opzione **\-u** . Se si specifica l'opzione **\-u** ma si omette l'opzione **\-p** , viene richiesta la password dell'utente.  
  
#### <a name="administering-server-for-nfs"></a>Amministrazione di server per NFS  
Usare il comando **Server nfsadmin** per amministrare server per NFS. L'azione specifica eseguita dal **Server nfsadmin** dipende dall'opzione di comando o dall'argomento specificato:  
  
**\-l**  
Elenca tutti i blocchi utilizzati dai client.  
  
**\-r** {*client* | **All**}  
Rilascia i blocchi mantenuti da un *client* o, se viene specificato **All** , da tutti i client.  
  
**start**  
avvia il server per il servizio NFS.  
  
**stop**  
Arresta il servizio Server per NFS.  
  
**config**  
Specifica le impostazioni generali per server per NFS. Con l'argomento del comando **config** è necessario specificare almeno una delle opzioni seguenti:  
  
<em>server</em> **\=mapsvr**  
Imposta il *Server* come server di mapping dei nomi utente per server per NFS. Anche se questa opzione continua a essere supportata per la compatibilità con le versioni precedenti, è consigliabile usare invece l'utilità **sfuadmin** .  
  
**auditlocation\=** {**eventlog** | **file** | **entrambi** | **nessuno**}  
Specifica se gli eventi verranno controllati e verranno registrati gli eventi. È necessario uno degli argomenti seguenti.  
  
**EventLog**  
Specifica che gli eventi controllati verranno registrati solo nel registro applicazioni Visualizzatore eventi.  
  
**file**  
Specifica che gli eventi controllati verranno registrati solo nel file specificato da fname di **configurazione**.  
  
**sia**  
Specifica che gli eventi controllati verranno registrati nel registro applicazioni di Visualizzatore eventi e nel file specificato dalla **configurazione fname**.  
  
**nessuno**  
Specifica che gli eventi non verranno controllati.  
  
**fname\=** <em>file</em>  
Imposta il file specificato da *file* come file di controllo. Il valore predefinito è% sfudir%\\log\\NfsSvr. log  
  
*dimensioni*\= **\=fsize**  
Imposta *size* come dimensione massima in megabyte del file di controllo. La dimensione massima predefinita è 7 MB.  
  
**audit\=** \[ **\+** | **\-** \]**Mount** \[ **\+** | **\-** \] **\[\+|** **\-\]** \[**Write** **\+|** **\-\]** **\[** **\+|** **\-\]** **Delete** \[ **\+** | **\-** \]**blocco** \[ **\+** | **\-** **tutti**\]  
Specifica gli eventi da registrare. Per avviare la registrazione di un evento, digitare un segno più \( **\+** \) prima del nome dell'evento; per arrestare la registrazione di un evento, digitare un segno meno \( **\-** \) prima del nome dell'evento. Se il segno viene omesso, viene utilizzato il segno più. Non utilizzare **All** con nessun altro nome di evento.  
  
**lockperiod\=** <em>secondi</em>  
Specifica il numero di secondi di attesa da parte di server per NFS per recuperare i blocchi dopo che una connessione a server per NFS è stata persa e quindi ristabilita o dopo il riavvio del servizio Server per NFS.  
  
Portmapprotocol\={TCP | UDP | TCP\+UDP  
Specifica i protocolli di trasporto supportati da portmap. L'impostazione predefinita è **TCP\+UDP**.  
  
mountprotocol\={TCP | UDP | TCP\+UDP}  
Specifica quale protocollo di trasporto supporta il montaggio. L'impostazione predefinita è **TCP\+UDP**.  
  
nfsprotocol\={TCP | UDP | TCP\+UDP}  
Specifica i protocolli di trasporto \(NFS\) supportato da file System di rete. L'impostazione predefinita è **TCP\+UDP**  
  
nlmprotocol\={TCP | UDP | TCP\+UDP}  
Specifica i protocolli di trasporto che Gestione blocchi di rete \(NLM\) supporta. L'impostazione predefinita è **TCP\+UDP**.  
  
nsmprotocol\={TCP | UDP | TCP\+UDP}  
Specifica i protocolli di trasporto che Gestione stato rete \(NSM\) supporta. L'impostazione predefinita è **TCP\+UDP**.  
  
**enableV3\=** {**Yes** | **No**}  
Specifica se saranno supportati i protocolli NFS versione 3. L'impostazione predefinita è **Sì**.  
  
**renewauth\=** {**Yes** | **No**}  
Specifica se sarà necessario riautenticare le connessioni client dopo il periodo specificato da **config renewauthinterval**. L'impostazione predefinita è **No**.  
  
**renewauthinterval\=** <em>secondi</em>  
Specifica il numero di secondi che devono trascorrere prima che venga forzata la riautenticazione di un client se **config renewauth** è impostato su **Yes**. Il valore predefinito è 600 secondi.  
  
<em>dimensioni</em> **\=dircache**  
Specifica le dimensioni in kilobyte della cache di directory. Il numero specificato come *dimensione* deve essere un multiplo di 4 compreso tra 4 e 128. La directory predefinita\-dimensione della cache è 128 KB.  
  
**translationfile**\=\[file\]  
Specifica un file contenente le informazioni di mapping per la sostituzione di caratteri nei nomi dei file quando vengono spostati da Windows\-in base a file System basati su UNIX\-. Se il *file* non è specificato, la conversione di caratteri del nome file è disabilitata. Se il valore di **translationfile** viene modificato, è necessario riavviare il server per rendere effettive le modifiche.  
  
**dotfileshidden**\={**Yes** | **No**}  
Specifica se i file creati con nomi che iniziano con un punto \(.\) verranno contrassegnati come nascosti nel file system di Windows e, di conseguenza, nascosti ai client NFS. L'impostazione predefinita è **No**.  
  
**casesensitivelookups\=** {**Yes** | **No**}  
Specifica se le ricerche nelle directory faranno distinzione tra maiuscole e minuscole \(che richiedono una corrispondenza esatta del case di caratteri\).  
  
È anche necessario disabilitare il case kernel di Windows\-insensibilità affinché Server per NFS supporti i nomi file sensibili\-caso. È possibile disabilitare la distinzione tra maiuscole e minuscole nel kernel di Windows\-la seguente chiave del registro di sistema su 0:  
  
HKLM\\SYSTEM\\CurrentControlSet\\Control\\Session Manager\\kernel  
  
ObCaseInsensitive DWOrd   
  
> [!IMPORTANT]  
> Questa sezione si applica solo a Windows Server 2008 R2, Windows Server 2008 e Windows Server 2003. Questa sezione non si applica a Windows Server 2012 R2 o Windows Server 2012.  
  
**ntfscase\=** {**lower** | **Upper** | **Preserve**}  
Specifica se la distinzione tra maiuscole e minuscole nei nomi dei file del file system NTFS verrà restituita in lettere minuscole, maiuscole o nel formato archiviato nella directory. L'impostazione predefinita è **Preserve**. Questa impostazione non può essere modificata se **casesensitivelookups** è impostata su **Sì**.  
  
*nome* CreateGroup  
Crea un nuovo gruppo di client, assegnando il *nome*specificato.  
  
**listgroups**  
Visualizza i nomi di tutti i gruppi di client.  
  
*nome* DeleteGroup  
rimuove il gruppo di client specificato in base al *nome*.  
  
**RENAMEGROUP** *OldName NewName*  
modifica il nome del gruppo di client specificato da *OldName* in *newname*  
  
**AddMembers** *nome host*\[...\]  
aggiunge l' *host* al gruppo di client specificato in base al *nome*.  
  
*nome* ListMembers  
elenca i computer host nel gruppo client specificato in base al *nome*.  
  
\[*host del gruppo* **deletemembers** ...\]  
rimuove il client specificato dall' *host* dal gruppo di client specificato dal *gruppo*.  
  
Se non si specifica un'opzione di comando o un argomento, il **Server nfsadmin** Visualizza le impostazioni di configurazione del server corrente per NFS.  
  
#### <a name="administering-client-for-nfs"></a>Amministrazione client per NFS  
Usare il comando **client nfsadmin** per amministrare client per NFS. L'azione specifica eseguita dal **client nfsadmin** dipende dall'argomento del comando specificato:  
  
**start**  
avvia il client per il servizio NFS.  
  
**stop**  
Arresta il client per il servizio NFS.  
  
**config**  
Specifica le impostazioni generali per client per NFS. Con l'argomento del comando **config** è necessario specificare almeno una delle opzioni seguenti:  
  
<em>modalità</em> **\=FileAccess**  
-   Specifica la modalità di autorizzazione predefinita per i file creati in Network File System \(NFS\) server. L'argomento *mode* è costituito da un numero di tre cifre compreso tra 0 e 7 \(inclusi\) che rappresentano le autorizzazioni predefinite concesse rispettivamente a utenti, gruppi e altri \(\). Le cifre vengono convertite in UNIX\-stile le autorizzazioni come segue: 0\=None, 1\=x, 2\=w, 3\=WX, 4\=r, 5\=RX, 6\=RW e 7\=rwx. Ad esempio, **fileaccess\=750** concede a rwx l'autorizzazione per il proprietario, l'autorizzazione RX per il gruppo e nessuna autorizzazione di accesso ad altri utenti.  
  
<em>server</em> **\=mapsvr**  
Imposta il *Server* come server di mapping dei nomi utente per client per NFS. Anche se questa opzione continua a essere supportata per la compatibilità con le versioni precedenti, è consigliabile usare invece l'utilità **sfuadmin** .  
  
**mtype\=** {**hard** | **Soft**}  
Specifica il tipo di montaggio predefinito. Per un hard mount, il client per NFS continua a ritentare una RPC non riuscita fino a quando non ha esito positivo. Per un montaggio soft, client per NFS restituisce un errore all'applicazione chiamante dopo aver ritentato la chiamata il numero di volte specificato dall'opzione di **ripetizione dei tentativi** .  
  
<em>numero</em> di **tentativi\=**  
Specifica il numero di volte in cui tentare di effettuare una connessione per un montaggio soft. Questo valore deve essere compreso tra 1 e 10, inclusi. Il valore predefinito è 1.  
  
**timeout\=** <em>secondi</em>  
Specifica il numero di secondi di attesa per una connessione \(\)di chiamata di procedura remota. Questo valore deve essere 0,8, 0,9 o un numero intero compreso tra 1 e 60 inclusi. Il valore predefinito è 0,8.  
  
**\=di protocollo {TCP | UDP | TCP\+UDP}**  
Specifica i protocolli di trasporto supportati dal client. L'impostazione predefinita è **TCP\+UDP**  
  
<em>dimensioni</em> **\=rsize.**  
Specifica le dimensioni, in kilobyte, del buffer di lettura. Questo valore può essere 0,5, 1, 2, 4, 8, 16 o 32. Il valore predefinito è 32.  
  
<em>dimensioni</em> **\=wsize**  
Specifica le dimensioni, in kilobyte, del buffer di scrittura. Questo valore può essere 0,5, 1, 2, 4, 8, 16 o 32. Il valore predefinito è 32.  
  
**prestazioni\=predefinite**  
Ripristina i valori predefiniti delle impostazioni relative alle prestazioni seguenti:  
  
-   **mtype**  
  
-   **tentativi**  
  
-   **timeout**  
  
-   **rsize.**  
  
-   **wsize**  
  
<em>modalità</em> **\=FileAccess**  
Specifica la modalità di autorizzazione predefinita per i file creati in Network File System \(NFS\) server. L'argomento *mode* è costituito da un numero di tre cifre compreso tra 0 e 7 \(inclusi\) che rappresentano le autorizzazioni predefinite concesse rispettivamente a utenti, gruppi e altri \(\). Le cifre vengono convertite in UNIX\-stile le autorizzazioni come segue: 0\=None, 1\=x, 2\=w, 3\=WX, 4\=r, 5\=RX, 6\=RW e 7\=rwx. Ad esempio, **fileaccess\=750** concede a rwx l'autorizzazione per il proprietario, l'autorizzazione RX per il gruppo e nessuna autorizzazione di accesso ad altri utenti.  
  
Se non si specifica un'opzione o un argomento di comando, il **client nfsadmin** Visualizza le impostazioni di configurazione del client corrente per NFS.  
  

