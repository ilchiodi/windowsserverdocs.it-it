---
title: Assegnare preventivamente gli oggetti computer del cluster in servizi di dominio Active Directory
description: Viene descritto come configurare pre-installazione gli oggetti computer del cluster in servizi di dominio Active Directory.
ms.prod: windows-server-threshold
ms.topic: article
author: JasonGerend
ms.author: jgerend
ms.technology: storage-failover-clustering
ms.date: 04/25/2018
ms.localizationpriority: medium
ms.openlocfilehash: 111969b074b33764dbbf72bfb24ad606f8314e41
ms.sourcegitcommit: e0479b0114eac7f232e8b1e45eeede96ccd72b26
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 06/22/2018
ms.locfileid: "2082262"
---
# <a name="prestage-cluster-computer-objects-in-active-directory-domain-services"></a>Assegnare preventivamente gli oggetti computer del cluster in servizi di dominio Active Directory

>Si applica a: Windows Server 2012 R2, Windows Server 2012, Windows Server 2016

In questo argomento viene illustrato come assegnare preventivamente gli oggetti computer del cluster in servizi di dominio Active Directory (AD DS). È possibile utilizzare questa procedura per abilitare un utente o gruppo creare un cluster di failover quando non sono autorizzati a creare gli oggetti computer nel dominio Active Directory.

Quando si crea un cluster di failover utilizzando la creazione guidata Cluster o tramite Windows PowerShell, è necessario specificare un nome per il cluster. Se si dispongono di autorizzazioni sufficienti quando si crea il cluster, il processo di creazione di cluster crea automaticamente un oggetto computer nel dominio di Active Directory che corrisponde al nome del cluster. Questo oggetto viene chiamato *l'oggetto nome cluster* o un oggetto nome cluster. Tramite il CNO, gli oggetti computer virtuali (VCOs) vengono creati automaticamente quando si configurano i ruoli cluster che utilizzano i punti di accesso client. Ad esempio, se si crea un file ad alta disponibilità server con un punto di accesso client denominato *FileServer1*, il CNO creerà un VCO corrispondente in AD DS.

>[!NOTE]
>In Windows Server 2012 R2 è la possibilità di creare un cluster scollegato Active Directory, in cui non vengono creati CNO o VCOs in AD DS. Ciò è prevista per specifici tipi di distribuzioni di cluster. Per ulteriori informazioni, vedere [distribuzione di un Cluster Active Directory-Detached](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/dn265970(v%3dws.11)>).

Per creare automaticamente il CNO, l'utente che crea il cluster di failover deve disporre dell'autorizzazione di **oggetti Computer creare** per l'unità organizzativa (OU) o il contenitore in cui si trovano i server che verranno formare il cluster. Per abilitare un utente o gruppo creare un cluster senza la necessità di questa autorizzazione, è possibile assegnare un utente con autorizzazioni appropriate in servizi di dominio Active Directory (in genere un amministratore di dominio) preventivamente il CNO in AD DS. Questo inoltre fornisce all'amministratore di dominio più preciso la convenzione di denominazione che viene utilizzata per il cluster e controllo su quali unità Organizzativa vengono creati gli oggetti del cluster in.

## <a name="step-1-prestage-the-cno-in-ad-ds"></a>Passaggio 1: Configurare pre-installazione il CNO in AD DS

Prima di iniziare, assicurarsi di conoscere le operazioni seguenti:

- Il nome che si desidera assegnare al cluster
- Il nome dell'account utente o del gruppo a cui si desidera concedere i diritti per creare il cluster

Per una protezione ottimale, è consigliabile creare un'unità Organizzativa per gli oggetti del cluster. Se un'unità Organizzativa esiste già che si desidera utilizzare, l'appartenenza al gruppo **Account Operators** è il minimo indispensabile per completare questo passaggio. Se è necessario creare un'unità Organizzativa per gli oggetti cluster, l'appartenenza al gruppo **Domain Admins** o equivalente, è il minimo indispensabile per completare questo passaggio.

>[!NOTE]
>Se si crea il CNO nel contenitore di computer predefinito anziché un'unità Organizzativa, non è necessario completare il passaggio 3 di questo argomento. In questo scenario, un amministratore di cluster possibile creare fino a 10 VCOs senza necessità di ulteriore configurazione.

### <a name="prestage-the-cno-in-ad-ds"></a>Assegnare preventivamente il CNO in AD DS

