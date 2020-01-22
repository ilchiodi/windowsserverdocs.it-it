---
title: Desktop remoto - Consentire l'accesso al PC
description: Informazioni sulle opzioni per accedere in remoto al PC
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 0f1557ed-53f7-4333-b023-c8e0f4b58bf4
author: lizap
manager: dongill
ms.author: elizapo
ms.date: 06/05/2018
ms.localizationpriority: medium
ms.openlocfilehash: de08101ed1d4d4527242778d657778f1a16b3dad
ms.sourcegitcommit: 5b055fc1d73375f68149c214152f1d63396dd6ca
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 01/17/2020
ms.locfileid: "76248403"
---
# <a name="remote-desktop---allow-access-to-your-pc"></a>Desktop remoto - Consentire l'accesso al PC

>Si applica a: Windows 10, Windows 8.1, Windows Server 2019, Windows Server 2016, Windows Server 2012 R2

Puoi usare Desktop remoto per connetterti al tuo PC e controllarlo da un dispositivo remoto tramite un [client Desktop remoto Microsoft](remote-desktop-clients.md) (disponibile per Windows, iOS, macOS e Android). Quando consenti connessioni remote al tuo PC, puoi usare un altro dispositivo per connetterti al PC e avere accesso a tutte le app, tutti i file e tutte le risorse di rete disponibili come se fossi seduto alla scrivania.  

> [!NOTE]
> Puoi usare Desktop remoto per effettuare la connessione a Windows 10 Pro ed Enteprise, a Windows 8.1 e 8 Enterprise e Pro, a Windows 7 Professional, Enterprise e Ultimate, nonché alle versioni di Windows Server successive a Windows Server 2008. Non puoi connetterti a computer che eseguono l'edizione Home, ad esempio Windows 10 Home. 

Affinché la connessione a un PC remoto sia possibile, tale computer deve essere acceso, deve avere una connessione di rete e Desktop remoto deve essere abilitato. Devi inoltre disporre dell'accesso in rete al computer remoto (ad esempio, tramite Internet) e devi essere autorizzato a connetterti. Per essere autorizzato, devi essere incluso nell'elenco degli utenti che dispongono dell'autorizzazione per la connessione. Prima di avviare una connessione, è opportuno cercare il nome del computer a cui intendi connetterti e verificare che le connessioni Desktop remoto siano consentite attraverso il relativo firewall.

## <a name="how-to-enable-remote-desktop"></a>Come abilitare Desktop remoto

Il modo più semplice per consentire l'accesso al tuo PC da un dispositivo remoto è quello di usare le opzioni Desktop remoto presenti in Impostazioni. Da quando questa funzionalità è stata aggiunta in Windows 10 Fall Creators Update (1709), è disponibile anche un'app separata e scaricabile che offre funzionalità analoghe per le versioni precedenti di Windows. Puoi abilitare Desktop remoto anche usando il metodo legacy, ma in questo modo le funzionalità e la convalida sono ridotte.

### <a name="windows-10-fall-creator-update-1709-or-later"></a>Windows 10 Fall Creators Update (1709) o versioni successive

Per configurare il tuo PC per l'accesso remoto ti bastano pochi passaggi di facile esecuzione.
1. Nel dispositivo a cui vuoi connetterti seleziona **Start** e quindi fai clic sull'icona **Impostazioni** a sinistra.
2. Seleziona il gruppo **Sistema** e quindi la voce [**Desktop remoto**](ms-settings:remotedesktop).
3. Usa il dispositivo di scorrimento per abilitare Desktop remoto.
4. È anche consigliabile mantenere il PC attivo e individuabile per facilitare le connessioni. Per abilitare l'apposita impostazione, fai clic su **Mostra impostazioni**.
5. Secondo le necessità, aggiungi gli utenti che possono connettersi in remoto facendo clic su **Select users that can remotely access this PC** (Seleziona gli utenti che possono accedere in remoto al PC).
   1. I membri del gruppo Administrators hanno accesso automaticamente.
6. Prendi nota del nome del PC visualizzato in **How to connect to this PC** (Come connettersi al PC). Tale nome sarà necessario per configurare i client.

### <a name="windows-7-and-early-version-of-windows-10"></a>Windows 7 e versioni precedenti di Windows 10

Per configurare il PC per l'accesso remoto, scarica ed esegui [Microsoft Remote Desktop Assistant](https://www.microsoft.com/download/details.aspx?id=50042) (Assistente Desktop remoto Microsoft). Tale assistente aggiorna le impostazioni di sistema in modo da abilitare l'accesso remoto, verifica che il computer sia attivo per le connessioni e controlla che il firewall consenta le connessioni Desktop remoto. 

### <a name="all-versions-of-windows-legacy-method"></a>Tutte le versioni di Windows (metodo legacy)

Per abilitare Desktop remoto usando le proprietà di sistema legacy, segui le istruzioni contenute in [Connettersi a un altro computer mediante Connessione Desktop remoto](https://windows.microsoft.com/windows/remote-desktop-connection-faq).

## <a name="should-i-enable-remote-desktop"></a>Quando abilitare Desktop remoto

Se vuoi accedere al tuo PC solo quando usi il dispositivo fisicamente, non hai necessità di abilitare Desktop remoto. Abilitando Desktop remoto, apri una porta del PC che è visibile alla rete locale. Dovresti pertanto abilitare Desktop remoto esclusivamente in reti affidabili, come ad esempio quella domestica. Evita inoltre di abilitare Desktop remoto in PC in cui l'accesso è strettamente controllato.

Tieni presente che, abilitando l'accesso a Desktop remoto, concedi a tutti i membri del gruppo Administrators (e a tutti gli altri utenti che selezioni) la possibilità di accedere in remoto ai rispettivi account sul computer.

Assicurati che tutti gli account che hanno accesso al tuo PC siano configurati con una password complessa.

## <a name="why-allow-connections-only-with-network-level-authentication"></a>Perché consentire le connessioni solo con l'Autenticazione a livello di rete 

Se vuoi limitare chi può accedere al tuo PC, scegli di consentire l'accesso solo con l'Autenticazione a livello di rete. Quando abiliti questa opzione, gli utenti devono autenticarsi sulla rete prima di potersi connettere al tuo PC. Consentire le connessioni solo da computer che eseguono Desktop remoto con l'Autenticazione a livello di rete costituisce un metodo di autenticazione più sicuro che può contribuire a proteggere il tuo computer da utenti malintenzionati e da software dannoso. Per altre informazioni sull'Autenticazione a livello di rete e su Desktop remoto, vedi [Configurare l'Autenticazione a livello di rete per le connessioni Servizi Desktop remoto](https://technet.microsoft.com/library/cc732713(v=ws.11).aspx).

Se ti connetti in remoto a un PC della tua rete domestica dall'esterno di tale rete, non selezionare questa opzione.
