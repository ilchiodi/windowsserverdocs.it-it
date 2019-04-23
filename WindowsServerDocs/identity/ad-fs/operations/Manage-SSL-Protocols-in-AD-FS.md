---
title: Gestione dei pacchetti di crittografia e i protocolli SSL/TLS per AD FS
description: Domande frequenti per AD FS 2016
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: e834c50965c3af569dbe3756d677ec4cb2372542
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59883922"
---
# <a name="managing-ssltls-protocols-and-cipher-suites-for-ad-fs"></a>Gestione dei pacchetti di crittografia e i protocolli SSL/TLS per AD FS
La documentazione seguente fornisce informazioni su come disabilitare e abilitare alcuni protocolli TLS/SSL e pacchetti che vengono usati da AD FS di crittografia

## <a name="tlsssl-schannel-and-cipher-suites-in-ad-fs"></a>TLS/SSL, SChannel e pacchetti di crittografia in AD FS

Il Transport Layer Security (TLS) e il livello SSL (Secure Sockets) sono protocolli che forniscono per proteggere le comunicazioni.  Active Directory Federation Services utilizza questi protocolli per le comunicazioni.  Oggi esistono versioni diverse di questi protocolli.

Schannel è un Security Support Provider (SSP) che implementa i protocolli di autenticazione standard Internet SSL, TLS E DTLS. Security Support Provider Interface (SSPI) è un'API utilizzata dai sistemi Windows per eseguire funzioni correlate alla sicurezza, tra cui l'autenticazione. SSPI funziona come un'interfaccia comune per diversi Security Support Provider (SSP), tra cui l'SSP Schannel.

Un pacchetto di crittografia è un set di algoritmi di crittografia. L'implementazione di SSP schannel dei protocolli TLS/SSL Usa algoritmi usati da un pacchetto di crittografia per creare chiavi e crittografare le informazioni. Un pacchetto di crittografia specifica un algoritmo per ognuna delle attività seguenti:

- Scambio di chiave
- Crittografia di massa
- Autenticazione dei messaggi

ADFS utilizza Schannel. dll per eseguire le relative interazioni di proteggere le comunicazioni.  ADFS supporta attualmente tutti i protocolli e pacchetti di crittografia supportati da Schannel. dll.

## <a name="managing-the-tlsssl-protocols-and-cipher-suites"></a>Gestione dei protocolli TLS/SSL e pacchetti di crittografia
> [!IMPORTANT]
> Questa sezione sono disponibili informazioni che spiegano come modificare il Registro di sistema. Tuttavia, se si modifica il Registro di sistema in modo non corretto può causare gravi problemi. Di conseguenza, è importante seguire questa procedura. 
> 
> Tenere presente che la modifica le impostazioni di sicurezza predefinite per SCHANNEL potrebbe interrompere o impedire le comunicazioni tra determinati client e server.  Ciò si verifica se è necessaria la comunicazione protetta e non hanno un protocollo di negoziazione delle comunicazioni con.
> 
> Se si applicano queste modifiche, devono essere applicati a tutti i server AD FS nella farm.  Dopo aver applicato queste modifiche è necessario un riavvio.

Nella giornata odierna ed età, protezione avanzata dei server e rimozione di vecchi o pacchetti di crittografia debole sta diventando delle principali priorità per molte organizzazioni.  Pacchetti software sono disponibili che sarà il server di prova e forniscono informazioni dettagliate su questi protocolli e i gruppi.  Per garantire la conformità o ottenere le classificazioni di sicure, rimozione o la disabilitazione di protocolli più deboli o pacchetti di crittografia è diventato un must.  Il resto di questo documento verrà fornite istruzioni su come abilitare o disabilitare alcuni protocolli e pacchetti di crittografia.

Le chiavi del Registro di sistema riportata di seguito si trovano nello stesso percorso:  HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols.  Utilizzare regedit o PowerShell per abilitare o disabilitare tali protocolli e pacchetti di crittografia.

