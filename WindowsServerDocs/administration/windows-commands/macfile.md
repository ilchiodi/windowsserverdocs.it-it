---
title: macfile
description: Argomento di riferimento per il comando MacFile, che gestisce il file server per server Macintosh, volumi, directory e file.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: e2ce586c-b316-41d3-90f8-4be0d074cc0e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 740044088bef1537b5b41493f46be9275be84874
ms.sourcegitcommit: 29bc8740e5a8b1ba8f73b10ba4d08afdf07438b0
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/30/2020
ms.locfileid: "84223014"
---
# <a name="macfile"></a>macfile

> Si applica a: Windows Server (canale semestrale), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Gestisce i File Server per server, volumi, directory e file Macintosh. È possibile automatizzare le attività amministrative includendo una serie di comandi nel file batch e li avviando manualmente o a orari prestabiliti.

## <a name="modify-directories-in-macintosh-accessible-volumes"></a>Modificare le directory nei volumi accessibili da Macintosh

Per modificare il nome della directory, il percorso, il proprietario, il gruppo e le autorizzazioni per i volumi accessibili da Macintosh.

### <a name="syntax"></a>Sintassi

```
macfile directory[/server:\\<computername>] /path:<directory> [/owner:<ownername>] [/group:<groupname>] [/permissions:<permissions>]
```

#### <a name="parameters"></a>Parametri

| Parametro | Descrizione |
| --------- | ----------- |
| /server:`\\<computername>` | Specifica il server in cui modificare una directory. Se omesso, l'operazione viene eseguita nel computer locale. |
| /Path`<directory>` | Specifica il percorso della directory che si desidera modificare. Questo parametro è obbligatorio. **Nota:** La directory deve esistere, usando la **directory MacFile** non verranno create directory. |
| /Owner.`<ownername>` | Modifica il proprietario della directory. Se omesso, il nome del proprietario non verrà modificato. |
| /Group`<groupname>` | Specifica o modifica il gruppo primario Macintosh associato alla directory. Se omesso, il gruppo primario rimane invariato. |
| autorizzazioni`<permissions>` | Imposta le autorizzazioni per la directory per il proprietario, il gruppo primario e il mondo (Everyone). Deve essere un numero a 11 cifre, in cui il numero 1 concede l'autorizzazione e 0 revoca l'autorizzazione (ad esempio, 11111011000). Se questo parametro viene omesso, le autorizzazioni rimarranno invariate. |
| /? | Visualizza la guida al prompt dei comandi. |

##### <a name="position-of-permissions-digit"></a>Posizione della cifra delle autorizzazioni

La posizione della cifra delle autorizzazioni determina l'autorizzazione impostata, tra cui:

| Posizione | Imposta l'autorizzazione |
| -------- | --------------- |
| First (Primo) | OwnerSeeFiles |
| Second | OwnerSeeFolders |
| Terzo | OwnerMakechanges |
| Quarto | GroupSeeFiles |
| Quinto | GroupSeeFolders |
| Sesto | GroupMakechanges |
| Settimo | WorldSeeFiles |
| Ottavo | WorldSeeFolders |
| Nono | WorldMakechanges |
| Decimo | La directory non può essere rinominata, spostata o eliminata. |
| Undicesimo | Le modifiche verranno applicate alla directory corrente e tutte le sottodirectory. |

##### <a name="remarks"></a>Commenti

- Se le informazioni fornite contengono spazi o caratteri speciali, racchiudere il testo tra virgolette (ad esempio, " `<computer name>` ").

- Utilizzare la **directory MacFile** per rendere disponibile agli utenti Macintosh una directory esistente in un volume accessibile da Macintosh. Il comando **MacFile directory** non crea directory.

- Utilizzare la gestione di File, il prompt dei comandi, o **nuova cartella macintosh** comando per creare una directory in un volume accessibile da Macintosh prima di utilizzare il **macfile directory** comando.

#### <a name="examples"></a>Esempi

Per assegnare *Visualizza file*, *vedere cartelle*e *modificare* le autorizzazioni per il proprietario, impostare Visualizza autorizzazioni *cartella* su tutti gli altri utenti e impedire che la directory venga rinominata, spostata o eliminata, digitare:

```
macfile directory /path:e:\statistics\may sales /permissions:11111011000
```

Dove la sottodirectory è *May Sales*, situata nelle *statistiche*del volume accessibile da Macintosh, nel E:\ unità del server locale.

## <a name="join-a-macintosh-files-data-and-resource-forks"></a>Aggiungere i dati e i fork di risorse di un file Macintosh

