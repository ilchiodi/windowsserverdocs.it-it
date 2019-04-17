---
title: Panoramica di NTLM
description: Protezione di Windows Server
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: security-kerberos
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 773909fd-c0bc-498a-95fc-bb452ec04d90
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/12/2016
ms.openlocfilehash: 523fb71304ae55d17203cab4d1c5a17551bf8fdf
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/12/2017
---
# <a name="ntlm-overview"></a>Panoramica di NTLM

>Si applica a: Windows Server (canale annuale e virgola), Windows Server 2016

Questo argomento, destinato ai professionisti IT descrive NTLM, eventuali modifiche apportate alle funzionalità e vengono forniti collegamenti a risorse tecniche per l'autenticazione di Windows e NTLM per Windows Server 2012 e versioni precedenti.

## <a name="BKMK_OVER"></a>Descrizione delle funzionalità
L'autenticazione NTLM è una famiglia di protocolli di autenticazione Windows inclusi nel Msv1\_0.dll di Windows. I protocolli di autenticazione NTLM comprendono LAN Manager versione 1 e 2 ed NTLM versione 1 e 2. I protocolli di autenticazione NTLM autenticano utenti e computer basati su un meccanismo challenge\/risposta che dimostra a un server o un controller di dominio che un utente conosce la password associata a un account. Quando viene utilizzato il protocollo NTLM, un server di risorse deve eseguire una delle operazioni seguenti per verificare l'identità di un computer o utente ogni volta che è necessario un nuovo token di accesso:

-   Se si tratta di un account di dominio, contattare un servizio di autenticazione di dominio nel controller di dominio per il dominio account del computer o dell'utente.

-   Cercare l'account del computer o dell'utente nel database di account locale, se l'account è un account locale.

## <a name="BKMK_APP"></a>Applicazioni correnti
L'autenticazione NTLM è ancora supportata e deve essere utilizzato per l'autenticazione di Windows con sistemi configurati come membri di un gruppo di lavoro. L'autenticazione NTLM viene inoltre utilizzato per l'autenticazione di accesso locale nei controller di dominio. Autenticazione Kerberos versione 5 è il metodo di autenticazione preferito per gli ambienti Active Directory, ma un'applicazione di Microsoft o fornitori non Microsoft potrebbe comunque usare NTLM.

Per ridurre l'utilizzo del protocollo NTLM in un ambiente IT richiede a livello di conoscenza dei requisiti delle applicazioni distribuite in NTLM e le strategie e passaggi necessari per configurare gli ambienti di elaborazione per l'uso di altri protocolli. Per scoprire come usare NTLM per ridurre selettivamente il traffico NTLM in che sono stati aggiunti nuovi strumenti e impostazioni. Per informazioni su come analizzare e ridurre l'utilizzo di NTLM negli ambienti, vedere [Introduzione alla restrizione dell'autenticazione NTLM](https://technet.microsoft.com/library/dd560653(v=ws.10).aspx) per accedere a Guida all'utilizzo NTLM limitazione e al controllo.

## <a name="BKMK_NEW"></a>Funzionalità nuove e modificate
Sono state apportate modifiche delle funzionalità per NTLM per Windows Server 2012.

## <a name="BKMK_DEP"></a>Funzionalità rimosse o deprecate
Non esiste alcuna funzionalità rimosse o deprecate per NTLM per Windows Server 2012.

## <a name="BKMK_INSTALL"></a>Informazioni su Server Manager
Impossibile configurare NTLM da Server Manager. È possibile utilizzare le impostazioni di criteri di sicurezza o criteri di gruppo per gestire l'utilizzo dell'autenticazione NTLM tra sistemi di computer. In un dominio, Kerberos è il protocollo di autenticazione predefinito.

## <a name="BKMK_LINKS"></a>Vedere anche
Nella tabella seguente elenca le risorse pertinenti per NTLM e altre tecnologie di autenticazione di Windows.

|Tipo di contenuto|Riferimenti|
|--------|-------|
|**Valutazione del prodotto**|[Introduzione alla restrizione dell'autenticazione NTLM](https://technet.microsoft.com/library/dd560653.aspx)<br /><br />[Modifiche all'autenticazione NTLM](https://technet.microsoft.com/library/dd566199.aspx)|
|**Pianificazione**|[Guida alla modellazione delle minacce infrastruttura IT](https://technet.microsoft.com/library/dd941826.aspx)<br /><br />[Minacce e contromisure: impostazioni di sicurezza in Windows Server 2003 e Windows XP](https://technet.microsoft.com/library/dd162275.aspx)<br /><br />[Guida alle minacce e contromisure: impostazioni di sicurezza in Windows Server 2008 e Windows Vista](https://technet.microsoft.com/library/dd349791.aspx)<br /><br />[Guida alle minacce e contromisure: impostazioni di sicurezza in Windows Server 2008 R2 e Windows 7](https://technet.microsoft.com/library/hh125921.aspx)|
|**Distribuzione**|[Protezione estesa per l'autenticazione](https://support.microsoft.com/kb/968389)<br /><br />[Guida di utilizzo NTLM limitazione e al controllo](https://technet.microsoft.com/library/jj865674(v=ws.10).aspx)<br /><br />[Chiedere al Team di servizi di Directory: sul blocco NTLM e: analisi delle applicazioni e metodologie di controllo in Windows 7](https://blogs.technet.com/askds/archive/2009/10/08/ntlm-blocking-and-you-application-analysis-and-auditing-methodologies-in-windows-7.aspx)<br /><br />[Blog sull'autenticazione Windows](https://blogs.technet.com/authentication/)<br /><br />[Configurazione di MaxConcurrentAPI per l'autenticazione NTLM pass\-through](https://social.technet.microsoft.com/wiki/contents/articles/9759.configuring-maxconcurrentapi-for-ntlm-pass-through-authentication.aspx)|
|**Sviluppo**|[Microsoft NTLM \(Windows\)](https://msdn.microsoft.com/library/aa378749(VS.85).aspx)<br /><br />[\[MS\-NLMP\]: specifica del protocollo di autenticazione NT LAN Manager \(NTLM\)](https://msdn.microsoft.com/library/cc236621(PROT.10).aspx)<br /><br />[\[MS\-NNTP\]: Authentication \(NTLM\) NT LAN Manager: News Transfer Protocol \(NNTP\) estensione di rete](https://msdn.microsoft.com/library/cc236774(PROT.10).aspx)<br /><br />[\[MS\-NTHT\]: specifica del protocollo HTTP NTLM](https://msdn.microsoft.com/library/cc237488(PROT.10).aspx)|
|**Risoluzione dei problemi**|Non è ancora disponibile|
|**Risorse della community**|[È gestire risorse obsolete: colli di bottiglia NTLM e runtime RPC](http://blogs.technet.com/b/askds/archive/2011/09/15/is-this-horse-dead-yet-ntlm-bottlenecks-and-the-rpc-runtime.aspx)|



