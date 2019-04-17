---
title: Panoramica delle password
description: Protezione di Windows Server
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: security-kerberos
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: f608960e-2039-4c91-9c8c-9b81053c675e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/12/2016
ms.openlocfilehash: 6c1b8d56b5c0da738e7dae5c0072be81040f90d8
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/12/2017
---
# <a name="passwords-overview"></a>Panoramica delle password

>Si applica a: Windows Server (canale annuale e virgola), Windows Server 2016

Questo argomento per professionisti IT descrive le password utilizzate in sistemi operativi Windows e i collegamenti alla documentazione e le discussioni sull'utilizzo delle password in una strategia di gestione delle credenziali.

## <a name="BKMK_OVER"></a>Descrizione delle funzionalità
Architettura di sistemi operativi e applicazioni oggi alle password e anche se si utilizzano smart card o sistemi biometrici, tutti gli account hanno ancora le password e possono comunque essere utilizzati in alcune circostanze. Alcuni account, in particolare gli account utilizzati per eseguire i servizi, non può anche utilizzare smart card e token biometrici e pertanto necessario utilizzare una password per l'autenticazione. Windows consente di proteggere le password con hash di crittografia.

For more information about Windows passwords, see [Passwords Technical Overview](https://technet.microsoft.com/library/hh994558(WS.10).aspx).

## <a name="BKMK_APP"></a>Applicazioni pratiche
In Windows e molti altri sistemi operativi, il metodo più comune per l'autenticazione dell'identità di un utente consiste nell'usare una passphrase segreta o una password. Protezione dell'ambiente di rete è necessario utilizzare password complesse da tutti gli utenti. Ciò aiuta a evitare il rischio di un utente malintenzionato indovinare una password vulnerabile, tramite metodi manuali o utilizzando gli strumenti, per acquisire le credenziali di un account utente compromessi. Ciò vale soprattutto per gli account amministrativi. Quando si modifica una password complessa regolarmente, riduce il rischio di un attacco di password compromettere tale account.

## <a name="BKMK_NEW"></a>Funzionalità nuove e modificate
In Windows Server 2012 e Windows 8, le password dell'immagine sono nuove. Le password dell'immagine sono una combinazione di un'immagine selezionata utente abbinata a una serie di movimenti. La funzionalità di password immagine è disattivata sul computer appartenenti a un domain \. Collegamenti a ulteriori informazioni sulle password immagine vengono elencati [vedere anche](#BKMK_LINKS) di sotto.

È non stata apportata alcuna modifica alla funzionalità delle password in Windows Server 2012 e Windows 8. Nessun nuove impostazioni di criteri di gruppo sono stati aggiunti. Tuttavia, miglioramenti apportati in Gestione \(and password\) delle credenziali, ad esempio con password grafiche, casella di sicurezza delle credenziali e accedere a Windows 8 con un account Microsoft, precedentemente noto come un Windows Live ID.

## <a name="BKMK_DEP"></a>Funzionalità deprecate
La funzionalità di password non è stata deprecata in Windows Server 2012 e Windows 8.

## <a name="BKMK_SOFT"></a>Requisiti software
In ambienti aziendali, le password vengono gestite in genere con servizi di dominio Active Directory. Le password possono essere gestite anche nel computer locale usando le impostazioni in Criteri di Password criteri Account, impostazioni di sicurezza locali.

## <a name="BKMK_LINKS"></a>Vedere anche
Questa tabella elenca le risorse aggiuntive per la funzionalità di password, gestione tecnologia e le credenziali.

|Tipo di contenuto|Riferimenti|
|--------|-------|
|**Documentazione dello scenario**|[Protezione dell'identità digitale](http://blogs.msdn.com/b/b8/archive/2011/12/14/protecting-your-digital-identity.aspx)|
|**Operazioni**|[Utenti e computer active Directory](https://technet.microsoft.com/library/cc754217.aspx)|
|**Risoluzione dei problemi**|[Scopri quando la Password scade \-Blog di PowerShell Active Directory](http://blogs.msdn.com/b/adpowershell/archive/2010/08/09/9970198.aspx)|
|**Protezione**| Windows Server 2008 R2  and  Windows 7 [Threats and Countermeasures Guide: Account Policies](https://technet.microsoft.com/library/hh125920(v=ws.10).aspx)<br /><br />Guidance to [change and create strong passwords](https://www.microsoft.com/security/online-privacy/passwords-create.aspx)|
|**Strumenti e impostazioni**|[Impostazioni di criteri di gruppo per Windows e Windows Server nell'area Download Microsoft](https://www.microsoft.com/download/en/details.aspx?amp;displaylang=en&displaylang=en&id=25250)|
|**Risorse della community**|[Protezione dell'identità digitale](http://blogs.msdn.com/b/b8/archive/2011/12/14/protecting-your-digital-identity.aspx)<br /><br />[Accesso a Windows 8 con un Windows Live ID](http://blogs.msdn.com/b/b8/archive/2011/09/26/signing-in-to-windows-8-with-a-windows-live-id.aspx)<br /><br />[Accesso con una password grafica](http://blogs.msdn.com/b/b8/archive/2011/12/16/signing-in-with-a-picture-password.aspx)<br /><br />[Ottimizzazione della protezione di password immagine](http://blogs.msdn.com/b/b8/archive/2011/12/19/optimizing-picture-password-security.aspx)|


