---
ms.assetid: a3f50046-5d48-43d3-b0f8-ac2346b15285
title: Gestione di certificati SSL in AD FS e WAP in Windows Server 2016
description: Gestione di certificati SSL in AD FS e WAP in Windows Server 2016
author: jenfieldmsft
ms.author: billmath
manager: samueld
ms.date: 10/02/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 9bae831da9d247c423c2874a5928b7f811ef65dc
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/24/2019
ms.locfileid: "66188713"
---
# <a name="managing-ssl-certificates-in-ad-fs-and-wap-in-windows-server-2016"></a>Gestione di certificati SSL in AD FS e WAP in Windows Server 2016



Questo articolo descrive come distribuire un nuovo certificato SSL per i server AD FS e WAP.

>[!NOTE]
>Il metodo consigliato per sostituire il certificato SSL in futuro per una farm AD FS è usare Azure AD Connect.  Per altre informazioni vedere [aggiornare il certificato SSL per una farm Active Directory Federation Services (ADFS)](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnectfed-ssl-update)

## <a name="obtaining-your-ssl-certificates"></a>Come ottenere i certificati SSL
Per le farm di produzione AD FS è consigliabile un certificato SSL pubblicamente attendibile. Questo viene in genere ottenuto tramite l'invio di una firma richiesta del certificato (CSR) a terze parti, l'autorità di certificazione pubblica. Esistono diversi modi per generare il file CSR, ad esempio da un PC versioni successive o Windows 7. Il fornitore deve avere la documentazione per l'oggetto.

