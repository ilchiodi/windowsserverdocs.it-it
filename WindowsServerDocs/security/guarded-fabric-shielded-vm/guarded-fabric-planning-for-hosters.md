---
title: Infrastruttura sorvegliata e guida alla pianificazione delle VM schermate per i provider di hosting
ms.custom: na
ms.prod: windows-server-threshold
ms.topic: article
ms.assetid: 854defc8-99f8-4573-82c0-f484e0785859
manager: dongill
author: nirb-ms
ms.technology: security-guarded-fabric
ms.date: 08/29/2018
ms.openlocfilehash: baa360ecb81c0e8bd54e66771d41c11968b57714
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/10/2019
ms.locfileid: "70870448"
---
# <a name="guarded-fabric-and-shielded-vm-planning-guide-for-hosters"></a>Infrastruttura sorvegliata e guida alla pianificazione delle VM schermate per i provider di hosting

>Si applica a Windows Server 2019, Windows Server (canale semestrale), Windows Server 2016

Questo argomento descrive le decisioni di pianificazione che dovranno essere eseguite per consentire l'esecuzione delle macchine virtuali schermate nell'infrastruttura. Se si aggiorna un'infrastruttura di Hyper-V esistente o si crea una nuova infrastruttura, l'esecuzione di VM schermate è costituita da due componenti principali:

- Il servizio sorveglianza host (HGS) fornisce l'attestazione e la protezione delle chiavi, in modo che sia possibile verificare che le macchine virtuali schermate vengano eseguite solo su host Hyper-V approvati e integri. 
- Gli host Hyper-V approvati e integri in cui è possibile eseguire VM schermate (e VM normali) sono noti come host sorvegliati.

![HGS e host sorvegliato](../media/Guarded-Fabric-Shielded-VM/guarded-host-hgs-plus-host-diagram-basic.png)

## <a name="decision-1-trust-level-in-the-fabric"></a>#1 decisionale: Livello di attendibilità nell'infrastruttura

La modalità di implementazione del servizio sorveglianza host e degli host Hyper-V sorvegliati dipende principalmente dal livello di attendibilità che si sta cercando di realizzare nell'infrastruttura. Il livello di attendibilità è regolato dalla modalità di attestazione. Sono disponibili due opzioni che si escludono a vicenda:

1. Attestazione TPM

    Se l'obiettivo è quello di proteggere le macchine virtuali da amministratori malintenzionati o da un'infrastruttura compromessa, sarà possibile usare l'attestazione TPM attendibile. Questa opzione funziona bene per gli scenari di hosting multi-tenant e per gli asset di valore elevato in ambienti aziendali, ad esempio controller di dominio o server di contenuti come SQL o SharePoint.
    I criteri di integrità del codice protetti da hypervisor (HVCI obbligatoria) vengono misurati e la loro validità viene applicata da HGS prima che l'host sia autorizzato a eseguire macchine virtuali schermate. 

2. Attestazione chiave host

    Se i requisiti sono determinati principalmente dalla conformità che richiede la crittografia delle macchine virtuali sia inattivi sia in corso, si userà l'attestazione chiave host. Questa opzione funziona bene per i Data Center per utilizzo generico in cui si ha familiarità con gli amministratori dell'infrastruttura e dell'host Hyper-V che hanno accesso ai sistemi operativi guest delle macchine virtuali per operazioni di manutenzione e manutenzione quotidiane. 

    In questa modalità, l'amministratore dell'infrastruttura è responsabile solo per garantire l'integrità degli host Hyper-V. 
    Poiché HGS non è in grado di decidere quale sia o non è consentita l'esecuzione, malware e debugger funzioneranno nel modo previsto. 
    
    Tuttavia, i debugger che tentano di connettersi direttamente a un processo (ad esempio, WinDbg. exe) sono bloccati per le macchine virtuali schermate, perché il processo di lavoro della macchina virtuale (VMWP. exe) è una libreria PPL (Protected Process Light). 
    Le tecniche di debug alternative, ad esempio quelle usate da LiveKd. exe, non sono bloccate. 
    Diversamente dalle VM schermate, il processo di lavoro per le macchine virtuali supportate dalla crittografia non viene eseguito come PPL, quindi i debugger tradizionali come WinDbg. exe continueranno a funzionare normalmente.

    Una modalità di attestazione simile denominata amministrazione-attendibile attestazione (basata su Active Directory) è deprecata a partire da Windows Server 2019. 

