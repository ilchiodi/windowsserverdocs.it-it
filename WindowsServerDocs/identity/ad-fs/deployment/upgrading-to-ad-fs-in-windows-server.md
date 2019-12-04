---
ms.assetid: 7671e0c9-faf0-40de-808a-62f54645f891
title: Aggiornamento ad AD FS in Windows Server 2016
description: ''
author: billmath
manager: femila
ms.date: 04/09/2018
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 428e35524fbcfe5177b544e1c6cc6fa32ec32056
ms.sourcegitcommit: 4a03f263952c993dfdf339dd3491c73719854aba
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/03/2019
ms.locfileid: "74791364"
---
# <a name="upgrading-to-ad-fs-in-windows-server-2016-using-a-wid-database"></a>Aggiornamento ad AD FS in Windows Server 2016 con un database interno di Windows


> [!NOTE]
> Avviare un aggiornamento solo con un intervallo di tempo definitivo pianificato per il completamento. Non è consigliabile mantenere AD FS in uno stato in modalità mista per un periodo di tempo prolungato, lasciando AD FS in uno stato in modalità mista può causare problemi con la farm.

## <a name="upgrading-a-windows-server-2012-r2-or-2016-ad-fs-farm-to-windows-server-2019"></a>Aggiornamento di una farm di Windows Server 2012 R2 o 2016 AD FS a Windows Server 2019
Nel documento seguente viene descritto come aggiornare la farm AD FS per AD FS in Windows Server 2019 quando si utilizza un database WID.

### <a name="ad-fs-farm-behavior-levels-fbl"></a>Livelli di comportamento della farm AD FS (FBI)
In AD FS per Windows Server 2016 è stato introdotto il livello di comportamento della farm (FBI). Si tratta di un'impostazione a livello di farm che determina le funzionalità che possono essere utilizzate dalla farm AD FS.

La tabella seguente elenca i valori dell'FBI in base alla versione di Windows Server:

| Versione di Windows Server  | FBL | Nome del database di configurazione AD FS |
| ------------- | ------------- | ------------- |
| 2012 R2  | 1  | AdfsConfiguration |
| 2016  | 3  | AdfsConfigurationV3 |
| 2019  | 4  | AdfsConfigurationV4 |

> [!NOTE]
> Con l'aggiornamento dell'FBI viene creato un nuovo database di configurazione AD FS.  Vedere la tabella precedente per i nomi del database di configurazione per ogni versione di Windows Server AD FS e il valore FBL

### <a name="new-vs-upgraded-farms"></a>Nuove farm con aggiornamento di Visual Studio
Per impostazione predefinita, l'FBI in una nuova farm AD FS corrisponde al valore per la versione di Windows Server del primo nodo della farm installato.

Un server AD FS di una versione successiva può essere aggiunto a una farm AD FS 2012 R2 o 2016 e la farm opererà nello stesso FBI dei nodi esistenti. Quando si dispone di più versioni di Windows Server che operano nella stessa farm del valore dell'FBI della versione più bassa, la farm è detta "mixed". Tuttavia, non sarà possibile sfruttare le funzionalità delle versioni successive fino a quando non viene generato il FBI. Con una farm mista:

- Gli amministratori possono aggiungere nuovi server federativi di Windows Server 2019 a una farm di Windows Server 2012 R2 o 2016 esistente. Di conseguenza, la farm si trova in modalità mista e opera allo stesso livello di comportamento della farm originale. Per garantire un comportamento coerente nella farm, le funzionalità delle versioni più recenti di Windows Server AD FS non possono essere configurate o utilizzate.

- Prima che l'FBI possa essere generato, gli amministratori devono rimuovere i nodi AD FS delle versioni precedenti di Windows Server dalla farm.  Nel caso di una farm di database interno di Windows, si noti che questo richiede che uno dei nuovi server federativi di Windows Server 2019 venga innalzato al ruolo di nodo primario nella farm.

- Quando tutti i server federativi della farm si trovano nella stessa versione di Windows Server, è possibile generare l'FBI.  Di conseguenza, è possibile configurare e utilizzare tutte le nuove funzionalità di AD FS Windows Server 2019.

Tenere presente che, in modalità farm mista, la farm AD FS non è in grado di nuove funzionalità o funzionalità introdotte in AD FS in Windows Server 2019. Questo significa che le organizzazioni che vogliono provare le nuove funzionalità non finché non viene generato il FBL. Quindi, se l'organizzazione sta cercando di testare le nuove funzionalità prima di sollevare l'FBI, sarà necessario distribuire una farm separata a tale scopo.

Il resto del documento è la procedura per l'aggiunta di un server federativo di Windows Server 2019 a un ambiente Windows Server 2016 o 2012 R2 e quindi la generazione dell'FBI a Windows Server 2019. Questi passaggi sono stati eseguiti in un ambiente di test descritto nel diagramma dell'architettura riportato di seguito.

> [!NOTE]
> Prima di poter passare a AD FS in Windows Server 2019 FBI, è necessario rimuovere tutti i nodi Windows Server 2016 o 2012 R2. Non è possibile aggiornare solo un sistema operativo Windows Server 2016 o 2012 R2 a Windows Server 2019 e diventare un nodo 2019. Sarà necessario rimuoverlo e sostituirlo con un nuovo nodo 2019.

