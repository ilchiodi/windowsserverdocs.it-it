---
title: Ripristino della foresta Active Directory - backup di un server completo
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/09/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.assetid: 398918dc-c8ab-41a6-a377-95681ec0b543
ms.technology: identity-adds
ms.openlocfilehash: 455c930a90cd443cf1fe62f436abca88384f79c1
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59865562"
---
# <a name="resetting-a-trust-password-on-one-side-of-the-trust"></a>Reimpostare una password di attendibilità sul uno lato della relazione di trust  

>Si applica a: Windows Server 2016, Windows Server 2012 e 2012 R2, Windows Server 2008 e 2008 R2

 Se il ripristino della foresta è correlato a una violazione della sicurezza, usare la procedura seguente per reimpostare una password di attendibilità sul uno lato della relazione di trust. Sono incluse relazioni di trust implicite tra figlio e padre domini e le relazioni di trust esplicite tra il dominio corrente (il dominio trusting) e un altro dominio (dominio trusted). 
  
 Reimpostare la password solo sul lato dominio trusting del trust, noto anche come il trust in ingresso (il lato a cui appartiene questo dominio). Quindi, usare la stessa password sul lato della relazione di trust, noto anche come la relazione di trust in uscita dominio trusted. Reimpostare la password del trust in uscita quando si ripristina il primo controller di dominio in ognuno degli altri domini (attendibile). 
  
 La reimpostazione della password di trust assicura che il controller di dominio non viene replicato con un controller di dominio potenzialmente errati di fuori del dominio. Impostando la stessa password di trust durante il ripristino del primo controller di dominio in ognuno dei domini, assicurarsi che il controller di dominio viene replicato con ognuno dei controller di dominio ripristinato. Successivi controller di dominio nel dominio che vengono ripristinati tramite l'installazione di Active Directory Domain Services eseguirà automaticamente la replica di queste nuove password durante il processo di installazione. 
  
## <a name="to-reset-a-trust-password-on-one-side-of-the-trust"></a>Per reimpostare una password di attendibilità sul uno lato della relazione di trust  
  
1. Al prompt dei comandi digitare il comando seguente e quindi premere INVIO:  

   ```  
   netdom experthelp trust  
   ```  
  
2. Usare la sintassi che questo comando fornisce per utilizzare lo strumento NetDom per reimpostare la password di trust.
   Ad esempio, se sono presenti due domini nella foresta, ovvero padre e figlio e si esegue questo comando nel controller di dominio ripristinato nel dominio padre, usare la sintassi del comando seguente:  

   ```  
   netdom trust parent domain name /domain:child domain name /resetOneSide /passwordT:password /userO:administrator /passwordO:*  
   ```  

   Quando si esegue questo comando nel dominio figlio, usare la sintassi del comando seguente:  

   ```  
   netdom trust child domain name /domain:parent domain name /resetOneSide /passwordT:password /userO:administrator /passwordO:*  
   ```  

   > [!NOTE]
   > **passwordT** deve essere lo stesso valore in entrambi i lati del trust. Eseguire questo comando solo una volta (a differenza di **netdom resetpwd** comandi) poiché viene reimpostato automaticamente la password due volte. 
  
## <a name="next-steps"></a>Passaggi successivi

- [Guida al ripristino della foresta AD](AD-Forest-Recovery-Guide.md)
- [Ripristino della foresta Active Directory - procedure](AD-Forest-Recovery-Procedures.md)
