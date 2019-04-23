---
title: Risolvere i problemi di macchine virtuali schermate
ms.custom: na
ms.prod: windows-server-threshold
ms.topic: article
manager: dongill
author: rpsqrd
ms.technology: security-guarded-fabric
ms.date: 10/3/2018
ms.openlocfilehash: 13ff0dad1519d394ce74a91efbfcc9e2f237e4a5
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59850032"
---
# <a name="troubleshoot-shielded-vms"></a>Risolvere i problemi di macchine virtuali schermate

>Si applica a: Windows Server 2019, Windows Server (canale semestrale), Windows Server 2016

A partire da Windows Server versione 1803, la modalità sessione avanzata di Virtual Machine Connection (VMConnect) e Powershell Direct vengono riabilitati per le macchine virtuali schermate completamente. L'amministratore di virtualizzazione richiede comunque credenziali guest VM per ottenere l'accesso alla macchina virtuale, ma questo rende più semplice per un host risolvere i problemi di una macchina virtuale schermata durante la configurazione di rete viene interrotta.

Per abilitare VMConnect e PS diretta per le macchine virtuali schermate, è sufficiente spostarli in un host Hyper-V che esegue Windows Server versione 1803 o successiva. I dispositivi virtuali consentendo per queste funzionalità verranno riabilitati automaticamente. Se una macchina virtuale schermata viene spostata in un host che esegue e una versione precedente di Windows Server, VMConnect e PS diretta verrà disabilitato anche in questo caso.

Per i clienti sensibili alla sicurezza che è necessario sapere hoster dispone dell'accesso alla macchina virtuale e si vuole ripristinare il comportamento originale, è necessario disabilitare le funzionalità seguenti del sistema operativo guest:

- Disabilitare il servizio di PowerShell Direct nella macchina virtuale:

  ```powershell
  Stop-Service vmicvmsession
  Set-Service vmicvmsession -StartupType Disabled
  ```

- Modalità sessione avanzata VMConnect può essere disabilitata solo se il sistema operativo guest è almeno Windows Server 2019 o Windows 10, versione 1809. Aggiungere la seguente chiave del Registro di sistema nella macchina virtuale per disabilitare le connessioni VMConnect migliorato sessione della console:

  ```
  reg add "HKLM\Software\Microsoft\Virtual Machine\Guest" /v DisableEnhancedSessionConsoleConnection /t REG_DWORD /d 1
  ```
