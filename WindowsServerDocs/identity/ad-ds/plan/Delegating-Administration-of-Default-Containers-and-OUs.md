---
ms.assetid: ac6604b0-7459-4ff3-af1c-4936897f5d14
title: Delega dell'amministrazione di unità organizzative e contenitori predefiniti
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: a8523ee738b991714a9c8673b6faaff7d9003987
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80822654"
---
# <a name="delegating-administration-of-default-containers-and-ous"></a>Delega dell'amministrazione di unità organizzative e contenitori predefiniti

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Ogni dominio Active Directory contiene un insieme standard di contenitori e unità organizzative create durante l'installazione di Active Directory Domain Services (AD DS). tra cui i seguenti:  
  
-   Contenitore di dominio, che funge da contenitore radice per la gerarchia  
  
-   Contenitore incorporato, che include gli account di amministratore del servizio predefiniti  
  
-   Container utenti, ovvero il percorso predefinito per i nuovi account utente e i gruppi creati nel dominio  
  
-   Computer container, ovvero il percorso predefinito per i nuovi account computer creati nel dominio  
  
-   Controller di dominio OU, che è il percorso predefinito per gli account computer per i controller di dominio account computer  
  
Il proprietario della foresta controlla i contenitori predefiniti e le unità organizzative.  
  
## <a name="domain-container"></a>Contenitore di dominio  
Il contenitore del dominio è il contenitore radice della gerarchia di un dominio. Le modifiche ai criteri o all'elenco di controllo di accesso (ACL) in questo contenitore possono potenzialmente avere un effetto a livello di dominio. Non delegare il controllo del contenitore; deve essere controllata dagli amministratori del servizio.  
  
## <a name="users-and-computers-containers"></a>Contenitori di utenti e computer  
Quando si esegue un aggiornamento del dominio sul posto da Windows Server 2003 a Windows Server 2008, gli utenti e i computer esistenti vengono inseriti automaticamente nei contenitori utenti e computer. Se si sta creando un nuovo dominio Active Directory, i contenitori utenti e computer sono i percorsi predefiniti per tutti i nuovi account utente e per gli account computer non controller di dominio nel dominio.  
  
> [!IMPORTANT]  
> Se è necessario delegare il controllo su utenti o computer, non modificare le impostazioni predefinite nei contenitori utenti e computer. In alternativa, creare nuove unità organizzative (se necessario) e spostare gli oggetti utente e computer dai contenitori predefiniti e nelle nuove unità organizzative. Delegare il controllo sulle nuove unità organizzative, in base alle esigenze. Si consiglia di non modificare chi controlla i contenitori predefiniti.  
  
Non è inoltre possibile applicare le impostazioni Criteri di gruppo ai contenitori utenti e computer predefiniti. Per applicare Criteri di gruppo a utenti e computer, creare nuove unità organizzative e spostare gli oggetti utente e computer in tali unità organizzative. Applicare le impostazioni Criteri di gruppo alle nuove unità organizzative.  
  
Facoltativamente, è possibile reindirizzare la creazione di oggetti inseriti nei contenitori predefiniti da inserire nei contenitori di propria scelta.  
  
## <a name="well-known-users-and-groups-and-built-in-accounts"></a>Utenti e gruppi ben noti e account predefiniti  
Per impostazione predefinita, diversi utenti e gruppi noti e account predefiniti vengono creati in un nuovo dominio. È consigliabile che la gestione di questi account rimanga sotto il controllo degli amministratori dei servizi. Non delegare la gestione di questi account a un utente che non è un amministratore del servizio. Nella tabella seguente sono elencati gli utenti e i gruppi noti e gli account predefiniti che devono rimanere sotto il controllo degli amministratori dei servizi.  
  
|Utenti e gruppi noti|Account predefiniti|  
|--------------------------------|----------------------|  
|Cert Publishers<p>Controller di dominio<p>Proprietari autori criteri di gruppo<p>KRBTGT<p>Domain Guests<p>Amministratore<p>Domain Admins<p>Schema Admins (solo dominio radice della foresta)<p>Enterprise Admins (solo dominio radice della foresta)<p>Domain Users|Amministratore<p>Guest<p>Guests<p>Account Operators<p>Administrators<p>Backup Operators<p>Generatori di trust tra foreste in ingresso<p>Print Operators<p>Accesso compatibile precedente a Windows 2000<p>Server Operators<p>Utenti|  
  
## <a name="domain-controller-ou"></a>Unità organizzativa del controller di dominio  
Quando i controller di dominio vengono aggiunti al dominio, i relativi oggetti computer vengono aggiunti automaticamente all'unità organizzativa del controller di dominio. A questa unità organizzativa è applicato un set predefinito di criteri. Per assicurarsi che questi criteri vengano applicati in modo uniforme a tutti i controller di dominio, è consigliabile non spostare gli oggetti computer dei controller di dominio fuori da questa OU. L'impossibilità di applicare i criteri predefiniti può causare un errore del controller di dominio per il corretto funzionamento.  
  
Per impostazione predefinita, gli amministratori del servizio controllano questa OU. Non delegare il controllo di questa OU a singoli utenti diversi dagli amministratori del servizio.  
  


