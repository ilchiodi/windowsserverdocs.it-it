---
title: Creare pacchetti di dati del server di contenuti per contenuti file e Web (facoltativo)
description: In questa guida vengono fornite istruzioni sulla distribuzione di BranchCache in modalità cache ospitata sul computer che eseguono Windows Server 2016 e Windows 10
manager: brianlic
ms.prod: windows-server
ms.technology: networking-bc
ms.topic: article
ms.assetid: 31e8428f-a482-4734-be1b-213912e34825
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 104e3cfd0525c43857bb37d781f6b2475978238e
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71406391"
---
# <a name="create-content-server-data-packages-for-web-and-file-content-optional"></a>Creare pacchetti di dati del server di contenuti per contenuti file e Web (facoltativo)

>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

È possibile utilizzare questa procedura per eseguire la prehashing del contenuto sul Web e sui file server, quindi creare pacchetti di dati da importare nel server cache ospitata. 

Questa procedura è facoltativa perché non si deve prehash e precaricamento contenuto nei server cache ospitata. Se non si carica il contenuto, i dati vengono aggiunti automaticamente alla cache ospitata quando i client lo scaricano tramite la connessione WAN.

Questa procedura fornisce istruzioni per la prehashing del contenuto su file server e server Web. Se non si dispone di uno di questi tipi di server di contenuti, non è necessario eseguire le istruzioni per quel tipo di server di contenuti.

>[!IMPORTANT]
>Prima di eseguire questa procedura, è necessario installare e configurare BranchCache nei server di contenuti. Inoltre, se si prevede di modificare il segreto server su un server di contenuti, eseguire questa operazione prima pre\-hash contenuto: modifica il segreto server invalida precedentemente\-hash generato.

Per eseguire questa procedura, è necessario essere un membro del gruppo Administrators.

## <a name="to-create-content-server-data-packages"></a>Per creare pacchetti di dati del server di contenuti

1. In ogni server di contenuti individuare le cartelle e i file che si desidera prehash e aggiungere a un pacchetto di dati. Identificare o creare una cartella in cui si desidera salvare il pacchetto di dati più avanti in questa procedura.

2. Nel computer del server, aprire Windows PowerShell con privilegi di amministratore.

3. Eseguire una o entrambe le operazioni seguenti, a seconda dei tipi di server di contenuti:

    > [!NOTE]
    > Il valore del parametro – path è la cartella in cui si trova il contenuto. È necessario sostituire i valori di esempio nei comandi seguenti con un percorso di cartella valido sul server di contenuti che contiene i dati che si desidera prehash e aggiungere a un pacchetto.
  
    - Se il contenuto che si desidera prehash si trova in un file server, digitare il comando seguente e quindi premere INVIO.

        ```  
        Publish-BCFileContent -Path D:\share -StageData
        ```  

    -   Se il contenuto che si desidera prehash si trova in un server Web, digitare il comando seguente e quindi premere INVIO.

        ```  
        Publish-BCWebContent –Path D:\inetpub\wwwroot -StageData
        ```  

4. Creare il pacchetto di dati eseguendo il comando seguente in ogni server di contenuti. Sostituire il valore di esempio \(unità d:\\temp\) – parametro di destinazione con il percorso in cui è stato identificato o creato all'inizio di questa procedura.

    ```  
    Export-BCDataPackage –Destination D:\temp
    ```  

5. Dal server di contenuti accedere alla condivisione nei server cache ospitata in cui si desidera precaricare il contenuto e copiare i pacchetti di dati nelle condivisioni nei server cache ospitata.

Per continuare con questa Guida, vedere [Importa pacchetti di dati nel Server Cache ospitata & #40; facoltativo & #41;](9-Bc-Import-Data.md).