![Percorso nel Registro di sistema](media/Managing-SSL-Protocols-in-AD-FS/registry.png)

## <a name="enable-and-disable-ssl-20"></a>Abilitare e disabilitare SSL 2.0
Per abilitare e disabilitare SSL 2.0, usare le seguenti chiavi del Registro di sistema e i relativi valori.

### <a name="enable-ssl-20"></a>Abilitazione di SSL 2.0
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\SSL 2.0\Server] "Enabled"=dword:00000001
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\SSL 2.0\Server] "DisabledByDefault"=dword:00000000
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\SSL 2.0\Client] "Enabled"=dword:00000001
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\SSL 2.0\Client] "DisabledByDefault"=dword:00000000 

### <a name="disable-ssl-20"></a>Disabilitare SSL 2.0
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\SL 2.0\Server] "Enabled"=dword:00000000
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\SSL 2.0\Server] "DisabledByDefault"=dword:00000001 
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\SSL 2.0\Client] "Enabled"=dword:00000000
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\SSL 2.0\Client] "DisabledByDefault"=dword:00000001

### <a name="using-powershell-to-disable-ssl-20"></a>Uso di PowerShell per disabilitare SSL 2.0

``` powershell
New-Item 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\SSL 2.0\Server' -Force | Out-Null
    
New-ItemProperty -path 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\SSL 2.0\Server' -name 'Enabled' -value '0' -PropertyType 'DWord' -Force | Out-Null
            
New-ItemProperty -path 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\SSL 2.0\Server' -name 'DisabledByDefault' -value 1 -PropertyType 'DWord' -Force | Out-Null
            
New-Item 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\SSL 2.0\Client' -Force | Out-Null
            
New-ItemProperty -path 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\SSL 2.0\Client' -name 'Enabled' -value '0' -PropertyType 'DWord' -Force | Out-Null
            
New-ItemProperty -path 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\SSL 2.0\Client' -name 'DisabledByDefault' -value 1 -PropertyType 'DWord' -Force | Out-Null
Write-Host 'SSL 2.0 has been disabled.'
```

## <a name="enable-and-disable-ssl-30"></a>Abilitare e disabilitare SSL 3.0
Per abilitare e disabilitare SSL 3.0, usare le seguenti chiavi del Registro di sistema e i relativi valori.

### <a name="enable-ssl-30"></a>Abilitare SSL 3.0
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\SSL 3.0\Server] "Enabled"=dword:00000001
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\SSL 3.0\Server] "DisabledByDefault"=dword:00000000 
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\SSL 3.0\Client] "Enabled"=dword:00000001
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\SSL 3.0\Client] "DisabledByDefault"=dword:00000000 

### <a name="disable-ssl-30"></a>Disabilitare SSL 3.0
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\SSL 3.0\Server] "Enabled"=dword:00000000
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\SSL 3.0\Server] "DisabledByDefault"=dword:00000001
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\SSL 3.0\Client] "Enabled"=dword:00000000
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\SSL 3.0\Client] "DisabledByDefault"=dword:00000001 

### <a name="using-powershell-to-disable-ssl-30"></a>Uso di PowerShell per disabilitare SSL 3.0

```powershell
    New-Item 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\SSL 3.0\Server' -Force | Out-Null
    
    New-ItemProperty -path 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\SSL 3.0\Server' -name 'Enabled' -value '0' -PropertyType 'DWord' -Force | Out-Null
    
    New-ItemProperty -path 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\SSL 3.0\Server' -name 'DisabledByDefault' -value 1 -PropertyType 'DWord' -Force | Out-Null
    
    New-Item 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\SSL 3.0\Client' -Force | Out-Null
    
    New-ItemProperty -path 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\SSL 3.0\Client' -name 'Enabled' -value '0' -PropertyType 'DWord' -Force | Out-Null
    
    New-ItemProperty -path 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\SSL 3.0\Client' -name 'DisabledByDefault' -value 1 -PropertyType 'DWord' -Force | Out-Null
    Write-Host 'SSL 3.0 has been disabled.'
```

