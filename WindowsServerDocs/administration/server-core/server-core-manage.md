---
title: Gestire Server Core
description: Informazioni su come gestire un'installazione Server Core di Windows Server
ms.prod: windows-server
ms.mktglfcycl: manage
ms.sitesec: library
author: lizap
ms.localizationpriority: medium
ms.date: 07/23/2019
ms.openlocfilehash: bd96dbfc93f3999d8fb3ddf7ec94cc11025bba30
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71383403"
---
# <a name="manage-a-server-core-server"></a>Gestire un server Server Core
 
> Si applica a: Windows Server 2019, Windows Server 2016 e Windows Server (canale semestrale)

È possibile gestire un server Server Core nei modi seguenti:
- Uso dell'interfaccia di [amministrazione di Windows](../../manage/windows-admin-center/overview.md)
- Uso di [strumenti di amministrazione remota del server](../../remote/remote-server-administration-tools.md) in esecuzione in Windows 10
- In locale e in remoto mediante Windows PowerShell
- Uso remoto di [Server Manager](../server-manager/server-manager.md)
- In modalità remota tramite uno [snap-in MMC](#managing-with-microsoft-management-console)
- In remoto con [Servizi Desktop remoto](#managing-with-remote-desktop-services)

È anche possibile aggiungere hardware e gestire i driver localmente, purché si esegua questa operazione dalla riga di comando.

Quando si lavora con Server Core, è necessario tenere presenti alcune limitazioni e suggerimenti importanti:

- Se si chiudono tutte le finestre del prompt dei comandi e si desidera aprire una nuova finestra del prompt dei comandi, è possibile eseguire questa operazione da Gestione attività. Premere **CTRL\+ALT\+Elimina**, fare clic su **Avvia Gestione attività**, fare clic su **altri dettagli > File > Esegui**, quindi digitare **cmd. exe**. Digitare **PowerShell. exe** per aprire una finestra di comando di PowerShell. In alternativa, è possibile disconnettersi e quindi eseguire di nuovo l'accesso.
- Qualsiasi comando o strumento tenti di avviare Esplora risorse non funzionerà. Ad esempio, l'esecuzione di **Start.** da un prompt dei comandi non funzionerà.
- Non è disponibile alcun supporto per il rendering HTML o la Guida HTML in Server Core.
- Server Core supporta Windows Installer in modalità non interattiva, in modo che sia possibile installare strumenti e utilità da file di Windows Installer. Quando si installano pacchetti di Windows Installer in Server Core, usare l'opzione **/qb** per visualizzare l'interfaccia utente di base.
- Per modificare il fuso orario, eseguire **set-date**.
- Per modificare le impostazioni internazionali, eseguire **Control Intl. cpl**.
- **Control. exe** non verrà eseguito autonomamente. È necessario eseguirlo con **timet. cpl** o **Intl. cpl**.
- **WINVER. exe** non è disponibile in Server Core. Per ottenere informazioni sulla versione, utilizzare **systeminfo. exe**.

## <a name="managing-server-core-with-windows-admin-center"></a>Gestione di Server Core con l'interfaccia di amministrazione di Windows
[Windows Admin Center](../../manage/windows-admin-center/overview.md) è un'applicazione di gestione basata su browser che consente l'amministrazione in locale di Windows Server senza dipendenze Azure o cloud. Windows Admin Center offre il controllo completo su tutti gli aspetti dell'infrastruttura server ed è particolarmente utile per la gestione delle reti private non connesse a Internet. È possibile installare centro di amministrazione di Windows in Windows 10, in un server gateway o in un'installazione di Windows Server con esperienza desktop, quindi connettersi al sistema Server Core che si desidera gestire.

## <a name="managing-server-core-remotely-with-server-manager"></a>Gestione remota di Server Core con Server Manager

Server Manager è una console di gestione di Windows Server che consente di effettuare il provisioning e gestire server Windows locali e remoti dai propri desktop, senza richiedere l'accesso fisico ai server o la necessità di abilitare il protocollo RDP (Desktop remoto Protocol) connessioni a ogni server. Server Manager supporta la gestione remota multiserver.

Per consentire la gestione del server locale con Server Manager in esecuzione in un server remoto, eseguire il cmdlet di Windows PowerShell **Configure-SMRemoting. exe – enable**.

## <a name="managing-with-microsoft-management-console"></a>Gestione con Microsoft Management Console

È possibile utilizzare molti snap-in per Microsoft Management Console (MMC) in remoto per gestire il server Server Core.

Per utilizzare uno snap-in MMC per gestire un server Server Core che è un membro di dominio: 

1. Avviare uno snap-in MMC, ad esempio Gestione computer.
2. Fare clic con il pulsante destro del mouse sullo snap-in, quindi scegliere **Connetti a un altro computer**.
2. Digitare il nome del computer server core, quindi fare clic su **OK**. È ora possibile utilizzare lo snap-in MMC per gestire il server Server Core come per qualsiasi altro PC o server.

Per utilizzare uno snap-in MMC per gestire un server Server Core che *non* è un membro di dominio: 

1. Definire credenziali alternative da usare per connettersi al computer Server Core digitando il comando seguente al prompt dei comandi nel computer remoto:

   ```
   cmdkey /add:<ServerName> /user:<UserName> /pass:<password>
   ```

   Se si desidera che venga richiesta una password, omettere l'opzione **/pass** .

2. Quando richiesto, digitare la password per il nome utente specificato.
   Se il firewall nel server Server Core non è già configurato per consentire la connessione degli snap-in MMC, attenersi alla procedura seguente per configurare Windows Firewall per consentire lo snap-in MMC. Continuare con il passaggio 3.
3. In un altro computer avviare uno snap-in MMC, ad esempio **Gestione computer**.
4. Nel riquadro sinistro fare clic con il pulsante destro del mouse sullo snap-in, quindi scegliere **Connetti a un altro computer**. Ad esempio, nell'esempio di gestione computer, fare clic con il pulsante destro del mouse su **Gestione computer (locale)** .
5. In **un altro computer**, digitare il nome del computer server core, quindi fare clic su **OK**. È ora possibile utilizzare lo snap-in MMC per gestire il server in modalità Server Core come si farebbe con qualsiasi altro computer dotato di sistema operativo Windows Server.

### <a name="to-configure-windows-firewall-to-allow-mmc-snap-ins-to-connect"></a>Per configurare Windows Firewall per consentire la connessione degli snap-in MMC
Per consentire la connessione di tutti gli snap-in MMC, eseguire il comando seguente:

```PowerShell
Enable-NetFirewallRule -DisplayGroup "Remote Administration"
```

Per consentire la connessione solo di specifici snap-in MMC, eseguire le operazioni seguenti:

```PowerShell
Enable-NetFirewallRule -DisplayGroup "<rulegroup>"
```

Dove *RuleGroup* è uno dei seguenti, a seconda dello snap-in che si desidera connettere:

| Snap-in MMC                            | Gruppo di regole                                            |
| ---------------------------------------- | ------------------------------------------------------- |
| Visualizzatore eventi                           | Gestione remota registro eventi                           |
| Servizi                               | Gestione remota servizi                             |
| Cartelle condivise                         | Condivisione file e stampanti                              |
| Utilità di pianificazione                         | Condivisione di file e stampanti Avvisi e registri di prestazioni |
| Gestione disco                        | Gestione volumi remota                              |
| Windows Firewall e sicurezza avanzata | Gestione remota firewall Windows                    |


> [!NOTE] 
> Alcuni snap-in MMC non dispongono di un gruppo di regole corrispondente che consenta loro di connettersi attraverso il firewall. Tuttavia, l'attivazione dei gruppi di regole relativi a Visualizzatore eventi, Servizi o Cartelle condivise consente la connessione della maggior parte degli altri snap-in. 
>
> Inoltre, alcuni snap-in richiedono ulteriori configurazioni per potersi connettere attraverso Windows Firewall:
>
> - Gestione disco. È necessario prima di tutto avviare Servizio dischi virtuali nel computer in modalità Server Core. È inoltre necessario configurare correttamente le regole di Gestione disco nel computer che esegue lo snap-in MMC.
> - Monitor di sicurezza IP. È necessario prima di tutto attivare la gestione remota di questo snap-in. A tale scopo, al prompt dei comandi digitare **cscript \windows\system32\scregedit.wsf/im 1**
> - Monitoraggio affidabilità e prestazioni. Lo snap-in non richiede altre configurazioni, ma quando lo si usa per monitorare un computer in modalità Server Core, è possibile monitorare solo i dati sulle prestazioni. I dati sull'affidabilità non sono infatti disponibili.

## <a name="managing-with-remote-desktop-services"></a>Gestione con Servizi Desktop remoto

È possibile utilizzare [Desktop remoto](../../remote/remote-desktop-services/welcome-to-rds.md) per gestire un server Server Core da computer remoti.

Prima di poter accedere a Server Core, è necessario eseguire il comando seguente: 

```
cscript C:\Windows\System32\Scregedit.wsf /ar 0
```

Si consentirà così alla modalità Desktop remoto per amministrazione di accettare connessioni.

## <a name="add-hardware-and-manage-drivers-locally"></a>Aggiungere hardware e gestire i driver localmente

Per aggiungere hardware a un server Server Core, seguire le istruzioni fornite dal produttore dell'hardware per l'installazione di un nuovo hardware. 

Se l'hardware non è plug and Play, è necessario installare manualmente il driver. A tale scopo, copiare i file dei driver in un percorso temporaneo sul server, quindi eseguire il comando seguente:

```
pnputil –i –a <driverinf>
```

Dove *driverINF* è il nome del file. inf del driver.

Se richiesto, riavviare il computer.

Per visualizzare i driver installati, eseguire il comando seguente: 

```
sc query type= driver
```

> [!NOTE] 
> Perché il comando venga correttamente completato, è necessario includere uno spazio dopo il segno di uguale.

Per disabilitare un driver di dispositivo, eseguire il comando seguente:

```
sc delete <service_name>
```

Dove *SERVICE_NAME* è il nome del servizio ottenuto quando è stato eseguito **SC query type = driver**.