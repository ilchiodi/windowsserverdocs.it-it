---
ms.assetid: d8e61aa4-8e4b-4097-83ca-70cf61366b75
title: Delega dell'amministrazione tramite oggetti unità organizzative
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: af13896c07c10710be6e087be5d31dbd4aec698f
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80822714"
---
# <a name="delegating-administration-by-using-ou-objects"></a>Delega dell'amministrazione tramite oggetti unità organizzative

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

È possibile utilizzare le unità organizzative (OU) per delegare l'amministrazione di oggetti, ad esempio utenti o computer, all'interno dell'unità organizzativa a un singolo o a un gruppo designato. Per delegare l'amministrazione utilizzando un'unità organizzativa, inserire il singolo o il gruppo a cui si delegano i diritti amministrativi in un gruppo, inserire il set di oggetti da controllare in un'unità organizzativa e quindi delegare le attività amministrative per l'unità organizzativa al gruppo.  
  
Active Directory Domain Services (AD DS) consente di controllare le attività amministrative che possono essere delegate a un livello molto dettagliato. Ad esempio, è possibile assegnare un gruppo per avere il controllo completo di tutti gli oggetti in un'unità organizzativa. assegnare a un altro gruppo i diritti solo per creare, eliminare e gestire gli account utente nell'unità organizzativa; quindi assegnare a un terzo gruppo il diritto solo per reimpostare le password degli account utente. È possibile rendere ereditabili queste autorizzazioni in modo che si applichino a tutte le unità organizzative posizionate in sottoalberi dell'unità organizzativa originale.  
  
Le unità organizzative e i contenitori predefiniti vengono creati durante l'installazione di servizi di dominio Active Directory e sono controllati dagli amministratori del servizio. È consigliabile che gli amministratori del servizio continuino a controllare questi contenitori. Se è necessario delegare il controllo sugli oggetti nella directory, creare unità organizzative aggiuntive e inserire gli oggetti in queste unità organizzative. Delegare il controllo su queste unità organizzative agli amministratori dati appropriati. In questo modo è possibile delegare il controllo sugli oggetti nella directory senza modificare il controllo predefinito assegnato agli amministratori del servizio.  
  
Il proprietario della foresta determina il livello di autorità delegato a un proprietario dell'unità organizzativa. Questa operazione può variare in base alla possibilità di creare e modificare gli oggetti all'interno dell'unità organizzativa per consentire solo il controllo di un singolo attributo di un singolo tipo di oggetto nell'unità organizzativa. Concedere a un utente la possibilità di creare un oggetto nell'unità organizzativa concede in modo implicito a tale utente la possibilità di modificare qualsiasi attributo di qualsiasi oggetto creato dall'utente. Inoltre, se l'oggetto creato è un contenitore, l'utente ha la possibilità di creare e manipolare tutti gli oggetti inseriti nel contenitore.  
  
## <a name="in-this-section"></a>Contenuto della sezione  
  
-   [Delega dell'amministrazione di unità organizzative e contenitori predefiniti](../../ad-ds/plan/Delegating-Administration-of-Default-Containers-and-OUs.md)  
  
-   [Delega dell'amministrazione di unità organizzative di account e unità organizzative di risorse](../../ad-ds/plan/Delegating-Administration-of-Account-OUs-and-Resource-OUs.md)  
  


