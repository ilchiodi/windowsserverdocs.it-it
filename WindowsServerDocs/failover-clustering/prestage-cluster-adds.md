---
title: Pre-installare oggetti computer del cluster in servizi di dominio Active Directory
description: Come pre-installare oggetti computer del cluster in servizi di dominio Active Directory.
ms.prod: windows-server-threshold
ms.topic: article
author: JasonGerend
ms.author: jgerend
ms.manager: daveba
ms.technology: storage-failover-clustering
ms.date: 05/09/2019
ms.localizationpriority: medium
ms.openlocfilehash: 151f02572d7595776539af163831b4a7a060c1c7
ms.sourcegitcommit: 75f257d97d345da388cda972ccce0eb29e82d3bc
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/14/2019
ms.locfileid: "65613163"
---
# <a name="prestage-cluster-computer-objects-in-active-directory-domain-services"></a>Pre-installare oggetti computer del cluster in servizi di dominio Active Directory

>Si applica a: Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

In questo argomento viene illustrato come pre-installare oggetti computer del cluster in Servizi di dominio Active Directory. Eseguendo questa procedura, è possibile consentire a un utente o a un gruppo di creare un cluster di failover quando non dispone delle autorizzazioni necessarie per creare oggetti computer in Servizi di dominio Active Directory.

Quando si crea un cluster di failover mediante la Creazione guidata cluster o Windows PowerShell, è necessario specificare un nome per il cluster. Se si dispone di autorizzazioni sufficienti mentre si crea il cluster, il processo di creazione genera automaticamente in Servizi di dominio Active Directory un oggetto computer corrispondente al nome del cluster. Tale oggetto è noto come *oggetto nome cluster*. Tramite l'oggetto nome cluster vengono creati automaticamente oggetti computer virtuale quando si configurano ruoli del cluster che utilizzano punti di accesso client. Se ad esempio si crea un file server a disponibilità elevata con un punto di accesso client denominato *FileServer1*, l'oggetto nome cluster creerà un oggetto computer virtuale corrispondente in Servizi di dominio Active Directory.

>[!NOTE]
>È disponibile l'opzione per creare un cluster scollegato da Active Directory, in cui nessun oggetto nome cluster o oggetti computer virtuale vengono creati in Active Directory Domain Services. Tale opzione è per tipi specifici di distribuzioni di cluster. Per ulteriori informazioni, vedere [Distribuire un cluster scollegato da Active Directory](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/dn265970(v%3dws.11)>).

Per creare automaticamente l'oggetto nome cluster, l'utente che crea il cluster di failover deve disporre dell'autorizzazione **Crea oggetti computer** per l'unità organizzativa (OU) o il contenitore in cui risiedono i server che costituiranno il cluster. Per consentire a un utente o a un gruppo di creare un cluster senza disporre di tale autorizzazione, un utente con le autorizzazioni appropriate in Servizi di dominio Active Directory (in genere, un amministratore di dominio) può pre-installare l'oggetto nome cluster in Servizi di dominio Active Directory. Questo inoltre conferisce all'amministratore di dominio maggiore controllo sulla convenzione di denominazione utilizzata per il cluster, nonché sull'unità organizzativa in cui vengono creati gli oggetti cluster.

## <a name="step-1-prestage-the-cno-in-ad-ds"></a>Passaggio 1: Pre-installare l'oggetto CNO in Active Directory Domain Services

Prima di iniziare, verificare di disporre delle informazioni seguenti:

- Il nome che si desidera assegnare al cluster
- Il nome dell'account utente o gruppo a cui si desidera concedere i diritti per creare il cluster

Come procedura consigliata, creare un'unità organizzativa per gli oggetti cluster. Se l'unità organizzativa che si intende utilizzare esiste già, per eseguire questo passaggio è richiesta almeno l'appartenenza al gruppo **Account Operators**. Se è necessario creare un'unità organizzativa per gli oggetti cluster, per eseguire questo passaggio è richiesta almeno l'appartenenza al gruppo **Domain Admins** o a un gruppo equivalente.

>[!NOTE]
>Se si crea l'oggetto nome cluster nel contenitore Computer predefinito anziché in un'unità organizzativa, non è necessario eseguire il passaggio 3 descritto in questo argomento. In tale scenario un amministratore di cluster può creare fino a 10 oggetti computer virtuale senza ulteriori attività di configurazione.

### <a name="prestage-the-cno-in-ad-ds"></a>Pre-installare l'oggetto CNO in Active Directory Domain Services

