---
title: Risoluzione dei problemi relativi ad ADFS
description: In questo documento viene descritto come risolvere i diversi aspetti di AD FS
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 01/12/2018
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 416d9a68326927fcdf5884ef794a74e24a25bb8b
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71366098"
---
# <a name="troubleshooting-ad-fs"></a>Risoluzione dei problemi relativi ad ADFS
AD FS dispone di molti elementi mobili, tocca molti elementi diversi e presenta diverse dipendenze.  Naturalmente, ciò può dare luogo a vari problemi.  Questo documento è stato progettato per iniziare a risolvere questi problemi.  In questo documento vengono presentate le aree tipiche in cui è necessario concentrarsi, come abilitare le funzionalità per ulteriori informazioni e diversi strumenti che possono essere utilizzati per tenere traccia dei problemi.  

>[!NOTE]
>Per ulteriori informazioni, vedere la [Guida di ADFS](http://adfshelp.microsoft.com) che fornisce strumenti efficaci in un'unica posizione che semplifica la risoluzione dei problemi di autenticazione da parte di utenti e amministratori a un ritmo più rapido. 


## <a name="what-to-check-first"></a>Cosa verificare prima
Prima di approfondire la risoluzione dei problemi, è necessario verificare prima di tutto.  Sono:
- **Configurazione DNS** : è possibile risolvere il nome del servizio federativo?  Questa operazione dovrebbe essere risolta nell'indirizzo IP del servizio di bilanciamento del carico o nell'indirizzo IP di uno dei server AD FS della farm.  Per ulteriori informazioni [, vedere ad FS risoluzione dei problemi-DNS](ad-fs-tshoot-dns.md).
- **Ad FS endpoint** : è possibile passare agli endpoint ad FS?  Accedendo a questa pagina è possibile determinare se il server Web AD FS sta rispondendo alle richieste.  Se è possibile ottenere questo file, si è a conoscenza che AD FS sta richiedendo la manutenzione delle richieste rispetto a 443.  Per ulteriori informazioni [, vedere ad FS risoluzione dei problemi-endpoint](ad-fs-tshoot-endpoints.md).
- **Accesso avviato da IDP** : è possibile accedere ed eseguire l'autenticazione tramite la pagina di accesso avviata da IDP?  È necessario assicurarsi che la pagina sia stata abilitata perché è disabilitata per impostazione predefinita.  Utilizzare `Set-AdfsProperties -EnableIdPInitiatedSignOn $true` per abilitare la pagina.  Se è possibile effettuare l'accesso e l'autenticazione, si sa che AD FS funziona in quest'area.  Per ulteriori informazioni [, vedere ad FS risoluzione dei problemi-Sign-on](ad-fs-tshoot-initiatedsignon.md).
  ##  <a name="common-troubleshooting-areas"></a>Aree comuni per la risoluzione dei problemi

|Nome|Descrizione|
|-----|-----|
|[Registrazione e controllo degli eventi](ad-fs-tshoot-logging.md)|Usare i registri eventi di Windows per visualizzare informazioni di alto livello e di basso livello tramite i registri di amministrazione e di traccia.  Può essere usato anche per visualizzare il controllo della sicurezza.|
|[Connettività SQL](ad-fs-tshoot-sql.md)|Informazioni sul test della connettività tra i server AD FS e i database SQL back-end|
|[Rilascio di attestazioni](ad-fs-tshoot-claims-issuance.md)|Informazioni su come determinare se AD FS emette correttamente attestazioni.|
|[Rilevamento cicli](ad-fs-tshoot-loop.md)|Informazioni su come determinare e impedire agli utenti di rimbalzare tra IDP e un RP.|
|[Certificati](ad-fs-tshoot-certs.md)|Problemi relativi ai certificati tipico che possono verificarsi|
|[Fiddler](ad-fs-tshoot-fiddler.md)|Informazioni su come installare e usare Fiddler|
|[WS-Federation con Fiddler](ad-fs-tshoot-fiddler-ws-fed.md)|Traccia di Fiddler dettagliata di un'interazione WS-Federation|
|[Regole attestazione](ad-fs-tshoot-claims-rules.md)|Informazioni sulla risoluzione dei problemi relativi alle regole attestazioni e alla relativa sintassi|
|[Autenticazione integrata di Windows](ad-fs-tshoot-iwa.md)|Informazioni sulla risoluzione dei problemi di autenticazione integrata.|
|[Azure AD](ad-fs-tshoot-azure.md)|Informazioni sulla risoluzione dei problemi AD FS interazione con Azure AD.|
|[Analizzatore diagnostica AD FS](ad-fs-diagnostics-analyzer.md)|AD FS Help Diagnostics Analyzer consente di eseguire controlli di AD FS di base usando il modulo di PowerShell di diagnostica. 