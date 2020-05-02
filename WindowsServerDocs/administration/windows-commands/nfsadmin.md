---
title: nfsadmin
description: Argomento di riferimento per * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 7375b2cf-c6b8-45b5-abf6-6c10e462defd
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: c8020b028a046ead36b5f95604cd81d679861746
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/01/2020
ms.locfileid: "82723770"
---
# <a name="nfsadmin"></a>nfsadmin

> Si applica a: Windows Server (canale semestrale), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

È possibile utilizzare **nfsadmin** per gestire server per NFS e client per NFS.  
  
## <a name="syntax"></a>Sintassi  
**nfsadmin server** `[` *ComputerName*`] [`\-u *UserName*`[`username\-p *Password* password`]]` l \-  
  
**nfsadmin server** `[` *ComputerName*`] [`\-u *username* `[` \- *Password* `{` *client* p password`]]` r client `|` All \-`}`  
  
**nfsadmin server** `[` *ComputerName*`] [`\-u *UserName* `[`username \-p *Password*password`]] {`Start `|` stop`}`  
  
*opzione* di configurazione **nfsadmin server** `[` *nomecomputer*`] [`\-u *nomeutente* `[` \-p *password* `]]``[...]`  
  
**nfsadmin server** `[` *nomecomputer*`] [` \-`]]` *UserName* `[` *Name* u nome utente p *password* CreateGroup\-  
  
**nfsadmin server** `[` *ComputerName*`] [`\-u *UserName* `[`username \-`]]` p *password* listgroups  
  
**nfsadmin server** `[` *nomecomputer*`] [` \-`]]` *UserName* `[` *Name* u nome utente p *password* DeleteGroup\-  
  
**nfsadmin server** `[` *ComputerName*`] [`\-u *UserName* `[`username \-`]]` p *password* RENAMEGROUP *OldName NewName*  
  
**nfsadmin server** `[` *ComputerName*`] [`\-u *UserName* `[`username \-`]]` p *password* AddMembers *host nome*`[...]`  
  
**nfsadmin server** `[` *ComputerName*`] [`\-u *UserName* `[`username \-`]]` p *password* ListMembers  
  
**nfsadmin server** `[` *ComputerName*`] [`\-u *UserName* `[`nomeutente \-`]]` p *password* deletemembers *gruppo host*`[...]`  
  
**nfsadmin client** `[` *ComputerName*`] [`\-u *UserName* `[`username \-p *Password*password`]] {`Start `|` stop`}`  
  
**nfsadmin client** `[` *ComputerName*`] [`\-u *UserName* `[`username \-`]]` p *password* config *opzione*`[...]`  
  
## <a name="description"></a>Descrizione  
L' **nfsadmin** utilità da\-riga di comando nfsadmin amministra server per NFS o client per NFS nel computer locale o remoto che esegue i servizi Microsoft per NFS \(\)di file System di rete. Se si è connessi con un account che non dispone dei privilegi necessari, è possibile specificare un nome utente e una password di un account che lo esegue. L'azione eseguita da **nfsadmin** dipende dagli argomenti del comando forniti.  
  
Oltre a opzioni e\-argomenti del comando specifici del servizio, **nfsadmin** accetta quanto segue:  
  
*computerName*  
Specifica il computer remoto che si desidera amministrare. È possibile specificare il computer utilizzando il nome WINS \(\) di Windows Internet name service o un \(Domain Name System\) nome DNS o l'indirizzo IP \(\) del protocollo Internet.  
  
*nome utente* u ** \-**  
Specifica il nome utente dell'utente di cui si desidera utilizzare le credenziali. Potrebbe essere necessario aggiungere il nome di dominio al nome utente nel formato <em>dominio</em>**\\**<em>nomeutente</em>  
  
*password p* ** \-**  
Specifica la password dell'utente specificato utilizzando l' ** \-opzione u** . Se si specifica l' ** \-opzione u** , ma si ** \-** omette l'opzione p, viene richiesta la password dell'utente.  
  
