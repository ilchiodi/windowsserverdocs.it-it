---
title: Gestione dei protocolli SSL/TLS e dei pacchetti di crittografia per AD FS
description: Domande frequenti per AD FS 2016
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 44fb4c02421a431edb502daecaa38f00fb4dd2ad
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71407531"
---
# <a name="managing-ssltls-protocols-and-cipher-suites-for-ad-fs"></a>Gestione dei protocolli SSL/TLS e dei pacchetti di crittografia per AD FS
Nella documentazione seguente vengono fornite informazioni su come disabilitare e abilitare alcuni protocolli TLS/SSL e pacchetti di crittografia utilizzati da AD FS

## <a name="tlsssl-schannel-and-cipher-suites-in-ad-fs"></a>TLS/SSL, SChannel e pacchetti di crittografia in AD FS

Il Transport Layer Security (TLS) e il Secure Sockets Layer (SSL) sono protocolli che forniscono comunicazioni sicure.  Active Directory Federation Services utilizza questi protocolli per le comunicazioni.  Attualmente sono disponibili diverse versioni di questi protocolli.

Schannel è un Security Support Provider (SSP) che implementa i protocolli di autenticazione standard Internet SSL, TLS E DTLS. Security Support Provider Interface (SSPI) è un'API utilizzata dai sistemi Windows per eseguire funzioni correlate alla sicurezza, tra cui l'autenticazione. SSPI funziona come un'interfaccia comune per diversi Security Support Provider (SSP), tra cui l'SSP Schannel.

Un pacchetto di crittografia è un set di algoritmi di crittografia. L'implementazione SSP Schannel dei protocolli TLS/SSL usa gli algoritmi di un pacchetto di crittografia per creare chiavi e crittografare le informazioni. Un pacchetto di crittografia specifica un algoritmo per ognuna delle attività seguenti:

- Scambio di chiave
- Crittografia di massa
- Autenticazione dei messaggi

AD FS utilizza Schannel. dll per eseguire le interazioni di comunicazione sicure.  Attualmente AD FS supporta tutti i protocolli e i pacchetti di crittografia supportati da Schannel. dll.

## <a name="managing-the-tlsssl-protocols-and-cipher-suites"></a>Gestione dei protocolli TLS/SSL e dei pacchetti di crittografia
> [!IMPORTANT]
> Questa sezione contiene i passaggi che indicano come modificare il registro di sistema. Tuttavia, potrebbero verificarsi gravi problemi se si modifica il registro di sistema in modo errato. Di conseguenza, è importante seguire questa procedura. 
> 
> Tenere presente che la modifica delle impostazioni di sicurezza predefinite per SCHANNEL potrebbe interrompere o impedire le comunicazioni tra determinati client e server.  Questo problema si verificherà se è necessaria una comunicazione protetta e non si dispone di un protocollo per negoziare le comunicazioni con.
> 
> Se si applicano queste modifiche, è necessario applicarle a tutti i server AD FS della farm.  Dopo aver applicato queste modifiche, è necessario riavviare il computer.

In giorni ed età, la protezione avanzata dei server e la rimozione di pacchetti di crittografia meno recenti o vulnerabili stanno diventando una priorità fondamentale per molte organizzazioni.  Sono disponibili gruppi software che consentiranno di testare i server e fornire informazioni dettagliate su questi protocolli e gruppi.  Per mantenere la conformità o ottenere valutazioni sicure, la rimozione o la disabilitazione di protocolli o pacchetti di crittografia più vulnerabili è diventata un must.  Nella parte restante di questo documento vengono fornite indicazioni su come abilitare o disabilitare alcuni protocolli e pacchetti di crittografia.

Le chiavi del registro di sistema riportate di seguito si trovano nello stesso percorso: HKEY_LOCAL_MACHINE \SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols.  Usare regedit o PowerShell per abilitare o disabilitare questi protocolli e pacchetti di crittografia.

![Percorso nel Registro di sistema](media/Managing-SSL-Protocols-in-AD-FS/registry.png)

## <a name="enable-and-disable-ssl-20"></a>Abilitare e disabilitare SSL 2,0
Utilizzare le seguenti chiavi del registro di sistema e i relativi valori per abilitare e disabilitare SSL 2,0.

