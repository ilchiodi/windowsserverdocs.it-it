---
title: Gestione di Server dei criteri di rete con gli strumenti di amministrazione
description: È possibile utilizzare questo argomento per informazioni sugli strumenti che è possibile utilizzare per gestire Server dei criteri di rete in Windows Server 2016.
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 5de80dc0-53be-42b7-8e5b-24d213bf2b25
ms.author: pashort
author: shortpatti
ms.openlocfilehash: cdb82c5bc1c541f19b1b2f4c8db837af4a812f81
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/28/2018
---
# <a name="network-policy-server-management-with-administration-tools"></a>Gestione di Server dei criteri di rete con gli strumenti di amministrazione

>Si applica a: Windows Server (canale annuale e virgola), Windows Server 2016

È possibile utilizzare questo argomento per informazioni sugli strumenti che è possibile utilizzare per gestire i server dei criteri di rete.

Dopo l'installazione dei criteri di rete, è possibile amministrare i server dei criteri di rete:

- In locale, utilizzando lo snap-in Criteri di rete Microsoft Management Console \(MMC\), la console dei criteri di rete statica in strumenti di amministrazione, i comandi di Windows PowerShell o i comandi di Network Shell \(Netsh\) dei criteri di rete.
- Da server dei criteri di rete remoto, utilizzando lo snap-in MMC NPS, i comandi Netsh per criteri di rete, i comandi di Windows PowerShell per criteri di rete o connessione Desktop remoto.
- Da una workstation remota utilizzando connessione Desktop remoto in combinazione con altri strumenti, ad esempio la NPS MMC o Windows PowerShell.

>[!NOTE]
>In Windows Server 2016, è possibile gestire il server dei criteri di rete locale tramite la console dei criteri di rete. Per gestire i server dei criteri di rete locali e remoti, è necessario utilizzare MMC NPS snap-in.

Le sezioni seguenti forniscono istruzioni su come gestire i server dei criteri di rete locali e remoti.

## <a name="configure-the-local-nps-server-by-using-the-nps-console"></a>Configurare il Server dei criteri di rete locale tramite la Console dei criteri di rete

Dopo aver installato dei criteri di rete, è possibile utilizzare questa procedura per gestire il server dei criteri di rete locale usando NPS MMC.

**Credenziali amministrative** 

Per completare questa procedura, è necessario essere un membro del gruppo Administrators.

### <a name="to-configure-the-local-nps-server-by-using-the-nps-console"></a>Per configurare il server dei criteri di rete locale tramite la console dei criteri di rete

1. In Server Manager, fare clic su **strumenti**, quindi fare clic su **Server dei criteri di rete**. Apre la console dei criteri di rete.

2. Nella console di criteri di rete, fare clic su \(Local\) dei criteri di rete. Nel riquadro dei dettagli, scegliere **configurazione Standard** o **Advanced Configuration**, quindi effettuare una delle operazioni seguenti in base alle selezioni:
    - Se si sceglie **configurazione Standard**, selezionare uno scenario dall'elenco e quindi segui le istruzioni per avviare una procedura guidata configurazione.
    - Se si sceglie **Advanced Configuration**, fare clic sulla freccia per espandere **opzioni di configurazione avanzate**, quindi esaminare e configurare le opzioni disponibili basate sulle funzionalità dei criteri di rete che si desidera - server RADIUS, proxy RADIUS o entrambi.

## <a name="manage-multiple-nps-servers-by-using-the-nps-mmc-snap-in"></a>Gestire più server dei criteri di rete usando snap-in MMC Criteri di rete

È possibile utilizzare questa procedura per gestire più server dei criteri di rete remoto e server dei criteri di rete locale con MMC NPS snap-in.

Prima di eseguire la procedura seguente, è necessario installare dei criteri di rete nel computer locale e nei computer remoti.

A seconda delle condizioni della rete e il numero di server dei criteri di rete in che è gestire tramite MMC NPS snap-, potrebbe essere lenta risposta di snap-in MMC. Inoltre, il traffico di configurazione server dei criteri di rete viene inviato attraverso la rete durante una sessione di amministrazione remota utilizzando NPS snap-in. Assicurarsi che la rete è fisicamente sicura e che gli utenti malintenzionati non hanno accesso per il traffico di rete.

**Credenziali amministrative** 

Per completare questa procedura, è necessario essere un membro del gruppo Administrators.

### <a name="to-manage-multiple-nps-servers-by-using-the-nps-snap-in"></a>Per gestire più server dei criteri di rete utilizzando NPS snap-in

