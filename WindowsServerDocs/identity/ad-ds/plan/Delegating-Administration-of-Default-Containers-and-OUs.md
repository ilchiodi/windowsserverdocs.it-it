---
ms.assetid: ac6604b0-7459-4ff3-af1c-4936897f5d14
title: Delega dell'amministrazione di unità organizzative e contenitori predefiniti
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 9d482854fd82b4bf0d0e61315d36e6222470ca55
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59830892"
---
# <a name="delegating-administration-of-default-containers-and-ous"></a>Delega dell'amministrazione di unità organizzative e contenitori predefiniti

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Ogni dominio di Active Directory contiene un set standard di contenitori e unità organizzative (OU) che vengono create durante l'installazione di Active Directory Domain Services (AD DS). tra cui:  
  
-   Contenitore di dominio, che funge da contenitore radice nella gerarchia di  
  
-   Contenitore predefinito, che contiene il valore predefinito di account amministratore del servizio  
  
-   Contenitore di utenti, ovvero il percorso predefinito per nuovi account utente e i gruppi creati nel dominio  
  
-   Contenitore di computer, ovvero il percorso predefinito per nuovi account computer creato nel dominio  
  
-   Unità Organizzativa controller di dominio, ovvero il percorso predefinito per gli account computer per gli account computer controller di dominio  
  
Il proprietario della foresta controlla questi contenitori predefiniti e unità organizzative.  
  
## <a name="domain-container"></a>Contenitore di dominio  
Il contenitore di dominio è il contenitore radice della gerarchia di un dominio. Modifiche ai criteri o l'elenco di controllo di accesso (ACL) in questo contenitore possono potenzialmente influire a livello di dominio. Non delegare il controllo di questo contenitore. deve essere controllato dagli amministratori del servizio.  
  
## <a name="users-and-computers-containers"></a>Utenti e computer di contenitori  
Quando si esegue un aggiornamento sul posto dominio da Windows Server 2003 a Windows Server 2008, gli utenti e computer esistenti vengono automaticamente inseriti nei contenitori i computer e gli utenti. Se si sta creando un nuovi contenitori di dominio, utenti e computer di Active Directory sono i percorsi predefiniti per tutti i nuovi account utente e gli account computer non rappresentano controller di dominio nel dominio.  
  
> [!IMPORTANT]  
> Se è necessario delegare il controllo su utenti o computer, non modificare le impostazioni predefinite nei computer degli utenti e i contenitori. In alternativa, creare nuove unità organizzative (se necessario) e spostare gli oggetti utente e computer dai relativi contenitori predefiniti e nelle nuove unità organizzative. Delegare il controllo di nuove unità organizzative, in base alle esigenze. È consigliabile non modificare chi controlla contenitori predefiniti.  
  
Inoltre, non è possibile applicare le impostazioni di criteri di gruppo ai computer e utenti predefiniti dei contenitori. Per applicare criteri di gruppo a utenti e computer, creare nuove unità organizzative e spostare gli oggetti utente e computer in tali unità organizzative. Applicare le impostazioni di criteri di gruppo per le nuove unità organizzative.  
  
Facoltativamente, è possibile reindirizzare la creazione di oggetti che vengono inseriti nei contenitori predefiniti da inserire nei contenitori di propria scelta.  
  
## <a name="well-known-users-and-groups-and-built-in-accounts"></a>Well-Known utenti e gruppi e account predefiniti  
Per impostazione predefinita, vengono creati diversi noto agli utenti e gruppi e account predefiniti in un nuovo dominio. È consigliabile che la gestione di questi account rimane sotto il controllo degli amministratori del servizio. Non delegare la gestione di questi account a un singolo utente che non sia un amministratore del servizio. La tabella seguente elenca i ben noti agli utenti e gruppi e agli account predefiniti devono essere mantenuti sotto il controllo degli amministratori del servizio.  
  
|Well-Known utenti e gruppi|Account predefiniti|  
|--------------------------------|----------------------|  
|Cert Publishers<br /><br />Controller di dominio<br /><br />Proprietari autori criteri di gruppo<br /><br />KRBTGT<br /><br />Domain Guests<br /><br />Administrator<br /><br />Domain Admins<br /><br />Schema Admins (solo dominio radice della foresta)<br /><br />Enterprise Admins (solo dominio radice della foresta)<br /><br />Domain Users|Administrator<br /><br />Guest<br /><br />Guests<br /><br />Account Operators<br /><br />Administrators<br /><br />Backup Operators<br /><br />In ingresso Forest Trust Builders<br /><br />Print Operators<br /><br />Compatibile con l'accesso a Windows 2000<br /><br />Server Operators<br /><br />Utenti|  
  
## <a name="domain-controller-ou"></a>Unità Organizzativa Controller di dominio  
Quando i controller di dominio vengono aggiunti al dominio, vengono aggiunti automaticamente i relativi oggetti computer nell'unità organizzativa Controller di dominio. Questa unità Organizzativa ha un set predefinito di criteri applicati a essa. Per garantire che questi criteri vengono applicati in modo uniforme a tutti i controller di dominio, è consigliabile non spostare gli oggetti computer del controller di dominio all'esterno di questa unità Organizzativa. Tentativo di applicare i criteri predefiniti può causare un controller di dominio a non funzionare correttamente.  
  
Per impostazione predefinita, gli amministratori del servizio di controllo in questa unità Organizzativa. Non delegare il controllo di questa unità Organizzativa a singoli utenti diversi da amministratori del servizio.  
  


