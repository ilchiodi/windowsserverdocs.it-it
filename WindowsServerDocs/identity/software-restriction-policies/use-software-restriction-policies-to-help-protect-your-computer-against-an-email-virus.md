---
title: Usare i criteri di restrizione software per proteggere il computer da un virus di posta elettronica
description: Sicurezza di Windows Server
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
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59850672"
---
# <a name="use-software-restriction-policies-to-help-protect-your-computer-against-an-email-virus"></a>Usare i criteri di restrizione software per proteggere il computer da un virus di posta elettronica

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

In questo argomento vengono fornite informazioni sull'impostazione di controllo delle applicazioni di criteri usando i criteri di restrizione Software (SRP) per proteggere il computer dall'inizio di virus tramite posta elettronica con Windows Server 2008 e Windows Vista.

## <a name="introduction"></a>Introduzione
I criteri di restrizione software sono una funzionalità basata su Criteri di gruppo che identifica i programmi software in esecuzione nei computer di un dominio e controlla la possibilità di tali programmi di essere eseguiti. È possibile utilizzare i criteri di restrizione software per creare una configurazione molto restrittiva per i computer, in cui consentire l'esecuzione solo di applicazioni specifiche identificate. Questi sono integrati con Microsoft Active Directory Domain Services e criteri di gruppo, ma può anche essere configurati in computer autonomi. Per un punto di partenza per criteri di restrizione software, vedere la [criteri di restrizione Software](software-restriction-policies.md).

A partire da Windows Server 2008 R2 e Windows 7, Windows AppLocker utilizzabile al posto di o in concerto con criteri di restrizione software per una parte della strategia di controllo dell'applicazione. 

#### <a name="configure-srp-to-help-protect-against-an-e-mail-virus"></a>Configurare criteri di restrizione software per proteggersi da virus della posta elettronica

1.  Esaminare le procedure consigliate per i criteri di restrizione software comprendere il funzionamento di criteri di restrizione software.

    -   [Procedure consigliate](software-restriction-policies-technical-overview.md#BKMK_Best_Practices)

    -   [Funzionano dei criteri di restrizione Software](https://technet.microsoft.com/library/cc786941(v=WS.10).aspx)

2.  Aprire Criteri restrizione software.

    -   [Per il computer locale](administer-software-restriction-policies.md#BKMK_1)

    -   [Per un dominio, sito, o unità organizzativa e si sia in un server membro o una workstation appartenente a un dominio](administer-software-restriction-policies.md#BKMK_2)

3.  Se i criteri di restrizione software non è stato definito in precedenza, creare nuovi criteri di restrizione software.

    -   [Per creare nuovi criteri di restrizione software](administer-software-restriction-policies.md#BKMK_Create_SRP)

4.  Creare una regola di percorso per la cartella usata dal programma di posta elettronica per l'esecuzione degli allegati di posta elettronica e quindi impostare la sicurezza di livello **vietate**.

    -   [Utilizzo delle regole di percorso](work-with-software-restriction-policies-rules.md#BKMK_Path_Rules)

5.  Specificare i tipi di file a cui viene applicata la regola.

    -   [Per aggiungere o eliminare un tipo di file](administer-software-restriction-policies.md#BKMK_Add_Del)

6.  Modificare le impostazioni dei criteri in modo che si applicano a utenti e gruppi da:

    -   Specificare gli utenti o gruppi a cui non si desidera l'oggetto Criteri di gruppo (oggetto criteri di gruppo) per applicare impostazioni di criteri.

    -   Escludere gli amministratori locali tra i criteri di restrizione software di un'impostazione di criteri specifiche nei criteri di gruppo e ancora il resto dei criteri di gruppo si applicano agli amministratori.

        -   [Per impedire che i criteri di restrizione software applicati agli amministratori locali](administer-software-restriction-policies.md#BKMK_Prevent_Admin)

7.  Testare i criteri.


