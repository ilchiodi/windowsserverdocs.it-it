---
title: Configurare Features on Demand in Windows Server
description: Server Manager
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-server-manager
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: e663bbea-d025-41fa-b16c-c2bff00a88e8
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: f834ca2e4c4acd045ccaeb4f46142dcc0e86f674
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71383272"
---
# <a name="configure-features-on-demand-in-windows-server"></a>Configurare Features on Demand in Windows Server

>Si applica a: Windows Server 2016

In questo argomento viene descritto come rimuovere file di funzionalità in una configurazione Funzionalità su richiesta utilizzando il cmdlet Uninstall-WindowsFeature.

Funzionalità su richiesta è una funzionalità, introdotta in Windows 8 e Windows Server 2012, che consente di rimuovere i file di ruolo e funzionalità (detto anche funzionalità *payload*) dal sistema operativo per risparmiare spazio su disco e installare i ruoli e funzionalità da posizioni remote o supporti di installazione invece che dal computer locale. È possibile rimuovere file di funzionalità da computer fisici o virtuali in esecuzione. È anche possibile aggiungere o rimuovere file di funzionalità da file immagine Windows (WIM) o dischi rigidi virtuali (VHD) offline, per creare una copia riproducibile delle configurazioni Funzionalità su richiesta.

In una configurazione funzionalità su richiesta, quando i file di funzionalità non sono disponibili in un computer, se un'installazione richiede i file di funzionalità, Windows Server 2012 R2 o Windows Server 2012 possa essere indirizzata a ottenere i file da un archivio di funzionalità side-by-side (una cartella condivisa che contiene i file di funzionalità ed è disponibile per il computer sulla rete,) da Windows Update , o dal supporto di installazione. Per impostazione predefinita, quando non sono disponibili file di funzionalità sul server di destinazione, Funzionalità su richiesta ricerca i file di funzionalità mancanti eseguendo le attività riportate di seguito nell'ordine indicato.

1.  Ricerca in un percorso specificato dagli utenti dell'aggiunta guidata ruoli e funzionalità o dei comandi di installazione DISM

2.  Valutazione della configurazione dell'impostazione di Criteri di gruppo, configurazione **computer\Modelli amministrativi\Sistema\Specifica le impostazioni per l'installazione e il ripristino dei componenti facoltativi**

3.  Ricerca in Windows Update

È possibile ignorare il comportamento predefinito di Funzionalità su richiesta effettuando una delle seguenti operazioni.

-   Specifica di un percorso di origine alternativo come parte del cmdlet `Install-WindowsFeature` , mediante l'aggiunta del parametro `Source`

-   Specifica di un percorso di origine alternativo nella pagina **Conferma opzioni di installazione** durante l'installazione di funzionalità mediante l'aggiunta guidata ruoli e funzionalità

-   Configurazione dell'impostazione Criteri di gruppo, **Specifica le impostazioni per l'installazione e il ripristino dei componenti facoltativi**

In questo argomento sono incluse le sezioni seguenti.

