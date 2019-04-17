---
ms.assetid: cf32926a-2083-408b-a264-2cad179ed18a
title: Distribuisce certificati ai computer Client mediante criteri di gruppo
description: 
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: d3a7e05e4d16565b17b69de254e353df749bbc3a
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/12/2017
---
# <a name="distribute-certificates-to-client-computers-by-using-group-policy"></a>Distribuisce certificati ai computer Client mediante criteri di gruppo

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012


È possibile utilizzare la procedura seguente per raggiungere i certificati SSL (Secure Sockets Layer) \(SSL\) appropriati \ (o equivalente certificati concatenati a un'attendibile root) per i server federativi di account, i server federativi di risorsa e server Web per ogni computer client nella foresta del partner account tramite criteri di gruppo.  
  
Appartenenza al gruppo **Domain Admins** o **Enterprise Admins**, o equivalente in servizi di dominio Active Directory \(AD DS\) è il requisito minimo necessario per completare questa procedura.  Ulteriori informazioni sull'utilizzo degli account appropriati e appartenenze [dominio gruppi predefiniti locali e](https://go.microsoft.com/fwlink/?LinkId=83477) \ (http:///\/ go.microsoft.com\/fwlink\ /? LinkId\ = 83477\).   
  
### <a name="to-distribute-certificates-to-client-computers-by-using-group-policy"></a>Per distribuire i certificati ai computer client utilizzando criteri di gruppo  
  
1.  In un controller di dominio nella foresta dell'organizzazione partner account, avviare il **Gestione criteri di gruppo** snap-in.  
  
2.  Trovare un \(GPO\) oggetto Criteri di gruppo esistente o creare un nuovo oggetto Criteri di gruppo per contenere le impostazioni del certificato. Assicurarsi che l'oggetto Criteri di gruppo sia associato al dominio, sito o un'unità organizzativa \(OU\) in cui risiedono gli account utente e computer appropriati.  
  
3.  Fare clic con l'oggetto Criteri di gruppo, quindi fare clic su **modifica**.  
  
4.  Nell'albero della console, aprire **criteri di chiave Computer Configuration\\Policies\\Windows sicurezza Settings\\Public**, fare clic con **autorità di certificazione radice attendibili**, quindi fare clic su **importazione**.  
  
5.  Nel **importazione guidata certificati** pagina, fare clic su **Avanti**.  
  
6.  Nel **File da importare**, digitare il percorso dei file di certificato appropriato \ (ad esempio, \\\fs1\\c$\\fs1.cer\), quindi fare clic su **Avanti**.  
  
7.  Nel **archivio certificati** pagina, fare clic su **colloca tutti i certificati nel seguente archivio**, quindi fare clic su **Avanti**.  
  
8.  Nel **il completamento dell'importazione guidata certificati** pagina, verificare che le informazioni fornite siano corretto e quindi fare clic su **fine**.  
  
9. Ripetere i passaggi da 2 a 6 per aggiungere certificati aggiuntivi per ogni server federativo nella farm.  
