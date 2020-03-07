---
title: Considerazioni sulle succursali
ms.custom: na
ms.prod: windows-server
ms.topic: article
manager: dongill
author: rpsqrd
ms.technology: security-guarded-fabric
ms.openlocfilehash: 5a07553e6662fd79230d566ba2049c5e8997f4d6
ms.sourcegitcommit: 06ae7c34c648538e15c4d9fe330668e7df32fbba
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/05/2020
ms.locfileid: "78371369"
---
# <a name="branch-office-considerations"></a>Considerazioni sulla succursale

> Si applica a: Windows Server 2019, Windows Server (canale semestrale), 

Questo articolo descrive le procedure consigliate per l'esecuzione di macchine virtuali schermate nelle succursali e in altri scenari remoti in cui gli host Hyper-V possono avere periodi di tempo con connettività limitata a HGS.

## <a name="fallback-configuration"></a>Configurazione di fallback

A partire da Windows Server versione 1709, è possibile configurare un set aggiuntivo di URL del servizio sorveglianza host negli host Hyper-V da usare quando il HGS primario non risponde.
In questo modo è possibile eseguire un cluster HGS locale che viene usato come server primario per ottenere prestazioni migliori con la possibilità di eseguire il fallback al HGS del data center aziendale se i server locali sono inattivi.

Per usare l'opzione fallback, è necessario configurare due server HGS. Possono eseguire Windows Server 2019 o Windows Server 2016 e far parte dello stesso cluster o di cluster diversi. Se si tratta di cluster diversi, sarà necessario stabilire le procedure operative per assicurarsi che i criteri di attestazione siano sincronizzati tra i due server. Entrambi devono essere in grado di autorizzare correttamente l'host Hyper-V a eseguire macchine virtuali schermate e avere il materiale della chiave necessario per avviare le VM schermate. È possibile scegliere di disporre di una coppia di certificati di firma e firma condivisa tra i due cluster oppure usare certificati distinti e configurare la macchina virtuale schermata di HGS per autorizzare entrambi i tutori (coppie di certificati di crittografia/firma) nel file di dati di schermatura.

Aggiornare quindi gli host Hyper-V a Windows Server versione 1709 o Windows Server 2019 ed eseguire il comando seguente:
```powershell
# Replace https://hgs.primary.com and https://hgs.backup.com with your own domain names and protocols
Set-HgsClientConfiguration -KeyProtectionServerUrl 'https://hgs.primary.com/KeyProtection' -AttestationServerUrl 'https://hgs.primary.com/Attestation' -FallbackKeyProtectionServerUrl 'https://hgs.backup.com/KeyProtection' -FallbackAttestationServerUrl 'https://hgs.backup.com/Attestation'
```

Per annullare la configurazione di un server di fallback, è sufficiente omettere entrambi i parametri di fallback:
```powershell
Set-HgsClientConfiguration -KeyProtectionServerUrl 'https://hgs.primary.com/KeyProtection' -AttestationServerUrl 'https://hgs.primary.com/Attestation'
```

Per consentire all'host Hyper-V di passare l'attestazione con i server primari e di fallback, sarà necessario assicurarsi che le informazioni di attestazione siano aggiornate con entrambi i cluster HGS.
Inoltre, i certificati usati per decrittografare il TPM della macchina virtuale devono essere disponibili in entrambi i cluster HGS.
È possibile configurare ogni HGS con certificati diversi e configurare la macchina virtuale in modo da considerarla attendibile o aggiungere un set condiviso di certificati a entrambi i cluster HGS.

Per altre informazioni sulla configurazione di HGS in una succursale usando URL di fallback, vedere il post di Blog [miglioramento del supporto per le succursali per le VM schermate in Windows Server, versione 1709](https://blogs.technet.microsoft.com/datacentersecurity/2017/11/15/improved-branch-office-support-for-shielded-vms-in-windows-server-version-1709/).


## <a name="offline-mode"></a>Modalità offline

La modalità offline consente di attivare la macchina virtuale schermata quando non è possibile raggiungere HGS, purché la configurazione di sicurezza dell'host Hyper-V non sia cambiata.
La modalità offline funziona memorizzando nella cache una versione speciale della protezione con chiave TPM della macchina virtuale nell'host Hyper-V.
La protezione con chiave viene crittografata con la configurazione di sicurezza corrente dell'host (usando la chiave di identità di sicurezza basata sulla virtualizzazione).
Se l'host non riesce a comunicare con HGS e la relativa configurazione di sicurezza non è cambiata, sarà in grado di usare la protezione con chiave memorizzata nella cache per avviare la macchina virtuale schermata.
Quando si modificano le impostazioni di sicurezza nel sistema, ad esempio un nuovo criterio di integrità del codice applicato o l'avvio protetto viene disabilitato, le protezioni con chiave memorizzata nella cache verranno invalidate e l'host dovrà attestare con un HGS prima che le macchine virtuali schermate possano essere riavviate offline.

La modalità offline richiede Windows Server Insider Preview Build 17609 o successiva sia per il cluster del servizio sorveglianza host che per l'host Hyper-V.
Viene controllata da un criterio in HGS, che è disabilitato per impostazione predefinita.
Per abilitare il supporto per la modalità offline, eseguire il comando seguente in un nodo HGS:

```powershell
Set-HgsKeyProtectionConfiguration -AllowKeyMaterialCaching:$true
```

Poiché le protezioni con chiave memorizzabili nella cache sono univoche per ogni macchina virtuale schermata, sarà necessario arrestare completamente (non riavviare) e avviare le macchine virtuali schermate per ottenere una protezione con chiave memorizzabile nella cache dopo che questa impostazione è abilitata in HGS.
Se la macchina virtuale schermata viene migrata a un host Hyper-V che esegue una versione precedente di Windows Server o ottiene una nuova protezione con chiave da una versione precedente di HGS, non sarà in grado di avviarsi in modalità offline, ma può continuare l'esecuzione in modalità online quando è disponibile l'accesso a HGS in grado.