-   [Creazione di un file di funzionalità o di un archivio affiancato](#BKMK_store)

-   [Metodi di rimozione dei file di funzionalità](#BKMK_methods)

-   [Rimuovere i file di funzionalità tramite Uninstall-WindowsFeature](#BKMK_remove)

## <a name="BKMK_store"></a>Creazione di un file di funzionalità o di un archivio affiancato
In questa sezione viene descritto come configurare una funzionalità remoto condiviso cartella di file (denominata anche archivio side-by-side) che memorizza i file necessari per installare ruoli, servizi ruolo e funzionalità in server che eseguono Windows Server 2012 R2 o Windows Server 2012. Dopo avere configurato un archivio di funzionalità, è possibile installare ruoli, servizi ruolo e funzionalità in server che eseguono i sistemi operativi e specificare l'archivio di funzionalità come il percorso dei file di origine di installazione.

#### <a name="to-create-a-feature-file-store"></a>Per creare un archivio di file di funzionalità

1.  Creare una cartella condivisa in un server in rete. Ad esempio, *\\\network\share\sxs*.

2.  Verificare di disporre delle autorizzazioni corrette assegnate all'archivio di funzionalità. Il percorso di origine o la condivisione file deve concedere autorizzazioni di **lettura** al gruppo **Everyone** (sconsigliato per motivi di sicurezza) o agli account computer (*dominio*\\*SERverNAME*$) dei server in cui si intende installare funzionalità tramite questo archivio di funzionalità; la concessione dell'accesso all'account utente non è sufficiente.

    Per accedere alle impostazioni delle autorizzazioni e di condivisione file è possibile eseguire una delle due operazioni seguenti sul desktop Windows.

    -   Fare clic con il pulsante destro del mouse sulla cartella condivisa, scegliere **Proprietà**e quindi modificare gli utenti autorizzati e i relativi diritti di accesso alla cartella nella scheda **Sicurezza** .

    -   Fare clic con il pulsante destro del mouse sulla cartella condivisa, scegliere **Condividi con**e quindi fare clic su **Utenti specifici**.

    > [!NOTE]
    > I server appartenenti a gruppi di lavoro non possono accedere a condivisioni file esterne, anche se l'account computer del server del gruppo di lavoro dispone delle autorizzazioni **Lettura** per la condivisione esterna. Tra le posizioni di origine alternative valide per i server di gruppi di lavoro sono inclusi supporti di installazione, Windows Update e file VHD o WIM archiviati nel server del gruppo di lavoro locale.

3.  Copiare la cartella **Sources\SxS** dai supporti di installazione di Windows Server alla cartella condivisa creata al passaggio 1.

## <a name="BKMK_methods"></a>Metodi di rimozione dei file di funzionalità
Sono disponibili due metodi per rimuovere file di funzionalità da Windows Server in una configurazione Funzionalità su richiesta.

-   Il `remove` del parametro di `Uninstall-WindowsFeature` cmdlet consente di eliminare i file di funzionalità da un server o non in linea disco rigido virtuale (VHD) che esegue Windows Server 2012 R2 o Windows Server 2012. I valori validi per il `remove` parametro sono i nomi di ruoli, servizi ruolo e funzionalità.

-   I comandi di Gestione e manutenzione immagini distribuzione (DISM) consentono di creare file WIM personalizzati, per conservare spazio su disco omettendo i file di configurazione non necessari o ottenibili da altre origini remote. Per altre informazioni sull'uso di Gestione e manutenzione immagini distribuzione per preparare immagini personalizzate, vedere [Come abilitare o disabilitare le funzionalità di Windows](https://technet.microsoft.com/library/hh824822.aspx).

## <a name="BKMK_remove"></a>Rimuovere i file di funzionalità tramite Uninstall-WindowsFeature
È possibile utilizzare il cmdlet Uninstall-WindowsFeature per disinstallare ruoli, servizi ruolo e funzionalità dal server e dischi rigidi virtuali offline che eseguono Windows Server 2012 R2 o Windows Server 2012 e per eliminare i file di funzionalità. È possibile disinstallare ed eliminare i ruoli, servizi ruolo e funzionalità nello stesso comando stesso, se desiderato.

> [!IMPORTANT]
> Quando si eliminano il file di funzionalità per un ruolo, vengono eliminate anche servizio ruolo o funzionalità, ruoli, servizi ruolo e funzionalità che dipendono da file di cui che si desidera rimuovere. Se si stanno eliminando file di funzionalità per un servizio ruolo o una funzionalità secondaria e non restano installati altri servizio ruolo o funzionalità secondarie per il ruolo padre, verranno eliminati i file per l'intero ruolo o funzionalità padre. Per visualizzare tutti i file di funzionalità che verranno eliminati dal comando `Uninstall-WindowsFeature -remove`, aggiungere il parametro `whatif` al comando, per eseguirlo e visualizzare i risultati senza eliminare effettivamente i file di funzionalità.

#### <a name="to-remove-role-and-feature-files-by-using-uninstall-windowsfeature"></a>Per rimuovere file di ruolo e funzionalità mediante Uninstall-WindowsFeature

1.  Per aprire una sessione di Windows PowerShell con diritti utente elevati, eseguire una di queste operazioni.

    > [!NOTE]
    > Se si disinstallano ruoli e funzionalità da un server remoto, non è necessario eseguire Windows PowerShell con diritti utente elevati.

    -   Nel desktop di Windows fare clic con il pulsante destro del mouse su **Windows PowerShell** nella barra delle applicazioni e scegliere **Esegui come amministratore**.

    -   Nella schermata **Start** di Windows fare clic con il pulsante destro del mouse sul riquadro Windows PowerShell e quindi, nella barra dell'app, fare clic su **Esegui come amministratore**.

    -   In un server che esegue l'opzione di installazione dei componenti di base del server, digitare **PowerShell** in un prompt dei comandi, quindi premere **invio**.

2.  Digitare il comando seguente e quindi premere **INVIO**.

    ```
    Uninstall-WindowsFeature -Name <feature_name> -computerName <computer_name> -remove
    ```

    **Esempio:** Servizio licenze Desktop remoto è l'ultimo servizio ruolo ancora installato di Servizi Desktop remoto. Il comando disinstalla Servizio licenze Desktop remoto e quindi elimina i file di funzionalità per l'intero ruolo Servizi Desktop remoto dal server specificato, *contoso_1*.

    ```
    Uninstall-WindowsFeature -Name rdS-Licensing -computerName contoso_1 -remove
    ```

    **Esempio:** Nell'esempio seguente il comando rimuove servizi di dominio Active Directory e gestione Criteri di gruppo da un disco rigido virtuale offline. Vengono prima disinstallati il ruolo e la funzionalità, quindi vengono interamente rimossi i relativi file di funzionalità dal disco rigido virtuale offline *Contoso.vhd*.

    > [!NOTE]
    > È necessario aggiungere il `computerName` parametro se si esegue il cmdlet da un computer che esegue Windows 8.1 o Windows 8.
    > 
    > Se si immette il nome di un file VHD da una condivisione di rete, tale condivisione deve concedere le autorizzazioni di **lettura** e **scrittura** all'account computer del server selezionato per montare il disco rigido virtuale. L'accesso con account solo utente non è sufficiente. La condivisione può concedere le autorizzazioni **Lettura** e **Scrittura** al gruppo **Everyone** per consentire l'accesso al disco rigido virtuale, ma si tratta di un'impostazione sconsigliata per motivi di sicurezza.

    ```
    Uninstall-WindowsFeature -Name AD-Domain-Services,GPMC -VHD C:\WS2012VHDs\Contoso.vhd -computerName ContosoDC1
    ```

## <a name="see-also"></a>Vedere anche
[Installare o disinstallare ruoli, servizi ruolo o funzionalità](install-or-uninstall-roles-role-services-or-features.md)
[Opzioni di installazione di Windows Server](https://technet.microsoft.com/library/hh831786.aspx)
[come abilitare o disabilitare le funzionalità di Windows](https://technet.microsoft.com/library/hh824822.aspx)
[distribuzione Panoramica di gestione e Manutenzione e manutenzione immagini distribuzione](https://technet.microsoft.com/library/hh825236.aspx)


