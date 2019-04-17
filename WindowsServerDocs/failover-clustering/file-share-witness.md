---
title: Distribuire un controllo di condivisione File in Windows Server 2019
ms.prod: windows-server-threshold
ms.manager: eldenc
ms.technology: failover-clustering
ms.topic: article
author: johnmarlin-msft
ms.date: 01/24/2019
description: Testimoni di condivisione file consentono di usare una condivisione file di voto quorum del cluster. Questo argomento descrive testimoni condivisione di file e nuove funzionalità, tra cui usando un'unità USB collegata a un router come un controllo di condivisione file.
ms.localizationpriority: medium
ms.openlocfilehash: 1888142f96208800a0417c9caeea89e8a0472e88
ms.sourcegitcommit: d622f7af181ed0063d716b30278d41887a57db19
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/08/2019
ms.locfileid: "9150891"
---
# Distribuire una condivisione di controllo dei file

> Si applica a: Windows Server 2019 Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Un controllo di condivisione file è una condivisione SMB che utilizza Cluster di Failover come il voto il quorum del cluster. In questo argomento fornisce una panoramica della tecnologia e le nuove funzionalità in Windows Server 2019, tra cui usando un'unità USB collegata a un router come un controllo di condivisione file.

Testimoni di condivisione file sono utili nelle seguenti circostanze:  

- Un cloud di controllo non possono essere usate perché non tutti i server del cluster dispone di una connessione Internet affidabile
- Poiché non sono presenti qualsiasi unità condivisa da usare per un controllo disco è possibile usare un controllo disco. Può trattarsi di un cluster spazi di archiviazione diretta, SQL Server sempre sulla disponibilità gruppi (AG), Exchange Database disponibilità gruppo (DAG), e così via.  Nessuno di questi tipi di cluster utilizza i dischi condivisi.

## Requisiti di controllo di condivisione di file

È possibile ospitare un controllo di condivisione file in un server di Windows aggiunti al dominio o se il cluster è in esecuzione Windows Server 2019, qualsiasi dispositivo che possa condividere un 2 SMB host o un file successiva.

|Tipo di file server                 | Cluster supportati |
|---------------------------------|--------------------|
|Qualsiasi dispositivo w/un SMB 2 in una condivisione | Windows Server 2019|
|Aggiunto al dominio Windows Server     | Windows Server 2008 e versioni successive|

Se il cluster è in esecuzione Windows Server 2019, ecco i requisiti:

- Condividere un file SMB *su qualsiasi dispositivo che usa il SMB 2 o versione successiva protocollo*, tra cui:
    - Dispositivi NAS (NAS)
    - I computer Windows aggiunti a un gruppo di lavoro
    - Router con l'archiviazione USB connessi localmente
- Un account locale nel dispositivo per l'autenticazione del cluster
- Se invece usi Active Directory per l'autenticazione del cluster con la condivisione di file, l'oggetto di nome del Cluster (CNO) deve disporre delle autorizzazioni di scrittura nella condivisione di e il server deve essere nella stessa foresta di Active Directory come il cluster
- La condivisione file ha un minimo di 5 MB di spazio libero

Se il cluster è in esecuzione Windows Server 2016 o versioni precedenti, di seguito sono riportati i requisiti:

- Controllo di condivisione file SMB *in un server Windows aggiunto alla stessa foresta di Active Directory come il cluster*
- L'oggetto di nome del Cluster (CNO) deve disporre delle autorizzazioni di scrittura nella condivisione di
- La condivisione file ha un minimo di 5 MB di spazio libero

Altre note:
- Per usare un controllo di condivisione file ospitato da dispositivi diversi da un dominio Windows server, è attualmente devono utilizzare il **Set-ClusterQuorum-Credential** cmdlet di PowerShell per impostare il tipo di controllo, come descritto più avanti in questo argomento.
- Per una disponibilità elevata, puoi usare un controllo di condivisione file in un Cluster di Failover separato
- La condivisione di file può essere usata da più cluster
- L'uso di una condivisione di File System distribuito (DFS) o l'archiviazione replicata non è supportato con qualsiasi versione di clustering di failover.  Questi possono causare una situazione cervello doppia in cui i server di cluster eseguono indipendentemente da altro e potrebbero causare la perdita di dati.

## Creazione di un controllo di condivisione file in un router con un dispositivo USB

In [Microsoft Ignite 2018](https://azure.microsoft.com/ignite/), [Archiviazione DataOn](http://www.dataonstorage.com/) era un Cluster direttamente spazi di archiviazione nella relativa area di modalità tutto schermo.  Questo cluster è stato connesso a un Router di Wi-Fi [NetGear](https://www.netgear.com) Nighthawk X4S tramite la porta USB come un controllo di condivisione file simile al seguente.

![Controllo NetGear](media\File-Share-Witness\FSW1.png)

Di seguito sono elencati i passaggi per la creazione di un controllo di condivisione file con un dispositivo USB su questo particolare router.  Nota che i passaggi in altri router e dispositivi NAS variano e devono essere eseguiti mediante il fornitore fornite le indicazioni stradali.


1. Accedere al router con il dispositivo USB da rete elettrica.

   ![Interfaccia NetGear](media\File-Share-Witness\FSW2.png)

2. Nell'elenco delle opzioni, seleziona ReadySHARE che è in cui è possibile creare condivisioni.

   ![NetGear ReadySHARE](media\File-Share-Witness\FSW3.png)

3. Per un controllo di condivisione file, una condivisione di base è tutto ciò che è necessario.  La selezione del pulsante Modifica verrà visualizzata una finestra di dialogo in cui la condivisione di possa essere creata nel dispositivo USB.

   ![Interfaccia NetGear condivisione](media\File-Share-Witness\FSW4.png)

4. Dopo aver selezionato il pulsante Applica, la condivisione viene creata e possa essere visualizzata nell'elenco.

   ![Condivisioni NetGear](media\File-Share-Witness\FSW5.png)

5. Dopo aver creata la condivisione, creando il controllo di condivisione file per Cluster viene eseguita con PowerShell.

   ```PowerShell
   Set-ClusterQuorum -FileShareWitness \\readyshare\Witness -Credential (Get-Credential)
   ```

   Verrà visualizzata una finestra di dialogo per immettere l'account locale nel dispositivo.

Gli stessi passaggi simili possono essere eseguiti su altri router con funzionalità USB, dispositivi NAS o altri dispositivi Windows.
