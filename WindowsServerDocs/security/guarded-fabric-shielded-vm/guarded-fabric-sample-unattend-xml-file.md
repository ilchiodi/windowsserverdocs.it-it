---
title: Creare file di risposte di specializzazione del sistema operativo
ms.prod: windows-server
ms.topic: article
ms.assetid: 299aa38e-28d2-4cbe-af16-5b8c533eba1f
manager: dongill
author: rpsqrd
ms.author: ryanpu
ms.technology: security-guarded-fabric
ms.date: 08/29/2018
ms.openlocfilehash: be099a234b7e2e73375d23b19161e59876f71d61
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80856504"
---
# <a name="create-os-specialization-answer-file"></a>Creare file di risposte di specializzazione del sistema operativo

>Si applica a: Windows Server 2019, Windows Server (canale semestrale), Windows Server 2016

Per preparare la distribuzione di VM schermate, potrebbe essere necessario creare un file di risposte di specializzazione del sistema operativo. In Windows questo è comunemente noto come file Unattend. XML. La funzione **New-ShieldingDataAnswerFile di** Windows PowerShell consente di eseguire questa operazione. È quindi possibile usare il file di risposte quando si creano macchine virtuali schermate da un modello usando System Center Virtual Machine Manager (o qualsiasi altro controller di infrastruttura).

