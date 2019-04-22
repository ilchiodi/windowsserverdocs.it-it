---
title: Configurazioni di sicurezza di Windows 10 supportate per l'infrastruttura VDI di Servizi Desktop remoto
description: Vengono fornite informazioni sulle configurazioni supportate per Windows 10 VDI con Servizi Desktop remoto in Windows Server 2016.
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
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59820082"
---
# <a name="supported-windows-10-security-configurations-for-remote-desktop-services-vdi"></a>Configurazioni di sicurezza di Windows 10 supportate per l'infrastruttura VDI di Servizi Desktop remoto

> Si applica a: Windows Server 2016

Windows 10 e Windows Server 2016 hanno nuovi livelli di protezione incorporato nel sistema operativo per un'ulteriore protezione contro le violazioni della sicurezza, bloccare gli attacchi dannosi e migliorare la sicurezza delle macchine virtuali, applicazioni e dati.

> [!NOTE]
> Assicurarsi di consultare il [Servizi Desktop remoto supportate le informazioni di configurazione](rds-supported-config.md).

La seguente tabella indica quali di queste nuove funzionalità è supportata in una distribuzione VDI con Servizi Desktop remoto.

|  Tipo di raccolta di VDI               |  Gestito in pool |  Managed personali |  Non è gestito in pool                                     |  Personale non gestita                                    |
|-------------------------------------|------------------|--------------------|--------------------------------------------------------|--------------------------------------------------------|
| [Credential Guard](https://technet.microsoft.com/itpro/windows/keep-secure/credential-guard)                    | Yes              | Yes                | Yes                                                    | Yes                                                    |
| [Device Guard](https://technet.microsoft.com/itpro/windows/keep-secure/device-guard-deployment-guide)                        | Yes              | Yes                | Yes                                                    | Yes                                                    |
| [Credential Guard remoto](https://technet.microsoft.com/itpro/windows/keep-secure/remote-credential-guard)             | No               | No                 | No                                                     | No                                                     |
| [Schermate e le VM supportate da crittografia](../../security/guarded-fabric-shielded-vm/guarded-fabric-and-shielded-vms.md) | No               | No                 | Le VM supportate da crittografia senza configurazione aggiuntiva | Le VM supportate da crittografia senza configurazione aggiuntiva |

## <a name="remote-credential-guard"></a>Credential Guard remoto:

Credential Guard remoto è supportato solo per connessioni dirette ai computer di destinazione e non per quelli con Gestore connessione Desktop remoto e Gateway Desktop remoto.
> [!NOTE]
> Se si dispone di un gestore di connessione in un ambiente a istanza singola e il nome DNS corrisponde al nome del computer, è possibile usare Credential Guard remoto, anche se ciò non è supportato.

## <a name="shielded-vms-and-encryption-supported-vms"></a>Le VM supportate da crittografia e le macchine virtuali schermate: 

- Le macchine virtuali schermate non sono supportate in VDI di Servizi Desktop remoto 

Per sfruttare le macchine virtuali supportate da crittografia:
- Usare una raccolta non gestita e una tecnologia di provisioning all'esterno del processo di creazione della raccolta di Servizi Desktop remoto per effettuare il provisioning di macchine virtuali. 
- I dischi dei profili utente non sono supportati perché si basano su dischi differenziali 

