---
title: Gestire in remoto gli host Hyper-V
description: Descrive la compatibilità delle versioni tra gli host Hyper-V e la console di gestione di Hyper-V e come connettersi a host remoti in ambienti diversi, inclusi tra domini e autonomi.
ms.prod: windows-server
manager: dongill
ms.technology: compute-hyper-v
ms.topic: article
ms.assetid: 2d34e98c-6134-479b-8000-3eb360b8b8a3
author: kbdazure
ms.author: kathydav
ms.date: 12/06/2016
ms.openlocfilehash: 592bb6352c4ca56770e1a3051ecbc88d9d378467
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80859414"
---
# <a name="remotely-manage-hyper-v-hosts-with-hyper-v-manager"></a>Gestire in remoto gli host Hyper-V con la console di gestione di Hyper-V

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows 10, Windows 8.1

Questo articolo elenca le combinazioni supportate di host Hyper-V e versioni di console di gestione di Hyper-V e descrive come connettersi a host Hyper-V locali e remoti in modo da poterli gestire. 

La console di gestione di Hyper-V consente di gestire un numero ridotto di host Hyper-V, sia remoti che locali. Viene installato quando si installano gli strumenti di gestione di Hyper-V, che possono essere eseguite tramite un'installazione completa di Hyper-V o un'installazione di tipo "solo strumenti". Per eseguire un'installazione di soli strumenti, è possibile usare gli strumenti in computer che non soddisfano i requisiti hardware per ospitare Hyper-V. Per informazioni dettagliate sull'hardware per gli host Hyper-V, vedere [requisiti di sistema](../System-requirements-for-Hyper-V-on-Windows.md).

