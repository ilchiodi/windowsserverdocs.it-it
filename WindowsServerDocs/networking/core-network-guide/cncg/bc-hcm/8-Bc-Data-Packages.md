---
title: Creare contenuto pacchetti di dati di Server Web e File di contenuto (facoltativo)
description: Questa guida vengono fornite istruzioni sulla distribuzione di BranchCache in modalità cache ospitata in computer che eseguono Windows Server 2016 e Windows 10
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-bc
ms.topic: article
ms.assetid: 31e8428f-a482-4734-be1b-213912e34825
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 8f814bbac5c74081259d8eef6deda79d914bfec7
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/28/2018
---
# <a name="create-content-server-data-packages-for-web-and-file-content-optional"></a>Creare contenuto pacchetti di dati di Server Web e File di contenuto (facoltativo)

>Si applica a: Windows Server (canale annuale e virgola), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

È possibile utilizzare questa procedura per prehash contenuto nel server Web e file e quindi creare i pacchetti di dati per importare nel server cache ospitata. 

Questa procedura è facoltativa perché non si deve prehash e precaricamento contenuto nei server cache ospitata. Se non si precaricare contenuto, i dati viene aggiunto alla cache ospitata automaticamente come client scaricano tramite una connessione WAN.

Questa procedura vengono fornite istruzioni per prehashing di contenuto nel file server e i server Web. Se non hai uno di questi tipi di server di contenuti, non è necessario eseguire le istruzioni per quel tipo di server di contenuti.

>[!IMPORTANT]
>Prima di eseguire questa procedura, è necessario installare e configurare BranchCache sui server di contenuti. Inoltre, se prevede di modificare il segreto server su un server di contenuti, eseguire questa operazione prima pre-hash contenuto: modifica il segreto server invalida hash generato previously\.

Per eseguire questa procedura, è necessario essere un membro del gruppo Administrators.

## <a name="to-create-content-server-data-packages"></a>Per creare pacchetti di dati di server di contenuti

1. In ogni server di contenuti, individuare il file e cartelle che si desidera prehash e aggiungere a un pacchetto di dati. Identificare o creare una cartella in cui si desidera salvare il pacchetto di dati più avanti in questa procedura.

2. Nel computer del server, aprire Windows PowerShell con privilegi di amministratore.

3. Effettuare una o entrambe le opzioni seguenti, a seconda i tipi di server di contenuti che hai:

    > [!NOTE]
    > Il valore – percorso parametro è la cartella in cui si trova il contenuto. È necessario sostituire i valori di esempio nei comandi sotto con un percorso di cartella valido nel server di contenuti che contiene i dati che si desidera prehash e aggiungere a un pacchetto.
  
    - Se il contenuto che si desidera prehash in un file server, digitare il comando seguente e quindi premere INVIO.

        ```  
        Publish-BCFileContent -Path D:\share -StageData
        ```  

    -   Se il contenuto che si desidera prehash in un server Web, digitare il comando seguente e quindi premere INVIO.

        ```  
        Publish-BCWebContent –Path D:\inetpub\wwwroot -StageData
        ```  

4. Creare il pacchetto di dati eseguendo il comando seguente in ciascuno dei server di contenuti. Sostituire il \(D:\\temp\) valore riportato per – parametro di destinazione con il percorso che è stato identificato o creato all'inizio di questa procedura.

    ```  
    Export-BCDataPackage –Destination D:\temp
    ```  

5. Dal server di contenuti, accedere alla condivisione nel server cache ospitata in cui si desidera precaricare contenuto e copiare i pacchetti di dati per le condivisioni nei server cache ospitata.

Per continuare con questa Guida, vedere [Importa pacchetti di dati nel Server Cache ospitata & #40; facoltativo & #41; ](9-Bc-Import-Data.md).

