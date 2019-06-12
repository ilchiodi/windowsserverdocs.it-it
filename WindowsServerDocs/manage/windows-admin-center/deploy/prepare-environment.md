---
title: Preparare l'ambiente per Windows Admin Center
description: Preparare l'ambiente per Windows Admin Center (Project Honolulu)
ms.technology: manage
ms.topic: article
author: nwashburn-ms
ms.author: niwashbu
ms.date: 06/07/2019
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: d018ea65ce61cab67fe2041b9ef885d32de51b17
ms.sourcegitcommit: 6ef4986391607bb28593852d06cc6645e548a4b3
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 06/07/2019
ms.locfileid: "66811919"
---
# <a name="prepare-your-environment-for-windows-admin-center"></a>Preparare l'ambiente per Windows Admin Center

> Si applica a: Windows Admin Center, Windows Admin Center anteprima

Alcune versioni Server necessitano di attività di preparazione aggiuntive prima di poter iniziare la gestione con Windows Admin Center:

- [Windows Server 2012 e 2012 R2](#prepare-windows-server-2012-and-2012-r2)
- [Windows Server 2008 R2](#prepare-windows-server-2008-r2)
- [Microsoft Hyper-V Server 2016](#prepare-microsoft-hyper-v-server-2016)
- [Microsoft Hyper-V Server 2012 R2](#prepare-microsoft-hyper-v-server-2012-r2)

## <a name="prepare-windows-server-2012-and-2012-r2"></a>Preparare Windows Server 2012 e 2012 R2

### <a name="install-wmf-version-51-or-higher"></a>Installare WMF 5.1 o versione successiva

Windows Admin Center richiede funzionalità PowerShell non incluse per impostazione predefinita in Windows Server 2012 e 2012 R2. Per gestire Windows Server 2012 o 2012 R2 con Windows Admin Center, devi installare WMF 5.1 o versione successiva in tali server.

Digita `$PSVersiontable` in PowerShell per verificare che WMF 5.1 o versione successiva sia installato.

Se non è installato, puoi [scaricare e installare WMF 5.1](https://docs.microsoft.com/powershell/wmf/5.1/install-configure).

## <a name="prepare-windows-server-2008-r2"></a>Preparare Windows Server 2008 R2

### <a name="install-wmf-version-51-or-higher"></a>Installare WMF 5.1 o versione successiva

Windows Admin Center richiede funzionalità PowerShell non incluse per impostazione predefinita in Windows Server 2008 R2. Per gestire Windows Server 2008 R2 con Windows Admin Center, devi installare WMF 5.1 o versione successiva in tali server. 

Assicurarsi che [.NET Framework 4.5.2 o versione successiva](https://docs.microsoft.com/dotnet/framework/install/on-windows-7) è già installato nel computer.

Digita `$PSVersiontable` in PowerShell per verificare che WMF 5.1 o versione successiva sia installato.

Se non è installato, puoi [scaricare e installare WMF 5.1](https://docs.microsoft.com/powershell/wmf/5.1/install-configure).

Esegui `Enable-PSRemoting –force` in una console di PowerShell per consentire la connessione remota Powershell. 

### <a name="enable-remote-desktop"></a>Attivare Desktop remoto

Per l'utilizzo di Desktop remoto in Windows Admin Center, dovrai attivare Desktop remoto nel server Windows Server 2008 R2.

Da **Server Manager**, vai a **Configurazione Desktop remoto**. In Desktop remoto attiva "Consenti connessioni dai computer che eseguono qualsiasi versione di Desktop remoto".

## <a name="prepare-microsoft-hyper-v-server-2016"></a>Preparare Microsoft Hyper-V Server 2016

Per gestire Microsoft Hyper-V Server 2016 con Windows Admin Center, dovrà prima abilitare alcuni ruoli Server.

### <a name="to-manage-microsoft-hyper-v-server-2016-with-windows-admin-center"></a>Per gestire Microsoft Hyper-V Server 2016 con Windows Admin Center:

1. Abilita la gestione remota.
2. Attiva Ruolo File server.
3. Attiva Hyper-V per PowerShell.

### <a name="step-1-enable-remote-management"></a>**Passaggio 1:** Abilita la gestione remota

Per abilitare la gestione remota in Hyper-V Server:

1. Esegui l'accesso a Hyper-V Server.
2. Nello strumento **Configurazione server** (SCONFIG), digita **4** per configurare la gestione remota.
3. Digita **1** per abilitare la gestione remota.
4. Digita **4** per tornare al menu principale.

### <a name="step-2-enable-file-server-role"></a>**Passaggio 2:** Attiva Ruolo File server

Per attivare Ruolo File server per la gestione remota e la condivisione file di base:

1. Fai clic su **Ruoli e funzionalità** dal menu **Strumenti** .
2. In **Ruoli e funzionalità**, trova **Servizi file e archiviazione**e seleziona **Servizi file e iSCSI** e **File Server**:

![Schermata di ruoli e funzionalità che mostra il File e iSCSI ruolo di servizi selezionato](../media/prepare-environment/c6c30b812d96afcc1edcdb6f52f0e13c.png)

### <a name="step-3-enable-hyper-v-module-for-powershell"></a>**Passaggio 3:** Abilitare Modulo Hyper-V per PowerShell

Per abilitare le funzionalità Modulo Hyper-V per Windows PowerShell:

1. Fai clic su **Ruoli e funzionalità** dal menu **Strumenti** .
2. In **Ruoli e funzionalità**, trova **Strumenti di amministrazione remota del Server** e seleziona **Strumenti di amministrazione ruoli** e **Modulo Hyper-V per PowerShell** :

![Schermata di ruoli e funzionalità che mostra i ruoli Hyper-V selezionati](../media/prepare-environment/7ab0999602b7083733525bd0c1ba2747.png)

Microsoft Hyper-V Server 2016 è ora pronto per la gestione con Windows Admin Center.

## <a name="prepare-microsoft-hyper-v-server-2012-r2"></a>Preparare Microsoft Hyper-V Server 2012 R2

Per gestire Microsoft Hyper-V Server 2012 R2 con Windows Admin Center, esistono alcuni ruoli Server che dovrai abilitare prima di poter procedere.  Inoltre, dovrai installare WMF 5.1 o versione successiva.

### <a name="to-manage-microsoft-hyper-v-server-2012-r2-with-windows-admin-center"></a>Per gestire Microsoft Hyper-V Server 2012 R2 con Windows Admin Center:

1. Installa Windows Management Framework (WMF) 5.1 o versione successiva
2. Abilita la gestione remota
3. Attiva Ruolo File server
4. Abilitare Modulo Hyper-V per PowerShell

### <a name="step-1-install-windows-management-framework-51"></a>Passaggio 1: Installare Windows Management Framework 5.1

Windows Admin Center richiede funzionalità PowerShell non incluse per impostazione predefinita in Microsoft Hyper-V Server 2012 R2. Per gestire Microsoft Hyper-V Server 2012 R2 con Windows Admin Center, devi installare WMF 5.1 o versione successiva.

Digita `$PSVersiontable` in PowerShell per verificare che WMF 5.1 o versione successiva sia installato. 

Se non è installato, puoi eseguire il [download di WMF 5.1](https://docs.microsoft.com/powershell/wmf/5.1/install-configure).

### <a name="step-2-enable-remote-management"></a>Passaggio 2: Abilita la gestione remota

Per abilitare la gestione remota di Hyper-V Server:

1. Esegui l'accesso a Hyper-V Server.
2. Nello strumento **Configurazione server** (SCONFIG), digita **4** per configurare la gestione remota.
3. Digita **1** per abilitare la gestione remota.
4. Digita **4** per tornare al menu principale.

### <a name="step-3-enable-file-server-role"></a>Passaggio 3: Attiva Ruolo File server

Per attivare Ruolo File server per la gestione remota e la condivisione file di base:

1. Fai clic su **Ruoli e funzionalità** dal menu **Strumenti** .
2. In **Ruoli e funzionalità**, trova **Servizi file e archiviazione** e seleziona **Servizi file e iSCSI** e **File Server**:

![Schermata di ruoli e funzionalità che mostra il File e iSCSI ruolo di servizi selezionato](../media/prepare-environment/c6c30b812d96afcc1edcdb6f52f0e13c.png)

### <a name="step-4-enable-hyper-v-module-for-powershell"></a>Passaggio 4: Abilitare Modulo Hyper-V per PowerShell

Per abilitare le funzionalità Modulo Hyper-V per Windows PowerShell:

1. Fai clic su **Ruoli e funzionalità** dal menu **Strumenti** .
2. In **Ruoli e funzionalità**, trova **Strumenti di amministrazione remota del Server** e seleziona **Strumenti di amministrazione ruoli** e **Modulo Hyper-V per PowerShell** :

![Schermata di ruoli e funzionalità che Mostra strumenti di amministrazione remota del server Hyper-V selezionati](../media/prepare-environment/7ab0999602b7083733525bd0c1ba2747.png)

Microsoft Hyper-V Server 2012 R2 è ora pronto per la gestione con Windows Admin Center.

> [!Tip]
> Pronto per installare Windows Admin Center? [Scarica subito](https://docs.microsoft.com/windows-server/manage/windows-admin-center/understand/windows-admin-center#download-now)