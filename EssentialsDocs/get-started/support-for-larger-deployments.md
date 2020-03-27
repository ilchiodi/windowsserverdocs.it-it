---
title: Supporto per distribuzioni estese
description: Viene descritto come usare Windows Server Essentials
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 07d0c4c6-3e92-4969-82b8-105e46ab8d97
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 056e41413d8e11f1f65ab17c1ba365eba2ee2d1a
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/26/2020
ms.locfileid: "80310349"
---
# <a name="support-for-larger-deployments"></a>Supporto per distribuzioni estese

>Si applica a: Windows Server 2016 Essentials

> [!IMPORTANT]  
> Le funzionalità descritte in questo argomento funzionano solo in Windows Server 2016 con il ruolo Esperienza Essentials abilitato e non con lo SKU Windows Server 2016 Essentials.


Windows Server Essentials supporta ora distribuzioni di dimensioni maggiori con:

- più domini
- più controller di dominio
- possibilità di specificare un controller di dominio designato
- supporto per un massimo di 500 utenti e 500 dispositivi

## <a name="support-for-multiple-domains"></a>Supporto di più domini

Windows Server 2012 R2 Essentials supporta un solo dominio per server, che è obbligatorio e il server Essentials deve essere la radice della foresta. Mentre un dominio e una foresta sono ancora necessari, è ora possibile distribuire il ruolo esperienza Windows Server 2016 Essentials in Windows Server 2016 standard o Datacenter per supportare più domini.

## <a name="support-for-multiple-domain-controllers"></a>Supporto per più controller di dominio

 Windows Server Essentials 2012 R2 blocca tutti i servizi che sfruttano Azure Active Directory, ad esempio Office 365, in cui viene distribuito più di un controller di dominio. Il motivo è che la sincronizzazione di account e password tra i controller di dominio locali e Azure Active Directory può compromettere le credenziali dell'account con password non sincronizzate. Questa limitazione è stata rimossa in Windows Server 2016 Essentials.

## <a name="ability-to-specify-a-designated-domain-controller"></a>possibilità di specificare un controller di dominio designato

È ora possibile scegliere un controller di dominio designato che consente di migliorare i tempi di recupero per Active Directory oggetti di dominio, nonché di coordinare la sincronizzazione delle modifiche dell'account tra altri controller di dominio nel dominio.

Il controller di dominio designato predefinito sarà lo stesso server che esegue il ruolo server esperienza Windows Server Essentials. Se il server è un server membro, ovvero non è un controller di dominio, il controller di dominio designato predefinito verrà determinato automaticamente in base alla verifica del controller di dominio nel dominio con latenza di rete più bassa per il server in cui viene eseguito il Ruolo server esperienza Windows Server. Per modificare manualmente il server che rappresenta il controller di dominio designato, è possibile eseguire questa operazione nelle **Impostazioni** del **dashboard di Windows Server Essentials** , come illustrato di seguito.

![Screenshot che mostra il pannello di controllo delle impostazioni in primo piano e il dashboard di Windows Server Essentials in background. La pagina controller di dominio designato del pannello di controllo impostazioni è attualmente selezionata.](media/larger-deployments-1.PNG)

## <a name="support-for-500-users-and-500-devices"></a>Supporto per utenti 500 e 500 dispositivi
-------------------------------------

Il numero massimo di utenti e dispositivi supportati in Windows Server 2012 R2 Essentials è rispettivamente 25 e 50. Con l'introduzione del ruolo server esperienza Windows Server Essentials, tale limite è stato aumentato a 100 utenti e 200 dispositivi.

Windows Server 2016 Essentials supporta 500 utenti e 500 dispositivi. Questa possibilità è un aggiornamento del Framework del provider e dei controlli elenco oggetti in modo da memorizzare nella cache ed eseguire rapidamente il rendering di grandi elenchi di oggetti utente e dispositivo. È stata aggiunta anche una funzionalità di ricerca e filtro che consente di trovare rapidamente l'utente o il dispositivo che si sta cercando (vedere la figura 5-2). Sono disponibili funzionalità di ricerca e filtro nel **dashboard di Windows Server Essentials**, **accesso Web remoto**e Windows e Windows Phone archiviare le applicazioni **server** .

![Screenshot che illustra l'uso della funzionalità di ricerca del dashboard di Windows Server Essentials per cercare la stringa "D5C". I risultati di questa ricerca includono due file e cartelle e due utenti.](media/larger-deployments-2.PNG)

Screenshot che illustra l'uso della funzionalità di ricerca del dashboard di Windows Server Essentials per cercare la stringa "D5C". I risultati di questa ricerca includono due file e cartelle e due utenti.

> [!NOTE]  
> Anche se il limite di utenti e dispositivi supportati è aumentato per il ruolo del server di Windows Server Essentials, il limite supportato per il backup del client rimane a 75.

<a name="see-also"></a>Vedere anche
--------
[Introduzione a Windows Server Essentials](get-started.md)