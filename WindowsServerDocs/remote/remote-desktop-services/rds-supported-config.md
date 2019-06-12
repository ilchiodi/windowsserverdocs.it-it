---
title: Configurazioni supportate per Servizi Desktop remoto
description: Vengono fornite informazioni sulle configurazioni supportate per Servizi Desktop remoto in Windows Server 2016.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.author: elizapo
ms.date: 12/20/2018
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: c925c7eb-6880-411f-8e59-bd0f57cc5fc3
author: lizap
manager: dongill
ms.openlocfilehash: 8571c2220f804a27e4e1a6b744e8e15e38bd53a3
ms.sourcegitcommit: 48bb3e5c179dc520fa879b16c9afe09e07c87629
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/31/2019
ms.locfileid: "66453082"
---
# <a name="supported-configurations-for-remote-desktop-services-in-windows-server-2016"></a>Configurazioni supportate per Servizi Desktop remoto in Windows Server 2016

> Si applica a: Windows Server 2016

Quando si parla di configurazioni supportate per gli ambienti di Servizi Desktop remoto, il problema più grande tende a essere l'interoperabilità di versione. La maggior parte degli ambienti sono installate più versioni di Windows Server: ad esempio, si potrebbe dispone di una distribuzione di Windows Server 2012 R2 Servizi Desktop remoto esistente ma vuole eseguire l'aggiornamento a Windows Server 2016 per sfruttare i vantaggi delle nuove funzionalità (ad esempio, il supporto per OpenGL\OpenCL, valori discreti Assegnazione di dispositivi o spazi di archiviazione diretta). Il problema diventa quindi, i componenti Servizi Desktop remoto possono usare versioni diverse e che richiedono sia lo stesso?

Quindi, tenendo presente, di seguito sono linee guida fondamentali per le configurazioni supportate di Servizi Desktop remoto in Windows Server 2016.

> [!NOTE]
> Assicurarsi di consultare il [requisiti di sistema per Windows Server 2016](../../get-started/system-requirements.md).

## <a name="best-practices"></a>Procedure consigliate
- Usare Windows Server 2016 per l'infrastruttura di Desktop remoto - l'accesso Web, Gateway, Broker di connessione e server licenze. Windows Server 2016 è compatibile con le versioni precedenti per questi componenti, in modo che un Host sessione Desktop remoto R2 2012 può connettersi a un gestore connessione desktop remoto di 2016, ma non è vero il contrario.

- Per gli host sessione Desktop remoto - tutti gli host della sessione in una raccolta devono essere dello stesso livello, ma è possibile avere più raccolte. È possibile avere una raccolta con gli host della sessione di Windows Server 2012 R2 e uno con gli host della sessione di Windows Server 2016.

- Se si aggiorna Host sessione Desktop remoto per Windows Server 2016, è necessario aggiornare anche il server licenze. Tenere presente che un server licenze 2016 può elaborare le licenze CAL da tutte le versioni precedenti di Windows Server, fino a Windows Server 2003.

- Seguire l'ordine di aggiornamento consigliata nelle [l'aggiornamento dell'ambiente di Servizi Desktop remoto](upgrade-to-rds.md#flow-for-deployment-upgrades). 

- Se si sta creando un ambiente a disponibilità elevata, tutti i gestori connessione devono essere dello stesso livello del sistema operativo.

## <a name="rd-connection-brokers"></a>Gestori connessione desktop remoto

Quando si usano gli host sessione Desktop remoto (RDSH) e Remote Desktop Virtualization Hosts (RDVH) che eseguono anche Windows Server 2016, Windows Server 2016 rimossa la restrizione per il numero di gestori di connessione è possibile avere in una distribuzione. Nella tabella seguente sono indicate le versioni di lavoro di componenti Servizi Desktop remoto con le versioni R2 2016 e 2012 di gestore di connessione in una distribuzione a disponibilità elevata con tre o più gestori di connessione.

| Gestori connessione 3 + in a disponibilità elevata              | RDSH 2016 | RDVH 2016 | RDSH 2012 R2  | RDVH 2012 R2  |
|------------------------------------------|-----------|-----------|---------------|---------------|
| Gestore di connessione di Windows Server 2016    | Supportato | Supportato | Supportato     | Supportato     |
| Gestore di connessione di Windows Server 2012 R2 | N/D       | N/D       | Supportato     | Supportato     |

## <a name="support-for-gpu-acceleration-with-hyper-v"></a>Supporto per l'accelerazione GPU con tecnologia Hyper-V
La tabella seguente illustra il supporto per l'accelerazione GPU nelle macchine virtuali. Visualizzare [la tecnologia di virtualizzazione della grafica è adatta a te?](rds-graphics-virtualization.md) di aiuto per capire cosa è necessario. Per informazioni specifiche su DDA, consultare [pianificare la distribuzione discreti dispositivo assegnazione](../../virtualization/hyper-v/plan/plan-for-deploying-devices-using-discrete-device-assignment.md).

