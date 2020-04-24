---
title: Configurazioni supportate per Servizi Desktop remoto
description: Fornisce informazioni sulle configurazioni supportate per Servizi Desktop remoto in Windows Server 2016 e Windows Server 2019.
ms.prod: windows-server
ms.technology: remote-desktop-services
ms.author: elizapo
ms.date: 10/22/2019
ms.topic: article
ms.assetid: c925c7eb-6880-411f-8e59-bd0f57cc5fc3
author: lizap
manager: dongill
ms.openlocfilehash: dae6c00bd09e9c10e32932701095244a75f9ca7a
ms.sourcegitcommit: 3a3d62f938322849f81ee9ec01186b3e7ab90fe0
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 04/23/2020
ms.locfileid: "80860114"
---
# <a name="supported-configurations-for-remote-desktop-services"></a>Configurazioni supportate per Servizi Desktop remoto

> Si applica a: Windows Server 2016, Windows Server 2019

Quando si parla di configurazioni supportate per gli ambienti di Servizi Desktop remoto, la preoccupazione principale riguarda l'interoperabilità tra le versioni. La maggior parte degli ambienti include più versioni di Windows Server, ad esempio potresti avere una distribuzione di Servizi Desktop remoto Windows Server 2012 R2 ma voler eseguire l'aggiornamento a Windows Server 2016 per sfruttare i vantaggi delle nuove funzionalità (come il supporto per OpenGL\OpenCL, la funzionalità di assegnazione discreta di dispositivi o Spazi di archiviazione diretta). È quindi necessario capire quali componenti di Servizi Desktop remoto funzionano con diverse versioni e quali richiedono la stessa versione.

Tenendo presente questo aspetto, di seguito vengono fornite le indicazioni di base per le configurazioni supportate di Servizi Desktop remoto in Windows Server.

> [!NOTE]
> Verifica i [requisiti di sistema per Windows Server 2016](../../get-started/system-requirements.md) e i [requisiti di sistema per Windows Server 2019](../../get-started-19/sys-reqs-19.md).

## <a name="best-practices"></a>Procedure consigliate

- Usa Windows Server 2019 per l'infrastruttura di Desktop remoto (Accesso Web, Gateway, Gestore connessione e server licenze). Windows Server 2019 è compatibile con le versioni precedenti di questi componenti. Questo significa che un host sessione Desktop remoto di Windows Server 2016 o Windows Server 2012 R2 è in grado di connettersi a un'istanza di Gestore connessione Desktop remoto 2019, ma non viceversa.

- Per Host sessione Desktop remoto: tutti gli host sessione in un insieme devono essere allo stesso livello, ma è possibile avere più insiemi. Puoi avere una raccolta con host sessione Windows Server 2016 e una con host sessione Windows Server 2019.

- Se aggiorni l'host sessione Desktop remoto a Windows Server 2019, esegui anche l'aggiornamento del server licenze. Tieni presente che un server licenze 2019 può elaborare le licenze CAL di tutte le versioni precedenti di Windows Server fino a Windows Server 2003.

