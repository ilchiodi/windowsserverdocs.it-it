---
title: Risolvere i problemi relativi alle macchine virtuali schermate
ms.prod: windows-server
ms.topic: article
manager: dongill
author: rpsqrd
ms.author: ryanpu
ms.technology: security-guarded-fabric
ms.date: 10/3/2018
ms.openlocfilehash: c80663256a2e3404666b739c0a81cd06ec3caced
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80856374"
---
# <a name="troubleshoot-shielded-vms"></a>Risolvere i problemi relativi alle macchine virtuali schermate

>Si applica a: Windows Server 2019, Windows Server (canale semestrale), Windows Server 2016

A partire dalla versione 1803 di Windows Server, la modalità sessione avanzata Virtual Machine Connection (VMConnect) e PS Direct vengono riabilitate per le macchine virtuali completamente schermate. L'amministratore della virtualizzazione richiede ancora le credenziali Guest della macchina virtuale per ottenere l'accesso alla macchina virtuale, ma ciò rende più semplice per un provider di hosting risolvere i problemi di una VM schermata quando la relativa configurazione di rete è interruppe.

Per abilitare VMConnect e PS Direct per le macchine virtuali schermate, è sufficiente spostarle in un host Hyper-V che esegue Windows Server versione 1803 o successiva. I dispositivi virtuali che consentono di eseguire queste funzionalità verranno riabilitati automaticamente. Se una macchina virtuale schermata viene spostata in un host che esegue e una versione precedente di Windows Server, VMConnect e PS Direct verranno nuovamente disabilitati.

Per i clienti sensibili alla sicurezza che si preoccupano se i provider di servizi di hosting hanno accesso alla macchina virtuale e vogliono tornare al comportamento originale, è necessario disabilitare le funzionalità seguenti nel sistema operativo guest:

- Disabilitare il servizio PowerShell Direct nella macchina virtuale:

  ```powershell
  Stop-Service vmicvmsession
  Set-Service vmicvmsession -StartupType Disabled
  ```

- La modalità sessione avanzata VMConnect può essere disabilitata solo se il sistema operativo guest è almeno Windows Server 2019 o Windows 10, versione 1809. Aggiungere la chiave del registro di sistema seguente nella macchina virtuale per disabilitare le connessioni alla console di sessione avanzata di VMConnect:

  ```
  reg add "HKLM\Software\Microsoft\Virtual Machine\Guest" /v DisableEnhancedSessionConsoleConnection /t REG_DWORD /d 1
  ```