1. In un computer con servizi di dominio Active Directory stati installati gli strumenti da strumenti di amministrazione remota del Server o in un controller di dominio, aprire **utenti e computer**. Per ottenere questo risultato in un server, avviare Server Manager e quindi selezionare **e computer di Active Directory Users**dal menu **Strumenti** .
2. Per creare un'unità Organizzativa per il cluster di oggetti computer, destro del mouse sul nome di dominio o un'unità Organizzativa esistente, scegliere **Nuovo**e quindi selezionare **Un'unità organizzativa**. Nella casella **nome** immettere il nome dell'unità Organizzativa e quindi scegliere **OK**.
3. Nell'albero della console, fare clic sull'unità Organizzativa in cui si desidera creare il CNO, scegliere **Nuovo**e quindi scegliere **Computer**.
4. Nella casella **nome Computer** , immettere il nome che verrà utilizzato per il cluster di failover e quindi scegliere **OK**.

  >[!NOTE]
  >Questo è il nome del cluster specificati nella pagina **Punto di accesso per l'amministrazione Cluster** nella procedura guidata Creazione di Cluster o come valore dell'utente che crea il cluster di *– Name* parametro per il **Nuovo Cluster** di Windows PowerShell cmdlet.

5. È consigliabile, destro l'account del computer appena creato, scegliere **proprietà**e quindi selezionare la scheda **oggetto** . Nella scheda **oggetti** selezionare la casella di controllo di **oggetto di protezione dall'eliminazione accidentale** e quindi scegliere **OK**.
6. Pulsante destro del mouse l'account del computer appena creata e quindi selezionare **Disabilita Account**. Selezionare **Sì** per confermare e quindi scegliere **OK**.

  >[!NOTE]
  >È necessario disabilitare l'account in modo che durante la creazione di cluster, il processo di creazione del cluster può verificare che l'account non attualmente in uso da un computer esistente o cluster nel dominio.

