---
title: Panoramica di NTLM
description: Sicurezza di Windows Server
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: b8dec2877646fd2bfe00da9d5c9047e8edfd6f1d
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71386262"
---
# <a name="ntlm-overview"></a>Panoramica di NTLM

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016

Questo argomento per i professionisti IT descrive NTLM, qualsiasi modifica alla funzionalità e fornisce collegamenti a risorse tecniche per l'autenticazione di Windows e NTLM per Windows Server 2012 e versioni precedenti.

## <a name="BKMK_OVER"></a>Descrizione della funzionalità
L'autenticazione NTLM è una famiglia di protocolli di autenticazione inclusi in Windows Msv1\_0. dll. I protocolli di autenticazione NTLM comprendono LAN Manager versione 1 e 2 ed NTLM versione 1 e 2. I protocolli di autenticazione NTLM autenticano gli utenti e i computer in base a un meccanismo di risposta\/Challenge che dimostra a un server o a un controller di dominio che un utente è a conoscenza della password associata a un account. Quando viene utilizzato il protocollo NTLM, un server delle risorse deve eseguire una delle azioni seguenti per verificare l'identità di un computer o utente ogni volta che è necessario un nuovo token di accesso:

-   Contattare un servizio di autenticazione del dominio nel controller di dominio per il dominio dell'account del computer o dell'utente, se l'account è un account di dominio.

-   Cercare l'account del computer o dell'utente nel database di account locale, se l'account è un account locale.

## <a name="BKMK_APP"></a>Applicazioni correnti
L'autenticazione NTLM è ancora supportata e deve essere usata per l'autenticazione Windows con i sistemi configurati come membri di un gruppo di lavoro. L'autenticazione NTLM viene usata anche per l'autenticazione di accesso locale nei controller di dominio non\-. L'autenticazione Kerberos versione 5 è il metodo di autenticazione preferito per gli ambienti Active Directory, ma un'applicazione non\-Microsoft o Microsoft potrebbe comunque usare NTLM.

Per ridurre l'utilizzo del protocollo NTLM in un ambiente IT è necessario conoscere i requisiti per le applicazioni distribuite in NTLM e le strategie e le procedure richieste per configurare l'uso di altri protocolli negli ambienti informatici. Sono stati aggiunti nuovi strumenti e impostazioni per comprendere come usare NTLM per ridurre selettivamente il traffico NTLM. Per informazioni su come analizzare e ridurre l'utilizzo di NTLM negli ambienti, vedere [Introduzione alla restrizione dell'autenticazione NTLM](https://technet.microsoft.com/library/dd560653(v=ws.10).aspx) per accedere alla Guida alla limitazione e al controllo dell'utilizzo di NTLM.

## <a name="BKMK_NEW"></a>Funzionalità nuove e modificate
Non sono state apportate modifiche alla funzionalità per NTLM per Windows Server 2012.

## <a name="BKMK_DEP"></a>Funzionalità rimosse o deprecate
Non sono disponibili funzionalità rimosse o deprecate per NTLM per Windows Server 2012.

## <a name="BKMK_INSTALL"></a>Informazioni Server Manager
Non è possibile configurare NTLM da Server Manager. Per la gestione dell'utilizzo dell'autenticazione NTLM tra computer è possibile utilizzare le impostazioni relative ai criteri di sicurezza o Criteri di gruppo. In un dominio Kerberos è il protocollo di autenticazione predefinito.

## <a name="BKMK_LINKS"></a>Vedere anche
Nella tabella seguente è riportato l'elenco delle risorse pertinenti per NTLM e altre tecnologie di autenticazione di Windows.

|Tipo di contenuto|Riferimenti|
|--------|-------|
|**Valutazione del prodotto**|[Introduzione alla restrizione dell'autenticazione NTLM](https://technet.microsoft.com/library/dd560653.aspx)<br /><br />[Modifiche all'autenticazione NTLM](https://technet.microsoft.com/library/dd566199.aspx)|
|**Pianificazione**|[Guida alla modellazione delle minacce per l'infrastruttura IT](https://technet.microsoft.com/library/dd941826.aspx)<br /><br />[Minacce e contromisure: impostazioni di sicurezza in Windows Server 2003 e Windows XP](https://technet.microsoft.com/library/dd162275.aspx)<br /><br />[Guida alle minacce e alle contromisure: impostazioni di sicurezza in Windows Server 2008 e Windows Vista](https://technet.microsoft.com/library/dd349791.aspx)<br /><br />[Guida alle minacce e alle contromisure: impostazioni di sicurezza in Windows Server 2008 R2 e Windows 7](https://technet.microsoft.com/library/hh125921.aspx)|
|**Distribuzione**|[protezione estesa per l'autenticazione](https://support.microsoft.com/kb/968389)<br /><br />[Guida alla limitazione e al controllo dell'utilizzo di NTLM](https://technet.microsoft.com/library/jj865674(v=ws.10).aspx)<br /><br />[Domande al team di servizi directory: blocco NTLM e: metodologia di analisi delle applicazioni e controllo in Windows 7](https://blogs.technet.com/askds/archive/2009/10/08/ntlm-blocking-and-you-application-analysis-and-auditing-methodologies-in-windows-7.aspx)<br /><br />[Blog sull'autenticazione di Windows](https://blogs.technet.com/authentication/)<br /><br />[Configurazione di MaxConcurrentAPI per il passaggio NTLM\-tramite autenticazione](https://social.technet.microsoft.com/wiki/contents/articles/9759.configuring-maxconcurrentapi-for-ntlm-pass-through-authentication.aspx)|
|**Sviluppo**|[Microsoft NTLM \(Windows\)](https://msdn.microsoft.com/library/aa378749(VS.85).aspx)<br /><br />[\[MS\-NLMP\]: NT LAN Manager \(la specifica del protocollo di autenticazione NTLM\)](https://msdn.microsoft.com/library/cc236621(PROT.10).aspx)<br /><br />[\[MS\-NNTP\]: NT LAN Manager \(autenticazione NTLM\): Network News Transfer Protocol \(NNTP\) Extension](https://msdn.microsoft.com/library/cc236774(PROT.10).aspx)<br /><br />[\[MS\-NTHT\]: specifica del protocollo NTLM su HTTP](https://msdn.microsoft.com/library/cc237488(PROT.10).aspx)|
|**Risoluzione dei problemi**|Non ancora disponibile|
|**Risorse della community**|[Questo cavallo è ancora morto: colli di bottiglia NTLM e runtime RPC](http://blogs.technet.com/b/askds/archive/2011/09/15/is-this-horse-dead-yet-ntlm-bottlenecks-and-the-rpc-runtime.aspx)|



