---
title: Creare file di risposte di specializzazione del sistema operativo
ms.custom: na
ms.prod: windows-server-threshold
ms.topic: article
ms.assetid: 299aa38e-28d2-4cbe-af16-5b8c533eba1f
manager: dongill
author: rpsqrd
ms.technology: security-guarded-fabric
ms.date: 08/29/2018
ms.openlocfilehash: 5717fcc9e1732b6273620e633c140c6df58ec8b7
ms.sourcegitcommit: 29ad32b9dea298a7fe81dcc33d2a42d383018e82
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/15/2019
ms.locfileid: "65624651"
---
# <a name="create-os-specialization-answer-file"></a>Creare file di risposte di specializzazione del sistema operativo

>Si applica a: Windows Server 2019, Windows Server (canale semestrale), Windows Server 2016

Prepararsi a distribuire le macchine virtuali schermate, devi creare un file di risposte di specializzazione del sistema operativo. In Windows, questo è comunemente noto come il file "Unattend. XML". Il **New-ShieldingDataAnswerFile** funzione di Windows PowerShell consente di eseguire questa operazione. È possibile quindi usare il file di risposte durante la creazione di macchine virtuali schermate da un modello usando System Center Virtual Machine Manager (o qualsiasi altro controller di infrastruttura).

Per linee guida generali per i file di installazione automatica per le macchine virtuali schermate, vedere [creare un file di risposte](guarded-fabric-tenant-creates-shielding-data.md#create-an-answer-file).
 
## <a name="downloading-the-new-shieldingdataanswerfile-function"></a>Scaricare la funzione New-ShieldingDataAnswerFile

È possibile ottenere il **New-ShieldingDataAnswerFile** funzione di [PowerShell Gallery](https://aka.ms/gftools). Se il computer abbia la connettività Internet, è possibile installarlo da PowerShell con il comando seguente:

```powershell
Install-Module GuardedFabricTools -Repository PSGallery -MinimumVersion 1.0.0
```

Il `unattend.xml` output può essere assemblato in dati di schermatura, con gli elementi aggiuntivi, in modo che può essere utilizzato per creare le macchine virtuali schermate da modelli.

Le sezioni seguenti illustrano come usare i parametri della funzione per un `unattend.xml` file che contiene le varie opzioni:

- [File di risposte di base di Windows](#basic-windows-answer-file)
- [File con aggiunta al dominio di risposte Windows](#windows-answer-file-with-domain-join)
- [File di risposte Windows con indirizzi IPv4 statici](#windows-answer-file-with-static-ipv4-addresses)
- [File di risposte Windows con le impostazioni locali personalizzate](#windows-answer-file-with-a-custom-locale)
- [File di risposte di Linux base](#basic-linux-answer-file)

## <a name="basic-windows-answer-file"></a>File di risposte Windows di base

I comandi seguenti creano un file di risposte Windows imposta semplicemente la password dell'account amministratore e il nome host.
Le schede di rete della macchina virtuale utilizzerà DHCP per ottenere gli indirizzi IP e la macchina virtuale non verrà aggiunti a un dominio di Active Directory.
Quando viene richiesto di immettere credenziali di amministratore, specificare il nome utente desiderato e la password.
Utilizzare "Administrator" per il nome utente se si vuole configurare l'account Administrator predefinito.

```powershell
$adminCred = Get-Credential -Message "Local administrator account"

New-ShieldingDataAnswerFile -Path '.\ShieldedVMAnswerFile.xml' -AdminCredentials $adminCred
```

## <a name="windows-answer-file-with-domain-join"></a>File con aggiunta al dominio di risposte Windows

I comandi seguenti creano un file di risposte Windows che unisce in join la macchina virtuale schermata a un dominio di Active Directory.
Le schede di rete della macchina virtuale utilizzerà DHCP per ottenere gli indirizzi IP.

La prima richiesta di credenziali verrà richiesto per le informazioni dell'account amministratore locale.
Utilizzare "Administrator" per il nome utente se si vuole configurare l'account Administrator predefinito.

La seconda richiesta di credenziali verrà richieste le credenziali che hanno il diritto di aggiungere la macchina al dominio di Active Directory.

Assicurarsi di modificare il valore della "-DomainName" parametro per il nome FQDN del dominio di Active Directory.

```powershell
$adminCred = Get-Credential -Message "Local administrator account"
$domainCred = Get-Credential -Message "Domain join credentials"

New-ShieldingDataAnswerFile -Path '.\ShieldedVMAnswerFile.xml' -AdminCredentials $adminCred -DomainName 'my.contoso.com' -DomainJoinCredentials $domainCred
```
## <a name="windows-answer-file-with-static-ipv4-addresses"></a>File di risposte Windows con indirizzi IPv4 statici

I comandi seguenti creano un file di risposte Windows che usa indirizzi IP statici forniti in fase di distribuzione per la gestione dell'infrastruttura, ad esempio System Center Virtual Machine Manager.

Virtual Machine Manager offre tre componenti per l'indirizzo IP statico con un pool di indirizzi IP: Indirizzo IPv4, IPv6 indirizzo, indirizzo del gateway e l'indirizzo DNS. Se si desidera che i campi aggiuntivi da includere o richiedono una configurazione di rete personalizzata, è necessario modificare manualmente il file di risposta generato dallo script.

Le schermate seguenti illustrano i pool IP che è possibile configurare in Virtual Machine Manager. Questi pool sono necessari se si vuole usare un indirizzo IP statico.

Attualmente, la funzione supporta solo un server DNS. Ecco le impostazioni DNS aspetto:

![La configurazione del Server DNS con pool IP statici](../media/Guarded-Fabric-Shielded-VM/guarded-host-unattend-static-ip-address-pool-dns-settings.png)

Ecco l'aspetto di riepilogo per la creazione di pool di indirizzi IP statici. In breve, è necessario disporre di una sola rete route, un gateway e un server DNS, ed è necessario specificare l'indirizzo IP.

![Riepilogo della creazione di pool IP statica](../media/Guarded-Fabric-Shielded-VM/guarded-host-unattend-static-ip-address-pool-summary.png)

È necessario configurare la scheda di rete per la macchina virtuale. Lo screenshot seguente mostra dove impostare che la configurazione e come passa all'indirizzo IP statico.

![Configurare l'hardware per l'uso di IP statico](../media/Guarded-Fabric-Shielded-VM/guarded-host-unattend-static-ip-address-pool-network-adapter-settings.png)

È quindi possibile usare il `-StaticIPPool` parametro per includere gli elementi IP statici nel file di risposte. I parametri `@IPAddr-1@`, `@NextHop-1-1@`, e `@DNSAddr-1-1@` nella risposta alla domanda file verrà sostituiti con i valori reali in Virtual Machine Manager specificato al momento della distribuzione.

```powershell
$adminCred = Get-Credential -Message "Local administrator account"

New-ShieldingDataAnswerFile -Path '.\ShieldedVMAnswerFile.xml' -AdminCredentials $adminCred -StaticIPPool IPv4Address
```

## <a name="windows-answer-file-with-a-custom-locale"></a>File di risposte Windows con le impostazioni locali personalizzate

I comandi seguenti creano un file di risposte Windows con le impostazioni locali personalizzate.

Quando viene richiesto di immettere credenziali di amministratore, specificare il nome utente desiderato e la password.
Utilizzare "Administrator" per il nome utente se si vuole configurare l'account Administrator predefinito.

```powershell
$adminCred = Get-Credential -Message "Local administrator account"
$domainCred = Get-Credential -Message "Domain join credentials"

New-ShieldingDataAnswerFile -Path '.\ShieldedVMAnswerFile.xml' -AdminCredentials $adminCred -Locale es-ES
```

## <a name="basic-linux-answer-file"></a>File di risposte di Linux base

A partire da Windows Server versione 1709, è possibile eseguire determinati sistemi operativi guest Linux in macchine virtuali schermate.
Se si usa l'agente Linux di System Center Virtual Machine Manager per specializzare tali macchine virtuali, il cmdlet New-ShieldingDataAnswerFile può creare i file di risposte compatibile per tale.

In un file di risposte di Linux, è in genere includerà la password radice, chiave SSH radice e informazioni sul pool di IP statici facoltativamente.
Sostituire il percorso per la parte pubblica della chiave SSH prima di eseguire lo script seguente.

```powershell
$rootPassword = Read-Host -Prompt "Root password" -AsSecureString

New-ShieldingDataAnswerFile -Path '.\ShieldedVMAnswerFile.xml' -RootPassword $rootPassword -RootSshKey '~\.ssh\id_rsa.pub'
```

## <a name="see-also"></a>Vedere anche

- [Distribuire macchine virtuali schermate](guarded-fabric-configuration-scenarios-for-shielded-vms-overview.md)
- [Infrastruttura sorvegliata e macchine virtuali schermate](guarded-fabric-and-shielded-vms-top-node.md)
