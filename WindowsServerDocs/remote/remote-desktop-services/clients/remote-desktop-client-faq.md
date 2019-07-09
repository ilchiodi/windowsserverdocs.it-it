---
title: Domande frequenti sui client Desktop remoto
description: Domande frequenti sui client Desktop remoto
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 785a18cf-a5d0-4bc2-95e4-9ef53ee8f65a
author: lizap
manager: dongill
ms.author: elizapo
ms.date: 07/16/2018
ms.localizationpriority: medium
ms.openlocfilehash: e6f91aa02cd0f19d480c24309be5797c273b0f2e
ms.sourcegitcommit: 3743cf691a984e1d140a04d50924a3a0a19c3e5c
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 06/17/2019
ms.locfileid: "66804954"
---
# <a name="frequently-asked-questions-about-the-remote-desktop-clients"></a>Domande frequenti sui client Desktop remoto

>Si applica a: Windows 10, Windows 8.1, Windows Server 2019, Windows Server 2016, Windows Server 2012 R2

Ora che hai impostato il client Desktop remoto nel tuo dispositivo (Android, Mac, iOS o Windows), potresti avere la necessità di risolvere alcuni dubbi. Di seguito sono riportate le risposte alle domande più frequenti sui client Desktop remoto. 

- [Configurazione](#setting-up)
- [Connessioni, gateway e reti](#connection-gateway-and-networks)
- [Client Web](#web-client)
- [Monitor, audio e mouse](#monitors-audio-and-mouse)
- [Hardware Mac](#mac-client---hardware-questions)
- [Messaggi di errore specifici](#specific-errors)

La maggior parte di queste domande riguarda tutti i client, ma alcune sezioni sono più specifiche.

Se hai altre domande per cui desideri una risposta, inviale come feedback su questo articolo.

## <a name="setting-up"></a>Impostazione di

### <a name="which-pcs-can-i-connect-to"></a>Dei PC che è possibile connettersi a

Estrarre il [configurazione supportata](remote-desktop-supported-config.md) per informazioni su quali computer è possibile connettersi.

### <a name="how-do-i-set-up-a-pc-for-remote-desktop"></a>Come posso configurare un computer per Desktop remoto?

Il dispositivo è configurato, ma non credo che il PC sia pronto. Come devo procedere?

Hai visto innanzitutto Remote Desktop Setup Wizard (Configurazione guidata Desktop remoto)? Consente di eseguire in modo semplificato i passaggi necessari per preparare il PC per l'accesso remoto. Scarica ed esegui questo strumento nel PC per avere tutto impostato automaticamente. 

Se invece preferisci procedere manualmente, continua a leggere.

Per Windows 10, eseguire le operazioni seguenti:

1. Sul dispositivo a cui vuoi connetterti apri **Impostazioni**.
2. Seleziona **Sistema** e quindi **Desktop remoto**.
3. Usa il dispositivo di scorrimento per abilitare Desktop remoto.
4. In generale è consigliabile mantenere il PC attivo e individuabile per facilitare le connessioni. Fai clic su **Mostra impostazioni** per accedere alle impostazioni di risparmio energia del PC, dove puoi modificare questa impostazione.
   > [!NOTE]
   > Non puoi connetterti a un PC in modalità sospensione o ibernazione, quindi verifica che le impostazioni di sospensione e ibernazione nel PC remoto siano impostate su **Mai**. L'ibernazione non è disponibile in tutti i PC.


Prendi nota del nome del PC visualizzato in **How to connect to this PC** (Come connettersi al PC). Tale nome sarà necessario per configurare i client.

Puoi concedere a utenti specifici l'autorizzazione per accedere al PC. A tale scopo, fai clic su **Select users that can remotely access this PC** (Seleziona gli utenti che possono accedere in remoto al PC).
I membri del gruppo Administrators hanno accesso automaticamente.

Per Windows 8.1, seguire le istruzioni per consentire le connessioni remote in [connettersi a un altro desktop mediante connessioni Desktop remoto](https://support.microsoft.com/en-us/help/17463/windows-7-connect-to-another-computer-remote-desktop-connection#1TC=windows-8).



## <a name="connection-gateway-and-networks"></a>Connessione gateway e reti

### <a name="why-cant-i-connect-using-remote-desktop"></a>Perché non riesco a connettermi tramite Desktop remoto?

Ecco alcune possibili soluzioni a problemi comuni che possono verificarsi quando si tenta di connettersi a un computer remoto. Se queste soluzioni non funzionano, è possibile trovare ulteriori informazioni sul [sito Web Community Microsoft](https://go.microsoft.com/fwlink/p/?LinkId=242079).

- **Impossibile trovare il PC remoto.** Verifica di avere il nome del PC corretto e quindi controlla di averlo immesso correttamente. Se ancora non riesci a connetterti, prova a usare l'indirizzo IP anziché il nome del PC remoto.
- **Si è verificato un problema con la rete.** Verifica di avere la connessione Internet. 
- **La porta di Desktop remoto potrebbe essere bloccata da un firewall.** Se si utilizza Windows Firewall, seguire questi passaggi:

  1. Aprire Windows Firewall. 
  2. Fai clic su **Consenti app o funzionalità attraverso Windows Firewall**. 
  3. Fai clic su **Modifica impostazioni**. È possibile che venga richiesto per una password di amministratore o di confermare la scelta.
  4. In **applicazioni e funzionalità consentiti**, selezionare **Desktop remoto**, e quindi toccare o fare clic su **OK**.

     Se si utilizza un firewall diverso, assicurarsi che la porta per Desktop remoto (in genere la 3389) sia aperta.
- **Le connessioni remote potrebbero non essere configurate nel PC remoto.** Per risolvere il problema, scorri verso l'alto fino alla domanda [Come posso configurare un computer per Desktop remoto?](#how-do-i-set-up-a-pc-for-remote-desktop) in questo argomento.
- **Il PC remoto potrebbe consentire la connessione solo da parte di PC in cui è impostata l'Autenticazione a livello di rete.** 
- **Il PC remoto potrebbe essere spento.** Non è possibile connettersi a un computer è spento, nello stato di sospensione o ibernazione, quindi verificare che le impostazioni di sospensione e ibernazione nel computer remoto è impostate su **mai** (modalità di sospensione non è disponibile in tutti i PC.).

### <a name="why-cant-i-find-or-connect-to-my-pc"></a>Impossibile trovare o connettersi al computer.

Verificare quanto segue:

- Il PC è acceso e attivo?
- Hai immesso il nome o l'indirizzo IP corretto?

   > [!IMPORTANT]
   > Se usi il nome del PC, la rete deve risolvere correttamente tale nome mediante DNS. In molte reti domestiche, è necessario utilizzare l'indirizzo IP anziché il nome host per la connessione.
- Il PC si trova su una rete diversa? Hai configurato il PC affinché consenta le connessioni dall'esterno?  Per assistenza, vedi [Consentire l'accesso al PC dall'esterno della rete](remote-desktop-allow-outside-access.md).
- Ti stai connettendo a una versione supportata di Windows? 

   > [!NOTE]
   > Windows XP Home Edition, Windows Media Center Edition, Windows Vista e Windows 7 Home o Starter non sono supportate senza 3 software di terze parti.

### <a name="why-cant-i-sign-in-to-a-remote-pc"></a>Perché non posso accedere a un computer remoto?

Se è possibile visualizzare la schermata di accesso del PC remoto, ma non è possibile accedere, è possibile che non sono state aggiunte al gruppo utenti Desktop remoto o a qualsiasi gruppo con diritti di amministratore nel computer remoto. Chiedere all'amministratore di sistema per eseguire questa operazione.

### <a name="which-connection-methods-are-supported-for-company-networks"></a>Sono supportati i metodi di connessione per reti aziendali?

Se si desidera accedere al desktop di office dall'esterno della rete aziendale, la società deve fornire un mezzo di accesso remoto. Il Client desktop remoto supporta attualmente le operazioni seguenti:

- Gateway di Terminal Server o Gateway Desktop remoto
- Accesso Web Desktop remoto
- VPN (tramite opzioni VPN incorporate iOS)

### <a name="vpn-doesnt-work"></a>VPN non funziona

Problemi VPN possono avere cause diverse. Il primo passaggio consiste nel verificare che la connessione VPN funziona nella stessa rete del computer Mac o PC. Se non è possibile testare con un PC o Mac, è possibile accedere a una pagina web intranet aziendale con il browser del dispositivo.

Controllare gli altri elementi:
- **La rete 3G blocca o interrompe il funzionamento della VPN.** Esistono diversi provider 3G nel mondo che sembrano danneggiati traffico 3G o blocco. Verificare la connettività VPN funziona correttamente per più di un minuto.
- **VPN L2TP o PPTP.** Se si utilizza nella VPN L2TP o PPTP, impostare inviare tutto il traffico su ON nella configurazione VPN.
- **La VPN non è configurata correttamente.** Una configurazione errata del server VPN può essere il motivo le connessioni VPN mai lavorano o ha smesso di funzionare dopo un certo tempo. Assicurarsi che il test con iOS, web browser del dispositivo o un PC o Mac nella stessa rete in questo caso.

### <a name="how-can-i-test-if-vpn-is-working-properly"></a>Come posso verificare se funziona correttamente VPN?

Verificare che sia abilitata la VPN nel dispositivo. Passare a una pagina Web nella rete interna o utilizzando un servizio web disponibile solo tramite la connessione VPN, è possibile testare la connessione VPN.

### <a name="how-do-i-configure-l2tp-or-pptp-vpn-connections"></a>Configurazione delle connessioni VPN PPTP o L2TP

Se si utilizza L2TP o PPTP nella VPN, assicurarsi di impostare **Invia tutto il traffico** a **via** nella configurazione VPN.

## <a name="web-client"></a>Client Web

### <a name="which-browsers-can-i-use"></a>Quali browser posso usare?

Il client Web supporta Microsoft Edge, Internet Explorer 11, Mozilla Firefox (v55.0 e versioni successive), Safari e Google Chrome.

### <a name="what-pcs-can-i-use-to-access-the-web-client"></a>Quali PC posso usare per accedere al client Web?

Il client Web supporta Windows, macOS, Linux e ChromeOS. I dispositivi mobili attualmente non sono supportati.

### <a name="can-i-use-the-web-client-in-a-remote-desktop-deployment-without-a-gateway"></a>Posso usare il client Web in una distribuzione Desktop remoto senza un gateway?

No. Il client necessita di un Gateway Desktop remoto per la connessione. Per altre informazioni, chiedi all'amministratore.

### <a name="does-the-remote-desktop-web-client-replace-the-remote-desktop-web-access-page"></a>Il client Web Desktop remoto sostituisce la pagina Accesso Web Desktop remoto?

No. Il client Web Desktop remoto è ospitato in un URL diverso da quello della pagina Accesso Web Desktop remoto. Puoi usare il client Web oppure la pagina Accesso Web per visualizzare le risorse remote in un browser.

### <a name="can-i-embed-the-web-client-in-another-web-page"></a>Posso incorporare il client Web in un'altra pagina Web?

Questa funzionalità attualmente non è supportata.

## <a name="monitors-audio-and-mouse"></a>Audio, monitor e mouse

### <a name="how-do-i-use-all-of-my-monitors"></a>Come è possibile utilizzare tutti i monitor?
Per utilizzare due o più schermate, eseguire le operazioni seguenti:

1. Fai clic con il pulsante destro del mouse sul desktop remoto per cui vuoi abilitare più schermate e quindi scegli **Modifica**.
2. Abilitare **utilizzare tutti i monitoraggi** e **schermo**.

### <a name="is-bi-directional-sound-supported"></a>Audio bidirezionale è supportata?
Audio a monte (dal client al server, per microfoni) non è supportata dal Client Desktop remoto.

### <a name="what-can-i-do-if-the-sound-wont-play"></a>Cosa può fare se non riproduce il suono?
Disconnette la sessione (non solo disconnettere, disconnettersi completamente) e quindi accedere di nuovo.

## <a name="mac-client---hardware-questions"></a>Client Mac - Domande relative all'hardware
### <a name="is-retina-resolution-supported"></a>Risoluzione retina è supportata?
Sì, il client desktop remoto supporta la risoluzione retina.

### <a name="how-do-i-enable-secondary-right-click"></a>Come è possibile abilitare rapida secondario?
Per utilizzare il pulsante destro del mouse all'interno di una sessione aperta sono disponibili tre opzioni:

- Mouse USB a standard PC due pulsanti
- Apple Magic Mouse: per abilitare il clic con il pulsante destro del mouse, fai clic su **Preferenze di Sistema** nel Dock, fai clic su **Mouse** e quindi abilita **Clic secondario**.
- Apple Magic Trackpad o MacBook Trackpad: per abilitare il clic con il pulsante destro del mouse, fai clic su **Preferenze di Sistema** nel Dock, fai clic su **Mouse** e quindi abilita **Clic secondario**.

### <a name="is-airprint-supported"></a>AirPrint è supportato?
No, il client Desktop remoto non supporta AirPrint. Questo vale per i client sia Mac che iOS.

### <a name="why-do-incorrect-characters-appear-in-the-session"></a>Perché inclusi caratteri non validi vengono visualizzati nella sessione?
Se si utilizza una tastiera internazionale, è possibile visualizzare un problema in cui i caratteri visualizzati nella sessione corrisponde ai caratteri digitato sulla tastiera Mac.

Ciò può verificarsi nei seguenti scenari:

- Si sta utilizzando una tastiera che non riconosce la sessione remota. Se Desktop remoto non riconosce la tastiera, per impostazione predefinita la lingua utilizzata l'ultima volta con il computer remoto.
- Ci si connette a una sessione disconnessa in precedenza in un computer remoto e che PC remoto utilizza una lingua di tastiera diversa da quella di attualmente si sta tentando di utilizzare.

È possibile risolvere questo problema impostando manualmente la lingua della tastiera per la sessione remota. Vedi la procedura nella sezione successiva.

### <a name="how-do-language-settings-affect-keyboards-in-a-remote-session"></a>In che modo le impostazioni della lingua influiscono sulle tastiere in una sessione remota?
Esistono molti tipi di layout di tastiera Mac. Alcune di queste sono layout specifici Mac o layout personalizzati per il quale una corrispondenza esatta potrebbe non essere disponibile nella versione di Windows si è .NET remoting in. La sessione remota esegue il mapping tra la tastiera e la migliore lingua di tastiera corrispondente disponibile nel PC remoto. 

Se il layout di tastiera Mac è impostato su dovrebbe funzionare solo alla versione PC di tutte le chiavi devono essere mappate correttamente la tastiera nella lingua (ad esempio, francese-PC) e la tastiera.

Se è impostato il layout di tastiera Mac alla versione Mac di una tastiera (ad esempio, francese) la sessione remota verrà eseguito il mapping è alla versione PC di lingua francese. Alcune delle scelte rapide da tastiera Mac che sei abituato a usare in OSX non funzioneranno nella sessione Windows remota.

Se il layout di tastiera è impostato su una variante di una lingua (ad esempio, francese (Canada)) e la sessione remota non può associare tale variazione esatta, la sessione remota verrà eseguito il mapping è il linguaggio più vicino (ad esempio, francese). Alcune delle scelte rapide da tastiera Mac che sei abituato a usare in OSX non funzioneranno nella sessione Windows remota.

Se il layout di tastiera è impostato su un layout che della sessione remota non può corrispondere a tutti, verrà automaticamente la sessione remota per fornire la lingua che è utilizzata l'ultima volta con il PC. In questo caso, o nei casi in cui è necessario modificare la lingua della sessione remota in modo che corrisponda tastiera Mac, è possibile impostare manualmente la lingua della tastiera nella sessione remota per la lingua che è la corrispondenza più vicina a quella che si desidera utilizzare come indicato di seguito.

Utilizzare le istruzioni seguenti per modificare il layout di tastiera all'interno della sessione di desktop remoto:

**In Windows 10 o Windows 8:**

1. All'interno della sessione remota, aprire paese e lingua. Fare clic su **Start > Impostazioni > ora e la lingua**. Aprire **paese e lingua**.
2. Aggiungere la lingua che si desidera utilizzare. Quindi chiudere la finestra di paese e lingua.
3. A questo punto, nella sessione remota, si noterà la possibilità di passare tra i linguaggi. (Nella parte destra della sessione remota, accanto all'orologio.) Selezionare la lingua che si desidera passare (ad esempio **Eng**).

Potrebbe essere necessario chiudere e riavviare l'applicazione in uso per la tastiera modifica diventino effettive.


## <a name="specific-errors"></a>Errori specifici

### <a name="why-do-i-get-an-insufficient-privileges-error"></a>Perché viene visualizzato un errore "Privilegi insufficienti"?
Non è consentito accedere alla sessione in cui che si desidera connettersi. La causa più probabile è che si sta tentando di connettersi a una sessione di amministrazione. Solo gli amministratori sono autorizzati a connettersi alla console. Verificare che la console è disattiva nelle impostazioni avanzate di desktop remoto. Se non è l'origine del problema, contattare l'amministratore di sistema per ulteriore assistenza.

### <a name="why-does-the-client-say-that-there-is-no-cal"></a>Perché il client si supponga che non vi sia alcuna licenza CAL
Quando un client di desktop remoto si connette a un server di Desktop remoto, il server rilascia un Remote Desktop Services licenza di accesso Client (CAL Servizi Desktop REMOTO) archiviato dal client. Ogni volta che il client si connette nuovamente verranno utilizzate le CAL RDS e il server non creerà un'altra licenza. Se le Licenze CAL sul dispositivo è mancante o danneggiato, il server rilascerà un'altra licenza. Quando viene raggiunto il numero massimo di dispositivi con licenza server non creerà nuove licenze CAL Servizi Desktop REMOTO. Per assistenza, contattare l'amministratore di rete.

### <a name="why-did-i-get-an-access-denied-error"></a>Perché viene visualizzato un errore "Accesso negato"?
L'errore "Accesso negato" è un oggetto generato dal Gateway Desktop remoto e il risultato di credenziali non corrette durante il tentativo di connessione. Verificare il nome utente e password. Se la connessione ha lavorato prima e l'errore si è verificato di recente, possibilmente modificato la password dell'account utente Windows e non ancora aggiornate nelle impostazioni di desktop remote.

### <a name="what-does-rpc-error-23014-or-error-0x59e6-mean"></a>Che cosa significa "Errore RPC 23014" o Media "Errore 0x59e6"?
In caso di un **errore RPC 23014** o **errore 0x59E6 riprovare dopo alcuni minuti**, il server Gateway Desktop remoto ha raggiunto il numero massimo di connessioni attive. Il numero massimo di connessioni varia a seconda della versione di Windows in esecuzione nel Gateway Desktop remoto: l'implementazione di Windows Server 2008 R2 Standard limita il numero di connessioni a 250. L'implementazione di Windows Server 2008 R2 Foundation limita il numero di connessioni a 50. Tutte le altre implementazioni di Windows consentono un numero illimitato di connessioni.

### <a name="what-does-the-failed-to-parse-ntlm-challenge-error-mean"></a>Qual è il significato dell'errore "Failed to parse NTLM challenge" (Impossibile analizzare la richiesta di verifica NTLM)?
Questo errore è causato da una configurazione errata nel PC remoto. Verifica che l'impostazione del livello di sicurezza RDP nel PC remoto sia configurata su "Compatibile con client". Rivolgiti all'amministratore di sistema se hai bisogno di aiuto per eseguire questa operazione.

### <a name="what-does-tsrap-you-are-not-allowed-to-connect-to-the-given-host-mean"></a>Qual è il significato dell'errore "TS_RAP You are not allowed to connect to the given host" (TS_RAP Non è consentito connettersi all'host specificato)?
Questo errore si verifica quando un criterio di autorizzazione delle risorse nel server gateway impedisce al tuo nome utente di connettersi al PC remoto. Ciò può accadere nei casi seguenti:

- Il nome del PC remoto è identico al nome del gateway. Quindi, quando provi a connetterti al PC remoto, la connessione viene invece stabilita con il gateway, al quale probabilmente non sei autorizzato ad accedere. Se devi connetterti al gateway, non usare il nome gateway esterno come nome del PC. Usa invece "localhost" o l'indirizzo IP (127.0.0.1) oppure il nome server interno.
- Il tuo account utente non è membro del gruppo di utenti per l'accesso remoto.