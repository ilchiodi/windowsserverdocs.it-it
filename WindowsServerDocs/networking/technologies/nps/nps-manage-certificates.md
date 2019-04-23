---
title: Gestire i certificati utilizzati con Server dei criteri di rete
description: In questo argomento vengono fornite informazioni sull'uso dei certificati server con Server dei criteri di rete in Windows Server 2016.
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 204a4ef4-9d78-4a62-9940-43cc0e1c39d0
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 73f3d6a1e9dc6ae1520b1d685b6b05b5f3aed601
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59864232"
---
# <a name="manage-certificates-used-with-nps"></a>Gestire i certificati utilizzati con Server dei criteri di rete

>Si applica a: Windows Server (canale semestrale), Windows Server 2016

Se si distribuisce un metodo di autenticazione basata su certificato, ad esempio Extensible Authentication Protocol\-Transport Layer Security \(EAP\-TLS\), Protected Extensible Authentication Protocol\-Transport Layer Security \(PEAP\-TLS\)e PEAP\-Microsoft Challenge Handshake Authentication Protocol versione 2 \(MS\-CHAP v2\), è necessario registrare un certificato del server a tutti i NPSs. Il certificato del server deve:

- Soddisfare i requisiti del certificato server minimo come descritto in [configurare modelli di certificato per i requisiti di EAP e PEAP](nps-manage-cert-requirements.md)

- Essere emesso da un'autorità di certificazione \(CA\) considerata attendibile dai computer client. Un'autorità di certificazione viene considerato attendibile quando il relativo certificato presente nell'archivio certificati Autorità di certificazione radice attendibili per l'utente corrente e nel computer locale.

Le istruzioni seguenti facilitano la gestione dei certificati dei criteri di rete nelle distribuzioni in cui la CA radice attendibile è un'autorità di certificazione di terze parti, ad esempio Verisign, oppure è un'autorità di certificazione è stata distribuita per l'infrastruttura a chiave pubblica \(PKI\) utilizzando Active Servizi certificati \(Servizi certificati Active Directory\).

## <a name="change-the-cached-tls-handle-expiry"></a>Modifiche alla scadenza di Handle TLS memorizzato nella cache

Durante i processi di autenticazione iniziale per l'autenticazione EAP\-TLS, PEAP\-TLS e PEAP\-MS\-CHAP v2, i criteri di rete vengono memorizzati nella cache una parte delle proprietà di connessione del client che esegue la connessione TLS. Il client memorizza nella cache anche una parte TLS del NPS proprietà di connessione.

Ogni singola raccolta di queste proprietà di connessione TLS viene chiamato un handle TLS.

I computer client è possono di memorizzare nella cache di handle TLS per più autenticatori, mentre NPSs può memorizzare nella cache l'handle TLS per molti computer client.

Gli handle TLS memorizzato nella cache sul client e server consentono al processo di riautenticazione di si verificano più rapidamente. Ad esempio, quando un computer wireless effettua una nuova autenticazione con un criteri di rete, i criteri di rete può esaminare l'handle TLS per il client senza fili e possono determinare rapidamente che la connessione client è una riconnessione. I criteri di rete autorizza la connessione senza eseguire l'autenticazione completa.

Di conseguenza, il client esamina l'handle TLS per i criteri di rete, determina che è una riconnessione e non è necessario eseguire l'autenticazione server.

Nei computer che eseguono Windows 10 e Windows Server 2016, la scadenza di handle TLS predefinito è 10 ore.

In alcuni casi, è possibile aumentare o ridurre il tempo di scadenza handle TLS.

Ad esempio, è possibile per ridurre i tempi di scadenza handle TLS in circostanze in cui viene revocato un certificato utente da un amministratore e il certificato è scaduto. In questo scenario, l'utente può comunque connettersi alla rete, se un NPS è dotato di un handle TLS memorizzato nella cache che non sia scaduto. Riducendo il TLS scadenza handle potrebbe impedire tali utenti con i certificati revocati la riconnessione.

>[!NOTE]
>La soluzione migliore in questo scenario è per disabilitare l'account utente in Active Directory o per rimuovere l'account utente dal gruppo di Active Directory che viene concessa l'autorizzazione per connettersi alla rete nei criteri di rete. La propagazione delle modifiche a tutti i controller di dominio potrebbe anche essere tuttavia ritardata a causa della latenza di replica. 

## <a name="configure-the-tls-handle-expiry-time-on-client-computers"></a>Configurare l'ora di scadenza Handle TLS nei computer Client

È possibile utilizzare questa procedura per modificare la quantità di tempo che i computer client memorizzano nella cache l'handle TLS di un criteri di rete. Dopo l'autenticazione un criteri di rete, i computer client memorizzano nella cache le proprietà di connessione TLS di NPS come handle TLS. L'handle TLS ha una durata predefinita di 10 ore \(36,000,000 millisecondi\). È possibile aumentare o diminuire il tempo di scadenza di handle TLS usando la procedura seguente.

L'appartenenza a **amministratori**, o equivalente è il requisito minimo necessario per completare questa procedura.

>[!IMPORTANT]
>Questa procedura deve essere eseguita su un criteri di rete, non in un computer client.

### <a name="to-configure-the-tls-handle-expiry-time-on-client-computers"></a>Per configurare TLS gestiscono l'ora di scadenza nei computer client

1. In un NPS, aprire l'Editor del Registro di sistema.

2. Individuare la chiave del Registro di sistema **HKEY\_locale\_MACHINE\System\CurrentControlSet\Control\SecurityProviders\SCHANNEL**

3. Nel **Edit** menu, fare clic su **New**e quindi fare clic su **chiave**.

