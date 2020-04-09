---
title: Usare i criteri di restrizione software per proteggere il computer da un virus di posta elettronica
description: Sicurezza di Windows Server
ms.prod: windows-server
ms.technology: security-software-restriction-policies
ms.topic: article
ms.assetid: 02f23979-f832-4e46-bdea-21fd77db35b2
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/12/2016
ms.openlocfilehash: 680d0435d77164e101f045b439be6ccb6601dfef
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80855734"
---
# <a name="use-software-restriction-policies-to-help-protect-your-computer-against-an-email-virus"></a>Usare i criteri di restrizione software per proteggere il computer da un virus di posta elettronica

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

In questo argomento vengono fornite informazioni su come impostare i criteri di controllo delle applicazioni utilizzando i criteri di restrizione software (SRP) per proteggere il computer da virus di posta elettronica a partire da Windows Server 2008 e Windows Vista.

## <a name="introduction"></a>Introduzione
I criteri di restrizione software sono una funzionalità basata su Criteri di gruppo che identifica i programmi software in esecuzione nei computer di un dominio e controlla la possibilità di tali programmi di essere eseguiti. È possibile utilizzare i criteri di restrizione software per creare una configurazione molto restrittiva per i computer, in cui consentire l'esecuzione solo di applicazioni specifiche identificate. Queste sono integrate con Microsoft Active Directory Domain Services e Criteri di gruppo ma possono essere configurate anche nei computer autonomi. Per un punto di partenza per SRP, vedere [criteri di restrizione software](software-restriction-policies.md).

A partire da Windows Server 2008 R2 e Windows 7, è possibile utilizzare Windows AppLocker anziché o in concerto con SRP per una parte della strategia di controllo delle applicazioni. 

#### <a name="configure-srp-to-help-protect-against-an-e-mail-virus"></a>Configurare i criteri di RESTRIzione per la protezione da un virus di posta elettronica

1.  Esaminare le procedure consigliate per i criteri di restrizione software per comprendere il funzionamento di SRP.

    -   [Procedure consigliate](software-restriction-policies-technical-overview.md#BKMK_Best_Practices)

    -   [Come funzionano i criteri di restrizione software](https://technet.microsoft.com/library/cc786941(v=WS.10).aspx)

2.  Aprire Criteri restrizione software.

    -   [Per il computer locale](administer-software-restriction-policies.md#BKMK_1)

    -   [Per un dominio, un sito o un'unità organizzativa e l'utente si trova in un server membro o in una workstation aggiunta a un dominio](administer-software-restriction-policies.md#BKMK_2)

3.  Se in precedenza non sono stati definiti criteri di restrizione software, creare nuovi criteri di restrizione software.

    -   [Per creare nuovi criteri di restrizione software](administer-software-restriction-policies.md#BKMK_Create_SRP)

4.  Creare una regola di percorso per la cartella utilizzata dal programma di posta elettronica per eseguire allegati di posta elettronica, quindi impostare il livello di sicurezza su non **consentito**.

    -   [Utilizzo delle regole di percorso](work-with-software-restriction-policies-rules.md#BKMK_Path_Rules)

5.  Specificare i tipi di file a cui si applica la regola.

    -   [Per aggiungere o eliminare un tipo di file designato](administer-software-restriction-policies.md#BKMK_Add_Del)

6.  Modificare le impostazioni dei criteri in modo che vengano applicate agli utenti e ai gruppi desiderati:

    -   Specificare gli utenti o i gruppi a cui non si desidera applicare le impostazioni dei criteri dell'oggetto di Criteri di gruppo (GPO).

    -   Escludere gli amministratori locali dai criteri di restrizione software di un'impostazione dei criteri specifica in Criteri di gruppo e continuare a applicare gli altri Criteri di gruppo agli amministratori.

        -   [Per impedire l'applicazione di criteri di restrizione software agli amministratori locali](administer-software-restriction-policies.md#BKMK_Prevent_Admin)

7.  Testare i criteri.


