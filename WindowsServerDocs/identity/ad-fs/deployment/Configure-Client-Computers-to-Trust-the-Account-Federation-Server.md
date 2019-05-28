---
ms.assetid: 4ae26970-e42e-4e69-887a-b16d2f8d0695
title: Configurare i computer Client per considerare attendibile il Server federativo di Account
description: ''
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: e28d050a9aa40c015af16a665e90535cb810b4ff
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/24/2019
ms.locfileid: "66192335"
---
# <a name="configure-client-computers-to-trust-the-account-federation-server"></a>Configurare i computer Client per considerare attendibile il Server federativo di Account

In modo che i computer client possono accedere alle applicazioni federate con Active Directory Federation Services \(ADFS\), è innanzitutto necessario configurare le impostazioni di Internet Explorer in ogni computer client in modo che considera attendibile il browser il server federativo di account. È possibile farlo manualmente o tramite criteri di gruppo, a seconda delle preferenze amministrative, completando una delle procedure riportate di seguito.  
  
## <a name="configuring-internet-explorer-settings-manually"></a>Configurazione delle impostazioni di Internet Explorer manualmente  
È possibile utilizzare la procedura seguente per configurare manualmente le impostazioni di Internet Explorer di ogni utente per il supporto della federazione tramite AD FS. Se più utenti utilizzano un singolo computer, completare questa procedura più volte, una volta per ogni profilo utente.  
  
Per eseguire questa procedura, accedere come utente che accederanno alle applicazioni federate. Si tratta di un profilo\-impostazione specifica. È pertanto necessario aggiungere manualmente questa impostazione per ogni profilo esistente in un computer specifico.  
  
#### <a name="to-manually-configure-client-computers-to-trust-the-account-federation-server"></a>Per configurare manualmente i computer client per considerare attendibile il server federativo di account  
  
1.  Nel computer client, avviare Internet Explorer.  
  
2.  Nel menu **Strumenti**, fare clic su **Opzioni Internet**.  
  
3.  Nel **sicurezza** scheda, fare clic sui **intranet locale** icona e quindi fare clic su **siti**.  
  
4.  Fare clic su **avanzate**e nella **aggiungere questo sito Web all'area**, digitare Domain Name System completo \(DNS\) nome del server federativo di account \(, ad esempio, https :\/\/fs1.fabrikam.com\), quindi fare clic su **Add**.  
  
5.  Fare tre volte clic su **OK**.  
  
## <a name="configuring-internet-explorer-settings-by-using-grouppolicy"></a>Configurazione delle impostazioni di Internet Explorer utilizzando criteri di gruppo  
Per la maggior parte delle distribuzioni, è consigliabile utilizzare criteri di gruppo per inviare le impostazioni di Internet Explorer appropriate a ogni computer client.  
  
L'appartenenza al **Domain Admins** oppure **Enterprise Admins**, o equivalente, in servizi di dominio Active Directory \(Active Directory Domain Services\) è il requisito minimo necessario per completare questa procedura.  Informazioni dettagliate sull'utilizzo degli account appropriati e appartenenze [dominio gruppi predefiniti locali e](https://go.microsoft.com/fwlink/?LinkId=83477) \(http:\/\/go.microsoft.com\/fwlink\/? LinkId\=83477\).   
  
#### <a name="to-configure-client-computers-to-trust-the-account-federation-server-by-using-grouppolicy"></a>Configurare i computer client per considerare attendibile il server federativo di account usando criteri di gruppo  
  
1.  In un controller di dominio nella foresta dell'organizzazione partner account, avviare il **Gestione criteri di gruppo** blocca\-in.  
  
2.  Trovare l'oggetto Criteri di gruppo appropriato \(GPO\), a destra\-selezionarlo e quindi fare clic su **modifica**.  
  
3.  Nell'albero della console, aprire **configurazione utente\\Preferences\\le impostazioni di Windows\\manutenzione di Internet Explorer**, quindi fare clic su **sicurezza**.  
  
4.  Nel riquadro dei dettagli fare doppio\-fare clic su **aree di sicurezza e classificazioni del contenuto**.  
  
5.  Sotto **aree di sicurezza e Privacy**, fare clic su **Importa le impostazioni di privacy e le aree di sicurezza**, quindi fare clic su **modificare le impostazioni di**.  
  
6.  Fare clic su **intranet locale**, quindi fare clic su **siti**.  
  
7.  Nella **aggiungere il sito Web all'area**, digitare il nome DNS completo del server federativo di account \(ad esempio, https:\/\/fs1.fabrikam.com\), fare clic su **Aggiungi**, quindi fare clic su **Chiudi**.  
  
8.  Fare clic su **OK** due volte per applicare tali modifiche a criteri di gruppo.  
  