1. Per aprire MMC, eseguire Windows PowerShell come amministratore. In Windows PowerShell, digitare **mmc**, quindi premere INVIO. Verrà visualizzata la finestra di Microsoft Management Console.
2. In MMC nel **File** menu, fare clic su **Aggiungi/Rimuovi snap-in**. Il **Aggiungi o Rimuovi snap-in** apre la finestra di dialogo.
3. In **Aggiungi o Rimuovi snap-in**, in **snap-in disponibili**, scorrere verso il basso nell'elenco, fare clic su **Server dei criteri di rete**, quindi fare clic su **Aggiungi**. Il **seleziona Computer** apre la finestra di dialogo.
4. In **seleziona Computer**, verificare che **computer locale \ (il computer in cui questa console è running\)** sia selezionata e quindi fare clic su **OK**. Lo snap-in per il server dei criteri di rete locale viene aggiunto all'elenco in **snap-in selezionato**.
5. In **Aggiungi o Rimuovi snap-in**, in **snap-in disponibili**, assicurarsi che **Server dei criteri di rete** è ancora selezionato, quindi fare clic su **Aggiungi**. Il **seleziona Computer** verrà visualizzata la finestra di dialogo Nuovo.
6. In **seleziona Computer**, fare clic su **un altro computer**, quindi digitare l'indirizzo IP o nome di dominio completo nome \(FQDN\) del server dei criteri di rete remoto che si desidera gestire utilizzando NPS snap-in. Facoltativamente, è possibile fare clic su **Sfoglia** per esplorare la directory per il computer che si desidera aggiungere. Fare clic su **OK**.
7. Ripetere i passaggi 5 e 6 per aggiungere più server dei criteri di rete NPS snap-in. Dopo aver aggiunto tutti i server dei criteri di rete che si desidera gestire, fare clic su **OK**.
8. Per salvare lo snap-in Criteri di rete per un uso successivo, fare clic su **File**, quindi fare clic su **salvare**. Nel **Salva con nome** la finestra di dialogo, selezionare il percorso del disco rigido in cui si desidera salvare il file, digitare un nome per il file \(.msc\) Microsoft Management Console e quindi fare clic su **salvare**. 

## <a name="manage-an-nps-server-by-using-remote-desktop-connection"></a>Gestire un Server dei criteri di rete con connessione Desktop remoto

È possibile utilizzare questa procedura per gestire un server dei criteri di rete remoto utilizzando connessione Desktop remoto.

Utilizzando connessione Desktop remoto, è possibile gestire in remoto i server dei criteri di rete che esegue Windows Server 2016. È possibile gestire anche in modalità remota il server dei criteri di rete da un computer che esegue Windows 10 o versioni precedenti i sistemi operativi client Windows.

È possibile utilizzare connessione Desktop remoto per gestire più server dei criteri di rete utilizzando uno dei due metodi.

1. Creare una connessione Desktop remoto per ogni server dei criteri di rete singolarmente.
2. Utilizzare Desktop remoto per connettersi a un server dei criteri di rete e quindi utilizzare su tale server NPS MMC per gestire altri server remoti. Per ulteriori informazioni, vedere la sezione precedente **gestire più server dei criteri di rete usando snap-in MMC NPS**.

**Credenziali amministrative** 

Per completare questa procedura, è necessario essere un membro del gruppo Administrators nel server dei criteri di rete.

### <a name="to-manage-an-nps-server-by-using-remote-desktop-connection"></a>Per gestire un server dei criteri di rete con connessione Desktop remoto

1. In ogni server dei criteri di rete che si desidera gestire in remoto, in Server Manager, selezionare **Server locale**. Nel riquadro dei dettagli di Server Manager, visualizzare il **Desktop remoto** impostazione ed effettuare una delle operazioni seguenti. 
    1. Se il valore di **Desktop remoto** impostazione è **abilitato**, non è necessario eseguire alcuni dei passaggi di questa procedura. Ignorare verso il basso per il passaggio 4 per avviare la configurazione delle autorizzazioni di utenti Desktop remoto.
    2. Se il **Desktop remoto** impostazione è **disabilitato**, fare clic su **disabilitato**. Il **proprietà del sistema** visualizzata nella finestra di dialogo di **remoto** scheda.
2. In **Desktop remoto**, fare clic su **consentire le connessioni remote al computer**. Il **connessione Desktop remoto** apre la finestra di dialogo. Eseguire una delle operazioni seguenti.
    1. Per personalizzare le connessioni di rete che sono consentite, fare clic su **Windows Firewall con sicurezza avanzata**e quindi configurare le impostazioni che si desidera consentire. 
    2. Per abilitare la connessione Desktop remoto di tutte le connessioni del computer di rete, fare clic su **OK**.
