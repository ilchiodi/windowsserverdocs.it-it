---
title: What ' s New in Windows Server, versione 1903
description: Questo argomento descrive alcune delle nuove funzionalità in Windows Server, versione 1903, ovvero un rilascio del canale semestrale.
ms.prod: windows-server-threshold
ms.technology: server-general
ms.topic: article
author: jasongerend
ms.author: jgerend
ms.date: 05/21/2019
ms.openlocfilehash: 2aa84df1ca162cb6e2dfa580b4b0744ff08cc95a
ms.sourcegitcommit: c8cc0b25ba336a2aafaabc92b19fe8faa56be32b
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/21/2019
ms.locfileid: "65983441"
---
# <a name="whats-new-in-windows-server-version-1903"></a>Novità in Windows Server, versione 1903

>Si applica a: Windows Server (Canale semestrale)

Questo argomento descrive alcune delle nuove funzionalità in Windows Server, versione 1903, ovvero un rilascio del canale semestrale. Queste funzionalità includono miglioramenti per l'esecuzione e gestione di contenitori, gli strumenti per l'utilizzo nelle installazioni Server Core e la possibilità di eseguire la migrazione di archiviazione da dispositivi Linux.

Per trovare invece le novità nella versione di Long-Term Servicing Channel (LTSC) più recente di Windows Server, vedere visualizzare [What ' s New in Windows Server 2019](../get-started-19/whats-new-19.md). Vedere anche [novità in Windows 10, contenuti per professionisti IT versione 1903](https://docs.microsoft.com/windows/whats-new/whats-new-windows-10-version-1903).

I requisiti di sistema per questa versione sono gli stessi 2019 di Windows Server, vedere [requisiti di sistema](../get-started-19/sys-reqs-19.md) per altre informazioni. Per informazioni su ciò che è stata rimossa di recente, vedere [funzionalità rimosse o pianificato per la sostituzione a partire da Windows Server, versione 1903](../get-started-19/removed-features-1903.md)

> [!NOTE]
> I contenitori di Windows devono usare la stessa versione di Windows come server host, o un' *precedenti* versione. Ad esempio, un server host che eseguono la versione rilasciata di Windows Server, versione 1903 (build 18342) può eseguire contenitori Windows Server con la versione stessa o versioni precedente e numero di build (anche se il contenitore utilizza una versione di anteprima di Insider di Windows). Per altre informazioni, vedi [compatibilità delle versioni di Windows contenitore](https://docs.microsoft.com/virtualization/windowscontainers/deploy-containers/version-compatibility).

## <a name="enhanced-support-for-non-microsoft-container-services"></a>Supporto avanzato per i servizi contenitore non Microsoft

Sono migliorate funzionalità di piattaforma per supportare i servizi contenitore di Azure e servizi contenitore non Microsoft.

- Abbiamo integrato containerd di elementi del report personalizzati con Host Compute Service (HCS) per supportare il POD dei contenitori di Windows e Linux in Windows (LCOW) in Azure.
- Abbiamo collaborato con la community di Kubernetes per abilitare il supporto dei contenitori Windows. Con la versione di Kubernetes v1.14, supporto del nodo Windows Server ufficialmente è laureato con lode dalla versione beta stabile. Per altre informazioni, vedi [i contenitori di Windows supportati in Kubernetes](https://cloudblogs.microsoft.com/opensource/2019/03/25/windows-server-containers-now-supported-kubernetes/).
- Tigrato Tigera per Windows è ora disponibile a livello generale come parte della sottoscrizione Tigera Essentials e offerte non-overlay rete e rete criteri interoperativo tra ambienti di Linux/Windows misti.
- È stato recapitato miglioramenti della scalabilità miglioramento della sovrimpressione il supporto per i contenitori di Windows, inclusa l'integrazione con Kubernetes nella versione più recente di Kubernetes e Flannel v1.14 di rete. Per altre informazioni, vedi [Introduzione al supporto tecnico di Windows in Kubernetes](https://kubernetes.io/docs/setup/windows/).

## <a name="directx-hardware-acceleration-in-containers"></a>Accelerazione hardware DirectX nei contenitori

Si abilita il supporto per l'accelerazione hardware di DirectX APIs in scenari di supporto tp contenitori Windows, ad esempio inferenza di Machine Learning (ML) utilizza locale di elaborazione grafica (GPU) unit hardware. Per altre informazioni, vedi [accelerazione GPU offrendo ai contenitori Windows](https://techcommunity.microsoft.com/t5/Containers/Bringing-GPU-acceleration-to-Windows-containers/ba-p/393939).

## <a name="updated-container-identity-and-group-managed-service-account-documentation"></a>Identità del contenitore aggiornata e il gruppo gestito documentazione account del servizio

Abbiamo aggiunto altri esempi e informazioni di compatibilità per il [Group Managed Service Accounts](https://docs.microsoft.com/virtualization/windowscontainers/manage-containers/manage-serviceaccounts) documentazione e reso il [modulo di PowerShell di specifica delle credenziali](https://www.powershellgallery.com/packages/CredentialSpec) disponibile in PowerShell Gallery. Per altre informazioni, vedere la [nuove funzionalità per l'identità contenitore](https://techcommunity.microsoft.com/t5/Containers/What-s-new-for-container-identity/ba-p/389151) post di blog.

## <a name="add-task-scheduler-and-hyper-v-manager-to-server-core-installations"></a>Aggiungere l'utilità di pianificazione e gestione di Hyper-V per le installazioni Server Core

Come sapete, è consigliabile usare l'opzione di installazione Server Core, quando si usa Windows Server, come canale semestrale in fase di produzione. Server Core per impostazione predefinita, tuttavia, omette una serie di strumenti di gestione utile. È possibile aggiungere molti più comunemente usati strumenti installando la funzionalità di compatibilità dell'applicazione, ma sono ancora state alcuni strumenti mancante.

In base ai suggerimenti dei clienti, Aggiungemmo due ulteriori strumenti per la funzionalità di compatibilità dell'applicazione in questa versione: Utilità di pianificazione (Taskschd. msc) e gestione di Hyper-V (virtmgmt.msc).

Per altre informazioni, vedi [funzionalità di compatibilità di base del Server app](../get-started-19/install-fod-19.md).

## <a name="storage-migration-service-now-migrates-local-accounts-clusters-and-linux-servers"></a>Il servizio di migrazione di archiviazione ora esegue la migrazione di account locali, cluster e server Linux

Il servizio di migrazione di archiviazione rende più semplice la migrazione di server a una versione più recente di Windows Server. Fornisce uno strumento grafico che archivia i dati nei server, quindi trasferisce i dati e configurazione al server più recenti, tutto senza dover apportare alcuna modifica di utenti o App.

Quando si usa questa versione di Windows Server per orchestrare le migrazioni, sono state aggiunte le possibilità seguenti:

- Eseguire la migrazione di utenti e gruppi locali nel nuovo server
- La migrazione dell'archiviazione dal cluster di failover
- Eseguire la migrazione di archiviazione da un server Linux che usa Samba
- Migrazione di condivisioni di sincronizzazione più facilmente in Azure con sincronizzazione File di Azure
- Eseguire la migrazione a nuove reti, ad esempio Azure

Per altre informazioni sul servizio di migrazione di archiviazione, vedi [Panoramica del servizio di migrazione archiviazione](../storage/storage-migration-service/overview.md).

## <a name="system-insights-disk-anomaly-detection"></a>Rilevamento delle anomalie disco del sistema Insights

[Sistema Insights](../manage/system-insights/overview.md) è una funzione analitica predittiva che analizza i dati di sistema di Windows Server in locale e offre informazioni approfondite sul funzionamento del server. È dotato di numerose funzionalità incorporate, ma è stata aggiunta la possibilità per installare funzionalità aggiuntive tramite Windows Admin Center, inizia con il rilevamento delle anomalie del disco.

Rilevamento delle anomalie di disco è una nuova funzionalità che mette in evidenza quando i dischi si comportino *in modo diverso* superiore al consueto. Anche se diverse non è necessariamente negativa, visualizzare questi istanti anomale può essere utile durante la risoluzione dei problemi dei sistemi.

Questa funzionalità è disponibile anche per i server che eseguono Windows Server 2019.

## <a name="windows-admin-center-enhancements"></a>Miglioramenti di Windows Admin Center

Una nuova versione di Windows Admin Center non è più valido, l'aggiunta di nuove funzionalità a Windows Server. Per informazioni sulle funzionalità più recenti, vedi [Windows Admin Center](../manage/windows-admin-center/understand/windows-admin-center.md).

## <a name="security-baseline-for-windows-10-and-windows-server"></a>Baseline della sicurezza per Windows 10 e Windows Server

La versione bozza del [impostazioni di base di configurazione della sicurezza](https://blogs.technet.microsoft.com/secguide/2019/04/24/security-baseline-draft-for-windows-10-v1903-and-windows-server-v1903/) per Windows 10 versione 1903 e per Windows Server versione 1903 è disponibile.

## <a name="setupdiag"></a>SetupDiag
[SetupDiag](https://docs.microsoft.com/windows/deployment/upgrade/setupdiag) versione 1.4.1 è disponibile.

SetupDiag è uno strumento da riga di comando che consente di diagnosticare il motivo per cui un aggiornamento di Windows non è riuscito. SetupDiag funziona con la ricerca dei file di log dell'installazione di Windows. Durante la ricerca dei file di log, SetupDiag usa un set di regole per identificare problemi noti. Nella versione corrente di SetupDiag esistono 53 regole contenute nel file rules.xml, che viene estratto quando SetupDiag viene eseguito. Il file rules.xml viene aggiornato man mano che vengono rese disponibili le nuove versioni di SetupDiag.

## <a name="update-rollback-improvements"></a>Miglioramenti di rollback degli aggiornamenti

I server ora possono recuperare automaticamente da errori di avvio rimuovendo gli aggiornamenti se è stata introdotta l'errore di avvio dopo l'installazione degli aggiornamenti recenti di qualità o driver. Quando un dispositivo non riesce ad avviarsi correttamente dopo l'installazione recente di qualità di aggiornamenti dei driver, Windows ora disinstalleranno automaticamente gli aggiornamenti a portare il dispositivo di backup e viene eseguito normalmente.

Questa funzionalità richiede che il server di usare l'opzione di installazione Server Core con un [ambiente ripristino Windows](https://docs.microsoft.com/windows-hardware/manufacture/desktop/windows-recovery-environment--windows-re--technical-reference) partizione.

## <a name="microsoft-defender-advanced-threat-protection-atp-improvements"></a>Miglioramenti di Microsoft Defender Advanced Threat Protection (ATP)

Windows Server include Microsoft Defender Advanced Thread Protection (per altre informazioni, vedi [Windows Defender Antivirus nei Server Windows](https://docs.microsoft.com/windows/security/threat-protection/windows-defender-antivirus/windows-defender-antivirus-on-windows-server-2016)). Questa versione include i miglioramenti seguenti:

- [Riduzione della superficie di attacco](https://docs.microsoft.com/windows/security/threat-protection/windows-defender-atp/overview-attack-surface-reduction) – gli amministratori IT possono configurare i dispositivi con protezione avanzata di web che consente di definire consentire e negare elenchi per gli indirizzi IP e URL specifici.
- [Protezione di prossima generazione](https://docs.microsoft.com/windows/security/threat-protection/windows-defender-antivirus/windows-defender-antivirus-in-windows-10) – i controlli sono stati estesi per protezione da ransomware, usi impropri delle credenziali e gli attacchi che vengono trasmessi tramite Archivi rimovibili.
    - Funzionalità di imposizione di integrità – Enable remote runtime attestazione.
    - Funzionalità a prova di manomissione: sicurezza basata su virtualizzazione Usa per isolare funzionalità di sicurezza ATP critiche del sistema operativo e gli utenti malintenzionati.
- Tecnologie di protezione di nuova generazione Microsoft Defender ATP:
    - **Apprendimento avanzato**: Migliorata con avanzate di machine learning e i modelli di intelligenza artificiale per abilitare la protezione contro i pirati informatici vertice tramite malware, strumenti e tecniche di exploit di vulnerabilità innovative.
    - **Protezione di emergenza della diffusione**: Fornisce la protezione di emergenza della diffusione che aggiorna automaticamente i dispositivi con nuove informazioni quando viene rilevato un attacco di nuovo.
    - **Certificazione ISO 27001 conformità**: Garantisce che il servizio cloud ha analizzato le minacce, vulnerabilità e gli impatti e che vengono implementati controlli di sicurezza e la gestione dei rischi.
    - **Supporto di Georilevazione**: Supporto di georilevazione e la sovranità dei dati di esempio, nonché i criteri di conservazione configurabile.