4. Tipo di **ClientCacheTime**, quindi premere INVIO.

5. Fare doppio clic su **ClientCacheTime**, fare clic su **New**, quindi fare clic su **valore DWORD (32 bit)**.

6. Digitare la quantità di tempo, espresso in millisecondi, che per i computer client per memorizzare nella cache l'handle TLS di un criteri di rete dopo la prima autenticazione riuscita tentativo da parte di criteri di rete.

## <a name="configure-the-tls-handle-expiry-time-on-npss"></a>Configurare l'ora di scadenza Handle TLS su NPSs

Utilizzare questa procedura per modificare la quantità di tempo che NPSs memorizza nella cache l'handle TLS dei computer client. Dopo l'autenticazione un client di accesso, NPSs memorizzare nella cache le proprietà di connessione di TLS del computer client come handle TLS. L'handle TLS ha una durata predefinita di 10 ore \(36,000,000 millisecondi\). È possibile aumentare o diminuire il tempo di scadenza di handle TLS usando la procedura seguente.

L'appartenenza a **amministratori**, o equivalente è il requisito minimo necessario per completare questa procedura.

>[!IMPORTANT]
>Questa procedura deve essere eseguita su un criteri di rete, non in un computer client.

### <a name="to-configure-the-tls-handle-expiry-time-on-npss"></a>Per configurare TLS gestiscono l'ora di scadenza su NPSs

1. In un NPS, aprire l'Editor del Registro di sistema.

2. Individuare la chiave del Registro di sistema **HKEY\_locale\_MACHINE\System\CurrentControlSet\Control\SecurityProviders\SCHANNEL**

3. Nel **Edit** menu, fare clic su **New**e quindi fare clic su **chiave**.

4. Tipo di **ServerCacheTime**, quindi premere INVIO.

5. Fare doppio clic su **ServerCacheTime**, fare clic su **New**, quindi fare clic su **valore DWORD (32 bit)**.

6. Digitare la quantità di tempo, espresso in millisecondi, che si desidera NPSs per memorizzare nella cache l'handle TLS di un computer client dopo la prima autenticazione riuscita tentato dal client.

## <a name="obtain-the-sha-1-hash-of-a-trusted-root-ca-certificate"></a>Ottenere l'Hash SHA-1 di un certificato CA radice attendibile

Utilizzare questa procedura per ottenere il Secure Hash Algorithm (SHA-1) l'hash di un'autorità di certificazione radice attendibile (CA) da un certificato installato nel computer locale. In alcuni casi, ad esempio quando si distribuiscono criteri di gruppo, è necessario designare un certificato usando l'hash SHA-1 del certificato.

Quando si usano criteri di gruppo, è possibile designare uno o più certificati di autorità di certificazione radice attendibili che i client devono usare per autenticare i criteri di rete durante il processo di autenticazione reciproca con EAP o PEAP. Per designare un certificato CA radice attendibile che i client devono usare per convalidare il certificato del server, è possibile immettere l'hash SHA-1 del certificato.

Questa procedura viene illustrato come ottenere il SHA-1 hash di un certificato CA radice attendibile usando lo snap-in certificati di Microsoft Management Console (MMC). 

Per completare questa procedura, è necessario essere un membro del **utenti** gruppo nel computer locale.

### <a name="to-obtain-the-sha-1-hash-of-a-trusted-root-ca-certificate"></a>Per ottenere l'hash di un certificato CA radice attendibile di SHA-1

1. Nella finestra di dialogo Run o Windows PowerShell, digitare **mmc**, quindi premere INVIO. Microsoft Management Console \(MMC\) apre. In MMC, fare clic su **File**, quindi fare clic su **Aggiungi/Rimuovi Snap\in**. Il **Aggiungi o Rimuovi Snap-in** viene visualizzata la finestra di dialogo.

2. In **Aggiungi o rimuovi snap-in** fare doppio clic su **Certificati** in **Snap-in disponibili**. Verrà visualizzata la procedura guidata lo snap-in certificati. Fare clic su **Account del computer** e quindi su **Avanti**.

3. In **seleziona Computer**, assicurarsi che **computer locale (il computer è in esecuzione questa console)** è selezionato, fare clic su **fine**, quindi fare clic su **OK**.

4. Nel riquadro sinistro, fare doppio clic su **certificati (Computer locale)**, quindi fare doppio clic il **autorità di certificazione radice attendibili** cartella.

5. Il **certificati** cartella è una sottocartella delle **autorità di certificazione radice attendibili** cartella. Fare clic sulla cartella **Certificati**.

6. Nel riquadro dei dettagli passare al certificato per la CA radice attendibile. Fare doppio clic sul certificato. Verrà visualizzata la finestra di dialogo **Certificato**.

7. Nella finestra di dialogo **Certificato** fare clic sulla scheda **Dettagli**.

8. Nell'elenco dei campi, scorrere e selezionare **identificazione personale**.

9. Nel riquadro inferiore viene visualizzata la stringa esadecimale che corrisponde all'hash SHA-1 del certificato. Selezionare l'hash SHA-1 e quindi premere il tasto di scelta rapida Windows per il comando di copia \(CTRL\+C\) per copiare l'hash negli Appunti di Windows.

10. Aprire il percorso in cui si desidera incollare l'hash SHA-1, individuare correttamente il cursore e quindi premere il tasto di scelta rapida Windows per il comando Incolla \(CTRL\+V\). 

Per altre informazioni sui certificati e i criteri di rete, vedere [configurare i modelli di certificato per i requisiti di EAP e PEAP](nps-manage-cert-requirements.md).

Per altre informazioni sui criteri di rete, vedere [Strumentazione gestione Windows (NPS, Network Policy Server)](nps-top.md).
