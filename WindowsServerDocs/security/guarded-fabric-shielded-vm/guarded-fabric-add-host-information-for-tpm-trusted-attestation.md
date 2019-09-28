---
title: Aggiungere informazioni sull'host per l'attestazione TPM
ms.custom: na
ms.prod: windows-server
ms.topic: article
ms.assetid: f0aa575b-b34e-4f6c-8416-ed3e398e0ad2
manager: dongill
author: rpsqrd
ms.technology: security-guarded-fabric
ms.date: 06/21/2019
ms.openlocfilehash: 35efca71278c288189819d6c9fc49ba8195d18a1
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71386823"
---
>Si applica a: Windows Server 2019, Windows Server (canale semestrale), Windows Server 2016

### <a name="add-host-information-for-tpm-trusted-attestation"></a>Aggiungere informazioni sull'host per l'attestazione TPM

Per la modalità TPM, l'amministratore dell'infrastruttura acquisisce tre tipi di informazioni sull'host, ognuno dei quali deve essere aggiunto alla configurazione HGS:

- Identificatore TPM (EKpub) per ogni host Hyper-V
- Criteri di integrità del codice, un elenco di file binari consentiti per gli host Hyper-V
- Una baseline TPM (misurazioni di avvio) che rappresenta un set di host Hyper-V in esecuzione sulla stessa classe di hardware

Quando l'amministratore dell'infrastruttura acquisisce le informazioni, aggiungerle alla configurazione di HGS, come descritto nella procedura seguente.

1. Ottenere i file XML che contengono le informazioni EKpub e copiarli in un server HGS. Sarà presente un file XML per ogni host. Quindi, in una console di Windows PowerShell con privilegi elevati in un server HGS, eseguire il comando seguente. Ripetere il comando per ogni file XML.

    ```powershell
    Add-HgsAttestationTpmHost -Path <Path><Filename>.xml -Name <HostName>
    ```

    > [!NOTE]
    > Se si verifica un errore durante l'aggiunta di un identificatore TPM relativo a un certificato della chiave di verifica dell'autenticità (EKCert) non attendibile, verificare che i [certificati radice TPM attendibili siano stati aggiunti](guarded-fabric-install-trusted-tpm-root-certificates.md) al nodo HGS.
    > Alcuni fornitori di TPM, inoltre, non utilizzano EKCerts.
    > È possibile verificare se manca un EKCert aprendo il file XML in un editor, ad esempio Blocco note, e verificando la presenza di un messaggio di errore che indica che non è stato trovato alcun EKCert.
    > Se questo è il caso e si considera attendibile l'autenticità del TPM nel computer, è possibile usare il flag `-Force` per ignorare questo controllo di sicurezza e aggiungere l'identificatore host a HGS.

2. Ottenere i criteri di integrità del codice creati dall'amministratore dell'infrastruttura per gli host, in formato binario (@no__t 0. p7b). Copiarlo in un server HGS. Eseguire quindi il comando seguente.

    Per `<PolicyName>`, specificare un nome per il criterio CI che descrive il tipo di host a cui si applica. Una procedura consigliata consiste nel denominarla dopo la marca/modello del computer e qualsiasi configurazione software speciale in esecuzione su di essa.<br>Per `<Path>`, specificare il percorso e il nome file del criterio di integrità del codice.

    ```powershell
    Add-HgsAttestationCIPolicy -Path <Path> -Name '<PolicyName>'
    ```
    
    > [!NOTE]
    > Se si usa un criterio di integrità del codice firmato, registrare una copia senza segno dello stesso criterio con HGS.
    > La firma sui criteri di integrità del codice viene utilizzata per controllare gli aggiornamenti ai criteri, ma non viene misurata nel TPM host e pertanto non può essere attestata da HGS.

3. Ottenere il file TCGlog acquisito dall'amministratore dell'infrastruttura da un host di riferimento. Copiare il file in un server HGS. Eseguire quindi il comando seguente. In genere, i criteri vengono denominati in seguito alla classe di hardware che rappresenta, ad esempio "Revisione del modello del produttore".

    ```powershell
    Add-HgsAttestationTpmPolicy -Path <Filename>.tcglog -Name '<PolicyName>'
    ```

Questa operazione completa il processo di configurazione di un cluster HGS per la modalità TPM. L'amministratore dell'infrastruttura potrebbe avere la necessità di fornire due URL da HGS prima che la configurazione possa essere completata per gli host. Per ottenere questi URL, eseguire [Get-HgsServer](https://docs.microsoft.com/powershell/module/hgsserver/get-hgsserver?view=win10-ps)in un server HGS.

## <a name="next-step"></a>Passaggio successivo

> [!div class="nextstepaction"]
> [Confermare l'attestazione](guarded-fabric-confirm-hosts-can-attest-successfully.md)