3. In **proprietà del sistema**, in **Desktop remoto**, decidere se abilitare **Consenti connessioni solo dai computer che eseguono Desktop remoto con autenticazione a livello di rete**ed effettuare la selezione.
4. Fare clic su **selezionare gli utenti**. Il **utenti Desktop remoto** apre la finestra di dialogo.
5. In **utenti Desktop remoto**, per concedere l'autorizzazione a un utente di connettersi in modalità remota il server dei criteri di rete, fare clic su **Aggiungi**e quindi digitare il nome utente per l'account dell'utente. Fare clic su **OK**.
6. Ripetere il passaggio 5 per ogni utente per il quale si desidera concedere l'autorizzazione di accesso remoto al server dei criteri di rete. Quando hai finito di aggiunta di utenti, fare clic su **OK** per chiudere la **utenti Desktop remoto** la finestra di dialogo e **OK** per chiudere la **proprietà del sistema** la finestra di dialogo.
7. Per connettersi a un server dei criteri di rete remoto che è stato configurato con i passaggi precedenti, fare clic su **Start**, scorrere verso il basso l'elenco in ordine alfabetico e quindi fare clic su **accessori Windows**e fare clic su **connessione Desktop remoto**. Il **connessione Desktop remoto** apre la finestra di dialogo.
8. Nel **connessione Desktop remoto** della finestra di dialogo **Computer**, digitare il nome del server dei criteri di rete o l'indirizzo IP. Se si preferisce, fare clic su **opzioni**, configurare ulteriori opzioni di connessione e quindi fare clic su **salvare** per salvare la connessione per l'uso ripetuto.
9. Fare clic su **Connetti**e quando viene richiesto immettere le credenziali dell'account utente per un account che disponga delle autorizzazioni per accedere a e configurare il server dei criteri di rete.

## <a name="use-netsh-nps-commands-to-manage-an-nps-server"></a>Utilizzare i comandi Netsh NPS per gestire un Server dei criteri di rete

È possibile utilizzare i comandi nel contesto Netsh NPS per visualizzare e impostare la configurazione dell'autenticazione, autorizzazione, accounting e controllo database utilizzato sia da criteri di rete e il servizio di accesso remoto. Utilizzare i comandi nel contesto Netsh NPS per:

- Configurare o riconfigurare in un server dei criteri di rete, inclusi tutti gli aspetti di criteri di rete che sono anche disponibili per la configurazione tramite la console dei criteri di rete nell'interfaccia di Windows.
- Esportare la configurazione di un server dei criteri di rete (il server di origine), incluse le chiavi del Registro di sistema e archivio di configurazione dei criteri di rete, come script Netsh.
- Importare la configurazione in un altro server dei criteri di rete utilizzando uno script di Netsh e file di configurazione esportato dal server dei criteri di rete di origine.

È possibile eseguire questi comandi di Windows Server 2016 prompt dei comandi di o da Windows PowerShell. È anche possibile eseguire comandi netsh nps in script e file batch.

**Credenziali amministrative** 

Per eseguire questa procedura, è necessario essere un membro del gruppo Administrators nel computer locale.

### <a name="to-enter-the-netsh-nps-context-on-an-nps-server"></a>Per accedere al contesto Netsh NPS in un server dei criteri di rete

1. Aprire il prompt dei comandi o Windows PowerShell.
2. Tipo **netsh**, quindi premere INVIO.
3. Tipo **dei criteri di rete**, quindi premere INVIO.
4. Per visualizzare un elenco di comandi disponibili, digitare \(?\) un punto interrogativo e premere INVIO.


Per ulteriori informazioni sui comandi Netsh NPS, vedere [comandi Netsh per Server dei criteri di rete in Windows Server 2008](https://technet.microsoft.com/library/cc754428(v=ws.10).aspx), oppure scaricare l'intero [riferimento tecnico su Netsh](https://gallery.technet.microsoft.com/Netsh-Technical-Reference-c46523dc?redir=0) dalla TechNet Gallery. Questo download è la rete Shell tecniche riferimento per Windows Server 2008 e Windows Server 2008 R2 completo. Il formato è \(*.chm\) Guida di Windows in un file zip. Questi comandi sono ancora presenti in Windows Server 2016 e Windows 10, quindi è possibile usare netsh in questi ambienti, anche se è consigliabile utilizzare Windows PowerShell.

## <a name="use-windows-powershell-to-manage-nps-servers"></a>Utilizzare Windows PowerShell per gestire i server dei criteri di rete

È possibile utilizzare i comandi di Windows PowerShell per gestire i server dei criteri di rete. Per ulteriori informazioni, vedere gli argomenti di riferimento di comando Windows PowerShell seguenti.

- [Cmdlet di Server dei criteri di criteri in Windows PowerShell di rete](https://technet.microsoft.com/library/jj872739(v=wps.630).aspx). È possibile utilizzare questi comandi netsh di Windows Server 2012 R2 o versioni successive.
- [Modulo dei criteri di rete](https://technet.microsoft.com/itpro/powershell/windows/nps/index). È possibile utilizzare questi comandi netsh di Windows Server 2016.

Per ulteriori informazioni sull'amministrazione dei criteri di rete, vedere [gestire Server criteri di rete (NPS)](nps-manage-top.md).
