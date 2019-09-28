---
title: Gestione di Server dei criteri di rete con gli strumenti di amministrazione
description: È possibile utilizzare questo argomento per ottenere informazioni sugli strumenti che è possibile utilizzare per gestire server dei criteri di rete in Windows Server 2016.
manager: brianlic
ms.prod: windows-server
ms.technology: networking
ms.topic: article
ms.assetid: 5de80dc0-53be-42b7-8e5b-24d213bf2b25
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 527fbf52d68f36d198068514476868bcba930a68
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71396447"
---
# <a name="network-policy-server-management-with-administration-tools"></a>Gestione di Server dei criteri di rete con gli strumenti di amministrazione

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016

È possibile utilizzare questo argomento per ottenere informazioni sugli strumenti che è possibile utilizzare per gestire i NPSs.

Dopo aver installato NPS, è possibile amministrare NPSs:

- Localmente, tramite il server dei criteri di rete Microsoft Management Console \(MMC @ no__t-1 snap-in, la console di NPS statica in strumenti di amministrazione, i comandi di Windows PowerShell o i comandi della shell di rete \(Netsh @ no__t-3 per NPS.
- Da un server dei criteri di ricerca remoto, utilizzando lo snap-in MMC NPS, i comandi Netsh per NPS, i comandi di Windows PowerShell per NPS o Connessione Desktop remoto.
- Da una workstation remota, usando Connessione Desktop remoto in combinazione con altri strumenti, ad esempio il server dei criteri di console o Windows PowerShell.

>[!NOTE]
>In Windows Server 2016 è possibile gestire il server dei criteri di server locale usando la console NPS. Per gestire NPSs sia remote che locali, è necessario usare lo snap-in MMC NPS @ no__t-0cm.

Le sezioni seguenti forniscono istruzioni su come gestire la NPSs locale e remota.

## <a name="configure-the-local-nps-by-using-the-nps-console"></a>Configurare il server dei criteri di criteri locale usando la console NPS

Al termine dell'installazione di NPS, è possibile utilizzare questa procedura per gestire il server dei criteri di server locale utilizzando la MMC NPS.

**Credenziali amministrative** 

Per completare questa procedura, è necessario essere un membro del gruppo Administrators.

### <a name="to-configure-the-local-nps-by-using-the-nps-console"></a>Per configurare il server dei criteri di criteri locale utilizzando la console NPS

1. In Server Manager, fare clic su **strumenti**, quindi fare clic su **Server dei criteri di rete**. Si apre la console NPS.

2. Nella console NPS fare clic su NPS \(Local @ no__t-1. Nel riquadro dei dettagli scegliere **configurazione standard** o **configurazione avanzata**, quindi eseguire una delle operazioni seguenti in base alla selezione:
    - Se si sceglie **configurazione standard**, selezionare uno scenario dall'elenco e quindi seguire le istruzioni per avviare una configurazione guidata.
    - Se si sceglie **configurazione avanzata**, fare clic sulla freccia per espandere **Opzioni di configurazione avanzate**, quindi rivedere e configurare le opzioni disponibili in base alla funzionalità server dei criteri di server che si desidera: server RADIUS, proxy RADIUS o entrambi.

## <a name="manage-multiple-npss-by-using-the-nps-mmc-snap-in"></a>Gestire più NPSs usando il server dei criteri di console MMC snap @ no__t-0cm

È possibile utilizzare questa procedura per gestire il server dei criteri di server locale e più NPSs remoti utilizzando il server NPS MMC snap @ no__t-0cm.

Prima di eseguire la procedura riportata di seguito, è necessario installare NPS nel computer locale e nei computer remoti.

A seconda delle condizioni di rete e del numero di NPSs gestiti tramite il server dei criteri di rete MMC snap @ no__t-0cm, la risposta dello snap-in MMC @ no__t-1in potrebbe essere lenta. Inoltre, il traffico di configurazione NPS viene inviato in rete durante una sessione di amministrazione remota utilizzando il server dei criteri di rete snap @ no__t-0cm. Assicurarsi che la rete sia fisicamente sicura e che gli utenti malintenzionati non abbiano accesso al traffico di rete.

**Credenziali amministrative** 

Per completare questa procedura, è necessario essere un membro del gruppo Administrators.

### <a name="to-manage-multiple-npss-by-using-the-nps-snap-in"></a>Per gestire più NPSs usando lo snap NPS @ no__t-0cm

1. Per aprire MMC, eseguire Windows PowerShell come amministratore. In Windows PowerShell digitare **MMC**, quindi premere INVIO. Verrà visualizzata la finestra di Microsoft Management Console.
2. In MMC scegliere **Aggiungi/Rimuovi snap @ no__t-2in**dal menu **file** . Verrà visualizzata la finestra di dialogo **Aggiungi o Rimuovi snap @ no__t-1ins** .
3. In **Aggiungi o Rimuovi snap @ no__t-1ins**, in **snap-in disponibili @ no__t-3ins**scorrere verso il basso nell'elenco, fare clic su **Server dei criteri di rete**e quindi su **Aggiungi**. Viene visualizzata la finestra di dialogo **Seleziona computer**.
4. In **Seleziona computer**verificare che sia selezionata **l'opzione computer locale \(Il in cui è in esecuzione la console @ no__t-3** , quindi fare clic su **OK**. Lo snap @ no__t-0cm per il server dei criteri di server locale viene aggiunto all'elenco nello **snap @ no__t-2Ins selezionato**.
5. In **Aggiungi o Rimuovi snap @ no__t-1ins**, in **snap @ no__t-3ins**, assicurarsi che **Server dei criteri di rete** sia ancora selezionato, quindi fare clic su **Aggiungi**. Verrà nuovamente visualizzata la finestra di dialogo **Seleziona computer** .
6. In **Seleziona computer**, fare clic su **un altro computer**, quindi digitare l'indirizzo IP o il nome di dominio completo \(FQDN @ NO__T-3 del server dei criteri di dominio remoto che si desidera gestire utilizzando lo snap NPS @ no__t-4in. Facoltativamente, è possibile fare clic su **Sfoglia** per esaminare la directory del computer che si desidera aggiungere. Fare clic su **OK**.
7. Ripetere i passaggi 5 e 6 per aggiungere altri NPSs allo snap-in NPS @ no__t-0cm. Dopo aver aggiunto tutti i NPSs che si desidera gestire, fare clic su **OK**.
8. Per salvare lo snap-in NPS per utilizzarlo in un secondo momento, fare clic su **file**, quindi fare clic su **Salva**. Nella finestra di dialogo **Salva con** nome individuare il percorso del disco rigido in cui si desidera salvare il file, digitare un nome per il file di Microsoft Management Console @no__t 1. msc @ no__t-2, quindi fare clic su **Salva**. 

## <a name="manage-an-nps-by-using-remote-desktop-connection"></a>Gestire un server dei criteri di server utilizzando Connessione Desktop remoto

È possibile utilizzare questa procedura per gestire un server dei criteri di server remoto utilizzando Connessione Desktop remoto.

Con Connessione Desktop remoto è possibile gestire in remoto i NPSs che eseguono Windows Server 2016. È anche possibile gestire in remoto NPSs da un computer che esegue Windows 10 o sistemi operativi client Windows precedenti.

È possibile usare Desktop remoto connessione per gestire più NPSs usando uno dei due metodi.

1. Creare una connessione Desktop remoto a ogni singolo NPSs.
2. Utilizzare Desktop remoto per connettersi a un server dei criteri di server, quindi utilizzare la MMC NPS sul server per gestire altri server remoti. Per ulteriori informazioni, vedere la sezione precedente **gestire più NPSs utilizzando il server dei criteri di ricerca MMC snap @ no__t-1in**.

**Credenziali amministrative** 

Per completare questa procedura, è necessario essere un membro del gruppo Administrators nel server dei criteri di gruppo.

### <a name="to-manage-an-nps-by-using-remote-desktop-connection"></a>Per gestire un server dei criteri di server utilizzando Connessione Desktop remoto

1. In ogni server dei criteri di server che si desidera gestire in remoto, in Server Manager, selezionare **server locale**. Nel riquadro Dettagli Server Manager visualizzare l'impostazione di **Desktop remoto** ed eseguire una delle operazioni seguenti. 
    1. Se il valore dell'impostazione **Desktop remoto** è **abilitato**, non è necessario eseguire alcuni passaggi di questa procedura. Passare al passaggio 4 per avviare la configurazione di Desktop remoto autorizzazioni utente.
    2. Se l'impostazione **Desktop remoto** è **disabilitata**, fare clic sulla parola **disabilitata**. Verrà visualizzata la finestra di dialogo **proprietà del sistema** nella scheda **Remote** .
2. In **Desktop remoto**fare clic su **Consenti connessioni remote al computer**. Verrà visualizzata la finestra di dialogo **Connessione desktop remoto** . Effettuare una delle operazioni seguenti.
    1. Per personalizzare le connessioni di rete consentite, fare clic su **Windows Firewall con sicurezza avanzata**e quindi configurare le impostazioni che si desidera consentire. 
    2. Per abilitare Connessione Desktop remoto per tutte le connessioni di rete nel computer, fare clic su **OK**.
3. In **proprietà di sistema**, in **Desktop remoto**decidere se abilitare **Consenti connessioni solo da computer che eseguono Desktop remoto con autenticazione a livello di rete**ed effettuare la selezione.
4. Fare clic su **selezionare utenti**. Verrà visualizzata la finestra di dialogo **Desktop remoto utenti** .
5. In **Desktop remoto utenti**, per concedere l'autorizzazione a un utente per la connessione remota al server dei criteri di server, fare clic su **Aggiungi**e quindi digitare il nome utente per l'account dell'utente. Fare clic su **OK**.
6. Ripetere il passaggio 5 per ogni utente per il quale si desidera concedere l'autorizzazione di accesso remoto al server dei criteri di accesso. Al termine dell'aggiunta di utenti, fare clic su **OK** per chiudere la finestra di dialogo **Desktop remoto utenti** e quindi di nuovo su **OK** per chiudere la finestra di dialogo **Proprietà sistema** .
7. Per connettersi a un server dei criteri di configurazione remoto configurato usando i passaggi precedenti, fare clic sul pulsante **Start**, scorrere l'elenco alfabetico e quindi fare clic su **Accessori Windows**e quindi su **Connessione desktop remoto**. Verrà visualizzata la finestra di dialogo **Connessione desktop remoto** .
8. Nella finestra di dialogo **Connessione desktop remoto** , in **computer**, digitare il nome o l'indirizzo IP del server dei criteri di server. Se si preferisce, fare clic su **Opzioni**, configurare opzioni di connessione aggiuntive e quindi fare clic su **Salva** per salvare la connessione per utilizzarlo ripetutamente.
9. Fare clic su **Connetti**e, quando richiesto, fornire le credenziali dell'account utente per un account che dispone delle autorizzazioni per accedere a e configurare NPS.

## <a name="use-netsh-nps-commands-to-manage-an-nps"></a>Usare i comandi netsh NPS per gestire un server dei criteri di server

È possibile usare i comandi nel contesto netsh NPS per visualizzare e impostare la configurazione del database di autenticazione, autorizzazione, contabilità e controllo usato sia da server dei criteri di gruppo che dal servizio di accesso remoto. Usare i comandi nel contesto netsh NPS per:

- Configurare o riconfigurare un server dei criteri di server, inclusi tutti gli aspetti di NPS disponibili anche per la configurazione tramite la console NPS nell'interfaccia di Windows.
- Esportare la configurazione di un server dei criteri di server (il server di origine), incluse le chiavi del registro di sistema e l'archivio di configurazione NPS, come script Netsh.
- Importare la configurazione in un altro server dei criteri di server usando uno script Netsh e il file di configurazione esportato dal server dei criteri di origine.

È possibile eseguire questi comandi dal prompt dei comandi di Windows Server 2016 o da Windows PowerShell. È anche possibile eseguire comandi netsh NPS in script e file batch.

**Credenziali amministrative** 

Per eseguire questa procedura, è necessario essere membri del gruppo Administrators nel computer locale.

### <a name="to-enter-the-netsh-nps-context-on-an-nps"></a>Per immettere il contesto netsh NPS in un server dei criteri di server

1. Aprire il prompt dei comandi o Windows PowerShell.
2. Digitare **netsh**, quindi premere INVIO.
3. Digitare **NPS**, quindi premere INVIO.
4. Per visualizzare un elenco di comandi disponibili, digitare un punto interrogativo \(? \) e premere INVIO.


Per ulteriori informazioni sui comandi netsh NPS, vedere [comandi Netsh per server dei criteri di rete in Windows Server 2008](https://technet.microsoft.com/library/cc754428(v=ws.10).aspx)oppure scaricare l'intero [riferimento tecnico di netsh](https://gallery.technet.microsoft.com/Netsh-Technical-Reference-c46523dc?redir=0) dalla raccolta TechNet. Questo download è il riferimento tecnico completo della shell di rete per Windows Server 2008 e Windows Server 2008 R2. Il formato è la Guida di Windows \( *. chm @ no__t-1 in un file zip. Questi comandi sono ancora presenti in Windows Server 2016 e Windows 10, quindi è possibile usare netsh in questi ambienti, anche se è consigliabile usare Windows PowerShell.

## <a name="use-windows-powershell-to-manage-npss"></a>Usare Windows PowerShell per gestire NPSs

È possibile utilizzare i comandi di Windows PowerShell per gestire NPSs. Per ulteriori informazioni, vedere gli argomenti di riferimento sui comandi di Windows PowerShell seguenti.

- [Cmdlet di server dei criteri di rete in Windows PowerShell](https://technet.microsoft.com/library/jj872739(v=wps.630).aspx). È possibile usare questi comandi Netsh in Windows Server 2012 R2 o sistemi operativi successivi.
- [Modulo NPS](https://technet.microsoft.com/itpro/powershell/windows/nps/index). È possibile usare questi comandi Netsh in Windows Server 2016.

Per ulteriori informazioni sull'amministrazione dei server dei criteri di rete, vedere [Manage Network Policy Server (NPS)](nps-manage-top.md).
