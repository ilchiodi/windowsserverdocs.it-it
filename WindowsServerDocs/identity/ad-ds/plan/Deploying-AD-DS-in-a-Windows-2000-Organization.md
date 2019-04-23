---
ms.assetid: 7530cafe-98d7-46c9-95d9-e49d39caa021
title: Distribuzione di Active Directory Domain Services in un'organizzazione con Windows 2000
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 5015c0ca2c01fca966927af4207a7e3501d9cd39
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59854282"
---
# <a name="deploying-ad-ds-in-a-windows-2000-organization"></a>Distribuzione di Active Directory Domain Services in un'organizzazione con Windows 2000

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Se l'organizzazione è in esecuzione Windows 2000 Active Directory, è possibile distribuire Windows Server 2008 Active Directory Domain Services (AD DS), entrambi eseguendo un aggiornamento sul posto di alcuni o tutti i sistemi operativi dei controller di dominio a Windows Server 2008 o introducendo i controller di dominio che esegue Windows Server 2008 nell'ambiente.  
  
Prima di poter aggiungere un controller di dominio che esegue Windows Server 2008 a un dominio Windows 2000 Active Directory esistente, è necessario eseguire **adprep**, uno strumento da riga di comando. Adprep estende lo schema di Active Directory, aggiorna i descrittori di protezione predefinito degli oggetti selezionati e aggiunge i nuovi oggetti di directory come richiesto da alcune applicazioni. Adprep è disponibile sul disco di installazione di Windows Server 2008 (\sources\adprep\adprep.exe). Per altre informazioni, vedere Adprep ([https://go.microsoft.com/fwlink/?LinkId=99215](https://go.microsoft.com/fwlink/?LinkId=99215)).  
  
> [!NOTE]  
> Se si desidera eseguire un aggiornamento sul posto di un controller di dominio Windows 2000 Active Directory esistente a Windows Server 2008, è necessario prima aggiornare il server a Windows Server 2003 e quindi eseguirne l'aggiornamento a Windows Server 2008.  
  
Nella figura seguente vengono illustrati i passaggi per la distribuzione di Windows Server 2008 Active Directory in un ambiente di rete attualmente in esecuzione Windows 2000 Active Directory.  
  
![distribuzione in un'organizzazione di windows 2000](media/Deploying-AD-DS-in-a-Windows-2000-Organization/ee51218a-a858-49d9-8b99-9986679191c1.gif)  
  
> [!NOTE]  
> Se si desidera impostare il livello di funzionalità del dominio o foresta a Windows Server 2008, tutti i controller di dominio nell'ambiente in uso devono eseguire il sistema operativo Windows Server 2008.  
  
Consolidamento dei domini di account e risorse che vengono aggiornati sul posto da un ambiente Windows 2000 come parte di Active Directory di Windows Server 2008 potrebbe richiedere la distribuzione tra insiemi di strutture o intraforest ristrutturazione di dominio. Ristrutturazione dei domini di Active Directory tra foreste consente di ridurre la complessità dell'organizzazione e i relativi costi amministrativi. Ristrutturazione dei domini di Active Directory all'interno di un insieme di strutture consente di ridurre il carico amministrativo per l'organizzazione di riduzione del traffico di replica, riducendo la quantità di utente e l'amministrazione di gruppo che è necessario e semplificare l'amministrazione dei criteri di gruppo. Per altre informazioni, vedere ADMT v 3.1 Guida alla migrazione ([https://go.microsoft.com/fwlink/?LinkId=93678](https://go.microsoft.com/fwlink/?LinkId=93678)).  
  
Per un elenco di attività dettagliate che è possibile usare per pianificare e distribuire Active Directory Domain Services in un'organizzazione che è attualmente in esecuzione Windows 2000 Active Directory, vedere [elenco di controllo: Distribuzione di Active Directory in un'organizzazione di Windows 2000](https://technet.microsoft.com/library/cc732737.aspx).  
  


