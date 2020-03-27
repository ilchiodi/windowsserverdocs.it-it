---
title: Esportare una configurazione NPS per l'importazione in un altro server
description: È possibile utilizzare questo argomento per informazioni su come esportare una configurazione del server dei criteri di rete in Windows Server 2016.
manager: brianlic
ms.prod: windows-server
ms.technology: networking
ms.topic: article
ms.assetid: d268dc57-78f8-47ba-9a7a-a607e8b9225c
ms.author: lizross
author: eross-msft
ms.openlocfilehash: 8c1aef88aec45ee63614b889658daceca3779e91
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/26/2020
ms.locfileid: "80316007"
---
# <a name="export-an-nps-configuration-for-import-on-another-server"></a>Esportare una configurazione NPS per l'importazione in un altro server

Si applica a: Windows Server 2016

È possibile esportare l'intera configurazione del server dei criteri di rete, inclusi i client e i server RADIUS, i criteri di rete, i criteri di richiesta di connessione, il registro di sistema e la configurazione di registrazione, da un server dei criteri di 

Per esportare la configurazione NPS, utilizzare uno degli strumenti seguenti:

- In Windows Server 2016, Windows Server 2012 R2 e Windows Server 2012, è possibile usare netsh oppure è possibile usare Windows PowerShell.
- In Windows Server 2008 R2 e Windows Server 2008 usare netsh.

> [!IMPORTANT]
> Non utilizzare questa procedura se il numero di versione del database NPS di origine è superiore a quello del database di server dei criteri di destinazione. È possibile visualizzare il numero di versione del database NPS dalla visualizzazione del comando **netsh nps show config** .

Poiché le configurazioni del server dei criteri di rete non sono crittografate nel file XML esportato, l'invio su una rete potrebbe causare un rischio per la sicurezza, pertanto è necessario prestare attenzione quando si trasferisce il file XML dal server di origine ai server di destinazione. Ad esempio, aggiungere il file a un file di archivio crittografato e protetto da password prima di trasferire il file. Inoltre, archiviare il file in un percorso sicuro per impedire l'accesso da parte di utenti malintenzionati.

> [!NOTE]
> Se SQL Server registrazione è configurata nel server dei criteri di accesso di origine, le impostazioni di registrazione SQL Server non vengono esportate nel file XML. Dopo aver importato il file in un altro server dei criteri di server, è necessario configurare manualmente la registrazione SQL Server.

## <a name="export-and-import-the-nps-configuration-by-using-windows-powershell"></a>Esportare e importare la configurazione NPS usando Windows PowerShell

Per Windows Server 2012 e versioni successive del sistema operativo, è possibile esportare la configurazione del server dei criteri di rete tramite Windows PowerShell.

Di seguito è riportata la sintassi del comando per l'esportazione della configurazione NPS. 

    Export-NpsConfiguration -Path <filename>

La tabella seguente elenca i parametri per il cmdlet **Export-NpsConfiguration** in Windows PowerShell. I parametri in grassetto sono obbligatori.

|Parametro|Descrizione|
|---------|-----------|
|Percorso|Specifica il nome e il percorso del file XML in cui si desidera esportare la configurazione NPS.|

**Credenziali amministrative**

Per completare questa procedura è necessaria l'appartenenza al gruppo Administrators.

### <a name="export-example"></a>Esempio di esportazione 

Nell'esempio seguente, la configurazione NPS viene esportata in un file XML che si trova nell'unità locale. Per eseguire questo comando, eseguire Windows PowerShell come amministratore nel server dei criteri di accesso di origine, digitare il comando seguente e premere INVIO.

`Export-NpsConfiguration –Path c:\config.xml` 

Per altre informazioni, vedere [Export-NpsConfiguration](https://technet.microsoft.com/library/jj872749.aspx).

Dopo aver esportato la configurazione NPS, copiare il file XML nel server di destinazione.

Di seguito è riportata la sintassi del comando per l'importazione della configurazione NPS nel server di destinazione.

    Import-NpsConfiguration [-Path] <String> [ <CommonParameters>]

### <a name="import-example"></a>Esempio di importazione

Il seguente comando importa le impostazioni dal file denominato C:\Npsconfig.xml a NPS. Per eseguire questo comando, eseguire Windows PowerShell come amministratore nel server dei criteri di accesso di destinazione, digitare il comando seguente e premere INVIO.

    PS C:\> Import-NpsConfiguration -Path "C:\Npsconfig.xml"

Per altre informazioni, vedere [Import-NpsConfiguration](https://technet.microsoft.com/library/jj872750.aspx).

## <a name="export-and-import-the-nps-configuration-by-using-netsh"></a>Esportare e importare la configurazione NPS tramite Netsh

È possibile utilizzare la shell di rete \(netsh\) per esportare la configurazione del server dei criteri di rete utilizzando il comando **netsh nps export** .

Quando si esegue il comando **netsh nps import** , il server dei criteri di server viene aggiornato automaticamente con le impostazioni di configurazione aggiornate. Non è necessario arrestare il server dei criteri di server nel computer di destinazione per eseguire il comando **netsh nps import** . Tuttavia, se la console NPS o lo snap-in MMC NPS è aperto durante l'importazione della configurazione, le modifiche apportate alla configurazione del server non saranno visibili finché non si aggiorna la visualizzazione. 

> [!NOTE]
> Quando si usa il comando **netsh nps export** , è necessario specificare il parametro Command **exportPSK** con il valore **Yes**. Questo parametro e il valore dichiarano in modo esplicito che si è a conoscenza dell'esportazione della configurazione NPS e che il file XML esportato contiene segreti condivisi non crittografati per i client RADIUS e i membri dei gruppi di server RADIUS remoti.

**Credenziali amministrative**

Per completare questa procedura è necessaria l'appartenenza al gruppo Administrators.

### <a name="to-copy-an-nps-configuration-to-another-nps-using-netsh-commands"></a>Per copiare una configurazione NPS in un altro server dei criteri di server utilizzando comandi netsh

1. Nel server dei criteri di accesso di origine aprire il **prompt dei comandi**, digitare **netsh**, quindi premere INVIO.

2. Al prompt dei **comandi netsh** Digitare **NPS**, quindi premere INVIO. 

3. Al prompt **netsh NPS** Digitare **Export filename =** "*path\file.XML*" **exportPSK = Yes**, dove *percorso* è il percorso della cartella in cui si desidera salvare il file di configurazione NPS e *file* è il nome del file XML che si desidera salvare. Premere INVIO. 

Archivia le impostazioni di configurazione \(incluse le impostazioni del registro di sistema\) in un file XML. Il percorso può essere relativo o assoluto oppure può essere un Universal Naming Convention \(percorso UNC\). Dopo aver premuto INVIO, viene visualizzato un messaggio che indica se l'esportazione nel file è stata completata correttamente.

4. Copiare il file creato nel server dei criteri di server di destinazione.

5. Al prompt dei comandi nel server dei criteri di accesso di destinazione, digitare **netsh nps import filename =** "*path\file.XML*" e quindi premere INVIO. Viene visualizzato un messaggio che indica se l'importazione dal file XML è stata completata correttamente.

Per ulteriori informazioni su netsh, vedere la pagina relativa alla [Shell di rete (netsh)](../netsh/netsh.md).