#### <a name="administering-server-for-nfs"></a>Amministrazione di server per NFS  
Usare il comando **Server nfsadmin** per amministrare server per NFS. L'azione specifica eseguita dal **Server nfsadmin** dipende dall'opzione di comando o dall'argomento specificato:  
  
**\-l**  
Elenca tutti i blocchi utilizzati dai client.  
  
r {*client* | **All**} ** \-**  
Rilascia i blocchi mantenuti da un *client* o, se viene specificato **All** , da tutti i client.  
  
**start**  
avvia il server per il servizio NFS.  
  
**stop**  
Arresta il servizio Server per NFS.  
  
**config**  
Specifica le impostazioni generali per server per NFS. Con l'argomento del comando **config** è necessario specificare almeno una delle opzioni seguenti:  
  
<em>server</em> **mapsvr\=**  
Imposta il *Server* come server di mapping dei nomi utente per server per NFS. Anche se questa opzione continua a essere supportata per la compatibilità con le versioni precedenti, è consigliabile usare invece l'utilità **sfuadmin** .  
  
**auditlocation\=**{**both**il | **file****eventlog** |  | EventLog è**None**}  
Specifica se gli eventi verranno controllati e verranno registrati gli eventi. È necessario uno degli argomenti seguenti.  
  
**EventLog**  
Specifica che gli eventi controllati verranno registrati solo nel registro applicazioni Visualizzatore eventi.  
  
**file**  
Specifica che gli eventi controllati verranno registrati solo nel file specificato da fname di **configurazione**.  
  
**sia**  
Specifica che gli eventi controllati verranno registrati nel registro applicazioni di Visualizzatore eventi e nel file specificato dalla **configurazione fname**.  
  
**nessuno**  
Specifica che gli eventi non verranno controllati.  
  
<em>file</em> **fname\=**  
Imposta il file specificato da *file* come file di controllo. Il valore predefinito è% sfudir\\%\\log NfsSvr. log  
  
**dimensioni\=fsize**\=*size*  
Imposta *size* come dimensione massima in megabyte del file di controllo. La dimensione massima predefinita è 7 MB.  
  