1. In un computer in cui tramite Strumenti di amministrazione remota del server siano stati installati gli strumenti per Servizi di dominio Active Directory oppure in un controller di dominio aprire **Utenti e computer di Active Directory**. A tale scopo in un server avviare Server Manager e quindi scegliere il **Tools** dal menu **Active Directory Users and Computers**.
2. Per creare un'unità Organizzativa per i cluster degli oggetti computer, fare doppio clic il nome di dominio o un'unità Organizzativa esistente, scegliere **New**, quindi selezionare **unità organizzativa**. Nel **Name** casella, immettere il nome dell'unità Organizzativa e quindi selezionare **OK**.
3. Nell'albero della console, fare doppio clic su unità Organizzativa in cui si desidera creare l'oggetto nome cluster, scegliere **New**, quindi selezionare **Computer**.
4. Nel **nome Computer** casella, immettere il nome che verrà utilizzato per il cluster di failover e quindi seleziona **OK**.

  >[!NOTE]
  >Questo è il nome cluster che l'utente che crea il cluster specificherà nella pagina **Punto di accesso per l'amministrazione del cluster** della Creazione guidata cluster o come valore del parametro *–Name* per il cmdlet **New-Cluster** di Windows PowerShell.

5. Come procedura consigliata, fare clic sull'account computer appena creato, selezionare **delle proprietà**, quindi selezionare la **oggetto** scheda. Nel **oggetti** scheda, seleziona la **Proteggi oggetto da eliminazioni accidentali** casella di controllo e quindi selezionare **OK**.
6. L'account computer appena creato e quindi scegliere **Disabilita Account**. Selezionare **Yes** per confermare e quindi selezionare **OK**.

  >[!NOTE]
  >È necessario disabilitare l'account in modo che, durante la creazione del cluster, il processo possa verificare che l'account attualmente non viene utilizzato da un computer o un cluster esistente nel dominio.

