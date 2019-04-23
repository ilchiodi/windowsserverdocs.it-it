---
title: Panoramica di NTLM
description: Sicurezza di Windows Server
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
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59890642"
---
# <a name="ntlm-overview"></a>Panoramica di NTLM

>Si applica a: Windows Server (canale semestrale), Windows Server 2016

Questo argomento destinato ai professionisti IT descrive NTLM, eventuali modifiche apportate alle funzionalità e vengono forniti collegamenti alle risorse tecniche per l'autenticazione di Windows e NTLM per Windows Server 2012 e versioni precedenti.

## <a name="BKMK_OVER"></a>Descrizione funzionalità
L'autenticazione NTLM è una famiglia di protocolli di autenticazione del Msv1 Windows\_0.dll. I protocolli di autenticazione NTLM comprendono LAN Manager versione 1 e 2 ed NTLM versione 1 e 2. I protocolli di autenticazione NTLM autenticano gli utenti e computer in base a una richiesta di verifica\/meccanismo di risposta che dimostra a un server o un controller di dominio che un utente conosca la password associata a un account. Quando viene utilizzato il protocollo NTLM, un server delle risorse deve eseguire una delle azioni seguenti per verificare l'identità di un computer o utente ogni volta che è necessario un nuovo token di accesso:

-   Contattare un servizio di autenticazione del dominio nel controller di dominio per il dominio dell'account del computer o dell'utente, se l'account è un account di dominio.

-   Cercare l'account del computer o dell'utente nel database di account locale, se l'account è un account locale.

## <a name="BKMK_APP"></a>Applicazioni correnti
L'autenticazione NTLM è ancora supportata e deve essere usata per l'autenticazione Windows con i sistemi configurati come membri di un gruppo di lavoro. L'autenticazione NTLM viene usata anche per l'autenticazione di accesso locale sul non\-i controller di dominio. L'autenticazione Kerberos versione 5 è il metodo di autenticazione preferito per gli ambienti Active Directory, ma non\-Microsoft o Microsoft applicazione potrebbe comunque usare NTLM.

Per ridurre l'utilizzo del protocollo NTLM in un ambiente IT è necessario conoscere i requisiti per le applicazioni distribuite in NTLM e le strategie e le procedure richieste per configurare l'uso di altri protocolli negli ambienti informatici. Sono stati aggiunti nuovi strumenti e impostazioni per comprendere come usare NTLM per ridurre selettivamente il traffico NTLM. Per informazioni su come analizzare e ridurre l'utilizzo di NTLM negli ambienti, vedere [Introduzione alla restrizione dell'autenticazione NTLM](https://technet.microsoft.com/library/dd560653(v=ws.10).aspx) per accedere alla Guida alla limitazione e al controllo dell'utilizzo di NTLM.

## <a name="BKMK_NEW"></a>Funzionalità nuove e modificate
Sono state apportate modifiche di funzionalità per NTLM per Windows Server 2012.

## <a name="BKMK_DEP"></a>Funzionalità rimosse o deprecate
Non vi è alcuna funzionalità rimosse o deprecate per NTLM per Windows Server 2012.

## <a name="BKMK_INSTALL"></a>Informazioni su Server Manager
Non è possibile configurare NTLM da Server Manager. Per la gestione dell'utilizzo dell'autenticazione NTLM tra computer è possibile utilizzare le impostazioni relative ai criteri di sicurezza o Criteri di gruppo. In un dominio Kerberos è il protocollo di autenticazione predefinito.

## <a name="BKMK_LINKS"></a>Vedere anche
Nella tabella seguente è riportato l'elenco delle risorse pertinenti per NTLM e altre tecnologie di autenticazione di Windows.

|Tipo di contenuto|Riferimenti|
|--------|-------|
|**Valutazione del prodotto**|[Introduzione alla restrizione dell'autenticazione NTLM](https://technet.microsoft.com/library/dd560653.aspx)<br /><br />[Modifiche all'autenticazione NTLM](https://technet.microsoft.com/library/dd566199.aspx)|
|**Pianificazione**|[Guida alla modellazione delle minacce di infrastruttura IT](https://technet.microsoft.com/library/dd941826.aspx)<br /><br />[Minacce e contromisure: Impostazioni di sicurezza in Windows Server 2003 e Windows XP](https://technet.microsoft.com/library/dd162275.aspx)<br /><br />[Guida ai rischi e alle contromisure: Impostazioni di sicurezza in Windows Server 2008 e Windows Vista](https://technet.microsoft.com/library/dd349791.aspx)<br /><br />[Guida ai rischi e alle contromisure: Impostazioni di sicurezza in Windows Server 2008 R2 e Windows 7](https://technet.microsoft.com/library/hh125921.aspx)|
|**Distribuzione**|[Protezione estesa per l'autenticazione](https://support.microsoft.com/kb/968389)<br /><br />[Guida all'utilizzo NTLM di limitazione e al controllo](https://technet.microsoft.com/library/jj865674(v=ws.10).aspx)<br /><br />[Domande al Team di servizi di Directory: sul blocco NTLM: Analisi delle applicazioni e metodologie di controllo in Windows 7](https://blogs.technet.com/askds/archive/2009/10/08/ntlm-blocking-and-you-application-analysis-and-auditing-methodologies-in-windows-7.aspx)<br /><br />[Blog sull'autenticazione di Windows](https://blogs.technet.com/authentication/)<br /><br />[Configurazione di MaxConcurrentAPI per pass NTLM\-tramite l'autenticazione](https://social.technet.microsoft.com/wiki/contents/articles/9759.configuring-maxconcurrentapi-for-ntlm-pass-through-authentication.aspx)|
|**Sviluppo**|[Microsoft NTLM \(Windows\)](https://msdn.microsoft.com/library/aa378749(VS.85).aspx)<br /><br />[\[MS\-NLMP\]: NT LAN Manager \(NTLM\) specifica del protocollo di autenticazione](https://msdn.microsoft.com/library/cc236621(PROT.10).aspx)<br /><br />[\[MS\-NNTP\]: NT LAN Manager \(NTLM\) autenticazione: Network News Transfer Protocol \(NNTP\) estensione](https://msdn.microsoft.com/library/cc236774(PROT.10).aspx)<br /><br />[\[MS\-NTHT\]: NTLM su una specifica del protocollo HTTP](https://msdn.microsoft.com/library/cc237488(PROT.10).aspx)|
|**Risoluzione dei problemi**|Non ancora disponibile|
|**Risorse della community**|[È inattivo ancora gestire risorse obsolete: Colli di bottiglia NTLM e runtime RPC](http://blogs.technet.com/b/askds/archive/2011/09/15/is-this-horse-dead-yet-ntlm-bottlenecks-and-the-rpc-runtime.aspx)|



