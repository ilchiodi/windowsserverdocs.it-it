---
title: Gestione dei pacchetti di crittografia e protocolli SSL/TLS per AD FS
description: Domande frequenti per AD FS 2016
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 23055a1b727e4f1a6b1ccafdea9a410c6021c432
ms.sourcegitcommit: 2782a80a916f8432c030af76e860a72f08481899
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/13/2018
---
# <a name="managing-ssltls-protocols-and-cipher-suites-for-ad-fs"></a>Gestione dei pacchetti di crittografia e protocolli SSL/TLS per AD FS
La documentazione seguente vengono fornite informazioni su come disabilitare e abilitare alcuni protocolli TLS/SSL e pacchetti utilizzati da AD FS di crittografia

## <a name="tlsssl-schannel-and-cipher-suites-in-ad-fs"></a>TLS/SSL, SChannel e pacchetti di crittografia in ADFS

Il Transport Layer Security (TLS) e sicuro Sockets Layer (SSL) sono protocolli che offrono per proteggere le comunicazioni.  Active Directory Federation Services utilizza questi protocolli per le comunicazioni.  Attualmente esistono diverse versioni di questi protocolli.

Schannel è un Security Support Provider (SSP) che implementa i protocolli di autenticazione standard SSL, TLS e DTLS Internet. Security Support Provider Interface (SSPI) è un'API utilizzata dai sistemi Windows per eseguire funzioni correlate alla sicurezza tra cui l'autenticazione. SSPI funziona come un'interfaccia comune per diversi Security Support Provider (SSP), tra cui SSP. Schannel.

Un pacchetto di crittografia è un set di algoritmi di crittografia. L'implementazione di SSP schannel dei protocolli TLS/SSL usano gli algoritmi di un pacchetto di crittografia per creare chiavi e crittografare informazioni. Un pacchetto di crittografia specifica un algoritmo per ognuna delle attività seguenti:

- Scambio di chiave
- Crittografia di massa
- Autenticazione dei messaggi

Per eseguire le interazioni di proteggere le comunicazioni, ADFS utilizza Schannel. dll.  ADFS supporta attualmente tutti i protocolli e pacchetti di crittografia supportati da Schannel. dll.

## <a name="managing-the-tlsssl-protocols-and-cipher-suites"></a>Gestione dei pacchetti di crittografia e protocolli TLS/SSL
> [!IMPORTANT]
> Questa sezione contiene i passaggi che spiegano come modificare il Registro di sistema. Tuttavia, potrebbero verificarsi problemi gravi se si modifica il Registro di sistema in modo errato. Pertanto, assicurarsi che seguire questa procedura. 
> 
> Tenere presente che la modifica delle impostazioni di protezione SCHANNEL potrebbe interrompere o impedire le comunicazioni tra client e i server.  Ciò si verifica se è necessaria una comunicazione protetta e non hanno un protocollo di negoziazione delle comunicazioni con.
> 
> Se si applicano queste modifiche, devono essere applicati a tutti i server ADFS nella farm.  Dopo aver applicato queste modifiche è necessario un riavvio.

Nel giorno odierna e fascia d'età, protezione avanzata dei server e la rimozione di vecchi o pacchetti di crittografia vulnerabili sta diventando delle principali priorità per molte organizzazioni.  Sono disponibili pacchetti di software che verrà testare i server e forniscono informazioni dettagliate su questi protocolli e pacchetti.  Per elementi rimangono conformi o ottenere classificazioni sicure, rimuovere o disattivare i protocolli più vulnerabili o pacchetti di crittografia è diventato un deve.  Nel resto di questo documento verrà fornite informazioni aggiuntive su come abilitare o disabilitare determinati protocolli e pacchetti di crittografia.

Le chiavi del Registro di sistema seguente si trovano nello stesso percorso: HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols.  Usare regedit o PowerShell per abilitare o disabilitare questi protocolli e pacchetti di crittografia.

![Percorso del Registro di sistema](media/Managing-SSL-Protocols-in-AD-FS/registry.png)

