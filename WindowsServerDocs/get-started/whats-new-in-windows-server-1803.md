---
title: Novità di Windows Server, versione 1803
description: Quali sono le nuove funzionalità di calcolo, identità, gestione, automazione, rete, sicurezza e archiviazione.
ms.prod: windows-server
ms.technology: server-general
ms.topic: article
author: greg-lindsay
ms.author: greg-lindsay
ms.localizationpriority: high
ms.date: 05/07/2018
ms.openlocfilehash: 8b359ac883c24d559e2c3d47db5b68e4f5341338
ms.sourcegitcommit: 3a3d62f938322849f81ee9ec01186b3e7ab90fe0
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 04/23/2020
ms.locfileid: "80826004"
---
# <a name="whats-new-in-windows-server-version-1803"></a>Novità di Windows Server versione 1803

>Si applica a: Windows Server (Canale semestrale)

<img src=../media/landing-icons/new.png style='float:left; padding:.5em;' alt=Icon showing a newspaper>&nbsp;Per informazioni sulle funzionalità più recenti di Windows, vedi [Novità di Windows Server](whats-new-in-windows-server.md). Questa sezione illustra le funzionalità nuove e modificate di Windows Server, versione 1803. Le nuove funzionalità e modifiche elencate di seguito sono quelle che più probabilmente avranno il massimo impatto sull'uso di questa versione. Vedi anche l'articolo relativo all'[aggiornamento del Canale semestrale di Windows Server](https://cloudblogs.microsoft.com/windowsserver/2018/03/29/windows-server-semi-annual-channel-update/).

## <a name="windows-admin-center"></a>Windows Admin Center

