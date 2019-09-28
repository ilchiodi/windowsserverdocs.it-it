---
title: Installare i certificati radice TPM attendibili
ms.custom: na
ms.prod: windows-server
ms.topic: article
manager: dongill
author: rpsqrd
ms.technology: security-guarded-fabric
ms.date: 06/27/2019
ms.openlocfilehash: 15614ce1065170bc557fad10a168b3dda6a5b05a
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71386553"
---
# <a name="install-trusted-tpm-root-certificates"></a>Installare i certificati radice TPM attendibili

>Si applica a: Windows Server 2019, Windows Server (canale semestrale), Windows Server 2016

Quando si configura HGS per l'uso dell'attestazione TPM, è necessario anche configurare HGS in modo da considerare attendibili i fornitori di TPMs nei server.
Questo processo di verifica aggiuntivo garantisce che solo TPMs attendibili e affidabili possano attestare con il HGS.
Se si tenta di registrare un TPM non trusted con `Add-HgsAttestationTpmHost`, verrà visualizzato un errore che indica che il fornitore del TPM non è attendibile.

Per considerare attendibile la TPMs, è necessario installare i certificati radice e di firma intermedia usati per firmare la chiave di verifica dell'autenticità nel TPMs server in HGS.
Se si usa più di un modello TPM nel Data Center, potrebbe essere necessario installare certificati diversi per ogni modello.
HGS cercherà negli archivi certificati "TrustedTPM_RootCA" e "TrustedTPM_IntermediateCA" per i certificati del fornitore.

> [!NOTE]
> I certificati del fornitore TPM sono diversi da quelli installati per impostazione predefinita in Windows e rappresentano i certificati radice e intermedi specifici utilizzati dai fornitori di TPM.

Una raccolta di certificati di livello intermedio e radice TPM attendibile viene pubblicata da Microsoft per praticità.
È possibile utilizzare la procedura seguente per installare questi certificati.
Se i certificati TPM non sono inclusi nel pacchetto riportato di seguito, rivolgersi al fornitore del TPM o al server OEM per ottenere i certificati radice e intermedi per il modello TPM specifico.

Ripetere i passaggi seguenti in **ogni server HGS**:

1.  Scaricare il pacchetto più recente da [https://go.microsoft.com/fwlink/?linkid=2097925](https://go.microsoft.com/fwlink/?linkid=2097925).

2.  Verificare la firma del file CAB per garantirne l'autenticità. Non continuare se la firma non è valida.

    ```powershell
    Get-AuthenticodeSignature .\TrustedTpm.cab
    ```
    
    Ecco un output di esempio:
    
    ```
    Directory: C:\Users\Administrator\Downloads
        
    SignerCertificate                         Status                                 Path
    -----------------                         ------                                 ----
    0DD6D4D4F46C0C7C2671962C4D361D607E370940  Valid                                  TrustedTpm.cab
    ```

2.  Espandere il file CAB.

    ```
    mkdir .\TrustedTPM
    expand.exe -F:* <Path-To-TrustedTpm.cab> .\TrustedTPM
    ```

3.  Per impostazione predefinita, lo script di configurazione installerà i certificati per ogni fornitore TPM. Se si desidera importare solo certificati per il fornitore TPM specifico, eliminare le cartelle per i fornitori di TPM non ritenuti attendibili dall'organizzazione.

4.  Installare il pacchetto di certificati attendibili eseguendo lo script di installazione nella cartella espansa.

    ```
    cd .\TrustedTPM
    .\setup.cmd
    ```

Per aggiungere nuovi certificati o uno intenzionalmente ignorato durante un'installazione precedente, è sufficiente ripetere i passaggi precedenti in ogni nodo del cluster HGS.
I certificati esistenti rimarranno attendibili, ma i nuovi certificati trovati nel file cab espanso verranno aggiunti agli archivi TPM attendibili.

## <a name="next-step"></a>Passaggio successivo

> [!div class="nextstepaction"]
> [Configurare il DNS di infrastruttura](guarded-fabric-configuring-fabric-dns-tpm.md)



