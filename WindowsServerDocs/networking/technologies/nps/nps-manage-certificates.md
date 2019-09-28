---
title: Gestire i certificati utilizzati con Server dei criteri di rete
description: In questo argomento vengono fornite informazioni sull'utilizzo di certificati server con server dei criteri di rete in Windows Server 2016.
manager: brianlic
ms.prod: windows-server
ms.technology: networking
ms.topic: article
ms.assetid: 204a4ef4-9d78-4a62-9940-43cc0e1c39d0
ms.author: pashort
author: shortpatti
ms.openlocfilehash: b83f68b52a9cceef779e5204e295bbc9e45e7a14
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71396203"
---
# <a name="manage-certificates-used-with-nps"></a>Gestire i certificati utilizzati con Server dei criteri di rete

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016

Se si distribuisce un metodo di autenticazione basato su certificati, ad esempio Extensible Authentication Protocol @ no__t-0Transport Layer Security \(EAP @ no__t-2TLS @ no__t-3, Protected Extensible Authentication Protocol @ no__t-4Transport Layer Security @no__ t-5PEAP @ no__t-6TLS @ no__t-7 e PEAP @ no__t-8Microsoft Challenge Handshake Authentication Protocol versione 2 \(MS @ no__t-10CHAP V2 @ no__t-11, è necessario registrare un certificato server in tutti i NPSs. Il certificato del server deve:

- Soddisfare i requisiti minimi del certificato server come descritto in [configurare i modelli di certificato per i requisiti PEAP e EAP](nps-manage-cert-requirements.md)

- Essere emesso da un'autorità di certificazione \(CA @ no__t-1 considerata attendibile dai computer client. Una CA è considerata attendibile quando il relativo certificato è presente nell'archivio certificati delle autorità di certificazione radice attendibili per l'utente corrente e il computer locale.

Le istruzioni seguenti consentono di gestire i certificati NPS nelle distribuzioni in cui la CA radice attendibile è una CA di terze parti, ad esempio VeriSign, oppure è un'autorità di certificazione distribuita per l'infrastruttura a chiave pubblica \(PKI @ no__t-1 tramite Active Directory Servizi certificati \(AD CS @ no__t-3.

## <a name="change-the-cached-tls-handle-expiry"></a>Modificare la scadenza dell'handle TLS memorizzato nella cache

Durante i processi di autenticazione iniziali per EAP @ no__t-0TLS, PEAP @ no__t-1TLS e PEAP @ no__t-2 ms @ no__t-3CHAP V2, NPS memorizza nella cache una parte delle proprietà di connessione TLS del client che si connette. Il client memorizza nella cache anche una parte delle proprietà di connessione TLS di NPS.

Ogni singola raccolta di queste proprietà di connessione TLS viene chiamata handle TLS.

I computer client possono memorizzare nella cache gli handle TLS per più autenticatori, mentre NPSs può memorizzare nella cache gli handle TLS di molti computer client.

Gli handle TLS memorizzati nella cache nel client e nel server consentono un processo di riautenticazione più rapido. Ad esempio, quando un computer wireless viene riautenticato con un server dei criteri di rete, il server dei criteri di rete può esaminare l'handle TLS per il client wireless e può determinare rapidamente che la connessione client è una riconnessione. Il server dei criteri di accesso autorizza la connessione senza eseguire l'autenticazione completa.

In modo analogo, il client esamina l'handle TLS per NPS, determina che si tratta di una riconnessione e non deve eseguire l'autenticazione del server.

Nei computer che eseguono Windows 10 e Windows Server 2016, la scadenza predefinita dell'handle TLS è 10 ore.

In alcune circostanze, potrebbe essere necessario aumentare o ridurre il tempo di scadenza dell'handle TLS.

Ad esempio, potrebbe essere necessario ridurre il tempo di scadenza dell'handle TLS nei casi in cui il certificato di un utente viene revocato da un amministratore e il certificato è scaduto. In questo scenario, l'utente può comunque connettersi alla rete se un server dei criteri di rete ha un handle TLS memorizzato nella cache che non è scaduto. La riduzione della scadenza dell'handle TLS potrebbe contribuire a impedire la riconnessione di tali utenti con certificati revocati.

>[!NOTE]
>La soluzione migliore a questo scenario è disabilitare l'account utente in Active Directory o rimuovere l'account utente dal gruppo Active Directory a cui viene concessa l'autorizzazione per la connessione alla rete nei criteri di rete. Tuttavia, la propagazione di queste modifiche a tutti i controller di dominio potrebbe essere ritardata, a causa della latenza di replica. 

## <a name="configure-the-tls-handle-expiry-time-on-client-computers"></a>Configurare l'ora di scadenza dell'handle TLS nei computer client

È possibile utilizzare questa procedura per modificare la quantità di tempo in cui i computer client memorizzano nella cache l'handle TLS di un server dei criteri di server. Dopo aver autenticato un server dei criteri di server, i computer client memorizzano nella cache le proprietà di connessione TLS di NPS come handle TLS. L'handle TLS ha una durata predefinita di 10 ore \(36.000.000 millisecondi @ no__t-1. È possibile aumentare o diminuire l'ora di scadenza dell'handle TLS utilizzando la procedura riportata di seguito.

L'appartenenza a **amministratori**, o equivalente è il requisito minimo necessario per completare questa procedura.

>[!IMPORTANT]
>Questa procedura deve essere eseguita su un server dei criteri di server, non su un computer client.

### <a name="to-configure-the-tls-handle-expiry-time-on-client-computers"></a>Per configurare l'ora di scadenza dell'handle TLS nei computer client

1. In un server dei criteri di server, aprire l'editor del registro.

2. Passare alla chiave del registro di sistema **HKEY @ no__t-1LOCAL @ no__t-2MACHINE\System\CurrentControlSet\Control\SecurityProviders\SCHANNEL**

3. Scegliere **nuovo**dal menu **modifica** e quindi fare clic su **chiave**.

4. Digitare **ClientCacheTime**, quindi premere INVIO.

5. Fare clic con il pulsante destro del mouse su **ClientCacheTime**, scegliere **nuovo**, quindi fare clic su **valore DWORD (32-bit)** .

6. Digitare il periodo di tempo, in millisecondi, in cui si desidera che i computer client memorizzano nella cache l'handle TLS di un server dei criteri di server dopo il primo tentativo di autenticazione riuscito da server dei criteri di server.

## <a name="configure-the-tls-handle-expiry-time-on-npss"></a>Configurare l'ora di scadenza dell'handle TLS in NPSs

Usare questa procedura per modificare la quantità di tempo per cui NPSs memorizza nella cache il handle TLS dei computer client. Dopo l'autenticazione di un client di Access, NPSs memorizza nella cache le proprietà di connessione TLS del computer client come handle TLS. L'handle TLS ha una durata predefinita di 10 ore \(36.000.000 millisecondi @ no__t-1. È possibile aumentare o diminuire l'ora di scadenza dell'handle TLS utilizzando la procedura riportata di seguito.

L'appartenenza a **amministratori**, o equivalente è il requisito minimo necessario per completare questa procedura.

>[!IMPORTANT]
>Questa procedura deve essere eseguita su un server dei criteri di server, non su un computer client.

### <a name="to-configure-the-tls-handle-expiry-time-on-npss"></a>Per configurare l'ora di scadenza dell'handle TLS in NPSs

1. In un server dei criteri di server, aprire l'editor del registro.

2. Passare alla chiave del registro di sistema **HKEY @ no__t-1LOCAL @ no__t-2MACHINE\System\CurrentControlSet\Control\SecurityProviders\SCHANNEL**

3. Scegliere **nuovo**dal menu **modifica** e quindi fare clic su **chiave**.

4. Digitare **ServerCacheTime**, quindi premere INVIO.

5. Fare clic con il pulsante destro del mouse su **ServerCacheTime**, scegliere **nuovo**, quindi fare clic su **valore DWORD (32-bit)** .

6. Digitare la quantità di tempo, in millisecondi, in cui si desidera che NPSs memorizza nella cache l'handle TLS di un computer client dopo il primo tentativo di autenticazione eseguito dal client.

## <a name="obtain-the-sha-1-hash-of-a-trusted-root-ca-certificate"></a>Ottenere l'hash SHA-1 di un certificato CA radice attendibile

Utilizzare questa procedura per ottenere l'hash SHA-1 (Secure Hash Algorithm) di un'autorità di certificazione radice attendibile (CA) da un certificato installato nel computer locale. In alcune circostanze, ad esempio quando si distribuisce Criteri di gruppo, è necessario designare un certificato usando l'hash SHA-1 del certificato.

Quando si utilizza Criteri di gruppo, è possibile designare uno o più certificati CA radice attendibili che devono essere utilizzati dai client per autenticare il server dei criteri di accesso durante il processo di autenticazione reciproca con EAP o PEAP. Per designare un certificato CA radice attendibile che i client devono usare per convalidare il certificato del server, è possibile immettere l'hash SHA-1 del certificato.

Questa procedura illustra come ottenere l'hash SHA-1 di un certificato CA radice attendibile usando lo snap-in certificati di Microsoft Management Console (MMC). 

Per completare questa procedura, è necessario essere un membro del gruppo **Users** nel computer locale.

### <a name="to-obtain-the-sha-1-hash-of-a-trusted-root-ca-certificate"></a>Per ottenere l'hash SHA-1 di un certificato CA radice attendibile

1. Nella finestra di dialogo Esegui o Windows PowerShell digitare **MMC**, quindi premere INVIO. Verrà aperto Microsoft Management Console \(MMC @ no__t-1. In MMC fare clic su **file**, quindi su **Aggiungi/Rimuovi Snap\in**. Il **Aggiungi o Rimuovi Snap-in** viene visualizzata la finestra di dialogo.

2. In **Aggiungi o rimuovi snap-in** fare doppio clic su **Certificati** in **Snap-in disponibili**. Verrà visualizzata la procedura guidata snap-in certificati. Fare clic su **Account del computer** e quindi su **Avanti**.

3. In **Seleziona computer**verificare che **l'opzione computer locale (computer in cui è in esecuzione la console)** sia selezionata, fare clic su **fine**e quindi fare clic su **OK**.

4. Nel riquadro sinistro fare doppio clic su **certificati (computer locale)** , quindi fare doppio clic sulla cartella **autorità di certificazione radice attendibili** .

5. La cartella **Certificates** è una sottocartella della cartella **autorità di certificazione radice attendibili** . Fare clic sulla cartella **Certificati**.

6. Nel riquadro dei dettagli passare al certificato per la CA radice attendibile. Fare doppio clic sul certificato. Verrà visualizzata la finestra di dialogo **Certificato**.

7. Nella finestra di dialogo **Certificato** fare clic sulla scheda **Dettagli**.

8. Nell'elenco dei campi scorrere fino a e selezionare **identificazione personale**.

9. Nel riquadro inferiore viene visualizzata la stringa esadecimale che corrisponde all'hash SHA-1 del certificato. Selezionare l'hash SHA-1, quindi premere il tasto di scelta rapida di Windows per il comando copy \(CTRL @ no__t-1C @ no__t-2 per copiare l'hash negli Appunti di Windows.

10. Aprire il percorso in cui si desidera incollare l'hash SHA-1, individuare correttamente il cursore, quindi premere il tasto di scelta rapida di Windows per il comando Incolla \(CTRL @ no__t-1V @ no__t-2. 

Per ulteriori informazioni sui certificati e sui server dei criteri di server, vedere [configurare i modelli di certificato per i requisiti PEAP e EAP](nps-manage-cert-requirements.md).

Per ulteriori informazioni su NPS, vedere [Server dei criteri di rete (NPS)](nps-top.md).
