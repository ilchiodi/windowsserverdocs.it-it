---
ms.assetid: a3f50046-5d48-43d3-b0f8-ac2346b15285
title: Gestione di certificati SSL in ADFS e WAP in Windows Server 2016
description: Gestione di certificati SSL in ADFS e WAP in Windows Server 2016
author: jenfieldmsft
ms.author: billmath
manager: samueld
ms.date: 10/02/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 5156b3ad357ab8edb5e08a89a459beaf9b8c9b1a
ms.sourcegitcommit: ca7dc3d56a33924ae5fe0e9ffb1274da6dc4e54d
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 10/03/2017
---
# <a name="managing-ssl-certificates-in-ad-fs-and-wap-in-windows-server-2016"></a>Gestione di certificati SSL in ADFS e WAP in Windows Server 2016

>Si applica a: Windows Server 2016

Questo articolo descrive come distribuire un nuovo certificato SSL per i server ADFS e WAP.

>[!NOTE]
>Il metodo consigliato per sostituire il certificato SSL per il futuro per una farm ADFS consiste nell'usare Azure AD Connect.  Per ulteriori informazioni vedere [aggiornare il certificato SSL per una farm di Active Directory Federation Services (ADFS)](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnectfed-ssl-update)

## <a name="obtaining-your-ssl-certificates"></a>Ottenere i certificati SSL
Per le farm di produzione ADFS, è consigliabile un certificato SSL attendibile pubblicamente. In genere, questo viene ottenuto tramite l'invio di una firma certificato (CSR) a una terza parte, l'autorità di certificazione pubblica. Esistono diversi modi per generare il rappresentante del servizio, tra cui da un Windows 7 o versione successiva PC. Il fornitore deve disporre di questa documentazione.

