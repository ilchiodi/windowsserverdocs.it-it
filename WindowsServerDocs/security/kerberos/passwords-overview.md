---
title: Panoramica delle password
description: Sicurezza di Windows Server
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 27d456dd274b917233f0484f055b679dc8c73214
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71403503"
---
# <a name="passwords-overview"></a>Panoramica delle password

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016

Questo argomento per i professionisti IT descrive le password usate nei sistemi operativi Windows e fornisce collegamenti alla documentazione e alle discussioni sull'uso delle password in una strategia di gestione delle credenziali.

## <a name="BKMK_OVER"></a>Descrizione della funzionalità
Attualmente i sistemi operativi e le applicazioni sono progettati per aggirare le password e anche se si usano smart card o sistemi biometrici, tutti gli account hanno ancora password e possono comunque essere usati in alcune circostanze. Alcuni account, in particolare gli account usati per eseguire servizi, non possono anche usare Smart Card e token biometrici e pertanto devono usare una password per l'autenticazione. Windows protegge le password usando gli hash crittografici.

Per ulteriori informazioni sulle password di Windows, vedere la [Panoramica tecnica sulle password](https://technet.microsoft.com/library/hh994558(WS.10).aspx).

## <a name="BKMK_APP"></a>Applicazioni pratiche
In Windows e in molti altri sistemi operativi, il metodo più comune per autenticare l'identità di un utente consiste nell'usare una passphrase o una password segreta. Per proteggere l'ambiente di rete, è necessario che tutti gli utenti debbano usare password complesse. Questo consente di evitare la minaccia di un utente malintenzionato che indovina una password vulnerabile, sia tramite metodi manuali che tramite strumenti, per acquisire le credenziali di un account utente compromesso. Questa operazione è particolarmente valida per gli account amministrativi. Quando si modifica regolarmente una password complessa, si riduce la probabilità che un attacco con password compromettere l'account.

## <a name="BKMK_NEW"></a>Funzionalità nuove e modificate
In Windows Server 2012 e Windows 8 le password per le immagini sono nuove. Le password immagine sono una combinazione di un'immagine selezionata dall'utente associata a una serie di movimenti. La funzionalità password immagine è disabilitata nel dominio\-i computer aggiunti. I collegamenti ad altre informazioni sulle password dell'immagine sono elencati in [vedere anche](#BKMK_LINKS) di seguito.

Non sono state apportate modifiche alle funzionalità delle password in Windows Server 2012 e Windows 8. Non sono state aggiunte nuove impostazioni di Criteri di gruppo. Tuttavia, sono stati apportati miglioramenti e miglioramenti nelle credenziali \(e gestione\) delle password, ad esempio con le password di immagine, la casella di sicurezza delle credenziali e l'accesso a Windows 8 con un account Microsoft, precedentemente noto come Windows Live ID.

## <a name="BKMK_DEP"></a>Funzionalità deprecate
Nessuna funzionalità password è stata deprecata in Windows Server 2012 e Windows 8.

## <a name="BKMK_SOFT"></a>Requisiti software
Negli ambienti aziendali, le password vengono in genere gestite con Active Directory Domain Services. Le password possono anche essere gestite nel computer locale usando le impostazioni in impostazioni di protezione locali, criteri account e password.

## <a name="BKMK_LINKS"></a>Vedere anche
In questa tabella sono elencate risorse aggiuntive per le funzionalità delle password, la tecnologia e la gestione delle credenziali.

|Tipo di contenuto|Riferimenti|
|--------|-------|
|**Documentazione dello scenario**|[Protezione dell'identità digitale](http://blogs.msdn.com/b/b8/archive/2011/12/14/protecting-your-digital-identity.aspx)|
|**Operazioni**|[Utenti e computer Active Directory](https://technet.microsoft.com/library/cc754217.aspx)|
|**Risoluzione dei problemi**|[Scopri quando la password scade \- Blog di Active Directory PowerShell](http://blogs.msdn.com/b/adpowershell/archive/2010/08/09/9970198.aspx)|
|**Sicurezza**| [Guida alle minacce e alle contromisure di](https://technet.microsoft.com/library/hh125920(v=ws.10).aspx) windows Server 2008 R2 e Windows 7: criteri account<br /><br />Linee guida per la [modifica e la creazione di password complesse](https://www.microsoft.com/security/online-privacy/passwords-create.aspx)|
|**Strumenti e impostazioni**|[Informazioni di riferimento sulle impostazioni di Criteri di gruppo per Windows e Windows Server nell'area download Microsoft](https://www.microsoft.com/download/en/details.aspx?amp;displaylang=en&displaylang=en&id=25250)|
|**Risorse della community**|[Protezione dell'identità digitale](http://blogs.msdn.com/b/b8/archive/2011/12/14/protecting-your-digital-identity.aspx)<br /><br />[Accesso a Windows 8 con Windows Live ID](http://blogs.msdn.com/b/b8/archive/2011/09/26/signing-in-to-windows-8-with-a-windows-live-id.aspx)<br /><br />[Accesso con una password immagine](http://blogs.msdn.com/b/b8/archive/2011/12/16/signing-in-with-a-picture-password.aspx)<br /><br />[Ottimizzazione della sicurezza delle password per le immagini](http://blogs.msdn.com/b/b8/archive/2011/12/19/optimizing-picture-password-security.aspx)|


