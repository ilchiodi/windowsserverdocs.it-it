---
title: Ripristino della foresta Active Directory - backup di un intero server
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 07/07/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.assetid: 398918dc-c8ab-41a6-a377-95681ec0b543
ms.technology: identity-adfs
ms.openlocfilehash: 51b53e7348ae00ed2fc63711b9c3297279ebe033
ms.sourcegitcommit: 84a2bdcb92ba6af45781fab9727617e50fa5e911
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/07/2017
---
# <a name="resetting-a-trust-password-on-one-side-of-the-trust"></a>Reimpostare una password di trust su un lato della relazione di trust  

>Si applica a: Windows Server 2016, Windows Server 2012 e 2012 R2, Windows Server 2008 e 2008 R2

 Se il ripristino della foresta è correlato a una violazione della protezione, utilizzare la procedura seguente per reimpostare una password di trust su un lato della relazione di trust. Questo include impliciti relazioni di trust tra figlio e domini padre, nonché esplicite relazioni di trust tra il dominio (il dominio trusting) e un altro dominio (il dominio trusted).  
  
 Reimpostare la password solo sul lato dominio trusting del trust, noto anche come il trust in ingresso (il lato in cui appartiene il dominio). Quindi, usare la stessa password sul lato dominio trusted del trust, noto anche come il trust in uscita. Reimpostare la password del trust in uscita quando si ripristina il primo controller di dominio in ognuno degli altri domini (attendibile).  
  
 La reimpostazione della password di trust garantisce che il controller di dominio non viene replicato con un controller di dominio potenzialmente non valido di fuori del dominio. Impostando la stessa password di trust durante il ripristino del primo controller di dominio in ognuno dei domini, assicurarsi che il controller di dominio viene replicato con ognuno dei controller di dominio ripristinato. Successivi controller di dominio nel dominio vengono ripristinati mediante l'installazione di servizi di dominio Active Directory verranno automaticamente replicate queste nuove password durante il processo di installazione.  
  
## <a name="to-reset-a-trust-password-on-one-side-of-the-trust"></a>Per reimpostare una password di trust su un lato della relazione di trust  
  
1.  Al prompt dei comandi, digitare il comando seguente e quindi premere INVIO:  
  
    ```  
    netdom experthelp trust  
    ```  
  
2.  Utilizzare la sintassi che questo comando fornisce per utilizzare lo strumento NetDom per reimpostare la password di trust.  
  
     Ad esempio, se non ci sono due domini nella foresta, padre e figlio e si esegue questo comando sul controller di dominio ripristinato nel dominio padre, utilizzare la sintassi del comando seguente:  
  
    ```  
    netdom trust parent domain name /domain:child domain name /resetOneSide /passwordT:password /userO:administrator /passwordO:*  
    ```  
  
     Quando si esegue questo comando nel dominio figlio, utilizzare la sintassi del comando seguente:  
  
    ```  
    netdom trust child domain name /domain:parent domain name /resetOneSide /password:password /userO:administrator /passwordO:*  
    ```  
  
    > [!NOTE]
    >  **passwordT** deve essere lo stesso valore in entrambe le parti del trust. Eseguire questo comando solo una volta (a differenza di **netdom resetpwd** comando) poiché viene reimpostato automaticamente la password due volte.  
  
## <a name="next-steps"></a>Passaggi successivi

- [Guida di ripristino della foresta Active Directory](AD-Forest-Recovery-Guide.md)
- [Ripristino della foresta Active Directory - procedure](AD-Forest-Recovery-Procedures.md)
