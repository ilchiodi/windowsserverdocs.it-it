---
ms.assetid: 231158d8-5e81-4630-b8d5-93fee16e0cd3
title: Identificazione dell'aggiornamento del livello funzionale
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 8aee6a5560edfae656582b3c2812ca69c6b90f57
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59812042"
---
# <a name="identifying-your-functional-level-upgrade"></a>Identificazione dell'aggiornamento del livello funzionale

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Prima è possibile generare i livelli di funzionalità del dominio e foresta, è necessario valutare l'ambiente corrente e identificare il livello funzionale che meglio soddisfa le esigenze della propria organizzazione. Valutare l'ambiente corrente identificando i domini nella foresta, i controller di dominio che si trovano in ogni dominio, il sistema operativo e service pack che ogni controller di dominio è in esecuzione e la data in cui si prevede di aggiornare il dominio controller. Se si intende ritirare un controller di dominio, verificare che l'opzione comprendere l'impatto complessivo che in questo modo hanno sull'ambiente.  
  
Situazioni seguenti potrebbero impedire l'aggiornamento di una versione precedente del sistema operativo Windows Server per il livello di funzionalità di Windows Server 2008 o Windows Server 2008 R2:  
  
-   Componenti hardware inadeguati  
  
-   Un controller di dominio che eseguono un software antivirus che non è compatibile con Windows Server 2008 o Windows Server 2008 R2   
  
-   Uso di un programma specifico della versione che non viene eseguito in Windows Server 2008 o Windows Server 2008 R2   
  
-   La necessità di eseguire l'aggiornamento di un programma con il service pack più recente  
  
Documentare queste informazioni consentono di identificare i passaggi da eseguire per assicurarsi di avere un ambiente completamente funzionante di Windows Server 2008 o Windows Server 2008 R2.  
  
Dopo aver valutato l'ambiente corrente, è necessario identificare l'aggiornamento del livello funzionale che si applica all'organizzazione. Queste opzioni sono disponibili:  
  
-   Ambiente in modalità nativa Windows 2000 a Windows Server 2008 o Windows Server 2008 R2   
  
-   Foresta di Windows Server 2003 a Windows Server 2008 o Windows Server 2008 R2   
  
-   Nuova foresta di Windows Server 2008  
  
-   Nuova foresta di Windows Server 2008 R2  
  
## <a name="upgrading-functional-levels-in-a-native-windows-2000-active-directory-forest"></a>L'aggiornamento dei livelli di funzionalità in una foresta di Active Directory di Windows 2000 nativo  
In un ambiente nativo di Windows 2000 costituita solo da controller di dominio basati su Windows 2000, i livelli di funzionalità per impostazione predefinita per i seguenti livelli e rimangano a livelli fino a quando non si generano manualmente:  
  
-   Livello di funzionalità dominio nativo di Windows 2000  
  
-   Livello funzionalità foresta Windows 2000  
  
Per usare tutte le funzionalità a livello di foreste e a livello di dominio in Windows Server 2008 o Windows Server 2008 R2, è necessario aggiornare l'ambiente Windows 2000 a Windows Server 2008 o Windows Server 2008 R2. È possibile eseguire l'aggiornamento in uno dei modi seguenti:  
  
-   Introdurre appena installato Windows Server 2008-base o Windows Server 2008 R2-basati su controller di dominio nella foresta e quindi ritirare tutti i controller di dominio che eseguono Windows 2000.  
  
