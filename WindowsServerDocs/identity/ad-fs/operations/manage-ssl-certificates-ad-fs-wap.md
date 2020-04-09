---
ms.assetid: a3f50046-5d48-43d3-b0f8-ac2346b15285
title: Gestione di certificati SSL in AD FS e WAP in Windows Server 2016
description: Gestione di certificati SSL in AD FS e WAP in Windows Server 2016
author: jenfieldmsft
ms.author: billmath
manager: samueld
ms.date: 10/02/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: b832756e123bee0223738ee804ac3a4db2371e84
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80855294"
---
# <a name="managing-ssl-certificates-in-ad-fs-and-wap-in-windows-server-2016"></a>Gestione di certificati SSL in AD FS e WAP in Windows Server 2016



Questo articolo descrive come distribuire un nuovo certificato SSL nei server di AD FS e WAP.

>[!NOTE]
>Il metodo consigliato per sostituire il certificato SSL in futuro per una AD FS farm consiste nell'usare Azure AD Connect.  Per ulteriori informazioni, vedere [aggiornare il certificato SSL per una farm di Active Directory Federation Services (ad FS)](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnectfed-ssl-update)

## <a name="obtaining-your-ssl-certificates"></a>Acquisizione dei certificati SSL
Per le farm AD FS di produzione è consigliabile usare un certificato SSL attendibile pubblicamente. Questa operazione viene in genere ottenuta inviando una richiesta di firma del certificato (CSR) a un provider di certificati pubblico di terze parti. Esistono diversi modi per generare la CSR, incluso da un PC Windows 7 o versione successiva. Il fornitore deve avere la documentazione per questa operazione.

