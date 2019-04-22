---
title: L'applicazione di patch Server Core
description: Informazioni su come aggiornare un'installazione Server Core di Windows Server
ms.prod: windows-server-threshold
ms.mktglfcycl: manage
ms.sitesec: library
author: lizap
ms.localizationpriority: medium
ms.date: 10/17/2017
ms.openlocfilehash: f51ffae5ed8f91cca386eb209e7a1d8cc664ceeb
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59817352"
---
# <a name="patch-a-server-core-installation"></a>Patch per un'installazione Server Core

> Si applica a: Windows Server (canale semestrale) e Windows Server 2016

È possibile applicare patch a un server che esegue l'installazione Server Core nei modi seguenti:

- **Utilizzo di Windows Update automaticamente o con Windows Server Update Services (WSUS)**. Tramite Windows Update, automaticamente o con gli strumenti da riga di comando o Windows Server Update Services (WSUS), si possono gestire i server che eseguono un'installazione Server Core.

- **Manualmente**. Anche nelle organizzazioni che non usano Windows update o WSUS, è possibile applicare manualmente gli aggiornamenti.

## <a name="view-the-updates-installed-on-your-server-core-server"></a>Visualizzare gli aggiornamenti installati nel server Server Core
Prima di aggiungere un nuovo aggiornamento in Server Core, è una buona idea per verificare quali aggiornamenti sono già stati installati.

Per visualizzare gli aggiornamenti tramite Windows PowerShell, eseguire **Get-Hotfix**.

Per visualizzare gli aggiornamenti tramite un comando, eseguire **systeminfo.exe**. Potrebbe verificarsi un breve ritardo mentre lo strumento controlla il sistema.

È anche possibile eseguire **wmic qfe elenco** dalla riga di comando. 

## <a name="patch-server-core-automatically-with-windows-update"></a>Patch di Server Core automaticamente con Windows Update

Usare la procedura seguente per le patch necessarie automaticamente con Windows Update:

1. Verificare l'impostazione corrente di Windows Update:
   ```
   %systemroot%\system32\Cscript scregedit.wsf /AU /v 
   ```

2. Per abilitare gli aggiornamenti automatici:

   ```
   Net stop wuauserv 
   %systemroot%\system32\Cscript scregedit.wsf /AU 4 
   Net start wuauserv
   ```  

3. Per disabilitare gli aggiornamenti automatici, eseguire:

   ```
   Net stop wuauserv 
   %systemroot%\system32\Cscript scregedit.wsf /AU 1 
   Net start wuauserv 
   ```

Se il server è membro di un dominio, è inoltre possibile configurare Windows Update tramite Criteri di gruppo. Per altre informazioni, vedere https://go.microsoft.com/fwlink/?LinkId=192470. Tuttavia, quando si usa questo metodo, solo l'opzione 4 ("download automatico e pianificazione dell'installazione") è rilevante per le installazioni Server Core a causa della mancanza di un'interfaccia grafica. Per un maggiore controllo sui tipi di aggiornamenti da installare e i relativi tempi, è possibile utilizzare uno script che fornisce un equivalente della riga di comando per la maggior parte delle funzionalità dell'interfaccia grafica di Windows Update. Per informazioni sullo script, vedere https://go.microsoft.com/fwlink/?LinkId=192471.

Per forzare in Windows Update il rilevamento e l'installazione immediati di eventuali aggiornamenti disponibili, eseguire il comando seguente:

```
Wuauclt /detectnow 
```

A seconda degli aggiornamenti installati, potrebbe essere necessario riavviare il computer, benché non si riceva alcuna notifica in merito dal sistema. Per determinare se il processo di installazione è stata completata, utilizzare Gestione attività per verificare che il **Wuauclt** oppure **programma di installazione Trusted** processi non sono attivamente in esecuzione. È inoltre possibile utilizzare i metodi [consente di visualizzare gli aggiornamenti installati nel server Server Core](#view-the-updates-installed-on-your-Server-Core-server) per controllare l'elenco degli aggiornamenti installati.

## <a name="patch-the-server-with-wsus"></a>Patch per il server con WSUS 

Se il server in modalità Server Core è membro di un dominio, è possibile configurarlo per l'utilizzo di un server WSUS con Criteri di gruppo. Per altre informazioni, scaricare il [informazioni di riferimento dei criteri di gruppo](https://www.microsoft.com/download/details.aspx?id=25250). È anche possibile esaminare [configurare le impostazioni di criteri di gruppo per gli aggiornamenti automatici](../windows-server-update-services/deploy/4-configure-group-policy-settings-for-automatic-updates.md)

## <a name="patch-the-server-manually"></a>Patch manualmente il server

Scaricare l'aggiornamento e renderla disponibile per l'installazione Server Core.
Un prompt dei comandi, eseguire il comando seguente:

```
Wusa <update>.msu /quiet 
```

A seconda degli aggiornamenti installati, potrebbe essere necessario riavviare il computer, benché non si riceva alcuna notifica in merito dal sistema.

Per disinstallare un aggiornamento manualmente, eseguire il comando seguente:

```
Wusa /uninstall <update>.msu /quiet 
```

