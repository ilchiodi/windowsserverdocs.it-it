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

>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

È possibile utilizzare **nfsadmin** per gestire server per NFS e client per NFS.  
  
## <a name="syntax"></a>Sintassi  
**Server nfsadmin** `[`*nomecomputer*`] [` @ no__t-4U *nomeutente*`[` @ no__t-7P *password*`]]` 0L  
  
**Server nfsadmin** `[`*nomecomputer*`] [` @ no__t-4U *nomeutente* `[` @ no__t-7P *Password*`]]` 0R 1*client* 3 All @ no__t-14  
  
**Server nfsadmin** `[`*nomecomputer*`] [` @ no__t-4U *nomeutente* `[` @ no__t-7P *password*`]] {`start 0 stop @ no__t-11  
  
**Server nfsadmin** `[`*nomecomputer*`] [` @ no__t-4U *nomeutente* `[` @ no__t-7p *password*@no__t- *9 config @no__t*-11  
  
**Server nfsadmin** `[`*nomecomputer*`] [` @ no__t-4U *nomeutente* `[` @ no__t-7P *password*`]]` *nome* CreateGroup  
  
**Server nfsadmin** `[`*nomecomputer*`] [` @ no__t-4U *nomeutente* `[` @ no__t-7P *password*`]]` listgroups  
  
**Server nfsadmin** `[`*nomecomputer*`] [` @ no__t-4U *nomeutente* `[` @ no__t-7P *password*`]]` *nome* DeleteGroup  
  
**Server nfsadmin** `[`*nomecomputer*`] [` @ no__t-4U *nomeutente* `[` @ no__t-7P *password*`]]` RENAMEGROUP *OldName NewName*  
  
**Server nfsadmin** `[`*nomecomputer*`] [` @ no__t-4U *nomeutente* `[` @ no__t-7P *Password*`]]` AddMembers *nome host*1  
  
**Server nfsadmin** `[`*nomecomputer*`] [` @ no__t-4U *nomeutente* `[` @ no__t-7P *password*`]]` ListMembers  
  
**Server nfsadmin** `[`*nomecomputer*`] [` @ no__t-4U *nomeutente* `[` @ no__t-7P *Password*`]]` deletemembers *gruppo host*1  
  
**nfsadmin client** `[`*nomecomputer*`] [` @ no__t-4U *nomeutente* `[` @ no__t-7P *password*`]] {`start 0 stop @ no__t-11  
  
**nfsadmin client** `[`*nomecomputer*`] [` @ no__t-4U *nomeutente* `[` @ no__t-7p *password*@no__t- *9 config @no__t*-11  
  
