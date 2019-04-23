---
ms.assetid: d8e61aa4-8e4b-4097-83ca-70cf61366b75
title: Delega dell'amministrazione tramite oggetti unità organizzative
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 39c4278270e4ab4fba9ff1062d2aa043d203a74b
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59886942"
---
# <a name="delegating-administration-by-using-ou-objects"></a>Delega dell'amministrazione tramite oggetti unità organizzative

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

È possibile usare unità organizzative (OU) per delegare l'amministrazione degli oggetti, quali utenti o computer, nell'unità Organizzativa per un gruppo o utente designato. Per delegare l'amministrazione tramite un'unità Organizzativa, posizionare persona o gruppo a cui si delega dei diritti amministrativi in un gruppo, posizionare il set di oggetti deve essere controllato in un'unità Organizzativa e quindi delegare attività amministrative per l'unità Organizzativa a tale gruppo.  
  
Servizi di dominio Active Directory (AD DS) consente di controllare le attività amministrative che possono essere delegate a un livello molto dettagliato. Ad esempio, è possibile assegnare un gruppo per avere controllo completo di tutti gli oggetti in un'unità Organizzativa. assegnare un altro gruppo i diritti solo per creare, eliminare e gestire gli account utente nell'unità Organizzativa; e quindi assegnare un terzo gruppo il diritto solo per reimpostare le password degli account utente. È possibile rendere ereditabili queste autorizzazioni in modo che si applicano a tutte le unità organizzative che vengono inserite in sottoalberi dell'unità Organizzativa originale.  
  
Impostazione predefinita le unità organizzative e contenitori vengono creati durante l'installazione di Active Directory Domain Services e sono controllati dagli amministratori del servizio. È consigliabile se gli amministratori del servizio continuano a controllare questi contenitori. Se è necessario delegare il controllo degli oggetti nella directory, creare altre unità organizzative e posizionare gli oggetti in queste unità organizzative. Delegare il controllo su queste unità organizzative per gli amministratori di dati appropriato. Questo rende possibile delegare il controllo degli oggetti nella directory senza modificare il controllo predefinito in base agli amministratori del servizio.  
  
Il proprietario della foresta determina il livello di autorità che viene delegata a un proprietario dell'unità Organizzativa. Questo può variare dalla possibilità di creare e modificare gli oggetti all'interno dell'unità Organizzativa a in corso consentito controllare solo un singolo attributo di un singolo tipo di oggetto nell'unità Organizzativa. Concedere a un utente la possibilità di creare un oggetto nell'unità Organizzativa in modo implicito che l'utente concede la possibilità di modificare qualsiasi attributo di qualsiasi oggetto creato dall'utente. Inoltre, se l'oggetto che viene creato un contenitore, l'utente ha implicitamente la possibilità di creare e modificare tutti gli oggetti che vengono inseriti nel contenitore.  
  
## <a name="in-this-section"></a>Contenuto della sezione  
  
-   [Delega dell'amministrazione di contenitori predefiniti e unità organizzative](../../ad-ds/plan/Delegating-Administration-of-Default-Containers-and-OUs.md)  
  
-   [Delega dell'amministrazione di OU di Account e OU di risorse](../../ad-ds/plan/Delegating-Administration-of-Account-OUs-and-Resource-OUs.md)  
  