## <a name="enable-and-disable-tls-10"></a>Abilitare e disabilitare TLS 1.0
Per abilitare e disabilitare TLS 1.0, utilizzare le seguenti chiavi del Registro di sistema e i relativi valori.

> [!IMPORTANT]
> Disabilitare TLS 1.0 interromperà WAP a trust AD FS.  Se si disabilita TLS 1.0 è consigliabile abilitare l'autenticazione avanzata per le applicazioni.  Vedere [abilitare l'autenticazione avanzata](#enabling-strong-authentication-for-net-applications) 



### <a name="enable-tls-10"></a>Abilitare TLS 1.0
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS securityproviders\schannel\protocols\tls 1.0\Server] "Enabled" runasppl"=DWORD:00000001
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.0\Server] "DisabledByDefault"=dword:00000000 
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.0\Client] "Enabled"=dword:00000001
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.0\Client] "DisabledByDefault"=dword:00000000 

### <a name="disable-tls-10"></a>Disabilitare TLS 1.0
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS securityproviders\schannel\protocols\tls 1.0\Server] "Enabled" = dword:00000000
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.0\Server] "DisabledByDefault"=dword:00000001
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.0\Client] "Enabled"=dword:00000000
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.0\Client] "DisabledByDefault"=dword:00000001 

### <a name="using-powershell-to-disable-tls-10"></a>Uso di PowerShell per disabilitare TLS 1.0

```powershell
    New-Item 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.0\Server' -Force | Out-Null
    
    New-ItemProperty -path 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.0\Server' -name 'Enabled' -value '0' -PropertyType 'DWord' -Force | Out-Null
    
    New-ItemProperty -path 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.0\Server' -name 'DisabledByDefault' -value 1 -PropertyType 'DWord' -Force | Out-Null
    
    New-Item 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.0\Client' -Force | Out-Null
    
    New-ItemProperty -path 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.0\Client' -name 'Enabled' -value '0' -PropertyType 'DWord' -Force | Out-Null
    
    New-ItemProperty -path 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.0\Client' -name 'DisabledByDefault' -value 1 -PropertyType 'DWord' -Force | Out-Null
    Write-Host 'TLS 1.0 has been disabled.'
```


## <a name="enable-and-disable-tls-11"></a>Abilitare e disabilitare TLS 1.1
Usare le seguenti chiavi del Registro di sistema e i relativi valori per abilitare e disabilitare TLS 1.1.

### <a name="enable-tls-11"></a>Abilitare TLS 1.1
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.1\Server] "Enabled"=dword:00000001
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.1\Server] "DisabledByDefault"=dword:00000000
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.1\Client] "Enabled"=dword:00000001
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.1\Client] "DisabledByDefault"=dword:00000000

### <a name="disable-tls-11"></a>Disabilitare TLS 1.1
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.1\Server] "Enabled"=dword:00000000
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.1\Server] "DisabledByDefault"=dword:00000001
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.1\Client] "Enabled"=dword:00000000
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.1\Client] "DisabledByDefault"=dword:00000001 

### <a name="using-powershell-to-disable-tls-11"></a>Uso di PowerShell per disabilitare TLS 1.1

``` powershell
    New-Item 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.1\Server' -Force | Out-Null
    
    New-ItemProperty -path 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.1\Server' -name 'Enabled' -value '0' -PropertyType 'DWord' -Force | Out-Null
    
    New-ItemProperty -path 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.1\Server' -name 'DisabledByDefault' -value 1 -PropertyType 'DWord' -Force | Out-Null
    
    New-Item 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.1\Client' -Force | Out-Null
    
    New-ItemProperty -path 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.1\Client' -name 'Enabled' -value '0' -PropertyType 'DWord' -Force | Out-Null
    
    New-ItemProperty -path 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.1\Client' -name 'DisabledByDefault' -value 1 -PropertyType 'DWord' -Force | Out-Null
    Write-Host 'TLS 1.1 has been disabled.'
```

