---
title: wevtutil
description: 'Argomento i comandi di Windows per * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: d4c791e0-7e59-45c5-aa55-0223b77a4822 vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: e6d57f95379fce80bec9cb5e8445b28f887123c8
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59826732"
---
# <a name="wevtutil"></a>wevtutil



Consente di recuperare informazioni su log eventi ed entità di pubblicazione. È anche possibile usare questo comando per installare e disinstallare i manifesti dell'evento, eseguire query ed esportare, archiviare e cancellare i registri. Per esempi di utilizzo di questo comando, vedere [Esempi](#BKMK_examples).

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

## <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|{el \| enum-log}|Visualizza i nomi di tutti i log.|
|{gl \| get-log} \<Logname > [/ f:\<formato >]|Visualizza informazioni di configurazione per il log specificato, incluso il fatto che il log è abilitato o disabilitato, il limite corrente di dimensione massima del log e il percorso del file in cui è archiviato il log.|
|{sl \| set-log} \<Logname > [/ e:.\<abilitato >] [/ ricerca per categorie:\<isolamento >] [/ lungo:\<Logpath >] [/ rt:\<conservazione >] [/ ab:\<automatico >] [/ ms:\< MaxSize >] [/ l:\<livello >] [/ k\<parole chiave >] [/ autorità di certificazione:\<canale >] [/ c:\<Config >]|Modifica la configurazione del log specificato.|
|{ep \| enum-publishers}|Consente di visualizzare gli autori di eventi nel computer locale.|
|{gp \| get-publisher} \<Publishername > [/ ge:\<metadati >] [/ gm:\<messaggio >] [/ f:\<formato >]]|Visualizza le informazioni di configurazione per il server di pubblicazione di eventi specificato.|
|{im \| install-manifest} \<manifesto >|Installa gli autori di eventi e log da un manifesto. Per altre informazioni sui manifesti di eventi e l'utilizzo di questo parametro, vedere il SDK del registro eventi di Windows nel sito Web Microsoft Developers Network (MSDN) (https://msdn.microsoft.com).|
|{um \| manifesto disinstallare} \<manifesto >|Disinstalla tutti gli autori e i log da un manifesto. Per altre informazioni sui manifesti di eventi e l'utilizzo di questo parametro, vedere il SDK del registro eventi di Windows nel sito Web Microsoft Developers Network (MSDN) (https://msdn.microsoft.com).|
|{qe \| gli eventi di query} \<percorso > [/ lf:\<FileLog >] [/SQ:\<Structquery >] [/ q:\<Query >] [/ bm:\<segnalibro >] [/ sbm:\<Savebm >] [/ desktop remoto:\< Direzione >] [/ f\<formato >] [/ l:\<delle impostazioni locali >] [/ c:\<conteggio >] [/ e:\<elemento >]|Legge gli eventi dal log eventi, da un file di log o tramite una query strutturata. Per impostazione predefinita, si specifica un nome di log per \<Path >. Tuttavia, se si usa la **/lf** opzione, quindi \<percorso > deve essere un percorso di un file di log. Se si usa la **/sq** parametro, \<percorso > deve essere un percorso di un file contenente una query strutturata.|
|{gli \| get-loginfo} \<Logname > [/ lf:\<FileLog >]|Visualizza informazioni sullo stato relative a un registro eventi o file di log. Se il **/lf** opzione viene utilizzata, \<Logname > è un percorso a un file di log. È possibile eseguire **wevtutil el** per ottenere un elenco di nomi di registro.|
|{epl \| export-log} \<Path > \<Exportfile > [/ lf:\<Logfile >] [/SQ:\<Structquery >] [/ q:\<Query >] [/ow:\<Overwrite >]|Esporta gli eventi dal log eventi da un file di log o utilizzando una query strutturata per il file specificato. Per impostazione predefinita, si specifica un nome di log per \<Path >. Tuttavia, se si usa la **/lf** opzione, quindi \<percorso > deve essere un percorso di un file di log. Se si usa la **/sq** opzione \<percorso > deve essere un percorso di un file contenente una query strutturata. \<Exportfile > è un percorso per il file in cui verranno archiviati gli eventi esportati.|
|{al \| archivelog} \<Logpath > [/ l:\<delle impostazioni locali >]|Consente di archiviare file di log specificato in un formato indipendente. Viene creata una sottodirectory con il nome delle impostazioni locali e tutte le informazioni specifiche delle impostazioni locali vengono salvate in quella sottodirectory. Dopo aver creato il file di log e directory eseguendo **wevtutil al**, gli eventi nel file possono essere letti se il server di pubblicazione sia installato o meno.|
|{cl \| clear-log} \<Logname > [/ bu:\<Backup >]|Cancella eventi dal registro eventi specificato. Il **il comando** opzione può essere utilizzata per eseguire il backup gli eventi cancellati.|

