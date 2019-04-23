---
title: Risoluzione dei problemi relativi ad ADFS
description: Questo documento descrive come risolvere i vari aspetti di AD FS.
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 01/12/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 60ecf94b72e58aed4d3718b19f6007cdad1c9578
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59840952"
---
# <a name="troubleshooting-ad-fs"></a>Risoluzione dei problemi relativi ad ADFS
ADFS dispone di molte parti in movimento, interessa molti aspetti diversi e presenta numerose dipendenze diversi.  Naturalmente, ciò può dar luogo a diversi problemi.  Questo documento è progettato per iniziare a usare sulla risoluzione di questi problemi.  Questo documento verrà presentate le aree tipiche che è necessario concentrarsi, come abilitare le funzionalità per altre informazioni e vari strumenti che possono essere utilizzati per tenere traccia dei problemi.  

>[!NOTE]
>Per altre informazioni, vedere [aiutare ad FS](http://adfshelp.microsoft.com) che fornisce strumenti efficaci in uno posizionare che rende più semplice per gli utenti e amministratori a risolvere i problemi di autenticazione a un ritmo più veloce. 


## <a name="what-to-check-first"></a>Gli elementi da un controllo
Prima di approfondire la risoluzione dei problemi approfondita, esistono alcuni aspetti che è consigliabile controllare prima di tutto.  Sono:
- **Configurazione DNS** -è possibile risolvere il nome del servizio federativo?  Questo dovrebbe risolvere indirizzo IP di uno di load balancer o l'indirizzo IP di uno dei server AD FS nella farm.  Per altre informazioni, vedere [AD FS risoluzione dei problemi - DNS](ad-fs-tshoot-dns.md).
- **Gli endpoint di AD FS** -è possibile accedere agli endpoint di AD FS?  Passando a ciò è possibile determinare se il server web AD FS risponda alle richieste.  Se è possibile ottenere questo file, si saprà che AD FS è soddisfare le richieste tramite la porta 443 correttamente.  Per altre informazioni, vedere [AD FS risoluzione dei problemi - endpoint](ad-fs-tshoot-endpoints.md).
- **Sign On avviato dal provider di identità** -possibile accedere e autenticarsi tramite la pagina Idp-Initiated Sign On?  È necessario assicurarsi che questa pagina è stata abilitata perché è disabilitato per impostazione predefinita.  Usare `Set-AdfsProperties -EnableIdPInitiatedSignOn $true` per abilitare la pagina.  Se è possibile accedere e autenticarsi si sa che AD FS funziona in quest'area.  Per altre informazioni, vedere [AD FS risoluzione dei problemi - SignOn](ad-fs-tshoot-initiatedsignon.md).
##  <a name="common-troubleshooting-areas"></a>Aree di risoluzione dei problemi comuni

|Nome|Descrizione|
|-----|-----|
|[La registrazione degli eventi e il controllo](ad-fs-tshoot-logging.md)|Utilizzare i registri eventi di Windows per visualizzare minimo e a livello informazioni di alto livello tramite i registri di traccia e di amministratore.  Può anche essere utilizzato per visualizzare il controllo della sicurezza.|
|[Connettività SQL](ad-fs-tshoot-sql.md)|Informazioni sulla verifica della connettività tra i server AD FS e i database SQL di back-end|
|[Rilascio delle attestazioni](ad-fs-tshoot-claims-issuance.md)|Informazioni su come determinare se ADFS emette attestazioni correttamente.|
|[Individuazione di loop](ad-fs-tshoot-loop.md)|Informazioni sulla determinazione e impedire che gli utenti in corso al mittente avanti e indietro tra il provider di identità e un'applicazione relying Party.|
|[Certificati](ad-fs-tshoot-certs.md)|Problemi di certificato Typcial che possono verificarsi|
|[Fiddler](ad-fs-tshoot-fiddler.md)|Informazioni su come installare e utilizzare Fiddler|
|[WS-Federation con Fiddler](ad-fs-tshoot-fiddler-ws-fed.md)|Traccia di Fiddler dettagliati di un'interazione di WS-Federation|
|[Regole attestazioni](ad-fs-tshoot-claims-rules.md)|Informazioni sulla risoluzione dei problemi di regole attestazione e la relativa sintassi|
|[Autenticazione integrata di Windows](ad-fs-tshoot-iwa.md)|Informazioni sulla risoluzione dei problemi di autenticazione integrata.|
|[Azure AD](ad-fs-tshoot-azure.md)|Informazioni sulla risoluzione dei problemi di interazione di AD FS con Azure AD.|
|[AD FS diagnostica Analyzer](ad-fs-diagnostics-analyzer.md)|AD FS della Guida diagnostica Analyzer aiuta a eseguire controlli di AD FS di base usando il modulo PowerShell di diagnostica. 