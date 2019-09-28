---
ms.assetid: 4ae26970-e42e-4e69-887a-b16d2f8d0695
title: Configurare i computer client per considerare attendibile il server federativo di account
description: ''
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 8c047f69cb0cb2db57ea33697c49e53a212fe823
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71359856"
---
# <a name="configure-client-computers-to-trust-the-account-federation-server"></a>Configurare i computer client per considerare attendibile il server federativo di account

In modo che i computer client possano accedere correttamente alle applicazioni federate utilizzando Active Directory Federation Services \(AD FS @ no__t-1, è necessario configurare le impostazioni di Internet Explorer in ogni computer client in modo che il browser riconsideri attendibile l'account server federativo. Questa operazione può essere eseguita manualmente o tramite Criteri di gruppo, a seconda delle preferenze amministrative, completando una delle procedure riportate di seguito.  
  
## <a name="configuring-internet-explorer-settings-manually"></a>Configurazione manuale delle impostazioni di Internet Explorer  
È possibile utilizzare la procedura seguente per configurare manualmente le impostazioni di Internet Explorer di ogni utente per supportare la Federazione tramite AD FS. Se più utenti usano un solo computer, completare questa procedura più volte, una volta per ogni profilo utente.  
  
Per eseguire questa procedura, accedere come utente che effettuerà l'accesso alle applicazioni federate. Si tratta di un'impostazione profile @ no__t-0specific. Pertanto, è necessario aggiungere manualmente questa impostazione per ogni profilo presente in un computer specifico.  
  
#### <a name="to-manually-configure-client-computers-to-trust-the-account-federation-server"></a>Per configurare manualmente i computer client in modo da considerare attendibile il server federativo di account  
  
1.  Nel computer client avviare Internet Explorer.  
  
2.  Nel menu **Strumenti**, fare clic su **Opzioni Internet**.  
  
3.  Nella scheda **sicurezza** fare clic sull'icona **Intranet locale** , quindi fare clic su **siti**.  
  
4.  Fare clic su **Avanzate**e in **Aggiungi il sito Web all'area**digitare il nome completo Domain Name System \(DNS @ no__t-3 del server federativo account \(per esempio, https: @no__t -5\/fs1.fabrikam.com @ no__t-7 e quindi fare clic su **Aggiungi.** .  
  
5.  Fare tre volte clic su **OK**.  
  
## <a name="configuring-internet-explorer-settings-by-using-grouppolicy"></a>Configurazione delle impostazioni di Internet Explorer tramite Criteri di gruppo  
Per la maggior parte delle distribuzioni, è consigliabile utilizzare Criteri di gruppo per eseguire il push delle impostazioni di Internet Explorer appropriate a ogni computer client.  
  
Per completare questa procedura, è necessaria almeno l'appartenenza al gruppo **Domain Admins** o **Enterprise Admins**o a un gruppo equivalente in Active Directory Domain Services \(AD DS @ no__t-3.  Per informazioni dettagliate sull'uso degli account appropriati e delle appartenenze a gruppi, vedere \( [gruppi predefiniti locali e](https://go.microsoft.com/fwlink/?LinkId=83477) di\/dominio http:\/\/go.Microsoft.com\/fwlink? LinkId\=83477\).   
  
#### <a name="to-configure-client-computers-to-trust-the-account-federation-server-by-using-grouppolicy"></a>Per configurare i computer client in modo da considerare attendibile il server federativo di account utilizzando Criteri di gruppo  
  
1.  In un controller di dominio nella foresta dell'organizzazione partner account, avviare lo snap-in di **gestione criteri di gruppo** @ no__t-1in.  
  
2.  Trovare l'oggetto Criteri di gruppo appropriato \(GPO @ no__t-1, right @ no__t-2Cliccare it, quindi fare clic su **Edit (modifica**).  
  
3.  Nell'albero della console aprire **Configurazione utente @ no__t-1Preferences @ no__t-2Windows Settings @ no__t-3Internet Explorer Maintenance**, quindi fare clic su **Security**.  
  
4.  Nel riquadro dei dettagli, raddoppiare le aree di sicurezza @ no__t-0click **e le classificazioni del contenuto**.  
  
5.  In **aree di sicurezza e privacy**fare clic su **Importa le impostazioni per la privacy e le aree di sicurezza correnti**, quindi fare clic su **Modifica impostazioni**.  
  
6.  Fare clic su **Intranet locale**e quindi fare clic su **siti**.  
  
7.  In **Aggiungi il sito Web all'area**Digitare il nome DNS completo del server federativo di account \(per esempio, https: @no__t -2\/fs1.fabrikam.com @ no__t-4, fare clic su **Aggiungi**e quindi su **Chiudi**.  
  
8.  Fare clic su **OK** due volte per applicare queste modifiche criteri di gruppo.  
  
