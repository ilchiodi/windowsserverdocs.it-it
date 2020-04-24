---
title: Impostazioni di sicurezza di generazione 2 macchina virtuale per Hyper-V
description: Descrive le impostazioni di sicurezza disponibili nella console di gestione di Hyper-V per macchine virtuali di seconda generazione
ms.prod: windows-server
manager: dongill
ms.technology: compute-hyper-v
ms.topic: article
ms.assetid: 06ab4f5f-6b8e-4058-8108-76785aa93d4c
author: larsiwer
ms.author: kathydav
ms.date: 10/04/2016
ms.openlocfilehash: 7eb867529d38ab21ee21c19f92c89ed4128b0ea4
ms.sourcegitcommit: 3a3d62f938322849f81ee9ec01186b3e7ab90fe0
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 04/23/2020
ms.locfileid: "80860804"
---
# <a name="generation-2-virtual-machine-security-settings-for-hyper-v"></a>Impostazioni di sicurezza di generazione 2 macchina virtuale per Hyper-V

>Si applica a: Windows Server 2016, Microsoft Hyper-V Server 2016, Windows Server 2019, Microsoft Hyper-V Server 2019

Utilizzare le impostazioni di protezione della macchina virtuale in Hyper-V Manager per proteggere i dati e lo stato di una macchina virtuale. È possibile proteggere le macchine virtuali da ispezione, furti e manomissione da entrambi malware che può essere eseguito nell'host e amministratori del centro dati. Il livello di sicurezza che si ottiene dipende dall'hardware dell'host che viene eseguito, la generazione della macchina virtuale, e se si configura il servizio, denominato servizio sorveglianza Host, che autorizza l'host per avviare le macchine virtuali schermate.  

Il servizio sorveglianza Host è un nuovo ruolo in Windows Server 2016. Identifica gli host Hyper-V legittimi e consente loro di eseguire una determinata macchina virtuale. È in genere necessario configurare il servizio sorveglianza Host per un Data Center. Ma è possibile creare una macchina virtuale schermata per eseguirlo in locale senza configurare il servizio sorveglianza Host. In un secondo momento, è possibile distribuire la macchina virtuale schermata a un'infrastruttura di sorveglianza Host.  

Se è stato ancora configurato il servizio sorveglianza Host o in esecuzione in modalità locale sull'host Hyper-V e l'host ha una chiave di sorveglianza del proprietario della macchina virtuale, è possibile modificare le impostazioni descritte in questo argomento.   Un proprietario di una chiave di sorveglianza è un'organizzazione che crea e condivisioni una chiave pubblica o privata per gestire tutte le macchine virtuali creata con tale chiave.  

Per informazioni su come aumentare la più sicura con il servizio sorveglianza Host delle macchine virtuali, vedere le risorse seguenti.  

