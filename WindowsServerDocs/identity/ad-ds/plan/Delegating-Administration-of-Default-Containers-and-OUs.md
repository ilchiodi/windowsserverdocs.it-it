---
ms.assetid: ac6604b0-7459-4ff3-af1c-4936897f5d14
title: "Delega dell'amministrazione di unità organizzative e contenitori predefiniti"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 2504208210c03193451d19478f3bc8c98ec98f23
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2017
---
# <a name="delegating-administration-of-default-containers-and-ous"></a>Delega dell'amministrazione di unità organizzative e contenitori predefiniti

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Ogni dominio di Active Directory contiene un set standard di contenitori e unità organizzative (OU) che vengono create durante l'installazione di servizi di dominio Active Directory (AD DS). Queste includono quanto segue:  
  
-   Contenitore di dominio, che funge da contenitore principale nella gerarchia di  
  
-   Contenitore predefinito, che contiene il valore predefinito di account amministratore del servizio  
  
-   Contenitore di utenti, che è il percorso predefinito per i nuovi account utente e gruppi creati nel dominio  
  
-   Contenitore di computer, che è il percorso predefinito per i nuovi account computer creato nel dominio  
  
-   Unità Organizzativa controller di dominio, che è il percorso predefinito per gli account computer per gli account computer controller di dominio  
  
Il proprietario della foresta controlla questi contenitori predefiniti e unità organizzative.  
  
## <a name="domain-container"></a>Contenitore di dominio  
Il contenitore di dominio è il contenitore radice della gerarchia di un dominio. Le modifiche ai criteri o elenco di controllo di accesso (ACL) nel contenitore possono potenzialmente influire a livello di dominio. Non delegare il controllo di questo contenitore. si devono essere controllato dagli amministratori del servizio.  
  
## <a name="users-and-computers-containers"></a>Utenti e computer di contenitori  
Quando si esegue un aggiornamento sul posto da Windows Server 2003 a Windows Server 2008, gli utenti e computer esistenti vengono inseriti automaticamente in utenti e i contenitori di computer. Se si sta creando un nuovi contenitori di dominio, utenti e computer di Active Directory sono i percorsi predefiniti per tutti i nuovi account utente e account computer non-controller di dominio nel dominio.  
  
> [!IMPORTANT]  
> Se si desidera delegare il controllo su utenti o computer, non modificare le impostazioni predefinite in utenti e computer di contenitori. Al contrario, creare nuove unità organizzative (in base alle esigenze) e spostare gli oggetti utente e computer dal loro contenitori predefiniti e nelle nuove unità organizzative. Delegare il controllo tramite le nuove unità organizzative, in base alle esigenze. È consigliabile non modificare che controlla i contenitori predefiniti.  
  
Inoltre, non è possibile applicare le impostazioni di criteri di gruppo ai computer e utenti predefiniti dei contenitori. Per applicare criteri di gruppo a utenti e computer, creare nuove unità organizzative e spostare gli oggetti utente e computer in tali unità organizzative. Applicare le impostazioni di criteri di gruppo per le nuove unità organizzative.  
  
Facoltativamente, è possibile reindirizzare la creazione di oggetti che si trovano nei contenitori predefiniti da inserire in contenitori di tua scelta.  
  
## <a name="well-known-users-and-groups-and-built-in-accounts"></a>Utenti noti e gruppi e account predefiniti  
Per impostazione predefinita, i diversi utenti noti e gruppi e account predefiniti vengono creati in un nuovo dominio. È consigliabile che rimanga sotto il controllo degli amministratori del servizio di gestione di questi account. Non delegare la gestione di questi account a un singolo che non sia un amministratore del servizio. La tabella seguente elenca i noto agli utenti e gruppi e account predefiniti che devono restare sotto il controllo degli amministratori del servizio.  
  
|Utenti noti e gruppi|Account predefiniti|  
|--------------------------------|----------------------|  
|Cert Publishers<br /><br />Controller di dominio<br /><br />Proprietari autori criteri di gruppo<br /><br />KRBTGT<br /><br />Domain Guests<br /><br />Amministratore<br /><br />Domain Admins<br /><br />Schema Admins (solo dominio radice della foresta)<br /><br />Enterprise Admins (solo dominio radice della foresta)<br /><br />Utenti del dominio|Amministratore<br /><br />Guest<br /><br />Utenti guest<br /><br />Account Operators<br /><br />Amministratori<br /><br />Gruppo Backup Operators<br /><br />In ingresso Forest Trust Builders<br /><br />Operatori di stampa<br /><br />Accesso compatibile precedente a Windows 2000<br /><br />Server Operators<br /><br />Utenti|  
  
## <a name="domain-controller-ou"></a>Unità Organizzativa Controller di dominio  
Quando i controller di dominio vengono aggiunti al dominio, gli oggetti computer vengono aggiunti automaticamente all'unità Organizzativa Controller di dominio. Questa unità Organizzativa include un set predefinito di criteri applicati a essa. Per garantire che i criteri vengono applicati in modo uniforme a tutti i controller di dominio, è consigliabile non spostare gli oggetti computer del controller di dominio da questa unità Organizzativa. Impossibile applicare i criteri predefiniti può causare un controller di dominio funzionare correttamente.  
  
Per impostazione predefinita, gli amministratori del servizio di controllano l'unità organizzativa specifica. Non delegare il controllo di questa unità Organizzativa a singoli utenti diversi di amministratori del servizio.  
  


