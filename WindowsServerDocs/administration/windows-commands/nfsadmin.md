---
title: nfsadmin
description: 'Argomento i comandi di Windows per * * *- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: c4dc49e23d67ae68c598367de5a3fb0d7d6398a8
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/31/2019
ms.locfileid: "66437160"
---
# <a name="nfsadmin"></a>nfsadmin

>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

È possibile usare **nfsadmin** gestire Server per NFS e Client per NFS.  
  
## <a name="syntax"></a>Sintassi  
**server nfsadmin** `[` *computerName*`] [`\-u *UserName*`[`\-p *Password* `]]` \-l  
  
**server nfsadmin** `[` *computerName*`] [`\-u *UserName* `[` \-p *Password* `]]` \-r `{` *client* `|` tutti`}`  
  
**server nfsadmin** `[` *computerName*`] [`\-u *UserName* `[` \-p *Password* `]] {`avviare `|` arrestare`}`  
  
**server nfsadmin** `[` *computerName*`] [`\-u *UserName* `[` \-p *Password* `]]` config *opzione*`[...]`  
  
**server nfsadmin** `[` *computerName*`] [`\-u *UserName* `[` \-p *Password* `]]` creategroup *nome*  
  
**server nfsadmin** `[` *computerName*`] [`\-u *UserName* `[` \-p *Password* `]]` listgroups  
  
**server nfsadmin** `[` *computerName*`] [`\-u *UserName* `[` \-p *Password* `]]` deletegroup *nome*  
  
**server nfsadmin** `[` *computerName*`] [`\-u *UserName* `[` \-p *Password* `]]` renamegroup *OldName NewName*  
  
**server nfsadmin** `[` *computerName*`] [`\-u *UserName* `[` \-p *Password* `]]` addmembers *nome Host*`[...]`  
  
**server nfsadmin** `[` *computerName*`] [`\-u *UserName* `[` \-p *Password* `]]` listmembers  
  
**server nfsadmin** `[` *computerName*`] [`\-u *UserName* `[` \-p *Password* `]]` deletemembers *gruppo Host*`[...]`  
  
**nfsadmin client** `[` *computerName*`] [`\-u *UserName* `[` \-p *Password* `]] {`avviare `|` arrestare`}`  
  
**nfsadmin client** `[` *computerName*`] [`\-u *UserName* `[` \-p *Password* `]]` config *opzione*`[...]`  
  
## <a name="description"></a>Descrizione  
Il **nfsadmin** comandi\-utilità della riga di Amministra Server per NFS o Client for NFS nel computer locale o remoto che esegue Microsoft Services for Network File System \(NFS\). Se è connessi con un account che non dispone dei privilegi necessari, è possibile specificare un nome utente e password di un account che esegue. Azione eseguita dal **nfsadmin** dipende dagli argomenti di comando immesso.  
  
Oltre alla assistenza\-argomenti specifici del comando e opzioni **nfsadmin** accetta i seguenti:  
  
*computerName*  
Specifica il computer remoto che si desidera amministrare. È possibile specificare il computer utilizzando un Windows Internet Name Service \(WINS\) nome o un sistema di nome di dominio \(DNS\) il nome, o da Internet Protocol \(IP\) indirizzo.  
  
**\-u** *UserName*  
Specifica il nome utente dell'utente le cui credenziali devono essere utilizzati. Potrebbe essere necessario aggiungere il nome di dominio per il nome utente nel formato <em>domain</em> **\\** <em>UserName</em>  
  
**\-p** *Password*  
Specifica la password dell'utente specificato usando il  **\-u** opzione. Se si specifica la  **\-u** opzione ma si omette il  **\-p** opzione, viene richiesto per la password dell'utente.  
  
#### <a name="administering-server-for-nfs"></a>Amministrazione di Server per NFS  
Usare la **nfsadmin server** comando per l'amministrazione Server per NFS. L'azione specifica che **nfsadmin server** dipende l'opzione di comando o l'argomento specificato:  
  
