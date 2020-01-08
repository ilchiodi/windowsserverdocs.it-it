---
title: Connessione TCP interrotta durante la negoziazione della convalida
description: Viene illustrato come risolvere il problema SMB quando la connessione TCP viene interrotta durante la negoziazione di convalida.
author: Deland-Han
manager: dcscontentpm
audience: ITPro
ms.topic: article
ms.author: delhan
ms.date: 12/25/2019
ms.openlocfilehash: 3455b4ac0a2706f80702378dda02c1877af219ca
ms.sourcegitcommit: 8cf04db0bc44fd98f4321dca334e38c6573fae6c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/03/2020
ms.locfileid: "75654622"
---
# <a name="tcp-connection-is-aborted-during-validate-negotiate"></a>Connessione TCP interrotta durante la negoziazione della convalida

Nella traccia di rete per il problema SMB si nota che si è verificata un'interruzione della reimpostazione TCP durante il processo di convalida della negoziazione. Questo articolo descrive come risolvere il problema.

## <a name="cause"></a>Causa

Questo problema può essere causato da una convalida della negoziazione non riuscita. Questo problema si verifica in genere perché un acceleratore WAN modifica il pacchetto di negoziazione SMB originale.

Microsoft non consente più di apportare modifiche al pacchetto Validate Negotiate per qualsiasi motivo. Questo è dovuto al fatto che questo comportamento crea un grave rischio per la sicurezza.

I requisiti seguenti si applicano al pacchetto Validate Negotiate:

- Il processo Validate Negotiate usa il comando FSCTL\_VALIDATE\_NEGOTIATe\_INFO.

- La risposta Validate Negotiate deve essere firmata. In caso contrario, la connessione viene interrotta.

- È necessario confrontare il FSCTL\_convalidare\_NEGOZIare\_informazioni messaggi ai messaggi Negotiate per assicurarsi che non sia stato modificato nulla.

## <a name="workaround"></a>Soluzione alternativa

È possibile disabilitare temporaneamente il processo Validate Negotiate. A tale scopo, individuare la seguente sottochiave del registro di sistema:

**HKEY\_LOCAL\_MACHINE\\SYSTEM\\CurrentControlSet\\Services\\LanmanWorkstation\\parametri**

Nella chiave **Parameters** impostare **RequireSecureNegotiate** su **0**.

In Windows PowerShell è possibile eseguire il comando seguente per impostare questo valore:

```PowerShell
Set-ItemProperty -Path "HKLM:\SYSTEM\CurrentControlSet\Services\LanmanWorkstation\Parameters" RequireSecureNegotiate -Value 0 -Force
```

> [!NOTE]
> Il processo Validate Negotiate non può essere disabilitato in Windows 10, Windows Server 2016 o versioni successive di Windows.

Se il client o il server non è in grado di supportare il comando Validate Negotiate, è possibile aggirare questo problema impostando la firma SMB come obbligatoria. La firma SMB è considerata più sicura rispetto alla negoziazione di convalida. Tuttavia, se è necessaria la firma, è possibile che si verifichi un calo delle prestazioni.

## <a name="reference"></a>Informazioni di riferimento

Per informazioni, vedi gli articoli seguenti:

[3.3.5.15.12 gestione di una richiesta di informazioni di negoziazione di convalida](https://docs.microsoft.com/openspecs/windows_protocols/ms-smb2/0b7803eb-d561-48a4-8654-327803f59ec6)

[3.2.5.14.12 gestione di una risposta di convalida delle informazioni di negoziazione](https://docs.microsoft.com/openspecs/windows_protocols/ms-smb2/6a5bc90d-3c08-4498-905b-e7dab30b2e0e)
