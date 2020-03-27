---
title: Aggiunta a dominio offline di DirectAccess
description: Questa guida illustra i passaggi per eseguire un'aggiunta a un dominio offline con DirectAccess in Windows Server 2016.
manager: brianlic
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-da
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 55528736-6c19-40bd-99e8-5668169ef3c7
ms.author: lizross
author: eross-msft
ms.openlocfilehash: 09ed401fa4912a48033e4a51a29309e3fd4cc998
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/26/2020
ms.locfileid: "80310899"
---
# <a name="directaccess-offline-domain-join"></a>Aggiunta a dominio offline di DirectAccess

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016

Questa guida illustra i passaggi per eseguire un'aggiunta a un dominio offline con DirectAccess. Durante un'aggiunta a un dominio offline, un computer è configurato per l'aggiunta a un dominio senza connessione fisica o VPN.  
  
In questa guida sono inclusi gli argomenti seguenti:  
  
- Panoramica dell'aggiunta a un dominio offline  
  
- Requisiti per l'aggiunta a un dominio offline
  
- Processo di aggiunta al dominio offline
  
- Passaggi per l'esecuzione di un aggiunta a un dominio offline  
  
## <a name="offline-domain-join-overview"></a>Panoramica dell'aggiunta a un dominio offline  
Introdotti in Windows Server 2008 R2, i controller di dominio includono una funzionalità denominata aggiunta a un dominio offline. Un'utilità della riga di comando denominata Djoin. exe consente di aggiungere un computer a un dominio senza contattare fisicamente un controller di dominio durante il completamento dell'operazione di aggiunta a un dominio. I passaggi generali per l'utilizzo di Djoin. exe sono:  
  
1.  Eseguire **djoin/provision** per creare i metadati dell'account computer. L'output di questo comando è un file con estensione txt che include un BLOB con codifica base 64.  
  
2.  Eseguire **Djoin/requestodj** per inserire i metadati dell'account computer dal file con estensione txt nella directory Windows del computer di destinazione.  
  
3.  Riavviare il computer di destinazione e il computer verrà aggiunto al dominio.  
  
### <a name="offline-domain-join-with-directaccess-policies-scenario-overview"></a><a name="BKMK_ODJOverview"></a>Panoramica dello scenario di aggiunta a un dominio offline con criteri DirectAccess  
L'aggiunta a un dominio offline di DirectAccess è un processo che i computer che eseguono Windows Server 2016, Windows Server 2012, Windows 10 e Windows 8 possono utilizzare per aggiungere un dominio senza essere fisicamente aggiunti alla rete aziendale o connessi tramite VPN. In questo modo è possibile aggiungere i computer a un dominio da percorsi in cui non è presente connettività a una rete aziendale. Aggiunta a un dominio offline per DirectAccess fornisce criteri DirectAccess ai client per consentire il provisioning remoto.  
  
Un join a un dominio crea un account computer e stabilisce una relazione di trust tra un computer che esegue un sistema operativo Windows e un dominio Active Directory.  
  
## <a name="prepare-for-offline-domain-join"></a><a name="BKMK_ODJRequirements"></a>Preparare l'aggiunta a un dominio offline  
  
1.  Creare l'account del computer.  
  
2.  Eseguire l'inventario dell'appartenenza di tutti i gruppi di sicurezza a cui appartiene l'account del computer.  
  
3.  Raccogliere i certificati computer, i criteri di gruppo e gli oggetti Criteri di gruppo necessari da applicare ai nuovi client.  
  
. Le sezioni seguenti illustrano i requisiti del sistema operativo e i requisiti relativi alle credenziali per l'esecuzione di un aggiunta a un dominio offline DirectAccess con Djoin. exe.  
  
### <a name="operating-system-requirements"></a>Requisiti del sistema operativo  
È possibile eseguire Djoin. exe per DirectAccess solo nei computer che eseguono Windows Server 2016, Windows Server 2012 o Windows 8. Il computer in cui si esegue Djoin. exe per effettuare il provisioning dei dati degli account computer in servizi di dominio Active Directory deve eseguire Windows Server 2016, Windows 10, Windows Server 2012 o Windows 8. Il computer che si desidera aggiungere al dominio deve eseguire anche Windows Server 2016, Windows 10, Windows Server 2012 o Windows 8.  
  