### <a name="enable-ssl-20"></a>Abilitare SSL 2,0
- [HKEY_LOCAL_MACHINE \SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\SSL 2.0 \ Server] "Enabled" = DWORD: 00000001
- [HKEY_LOCAL_MACHINE \SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\SSL 2.0 \ Server] "DisabledByDefault" = DWORD: 00000000
- [HKEY_LOCAL_MACHINE \SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\SSL 2.0 \ client] "Enabled" = DWORD: 00000001
- [HKEY_LOCAL_MACHINE \SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\SSL 2.0 \ client] "DisabledByDefault" = DWORD: 00000000 

### <a name="disable-ssl-20"></a>Disabilitare SSL 2,0
- [HKEY_LOCAL_MACHINE \SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\SSL 2.0 \ Server] "Enabled" = DWORD: 00000000
- [HKEY_LOCAL_MACHINE \SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\SSL 2.0 \ Server] "DisabledByDefault" = DWORD: 00000001 
- [HKEY_LOCAL_MACHINE \SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\SSL 2.0 \ client] "Enabled" = DWORD: 00000000
- [HKEY_LOCAL_MACHINE \SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\SSL 2.0 \ client] "DisabledByDefault" = DWORD: 00000001

### <a name="using-powershell-to-disable-ssl-20"></a>Uso di PowerShell per disabilitare SSL 2,0

``` powershell
New-Item 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\SSL 2.0\Server' -Force | Out-Null
    
New-ItemProperty -path 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\SSL 2.0\Server' -name 'Enabled' -value '0' -PropertyType 'DWord' -Force | Out-Null
            
New-ItemProperty -path 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\SSL 2.0\Server' -name 'DisabledByDefault' -value 1 -PropertyType 'DWord' -Force | Out-Null
            
New-Item 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\SSL 2.0\Client' -Force | Out-Null
            
New-ItemProperty -path 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\SSL 2.0\Client' -name 'Enabled' -value '0' -PropertyType 'DWord' -Force | Out-Null
            
New-ItemProperty -path 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\SSL 2.0\Client' -name 'DisabledByDefault' -value 1 -PropertyType 'DWord' -Force | Out-Null
Write-Host 'SSL 2.0 has been disabled.'
```

## <a name="enable-and-disable-ssl-30"></a>Abilitare e disabilitare SSL 3,0
Utilizzare le seguenti chiavi del registro di sistema e i relativi valori per abilitare e disabilitare SSL 3,0.

### <a name="enable-ssl-30"></a>Abilitare SSL 3.0
- [HKEY_LOCAL_MACHINE \SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\SSL 3.0 \ Server] "Enabled" = DWORD: 00000001
- [HKEY_LOCAL_MACHINE \SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\SSL 3.0 \ Server] "DisabledByDefault" = DWORD: 00000000 
- [HKEY_LOCAL_MACHINE \SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\SSL 3.0 \ client] "Enabled" = DWORD: 00000001
- [HKEY_LOCAL_MACHINE \SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\SSL 3.0 \ client] "DisabledByDefault" = DWORD: 00000000 

### <a name="disable-ssl-30"></a>Disabilitare SSL 3,0
- [HKEY_LOCAL_MACHINE \SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\SSL 3.0 \ Server] "Enabled" = DWORD: 00000000
- [HKEY_LOCAL_MACHINE \SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\SSL 3.0 \ Server] "DisabledByDefault" = DWORD: 00000001
- [HKEY_LOCAL_MACHINE \SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\SSL 3.0 \ client] "Enabled" = DWORD: 00000000
- [HKEY_LOCAL_MACHINE \SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\SSL 3.0 \ client] "DisabledByDefault" = DWORD: 00000001 

### <a name="using-powershell-to-disable-ssl-30"></a>Uso di PowerShell per disabilitare SSL 3,0

```powershell
    New-Item 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\SSL 3.0\Server' -Force | Out-Null
    
    New-ItemProperty -path 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\SSL 3.0\Server' -name 'Enabled' -value '0' -PropertyType 'DWord' -Force | Out-Null
    
    New-ItemProperty -path 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\SSL 3.0\Server' -name 'DisabledByDefault' -value 1 -PropertyType 'DWord' -Force | Out-Null
    
    New-Item 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\SSL 3.0\Client' -Force | Out-Null
    
    New-ItemProperty -path 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\SSL 3.0\Client' -name 'Enabled' -value '0' -PropertyType 'DWord' -Force | Out-Null
    
    New-ItemProperty -path 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\SSL 3.0\Client' -name 'DisabledByDefault' -value 1 -PropertyType 'DWord' -Force | Out-Null
    Write-Host 'SSL 3.0 has been disabled.'
```

