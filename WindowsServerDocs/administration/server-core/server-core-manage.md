---
title: Gestire Server Core
description: Informazioni su come gestire un'installazione Server Core di Windows Server
ms.prod: windows-server-threshold
ms.mktglfcycl: manage
ms.sitesec: library
author: lizap
ms.localizationpriority: medium
ms.date: 10/17/2017
ms.openlocfilehash: 6836e5db36727294d215f7f98e0faeede55a612a
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59869302"
---
# <a name="manage-a-server-core-server"></a>Gestire un Server server Core
 
> Si applica a: Windows Server (canale semestrale) e Windows Server 2016

È possibile gestire un Server server Core nei modi seguenti:
- Usando [Windows Admin Center](../../manage/windows-admin-center/overview.md)
- Usando [strumenti di amministrazione remota del Server](../../remote/remote-server-administration-tools.md) in esecuzione in Windows 10
- In locale e in remoto mediante Windows PowerShell
- In modalità remota utilizzando [Server Manager](../server-manager/server-manager.md)
- In modalità remota tramite un [snap-in MMC](#managing-with-microsoft-management-console)
- In modalità remota con [Servizi Desktop remoto](#managing-with-remote-desktop-services)

È anche possibile aggiungere hardware e gestire i driver in locale, purché si farlo dalla riga di comando.

Esistono alcuni importanti suggerimenti e limitazioni da tenere presenti quando si lavora con Server Core:

- Se si chiude tutte le finestre del prompt dei comandi e si desidera aprire una nuova finestra del prompt dei comandi, è possibile farlo dalla Gestione attività. Premere **CTRL\+ALT\+eliminare**, fare clic su **avvia Gestione attività**, fare clic su **più dettagli > File > eseguire**, quindi digitare  **cmd.exe**. (Tipo **Powershell.exe** per aprire una finestra di comando di PowerShell.) In alternativa, è possibile disconnettersi e accedere nuovamente.
- Qualsiasi comando o strumento tenti di avviare Esplora risorse non funzionerà. Ad esempio, l'esecuzione **start.** al prompt dei comandi non funzionerà.
- Non vi è alcun supporto per il rendering HTML o la Guida HTML in Server Core.
- Server Core supporta Windows Installer in modalità non interattiva in modo che è possibile installare gli strumenti e utilità dal file Windows Installer. Quando si installano pacchetti Windows Installer in Server Core, usare il **/qb** opzione per visualizzare l'interfaccia utente di base.
- Per modificare il fuso orario, eseguire **Set-Date**.
- Per modificare le impostazioni internazionali, eseguire **control intl. cpl**.
- **Control.exe** non verrà eseguito in modo automatico. È necessario eseguirlo con uno **timedate. cpl** oppure **intl**.
- **Winver.exe** non è disponibile in Server Core. Per ottenere informazioni di versione, utilizzare **Systeminfo.exe**.

## <a name="managing-server-core-with-windows-admin-center"></a>La gestione di Server Core con Windows Admin Center
[Windows Admin Center](../../manage/windows-admin-center/overview.md) è un'applicazione di gestione basata su browser che consente l'amministrazione in locale di Windows Server senza dipendenze Azure o cloud. Windows Admin Center offre il controllo completo su tutti gli aspetti dell'infrastruttura server ed è particolarmente utile per la gestione delle reti private non connesse a Internet. È possibile installare Windows Admin Center, in Windows 10, in un server gateway o in un'installazione di Windows Server con esperienza Desktop e quindi connettersi al sistema di base del Server che si desidera gestire.

## <a name="managing-server-core-remotely-with-server-manager"></a>La gestione di base del Server in modalità remota con Server Manager

Server Manager è una console di gestione in Windows Server che consente di eseguire il provisioning e gestire i server locali e remoti basati su Windows da desktop, senza richiedere l'accesso fisico ai server o la necessità di abilitare Desktop Remote protocol (RDP) connessioni a ogni server. Server Manager supporta la gestione remota multiserver.

Per abilitare il server locale da gestire con Server Manager in esecuzione in un server remoto, eseguire il cmdlet di Windows PowerShell **Configura SMRemoting.exe – abilitare**.

## <a name="managing-with-microsoft-management-console"></a>La gestione con Microsoft Management Console

È possibile usare molti snap-in per Microsoft Management Console (MMC) in modalità remota per gestire il server di Server Core.

Per utilizzare uno snap-in MMC per gestire un server Server Core che è membro del dominio: 

1. Avviare uno snap-in MMC, ad esempio Gestione Computer.
2. Fare doppio clic lo snap-in e quindi fare clic su **Connetti a un altro computer**.
2. Digitare il nome del computer del server Server Core e quindi fare clic su **OK**. È ora possibile usare lo snap-in MMC per gestire il server di Server Core, come si farebbe con qualsiasi altro PC o server.

Utilizzare uno snap-in MMC per gestire un server Server Core che è *non* un membro del dominio: 

1. Definire credenziali alternative da usare per connettersi al computer Server Core digitando il comando seguente al prompt dei comandi nel computer remoto:
   ```
   cmdkey /add:<ServerName> /user:<UserName> /pass:<password>
   ```
   Se si desidera venga richiesta una password, omettere il **/passare** opzione.

2. Quando richiesto, digitare la password per il nome utente specificato.
   Se il firewall del server Server Core non è già configurato per consentire lo snap-in MMC per la connessione, attenersi alla procedura seguente per configurare Windows Firewall per consentire lo snap-in di MMC. Procedere quindi al passaggio 3.
3. In un altro computer, avviare uno snap-in MMC, ad esempio **Gestione Computer**.
4. Nel riquadro sinistro, fare doppio clic sullo snap-in e quindi fare clic su **Connetti a un altro computer**. (Ad esempio, nell'esempio di gestione di Computer, è necessario fare doppio clic **Gestione Computer (locale)**.)
5. Nelle **un altro computer**, digitare il nome del computer del server Server Core e quindi fare clic su **OK**. È ora possibile utilizzare lo snap-in MMC per gestire il server in modalità Server Core come si farebbe con qualsiasi altro computer dotato di sistema operativo Windows Server.

### <a name="to-configure-windows-firewall-to-allow-mmc-snap-ins-to-connect"></a>Per configurare Windows Firewall per consentire la connessione degli snap-in MMC
Per consentire tutti gli snap-in MMC per la connessione, eseguire il comando seguente:

```
Enable-NetFirewallRule -DisplayGroup "Remote Administration"
```

Per consentire solo specifiche snap-in MMC per la connessione, eseguire le operazioni seguenti:
```
Enable-NetFirewallRule -DisplayGroup "<rulegroup>"
```

In cui *rulegroup* è uno dei seguenti, a seconda dello snap-in si desidera connettersi:

| Snap-in MMC                            | Gruppo di regole                                            |
|----------------------------------------|-------------------------------------------------------|
| Visualizzatore eventi                           | Gestione remota registro eventi                           |
| Servizi                               | Gestione remota servizi                             |
| Cartelle condivise                         | Condivisione file e stampanti                              |
| Utilità di pianificazione                         | Registri di prestazioni e avvisi, condivisione File e stampanti |
| Gestione disco                        | Gestione volumi remota                              |
| Windows Firewall e della sicurezza avanzata | Gestione remota firewall Windows                    |


> [!NOTE] 
> Alcuni snap-in MMC non dispone di un gruppo di regole corrispondente che consenta loro di connettersi attraverso il firewall. Tuttavia, l'attivazione dei gruppi di regole relativi a Visualizzatore eventi, Servizi o Cartelle condivise consente la connessione della maggior parte degli altri snap-in. 
>
> Inoltre, alcuni snap-in richiedono ulteriori configurazioni per potersi connettere attraverso Windows Firewall:
>
> - Gestione disco. È necessario prima di tutto avviare Servizio dischi virtuali nel computer in modalità Server Core. È inoltre necessario configurare correttamente le regole di Gestione disco nel computer che esegue lo snap-in MMC.
> - Monitor di sicurezza IP. È necessario prima di tutto attivare la gestione remota di questo snap-in. A tale scopo, al prompt digitare **Cscript \windows\system32\scregedit.wsf /im 1**
> - Monitoraggio affidabilità e prestazioni. Lo snap-in non richiede altre configurazioni, ma quando lo si usa per monitorare un computer in modalità Server Core, è possibile monitorare solo i dati sulle prestazioni. I dati sull'affidabilità non sono infatti disponibili.

## <a name="managing-with-remote-desktop-services"></a>La gestione con Servizi Desktop remoto

È possibile usare [Desktop remoto](../../remote/remote-desktop-services/welcome-to-rds.md) per gestire un server Server Core da computer remoti.

Prima di poter accedere a Server Core, è necessario eseguire il comando seguente: 
```
cscript C:\Windows\System32\Scregedit.wsf /ar 0
```
Si consentirà così alla modalità Desktop remoto per amministrazione di accettare connessioni.

## <a name="add-hardware-and-manage-drivers-locally"></a>Aggiungere hardware e gestire i driver in locale

Per aggiungere hardware a un Server server Core, seguire le istruzioni fornite dal fornitore dell'hardware per l'installazione di nuovo hardware. 

Se l'hardware non plug and play, è necessario installare manualmente il driver. A tale scopo, copiare i file dei driver in un percorso temporaneo nel server e quindi eseguire il comando seguente:
```
pnputil –i –a <driverinf>
```
In cui *driverinf* è il nome file del file. inf del driver.

Se richiesto, riavviare il computer.

Per verificare quali driver sono installati, eseguire il comando seguente: 
```
sc query type= driver
```

> [!NOTE] 
> Perché il comando venga correttamente completato, è necessario includere uno spazio dopo il segno di uguale.

Per disabilitare un driver di dispositivo, eseguire le operazioni seguenti: 
```
sc delete <service_name>
```

In cui *service_name* è il nome del servizio ottenuta al momento dell'esecuzione **tipo di query sc = driver**.