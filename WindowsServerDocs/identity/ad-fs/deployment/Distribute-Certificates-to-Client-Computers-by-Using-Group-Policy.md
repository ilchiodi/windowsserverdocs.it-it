---
ms.assetid: cf32926a-2083-408b-a264-2cad179ed18a
title: Distribuire i certificati ai computer client usando Criteri di gruppo
description: ''
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 1d9692bc099174f15b77e792087f4c7055bf85d1
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71359618"
---
# <a name="distribute-certificates-to-client-computers-by-using-group-policy"></a>Distribuire i certificati ai computer client usando Criteri di gruppo


È possibile utilizzare la procedura seguente per eseguire il push del Secure Sockets Layer appropriato \(certificati\) SSL \(o certificati equivalenti concatenati a un\) radice attendibile per i server federativi di account, i server federativo di risorsa e i server Web a ogni computer client nella foresta del partner account utilizzando Criteri di gruppo.  
  
Per completare questa procedura, è necessaria almeno l'appartenenza al gruppo **Domain Admins** o **Enterprise Admins**o a un gruppo equivalente in Active Directory Domain Services \(ad DS\).  Per informazioni dettagliate sull'uso degli account appropriati e delle appartenenze a gruppi, vedere [gruppi predefiniti locali e di dominio](https://go.microsoft.com/fwlink/?LinkId=83477) \(http:\/\/go.Microsoft.com\/fwlink\/? LinkId\=83477\).   
  
### <a name="to-distribute-certificates-to-client-computers-by-using-group-policy"></a>Per distribuire i certificati ai computer client tramite Criteri di gruppo  
  
1.  In un controller di dominio nella foresta dell'organizzazione partner account avviare lo snap-in di **gestione Criteri di gruppo**\-in.  
  
2.  Trovare un oggetto Criteri di gruppo esistente \(oggetto Criteri di gruppo\) o creare un nuovo oggetto Criteri di gruppo per contenere le impostazioni del certificato. Verificare che l'oggetto Criteri di gruppo sia associato al dominio, al sito o all'unità organizzativa \(OU\) in cui risiedono gli account utente e computer appropriati.  
  
3.  A destra\-fare clic sull'oggetto Criteri di gruppo e quindi fare clic su **modifica**.  
  
4.  Nell'albero della console aprire **Configurazione Computer\\criteri\\impostazioni di Windows\\impostazioni di sicurezza\\criteri chiave pubblica**, fare clic con il pulsante\-destro del mouse su **autorità di certificazione radice attendibili**, quindi fare clic su **Importa**.  
  
5.  Nella pagina **Importazione guidata certificati** fare clic su **Avanti**.  
  
6.  Nella pagina **file da importare** Digitare il percorso dei file di certificato appropriati \(ad esempio \\\\FS1\\c $\\FS1. cer\)e quindi fare clic su **Avanti**.  
  
7.  Nella pagina **archivio certificati** fare clic su **Inserisci tutti i certificati nel seguente archivio**, quindi fare clic su **Avanti**.  
  
8.  Nella pagina **completamento dell'importazione guidata certificati** verificare che le informazioni fornite siano accurate, quindi fare clic su **fine**.  
  
9. Ripetere i passaggi da 2 a 6 per aggiungere ulteriori certificati per ogni server federativo della farm.  