## <a name="enable-and-disable-tls-10"></a>Abilitare e disabilitare TLS 1,0
Usare le chiavi del registro di sistema seguenti e i relativi valori per abilitare e disabilitare TLS 1,0.

> [!IMPORTANT]
> La disabilitazione di TLS 1,0 interrompe il WAP per AD FS attendibilità.  Se si disattiva TLS 1,0, è necessario abilitare l'autenticazione avanzata per le applicazioni.  Vedere [abilitare l'autenticazione avanzata](#enabling-strong-authentication-for-net-applications) 



### <a name="enable-tls-10"></a>Abilitare TLS 1,0
- [HKEY_LOCAL_MACHINE \SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.0 \ Server] "Enabled" = DWORD: 00000001
- [HKEY_LOCAL_MACHINE \SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.0 \ Server] "DisabledByDefault" = DWORD: 00000000 
- [HKEY_LOCAL_MACHINE \SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.0 \ client] "Enabled" = DWORD: 00000001
- [HKEY_LOCAL_MACHINE \SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.0 \ client] "DisabledByDefault" = DWORD: 00000000 

### <a name="disable-tls-10"></a>Disabilitare TLS 1,0
- [HKEY_LOCAL_MACHINE \SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.0 \ Server] "Enabled" = DWORD: 00000000
- [HKEY_LOCAL_MACHINE \SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.0 \ Server] "DisabledByDefault" = DWORD: 00000001
- [HKEY_LOCAL_MACHINE \SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.0 \ client] "Enabled" = DWORD: 00000000
- [HKEY_LOCAL_MACHINE \SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.0 \ client] "DisabledByDefault" = DWORD: 00000001 

### <a name="using-powershell-to-disable-tls-10"></a>Uso di PowerShell per disabilitare TLS 1,0

```powershell
    New-Item 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.0\Server' -Force | Out-Null
    
    New-ItemProperty -path 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.0\Server' -name 'Enabled' -value '0' -PropertyType 'DWord' -Force | Out-Null
    
    New-ItemProperty -path 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.0\Server' -name 'DisabledByDefault' -value 1 -PropertyType 'DWord' -Force | Out-Null
    
    New-Item 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.0\Client' -Force | Out-Null
    
    New-ItemProperty -path 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.0\Client' -name 'Enabled' -value '0' -PropertyType 'DWord' -Force | Out-Null
    
    New-ItemProperty -path 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.0\Client' -name 'DisabledByDefault' -value 1 -PropertyType 'DWord' -Force | Out-Null
    Write-Host 'TLS 1.0 has been disabled.'
```


## <a name="enable-and-disable-tls-11"></a>Abilitare e disabilitare TLS 1,1
Usare le chiavi del registro di sistema seguenti e i relativi valori per abilitare e disabilitare TLS 1,1.

### <a name="enable-tls-11"></a>Abilitare TLS 1,1
- [HKEY_LOCAL_MACHINE \SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.1 \ Server] "Enabled" = DWORD: 00000001
- [HKEY_LOCAL_MACHINE \SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.1 \ Server] "DisabledByDefault" = DWORD: 00000000
- [HKEY_LOCAL_MACHINE \SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.1 \ client] "Enabled" = DWORD: 00000001
- [HKEY_LOCAL_MACHINE \SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.1 \ client] "DisabledByDefault" = DWORD: 00000000

### <a name="disable-tls-11"></a>Disabilitare TLS 1,1
- [HKEY_LOCAL_MACHINE \SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.1 \ Server] "Enabled" = DWORD: 00000000
- [HKEY_LOCAL_MACHINE \SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.1 \ Server] "DisabledByDefault" = DWORD: 00000001
- [HKEY_LOCAL_MACHINE \SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.1 \ client] "Enabled" = DWORD: 00000000
- [HKEY_LOCAL_MACHINE \SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.1 \ client] "DisabledByDefault" = DWORD: 00000001 

### <a name="using-powershell-to-disable-tls-11"></a>Uso di PowerShell per disabilitare TLS 1,1

``` powershell
    New-Item 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.1\Server' -Force | Out-Null
    
    New-ItemProperty -path 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.1\Server' -name 'Enabled' -value '0' -PropertyType 'DWord' -Force | Out-Null
    
    New-ItemProperty -path 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.1\Server' -name 'DisabledByDefault' -value 1 -PropertyType 'DWord' -Force | Out-Null
    
    New-Item 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.1\Client' -Force | Out-Null
    
    New-ItemProperty -path 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.1\Client' -name 'Enabled' -value '0' -PropertyType 'DWord' -Force | Out-Null
    
    New-ItemProperty -path 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.1\Client' -name 'DisabledByDefault' -value 1 -PropertyType 'DWord' -Force | Out-Null
    Write-Host 'TLS 1.1 has been disabled.'
```

## <a name="enable-and-disable-tls-12"></a>Abilitare e disabilitare TLS 1,2

Usare le chiavi del registro di sistema seguenti e i relativi valori per abilitare e disabilitare TLS 1,2.

### <a name="enable-tls-12"></a>Abilitare TLS 1,2
- [HKEY_LOCAL_MACHINE \SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2 \ Server] "Enabled" = DWORD: 00000001
- [HKEY_LOCAL_MACHINE \SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2 \ Server] "DisabledByDefault" = DWORD: 00000000 
- [HKEY_LOCAL_MACHINE \SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2 \ client] "Enabled" = DWORD: 00000001
- [HKEY_LOCAL_MACHINE \SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2 \ client] "DisabledByDefault" = DWORD: 00000000

### <a name="disable-tls-12"></a>Disabilitare TLS 1,2
- [HKEY_LOCAL_MACHINE \SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2 \ Server] "Enabled" = DWORD: 00000000
- [HKEY_LOCAL_MACHINE \SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2 \ Server] "DisabledByDefault" = DWORD: 00000001
- [HKEY_LOCAL_MACHINE \SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2 \ client] "Enabled" = DWORD: 00000000
- [HKEY_LOCAL_MACHINE \SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2 \ client] "DisabledByDefault" = DWORD: 00000001

### <a name="using-powershell-to-disable-tls-12"></a>Uso di PowerShell per disabilitare TLS 1,2

```powershell
    New-Item 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2\Server' -Force | Out-Null
    
    New-ItemProperty -path 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2\Server' -name 'Enabled' -value '0' -PropertyType 'DWord' -Force | Out-Null
    
    New-ItemProperty -path 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2\Server' -name 'DisabledByDefault' -value 1 -PropertyType 'DWord' -Force | Out-Null
    
    New-Item 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2\Client' -Force | Out-Null
    
    New-ItemProperty -path 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2\Client' -name 'Enabled' -value '0' -PropertyType 'DWord' -Force | Out-Null
    
    New-ItemProperty -path 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2\Client' -name 'DisabledByDefault' -value 1 -PropertyType 'DWord' -Force | Out-Null
    Write-Host 'TLS 1.2 has been disabled.'
```

## <a name="enable-and-disable-rc4"></a>Abilitare e disabilitare RC4 

Utilizzare le seguenti chiavi del registro di sistema e i relativi valori per abilitare e disabilitare RC4.  Queste chiavi del registro di sistema del pacchetto di crittografia sono disponibili qui:

- HKEY_LOCAL_MACHINE \SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Ciphers\

![Percorso nel Registro di sistema](media/Managing-SSL-Protocols-in-AD-FS/cipher.png)



### <a name="enable-rc4"></a>Abilita RC4

- [HKEY_LOCAL_MACHINE \SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Ciphers\RC4 128/128] "Enabled" = DWORD: 00000001
- [HKEY_LOCAL_MACHINE \SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Ciphers\RC4 40/128] "Enabled" = DWORD: 00000001
- [HKEY_LOCAL_MACHINE \SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Ciphers\RC4 56/128] "Enabled" = DWORD: 00000001 

### <a name="disable-rc4"></a>Disabilitare RC4

- [HKEY_LOCAL_MACHINE \SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Ciphers\RC4 128/128] "Enabled" = DWORD: 00000000
- [HKEY_LOCAL_MACHINE \SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Ciphers\RC4 40/128] "Enabled" = DWORD: 00000000
- [HKEY_LOCAL_MACHINE \SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Ciphers\RC4 56/128] "Enabled" = DWORD: 00000000 

### <a name="using-powershell"></a>Mediante PowerShell

```powershell
    ([Microsoft.Win32.RegistryKey]::OpenRemoteBaseKey([Microsoft.Win32.RegistryHive]::LocalMachine,$env:COMPUTERNAME)).CreateSubKey('SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Ciphers\RC4 128/128') 
    New-ItemProperty -path 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Ciphers\RC4 128/128' -name 'Enabled' -value '0' -PropertyType 'DWord' -Force | Out-Null
    
    ([Microsoft.Win32.RegistryKey]::OpenRemoteBaseKey([Microsoft.Win32.RegistryHive]::LocalMachine,$env:COMPUTERNAME)).CreateSubKey('SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Ciphers\RC4 40/128') 
    New-ItemProperty -path 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Ciphers\RC4 40/128' -name 'Enabled' -value '0' -PropertyType 'DWord' -Force | Out-Null
    
    ([Microsoft.Win32.RegistryKey]::OpenRemoteBaseKey([Microsoft.Win32.RegistryHive]::LocalMachine,$env:COMPUTERNAME)).CreateSubKey('SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Ciphers\RC4 56/128') 
    New-ItemProperty -path 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Ciphers\RC4 56/128' -name 'Enabled' -value '0' -PropertyType 'DWord' -Force | Out-Null
```

## <a name="enabling-or-disabling-additional-cipher-suites"></a>Abilitazione o disabilitazione di gruppi di crittografia aggiuntivi

È possibile disabilitare determinate crittografie specifiche rimuovendo tali crittografie da HKEY_LOCAL_MACHINE \SYSTEM\CurrentControlSet\Control\Cryptography\Configuration\Local\SSL\00010002 

![Percorso nel Registro di sistema](media/Managing-SSL-Protocols-in-AD-FS/suites.png)

Per abilitare un pacchetto di crittografia, aggiungere il relativo valore stringa alla chiave del valore multistringa Functions.  Ad esempio, se si vuole abilitare TLS_ECDHE_RSA_WITH_AES_256_CBC_SHA384_P521, è necessario aggiungerlo alla stringa.

Per un elenco completo dei pacchetti di crittografia supportati, vedere pacchetti di [crittografia in TLS/SSL (SSP Schannel)](https://msdn.microsoft.com/library/windows/desktop/aa374757.aspx).  Questo documento fornisce una tabella di gruppi abilitati per impostazione predefinita e quelli supportati ma non abilitati per impostazione predefinita.  Per classificare in ordine di priorità i pacchetti di crittografia vedere priorità dei pacchetti di [crittografia Schannel](https://msdn.microsoft.com/library/windows/desktop/bb870930.aspx).

## <a name="enabling-strong-authentication-for-net-applications"></a>Abilitazione dell'autenticazione avanzata per le applicazioni .NET
Le applicazioni .NET Framework 3.5/4.0/4.5. x possono impostare il protocollo predefinito su TLS 1,2 abilitando la chiave del registro di sistema SchUseStrongCrypto.  Questa chiave del registro di sistema forza le applicazioni .NET a usare TLS 1,2.

> [!IMPORTANT]
> Per AD FS in Windows Server 2016 e Windows Server 2012 R2, è necessario usare la chiave .NET Framework 4.0/4.5. x: HKEY_LOCAL_MACHINE \SOFTWARE\Microsoft\\. NETFramework\v4.0.30319


Per la .NET Framework 3,5 usare la chiave del registro di sistema seguente:

[HKEY_LOCAL_MACHINE\\\SOFTWARE\Wow6432Node\Microsoft. NETFramework\v2.0.50727] "SchUseStrongCrypto" = DWORD: 00000001

Per la .NET Framework 4.0/4.5. x usare la chiave del registro di sistema seguente: HKEY_LOCAL_MACHINE \SOFTWARE\Microsoft\\. NETFramework\v4.0.30319 "SchUseStrongCrypto" = DWORD: 00000001

![Autenticazione avanzata](media/Managing-SSL-Protocols-in-AD-FS/strongauth.png)

```powershell
    
    New-ItemProperty -path 'HKLM:\SOFTWARE\Microsoft\.NetFramework\v4.0.30319' -name 'SchUseStrongCrypto' -value '1' -PropertyType 'DWord' -Force | Out-Null
```

## <a name="additional-information"></a>Informazioni aggiuntive

- [Pacchetti di crittografia in TLS/SSL (SSP Schannel)](https://msdn.microsoft.com/library/windows/desktop/aa374757.aspx)
- [Pacchetti di crittografia TLS in Windows 8.1](https://msdn.microsoft.com/library/windows/desktop/mt767781.aspx)
- [Assegnazione delle priorità ai pacchetti di crittografia Schannel](https://msdn.microsoft.com/library/windows/desktop/bb870930.aspx)
- [Parlare con crittografie e altre lingue enigmatiche](https://blogs.technet.microsoft.com/askds/2015/12/08/speaking-in-ciphers-and-other-enigmatic-tonguesupdate/)
