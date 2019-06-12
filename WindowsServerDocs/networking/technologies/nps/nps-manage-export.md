---
title: Esportare una configurazione di NPS per l'importazione in un altro Server
description: È possibile utilizzare questo argomento per informazioni su come esportare una configurazione di Server dei criteri di rete in Windows Server 2016.
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: d268dc57-78f8-47ba-9a7a-a607e8b9225c
ms.author: pashort
author: shortpatti
ms.openlocfilehash: b95e39af63e284d0147335faabfb740c0dd175bc
ms.sourcegitcommit: 6ef4986391607bb28593852d06cc6645e548a4b3
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 06/07/2019
ms.locfileid: "66812298"
---
# <a name="export-an-nps-configuration-for-import-on-another-server"></a>Esportare una configurazione di NPS per l'importazione in un altro Server

Si applica a: Windows Server 2016

È possibile esportare l'intera configurazione dei criteri di rete, tra cui i client RADIUS e server, criteri di rete, il criterio di richiesta di connessione, del Registro di sistema e la configurazione della registrazione, ovvero da uno dei criteri di rete per l'importazione in un'altra dei criteri di rete. 

Per esportare la configurazione dei criteri di rete, usare uno degli strumenti seguenti:

- In Windows Server 2016, Windows Server 2012 R2 e Windows Server 2012, è possibile usare Netsh o è possibile usare Windows PowerShell.
- In Windows Server 2008 R2 e Windows Server 2008, è possibile usare Netsh.

> [!IMPORTANT]
> Non utilizzare questa procedura se il database dei criteri di rete di origine ha un numero di versione superiore rispetto al numero di versione del database dei criteri di rete di destinazione. È possibile visualizzare il numero di versione del database dei criteri di rete dalla visualizzazione del **netsh nps Mostra config** comando.

Poiché le configurazioni dei criteri di rete non sono crittografate nel file XML esportato, inviarli in una rete potrebbe comportare un rischio per la sicurezza, pertanto, adottare precauzioni quando si sposta il file XML dal server di origine per i server di destinazione. Ad esempio, aggiungere il file in un file di archivio protetto password crittografata, prima di spostare il file. Inoltre, è possibile archiviare il file in un luogo sicuro per impedire a utenti malintenzionati di accedervi.

> [!NOTE]
> Se la registrazione di SQL Server è configurata nell'origine dei criteri di rete, le impostazioni di registrazione di SQL Server non vengono esportate nel file XML. Dopo aver importato il file in un altro dei criteri di rete, è necessario configurare manualmente la registrazione di SQL Server.

## <a name="export-and-import-the-nps-configuration-by-using-windows-powershell"></a>Esportare e importare la configurazione dei criteri di rete tramite Windows PowerShell

Per Windows Server 2012 e versioni successive del sistema operativo, è possibile esportare la configurazione dei criteri di rete tramite Windows PowerShell.

La sintassi del comando per esportare la configurazione dei criteri di rete è come indicato di seguito. 

    Export-NpsConfiguration -Path <filename>

La tabella seguente elenca i parametri per il **Export-NpsConfiguration** cmdlet in Windows PowerShell. Parametri in grassetto sono obbligatori.

|Parametro|Descrizione|
|---------|-----------|
|`Path`|Specifica il nome e percorso del file XML a cui si desidera esportare la configurazione dei criteri di rete.|

**Credenziali amministrative**

Per completare questa procedura, è necessario essere un membro del gruppo Administrators.

### <a name="export-example"></a>Esempio di esportazione 

Nell'esempio seguente, la configurazione dei criteri di rete viene esportata in un file XML che si trova nell'unità locale. Per eseguire questo comando, eseguire Windows PowerShell come amministratore dell'origine dei criteri di rete, digitare il comando seguente e premere INVIO.

`Export-NpsConfiguration –Path c:\config.xml` 

Per altre informazioni, vedere [Export-NpsConfiguration](https://technet.microsoft.com/library/jj872749.aspx).

Dopo aver esportato la configurazione dei criteri di rete, copiare il file XML al server di destinazione.

La sintassi del comando per importare la configurazione dei criteri di rete nel server di destinazione è come indicato di seguito.

    Import-NpsConfiguration [-Path] <String> [ <CommonParameters>]

### <a name="import-example"></a>Esempio di importazione

Il comando seguente importa le impostazioni nel file denominato C:\Npsconfig.xml a NPS. Per eseguire questo comando, eseguire Windows PowerShell come amministratore nella destinazione dei criteri di rete, digitare il comando seguente e premere INVIO.

    PS C:\> Import-NpsConfiguration -Path "C:\Npsconfig.xml"

Per altre informazioni, vedere [Import-NpsConfiguration](https://technet.microsoft.com/library/jj872750.aspx).

## <a name="export-and-import-the-nps-configuration-by-using-netsh"></a>Esportare e importare la configurazione dei criteri di rete usando Netsh

È possibile usare la Shell di rete \(Netsh\) per esportare la configurazione dei criteri di rete tramite il **netsh nps export** comando.

Quando la **netsh nps importare** comando viene eseguito, dei criteri di rete viene aggiornato automaticamente con le impostazioni di configurazione aggiornato. Non è necessario arrestare dei criteri di rete nel computer di destinazione per eseguire la **netsh nps importare** comando, tuttavia se la console Criteri di rete o lo snap-in NPS MMC è aperto durante l'importazione di configurazione, le modifiche alla configurazione del server non sono visibili fino a aggiornare la vista. 

> [!NOTE]
> Quando si usa la **netsh nps export** comando, è necessario fornire il parametro del comando **exportPSK** con il valore **Sì**. Questo parametro e il valore in modo esplicito lo stato di aver compreso che si sta esportando la configurazione dei criteri di rete e che contiene il file XML esportato non crittografato i segreti condivisi per i client RADIUS e i membri di gruppi di server RADIUS remoti.

**Credenziali amministrative**

Per completare questa procedura, è necessario essere un membro del gruppo Administrators.

### <a name="to-copy-an-nps-configuration-to-another-nps-using-netsh-commands"></a>Per copiare una configurazione dei criteri di rete in un altro dei criteri di rete usando i comandi Netsh

1. L'origine dei criteri di rete, aprire **prompt dei comandi**, digitare **netsh**, quindi premere INVIO.

2. Nel **netsh** , digitare **nps**, e quindi premere INVIO. 

3. Nel **netsh nps** , digitare **esportare filename =** "*path\file.xml*" **exportPSK = YES**, dove *percorso* è il percorso della cartella in cui si desidera salvare il file di configurazione dei criteri di rete, e *file* è il nome del file XML che si desidera salvare. Premi INVIO. 

Consente di archiviare le impostazioni di configurazione \(incluse le impostazioni del Registro di sistema\) in un file XML. Il percorso può essere assoluto o relativo, o può essere un Universal Naming Convention \(UNC\) percorso. Dopo aver premuto INVIO, viene visualizzato un messaggio che indicano se l'esportazione in file ha avuto esito positivo.

4. Copiare il file che è stato creato per la destinazione dei criteri di rete.

5. Al prompt dei comandi nella destinazione dei criteri di rete, digitare **netsh nps importare filename =** "*path\file.xml*", quindi premere INVIO. Viene visualizzato un messaggio che indicano se l'importazione dal file XML ha avuto esito positivo.

Per altre informazioni su netsh, vedere [Network Shell (Netsh)](../netsh/netsh.md).

