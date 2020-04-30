---
ms.assetid: 231158d8-5e81-4630-b8d5-93fee16e0cd3
title: Identificazione dell'aggiornamento del livello funzionale
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: 02620fdce6132fa3868207ffe2afbf6c95e6a37b
ms.sourcegitcommit: 11421f4005f9f3a3f6c0db95b1836d0f765a9fa3
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2020
ms.locfileid: "81624199"
---
# <a name="identifying-your-functional-level-upgrade"></a>Identificazione dell'aggiornamento del livello funzionale

> Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Prima di poter aumentare i livelli di funzionalità di domini e foreste, è necessario valutare l'ambiente corrente e identificare il requisito del livello di funzionalità che meglio soddisfa le esigenze dell'organizzazione. Valutare l'ambiente corrente identificando i domini nella foresta, i controller di dominio che si trovano in ogni dominio, il sistema operativo e i Service Pack in esecuzione in ogni controller di dominio e la data in cui si intende aggiornare i controller di dominio. Se si prevede di ritirare un controller di dominio, assicurarsi di comprendere l'impatto completo che avrà sull'ambiente in uso.

Le circostanze seguenti potrebbero impedire l'aggiornamento di una versione precedente del sistema operativo Windows Server al livello di funzionalità di Windows Server 2008 o Windows Server 2008 R2:

- Hardware insufficiente

- Un controller di dominio che esegue un programma antivirus non compatibile con Windows Server 2008 o Windows Server 2008 R2

- Uso di un programma specifico della versione che non viene eseguito in Windows Server 2008 o Windows Server 2008 R2

- La necessità di aggiornare un programma con la Service Pack più recente

La documentazione di queste informazioni può essere utile per identificare i passaggi da eseguire per assicurarsi di disporre di un ambiente Windows Server 2008 o Windows Server 2008 R2 completamente funzionante.

Dopo aver valutato l'ambiente corrente, è necessario identificare l'aggiornamento del livello funzionale che si applica all'organizzazione. Sono disponibili le opzioni seguenti:

- Ambiente in modalità nativa di Windows 2000 per Windows Server 2008 o Windows Server 2008 R2

- Foresta Windows Server 2003 a Windows Server 2008 o Windows Server 2008 R2

- Nuova foresta Windows Server 2008

- Nuova foresta Windows Server 2008 R2

## <a name="upgrading-functional-levels-in-a-native-windows-2000-active-directory-forest"></a>Aggiornamento di livelli di funzionalità in una foresta nativa di Windows 2000 Active Directory
In un ambiente nativo Windows 2000 costituito solo da controller di dominio basati su Windows 2000, i livelli di funzionalità vengono impostati per impostazione predefinita sui livelli seguenti e rimangono a questi livelli fino a quando non vengono aumentati manualmente:

- Livello di funzionalità del dominio nativo di Windows 2000

- Livello di funzionalità della foresta di Windows 2000

Per utilizzare tutte le funzionalità a livello di foresta e di dominio in Windows Server 2008 o Windows Server 2008 R2, è necessario eseguire l'aggiornamento di questo ambiente Windows 2000 a Windows Server 2008 o Windows Server 2008 R2. È possibile eseguire l'aggiornamento in uno dei modi seguenti:

- Introdurre nella foresta i controller di dominio basati su Windows Server 2008 o Windows Server 2008 R2 appena installati, quindi disattivare tutti i controller di dominio che eseguono Windows 2000.

