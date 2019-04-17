---
title: Gestire i certificati utilizzati con criteri di rete
description: In questo argomento fornisce informazioni sull'uso dei certificati server con Server dei criteri di rete in Windows Server 2016.
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 204a4ef4-9d78-4a62-9940-43cc0e1c39d0
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 2044cf30cc90c1673e05a1948ac9196d05940d1f
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/28/2018
---
# <a name="manage-certificates-used-with-nps"></a>Gestire i certificati utilizzati con criteri di rete

>Si applica a: Windows Server (canale annuale e virgola), Windows Server 2016

Se si distribuisce un metodo di autenticazione basata su certificati, ad esempio \(EAP\-TLS\) Extensible Authentication Protocol\-Transport Layer Security, \(PEAP\-TLS\) Protected Extensible Authentication Protocol\-Transport Layer Security e PEAP\-Microsoft Challenge Handshake Authentication Protocol versione 2 \ (v2\ MS\-CHAP), è necessario registrare un certificato del server per tutti i server dei criteri di rete. Il certificato del server deve:

- Soddisfare i requisiti del certificato server minimo come descritto in [configurare modelli di certificato per i requisiti di EAP e PEAP](nps-manage-cert-requirements.md)

- Essere emesso da un'autorità di certificazione \(CA\) ritenuta attendibile dai computer client. Un'autorità di certificazione è attendibile quando è presente il certificato nell'archivio certificati Autorità di certificazione radice attendibili per l'utente corrente e il computer locale.

Le istruzioni seguenti semplificano la gestione di certificati del server dei criteri di rete nelle distribuzioni in cui la CA radice attendibile un'autorità di certificazione di terze parti, ad esempio Verisign, o un'autorità di certificazione è stato distribuito per l'infrastruttura a chiave pubblica utilizzando Servizi certificati Active Directory \(AD CS\) \(PKI\).

## <a name="change-the-cached-tls-handle-expiry"></a>Modificare la scadenza di Handle TLS memorizzato nella cache

Durante i processi di autenticazione iniziale per EAP\-TLS, PEAP\ TLS e PEAP\-MS\-CHAP v2, il server dei criteri di rete, memorizza nella cache una parte delle proprietà di connessione del client di connessione TLS. Il client memorizza anche una parte delle proprietà di connessione del server dei criteri di rete TLS.

Ogni raccolta di queste proprietà connessione TLS singoli viene chiamato un handle TLS.

I computer client possono cache l'handle TLS per più autenticatori, mentre il server dei criteri di rete può memorizzare nella cache l'handle TLS di molti computer client.

L'handle TLS memorizzato nella cache sul client e server consentono il processo di riautenticazione si verificano più rapidamente. Ad esempio, quando un computer wireless protocollo con un server dei criteri di rete, il server dei criteri di rete può esaminare l'handle TLS per il client wireless e può determinare rapidamente che la connessione client è una riconnessione. Il server dei criteri di rete in questione autorizza la connessione senza eseguire l'autenticazione completa.

Analogamente, il client esamina l'handle TLS per il server dei criteri di rete, determina che è una riconnessione e non è necessario eseguire l'autenticazione server.

Nei computer che eseguono Windows 10 e Windows Server 2016, la scadenza di handle TLS predefinito è 10 ore.

In alcuni casi, si desidera aumentare o diminuire l'ora di scadenza handle TLS.

Ad esempio, si desidera ridurre il tempo di scadenza handle TLS in circostanze in cui viene revocato un certificato utente da un amministratore e il certificato è scaduto. In questo scenario, l'utente può comunque connettersi alla rete se un server dei criteri di rete ha un handle TLS memorizzato nella cache che non sia scaduta. Riducendo il TLS handle scadenza risultino utili per impedire che gli utenti con i certificati revocati riconnessione.

>[!NOTE]
>La soluzione migliore per questo scenario è per disabilitare l'account utente in Active Directory o per rimuovere l'account utente dal gruppo di Active Directory che è autorizzato a connettersi alla rete in Criteri di rete. La propagazione di queste modifiche per tutti i controller di dominio potrebbe anche subire un ritardo, tuttavia, a causa della latenza di replica. 

## <a name="configure-the-tls-handle-expiry-time-on-client-computers"></a>Configurare l'ora di scadenza Handle TLS nei computer Client

È possibile utilizzare questa procedura per modificare la quantità di tempo che i computer client memorizzano nella cache l'handle TLS di un server dei criteri di rete. Dopo l'autenticazione di un server dei criteri di rete, i computer client memorizzano nella cache le proprietà di connessione TLS del server dei criteri di rete come un handle TLS. L'handle TLS ha una durata predefinita di 10 ore \(36,000,000 milliseconds\). È possibile aumentare o diminuire l'ora di scadenza handle TLS utilizzando la procedura seguente.

Appartenenza al gruppo **amministratori**, o equivalente è il requisito minimo necessario per completare questa procedura.

>[!IMPORTANT]
>Questa procedura deve essere eseguita su un server dei criteri di rete, non in un computer client.

### <a name="to-configure-the-tls-handle-expiry-time-on-client-computers"></a>Per configurare il TLS gestione ora di scadenza nei computer client

1. In un server dei criteri di rete, aprire l'Editor del Registro di sistema.

2. Individuare la chiave del Registro di sistema **HKEY\_LOCAL\_MACHINE\System\CurrentControlSet\Control\SecurityProviders\SCHANNEL**

3. Nel **modifica** menu, fare clic su **New**e quindi fare clic su **chiave**.

4. Tipo **ClientCacheTime**, quindi premere INVIO.

5. Fare doppio clic su **ClientCacheTime**, fare clic su **New**, quindi fare clic su **valore DWORD (32 bit)**.