![Oggetto nome cluster disabilitati nell'unità Organizzativa i gruppi di esempio](media/prestage-cluster-adds/disabled-cno-in-the-example-clusters-ou.png)

**Figura 1. Oggetto nome cluster disabilitati nell'unità Organizzativa i gruppi di esempio**

## <a name="step-2-grant-the-user-permissions-to-create-the-cluster"></a>Passaggio 2: Concedere le autorizzazioni utente per creare il cluster

È necessario configurare le autorizzazioni in modo che l'account utente che verrà utilizzato per creare il cluster di failover disponga delle autorizzazioni controllo completo per il CNO.

L'appartenenza al gruppo **Account Operators** è il minimo indispensabile per completare questo passaggio.

Eseguire la procedura concedere le autorizzazioni utente per creare il cluster:

1. In Active Directory utenti e computer, nel menu **Visualizza** , assicurarsi che sia selezionato **Funzionalità avanzate** .
2. Individuare e quindi fare clic il CNO e quindi scegliere **proprietà**.
3. Nella scheda **protezione** , selezionare **Aggiungi**.
4. Nella finestra di dialogo **Seleziona utenti, computer o gruppi** , specificare l'account utente o gruppo che si desidera concedere le autorizzazioni per e quindi scegliere **OK**.
5. Selezionare l'account utente o il gruppo appena aggiunto e quindi selezionare la casella di controllo **Consenti** accanto a **controllo completo**.
  
  ![Concedere il controllo completo per l'utente o gruppo a cui verrà creato il cluster](media/prestage-cluster-adds/granting-full-control-to-the-user-create-the-cluster.png)
  
  **Figura 2. Concedere il controllo completo per l'utente o gruppo a cui verrà creato il cluster**
6. Seleziona **OK**.

Dopo aver completato questo passaggio, l'utente che sono concesse le autorizzazioni per può creare il cluster di failover. Tuttavia, se l'oggetto nome cluster si trova in un'unità Organizzativa, l'utente non può creare ruoli cluster che richiedono un punto di accesso client fino a completare il passaggio 3.

>[!NOTE]
>Se l'oggetto nome cluster nel contenitore di computer predefinito, un amministratore di cluster possibile creare fino a 10 VCOs senza necessità di ulteriore configurazione. Per aggiungere più di 10 VCOs, è necessario concedere esplicitamente l'autorizzazione di **oggetti Computer creare** per l'oggetto nome cluster per il contenitore computer.

## <a name="step-3-grant-the-cno-permissions-to-the-ou-or-prestage-vcos-for-clustered-roles"></a>Passaggio 3: Concedere le autorizzazioni di oggetto nome cluster per l'unità Organizzativa o assegnare preventivamente VCOs per i ruoli del cluster

Quando si crea un ruolo non in pila con un punto di accesso client, il cluster consente di creare un VCO nella stessa unità Organizzativa come il CNO. Per questo oggetto deve essere eseguita automaticamente, l'oggetto nome cluster deve disporre delle autorizzazioni per creare gli oggetti computer nell'unità Organizzativa.

Se è assegnato preventivamente il CNO di dominio Active Directory, è possibile eseguire una delle operazioni seguenti per creare VCOs:

- Opzione 1: [concedere le autorizzazioni di oggetto nome cluster per l'unità Organizzativa](#grant-the-cno-permissions-to-the-ou). Se si utilizza questa opzione, il cluster di creare automaticamente VCOs in AD DS. Pertanto, un amministratore per il cluster di failover può creare ruoli cluster senza dover richiedere che si assegna preventivamente VCOs in AD DS.

>[!NOTE]
>L'appartenenza al gruppo **Domain Admins** o equivalente, è il minimo indispensabile per completare i passaggi necessari per questa opzione.

- Opzione 2: [Prestage VCO per un ruolo non in pila](#prestage-a-vco-for-the-clustered-role). Utilizzare questa opzione se è necessario assegnare preventivamente gli account per i ruoli del cluster a causa di requisiti dell'organizzazione. Ad esempio, si desidera controllare la convenzione di denominazione o il controllo vengono creati i ruoli che non in pila.

>[!NOTE]
>L'appartenenza al gruppo **Account Operators** è il minimo indispensabile per completare i passaggi necessari per questa opzione.

### <a name="grant-the-cno-permissions-to-the-ou"></a>Concedere le autorizzazioni di oggetto nome cluster per l'unità Organizzativa

1. In Active Directory utenti e computer, nel menu **Visualizza** , assicurarsi che sia selezionato **Funzionalità avanzate** .
2. Pulsante destro del mouse l'unità Organizzativa cui è stato creato il CNO in [passaggio 1: preventivamente il CNO in AD DS](#step-1:-prestage-the-CNO-in-ad-ds)e quindi scegliere **proprietà**.
3. Nella scheda **protezione** selezionare **Avanzate**.
4. Nella finestra di dialogo **Advanced Security Settings** selezionare **Aggiungi**.
5. Accanto a **principale**, selezionare **Selezionare un'entità**.
6. Nella finestra di dialogo **Seleziona utenti, Computer, Account di servizio o gruppi** , selezionare **I tipi di oggetto**, selezionare la casella di controllo del **computer** e quindi scegliere **OK**.
7. In **Immettere i nomi degli oggetti da selezionare**, immettere il nome del CNO, selezionare **Controlla nomi**e quindi scegliere **OK**. In risposta al messaggio di avviso che informa che si sta per aggiungere un oggetto disabilitato, selezionare **OK**.
8. Nella finestra di dialogo **Permission Entry** , verificare che nell'elenco **tipo** è impostata su **Consenti**e nell'elenco **si applica a** è impostato per **l'oggetto e tutti gli oggetti discendenti**.
9. In **autorizzazioni**selezionare la casella di controllo **Crea Computer** .

  ![Concedere l'autorizzazione di oggetti Computer creare per il CNO](media/prestage-cluster-adds/granting-create-computer-objects-permission-to-the-cno.png)

  **Figura 3. Concedere l'autorizzazione di oggetti Computer creare per il CNO**
10. Fino a tornare a utenti di Active Directory e lo snap-in computer, scegliere **OK** .

Un amministratore del cluster di failover può ora creare ruoli cluster con client punti di accesso e connettere le risorse.

### <a name="prestage-a-vco-for-a-clustered-role"></a>Assegnare preventivamente VCO per un ruolo in cluster

1. Prima di iniziare, assicurarsi di conoscere il nome del cluster e il nome contenenti il ruolo del cluster.
2. In Active Directory utenti e computer, nel menu **Visualizza** , assicurarsi che sia selezionato **Funzionalità avanzate** .
3. In utenti e computer, destro dell'unità Organizzativa in cui si trova l'oggetto nome cluster per il cluster, scegliere **Nuovo**e quindi fare **Computer**.
4. Nella casella **nome Computer** , immettere il nome viene utilizzare per il ruolo del cluster e quindi scegliere **OK**.
5. È consigliabile, destro l'account del computer appena creato, scegliere **proprietà**e quindi selezionare la scheda **oggetto** . Nella scheda **oggetti** selezionare la casella di controllo di **oggetto di protezione dall'eliminazione accidentale** e quindi scegliere **OK**.
6. Pulsante destro del mouse l'account del computer appena creata e quindi scegliere **proprietà**.
7. Nella scheda **protezione** , selezionare **Aggiungi**.
8. Nella finestra di dialogo **Seleziona utenti, Computer, Account di servizio o gruppi** , selezionare **I tipi di oggetto**, selezionare la casella di controllo del **computer** e quindi scegliere **OK**.
9. In **Immettere i nomi degli oggetti da selezionare**, immettere il nome del CNO, selezionare **Controlla nomi**e quindi scegliere **OK**. Se si riceve un messaggio di avviso che informa che si sta per aggiungere un oggetto disabilitato, fare **clic su OK**.
10. Verificare che il CNO sia selezionato e quindi selezionare la casella di controllo **Consenti** accanto a **controllo completo**.
11. Seleziona **OK**.

Un amministratore del cluster di failover può ora creare il ruolo del cluster con un punto di accesso client che corrisponde al nome VCO pre-installazione e connettere la risorsa.

## <a name="more-information"></a>Ulteriori informazioni

- [Clustering di failover](failover-clustering.md)