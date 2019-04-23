---
title: Distribuire una condivisione File di controllo in Windows Server 2019
ms.prod: windows-server-threshold
ms.manager: eldenc
ms.technology: failover-clustering
ms.topic: article
author: johnmarlin-msft
ms.date: 01/24/2019
description: Condivisioni file di controllo consentono di usare una condivisione file di voto di quorum del cluster. Questo argomento descrive le nuove funzionalità, incluso l'uso di un'unità USB connessa a un router come controllo di condivisione file e condivisioni file di controllo.
ms.localizationpriority: medium
ms.openlocfilehash: 1888142f96208800a0417c9caeea89e8a0472e88
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59831752"
---
# <a name="deploy-a-file-share-witness"></a>Distribuire una condivisione file di controllo

> Si applica a: Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Controllo di condivisione file è una condivisione SMB che il Cluster di Failover Usa come il voto del quorum del cluster. In questo argomento viene fornita una panoramica della tecnologia e le nuove funzionalità di Windows Server 2019, incluso l'uso di un'unità USB connessa a un router come controllo di condivisione file.

Condivisioni file di controllo è utili nelle situazioni seguenti:  

- Un cloud di controllo non può essere usato perché non tutti i server del cluster dispongano di una connessione Internet affidabile
- Impossibile utilizzare un disco di controllo perché non ci sono eventuali unità condivise da utilizzare per un disco di controllo. Potrebbe trattarsi di un cluster spazi di archiviazione diretta, SQL Server Always in gruppi di disponibilità (AG), gruppo di disponibilità del Database Exchange (DAG) e così via.  Nessuno di questi tipi di cluster di usare i dischi condivisi.

## <a name="file-share-witness-requirements"></a>I requisiti di controllo di condivisione di file

È possibile ospitare un controllo di condivisione file in un server di Windows aggiunti a un dominio o se il cluster è in esecuzione Windows Server 2019, tutti i dispositivi che possa host un SMB 2 o versione successiva nella condivisione file.

|Tipo di file server                 | Cluster supportato |
|---------------------------------|--------------------|
|Qualsiasi dispositivo w/un SMB 2 in una condivisione | Windows Server 2019|
|Dominio Windows Server     | Windows Server 2008 e versioni successive|

Se il cluster è in esecuzione Windows Server 2019, ecco i requisiti:

- Una condivisione file SMB *su qualsiasi dispositivo che usa il protocollo versione successiva o SMB 2*, tra cui:
    - Dispositivi di archiviazione collegati alla rete (NAS)
    - I computer Windows aggiunti a un gruppo di lavoro
    - Router con archiviazione tramite dispositivo USB collegato localmente
- Un account locale nel dispositivo per l'autenticazione del cluster
- Se invece si usa Active Directory per l'autenticazione del cluster con la condivisione file, l'oggetto nome Cluster (CNO) deve disporre delle autorizzazioni di scrittura nella condivisione e il server deve essere nella stessa foresta Active Directory al cluster
- La condivisione file disponga di almeno 5 MB di spazio libero

Se il cluster è in esecuzione Windows Server 2016 o versioni precedenti, ecco i requisiti:

- Condivisione file SMB *su un server Windows aggiunto alla stessa foresta di Active Directory del cluster*
- L'oggetto nome Cluster (CNO) deve disporre delle autorizzazioni di scrittura nella condivisione
- La condivisione file disponga di almeno 5 MB di spazio libero

Altre note:
- Per usare una condivisione file di controllo ospitate da dispositivi diversi da un server di Windows aggiunti a un dominio, è attualmente necessario usare il **Set-ClusterQuorum-Credential** cmdlet di PowerShell per impostare il tipo di controllo, come descritto più avanti in questo argomento.
- Per la disponibilità elevata, è possibile usare una condivisione file di controllo in un Cluster di Failover separato
- La condivisione file è utilizzabile da più cluster
- L'uso di una condivisione di File System distribuito (DFS) o archiviazione replicata non è supportato con le versioni del clustering di failover.  Tali può causare una situazione Perle di divisione in cui i server del cluster sono in esecuzione indipendentemente uno da altro e può provocare la perdita di dati.

## <a name="creating-a-file-share-witness-on-a-router-with-a-usb-device"></a>Creazione di un controllo di condivisione file su un router con un dispositivo USB

Alla [Microsoft Ignite 2018](https://azure.microsoft.com/ignite/), [DataOn archiviazione](http://www.dataonstorage.com/) è stato rilevato un Cluster di spazi di archiviazione diretta in base alla relativa area per chiosco multimediale.  In questo cluster è stato connesso a un [NetGear](https://www.netgear.com) Nighthawk X4S Wi-Fi Router utilizza la porta USB di un file di controllo analogo al seguente di condivisione.

![Controllo del mirroring NetGear](media\File-Share-Witness\FSW1.png)

I passaggi per la creazione di un controllo di condivisione file usando un dispositivo USB su questo particolare router sono elencati di seguito.  Si noti che questa procedura su altri router e i dispositivi NAS varia e deve essere eseguita mediante il fornitore fornito le direzioni.


1. Accedere al router con il dispositivo USB collegato.

   ![Interfaccia NetGear](media\File-Share-Witness\FSW2.png)

2. Nell'elenco delle opzioni, selezionare ReadySHARE ovvero dove è possibile creare condivisioni.

   ![NetGear ReadySHARE](media\File-Share-Witness\FSW3.png)

3. Per un controllo di condivisione file, una condivisione di base è tutto ciò che serve.  Selezione del pulsante di modifica viene visualizzata una finestra di dialogo in cui è possibile creare la condivisione nel dispositivo USB.

   ![Interfaccia NetGear condivisione](media\File-Share-Witness\FSW4.png)

4. Dopo aver selezionato il pulsante Applica, la condivisione viene creata e può essere visualizzata nell'elenco.

   ![Condivisioni NetGear](media\File-Share-Witness\FSW5.png)

5. Dopo aver creata la condivisione, Creazione condivisione file di controllo per il Cluster viene eseguita con PowerShell.

   ```PowerShell
   Set-ClusterQuorum -FileShareWitness \\readyshare\Witness -Credential (Get-Credential)
   ```

   Verrà visualizzata una finestra di dialogo per immettere l'account locale nel dispositivo.

Questi stessi passaggi simile possono avvenire su altri router con funzionalità USB, i dispositivi NAS o altri dispositivi Windows.