## <a name="enable-and-disable-ssl-20"></a>Abilitare e disabilitare SSL 2.0
Per abilitare e disabilitare SSL 2.0, utilizzare le seguenti chiavi del Registro di sistema e i relativi valori.

### <a name="enable-ssl-20"></a>Attivare SSL 2.0
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\SSL 2.0\Server] "Attivato" = DWORD: 00000001
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\SSL 2.0\Server] "DisabledByDefault" = DWORD: 00000000
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\SSL 2.0\Client] "Attivato" = DWORD: 00000001
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\SSL 2.0\Client] "DisabledByDefault" = DWORD: 00000000 

### <a name="disable-ssl-20"></a>Disabilitare SSL 2.0
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\SL 2.0\Server] "Attivato" = DWORD: 00000000
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\SSL 2.0\Server] "DisabledByDefault" = DWORD: 00000001 
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\SSL 2.0\Client] "Attivato" = DWORD: 00000000
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\SSL 2.0\Client] "DisabledByDefault" = DWORD: 00000001

### <a name="using-powershell-to-disable-ssl-20"></a>Utilizzo di PowerShell per disabilitare SSL 2.0

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
Per abilitare e disabilitare SSL 3.0, utilizzare le seguenti chiavi del Registro di sistema e i relativi valori.

### <a name="enable-ssl-30"></a>Abilitare SSL 3.0
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\SSL 3.0\Server] "Attivato" = DWORD: 00000001
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\SSL 3.0\Server] "DisabledByDefault" = DWORD: 00000000 
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\SSL 3.0\Client] "Attivato" = DWORD: 00000001
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\SSL 3.0\Client] "DisabledByDefault" = DWORD: 00000000 

### <a name="disable-ssl-30"></a>Disabilitare SSL 3.0
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\SSL 3.0\Server] "Attivato" = DWORD: 00000000
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\SSL 3.0\Server] "DisabledByDefault" = DWORD: 00000001
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\SSL 3.0\Client] "Attivato" = DWORD: 00000000
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\SSL 3.0\Client] "DisabledByDefault" = DWORD: 00000001 

### <a name="using-powershell-to-disable-ssl-30"></a>Utilizzo di PowerShell per disabilitare SSL 3.0

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
> La disabilitazione di TLS 1.0 interromperà il WAP attendibilità di AD FS.  Se si disabilita TLS 1.0 è necessario abilitare l'autenticazione avanzata per le applicazioni.  Vedere [abilitare l'autenticazione avanzata](#enabling-strong-authentication-for-net-applications) 



### <a name="enable-tls-10"></a>Abilitare TLS 1.0
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.0\Server] "Attivato" = DWORD: 00000001
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.0\Server] "DisabledByDefault" = DWORD: 00000000 
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.0\Client] "Attivato" = DWORD: 00000001
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.0\Client] "DisabledByDefault" = DWORD: 00000000 

### <a name="disable-tls-10"></a>Disabilitare TLS 1.0
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.0\Server] "Attivato" = DWORD: 00000000
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.0\Server] "DisabledByDefault" = DWORD: 00000001
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.0\Client] "Attivato" = DWORD: 00000000
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.0\Client] "DisabledByDefault" = DWORD: 00000001 

### <a name="using-powershell-to-disable-tls-10"></a>Utilizzo di PowerShell per disabilitare TLS 1.0

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
Per abilitare e disabilitare TLS 1.1, utilizzare le seguenti chiavi del Registro di sistema e i relativi valori.

### <a name="enable-tls-11"></a>Abilitare TLS 1.1
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.1\Server] "Attivato" = DWORD: 00000001
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.1\Server] "DisabledByDefault" = DWORD: 00000000
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.1\Client] "Attivato" = DWORD: 00000001
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.1\Client] "DisabledByDefault" = DWORD: 00000000

