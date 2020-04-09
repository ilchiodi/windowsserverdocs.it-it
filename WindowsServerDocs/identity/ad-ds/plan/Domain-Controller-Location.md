---
ms.assetid: cc2834ec-8f66-4209-aba3-402d710cd1bd
title: Posizione del controller di dominio
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: 2b76f3fcad72c875a83f00e6ade37cfeb5675bf9
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80822524"
---
# <a name="domain-controller-location"></a>Posizione del controller di dominio

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

I client utilizzano Domain Name System (DNS) per individuare i controller di dominio per completare operazioni quali l'elaborazione delle richieste di accesso o la ricerca di risorse pubblicate nella directory. I controller di dominio registrano una serie di record in DNS per consentire ai client e ad altri computer di individuarli. Questi record sono definiti collettivamente come record del localizzatore.  
  
I controller di dominio usano anche DNS per individuare altri controller di dominio e per eseguire attività come la replica. Il processo mediante il quale i controller di dominio individuano altri controller di dominio equivale al processo mediante il quale i client individuano i controller di dominio.  
  