|Sistema operativo guest della macchina virtuale  |Windows Server 2012 R2 o Windows Server 2016<br> GPU virtualizzata RemoteFX Hyper-V (generazione 1 macchina virtuale) |  Windows Server 2016 Hyper-V RemoteFX vGPU (generazione 2 macchina virtuale) |  Assegnazione dispositivo discreti di Windows Server 2016 Hyper-V (generazione 2 macchina virtuale) |
|-----------------------------|------------------------------------------------------------|--------------------------------------------------------|---------------------------------------------------------------------|
| Windows 7 SP1               | Yes                                                        | No                                                     | No                                                                  |
| Windows 8.1                 | Yes                                                        | No                                                     | No                                                                  |
| Windows 10 1511 Update      | Yes                                                        | Yes                                                    | Yes                                                                 |
| Windows Server 2012 R2      | Yes                                                        | No                                                     | Sì (richiede 3133690 KB)                                           |
| Windows Server 2016         | Yes                                                        | Yes                                                    | Yes                                                                 |
| Windows Server 2012 R2 RDSH | No                                                         | No                                                     | Sì (richiede 3133690 KB)                                           |
| Windows Server 2016 RDSH    | No                                                         | No                                                     | Yes                                                                 |
## <a name="vdi-deployment--supported-guest-oss"></a>Distribuzione di un'infrastruttura VDI, sistemi operativi guest supportati 
Server Host di virtualizzazione Desktop remoto di Windows Server 2016 supporta i sistemi operativi guest seguenti:

- Windows 10 Enterprise
- Windows 8.1 Enterprise 
- Windows 8 Enterprise 
- Windows 7 SP1 Enterprise 

Nella tabella seguente mostra le combinazioni di sistemi operativi guest supportati i sistemi operativi host di virtualizzazione Desktop remoto:

| Versione del sistema operativo RDVH        | Versione del sistema operativo guest           |
| ------------- |-------------|
| Windows Server 2016      | Windows 7 SP1, Windows 8, Windows 8.1, Windows 10 |
| Windows Server 2012 R2   | Windows 7 SP1, Windows 8, Windows 8.1, Windows 10 |
| Windows Server 2012      | Windows 7 SP1, Windows 8, Windows 8.1 |

> [!NOTE]  
> - Servizi Desktop remoto di Windows Server 2016 non supporta raccolte eterogenee. Tutte le macchine virtuali in una raccolta devono essere stessa versione del sistema operativo. 
> - È possibile configurare raccolte omogenee separate con le versioni del sistema operativo guest diversi nello stesso host. 
> - Modelli di macchina Virtuale devono essere creati in un host Windows Server 2016 Hyper-V a utilizzato come sistema operativo guest in un host Windows Server 2016 Hyper-V.

## <a name="single-sign-on-sso"></a>Single Sign-On (SSO)
Servizi Desktop remoto di Windows Server 2016 supporta due esperienze SSO principale:

 - All'interno dell'applicazione (applicazione Desktop remoto in Windows, iOS, Android e Mac)
 - Web SSO
 
Uso dell'applicazione Desktop remoto, è possibile archiviare come parte delle informazioni di connessione delle credenziali ([Mac](clients/remote-desktop-mac.md)) o come parte di account gestiti ([iOS](clients/remote-desktop-ios.md#manage-your-user-accounts), [Android](clients/remote-desktop-android.md#manage-your-user-accounts), Windows) in modo sicuro tramite i meccanismi univoci per ogni sistema operativo.

Per connettersi a desktop e RemoteApp con SSO tramite il client connessione Desktop remoto della posta in arrivo in Windows, è necessario connettersi alla pagina Web desktop remoto tramite Internet Explorer. Sono necessarie le seguenti opzioni di configurazione sul lato server. Nessun altre configurazioni sono supportate per l'accesso SSO Web:

 - Web desktop remoto è impostato per l'autenticazione basata su form (impostazione predefinita)
 - Gateway Desktop remoto è impostato su autenticazione della Password (impostazione predefinita)
 - Distribuzione di servizi desktop remoto è impostato su "Credenziali di Gateway Desktop remoto di utilizzo per i computer remoti" (predefinito) nelle proprietà del Gateway Desktop remoto

> [!NOTE]
> A causa di opzioni di configurazione necessarie, Web SSO non è supportato con smart card. Utenti che hanno accesso tramite smart card può verificarsi più richieste per eseguire l'accesso.

Per altre informazioni sulla creazione di distribuzione VDI di Servizi Desktop remoto, estrarre [configurazioni di sicurezza di Windows 10 è supportato per l'infrastruttura VDI di Servizi Desktop remoto](rds-vdi-supported-config.md).

## <a name="using-remote-desktop-services-with-application-proxy-services"></a>Uso di Servizi Desktop remoto con servizi proxy dell'applicazione

È possibile usare Servizi Desktop remoto, eccetto il client web, con [Azure AD Application Proxy](https://docs.microsoft.com/azure/active-directory/application-proxy-publish-remote-desktop). Servizi Desktop remoto non supporta l'utilizzo [Proxy applicazione Web](https://docs.microsoft.com/windows-server/remote/remote-access/web-application-proxy/web-application-proxy-windows-server), incluso in Windows Server 2016 e versioni precedenti.