- Assicurarsi che il certificato soddisfa i [requisiti dei certificati ADFS e SSL di Proxy applicazione Web](https://technet.microsoft.com/en-us/windows-server-docs/identity/ad-fs/overview/AD-FS-2016-Requirements#BKMK_1)

### <a name="how-many-certificates-are-needed"></a>Sono necessari certificati quanti
È consigliabile utilizzare un certificato SSL comune tra server tutti ADFS e Proxy applicazione Web. Per informazioni dettagliate sui requisiti, vedere il documento [requisiti dei certificati ADFS e SSL di Proxy applicazione Web](https://technet.microsoft.com/en-us/windows-server-docs/identity/ad-fs/overview/AD-FS-2016-Requirements#BKMK_1)

### <a name="ssl-certificate-requirements"></a>Requisiti dei certificati SSL
Per requisiti inclusi nomi, radice di attendibilità ed estensioni vedere il documento [requisiti dei certificati ADFS e SSL di Proxy applicazione Web](https://technet.microsoft.com/en-us/windows-server-docs/identity/ad-fs/overview/AD-FS-2016-Requirements#BKMK_1)

## <a name="replacing-the-ssl-certificate-for-ad-fs"></a>Sostituire il certificato SSL per ADFS
> [!NOTE]
> Il certificato SSL ADFS non è lo stesso certificato per le comunicazioni servizio ADFS disponibile nello snap-in Gestione ADFS. Per modificare il certificato SSL ADFS, è necessario utilizzare PowerShell.

Innanzitutto, determinare quale certificato modalità di binding i server ADFS sono in esecuzione: binding autenticazione certificato predefinito o modalità di associazione alternativo client TLS.

### <a name="replacing-the-ssl-certificate-for-ad-fs-running-in-default-certificate-authentication-binding-mode"></a>Sostituire il certificato SSL per ADFS in esecuzione in modalità di associazione di autenticazione certificato
AD FS per impostazione predefinita esegue l'autenticazione del certificato dispositivo sulla porta 443 e l'autenticazione del certificato utente sulla porta 49443 (o una porta configurabile che non è 443).
In questa modalità, utilizzare il cmdlet di powershell Set-AdfsSslCertificate per gestire il certificato SSL.

Attenersi alla procedura seguente:

1. Prima di tutto, è necessario ottenere il nuovo certificato. Questa operazione viene eseguita in genere inviando una richiesta firma certificato (CSR) a una terza parte, l'autorità di certificazione pubblica. Esistono diversi modi per generare il rappresentante del servizio, tra cui da un Windows 7 o versione successiva PC. Il fornitore deve disporre di questa documentazione.

    * Assicurarsi che il certificato soddisfa i [requisiti dei certificati ADFS e SSL di Proxy applicazione Web](https://technet.microsoft.com/en-us/windows-server-docs/identity/ad-fs/overview/AD-FS-2016-Requirements#BKMK_1)

1. Quando si arriva la risposta dal provider di certificati, importarlo nell'archivio computer locale in ogni server ADFS e Proxy applicazione Web.

1. Nel **primario** server ADFS, utilizzare il cmdlet seguente per installare il nuovo certificato SSL

```powershell
Set-AdfsSslCertificate -Thumbprint '<thumbprint of new cert>'
```

Identificazione personale del certificato sono reperibili eseguendo questo comando:

```powershell
dir Cert:\LocalMachine\My\
```

#### <a name="additional-notes"></a>Note aggiuntive

* Il cmdlet Set-AdfsSslCertificate è un cmdlet a più nodi; Ciò significa deve eseguire dal database primario e tutti i nodi della farm verranno aggiornati. Questa è una novità in Server 2016. In Server 2012 R2 è necessario eseguire Set-AdfsSslCertificate in ogni server.
* Il cmdlet Set-AdfsSslCertificate deve essere eseguito solo sul server primario. Il server primario deve essere in esecuzione Server 2016 e deve essere generato il livello di comportamento Farm 2016.
* Il cmdlet Set-AdfsSslCertificate userà comunicazione remota di PowerShell per configurare gli altri server AD FS, assicurarsi che la porta 5985 (TCP) sia aperta su altri nodi.
* Il cmdlet Set-AdfsSslCertificate concederà le adfssrv entità autorizzazioni di lettura per le chiavi private del certificato SSL. Questa entità di sicurezza rappresenta il servizio ADFS. Non è necessario concedere l'accesso in lettura AD FS service account alle chiavi private del certificato SSL.

### <a name="replacing-the-ssl-certificate-for-ad-fs-running-in-alternate-tls-binding-mode"></a>Sostituire il certificato SSL per ADFS in esecuzione in modalità di associazione TLS alternativa
Quando è configurato nel client alternativo modalità di associazione TLS, AD FS esegue l'autenticazione del certificato dispositivo sulla porta 443 e l'autenticazione del certificato utente sulla porta 443, in un nome host diverso. Il nome host certificato utente è di AD FS hostname preceduto da "certauth", ad esempio "certauth.fs.contoso.com".
In questa modalità, utilizzare il cmdlet di powershell Set-AdfsAlternateTlsClientBinding per gestire il certificato SSL. Gestire non solo l'associazione di TLS client alternativi, ma tutte le altre associazioni in cui ADFS imposta anche il certificato SSL.

Attenersi alla procedura seguente:

1. Prima di tutto, è necessario ottenere il nuovo certificato. Questa operazione viene eseguita in genere inviando una richiesta firma certificato (CSR) a una terza parte, l'autorità di certificazione pubblica. Esistono diversi modi per generare il rappresentante del servizio, tra cui da un Windows 7 o versione successiva PC. Il fornitore deve disporre di questa documentazione.

    * Assicurarsi che il certificato soddisfa i [requisiti dei certificati ADFS e SSL di Proxy applicazione Web](https://technet.microsoft.com/en-us/windows-server-docs/identity/ad-fs/overview/AD-FS-2016-Requirements#BKMK_1)

1. Quando si arriva la risposta dal provider di certificati, importarlo nell'archivio computer locale in ogni server ADFS e Proxy applicazione Web.

1. Nel **primario** server ADFS, utilizzare il cmdlet seguente per installare il nuovo certificato SSL

```powershell
Set-AdfsAlternateTlsClientBinding -Thumbprint '<thumbprint of new cert>'
```

Identificazione personale del certificato sono reperibili eseguendo questo comando:

```powershell
dir Cert:\LocalMachine\My\
```

#### <a name="additional-notes"></a>Note aggiuntive

* Il cmdlet Set-AdfsAlternateTlsClientBinding è un cmdlet a più nodi; Ciò significa deve eseguire dal database primario e tutti i nodi della farm verranno aggiornati.
* Il cmdlet Set-AdfsAlternateTlsClientBinding deve essere eseguito solo sul server primario. Il server primario deve essere in esecuzione Server 2016 e deve essere generato il livello di comportamento Farm 2016.
* Il cmdlet Set-AdfsAlternateTlsClientBinding userà comunicazione remota di PowerShell per configurare gli altri server AD FS, assicurarsi che la porta 5985 (TCP) sia aperta su altri nodi.
* Il cmdlet Set-AdfsAlternateTlsClientBinding concederà le adfssrv entità autorizzazioni di lettura per le chiavi private del certificato SSL. Questa entità di sicurezza rappresenta il servizio ADFS. Non è necessario concedere l'accesso in lettura AD FS service account alle chiavi private del certificato SSL.

## <a name="replacing-the-ssl-certificate-for-the-web-application-proxy"></a>Sostituire il certificato SSL per il Proxy dell'applicazione Web
Per la configurazione di entrambi i binding di autenticazione certificato predefinito o la modalità di associazione alternativo client TLS nel WAP possiamo usare il cmdlet Set-WebApplicationProxySslCertificate.
Sostituire il certificato SSL di Proxy applicazione Web, in **ogni** server Proxy applicazione Web Usa il cmdlet seguente per installare il nuovo certificato SSL:

```powershell
Set-WebApplicationProxySslCertificate '<thumbprint of new cert>'
```

Se il cmdlet precedente ha esito negativo perché il certificato precedente è già scaduto, riconfigurare il proxy usando i cmdlet seguenti:

```powershell
$cred = Get-Credential
```

Immettere le credenziali dell'utente di dominio amministratore locale sul server ADFS

```powershell
Install-WebApplicationProxy -FederationServiceTrustCredential $cred -CertificateThumbprint '<thumbprint of new cert>' -FederationServiceName 'fs.contoso.com'
```

## <a name="additional-references"></a>Riferimenti aggiuntivi  
* [Supporto di ADFS per l'associazione di nome host alternativo per l'autenticazione del certificato](../operations/AD-FS-support-for-alternate-hostname-binding-for-certificate-authentication.md)
* [AD FS e certificato KeySpec proprietà informazioni](../technical-reference/AD-FS-and-KeySpec-Property.md)