##### <a name="to-upgrade-your-ad-fs-farm-to-windows-server-2019-farm-behavior-level"></a>Per aggiornare la farm AD FS al livello di comportamento della farm di Windows Server 2019

1. Con Server Manager, installare il ruolo Active Directory Federation Services in Windows Server 2019

2. Utilizzando la configurazione guidata AD FS, aggiungere il nuovo server Windows Server 2019 alla farm AD FS esistente.

![aggiornare](media/Upgrading-to-AD-FS-in-Windows-Server-2016/ADFS_Mixed_1.png)

3. Nel server federativo di Windows Server 2019 aprire Gestione AD FS. Si noti che le funzionalità di gestione non sono disponibili perché il server federativo non è il server primario.

![aggiornare](media/Upgrading-to-AD-FS-in-Windows-Server-2016/ADFS_Mixed_3.png)

4. Nel server Windows Server 2019 aprire una finestra di comando di PowerShell con privilegi elevati ed eseguire il cmdlet seguente:

```PowerShell
Set-AdfsSyncProperties -Role PrimaryComputer
```

![aggiornare](media/Upgrading-to-AD-FS-in-Windows-Server-2016/ADFS_Mixed_4.png)

5. Nel server AD FS precedentemente configurato come primario, aprire una finestra di comando di PowerShell con privilegi elevati ed eseguire il cmdlet seguente:

```PowerShell
Set-AdfsSyncProperties -Role SecondaryComputer -PrimaryComputerName {FQDN}
```

![aggiornare](media/Upgrading-to-AD-FS-in-Windows-Server-2016/ADFS_Mixed_5.png)

6. Ora sul server federativo a Windows Server 2016 aprire Gestione ADFS. Si noti che ora tutte le funzionalità di amministrazione vengono visualizzate perché il ruolo primario è stato trasferito a questo server.

![aggiornare](media/Upgrading-to-AD-FS-in-Windows-Server-2016/ADFS_Mixed_6.png)

7. Se si esegue l'aggiornamento di una farm AD FS 2012 R2 a 2016 o 2019, per l'aggiornamento della farm è necessario che lo schema di Active Directory sia almeno di livello 85.  Per aggiornare lo schema, con il supporto di installazione di Windows Server 2016, aprire un prompt dei comandi e passare alla directory support\adprep Eseguire il comando seguente: `adprep /forestprep`

![aggiornare](media/Upgrading-to-AD-FS-in-Windows-Server-2016/ADFS_Mixed_7.png)

Una volta completata l'esecuzione `adprep/domainprep`

> [!NOTE]
> Prima di eseguire il passaggio successivo, verificare che Windows Server sia aggiornato eseguendo Windows Update da impostazioni. Continua questo processo fino a quando non sono necessari altri aggiornamenti.

![aggiornare](media/Upgrading-to-AD-FS-in-Windows-Server-2016/ADFS_Mixed_8.png)

8. A questo punto, nel server Windows Server 2016 aprire PowerShell ed eseguire il cmdlet seguente:

> [!NOTE]
> Tutti i Server 2012 R2 devono essere rimossi dalla farm prima di eseguire il passaggio successivo.

```PowerShell
Invoke-AdfsFarmBehaviorLevelRaise
```

![aggiornare](media/Upgrading-to-AD-FS-in-Windows-Server-2016/ADFS_Mixed_9.png)

9. Quando richiesto, digitare Y. Verrà avviata la generazione del livello. Questo termine si sono generati correttamente il FBL.

![aggiornare](media/Upgrading-to-AD-FS-in-Windows-Server-2016/ADFS_Mixed_10.png)

10. A questo punto, se si passa alla gestione AD FS, si noterà che sono state aggiunte nuove funzionalità per la versione successiva del AD FS

![aggiornare](media/Upgrading-to-AD-FS-in-Windows-Server-2016/ADFS_Mixed_12.png)

11. Analogamente, è possibile usare il cmdlet di PowerShell: `Get-AdfsFarmInformation` per visualizzare l'FBI corrente.

![aggiornare](media/Upgrading-to-AD-FS-in-Windows-Server-2016/ADFS_Mixed_13.png)

12. Per aggiornare i server WAP al livello più recente, in ogni proxy dell'applicazione Web riconfigurare il WAP eseguendo il cmdlet di PowerShell seguente in una finestra con privilegi elevati:

```PowerShell
$trustcred = Get-Credential -Message "Enter Domain Administrator credentials"
Install-WebApplicationProxy -CertificateThumbprint {SSLCert} -fsname fsname -FederationServiceTrustCredential $trustcred
```

Rimuovere i server obsoleti dal cluster e conservare solo i server WAP che eseguono la versione più recente del server, che sono stati riconfigurati in precedenza, eseguendo il cmdlet di PowerShell seguente.

```PowerShell
Set-WebApplicationProxyConfiguration -ConnectedServersName WAPServerName1, WAPServerName2
```

Controllare la configurazione WAP eseguendo il cmdlet Get-WebApplicationProxyConfiguration. Il ConnectedServersName rifletterà l'esecuzione del server dal comando precedente.

```PowerShell
Get-WebApplicationProxyConfiguration
```
Per aggiornare il ConfigurationVersion dei server WAP, eseguire il comando di PowerShell seguente.

```PowerShell
Set-WebApplicationProxyConfiguration -UpgradeConfigurationVersion
```

Verrà completato l'aggiornamento dei server WAP.
