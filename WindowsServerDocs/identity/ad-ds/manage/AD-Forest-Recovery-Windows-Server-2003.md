---
title: Ripristino della foresta di Active Directory-ripristino di Windows Server 2003
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 07/07/2017
ms.topic: article
ms.prod: windows-server
ms.assetid: 5a291f65-794e-4fc3-996e-094c5845a383
ms.technology: identity-adds
ms.openlocfilehash: 05fece3093d36073358d0d1822559c5b030085d7
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80823344"
---
# <a name="ad-forest-recovery---windows-server-2003-recovery"></a>Ripristino della foresta di Active Directory-ripristino di Windows Server 2003

>Si applica a: Windows Server 2003

Questo argomento include le procedure di ripristino della foresta per i controller di dominio che eseguono Windows Server 2003. Il processo generale per il ripristino della foresta non è diverso con i controller di dominio di Windows Server 2003, ma le procedure specifiche possono variare a causa di strumenti diversi. Ad esempio, è possibile usare Ntdsutil. exe per eseguire il backup e il ripristino dei controller di dominio che eseguono i controller di dominio Windows Server 2003, mentre Windows Server Backup o Wbadmin. exe viene usato per i controller di dominio che eseguono Windows Server 2008 o versioni successive.  
  
- [Backup dei dati sullo stato del sistema](#backing-up-the-system-state-data)  
- [Esecuzione di un ripristino non autorevole](#performing-a-nonauthoritative-restore)  
- [Installare e configurare il servizio server DNS](#install-and-configure-the-dns-server-service)

## <a name="backing-up-the-system-state-data"></a>Backup dei dati sullo stato del sistema
Usare la procedura seguente per eseguire il backup dei dati dello stato del sistema, insieme a tutti gli altri dati selezionati per l'operazione di backup corrente, di un controller di dominio che esegue Windows Server 2003. Windows Server 2003 include lo strumento Ntbackup, che è possibile usare per eseguire il backup dei dati dello stato del sistema.  
  
Per eseguire il backup di file e cartelle, è necessaria almeno l'appartenenza a **amministratori** o **operatori di backup**o a un gruppo equivalente.   
  
Se si esegue il backup dei dati dello stato del sistema su un nastro e il programma di backup indica che non è disponibile alcun supporto inutilizzato, potrebbe essere necessario utilizzare archivi rimovibili. In questo modo, il nastro viene aggiunto al pool di supporti libero in modo che possa essere usato dal backup.  
  
È possibile eseguire il backup solo dei dati dello stato del sistema in un computer locale. Non è possibile eseguire il backup in un computer remoto.  
  
### <a name="to-back-up-the-system-state-data-on-a-domain-controller-that-runs-windows-server-2003"></a>Per eseguire il backup dei dati dello stato del sistema in un controller di dominio che esegue Windows Server 2003  
  
1. Fare clic sul pulsante **Start**, scegliere **tutti i programmi**, **Accessori**, **utilità di sistema**, quindi fare clic su **backup**.  
2. Nella pagina di **benvenuto** fare clic su **modalità avanzata**.  
3. Nella scheda **backup** selezionare la casella di controllo relativa a un'unità, a una cartella o a un file di cui si desidera eseguire il backup.  
4. Selezionare la casella di controllo **stato del sistema** .  
5. Fare clic su **Avvia backup**.  
  
## <a name="performing-a-nonauthoritative-restore"></a>Esecuzione di un ripristino non autorevole  

Utilizzare la procedura seguente per eseguire un ripristino non autorevole di un controller di dominio che esegue Windows Server 2003. Eseguendo un ripristino non autorevole per Active Directory in Windows Server 2003, viene eseguito automaticamente un ripristino non autorevole di SYSVOL. Non sono necessari passaggi aggiuntivi.  
  
> [!NOTE]
> Se si reinstalla anche il sistema operativo Windows Server 2003, è possibile o meno aggiungere il computer al dominio ed è possibile assegnare qualsiasi nome al computer durante l'installazione del sistema operativo. Non installare Active Directory. Dopo aver reinstallato il sistema operativo, andare direttamente al passaggio 4.  
  
Nei controller di dominio Windows Server 2003 in cui sono stati ripristinati solo i dati relativi allo stato del sistema, è necessario reinstallare anche eventuali applicazioni software in esecuzione nei controller di dominio prima del ripristino. Il ripristino di servizi di dominio Active Directory nel primo controller di dominio nel dominio ripristina anche il registro di sistema perché entrambi fanno parte dei dati sullo stato del sistema. Tenere presente questo aspetto se si dispone di applicazioni in esecuzione in questi controller di dominio e se sono presenti informazioni archiviate nel registro di sistema.  
  
Per ridurre il tempo necessario per reinstallare il software, determinare se le applicazioni che devono essere installate nei controller di dominio sono compatibili con la clonazione del controller di dominio virtuale. Tali applicazioni possono essere installate nel controller di dominio di origine prima della clonazione per risparmiare tempo e impegno necessario per installarle nei controller di dominio virtuali clonati.  
  
### <a name="to-perform-a-nonauthoritative-restore"></a>Per eseguire un ripristino non autorevole
  
1. Dopo l'avvio del controller di dominio, premere F8 per riavviare il computer in modalità ripristino servizi directory.  
2. Selezionare **modalità ripristino servizi directory (solo controller di dominio Windows)** .  
3. Selezionare il sistema operativo che si desidera avviare in modalità di ripristino.  
4. Accedere come amministratore. è possibile utilizzare solo un account computer locale, ma non è disponibile alcuna opzione di accesso al dominio.  
5. Al prompt dei comandi digitare **Ntbackup**, quindi premere INVIO.  
6. Nella pagina di **benvenuto** fare clic su **modalità avanzata**e quindi selezionare la scheda **Ripristina e gestisci supporti** . non selezionare **Ripristino guidato**.  
7. Selezionare il file di backup appropriato da cui eseguire il ripristino e verificare che siano selezionate le caselle di controllo **stato del sistema** e del **disco di sistema** .  
8. Fare clic su **Avvia ripristino**.  
9. Al termine dell'operazione di ripristino, riavviare il computer.  
  
Usare la procedura seguente per eseguire un ripristino autorevole (noto anche come primario) di SYSVOL in un controller di dominio che esegue Windows Server 2003. Eseguire questa procedura solo nel primo controller di dominio di Windows Server 2003 ripristinato nel dominio.  
  
### <a name="to-perform-an-authoritative-restore-of-sysvol"></a>Per eseguire un ripristino autorevole di SYSVOL  
  
1. Eseguire i passaggi da 1 a 8 della procedura precedente.  
2. Nella finestra di dialogo **Conferma ripristino** , fare clic su **Avanzate**.  
3. Per eseguire un ripristino autorevole di SYSVOL, selezionare la casella di controllo **quando si ripristinano i set di dati replicati, contrassegnare i dati ripristinati come dati primari per tutte le repliche**.  

   > [!NOTE]
   > Contrassegnare i dati ripristinati come dati primari nel backup equivale a impostare la voce **BurFlags** su D4 sotto la seguente sottochiave del registro di sistema:  
   >   
   > **HKEY_LOCAL_MACHINE set di repliche \system\currentcontrolset\services\ntfrs\parameters\cumulative\\** *GUID*  

4. Al termine dell'operazione di ripristino, riavviare il computer.  
  
## <a name="install-and-configure-the-dns-server-service"></a>Installare e configurare il servizio server DNS

Se il controller di dominio ripristinato dal backup esegue Windows Server 2003, è possibile installare il server DNS senza connettere il controller di dominio a una rete.  
  
### <a name="to-install-and-configure-the-dns-server-service"></a>Per installare e configurare il servizio server DNS  
  
1. Apertura guidata componenti di Windows. Per aprire la procedura guidata:  

   - Fare clic su **Start**, su **Pannello di controllo** e infine su **Installazione applicazioni**.  
   - Fare clic su **Aggiungi/Rimuovi componenti di Windows**.  

2. In **componenti**selezionare la casella di controllo **servizi di rete** , quindi fare clic su **Dettagli**.  
3. In **sottocomponenti di servizi di rete**selezionare la casella di controllo **Domain Name System (DNS)** , fare clic su **OK**e quindi su **Avanti**.  
4. Se richiesto, in **copia file da**Digitare il percorso completo dei file di distribuzione e quindi fare clic su **OK**.  

   Al termine dell'installazione, completare la procedura seguente per configurare il server DNS.  

5. Fare clic sul pulsante **Start**, scegliere **tutti i programmi**, **strumenti di amministrazione**e quindi fare clic su **DNS**.  
6. Creare zone DNS per gli stessi nomi di dominio DNS ospitati nei server DNS prima del malfunzionamento critico. Per ulteriori informazioni, vedere la pagina relativa all'aggiunta di una zona di ricerca diretta ([https://go.microsoft.com/fwlink/?LinkId=74574](https://go.microsoft.com/fwlink/?LinkId=74574)).  
7. Configurare i dati DNS esistenti prima del malfunzionamento critico. Ad esempio,  

   - Configurare le zone DNS da archiviare in servizi di dominio Active Directory. Per ulteriori informazioni, vedere Modificare il tipo di zona ([https://go.microsoft.com/fwlink/?LinkId=74579](https://go.microsoft.com/fwlink/?LinkId=74579)).  
   - Configurare la zona DNS autorevole per i record di risorse del localizzatore controller di dominio (DC Locator) per consentire l'aggiornamento dinamico protetto. Per ulteriori informazioni, vedere Consenti solo aggiornamenti dinamici protetti ([https://go.microsoft.com/fwlink/?LinkId=74580](https://go.microsoft.com/fwlink/?LinkId=74580)).  

8. Verificare che la zona DNS padre contenga i record di risorse di delega (server dei nomi (NS) e i record di risorse host (A) Glue) per la zona figlio ospitata in questo server DNS. Per ulteriori informazioni, vedere la pagina relativa alla creazione di una delega di zona ([https://go.microsoft.com/fwlink/?LinkId=74562](https://go.microsoft.com/fwlink/?LinkId=74562)).  
9. Dopo aver configurato il DNS, al prompt dei comandi digitare il comando seguente e quindi premere INVIO:  

   **NET stop netlogon**

10. Digitare il comando seguente e quindi premere INVIO:  

    **NET START NETLOGON**

    > [!NOTE]
    > NET Access registrerà i record di risorse del localizzatore del controller di dominio in DNS per il controller di dominio. Se si installa il servizio server DNS in un server nel dominio figlio, il controller di dominio non sarà in grado di registrare i record immediatamente. Questo perché è attualmente isolato come parte del processo di ripristino e il server DNS primario è il server DNS radice della foresta. Configurare il computer con lo stesso indirizzo IP che aveva prima dell'emergenza per evitare errori di ricerca del servizio DC.

## <a name="next-steps"></a>Passaggi successivi

- [Ripristino della foresta di Active Directory - Prerequisiti](AD-Forest-Recovery-Prerequisties.md)  
- [Ripristino della foresta di Active Directory-definizione di un piano di ripristino della foresta personalizzato](AD-Forest-Recovery-Devising-a-Plan.md)  
- [Ripristino della foresta di Active Directory-identificare il problema](AD-Forest-Recovery-Identify-the-Problem.md)
- [Ripristino della foresta di Active Directory-determinare la modalità di ripristino](AD-Forest-Recovery-Determine-how-to-Recover.md)
- [Ripristino della foresta di Active Directory-esecuzione del ripristino iniziale](AD-Forest-Recovery-Perform-initial-recovery.md)  
- [Ripristino della foresta di Active Directory - Procedure](AD-Forest-Recovery-Procedures.md)  
- [Ripristino della foresta di Active Directory-Domande frequenti](AD-Forest-Recovery-FAQ.md)  
- [Ripristino della foresta di Active Directory-ripristino di un singolo dominio in una foresta con più domini](AD-Forest-Recovery-Single-Domain-in-Multidomain-Recovery.md)  
- [Ripristino della foresta di Active Directory-ripristino della foresta con i controller di dominio Windows Server 2003](AD-Forest-Recovery-Windows-Server-2003.md) 
