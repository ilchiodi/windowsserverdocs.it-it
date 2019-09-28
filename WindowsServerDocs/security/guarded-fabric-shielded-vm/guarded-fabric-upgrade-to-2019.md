---
title: Eseguire l'aggiornamento di un'infrastruttura sorvegliata a Windows Server 2019
ms.prod: windows-server
ms.topic: article
manager: dongill
author: rpsqrd
ms.technology: security-guarded-fabric
ms.date: 11/21/2018
ms.openlocfilehash: 621d4175894bb235475155507a896a251dec0f7e
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71386345"
---
# <a name="upgrade-a-guarded-fabric-to-windows-server-2019"></a>Eseguire l'aggiornamento di un'infrastruttura sorvegliata a Windows Server 2019

> Si applica a: Windows Server 2019, Windows Server (canale semestrale), Windows Server 2016

Questo articolo descrive i passaggi necessari per aggiornare un'infrastruttura sorvegliata esistente da Windows Server 2016, Windows Server versione 1709 o Windows Server versione 1803 a Windows Server 2019.

## <a name="whats-new-in-windows-server-2019"></a>Novità di Windows Server 2019

Quando si esegue un'infrastruttura sorvegliata in Windows Server 2019, è possibile sfruttare alcune nuove funzionalità:

L' **attestazione della chiave host** è la modalità di attestazione più recente, progettata per semplificare l'esecuzione di VM schermate quando gli host Hyper-V non hanno dispositivi TPM 2,0 disponibili per l'attestazione TPM. L'attestazione chiave host utilizza coppie di chiavi per autenticare gli host con HGS, rimuovendo il requisito per l'aggiunta di host a un dominio Active Directory, eliminando il trust di Active Directory tra HGS e la foresta aziendale e riducendo il numero di porte del firewall aperte. L'attestazione chiave host sostituisce Active Directory attestazione, che è deprecata in Windows Server 2019.

**Versione di attestazione V2** : per supportare l'attestazione chiave host e le nuove funzionalità in futuro, è stato introdotto il controllo delle versioni in HGS. Una nuova installazione di HGS in Windows Server 2019 produrrà il server con attestazione V2, il che significa che può supportare l'attestazione chiave host per gli host Windows Server 2019 e supportare comunque gli host V1 in Windows Server 2016. Gli aggiornamenti sul posto a 2019 rimarranno nella versione V1 fino a quando non si abilita manualmente V2. La maggior parte dei cmdlet dispone ora di un parametro-HgsVersion che consente di specificare se si desidera utilizzare i criteri di attestazione legacy o moderni.

**Supporto per VM schermate Linux** : gli host Hyper-V che eseguono Windows Server 2019 possono eseguire VM schermate Linux. Sebbene le macchine virtuali schermate di Linux siano state apportate a partire da Windows Server versione 1709, Windows Server 2019 è la prima versione del canale di manutenzione a lungo termine per supportarli.

**Miglioramenti per le succursali** : è stato semplificato l'esecuzione di VM schermate nelle succursali con supporto per le macchine virtuali schermate offline e le configurazioni di fallback negli host Hyper-V.

**Binding host TPM** : per i carichi di lavoro più sicuri, in cui si vuole che una macchina virtuale schermata venga eseguita solo nel primo host in cui è stata creata ma non in altre, è ora possibile associare la macchina virtuale all'host usando il TPM dell'host. Questo è il modo migliore per le workstation con accesso con privilegi e le succursali, anziché i carichi di lavoro di datacenter generali che devono eseguire la migrazione tra gli host.

## <a name="compatibility-matrix"></a>Matrice di compatibilità

Prima di aggiornare l'infrastruttura sorvegliata a Windows Server 2019, esaminare la seguente matrice di compatibilità per verificare se la configurazione è supportata.

|  | HGS WS2016 | HGS WS2019|
|---|---|---|
|**WS2016 Hyper-V host** | Supportato | Supportato<sup>1</sup>|
|**WS2019 Hyper-V host** | Non supportato<sup>2</sup> | Supportato|

<sup>1</sup> gli host windows server 2016 possono attestare solo i server HGS di windows server 2019 usando il protocollo di attestazione V1. Le nuove funzionalità disponibili esclusivamente nel protocollo di attestazione V2, inclusa l'attestazione della chiave host, non sono supportate per gli host Windows Server 2016.

<sup>2</sup> Microsoft è a conoscenza di un problema che impedisce agli host windows server 2019 che usano l'attestazione TPM di essere attestati correttamente in un server HGS windows server 2016. Questa limitazione verrà risolta in un aggiornamento futuro per Windows Server 2016.

## <a name="upgrade-hgs-to-windows-server-2019"></a>Aggiornare HGS a Windows Server 2019

È consigliabile aggiornare il cluster HGS a Windows Server 2019 prima di aggiornare gli host Hyper-V per assicurarsi che tutti gli host, che eseguono Windows Server 2016 o 2019, possano continuare ad attestare correttamente.

Per l'aggiornamento del cluster HGS sarà necessario rimuovere temporaneamente un nodo dal cluster alla volta durante l'aggiornamento. In questo modo si riduce la capacità del cluster per rispondere alle richieste provenienti dagli host Hyper-V e potrebbero verificarsi tempi di risposta o interruzioni del servizio lenti per i tenant. Assicurarsi di disporre di capacità sufficiente per gestire le richieste di attestazione e rilascio della chiave prima di aggiornare un server HGS.

