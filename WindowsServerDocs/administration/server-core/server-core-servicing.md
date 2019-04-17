---
title: Applicazione delle patch Server Core
description: Informazioni su come aggiornare un'installazione Server Core di Windows Server
ms.prod: windows-server-threshold
ms.mktglfcycl: manage
ms.sitesec: library
author: lizap
ms.localizationpriority: medium
ms.date: 10/17/2017
ms.openlocfilehash: f51ffae5ed8f91cca386eb209e7a1d8cc664ceeb
ms.sourcegitcommit: 1533d994a6ddea54ac189ceb316b7d3c074307db
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 06/01/2018
ms.locfileid: "1448094"
---
# <a name="patch-a-server-core-installation"></a>Patch di un'installazione Server Core

> Si applica a: Windows Server (Channel semi-annuale) e Windows Server 2016

È possibile applicare patch a un server che esegue installazione Server Core nei modi seguenti:

- **Utilizzo di Windows Update automaticamente o con Windows Server Update Services (WSUS)**. Utilizzando Windows Update, automaticamente o con gli strumenti da riga di comando o Windows Server Update Services (WSUS), si possono gestire server che esegue un'installazione Server Core.

- **Manualmente**. Anche in organizzazioni che non utilizzano Windows update o Windows Server Update Services, è possibile applicare manualmente gli aggiornamenti.

## <a name="view-the-updates-installed-on-your-server-core-server"></a>Visualizza gli aggiornamenti installati nel server Server Core
Prima di aggiungere un nuovo aggiornamento al Server principale, è consigliabile vedere quali aggiornamenti sono già stati installati.

Per visualizzare gli aggiornamenti tramite Windows PowerShell, eseguire **Get-Hotfix**.

Per visualizzare gli aggiornamenti eseguendo un comando, eseguire **systeminfo.exe**. Potrebbero verificarsi una breve attesa mentre lo strumento analizza il sistema.

È inoltre possibile eseguire **wmic qfe elenco** dalla riga di comando. 

## <a name="patch-server-core-automatically-with-windows-update"></a>Patch Server Core automaticamente con Windows Update

Utilizzare la procedura seguente per le patch necessarie automaticamente con l'aggiornamento di Windows:

1. Verificare l'impostazione di Windows Update corrente:
   ```
   %systemroot%\system32\Cscript scregedit.wsf /AU /v 
   ```

2. Per abilitare gli aggiornamenti automatici:

   ```
   Net stop wuauserv 
   %systemroot%\system32\Cscript scregedit.wsf /AU 4 
   Net start wuauserv
   ```  

3. Per disattivare gli aggiornamenti automatici, eseguire:

   ```
   Net stop wuauserv 
   %systemroot%\system32\Cscript scregedit.wsf /AU 1 
   Net start wuauserv 
   ```

Se il server è un membro di un dominio, è inoltre possibile configurare Windows Update tramite criteri di gruppo. Per altre informazioni, vedi https://go.microsoft.com/fwlink/?LinkId=192470. Tuttavia, quando si utilizza questo metodo, 4 ("download e pianifica l'installazione") è solo rilevanti per le installazioni Server Core a causa della mancanza di un'interfaccia grafica. Per maggiore controllo su cui sono installati gli aggiornamenti e, è possibile utilizzare uno script che fornisce un equivalente della riga di comando della maggior parte dell'interfaccia grafica Windows Update. Per informazioni relative allo script, vedere https://go.microsoft.com/fwlink/?LinkId=192471.

Per forzare l'aggiornamento di Windows per rilevare e installare gli aggiornamenti disponibili immediatamente, eseguire il comando seguente:

```
Wuauclt /detectnow 
```

A seconda degli aggiornamenti installati, potrebbe essere necessario riavviare il computer, anche se il sistema non visualizzerà di questo oggetto. Per determinare se il processo di installazione è stata completata, utilizzare Gestione attività per verificare che i processi **Wuauclt** o **Installer attendibili** sono non attivamente in esecuzione. È inoltre possibile utilizzare i metodi nella [visualizzazione gli aggiornamenti installati nel server Server Core](#view-the-updates-installed-on-your-Server-Core-server) per controllare l'elenco degli aggiornamenti installati.

## <a name="patch-the-server-with-wsus"></a>Patch per il server con WSUS 

Se il server a Server Core è un membro di un dominio, è possibile configurare per l'utilizzo di un server WSUS con criteri di gruppo. Per ulteriori informazioni, scaricare le [informazioni di riferimento di criteri di gruppo](https://www.microsoft.com/download/details.aspx?id=25250). È anche possibile esaminare [Configurare le impostazioni di criteri di gruppo per gli aggiornamenti automatici](../windows-server-update-services/deploy/4-configure-group-policy-settings-for-automatic-updates.md)

## <a name="patch-the-server-manually"></a>Le patch necessarie manualmente

Scaricare l'aggiornamento e renderla disponibile per l'installazione Server Core.
Al prompt dei comandi, eseguire il comando seguente:

```
Wusa <update>.msu /quiet 
```

A seconda degli aggiornamenti installati, potrebbe essere necessario riavviare il computer, anche se il sistema non visualizzerà di questo oggetto.

Per disinstallare manualmente un aggiornamento, eseguire il comando seguente:

```
Wusa /uninstall <update>.msu /quiet 
```

