---
title: Utilizzare criteri di restrizione Software per proteggere il Computer da un Virus di posta elettronica
description: Protezione di Windows Server
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: security-software-restriction-policies
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 02f23979-f832-4e46-bdea-21fd77db35b2
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/12/2016
ms.openlocfilehash: 41b4c2399a86ef96d34b62295eda4a1ce9300609
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/12/2017
---
# <a name="use-software-restriction-policies-to-help-protect-your-computer-against-an-email-virus"></a>Utilizzare criteri di restrizione Software per proteggere il Computer da un Virus di posta elettronica

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

In questo argomento fornisce informazioni sull'impostazione di controllo delle applicazioni criteri tramite Software di criteri di restrizione per proteggere il computer da a partire da virus di posta elettronica da Windows Server 2008 e Windows Vista.

## <a name="introduction"></a>Introduzione
Software di criteri di restrizione è una funzionalità basata su criteri di gruppo che identifica i programmi software in esecuzione nel computer in un dominio e controlla la possibilità di eseguire tali programmi. Utilizzare criteri di restrizione software per creare una configurazione molto restrittiva per i computer, in cui consentire l'esecuzione solo di applicazioni specifiche identificate eseguire. Questi sono integrati in servizi di dominio Microsoft Active Directory e criteri di gruppo, ma possono anche essere configurati in computer autonomi. Per un punto di partenza per criteri di restrizione software, vedere il [criteri di restrizione Software](software-restriction-policies.md).

A partire da Windows Server 2008 R2 e Windows 7, Windows AppLocker può essere utilizzato invece di o in combinazione con criteri di restrizione software per una parte della strategia di controllo dell'applicazione. 

#### <a name="configure-srp-to-help-protect-against-an-e-mail-virus"></a>Configurare i criteri di restrizione software per proteggersi da un virus di posta elettronica

1.  Esaminare le procedure consigliate per i criteri di restrizione software comprendere il funzionamento di criteri di restrizione software.

    -   [Procedure consigliate](software-restriction-policies-technical-overview.md#BKMK_Best_Practices)

    -   [Funzionano di criteri di restrizione Software](https://technet.microsoft.com/library/cc786941(v=WS.10).aspx)

2.  Aprire criteri di restrizione Software.

    -   [Per il computer locale](administer-software-restriction-policies.md#BKMK_1)

    -   [Per un dominio, sito, o unità organizzativa e si sono su un server membro o una workstation in cui viene aggiunto a un dominio](administer-software-restriction-policies.md#BKMK_2)

3.  Se i criteri di restrizione software non è stato definito in precedenza, è possibile creare nuovi criteri di restrizione software.

    -   [Per creare nuovi criteri di restrizione software](administer-software-restriction-policies.md#BKMK_Create_SRP)

4.  Creare una regola di percorso per la cartella che utilizza il programma di posta elettronica per l'esecuzione degli allegati di posta elettronica e quindi impostare la protezione di livello **non consentito**.

    -   [Utilizzo delle regole di percorso](work-with-software-restriction-policies-rules.md#BKMK_Path_Rules)

5.  Specificare i tipi di file a cui si applica la regola.

    -   [Per aggiungere o eliminare un tipo di file](administer-software-restriction-policies.md#BKMK_Add_Del)

6.  Modificare le impostazioni di criteri in modo che si applicano a utenti e gruppi che si desidera:

    -   Specificare gli utenti o gruppi a cui non si desidera l'oggetto Criteri di gruppo (oggetto criteri di gruppo) per applicare le impostazioni di criteri.

    -   Escludere gli amministratori locali da criteri di restrizione software di un'impostazione di criteri specifici in Criteri di gruppo e ancora il resto dei criteri di gruppo si applicano agli amministratori.

        -   [Per impedire che i criteri di restrizione software applicati agli amministratori locali](administer-software-restriction-policies.md#BKMK_Prevent_Admin)

7.  Testare i criteri.


