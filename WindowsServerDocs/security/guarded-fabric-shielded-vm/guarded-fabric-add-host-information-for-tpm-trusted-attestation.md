---
title: Aggiungere informazioni sull'host per l'attestazione TPM
ms.custom: na
ms.prod: windows-server-threshold
ms.topic: article
ms.assetid: f0aa575b-b34e-4f6c-8416-ed3e398e0ad2
manager: dongill
author: rpsqrd
ms.technology: security-guarded-fabric
ms.date: 08/29/2018
ms.openlocfilehash: 3647c9708ad68dec0ac13c85fced2b12150ccf60
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/31/2019
ms.locfileid: "66447196"
---
>Si applica a: Windows Server 2019, Windows Server (canale semestrale), Windows Server 2016

### <a name="add-host-information-for-tpm-trusted-attestation"></a>Aggiungere informazioni sull'host per l'attestazione TPM

Per la modalità TPM, l'amministratore dell'infrastruttura acquisisce tre tipi di informazioni sull'host, ognuno dei quali deve essere aggiunto alla configurazione del servizio HGS:

- Un identificatore TPM (EKpub) per ogni host Hyper-V
- Criteri di integrità di codice, un elenco vuoto di binari consentiti per gli host Hyper-V
- Una linea di base TPM (misurazioni di avvio) che rappresenta un set di Hyper-V ospita eseguiti sulla stessa classe dell'hardware

Dopo l'infrastruttura di amministratore vengono acquisite le informazioni, aggiungerlo alla configurazione del servizio HGS come descritto nella procedura seguente.

1.  Ottenere i file XML che contengono le informazioni di EKpub e copiarli in un server HGS. Vi sarà un file XML per ogni host. Eseguire quindi il comando seguente in una console Windows PowerShell con privilegi elevata in un server HGS. Ripetere il comando per ognuno dei file XML.

    ```powershell
    Add-HgsAttestationTpmHost -Path <Path><Filename>.xml -Name <HostName>
    ```

    > [!NOTE]
    > Se si verifica un errore durante l'aggiunta di un identificatore TPM riguardanti un certificato di chiave di verifica dell'autenticità non attendibile (EKCert), assicurarsi che il [attendibili i certificati radice TPM sono state aggiunte](guarded-fabric-install-trusted-tpm-root-certificates.md) al nodo di HGS.
    > Inoltre, alcuni fornitori TPM non utilizzano EKCerts.
    > È possibile controllare se è presente un EKCert, aprire il file XML in un editor, ad esempio Blocco note e controllo per un messaggio di errore che indica nessuna EKCert è stato trovato.
    > Se questo è il caso e si ritiene attendibile che il TPM nel computer è autenticato, è possibile usare il `-Force` flag per eseguire l'override di questo controllo di sicurezza e aggiungere l'identificatore dell'host servizio HGS.

2. Ottenere i criteri di integrità del codice che ha creato l'amministratore dell'infrastruttura per gli host, in formato binario (*. p7b). Copiarlo in un server HGS. Eseguire quindi il comando seguente.

    Per `<PolicyName>`, specificare un nome per il criterio di integrazione continua che descrive il tipo di applicazione all'host. Una procedura consigliata è assegnare il nome dopo il marca o modello della macchina e qualsiasi configurazione software speciale per l'esecuzione su di esso.<br>Per `<Path>`, specificare il percorso e nome file di criteri di integrità del codice.

    ```powershell
    Add-HgsAttestationCIPolicy -Path <Path> -Name '<PolicyName>'
    ```

3. Ottenere il file TCGlog che l'amministratore dell'infrastruttura acquisita da un host di riferimento. Copiare il file a un server HGS. Eseguire quindi il comando seguente. In genere, si verrà denomina il criterio dopo la classe di hardware che rappresenta (ad esempio, "Produttore modello revisione").

    ```powershell
    Add-HgsAttestationTpmPolicy -Path <Filename>.tcglog -Name '<PolicyName>'
    ```

In questo passaggio si completa il processo di configurazione di un cluster del servizio HGS per la modalità TPM. L'amministratore dell'infrastruttura potrebbe essere necessario è possibile specificare due URL di HGS per poter completare la configurazione per gli host. Per ottenere questi URL, in un server HGS, eseguire [Get-HgsServer](https://docs.microsoft.com/powershell/module/hgsserver/get-hgsserver?view=win10-ps).

## <a name="next-step"></a>Passaggio successivo

> [!div class="nextstepaction"]
> [Confermare l'attestazione](guarded-fabric-confirm-hosts-can-attest-successfully.md)