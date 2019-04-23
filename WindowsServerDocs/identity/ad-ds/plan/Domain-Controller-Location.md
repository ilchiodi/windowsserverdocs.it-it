---
ms.assetid: cc2834ec-8f66-4209-aba3-402d710cd1bd
title: Posizione del controller di dominio
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 6b66b224278f15b6abeecbef8fe0778a98159bb7
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59872622"
---
# <a name="domain-controller-location"></a>Posizione del controller di dominio

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

I client usano sistema DNS (Domain Name) per individuare i controller di dominio per operazioni completate, ad esempio l'elaborazione delle richieste di accesso o la ricerca della directory per le risorse pubblicate. Controller di dominio registrano una serie di record in DNS per aiutare i client e di altri computer individuarli. Questi record sono definiti collettivamente come il record del localizzatore.  
  
I controller di dominio anche usano DNS per individuare altri controller di dominio e per eseguire attività quali la replica. Il processo mediante il quale i controller di dominio individuano altri controller di dominio è quello utilizzato per il processo mediante il quale i client individuano i controller di dominio.  
  


