---
title: Eseguire l'aggiornamento di un'infrastruttura sorvegliata a Windows Server 2019
ms.prod: windows-server-threshold
ms.topic: article
manager: dongill
author: rpsqrd
ms.technology: security-guarded-fabric
ms.date: 11/21/2018
ms.openlocfilehash: 274bdf027947ffb6fe807d4acd0a3b2174c20e28
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59867452"
---
# <a name="upgrade-a-guarded-fabric-to-windows-server-2019"></a>Eseguire l'aggiornamento di un'infrastruttura sorvegliata a Windows Server 2019

> Si applica a: Windows Server 2019, Windows Server (canale semestrale), Windows Server 2016

Questo articolo descrive i passaggi necessari per eseguire l'aggiornamento di un'infrastruttura sorvegliata esistente da Windows Server 2016, Windows Server versione 1709 o Windows Server versione 1803 per Windows Server 2019.

## <a name="whats-new-in-windows-server-2019"></a>Novità di Windows Server 2019

Quando si esegue un'infrastruttura sorvegliata in Windows Server 2019, è possibile sfruttare molte nuove funzionalità:

**Ospitare l'attestazione chiave** è la modalità di attestazione più recente, progettate per rendere più semplice eseguire le macchine virtuali schermate quando gli host Hyper-V non hanno dispositivi TPM 2.0 disponibili per l'attestazione TPM. Attestazione chiave host utilizza coppie di chiavi per autenticare gli host con HGS, eliminando la necessità per gli host venga aggiunto a un dominio di Active Directory, eliminando la relazione di trust di Active Directory tra la foresta aziendale e servizio HGS e riducendo il numero di porte del firewall aperta. Attestazione chiave host sostituisce l'attestazione di Active Directory, che è stato deprecato in Windows Server 2019.

**Versione di attestazione v2** : per supportare l'attestazione chiave Host e le nuove funzionalità in futuro, è stato introdotto il controllo delle versioni di HGS. Una nuova installazione di Windows Server 2019 HGS comporterà il server utilizzando l'attestazione v2, che significa che può supportare l'attestazione chiave Host per gli host Windows Server 2019 e comunque supportare v1 host in Windows Server 2016. Gli aggiornamenti sul posto a 2019 rimarrà nella versione v1 fino a quando non si abiliti manualmente v2. La maggior parte dei cmdlet ora disponibile il parametro - HgsVersion che consente di specificare se si desidera usare i criteri di attestazione legacy o moderno.

**Supporto per Linux di macchine virtuali schermate** -gli host Hyper-V che eseguono Windows Server 2019 può eseguire Linux macchine virtuali schermate. Sebbene schermata di Linux macchine virtuali sono stati introdotti sin Windows Server versione 1709, Windows Server 2019 è la prima versione di canale di manutenzione di lungo termine per supportarli.

**Creare un ramo miglioramenti office** -è stata semplificata per l'esecuzione di macchine virtuali schermate in succursali con supporto per le macchine virtuali schermate non in linea e configurazioni di fallback in host Hyper-V.

**Binding host TPM** -per più di proteggere carichi di lavoro, in cui desidera che una macchina virtuale schermata esecuzione solo il primo host in cui è stato creato, ma nessun altro, è ora possibile associare la macchina virtuale per tale host tramite TPM dell'host. È consigliabile utilizzarla per workstation con accesso con privilegi e succursali, uffici, piuttosto che dei carichi di lavoro di Data Center generale che è necessario eseguire la migrazione tra host.

## <a name="compatibility-matrix"></a>Matrice di compatibilità

Prima di aggiornare l'infrastruttura sorvegliata per Windows Server 2019, esaminare la matrice di compatibilità seguenti per verificare se la configurazione è supportata.

|  | WS2016 HGS | WS2019 HGS|
|---|---|---|
|**WS2016 Host Hyper-V** | Supportato | Supportato<sup>1</sup>|
|**WS2019 Host Hyper-V** | Non supportato<sup>2</sup> | Supportato|

<sup>1</sup> gli host Windows Server 2016 consente di attestare solo su server HGS 2019 di Windows Server usando il protocollo di attestazione v1. Nuove funzionalità che sono disponibili esclusivamente nel protocollo di attestazione v2, inclusa l'attestazione chiave Host, non sono supportate per gli host Windows Server 2016.

<sup>2</sup> Microsoft è a conoscenza di un problema che impedisce gli host Windows Server 2019 usando un'attestazione TPM di attestare correttamente rispetto a un server Windows Server 2016 HGS. Questa limitazione verrà risolta in un futuro aggiornamento per Windows Server 2016.

## <a name="upgrade-hgs-to-windows-server-2019"></a>Eseguire l'aggiornamento a Windows Server 2019 HGS

È consigliabile aggiornare il cluster del servizio HGS per Windows Server 2019 prima di aggiornare gli host Hyper-V per garantire che tutti gli host, se sono in esecuzione Windows Server 2016 o 2019, possono continuare attestare correttamente.

L'aggiornamento del cluster del servizio HGS verrà richiesto di rimuovere temporaneamente un nodo dal cluster alla volta mentre viene aggiornato. Questo riduce la capacità del cluster di rispondere alle richieste dagli host Hyper-V e può comportare tempi di risposta lenti o interruzioni del servizio per i tenant. Assicurarsi di che avere una capacità sufficiente per gestire l'attestazione e le richieste di rilasciare il tasto prima di aggiornare un server HGS.

