---
title: Desktop remoto - consentire l'accesso al PC
description: Scopri le opzioni per l'accesso remoto ai PC
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: d3c85c1c765583eb26cef60ecd245708e0e02772
ms.sourcegitcommit: d3f73936160505a40633ad8dd5931ac5fe3eccdb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/12/2019
ms.locfileid: "9297220"
---
# Desktop remoto - consentire l'accesso al PC

>Si applica a: Windows 10, Windows 8.1, Windows Server 2019, Windows Server 2016, Windows Server 2012 R2

È possibile utilizzare Desktop remoto per connettersi e controllare il PC da un dispositivo remoto usando un [client Desktop remoto Microsoft](remote-desktop-clients.md) (disponibile per Windows, iOS, macOS e Android). Quando si Consenti connessioni remote al PC, è possibile utilizzare un altro dispositivo di connettersi al PC e avere accesso a tutte le app, i file e risorse di rete come se trovasse in ufficio.  

> [!NOTE]
> È possibile utilizzare Desktop remoto per connettersi a Windows 10 Pro e modello, Windows 8.1 e 8 versioni Enterprise e Pro, Windows 7 Professional, Enterprise e Ultimate e Windows Server più recente di Windows Server 2008. Non riesce a connettersi al computer che eseguono l'edizione Home (ad esempio Windows 10 Home). 

Per connetterti a un PC remoto, che deve essere attivato computer, deve disporre di una connessione di rete, Desktop remoto deve essere abilitato, deve avere accesso alla rete al computer remoto (che potrebbe essere tramite Internet) e devono disporre dell'autorizzazione per la connessione. Le autorizzazioni per la connessione, è necessario essere nell'elenco degli utenti. Prima di iniziare una connessione, è consigliabile per cercare il nome del computer che si connette e per assicurarti che le connessioni Desktop remoto sono consentite attraverso il firewall.

## Come abilitare Desktop remoto

Il modo più semplice per consentire l'accesso al PC da un dispositivo remoto Usa le opzioni di Desktop remoto in impostazioni. Dato che questa funzionalità sono state aggiunte in Windows 10 Fall Creators update (1709), separato scaricabile app è disponibile anche per le versioni precedenti di Windows che fornisce una funzionalità simile. Puoi anche usare il modo legacy di attivazione di Desktop remoto, tuttavia, questo metodo offre funzionalità di convalida.

### Windows 10 Fall Creators Update (1709) o versione successiva

È possibile configurare il PC per l'accesso remoto con pochi semplici passaggi.
1. Nel dispositivo si desidera connettersi per, selezionare **Avvia** quindi fare clic sull'icona delle **Impostazioni** a sinistra.
2. Seleziona il gruppo di **sistema** seguito dall'elemento di [**Desktop remoto**](ms-settings:remotedesktop) .
3. Usare il dispositivo di scorrimento per abilitare Desktop remoto.
4. È inoltre consigliabile per garantire il PC attivo e l'intuitività per facilitare le connessioni. Fai clic su **Mostra le impostazioni** per abilitare.
5. In base alle esigenze, aggiungere gli utenti possono connettersi in remoto, fai clic su **Seleziona gli utenti che possono accedere in remoto a questo PC**.
   1. I membri del gruppo Administrators hanno automaticamente l'accesso.
6. Prendere nota del nome di questo PC in **come connettersi a questo PC**. È necessario per configurare i client.

### Windows 7 e versione preliminare di Windows 10

Per configurare il PC per l'accesso remoto, Scarica ed Esegui [l'Assistente Desktop remoto di Microsoft](https://www.microsoft.com/download/details.aspx?id=50042). Questo assistente Aggiorna le impostazioni di sistema per abilitare l'accesso remoto, garantisce che il computer è attivo per le connessioni e controlla che il firewall consenta le connessioni Desktop remoto. 

### Tutte le versioni di Windows (metodo Legacy)

Per abilitare Desktop remoto usando le proprietà di sistema legacy, segui le istruzioni per [connettersi a un altro computer mediante connessione Desktop remoto](https://windows.microsoft.com/windows/remote-desktop-connection-faq).

## Devo abilitare Desktop remoto?

Se vuoi solo accedere al PC, quando sono fisicamente una postazione, non dovrai attivare Desktop remoto. L'abilitazione di Desktop remoto, si apre una porta nel PC che è visibile alla rete locale. È necessario abilitare solo Desktop remoto nelle reti attendibili, ad esempio casa. Anche non vuoi abilitare Desktop remoto in qualsiasi PC in cui l'accesso è strettamente controllato.

Tieni presente che quando si abilita l'accesso a Desktop remoto, l'utente concede tutti gli utenti nel gruppo di amministratori, nonché eventuali altri utenti selezioni, la possibilità di accedere ai propri account nel computer in remoto.

È necessario assicurarsi che ogni account che ha accesso al PC è configurato con una password complessa.

## Il motivo per cui Consenti connessioni solo con autenticazione a livello di rete? 
 
Se vuoi limitare l'accesso al PC, scegliere di consentire l'accesso solo con livello di autenticazione NLA (Network). Quando si abilita questa opzione, gli utenti devono autenticarsi alla rete prima che possa connettersi al PC. Consentire le connessioni solo dai computer che eseguono Desktop remoto con NLA è un metodo di autenticazione più sicuro che consenta di proteggere il computer da utenti malintenzionati e software. Per ulteriori informazioni sulle NLA e Desktop remoto, consulta [Configurare NLA per le connessioni di servizi desktop remoto](https://technet.microsoft.com/library/cc732713(v=ws.11).aspx). 

Se ci si connette a un PC in modalità remota della rete domestica di fuori di tale rete, non selezionate questa opzione.
