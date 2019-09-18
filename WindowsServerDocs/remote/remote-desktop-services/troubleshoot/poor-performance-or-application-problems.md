---
title: Problemi di prestazioni insoddisfacenti o dell'applicazione durante la connessione Desktop remoto
description: Risoluzione dei problemi di prestazioni insoddisfacenti o dell'applicazione durante la connessione Desktop remoto.
audience: itpro
ms.custom: na
ms.reviewer: rklemen
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: troubleshooting
ms.assetid: ''
author: kaushika-msft
manager: ''
ms.author: delhan
ms.date: 07/24/2019
ms.localizationpriority: medium
ms.openlocfilehash: c65683a69633a950630b7fd74e1181da767ae35b
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 09/10/2019
ms.locfileid: "70870577"
---
# <a name="poor-performance-or-application-problems-during-remote-desktop-connection"></a>Problemi di prestazioni insoddisfacenti o dell'applicazione durante la connessione Desktop remoto

Questo articolo tratta diversi problemi comuni che gli utenti possono incontrare quando usano la funzionalità Desktop remoto.

### <a name="intermittent-problems-with-new-microsoft-azure-virtual-machines"></a>Problemi intermittenti con le nuove macchine virtuali di Microsoft Azure

Questo problema riguarda le macchine virtuali di cui è stato effettuato di recente il provisioning. Dopo che l'utente si è connesso alla macchina virtuale, la sessione Desktop remoto non carica correttamente tutte le sue impostazioni.

Per ovviare a questo problema, disconnettiti dalla macchina virtuale, attendi almeno 20 minuti e quindi connettiti di nuovo.

Per risolvere questo problema, applica gli aggiornamenti seguenti alle macchine virtuali a seconda dei casi:

  - Windows 10 e Windows Server 2016: KB 4343884, [30 agosto 2018 - KB4343884 (build sistema operativo 14393.2457)](https://support.microsoft.com/help/4343884/windows-10-update-kb4343884)
  - Windows Server 2012 R2: KB 4343891, [30 agosto 2018 - KB4343891 (anteprima del rollup mensile)](https://support.microsoft.com/help/4343891/windows-81-update-kb4343891)

### <a name="video-playback-issues-on-windows-10-version-1709"></a>Problemi di riproduzione video in Windows 10 versione 1709

Questo problema si verifica quando gli utenti si connettono a computer remoti che eseguono Windows 10, versione 1709. Se tali utenti riproducono video usando il codec VMR9 (Video Mixing Renderer 9), il lettore mostra solo una finestra nera.

Si tratta di un problema noto in Windows 10, versione1709. Il problema non si verifica in Windows 10, versione 1703.

### <a name="desktop-sharing-issues-on-windows-10"></a>Problemi di condivisione del desktop in Windows 10

Questo problema si verifica quando l'utente ha un profilo utente di sola lettura (e un hive del Registro di sistema associato), come nel caso di uno scenario con un chiosco multimediale. Se tale utente si connette a un computer remoto che esegue Windows 10, versione 1803, non riesce a condividere il proprio desktop.

Per risolvere questo problema, applica l'aggiornamento 4340917 di Windows 10, [24 luglio 2018 - KB4340917 (build sistema operativo 17134.191)](https://support.microsoft.com/help/4340917/windows-10-update-kb4340917).

### <a name="performance-issues-when-mixing-versions-of-windows-10-if-nla-is-disabled"></a>Problemi di prestazioni quando vengono usate insieme versioni diverse di Windows 10 se l'Autenticazione a livello di rete è disabilitata

Questo problema si verifica quando i computer client Desktop remoto che eseguono Windows 10 si connettono a desktop remoti che eseguono versioni diverse di Windows 10 mentre l'Autenticazione a livello di rete è disabilitata. Gli utenti dei client Desktop remoto nei computer che eseguono Windows 10, versione 1709 o precedenti, riscontrano scarse prestazioni quando si connettono a desktop remoti che eseguono Windows 10, versione 1803 o successive.

Ciò si verifica perché, se l'Autenticazione a livello di rete è disabilitata, i computer client meno recenti usano un protocollo più lento per connettersi a Windows 10 versione 1803 o a una versione successiva.

Per risolvere questo problema, applica l'aggiornamento KB 4340917, [24 luglio 2018 - KB4340917 (build sistema operativo 17134.191)](https://support.microsoft.com/help/4340917/windows-10-update-kb4340917).

### <a name="black-screen-issue"></a>Problema della schermata nera

Questo problema si verifica in Windows 8.0, Windows 8.1, Windows 10 RTM e Windows Server 2012 R2. Un utente avvia più applicazioni in un desktop remoto e quindi si disconnette dalla sessione. L'utente poi si riconnette periodicamente al desktop remoto per interagire con le applicazioni e quindi si disconnette di nuovo. A un certo punto, quando l'utente si riconnette, la sessione Desktop remoto mostra solo una schermata nera. Per fare in modo che la sessione venga di nuovo visualizzata correttamente, l'utente deve terminare la sessione dalla console del computer remoto o dalla console del server Host sessione Desktop remoto e arrestare le applicazioni della sessione.

Per risolvere questo problema, applica gli aggiornamenti seguenti a seconda dei casi:

  - Windows 8 e Windows Server 2012: KB 4103719, [17 maggio 2018 - KB4103719 (anteprima del rollup mensile)](https://support.microsoft.com/help/4103719/windows-server-2012-update-kb4103719)
  - Windows 8.1 e Windows Server 2012 R2: KB 4103724, [17 maggio 2018 - KB4103724 (anteprima del rollup mensile)](https://support.microsoft.com/help/4103724/windows-81-update-kb4103724) e KB 4284863, [21 giugno 2018 - KB4284863 (anteprima del rollup mensile)](https://support.microsoft.com/help/4284863/windows-81-update-kb4284863)
  - Windows 10: corretto in KB 4284860, [12 giugno 2018 - KB4284860 (build sistema operativo 10240.17889)](https://support.microsoft.com/help/4284860/windows-10-update-kb4284860)