- [Rafforzare la protezione dell'infrastruttura: protezione dei segreti Tenant in Hyper-V (video su Ignite)](https://go.microsoft.com/fwlink/?LinkId=746379)
- [Infrastruttura sorvegliata e macchine virtuali schermate](https://go.microsoft.com/fwlink/?LinkId=746381)

## <a name="secure-boot-setting-in-hyper-v-manager"></a>Proteggere l'impostazione dell'avvio di gestione di Hyper-V  

Avvio protetto è una funzionalità disponibile in macchine virtuali di generazione 2 che consente di evitare che utenti non autorizzato firmware, sistemi operativi o driver di Unified Extensible Firmware Interface (UEFI) (noto anche come opzione ROM) in esecuzione al momento dell'avvio. Avvio protetto è abilitato per impostazione predefinita. È possibile utilizzare avvio protetto con macchine virtuali di generazione 2 che eseguono sistemi operativi di distribuzione Windows o Linux.  

I modelli descritti nella tabella riportata di seguito fanno riferimento ai certificati che è necessario verificare l'integrità del processo di avvio.  

|Nome del modello|Descrizione|  
|-----------------|---------------|  
|Microsoft Windows|Selezionare questa opzione per avvio protetto la macchina virtuale per un sistema operativo Windows.|  
|Autorità di certificazione Microsoft UEFI|Selezionare questa opzione per attivare l'avvio protetto della macchina virtuale per un sistema operativo di distribuzione Linux.|  
|Macchina virtuale schermata open source|Questo modello viene usato per attivare l'avvio protetto per [macchine virtuali schermate basate su Linux](https://docs.microsoft.com/windows-server/security/guarded-fabric-shielded-vm/guarded-fabric-create-a-linux-shielded-vm-template).|

Per ulteriori informazioni, vedere gli argomenti seguenti.  

- [Panoramica della sicurezza di Windows 10](https://docs.microsoft.com/windows/security/threat-protection/overview-of-threat-mitigations-in-windows-10)  
- [È necessario creare una macchina virtuale di generazione 1 o 2 in Hyper-V?](../plan/Should-I-create-a-generation-1-or-2-virtual-machine-in-Hyper-V.md)  
- [Macchine virtuali Linux e FreeBSD in Hyper-V](../Supported-Linux-and-FreeBSD-virtual-machines-for-Hyper-V-on-Windows.md)  

## <a name="encryption-support-settings-in-hyper-v-manager"></a>Impostazioni di supporto di crittografia nella gestione di Hyper-V

È possibile proteggere i dati e lo stato della macchina virtuale selezionando le seguenti opzioni di supporto di crittografia.  

- **Abilitare Trusted Platform Module** -questa impostazione rende disponibili per la macchina virtuale un chip Trusted Platform Module (TPM) virtualizzate. In questo modo il guest crittografare il disco di macchina virtuale utilizzando BitLocker.
  - Se l'host Hyper-V è in esecuzione Windows 1511 10, è necessario abilitare la modalità utente isolato. 
- **Crittografare il traffico di migrazione dello stato e VM** - consente di crittografare lo stato della macchina virtuale salvata e live migration.

### <a name="enable-isolated-user-mode"></a>Abilitare la modalità utente isolato

Se si seleziona **abilitare Trusted Platform Module** sugli host Hyper-V che eseguono versioni di Windows precedenti a Windows 10 ricorrenza annuale dell'aggiornamento, è necessario attivare la modalità utente isolato. Non è necessario eseguire questa operazione per gli host Hyper-V che eseguono Windows Server 2016 o Windows 10 Anniversary Update o versione successiva.

La modalità utente isolato è l'ambiente di runtime che ospita applicazioni per la sicurezza in modalità protetta virtuale nell'host Hyper-V. Modalità virtuale sicura viene utilizzata per proteggere e lo stato del chip TPM virtuale.  

Per abilitare la modalità utente isolato nell'host Hyper-V che eseguono versioni precedenti di Windows 10,  

1.  Aprire Windows PowerShell come amministratore.  

2.  Eseguire i comandi seguenti:  

    ```  
    Enable-WindowsOptionalFeature -Feature IsolatedUserMode -Online  
    New-Item -Path HKLM:\SYSTEM\CurrentControlSet\Control\DeviceGuard -Force  
    New-ItemProperty -Path HKLM:\SYSTEM\CurrentControlSet\Control\DeviceGuard -Name EnableVirtualizationBasedSecurity -Value 1 -PropertyType DWord -Force  

    ```  

È possibile eseguire la migrazione di una macchina virtuale con virtual TPM è attivato in qualsiasi host che esegue Windows Server 2016, Windows 10 consente di creare versioni 10586 o versione successiva. Ma se si esegue la migrazione, in un altro host, potrebbe non essere in grado di avviare il servizio. È necessario aggiornare la protezione con chiave per la macchina virtuale per autorizzare il nuovo host per eseguire la macchina virtuale. Per ulteriori informazioni, vedere [infrastruttura protetta e macchine virtuali schermati](https://go.microsoft.com/fwlink/?LinkId=746381) e [Requisiti di sistema per Hyper-V in Windows Server 2016](../System-requirements-for-Hyper-V-on-Windows.md).  

## <a name="security-policy-in-hyper-v-manager"></a>Criteri di sicurezza nella gestione di Hyper-V  
Per una maggiore protezione macchina virtuale, utilizzare il **abilitare schermatura** opzione per disabilitare le funzionalità di gestione connessione alla console, PowerShell Direct e alcuni componenti di integrazione. Se si seleziona questa opzione, **avvio protetto**, **abilitare Trusted Platform Module**, e **il traffico di migrazione dello stato di crittografia e VM** opzioni sono selezionate e applicate.   

È possibile eseguire localmente la macchina virtuale schermata senza configurare il servizio sorveglianza Host. Ma se si esegue la migrazione, in un altro host, potrebbe non essere in grado di avviare il servizio. È necessario aggiornare la protezione con chiave per la macchina virtuale per autorizzare il nuovo host per eseguire la macchina virtuale. Per ulteriori informazioni, vedere [infrastruttura protetta e macchine virtuali schermati](https://go.microsoft.com/fwlink/?LinkId=746381).  

Per altre informazioni sulla sicurezza in Windows Server, vedere [Sicurezza e controllo](../../../security/Security-and-Assurance.md).  