### <a name="disable-tls-11"></a>Disabilitare TLS 1.1
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.1\Server] "Attivato" = DWORD: 00000000
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.1\Server] "DisabledByDefault" = DWORD: 00000001
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.1\Client] "Attivato" = DWORD: 00000000
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.1\Client] "DisabledByDefault" = DWORD: 00000001 

### <a name="using-powershell-to-disable-tls-11"></a>Utilizzo di PowerShell per disabilitare TLS 1.1

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

Per abilitare e disabilitare TLS 1.2, utilizzare le seguenti chiavi del Registro di sistema e i relativi valori.

### <a name="enable-tls-12"></a>Abilitare TLS 1.2
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2\Server] "Attivato" = DWORD: 00000001
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2\Server] "DisabledByDefault" = DWORD: 00000000 
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2\Client] "Attivato" = DWORD: 00000001
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2\Client] "DisabledByDefault" = DWORD: 00000000

### <a name="disable-tls-12"></a>Disabilitare TLS 1.2
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2\Server] "Attivato" = DWORD: 00000000
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2\Server] "DisabledByDefault" = DWORD: 00000001
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2\Client] "Attivato" = DWORD: 00000000
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2\Client] "DisabledByDefault" = DWORD: 00000001

### <a name="using-powershell-to-disable-tls-12"></a>Utilizzo di PowerShell per disabilitare TLS 1.2

```powershell
    New-Item 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2\Server' -Force | Out-Null
    
    New-ItemProperty -path 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2\Server' -name 'Enabled' -value '0' -PropertyType 'DWord' -Force | Out-Null
    
    New-ItemProperty -path 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2\Server' -name 'DisabledByDefault' -value 1 -PropertyType 'DWord' -Force | Out-Null
    
    New-Item 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2\Client' -Force | Out-Null
    
    New-ItemProperty -path 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2\Client' -name 'Enabled' -value '0' -PropertyType 'DWord' -Force | Out-Null
    
    New-ItemProperty -path 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2\Client' -name 'DisabledByDefault' -value 1 -PropertyType 'DWord' -Force | Out-Null
    Write-Host 'TLS 1.2 has been disabled.'
```

## <a name="enable-and-disable-rc4"></a>Abilitare e disabilitare la crittografia RC4 

Per abilitare e disabilitare RC4, utilizzare le seguenti chiavi del Registro di sistema e i relativi valori.  Le chiavi del Registro di sistema di questo pacchetto di crittografia si trovano qui:

- HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Ciphers\

![Percorso del Registro di sistema](media/Managing-SSL-Protocols-in-AD-FS/cipher.png)



### <a name="enable-rc4"></a>Abilitare la crittografia RC4

- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Ciphers\RC4 128/128] "Attivato" = DWORD: 00000001
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Ciphers\RC4 40/128] "Attivato" = DWORD: 00000001
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Ciphers\RC4 56/128] "Attivato" = DWORD: 00000001 

### <a name="disable-rc4"></a>Disabilitare la crittografia RC4

- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Ciphers\RC4 128/128] "Attivato" = DWORD: 00000000
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Ciphers\RC4 40/128] "Attivato" = DWORD: 00000000
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Ciphers\RC4 56/128] "Attivato" = DWORD: 00000000 

### <a name="using-powershell"></a>Utilizzo di PowerShell

```powershell
    ([Microsoft.Win32.RegistryKey]::OpenRemoteBaseKey([Microsoft.Win32.RegistryHive]::LocalMachine,$env:COMPUTERNAME)).CreateSubKey('SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Ciphers\RC4 128/128') 
    New-ItemProperty -path 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Ciphers\RC4 128/128' -name 'Enabled' -value '0' -PropertyType 'DWord' -Force | Out-Null
    
    ([Microsoft.Win32.RegistryKey]::OpenRemoteBaseKey([Microsoft.Win32.RegistryHive]::LocalMachine,$env:COMPUTERNAME)).CreateSubKey('SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Ciphers\RC4 40/128') 
    New-ItemProperty -path 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Ciphers\RC4 40/128' -name 'Enabled' -value '0' -PropertyType 'DWord' -Force | Out-Null
    
    ([Microsoft.Win32.RegistryKey]::OpenRemoteBaseKey([Microsoft.Win32.RegistryHive]::LocalMachine,$env:COMPUTERNAME)).CreateSubKey('SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Ciphers\RC4 56/128') 
    New-ItemProperty -path 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Ciphers\RC4 56/128' -name 'Enabled' -value '0' -PropertyType 'DWord' -Force | Out-Null
```

