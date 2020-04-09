---
title: Distribuire una condivisione file di controllo in Windows Server 2019
description: I witness di condivisione file consentono di usare una condivisione file per votare il quorum del cluster. Questo argomento descrive i witness di condivisione file e le nuove funzionalità, tra cui l'uso di un'unità USB connessa a un router come condivisione file di controllo.
ms.prod: windows-server
manager: eldenc
ms.technology: failover-clustering
ms.topic: article
author: johnmarlin-msft
ms.author: johnmar
ms.date: 01/24/2019
ms.localizationpriority: medium
ms.openlocfilehash: 63e016b8e00482529e69aaa12727f854afd51e41
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80827674"
---
# <a name="deploy-a-file-share-witness"></a>Distribuire una condivisione file di controllo

> Si applica a: Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Una condivisione file di controllo è una condivisione SMB utilizzata dal cluster di failover come voto nel quorum del cluster. Questo argomento fornisce una panoramica della tecnologia e delle nuove funzionalità di Windows Server 2019, tra cui l'uso di un'unità USB connessa a un router come condivisione file di controllo.

I witness di condivisione file sono utili nelle circostanze seguenti:  

- Non è possibile usare un cloud di controllo perché non tutti i server del cluster hanno una connessione Internet affidabile
- Non è possibile usare un disco di controllo perché non sono presenti unità condivise da usare per un disco di controllo. Potrebbe trattarsi di un cluster Spazi di archiviazione diretta, SQL Server Always On gruppi di disponibilità (AG), gruppo di disponibilità del database di Exchange (DAG) e così via.  Nessuno di questi tipi di cluster usa dischi condivisi.

## <a name="file-share-witness-requirements"></a>Requisiti di controllo della condivisione file

È possibile ospitare una condivisione file di controllo in un server Windows aggiunto a un dominio o se il cluster esegue Windows Server 2019, qualsiasi dispositivo in grado di ospitare una condivisione file SMB 2 o successiva.

|Tipo di file server                 | Cluster supportati |
|---------------------------------|--------------------|
|Qualsiasi dispositivo con una condivisione file SMB 2 | Windows Server 2019|
|Windows Server aggiunto a un dominio     | Windows Server 2008 e versioni successive|

Se il cluster esegue Windows Server 2019, di seguito sono riportati i requisiti:

- Una condivisione file SMB *su qualsiasi dispositivo che usa il protocollo SMB 2 o versione successiva*, tra cui:
    - Dispositivi NAS (Network Attached Storage)
    - Computer Windows aggiunti a un gruppo di lavoro
    - Router con archiviazione USB connessa localmente
- Un account locale nel dispositivo per l'autenticazione del cluster
- Se invece si usa Active Directory per autenticare il cluster con la condivisione file, l'oggetto nome cluster (oggetto nome cluster) deve disporre delle autorizzazioni di scrittura per la condivisione e il server deve trovarsi nella stessa foresta Active Directory del cluster
- La condivisione file ha un minimo di 5 MB di spazio disponibile

Se il cluster esegue Windows Server 2016 o versione precedente, di seguito sono riportati i requisiti:

- Condivisione file SMB *in un server Windows aggiunto alla stessa foresta Active Directory del cluster*
- L'oggetto nome cluster (oggetto nome cluster) deve disporre delle autorizzazioni di scrittura per la condivisione
- La condivisione file ha un minimo di 5 MB di spazio disponibile

Altre note:
- Per usare una condivisione file di controllo ospitata da dispositivi diversi da Windows Server aggiunto a un dominio, attualmente è necessario usare il cmdlet di PowerShell **set-ClusterQuorum-Credential** per impostare il server di controllo del mirroring, come descritto più avanti in questo argomento.
- Per la disponibilità elevata, è possibile usare una condivisione file di controllo in un cluster di failover separato
- La condivisione file può essere usata da più cluster
- L'utilizzo di una condivisione file system distribuito (DFS) o di un archivio replicato non è supportato con alcuna versione del clustering di failover.  Questi possono causare una situazione di divisione dei cervelli in cui i server del cluster vengono eseguiti in modo indipendente l'uno dall'altro e possono causare la perdita di dati.

## <a name="creating-a-file-share-witness-on-a-router-with-a-usb-device"></a>Creazione di una condivisione file di controllo in un router con un dispositivo USB

In [Microsoft ignite 2018](https://azure.microsoft.com/ignite/), [DataOn storage](http://www.dataonstorage.com/) disponeva di un cluster spazi di archiviazione diretta nell'area chiosco multimediale.  Questo cluster è stato connesso a un router [Netgear](https://www.netgear.com) Nighthawk X4S Wi-Fi usando la porta USB come una condivisione file di controllo simile a questa.

![Server di controllo NetGear](media/File-Share-Witness/FSW1.png)

Di seguito sono elencati i passaggi per la creazione di un controllo di condivisione file usando un dispositivo USB in questo particolare router.  Si noti che i passaggi su altri router e appliance NAS variano e devono essere eseguiti usando le direzioni fornite dal fornitore.


1. Accedere al router con il dispositivo USB collegato.

   ![Interfaccia NetGear](media/File-Share-Witness/FSW2.png)

2. Dall'elenco di opzioni selezionare ReadySHARE, in cui è possibile creare le condivisioni.

   ![ReadySHARE NetGear](media/File-Share-Witness/FSW3.png)

3. Per una condivisione file di controllo, è sufficiente una condivisione di base.  Se si seleziona il pulsante modifica, viene visualizzata una finestra di dialogo in cui è possibile creare la condivisione sul dispositivo USB.

   ![Interfaccia di condivisione NetGear](media/File-Share-Witness/FSW4.png)

4. Dopo aver selezionato il pulsante Applica, la condivisione viene creata e può essere visualizzata nell'elenco.

   ![Condivisioni NetGear](media/File-Share-Witness/FSW5.png)

5. Una volta creata la condivisione, la creazione della condivisione file di controllo per il cluster viene eseguita con PowerShell.

   ```PowerShell
   Set-ClusterQuorum -FileShareWitness \\readyshare\Witness -Credential (Get-Credential)
   ```

   Verrà visualizzata una finestra di dialogo in cui è possibile immettere l'account locale nel dispositivo.

Questi stessi passaggi simili possono essere eseguiti in altri router con funzionalità USB, dispositivi NAS o altri dispositivi Windows.
