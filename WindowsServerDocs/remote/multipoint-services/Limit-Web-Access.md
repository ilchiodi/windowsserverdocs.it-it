---
title: Limitare l'accesso al Web
description: Informazioni su come limitare l'accesso degli utenti a Internet in MultiPoint Services
ms.custom: na
ms.date: 07/08/2016
ms.prod: windows-server-threshold
ms.technology: multipoint-services
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 044f2fd5-5b87-42bb-ba0d-c06516ac48c8
author: lizap
manager: dongill
ms.author: elizapo
ms.openlocfilehash: cb91914608110d26ff2db8eff1cd28d26d04669b
ms.sourcegitcommit: 9f955be34c641b58ae8b3000768caa46ad535d43
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/27/2019
ms.locfileid: "68590346"
---
# <a name="limit-web-access"></a>Limitare l'accesso al Web
Oltre a monitorare le attività degli utenti sui singoli desktop, l'utente amministratore può limitare l'accesso degli utenti ai siti Web specificati indicando i siti Web e i siti Web consentiti a cui si desidera bloccare l'accesso utente.  
  
## <a name="to-limit-web-access-on-a-station"></a>Per limitare l'accesso al Web su una stazione  
  
1. Nel Dashboard MultiPoint, nella scheda **limitazione Web** , fare clic su **Configura**. Verrà visualizzata la pagina **Configure Web Limiting** (Configura limitazione Web), con l'elenco dei siti a cui l'utente può accedere.  
  
2. Fare clic sull'immagine di anteprima della stazione utente su cui si desidera limitare l'accesso al Web.  
  
3. In **Selected Item Tasks** (Attività elemento selezionato) fare clic su **Limit web access on this station** (Limita l'accesso Web su questa stazione). Verrà visualizzata la pagina **Configure Web Limiting** (Configura limitazione Web), con l'elenco dei siti a cui l'utente può accedere.  
  
4. Per aggiungere un sito consentito, digitare l'indirizzo Web e quindi fare clic su **Add** (Aggiungi).  
  
   > [!NOTE]
   > Ad esempio, l'immissione di "contoso.com" consente o blocca i siti relativi a\.www contoso.com (ad esempio,\.www NewPage.contoso.com). Immettendo "contoso" si consentiranno o limiteranno tutti i siti correlati a Contoso (inclusi contoso.com, contoso.uk e così via).  
  
5. Per rimuovere un indirizzo Web dall'elenco dei siti consentiti, fare clic sull'indirizzo Web a cui si vuole proibire l'accesso e quindi fare clic su **Remove** (Rimuovi).  
  
## <a name="to-limit-web-access-on-all-stations"></a>Per limitare l'accesso al Web su tutte le stazioni  
  
1. Nel Dashboard MultiPoint, nella scheda **limitazione Web** , fare clic sul menu a discesa\-avvia, quindi fare clic **su limita accesso Web su tutti i desktop**.  
  
   Verrà visualizzata la pagina **Configure Web Limiting** (Configura limitazione Web), con l'elenco dei siti a cui l'utente può accedere. Effettua una delle seguenti operazioni:  
  
2. Per aggiungere un sito consentito, fare clic su **Allow only these sites** (Consenti solo questi siti), digitare l'indirizzo Web e quindi fare clic su **Add** (Aggiungi).  
  
   Per aggiungere un sito che non si desidera venga visitato da altri utenti, fare clic su **Disallow only these sites** (Proibisci solo questi siti), digitare l'indirizzo Web e quindi fare clic su **Add** (Aggiungi).  
  
   > [!NOTE]
   > Ad esempio, l'immissione di "Contoso.com" consente o blocca i siti relativi a www.contoso.com (ad esempio, www.newpage.contoso.com). Immettendo "contoso" si consentiranno o limiteranno tutti i siti correlati a Contoso (inclusi contoso.com, contoso.uk e così via).  
  
3. Per rimuovere un indirizzo Web dall'elenco dei siti consentiti o bloccati, selezionare tale indirizzo e quindi fare clic su **Remove** (Rimuovi).  
  
## <a name="see-also"></a>Vedere anche  
[Gestire i desktop degli utenti](manage-user-desktops-using-multipoint-dashboard.md)  
