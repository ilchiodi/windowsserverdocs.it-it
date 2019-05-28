---
title: Ripristino della foresta Active Directory - ripristino di Windows Server 2003
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 07/07/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.assetid: 5a291f65-794e-4fc3-996e-094c5845a383
ms.technology: identity-adds
ms.openlocfilehash: e2af1bfc295469d43e59593d69d4ba88f476e427
ms.sourcegitcommit: 21165734a0f37c4cd702c275e85c9e7c42d6b3cb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/03/2019
ms.locfileid: "65034143"
---
# <a name="ad-forest-recovery---windows-server-2003-recovery"></a>Ripristino della foresta Active Directory - ripristino di Windows Server 2003

>Si applica a: Windows Server 2003

In questo argomento include le procedure di ripristino di foreste per i controller di dominio (DC) che eseguono Windows Server 2003. Il processo generale di ripristino della foresta è alcuna differenza con i controller di dominio di Windows Server 2003, ma le procedure specifiche possono essere diverso a causa di diversi strumenti. Ad esempio, Ntdsutil.exe utilizzabile per eseguire il backup e ripristinare i controller di dominio che eseguono Windows Server 2003 i controller di dominio, mentre Windows Server Backup o Wbadmin.exe viene usato per i controller di dominio che eseguono Windows Server 2008 o versioni successive.  
  
