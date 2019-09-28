---
ms.assetid: cc2834ec-8f66-4209-aba3-402d710cd1bd
title: Posizione del controller di dominio
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: 48ea357b952738c63274d194b4a5aa5d4adcb3d8
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71408844"
---
# <a name="domain-controller-location"></a>Posizione del controller di dominio

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

I client utilizzano Domain Name System (DNS) per individuare i controller di dominio per completare operazioni quali l'elaborazione delle richieste di accesso o la ricerca di risorse pubblicate nella directory. I controller di dominio registrano una serie di record in DNS per consentire ai client e ad altri computer di individuarli. Questi record sono definiti collettivamente come record del localizzatore.  
  
I controller di dominio usano anche DNS per individuare altri controller di dominio e per eseguire attività come la replica. Il processo mediante il quale i controller di dominio individuano altri controller di dominio equivale al processo mediante il quale i client individuano i controller di dominio.  
  