Per specificare il server in cui unire i file, chi ha creato il file, il tipo di file, in cui si trova il fork di dati, dove si trova il fork di risorse e dove deve trovarsi il file di output.

### <a name="syntax"></a>Sintassi

```
macfile forkize[/server:\\<computername>] [/creator:<creatorname>] [/type:<typename>]  [/datafork:<filepath>] [/resourcefork:<filepath>] /targetfile:<filepath>
```

#### <a name="parameters"></a>Parametri

| Parametro | Descrizione |
| --------- | ----------- |
| /server:`\\<computername>` | Specifica il server in cui unire i file. Se omesso, l'operazione viene eseguita nel computer locale. |
| Creatore`<creatorname>` | Specifica l'autore del file. Macintosh Finder USA l'opzione della riga di comando **/Creator** per determinare l'applicazione che ha creato il file. |
| /Type`<typename>` | Specifica il tipo di file. Macintosh Finder USA l'opzione della riga di comando **/Type** per determinare il tipo di file all'interno dell'applicazione che ha creato il file. |
| /DATAFORK:`<filepath>` | Specifica la posizione del fork di dati che deve essere aggiunto. È possibile specificare un percorso remoto. |
| /RESOURCEFORK:`<filepath>` | Specifica la posizione del fork di risorse che deve essere aggiunto. È possibile specificare un percorso remoto. |
| /TargetFile`<filepath>` | Specifica il percorso del file creato mediante l'aggiunta di un fork di dati e un fork di risorse oppure specifica il percorso del file di cui si sta modificando il tipo o l'autore. Il file deve essere nel server specificato. Questo parametro è obbligatorio. |
| /? | Visualizza la guida al prompt dei comandi. |

##### <a name="remarks"></a>Commenti

- Se le informazioni fornite contengono spazi o caratteri speciali, racchiudere il testo tra virgolette (ad esempio, " `<computer name>` ").

#### <a name="examples"></a>Esempi

Per creare il file *tree_app* nel volume accessibile da Macintosh *D:\Release*, usando il fork di risorse *C:\Cross\Mac\Appcode*e per fare in modo che il nuovo file venga visualizzato nei client Macintosh come applicazione (le applicazioni Macintosh usano il tipo *appl*) con l'autore (Signature) impostato su *Magnolia*, digitare:

```
macfile forkize /resourcefork:c:\cross\mac\appcode /type:APPL /creator:MAGNOLIA /targetfile:D:\Release\tree_app
```

Per modificare l'autore del file in *Microsoft word 5,1*, per il file *Word. txt* nella directory *D:\WORD Documents\Group files*, nel server * \\ Servera*, digitare:

```
macfile forkize /server:\\ServerA /creator:MSWD /type:TEXT /targetfile:d:\Word documents\Group files\Word.txt
```

## <a name="change-the-sign-in-message-and-limit-sessions"></a>Modificare il messaggio di accesso e limitare le sessioni

Per modificare il messaggio di accesso visualizzato quando un utente accede al file server per Macintosh Server e per limitare il numero di utenti che possono utilizzare contemporaneamente server di file e di stampa per Macintosh.

### <a name="syntax"></a>Sintassi

```
macfile server [/server:\\<computername>] [/maxsessions:{number | unlimited}] [/loginmessage:<message>]
```

#### <a name="parameters"></a>Parametri

| Parametro | Descrizione |
| --------- |------------ |
| /server:`\\<computername>` | Specifica il server in cui si desidera modificare i parametri. Se omesso, l'operazione viene eseguita nel computer locale. |
| maxsessions`{number | unlimited}` | Specifica il numero massimo di utenti che possono utilizzare contemporaneamente server di file e di stampa per Macintosh. Se omesso, il **maxsessions** l'impostazione per il server rimane invariato. |
| /loginmessage:`<message>` | Modifica il messaggio visualizzato dagli utenti Macintosh quando si accede al file server per Macintosh Server. Il numero massimo di caratteri per il messaggio di accesso è 199. Se omesso, il **loginmessage** messaggio per il server rimane invariato. Per rimuovere un messaggio di accesso esistente, includere il parametro **/loginmessage** , ma lasciare vuota la variabile del *messaggio* . |
| /? | Visualizza la guida al prompt dei comandi. |

##### <a name="remarks"></a>Commenti

- Se le informazioni fornite contengono spazi o caratteri speciali, racchiudere il testo tra virgolette (ad esempio, " `<computer name>` ").

