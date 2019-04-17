---
title: Client Desktop remoto domande frequenti
description: Domande frequenti sul client Desktop remoto
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
ms.openlocfilehash: ec1b0a17c578f2d8ac55d1704af6b267b6bb8e5c
ms.sourcegitcommit: d5f10c0c98a9976a86be9f4fa8866650c7fcfc1a
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/12/2019
ms.locfileid: "9065948"
---
# Domande frequenti sul client Desktop remoto

>Si applica a: Windows 10, Windows 8.1, Windows Server 2012 R2, Windows Server 2016

Ora che hai configurato il client Desktop remoto nel dispositivo (Android, Mac, iOS o Windows), potresti avere domande. Di seguito sono riportate le risposte alle domande più frequenti sui client Desktop remoto. 

- [Configurazione](#Setting-up)
- [Le connessioni, gateway e reti](#connection-gateway-and-networks)
- [Client Web](#web-client)
- [Monitor, audio e mouse](#monitors-audio-and-mouse)
- [Hardware Mac](#mac-client---hardware-questions)
- [Messaggi di errore specifici](#specific-errors)

La maggior parte di queste domande si applicano a tutti i client, ma ci sono alcuni elementi specifici di client.

Se hai altre domande che vorresti noi rispondere alle, lasciarli come feedback in questo articolo.

## Configurazione

### I PC che è possibile connettersi a?

Vedi l'articolo per informazioni su quali PC puoi connetterti a [configurazione supportata](remote-desktop-supported-config.md) .

### Come configurare un PC per Desktop remoto?

Ho configurare il mio dispositivo, ma non credo ready del PC. aiuto?

Prima di tutto, hai visto l'installazione guidata di Desktop remoto? Illustra preparativi per l'accesso remoto nel PC. Download e l'esecuzione che lo strumento nel tuo PC per tutti gli elementi impostato. 

In caso contrario, se preferisci eseguire operazioni manualmente, continua a leggere.

Per Windows 10, le operazioni seguenti:

1. Sul dispositivo che si desidera connettersi, aprire **Impostazioni**.
2. Seleziona **sistema** e quindi **Desktop remoto**.
3. Usare il dispositivo di scorrimento per abilitare Desktop remoto.
4. In generale, è consigliabile mantenere il PC attivo e individuabile per facilitare le connessioni. Fai clic su **Mostra le impostazioni** per passare alle impostazioni di risparmio energia per il tuo PC in cui è possibile modificare questa impostazione.
   > [!NOTE]
   > Non puoi connetterti a un PC è in stato di sospensione o ibernazione, quindi verifica che le impostazioni per la sospensione e ibernazione nel PC remoto siano impostate su **mai**. (Ibernazione non è disponibile in tutti i PC).


Prendere nota del nome di questo PC in **come connettersi a questo PC**. È necessario per configurare i client.

Puoi concedere l'autorizzazione per utenti specifici accedere a questo PC - a tale scopo, fai clic su **Seleziona gli utenti che possono accedere in remoto a questo PC**.
I membri del gruppo Administrators hanno automaticamente l'accesso.

Per Windows 8.1, segui le istruzioni per consentire le connessioni remote in [connessione a un altro desktop mediante le connessioni Desktop remoto](https://support.microsoft.com/en-us/help/17463/windows-7-connect-to-another-computer-remote-desktop-connection#1TC=windows-8).



## Connessione, gateway e reti

### Perché non riesco a connettermi tramite Desktop remoto?

Ecco alcuni possibili soluzioni ai problemi comuni che possono verificarsi quando si tenta di connettersi a un PC remoto. Se queste soluzioni non funzionano, è possibile trovare ulteriori informazioni sul [sito Web Microsoft Community](https://go.microsoft.com/fwlink/p/?LinkId=242079).

- **Non è possibile trovare il PC remoto.** Assicurati che puoi avere il nome del PC a destra e quindi verificare se tale nome è stato immesso correttamente. Se è comunque possibile connettersi, prova a usare l'indirizzo IP del PC remoto anziché il nome del PC.
- **Si verifica un problema con la rete.** Assicurarsi di avere accesso a Internet. 
- **La porta di Desktop remoto possa essere bloccata da un firewall.** Se stai usando Windows Firewall, segui questi passaggi:

   1. Aprire Windows Firewall. 
   2. Fai clic su **Consenti un'app o funzionalità attraverso Windows Firewall**. 
   3. Fai clic su **Modifica le impostazioni**. Potrebbe essere richiesto per una password di amministratore o per confermare la scelta.
   4. In **App consentite e le funzionalità**, selezionare **Desktop remoto**e quindi tocca o fai clic su **OK**.

   Se stai usando un firewall diverso, assicurati che la porta per Desktop remoto (in genere la 3389) è aperta.
- **Le connessioni remote potrebbero non essere configurate nel PC remoto.** Per risolvere questo problema, scorrere indietro fino alla [come posso impostare un PC per Desktop remoto?](#how-do-i-set-up-a-pc-for-remote-desktop) domanda in questo argomento.
- **Il PC remoto potrebbe consentire i PC per la connessione con autenticazione a livello di rete configurare.** 
- **Il PC remoto possa essere disattivato.** Non puoi connetterti a un PC è spento, in, o in fase di sospensione, quindi verifica che le impostazioni per la sospensione e ibernazione nel PC remoto siano impostate su **mai** (ibernazione non è disponibile in tutti i PC.).

### Perché non riesco a trovare o connettersi al PC?
Verificare quanto segue:
- È il PC in e riattivato?
- È stato immesso l'indirizzo IP o nome giusto?

   > [!IMPORTANT]
   > Utilizzando il nome del PC richiede la rete per risolvere il nome correttamente tramite DNS. In molti le reti domestiche, devi usare l'indirizzo IP anziché il nome host per la connessione.
- È il PC in una rete diversa? È configurato il PC per consentire all'esterno di connessioni tramite?  Consulta [Consenti accesso al PC dall'esterno della rete](remote-desktop-allow-outside-access.md) per la Guida.
- Ti stai connettendo a una versione di Windows supportata? 

   > [!NOTE]
   > Windows XP Home, Windows Media Center Edition, Windows Vista e Windows 7 Home o Starter non sono supportati senza 3a software di terze parti.

### Perché non riesco a accedere a un PC remoto?
Se è possibile visualizzare la schermata di accesso del PC remoto, ma non è possibile accedere, potrebbe non sono state aggiunte al gruppo utenti di Desktop remoto o a qualsiasi gruppo con diritti di amministratore nel PC remoto. Chiedere l'amministratore di sistema per eseguire questa operazione per te.

### Quali metodi di connessione sono supportati per le reti aziendali?
Se vuoi accedere al desktop di office dall'esterno della rete aziendale, l'azienda deve fornire un mezzo di accesso remoto. Il Client desktop remoto supporta attualmente i seguenti:

- Server terminal Gateway o Gateway Desktop remoto
- Accesso Web Desktop remoto
- VPN (tramite iOS opzioni di VPN predefinite)

### VPN non funziona

Problemi VPN possono avere varie cause. Il primo passaggio consiste nel verificare che la connessione VPN funziona nella stessa rete come un computer PC o Mac. Se non puoi testare con un PC o un Mac, puoi provare accedere a una pagina web nella rete intranet aziendale con del browser del dispositivo.

Altre cose da controllare:
- **La rete 3G blocca o danneggia VPN.** Esistono diversi i provider 3G nel mondo che sembrano blocco o il traffico 3G danneggiati. Verificare la connettività VPN funziona correttamente per più di un minuto.
- **L2TP o PPTP VPN.** Se usi L2TP o PPTP nella tua VPN, impostare inviare tutto il traffico su ON nella configurazione VPN.
- **VPN non è configurato correttamente.** Un server VPN non configurati correttamente possa essere il motivo per cui le connessioni VPN mai lo lavorato o l'ha smesso di funzionare dopo alcuni minuti. Assicurati di test con il browser web del dispositivo o un PC o un Mac iOS nella stessa rete in questo caso.

### Come posso verificare se VPN funziona correttamente?
Verifica che VPN è abilitata nel dispositivo. Puoi testare la connessione VPN passando a una pagina Web nella rete interna o tramite un servizio web che è disponibile solo tramite la connessione VPN.

### Come configurare le connessioni VPN di PPTP o L2TP?
Se usi L2TP o PPTP nella tua VPN, assicurati di impostare **inviare tutto il traffico** su **ON** nella configurazione VPN.

## Client Web

### Quali browser è possibile usare?

Il client web supporta Microsoft Edge, Internet Explorer 11, Mozilla Firefox (v55.0 e versioni successive), Safari e Google Chrome.

### Il PC è possibile utilizzare per accedere al client web?

Il client web supporta Windows, macOS, Linux e ChromeOS. I dispositivi mobili non sono supportati in questo momento.

### È possibile utilizzare il client web in una distribuzione di Desktop remoto senza un gateway?

No. Il client richiede un Gateway Desktop remoto per connettersi. Non sai cosa significa che? Chiedere l'amministratore su di esso.

### Il client web di Desktop remoto sostituisce la pagina di accesso Web Desktop remoto?

No. Il client web di Desktop remoto è ospitato in un URL diverso rispetto alla pagina di accesso Web Desktop remoto. È possibile utilizzare il client web o la pagina di accesso Web per visualizzare le risorse remote in un browser.

### È possibile incorporare il client web in un'altra pagina web?

Questa funzionalità non è supportata in questo momento.

## Monitor, audio e mouse

### Come usare tutti i monitor?
Per usare due o più schermate, eseguire le operazioni seguenti:

1. Pulsante destro del mouse sul desktop remoto che vuoi abilitare più schermate per e quindi fai clic su **Modifica**.
2. Abilita la **modalità a schermo intero**e **usare tutti i monitor** .

### Suono bidirezionale è supportata?
Suono monte (dal client al server, per microfoni) non è supportato dal Client Desktop remoto.

### Cosa posso fare se riprodurre il suono?
Effettua la disconnessione dalla sessione (non appena disconnettere, Disconnetti compresi) e quindi accedi di nuovo.

## Client Mac - domande hardware
### Risoluzione retina è supportata?
Sì, il client desktop remoto supporta la risoluzione retina.

### Come faccio ad abilitare la scelta secondario?
Per rendere Usa il pulsante destro del mouse all'interno di una sessione aperta hai tre opzioni:

- Mouse USB a standard PC due pulsanti
- Del Mouse: Apple magia clic per abilitare pulsante destro del mouse, **Preferenze di sistema** nel dock, fai clic su **Mouse**e quindi abilitare **fai clic su secondario**.
- Il Trackpad magia di Apple o MacBook Trackpad: per abilitare pulsante destro del mouse, fai clic su **Preferenze di sistema** nel dock, fai clic su **Mouse**e quindi abilitare **fai clic su secondario**.

### AirPrint è supportata?
No, il client Desktop remoto non supporta AirPrint. (Ciò vale per i client iOS e Mac.)

### Nella sessione perché appaiono caratteri non corretti?
Se si utilizza una tastiera internazionale, potresti notare un problema in cui i caratteri che vengono visualizzati nella sessione corrispondano i caratteri digitati sulla tastiera Mac.

Ciò può verificarsi negli scenari seguenti:

- Si utilizza una tastiera che non riconosce la sessione remota. Quando Desktop remoto non riconosce la tastiera, l'impostazione predefinita è la lingua ultimo usata con il PC remoto.
- Si connette a una sessione disconnessa in precedenza in un PC remoto e che un PC remoto Usa una lingua tastiera diversa da quella di attualmente si sta tentando di usare.

Per risolvere questo problema impostando manualmente la lingua della tastiera per la sessione remota. Vedi i passaggi nella sezione successiva.

### Come le impostazioni della lingua incidono tastiere in una sessione remota?
Esistono molti tipi di layout di tastiera Mac. Alcuni di questi sono layout specifici Mac o layout personalizzati per cui una corrispondenza esatta potrebbe non essere disponibile nella versione di Windows che comunicazione remota in. La sessione remota esegue il mapping della tastiera con la migliore corrispondenza di lingua della tastiera disponibile nel PC remoto. 

Se il layout di tastiera Mac è impostato su alla versione PC di tutte le chiavi devono essere mappate correttamente la tastiera di lingua (ad esempio, francese-PC) e la tastiera deve funzionare correttamente.

Se il layout di tastiera Mac è impostato sulla versione di Mac di una tastiera (ad esempio, francese) la sessione remota eseguirà il mapping è nella versione di PC della lingua francese. Alcuni dei tasti di scelta rapida Mac che sono usato in OSX non funzionerà nella sessione remota di Windows.

Se il layout di tastiera è impostato su una variante di una lingua (ad esempio, francese (Canada)) e la sessione remota non può eseguire il mapping è a tale variante esatto, la sessione remota eseguirà il mapping è il più vicino lingua (ad esempio, francese). Alcuni dei tasti di scelta rapida Mac che sono usato in OSX non funzionerà nella sessione remota di Windows.

Se il layout di tastiera è impostato su un layout che la sessione remota non corrisponde affatto, la sessione remota userà per darti la lingua che è stato utilizzato ultimo con PC. In questo caso, o nei casi in cui è necessario modificare la lingua della sessione remota in modo che corrisponda tastiera Mac, è possibile impostare manualmente la lingua della tastiera nella sessione remota per la lingua che corrisponde maggiormente a quello che si desidera utilizzare come indicato di seguito.

Usa le istruzioni seguenti per modificare il layout di tastiera nella sessione desktop remoto:

**In Windows 10 o Windows 8:**

1. Nella sessione remota, aprire all'area geografica e lingua. Fare clic su **Start gt _ Impostazioni gt _ ora e lingua**. Apri **all'area geografica e lingua**.
2. Aggiungere la lingua che vuoi usare. Quindi chiudere la finestra di area geografica e lingua.
3. A questo punto, la sessione remota, vedrai la possibilità di passare da una lingua. (Nel lato destro della sessione remota, accanto all'orologio.) Scegliere la lingua che desideri passare a (ad esempio **Eng**).

Potresti dover Chiudi e riavvia l'applicazione che attualmente in uso per le modifiche della tastiera per rendere effettive.


## Errori specifici

### Perché viene visualizzato un errore "Privilegi insufficienti"?
Non è consentito accedere alla sessione che si desidera connettersi. La causa probabile è che si sta tentando di connettersi a una sessione di amministratore. Sono consentiti solo agli amministratori di connettersi alla console. Verifica che lo switch di console è disattivato nelle impostazioni avanzate del desktop remoto. Se non si tratta l'origine del problema, contatta l'amministratore di sistema per ulteriore assistenza.

### Il motivo per cui il client dire che non vi sia alcun CAL?
Quando un client desktop remoto si connette a un server di Desktop remoto, il server emette un Remote Desktop Services licenza di accesso Client (CAL Servizi Desktop remoto) archiviati dal client. Ogni volta che il client si connette anche in questo caso userà relativo CAL RDS e il server non rilascerà un'altra licenza. Il server invia un'altra licenza se il CAL Servizi Desktop remoto nel dispositivo è mancante o danneggiato. Quando viene raggiunto il numero massimo di dispositivi con una licenza il server non rilascerà nuove licenze CAL Servizi Desktop remoto. Per assistenza, contattare l'amministratore di rete.

### Perché ricevuto un errore "Accesso negato"?
L'errore "Accesso negato" è un oggetto generato il Gateway Desktop remoto e il risultato di credenziali non corrette durante il tentativo di connessione. Verifica il nome utente e password. Se la connessione lavorato prima e si è verificato l'errore di recente, puoi eventualmente modificato la password dell'account utente Windows e non sono stati aggiornati ancora nelle impostazioni di desktop remote.

### Che cos'è "Errore RPC 23014" o significa "Errore 0x59e6"?
In caso di un **errore RPC 23014** o **errore 0x59E6 provare nuovamente dopo alcuni minuti di attesa**, il server Gateway Desktop remoto ha raggiunto il numero massimo di connessioni attive. A seconda della versione di Windows è in esecuzione sul Gateway Desktop remoto il numero massimo di connessioni diverso: implementazione di Windows Server 2008 R2 Standard limita il numero di connessioni a 250. L'implementazione di Windows Server 2008 R2 Foundation limita il numero di connessioni a 50. Tutte le altre implementazioni di Windows consentono un numero illimitato di connessioni.

### Che cosa significa l'errore "Impossibile analizzare sfida NTLM"?
Questo errore è causato da una configurazione errata nel PC remoto. Assicurati che l'impostazione del livello di sicurezza RDP nel PC remoto è impostato su "Client compatibili con". (Comunicare con il tuo amministratore di sistema se hai bisogno di aiuto questa operazione.)

### "TS_RAP non possono connettersi all'host specificato" che cosa significa?
Questo errore si verifica quando un criterio di autorizzazione delle risorse nel server gateway interrompe il proprio nome utente di connettersi al PC remoto. Questa situazione può verificarsi nelle seguenti circostanze:

- Il nome di PC remoto è lo stesso nome del gateway. Quindi, quando si tenta di connettersi al PC remoto, la connessione passa al gateway invece che probabilmente non hai l'autorizzazione per accedere. Se hai bisogno per la connessione al gateway, non utilizzare il nome del gateway esterno come nome del PC. Usa invece "localhost" o l'indirizzo IP (127.0.0.1) o il nome del server interni.
- L'account utente non è un membro del gruppo utenti per l'accesso remoto.