## <a name="enabling-or-disabling-additional-cipher-suites"></a>Abilitazione o disabilitazione di pacchetti di crittografia aggiuntivi

È possibile disabilitare determinati crittografie a specifici rimuovendole dal HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Cryptography\Configuration\Local\SSL\0010002 

![Percorso del Registro di sistema](media/Managing-SSL-Protocols-in-AD-FS/suites.png)

Per abilitare un pacchetto di crittografia, questo è il valore di stringa per la chiave di valore multistringa funzioni.  Ad esempio, se si desidera abilitare TLS_ECDHE_RSA_WITH_AES_256_CBC_SHA384_P521 ci sarebbe aggiungerlo alla stringa.

Per un elenco completo di crittografia supportato vedere gruppi [pacchetti di crittografia di TLS/SSL (SSP Schannel)](https://msdn.microsoft.com/en-us/library/windows/desktop/aa374757.aspx).  Questo documento viene fornita una tabella dei pacchetti che sono abilitati per impostazione predefinita e quelli che sono supportati ma non abilitato per impostazione predefinita.  Per stabilire le priorità, vedere i pacchetti di crittografia [assegnare la priorità ai pacchetti di crittografia Schannel](https://msdn.microsoft.com/en-us/library/windows/desktop/bb870930.aspx).

## <a name="enabling-strong-authentication-for-net-applications"></a>Abilitazione dell'autenticazione avanzata per le applicazioni .NET
Le applicazioni di .NET Framework 3.5/4.0/4.5.x possono passare il protocollo predefinito per TLS 1.2 abilitando la chiave del Registro di sistema SchUseStrongCrypto.  Questa chiave del Registro di sistema forzerà applicazioni .NET utilizzano TLS 1.2.

> [!IMPORTANT]
> Per AD FS in Windows Server 2016 e Windows Server 2012 R2 è necessario utilizzare la chiave di .NET Framework 4.0 o 4.5: HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\. NETFramework\v4.0.30319


Per .NET Framework 3.5, utilizzare la seguente chiave del Registro di sistema:

[HKEY_LOCAL_MACHINE\SOFTWARE\Wow6432Node\Microsoft\. NETFramework\v2.0.50727] "SchUseStrongCrypto" = DWORD: 00000001

Per .NET Framework 4.0 o 4.5 utilizzare la seguente chiave del Registro di sistema: HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\. NETFramework\v4.0.30319 "SchUseStrongCrypto" = DWORD: 00000001

![Autenticazione avanzata](media/Managing-SSL-Protocols-in-AD-FS/strongauth.png)

```powershell
    
    New-ItemProperty -path 'HKLM:\SOFTWARE\Microsoft\.NetFramework\v4.0.30319' -name 'SchUseStrongCrypto' -value '1' -PropertyType 'DWord' -Force | Out-Null
```

## <a name="additional-information"></a>Informazioni aggiuntive

- [Pacchetti di crittografia di TLS/SSL (SSP Schannel)](https://msdn.microsoft.com/en-us/library/windows/desktop/aa374757.aspx)
- [Pacchetti di crittografia TLS in Windows 8.1](https://msdn.microsoft.com/en-us/library/windows/desktop/mt767781.aspx)
- [Definizione delle priorità dei pacchetti di crittografia Schannel](https://msdn.microsoft.com/en-us/library/windows/desktop/bb870930.aspx)
- [Crittografia e altre lingue Enigmatic parlare](https://blogs.technet.microsoft.com/askds/2015/12/08/speaking-in-ciphers-and-other-enigmatic-tonguesupdate/)