6. Digitare la quantità di tempo, in millisecondi, che si desidera che i computer client di memorizzare nella cache l'handle TLS di un server dei criteri di rete dopo la prima autenticazione riuscita tentato dal server dei criteri di rete.

## <a name="configure-the-tls-handle-expiry-time-on-nps-servers"></a>Configurare l'ora di scadenza Handle TLS in server dei criteri di rete

Utilizzare questa procedura per modificare la quantità di tempo che il server dei criteri di rete nella cache l'handle TLS dei computer client. Dopo l'autenticazione client access, server dei criteri di rete memorizza nella cache delle proprietà di connessione del computer client TLS come handle TLS. L'handle TLS ha una durata predefinita di 10 ore \(36,000,000 milliseconds\). È possibile aumentare o diminuire l'ora di scadenza handle TLS utilizzando la procedura seguente.

Appartenenza al gruppo **amministratori**, o equivalente è il requisito minimo necessario per completare questa procedura.

>[!IMPORTANT]
>Questa procedura deve essere eseguita su un server dei criteri di rete, non in un computer client.

### <a name="to-configure-the-tls-handle-expiry-time-on-nps-servers"></a>Per configurare il TLS gestione ora di scadenza in server dei criteri di rete

1. In un server dei criteri di rete, aprire l'Editor del Registro di sistema.

2. Individuare la chiave del Registro di sistema **HKEY\_LOCAL\_MACHINE\System\CurrentControlSet\Control\SecurityProviders\SCHANNEL**

3. Nel **modifica** menu, fare clic su **New**e quindi fare clic su **chiave**.

4. Tipo **ServerCacheTime**, quindi premere INVIO.

5. Fare doppio clic su **ServerCacheTime**, fare clic su **New**, quindi fare clic su **valore DWORD (32 bit)**.

6. Digitare la quantità di tempo, in millisecondi, che si desidera che server dei criteri di rete di memorizzare nella cache l'handle TLS di un computer client dopo la prima autenticazione riuscita tentato dal client.

## <a name="obtain-the-sha-1-hash-of-a-trusted-root-ca-certificate"></a>Ottenere il SHA-1 Hash di un certificato della CA radice attendibili

Utilizzare questa procedura per ottenere Secure Hash Algorithm (SHA-1) l'hash di un'autorità di certificazione (CA) radice attendibile di un certificato installato nel computer locale. In alcune circostanze, ad esempio durante la distribuzione di criteri di gruppo, è necessario designare un certificato con l'hash SHA-1 del certificato.

Quando si utilizza criteri di gruppo, è possibile designare uno o più certificati di autorità di certificazione radice attendibili che i client devono utilizzare per autenticare il server dei criteri di rete durante il processo di autenticazione reciproca con EAP o PEAP. Per designare un certificato della CA radice attendibili che i client devono utilizzare per convalidare il certificato del server, è possibile immettere l'hash SHA-1 del certificato.

Questa procedura illustra come ottenere l'hash SHA-1 di un certificato della CA radice attendibile utilizzando lo snap-in certificati di Microsoft Management Console (MMC). 

Per completare questa procedura, è necessario essere un membro del **utenti** gruppo nel computer locale.

### <a name="to-obtain-the-sha-1-hash-of-a-trusted-root-ca-certificate"></a>Per ottenere il SHA-1 hash di un certificato CA radice attendibile

1. Nella finestra di dialogo Esegui o Windows PowerShell, digitare **mmc**, quindi premere INVIO. Apre il \(MMC\) Microsoft Management Console. In MMC, fare clic su **File**, quindi fare clic su **Aggiungi/Rimuovi Snap\in**. Il **Aggiungi o Rimuovi Snap-in** apre la finestra di dialogo.

2. In **Aggiungi o Rimuovi Snap-in**, in **snap-in disponibili**, fare doppio clic su **certificati**. Apre la procedura guidata snap-in certificati. Fare clic su **account Computer**, quindi fare clic su **Avanti**.

3. In **seleziona Computer**, assicurarsi che **computer locale (il computer su cui è in esecuzione questa console)** è selezionata, fare clic su **fine**, quindi fare clic su **OK**.

4. Nel riquadro sinistro fare doppio clic su **certificati (Computer locale)**, quindi fare doppio clic il **autorità di certificazione radice attendibili** cartella.

5. Il **certificati** cartella è una sottocartella del **autorità di certificazione radice attendibili** cartella. Fare clic su di **certificati** cartella.

6. Nel riquadro dei dettagli, selezionare il certificato per la CA radice attendibile. Fare doppio clic il certificato. Il **certificato** apre la finestra di dialogo.

7. Nel **certificato** la finestra di dialogo, fare clic su di **dettagli** scheda.

8. Nell'elenco dei campi, scorrere e selezionare **identificazione personale**.

9. Nel riquadro inferiore, viene visualizzata la stringa esadecimale che rappresenta l'hash SHA-1 del certificato. Selezionare il SHA-1 hash e quindi premere il tasto di scelta rapida Windows per la copia di comando \(CTRL\+C\) per copiare l'hash negli Appunti di Windows.

10. Aprire il percorso a cui si desidera incollare l'hash SHA-1, individuare correttamente il cursore e quindi premere il tasto di scelta rapida Windows per incollare comando \(CTRL\+V\). 

Per ulteriori informazioni sui certificati e dei criteri di rete, vedere [configurare modelli di certificato per i requisiti di EAP e PEAP](nps-manage-cert-requirements.md).

Per ulteriori informazioni sui criteri di rete, vedere [Server dei criteri di rete (NPS)](nps-top.md).