### <a name="credential-requirements"></a>Requisiti relativi alle credenziali  
Per eseguire un'aggiunta a un dominio offline, è necessario disporre dei diritti necessari per aggiungere le workstation al dominio. Per impostazione predefinita, i membri del gruppo Domain Admins dispongono di questi diritti. Se non si è un membro del gruppo Domain Admins, è necessario che un membro del gruppo Domain Admins completi una delle azioni seguenti per consentire l'aggiunta di workstation al dominio:  
  
-   Utilizzare Criteri di gruppo per concedere i diritti utente richiesti. Questo metodo consente di creare computer nel contenitore computer predefinito e in qualsiasi unità organizzativa creata in un secondo momento (se non viene aggiunta alcuna voce di controllo di accesso (ACE) Deny).  
  
-   Modificare l'elenco di controllo di accesso (ACL) del contenitore computer predefinito per il dominio per delegare le autorizzazioni corrette.  
  
-   Creare un'unità organizzativa e modificare l'ACL in tale OU per concedere l'autorizzazione **Create Child-Allow** . Passare il parametro **/machineOU** al comando **djoin/provision** .  
  
Nelle procedure riportate di seguito viene illustrato come concedere i diritti utente con Criteri di gruppo e come delegare le autorizzazioni corrette.  
  
#### <a name="granting-user-rights-to-join-workstations-to-the-domain"></a>Concessione dei diritti utente per l'aggiunta di workstation al dominio  
È possibile utilizzare la Console Gestione Criteri di gruppo (GPMC) per modificare i criteri di dominio o creare un nuovo criterio con impostazioni che concedano i diritti utente per aggiungere workstation a un dominio.  
  