## <a name="enable-and-disable-tls-12"></a>Abilitare e disabilitare TLS 1.2

Usare le seguenti chiavi del Registro di sistema e i relativi valori per abilitare e disabilitare TLS 1.2.

### <a name="enable-tls-12"></a>Abilitare TLS 1.2
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2\Server] "Enabled"=dword:00000001
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2\Server] "DisabledByDefault"=dword:00000000 
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2\Client] "Enabled"=dword:00000001
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2\Client] "DisabledByDefault"=dword:00000000

### <a name="disable-tls-12"></a>Disabilitare TLS 1.2
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2\Server] "Enabled"=dword:00000000
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2\Server] "DisabledByDefault"=dword:00000001
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2\Client] "Enabled"=dword:00000000
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2\Client] "DisabledByDefault"=dword:00000001

### <a name="using-powershell-to-disable-tls-12"></a>Uso di PowerShell per disabilitare TLS 1.2

```powershell
    New-Item 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2\Server' -Force | Out-Null
    
    New-ItemProperty -path 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2\Server' -name 'Enabled' -value '0' -PropertyType 'DWord' -Force | Out-Null
    
    New-ItemProperty -path 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2\Server' -name 'DisabledByDefault' -value 1 -PropertyType 'DWord' -Force | Out-Null
    
    New-Item 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2\Client' -Force | Out-Null
    
    New-ItemProperty -path 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2\Client' -name 'Enabled' -value '0' -PropertyType 'DWord' -Force | Out-Null
    
    New-ItemProperty -path 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2\Client' -name 'DisabledByDefault' -value 1 -PropertyType 'DWord' -Force | Out-Null
    Write-Host 'TLS 1.2 has been disabled.'
```

## <a name="enable-and-disable-rc4"></a>Abilitazione e disabilitazione di RC4 

Usare le seguenti chiavi del Registro di sistema e i relativi valori per l'abilitazione e disabilitazione di RC4.  Di seguito sono riportate le chiavi del Registro di sistema di questo pacchetto di crittografia:

- HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Ciphers\

![Percorso nel Registro di sistema](media/Managing-SSL-Protocols-in-AD-FS/cipher.png)



### <a name="enable-rc4"></a>Abilitare la versione RC4

- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Ciphers\RC4 128/128] "Enabled"=dword:00000001
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Ciphers\RC4 40/128] "Enabled"=dword:00000001
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Ciphers\RC4 56/128] "Enabled"=dword:00000001 

### <a name="disable-rc4"></a>Disabilitazione di RC4

- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Ciphers\RC4 128/128] "Enabled"=dword:00000000
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Ciphers\RC4 40/128] "Enabled"=dword:00000000
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Ciphers\RC4 56/128] "Enabled"=dword:00000000 

### <a name="using-powershell"></a>Mediante PowerShell

```powershell
    ([Microsoft.Win32.RegistryKey]::OpenRemoteBaseKey([Microsoft.Win32.RegistryHive]::LocalMachine,$env:COMPUTERNAME)).CreateSubKey('SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Ciphers\RC4 128/128') 
    New-ItemProperty -path 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Ciphers\RC4 128/128' -name 'Enabled' -value '0' -PropertyType 'DWord' -Force | Out-Null
    
    ([Microsoft.Win32.RegistryKey]::OpenRemoteBaseKey([Microsoft.Win32.RegistryHive]::LocalMachine,$env:COMPUTERNAME)).CreateSubKey('SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Ciphers\RC4 40/128') 
    New-ItemProperty -path 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Ciphers\RC4 40/128' -name 'Enabled' -value '0' -PropertyType 'DWord' -Force | Out-Null
    
    ([Microsoft.Win32.RegistryKey]::OpenRemoteBaseKey([Microsoft.Win32.RegistryHive]::LocalMachine,$env:COMPUTERNAME)).CreateSubKey('SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Ciphers\RC4 56/128') 
    New-ItemProperty -path 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Ciphers\RC4 56/128' -name 'Enabled' -value '0' -PropertyType 'DWord' -Force | Out-Null
```

