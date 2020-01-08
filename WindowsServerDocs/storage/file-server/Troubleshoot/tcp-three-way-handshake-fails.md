---
title: Errore di handshake TCP a tre vie durante la connessione SMB
description: Introduce l'errore di handshake TCP a tre vie durante la connessione SMB.
author: Deland-Han
manager: dcscontentpm
audience: ITPro
ms.topic: article
ms.author: delhan
ms.date: 12/25/2019
ms.openlocfilehash: 8cef47e164b8768747cb383f4d7012130c7cb516
ms.sourcegitcommit: 8cf04db0bc44fd98f4321dca334e38c6573fae6c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/03/2020
ms.locfileid: "75654632"
---
# <a name="tcp-three-way-handshake-failure-during-smb-connection"></a>Errore di handshake TCP a tre vie durante la connessione SMB

Quando si analizza una traccia di rete, si nota che si verifica un errore di handshake a tre vie Transmission Control Protocol (TCP) che causa il problema SMB. Questo articolo descrive come risolvere questa situazione.

## <a name="troubleshooting"></a>Risoluzione dei problemi

In genere, la ragione è un firewall locale o di infrastruttura che blocca il traffico. Questo problema può verificarsi in uno degli scenari seguenti.

## <a name="scenario-1"></a>Scenario 1

Il pacchetto TCP SYN arriva sul server SMB, ma il server SMB non restituisce un pacchetto TCP SYN-ACK.

Per risolvere questo scenario, seguire questa procedura.

### <a name="step-1"></a>Passaggio 1

Eseguire **netstat** o **Get-NetTcpConnection** per verificare che sia presente un listener sulla porta TCP 445 che deve essere di proprietà del processo di sistema.

```cmd
netstat -ano | findstr :445
```

```PowerShell
Get-NetTcpConnection -LocalPort 445
```

### <a name="step-2"></a>Passaggio 2

Verificare che il servizio Server sia avviato e in esecuzione.

### <a name="step-3"></a>Passaggio 3

Eseguire un'acquisizione di Windows Filtering Platform (WFP) per determinare quale regola o programma sta eliminando il traffico. A tale scopo, eseguire il comando seguente in una finestra del prompt dei comandi:

```cmd
netsh wfp capture start
```

Riprodurre il problema, quindi eseguire il comando seguente:

```cmd
netsh wfp capture stop
```

Eseguire una traccia dello scenario e cercare le gocce del WFP nel traffico SMB (sulla porta TCP 445).

Facoltativamente, è possibile rimuovere i programmi antivirus perché non sono sempre basati su PAM.

### <a name="step-4"></a>Passaggio 4

Se Windows Firewall è abilitata, abilitare la registrazione del firewall per determinare se registra un calo del traffico.

Verificare che le regole "condivisione file e stampanti (SMB-in)" appropriate siano abilitate in **Windows Firewall con sicurezza avanzata** \> **regole in ingresso**.

> [!NOTE]
> A seconda della configurazione del computer, "Windows Firewall" potrebbe essere chiamato "Windows Defender Firewall".

## <a name="scenario-2"></a>Scenario 2

Il pacchetto SYN TCP non arriva mai al server SMB.

In questo scenario, è necessario esaminare i dispositivi lungo il percorso di rete. È possibile analizzare le tracce di rete acquisite in ogni dispositivo per determinare il dispositivo che blocca il traffico.
