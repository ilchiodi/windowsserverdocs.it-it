---
title: Gestire in remoto gli host Hyper-V
description: Viene descritta la compatibilità di versione tra gli host Hyper-V e gestione di Hyper-V e come connettersi agli host remoti in ambienti diversi, compresi domini e autonomo.
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 2d34e98c-6134-479b-8000-3eb360b8b8a3
author: KBDAzure
ms.author: kathydav
ms.date: 12/06/2016
ms.openlocfilehash: d4d9f2dd3727e196bb6893fd5041fa3f08c30796
ms.sourcegitcommit: 48bb3e5c179dc520fa879b16c9afe09e07c87629
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/31/2019
ms.locfileid: "66453186"
---
# <a name="remotely-manage-hyper-v-hosts-with-hyper-v-manager"></a>Gestire in remoto gli host Hyper-V con gestione Hyper-V

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows 10, Windows 8.1

Questo articolo elenca le combinazioni supportate di host Hyper-V e le versioni di Hyper-V Manager e viene descritto come connettersi agli host Hyper-V locali e remoti in modo da poterli gestire. 

Gestione di Hyper-V consente di gestire un numero ridotto di host Hyper-V, locali e remoti. È installato quando si installa gli strumenti di gestione di Hyper-V, ed è possibile farlo tramite una procedura completa di installazione di Hyper-V o un'installazione di soli strumenti. Esegue un metodo di installazione di soli strumenti, è possibile usare gli strumenti in computer che non soddisfano i requisiti hardware per host Hyper-V. Per informazioni dettagliate sull'hardware per gli host Hyper-V, vedere [requisiti di sistema](../System-requirements-for-Hyper-V-on-Windows.md).