## <a name="enabling-or-disabling-additional-cipher-suites"></a>Abilitazione o disabilitazione di pacchetti di crittografia aggiuntivi

È possibile disabilitare determinate specifiche crittografie rimuovendole da HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Cryptography\Configuration\Local\SSL\00010002 

![Percorso nel Registro di sistema](media/Managing-SSL-Protocols-in-AD-FS/suites.png)

Per abilitare un pacchetto di crittografia, aggiungere il valore di stringa della chiave di valore multistringa funzioni.  Ad esempio, se si desidera abilitare TLS_ECDHE_RSA_WITH_AES_256_CBC_SHA384_P521 quindi si sarebbe aggiungerlo alla stringa.

Per un elenco completo dei pacchetti di crittografia supportati vedere gruppi [pacchetti di crittografia in TLS/SSL (SSP Schannel)](https://msdn.microsoft.com/en-us/library/windows/desktop/aa374757.aspx).  Questo documento fornisce una tabella di gruppi che sono abilitati per impostazione predefinita e quelli che sono supportate, ma non abilitato per impostazione predefinita.  Per stabilire le priorità, vedere i pacchetti di crittografia [assegnare la priorità ai pacchetti di crittografia Schannel](https://msdn.microsoft.com/en-us/library/windows/desktop/bb870930.aspx).

## <a name="enabling-strong-authentication-for-net-applications"></a>Abilitazione dell'autenticazione avanzata per le applicazioni .NET
Le applicazioni 3.5/4.0/4.5.x di .NET Framework è possono passare il protocollo predefinito a TLS 1.2, consentendo la chiave del Registro di sistema SchUseStrongCrypto.  Questa chiave del Registro di sistema viene forzato delle applicazioni .NET per usare TLS 1.2.

> [!IMPORTANT]
> Per AD FS in Windows Server 2016 e Windows Server 2012 R2 è necessario usare la chiave di .NET Framework 4.0/4.5.x:  HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\\.NETFramework\v4.0.30319


Utilizzare la seguente chiave del Registro di sistema per .NET Framework 3.5:

[HKEY_LOCAL_MACHINE\SOFTWARE\Wow6432Node\Microsoft\\.NETFramework\v2.0.50727] "SchUseStrongCrypto"=dword:00000001

Per .NET Framework 4.0/4.5.x usare la chiave del Registro di sistema seguente: HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\\.NETFramework\v4.0.30319 "SchUseStrongCrypto"=dword:00000001

![Autenticazione avanzata](media/Managing-SSL-Protocols-in-AD-FS/strongauth.png)

```powershell
    
    New-ItemProperty -path 'HKLM:\SOFTWARE\Microsoft\.NetFramework\v4.0.30319' -name 'SchUseStrongCrypto' -value '1' -PropertyType 'DWord' -Force | Out-Null
```

## <a name="additional-information"></a>Informazioni aggiuntive

- [Cipher Suites in TLS/SSL (Schannel SSP)](https://msdn.microsoft.com/en-us/library/windows/desktop/aa374757.aspx)
- [Pacchetti di crittografia TLS in Windows 8.1](https://msdn.microsoft.com/en-us/library/windows/desktop/mt767781.aspx)
- [Definire la priorità dei pacchetti di crittografia Schannel](https://msdn.microsoft.com/en-us/library/windows/desktop/bb870930.aspx)
- [A proposito di crittografie e altri professionisti cuore](https://blogs.technet.microsoft.com/askds/2015/12/08/speaking-in-ciphers-and-other-enigmatic-tonguesupdate/)