![Oggetto nome cluster disabilitato nell'unità organizzativa Cluster di esempio](media/prestage-cluster-adds/disabled-cno-in-the-example-clusters-ou.png)

**Figura 1. Oggetto nome cluster disabilitato nell'unità Organizzativa cluster di esempio**

## <a name="step-2-grant-the-user-permissions-to-create-the-cluster"></a>Passaggio 2: Concedere le autorizzazioni per creare il cluster

È necessario configurare le autorizzazioni in modo che l'account utente che verrà utilizzato per creare il cluster di failover disponga delle autorizzazioni Controllo completo per l'oggetto nome cluster.

Per eseguire questo passaggio è richiesta almeno l'appartenenza al gruppo **Account Operators** .

Di seguito viene illustrato come concedere le autorizzazioni per creare il cluster:

1. In Utenti e computer di Active Directory verificare che nel menu **Visualizza** sia selezionata la voce **Funzionalità avanzate**.
2. Individuare e quindi l'oggetto nome cluster e quindi scegliere **proprietà**.
3. Nel **sicurezza** scheda, seleziona **Add**.
4. Nel **Seleziona utenti, computer o gruppi** finestra di dialogo, specificare l'account utente o gruppo che si desidera concedere le autorizzazioni per e quindi selezionare **OK**.
5. Selezionare l'account utente o il gruppo appena aggiunto e quindi accanto a **Controllo completo**selezionare la casella di controllo **Consenti** .
  
  ![Concessione del controllo completo all'utente o al gruppo che creerà il cluster](media/prestage-cluster-adds/granting-full-control-to-the-user-create-the-cluster.png)
  
  **Figura 2. Concessione del controllo completo all'utente o gruppo che creerà il cluster**
6. Scegliere **OK**.

Dopo l'esecuzione di questo passaggio, l'utente a cui sono state concesse le autorizzazioni potrà creare il cluster di failover. Se però l'oggetto nome cluster si trova in un'unità organizzativa, l'utente potrà creare ruoli del cluster che richiedono un punto di accesso client solo dopo che è stato eseguito il passaggio 3.

>[!NOTE]
>Se l'oggetto nome cluster si trova nel contenitore Computer predefinito, un amministratore di cluster può creare fino a 10 oggetti computer virtuale senza ulteriori attività di configurazione. Per aggiungere più di 10 oggetti computer virtuale, è necessario concedere esplicitamente all'oggetto nome cluster l'autorizzazione **Crea oggetti computer** per il contenitore Computer.

## <a name="step-3-grant-the-cno-permissions-to-the-ou-or-prestage-vcos-for-clustered-roles"></a>Passaggio 3: Concedere le autorizzazioni oggetto nome cluster all'unità Organizzativa o pre-installare oggetti computer virtuale per i ruoli del cluster

Quando si crea un ruolo del cluster con un punto di accesso client, il cluster crea un oggetto computer virtuale nella stessa unità organizzativa dell'oggetto nome cluster. Affinché questa operazione venga eseguita automaticamente, è necessario che l'oggetto nome cluster disponga delle autorizzazioni per la creazione di oggetti computer nell'unità organizzativa.

Se l'oggetto nome cluster è stato pre-installato in Servizi di dominio Active Directory, sarà possibile procedere in uno dei modi seguenti per creare gli oggetti computer virtuale:

- Opzione 1: [Concedere le autorizzazioni oggetto nome cluster all'unità Organizzativa](#grant-the-cno-permissions-to-the-ou). Se si sceglie questa opzione, il cluster potrà creare automaticamente gli oggetti computer virtuale in Servizi di dominio Active Directory. Un amministratore del cluster di failover potrà pertanto creare ruoli del cluster senza dover richiedere la pre-installazione di oggetti computer virtuale in Servizi di dominio Active Directory.

>[!NOTE]
>Per eseguire la procedura relativa a questa opzione è richiesta almeno l'appartenenza al gruppo **Domain Admins** o a un gruppo equivalente.

- Opzione 2: [Pre-installare un oggetto computer virtuale per un ruolo del cluster](#prestage-a-vco-for-a-clustered-role). Scegliere questa opzione se è necessario pre-installare gli account per i ruoli del server a causa dei requisiti da soddisfare nell'organizzazione. È ad esempio possibile avere la necessità di controllare la convenzione di denominazione oppure quali ruoli del cluster vengono creati.

>[!NOTE]
>Per eseguire la procedura relativa a questa opzione è richiesta almeno l'appartenenza al gruppo **Account Operators**.

### <a name="grant-the-cno-permissions-to-the-ou"></a>Concedere le autorizzazioni oggetto nome cluster all'unità Organizzativa

1. In Utenti e computer di Active Directory verificare che nel menu **Visualizza** sia selezionata la voce **Funzionalità avanzate**.
2. Fare doppio clic su unità Organizzativa in cui è stato creato l'oggetto CNO in [passaggio 1: Pre-installare l'oggetto CNO in AD DS](#step-1-prestage-the-cno-in-ad-ds), quindi selezionare **proprietà**.
3. Nel **sicurezza** scheda, seleziona **avanzate**.
4. Nel **impostazioni di sicurezza avanzate** finestra di dialogo **Add**.
5. Accanto a **Principal**, selezionare **seleziona un'entità**.
6. Nel **Seleziona utenti, Computer, Account del servizio o gruppi** finestra di dialogo, selezionare **tipi di oggetti**, selezionare il **computer** casella di controllo e quindi selezionare **OK** .
7. Sotto **immettere i nomi degli oggetti da selezionare**, immettere il nome dell'oggetto nome cluster, selezionare **Controlla nomi**, quindi selezionare **OK**. In risposta al messaggio di avviso che segnala che sta tentando di aggiungere un oggetto disabilitato, selezionare **OK**.
8. Nella finestra di dialogo **Voce autorizzazione** verificare che l'elenco **Tipo** sia impostato su **Consenti** e che l'elenco **Si applica a** sia impostato su **Questo oggetto e tutti i discendenti**.
9. In **Autorizzazioni** selezionare la casella di controllo **Crea oggetti computer**.

  ![Concessione dell'autorizzazione Crea oggetti computer all'oggetto nome cluster](media/prestage-cluster-adds/granting-create-computer-objects-permission-to-the-cno.png)

  **Figura 3. Concedere l'autorizzazione Crea oggetti Computer per l'oggetto nome cluster**
10. Selezionare **OK** fino a quando non si torna alla Active Directory Users e computer lo snap-in.

Un amministratore nel cluster di failover potrà ora creare ruoli del cluster con punti di accesso client e connettere le risorse.

### <a name="prestage-a-vco-for-a-clustered-role"></a>Pre-installare un oggetto computer virtuale per un ruolo del cluster

1. Prima di iniziare, verificare di conoscere il nome del cluster e il nome che avrà il ruolo del cluster.
2. In Utenti e computer di Active Directory verificare che nel menu **Visualizza** sia selezionata la voce **Funzionalità avanzate**.
3. In Active Directory Users and Computers, fare doppio clic nell'unità Organizzativa in cui l'oggetto nome cluster per il cluster si trova, scegliere **New**, quindi selezionare **Computer**.
4. Nel **nome Computer** casella, immettere il nome che verrà utilizzato per il ruolo del cluster e quindi selezionare **OK**.
5. Come procedura consigliata, fare clic sull'account computer appena creato, selezionare **delle proprietà**, quindi selezionare la **oggetto** scheda. Nel **oggetti** scheda, seleziona la **Proteggi oggetto da eliminazioni accidentali** casella di controllo e quindi selezionare **OK**.
6. L'account computer appena creato e quindi scegliere **proprietà**.
7. Nel **sicurezza** scheda, seleziona **Add**.
8. Nel **Seleziona utenti, Computer, Account del servizio o gruppi** finestra di dialogo, selezionare **tipi di oggetti**, selezionare il **computer** casella di controllo e quindi selezionare **OK** .
9. Sotto **immettere i nomi degli oggetti da selezionare**, immettere il nome dell'oggetto nome cluster, selezionare **Controlla nomi**, quindi selezionare **OK**. Se si riceve un messaggio di avviso che segnala che sta tentando di aggiungere un oggetto disabilitato, selezionare **OK**.
10. Verificare che l'oggetto nome cluster sia selezionato e quindi accanto a **Controllo completo** selezionare la casella di controllo **Consenti**.
11. Scegliere **OK**.

Un amministratore nel cluster di failover potrà ora creare il ruolo del cluster con un punto di accesso client corrispondente al nome dell'oggetto computer virtuale pre-installato e connettere la risorsa.

## <a name="more-information"></a>Altre informazioni

- [Clustering di failover](failover-clustering.md)
- [Configurazione di account di cluster in Active Directory](configure-ad-accounts.md)