Il livello di attendibilità scelto determinerà i requisiti hardware per gli host Hyper-V e i criteri applicati nell'infrastruttura. Se necessario, è possibile distribuire l'infrastruttura sorvegliata usando l'hardware esistente e l'attestazione attendibile per l'amministratore e quindi convertirla in attestazione Trusted TPM quando l'hardware è stato aggiornato ed è necessario rafforzare la sicurezza dell'infrastruttura.

## <a name="decision-2-existing-hyper-v-fabric-versus-a-new-separate-hyper-v-fabric"></a>#2 decisionale: Infrastruttura Hyper-V esistente rispetto a una nuova infrastruttura di Hyper-V separata

Se si dispone di un'infrastruttura esistente (Hyper-V o di altro tipo), è molto probabile che sia possibile usarla per eseguire VM schermate insieme a normali macchine virtuali. Alcuni clienti scelgono di integrare le VM schermate negli strumenti e nelle infrastrutture esistenti, mentre altre separano l'infrastruttura per motivi aziendali.

## <a name="hgs-admin-planning-for-the-host-guardian-service"></a>Pianificazione dell'amministratore di HGS per il servizio sorveglianza host

Distribuire il servizio sorveglianza host (HGS) in un ambiente a sicurezza elevata, sia che si trovi su un server fisico dedicato, una macchina virtuale schermata, una macchina virtuale in un host Hyper-V isolato (separato dall'infrastruttura che protegge) o uno separato logicamente usando un'altra versione di Azure abbonamento.   

| Area | Dettagli |
|------|---------|
| Requisiti di installazione | <ul><li>Un server (cluster a tre nodi consigliato per la disponibilità elevata)</li><li>Per il fallback sono necessari almeno due server HGS</li><li>I server possono essere virtuali o fisici (server fisico con TPM 2,0 consigliato; TPM 1,2 supportato anche</li><li>Installazione Server Core di Windows Server 2016 o versione successiva</li><li>Linea di rete per l'infrastruttura che consente la configurazione HTTP o di [fallback](guarded-fabric-manage-branch-office.md#fallback-configuration)</li><li>Certificato HTTPS consigliato per la convalida dell'accesso</li></ul> |
| Ridimensionamento | Ogni nodo del server HGS di dimensioni medie (8 core/4 GB) può gestire 1.000 host Hyper-V. |
| Management | Designare utenti specifici che gestiranno HGS. Devono essere separate dagli amministratori dell'infrastruttura. Per il confronto, i cluster HGS possono essere considerati in modo analogo a un'autorità di certificazione (CA) in termini di isolamento amministrativo, distribuzione fisica e livello generale di riservatezza della sicurezza. |
| Active Directory del servizio sorveglianza host | Per impostazione predefinita, HGS installa i propri Active Directory interni per la gestione. Si tratta di una foresta autonoma e gestita autonomamente ed è la configurazione consigliata per isolare HGS dall'infrastruttura.<br><br>Se si dispone già di una foresta Active Directory con privilegi elevati utilizzata per l'isolamento, è possibile utilizzare tale foresta anziché la foresta predefinita HGS. È importante che HGS non sia aggiunto a un dominio nella stessa foresta degli host Hyper-V o degli strumenti di gestione dell'infrastruttura. Questa operazione potrebbe consentire a un amministratore dell'infrastruttura di assumere il controllo di HGS. |
| Ripristino di emergenza | Sono disponibili tre opzioni:<br><ol><li>Installare un cluster HGS separato in ogni data center e autorizzare le macchine virtuali schermate per l'esecuzione nei data center primari e di backup. Questo consente di evitare la necessità di estendere il cluster attraverso una rete WAN e di isolare le macchine virtuali in modo che vengano eseguite solo nel sito designato.</li><li>Installare HGS in un cluster esteso tra due o più data center. Questo garantisce la resilienza se la rete WAN diventa inattiva, ma inserisce i limiti del clustering di failover. Non è possibile isolare i carichi di lavoro in un sito; una macchina virtuale autorizzata per l'esecuzione in un sito può essere eseguita su qualsiasi altro.</li><li>Registrare l'host Hyper-V con un altro HGS come failover.</li></ol>È inoltre consigliabile eseguire il backup di ogni HGS esportando la configurazione in modo che sia sempre possibile eseguire il ripristino localmente. Per altre informazioni, vedere [Export-HgsServerState](https://docs.microsoft.com/powershell/module/hgsserver/export-hgsserverstate) e [Import-HgsServerState](https://docs.microsoft.com/powershell/module/hgsserver/import-hgsserverstate). |
| Chiavi del servizio sorveglianza host | Un servizio sorveglianza host usa due coppie di chiavi asimmetriche, una chiave di crittografia e una chiave di firma, ciascuna rappresentata da un certificato SSL. Sono disponibili due opzioni per generare queste chiavi:<br><ol><li>Autorità di certificazione interna: è possibile generare queste chiavi usando l'infrastruttura PKI interna. Questo è adatto per un ambiente di Data Center.</li><li>Autorità di certificazione pubblicamente attendibili: usare un set di chiavi ottenute da un'autorità di certificazione pubblicamente attendibile. Questa è l'opzione che i provider di hosting devono usare.</li></ol>Si noti che sebbene sia possibile usare i certificati autofirmati, non è consigliabile per gli scenari di distribuzione diversi dai Lab di prova.<br><br>Oltre a disporre di chiavi di HGS, un provider di hosting può usare "Bring your own key", in cui i tenant possono fornire le proprie chiavi in modo che alcuni o tutti i tenant possano avere una propria chiave HGS specifica. Questa opzione è appropriata per i provider di hosting che possono fornire un processo fuori banda per consentire ai tenant di caricare le proprie chiavi. |
| Archiviazione delle chiavi del servizio sorveglianza host | Per la sicurezza più avanzata possibile, è consigliabile che le chiavi di HGS vengano create e archiviate esclusivamente in un modulo di protezione hardware (HSM). Se non si usa HSM, è consigliabile applicare BitLocker nei server HGS. |

## <a name="fabric-admin-planning-for-guarded-hosts"></a>Pianificazione dell'amministrazione dell'infrastruttura per gli host sorvegliati

| Area | Dettagli |
|------|---------|
| Hardware | <ul><li>Attestazione chiave host: È possibile usare qualsiasi hardware esistente come host sorvegliato. Esistono alcune eccezioni (per assicurarsi che l'host possa usare nuovi meccanismi di sicurezza a partire da Windows Server 2016, vedere [hardware compatibile con la protezione basata sulla virtualizzazione di Windows server 2016 per l'integrità del codice](guarded-fabric-compatible-hardware-with-virtualization-based-protection-of-code-integrity.md).</li><li>Attestazione TPM: È possibile usare qualsiasi hardware con la [garanzia hardware Assurance aggiuntiva](https://msdn.microsoft.com/windows/hardware/commercialize/design/compatibility/systems#system-server-assurance) , purché sia configurata in modo appropriato (vedere [configurazioni del server conformi alle VM schermate e protezione basata sulla virtualizzazione dell'integrità del codice ](guarded-fabric-compatible-hardware-with-virtualization-based-protection-of-code-integrity.md)per la configurazione specifica). Sono inclusi TPM 2,0 e UEFI versione 2.3.1 c e successive.</li></ul> |
| OS | È consigliabile usare l'opzione Server Core per il sistema operativo host Hyper-V. |
| Implicazioni sulle prestazioni | In base ai test delle prestazioni, si prevede una differenza circa il 5% di densità tra l'esecuzione di VM schermate e macchine virtuali non schermate. Ciò significa che se un host Hyper-V specificato può eseguire 20 macchine virtuali non schermate, si prevede che possa eseguire 19 VM schermate.<br><br>Assicurarsi di verificare il dimensionamento con i carichi di lavoro tipici. Potrebbero ad esempio essere presenti alcuni outlier con carichi di lavoro di i/o intensi orientati alla scrittura che influiranno ulteriormente sulla differenza di densità. |
| Considerazioni sulla succursale | A partire da Windows Server versione 1709, è possibile specificare un URL di fallback per un server HGS virtualizzato eseguito localmente come macchina virtuale schermata nella succursale. L'URL di fallback può essere usato quando la filiale perde la connettività ai server HGS nel Data Center. Nelle versioni precedenti di Windows Server, un host Hyper-V in esecuzione in una succursale richiede la connettività al servizio sorveglianza host per l'accensione o la migrazione in tempo reale di VM schermate. Per altre informazioni, vedere [considerazioni sulle succursali](guarded-fabric-manage-branch-office.md). |
