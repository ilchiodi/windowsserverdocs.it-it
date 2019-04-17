---
ms.assetid: 4ae26970-e42e-4e69-887a-b16d2f8d0695
title: Configurare i computer Client per considerare attendibile il Server federativo di Account
description: 
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: bdfb086c8177e72c074ac5b5b1a38aac49c4082c
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/12/2017
---
# <a name="configure-client-computers-to-trust-the-account-federation-server"></a>Configurare i computer Client per considerare attendibile il Server federativo di Account

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

In modo che i computer client possono accedere alle applicazioni federate usando \(AD FS\) Active Directory Federation Services, è prima necessario configurare le impostazioni di Internet Explorer in ogni computer client in modo che il browser considera attendibile il server federativo di account. È possibile farlo manualmente o tramite criteri di gruppo, a seconda della tua preferenza amministrativo, completando una delle procedure riportate di seguito.  
  
## <a name="configuring-internet-explorer-settings-manually"></a>Configurazione delle impostazioni di Internet Explorer manualmente  
È possibile utilizzare la procedura seguente per configurare manualmente le impostazioni di Internet Explorer di ciascun utente per supportare la federazione con AD FS. Se più utenti di usare un unico computer, completare questa procedura più volte, una volta per ogni profilo utente.  
  
Per eseguire questa procedura, accedere come utente che accederanno alle applicazioni federate. Si tratta di un'impostazione specifica profile\. Pertanto, è necessario aggiungere manualmente questa impostazione per ogni profilo esistente in un computer specifico.  
  
#### <a name="to-manually-configure-client-computers-to-trust-the-account-federation-server"></a>Per configurare manualmente i computer client per considerare attendibile il server federativo di account  
  
1.  Nel computer client, avviare Internet Explorer.  
  
2.  Nel **strumenti** menu, fare clic su **Opzioni Internet**.  
  
3.  Nel **sicurezza** scheda, fare clic su di **intranet locale** icona, quindi fare clic su **siti**.  
  
4.  Fare clic su **avanzate**e in **aggiungere il sito Web all'area**, digitare il nome del server federativo di account \(DNS\) Domain Name System \ (ad esempio, https:\/\/fs1.fabrikam.com\), quindi fare clic su **Aggiungi**.  
  
5.  Fare clic su **OK** tre volte.  
  
## <a name="configuring-internet-explorer-settings-by-using-group-policy"></a>Configurazione delle impostazioni di Internet Explorer utilizzando criteri di gruppo  
Per la maggior parte delle distribuzioni, è consigliabile utilizzare criteri di gruppo per effettuare il push le impostazioni di Internet Explorer appropriate per ogni computer client.  
  
Appartenenza al gruppo **Domain Admins** o **Enterprise Admins**, o equivalente in servizi di dominio Active Directory \(AD DS\) è il requisito minimo necessario per completare questa procedura.  Ulteriori informazioni sull'utilizzo degli account appropriati e appartenenze [dominio gruppi predefiniti locali e](https://go.microsoft.com/fwlink/?LinkId=83477) \ (http:///\/ go.microsoft.com\/fwlink\ /? LinkId\ = 83477\).   
  
#### <a name="to-configure-client-computers-to-trust-the-account-federation-server-by-using-group-policy"></a>Per configurare i computer client per considerare attendibile il server federativo di account utilizzando criteri di gruppo  
  
1.  In un controller di dominio nella foresta dell'organizzazione partner account, avviare il **Gestione criteri di gruppo** snap-in.  
  
2.  Trovare \(GPO\) l'oggetto Criteri di gruppo appropriato, fare clic con, quindi fare clic su **modifica**.  
  
3.  Nell'albero della console, aprire **utente configurazione computer\\preferenze\\impostazioni Settings\\Internet Explorer manutenzione**, quindi fare clic su **sicurezza**.  
  
4.  Nel riquadro dei dettagli fare doppio clic **aree di sicurezza e classificazioni del contenuto**.  
  
5.  In **aree di sicurezza e Privacy**, fare clic su **Importa le impostazioni di privacy e le aree di sicurezza**, quindi fare clic su **modifica le impostazioni di**.  
  
6.  Fare clic su **intranet locale**, quindi fare clic su **siti**.  
  
7.  In **aggiungere il sito Web all'area**, digitare il nome DNS completo del server federativo di account \ (ad esempio, https:\/\/fs1.fabrikam.com\), fare clic su **Aggiungi**, quindi fare clic su **Chiudi**.  
  
8.  Fare clic su **OK** due volte per applicare le modifiche ai criteri di gruppo.  
  
