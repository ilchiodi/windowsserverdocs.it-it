---
ms.assetid: 231158d8-5e81-4630-b8d5-93fee16e0cd3
title: Identificare l'aggiornamento del livello funzionale
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 9527a186cb20c470e0b5644fff58f90786520f97
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/12/2017
---
# <a name="identifying-your-functional-level-upgrade"></a>Identificare l'aggiornamento del livello funzionale

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

È possibile aumentare i livelli di funzionalità del dominio e foresta, è necessario valutare l'ambiente corrente e identificare il livello funzionale che meglio soddisfi le esigenze dell'organizzazione. Identificando i domini della foresta, i controller di dominio che si trovano in ogni dominio, il sistema operativo e service pack che ogni controller di dominio è in esecuzione e la data in cui si pianifica di aggiornare i controller di dominio, valutare l'ambiente corrente. Se si prevede di ritirare un controller di dominio, assicurarsi che è comprendere l'impatto completo di questa operazione verrà nell'ambiente.  
  
Le circostanze seguenti potrebbero impedire l'aggiornamento di una versione precedente del sistema operativo Windows Server per il livello di funzionalità di Windows Server 2008 o Windows Server 2008 R2:  
  
-   Hardware non sufficiente  
  
-   Un controller di dominio che esegue un programma antivirus non compatibile con Windows Server 2008 o Windows Server 2008 R2   
  
-   Uso di un programma specifico della versione che non viene eseguito in Windows Server 2008 o Windows Server 2008 R2   
  
-   La necessità di aggiornare un programma con service pack più recente  
  
La documentazione di queste informazioni consentono di identificare i passaggi da eseguire per verificare di disporre di un ambiente completamente funzionale di Windows Server 2008 o Windows Server 2008 R2.  
  
Dopo aver valutato l'ambiente corrente, è necessario identificare l'aggiornamento del livello funzionale che si applica all'organizzazione. Queste opzioni sono disponibili:  
  
-   Ambiente di Windows 2000 in modalità nativa di Windows Server 2008 o Windows Server 2008 R2   
  
-   Foresta di Windows Server 2003 a Windows Server 2008 o Windows Server 2008 R2   
  
-   Nuova foresta di Windows Server 2008  
  
-   Nuova foresta di Windows Server 2008 R2  
  
## <a name="upgrading-functional-levels-in-a-native-windows-2000-active-directory-forest"></a>Aggiornare i livelli di funzionalità in una foresta di Active Directory di Windows 2000 nativo  
In un ambiente nativo di Windows 2000 composto esclusivamente da controller di dominio basato su Windows 2000, i livelli di funzionalità per impostazione predefinita per i seguenti livelli e rimangano questi livelli fino a quando non si aumenta manualmente:  
  
-   Livello di funzionalità dominio nativo di Windows 2000  
  
-   Livello di funzionalità foresta Windows 2000  
  
Per usare tutte le funzionalità a livello di foresta e livello di dominio in Windows Server 2008 o Windows Server 2008 R2, è necessario eseguire l'aggiornamento di questo ambiente di Windows 2000 a Windows Server 2008 o Windows Server 2008 R2. È possibile eseguire l'aggiornamento in uno dei modi seguenti:  
  
-   Introdurre appena installato Windows Server 2008-basato su Windows Server 2008 R2 o -basato su controller di dominio nella foresta e quindi ritirarlo tutti i controller di dominio che eseguono Windows 2000.  
  