Se la console di gestione di Hyper-V non è installata, vedere le [istruzioni](#install-hyper-v-manager) seguenti.

## <a name="supported-combinations-of-hyper-v-manager-and-hyper-v-host-versions"></a>Combinazioni supportate di Hyper-V Manager e versioni host Hyper-V

In alcuni casi è possibile usare una versione diversa di gestione Hyper-V rispetto alla versione Hyper-V nell'host, come illustrato nella tabella. Quando si esegue questa operazione, la console di gestione di Hyper-V fornisce le funzionalità disponibili per la versione di Hyper-V nell'host che si sta gestendo. Se ad esempio si usa la versione della console di gestione di Hyper-V in Windows Server 2012 R2 per gestire in remoto un host che esegue Hyper-V in Windows Server 2012, non sarà possibile usare le funzionalità disponibili in Windows Server 2012 R2 nell'host Hyper-V.

La tabella seguente illustra le versioni di un host Hyper-V che è possibile gestire da una particolare versione della console di gestione di Hyper-V. Sono elencate solo le versioni del sistema operativo supportate. Per informazioni dettagliate sullo stato di supporto di una particolare versione del sistema operativo, usare il pulsante **Cerca vita del prodotto** nella pagina [criteri ciclo](https://support.microsoft.com/lifecycle) di vita Microsoft. In generale, le versioni precedenti della console di gestione di Hyper-V possono gestire solo un host Hyper-V che esegue la stessa versione o la versione di Windows Server paragonabile.

|Versione della console di gestione di Hyper-V | Versione host Hyper-V|
|---|---|
|Windows 2016, Windows 10|-Windows Server 2016-tutte le edizioni e le opzioni di installazione, tra cui nano server e la versione corrispondente di Hyper-V Server <br>-Windows Server 2012 R2-tutte le edizioni e le opzioni di installazione e la versione corrispondente di Hyper-V Server <br>-Windows Server 2012-tutte le edizioni e le opzioni di installazione e la versione corrispondente di Hyper-V Server <br> -Windows 10 <br> -Windows 8.1  |
| Windows Server 2012 R2, Windows 8.1 | -Windows Server 2012 R2-tutte le edizioni e le opzioni di installazione e la versione corrispondente di Hyper-V Server <br>-Windows Server 2012-tutte le edizioni e le opzioni di installazione e la versione corrispondente di Hyper-V Server <br>-Windows 8.1
| Windows Server 2012 | -Windows Server 2012-tutte le edizioni e le opzioni di installazione e la versione corrispondente di Hyper-V Server
| Windows Server 2008 R2 Service Pack 1, Windows 7 Service Pack 1 | -Windows Server 2008 R2-tutte le edizioni e le opzioni di installazione e la versione corrispondente di Hyper-V Server
| Windows Server 2008, Windows Vista Service Pack 2 | -Windows Server 2008-tutte le edizioni e le opzioni di installazione e la versione corrispondente di Hyper-V Server

>[!NOTE]
>Il supporto del Service Pack è terminato per Windows 8 il 12 gennaio 2016. Per ulteriori informazioni, vedere le [domande frequenti su Windows 8.1](https://support.microsoft.com/help/18581).

## <a name="connect-to-a-hyper-v-host"></a>Connettersi a un host Hyper-V

Per connettersi a un host Hyper-V dalla console di gestione di Hyper-V, fare clic con il pulsante destro del mouse su console di **gestione di Hyper-v** nel riquadro sinistro, quindi scegliere **Connetti al server**.

## <a name="manage-hyper-v-on-a-local-computer"></a>Gestire Hyper-V in un computer locale

La console di gestione di Hyper-V non elenca i computer che ospitano Hyper-V fino a quando non si aggiunge il computer, incluso un computer locale. A tale scopo, effettuare l'operazione seguente:

1. Nel riquadro sinistro fare clic con il pulsante destro del mouse su console di **gestione di Hyper-V**.
2. Fare clic su **Connetti al server**.
3. In **Seleziona computer**fare clic su **computer locale** e quindi su **OK**.

Se non è possibile connettersi:

* È possibile che siano installati solo gli strumenti di Hyper-V. Per verificare che la piattaforma Hyper-V sia installata, cercare il servizio Virtual Machine Management. /(Aprire l'app desktop dei servizi: fare clic su **Start**, fare clic sulla casella **Avvia ricerca** , digitare **Services. msc**, quindi premere **invio**. Se il servizio Virtual Machine Management non è elencato, installare la piattaforma Hyper-V seguendo le istruzioni riportate in [installare Hyper-v](../get-started/Install-the-Hyper-V-role-on-Windows-Server.md).
* Verificare che l'hardware soddisfi i requisiti. Vedere [requisiti di sistema](../System-requirements-for-Hyper-V-on-Windows.md).
* Verificare che l'account utente appartenga al gruppo Administrators o al gruppo Amministratori Hyper-V.

## <a name="manage-hyper-v-hosts-remotely"></a>Gestire gli host Hyper-V in modalità remota  

Per gestire gli host Hyper-V remoti, abilitare la gestione remota nel computer locale e nell'host remoto.

In Windows Server aprire Server Manager \>**server locale** \>**gestione remota** , quindi fare clic su **Consenti connessioni remote al computer**. 

In alternativa, da uno dei due sistemi operativi aprire Windows PowerShell come amministratore ed eseguire: 

```
Enable-PSRemoting
```

### <a name="connect-to-hosts-in-the-same-domain"></a>Connettersi agli host nello stesso dominio

Per Windows 8.1 e versioni precedenti, la gestione remota funziona solo quando l'host si trova nello stesso dominio e l'account utente locale si trova anche nell'host remoto.

Per aggiungere un host Hyper-V remoto alla console di gestione di Hyper-V, selezionare **un altro computer** nella finestra di dialogo **Seleziona computer** e digitare il nome host dell'host remoto, il nome NetBIOS o il nome di dominio completo \(\)FQDN.

La console di gestione di Hyper-V in Windows Server 2016 e Windows 10 offre più tipi di connessione remota rispetto alle versioni precedenti, descritti nelle sezioni riportate di seguito.  

### <a name="connect-to-a-windows-2016-or-windows-10-remote-host-as-a-different-user"></a>Connettersi a un host remoto Windows 2016 o Windows 10 come utente diverso

In questo modo è possibile connettersi all'host Hyper-V quando non è in esecuzione nel computer locale come utente membro del gruppo Amministratori Hyper-V o del gruppo Administrators nell'host Hyper-V. A tale scopo, effettuare l'operazione seguente:

1. Nel riquadro sinistro fare clic con il pulsante destro del mouse su console di **gestione di Hyper-V**.
1. Fare clic su **Connetti al server**.
1. Selezionare **Connetti come altro utente** nella finestra di dialogo **Seleziona computer** .
1. Selezionare **Imposta utente**.

>[!NOTE]
> Questa operazione funzionerà solo per gli host **remoti** windows server 2016 o Windows 10.

### <a name="connect-to-a-windows-2016-or-windows-10-remote-host-using-ip-address"></a>Connettersi a un host remoto Windows 2016 o Windows 10 usando un indirizzo IP

A tale scopo, effettuare l'operazione seguente:

1. Nel riquadro sinistro fare clic con il pulsante destro del mouse su console di **gestione di Hyper-V**.
1. Fare clic su **Connetti al server**.
1. Digitare l'indirizzo IP nel campo di testo **altro computer** .

>[!NOTE]
> Questa operazione funzionerà solo per gli host **remoti** windows server 2016 o Windows 10.

### <a name="connect-to-a-windows-2016-or-windows-10-remote-host-outside-your-domain-or-with-no-domain"></a>Connettersi a un host remoto Windows 2016 o Windows 10 all'esterno del dominio o senza dominio

A tale scopo, effettuare l'operazione seguente:

1. Nell'host Hyper-V da gestire, aprire una sessione di Windows PowerShell come amministratore.

1. Creare le regole del firewall necessarie per le aree di rete private:   
   
   ```
   Enable-PSRemoting
   ```

2. Per consentire l'accesso remoto sulle zone pubbliche, abilitare le regole del firewall per CredSSP e WinRM:
  
   ```
   Enable-WSManCredSSP -Role server
   ```

    Per informazioni dettagliate, vedere [Enable-PSRemoting](https://technet.microsoft.com/library/hh849694.aspx) e [Enable-WSManCredSSP](https://technet.microsoft.com/library/hh849872.aspx).

Configurare quindi il computer che verrà usato per gestire l'host Hyper-V.

1. Aprire una sessione di Windows PowerShell come amministratore.
1. Eseguire questi comandi:

     ```
     Set-Item WSMan:\localhost\Client\TrustedHosts -Value "fqdn-of-hyper-v-host"
     ```
     ```
     Enable-WSManCredSSP -Role client -DelegateComputer "fqdn-of-hyper-v-host"
     ```
1. Potrebbe anche essere necessario configurare i criteri di gruppo seguenti: 
    * **Configurazione Computer** \> **modelli amministrativi** \> la **delega delle credenziali** \> del **sistema** \> **consentire la delega di credenziali aggiornate con l'autenticazione server solo NTLM**
    * Fare clic su **Abilita** e aggiungere *WSMan/FQDN-of-Hyper-v-host*.
1. Aprire **Hyper-V Manager**.
1. Nel riquadro sinistro fare clic con il pulsante destro del mouse su console di **gestione di Hyper-V**.
1. Fare clic su **Connetti al server**.

>[!NOTE]
> Questa operazione funzionerà solo per gli host **remoti** windows server 2016 o Windows 10.

Per informazioni dettagliate sui cmdlet, vedere [Set-Item](https://msdn.microsoft.com/powershell/reference/5.1/microsoft.powershell.management/set-item) e [Enable-WSManCredSSP](https://technet.microsoft.com/library/hh849872.aspx).

## <a name="install-hyper-v-manager"></a>Installare la console di gestione di Hyper-V

Per usare uno strumento dell'interfaccia utente, scegliere quello appropriato per il sistema operativo del computer in cui verrà eseguita la console di gestione di Hyper-V:

In Windows Server aprire Server Manager \> **gestisci** \> **Aggiungi ruoli e funzionalità**. Passare alla pagina **funzionalità** ed espandere **strumenti di amministrazione remota del server** \> strumenti di **amministrazione ruoli** \> **strumenti di gestione di Hyper-V**. 

In Windows, la console di gestione di Hyper-V è disponibile in [qualsiasi sistema operativo Windows che includa Hyper-v](https://msdn.microsoft.com/virtualization/hyperv_on_windows/quick_start/walkthrough_compatibility).

1. Sul desktop di Windows fare clic sul pulsante Start e iniziare a digitare **programmi e funzionalità**. 
1. Nei risultati della ricerca fare clic su **programmi e funzionalità**.
1. Nel riquadro sinistro fare clic **su attivazione o disattivazione delle funzionalità Windows**.
1. Espandere la cartella Hyper-V, quindi **fare clic su strumenti di gestione Hyper-v**.
1. Per installare la console di gestione di Hyper-V, fare clic su **strumenti di gestione Hyper-v**. Se si vuole installare anche il modulo Hyper-V, fare clic su tale opzione.

Per usare Windows PowerShell, eseguire il comando seguente come amministratore:

```
add-windowsfeature rsat-hyper-v-tools
```

## <a name="see-also"></a>Vedere anche  
 
[Installazione di Hyper-V](../get-started/Install-the-Hyper-V-role-on-Windows-Server.md) 

