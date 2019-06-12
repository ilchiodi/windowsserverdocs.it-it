---
title: Desktop remoto - consentire l'accesso al computer
description: Scopri le opzioni per accedere in remoto il PC
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
ms.openlocfilehash: d03dcd307696aea55ab6a1569ab907635994772a
ms.sourcegitcommit: d888e35f71801c1935620f38699dda11db7f7aad
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 06/07/2019
ms.locfileid: "66804996"
---
# <a name="remote-desktop---allow-access-to-your-pc"></a>Desktop remoto - consentire l'accesso al computer

>Si applica a: Windows 10, Windows 8.1, Windows Server 2019, Windows Server 2016, Windows Server 2012 R2

È possibile usare Desktop remoto per connettersi a e controllare il PC da un dispositivo remoto utilizzando un [client Desktop remoto Microsoft](remote-desktop-clients.md) (disponibile per Windows, iOS, macOS e Android). Se si consentono le connessioni remote al computer, è possibile utilizzare un altro dispositivo di connettersi al computer e avere accesso a tutte le app, i file e le risorse di rete come se si utilizzasse dalla propria scrivania.  

> [!NOTE]
> È possibile usare Desktop remoto per connettersi a Windows 10 Pro ed Enterprise, Windows 8.1 e 8 versioni Enterprise e Pro, Windows 7 Professional, Enterprise e Ultimate e Windows Server più recenti rispetto a Windows Server 2008. Non è possibile connettersi ai computer che esegue l'edizione Home (ad esempio Windows 10 Home). 

Per connettersi a un PC remoto, che computer deve essere acceso, deve avere una connessione di rete, è necessario abilitare Desktop remoto, è necessario avere accesso di rete al computer remoto (potrebbe trattarsi di tramite Internet), ed è necessario disporre dell'autorizzazione per connettersi. Per l'autorizzazione per la connessione, è necessario l'elenco degli utenti. Prima di avviare una connessione, è una buona idea per cercare il nome del computer a che ci si connette e assicurarsi che le connessioni Desktop remoto siano consentite attraverso il firewall.

## <a name="how-to-enable-remote-desktop"></a>Come abilitare Desktop remoto

Il modo più semplice per consentire l'accesso al PC da un dispositivo remoto è utilizzando le opzioni Desktop remoto in impostazioni. Poiché questa funzionalità è stata aggiunta in Windows 10 Fall Creators update (1709), un oggetto separato scaricabile app è disponibile anche per le versioni precedenti di Windows che offre funzionalità simili. È anche possibile usare la modalità legacy di abilitazione di Desktop remoto, tuttavia, questo metodo offre meno funzionalità e la convalida.

### <a name="windows-10-fall-creator-update-1709-or-later"></a>Windows 10 Fall Creators Update (1709) o versione successiva

È possibile configurare il PC per l'accesso remoto con pochi semplici passaggi.
1. Nel dispositivo si desidera connettersi, selezionare **avviare** e quindi scegliere il **impostazioni** icona a sinistra.
2. Selezionare il **System** gruppo aggiungendo il [ **Desktop remoto** ](ms-settings:remotedesktop) elemento.
3. Usare il dispositivo di scorrimento per abilitare Desktop remoto.
4. È anche consigliabile mantenere i PC attivi e individuabile per facilitare le connessioni. Fare clic su **Mostra le impostazioni** abilitare.
5. In base alle esigenze, aggiungere gli utenti possono connettersi in remoto facendo **selezionare gli utenti che possono accedere in remoto il PC**.
   1. I membri del gruppo Administrators hanno automaticamente accesso.
6. Prendere nota del nome di questo PC sotto **come connettersi a questo PC**. È necessario per configurare i client.

### <a name="windows-7-and-early-version-of-windows-10"></a>Versione anticipata di Windows 10 e Windows 7

Per configurare il PC per l'accesso remoto, scaricare ed eseguire la [Microsoft Remote Desktop Assistant](https://www.microsoft.com/download/details.aspx?id=50042). Questo assistente Aggiorna le impostazioni di sistema per abilitare l'accesso remoto, assicura il computer è attivo per le connessioni e controlla che il firewall consenta le connessioni Desktop remoto. 

### <a name="all-versions-of-windows-legacy-method"></a>Tutte le versioni di Windows (metodo Legacy)

Per abilitare Desktop remoto usando le proprietà di sistema legacy, seguire le istruzioni per [connettersi a un altro computer mediante connessione Desktop remoto](https://windows.microsoft.com/windows/remote-desktop-connection-faq).

## <a name="should-i-enable-remote-desktop"></a>È consigliabile abilitare Desktop remoto?

Se si vuole solo accedere al computer quando sono fisicamente una postazione, non è necessario abilitare Desktop remoto. Abilitazione di Desktop remoto consente di aprire una porta nel PC che è visibile alla rete locale. È solo necessario abilitare Desktop remoto in reti attendibili, ad esempio la home page. Non bisogna neppure abilitare Desktop remoto su tutti i computer in cui l'accesso viene strettamente controllato.

Tenere presente che quando si abilita l'accesso a Desktop remoto, si concede a tutti gli utenti del gruppo Administrators, oltre a eventuali altri utenti si seleziona, la possibilità di accedere in remoto i propri account del computer.

È necessario assicurarsi che ogni account che dispone dell'accesso al computer sia configurato con una password complessa.

## <a name="why-allow-connections-only-with-network-level-authentication"></a>Il motivo per cui consentire le connessioni solo con l'autenticazione a livello di rete? 

Se si desidera limitare l'accesso al computer, scegliere di consentire l'accesso solo con livello di autenticazione NLA (Network). Quando si abilita questa opzione, gli utenti devono autenticarsi su rete prima di potersi connettere al computer. Consentire le connessioni solo da computer che eseguono Desktop remoto con NLA è un metodo di autenticazione più sicuro che consente di proteggere il computer dal software e gli utenti malintenzionati. Per altre informazioni su NLA e Desktop remoto, estrarre [NLA configurare per le connessioni RDS](https://technet.microsoft.com/library/cc732713(v=ws.11).aspx).

Se ci si connette a un PC in remoto nella rete domestica all'esterno di tale rete, non selezionare questa opzione.