Project Honolulu è ora **Windows Admin Center**.
<br>&nbsp;
> [!video https://www.youtube.com/embed/WCWxAp27ERk?autoplay=false]

[Windows Admin Center](https://docs.microsoft.com/windows-server/manage/windows-admin-center/overview) consolida tutti gli aspetti della gestione locale e remota del server. Windows Admin Center è un'esperienza di gestione distribuita in locale, basata su browser e utilizzabile anche in assenza di una connessione Internet, che offre un controllo completo di tutti gli aspetti della distribuzione di Windows Server.

## <a name="windows-server-release-strategy"></a>Strategia di rilascio di Windows Server

Windows Server, versione 1709, è stato rilasciato nel settembre 2017 come prima versione del Canale semestrale. Il Canale semestrale ha una frequenza di rilascio più elevata e accoglie il feedback degli utenti che vogliono vedere implementate innovazioni rapide a intervalli di pochi mesi. È complementare al Long-Term Servicing Channel, ovvero il canale di manutenzione a lungo termine, per il quale la frequenza di rilascio è di 2-3 anni.

In base al feedback e ai dati di telemetria raccolti, questi canali hanno dimostrato di rispondere adeguatamente alla seguente strategia generale:
- Il Canale semestrale è ideale per le applicazioni moderne e gli scenari di innovazione, ad esempio i contenitori e i microservizi.
- Il Long-Term Servicing Channel è la versione preferita per gli scenari di base dell'infrastruttura, ad esempio il data center software-defined e l'infrastruttura iperconvergente. 

Gli scenari specifici indicati per il Canale semestrale e il Long-Term Servicing Channel sono i seguenti:

|   | Long-Term Servicing Channel |  Canale semestrale |
| ------------- | ------------- | ------------ |
| Scenari consigliati     | File server per utilizzo generico, carichi di lavoro proprietari e di terze parti, app tradizionali, ruoli di infrastruttura, data center software-defined e infrastruttura iperconvergente  | Applicazioni in contenitori, host contenitore e scenari di applicazioni che traggono vantaggio dall'introduzione più rapida delle innovazioni |
| Nuove versioni  | Ogni 2-3 anni  | Ogni 6 mesi |
| Supporto  | 5 anni di supporto Mainstream + 5 anni di supporto Extended  | 18 mesi |
| Edizioni  | Tutte le edizioni di Windows Server disponibili  | Edizioni Standard e Datacenter |
| Destinatari  | Tutti i clienti attraverso tutti i canali | Solo clienti Software Assurance e cloud |
| Opzioni di installazione  | Server Core e Server con Esperienza desktop  | Server Core per host e immagine del contenitore e immagine del contenitore Nano Server |

## <a name="application-platform-and-containers"></a>Piattaforma e contenitori per le applicazioni

- Ottimizzazione
    - La dimensione dell'immagine di base del contenitore Server Core è stata ridotta del 30% rispetto a Windows Server, versione 1709. 
    - La compatibilità delle applicazioni è stata migliorata per supportare più efficacemente la containerizzazione delle applicazioni tradizionali.
    - Anche le prestazioni di avvio e in fase di esecuzione dei contenitori sono state migliorate tramite l'implementazione di varie correzioni e ottimizzazioni.
- Rete di contenitori: è stato aggiunto il supporto per localhost e proxy http e sono stati migliorati i tempi di avvio e la scalabilità dei contenitori.
- Strumenti: è stato migliorato il supporto per Curl.exe, Tar.exe e SSH a integrazione dell'utilizzo di PowerShell negli scenari di compilazione e di debug.

### <a name="server-core-container-image"></a>Immagine del contenitore Server Core

È ora disponibile un contenitore Server Core più piccolo e caratterizzato da una migliore compatibilità delle applicazioni. Informazioni dettagliate sono disponibili [qui](https://blogs.technet.microsoft.com/virtualization/2018/01/22/a-smaller-windows-server-core-container-with-better-application-compatibility/).

- Le funzionalità e i ruoli facoltativi inutilizzati sono stati rimossi. Per altre informazioni, vedi [Ruoli, servizi ruolo e funzionalità non inclusi nei contenitori Server Core](../administration/server-core/server-core-container-removed-roles.md).
    - La dimensione del download (1,58 GB) è stata ridotta del 30% rispetto a Windows Server, versione 1709.
    - La dimensione su disco (3,61 GB) è stata ridotta del 20% rispetto a Windows Server, versione 1709.
- L'immagine del contenitore Nano Server è inferiore a 100 MB.

### <a name="windows-subsystem-for-linux-wsl"></a>Sottosistema di Windows per Linux (WSL)

WSL consente agli amministratori di server di usare gli strumenti e gli script esistenti per Linux in Windows Server. Molti dei miglioramenti illustrati nel [blog dedicato alle funzionalità della riga di comando](https://blogs.msdn.microsoft.com/commandline/tag/wsl/), come le attività in background, DriveFS, WSLPath e molti altri ancora, sono stati estesi a Windows Server.

### <a name="kubernetes"></a>Kubernetes 

Kubernetes (comunemente denominato K8s) è un sistema open source sviluppato sotto la guida della [Cloud Native Computing Foundation](https://www.cncf.io), che consente di automatizzare la distribuzione, il ridimensionamento e la gestione delle applicazioni in contenitori. 

In Windows Server, versione 1709, gli utenti potevano sfruttare i vantaggi di Kubernetes per le funzionalità di rete di Windows, tra cui:
- Raggruppamenti di pod condivisi: i pod di lavoro e infrastruttura ora condividono un raggruppamento di rete (analogo a uno spazio dei nomi di Linux).
- Ottimizzazione dell'endpoint: grazie alla condivisione dei raggruppamenti, risulta almeno dimezzato il numero degli endpoint di cui i servizi contenitore devono tenere traccia.
- Ottimizzazione del percorso dati: i miglioramenti apportati a Virtual Filtering Platform e a Host Networking Service consentono il bilanciamento del carico basato su kernel.

Con il rilascio di Windows Server, versione 1803, saranno disponibili ulteriori funzionalità per le prossime versioni di Kubernetes: 
- [Plug-in di archiviazione](https://github.com/Microsoft/K8s-Storage-Plugins) per i contenitori di Windows orchestrati da Kubernetes.
- Reti a livello di cloud grazie a iniziative come la nostra partnership con [Tigera per il supporto di Project Calico](https://cloudblogs.microsoft.com/windowsserver/2017/12/07/securing-modernized-apps-and-simplified-networking-on-windows-with-calico/).
- Supporto della piattaforma Windows per i pod isolati tramite Hyper-V in scenari caratterizzati da più contenitori per pod.

### <a name="application-compatibility-and-feature-parity-issues-fixed"></a>Problemi di compatibilità delle applicazioni e di parità delle funzionalità risolti

- Microsoft Message Queuing (MSMQ) viene ora installato in un contenitore Server Core.
- È stato risolto un problema che interrompeva i contatori delle prestazioni di ASP.NET.
- È stato risolto un problema che impediva ai servizi in esecuzione nei contenitori di ricevere le notifiche di arresto.
    - In particolare, la funzionalità di notifica è stata impostata su CTRL_SHUTDOWN_EVENT sia per le immagini basate sul contenitore Server Core sia per quelle basate sul contenitore Nano Server. Inoltre, nelle immagini basate sul contenitore Server Core, la notifica, incluso l'invio delle notifiche di arresto del servizio, viene estesa a tutti i processi in esecuzione nel contenitore.
- È stato risolto un problema di incompatibilità di docker pull e docker load con l'impostazione dei criteri che determina se è richiesta la protezione con BitLocker per consentire la scrittura sulle unità dati fisse (FDVDenyWriteAccess). 

## <a name="storage"></a>Archiviazione

In questa versione, è possibile impedire la creazione di un journal delle modifiche (o journal USN) in tutti i volumi all'avvio del servizio Gestione risorse file server. Ciò consente di risparmiare spazio su ciascun volume, ma disabilita la classificazione dei file in tempo reale. Per altre informazioni, vedi [Panoramica di Gestione risorse file server](https://docs.microsoft.com/windows-server/storage/fsrm/fsrm-overview).

## <a name="features-added-to-server-core"></a>Funzionalità aggiunte a Server Core

A Server Core è stato aggiunto il ruolo Server di trasporto di Servizi di distribuzione Windows.

Server di trasporto contiene solo i componenti di rete di base di Servizi di distribuzione Windows. Puoi usare Server di trasporto per creare spazi dei nomi multicast per la trasmissione di dati (incluse le immagini del sistema operativo) da un server autonomo. Può inoltre esserti utile se vuoi disporre di un server PXE che consenta ai client l'avvio PXE e il download dell'applicazione di installazione personalizzata. È consigliabile usare questa opzione per uno di questi due scenari.

Per abilitare il servizio Server di trasporto in Server Core puoi usare il comando seguente di Windows PowerShell:

```
Install-WindowsFeature -Name WDS
```

## <a name="see-also"></a>Vedi anche

[Informazioni sulle versioni di Windows Server](https://docs.microsoft.com/windows-server/get-started/windows-server-release-info)<br>
[Novità nei contenuti per i professionisti IT di Windows 10, versione 1803](https://docs.microsoft.com/windows/whats-new/whats-new-windows-10-version-1803)
