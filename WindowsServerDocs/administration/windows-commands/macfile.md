---
title: macfile
description: 'Argomento dei comandi di Windows per * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: e2ce586c-b316-41d3-90f8-4be0d074cc0e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 194d1adaaf64ffee2a217982638ddf0661dd0369
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71374174"
---
# <a name="macfile"></a>macfile

>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Gestisce i File Server per server, volumi, directory e file Macintosh. È possibile automatizzare le attività amministrative includendo una serie di comandi nel file batch e li avviando manualmente o a orari prestabiliti. 
-   [Per modificare le directory nei volumi accessibili da Macintosh](#BKMK_Moddirs)
-   [Per aggiungere i dati e i fork delle risorse di un file Macintosh](#BKMK_Joinforks)
-   [Per modificare il messaggio di accesso e limitare le sessioni](#BKMK_LogonLimit)
-   [Per aggiungere, modificare o rimuovere volumi accessibili da Macintosh](#BKMK_addvol)

## <a name="BKMK_Moddirs"></a>Per modificare le directory nei volumi accessibili da Macintosh

### <a name="syntax"></a>Sintassi
```
macfile directory[/server:\\<computerName>] /path:<directory> [/owner:<OwnerName>] [/group:<GroupName>] [/permissions:<Permissions>]
```

### <a name="parameters"></a>Parametri
-   /Server: \\ @ no__t-1 @ no__t-2 Specifica il server in cui modificare una directory. Se omesso, l'operazione viene eseguita nel computer locale.
-   /path:<directory> Obbligatorio. Specifica il percorso della directory che si desidera modificare. La directory deve esistere. la **directory MacFile** non crea directory.
-   /Owner.: <OwnerName> modifica il proprietario della directory. Se omesso, il proprietario rimane invariato.
-   /Group: <GroupName> Specifica o modifica il gruppo primario Macintosh associato alla directory. Se omesso, il gruppo primario rimane invariato.
-   /Permissions: <Permissions> Imposta le autorizzazioni per la directory per il proprietario, il gruppo primario e il mondo (Everyone). Un numero a 11 cifre viene utilizzato per impostare le autorizzazioni. Il numero 1 concede l'autorizzazione e 0 la revoca dell'autorizzazione (ad esempio 11111011000). Se omesso, le autorizzazioni rimangono invariate.
    La posizione della cifra determina l'autorizzazione è impostata, come descritto nella tabella seguente.

    |Posizione|Imposta l'autorizzazione per|
    |------|------------|
    |Primo|OwnerSeeFiles|
    |Secondo|OwnerSeeFolders|
    |Terzo|OwnerMakechanges|
    |Quarto|GroupSeeFiles|
    |Quinto|GroupSeeFolders|
    |Sesto|GroupMakechanges|
    |Settimo|WorldSeeFiles|
    |Ottavo|WorldSeeFolders|
    |Nono|WorldMakechanges|
    |Decimo|La directory non può essere rinominata, spostata o eliminata.|
    |Undicesimo|Le modifiche verranno applicate alla directory corrente e tutte le sottodirectory.|

-   /?
    Visualizza la guida al prompt dei comandi.

### <a name="remarks"></a>Note
- Se le informazioni fornite contengono spazi o caratteri speciali, racchiudere il testo tra virgolette, ad esempio **"** <em>nome computer</em> **"** .
- Utilizzare **macfiledirectory** per rendere disponibile una directory esistente in un volume accessibile da Macintosh per gli utenti Macintosh. Il **macfiledirectory** comando non crea le directory. Utilizzare la gestione di File, il prompt dei comandi, o **nuova cartella macintosh** comando per creare una directory in un volume accessibile da Macintosh prima di utilizzare il **macfile directory** comando.
  ### <a name="BKMK_Examples"></a>Esempi
  Nell'esempio seguente modifica le autorizzazioni della sottodirectory maggio vendite, nel volume accessibile da Macintosh statistiche, nell'unità E del server locale. Nell'esempio viene assegnata l'autorizzazione Visualizza file, Visualizza cartelle e modifica le autorizzazioni per il proprietario e visualizza file e visualizza le autorizzazioni delle cartelle per tutti gli altri utenti, evitando in tal modo che la directory venga rinominata, spostata o eliminata.
  ```
  macfile directory /path:"e:\statistics\may sales" /permissions:11111011000
  ```

## <a name="BKMK_Joinforks"></a>Per aggiungere i dati e i fork delle risorse di un file Macintosh

### <a name="syntax"></a>Sintassi
```
macfile forkize[/server:\\<computerName>] [/creator:<CreatorName>] [/type:<typeName>]  [/datafork:<Filepath>] [/resourcefork:<Filepath>] /targetfile:<Filepath>
```

### <a name="parameters"></a>Parametri

|         Parametro          |                                                                                                           Descrizione                                                                                                            |
|----------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| /Server: \\ @ no__t-1 @ no__t-2 |                                                            Specifica il server in cui unire i file. Se omesso, l'operazione viene eseguita nel computer locale.                                                            |
|   /Creator: <CreatorName>   |                                      Specifica l'autore del file. Macintosh Finder USA l'opzione della riga di comando **/Creator** per determinare l'applicazione che ha creato il file.                                       |
|      /Type: <typeName>      |                                 Specifica il tipo di file. Macintosh Finder USA l'opzione della riga di comando **/Type** per determinare il tipo di file all'interno dell'applicazione che ha creato il file.                                 |
|    /datafork:<Filepath>    |                                                                   Specifica la posizione del fork di dati che deve essere aggiunto. È possibile specificare un percorso remoto.                                                                   |
|  /resourcefork:<Filepath>  |                                                                 Specifica la posizione del fork di risorse che deve essere aggiunto. È possibile specificare un percorso remoto.                                                                 |
|   /targetfile:<Filepath>   | Obbligatorio. Specifica il percorso del file che viene creato unendo un fork dei dati e un fork delle risorse, o specifica il percorso del file il cui tipo o l'autore si desidera modificare. Il file deve essere nel server specificato. |
|             /?             |                                                                                               Visualizza la guida al prompt dei comandi.                                                                                               |

### <a name="remarks"></a>Note
- Se le informazioni fornite contengono spazi o caratteri speciali, racchiudere il testo tra virgolette, ad esempio **"** <em>nome computer</em> **"** .

### <a name="examples"></a>Esempi
Per creare il file albero nel nel volume accessibile da Macintosh D:\Release, usando il fork di risorse C:\Cross\Mac\Appcode, e per fare in modo che il nuovo file venga visualizzato nei client Macintosh come applicazione (le applicazioni Macintosh usano il tipo APPL) con l'autore (firma ) impostato su MAGNOLIA, digitare:
```
macfile forkize /resourcefork:c:\cross\mac\appcode /type:APPL /creator:MAGNOLIA /targetfile:D:\Release\treeapp
```
Per modificare l'autore del file in Microsoft Word 5,1, per il file WOrd. txt nella directory D:\Word documents\Group files, nel server \\ \ SERverA, digitare:
```
macfile forkize /server:\\servera /creator:MSWD /type:TEXT /targetfile:"d:\Word documents\Group files\Word.txt"
```

## <a name="BKMK_LogonLimit"></a>Per modificare il messaggio di accesso e limitare le sessioni
### <a name="syntax"></a>Sintassi
```
macfile server [/server:\\<computerName>] [/maxsessions:{Number | unlimited}] [/loginmessage:<Message>]
```

### <a name="parameters"></a>Parametri

|               Parametro                |                                                                                                                                                                           Descrizione                                                                                                                                                                            |
|----------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|       /Server: \\ @ no__t-1 @ no__t-2       |                                                                                                                        Specifica il server in cui si desidera modificare i parametri. Se omesso, l'operazione viene eseguita nel computer locale.                                                                                                                         |
| /maxsessions: {numero &#124; illimitato} |                                                                                         Specifica il numero massimo di utenti che possono utilizzare contemporaneamente server di file e di stampa per Macintosh. Se omesso, il **maxsessions** l'impostazione per il server rimane invariato.                                                                                         |
|        /loginmessage: <Message>         | modifica il messaggio visualizzato dagli utenti Macintosh durante l'accesso al file server per Macintosh Server. Il numero massimo di caratteri per il messaggio di accesso è 199. Se omesso, il **loginmessage** messaggio per il server rimane invariato. Per rimuovere un messaggio di accesso esistente, includere il **/loginmessage** parametro, ma lasciare la *messaggio* variabile vuoto. |
|                   /?                   |                                                                                                                                                               Visualizza la guida al prompt dei comandi.                                                                                                                                                               |

### <a name="remarks"></a>Note
- Se le informazioni fornite contengono spazi o caratteri speciali, racchiudere il testo tra virgolette, ad esempio **"** <em>nome computer</em> **"** .

### <a name="examples"></a>Esempi
Per modificare il numero di file e server di stampa per le sessioni Macintosh consentite nel server locale dall'impostazione corrente a cinque sessioni e per aggiungere il messaggio di accesso "Disconnetti da server per Macintosh al termine dell'operazione", digitare:
```
macfile server /maxsessions:5 /loginmessage:"Log off from Server for Macintosh when you are finished."
```

## <a name="BKMK_addvol"></a>Per aggiungere, modificare o rimuovere volumi accessibili da Macintosh
### <a name="syntax"></a>Sintassi
```
macfile volume {/add|/set} [/server:\\<computerName>] /name:<volumeName>/path:<directory>[/readonly:{true | false}] [/guestsallowed:{true | false}] [/password:<Password>] [/maxusers:{<Number>>|unlimited}]
macfile volume /remove[/server:\\<computerName>] /name:<volumeName>
```

### <a name="parameters"></a>Parametri

|              Parametro               |                                                                                                                                                                       Descrizione                                                                                                                                                                        |
|--------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|          {/Add &#124; /set}          |                                                                                                                      Obbligatorio quando si aggiungono o si modifica un volume accessibile da Macintosh. aggiunge o modifica il volume specificato.                                                                                                                       |
|      /Server: \\ @ no__t-1 @ no__t-2      |                                                                                                             Specifica il server in cui si desidera aggiungere, modificare o rimuovere un volume. Se omesso, l'operazione viene eseguita nel computer locale.                                                                                                              |
|          /Name: <volumeName>          |                                                                                                                                          Obbligatorio. Specifica il nome del volume per essere aggiunto, modificato o rimosso.                                                                                                                                           |
|          /path:<directory>           |                                                                                                                Obbligatorio e valido solo quando si aggiunge un volume. Specifica il percorso alla directory radice del volume da aggiungere.                                                                                                                 |
|    /ReadOnly: {true &#124; false}     | Specifica se gli utenti possono modificare i file del volume. digitare true per specificare che gli utenti non possono modificare i file nel volume. digitare false per specificare che gli utenti possono modificare i file nel volume. Se omesso quando si aggiunge un volume, sono consentite modifiche ai file. Se omesso quando si modifica un volume, il **readonly** l'impostazione per il volume non subisce modifiche. |
|  /guestsallowed: {true &#124; false}  |      Specifica se gli utenti accedono come Guest possono utilizzare il volume. digitare true per specificare che gli utenti guest possono utilizzare il volume. digitare false per specificare che gli utenti guest non possono utilizzare il volume. Se omesso quando si aggiunge un volume, gli utenti guest possono utilizzare il volume. Se omesso quando si modifica un volume, il **guestsallowed** l'impostazione per il volume non subisce modifiche.       |
|         /password:<Password>         |                                                                               Specifica una password che sarà necessario accedere al volume. Se omesso quando si aggiunge un volume, verrà creata alcuna password. Se omesso quando si modifica un volume, la password rimarrà invariata.                                                                               |
| /maxusers: {<Number>> &#124; illimitato} |                                                 Specifica il numero massimo di utenti che possono utilizzare simultaneamente i file nel volume. Se omesso quando si aggiunge un volume, un numero illimitato di utenti può utilizzare il volume. Se omesso quando si modifica un volume, il **maxusers** valore rimane invariato.                                                 |
|               /remove                |                                                                                                                                Obbligatorio quando si rimuove un volume accessibile da Macintosh. rimuove il volume specificato.                                                                                                                                |
|                  /?                  |                                                                                                                                                           Visualizza la guida al prompt dei comandi.                                                                                                                                                           |

### <a name="remarks"></a>Note
- Se le informazioni fornite contengono spazi o caratteri speciali, racchiudere il testo tra virgolette, ad esempio **"** <em>nome computer</em> **"** .

### <a name="examples"></a>Esempi
Per creare un volume denominato statistiche di Marketing nel server locale, utilizzando la directory statistiche nell'unità e per specificare che il volume non possa accedere gli utenti Guest, digitare:
```
macfile volume /add /name:"US Marketing Statistics" /guestsallowed:false /path:e:\Stats
```
Per modificare il volume creato in precedenza sia di sola lettura e in modo da richiedere una password e per impostare il numero di utenti massimi di cinque, tipo:
```
macfile volume /set /name:"US Marketing Statistics" /readonly:true /password:saturn /maxusers:5
```
Per aggiungere un volume denominato Landscape Design, nel server \\ \ Magnolia, usando la directory Trees nell'unità E e per specificare che il volume può accedere ai guest, digitare:
```
macfile volume /add /server:\\Magnolia /name:"Landscape Design" /path:e:\trees
```
Per rimuovere il volume denominato rapporti vendite nel server locale, digitare:
```
macfile volume /remove /name:"Sales Reports"
```

## <a name="additional-references"></a>Riferimenti aggiuntivi
-   [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)