- Eseguire un aggiornamento sul posto di tutti i controller di dominio esistenti che eseguono Windows 2000 nella foresta ai controller di dominio che eseguono Windows Server 2003. Eseguire quindi un aggiornamento sul posto di tali controller di dominio a Windows Server 2008 o Windows Server 2008 R2. Per ulteriori informazioni, vedere [l'aggiornamento di domini Active Directory per Windows Server 2008 e Windows Server 2008 R2 AD DS domini](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc731188(v=ws.10)).

    > [!IMPORTANT]
    >  Windows Server 2008 R2 è un sistema operativo basato su x64. Se nel server è in esecuzione una versione basata su x64 di Windows Server 2003, è possibile eseguire correttamente un aggiornamento sul posto del sistema operativo del computer a Windows Server 2008 R2. Se nel server è in esecuzione una versione basata su x86 di Windows Server 2003, non è possibile eseguire l'aggiornamento di questo computer a Windows Server 2008 R2.

Per utilizzare le funzionalità a livello di dominio di Windows Server 2008 o Windows Server 2008 R2 senza aggiornare l'intera foresta di Windows 2000 a Windows Server 2008 o Windows Server 2008 R2, elevare solo il livello di funzionalità del dominio a Windows Server 2008 o Windows Server 2008 R2.

> [!NOTE]
> Prima di aumentare il livello di funzionalità del dominio, è necessario aggiornare tutti i controller di dominio basati su Windows 2000 in tale dominio a Windows Server 2008 o Windows Server 2008 R2.

Dopo aver sostituito tutti i controller di dominio basati su Windows 2000 nella foresta con controller di dominio che eseguono Windows Server 2008 o Windows Server 2008 R2, è possibile aumentare il livello di funzionalità della foresta a Windows Server 2008 o Windows Server 2008 R2. In questo modo, viene generato automaticamente il livello di funzionalità di tutti i domini della foresta impostati su Windows 2000 native o versione successiva a Windows Server 2008 o Windows Server 2008 R2.

Per ulteriori informazioni sull'aumento dei livelli di funzionalità della foresta e del dominio e sulle procedure per l'esecuzione di tali attività, vedere [distribuzione di un dominio radice della foresta Windows Server 2008](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-r2-and-2008/cc731174(v=ws.10)).

## <a name="upgrading-functional-levels-in-a-windows-server-2003-active-directory-forest"></a>Aggiornamento di livelli di funzionalità in una foresta di Active Directory Windows Server 2003
In un ambiente Windows Server 2003 costituito solo da controller di dominio basati su Windows Server 2003, i livelli di funzionalità sono impostati per impostazione predefinita sui livelli seguenti e rimangono a questi livelli fino a quando non vengono aumentati manualmente:

- Livello di funzionalità del dominio nativo di Windows 2000

- Livello di funzionalità della foresta di Windows 2000

Per utilizzare tutte le funzionalità a livello di foresta e di dominio in Windows Server 2008 o Windows Server 2008 R2, è necessario eseguire l'aggiornamento di questo ambiente Windows Server 2003 a Windows Server 2008 o Windows Server 2008 R2. È possibile eseguire l'aggiornamento in uno dei modi seguenti:

- Introdurre un controller di dominio basato su Windows Server 2008 o Windows Server 2008 R2 appena installato nella foresta, quindi disattivare tutti i controller di dominio che eseguono Windows Server 2003 o aggiornarli a Windows Server 2008 o Windows Server 2008 R2.

- Eseguire un aggiornamento sul posto di tutti i controller di dominio esistenti che eseguono Windows Server 2003 ai controller di dominio che eseguono Windows Server 2008 o Windows Server 2008 R2. Per ulteriori informazioni, vedere [l'aggiornamento di domini Active Directory per Windows Server 2008 e Windows Server 2008 R2 AD DS domini](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc731188(v=ws.10)).

> [!IMPORTANT]
> Windows Server 2008 R2 è un sistema operativo basato su x64. Se nel server è in esecuzione una versione basata su x64 di Windows Server 2003, è possibile eseguire correttamente un aggiornamento sul posto del sistema operativo del computer a Windows Server 2008 R2. Se nel server è in esecuzione una versione x86 di Windows Server 2003, non è possibile aggiornare questo computer per eseguire Windows Server 2008 R2.

Per usare tutte le funzionalità a livello di dominio di Windows Server 2008 o Windows Server 2008 R2 senza aggiornare l'intera foresta di Windows Server 2003 a Windows Server 2008 o Windows Server 2008 R2, elevare solo il livello di funzionalità del dominio a Windows Server 2008 o Windows Server 2008 R2.

> [!NOTE]
> Prima di aumentare il livello di funzionalità del dominio, è necessario aggiornare tutti i controller di dominio basati su Windows Server 2003 in tale dominio a Windows Server 2008 o Windows Server 2008 R2.

Dopo l'aggiornamento di tutti i controller di dominio basati su Windows Server 2003 nella foresta a Windows Server 2008 o Windows Server 2008 R2, è possibile aumentare il livello di funzionalità della foresta a Windows Server 2008 o Windows Server 2008 R2. In questo modo, viene generato automaticamente il livello di funzionalità di tutti i domini della foresta impostati su Windows Server 2003 a Windows Server 2008 o Windows Server 2008 R2.

Per ulteriori informazioni sull'aumento dei livelli di funzionalità della foresta e del dominio e sulle procedure per l'esecuzione di tali attività, vedere [distribuzione di un dominio radice della foresta Windows Server 2008](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-r2-and-2008/cc731174(v=ws.10)).

## <a name="upgrading-functional-levels-in-a-new-windows-server-2008-forest"></a>Aggiornamento di livelli di funzionalità in una nuova foresta Windows Server 2008
Quando si installa il primo controller di dominio in una nuova foresta Windows Server 2008, i livelli di funzionalità vengono impostati per impostazione predefinita sui livelli seguenti e rimangono a questi livelli fino a quando non vengono aumentati manualmente:

- Livello di funzionalità del dominio nativo di Windows 2000

- Livello di funzionalità della foresta di Windows 2000

I livelli di funzionalità sono impostati su questi livelli predefiniti per offrire la possibilità di aggiungere controller di dominio basati su Windows 2000 o Windows Server 2003 alla nuova foresta Windows Server 2008. Dopo aver creato un dominio radice della foresta, il livello di funzionalità del dominio per ogni dominio aggiunto alla foresta Windows Server 2008 viene impostato su Windows 2000 native. Tuttavia, se si desidera che tutti i controller di dominio nel nuovo ambiente Windows Server 2008 eseguano Windows Server 2008, impostare il livello di funzionalità della foresta e quindi il livello di funzionalità del dominio su Windows Server 2008 quando si installa il primo controller di dominio nella foresta. Questa operazione consente di risparmiare tempo e di abilitare tutte le funzionalità a livello di foresta e di dominio in Windows Server 2008.

> [!IMPORTANT]
> Se la foresta opera a livello di funzionalità di Windows Server 2008 e si tenta di installare Active Directory in un server membro basato su Windows Server 2003 o in un server membro basato su Windows 2000, l'installazione non riesce.

Per ulteriori informazioni sull'aumento dei livelli di funzionalità della foresta e del dominio e sulle procedure per l'esecuzione di tali attività, vedere [distribuzione di un dominio radice della foresta Windows Server 2008](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-r2-and-2008/cc731174(v=ws.10)).

## <a name="upgrading-functional-levels-in-a-new-windows-server-2008-r2-forest"></a>Aggiornamento di livelli di funzionalità in una nuova foresta di Windows Server 2008 R2
Quando si installa il primo controller di dominio in una nuova foresta di Windows Server 2008 R2, i livelli di funzionalità vengono impostati per impostazione predefinita sui livelli seguenti e rimangono a questi livelli fino a quando non vengono aumentati manualmente:

- Livello di funzionalità del dominio di Windows Server 2003

- Livello di funzionalità della foresta di Windows Server 2003

I livelli di funzionalità sono impostati su questi livelli predefiniti per offrire la possibilità di aggiungere controller di dominio basati su Windows Server 2003 alla nuova foresta Windows Server 2008 R2. Dopo aver creato un dominio radice della foresta, il livello di funzionalità del dominio per ogni dominio aggiunto alla foresta Windows Server 2008 R2 è impostato su Windows Server 2003. Tuttavia, se si desidera che tutti i controller di dominio nel nuovo ambiente Windows Server 2008 R2 eseguano Windows Server 2008 R2, impostare il livello di funzionalità della foresta e quindi il livello di funzionalità del dominio su Windows Server 2008 R2 quando si installa il primo controller di dominio nella foresta. Questa operazione consente di risparmiare tempo e di abilitare tutte le funzionalità a livello di foresta e di dominio in Windows Server 2008 R2.

> [!IMPORTANT]
> Se la foresta opera al livello di funzionalità di Windows Server 2008 R2 e si tenta di installare Active Directory in un server membro basato su Windows Server 2008 o Windows Server 2003 oppure su un server membro basato su Windows 2000, l'installazione non riesce.

Per ulteriori informazioni sull'aumento dei livelli di funzionalità della foresta e del dominio e sulle procedure per l'esecuzione di tali attività, vedere [distribuzione di un dominio radice della foresta Windows Server 2008](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-r2-and-2008/cc731174(v=ws.10)).

> [!NOTE]
> Sebbene sia necessario installare la versione 3.1 in Windows Server 2008, è possibile usare la versione 3.1 per eseguire la migrazione degli oggetti a un dominio ospitato da uno o più controller di dominio Windows Server 2008 R2. Per ulteriori informazioni, vedere l'articolo 976659 della Microsoft Knowledge base, [problemi noti che possono verificarsi quando si usa il valore di pro3,1 per eseguire la migrazione a un dominio che contiene controller di dominio di Windows Server 2008 R2](https://support.microsoft.com/help/976659/).
