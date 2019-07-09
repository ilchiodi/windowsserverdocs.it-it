---
title: Configurazioni supportate per Servizi Desktop remoto
description: Informazioni sulle configurazioni supportate per Servizi Desktop remoto in Windows Server 2016.
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
ms.sourcegitcommit: 3743cf691a984e1d140a04d50924a3a0a19c3e5c
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 06/17/2019
ms.locfileid: "66453082"
---
# <a name="supported-configurations-for-remote-desktop-services-in-windows-server-2016"></a>Configurazioni supportate per Servizi Desktop remoto in Windows Server 2016

> Si applica a: Windows Server 2016

Quando si parla di configurazioni supportate per gli ambienti di Servizi Desktop remoto, la preoccupazione principale riguarda l'interoperabilità tra le versioni. La maggior parte degli ambienti include più versioni di Windows Server, ad esempio potresti avere una distribuzione di Servizi Desktop remoto Windows Server 2012 R2 ma voler eseguire l'aggiornamento a Windows Server 2016 per sfruttare i vantaggi delle nuove funzionalità (come il supporto per OpenGL\OpenCL, la funzionalità di assegnazione discreta di dispositivi o Spazi di archiviazione diretta). È quindi necessario capire quali componenti di Servizi Desktop remoto funzionano con diverse versioni e quali richiedono la stessa versione.

Tenendo presente questo aspetto, di seguito vengono fornite le linee guida di base per le configurazioni supportate di Servizi Desktop remoto in Windows Server 2016.

> [!NOTE]
> Esamina anche i [requisiti di sistema per Windows Server 2016](../../get-started/system-requirements.md).

## <a name="best-practices"></a>Procedure consigliate
- Usa Windows Server 2016 per l'infrastruttura di Desktop remoto: Accesso Web, Gateway, Gestore connessione e server licenze. Windows Server 2016 è compatibile con le versioni precedenti per questi componenti, quindi un'istanza di Host sessione Desktop remoto 2012 R2 può connettersi a Gestore connessione Desktop remoto 2016, mentre non vale il contrario.

- Per Host sessione Desktop remoto: tutti gli host sessione in un insieme devono essere allo stesso livello, ma è possibile avere più insiemi. Può esserci un insieme di host sessione Windows Server 2012 R2 e uno di host sessione Windows Server 2016.

- Se aggiorni Host sessione Desktop remoto a Windows Server 2016, esegui l'aggiornamento anche del server licenze. Tieni presente che un server licenze 2016 può elaborare le licenze CAL di tutte le versioni precedenti di Windows Server, fino a Windows Server 2003.

- Segui l'ordine di aggiornamento consigliato in [Aggiornamento dell'ambiente di Servizi Desktop remoto](upgrade-to-rds.md#flow-for-deployment-upgrades). 

- Se stai creando un ambiente a disponibilità elevata, tutte le istanze di Gestore connessione devono essere allo stesso livello per quanto riguarda il sistema operativo.

## <a name="rd-connection-brokers"></a>Istanze di Gestore connessione Desktop remoto

In Windows Server 2016 non è più previsto un limite per il numero di istanze di Gestore connessione presenti in una distribuzione quando anche le istanze di Host sessione Desktop remoto e Host di virtualizzazione Desktop remoto eseguono Windows Server 2016. La tabella seguente indica le versioni dei componenti di Servizi Desktop remoto che funzionano con le versioni 2016 e 2012 R2 di Gestore connessione in una distribuzione a disponibilità elevata con tre o più istanze di Gestore connessione.

| 3+ istanze di Gestore connessione a disponibilità elevata              | Host sessione Desktop remoto 2016 | Host di virtualizzazione Desktop remoto 2016 | Host sessione Desktop remoto 2012 R2  | Host di virtualizzazione Desktop remoto 2012 R2  |
|------------------------------------------|-----------|-----------|---------------|---------------|
| Gestore connessione Windows Server 2016    | Supportato | Supportato | Supportato     | Supportato     |
| Gestore connessione Windows Server 2012 R2 | N/D       | N/D       | Supportato     | Supportato     |

## <a name="support-for-gpu-acceleration-with-hyper-v"></a>Supporto per l'accelerazione GPU con Hyper-V
La tabella seguente illustra il supporto per l'accelerazione GPU nelle macchine virtuali. Vedi [Qual è la tecnologia per la virtualizzazione della grafica più adatta?](rds-graphics-virtualization.md) per informazioni sui requisiti. Per informazioni specifiche sulla funzionalità di assegnazione discreta di dispositivi, vedi [Pianificare la distribuzione della funzionalità di assegnazione discreta di dispositivi](../../virtualization/hyper-v/plan/plan-for-deploying-devices-using-discrete-device-assignment.md).

|Sistema operativo guest macchina virtuale  |Windows Server 2012 R2 o Windows Server 2016<br> vGPU Hyper-V RemoteFX (macchina virtuale di prima generazione) |  vGPU Hyper-V RemoteFX Windows Server 2016 (macchina virtuale di seconda generazione) |  Funzionalità di assegnazione discreta di dispositivi Hyper-V Windows Server 2016 (macchina virtuale di seconda generazione) |
|-----------------------------|------------------------------------------------------------|--------------------------------------------------------|---------------------------------------------------------------------|
| Windows 7 SP1               | Sì                                                        | No                                                     | No                                                                  |
| Windows 8.1                 | Sì                                                        | No                                                     | No                                                                  |
| Windows 10 aggiornamento 1511      | Sì                                                        | Sì                                                    | Sì                                                                 |
| Windows Server 2012 R2      | Sì                                                        | No                                                     | Sì (richiede KB 3133690)                                           |
| Windows Server 2016         | Sì                                                        | Sì                                                    | Sì                                                                 |
| Host sessione Desktop remoto Windows Server 2012 R2 | No                                                         | No                                                     | Sì (richiede KB 3133690)                                           |
| Host sessione Desktop remoto Windows Server 2016    | No                                                         | No                                                     | Sì                                                                 |
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
Servizi Desktop remoto Windows Server 2016 supporta due esperienze SSO principali:

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
