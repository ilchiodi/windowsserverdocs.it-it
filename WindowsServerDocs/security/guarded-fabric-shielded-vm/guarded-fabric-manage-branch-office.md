---
title: Considerazioni su Office ramo
ms.custom: na
ms.prod: windows-server-threshold
ms.topic: article
manager: dongill
author: rpsqrd
ms.technology: security-guarded-fabric
ms.openlocfilehash: d93c37227af1eb62368fbcd4ec5d6a48374b45ff
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59877062"
---
# <a name="branch-office-considerations"></a>Considerazioni sulla succursale

> Si applica a: Windows Server 2019, Windows Server (canale semestrale), 

Questo articolo descrive le procedure consigliate per l'esecuzione di macchine virtuali schermate in succursali e altri scenari remoti in cui gli host Hyper-V potrebbero essere periodi di tempo con connettività limitata al servizio HGS.

## <a name="fallback-configuration"></a>Configurazione del fallback

A partire da Windows Server versione 1709, è possibile configurare un set aggiuntivo di URL del servizio sorveglianza Host negli host Hyper-V per l'uso quando il servizio HGS primario non risponde.
In questo modo è possibile eseguire un cluster locale di HGS utilizzato come server primario per ottenere prestazioni migliori grazie alla possibilità di eseguire il fallback a HGS aziendali del tuo Data Center se il server locale non sono attivi.

Per usare l'opzione di fallback, è necessario configurare due server HGS. È possibile eseguire Windows Server 2019 o Windows Server 2016 e deve essere parte di cluster uguali o diversi. Se sono cluster diversi, è consigliabile stabilire procedure operative per assicurarsi che i criteri di attestazione sono sincronizzate tra i due server. Entrambi devono essere in grado di autorizzare correttamente l'host Hyper-V per eseguire le macchine virtuali schermate e impostare il materiale della chiave necessari per avviare le macchine virtuali schermate. È possibile scegliere di disporre di una coppia di crittografia condivisa e firma dei certificati tra i due cluster, oppure utilizzare certificati separati e configurare il servizio HGS di schermatura della macchina virtuale per autorizzare (coppie certificato di crittografia o la firma) sia sorveglianti nei dati di schermatura file.

Quindi eseguire l'aggiornamento host Hyper-V per Windows Server versione 1709 o Windows Server 2019 ed eseguire il comando seguente:
```powershell
# Replace https://hgs.primary.com and https://hgs.backup.com with your own domain names and protocols
Set-HgsClientConfiguration -KeyProtectionServerUrl 'https://hgs.primary.com/KeyProtection' -AttestationServerUrl 'https://hgs.primary.com/Attestation' -FallbackKeyProtectionServerUrl 'https://hgs.backup.com/KeyProtection' -FallbackAttestationServerUrl 'https://hgs.backup.com/Attestation'
```

Per annullare la configurazione di un server di fallback, è sufficiente omettere entrambi i parametri di fallback:
```powershell
Set-HgsClientConfiguration -KeyProtectionServerUrl 'https://hgs.primary.com/KeyProtection' -AttestationServerUrl 'https://hgs.primary.com/Attestation'
```

Affinché l'host Hyper-V passare l'attestazione con entrambi i server primari e di fallback, è necessario assicurarsi che le informazioni di attestazione siano aggiornate con entrambi i cluster HGS.
Inoltre, i certificati usati per decrittografare TPM della macchina virtuale devono essere disponibili in entrambi i cluster HGS.
È possibile configurare ogni servizio HGS con certificati diversi e configurare la macchina virtuale per considerare attendibili entrambi o aggiungere un set di certificati condiviso per entrambi i cluster HGS.

Per altre informazioni sulla configurazione del servizio HGS in una succursale con gli URL di fallback, vedere il post di blog [migliorato supporto delle succursali per le macchine virtuali schermate in Windows Server, versione 1709](https://blogs.technet.microsoft.com/datacentersecurity/2017/11/15/improved-branch-office-support-for-shielded-vms-in-windows-server-version-1709/).


## <a name="offline-mode"></a>Modalità offline

La modalità non in linea consente la macchina virtuale schermata da attivare quando HGS non può essere raggiunto, purché la configurazione della sicurezza dell'host di Hyper-V non è stato modificato.
Modalità non in linea funziona mediante la memorizzazione nella cache una versione speciale della protezione con chiave TPM VM nell'host Hyper-V.
La protezione con chiave viene crittografata per la configurazione di sicurezza corrente dell'host (usando la chiave di identità di sicurezza basata sulla virtualizzazione).
Se l'host è in grado di comunicare con HGS e la configurazione di sicurezza non è stato modificato, sarà possibile utilizzare la protezione con chiave memorizzata nella cache per avviare la macchina virtuale schermata.
Quando vengono modificate impostazioni di sicurezza del sistema, ad esempio un nuovo criterio di integrità di codice in corso l'applicazione o l'avvio protetto viene disabilitata, verranno invalidate le protezioni con chiave memorizzata nella cache e l'host disporrà di attestare con un servizio HGS prima di qualsiasi VM schermate possano essere riavviate offline.

Modalità non in linea richiede la compilazione di Windows Server Insider Preview 17609 o versione successiva per il cluster del servizio sorveglianza Host e host Hyper-V.
È controllato da un criterio nel servizio HGS, che è disabilitato per impostazione predefinita.
Per abilitare il supporto per la modalità offline, eseguire il comando seguente in un nodo del servizio HGS:

```powershell
Set-HgsKeyProtectionConfiguration -AllowKeyMaterialCaching:$true
```

Poiché le protezioni con chiave inseribili nella cache sono univoche per ogni macchina virtuale schermata, dovrai completamente arrestato (non riavviare) e avviare le macchine virtuali schermate per ottenere una protezione con chiave inseribili nella cache dopo che questa impostazione è abilitata sul servizio HGS.
Se la macchina virtuale schermata viene eseguita la migrazione a un host Hyper-V che eseguono una versione precedente di Windows Server o ottiene una nuova protezione con chiave da una versione precedente del servizio HGS, non sarà in grado di avviare stessa in modalità offline, ma può continuare l'esecuzione in modalità online quando è l'accesso al servizio HGS Disp è possibile.