Se non è installato Hyper-V Manager, vedere la [istruzioni](#install-hyper-v-manager) sotto.

## <a name="supported-combinations-of-hyper-v-manager-and-hyper-v-host-versions"></a>Combinazioni supportate di gestione di Hyper-V e le versioni di host Hyper-V

In alcuni casi è possibile usare una versione diversa di gestione di Hyper-V rispetto alla versione di Hyper-V nell'host, come illustrato nella tabella. Quando si esegue questa operazione, gestione di Hyper-V fornisce le funzionalità disponibili per la versione di Hyper-V nell'host che si sta gestendo. Ad esempio, se si usa la versione di gestione di Hyper-V in Windows Server 2012 R2 per gestire in remoto un host che esegue Hyper-V in Windows Server 2012, sarà possibile utilizzare le funzionalità disponibili in Windows Server 2012 R2 nell'host Hyper-V.

La tabella seguente illustra le versioni di un host Hyper-V è possibile gestire da una particolare versione di gestione di Hyper-V. Supportato solo sono elencate le versioni del sistema operativo. Per informazioni dettagliate sullo stato di supporto di una versione particolare del sistema operativo, usare il **ricerca prodotto lifecyle** pulsante il [Microsoft Lifecycle Policy](https://support.microsoft.com/lifecycle) pagina. In generale, le versioni precedenti di Hyper-V Manager possono gestire solo un host Hyper-V che eseguono la stessa versione o la versione di Windows Server confrontabile.

|Versione di gestione di Hyper-V | Versione host Hyper-V|
|---|---|
|Windows 2016, Windows 10|-Windows Server 2016, tutte le edizioni e installazione delle opzioni, inclusi Nano Server e la versione corrispondente di Hyper-V Server <br>-Windows Server 2012 R2-tutte le edizioni e opzioni di installazione e la versione corrispondente di Hyper-V Server <br>-Windows Server 2012, tutte le edizioni e opzioni di installazione e la versione corrispondente di Hyper-V Server <br> - Windows 10 <br> - Windows 8.1  |
| Windows Server 2012 R2, Windows 8.1 | -Windows Server 2012 R2-tutte le edizioni e opzioni di installazione e la versione corrispondente di Hyper-V Server <br>-Windows Server 2012, tutte le edizioni e opzioni di installazione e la versione corrispondente di Hyper-V Server <br>- Windows 8.1
| Windows Server 2012 | -Windows Server 2012, tutte le edizioni e opzioni di installazione e la versione corrispondente di Hyper-V Server
| Windows Server 2008 R2 Service Pack 1, Windows 7 Service Pack 1 | -Windows Server 2008 R2-tutte le edizioni e opzioni di installazione e la versione corrispondente di Hyper-V Server
| Windows Server 2008, Windows Vista Service Pack 2 | -Windows Server 2008, ovvero tutte le edizioni e opzioni di installazione e la versione corrispondente di Hyper-V Server

>[!NOTE]
>Supporto di Service pack è terminato il 12 gennaio 2016 per Windows 8. Per altre informazioni, vedere la [domande frequenti su Windows 8.1](https://support.microsoft.com/help/18581).

## <a name="connect-to-a-hyper-v-host"></a>Connettersi a un host Hyper-V

Per connettersi a un host Hyper-V dalla console di gestione Hyper-V, fare doppio clic su **gestione di Hyper-V** nel riquadro a sinistra e quindi fare clic su **Connetti al Server**.

## <a name="manage-hyper-v-on-a-local-computer"></a>Gestire Hyper-V in un computer locale

Gestione di Hyper-V non vengono elencati tutti i computer che ospitano Hyper-V fino a quando non si aggiunge il computer, tra cui un computer locale. A tale scopo, effettuare le seguenti operazioni:

1. Nel riquadro sinistro, fare doppio clic su **Hyper-V Manager**.
2. Fare clic su **connettersi al Server**.
3. Dal **seleziona Computer**, fare clic su **computer locale** e quindi fare clic su **OK**.

Se non è possibile connettersi:

* È possibile che solo gli strumenti di Hyper-V siano installati. Per verificare che la piattaforma Hyper-V sia installata, cercare il servizio di gestione delle macchine virtuali. / (Aprire l'app desktop di servizi di: fare clic su **inizio**, fare clic sul **Inizia ricerca** , digitare **Services. msc**, quindi premere **invio**. Se il servizio di gestione delle macchine virtuali non è elencato, installare la piattaforma Hyper-V seguendo le istruzioni disponibili nel [installare Hyper-V](../get-started/Install-the-Hyper-V-role-on-Windows-Server.md).
* Verificare che l'hardware soddisfi i requisiti. Visualizzare [requisiti di sistema](../System-requirements-for-Hyper-V-on-Windows.md).
* Verificare che l'account utente appartenga al gruppo Administrators o al gruppo amministratori Hyper-V.

## <a name="manage-hyper-v-hosts-remotely"></a>Gestire gli host Hyper-V in modalità remota  

Per gestire gli host Hyper-V remoti, abilitare gestione remota sul computer locale sia host remoto.

In Windows Server, aprire Server Manager \> **Server locale** \> **gestione remota** e quindi fare clic su **Consenti connessioni remote a computer**. 

In alternativa, da entrambi i sistemi operativi, aprire Windows PowerShell come amministratore ed eseguire: 

```
Enable-PSRemoting
```

### <a name="connect-to-hosts-in-the-same-domain"></a>Connettersi agli host nello stesso dominio

Per Windows 8.1 e versioni precedenti, la gestione remota funziona solo quando l'host si trova nello stesso dominio e l'account utente locale è anche nell'host remoto.

Per aggiungere un host Hyper-V remoto alla gestione di Hyper-V, selezionare **un altro computer** nel **seleziona Computer** finestra e digitare il nome host dell'host remoto, nome NetBIOS o il nome di dominio completo \(FQDN\).

Gestione di Hyper-V in Windows 10 e Windows Server 2016 offre ulteriori tipi di connessione remota rispetto alle versioni precedenti, descritti nelle sezioni seguenti.  

### <a name="connect-to-a-windows-2016-or-windows-10-remote-host-as-a-different-user"></a>Connettersi a un host remoto Windows 2016 o Windows 10 come utente diverso

Ciò consente di connettersi all'host Hyper-V quando non si esegue nel computer locale come un utente che è un membro del gruppo amministratori Hyper-V o il gruppo Administrators nell'host Hyper-V. A tale scopo, effettuare le seguenti operazioni:

1. Nel riquadro sinistro, fare doppio clic su **Hyper-V Manager**.
1. Fare clic su **connettersi al Server**.
1. Selezionare **Connetti come altro utente** nel **seleziona Computer** finestra di dialogo.
1. Selezionare **Imposta utente**.

>[!NOTE]
> Questa impostazione funziona solo per Windows Server 2016 o Windows 10 **remoto** host.

### <a name="connect-to-a-windows-2016-or-windows-10-remote-host-using-ip-address"></a>Connettersi a un host remoto in Windows 2016 o Windows 10 con indirizzo IP

A tale scopo, effettuare le seguenti operazioni:

1. Nel riquadro sinistro, fare doppio clic su **Hyper-V Manager**.
1. Fare clic su **connettersi al Server**.
1. Digitare l'indirizzo IP nel **un altro Computer** campo di testo.

>[!NOTE]
> Questa impostazione funziona solo per Windows Server 2016 o Windows 10 **remoto** host.

### <a name="connect-to-a-windows-2016-or-windows-10-remote-host-outside-your-domain-or-with-no-domain"></a>Connettersi a un Windows 2016 o Windows 10 remoto host all'esterno del dominio o senza dominio

A tale scopo, effettuare le seguenti operazioni:

1. Nell'host Hyper-V da gestire, aprire una sessione di Windows PowerShell come amministratore.

1. Creare le regole firewall necessarie per le zone private di rete:   
   
   ```
   Enable-PSRemoting
   ```

2. Per consentire l'accesso remoto alle zone pubbliche, abilitare le regole del firewall per CredSSP e WinRM:
  
   ```
   Enable-WSManCredSSP -Role server
   ```

    Per informazioni dettagliate, vedere [Enable-PSRemoting](https://technet.microsoft.com/library/hh849694.aspx) e [Enable-WSManCredSSP](https://technet.microsoft.com/library/hh849872.aspx).

Successivamente, configurare il computer che si userà per gestire l'host Hyper-V.

1. Aprire una sessione di Windows PowerShell come amministratore.
1. Eseguire questi comandi:

     ```
     Set-Item WSMan:\localhost\Client\TrustedHosts -Value "fqdn-of-hyper-v-host"
     ```
     ```
     Enable-WSManCredSSP -Role client -DelegateComputer "fqdn-of-hyper-v-host"
     ```
1. È necessario anche configurare i criteri di gruppo seguenti: 
    * **Configurazione computer** \> **modelli amministrativi** \> **sistema** \> **ladelegadellecredenziali** \> **Consenti delega credenziali nuove con l'autenticazione server NTLM-only**
    * Fare clic su **abilitare** e aggiungere *wsman/fqdn-di-hyper-v-host*.
1. Aprire **Hyper-V Manager**.
1. Nel riquadro sinistro, fare doppio clic su **Hyper-V Manager**.
1. Fare clic su **connettersi al Server**.

>[!NOTE]
> Questa impostazione funziona solo per Windows Server 2016 o Windows 10 **remoto** host.

Per informazioni dettagliate sui cmdlet, vedere [Set-Item](https://msdn.microsoft.com/powershell/reference/5.1/microsoft.powershell.management/set-item) e [Enable-WSManCredSSP](https://technet.microsoft.com/library/hh849872.aspx).

## <a name="install-hyper-v-manager"></a>Installare Hyper-V Manager

Per usare uno strumento dell'interfaccia utente, scegliere quello appropriato per il sistema operativo nel computer in cui si eseguiranno di gestione di Hyper-V:

In Windows Server, aprire Server Manager \> **Manage** \> **Aggiungi ruoli e funzionalità**. Spostare il **caratteristiche** pagina, espandere **strumenti di amministrazione remota del server** \> **strumenti di amministrazione ruoli** \>  **Gli strumenti di gestione di Hyper-V**. 

In Windows, è disponibile in Gestione Hyper-V [qualsiasi sistema operativo Windows che include Hyper-V](https://msdn.microsoft.com/virtualization/hyperv_on_windows/quick_start/walkthrough_compatibility).

1. Sul desktop di Windows, fare clic sul pulsante Start e iniziare a digitare **programmi e funzionalità**. 
1. Nei risultati della ricerca, fare clic su **programmi e funzionalità**.
1. Nel riquadro sinistro, fare clic su **o disattivazione delle funzionalità Windows attivare**.
1. Espandere la cartella di Hyper-V, e **fare clic su strumenti di gestione di Hyper-V**.
1. Per installare Hyper-V Manager, fare clic su **strumenti di gestione di Hyper-V**. Se si desidera installare anche il modulo Hyper-V, fare clic su tale opzione.

Per usare Windows PowerShell, eseguire il comando seguente come amministratore:

```
add-windowsfeature rsat-hyper-v-tools
```

## <a name="see-also"></a>Vedere anche  
 
[Installazione di Hyper-V](../get-started/Install-the-Hyper-V-role-on-Windows-Server.md) 