## <a name="options"></a>Opzioni

|Opzione|Descrizione|
|------|-----------|
|/f:\<formato >|Specifica che l'output deve essere nel formato XML o testo. Se \<formato > è XML, viene visualizzato l'output in formato XML. Se \<formato > è testo, viene visualizzato l'output senza tag XML. Il valore predefinito è testo.|
|/e:\<Enabled>|Abilita o disabilita un log. \<Abilitato > può essere true o false.|
|/i:\<Isolation>|Imposta la modalità di isolamento di log. \<Isolamento > può essere sistema, applicazione o personalizzato. La modalità di isolamento di un log determina se un log condivide una sessione con altri registri nella stessa classe di isolamento. Se si specifica l'isolamento di sistema, il log di destinazione condividono almeno delle autorizzazioni con il Registro di sistema di scrittura. Se si specifica l'isolamento delle applicazioni, i registri di destinazione condividono almeno delle autorizzazioni con il registro applicazioni di scrittura. Se si specifica isolamento personalizzato, è necessario fornire anche un descrittore di sicurezza utilizzando il **/ca** (opzione).|
|/LFN:\<Logpath >|Definisce il nome di file di log. \<LogPath > è un percorso completo del file in cui il servizio Registro eventi archivia gli eventi per questo registro.|
|/RT:\<conservazione >|Imposta la modalità di conservazione dei log. \<Conservazione > può essere true o false. La modalità di memorizzazione log determina il comportamento del servizio Registro eventi quando un registro raggiunge la dimensione massima. Se un registro eventi raggiunge la dimensione massima e la modalità di memorizzazione del log è true, gli eventi esistenti vengono conservati e gli eventi in ingresso vengono ignorati. Se la modalità di conservazione dei log è false, gli eventi in ingresso sovrascriveranno gli eventi meno recenti nel registro.|
|/ab:\<automaticamente >|Specifica i criteri di backup automatico di log. \<Auto > può essere true o false. Se questo valore è true, il log verrà eseguito il automaticamente quando viene raggiunta la dimensione massima. Se questo valore è true, il periodo di conservazione (specificata con il **/rt** opzione) deve essere impostata su true.|
|/ms:\<MaxSize>|Imposta la dimensione massima del log in byte. Le dimensioni del log minimo sono 1048576 byte (1024KB) e i file di log vengono sempre multipli di 64KB, pertanto il valore immesso verrà arrotondata conseguenza.|
|/l:\<Level>|Definisce il filtro del livello del log. \<Livello > può essere qualsiasi valore valido di livello. Questa opzione è applicabile solo file di log con una sessione dedicata. È possibile rimuovere un filtro livello impostando <Level> su 0.|
|/k:\<Keywords>|Specifica il filtro di parole chiave del registro. \<Parole chiave > può essere qualsiasi maschera di parola chiave a 64 bit valido. Questa opzione è applicabile solo file di log con una sessione dedicata.|
|/ca:\<Channel>|Imposta l'autorizzazione di accesso per un log eventi. \<Canale > è un descrittore di sicurezza che usa il SDDL Security Descriptor Definition Language (). Per altre informazioni sul formato SDDL, vedere il sito Web Microsoft Developers Network (MSDN) (https://msdn.microsoft.com).|
|/c:\<Config>|Specifica il percorso di un file di configurazione. Questa opzione determinerà le proprietà del registro da leggere dal file di configurazione definito in \<Config >. Se si utilizza questa opzione, non è necessario specificare un <Logname> parametro. Il nome del log verrà letto dal file di configurazione.|
|/Ge:\<metadati >|Ottiene informazioni sui metadati per gli eventi generati dal server di pubblicazione. \<Metadati > può essere true o false.|
|/Gm:\<messaggio >|Viene visualizzato il messaggio effettivo anziché l'ID messaggio numerico. \<Messaggio > può essere true o false.|
|/LF:\<FileLog >|Specifica che gli eventi devono essere letti da un log o da un file di log. \<File di registro > può essere true o false. Se true, il parametro per il comando è il percorso di un file di log.|
|/sq:\<Structquery>|Specifica che gli eventi devono essere ottenuti con una query strutturata. \<Structquery > può essere true o false. Se true, <Path> è il percorso di un file contenente una query strutturata.|
|/q:\<Query>|Definisce la query XPath per filtrare gli eventi che vengono lette o esportati. Se questa opzione non è specificata, verranno restituiti tutti gli eventi o esportati. Questa opzione non è disponibile quando **/sq** è true.|
|/bm:\<segnalibro >|Specifica il percorso di un file che contiene un segnalibro da una query precedente.|
|/SBM:\<Savebm >|Specifica il percorso di un file che viene utilizzato per salvare un segnalibro di questa query. L'estensione del nome file deve essere XML.|
|/rd:\<Direction>|Specifica la direzione in cui gli eventi vengono letti. \<Direzione > può essere true o false. Se true, gli eventi più recenti vengono restituiti per primi.|
|/l:\<Locale>|Definisce una stringa di impostazioni locali che consente di stampare testo dell'evento in una lingua specifica. Disponibile solo durante la stampa di eventi in formato testo tramite il **/f** (opzione).|
|/c:\<Count>|Imposta il numero massimo di eventi da leggere.|
|/e:\<Element>|Include un elemento radice per la visualizzazione di eventi in formato XML. \<Elemento > è la stringa da all'interno dell'elemento radice. Ad esempio, **/e:root** comporterebbe XML che contiene la coppia di elemento radice \<radice ></root>.|
|/Ow:\<Overwrite >|Specifica che il file di esportazione deve essere sovrascritti. \<Sovrascrivere > può essere true o false. Se true e il file di esportazione specificato in <Exportfile> esiste già, verrà sovrascritto senza chiedere conferma.|
|/bu:\<Backup>|Specifica il percorso in un file in cui verranno archiviati gli eventi cancellati. Includere l'estensione evtx nel nome del file di backup.|
|/r:\<Remote>|Esegue il comando in un computer remoto. \<Remoto > è il nome del computer remoto. Il **im** e **um** parametri non supportano l'operazione in modalità remota.|
|/u:\<Username >|Specifica un utente diverso per accedere a un computer remoto. \<Nome utente > è un nome utente nel formato dominio\utente o utente. Questa opzione è applicabile solo quando il **/r** opzione specificata.|
|/p:\<Password>|Specifica la password per l'utente. Se il **/u** viene usata l'opzione e questa opzione non è specificata o \<Password > è "*", l'utente verrà richiesto di immettere una password. Questa opzione è applicabile solo quando il **/u** opzione specificata.|
|/a:\<Auth>|Definisce il tipo di autenticazione per la connessione a un computer remoto. \<Autenticazione > può essere predefinito, Negotiate, Kerberos o NTLM. Il valore predefinito è Negotiate.|
|/uni:\<Unicode>|Visualizza l'output in formato Unicode. \<Unicode > può essere true o false. Se <Unicode> è true, l'output in formato Unicode.|

## <a name="remarks"></a>Note

-   Utilizzo di un file di configurazione con il parametro sl

    Il file di configurazione è un file XML con lo stesso formato dell'output di wevtutil gl \<Logname > /f: XML. Nell'esempio seguente viene illustrato il formato di file di configurazione che Abilita memorizzazione consente autobackup e imposta la dimensione massima del registro nel registro dell'applicazione:  
    ```
    <?xml version="1.0" encoding="UTF-8"?>
    <channel name="Application" isolation="Application"
    xmlns="https://schemas.microsoft.com/win/2004/08/events">
    <logging>
    <retention>true</retention>
    <autoBackup>true</autoBackup>
    <maxSize>9000000</maxSize>
    </logging>
    <publishing>
    </publishing>
    </channel>
    ```

## <a name="BKMK_examples"></a>Esempi

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

#### <a name="additional-references"></a>Altri riferimenti

[Chiave sintassi della riga di comando](command-line-syntax-key.md)