## <a name="description"></a>Descrizione  
Il comando **nfsadmin** @ no__t-1line Utility amministra server per NFS o client per NFS nel computer locale o remoto che esegue Microsoft Services for Network File System \(NFS @ no__t-3. Se si è connessi con un account che non dispone dei privilegi necessari, è possibile specificare un nome utente e una password di un account che lo esegue. L'azione eseguita da **nfsadmin** dipende dagli argomenti del comando forniti.  
  
Oltre alle opzioni e agli argomenti di comando di Service @ no__t-0specific, **nfsadmin** accetta quanto segue:  
  
*computerName*  
Specifica il computer remoto che si desidera amministrare. È possibile specificare il computer utilizzando un nome Windows Internet Name Service \(WINS @ no__t-1 oppure un nome Domain Name System \(DNS @ no__t-3 o un indirizzo IP \(IP @ no__t-5.  
  
**\-U** *nome utente*  
Specifica il nome utente dell'utente di cui si desidera utilizzare le credenziali. Potrebbe essere necessario aggiungere il nome di dominio al nome utente nel formato <em>dominio</em> **\\** <em>nome utente</em>  
  
*Password* **\-P**  
Specifica la password dell'utente specificato utilizzando l'opzione **\-U** . Se si specifica l'opzione **\-U** ma si omette l'opzione **\-P** , viene richiesta la password dell'utente.  
  
#### <a name="administering-server-for-nfs"></a>Amministrazione di server per NFS  
Usare il comando **Server nfsadmin** per amministrare server per NFS. L'azione specifica eseguita dal **Server nfsadmin** dipende dall'opzione di comando o dall'argomento specificato:  
  
**\-L**  
Elenca tutti i blocchi utilizzati dai client.  
  
**\-r** {*client* | **All**}  
Rilascia i blocchi mantenuti da un *client* o, se viene specificato **All** , da tutti i client.  
  
**start**  
avvia il server per il servizio NFS.  
  
**stop**  
Arresta il servizio Server per NFS.  
  
**config**  
Specifica le impostazioni generali per server per NFS. Con l'argomento del comando **config** è necessario specificare almeno una delle opzioni seguenti:  
  
**mapsvr @ no__t-1**<em>Server</em>  
Imposta il *Server* come server di mapping dei nomi utente per server per NFS. Anche se questa opzione continua a essere supportata per la compatibilità con le versioni precedenti, è consigliabile usare invece l'utilità **sfuadmin** .  
  
**auditlocation @ no__t-1**{**EventLog** | **file** | **both** | **None**}  
Specifica se gli eventi verranno controllati e verranno registrati gli eventi. È necessario uno degli argomenti seguenti.  
  
**EventLog**  
Specifica che gli eventi controllati verranno registrati solo nel registro applicazioni Visualizzatore eventi.  
  
**file**  
Specifica che gli eventi controllati verranno registrati solo nel file specificato da fname di **configurazione**.  
  
**sia**  
Specifica che gli eventi controllati verranno registrati nel registro applicazioni di Visualizzatore eventi e nel file specificato dalla **configurazione fname**.  
  
**nessuno**  
Specifica che gli eventi non verranno controllati.  
  
**fname @ no__t-1**<em>file</em>  
Imposta il file specificato da *file* come file di controllo. Il valore predefinito è% sfudir% @no__t -0log\\nfssvr.log  
  
**fsize @ no__t-1**\=*dimensioni*  
Imposta *size* come dimensione massima in megabyte del file di controllo. La dimensione massima predefinita è 7 MB.  
  
**Audit @ no__t-1**\[ **\+** | **\-** \]**mount** \=0**2**3 @no__t-**15**6**Read** 8**0**@no__t **-21 @no__ t-23**4**scrivere** 6**8**9**1**2**creare** 4**6**7**9**0**Delete** 2 **@no__ t-44**5**7**8**blocco** 0**2**3 @no__t **-55**6**tutto**  
Specifica gli eventi da registrare. Per avviare la registrazione di un evento, digitare un segno più \( **\+** \) prima del nome dell'evento; per arrestare la registrazione di un evento, digitare un segno meno \( **\-** \) prima del nome dell'evento. Se il segno viene omesso, viene utilizzato il segno più. Non utilizzare **All** con nessun altro nome di evento.  
  
**lockperiod @ no__t-1**<em>secondi</em>  
Specifica il numero di secondi di attesa da parte di server per NFS per recuperare i blocchi dopo che una connessione a server per NFS è stata persa e quindi ristabilita o dopo il riavvio del servizio Server per NFS.  
  
Portmapprotocol @ no__t-0 {TCP | UDP | TCP @ no__t-1UDP  
Specifica i protocolli di trasporto supportati da portmap. L'impostazione predefinita è **TCP @ no__t-1UDP**.  
  
mountprotocol @ no__t-0 {TCP | UDP | TCP @ no__t-1UDP}  
Specifica quale protocollo di trasporto supporta il montaggio. L'impostazione predefinita è **TCP @ no__t-1UDP**.  
  
nfsprotocol @ no__t-0 {TCP | UDP | TCP @ no__t-1UDP}  
Specifica i protocolli di trasporto di rete \(NFS @ no__t-1 supportati dal file System. L'impostazione predefinita è **TCP @ no__t-1UDP**  
  
nlmprotocol @ no__t-0 {TCP | UDP | TCP @ no__t-1UDP}  
Specifica i protocolli di trasporto che Gestione blocchi di rete \(NLM @ no__t-1 supporta. L'impostazione predefinita è **TCP @ no__t-1UDP**.  
  
nsmprotocol @ no__t-0 {TCP | UDP | TCP @ no__t-1UDP}  
Specifica i protocolli di trasporto che Gestione stato rete \(NSM @ no__t-1 supporta. L'impostazione predefinita è **TCP @ no__t-1UDP**.  
  
**enableV3 @ no__t-1**{**Yes** | **No**}  
Specifica se saranno supportati i protocolli NFS versione 3. L'impostazione predefinita è **Sì**.  
  
**renewauth @ no__t-1**{**Yes** | **No**}  
Specifica se sarà necessario riautenticare le connessioni client dopo il periodo specificato da **config renewauthinterval**. L'impostazione predefinita è **No**.  
  
**renewauthinterval @ no__t-1**<em>secondi</em>  
Specifica il numero di secondi che devono trascorrere prima che venga forzata la riautenticazione di un client se **config renewauth** è impostato su **Yes**. Il valore predefinito è 600 secondi.  
  
**dircache @ no__t-1**<em>dimensione</em>  
Specifica le dimensioni in kilobyte della cache di directory. Il numero specificato come *dimensione* deve essere un multiplo di 4 compreso tra 4 e 128. La directory predefinita @ no__t-0cache size è 128 KB.  
  
**translationfile**\= @ no__t-2File @ no__t-3  
Specifica un file contenente le informazioni di mapping per la sostituzione di caratteri nei nomi dei file quando vengono spostati da Windows @ no__t-0based a file System UNIX @ no__t-1Basato. Se il *file* non è specificato, la conversione di caratteri del nome file è disabilitata. Se il valore di **translationfile** viene modificato, è necessario riavviare il server per rendere effettive le modifiche.  
  
**dotfileshidden**\= {**Yes** | **No**}  
Specifica se i file creati con nomi che iniziano con un punto \(. \) verranno contrassegnati come nascosti nei file system Windows e, di conseguenza, nascosti nei client NFS. L'impostazione predefinita è **No**.  
  
**casesensitivelookups @ no__t-1**{**Yes** | **No**}  
Specifica se le ricerche nelle directory faranno distinzione tra maiuscole e minuscole \(requiring la corrispondenza esatta del case del carattere @ no__t-1.  
  
È anche necessario disabilitare Windows kernel case @ no__t-0insensitivity affinché Server per NFS supporti i nomi di file case @ no__t-1sensitive. È possibile disabilitare il caso kernel di Windows @ no__t-0insensitivity deselezionando la seguente chiave del registro di sistema su 0:  
  
HKLM @ no__t-0SYSTEM @ no__t-1CurrentControlSet @ no__t-2Control @ no__t-3Session Manager @ no__t-4kernel  
  
ObCaseInsensitive DWOrd   
  
> [!IMPORTANT]  
> Questa sezione si applica solo a Windows Server 2008 R2, Windows Server 2008 e Windows Server 2003. Questa sezione non si applica a Windows Server 2012 R2 o Windows Server 2012.  
  
**ntfscase @ no__t-1**{**Lower** | **Upper** | **Preserve**}  
Specifica se la distinzione tra maiuscole e minuscole nei nomi dei file del file system NTFS verrà restituita in lettere minuscole, maiuscole o nel formato archiviato nella directory. L'impostazione predefinita è **Preserve**. Questa impostazione non può essere modificata se **casesensitivelookups** è impostata su **Sì**.  
  
*nome* CreateGroup  
Crea un nuovo gruppo di client, assegnando il *nome*specificato.  
  
**listgroups**  
Visualizza i nomi di tutti i gruppi di client.  
  
*nome* DeleteGroup  
rimuove il gruppo di client specificato in base al *nome*.  
  
**RENAMEGROUP** *OldName NewName*  
modifica il nome del gruppo di client specificato da *OldName* in *newname*  
  
**AddMembers** *nome host*\[... \]  
aggiunge l' *host* al gruppo di client specificato in base al *nome*.  
  
*nome* ListMembers  
elenca i computer host nel gruppo client specificato in base al *nome*.  
  
*host del gruppo*deletemembers \[... \]  
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
  
**FileAccess @ no__t-1**<em>modalità</em>  
-   Specifica la modalità di autorizzazione predefinita per i file creati in Network File System \(NFS @ no__t-1 server. L'argomento *mode* è costituito da tre cifre da 0 a 7 \(inclusive @ no__t-2 che rappresentano le autorizzazioni predefinite concesse all'utente, al gruppo e ad altre \(respectively @ no__t-4. Le cifre vengono convertite in autorizzazioni UNIX @ no__t-0style come indicato di seguito: 0 @ no__t-0NONE, 1 @ no__t-1x, 2 @ no__t-2W, 3 @ no__t-3wx, 4 @ no__t-4R, 5 @ no__t-5RX, 6 @ no__t-6RW e 7 @ no__t-7rwx. Ad esempio, **FileAccess @ no__t-1750** fornisce l'autorizzazione rwx al proprietario, l'autorizzazione RX al gruppo e nessuna autorizzazione di accesso ad altri utenti.  
  
**mapsvr @ no__t-1**<em>Server</em>  
Imposta il *Server* come server di mapping dei nomi utente per client per NFS. Anche se questa opzione continua a essere supportata per la compatibilità con le versioni precedenti, è consigliabile usare invece l'utilità **sfuadmin** .  
  
**mtype @ no__t-1**{**hard** | **Soft**}  
Specifica il tipo di montaggio predefinito. Per un hard mount, il client per NFS continua a ritentare una RPC non riuscita fino a quando non ha esito positivo. Per un montaggio soft, client per NFS restituisce un errore all'applicazione chiamante dopo aver ritentato la chiamata il numero di volte specificato dall'opzione di **ripetizione dei tentativi** .  
  
**retry @ no__t-1**<em>numero</em>  
Specifica il numero di volte in cui tentare di effettuare una connessione per un montaggio soft. Questo valore deve essere compreso tra 1 e 10, inclusi. Il valore predefinito è 1.  
  
**timeout @ no__t-1**<em>secondi</em>  
Specifica il numero di secondi di attesa per una connessione @no__t la chiamata di procedura 0remote @ no__t-1. Questo valore deve essere 0,8, 0,9 o un numero intero compreso tra 1 e 60 inclusi. Il valore predefinito è 0,8.  
  
**Protocollo @ no__t-1 {TCP | UDP | TCP @ no__t-2UDP}**  
Specifica i protocolli di trasporto supportati dal client. L'impostazione predefinita è **TCP @ no__t-1UDP**  
  
**rsize. @ no__t-1**<em>dimensione</em>  
Specifica le dimensioni, in kilobyte, del buffer di lettura. Questo valore può essere 0,5, 1, 2, 4, 8, 16 o 32. Il valore predefinito è 32.  
  
**wsize @ no__t-1**<em>dimensione</em>  
Specifica le dimensioni, in kilobyte, del buffer di scrittura. Questo valore può essere 0,5, 1, 2, 4, 8, 16 o 32. Il valore predefinito è 32.  
  
**Perf @ no__t-1default**  
Ripristina i valori predefiniti delle impostazioni relative alle prestazioni seguenti:  
  
-   **mtype**  
  
-   **tentativi**  
  
-   **timeout**  
  
-   **rsize.**  
  
-   **wsize**  
  
**FileAccess @ no__t-1**<em>modalità</em>  
Specifica la modalità di autorizzazione predefinita per i file creati in Network File System \(NFS @ no__t-1 server. L'argomento *mode* è costituito da tre cifre da 0 a 7 \(inclusive @ no__t-2 che rappresentano le autorizzazioni predefinite concesse all'utente, al gruppo e ad altre \(respectively @ no__t-4. Le cifre vengono convertite in autorizzazioni UNIX @ no__t-0style come indicato di seguito: 0 @ no__t-0NONE, 1 @ no__t-1x, 2 @ no__t-2W, 3 @ no__t-3wx, 4 @ no__t-4R, 5 @ no__t-5RX, 6 @ no__t-6RW e 7 @ no__t-7rwx. Ad esempio, **FileAccess @ no__t-1750** fornisce l'autorizzazione rwx al proprietario, l'autorizzazione RX al gruppo e nessuna autorizzazione di accesso ad altri utenti.  
  
Se non si specifica un'opzione o un argomento di comando, il **client nfsadmin** Visualizza le impostazioni di configurazione del client corrente per NFS.  
  

