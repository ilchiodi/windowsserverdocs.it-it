---
title: Panoramica della distribuzione in modalità Cache ospitata di BranchCache
description: In questa guida vengono fornite istruzioni sulla distribuzione di BranchCache in modalità cache ospitata sul computer che eseguono Windows Server 2016 e Windows 10
manager: brianlic
ms.prod: windows-server
ms.technology: networking-bc
ms.topic: article
ms.assetid: 55686a9c-60dd-47f4-9f1f-fe72c2873a44
ms.author: pashort
author: shortpatti
ms.openlocfilehash: dc6ade92eb5fe04271033973911ccb98e871d236
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71406377"
---
# <a name="branchcache-hosted-cache-mode-deployment-overview"></a>Panoramica della distribuzione in modalità Cache ospitata di BranchCache

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

È possibile utilizzare questa guida per distribuire un server cache ospitata di BranchCache in una succursale in cui i computer vengono aggiunti a un dominio. È possibile utilizzare questo argomento per ottenere una panoramica del processo di distribuzione modalità Cache ospitata BranchCache.

In questa panoramica include l'infrastruttura di BranchCache che è necessario, nonché una panoramica dettagliata semplice della distribuzione.

## <a name="bkmk_components"></a>Infrastruttura di distribuzione del server cache ospitata

In questa distribuzione, il server cache ospitata viene distribuito tramite punti di connessione del servizio in servizi di dominio Active Directory \(AD DS\), è disponibile l'opzione con BranchCache in Windows Server 2016, Windows Server 2012 R2 e Windows Server 2012, per prehash il contenuto condiviso nel Web e file server basati su contenuto, quindi caricare il contenuto nel server cache ospitata.

La figura seguente mostra l'infrastruttura necessaria per distribuire un server cache ospitata di BranchCache.

![Cenni preliminari sulla modalità Cache ospitata BranchCache](../../../media/BranchCache-Hcm-Overview/Bc-Hcm-Overview.jpg)

> [!IMPORTANT]
> Sebbene questa distribuzione illustra i server di contenuti in un data center cloud, è possibile utilizzare questa guida per distribuire un server cache ospitata BranchCache indipendentemente in cui distribuire il server di contenuti – nella sede principale o in una posizione sul cloud.

### <a name="hcs1-in-the-branch-office"></a>HCS1 nella succursale

È necessario configurare il computer come server cache ospitata. Se si sceglie di prehash dati server contenuto in modo che è possibile precaricare il contenuto nei server cache ospitata, è possibile importare pacchetti di dati che contengono il contenuto dai server Web e file.

### <a name="web1-in-the-cloud-data-center"></a>Web1 nel data center cloud

Web1 è un BranchCache\-contenuto server abilitato. Se si sceglie di prehash dati server contenuto in modo che è possibile precaricare il contenuto nei server cache ospitata, è possibile prehash il contenuto condiviso in WEB1, quindi creare un pacchetto di dati che verranno copiati HCS1.

### <a name="file1-in-the-cloud-data-center"></a>File1 nel data center cloud

File1 è un BranchCache\-contenuto server abilitato. Se si sceglie di prehash dati server contenuto in modo che è possibile precaricare il contenuto nei server cache ospitata, è possibile prehash il contenuto condiviso in FILE1, quindi creare un pacchetto di dati che verranno copiati HCS1.
  
### <a name="dc1-in-the-main-office"></a>DC1 nella sede centrale

DC1 è un controller di dominio, ed è necessario configurare il criterio dominio predefinito o un altro criterio che risulta più appropriato per la distribuzione, con le impostazioni di criteri di gruppo per abilitare ospitato Cache il rilevamento automatico dal punto di connessione del servizio.

Quando i computer client nel ramo dispongano di criteri di gruppo aggiornato e viene applicata questa impostazione di criteri, vengono automaticamente individuare e iniziare a utilizzare il server cache ospitata nella succursale.

### <a name="client-computers-in-the-branch-office"></a>Computer client nella succursale

È necessario aggiornare i criteri di gruppo nei computer client per applicare nuove impostazioni di criteri di gruppo e consentire ai client di individuare e utilizzare il server cache ospitata.

## <a name="bkmk_overview"></a>Panoramica del processo di distribuzione del server cache ospitata

>[!NOTE]
>Vengono forniti i dettagli di come eseguire questi passaggi nella sezione [distribuzione in modalità Cache ospitata BranchCache](4-Bc-Hcm-Deployment.md).

Si verifica il processo di distribuzione di un Server Cache ospitata di BranchCache in queste fasi:

>[!NOTE]
>Alcuni dei passaggi riportati di seguito sono facoltativi, ad esempio i passaggi che illustrano come prehash e a precaricare contenuto nel server cache ospitata. Quando si distribuisce BranchCache in modalità cache ospitata, non si deve prehash contenuto dei server Web e file di contenuto, per creare un pacchetto di dati e per importare il pacchetto di dati per precaricare i server cache ospitata con contenuto. I passaggi sono indicati come facoltativi in questa sezione e nella sezione [distribuzione in modalità Cache ospitata BranchCache](4-Bc-Hcm-Deployment.md) in modo che è possibile ignorare se si preferisce.

1. Nel HCS1, utilizzare i comandi di Windows PowerShell per configurare il computer come server cache ospitata e di registrare un punto di connessione del servizio in Active Directory.

2. \(facoltativo\) in HCS1, se i valori predefiniti di BranchCache non corrispondono agli obiettivi di distribuzione per il server e la cache ospitata, configurare la quantità di spazio su disco che si desidera allocare per la cache ospitata. Configurare inoltre il percorso del disco che si preferisce per la cache ospitata.

3. \(\) contenuto prehash facoltativo nei server di contenuti, creare pacchetti di dati e precaricare il contenuto nel server cache ospitata.

    > [!NOTE]
    > Prehashing e precaricamento dei contenuti sul server cache ospitata è facoltativo, tuttavia se si sceglie di prehash e precaricamento, è necessario eseguire tutte le operazioni descritte di seguito che sono applicabili alla distribuzione. \(ad esempio, se non si dispone di server Web, non è necessario eseguire alcuna procedura relativa alla prehashing e al precaricamento del contenuto del server Web.\)

    1. In WEB1 prehash contenuto server Web e creare un pacchetto di dati.

    2. In FILE1, prehash contenuto del file server e creare un pacchetto di dati.

    3. Copiare i pacchetti di dati per il server cache ospitata HCS1 WEB1 e FILE1.

    4. In HCS1, importare i pacchetti di dati per precaricare la cache dei dati.

4. Su DC1, configurare un dominio computer client della filiale per la modalità cache ospitata tramite la configurazione dei criteri di gruppo con le impostazioni dei criteri di BranchCache.

5. Nei computer client, aggiornare criteri di gruppo.

Per continuare con questa Guida, vedere [distribuzione in modalità di Cache ospitata BranchCache pianificazione](3-Bc-Hcm-Plan.md).