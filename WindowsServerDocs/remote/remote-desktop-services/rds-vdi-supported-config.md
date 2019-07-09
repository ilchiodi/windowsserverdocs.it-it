---
title: Configurazioni di sicurezza di Windows 10 supportate per l'infrastruttura VDI di Servizi Desktop remoto
description: Fornisce informazioni sulle configurazioni supportate per l'infrastruttura VDI di Windows 10 con Servizi Desktop remoto in Windows Server 2016.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.author: elizapo
ms.date: 10/27/2016
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 8f164f5d-a498-4f91-a12f-3e01d554f810
author: lizap
manager: dongill
ms.openlocfilehash: ff890150dcea30c425267dcaae9b1bdbc6d78b8c
ms.sourcegitcommit: 3743cf691a984e1d140a04d50924a3a0a19c3e5c
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 06/17/2019
ms.locfileid: "63743393"
---
# <a name="supported-windows-10-security-configurations-for-remote-desktop-services-vdi"></a>Configurazioni di sicurezza di Windows 10 supportate per l'infrastruttura VDI di Servizi Desktop remoto

> Si applica a: Windows Server 2016

Windows 10 e Windows Server 2016 dispongono di nuovi livelli di protezione integrati nel sistema operativo per salvaguardare ulteriormente da violazioni della sicurezza, consentire il blocco degli attacchi dannosi e migliorare la sicurezza di macchine virtuali, applicazioni e dati.

> [!NOTE]
> Fai riferimento alle [informazioni sulle configurazioni supportate per Servizi Desktop remoto](rds-supported-config.md).

La tabella seguente indica quali di queste nuove funzionalità sono supportate in una distribuzione VDI con Servizi Desktop remoto.

|  Tipo di raccolta VDI               |  Gestione in pool |  Gestione personale |  Nessuna gestione in pool                                     |  Nessuna gestione personale                                    |
|-------------------------------------|------------------|--------------------|--------------------------------------------------------|--------------------------------------------------------|
| [Credential Guard](https://technet.microsoft.com/itpro/windows/keep-secure/credential-guard)                    | Sì              | Sì                | Sì                                                    | Sì                                                    |
| [Device Guard](https://technet.microsoft.com/itpro/windows/keep-secure/device-guard-deployment-guide)                        | Sì              | Sì                | Sì                                                    | Sì                                                    |
| [Remote Credential Guard](https://technet.microsoft.com/itpro/windows/keep-secure/remote-credential-guard)             | No               | No                 | No                                                     | No                                                     |
| [Macchine virtuali schermate e con supporto della crittografia](../../security/guarded-fabric-shielded-vm/guarded-fabric-and-shielded-vms.md) | No               | No                 | Macchine virtuali con supporto della crittografia con configurazione aggiuntiva | Macchine virtuali con supporto della crittografia con configurazione aggiuntiva |

## <a name="remote-credential-guard"></a>Remote Credential Guard:

La funzionalità Remote Credential Guard è supportata solo per connessioni dirette ai computer di destinazione e non per le connessioni tramite Gestore connessione Desktop remoto e Gateway Desktop remoto.
> [!NOTE]
> Se disponi di un gestore di connessione in un ambiente a istanza singola e il nome DNS corrisponde al nome del computer, potresti essere in grado di usare la funzionalità Remote Credential Guard anche se non è supportata.

## <a name="shielded-vms-and-encryption-supported-vms"></a>Macchine virtuali schermate e con supporto della crittografia: 

- Le macchine virtuali schermate non sono supportate nell'infrastruttura VDI di Servizi Desktop remoto 

Per usare le macchine virtuali con supporto della crittografia:
- Usa una raccolta non gestita e una tecnologia di provisioning all'esterno del processo di creazione della raccolta di Servizi Desktop remoto per effettuare il provisioning delle macchine virtuali. 
- I dischi dei profili utente non sono supportati perché si basano su dischi differenziali 

