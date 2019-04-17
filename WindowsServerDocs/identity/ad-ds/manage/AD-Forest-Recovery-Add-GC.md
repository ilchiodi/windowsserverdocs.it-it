---
title: Ripristino della foresta Active Directory - aggiunta di catalogo globale
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 07/07/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.assetid: 5a291f65-794e-4fc3-996e-094c5845a383
ms.technology: identity-adfs
ms.openlocfilehash: 3749fd99737f61c55871f613b9feaa21090d3bae
ms.sourcegitcommit: 84a2bdcb92ba6af45781fab9727617e50fa5e911
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/07/2017
---
# <a name="ad-forest-recovery---adding-the-gc"></a>Ripristino della foresta Active Directory - aggiunta di catalogo globale 

>Si applica a: Windows Server 2016, Windows Server 2012 e 2012 R2, Windows Server 2008 e 2008 R2

 Utilizzare la procedura seguente per aggiungere il catalogo globale a un controller di dominio.  
  
## <a name="to-add-the-global-catalog"></a>Per aggiungere il catalogo globale  
  
1.  Fare clic su **Start**, scegliere **tutti i programmi**, scegliere **strumenti di amministrazione**, quindi fare clic su **Active Directory Sites and Services**.  
2.  Nell'albero della console, espandere il **siti** contenitore e quindi selezionare il sito appropriato che contiene il server di destinazione.  
3.  Espandere il **server** contenitore, quindi espandere l'oggetto server per il controller di dominio a cui si desidera aggiungere il catalogo globale.  
4.  Fare doppio clic su **impostazioni NTDS**, quindi fare clic su **proprietà **.  
5.  Selezionare il **catalogo globale** casella di controllo.  
![Aggiungere GC](media/AD-Forest-Recovery-Add-GC/addgc1.png)
  
## <a name="to-add-the-global-catalog-using-repadmin"></a>Per aggiungere il catalogo globale con Repadmin  
  
1.  Aprire un prompt dei comandi con privilegi elevati, digitare il comando seguente e premere INVIO:  
  
    ```  
    repadmin.exe /options DC_NAME +IS_GC  
    ```  
  
 Di seguito sono modi per velocizzare il processo di aggiunta del catalogo globale per il controller di dominio nel dominio radice:  
  
-   In teoria, il controller di dominio nel dominio radice deve essere un partner di replica dei controller di dominio ripristinato nei domini non radice. In tal caso, verificare che il controllo di coerenza informazioni (KCC) ha creato il corrispondente **repsFrom** oggetto per il controller di dominio di origine e una partizione nel controller di dominio radice. È possibile confermare questo eseguendo il **repadmin /showreps /v** comando.  
  
-   Se è presente alcun **repsFrom** oggetto creato, creare questo oggetto per la partizione di configurazione. In questo modo, il controller di dominio nel dominio radice della può determinare quale controller di dominio nel dominio non radice sono stati eliminati. È possibile farlo con i comandi seguenti:  
  
    ```  
    repadmin /add ConfigurationNamingContext DestinationDomainController SourceDomainControllerCNAME  
    ```  
  
    ```  
    repadmin /options DSA -Disable_NTDSCONN_XLATE  
    ```  
  
     Il formato per la *SourceDomainControllerCNAME* è:  
  
    ```  
  
    sourceDCGuid._msdcs.root domain  
    ```  
  
     Ad esempio, il comando repadmin /add per la partizione di configurazione del dominio contoso.com potrebbe essere:  
  
    ```  
    repadmin /add cn=configuration,DC=contoso,DC=com DC01 937ef930-7356-43c8-88dc-8baaaa781cf6._msdcs.dDSP17A22.contoso.com  
    ```  
  
-   Se il **repsFrom** oggetto è presente, tentare di sincronizzare il controller di dominio nel dominio radice con il controller di dominio nel dominio non radice come segue:  
  
    ```  
    Repadmin /sync DomainNamingContext DestinationDomainController SourceDomainControllerGUID  
    ```  
  
     Dove *DestinationDomainController* è il controller di dominio nel dominio radice e *SourceDomainController* è il controller di dominio ripristinato nel dominio non radice.  
  
-   Il server radice DNS di dominio deve avere i record di risorse alias (CNAME) per il controller di dominio di origine. Assicurarsi che la zona DNS padre contiene i record di risorse delega (server dei nomi (NS) e record di risorse host (A)) per il controller di dominio corretto (i controller di dominio che sono stati ripristinati da un backup) nella zona figlio.  
  
-   Assicurarsi che il controller di dominio nel dominio radice è contattando il corretto Centro distribuzione chiavi (KDC) nel dominio non radice. Per testare questa operazione, al prompt dei comandi, digitare il comando seguente e quindi premere INVIO:  
  
    ```  
    nltest /dsgetdc:nonroot domain name /KDC /Force  
    ```  
## <a name="next-steps"></a>Passaggi successivi

- [Guida di ripristino della foresta Active Directory](AD-Forest-Recovery-Guide.md)
- [Ripristino della foresta Active Directory - procedure](AD-Forest-Recovery-Procedures.md)  
