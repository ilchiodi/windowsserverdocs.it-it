---
title: Esportare una configurazione di Server dei criteri di rete per l'importazione in un altro Server
description: È possibile utilizzare questo argomento per informazioni su come esportare una configurazione di Server dei criteri di rete in Windows Server 2016.
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: d268dc57-78f8-47ba-9a7a-a607e8b9225c
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 141a99e930672d8403315cb6804290d184ef3007
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/28/2018
---
# <a name="export-an-nps-server-configuration-for-import-on-another-server"></a>Esportare una configurazione di Server dei criteri di rete per l'importazione in un altro Server

Si applica a: Windows Server 2016

È possibile esportare l'intera configurazione dei criteri di rete, tra cui i client RADIUS e server, criteri di rete, criteri di richiesta di connessione, del Registro di sistema e la configurazione di registrazione, da un server dei criteri di rete per l'importazione in un altro server dei criteri di rete. 

Per esportare la configurazione dei criteri di rete, utilizzare uno degli strumenti seguenti:

- In Windows Server 2016, Windows Server 2012 R2 e Windows Server 2012, è possibile usare Netsh o è possibile utilizzare Windows PowerShell.
- In Windows Server 2008 R2 e Windows Server 2008, è possibile utilizzare Netsh.

>[!IMPORTANT]
>Non utilizzare questa procedura se il database dei criteri di rete di origine ha un numero di versione superiore rispetto al numero di versione del database dei criteri di rete di destinazione. È possibile visualizzare il numero di versione del database dei criteri di rete dalla visualizzazione del **netsh nps Mostra config** comando.

Poiché le configurazioni di server dei criteri di rete non vengono crittografate nel file XML esportato, l'invio su una rete può comportare un rischio di sicurezza, quindi adottare le precauzioni quando si sposta il file XML dal server di origine per il server di destinazione. Ad esempio, aggiungere il file a un file di archivio protetto crittografato, la password prima di spostare il file. Inoltre, è possibile archiviare il file in un luogo sicuro per impedire che utenti malintenzionati che vi accedono.

>[!NOTE]
>Se la registrazione SQL Server è configurata nel server dei criteri di rete di origine, le impostazioni di registrazione SQL Server non vengono esportate nel file XML. Dopo aver importato il file in un altro server dei criteri di rete, è necessario configurare manualmente la registrazione SQL Server.

## <a name="export-and-import-the-nps-configuration-by-using-windows-powershell"></a>Esportare e importare la configurazione dei criteri di rete utilizzando Windows PowerShell

Per Windows Server 2012 e versioni successive del sistema operativo, è possibile esportare la configurazione dei criteri di rete utilizzando Windows PowerShell.

La sintassi del comando per esportare la configurazione dei criteri di rete è come segue. 

    Export-NpsConfiguration -Path <filename>

La tabella seguente elenca i parametri per il **Export-NpsConfiguration** cmdlet in Windows PowerShell. Parametri in grassetto sono obbligatori.

|Parametro|Descrizione|
|---------|-----------|
|Percorso|Specifica il nome e percorso del file XML in cui si desidera esportare la configurazione del server dei criteri di rete.|

**Credenziali amministrative**

Per completare questa procedura, è necessario essere un membro del gruppo Administrators.

### <a name="export-example"></a>Esempio di esportazione 

Nell'esempio seguente, la configurazione dei criteri di rete viene esportata in un file XML che si trova nell'unità locale. Per eseguire questo comando, eseguire Windows PowerShell come amministratore nel server dei criteri di rete di origine, digitare il comando seguente e premere INVIO.

`Export-NpsConfiguration –Path c:\config.xml` 

Per ulteriori informazioni, vedere [Export-NpsConfiguration](https://technet.microsoft.com/library/jj872749.aspx).

Dopo avere esportato la configurazione dei criteri di rete, copiare il file XML nel server di destinazione.

La sintassi del comando per importare la configurazione dei criteri di rete nel server di destinazione è il seguente.

    Import-NpsConfiguration [-Path] <String> [ <CommonParameters>]

### <a name="import-example"></a>Esempio di importazione

Il comando seguente importa le impostazioni dal file denominato C:\Npsconfig.xml per criteri di rete. Per eseguire questo comando, eseguire Windows PowerShell come amministratore nel server dei criteri di rete di destinazione, digitare il comando seguente e premere INVIO.

    PS C:\> Import-NpsConfiguration -Path "C:\Npsconfig.xml"

Per ulteriori informazioni, vedere [Import-NpsConfiguration](https://technet.microsoft.com/library/jj872750.aspx).

## <a name="export-and-import-the-nps-configuration-by-using-netsh"></a>Esportare e importare la configurazione dei criteri di rete usando Netsh

È possibile utilizzare \(Netsh\) Shell di rete per esportare la configurazione del server dei criteri di rete utilizzando il **netsh nps esportare** comando.

Quando il **netsh nps importare** comando viene eseguito, dei criteri di rete viene aggiornata automaticamente con le impostazioni di configurazione aggiornate. Non è necessario arrestare dei criteri di rete nel computer di destinazione per eseguire il **netsh nps importare** comando, tuttavia se la console dei criteri di rete o snap-in MMC NPS è aperto durante l'importazione di configurazione, le modifiche alla configurazione del server non sono visibili fino a quando non si aggiorna la visualizzazione. 

>[!NOTE]
>Quando si utilizza il **netsh nps esportare** comando, è necessario specificare il parametro del comando **exportPSK** con il valore **Sì**. Questo parametro e il valore in modo esplicito dello stato di comprendere che si desidera esportare la configurazione del server dei criteri di rete e che contiene il file XML esportato, i segreti condivisi non crittografati per i client RADIUS e membri dei gruppi di server RADIUS remoti.

**Credenziali amministrative**

Per completare questa procedura, è necessario essere un membro del gruppo Administrators.

### <a name="to-copy-an-nps-server-configuration-to-another-nps-server-using-netsh-commands"></a>Per copiare una configurazione di server dei criteri di rete in un altro server dei criteri di rete utilizzando i comandi Netsh

1. Nel server dei criteri di rete di origine, aprire **prompt dei comandi**, tipo **netsh**, quindi premere INVIO.

2. Nel **netsh** digitare **dei criteri di rete**, quindi premere INVIO. 

3. Nel **netsh nps** digitare **esportare filename =**"*path\file.xml*" **exportPSK = YES**, dove *percorso* è il percorso della cartella in cui si desidera salvare il file di configurazione, il server dei criteri di rete e *file* è il nome del file XML che si desidera salvare. Premere INVIO. 

Questo archivia le impostazioni di configurazione \(including registry settings\) in un file XML. Il percorso può essere assoluto o relativo, oppure può essere un percorso Universal Naming Convention \(UNC\). Dopo aver premuto INVIO, viene visualizzato un messaggio che indicano se l'esportazione di file è stata eseguita correttamente.

4. Copiare il file creato nel server di destinazione dei criteri di rete.

5. Al prompt dei comandi nel server dei criteri di rete di destinazione, digitare **netsh nps importare filename =**"*path\file.xml*", quindi premere INVIO. Viene visualizzato un messaggio che indicano se l'importazione del file XML è stata completata.

Per ulteriori informazioni su netsh, vedere [Network Shell (Netsh)](../netsh/netsh.md).

