---
title: wevtutil
description: Argomento di riferimento per wevtutil, che consente di recuperare informazioni sui registri eventi e i Publisher.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: d4c791e0-7e59-45c5-aa55-0223b77a4822
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 22195f3a163e1a4123b51d005b0367cc61411651
ms.sourcegitcommit: 29bc8740e5a8b1ba8f73b10ba4d08afdf07438b0
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/30/2020
ms.locfileid: "84222617"
---
# <a name="wevtutil"></a>wevtutil



Consente di recuperare informazioni su log eventi ed entità di pubblicazione. È anche possibile usare questo comando per installare e disinstallare i manifesti dell'evento, eseguire query ed esportare, archiviare e cancellare i registri.

## <a name="syntax"></a>Sintassi

```
wevtutil [{el | enum-logs}] [{gl | get-log} <Logname> [/f:<Format>]]
[{sl | set-log} <Logname> [/e:<Enabled>] [/i:<Isolation>] [/lfn:<Logpath>] [/rt:<Retention>] [/ab:<Auto>] [/ms:<MaxSize>] [/l:<Level>] [/k:<Keywords>] [/ca:<Channel>] [/c:<Config>]]
[{ep | enum-publishers}]
[{gp | get-publisher} <Publishername> [/ge:<Metadata>] [/gm:<Message>] [/f:<Format>]] [{im | install-manifest} <Manifest>]
[{um | uninstall-manifest} <Manifest>] [{qe | query-events} <Path> [/lf:<Logfile>] [/sq:<Structquery>] [/q:<Query>] [/bm:<Bookmark>] [/sbm:<Savebm>] [/rd:<Direction>] [/f:<Format>] [/l:<Locale>] [/c:<Count>] [/e:<Element>]]
[{gli | get-loginfo} <Logname> [/lf:<Logfile>]]
[{epl | export-log} <Path> <Exportfile> [/lf:<Logfile>] [/sq:<Structquery>] [/q:<Query>] [/ow:<Overwrite>]]
[{al | archive-log} <Logpath> [/l:<Locale>]]
[{cl | clear-log} <Logname> [/bu:<Backup>]] [/r:<Remote>] [/u:<Username>] [/p:<Password>] [/a:<Auth>] [/uni:<Unicode>]
```