Per aggiornare il cluster HGS, eseguire la procedura seguente in ogni nodo del cluster, un nodo alla volta:

1.  Rimuovere il server HGS dal cluster eseguendo `Clear-HgsServer` in un prompt di PowerShell con privilegi elevati. Questo cmdlet rimuoverà l'archivio replicato di HGS, i siti Web HGS e il nodo dal cluster di failover.
2.  Se il server HGS è un controller di dominio (configurazione predefinita), è necessario eseguire `adprep /forestprep` e `adprep /domainprep` sul primo nodo da aggiornare per preparare il dominio per un aggiornamento del sistema operativo. Per ulteriori informazioni, vedere la [documentazione relativa all'aggiornamento Active Directory Domain Services](https://docs.microsoft.com/windows-server/identity/ad-ds/deploy/upgrade-domain-controllers#supported-in-place-upgrade-paths) .
3.  Eseguire un [aggiornamento sul posto](../../get-started-19/install-upgrade-migrate-19.md) a Windows Server 2019.
4.  Eseguire [Initialize-HgsServer](guarded-fabric-configure-additional-hgs-nodes.md) per aggiungere di nuovo il nodo al cluster.

Una volta che tutti i nodi sono stati aggiornati a Windows Server 2019, facoltativamente è possibile aggiornare la versione di HGS a V2 per supportare le nuove funzionalità, ad esempio l'attestazione della chiave host.

```powershell
Set-HgsServerVersion  v2
```

## <a name="upgrade-hyper-v-hosts-to-windows-server-2019"></a>Aggiornare gli host Hyper-V a Windows Server 2019

Prima di aggiornare gli host Hyper-V a Windows Server 2019, verificare che il cluster HGS sia già stato aggiornato a Windows Server 2019 e che siano state spostate tutte le macchine virtuali dal server Hyper-V.

1.  Se si usano i criteri di integrità del codice di controllo delle applicazioni di Windows Defender nel server (sempre quando si usa l'attestazione TPM), verificare che i criteri siano in modalità di controllo o disabilitati prima di tentare di aggiornare il server. [Informazioni su come disabilitare un criterio WDAC](https://docs.microsoft.com/windows/security/threat-protection/windows-defender-application-control/disable-windows-defender-application-control-policies)
2.  Seguire le istruzioni nel [contenuto dell'aggiornamento di Windows Server](../../upgrade/upgrade-overview.md) per aggiornare l'host a windows server 2019. Se l'host Hyper-V fa parte di un cluster di failover, prendere in considerazione l'uso di un [aggiornamento in sequenza del sistema operativo del cluster](../../failover-clustering/Cluster-Operating-System-Rolling-Upgrade.md).
3.  [Testare e riabilitare](https://docs.microsoft.com/windows/security/threat-protection/windows-defender-application-control/audit-windows-defender-application-control-policies) i criteri di controllo delle applicazioni di Windows Defender se ne è stato abilitato uno prima dell'aggiornamento.
4.  Eseguire `Get-HgsClientConfiguration` per verificare se **IsHostGuarded = true**, ovvero se l'host passa correttamente l'attestazione al server HGS.
5.  Se si usa l'attestazione TPM, potrebbe essere necessario [riacquisire i criteri di integrità del codice o di base del TPM](guarded-fabric-add-host-information-for-tpm-trusted-attestation.md) dopo l'aggiornamento per passare l'attestazione.
6.  Avviare di nuovo l'esecuzione di VM schermate nell'host.

## <a name="switch-to-host-key-attestation"></a>Passa a attestazione chiave host

Eseguire la procedura seguente se si sta eseguendo l'attestazione basata su Active Directory e si vuole eseguire l'aggiornamento all'attestazione chiave host. Si noti che l'attestazione basata su Active Directory è deprecata in Windows Server 2019 e potrebbe essere rimossa in una versione futura.

1.  Verificare che il server HGS funzioni in modalità di attestazione V2 eseguendo il comando seguente. Gli host V1 esistenti continueranno a essere attestati anche quando il server HGS viene aggiornato alla versione V2.

    ```powershell
    Set-HgsServerVersion v2
    ```

2.  [Generare chiavi host](guarded-fabric-create-host-key.md) da ognuno degli host Hyper-V e registrarli con HGS. Poiché HGS funziona ancora in modalità Active Directory, verrà visualizzato un avviso che indica che le nuove chiavi host non sono immediatamente valide. Questo comportamento è intenzionale, poiché non si desidera passare alla modalità chiave host fino a quando tutti gli host non sono in grado di attestare correttamente le chiavi host.

3.  Una volta registrate le chiavi host per ogni host, è possibile configurare HGS per l'uso della modalità di attestazione chiave host:

    ```powershell
    Set-HgsServer -TrustHostKey
    ```

    Se si riscontrano problemi con la modalità chiave host ed è necessario ripristinare l'attestazione basata su Active Directory, eseguire il comando seguente in HGS:

    ```powershell
    Set-HgsServer -TrustActiveDirectory
    ```