---
title: Novità di Windows Server, versione 1903
description: Questo argomento descrive alcune delle nuove funzionalità di Windows Server versione 1903, un rilascio del Canale semestrale.
ms.prod: windows-server-threshold
ms.technology: server-general
ms.topic: article
author: jasongerend
ms.author: jgerend
ms.date: 05/21/2019
ms.openlocfilehash: 0ec6a7ec624818b92fb306089f3dea3c786c0827
ms.sourcegitcommit: afb0602767de64a76aaf9ce6a60d2f0e78efb78b
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 06/20/2019
ms.locfileid: "67280311"
---
# <a name="whats-new-in-windows-server-version-1903"></a>Novità di Windows Server, versione 1903

>Si applica a: Windows Server (Canale semestrale)

Questo argomento descrive alcune delle nuove funzionalità di Windows Server versione 1903, un rilascio del Canale semestrale. Queste funzionalità includono miglioramenti per l'esecuzione e la gestione di contenitori, strumenti per l'utilizzo nelle installazioni Server Core e la possibilità di eseguire la migrazione degli archivi da dispositivi Linux.

Per informazioni sulle novità della versione Long-Term Servicing Channel (LTSC) più recente di Windows Server, vedi invece [Novità di Windows Server 2019](../get-started-19/whats-new-19.md). Vedi anche [Novità nei contenuti per i professionisti IT di Windows 10 versione 1903](https://docs.microsoft.com/windows/whats-new/whats-new-windows-10-version-1903).

I requisiti di sistema per questa versione sono gli stessi di Windows Server 2019. Per altre informazioni, vedi [Requisiti di sistema](../get-started-19/sys-reqs-19.md). Per informazioni sulle funzionalità rimosse di recente, vedi [Funzionalità rimosse o pianificate per la sostituzione a partire da Windows Server, versione 1903](../get-started-19/removed-features-1903.md).

> [!NOTE]
> I contenitori di Windows devono usare la stessa versione di Windows del server host o una versione *precedente*. Ad esempio, un server host che esegue la versione rilasciata di Windows Server, versione 1903 (build 18342) può eseguire contenitori di Windows Server con versione e numero di build uguali o precedenti (anche se il contenitore usa una versione Insider Preview di Windows). Per altre informazioni, vedi [Compatibilità delle versioni dei contenitori di Windows](https://docs.microsoft.com/virtualization/windowscontainers/deploy-containers/version-compatibility).

## <a name="enhanced-support-for-non-microsoft-container-services"></a>Supporto avanzato per i servizi contenitore non Microsoft

Abbiamo migliorato le funzionalità di piattaforma per supportare i servizi contenitore di Azure e i servizi contenitore non Microsoft.

- Abbiamo integrato contenitori di elementi del report personalizzati con Servizio di elaborazione host (HCS, Host Compute Service) per supportare in Azure pod di contenitori di Windows e Linux in Windows.
- Abbiamo collaborato con la community Kubernetes per abilitare il supporto dei contenitori di Windows. Con la versione di Kubernetes v1.14, il supporto dei nodi Windows Server è passato ufficialmente dalla versione beta alla versione stabile. Per altre informazioni, vedi [Contenitori di Windows ora supportati in Kubernetes](https://cloudblogs.microsoft.com/opensource/2019/03/25/windows-server-containers-now-supported-kubernetes/).
- Tigera Calico per Windows è ora disponibile a livello generale come parte della sottoscrizione Tigera Essentials e offre una rete non di sovrapposizione e criteri di rete interoperativi tra ambienti Linux/Windows misti.
- Abbiamo implementato miglioramenti della scalabilità ottimizzando il supporto della rete di sovrapposizione per i contenitori di Windows, inclusa l'integrazione con Kubernetes nella versione più recente di Flannel e Kubernetes v1.14. Per altre informazioni, vedi [Intro to Windows support in Kubernetes](https://kubernetes.io/docs/setup/windows/) (Introduzione al supporto Windows in Kubernetes).

## <a name="directx-hardware-acceleration-in-containers"></a>Accelerazione hardware DirectX nei contenitori

Stiamo abilitando il supporto per l'accelerazione hardware di API DirectX in contenitori di Windows per supportare scenari come l'inferenza di Machine Learning (ML) con hardware di unità di elaborazione grafica (GPU). Per altre informazioni, vedi [Implementazione dell'accelerazione GPU nei contenitori di Windows](https://techcommunity.microsoft.com/t5/Containers/Bringing-GPU-acceleration-to-Windows-containers/ba-p/393939).

## <a name="updated-container-identity-and-group-managed-service-account-documentation"></a>Documentazione aggiornata sull'identità del contenitore e sull'account del servizio gestito di gruppo

Abbiamo aggiunto altri esempi e informazioni di compatibilità nella documentazione sugli [account del servizio gestito di gruppo](https://docs.microsoft.com/virtualization/windowscontainers/manage-containers/manage-serviceaccounts) e abbiamo reso disponibile il [modulo di PowerShell Credential Spec](https://www.powershellgallery.com/packages/CredentialSpec) in PowerShell Gallery. Per altre informazioni, vedi il post del blog [Novità per l'identità contenitore](https://techcommunity.microsoft.com/t5/Containers/What-s-new-for-container-identity/ba-p/389151).

## <a name="add-task-scheduler-and-hyper-v-manager-to-server-core-installations"></a>Aggiungere Utilità di pianificazione e Console di gestione di Hyper-V alle installazioni Server Core

Come già saprai, è consigliabile usare l'opzione di installazione Server Core quando usi Windows Server, Canale semestrale in fase di produzione. Server Core omette tuttavia per impostazione predefinita una serie di strumenti di gestione utili. Puoi aggiungere molti degli strumenti più comuni installando la funzionalità di compatibilità app, ma continueranno a mancare ancora alcuni strumenti.

In base al feedback dei clienti, abbiamo aggiunto quindi altri due strumenti alla funzionalità di compatibilità app in questa versione: Utilità di pianificazione (taskschd.msc) e Console di gestione di Hyper-V (virtmgmt.msc).

Per altre informazioni, vedi [Funzionalità di compatibilità app Server Core](../get-started-19/install-fod-19.md).

## <a name="storage-migration-service-now-migrates-local-accounts-clusters-and-linux-servers"></a>Il servizio di migrazione archiviazione esegue ora la migrazione di account locali, cluster e server Linux

Il servizio di migrazione archiviazione semplifica la migrazione dei server a una versione più recente di Windows Server. Fornisce uno strumento grafico che archivia i dati nei server, quindi trasferisce i dati e la configurazione in server più recenti, tutto senza che le app o gli utenti debbano apportare alcuna modifica.

Per l'uso di questa versione di Windows Server per orchestrare le migrazioni, sono state aggiunte le funzionalità seguenti:

- Migrazione di utenti e gruppi locali al nuovo server
- Migrazione degli archivi dai cluster di failover
- Migrazione degli archivi da un server Linux che usa Samba
- Sincronizzazione semplificata tramite Sincronizzazione file di Azure delle condivisioni sottoposte a migrazione
- Migrazione a nuove reti, ad esempio Azure

Per altre informazioni sul servizio di migrazione archiviazione, vedi [Panoramica del servizio di migrazione archiviazione](../storage/storage-migration-service/overview.md).

## <a name="system-insights-disk-anomaly-detection"></a>Rilevamento delle anomalie dei dischi tramite informazioni dettagliate di sistema

[Informazioni dettagliate di sistema](../manage/system-insights/overview.md) è una funzionalità analitica predittiva che analizza i dati di sistema di Windows Server in locale e offre informazioni approfondite sul funzionamento del server. È dotata di numerose funzionalità incorporate, ma abbiamo aggiunto la possibilità di installare funzionalità aggiuntive tramite Windows Admin Center, a partire dal rilevamento delle anomalie dei dischi.

Il rilevamento delle anomalie dei dischi è una nuova funzionalità che indica quando i dischi si comportano *in modo diverso* rispetto alla normalità. Anche se un comportamento diverso non ha necessariamente una connotazione negativa, può essere utile esaminare questi momenti di anomalia nel corso della risoluzione dei problemi dei sistemi.

Questa funzionalità è disponibile anche per i server che eseguono Windows Server 2019.

## <a name="windows-admin-center-enhancements"></a>Miglioramenti di Windows Admin Center

È disponibile una nuova versione di Windows Admin Center, che aggiunge nuove funzionalità a Windows Server. Per informazioni sulle funzionalità più recenti, vedi [Windows Admin Center](../manage/windows-admin-center/understand/windows-admin-center.md).

## <a name="security-baseline-for-windows-10-and-windows-server"></a>Linea di base per la sicurezza per Windows 10 e Windows Server

È disponibile la versione bozza delle [impostazioni di base di configurazione della sicurezza](https://blogs.technet.microsoft.com/secguide/2019/04/24/security-baseline-draft-for-windows-10-v1903-and-windows-server-v1903/) per Windows 10 versione 1903 e per Windows Server versione 1903.

## <a name="setupdiag"></a>SetupDiag
È disponibile [SetupDiag](https://docs.microsoft.com/windows/deployment/upgrade/setupdiag) versione 1.4.1.

SetupDiag è uno strumento da riga di comando che permette di diagnosticare il motivo per cui un aggiornamento di Windows non è riuscito. SetupDiag funziona eseguendo la ricerca nei file di log dell'installazione di Windows. Durante la ricerca nei file di log, SetupDiag usa un set di regole per identificare problemi noti. Nella versione corrente di SetupDiag sono incluse 53 regole contenute nel file rules.xml che viene estratto durante l'esecuzione di SetupDiag. Il file rules.xml viene aggiornato man mano che vengono rese disponibili nuove versioni di SetupDiag.

## <a name="update-rollback-improvements"></a>Miglioramenti del ripristino dello stato precedente agli aggiornamenti

I server possono ora essere ripristinati automaticamente da errori di avvio tramite la rimozione di aggiornamenti se l'errore di avvio è stato introdotto dopo l'installazione di aggiornamenti qualitativi o di driver recenti. Se si verificano problemi di avvio di un dispositivo dopo l'installazione recente di aggiornamenti qualitativi dei driver, Windows ora disinstalla automaticamente gli aggiornamenti per ripristinare lo stato operativo normale del dispositivo.

Per questa funzionalità è necessario che il server usi l'opzione di installazione Server Core con una partizione [Ambiente ripristino Windows](https://docs.microsoft.com/windows-hardware/manufacture/desktop/windows-recovery-environment--windows-re--technical-reference).

## <a name="microsoft-defender-advanced-threat-protection-atp-improvements"></a>Miglioramenti di Microsoft Defender Advanced Threat Protection (ATP)

Windows Server include Microsoft Defender Advanced Thread Protection (per altre informazioni, vedi [Windows Defender Antivirus in Windows Server](https://docs.microsoft.com/windows/security/threat-protection/windows-defender-antivirus/windows-defender-antivirus-on-windows-server-2016)). Questa versione include i miglioramenti seguenti:

- [Riduzione della superficie di attacco](https://docs.microsoft.com/windows/security/threat-protection/windows-defender-atp/overview-attack-surface-reduction): gli amministratori IT possono configurare i dispositivi con una protezione Web avanzata che permette di definire elenchi di filtri di indirizzi IP e URL specifici consentiti o non consentiti.
- [Protezione di nuova generazione](https://docs.microsoft.com/windows/security/threat-protection/windows-defender-antivirus/windows-defender-antivirus-in-windows-10): i controlli sono stati estesi per la protezione da ransomware, da usi impropri delle credenziali e da attacchi che vengono trasmessi tramite risorse di archiviazione rimovibili.
    - Funzionalità di imposizione dei criteri di integrità: è possibile abilitare l'attestazione di runtime remota.
    - Funzionalità a prova di manomissione: viene usata la sicurezza basata sulla virtualizzazione per isolare funzionalità di sicurezza ATP critiche dal sistema operativo e da utenti malintenzionati.
- Tecnologie di protezione di nuova generazione Microsoft Defender ATP:
    - **Machine Learning avanzato**: abbiamo implementato miglioramenti con Machine Learning avanzato e modelli di intelligenza artificiale che abilitano la protezione da utenti malintenzionati apex che usano malware, strumenti e tecniche di exploit di vulnerabilità innovative.
    - **Protezione di emergenza da attacchi**: fornisce la protezione di emergenza da attacchi che comporta l'aggiornamento automatico dei dispositivi con nuove informazioni quando viene rilevato un nuovo attacco.
    - **Conformità con la certificazione ISO 27001**: garantisce che il servizio cloud abbia eseguito l'analisi per ricercare eventuali minacce, vulnerabilità e impatti e che siano implementati controlli di sicurezza e la gestione dei rischi.
    - **Supporto della georilevazione**: viene fornito il supporto della georilevazione e la sovranità dei dati di esempio, nonché criteri di conservazione configurabili.