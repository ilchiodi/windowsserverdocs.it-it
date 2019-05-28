---
ms.assetid: cf32926a-2083-408b-a264-2cad179ed18a
title: Distribuisce certificati ai computer Client mediante criteri di gruppo
description: ''
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 11cdd9c75ca588ebeac9387e6512fee439621bf8
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/24/2019
ms.locfileid: "66192164"
---
# <a name="distribute-certificates-to-client-computers-by-using-group-policy"></a>Distribuisce certificati ai computer Client mediante criteri di gruppo


È possibile utilizzare la procedura seguente per eseguire il push down Secure Sockets Layer appropriato \(SSL\) certificati \(o equivalente, i certificati concatenati a una fonte attendibile\) per i server federativi di account, i server federativi di risorsa e i server Web per ogni computer client nella foresta del partner account con i criteri di gruppo.  
  
L'appartenenza al **Domain Admins** oppure **Enterprise Admins**, o equivalente, in servizi di dominio Active Directory \(Active Directory Domain Services\) è il requisito minimo necessario per completare questa procedura.  Informazioni dettagliate sull'utilizzo degli account appropriati e appartenenze [dominio gruppi predefiniti locali e](https://go.microsoft.com/fwlink/?LinkId=83477) \(http:\/\/go.microsoft.com\/fwlink\/? LinkId\=83477\).   
  
### <a name="to-distribute-certificates-to-client-computers-by-using-group-policy"></a>Per distribuire i certificati ai computer client usando criteri di gruppo  
  
1.  In un controller di dominio nella foresta dell'organizzazione partner account, avviare il **Gestione criteri di gruppo** blocca\-in.  
  
2.  Trovare un oggetto Criteri di gruppo esistente \(oggetto Criteri di gruppo\) o creare un nuovo oggetto Criteri di gruppo per contenere le impostazioni del certificato. Verificare che l'oggetto Criteri di gruppo associato con il dominio, sito o unità organizzativa \(OU\) in cui si trovano gli account utente e computer appropriati.  
  
3.  A destra\-fare clic su oggetto Criteri di gruppo e quindi fare clic su **modifica**.  
  
4.  Nell'albero della console, aprire **configurazione Computer\\i criteri\\le impostazioni di Windows\\le impostazioni di sicurezza\\criteri chiave pubblica**, a destra\-fare clic su **Autorità di certificazione radice attendibili**, quindi fare clic su **importazione**.  
  
5.  Nella pagina **Importazione guidata certificati** fare clic su **Avanti**.  
  
6.  Nel **File da importare** , digitare il percorso ai file di certificato appropriato \(ad esempio \\ \\fs1\\c$\\fs1.cer\)e quindi fare clic su **Successivo**.  
  
7.  Nel **Store certificato** pagina, fare clic su **colloca tutti i certificati nel seguente archivio**e quindi fare clic su **Avanti**.  
  
8.  Nel **completamento dell'importazione guidata certificati** pagina, verificare che le informazioni fornite siano accurate e quindi fare clic su **fine**.  
  
9. Ripetere i passaggi da 2 a 6 per aggiungere i certificati aggiuntivi per ogni server federativo nella farm.  