#### <a name="examples"></a>Esempi

Per modificare il numero di file e server di stampa consentiti per le sessioni Macintosh nel server locale a cinque sessioni e per aggiungere il messaggio di accesso "Disconnetti da server per Macintosh al termine", digitare:

```
macfile server /maxsessions:5 /loginmessage:Sign off from Server for Macintosh when you are finished
```

## <a name="add-change-or-remove-macintosh-accessible-volumes"></a>Aggiungere, modificare o rimuovere volumi accessibili da Macintosh

Per aggiungere, modificare o rimuovere un volume accessibile da Macintosh.

### <a name="syntax"></a>Sintassi

```
macfile volume {/add|/set} [/server:\\<computername>] /name:<volumename>/path:<directory>[/readonly:{true | false}] [/guestsallowed:{true | false}] [/password:<password>] [/maxusers:{<number>>|unlimited}]
macfile volume /remove[/server:\\<computername>] /name:<volumename>
```

#### <a name="parameters"></a>Parametri

| Parametro | Descrizione |
| --------- | ----------- |
| `{/add | /set}` | Obbligatorio quando si aggiunge o si modifica un volume accessibile da Macintosh. Aggiunge o modifica il volume specificato. |
| /server:`\\<computername>` | Specifica il server in cui si desidera aggiungere, modificare o rimuovere un volume. Se omesso, l'operazione viene eseguita nel computer locale. |
| /Name`<volumename>` | Obbligatorio. Specifica il nome del volume per essere aggiunto, modificato o rimosso. |
| /Path`<directory>` | Obbligatorio e valido solo quando si aggiunge un volume. Specifica il percorso alla directory radice del volume da aggiungere. |
| ReadOnly`{true | false}` | Specifica se gli utenti possono modificare i file del volume. Usare **true** per specificare che gli utenti non possono modificare i file nel volume. Utilizzare **false** per specificare che gli utenti possono modificare i file nel volume. Se omesso quando si aggiunge un volume, sono consentite modifiche ai file. Se omesso quando si modifica un volume, il **readonly** l'impostazione per il volume non subisce modifiche. |
| guestsallowed`{true | false}` | Specifica se gli utenti accedono come Guest possono utilizzare il volume. Utilizzare **true** per specificare che gli utenti guest possono utilizzare il volume. Utilizzare **false** per specificare che gli utenti guest non possono utilizzare il volume. Se omesso quando si aggiunge un volume, gli utenti guest possono utilizzare il volume. Se omesso quando si modifica un volume, il **guestsallowed** l'impostazione per il volume non subisce modifiche. |
| /password`<password>` | Specifica una password che sarà necessario accedere al volume. Se omesso quando si aggiunge un volume, verrà creata alcuna password. Se omesso quando si modifica un volume, la password rimarrà invariata. |
| MaxUsers`{<number>> | unlimited}` | Specifica il numero massimo di utenti che possono utilizzare simultaneamente i file nel volume. Se omesso quando si aggiunge un volume, un numero illimitato di utenti può utilizzare il volume. Se omesso quando si modifica un volume, il **maxusers** valore rimane invariato.                                                 |
| /remove | Obbligatorio quando si rimuove un volume accessibile da Macintosh. rimuove il volume specificato. |
| /? | Visualizza la guida al prompt dei comandi. |

##### <a name="remarks"></a>Commenti

- Se le informazioni fornite contengono spazi o caratteri speciali, racchiudere il testo tra virgolette (ad esempio, " `<computer name>` ").

#### <a name="examples"></a>Esempi

Per creare un volume denominato *US marketing Statistics* sul server locale, usando la directory *stats* nell'unità e e per specificare che non è possibile accedere al volume dagli utenti guest, digitare:

```
macfile volume /add /name:US Marketing Statistics /guestsallowed:false /path:e:\Stats
```

Per modificare il volume creato in precedenza in modo che sia di sola lettura, per richiedere una password e per impostare il numero massimo di utenti su cinque, digitare:

```
macfile volume /set /name:US Marketing Statistics /readonly:true /password:saturn /maxusers:5
```

Per aggiungere un volume denominato *Landscape Design*, nel server * \\ Magnolia*, usando la directory *Trees* nell'unità e e per specificare che è possibile accedere al volume dagli utenti guest, digitare:

```
macfile volume /add /server:\\Magnolia /name:Landscape Design /path:e:\trees
```

Per rimuovere il volume denominato *report vendite* sul server locale, digitare:

```
macfile volume /remove /name:Sales Reports
```

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)