**\-l**  
Elenca tutti i blocchi utilizzati dai client.  
  
**\-r** {*client* | **all**}  
Rilascia i blocchi acquisiti da un *client* oppure, se **tutte** viene specificato, per tutti i client.  
  
**start**  
Avvia il servizio Server per NFS.  
  
**stop**  
Arresta il servizio Server per NFS.  
  
**config**  
Specifica le impostazioni generali per il Server per NFS. È necessario specificare almeno una delle opzioni seguenti con il **config** argomento di comando:  
  
**mapsvr\=** <em>server</em>  
Set *server* come il server Mapping nomi utente per il Server per NFS. Anche se questa opzione continua a essere supportato per la compatibilità con le versioni precedenti, è consigliabile usare la **sfuadmin** utilità invece.  
  
**Mode\=** {**eventlog** | **file** | **entrambi** | **none** }  
Specifica se gli eventi da controllare e in cui verrà registrato l'evento. Uno degli argomenti seguenti è necessario.  
  
**eventlog**  
Specifica che gli eventi controllati verranno registrati solo nell'evento di applicazione Visualizzatore log.  
  
**file**  
Specifica che gli eventi controllati verranno registrati solo nel file specificato da **fname config**.  
  
**both**  
Specifica che gli eventi controllati verranno registrati nel registro applicazioni di Visualizzatore eventi, nonché il file specificato da **fname config**.  
  
**Nessuno**  
Specifica che gli eventi non verranno controllati.  
  
**fname\=** <em>file</em>  
Imposta il file specificato da *file* come il file di controllo. Il valore predefinito è % sfudir\\log\\nfssvr.log  
  
**fsize\=** \=*size*  
Set *dimensioni* come le dimensioni massime in megabyte del file del controllo. La dimensione massima predefinita è 7 MB.  
  
**audit\=** \[ **\+** | **\-** \]**montare** \[ **\+** | **\-** \] **leggere** \[ **\+** | **\-** \] **scrivere** \[ **\+** | **\-** \] **creare** \[ **\+** | **\-** \] **eliminare** \[ **\+** | **\-** \] **blocco** \[ **\+** | **\-** \] **tutti**  
Specifica gli eventi da registrare. Per avviare la registrazione di un evento, digitare un segno più \( **\+** \) prima il nome dell'evento; per arrestare la registrazione di un evento, digitare un segno di sottrazione \( **\-** \) prima il nome dell'evento. Se si omette il segno, presuppone il segno più. Non utilizzare **tutti** con qualsiasi altro nome di evento.  
  
**lockperiod\=** <em>seconds</em>  
Specifica il numero di secondi di attesa di Server per NFS per recuperare i blocchi dopo aver perso e quindi ristabilita una connessione al Server per NFS o dopo che il servizio Server per NFS è stato riavviato.  
  
