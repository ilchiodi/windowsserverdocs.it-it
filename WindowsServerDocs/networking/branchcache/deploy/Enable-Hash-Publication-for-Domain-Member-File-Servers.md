---
title: Abilitare la pubblicazione di hash per i file server membri del dominio
description: Questo argomento fa parte di BranchCache distribuzione Guide per Windows Server 2016, che illustra come distribuire BranchCache in modalità cache distribuita e ospitato per ottimizzare l'utilizzo della larghezza di banda WAN nelle succursali
manager: brianlic
ms.prod: windows-server
ms.technology: networking-bc
ms.topic: get-started-article
ms.assetid: a3f1f7c4-d9b2-43e6-8bfa-fac707bbd4d3
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 1e450b9a2282cb4820b8802aa6d36e822f56ca12
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71356587"
---
# <a name="enable-hash-publication-for-domain-member-file-servers"></a>Abilitare la pubblicazione di hash per i file server membri del dominio

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016

Quando si utilizza servizi di dominio Active Directory (AD DS), è possibile utilizzare criteri di gruppo per abilitare la pubblicazione di hash di BranchCache per più file server. A tale scopo, è necessario creare un'unità organizzativa (OU), aggiungere i file server per l'unità Organizzativa, creare una pubblicazione di hash di BranchCache oggetto Criteri di gruppo (GPO) e quindi configurare l'oggetto Criteri di gruppo.  
  
Vedere gli argomenti seguenti per abilitare pubblicazione di hash per più file server.  
  
-   [Creare l'unità organizzativa di file server con BranchCache](../../branchcache/deploy/Create-the-BranchCache-File-Servers-Organizational-Unit.md)  
  
-   [Spostare i file server nell'unità organizzativa di file server con BranchCache](../../branchcache/deploy/Move-File-Servers-to-the-BranchCache-File-Servers-Organizational-Unit.md)  
  
-   [Creare la pubblicazione di hash di BranchCache Criteri di gruppo oggetto](../../branchcache/deploy/Create-the-BranchCache-Hash-Publication-Group-Policy-Object.md)  
  
-   [Configurare la pubblicazione di hash di BranchCache Criteri di gruppo oggetto](../../branchcache/deploy/Configure-the-BranchCache-Hash-Publication-Group-Policy-Object.md)  
  


