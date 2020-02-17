---
title: Preparare l'ambiente per Windows Admin Center
description: Preparare l'ambiente per Windows Admin Center (Project Honolulu)
ms.technology: manage
ms.topic: article
author: nwashburn-ms
ms.author: niwashbu
ms.date: 06/07/2019
ms.localizationpriority: medium
ms.prod: windows-server
ms.openlocfilehash: 72e71ce2d1427f392aa02d32597f92d031f9a5c9
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71407000"
---
# <a name="prepare-your-environment-for-windows-admin-center"></a>Preparare l'ambiente per Windows Admin Center

> Si applica a: Windows Admin Center, Windows Admin Center Preview

Prima di iniziare la gestione con Windows Admin Center, alcune versioni di Windows Server necessitano di attività di preparazione aggiuntive:

- [Windows Server 2012 e 2012 R2](#prepare-windows-server-2012-and-2012-r2)
- [Windows Server 2008 R2](#prepare-windows-server-2008-r2)
- [Microsoft Hyper-V Server 2016](#prepare-microsoft-hyper-v-server-2016)
- [Microsoft Hyper-V Server 2012 R2](#prepare-microsoft-hyper-v-server-2012-r2)

Prima di eseguire attività di gestione con Windows Admin Center, in alcuni scenari può essere necessario modificare la [configurazione della porta nel server di destinazione](#port-configuration-on-the-target-server).

## <a name="prepare-windows-server-2012-and-2012-r2"></a>Preparare Windows Server 2012 e 2012 R2

### <a name="install-wmf-version-51-or-higher"></a>Installare WMF 5.1 o versione successiva

Windows Admin Center richiede funzionalità PowerShell non incluse per impostazione predefinita in Windows Server 2012 e 2012 R2. Per gestire Windows Server 2012 o 2012 R2 con Windows Admin Center, devi installare WMF 5.1 o versione successiva in questi server.

Per verificare che WMF 5.1 o versione successiva sia installato, digita `$PSVersiontable` in PowerShell.

In caso contrario, puoi [scaricare e installare WMF 5.1](https://docs.microsoft.com/powershell/wmf/setup/install-configure).

## <a name="prepare-windows-server-2008-r2"></a>Preparare Windows Server 2008 R2

### <a name="install-wmf-version-51-or-higher"></a>Installare WMF 5.1 o versione successiva

Windows Admin Center richiede funzionalità PowerShell non incluse per impostazione predefinita in Windows Server 2008 R2. Per gestire Windows Server 2008 R2 con Windows Admin Center, devi installare WMF 5.1 o versione successiva in questi server. 

Verifica che nel computer sia già installato [.NET Framework 4.5.2 o versione successiva](https://docs.microsoft.com/dotnet/framework/install/on-windows-7).

Per verificare che WMF 5.1 o versione successiva sia installato, digita `$PSVersiontable` in PowerShell.

In caso contrario, puoi [scaricare e installare WMF 5.1](https://docs.microsoft.com/powershell/wmf/setup/install-configure).

Per consentire la connessione remota di PowerShell, esegui `Enable-PSRemoting –force` in una console di PowerShell. 

### <a name="enable-remote-desktop"></a>Abilitare Desktop remoto

Per l'uso di Desktop remoto in Windows Admin Center, dovrai abilitare Desktop remoto nel server Windows Server 2008 R2.

Da **Server Manager** passa a **Configurazione Desktop remoto**. In Desktop remoto abilita "Consenti connessioni dai computer che eseguono qualsiasi versione di Desktop remoto".

## <a name="prepare-microsoft-hyper-v-server-2016"></a>Preparare Microsoft Hyper-V Server 2016

Per gestire Microsoft Hyper-V Server 2016 con Windows Admin Center, dovrai prima abilitare alcuni ruoli Server.

### <a name="to-manage-microsoft-hyper-v-server-2016-with-windows-admin-center"></a>Per gestire Microsoft Hyper-V Server 2016 con Windows Admin Center:

1. Abilita la gestione remota.
2. Abilita il ruolo File server.
3. Abilita il modulo Hyper-V per PowerShell.

### <a name="step-1-enable-remote-management"></a>**Passaggio 1:** Abilitare la gestione remota

Per abilitare la gestione remota in Hyper-V Server:

1. Accedi a Hyper-V Server.
2. Nello strumento **Configurazione server** (SCONFIG) digita **4** per configurare la gestione remota.
3. Digita **1** per abilitare la gestione remota.
4. Digita **4** per tornare al menu principale.

### <a name="step-2-enable-file-server-role"></a>**Passaggio 2**: Abilitare il ruolo File server

Per abilitare il ruolo File server per la gestione remota e la condivisione file di base:

1. Fai clic su **Ruoli e funzionalità** nel menu **Strumenti**.
2. In **Ruoli e funzionalità** trova **Servizi file e archiviazione** e seleziona **Servizi file e iSCSI** e **File Server**:

![Screenshot di Ruoli e funzionalità con il ruolo Servizi file e iSCSI selezionato](../media/prepare-environment/c6c30b812d96afcc1edcdb6f52f0e13c.png)

### <a name="step-3-enable-hyper-v-module-for-powershell"></a>**Passaggio 3:** Abilitare il modulo Hyper-V per PowerShell

Per abilitare le funzionalità Modulo Hyper-V per Windows PowerShell:

1. Fai clic su **Ruoli e funzionalità** nel menu **Strumenti**.
2. In **Ruoli e funzionalità** trova **Strumenti di amministrazione remota del Server** e seleziona **Strumenti di amministrazione ruoli** e **Modulo Hyper-V per Windows PowerShell**:

![Screenshot di Ruoli e funzionalità con i ruoli Hyper-V selezionati](../media/prepare-environment/7ab0999602b7083733525bd0c1ba2747.png)

Microsoft Hyper-V Server 2016 è ora pronto per la gestione con Windows Admin Center.

## <a name="prepare-microsoft-hyper-v-server-2012-r2"></a>Preparare Microsoft Hyper-V Server 2012 R2

Prima di procedere alla gestione di Microsoft Hyper-V Server 2012 R2 con Windows Admin Center, devi abilitare alcuni ruoli Server.  Inoltre, devi installare WMF 5.1 o versione successiva.

### <a name="to-manage-microsoft-hyper-v-server-2012-r2-with-windows-admin-center"></a>Per gestire Microsoft Hyper-V Server 2012 R2 con Windows Admin Center:

1. Installa Windows Management Framework (WMF) 5.1 o versione successiva
2. Abilita la gestione remota
3. Abilita il ruolo File server
4. Abilita il modulo Hyper-V per PowerShell

### <a name="step-1-install-windows-management-framework-51"></a>Passaggio 1: Installare Windows Management Framework 5.1

Windows Admin Center richiede funzionalità PowerShell non incluse per impostazione predefinita in Microsoft Hyper-V Server 2012 R2. Per gestire Microsoft Hyper-V Server 2012 R2 con Windows Admin Center, devi installare WMF 5.1 o versione successiva.

Per verificare che WMF 5.1 o versione successiva sia installato, digita `$PSVersiontable` in PowerShell. 

In caso contrario, puoi [scaricare WMF 5.1](https://docs.microsoft.com/powershell/wmf/setup/install-configure).

### <a name="step-2-enable-remote-management"></a>Passaggio 2: Abilitare la gestione remota

Per abilitare la gestione remota di Hyper-V Server:

1. Accedi a Hyper-V Server.
2. Nello strumento **Configurazione server** (SCONFIG) digita **4** per configurare la gestione remota.
3. Digita **1** per abilitare la gestione remota.
4. Digita **4** per tornare al menu principale.

### <a name="step-3-enable-file-server-role"></a>Passaggio 3: Abilitare il ruolo File server

Per abilitare il ruolo File server per la gestione remota e la condivisione file di base:

1. Fai clic su **Ruoli e funzionalità** nel menu **Strumenti**.
2. In **Ruoli e funzionalità** trova **Servizi file e archiviazione** e seleziona **Servizi file e iSCSI** e **File Server**:

![Screenshot di Ruoli e funzionalità con il ruolo Servizi file e iSCSI selezionato](../media/prepare-environment/c6c30b812d96afcc1edcdb6f52f0e13c.png)

### <a name="step-4-enable-hyper-v-module-for-powershell"></a>Passaggio 4: Abilitare il modulo Hyper-V per PowerShell

Per abilitare le funzionalità Modulo Hyper-V per Windows PowerShell:

1. Fai clic su **Ruoli e funzionalità** nel menu **Strumenti**.
2. In **Ruoli e funzionalità** trova **Strumenti di amministrazione remota del Server** e seleziona **Strumenti di amministrazione ruoli** e **Modulo Hyper-V per Windows PowerShell**:

![Screenshot di Ruoli e funzionalità con gli strumenti di amministrazione remota del server Hyper-V selezionati](../media/prepare-environment/7ab0999602b7083733525bd0c1ba2747.png)

Microsoft Hyper-V Server 2012 R2 è ora pronto per la gestione con Windows Admin Center.

## <a name="port-configuration-on-the-target-server"></a>Configurazione delle porte nel server di destinazione

Windows Admin Center usa il protocollo di condivisione file SMB per alcune attività di copia dei file, ad esempio quando si importa un certificato in un server remoto. Affinché le operazioni di copia dei file abbiano esito positivo, il firewall nel server remoto deve consentire le connessioni in ingresso sulla porta 445.  Per verificare che la regola in ingresso per "Gestione remota file server (SMB-in)" sia impostata in modo da consentire l'accesso a questa porta, puoi usare lo strumento firewall in Windows Admin Center.

> [!Tip]
> Pronto per installare Windows Admin Center? [Scarica subito](https://docs.microsoft.com/windows-server/manage/windows-admin-center/understand/windows-admin-center#download-now)