Portmapprotocol\={TCP | UDP | TCP\+UDP  
Specifica i protocolli di trasporto Portmap supporta. L'impostazione predefinita è **TCP\+UDP**.  
  
mountprotocol\={TCP | UDP | TCP\+UDP}  
Specifica quale trasporto protocolli montare supporta. L'impostazione predefinita è **TCP\+UDP**.  
  
nfsprotocol\={TCP | UDP | TCP\+UDP}  
Specifica i protocolli di trasporto Network File System \(NFS\) supporta. L'impostazione predefinita è **TCP\+UDP**  
  
nlmprotocol\={TCP | UDP | TCP\+UDP}  
Specifica i protocolli di trasporto NLM \(NLM\) supporta. L'impostazione predefinita è **TCP\+UDP**.  
  
nsmprotocol\={TCP | UDP | TCP\+UDP}  
Specifica i protocolli di trasporto NSM \(NSM\) supporta. L'impostazione predefinita è **TCP\+UDP**.  
  
**enableV3\=** {**yes** | **no**}  
Specifica se i protocolli NFS versione 3 saranno supportati. L'impostazione predefinita è **Sì**.  
  
**renewauth\=** {**yes** | **no**}  
Specifica se le connessioni client verrà richiesto di essere autenticata di nuovo dopo il periodo specificato da **config specificare**. L'impostazione predefinita è **alcun**.  
  
**renewauthinterval\=** <em>seconds</em>  
Specifica il numero di secondi che devono trascorrere prima che un client deve essere autenticata di nuovo se **config renewauth** è impostata su **yes**. Il valore predefinito è 600 secondi.  
  
**dircache\=** <em>size</em>  
Specifica la dimensione in KB della cache di directory. Il numero specificato come *dimensioni* deve essere un multiplo di 4 tra 4 e 128. La directory predefinita\-dimensione della cache è 128 KB.  
  
**translationfile**\=\[file\]  
Specifica un file contenente le informazioni di mapping per la sostituzione dei caratteri nei nomi dei file quando vengono spostati da Windows\-basati su UNIX\-basati su file System. Se *file* non viene specificato, conversione caratteri dei nomi file è disabilitata. Se il valore di **translationfile** è stato modificato, è necessario riavviare il server rendere effettiva la modifica.  
  
**dotfileshidden**\={**yes** | **no**}  
Specifica se i file che sono creati con nomi che iniziano con un periodo \(.\) verranno contrassegnati come nascosti nel file system Windows e, di conseguenza, nascosta ai client NFS. L'impostazione predefinita è **alcun**.  
  
**casesensitivelookups\=** {**yes** | **alcun**}  
Specifica se le ricerche di directory saranno distinzione maiuscole / minuscole \(che richiedono la corrispondenza esatta del case carattere\).  
  
È inoltre necessario disabilitare case kernel di Windows\-e minuscole in ordine per il Server per NFS supportare i casi\-nomi file sensibili. È possibile disabilitare i case del kernel Windows\-e minuscole, deselezionando la chiave del Registro di sistema seguente su 0:  
  
HKLM\\SYSTEM\\CurrentControlSet\\controllo\\Gestione sessioni\\kernel  
  
DWOrd  obcaseinsensitive   
  
> [!IMPORTANT]  
> In questa sezione si applica solo a Windows Server 2008 R2, Windows Server 2008 e Windows Server 2003. In questa sezione non si applica a Windows Server 2012 R2 o Windows Server 2012.  
  
**ntfscase\=** {**lower** | **upper** | **preserve**}  
Specifica se le minuscole dei caratteri nei nomi dei file nel file system NTFS saranno restituite in caratteri minuscoli, maiuscoli, o nel formato archiviato nella directory. L'impostazione predefinita è **preservare**. Questa impostazione non può essere modificata se **casesensitivelookups** è impostata su **yes**.  
  
**creategroup** *name*  
Crea un nuovo gruppo di client, assegnare l'oggetto specificato *nome*.  
  
**listgroups**  
Visualizza i nomi di tutti i gruppi di client.  
  
**DeleteGroup** *nome*  
Rimuove il gruppo di client specificato dalla *nome*.  
  
**renamegroup** *OldName NewName*  
modifica il nome del gruppo di client specificato da *OldName* a *NewName*  
  
**AddMembers** *nome Host*\[...\]  
Aggiunge *Host* al gruppo di client specificato dal *nome*.  
  
**ListMembers** *nome*  
Elenca i computer host nel gruppo di client specificato dalla *nome*.  
  
**deletemembers** *gruppo Host*\[...\]  
Rimuove il client specificato dalla *Host* dal gruppo di client specificato dal *gruppo*.  
  
Se non si specifica un'opzione di comando o un argomento, **nfsadmin server** consente di visualizzare il Server corrente per le impostazioni di configurazione di NFS.  
  
#### <a name="administering-client-for-nfs"></a>Amministrazione di Client per NFS  
Usare la **nfsadmin client** comando per amministrare i Client per NFS. L'azione specifica che **nfsadmin client** dipende l'argomento del comando si specifica:  
  
**start**  
Avvia il servizio Client per NFS.  
  
**stop**  
Arresta il servizio Client per NFS.  
  
**config**  
Specifica le impostazioni generali per Client per NFS. È necessario specificare almeno una delle opzioni seguenti con il **config** argomento di comando:  
  
**fileaccess\=** <em>mode</em>  
-   Specifica la modalità di autorizzazione predefinita per i file creati sul File System di rete \(NFS\) server. Il *modalità* argomento è costituito da un tre cifre da 0 a 7 \(inclusivo\) che rappresenta le autorizzazioni predefinite concesse all'utente, gruppo e altri \(rispettivamente\). Le cifre convertite in UNIX\-definire lo stile delle autorizzazioni, come indicato di seguito: 0\=none, 1\=, x2\=w, 3\=wx, 4\=r, 5\=rx, 6\=rw, con la versione 7\=rwx. Ad esempio, **fileaccess\=750** offre autorizzazioni rwx per il proprietario dell'autorizzazione di rx per il gruppo e nessuna autorizzazione di accesso ad altri utenti.  
  
**mapsvr\=** <em>server</em>  
Set *server* come server di Mapping nomi utente per Client per NFS. Anche se questa opzione continua a essere supportato per la compatibilità con le versioni precedenti, è consigliabile usare la **sfuadmin** utilità invece.  
  
**mtype\=** {**hard** | **soft**}  
Specifica il tipo di montaggio predefinito. Per un hard mount Client per NFS continua a ritentare una chiamata RPC non riuscite, fino a quando ha esito positivo. Per un soft mount Client per NFS restituisce un errore all'applicazione chiamante dopo il nuovo tentativo di chiamata il numero di volte specificato dal **ripetere** opzione.  
  
**retry\=** <em>number</em>  
Specifica il numero di tentativi stabilire una connessione per un soft mount. Questo valore deve essere compreso tra 1 e 10, inclusi. Il valore predefinito è 1.  
  
**timeout\=** <em>seconds</em>  
Specifica il numero di secondi di attesa per una connessione \(chiamata di procedura remota\). Questo valore deve essere 0.8, 0.9 o intero compreso tra 1 e 60, inclusivo. Il valore predefinito è 0,8.  
  
**Protocollo\={TCP | UDP | TCP\+UDP}**  
Specifica il client supporta i protocolli di trasporto. L'impostazione predefinita è **TCP\+UDP**  
  
**rsize\=** <em>size</em>  
Specifica la dimensione, espressa in kilobyte, del buffer di lettura. Questo valore può essere 0.5, 1, 2, 4, 8, 16 o 32. Il valore predefinito è 32.  
  
**wsize\=** <em>size</em>  
Specifica la dimensione, espressa in kilobyte, del buffer di scrittura. Questo valore può essere 0.5, 1, 2, 4, 8, 16 o 32. Il valore predefinito è 32.  
  
**perf\=default**  
Consente di ripristinare le impostazioni delle prestazioni seguenti sui valori predefiniti:  
  
-   **mtype**  
  
-   **retry**  
  
-   **timeout**  
  
-   **rsize**  
  
-   **wsize**  
  
**fileaccess\=** <em>mode</em>  
Specifica la modalità di autorizzazione predefinita per i file creati sul File System di rete \(NFS\) server. Il *modalità* argomento è costituito da un tre cifre da 0 a 7 \(inclusivo\) che rappresenta le autorizzazioni predefinite concesse all'utente, gruppo e altri \(rispettivamente\). Le cifre convertite in UNIX\-definire lo stile delle autorizzazioni, come indicato di seguito: 0\=none, 1\=, x2\=w, 3\=wx, 4\=r, 5\=rx, 6\=rw, con la versione 7\=rwx. Ad esempio, **fileaccess\=750** offre autorizzazioni rwx per il proprietario dell'autorizzazione di rx per il gruppo e nessuna autorizzazione di accesso ad altri utenti.  
  
Se non si specifica un'opzione di comando o un argomento, **nfsadmin client** consente di visualizzare le impostazioni di configurazione di NFS Client corrente.  
  

