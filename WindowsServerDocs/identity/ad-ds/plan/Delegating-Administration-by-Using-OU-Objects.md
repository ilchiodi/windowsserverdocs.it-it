---
ms.assetid: d8e61aa4-8e4b-4097-83ca-70cf61366b75
title: "La delega dell'amministrazione tramite oggetti unità Organizzative"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 8d0b4765304d8b302fc174c191af2c8e87a25304
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2017
---
# <a name="delegating-administration-by-using-ou-objects"></a>La delega dell'amministrazione tramite oggetti unità Organizzative

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

È possibile utilizzare unità organizzative (OU) per delegare l'amministrazione di oggetti, quali utenti o computer, nell'unità Organizzativa a un singolo designato o un gruppo. Per delegare l'amministrazione tramite un'unità Organizzativa, inserire il singolo o un gruppo a cui si delega dei diritti amministrativi in un gruppo, Inserisci il set di oggetti da controllare in un'unità Organizzativa e quindi delegare le attività amministrative per l'unità Organizzativa a quel gruppo.  
  
Servizi di dominio Active Directory (AD DS) consente di controllare le attività amministrative che possono essere delegate a un livello molto dettagliato. Ad esempio, è possibile assegnare un gruppo di avere il controllo completo di tutti gli oggetti in un'unità Organizzativa. assegnare un altro gruppo i diritti solo per creare, eliminare e gestire gli account utente nell'unità Organizzativa; e quindi assegnare un terzo gruppo il diritto solo per reimpostare le password degli account utente. È possibile apportare queste autorizzazioni ereditabili in modo che si applicano a tutte le unità organizzative che si trovano in sottostrutture dell'unità Organizzativa originale.  
  
Impostazione predefinita le unità organizzative e contenitori vengono creati durante l'installazione di servizi di dominio Active Directory e sono controllati dagli amministratori dei servizi. È consigliabile se gli amministratori del servizio continuano a controllare questi contenitori. Se si desidera delegare il controllo degli oggetti nella directory, creare altre unità organizzative e posizionare gli oggetti in queste unità organizzative. Delegare il controllo tramite queste unità organizzative agli amministratori di dati appropriato. Questo rende possibile delegare il controllo degli oggetti nella directory senza modificare il controllo predefinito assegnato agli amministratori di servizio.  
  
Il proprietario della foresta determina il livello di autorità delegato a un proprietario di unità Organizzativa. Questo può variare dalla possibilità di creare e modificare gli oggetti all'interno di unità Organizzativa consentito solo per controllare un singolo attributo di un singolo tipo di oggetto nell'unità Organizzativa. Concessione di un utente la possibilità di creare un oggetto nell'unità Organizzativa in modo implicito concede che l'utente la possibilità di modificare qualsiasi attributo di qualsiasi oggetto creato dall'utente. Inoltre, se l'oggetto che viene creato un contenitore, l'utente in modo implicito ha la possibilità di creare e modificare tutti gli oggetti che vengono inseriti nel contenitore.  
  
## <a name="in-this-section"></a>In questa sezione  
  
-   [Delega dell'amministrazione di unità organizzative e contenitori predefiniti](../../ad-ds/plan/Delegating-Administration-of-Default-Containers-and-OUs.md)  
  
-   [Delega dell'amministrazione di OU di Account e OU di risorse](../../ad-ds/plan/Delegating-Administration-of-Account-OUs-and-Resource-OUs.md)  
  


