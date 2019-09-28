---
title: Ripristino della foresta di Active Directory-backup di un server completo
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/09/2018
ms.topic: article
ms.prod: windows-server
ms.assetid: 398918dc-c8ab-41a6-a377-95681ec0b543
ms.technology: identity-adds
ms.openlocfilehash: e9222685e8f6369e560a841990bc13ab8b0e4d37
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71390262"
---
# <a name="resetting-a-trust-password-on-one-side-of-the-trust"></a>Reimpostazione di una password di trust su un lato della relazione di trust  

>Si applica a: Windows Server 2016, Windows Server 2012 e 2012 R2, Windows Server 2008 e 2008 R2

 Se il ripristino della foresta è correlato a una violazione della sicurezza, attenersi alla procedura riportata di seguito per reimpostare una password di trust su un lato della relazione di trust. Sono inclusi i trust impliciti tra domini figlio e padre, nonché trust espliciti tra il dominio (il dominio trusting) e un altro dominio (dominio trusted). 
  
 Reimpostare la password solo sul lato del dominio trusting del trust, noto anche come trust in ingresso (il lato a cui appartiene il dominio). Quindi, usare la stessa password sul lato del dominio attendibile del trust, nota anche come trust in uscita. Reimpostare la password del trust in uscita quando si ripristina il primo controller di dominio in tutti gli altri domini (Trusted). 
  
 La reimpostazione della password di trust garantisce che il controller di dominio non venga replicato con controller di dominio potenzialmente danneggiati all'esterno del relativo dominio. Impostando la stessa password di trust durante il ripristino del primo controller di dominio in ognuno dei domini, si garantisce che il controller di dominio venga replicato con ognuno dei controller di dominio ripristinati. I controller di dominio successivi nel dominio ripristinati con l'installazione di servizi di dominio Active Directory eseguiranno automaticamente la replica di queste nuove password durante il processo di installazione. 
  
## <a name="to-reset-a-trust-password-on-one-side-of-the-trust"></a>Per reimpostare una password di trust su un lato della relazione di trust  
  
1. Al prompt dei comandi digitare il comando seguente e quindi premere INVIO:  

   ```  
   netdom experthelp trust  
   ```  
  
2. Utilizzare la sintassi fornita da questo comando per l'utilizzo dello strumento NetDom per reimpostare la password di trust.
   Se, ad esempio, nella foresta sono presenti due domini, padre e figlio, e si esegue questo comando sul controller di dominio ripristinato nel dominio padre, usare la sintassi del comando seguente:  

   ```  
   netdom trust parent domain name /domain:child domain name /resetOneSide /passwordT:password /userO:administrator /passwordO:*  
   ```  

   Quando si esegue questo comando nel dominio figlio, usare la sintassi del comando seguente:  

   ```  
   netdom trust child domain name /domain:parent domain name /resetOneSide /passwordT:password /userO:administrator /passwordO:*  
   ```  

   > [!NOTE]
   > il valore della **password** deve corrispondere a entrambi i lati del trust. Eseguire questo comando una sola volta (a differenza del comando **netdom resetpwd** ) perché la password viene reimpostata due volte. 
  
## <a name="next-steps"></a>Passaggi successivi

- [Guida al ripristino della foresta di Active Directory](AD-Forest-Recovery-Guide.md)
- [Ripristino della foresta di Active Directory - Procedure](AD-Forest-Recovery-Procedures.md)
