---
title: Panoramica delle password
description: Sicurezza di Windows Server
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
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59869602"
---
# <a name="passwords-overview"></a>Panoramica delle password

>Si applica a: Windows Server (canale semestrale), Windows Server 2016

Questo argomento destinato ai professionisti IT descrive le password come utilizzata per i sistemi operativi Windows e i collegamenti alla documentazione e discussioni sull'utilizzo delle password in una strategia di gestione delle credenziali.

## <a name="BKMK_OVER"></a>Descrizione funzionalità
Sistemi operativi e applicazioni odierne vengono progettate relativi alle password e anche se si usano le smart card o sistemi biometrici, tutti gli account hanno ancora le password e possono comunque essere utilizzati in alcuni casi. Alcuni account, in particolare gli account usati per eseguire i servizi, non è possibile anche utilizzare smart card e biometrico token e pertanto necessario utilizzare una password per l'autenticazione. Windows protegge le password con hash di crittografia.

Per altre informazioni sulle password di Windows, vedere [Panoramica tecnica sulle password](https://technet.microsoft.com/library/hh994558(WS.10).aspx).

## <a name="BKMK_APP"></a>Applicazioni pratiche
In Windows e molti altri sistemi operativi, il metodo più comune per autenticare l'identità dell'utente consiste nell'utilizzare un segreto passphrase o una password. Proteggere l'ambiente di rete richiede password complesse utilizzate da tutti gli utenti. Ciò consente di evitare il rischio di un utente malintenzionato indovinare una password debole, se tramite metodi manuali o usando gli strumenti, per acquisire le credenziali di un account utente compromessi. Ciò vale soprattutto per gli account amministrativi. Quando si modifica una password complessa regolarmente, riduce la probabilità di compromettere tale account di un attacco di password.

## <a name="BKMK_NEW"></a>Funzionalità nuove e modificate
In Windows Server 2012 e Windows 8, sono le nuove password grafiche. Password grafiche sono una combinazione di un'immagine selezionata dell'utente insieme a una serie di movimenti. La funzionalità Picture password è disabilitata nel dominio\-aggiunto al computer. Sono elencati collegamenti a ulteriori informazioni sulle password grafiche [vedere anche](#BKMK_LINKS) sotto.

È non stata apportata alcuna modifica alle funzionalità di password in Windows Server 2012 e Windows 8. Nessun nuovo impostazioni di criteri di gruppo sono state aggiunte. Tuttavia, miglioramenti sono state apportate nella credenziale \(e la password\) gestione, ad esempio con password grafiche, casella di sicurezza delle credenziali e accesso a Windows 8 con un account Microsoft, precedentemente noto come Windows Live ID .

## <a name="BKMK_DEP"></a>Funzionalità deprecate
È stata deprecata alcuna funzionalità di password in Windows Server 2012 e Windows 8.

## <a name="BKMK_SOFT"></a>Requisiti software
Negli ambienti aziendali, le password vengono in genere gestite con Active Directory Domain Services. Le password possono essere gestite nel computer locale usando le impostazioni di criteri Password le impostazioni di sicurezza, criteri di Account locali.

## <a name="BKMK_LINKS"></a>Vedere anche
Questa tabella sono elencate risorse aggiuntive per le funzionalità di password, la tecnologia e le credenziali di gestione.

|Tipo di contenuto|Riferimenti|
|--------|-------|
|**Documentazione dello scenario**|[Protezione dell'identità digitale](http://blogs.msdn.com/b/b8/archive/2011/12/14/protecting-your-digital-identity.aspx)|
|**Operazioni**|[Utenti e computer active Directory](https://technet.microsoft.com/library/cc754217.aspx)|
|**Risoluzione dei problemi**|[Scoprire quando scade la Password \- Blog di Active Directory PowerShell](http://blogs.msdn.com/b/adpowershell/archive/2010/08/09/9970198.aspx)|
|**Sicurezza**| Windows Server 2008 R2 e Windows 7 [Guida alle minacce e contromisure: Criteri di account](https://technet.microsoft.com/library/hh125920(v=ws.10).aspx)<br /><br />Materiale sussidiario per [modificare e creare le password complesse](https://www.microsoft.com/security/online-privacy/passwords-create.aspx)|
|**Strumenti e impostazioni**|[Riferimento delle impostazioni di criteri di gruppo per Windows e Windows Server nell'area download Microsoft](https://www.microsoft.com/download/en/details.aspx?amp;displaylang=en&displaylang=en&id=25250)|
|**Risorse della community**|[Protezione dell'identità digitale](http://blogs.msdn.com/b/b8/archive/2011/12/14/protecting-your-digital-identity.aspx)<br /><br />[Accesso a Windows 8 con un Windows Live ID](http://blogs.msdn.com/b/b8/archive/2011/09/26/signing-in-to-windows-8-with-a-windows-live-id.aspx)<br /><br />[Accesso con una password grafica](http://blogs.msdn.com/b/b8/archive/2011/12/16/signing-in-with-a-picture-password.aspx)<br /><br />[Ottimizzazione della sicurezza delle password immagine](http://blogs.msdn.com/b/b8/archive/2011/12/19/optimizing-picture-password-security.aspx)|


