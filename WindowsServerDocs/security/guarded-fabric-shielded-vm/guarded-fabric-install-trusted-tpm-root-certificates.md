---
title: Installare i certificati radice TPM attendibili
ms.custom: na
ms.prod: windows-server-threshold
ms.topic: article
manager: dongill
author: rpsqrd
ms.technology: security-guarded-fabric
ms.date: 06/21/2019
ms.openlocfilehash: 211d2f24b01d1e308f012df681f9e16a2190449f
ms.sourcegitcommit: 260b1d78cb28b88b876579e1ac9a41a74e8752fd
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 06/26/2019
ms.locfileid: "67398802"
---
# <a name="install-trusted-tpm-root-certificates"></a>Installare i certificati radice TPM attendibili

>Si applica a: Windows Server 2019, Windows Server (canale semestrale), Windows Server 2016

Quando si configura HGS per usare l'attestazione TPM, è necessario anche configurare il servizio HGS per considerare attendibili i fornitori di TPM nei server.
Questo processo di verifica aggiuntiva garantisce che solo autentico e attendibile i moduli TPM sono in grado di attestare con HGS.
Se si prova a registrare un modulo TPM non attendibili con `Add-HgsAttestationTpmHost`, si riceverà un errore che indica il produttore TPM non è considerato attendibile.

Per rendere attendibile il TPM, è necessario installare su HGS i certificati radice e intermedi firma usati per firmare la chiave di verifica dell'autenticità nel TPM dei server.
Se si usa più di un modello TPM nel tuo Data Center, si potrebbe essere necessario installare certificati diversi per ogni modello.
HGS avrà un aspetto in "TrustedTPM_RootCA" e "TrustedTPM_IntermediateCA" certificato vengono archiviati per i certificati del fornitore.

> [!NOTE]
> I certificati di fornitori TPM sono diversi da quelle installate per impostazione predefinita in Windows e rappresentano la radice specifico e i certificati intermedi usati dai fornitori TPM.

Una raccolta di certificati intermedi e radice attendibile di TPM è pubblicata da Microsoft per comodità.
È possibile utilizzare la procedura seguente per installare tali certificati.
Se i certificati TPM non sono incluse nel pacchetto riportato di seguito, contattare il fornitore TPM o server OEM per ottenere i certificati radice e intermedi per il modello TPM specifico.

Ripetere i passaggi seguenti sul **tutti i server HGS**:

1.  Scaricare il pacchetto più recente dal [ https://tpmsec.microsoft.com/TPMCerts/TrustedTPM.cab ](https://tpmsec.microsoft.com/TPMCerts/TrustedTPM.cab).

2.  Espandere il file cab.

    ```
    mkdir .\TrustedTPM
    expand.exe -F:* <Path-To-TrustedTpm.cab> .\TrustedTPM
    ```

3.  Per impostazione predefinita, lo script di configurazione installerà i certificati per tutti i fornitori TPM. Se si desidera importare i certificati per il fornitore specifico di TPM, eliminare le cartelle per i fornitori TPM non considerati attendibili dall'organizzazione.

4.  Installare il pacchetto di certificati attendibili eseguendo lo script di installazione nella cartella espansa.

    ```
    cd .\TrustedTPM
    .\setup.cmd
    ```

Per aggiungere nuovi certificati o quelle intenzionalmente ignorata durante un'installazione precedente, è sufficiente ripetere i passaggi precedenti in ogni nodo del cluster del servizio HGS.
I certificati esistenti verranno mantenuti attendibili, ma sono stati trovati nel file cab espansa nuovi certificati verranno aggiunti al modulo TPM attendibile Archivia.

## <a name="next-step"></a>Passaggio successivo

> [!div class="nextstepaction"]
> [Configurare il DNS di infrastruttura](guarded-fabric-configuring-fabric-dns-tpm.md)



