---
title: Pianificazione della distribuzione della modalità Cache ospitata di BranchCache
description: In questa guida vengono fornite istruzioni sulla distribuzione di BranchCache in modalità cache ospitata sul computer che eseguono Windows Server 2016 e Windows 10
manager: brianlic
ms.prod: windows-server
ms.technology: networking-bc
ms.topic: article
ms.assetid: bc44a7db-f7a5-4e95-9d95-ab8d334e885f
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 0fe55bc9971606559af652d592a91db7a89544a7
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71356369"
---
# <a name="branchcache-hosted-cache-mode-deployment-planning"></a>Pianificazione della distribuzione della modalità Cache ospitata di BranchCache

>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

È possibile utilizzare questo argomento per pianificare la distribuzione di BranchCache in modalità Cache ospitata.

>[!IMPORTANT]
>Il server cache ospitata deve essere in esecuzione Windows Server 2016, Windows Server 2012 R2 o Windows Server 2012.

Prima di distribuire il server cache ospitata, è necessario pianificare gli elementi seguenti:

- [Pianificare la configurazione del server di base](#bkmk_basic)

- [Pianificare l'accesso al dominio](#bkmk_domain)

- [Pianificare la posizione e le dimensioni della cache ospitata](#bkmk_cachelocation)

- [Pianificare la condivisione in cui verranno copiati i pacchetti del server di contenuti](#bkmk_package)

- [Pianificare la creazione di pacchetti di dati e prehash in server di contenuti](#bkmk_prehash)

## <a name="bkmk_basic"></a>Pianificare la configurazione del server di base
  
Se prevede di utilizzare un server esistente nella filiale come server cache ospitata, non è necessario eseguire questo passaggio di pianificazione, poiché il computer è già denominato e dispone di un indirizzo IP.

Dopo aver installato Windows Server 2016 sul server cache ospitata, è necessario rinominare il computer e assegnare e configurare un indirizzo IP statico per il computer locale.

>[!NOTE]
>In questa Guida, il server cache ospitata è denominato HCS1, tuttavia, è necessario utilizzare un nome di server che è appropriato per la distribuzione.

## <a name="bkmk_domain"></a>Pianificare l'accesso al dominio

Se prevede di utilizzare un server esistente nella filiale come server cache ospitata, non occorre eseguire questo passaggio della pianificazione, a meno che il computer non è attualmente connesso al dominio.
  
Per accedere al dominio, il computer deve essere un computer membro del dominio e l'account utente deve essere creato in Active Directory prima di tentare l'accesso. Inoltre, è necessario aggiungere il computer al dominio con un account con l'appartenenza al gruppo appropriato.

## <a name="bkmk_cachelocation"></a>Pianificare la posizione e le dimensioni della cache ospitata

Nella HCS1, determinare dove sul server cache ospitata si desidera posizionare la cache ospitata. Decidere, ad esempio, il disco rigido, volume e percorso della cartella in cui si prevede di archiviare la cache.

Inoltre, decidere quale percentuale di spazio su disco da allocare per la cache ospitata.

## <a name="bkmk_package"></a>Pianificare la condivisione in cui verranno copiati i pacchetti del server di contenuti

Dopo aver creato i pacchetti di dati nei server del contenuto, è necessario copiarli sulla rete in una condivisione sul server cache ospitata.

Pianificare il percorso della cartella e autorizzazioni di condivisione per la cartella condivisa. Inoltre, se il server di contenuti ospitare una grande quantità di dati e i pacchetti creati saranno file di grandi dimensioni, prevede di eseguire l'operazione di copia nelle ore di punta – {1>disattivato<1}. in modo che la larghezza di banda WAN non viene usato dall'operazione di copia di un periodo di tempo quando altre devono utilizzare la larghezza di banda per le operazioni aziendali normale.

## <a name="bkmk_prehash"></a>Pianificare la creazione di pacchetti di dati e prehash in server di contenuti

Si prehash contenuto sui server di contenuti, è necessario identificare le cartelle e file che contengono il contenuto che si desidera aggiungere al pacchetto di dati. 

Inoltre, è necessario pianificare il percorso della cartella locale in cui è possibile archiviare i pacchetti di dati prima di copiarli nel server cache ospitata.

Per continuare con questa Guida, vedere [distribuzione in modalità Cache ospitata BranchCache](4-Bc-Hcm-Deployment.md).