Per aggiornare il cluster del servizio HGS, seguire la procedura seguente in ogni nodo del cluster, un nodo alla fase:

1.  Rimuovere il server HGS dal cluster eseguendo `Clear-HgsServer` in un prompt di PowerShell con privilegi elevato. Questo cmdlet rimuoverà l'archivio replicato HGS, siti Web HGS e nodo dal cluster di failover.
2.  Se il server HGS è un controller di dominio (configurazione predefinita), è necessario eseguire `adprep /forestprep` e `adprep /domainprep` nel primo nodo in fase di aggiornamento per preparare il dominio per un aggiornamento del sistema operativo. Vedere le [documentazione relativa all'aggiornamento Active Directory Domain Services](https://docs.microsoft.com/windows-server/identity/ad-ds/deploy/upgrade-domain-controllers#supported-in-place-upgrade-paths) per altre informazioni.
3.  Eseguire un' [aggiornamena sul posa](../../get-started-19/install-upgrade-migrate-19.md) a Windows Server 2019.
4.  Eseguire [Initialize-HgsServer](guarded-fabric-configure-additional-hgs-nodes.md) aggiungere il nodo al cluster.

Dopo che tutti i nodi sono stati aggiornati a Windows Server 2019, si può scegliere di aggiornare la versione del servizio HGS a v2 per supportare le nuove funzionalità come l'attestazione chiave Host.

```powershell
Set-HgsServerVersion  v2
```

## <a name="upgrade-hyper-v-hosts-to-windows-server-2019"></a>Eseguire l'aggiornamento host Hyper-V per Windows Server 2019

Prima di aggiornare gli host Hyper-V per Windows Server 2019, assicurarsi che il cluster del servizio HGS è già aggiornato a Windows Server 2019 e che sono state spostate tutte le macchine virtuali dal server Hyper-V.

1.  Se si usano criteri di integrità di Windows Defender Application Control codice sul server (sempre il caso quando si usa l'attestazione TPM), assicurarsi che il criterio è in modalità di controllo oppure disabilitato prima di tentare di aggiornare il server. [Informazioni su come disabilitare un criterio WDAC](https://docs.microsoft.com/en-us/windows/security/threat-protection/windows-defender-application-control/disable-windows-defender-application-control-policies)
2.  Seguire le indicazioni fornite nel [centro di aggiornamento di Windows Server](http://aka.ms/upgradecenter) per aggiornare l'host di Windows Server 2019. Se l'host Hyper-V fa parte di un Cluster di Failover, è consigliabile usare un [aggiornamento in sequenza del Cluster del sistema operativo](../../failover-clustering/Cluster-Operating-System-Rolling-Upgrade.md).
3.  [Testare e abilitare di nuovo](https://docs.microsoft.com/en-us/windows/security/threat-protection/windows-defender-application-control/audit-windows-defender-application-control-policies) ai criteri di Windows Defender Application Control, se ne fosse presente una abilitata prima dell'aggiornamento.
4.  Eseguire `Get-HgsClientConfiguration` per controllare se **IsHostGuarded = True**, vale a dire l'host sia correttamente passando l'attestazione con il server HGS.
5.  Se si usa l'attestazione TPM, potrebbe essere necessario [acquisire nuovamente i criteri di integrità della linea di base o codice TPM](guarded-fabric-add-host-information-for-tpm-trusted-attestation.md) dopo l'aggiornamento per passare l'attestazione.
6.  Inizio esecuzione di macchine virtuali schermate nell'host nuovamente.

## <a name="switch-to-host-key-attestation"></a>Opzione per ospitare l'attestazione chiave

Seguire questa procedura se si utilizza attestazione basata su Active Directory e si vuole eseguire l'aggiornamento l'attestazione chiave Host. Si noti che l'attestazione basata su Active Directory è stato deprecato in Windows Server 2019 e potrebbe essere rimosso in una versione futura.

1.  Assicurarsi che il server HGS funziona in modalità di attestazione v2 eseguendo il comando seguente. Gli host esistenti in v1 continuerà attestare anche quando il server HGS viene aggiornato alla versione 2.

    ```powershell
    Set-HgsServerVersion v2
    ```

2.  [Generare chiavi host](guarded-fabric-create-host-key.md) da ognuna di Hyper-V ospita e questi vengono registrati con HGS. Poiché HGS comunque funziona in modalità Active Directory, si riceverà un avviso che le nuove chiavi di host non sono immediatamente valide. Ciò è intenzionale, come si desidera modificare in modalità chiave host fino a quando tutti gli host possono attestare correttamente con le chiavi host.

3.  Dopo che le chiavi host sono state registrate per ogni host, è possibile configurare HGS per usare la modalità di attestazione chiave host:

    ```powershell
    Set-HgsServer -TrustHostKey
    ```

    Se si riscontrano problemi con la modalità chiave host e necessario ripristinare l'attestazione basata su Active Directory, eseguire il comando seguente nel servizio HGS:

    ```powershell
    Set-HgsServer -TrustActiveDirectory
    ```