- Segui l'ordine di aggiornamento consigliato in [Aggiornamento dell'ambiente di Servizi Desktop remoto](upgrade-to-rds.md#flow-for-deployment-upgrades).

- Se stai creando un ambiente a disponibilità elevata, tutte le istanze di Gestore connessione devono essere allo stesso livello per quanto riguarda il sistema operativo.

## <a name="rd-connection-brokers"></a>Istanze di Gestore connessione Desktop remoto

In Windows Server 2016 non è più previsto un limite per il numero di istanze di Gestore connessione presenti in una distribuzione quando anche le istanze di Host sessione Desktop remoto e Host di virtualizzazione Desktop remoto eseguono Windows Server 2016. La tabella seguente indica le versioni dei componenti di Servizi Desktop remoto che funzionano con le versioni 2016 e 2012 R2 di Gestore connessione in una distribuzione a disponibilità elevata con tre o più istanze di Gestore connessione.

|3+ istanze di Gestore connessione a disponibilità elevata|RDSH o RDVH 2019|RDSH o RDVH 2016|RDSH o RDVH 2012 R2|
|---|---|---|---|
 |Gestore connessione Windows Server 2019|Supportato|Supportato|Supportato|
 |Gestore connessione Windows Server 2016|N/D|Supportato|Supportato|
 |Gestore connessione Windows Server 2012 R2|N/D|N/D|Non supportato|

## <a name="support-for-graphics-processing-unit-gpu-acceleration"></a>Supporto per l'accelerazione GPU

Servizi Desktop remoto supporta sistemi dotati di GPU. Le applicazioni che richiedono una GPU possono essere usate tramite la connessione remota. Inoltre, è possibile abilitare il rendering e la codifica con accelerazione GPU per migliorare le prestazioni e la scalabilità delle app.

Gli host sessione di Servizi Desktop remoto e i sistemi operativi client a singola sessione possono sfruttare i vantaggi offerti da GPU fisiche o virtuali presentate al sistema operativo in molti modi, incluse le [dimensioni delle macchine virtuali ottimizzate per la GPU di Azure](/en-us/azure/virtual-machines/windows/sizes-gpu), da GPU disponibili per il server RDSH fisico, da vGPU RemoteFX (solo in Windows Server 2016) e da GPU presentate alle macchine virtuali da hypervisor supportati.

Vedi [Qual è la tecnologia per la virtualizzazione della grafica più adatta?](rds-graphics-virtualization.md) per informazioni sui requisiti. Per informazioni specifiche sulla funzionalità di assegnazione discreta di dispositivi, vedi [Pianificare la distribuzione della funzionalità di assegnazione discreta di dispositivi](../../virtualization/hyper-v/plan/plan-for-deploying-devices-using-discrete-device-assignment.md).

I fornitori di GPU possono avere uno schema di gestione delle licenze separato per gli scenari RDSH o limitare l'uso della GPU nel sistema operativo server. Verifica i requisiti con il tuo fornitore preferito.

Le GPU presentate da una piattaforma cloud o un hypervisor non Microsoft devono avere driver firmati digitalmente da WHQL e distribuiti dal fornitore della GPU.

### <a name="remote-desktop-session-host-support-for-gpus"></a>Supporto dell'host sessione Desktop remoto per le GPU

La tabella seguente illustra gli scenari supportati da versioni diverse di host RDSH.

|Funzionalità|Windows Server 2008 R2|Windows Server 2012 R2|Windows Server 2016|Windows Server 2019|
|---|---|---|---|---|
|Uso della GPU hardware per tutte le sessioni RDP|No|Sì|Sì|Sì|
|Codifica hardware H. 264/AVC (se supportata dalla GPU)|No|No|Sì|Sì|
|Bilanciamento del carico tra più GPU presentate al sistema operativo|No|No|No|Sì|
|Ottimizzazioni della codifica H. 264/AVC per ridurre l'utilizzo della larghezza di banda|No|No|No|Sì|
|Supporto H. 264/AVC per la risoluzione 4K|No|No|No|Sì|

### <a name="vdi-support-for-gpus"></a>Supporto VDI per le GPU

La tabella seguente illustra il supporto per gli scenari GPU nel sistema operativo client.

|Funzionalità|Windows 7 SP1|Windows 8.1|Windows 10|
|---|---|---|---|
|Uso della GPU hardware per tutte le sessioni RDP|No|Sì|Sì|
|Codifica hardware H. 264/AVC (se supportata dalla GPU)|No|No|Windows 10 1703 e versioni successive|
|Bilanciamento del carico tra più GPU presentate al sistema operativo|No|No|Windows 10 1803 e versioni successive|
|Ottimizzazioni della codifica H. 264/AVC per ridurre l'utilizzo della larghezza di banda|No|No|Windows 10 1803 e versioni successive|
|Supporto H. 264/AVC per la risoluzione 4K|No|No|Windows 10 1803 e versioni successive|

### <a name="remotefx-3d-video-adapter-vgpu-support"></a>Supporto della scheda video 3D RemoteFX (vGPU)

Servizi Desktop remoto supporta le vGPU RemoteFX quando la macchina virtuale viene eseguita come guest Hyper-V in Windows Server 2012 R2 o Windows Server 2016. I sistemi operativi guest seguenti includono il supporto per la vGPU RemoteFX:

- Windows 7 SP1
- Windows 8.1
- Windows 10 1703 o versione successiva
- Windows Server 2016 solo in una distribuzione a sessione singola
- Windows Server 2019 solo in una distribuzione a sessione singola

### <a name="discrete-device-assignment-support"></a>Supporto per la funzionalità di assegnazione di dispositivi discreti

Servizi Desktop remoto supporta GPU fisiche presentate con la funzionalità di assegnazione di dispositivi discreti da host Hyper-V di Windows Server 2016 o Windows Server 2019. Per altre informazioni, vedi [Pianificare la distribuzione dell'assegnazione di dispositivi discreti](../../virtualization/hyper-v/plan/plan-for-deploying-devices-using-discrete-device-assignment.md).


## <a name="vdi-deployment--supported-guest-oses"></a>Distribuzione VDI: sistemi operativi guest supportati

I server Host di virtualizzazione Desktop remoto di Windows Server 2016 e Windows Server 2019 supportano i sistemi operativi guest seguenti:

- Windows 10 Enterprise
- Windows 8.1 Enterprise
- Windows 7 SP1 Enterprise

> [!NOTE]
> - Servizi Desktop remoto non supporta raccolte di sessioni eterogenee. Tutte le macchine virtuali di una raccolta devono avere la stessa versione del sistema operativo.
> - È possibile configurare raccolte omogenee separate con le versioni del sistema operativo guest diversi nello stesso host.
> - La versione dell'host Hyper-V usato per eseguire le macchine virtuali deve essere uguale a quella dell'host Hyper-V usato per creare i modelli di macchina virtuale originali.

## <a name="single-sign-on"></a>Accesso Single Sign-On

Servizi Desktop remoto di Windows Server 2016 e Windows Server 2019 supporta due esperienze SSO principali:

- Nell'app (applicazione Desktop remoto in Windows, iOS, Android e Mac)
- Web SSO

Usando l'applicazione Desktop remoto, puoi archiviare le credenziali come parte delle informazioni di connessione ([Mac](clients/remote-desktop-mac.md)) o come parte degli account gestiti ([iOS](clients/remote-desktop-ios.md#manage-your-user-accounts), [Android](clients/remote-desktop-android.md#manage-your-user-accounts), Windows) in modo sicuro tramite i meccanismi specifici di ogni sistema operativo.

Per connetterti a desktop e RemoteApp con SSO tramite il client Connessione Desktop remoto della Posta in arrivo in Windows, devi connetterti alla pagina Web Desktop remoto tramite Internet Explorer. Sono necessarie le opzioni di configurazione seguenti sul lato server. Per l'accesso SSO Web non sono supportate altre configurazioni:

- Web desktop remoto impostato per l'autenticazione basata su moduli (impostazione predefinita)
- Gateway desktop remoto impostato per l'autenticazione della password (impostazione predefinita)
- Distribuzione di Servizi desktop remoto impostata su "Usa credenziali Gateway Desktop remoto per computer remoti" (impostazione predefinita) nelle proprietà di Gateway Desktop remoto

> [!NOTE]
> A causa delle opzioni di configurazione richieste, l'accesso SSO Web non è supportato con le smart card. Gli utenti che accedono tramite smart card potrebbero visualizzare diverse richieste di accesso.

Per altre informazioni sulla creazione di una distribuzione VDI di Servizi Desktop remoto, vedi [Configurazioni di sicurezza di Windows 10 supportate per l'infrastruttura VDI di Servizi Desktop remoto](rds-vdi-supported-config.md).

## <a name="using-remote-desktop-services-with-application-proxy-services"></a>Uso di Servizi Desktop remoto con servizi proxy dell'applicazione

È possibile usare Servizi Desktop remoto, eccetto il client Web, con [Azure AD Application Proxy](https://docs.microsoft.com/azure/active-directory/application-proxy-publish-remote-desktop). Servizi Desktop remoto non supporta l'uso di [Proxy applicazione Web](https://docs.microsoft.com/windows-server/remote/remote-access/web-application-proxy/web-application-proxy-windows-server), incluso in Windows Server 2016 e nelle versioni precedenti.