L'appartenenza al gruppo **Domain Admins**o a un gruppo equivalente è il requisito minimo necessario per concedere diritti utente.  Per informazioni dettagliate sull'uso degli account appropriati e delle appartenenze a gruppi, vedere [gruppi predefiniti locali e di dominio](https://go.microsoft.com/fwlink/?LinkId=83477) (https://go.microsoft.com/fwlink/?LinkId=83477).   
  
###### <a name="to-grant-rights-to-join-workstations-to-a-domain"></a>Per concedere diritti per l'aggiunta di workstation a un dominio  
  
1.  Fare clic sul pulsante **Start**, scegliere **Strumenti di amministrazione** e quindi fare clic su **Gestione Criteri di gruppo**.  
  
2.  Fare doppio clic sul nome della foresta, fare doppio clic su **domini**, fare doppio clic sul nome del dominio in cui si desidera aggiungere un computer, fare clic con il pulsante destro del mouse su **criterio dominio predefinito**, quindi scegliere **modifica**.  
  
3.  Nell'albero della console fare doppio clic su **Configurazione computer**, fare doppio clic su **criteri**, fare doppio clic su **impostazioni di Windows**, fare doppio clic su impostazioni di **sicurezza**, fare doppio clic su **criteri locali**, quindi fare doppio clic su **assegnazione diritti utente**.  
  
4.  Nel riquadro dei dettagli fare doppio clic su **Aggiungi workstation al dominio**.  
  
5.  Selezionare la casella di controllo **Definisci queste impostazioni dei criteri** e quindi fare clic su **Aggiungi utente o gruppo**.  
  
6.  Digitare il nome dell'account a cui si desidera concedere i diritti utente, quindi fare clic su **OK** due volte.  
  
## <a name="offline-domain-join-process"></a><a name="BKMK_ODKSxS"></a>Processo di aggiunta al dominio offline  
Eseguire Djoin. exe a un prompt dei comandi con privilegi elevati per effettuare il provisioning dei metadati dell'account computer. Quando si esegue il comando di provisioning, i metadati dell'account computer vengono creati in un file binario specificato come parte del comando.  
  
Per ulteriori informazioni sulla funzione NetProvisionComputerAccount utilizzata per eseguire il provisioning dell'account computer durante un'aggiunta a un dominio offline, vedere [funzione NetProvisionComputerAccount](https://go.microsoft.com/fwlink/?LinkId=162426) (https://go.microsoft.com/fwlink/?LinkId=162426). Per ulteriori informazioni sulla funzione NetRequestOfflineDomainJoin eseguita localmente nel computer di destinazione, vedere [funzione NetRequestOfflineDomainJoin](https://go.microsoft.com/fwlink/?LinkId=162427) (https://go.microsoft.com/fwlink/?LinkId=162427).  
  
## <a name="steps-for-performing-a-directaccess-offline-domain-join"></a><a name="BKMK_ODJSteps"></a>Passaggi per l'esecuzione di un aggiunta a un dominio offline DirectAccess  
Il processo di aggiunta al dominio offline include i passaggi seguenti:  
  
1.  Creare un nuovo account computer per ognuno dei client remoti e generare un pacchetto di provisioning usando il comando Djoin. exe da un computer già aggiunto al dominio nella rete aziendale.  
  
2.  Aggiungere il computer client al gruppo di sicurezza DirectAccessClients  
  
3.  Trasferire il pacchetto di provisioning in modo sicuro ai computer remoti che entreranno in un join del dominio.  
  
4.  Applicare il pacchetto di provisioning e aggiungere il client al dominio.  
  
5.  Riavviare il client per completare l'aggiunta a un dominio e stabilire la connettività.  
  
Quando si crea il pacchetto di provisioning per il client, è possibile prendere in considerazione due opzioni. Se è stata usata la procedura guidata Introduzione per installare DirectAccess senza PKI, è necessario usare l'opzione 1 riportata di seguito. Se è stata utilizzata l'installazione guidata avanzata per installare DirectAccess con l'infrastruttura a chiave pubblica (PKI), è necessario utilizzare l'opzione 2 riportata di seguito.  
  
Completare la procedura seguente per eseguire l'aggiunta al dominio offline:  
  
##### <a name="option1-create-a-provisioning-package-for-the-client-without-pki"></a>Opzione1: creare un pacchetto di provisioning per il client senza PKI  
  
1.  Al prompt dei comandi del server di accesso remoto digitare il comando seguente per eseguire il provisioning dell'account computer:  
  
    ```  
    Djoin /provision /domain <your domain name> /machine <remote machine name> /policynames DA Client GPO name /rootcacerts /savefile c:\files\provision.txt /reuse  
    ```  
  
##### <a name="option2-create-a-provisioning-package-for-the-client-with-pki"></a>Opzione2: creare un pacchetto di provisioning per il client con PKI  
  
1.  Al prompt dei comandi del server di accesso remoto digitare il comando seguente per eseguire il provisioning dell'account computer:  
  
    ```  
    Djoin /provision /machine <remote machine name> /domain <Your Domain name> /policynames <DA Client GPO name> /certtemplate <Name of client computer cert template> /savefile c:\files\provision.txt /reuse  
    ```  
  
##### <a name="add-the-client-computer-to-the-directaccessclients-security-group"></a>Aggiungere il computer client al gruppo di sicurezza DirectAccessClients  
  
1.  Nel controller di dominio, dalla schermata **Start** digitare **Active** e selezionare **Active Directory utenti e computer** dalla schermata **app** .  
  
2.  Espandere l'albero sotto il dominio e selezionare il contenitore **utenti** .  
  
3.  Nel riquadro dei dettagli fare clic con il pulsante destro del mouse su **DirectAccessClients**e scegliere **Proprietà**.  
  
4.  Nella scheda **Membri** fare clic su **Aggiungi**.  
  
5.  Fare clic su **Tipi di oggetto**, selezionare **Computer**, quindi fare clic su **OK**.  
  
6.  Digitare il nome del client da aggiungere, quindi fare clic su **OK**.  
  
7.  Fare clic su **OK** per chiudere la finestra di dialogo proprietà **DirectAccessClients** , quindi chiudere **Active Directory utenti e computer**.  
  
##### <a name="copy-and-then-apply-the-provisioning-package-to-the-client-computer"></a>Copiare e quindi applicare il pacchetto di provisioning al computer client  
  
1.  Copiare il pacchetto di provisioning da c:\files\provision.txt nel server di accesso remoto, in cui è stato salvato, in c:\provision\provision.txt nel computer client.  
  
2.  Nel computer client aprire un prompt dei comandi con privilegi elevati e quindi digitare il comando seguente per richiedere l'aggiunta al dominio:  
  
    ```  
    Djoin /requestodj /loadfile C:\provision\provision.txt /windowspath %windir% /localos  
    ```  
  
3.  Riavviare il computer client. Il computer verrà aggiunto al dominio. Dopo il riavvio, il client verrà aggiunto al dominio e avrà la connettività alla rete aziendale con DirectAccess.  
  
## <a name="see-also"></a>Vedi anche  
[NetProvisionComputerAccount (funzione)](https://go.microsoft.com/fwlink/?LinkId=162426)  
[NetRequestOfflineDomainJoin (funzione)](https://go.microsoft.com/fwlink/?LinkId=162427)  
  