- [Backup dei dati dello stato del sistema](#backing-up-the-system-state-data)  
- [Esecuzione di un ripristino non autorevole](#performing-a-nonauthoritative-restore)  
- [Installare e configurare il servizio Server DNS](#install-and-configure-the-dns-server-service)

## <a name="backing-up-the-system-state-data"></a>Backup dei dati dello stato del sistema
Usare la procedura seguente per eseguire il backup dei dati dello stato del sistema, insieme a tutti gli altri dati selezionate per l'operazione di backup corrente, di un controller di dominio che esegue Windows Server 2003. Windows Server 2003 include lo strumento Ntbackup, che è possibile usare per eseguire il backup dei dati dello stato del sistema.  
  
L'appartenenza al **Administrators** oppure **Backup Operators**, o equivalente è il requisito minimo necessario per eseguire il backup di file e cartelle.   
  
Se esegue il backup dei dati dello stato del sistema su un nastro e il programma di Backup indica che non è disponibile alcun supporto inutilizzati, è necessario usare archivi rimovibili. Il nastro verrà aggiunta al pool di supporti liberi in modo che il Backup può usarla.  
  
È solo possibile eseguire il backup dei dati dello stato del sistema in un computer locale. È possibile eseguirne il backup in un computer remoto.  
  
### <a name="to-back-up-the-system-state-data-on-a-domain-controller-that-runs-windows-server-2003"></a>Per eseguire il backup dello stato del sistema in un controller di dominio che esegue Windows Server 2003  
  
1. Fare clic su **avviare**, scegliere **tutti i programmi**, scegliere **Accessori**, scegliere **utilità di sistema**e quindi fare clic su **Backup** .  
2. Nel **benvenuto** pagina, fare clic su **modalità avanzata**.  
3. Nel **Backup** scheda, selezionare la casella di controllo per qualsiasi unità, cartelle o file che si desidera eseguire il backup.  
4. Selezionare il **lo stato del sistema** casella di controllo.  
5. Fare clic su **avviare Backup**.  
  
## <a name="performing-a-nonauthoritative-restore"></a>Esecuzione di un ripristino non autorevole  

Utilizzare la procedura seguente per eseguire un ripristino non autorevole di un controller di dominio che esegue Windows Server 2003. Eseguendo un ripristino non autorevole in Active Directory in Windows Server 2003, è automaticamente eseguire un ripristino non autorevole di SYSVOL. Non sono altri passaggi sono necessari.  
  
> [!NOTE]
> Se inoltre si sta reinstallando il sistema operativo Windows Server 2003, potrebbe essere o non potrebbe aggiungere il computer al dominio ed è possibile assegnare qualsiasi nome al computer durante l'installazione del sistema operativo. È necessario installare Active Directory. Dopo la reinstallazione del sistema operativo, andare direttamente al passaggio 4.  
  
Nei controller di dominio di Windows Server 2003 in cui è stato ripristinato solo i dati di stato del sistema, è necessario reinstallare anche tutte le applicazioni software che erano in esecuzione nel controller di dominio prima di ripristino. Il ripristino di Active Directory Domain Services nel primo controller di dominio nel dominio consente inoltre di ripristinare il Registro di sistema perché entrambe fanno parte dei dati dello stato del sistema. Tenere a mente se si dispone di tutte le applicazioni in esecuzione in questi controller di dominio e se avessero tutte le informazioni archiviate nel Registro di sistema.  
  
Per ridurre i tempi necessari per installare nuovo software, determinare se le applicazioni che devono essere installati nei controller di dominio sono compatibili con la clonazione di controller di dominio virtuale. Tali applicazioni possono essere installate sul controller di dominio di origine prima della clonazione per risparmiare tempo e sforzi necessari per installarli nei controller di dominio virtuale clonato.  
  
### <a name="to-perform-a-nonauthoritative-restore"></a>Eseguire un ripristino non autorevole
  
1. Dopo avere avviato il controller di dominio, premere F8 per riavviare il computer in servizi di ripristino in modalità (Directory).  
2. Selezionare **modalità ripristino servizi Directory (solo controller di dominio Windows)** .  
3. Selezionare il sistema operativo che si desidera avviare in modalità di ripristino.  
4. Accedere come amministratore (è possibile solo usare un account computer locale, nessuna opzione di accesso di dominio è disponibile).  
5. Un prompt dei comandi, digitare **ntbackup**, quindi premere INVIO.  
6. Nel **benvenuto** pagina, fare clic su **modalità avanzata**e quindi selezionare il **Restore e gestione dei supporti** scheda. (Non si seleziona **procedura guidata Ripristino configurazione**.)  
7. Selezionare il file di backup appropriato per eseguire il ripristino da e assicurarsi che il **disco del sistema** e **lo stato del sistema** vengono selezionate le caselle di controllo.  
8. Fare clic su **Avvia ripristino**.  
9. Una volta completata l'operazione di ripristino, riavviare il computer.  
  
Utilizzare la procedura seguente per eseguire un ripristino autorevole (noto anche come primario) della cartella SYSVOL su un controller di dominio che esegue Windows Server 2003. Eseguire questa procedura solo sul primo Windows Server 2003 controller di dominio che viene ripristinato nel dominio.  
  
### <a name="to-perform-an-authoritative-restore-of-sysvol"></a>Per eseguire un ripristino autorevole di SYSVOL  
  
1. Eseguire i passaggi da 1 a 8 della procedura precedente.  
2. Nel **Conferma ripristino** finestra di dialogo, fare clic su **avanzate**.  
3. Per eseguire un ripristino autorevole di SYSVOL, selezionare la casella di controllo **durante il ripristino di set di dati replicati, contrassegnare i dati ripristinati come i dati per tutte le repliche primari**.  

   > [!NOTE]
   > Contrassegnare i dati ripristinati, come i dati primari nel Backup sono equivalenti all'impostazione di **BurFlags** voce a D4 della sottochiave del Registro di sistema seguente:  
   >   
   > **Set di repliche HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\NtFrs\Parameters\Cumulative\\**  *GUID*  

4. Una volta completata l'operazione di ripristino, riavviare il computer.  
  
## <a name="install-and-configure-the-dns-server-service"></a>Installare e configurare il servizio Server DNS

Se il controller di dominio che è stato ripristinato da un backup è in esecuzione Windows Server 2003, è possibile installare il server DNS senza doversi connettere il controller di dominio ad alcuna rete.  
  
### <a name="to-install-and-configure-the-dns-server-service"></a>Per installare e configurare il servizio Server DNS  
  
1. Aprire Aggiunta guidata componenti di Windows. Per aprire la procedura guidata:  

   - Fare clic su **Start**, su **Pannello di controllo** e infine su **Installazione applicazioni**.  
   - Fare clic su **Aggiungi/Rimuovi componenti di Windows**.  

2. Nelle **componenti**, selezionare la **servizi di rete** casella di controllo e quindi fare clic su **dettagli**.  
3. In **sottocomponenti di servizi di rete**, selezionare il **sistema DNS (Domain Name)** casella di controllo, fare clic su **OK**, quindi fare clic su **Avanti**.  
4. Se viene richiesto, in **copiarli**, digitare il percorso completo del file di distribuzione e quindi fare clic su **OK**.  

   Dopo l'installazione, completare i passaggi seguenti per configurare il server DNS.  

5. Fare clic su **avviare**, scegliere **tutti i programmi**, scegliere **strumenti di amministrazione**, quindi fare clic su **DNS**.  
6. Creare zone DNS per gli stessi nomi di dominio DNS ospitate sui server DNS prima il malfunzionamento critico. Per altre informazioni, vedere Aggiunta di una zona di ricerca diretta ([https://go.microsoft.com/fwlink/?LinkId=74574](https://go.microsoft.com/fwlink/?LinkId=74574)).  
7. Configurare i dati DNS esistente prima di malfunzionamento critico. Ad esempio:   

   - Configurare le zone DNS di essere archiviati in Active Directory Domain Services. Per altre informazioni, vedere Modifica il tipo di zona ([https://go.microsoft.com/fwlink/?LinkId=74579](https://go.microsoft.com/fwlink/?LinkId=74579)).  
   - Configurare la zona DNS autorevole per record di risorse (DC Locator) del localizzatore di controller di dominio consentire l'aggiornamento dinamico sicuro. Per altre informazioni, vedere consentire solo aggiornamenti dinamici protetti ([https://go.microsoft.com/fwlink/?LinkId=74580](https://go.microsoft.com/fwlink/?LinkId=74580)).  

8. Verificare che la zona DNS padre contenga risorse record di delega (assegna un nome server (NS) e glue host (A) resource record) per la zona figlio che è ospitato in questo server DNS. Per altre informazioni, vedere come creare una delega di Zone ([https://go.microsoft.com/fwlink/?LinkId=74562](https://go.microsoft.com/fwlink/?LinkId=74562)).  
9. Dopo la configurazione DNS, al prompt dei comandi, digitare il comando seguente e quindi premere INVIO:  

   **net stop netlogon**

10. Digitare il comando seguente e quindi premere INVIO:  

   **comando Net start netlogon**

   > [!NOTE]
   > Accesso alla rete verrà registrati i record di risorse localizzatore di controller di dominio in DNS per questo controller di dominio. Se si installa il servizio Server DNS in un server del dominio figlio, questo controller di dominio non sarà in grado di registrare i propri record immediatamente. Infatti è attualmente isolato come parte del processo di ripristino e il server DNS primario server DNS radice della foresta. Configurare il computer con lo stesso indirizzo IP che aveva prima dell'emergenza per evitare errori di ricerca del servizio di controller di dominio.

## <a name="next-steps"></a>Passaggi successivi

- [Ripristino della foresta di Active Directory - Prerequisiti](AD-Forest-Recovery-Prerequisties.md)  
- [Ripristino della foresta Active Directory - concepire un piano di ripristino personalizzata-foresta](AD-Forest-Recovery-Devising-a-Plan.md)  
- [Ripristino della foresta Active Directory - identificare il problema](AD-Forest-Recovery-Identify-the-Problem.md)
- [Ripristino della foresta Active Directory - determinare la modalità di ripristino](AD-Forest-Recovery-Determine-how-to-Recover.md)
- [Ripristino della foresta Active Directory - eseguire il ripristino iniziale](AD-Forest-Recovery-Perform-initial-recovery.md)  
- [Ripristino della foresta di Active Directory - Procedure](AD-Forest-Recovery-Procedures.md)  
- [Ripristino della foresta Active Directory - domande frequenti](AD-Forest-Recovery-FAQ.md)  
- [Ripristino della foresta Active Directory - il ripristino di un singolo dominio all'interno di un Multidomain foresta](AD-Forest-Recovery-Single-Domain-in-Multidomain-Recovery.md)  
- [Ripristino della foresta Active Directory - ripristino della foresta con controller di dominio di Windows Server 2003](AD-Forest-Recovery-Windows-Server-2003.md) 