### <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|{El \| enum-logs}|Visualizza i nomi di tutti i log.|
|{GL \| get-log} \<Logname> [/f: \<Format> ]|Visualizza informazioni di configurazione per il log specificato, incluso il fatto che il log è abilitato o disabilitato, il limite corrente di dimensione massima del log e il percorso del file in cui è archiviato il log.|
|{SL \| set-log} \<Logname> [/e: \<Enabled> ] [/i: \<Isolation> ] [/LFN: \<Logpath> ] [/RT: \<Retention> ] [/AB: \<Auto> ] [/MS: \<MaxSize> ] [/l: \<Level> ] [/k: \<Keywords> ] [/nome CA: \<Channel> ] [/c: \<Config> ]|Modifica la configurazione del log specificato.|
|{EP \| enum-Publishers}|Consente di visualizzare gli autori di eventi nel computer locale.|
|{GP \| Get-Publisher} \<Publishername> [/GE: \<Metadata> ] [/GM: \<Message> ] [/f: \<Format> ]]|Visualizza le informazioni di configurazione per il server di pubblicazione di eventi specificato.|
|{im \| Install-manifesto}\<Manifest>|Installa gli autori di eventi e log da un manifesto. Per ulteriori informazioni sui manifesti degli eventi e sull'utilizzo di questo parametro, vedere l'SDK di registro eventi di Windows nel sito Web Microsoft Developers Network (MSDN) ( [https://msdn.microsoft.com](https://msdn.microsoft.com) ).|
|{um \| Uninstall-manifest}\<Manifest>|Disinstalla tutti gli autori e i log da un manifesto. Per ulteriori informazioni sui manifesti degli eventi e sull'utilizzo di questo parametro, vedere l'SDK di registro eventi di Windows nel sito Web Microsoft Developers Network (MSDN) ( [https://msdn.microsoft.com](https://msdn.microsoft.com) ).|
|{QE \| query-eventi} \<Path> [/LF: \<Logfile> ] [/SQ: \<Structquery> ] [/q: \<Query> ] [/BM: \<Bookmark> ] [/SBM: \<Savebm> ] [/Rd: \<Direction> ] [/f: \<Format> ] [/l: \<Locale> ] [/c: \<Count> ] [/e: \<Element> ]|Legge gli eventi dal log eventi, da un file di log o tramite una query strutturata. Per impostazione predefinita, si specifica un nome di log per \<Path>. Tuttavia, se si utilizza il **/lf** opzione, quindi \<Path> deve essere un percorso di un file di log. Se si utilizza il **/sq** parametro \<Path> deve essere un percorso di un file contenente una query strutturata.|
|{gli \| Get-LogInfo} \<Logname> [/LF: \<Logfile> ]|Visualizza informazioni sullo stato relative a un registro eventi o file di log. Se il **/lf** opzione viene utilizzata, \<Logname> un percorso di un file di log. È possibile eseguire **wevtutil el** per ottenere un elenco di nomi di registro.|
|{EPL \| Export-log} \<Path> \<Exportfile> [/LF: \<Logfile> ] [/SQ: \<Structquery> ] [/q: \<Query> ] [/ow: \<Overwrite> ]|Esporta gli eventi dal log eventi da un file di log o utilizzando una query strutturata per il file specificato. Per impostazione predefinita, si specifica un nome di log per \<Path>. Tuttavia, se si utilizza il **/lf** opzione, quindi \<Path> deve essere un percorso di un file di log. Se si utilizza il **/sq** opzione \<Path> deve essere un percorso di un file contenente una query strutturata. \<Exportfile>percorso del file in cui verranno archiviati gli eventi esportati.|
|{al \| Archive-log} \<Logpath> [/l: \<Locale> ]|Consente di archiviare file di log specificato in un formato indipendente. Viene creata una sottodirectory con il nome delle impostazioni locali e tutte le informazioni specifiche delle impostazioni locali vengono salvate in quella sottodirectory. Dopo aver creato il file di log e directory eseguendo **wevtutil al**, gli eventi nel file possono essere letti se il server di pubblicazione sia installato o meno.|
|{CL \| Clear-log} \<Logname> [/Bu: \<Backup> ]|Cancella eventi dal registro eventi specificato. Il **il comando** opzione può essere utilizzata per eseguire il backup gli eventi cancellati.|

## <a name="options"></a>Opzioni

|       Opzione       |                                                                                                                                                                                                                                                                 Descrizione                                                                                                                                                                                                                                                                  |
|--------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|    /f\<Format>    |                                                                                                                                                               Specifica che l'output deve essere nel formato XML o testo. Se \<Format> è XML, viene visualizzato l'output in formato XML. Se \<Format> è il testo viene visualizzato l'output senza tag XML. L'impostazione predefinita è Text.                                                                                                                                                                |
|   /e:\<Enabled>    |                                                                                                                                                                                                                                         Abilita o disabilita un log. \<Enabled>può essere true o false.                                                                                                                                                                                                                                          |
|  /i\<Isolation>   | Imposta la modalità di isolamento di log. \<Isolation>può essere System, Application o Custom. La modalità di isolamento di un log determina se un log condivide una sessione con altri registri nella stessa classe di isolamento. Se si specifica l'isolamento di sistema, il log di destinazione condividono almeno delle autorizzazioni con il Registro di sistema di scrittura. Se si specifica l'isolamento delle applicazioni, i registri di destinazione condividono almeno delle autorizzazioni con il registro applicazioni di scrittura. Se si specifica isolamento personalizzato, è necessario fornire anche un descrittore di sicurezza utilizzando il **/ca** (opzione). |
|  /LFN:\<Logpath>   |                                                                                                                                                                                                           Definisce il nome di file di log. \<Logpath>è il percorso completo del file in cui il servizio Registro eventi archivia gli eventi per questo log.                                                                                                                                                                                                           |
|  /RT\<Retention>  |                                                            Imposta la modalità di conservazione dei log. \<Retention>può essere true o false. La modalità di memorizzazione log determina il comportamento del servizio Registro eventi quando un registro raggiunge la dimensione massima. Se un registro eventi raggiunge la dimensione massima e la modalità di memorizzazione del log è true, gli eventi esistenti vengono conservati e gli eventi in ingresso vengono ignorati. Se la modalità di conservazione dei log è false, gli eventi in ingresso sovrascriveranno gli eventi meno recenti nel registro.                                                             |
|    /AB\<Auto>     |                                                                                                                                   Specifica i criteri di backup automatico di log. \<Auto>può essere true o false. Se questo valore è true, il log verrà eseguito il automaticamente quando viene raggiunta la dimensione massima. Se questo valore è true, il periodo di conservazione (specificata con il **/rt** opzione) deve essere impostata su true.                                                                                                                                    |
|   /MS\<MaxSize>   |                                                                                                                                                                        Imposta la dimensione massima del log in byte. Le dimensioni del log minimo sono 1048576 byte (1024KB) e i file di log vengono sempre multipli di 64KB, pertanto il valore immesso verrà arrotondata conseguenza.                                                                                                                                                                         |
|    /l:\<Level>     |                                                                                                                                                                     Definisce il filtro del livello del log. \<Level>può essere qualsiasi valore di livello valido. Questa opzione è applicabile solo file di log con una sessione dedicata. È possibile rimuovere un filtro livello impostando <Level> su 0.                                                                                                                                                                      |
|   /k\<Keywords>   |                                                                                                                                                                                         Specifica il filtro di parole chiave del registro. \<Keywords>può essere qualsiasi maschera di parole chiave di 64 bit valida. Questa opzione è applicabile solo file di log con una sessione dedicata.                                                                                                                                                                                         |
|   /nome CA\<Channel>   |                                                                                                                   Imposta l'autorizzazione di accesso per un log eventi. \<Channel>descrittore di sicurezza che utilizza il linguaggio SDDL (Security Descriptor Definition Language). Per ulteriori informazioni sul formato SDDL, vedere il sito Web Microsoft Developers Network (MSDN) ( [https://msdn.microsoft.com](https://msdn.microsoft.com) ).                                                                                                                    |
|    /c\<Config>    |                                                                                                                                  Specifica il percorso di un file di configurazione. Questa opzione determinerà le proprietà del registro da leggere dal file di configurazione definito in \<Config>. Se si utilizza questa opzione, non è necessario specificare un <Logname> parametro. Il nome del log verrà letto dal file di configurazione.                                                                                                                                   |
|  /GE\<Metadata>   |                                                                                                                                                                                                                 Ottiene informazioni sui metadati per gli eventi generati dal server di pubblicazione. \<Metadata>può essere true o false.                                                                                                                                                                                                                 |
|   /GM\<Message>   |                                                                                                                                                                                                                       Viene visualizzato il messaggio effettivo anziché l'ID messaggio numerico. \<Message>può essere true o false.                                                                                                                                                                                                                        |
|   /LF\<Logfile>   |                                                                                                                                                                                  Specifica che gli eventi devono essere letti da un log o da un file di log. \<Logfile>può essere true o false. Se true, il parametro per il comando è il percorso di un file di log.                                                                                                                                                                                   |
| /sq\<Structquery> |                                                                                                                                                                                Specifica che gli eventi devono essere ottenuti con una query strutturata. \<Structquery>può essere true o false. Se true, <Path> è il percorso di un file contenente una query strutturata.                                                                                                                                                                                |
|    /q\<Query>     |                                                                                                                                                                     Definisce la query XPath per filtrare gli eventi che vengono lette o esportati. Se questa opzione non è specificata, verranno restituiti tutti gli eventi o esportati. Questa opzione non è disponibile quando **/sq** è true.                                                                                                                                                                     |
|  BM\<Bookmark>   |                                                                                                                                                                                                                                 Specifica il percorso di un file che contiene un segnalibro da una query precedente.                                                                                                                                                                                                                                 |
|   /SBM:\<Savebm>   |                                                                                                                                                                                                             Specifica il percorso di un file che viene utilizzato per salvare un segnalibro di questa query. L'estensione del nome file deve essere XML.                                                                                                                                                                                                              |
|  /Rd\<Direction>  |                                                                                                                                                                                                   Specifica la direzione in cui gli eventi vengono letti. \<Direction>può essere true o false. Se true, gli eventi più recenti vengono restituiti per primi.                                                                                                                                                                                                   |
|    /l:\<Locale>    |                                                                                                                                                                                          Definisce una stringa di impostazioni locali che consente di stampare testo dell'evento in una lingua specifica. Disponibile solo durante la stampa di eventi in formato testo tramite il **/f** (opzione).                                                                                                                                                                                          |
|    /c\<Count>     |                                                                                                                                                                                                                                                  Imposta il numero massimo di eventi da leggere.                                                                                                                                                                                                                                                  |
|   /e:\<Element>    |                                                                                                                                                           Include un elemento radice per la visualizzazione di eventi in formato XML. \<Element>stringa che si desidera includere all'interno dell'elemento radice. Ad esempio, **/e: root** genera il codice XML che contiene la coppia di elementi radice \<root> </root> .                                                                                                                                                            |
|  / Mostra:\<Overwrite>  |                                                                                                                                                                 Specifica che il file di esportazione deve essere sovrascritti. \<Overwrite>può essere true o false. Se true e il file di esportazione specificato in <Exportfile> esiste già, verrà sovrascritto senza chiedere conferma.                                                                                                                                                                 |
|   Bu\<Backup>    |                                                                                                                                                                                                      Specifica il percorso in un file in cui verranno archiviati gli eventi cancellati. Includere l'estensione evtx nel nome del file di backup.                                                                                                                                                                                                       |
|    /r\<Remote>    |                                                                                                                                                                                            Esegue il comando in un computer remoto. \<Remote>nome del computer remoto. Il **im** e **um** parametri non supportano l'operazione in modalità remota.                                                                                                                                                                                            |
|   /u\<Username>   |                                                                                                                                                                          Specifica un utente diverso per accedere a un computer remoto. \<Username>nome utente nel formato dominio\utente o utente. Questa opzione è applicabile solo quando il **/r** opzione specificata.                                                                                                                                                                          |
|   /p\<Password>   |                                                                                                                                               Specifica la password per l'utente. Se si usa l'opzione **/u** e questa opzione non è specificata o \<Password> è *, all'utente verrà richiesto di immettere una password. Questa opzione è applicabile solo quando si specifica l'opzione \* \* /u* \* .                                                                                                                                                |
|     /a\<Auth>     |                                                                                                                                                                                             Definisce il tipo di autenticazione per la connessione a un computer remoto. \<Auth>può essere default, Negotiate, Kerberos o NTLM. Il valore predefinito è Negotiate.                                                                                                                                                                                              |
|  Universal\<Unicode>   |                                                                                                                                                                                                             Visualizza l'output in formato Unicode. \<Unicode>può essere true o false. Se <Unicode> è true, l'output in formato Unicode.                                                                                                                                                                                                             |

## <a name="remarks"></a>Commenti

-   Utilizzo di un file di configurazione con il parametro sl

    Il file di configurazione è un file XML con lo stesso formato dell'output di wevtutil gl \<Logname> /f:xml. Per visualizzare il formato di un file di configurazione che Abilita la conservazione, Abilita il backup automatico e imposta la dimensione massima del log nel registro applicazioni:
    ```
    <?xml version=1.0 encoding=UTF-8?>
    <channel name=Application isolation=Application
    xmlns=https://schemas.microsoft.com/win/2004/08/events>
    <logging>
    <retention>true</retention>
    <autoBackup>true</autoBackup>
    <maxSize>9000000</maxSize>
    </logging>
    <publishing>
    </publishing>
    </channel>
    ```

## <a name="examples"></a>Esempi

Elencare i nomi di tutti i log:
```
wevtutil el
```
Visualizzare informazioni di configurazione nel Registro di sistema del computer locale in formato XML:
```
wevtutil gl System /f:xml
```
Utilizzare un file di configurazione per gli attributi del registro eventi di set (vedere la sezione Osservazioni per un esempio di un file di configurazione):
```
wevtutil sl /c:config.xml
```
Visualizzare informazioni sull'autore di eventi Microsoft-Windows-Eventlog, inclusi i metadati sugli eventi in grado di generare il server di pubblicazione:
```
wevtutil gp Microsoft-Windows-Eventlog /ge:true
```
Installare i server di pubblicazione e i registri dal file manifesto XML:
```
wevtutil im myManifest.xml
```
Disinstallare autori e i registri dal file manifesto XML:
```
wevtutil um myManifest.xml
```
Visualizzare i tre eventi più recenti nel registro applicazioni in formato testuale:
```
wevtutil qe Application /c:3 /rd:true /f:text
```
Per visualizzare lo stato del registro applicazioni:
```
wevtutil gli Application
```
Esportare gli eventi dal Registro di sistema per C:\backup\system0506.evtx:
```
wevtutil epl System C:\backup\system0506.evtx
```
Cancellare tutti gli eventi nel registro applicazioni dopo averle salvate C:\admin\backups\a10306.evtx:
```
wevtutil cl Application /bu:C:\admin\backups\a10306.evtx
```

#### <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)
