---
title: Gestire i Server Core
description: Informazioni su come gestire un'installazione Server Core di Windows Server
ms.prod: windows-server-threshold
ms.mktglfcycl: manage
ms.sitesec: library
author: lizap
ms.localizationpriority: medium
ms.date: 10/17/2017
ms.openlocfilehash: 6836e5db36727294d215f7f98e0faeede55a612a
ms.sourcegitcommit: 1533d994a6ddea54ac189ceb316b7d3c074307db
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 06/01/2018
ms.locfileid: "1718711"
---
# <a name="manage-a-server-core-server"></a>Gestire un server a Server Core
 
> Si applica a: Windows Server (Channel semi-annuale) e Windows Server 2016

È possibile gestire un server a Server Core nei modi seguenti:
- Utilizzo di [Interfaccia di amministrazione di Windows](../../manage/windows-admin-center/overview.md)
- Utilizzo di [Strumenti di amministrazione remota del Server](../../remote/remote-server-administration-tools.md) in esecuzione su Windows 10
- Modalità locale e remota tramite Windows PowerShell
- In modalità remota utilizzando [Server Manager](../server-manager/server-manager.md)
- In modalità remota utilizzando uno [snap-in MMC](#managing-with-microsoft-management-console)
- In remoto con [Servizi Desktop remoto](#managing-with-remote-desktop-services)

È inoltre possibile aggiungere hardware e gestire i driver localmente, purché tale scopo dalla riga di comando.

Esistono alcune limitazioni importanti e suggerimenti da tenere presenti quando si utilizzano i Server principali:

- Se si chiude tutte le finestre del prompt dei comandi e si desidera aprire una nuova finestra del Prompt, è possibile farlo da Task Manager. Premere **CTRL\ + ALT\ + CANC**, fare clic su **Avvia Gestione attività**, fare clic su **ulteriori dettagli > File > eseguire**, quindi digitare **cmd.exe**. (Digitare **Powershell.exe** per aprire windows PowerShell comando). In alternativa, è possibile disconnettersi e quindi accedere nuovamente.
- Qualsiasi comando o uno strumento che tenta di avviare Esplora risorse non funzionerà. Ad esempio, che esegue **avviare.** dal prompt dei comandi non funzionano.
- Non esiste alcun supporto per il rendering HTML o della Guida HTML in Server Core.
- Server Core supporta Windows Installer in modalità non interattiva in modo che è possibile installare gli strumenti e utilità dal file di Windows Installer. Durante l'installazione di pacchetti di Windows Installer su Server Core, utilizzare l'opzione **/qb** per visualizzare l'interfaccia utente di base.
- Per modificare il fuso orario, eseguire **Set-data**.
- Per modificare le impostazioni internazionali, eseguire il **controllo intl. cpl**.
- **Control.exe** non verrà eseguito in modo autonomo. È necessario eseguirlo con **timedate. cpl** o **intl. cpl**.
- **Winver.exe** non è disponibile in Server Core. Per ottenere informazioni sulla versione di utilizzare **Systeminfo.exe**.

## <a name="managing-server-core-with-windows-admin-center"></a>Gestione dei Server Core con l'interfaccia di amministrazione di Windows
[Interfaccia di amministrazione di Windows](../../manage/windows-admin-center/overview.md) è un'applicazione di gestione basata su browser che consente l'amministrazione in locale di Windows Server senza dipendenze Azure o cloud. Interfaccia di amministrazione di Windows offre controllo completo su tutti gli aspetti dell'infrastruttura server ed è particolarmente utile per la gestione delle reti private non connessi a Internet. È possibile installare Windows Admin Center in Windows 10, in un server gateway o in un'installazione di Windows Server con esperienza Desktop e quindi connettersi al sistema Server Core che si desidera gestire.

## <a name="managing-server-core-remotely-with-server-manager"></a>Gestione dei Server Core in remoto con Server Manager

Server Manager è una console di gestione in Windows Server che consente di eseguire il provisioning e gestire i server locali e remote basate su Windows dal desktop, senza richiedere l'accesso ai server fisico o la necessità di abilitare Remote Desktop protocol (RDP) connessioni a ogni server. Gestione server supporta la gestione remota, con più server.

Per abilitare il server locale essere gestiti da Gestione Server in esecuzione su un server remoto, eseguire il cmdlet di Windows PowerShell **Configura SMRemoting.exe – abilitare**.

## <a name="managing-with-microsoft-management-console"></a>Gestione di Microsoft Management Console

È possibile utilizzare in modalità remota molti snap-in di Microsoft Management Console (MMC) per gestire i server a Server Core.

Per utilizzare lo snap-in MMC per gestire un server a Server Core è un membro del dominio: 

1. Avviare lo snap-in MMC, ad esempio Gestione Computer.
2. Pulsante destro del mouse lo snap-in e fare clic su **Connetti a un altro computer**.
2. Digitare il nome del computer del server Server Core e quindi fare clic su **OK**. È ora possibile utilizzare lo snap-in MMC per gestire i server a Server Core come si farebbe qualsiasi altro PC o server.

Utilizzare lo snap-in MMC per gestire un server a Server Core vale a dire *non* appartenente al dominio: 

1. Definire le credenziali alternative da utilizzare per connettersi al computer Server Core digitando il comando seguente al prompt dei comandi sul computer remoto:
   ```
   cmdkey /add:<ServerName> /user:<UserName> /pass:<password>
   ```
   Se si desidera venga richiesta una password, è possibile omettere **l'opzione/passare** .

2. Quando richiesto, digitare la password per il nome utente specificato.
   Se il firewall nel server Server Core non è già configurato per consentire lo snap-in MMC per la connessione, eseguire la procedura seguente per configurare Windows Firewall per consentire lo snap-in MMC. Quindi procedere al passaggio 3.
3. In un computer differente, avviare lo snap-in MMC, ad esempio **Gestione Computer**.
4. Nel riquadro sinistro, destro dello snap-in e fare clic su **Connetti a un altro computer**. (Ad esempio, nell'esempio di gestione Computer si sarebbe destro **Gestione Computer (locale)**.)
5. In **un altro computer**, digitare il nome del computer del server Server Core e quindi fare clic su **OK**. È ora possibile utilizzare lo snap-in MMC per gestire i server a Server Core seguendo la procedura di qualsiasi altro computer che eseguono un sistema operativo Windows Server.

### <a name="to-configure-windows-firewall-to-allow-mmc-snap-ins-to-connect"></a>Per configurare Windows Firewall per consentire lo snap-in MMC (s) per la connessione
Per consentire a tutti gli snap-in MMC per la connessione, eseguire il comando seguente:

```
Enable-NetFirewallRule -DisplayGroup "Remote Administration"
```

Per consentire solo specifico snap-in MMC per la connessione, eseguire le operazioni seguenti:
```
Enable-NetFirewallRule -DisplayGroup "<rulegroup>"
```

Dove *rulegroup* è uno dei seguenti, a seconda di quale snap-in si desidera connettere:

| Snap-in MMC                            | Gruppo di regole                                            |
|----------------------------------------|-------------------------------------------------------|
| Visualizzatore eventi                           | Gestione remota registro eventi                           |
| Servizi                               | Gestione assistenza remota                             |
| Cartelle condivise                         | Condivisione file e stampanti                              |
| Utilità di pianificazione                         | Registri di prestazioni e avvisi, condivisione File e stampanti |
| Gestione disco                        | Gestione volumi remota                              |
| Windows Firewall e la protezione avanzata | Gestione remota Windows Firewall                    |


> [!NOTE] 
> Alcuni snap-in MMC non dispongono di un gruppo di regole corrispondenti che consente di connettersi attraverso il firewall. Tuttavia, abilitare i gruppi di regole per il Visualizzatore eventi, servizi o le cartelle condivise consentirà maggior parte degli altri snap-in per la connessione. 
>
> Inoltre, determinati snap-in richiedono un'ulteriore configurazione prima che possono essere connessi tramite Windows Firewall:
>
> - Gestione disco. È innanzitutto necessario avviare il servizio di dischi virtuali (VDS) nel computer Server Core. È inoltre necessario configurare le regole di Gestione disco in modo appropriato nel computer che esegue lo snap-in MMC.
> - Monitor di protezione IP. Innanzitutto è necessario abilitare la gestione remota dello snap-in. A tale scopo, al prompt dei comandi, **digitare/IM \windows\system32\scregedit.wsf Cscript 1**
> - Prestazioni e affidabilità. Lo snap-in non richiede ogni ulteriore configurazione, ma se utilizzato per monitorare un computer Server Core, è possibile monitorare solo i dati relativi alle prestazioni. Affidabilità dati non sono disponibili.

## <a name="managing-with-remote-desktop-services"></a>Gestione con Servizi Desktop remoto

È possibile utilizzare [Desktop remoto](../../remote/remote-desktop-services/welcome-to-rds.md) per gestire un server a Server Core da più computer.

Prima di poter accedere a Server Core, sarà necessario eseguire il comando seguente: 
```
cscript C:\Windows\System32\Scregedit.wsf /ar 0
```
In questo modo il Desktop remoto per la modalità di amministrazione accettare connessioni.

## <a name="add-hardware-and-manage-drivers-locally"></a>Aggiungere componenti hardware e gestire i driver localmente

Per aggiungere un server a Server Core hardware, seguire le istruzioni fornite dal produttore dell'hardware per l'installazione di nuovo hardware. 

Se i componenti hardware non è plug and play, sarà necessario installare manualmente il driver. A tale scopo, copiare i file dei driver in un percorso temporaneo nel server e quindi eseguire il comando seguente:
```
pnputil –i –a <driverinf>
```
Dove *driverinf* è il nome del file inf per il driver.

Se richiesto, riavviare il computer.

Per visualizzare i driver installati, eseguire il comando seguente: 
```
sc query type= driver
```

> [!NOTE] 
> È necessario includere lo spazio dopo il segno di uguale per il comando avrà esito positivo.

Per disabilitare un driver di periferica, eseguire le operazioni seguenti: 
```
sc delete <service_name>
```

Dove *nome_servizio* è il nome del servizio ottenuto durante l'esecuzione di **tipo di query sc = driver**.