Per linee guida generali per i file di installazione automatica per le macchine virtuali schermate, vedere [creare un file di risposte](guarded-fabric-tenant-creates-shielding-data.md#create-an-answer-file).
 
## <a name="downloading-the-new-shieldingdataanswerfile-function"></a>Download della funzione New-ShieldingDataAnswerFile

È possibile ottenere la funzione **New-ShieldingDataAnswerFile** dalla [PowerShell Gallery](https://aka.ms/gftools). Se il computer dispone di connettività Internet, è possibile installarlo da PowerShell con il comando seguente:

```powershell
Install-Module GuardedFabricTools -Repository PSGallery -MinimumVersion 1.0.0
```

L'output del `unattend.xml` può essere incluso nei dati di schermatura, insieme ad altri artefatti, in modo che possa essere usato per creare VM schermate dai modelli.

Le sezioni seguenti illustrano come è possibile usare i parametri di funzione per un file di `unattend.xml` contenente diverse opzioni:

- [File di risposte di base di Windows](#basic-windows-answer-file)
- [File di risposte di Windows con aggiunta a un dominio](#windows-answer-file-with-domain-join)
- [File di risposte di Windows con indirizzi IPv4 statici](#windows-answer-file-with-static-ipv4-addresses)
- [File di risposte di Windows con impostazioni locali personalizzate](#windows-answer-file-with-a-custom-locale)
- [File di risposte di base di Linux](#basic-linux-answer-file)

## <a name="basic-windows-answer-file"></a>File di risposte di base di Windows

I comandi seguenti creano un file di risposte di Windows che imposta semplicemente la password dell'account amministratore e il nome host.
Le schede di rete della macchina virtuale utilizzeranno DHCP per ottenere gli indirizzi IP e la macchina virtuale non verrà aggiunta a un dominio Active Directory.
Quando viene richiesto di immettere le credenziali di amministratore, specificare il nome utente e la password desiderati.
Usare "Administrator" per il nome utente se si vuole configurare l'account Administrator predefinito.

```powershell
$adminCred = Get-Credential -Message "Local administrator account"

New-ShieldingDataAnswerFile -Path '.\ShieldedVMAnswerFile.xml' -AdminCredentials $adminCred
```

## <a name="windows-answer-file-with-domain-join"></a>File di risposte di Windows con aggiunta a un dominio

I comandi seguenti consentono di creare un file di risposte di Windows che unisce la macchina virtuale schermata a un dominio Active Directory.
Le schede di rete della macchina virtuale utilizzeranno DHCP per ottenere gli indirizzi IP.

La prima richiesta di credenziale richiederà le informazioni sull'account amministratore locale.
Usare "Administrator" per il nome utente se si vuole configurare l'account Administrator predefinito.

La seconda richiesta di credenziali richiederà le credenziali che hanno il diritto di aggiungere il computer al dominio Active Directory.

Assicurarsi di modificare il valore del parametro "-DomainName" con il nome FQDN del dominio Active Directory.

```powershell
$adminCred = Get-Credential -Message "Local administrator account"
$domainCred = Get-Credential -Message "Domain join credentials"

New-ShieldingDataAnswerFile -Path '.\ShieldedVMAnswerFile.xml' -AdminCredentials $adminCred -DomainName 'my.contoso.com' -DomainJoinCredentials $domainCred
```
## <a name="windows-answer-file-with-static-ipv4-addresses"></a>File di risposte di Windows con indirizzi IPv4 statici

I comandi seguenti creano un file di risposte di Windows che usa indirizzi IP statici forniti in fase di distribuzione dal gestore dell'infrastruttura, ad esempio System Center Virtual Machine Manager.

Virtual Machine Manager fornisce tre componenti all'indirizzo IP statico tramite un pool IP: indirizzo IPv4, indirizzo IPv6, indirizzo gateway e indirizzo DNS. Per includere eventuali campi aggiuntivi o richiedere una configurazione di rete personalizzata, sarà necessario modificare manualmente il file di risposte prodotto dallo script.

Gli screenshot seguenti mostrano i pool IP che è possibile configurare in Virtual Machine Manager. Questi pool sono necessari se si vuole usare un indirizzo IP statico.

Attualmente, la funzione supporta un solo server DNS. Di seguito sono riportate le impostazioni DNS:

![Configurazione del server DNS con il pool di indirizzi IP statici](../media/Guarded-Fabric-Shielded-VM/guarded-host-unattend-static-ip-address-pool-dns-settings.png)

Di seguito è riportato il riepilogo per la creazione del pool di indirizzi IP statici. In breve, è necessario avere solo una route di rete, un gateway e un server DNS e specificare l'indirizzo IP.

![Riepilogo della creazione di pool di indirizzi IP statici](../media/Guarded-Fabric-Shielded-VM/guarded-host-unattend-static-ip-address-pool-summary.png)

È necessario configurare la scheda di rete per la macchina virtuale. Lo screenshot seguente mostra dove impostare la configurazione e come passarla a un indirizzo IP statico.

![Configurare l'hardware per l'uso dell'indirizzo IP statico](../media/Guarded-Fabric-Shielded-VM/guarded-host-unattend-static-ip-address-pool-network-adapter-settings.png)

Quindi, è possibile usare il parametro `-StaticIPPool` per includere gli elementi IP statici nel file di risposta. I parametri `@IPAddr-1@`, `@NextHop-1-1@`e `@DNSAddr-1-1@` nel file di risposte verranno sostituiti con i valori reali specificati in Virtual Machine Manager in fase di distribuzione.

```powershell
$adminCred = Get-Credential -Message "Local administrator account"

New-ShieldingDataAnswerFile -Path '.\ShieldedVMAnswerFile.xml' -AdminCredentials $adminCred -StaticIPPool IPv4Address
```

## <a name="windows-answer-file-with-a-custom-locale"></a>File di risposte di Windows con impostazioni locali personalizzate

I comandi seguenti consentono di creare un file di risposte di Windows con impostazioni locali personalizzate.

Quando viene richiesto di immettere le credenziali di amministratore, specificare il nome utente e la password desiderati.
Usare "Administrator" per il nome utente se si vuole configurare l'account Administrator predefinito.

```powershell
$adminCred = Get-Credential -Message "Local administrator account"
$domainCred = Get-Credential -Message "Domain join credentials"

New-ShieldingDataAnswerFile -Path '.\ShieldedVMAnswerFile.xml' -AdminCredentials $adminCred -Locale es-ES
```

## <a name="basic-linux-answer-file"></a>File di risposte di base di Linux

A partire da Windows Server versione 1709, è possibile eseguire determinati sistemi operativi guest Linux in VM schermate.
Se si usa l'agente di System Center Virtual Machine Manager Linux per specializzare tali macchine virtuali, il cmdlet New-ShieldingDataAnswerFile può creare file di risposte compatibili.

In un file di risposte Linux vengono in genere incluse la password radice, la chiave SSH radice e le informazioni facoltative sul pool di indirizzi IP statici.
Sostituire il percorso alla metà pubblica della chiave SSH prima di eseguire lo script seguente.

```powershell
$rootPassword = Read-Host -Prompt "Root password" -AsSecureString

New-ShieldingDataAnswerFile -Path '.\ShieldedVMAnswerFile.xml' -RootPassword $rootPassword -RootSshKey '~\.ssh\id_rsa.pub'
```

## <a name="see-also"></a>Vedere anche

- [Distribuire macchine virtuali schermate](guarded-fabric-configuration-scenarios-for-shielded-vms-overview.md)
- [Infrastruttura sorvegliata e macchine virtuali schermate](guarded-fabric-and-shielded-vms-top-node.md)
