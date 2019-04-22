---
title: Creare pacchetti di dati del server di contenuti per contenuti file e Web (facoltativo)
description: In questa guida vengono fornite istruzioni sulla distribuzione di BranchCache in modalità cache ospitata sul computer che eseguono Windows Server 2016 e Windows 10
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-bc
ms.topic: article
ms.assetid: 31e8428f-a482-4734-be1b-213912e34825
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 4b8cd284a83736d17859968947f381af171fd6bc
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59817172"
---
# <a name="create-content-server-data-packages-for-web-and-file-content-optional"></a>Creare pacchetti di dati del server di contenuti per contenuti file e Web (facoltativo)

>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

È possibile utilizzare questa procedura per prehash contenuto nel server Web e file e quindi creare pacchetti di dati da importare nel server cache ospitata. 

Questa procedura è facoltativa perché non si deve prehash e precaricamento contenuto nei server cache ospitata. Se è non precaricare contenuto, i dati viene aggiunto alla cache ospitata automaticamente come client scaricano tramite una connessione WAN.

Questa procedura include istruzioni per prehashing di contenuti sul server Web sia file server. Se non si dispone di uno di questi tipi di server di contenuti, non è necessario eseguire le istruzioni per quel tipo di server di contenuti.

>[!IMPORTANT]
>Prima di eseguire questa procedura, è necessario installare e configurare BranchCache sui server di contenuti. Inoltre, se si prevede di modificare il segreto server su un server di contenuti, eseguire questa operazione prima pre\-hash contenuto: modifica il segreto server invalida precedentemente\-hash generato.

Per eseguire questa procedura, è necessario essere un membro del gruppo Administrators.

## <a name="to-create-content-server-data-packages"></a>Per creare pacchetti di dati server contenuto

1. In ogni server di contenuti, individuare le cartelle e file che si desidera prehash e aggiungere a un pacchetto di dati. Identificare o creare una cartella in cui si desidera salvare il pacchetto di dati più avanti in questa procedura.

2. Nel computer del server, aprire Windows PowerShell con privilegi di amministratore.

3. Eseguire una o entrambe le operazioni seguenti, a seconda dei tipi di server di contenuti che è necessario:

    > [!NOTE]
    > Il valore per il percorso – parametro è la cartella in cui si trova il contenuto. È necessario sostituire i valori di esempio nei comandi di seguito con un percorso di cartella valido nel server di contenuti che contiene i dati che si desidera prehash e aggiungere a un pacchetto.
  
    - Se il contenuto che si desidera prehash è in un file server, digitare il comando seguente e quindi premere INVIO.

        ```  
        Publish-BCFileContent -Path D:\share -StageData
        ```  

    -   Se il contenuto che si desidera prehash è in un server Web, digitare il comando seguente e quindi premere INVIO.

        ```  
        Publish-BCWebContent –Path D:\inetpub\wwwroot -StageData
        ```  

4. Creare il pacchetto di dati eseguendo il comando seguente in ognuno dei server di contenuti. Sostituire il valore di esempio \(unità d:\\temp\) – parametro di destinazione con il percorso in cui è stato identificato o creato all'inizio di questa procedura.

    ```  
    Export-BCDataPackage –Destination D:\temp
    ```  

5. Dal server di contenuti, accedere alla condivisione sul server cache ospitata in cui si desidera precaricare contenuto e copiare i pacchetti di dati per le condivisioni nei server cache ospitata.

Per continuare con questa Guida, vedere [Importa pacchetti di dati nel Server Cache ospitata & #40; facoltativo & #41;](9-Bc-Import-Data.md).