-   Eseguire un aggiornamento sul posto di tutti i controller di dominio esistente che esegue Windows 2000 nella foresta a controller di dominio che esegue Windows Server 2003. Quindi, eseguire un aggiornamento sul posto di tali controller di dominio a Windows Server 2008 o Windows Server 2008 R2. Per altre informazioni, vedere [l'aggiornamento di domini Active Directory per Windows Server 2008 AD DS domini \[LH\]](assetId:///9c91be5f-df14-40b2-b176-2b1852a51e61).  
  
    > [!IMPORTANT]  
    >  Windows Server 2008 R2 è un sistema operativo basato su x64. Se il server è in esecuzione una versione basata su x64 di Windows Server 2003, è possibile eseguire correttamente un aggiornamento sul posto del sistema operativo del computer a Windows Server 2008 R2. Se il server è in esecuzione una versione basata su x86 di Windows Server 2003, è possibile aggiornare il computer a Windows Server 2008 R2.  
  
Per usare le funzionalità a livello di dominio di Windows Server 2008 o Windows Server 2008 R2 senza aggiornare l'intera foresta di Windows 2000 a Windows Server 2008 o Windows Server 2008 R2, genera solo il livello funzionale di dominio a Windows Server 2008 o Windows Server 2008 R2.  
  
> [!NOTE]  
> Prima di aumentare il livello funzionale del dominio, è necessario aggiornare tutti i controller di dominio basati su Windows 2000 in tale dominio a Windows Server 2008 o Windows Server 2008 R2.  
  
Dopo la sostituzione di tutti i controller di dominio basati su Windows 2000 nell'insieme di strutture con controller di dominio che eseguono Windows Server 2008 o Windows Server 2008 R2, è possibile generare il livello di funzionalità foresta Windows Server 2008 o Windows Server 2008 R2. In questo modo automatico innalza il livello funzionale di tutti i domini nella foresta configurati per Windows 2000 originale o versioni successive a Windows Server 2008 o Windows Server 2008 R2.  
  
Per altre informazioni su come generare livelli di funzionalità foresta e dominio e per le procedure per eseguire tali attività, vedere [distribuzione di un dominio radice foresta di Windows Server 2008 \[LH\]](assetId:///92406e8d-dc1c-4740-a00a-2c4032896dd1).  
  
## <a name="upgrading-functional-levels-in-a-windows-server-2003-active-directory-forest"></a>L'aggiornamento dei livelli di funzionalità in una foresta di Active Directory di Windows Server 2003  
In un ambiente di Windows Server 2003 che è costituito solo i controller di dominio basati su Windows Server 2003, i livelli di funzionalità per impostazione predefinita per i seguenti livelli e rimangano a livelli fino a quando non si generano manualmente:  
  
-   Livello di funzionalità dominio nativo di Windows 2000  
  
-   Livello funzionalità foresta Windows 2000  
  
Per usare tutte le funzionalità a livello di foreste e a livello di dominio in Windows Server 2008 o Windows Server 2008 R2, è necessario aggiornare l'ambiente di Windows Server 2003 a Windows Server 2008 o Windows Server 2008 R2. È possibile eseguire l'aggiornamento in uno dei modi seguenti:  
  
-   Introdurre una nuova installazione di Windows Server 2008-base o Windows Server 2008 R2-basati su controller di dominio nella foresta, quindi ritirare tutti i controller di dominio che esegue Windows Server 2003 o eseguirne l'aggiornamento a Windows Server 2008 o Windows Server 2008 R2.  
  
-   Eseguire un aggiornamento sul posto di tutti i controller di dominio esistenti eseguono Windows Server 2003 ai controller di dominio che eseguono Windows Server 2008 o Windows Server 2008 R2. Per altre informazioni, vedere [l'aggiornamento di domini Active Directory per Windows Server 2008 AD DS domini \[LH\]](assetId:///9c91be5f-df14-40b2-b176-2b1852a51e61).  
  
> [!IMPORTANT]  
>  Windows Server 2008 R2 è un sistema operativo basato su x64. Se il server è in esecuzione una versione basata su x64 di Windows Server 2003, è possibile eseguire correttamente un aggiornamento sul posto del sistema operativo del computer a Windows Server 2008 R2. Se il server è in esecuzione una versione basata su x86 di Windows Server 2003, è possibile aggiornare il computer per eseguire Windows Server 2008 R2.  
  
Per usare le funzionalità a livello di dominio di tutti i Windows Server°2008 o Windows Server 2008 R2 senza aggiornare l'intera foresta di Windows Server 2003 a Windows Server 2008 o Windows Server 2008 R2, genera solo il livello funzionale di dominio a Windows Server 2008 o Windows S Server 2008 R2.  
  
> [!NOTE]  
> Prima di aumentare il livello funzionale del dominio, è necessario aggiornare tutti i controller di dominio basati su Windows Server 2003 in tale dominio a Windows Server 2008 o Windows Server 2008 R2.  
  
Dopo aver aggiornato tutti i controller di dominio basati su Windows Server 2003 nella foresta a Windows Server 2008 o Windows Server 2008 R2, è possibile generare il livello di funzionalità foresta Windows Server 2008 o Windows Server 2008 R2. In questo modo automatico innalza il livello funzionale di tutti i domini nella foresta sono impostati su Windows Server 2003 a Windows Server 2008 o Windows Server 2008 R2.  
  
Per altre informazioni su come generare livelli di funzionalità foresta e dominio e per le procedure per eseguire tali attività, vedere [distribuzione di un dominio radice foresta di Windows Server 2008 \[LH\]](assetId:///92406e8d-dc1c-4740-a00a-2c4032896dd1).  
  
## <a name="upgrading-functional-levels-in-a-new-windows-server-2008-forest"></a>L'aggiornamento dei livelli di funzionalità in una nuova foresta di Windows Server 2008  
Quando si installa il primo controller di dominio in una nuova foresta di Windows Server 2008, livelli di funzionalità per impostazione predefinita per i seguenti livelli e rimangano a livelli fino a quando non si generano manualmente:  
  
-   Livello di funzionalità dominio nativo di Windows 2000  
  
-   Livello funzionalità foresta Windows 2000  
  
Livelli di funzionalità vengono impostati a livello predefinito per offrirti la possibilità di aggiungere alla foresta di Windows Server 2008 nuovo controller di dominio basati su Windows Server 2003 o Windows 2000. Dopo aver creato un dominio radice della foresta, il livello funzionale di dominio per ogni dominio che aggiunta alla foresta Windows Server 2008 è impostato su Windows 2000 nativo. Tuttavia, se si desidera che tutti i controller di dominio nel nuovo ambiente Windows Server 2008 per l'esecuzione di Windows Server 2008, impostare il livello funzionale della foresta, quindi il livello funzionale di dominio, a Windows Server 2008 quando si installa il primo controller di dominio nel fores t. Questa operazione consente di risparmiare tempo e Abilita tutte le funzionalità a livello di foreste e a livello di dominio di Windows Server 2008.  
  
> [!IMPORTANT]  
> Se la foresta opera con le funzionalità di Windows Server 2008 livello e si tenta di installare Active Directory in un server basato su Windows Server 2003 membro o un server membro basate su Windows 2000, l'installazione ha esito negativo.  
  
Per altre informazioni su come generare livelli di funzionalità foresta e dominio e per le procedure per eseguire tali attività, vedere [distribuzione di un dominio radice foresta di Windows Server 2008 \[LH\]](assetId:///92406e8d-dc1c-4740-a00a-2c4032896dd1).  
  
## <a name="upgrading-functional-levels-in-a-new-windows-server-2008-r2-forest"></a>L'aggiornamento dei livelli di funzionalità in una nuova foresta di Windows Server 2008 R2  
Quando si installa il primo controller di dominio in una nuova foresta di Windows Server 2008 R2, livelli di funzionalità per impostazione predefinita per i seguenti livelli e rimangano a livelli fino a quando non si generano manualmente:  
  
-   Livello di funzionalità dominio di Windows Server 2003  
  
-   Livello di funzionalità foresta Windows Server 2003  
  
Livelli di funzionalità vengono impostati a livello predefinito per offrirti la possibilità di aggiungere i controller di dominio basati su Windows Server 2003 alla nuova foresta Windows Server 2008 R2. Dopo aver creato un dominio radice della foresta, viene impostato il livello funzionale di dominio per ogni dominio che aggiunta alla foresta Windows Server 2008 R2 a Windows Server 2003. Tuttavia, se si desidera che tutti i controller di dominio nel nuovo ambiente Windows Server 2008 R2 per l'esecuzione di Windows Server 2008 R2, impostare il livello funzionale della foresta, quindi il livello funzionale di dominio, a Windows Server 2008 R2 quando si installa il primo controller di dominio in yo la foresta. Questa operazione consente di risparmiare tempo e Abilita tutte le funzionalità a livello di foreste e a livello di dominio in Windows Server 2008 R2.  
  
> [!IMPORTANT]  
> Se la foresta opera a livello funzionale di Windows Server 2008 R2 e si prova a installare Active Directory in Windows Server 2008-server membro in base o basato su Windows Server 2003, o in un server membro basate su Windows 2000, l'installazione non riesce.  
  
Per altre informazioni su come generare livelli di funzionalità foresta e dominio e per le procedure per eseguire tali attività, vedere [distribuzione di un dominio radice foresta di Windows Server 2008 \[LH\]](assetId:///92406e8d-dc1c-4740-a00a-2c4032896dd1).  
  
> [!NOTE]  
> Anche se ADMT v 3.1 devono essere installati in Windows Server 2008, è possibile usare ADMT v 3.1 per eseguire la migrazione di oggetti a un dominio che è ospitato da uno o più controller di dominio di Windows Server 2008 R2. Per altre informazioni, vedere [articolo 976659](https://go.microsoft.com/fwlink/?LinkId=180398) della Microsoft Knowledge Base (https://go.microsoft.com/fwlink/?LinkId=180398).  
  