-   Eseguire un aggiornamento sul posto di tutti i controller di dominio esistente che esegue Windows 2000 nella foresta a controller di dominio che esegue Windows Server 2003. Quindi, eseguire un aggiornamento sul posto dei controller di dominio di Windows Server 2008 o Windows Server 2008 R2. Per ulteriori informazioni, vedere [l'aggiornamento di domini Active Directory per Windows Server 2008 AD DS domini \[LH\]](assetId:///9c91be5f-df14-40b2-b176-2b1852a51e61).  
  
    > [!IMPORTANT]  
    >  Windows Server 2008 R2 è un sistema operativo basato su x64. Se il server è in esecuzione una versione basata su x64 di Windows Server 2003, è possibile eseguire correttamente un aggiornamento sul posto del sistema operativo del computer a Windows Server 2008 R2. Se il server è in esecuzione una versione basata su x86 di Windows Server 2003, è possibile aggiornare il computer a Windows Server 2008 R2.  
  
Per usare le funzionalità a livello di dominio Windows Server 2008 o Windows Server 2008 R2 senza aggiornare l'intera foresta Windows 2000 a Windows Server 2008 o Windows Server 2008 R2, genera solo il livello funzionale di dominio a Windows Server 2008 o Windows Server 2008 R2.  
  
> [!NOTE]  
> Prima di aumentare il livello di funzionalità del dominio, è necessario aggiornare tutti i controller di dominio basati su Windows 2000 in tale dominio a Windows Server 2008 o Windows Server 2008 R2.  
  
Dopo la sostituzione di tutti i controller di dominio basati su Windows 2000 nell'insieme di strutture con controller di dominio che eseguono Windows Server 2008 o Windows Server 2008 R2, è possibile aumentare il livello di funzionalità foresta Windows Server 2008 o Windows Server 2008 R2. In questo modo automatico genera il livello di funzionalità di tutti i domini nella foresta è impostato su Windows 2000 originale o versioni successive di Windows Server 2008 o Windows Server 2008 R2.  
  
Per ulteriori informazioni sull'aumento dei livelli di funzionalità foresta e del dominio e per le procedure per eseguire tali attività, vedere [la distribuzione di un Windows Server 2008 foresta radice dominio \[LH\]](assetId:///92406e8d-dc1c-4740-a00a-2c4032896dd1).  
  
## <a name="upgrading-functional-levels-in-a-windows-server-2003-active-directory-forest"></a>Aggiornare i livelli di funzionalità in una foresta di Active Directory di Windows Server 2003  
In un ambiente Windows Server 2003 che include solo i controller di dominio basati su Windows Server 2003, i livelli di funzionalità per impostazione predefinita per i seguenti livelli e rimangano questi livelli fino a quando non si aumenta manualmente:  
  
-   Livello di funzionalità dominio nativo di Windows 2000  
  
-   Livello di funzionalità foresta Windows 2000  
  
Per usare tutte le funzionalità a livello di foresta e livello di dominio in Windows Server 2008 o Windows Server 2008 R2, è necessario eseguire l'aggiornamento di questo ambiente di Windows Server 2003 a Windows Server 2008 o Windows Server 2008 R2. È possibile eseguire l'aggiornamento in uno dei modi seguenti:  
  
-   Introdurre una nuova installazione di Windows Server 2008-basato su Windows Server 2008 R2 o -basato su controller di dominio nella foresta e quindi rimuovere tutti i controller di dominio che esegue Windows Server 2003 o eseguirne l'aggiornamento a Windows Server 2008 o Windows Server 2008 R2.  
  
-   Eseguire un aggiornamento sul posto di tutti i controller di dominio esistente che esegue Windows Server 2003 ai controller di dominio che eseguono Windows Server 2008 o Windows Server 2008 R2. Per ulteriori informazioni, vedere [l'aggiornamento di domini Active Directory per Windows Server 2008 AD DS domini \[LH\]](assetId:///9c91be5f-df14-40b2-b176-2b1852a51e61).  
  
> [!IMPORTANT]  
>  Windows Server 2008 R2 è un sistema operativo basato su x64. Se il server è in esecuzione una versione basata su x64 di Windows Server 2003, è possibile eseguire correttamente un aggiornamento sul posto del sistema operativo del computer a Windows Server 2008 R2. Se il server è in esecuzione una versione basata su x86 di Windows Server 2003, è possibile aggiornare il computer per eseguire Windows Server 2008 R2.  
  
Per usare le funzionalità a livello di dominio di tutti i Windows Server 2008 o Windows Server 2008 R2 senza aggiornare l'intera foresta di Windows Server 2003 a Windows Server 2008 o Windows Server 2008 R2, genera solo il livello funzionale di dominio a Windows Server 2008 o Windows Server 2008 R2.  
  
> [!NOTE]  
> Prima di aumentare il livello di funzionalità del dominio, è necessario aggiornare tutti i controller di dominio basati su Windows Server 2003 in tale dominio a Windows Server 2008 o Windows Server 2008 R2.  
  
Dopo aver aggiornato tutti i controller di dominio basati su Windows Server 2003 nella foresta a Windows Server 2008 o Windows Server 2008 R2, è possibile aumentare il livello di funzionalità foresta Windows Server 2008 o Windows Server 2008 R2. In questo modo automatico genera il livello di funzionalità di tutti i domini nella foresta sono impostati su Windows Server 2003 a Windows Server 2008 o Windows Server 2008 R2.  
  
Per ulteriori informazioni sull'aumento dei livelli di funzionalità foresta e del dominio e per le procedure per eseguire tali attività, vedere [la distribuzione di un Windows Server 2008 foresta radice dominio \[LH\]](assetId:///92406e8d-dc1c-4740-a00a-2c4032896dd1).  
  
## <a name="upgrading-functional-levels-in-a-new-windows-server-2008-forest"></a>Aggiornare i livelli di funzionalità in una nuova foresta di Windows Server 2008  
Quando si installa il primo controller di dominio in una nuova foresta di Windows Server 2008, livelli di funzionalità per impostazione predefinita per i seguenti livelli e rimangano questi livelli fino a quando non si aumenta manualmente:  
  
-   Livello di funzionalità dominio nativo di Windows 2000  
  
-   Livello di funzionalità foresta Windows 2000  
  
Livelli di funzionalità vengono impostati questi livelli predefinito per offrirti la possibilità di aggiungere alla foresta di Windows Server 2008 nuovo controller di dominio basato su Windows Server 2003 o Windows 2000. Dopo aver creato un dominio radice della foresta, il livello di funzionalità del dominio per ogni dominio che aggiungere alla foresta Windows Server 2008 è impostato su Windows 2000 nativo. Tuttavia, se si desidera tutti i controller di dominio nel nuovo ambiente Windows Server 2008 per l'esecuzione di Windows Server 2008, impostare il livello di funzionalità foresta e il livello di funzionalità del dominio, a Windows Server 2008 quando si installa il primo controller di dominio della foresta. Questa operazione consente di risparmiare tempo e Abilita tutte le funzionalità a livello di foresta e livello di dominio in Windows Server 2008.  
  
> [!IMPORTANT]  
> Se l'insieme di strutture opera a funzionalità di Windows Server 2008 livello e si tenta di installare Active Directory in un server membro basato su Windows Server 2003 o un server membro basato su Windows 2000, l'installazione non riesce.  
  
Per ulteriori informazioni sull'aumento dei livelli di funzionalità foresta e del dominio e per le procedure per eseguire tali attività, vedere [la distribuzione di un Windows Server 2008 foresta radice dominio \[LH\]](assetId:///92406e8d-dc1c-4740-a00a-2c4032896dd1).  
  
## <a name="upgrading-functional-levels-in-a-new-windows-server-2008-r2-forest"></a>Aggiornare i livelli di funzionalità in una nuova foresta di Windows Server 2008 R2  
Quando si installa il primo controller di dominio in una nuova foresta di Windows Server 2008 R2, livelli di funzionalità per impostazione predefinita per i seguenti livelli e rimangano questi livelli fino a quando non si aumenta manualmente:  
  
-   Livello di funzionalità dominio di Windows Server 2003  
  
-   Livello di funzionalità foresta Windows Server 2003  
  
Livelli di funzionalità vengono impostati questi livelli predefinito per offrirti la possibilità di aggiungere i controller di dominio basati su Windows Server 2003 per la nuova foresta di Windows Server 2008 R2. Dopo aver creato un dominio radice della foresta, viene impostato il livello di funzionalità del dominio per ogni dominio che aggiungere alla foresta Windows Server 2008 R2 a Windows Server 2003. Tuttavia, se si desidera tutti i controller di dominio nel nuovo ambiente Windows Server 2008 R2 per l'esecuzione di Windows Server 2008 R2, impostare il livello di funzionalità foresta e il livello di funzionalità del dominio, a Windows Server 2008 R2 quando si installa il primo controller di dominio della foresta. Questa operazione consente di risparmiare tempo e Abilita tutte le funzionalità a livello di foresta e livello di dominio in Windows Server 2008 R2.  
  
> [!IMPORTANT]  
> Se l'insieme di strutture opera a livello funzionale di Windows Server 2008 R2 e si tenta di installare Active Directory in Windows Server 2008-server membro di base o basato su Windows Server 2003, o in un server membro basato su Windows 2000, l'installazione non riesce.  
  
Per ulteriori informazioni sull'aumento dei livelli di funzionalità foresta e del dominio e per le procedure per eseguire tali attività, vedere [la distribuzione di un Windows Server 2008 foresta radice dominio \[LH\]](assetId:///92406e8d-dc1c-4740-a00a-2c4032896dd1).  
  
> [!NOTE]  
> Anche se è necessario installare ADMT v 3.1 in Windows Server 2008, è possibile utilizzare ADMT v 3.1 per la migrazione degli oggetti a un dominio che è ospitato da uno o più controller di dominio di Windows Server 2008 R2. Per ulteriori informazioni, vedere [articolo 976659](https://go.microsoft.com/fwlink/?LinkId=180398) della Microsoft Knowledge Base (https://go.microsoft.com/fwlink/?LinkId=180398).  
  