- Verificare che il certificato soddisfi i [requisiti del certificato SSL di ad FS e proxy applicazione Web](https://technet.microsoft.com/windows-server-docs/identity/ad-fs/overview/AD-FS-2016-Requirements#BKMK_1)

### <a name="how-many-certificates-are-needed"></a>Quanti certificati sono necessari
Si consiglia di utilizzare un certificato SSL comune in tutti i server di AD FS e proxy applicazione Web. Per i requisiti dettagliati, vedere il documento [ad FS e i requisiti del certificato SSL del proxy dell'applicazione Web](https://technet.microsoft.com/windows-server-docs/identity/ad-fs/overview/AD-FS-2016-Requirements#BKMK_1)

### <a name="ssl-certificate-requirements"></a>Requisiti dei certificati SSL
Per i requisiti, tra cui denominazione, radice del trust ed estensioni, vedere i [requisiti dei certificati SSL del ad FS di documento e del proxy dell'applicazione Web](https://technet.microsoft.com/windows-server-docs/identity/ad-fs/overview/AD-FS-2016-Requirements#BKMK_1)

## <a name="replacing-the-ssl-certificate-for-ad-fs"></a>Sostituzione del certificato SSL per AD FS
> [!NOTE]
> Il certificato SSL di AD FS è diverso dal certificato di comunicazione del servizio AD FS disponibile nello snap-in Gestione AD FS. Per modificare il certificato SSL di AD FS, sarà necessario usare PowerShell.

Determinare prima di tutto la modalità di associazione del certificato eseguita dai server AD FS: associazione di autenticazione del certificato predefinita o modalità di associazione TLS client alternativa.

### <a name="replacing-the-ssl-certificate-for-ad-fs-running-in-default-certificate-authentication-binding-mode"></a>Sostituzione del certificato SSL per AD FS in esecuzione in modalità di associazione autenticazione del certificato predefinita
Per impostazione predefinita, AD FS esegue l'autenticazione del certificato del dispositivo sulla porta 443 e sull'autenticazione del certificato utente sulla porta 49443 (o su una porta configurabile che non è 443).
In questa modalità, usare il cmdlet di PowerShell set-AdfsSslCertificate per gestire il certificato SSL.

Attieniti alla procedura seguente:

1. Per prima cosa, è necessario ottenere il nuovo certificato. Questa operazione viene eseguita in genere inviando una richiesta di firma del certificato (CSR) a un provider di certificati pubblico di terze parti. Esistono diversi modi per generare la CSR, incluso da un PC Windows 7 o versione successiva. Il fornitore deve avere la documentazione per questa operazione.

    * Verificare che il certificato soddisfi i [requisiti del certificato SSL di ad FS e proxy applicazione Web](https://technet.microsoft.com/windows-server-docs/identity/ad-fs/overview/AD-FS-2016-Requirements#BKMK_1)

1. Una volta ottenuta la risposta dal provider di certificati, importarla nell'archivio del computer locale in ogni AD FS e il server proxy applicazione Web.

1. Nel server di AD FS **primario** , usare il cmdlet seguente per installare il nuovo certificato SSL

```powershell
Set-AdfsSslCertificate -Thumbprint '<thumbprint of new cert>'
```

È possibile trovare l'identificazione personale del certificato eseguendo questo comando:

```powershell
dir Cert:\LocalMachine\My\
```

#### <a name="additional-notes"></a>Note aggiuntive

* Il cmdlet Set-AdfsSslCertificate è un cmdlet a più nodi. Ciò significa che deve essere eseguito solo dal database primario e tutti i nodi della farm verranno aggiornati. Questo è nuovo nel server 2016. Nel Server 2012 R2 era necessario eseguire set-AdfsSslCertificate in ogni server.
* Il cmdlet Set-AdfsSslCertificate deve essere eseguito solo sul server primario. Il server primario deve eseguire il server 2016 e il livello di comportamento della farm deve essere elevato a 2016.
* Il cmdlet Set-AdfsSslCertificate utilizzerà la comunicazione remota di PowerShell per configurare gli altri server AD FS, assicurarsi che la porta 5985 (TCP) sia aperta negli altri nodi.
* Il cmdlet Set-AdfsSslCertificate concede all'entità adfssrv le autorizzazioni di lettura per le chiavi private del certificato SSL. Questa entità rappresenta il servizio AD FS. Non è necessario concedere all'account del servizio AD FS l'accesso in lettura alle chiavi private del certificato SSL.

### <a name="replacing-the-ssl-certificate-for-ad-fs-running-in-alternate-tls-binding-mode"></a>Sostituzione del certificato SSL per AD FS in esecuzione in modalità di associazione TLS alternativa
Quando è configurato in modalità di associazione TLS client alternativa, AD FS esegue l'autenticazione del certificato del dispositivo sulla porta 443 e sull'autenticazione del certificato utente sulla porta 443, su un nome host diverso. Il nome host del certificato utente è il nome host AD FS pre-sospeso con "certauth", ad esempio "certauth.fs.contoso.com".
In questa modalità, usare il cmdlet di PowerShell set-AdfsAlternateTlsClientBinding per gestire il certificato SSL. Questo consente di gestire non solo l'associazione TLS del client alternativa, ma anche tutte le altre associazioni in cui AD FS imposta il certificato SSL.

Attieniti alla procedura seguente:

1. Per prima cosa, è necessario ottenere il nuovo certificato. Questa operazione viene eseguita in genere inviando una richiesta di firma del certificato (CSR) a un provider di certificati pubblico di terze parti. Esistono diversi modi per generare la CSR, incluso da un PC Windows 7 o versione successiva. Il fornitore deve avere la documentazione per questa operazione.

    * Verificare che il certificato soddisfi i [requisiti del certificato SSL di ad FS e proxy applicazione Web](https://technet.microsoft.com/windows-server-docs/identity/ad-fs/overview/AD-FS-2016-Requirements#BKMK_1)

1. Una volta ottenuta la risposta dal provider di certificati, importarla nell'archivio del computer locale in ogni AD FS e il server proxy applicazione Web.

1. Nel server di AD FS **primario** , usare il cmdlet seguente per installare il nuovo certificato SSL

```powershell
Set-AdfsAlternateTlsClientBinding -Thumbprint '<thumbprint of new cert>'
```

È possibile trovare l'identificazione personale del certificato eseguendo questo comando:

```powershell
dir Cert:\LocalMachine\My\
```

#### <a name="additional-notes"></a>Note aggiuntive

* Il cmdlet Set-AdfsAlternateTlsClientBinding è un cmdlet a più nodi. Ciò significa che deve essere eseguito solo dal database primario e tutti i nodi della farm verranno aggiornati.
* Il cmdlet Set-AdfsAlternateTlsClientBinding deve essere eseguito solo sul server primario. Il server primario deve eseguire il server 2016 e il livello di comportamento della farm deve essere elevato a 2016.
* Il cmdlet Set-AdfsAlternateTlsClientBinding utilizzerà la comunicazione remota di PowerShell per configurare gli altri server AD FS, assicurarsi che la porta 5985 (TCP) sia aperta negli altri nodi.
* Il cmdlet Set-AdfsAlternateTlsClientBinding concede all'entità adfssrv le autorizzazioni di lettura per le chiavi private del certificato SSL. Questa entità rappresenta il servizio AD FS. Non è necessario concedere all'account del servizio AD FS l'accesso in lettura alle chiavi private del certificato SSL.

## <a name="replacing-the-ssl-certificate-for-the-web-application-proxy"></a>Sostituzione del certificato SSL per il proxy dell'applicazione Web
Per configurare sia l'associazione di autenticazione del certificato predefinita che la modalità di associazione TLS client alternativa nel WAP, è possibile usare il cmdlet Set-WebApplicationProxySslCertificate.
Per sostituire il certificato SSL del proxy dell'applicazione Web, in **ogni** server proxy applicazione Web usare il cmdlet seguente per installare il nuovo certificato SSL:

```powershell
Set-WebApplicationProxySslCertificate -Thumbprint '<thumbprint of new cert>'
```

Se il cmdlet precedente ha esito negativo perché il certificato precedente è già scaduto, riconfigurare il proxy usando i cmdlet seguenti:

```powershell
$cred = Get-Credential
```

Immettere le credenziali di un utente di dominio che è amministratore locale nel server AD FS

```powershell
Install-WebApplicationProxy -FederationServiceTrustCredential $cred -CertificateThumbprint '<thumbprint of new cert>' -FederationServiceName 'fs.contoso.com'
```

## <a name="additional-references"></a>Altri riferimenti  
* [Supporto di AD FS per l'associazione di nomi host alternativi per l'autenticazione del certificato](../operations/AD-FS-support-for-alternate-hostname-binding-for-certificate-authentication.md)
* [Informazioni sulle proprietà delle specifiche della AD FS e del certificato](../technical-reference/AD-FS-and-KeySpec-Property.md)