- Assicurarsi che il certificato soddisfa i [requisiti dei certificati di AD FS e Proxy di applicazione Web SSL](https://technet.microsoft.com/windows-server-docs/identity/ad-fs/overview/AD-FS-2016-Requirements#BKMK_1)

### <a name="how-many-certificates-are-needed"></a>Quanti certificati sono necessari
È consigliabile utilizzare un certificato SSL comune tra i server di tutto AD FS e Proxy applicazione Web. Per informazioni dettagliate sui requisiti, vedere il documento [requisiti dei certificati di AD FS e Proxy di applicazione Web SSL](https://technet.microsoft.com/windows-server-docs/identity/ad-fs/overview/AD-FS-2016-Requirements#BKMK_1)

### <a name="ssl-certificate-requirements"></a>Requisiti dei certificati SSL
Per i requisiti compresi di denominazione, radice di attendibilità ed estensioni vedere il documento [requisiti dei certificati di AD FS e Proxy di applicazione Web SSL](https://technet.microsoft.com/windows-server-docs/identity/ad-fs/overview/AD-FS-2016-Requirements#BKMK_1)

## <a name="replacing-the-ssl-certificate-for-ad-fs"></a>Sostituire il certificato SSL per ADFS
> [!NOTE]
> Il certificato SSL di AD FS non è quello utilizzato per il certificato di comunicazioni di servizio AD FS trovato nello snap-in Gestione ADFS. Per modificare il certificato SSL di AD FS, è necessario usare PowerShell.

In primo luogo, determinare quale certificato modalità di associazione sono in esecuzione il server AD FS: binding di autenticazione del certificato predefinito, o in modalità di associazione TLS client alternativo.

### <a name="replacing-the-ssl-certificate-for-ad-fs-running-in-default-certificate-authentication-binding-mode"></a>Sostituire il certificato SSL per AD FS in esecuzione in modalità di binding autenticazione certificato
AD FS per impostazione predefinita esegue l'autenticazione del certificato dispositivo sulla porta 443 e l'autenticazione del certificato utente sulla porta 49443 (o una porta configurabile che non è la porta 443).
In questa modalità, usare il cmdlet di powershell Set-AdfsSslCertificate per gestire il certificato SSL.

Attieniti alla procedura seguente:

1. In primo luogo, è necessario ottenere il nuovo certificato. Questa operazione viene in genere eseguita inviando una richiesta firma di certificato (CSR) a terze parti, autorità di certificazione pubblica. Esistono diversi modi per generare il file CSR, ad esempio da un PC versioni successive o Windows 7. Il fornitore deve avere la documentazione per l'oggetto.

    * Assicurarsi che il certificato soddisfa i [requisiti dei certificati di AD FS e Proxy di applicazione Web SSL](https://technet.microsoft.com/windows-server-docs/identity/ad-fs/overview/AD-FS-2016-Requirements#BKMK_1)

1. Dopo aver ottenuto la risposta dal provider di certificati, importarlo nell'archivio computer locale in ogni server AD FS e Proxy applicazione Web.

1. Nel **primaria** server AD FS, usare il cmdlet seguente per installare il nuovo certificato SSL

```powershell
Set-AdfsSslCertificate -Thumbprint '<thumbprint of new cert>'
```

L'identificazione personale del certificato sono reperibili eseguendo questo comando:

```powershell
dir Cert:\LocalMachine\My\
```

#### <a name="additional-notes"></a>Note aggiuntive

* Il cmdlet Set-AdfsSslCertificate è un cmdlet a più nodi; Ciò significa che è sufficiente per l'esecuzione dal server primario e verranno aggiornati tutti i nodi nella farm. È una novità in Server 2016. In Server 2012 R2 era necessario eseguire Set-AdfsSslCertificate in ogni server.
* Il cmdlet Set-AdfsSslCertificate deve essere eseguito solo sul server primario. Il server primario deve essere in esecuzione Server 2016 e deve essere generato il livello di comportamento Farm 2016.
* Il cmdlet Set-AdfsSslCertificate userà comunicazione remota di PowerShell per configurare gli altri server AD FS, assicurarsi che la porta 5985 (TCP) sia aperta in altri nodi.
* Il cmdlet Set-AdfsSslCertificate comporterà la concessione di adfssrv principale autorizzazioni di lettura per le chiavi private del certificato SSL. Questa entità rappresenta il servizio AD FS. Non è necessario concedere l'accesso in lettura account del servizio AD FS per le chiavi private del certificato SSL.

### <a name="replacing-the-ssl-certificate-for-ad-fs-running-in-alternate-tls-binding-mode"></a>Sostituire il certificato SSL per ADFS in esecuzione in modalità di associazione TLS alternativa
Quando è configurato nel client alternativo di modalità di associazione TLS, ADFS consente di eseguire l'autenticazione del certificato dispositivo sulla porta 443 e l'autenticazione del certificato utente sulla porta 443, anche su un nome host diverso. Il nome host del certificato utente è di AD FS hostname preceduto da "certauth", ad esempio "com".
In questa modalità, usare il cmdlet di powershell Set-AdfsAlternateTlsClientBinding per gestire il certificato SSL. Questa verrà gestita non solo l'associazione TLS client alternativi, ma tutti gli altri binding in cui ADFS viene impostato anche il certificato SSL.

Attieniti alla procedura seguente:

1. In primo luogo, è necessario ottenere il nuovo certificato. Questa operazione viene in genere eseguita inviando una richiesta firma di certificato (CSR) a terze parti, autorità di certificazione pubblica. Esistono diversi modi per generare il file CSR, ad esempio da un PC versioni successive o Windows 7. Il fornitore deve avere la documentazione per l'oggetto.

    * Assicurarsi che il certificato soddisfa i [requisiti dei certificati di AD FS e Proxy di applicazione Web SSL](https://technet.microsoft.com/windows-server-docs/identity/ad-fs/overview/AD-FS-2016-Requirements#BKMK_1)

1. Dopo aver ottenuto la risposta dal provider di certificati, importarlo nell'archivio computer locale in ogni server AD FS e Proxy applicazione Web.

1. Nel **primaria** server AD FS, usare il cmdlet seguente per installare il nuovo certificato SSL

```powershell
Set-AdfsAlternateTlsClientBinding -Thumbprint '<thumbprint of new cert>'
```

L'identificazione personale del certificato sono reperibili eseguendo questo comando:

```powershell
dir Cert:\LocalMachine\My\
```

#### <a name="additional-notes"></a>Note aggiuntive

* Il cmdlet Set-AdfsAlternateTlsClientBinding è un cmdlet a più nodi; Ciò significa che è sufficiente per l'esecuzione dal server primario e verranno aggiornati tutti i nodi nella farm.
* Il cmdlet Set-AdfsAlternateTlsClientBinding deve essere eseguito solo sul server primario. Il server primario deve essere in esecuzione Server 2016 e deve essere generato il livello di comportamento Farm 2016.
* Il cmdlet Set-AdfsAlternateTlsClientBinding userà comunicazione remota di PowerShell per configurare gli altri server AD FS, assicurarsi che la porta 5985 (TCP) sia aperta in altri nodi.
* Il cmdlet Set-AdfsAlternateTlsClientBinding comporterà la concessione di adfssrv principale autorizzazioni di lettura per le chiavi private del certificato SSL. Questa entità rappresenta il servizio AD FS. Non è necessario concedere l'accesso in lettura account del servizio AD FS per le chiavi private del certificato SSL.

## <a name="replacing-the-ssl-certificate-for-the-web-application-proxy"></a>Sostituire il certificato SSL per il Proxy applicazione Web
Per la configurazione sia la modalità di associazione TLS client alternativo o binding autenticazione del certificato predefinito in WAP è possibile usare il cmdlet Set-WebApplicationProxySslCertificate.
Sostituire il certificato SSL di Proxy applicazione Web, in **ogni** server Proxy applicazione Web usare il cmdlet seguente per installare il nuovo certificato SSL:

```powershell
Set-WebApplicationProxySslCertificate '<thumbprint of new cert>'
```

Se il cmdlet precedente non riesce perché il certificato precedente è già scaduto, riconfigurare il proxy usando i cmdlet seguenti:

```powershell
$cred = Get-Credential
```

Immettere le credenziali dell'utente di dominio che sia amministratore locale nel server AD FS

```powershell
Install-WebApplicationProxy -FederationServiceTrustCredential $cred -CertificateThumbprint '<thumbprint of new cert>' -FederationServiceName 'fs.contoso.com'
```

## <a name="additional-references"></a>Altri riferimenti  
* [Supporto di AD FS per l'associazione di nomi host alternativi per l'autenticazione del certificato](../operations/AD-FS-support-for-alternate-hostname-binding-for-certificate-authentication.md)
* [AD FS e proprietà KeySpec informazioni certificato](../technical-reference/AD-FS-and-KeySpec-Property.md)
