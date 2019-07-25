---
title: Applicazione di patch a Server Core
description: Informazioni su come aggiornare un'installazione Server Core di Windows Server
ms.prod: windows-server-threshold
ms.mktglfcycl: manage
ms.sitesec: library
author: lizap
ms.localizationpriority: medium
ms.date: 10/17/2017
ms.openlocfilehash: b649a3cc16bc1a527c5df0b4a0d543da22a882d2
ms.sourcegitcommit: 216d97ad843d59f12bf0b563b4192b75f66c7742
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/24/2019
ms.locfileid: "68476488"
---
# <a name="patch-a-server-core-installation"></a>Applicare patch a un'installazione Server Core

> Si applica a Windows Server 2019, Windows Server 2016 e Windows Server (canale semestrale)

È possibile applicare patch a un server che esegue l'installazione dei componenti di base del server nei modi seguenti:

- **Utilizzando Windows Update automaticamente o con Windows Server Update Services (WSUS)** . Utilizzando Windows Update, in modo automatico o con strumenti da riga di comando oppure Windows Server Update Services (WSUS), è possibile servire i server che eseguono un'installazione dei componenti di base del server.

- **Manualmente**. Anche nelle organizzazioni che non utilizzano Windows Update o WSUS, è possibile applicare gli aggiornamenti manualmente.

## <a name="view-the-updates-installed-on-your-server-core-server"></a>Visualizzare gli aggiornamenti installati nel server Server Core
Prima di aggiungere un nuovo aggiornamento a Server Core, è consigliabile vedere quali aggiornamenti sono già stati installati.

Per visualizzare gli aggiornamenti usando Windows PowerShell, eseguire **get-hotfix**.

Per visualizzare gli aggiornamenti eseguendo un comando, eseguire **systeminfo. exe**. È possibile che si verifichi un breve ritardo mentre lo strumento controlla il sistema.

È anche possibile eseguire l' **elenco QFE QFE** dalla riga di comando. 

## <a name="patch-server-core-automatically-with-windows-update"></a>Patch server core automaticamente con Windows Update

Usare la procedura seguente per applicare la patch al server automaticamente con Windows Update:

1. Verificare l'impostazione del Windows Update corrente:
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

Se il server è membro di un dominio, è inoltre possibile configurare Windows Update tramite Criteri di gruppo. Per altre informazioni, vedere https://go.microsoft.com/fwlink/?LinkId=192470. Tuttavia, quando si usa questo metodo, solo l'opzione 4 ("download automatico e pianificazione dell'installazione") è pertinente per le installazioni dei componenti di base del server a causa della mancanza di un'interfaccia grafica. Per un maggiore controllo sui tipi di aggiornamenti da installare e i relativi tempi, è possibile utilizzare uno script che fornisce un equivalente della riga di comando per la maggior parte delle funzionalità dell'interfaccia grafica di Windows Update. Per informazioni sullo script, vedere https://go.microsoft.com/fwlink/?LinkId=192471.

Per forzare in Windows Update il rilevamento e l'installazione immediati di eventuali aggiornamenti disponibili, eseguire il comando seguente:

```
Wuauclt /detectnow 
```

A seconda degli aggiornamenti installati, potrebbe essere necessario riavviare il computer, benché non si riceva alcuna notifica in merito dal sistema. Per determinare se il processo di installazione è stato completato, utilizzare Gestione attività per verificare che i processi di **wuauclt** o del **programma di installazione trusted** non siano attivamente in esecuzione. Per verificare l'elenco degli aggiornamenti installati, è inoltre possibile utilizzare i metodi descritti in [visualizzare gli aggiornamenti installati nel server Server Core](#view-the-updates-installed-on-your-server-core-server) .

## <a name="patch-the-server-with-wsus"></a>Applicare patch al server con WSUS 

Se il server in modalità Server Core è membro di un dominio, è possibile configurarlo per l'utilizzo di un server WSUS con Criteri di gruppo. Per ulteriori informazioni, scaricare le [informazioni di riferimento criteri di gruppo](https://www.microsoft.com/download/details.aspx?id=25250). È anche possibile vedere [configurare le impostazioni di criteri di gruppo per aggiornamenti automatici](../windows-server-update-services/deploy/4-configure-group-policy-settings-for-automatic-updates.md)

## <a name="patch-the-server-manually"></a>Applicare patch al server manualmente

Scaricare l'aggiornamento e renderlo disponibile per l'installazione dei componenti di base del server.
Al prompt dei comandi eseguire il comando seguente:

```
Wusa <update>.msu /quiet 
```

A seconda degli aggiornamenti installati, potrebbe essere necessario riavviare il computer, benché non si riceva alcuna notifica in merito dal sistema.

Per disinstallare un aggiornamento manualmente, eseguire il comando seguente:

```
Wusa /uninstall <update>.msu /quiet 
```