**Revisione\=** \] **create** **all** **mount** **read** **write** **locking** **delete** montaggio lettura scrittura creare \[Elimina **\+** \[ **\+** \] | **\-** \[ | **\-** \] **\-** | **\+** \[ \] **\-** | **\+** \[ \] **\-** | **\+** \[\]**\-**|**\+**\[ **\+** | **\-** \]  
Specifica gli eventi da registrare. Per avviare la registrazione di un evento, digitare un \( **\+** \) segno più prima del nome dell'evento; per arrestare la registrazione di un evento, digitare un \( **\-** \) segno meno prima del nome dell'evento. Se il segno viene omesso, viene utilizzato il segno più. Non utilizzare **All** con nessun altro nome di evento.  
  
**lockperiod\=**<em>secondi</em>  
Specifica il numero di secondi di attesa da parte di server per NFS per recuperare i blocchi dopo che una connessione a server per NFS è stata persa e quindi ristabilita o dopo il riavvio del servizio Server per NFS.  
  
Portmapprotocol\={TCP | UDP | UDP\+TCP  
Specifica i protocolli di trasporto supportati da portmap. L'impostazione predefinita è **UDP\+TCP**.  
  
mountprotocol\={TCP | UDP | UDP\+TCP}  
Specifica quale protocollo di trasporto supporta il montaggio. L'impostazione predefinita è **UDP\+TCP**.  
  
nfsprotocol\={TCP | UDP | UDP\+TCP}  
Specifica i protocolli di trasporto supportati da \(NFS\) per il file System di rete. L'impostazione predefinita è **TCP\+UDP**  
  
nlmprotocol\={TCP | UDP | UDP\+TCP}  
Specifica i protocolli di trasporto supportati da \(gestione\) blocchi di rete nlm. L'impostazione predefinita è **UDP\+TCP**.  
  
nsmprotocol\={TCP | UDP | UDP\+TCP}  
Specifica i protocolli di trasporto supportati da \(NSM\) per la gestione dello stato di rete. L'impostazione predefinita è **UDP\+TCP**.  
  
**enableV3\=**{**Yes** | **No**}  
Specifica se saranno supportati i protocolli NFS versione 3. L'impostazione predefinita è **Sì**.  
  
**renewauth\=**{**Yes** | **No**}  
Specifica se sarà necessario riautenticare le connessioni client dopo il periodo specificato da **config renewauthinterval**. L'impostazione predefinita è **No**.  
  
**renewauthinterval\=**<em>secondi</em>  
Specifica il numero di secondi che devono trascorrere prima che venga forzata la riautenticazione di un client se **config renewauth** è impostato su **Yes**. Il valore predefinito è 600 secondi.  
  
<em>dimensioni</em> **dircache\=**  
Specifica le dimensioni in kilobyte della cache di directory. Il numero specificato come *dimensione* deve essere un multiplo di 4 compreso tra 4 e 128. La dimensione predefinita\-della cache della directory è 128 KB.  
  
**file translationfile**\=\[\]  
Specifica un file contenente le informazioni di mapping per la sostituzione di caratteri nei nomi dei file quando vengono\-spostati da Windows\-in base ai file System basati su UNIX. Se il *file* non è specificato, la conversione di caratteri del nome file è disabilitata. Se il valore di **translationfile** viene modificato, è necessario riavviare il server per rendere effettive le modifiche.  
  
**dotfileshidden**\={**Yes** | **No**}  
Specifica se i file creati con nomi che iniziano con un punto \(. \) verrà contrassegnata come nascosta nel file System di Windows e, di conseguenza, nascosta ai client NFS. L'impostazione predefinita è **No**.  
  
**casesensitivelookups\=**{**Yes** | **No**}  
Specifica se le ricerche nelle directory faranno distinzione \(tra maiuscole e minuscole che richiedono\)una corrispondenza esatta del case di caratteri.  
  
È anche necessario disabilitare la distinzione tra maiuscole e minuscole\-nel kernel di Windows perché server per\-NFS supporti i nomi di file con distinzione tra maiuscole e minuscole È possibile disabilitare la distinzione tra\-maiuscole e minuscole del kernel Windows cancellando la seguente chiave del registro di sistema su 0:  
  
Kernel\\di\\gestione\\\\sessioni\\HKLM System CurrentControlSet Control  
  
ObCaseInsensitive DWOrd   
  
> [!IMPORTANT]  
> Questa sezione si applica solo a Windows Server 2008 R2, Windows Server 2008 e Windows Server 2003. Questa sezione non si applica a Windows Server 2012 R2 o Windows Server 2012.  
  
**ntfscase\=**{**lower** | **upper****Mantieni**superiore | inferiore}  
Specifica se la distinzione tra maiuscole e minuscole nei nomi dei file del file system NTFS verrà restituita in lettere minuscole, maiuscole o nel formato archiviato nella directory. L'impostazione predefinita è **Preserve**. Questa impostazione non può essere modificata se **casesensitivelookups** è impostata su **Sì**.  
  
**creategroup** *nome* CreateGroup  
Crea un nuovo gruppo di client, assegnando il *nome*specificato.  
  
**listgroups**  
Visualizza i nomi di tutti i gruppi di client.  
  
**deletegroup** *nome* DeleteGroup  
rimuove il gruppo di client specificato in base al *nome*.  
  
**RENAMEGROUP** *OldName NewName*  
modifica il nome del gruppo di client specificato da *OldName* in *newname*  
  
**AddMembers** *host*\[del nome...\]  
aggiunge l' *host* al gruppo di client specificato in base al *nome*.  
  
**listmembers** *nome* ListMembers  
elenca i computer host nel gruppo client specificato in base al *nome*.  
  
**deletemembers** *Group Host*host\[del gruppo deletemembers...\]  
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
  
<em>modalità</em> **FileAccess\=**  
-   Specifica la modalità di autorizzazione predefinita per i file creati nei server \(NFS\) di file System di rete. L'argomento *mode* è costituito da un numero di tre cifre \(compreso\) tra 0 e 7 che rappresenta le autorizzazioni predefinite concesse rispettivamente \(\)a user, Group e other. Le cifre vengono convertite\-in autorizzazioni dello stile UNIX come\=segue: 0\=None, 1\=x, 2\=w, 3\=WX, 4\=r, 5\=RX, 6 RW\=e 7 rwx. Ad esempio, **fileaccess\=750** concede a rwx l'autorizzazione per il proprietario, l'autorizzazione RX per il gruppo e nessuna autorizzazione di accesso ad altri utenti.  
  
<em>server</em> **mapsvr\=**  
Imposta il *Server* come server di mapping dei nomi utente per client per NFS. Anche se questa opzione continua a essere supportata per la compatibilità con le versioni precedenti, è consigliabile usare invece l'utilità **sfuadmin** .  
  
**mtype\=**{**hard** | **Soft**}  
Specifica il tipo di montaggio predefinito. Per un hard mount, il client per NFS continua a ritentare una RPC non riuscita fino a quando non ha esito positivo. Per un montaggio soft, client per NFS restituisce un errore all'applicazione chiamante dopo aver ritentato la chiamata il numero di volte specificato dall'opzione di **ripetizione dei tentativi** .  
  
<em>numero</em> di **tentativi\=**  
Specifica il numero di volte in cui tentare di effettuare una connessione per un montaggio soft. Questo valore deve essere compreso tra 1 e 10, inclusi. Il valore predefinito è 1.  
  
**timeout\=**(<em>secondi</em> )  
Specifica il numero di secondi di attesa per una chiamata \(\)di procedura remota della connessione. Questo valore deve essere 0,8, 0,9 o un numero intero compreso tra 1 e 60 inclusi. Il valore predefinito è 0,8.  
  
**Protocollo\={TCP | UDP | UDP\+TCP}**  
Specifica i protocolli di trasporto supportati dal client. L'impostazione predefinita è **TCP\+UDP**  
  
<em>dimensioni</em> **rsize.\=**  
Specifica le dimensioni, in kilobyte, del buffer di lettura. Questo valore può essere 0,5, 1, 2, 4, 8, 16 o 32. Il valore predefinito è 32.  
  
<em>dimensioni</em> **wsize\=**  
Specifica le dimensioni, in kilobyte, del buffer di scrittura. Questo valore può essere 0,5, 1, 2, 4, 8, 16 o 32. Il valore predefinito è 32.  
  
**prestazioni\=predefinite**  
Ripristina i valori predefiniti delle impostazioni relative alle prestazioni seguenti:  
  
-   **mtype**  
  
-   **tentativi**  
  
-   **timeout**  
  
-   **rsize.**  
  
-   **wsize**  
  
<em>modalità</em> **FileAccess\=**  
Specifica la modalità di autorizzazione predefinita per i file creati nei server \(NFS\) di file System di rete. L'argomento *mode* è costituito da un numero di tre cifre \(compreso\) tra 0 e 7 che rappresenta le autorizzazioni predefinite concesse rispettivamente \(\)a user, Group e other. Le cifre vengono convertite\-in autorizzazioni dello stile UNIX come\=segue: 0\=None, 1\=x, 2\=w, 3\=WX, 4\=r, 5\=RX, 6 RW\=e 7 rwx. Ad esempio, **fileaccess\=750** concede a rwx l'autorizzazione per il proprietario, l'autorizzazione RX per il gruppo e nessuna autorizzazione di accesso ad altri utenti.  
  
Se non si specifica un'opzione o un argomento di comando, il **client nfsadmin** Visualizza le impostazioni di configurazione del client corrente per NFS.  
  

