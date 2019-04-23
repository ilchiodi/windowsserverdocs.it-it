---
title: Aggiunta a dominio offline di DirectAccess
description: Questa guida illustra i passaggi per eseguire un'aggiunta a dominio offline con DirectAccess in Windows Server 2016.
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology:
- networking-da
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 55528736-6c19-40bd-99e8-5668169ef3c7
ms.author: pashort
author: shortpatti
ms.openlocfilehash: a9cd811c680f15d53ecbd28d9201f28d9cb8af2f
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59853062"
---
# <a name="directaccess-offline-domain-join"></a>Aggiunta a dominio offline di DirectAccess

>Si applica a: Windows Server (canale semestrale), Windows Server 2016

Questa guida illustra i passaggi per eseguire un'aggiunta a dominio offline con DirectAccess. Durante un'aggiunta a dominio offline, un computer è configurato per l'aggiunta a un dominio senza connessione VPN o fisica.  
  
In questa guida sono inclusi gli argomenti seguenti:  
  
- Panoramica di aggiunta dominio offline  
  
- Requisiti per l'aggiunta a dominio offline
  
- Processo di aggiunta al dominio offline
  
- Passaggi per l'esecuzione di un'aggiunta a dominio offline  
  
## <a name="offline-domain-join-overview"></a>Panoramica di aggiunta dominio offline  
Introdotta in Windows Server 2008 R2, i controller di dominio includono una funzionalità denominata aggiunta a dominio Offline. Un'utilità della riga di comando denominata Djoin.exe consente di aggiungere un computer a un dominio senza fisicamente il tentativo di contattare un controller di dominio durante il completamento dell'operazione di join di dominio. I passaggi generali per l'uso di Djoin.exe sono:  
  
1.  Eseguire **/provision djoin** per creare i metadati dell'account computer. L'output di questo comando è un file con estensione txt che include un blob con codificato base 64.  
  
2.  Eseguire **djoin /requestODJ** per inserire i metadati dell'account computer dal file con estensione txt nella directory di Windows del computer di destinazione.  
  
3.  Riavviare il computer di destinazione e il computer verrà aggiunto al dominio.  
  
### <a name="BKMK_ODJOverview"></a>Aggiunta a dominio offline con panoramica dello scenario criteri DirectAccess  
Aggiunta a dominio offline con DirectAccess è un processo che consentono di aggiungere un dominio senza fisicamente da unire in join alla rete aziendale, i computer che eseguono Windows Server 2016, Windows Server 2012, Windows 10 e Windows 8 o connesse tramite VPN. Questo rende possibile aggiungere i computer a un dominio da posizioni in cui è presente nessuna connettività a una rete aziendale. Aggiunta a dominio offline di DirectAccess fornisce criteri DirectAccess ai clienti per consentire l'acquisizione remota.  
  
Aggiunta a un dominio consente di creare un account computer e stabilisce una relazione di trust tra un computer che eseguono un sistema operativo Windows e un dominio Active Directory.  
  
## <a name="BKMK_ODJRequirements"></a>Preparazione per l'aggiunta a dominio offline  
  
1.  Creare l'account del computer.  
  
2.  L'appartenenza di tutti i gruppi di sicurezza a cui appartiene l'account del computer di inventario.  
  
3.  Raccogliere i certificati di computer necessari, i criteri di gruppo e oggetti Criteri di gruppo da applicare al client di nuovo.  
  
. Le sezioni seguenti illustrano i requisiti del sistema operativo e i requisiti delle credenziali per l'esecuzione di un join di dominio offline di DirectAccess utilizzando Djoin.exe.  
  
### <a name="operating-system-requirements"></a>Requisiti del sistema operativo  
È possibile eseguire Djoin.exe per DirectAccess solo nei computer che eseguono Windows Server 2016, Windows Server 2012 o Windows 8. Il computer in cui si verifica Djoin.exe a disposizione dati dell'account computer Active Directory Domain Services deve essere in esecuzione Windows Server 2016, Windows 10, Windows Server 2012 o Windows 8. Il computer che si desidera aggiungere al dominio deve inoltre essere in esecuzione Windows Server 2016, Windows 10, Windows Server 2012 o Windows 8.  
  
### <a name="credential-requirements"></a>Requisiti relativi alle credenziali  
Per eseguire un'aggiunta a dominio offline, sono necessari i diritti necessari per l'aggiunta di workstation al dominio. I membri del gruppo Domain Admins dispongono di questi diritti per impostazione predefinita. Se non si è un membro del gruppo Domain Admins, un membro del gruppo Domain Admins deve completare una delle azioni seguenti consentono di aggiungere le workstation al dominio:  
  
-   Usare criteri di gruppo per concedere i diritti utente necessari. Questo metodo consente di creare i computer nel contenitore computer predefinito e in qualsiasi unità organizzativa (OU) che viene creato in un secondo momento (se non vengono aggiunto alcun voci di controllo di Nega accesso (ACE)).  
  
-   Modificare l'elenco di controllo di accesso (ACL) del contenitore computer predefinito per il dominio delegare le autorizzazioni corrette all'utente.  
  
-   Creare un'unità Organizzativa e la modifica dell'ACL su quella OU di concedere il **Crea elemento figlio - Consenti** l'autorizzazione. Passare il **/machineOU** parametro per il **djoin /provision** comando.  
  
Le procedure seguenti illustrano come concedere i diritti utente con criteri di gruppo e su come delegare le autorizzazioni corrette.  
  
#### <a name="granting-user-rights-to-join-workstations-to-the-domain"></a>Concessione dei diritti utente per l'aggiunta di workstation al dominio  
È possibile utilizzare la Console Gestione criteri di gruppo (GPMC) per modificare i criteri di dominio o creare un nuovo criterio con le impostazioni che concedono i diritti utente per aggiungere le workstation a un dominio.  
  
L'appartenenza al **Domain Admins**, o equivalente è il requisito minimo necessario per concedere diritti utente.  Informazioni dettagliate sull'utilizzo degli account appropriati e appartenenze [dominio gruppi predefiniti locali e](https://go.microsoft.com/fwlink/?LinkId=83477) (https://go.microsoft.com/fwlink/?LinkId=83477).   
  
###### <a name="to-grant-rights-to-join-workstations-to-a-domain"></a>Per concedere diritti per aggiungere le workstation a un dominio  
  
1.  Fare clic sul pulsante **Start**, scegliere **Strumenti di amministrazione** e quindi fare clic su **Gestione Criteri di gruppo**.  
  
2.  Fare doppio clic sul nome della foresta, fare doppio clic su **domini**, fare doppio clic sul nome del dominio in cui si desidera aggiungere un computer, fare doppio clic su **criterio dominio predefinito**, quindi fare clic su  **Modifica**.  
  
3.  Nell'albero della console, fare doppio clic su **configurazione Computer**, fare doppio clic su **criteri**, fare doppio clic su **impostazioni di Windows**, fare doppio clic su **sicurezza Le impostazioni**, fare doppio clic su **criteri locali**, quindi fare doppio clic su **Assegnazione diritti utente**.  
  
4.  Nel riquadro dei dettagli fare doppio clic **aggiunta di workstation al dominio**.  
  
5.  Selezionare il **Definisci queste impostazioni dei criteri** casella di controllo e quindi fare clic su **Aggiungi utente o gruppo**.  
  
6.  Digitare il nome dell'account che si desidera concedere i diritti utente a e quindi fare clic su **OK** due volte.  
  
## <a name="BKMK_ODKSxS"></a>Processo di aggiunta al dominio offline  
Eseguire Djoin.exe in un prompt dei comandi con privilegi elevati per eseguire il provisioning di metadati dell'account computer. Quando si esegue il comando di provisioning, i metadati dell'account computer viene creato in un file binario specificato come parte del comando.  
  
Per altre informazioni sulla funzione NetProvisionComputerAccount che viene usata per effettuare il provisioning di account del computer durante un'aggiunta a dominio offline, vedere [funzione NetProvisionComputerAccount](https://go.microsoft.com/fwlink/?LinkId=162426) (https://go.microsoft.com/fwlink/?LinkId=162426). Per altre informazioni sulla funzione NetRequestOfflineDomainJoin che viene eseguito localmente nel computer di destinazione, vedere [funzione NetRequestOfflineDomainJoin](https://go.microsoft.com/fwlink/?LinkId=162427) (https://go.microsoft.com/fwlink/?LinkId=162427).  
  
## <a name="BKMK_ODJSteps"></a>Passaggi per l'esecuzione di un'aggiunta a dominio offline con DirectAccess  
Il processo di aggiunta dominio offline include i passaggi seguenti:  
  
1.  Creare un nuovo account computer per ogni client remoto e generare un pacchetto di provisioning usando il comando Djoin.exe da un dominio aggiunto già al computer nella rete aziendale.  
  
2.  Aggiungere il computer client al gruppo di sicurezza DirectAccessClients  
  
3.  Trasferisci il pacchetto di provisioning in modo sicuro per i computer remoti (s) che verranno aggiunto al dominio.  
  
4.  Applicare il pacchetto di provisioning e aggiungere il client al dominio.  
  
5.  Riavviare il client per completare l'aggiunta al dominio e stabilire la connettività.  
  
Sono disponibili due opzioni da prendere in considerazione quando si crea il pacchetto di provisioning per il client. Se si usa la procedura guidata introduttiva per installare DirectAccess senza infrastruttura a chiave pubblica, quindi utilizzare l'opzione 1 riportato di seguito. Se si usa la configurazione guidata avanzata per l'installazione di DirectAccess con infrastruttura a chiave pubblica, si deve usare l'opzione 2 di seguito.  
  
Completare la procedura seguente per eseguire l'aggiunta al dominio offline:  
  
##### <a name="option1-create-a-provisioning-package-for-the-client-without-pki"></a>Opzione 1: Creare un pacchetto di provisioning per il client senza infrastruttura a chiave pubblica  
  
1.  Al prompt dei comandi del server Accesso remoto, digitare il comando seguente per eseguire il provisioning di account del computer:  
  
    ```  
    Djoin /provision /domain <your domain name> /machine <remote machine name> /policynames DA Client GPO name /rootcacerts /savefile c:\files\provision.txt /reuse  
    ```  
  
##### <a name="option2-create-a-provisioning-package-for-the-client-with-pki"></a>Opzione 2: Creare un pacchetto di provisioning per il client con infrastruttura a chiave pubblica  
  
1.  Al prompt dei comandi del server Accesso remoto, digitare il comando seguente per eseguire il provisioning di account del computer:  
  
    ```  
    Djoin /provision /machine <remote machine name> /domain <Your Domain name> /policynames <DA Client GPO name> /certtemplate <Name of client computer cert template> /savefile c:\files\provision.txt /reuse  
    ```  
  
##### <a name="add-the-client-computer-to-the-directaccessclients-security-group"></a>Aggiungere il computer client al gruppo di sicurezza DirectAccessClients  
  
1.  Nel Controller di dominio, dalla **avviare** digitare **Active** e selezionare **Active Directory Users and Computers** da **app** dello schermo .  
  
2.  Espandere l'albero sotto il dominio e selezionare il **utenti** contenitore.  
  
3.  Nel riquadro dei dettagli, fare doppio clic su **DirectAccessClients**, fare clic su **proprietà**.  
  
4.  Nella scheda **Membri** fare clic su **Aggiungi**.  
  
5.  Fare clic su **Tipi di oggetto**, selezionare **Computer**, quindi fare clic su **OK**.  
  
6.  Digitare il nome del client per aggiungere e quindi fare clic su **OK**.  
  
7.  Fare clic su **OK** per chiudere la **DirectAccessClients** finestra di dialogo proprietà e quindi chiudere **Active Directory Users and Computers**.  
  
##### <a name="copy-and-then-apply-the-provisioning-package-to-the-client-computer"></a>Copiare e quindi applicare il pacchetto di provisioning per il computer client  
  
1.  Copiare il pacchetto di provisioning da c:\files\provision.txt nel Server di accesso remoto, in cui è stato salvato, in c:\provision\provision.txt nel computer client.  
  
2.  Nel computer client, aprire un prompt dei comandi con privilegi elevati e quindi digitare il comando seguente per richiedere l'aggiunta al dominio:  
  
    ```  
    Djoin /requestodj /loadfile C:\provision\provision.txt /windowspath %windir% /localos  
    ```  
  
3.  Riavviare il computer client. Il computer verrà aggiunto al dominio. Dopo il riavvio, il client verrà aggiunto al dominio e disporre della connettività alla rete azienda tramite DirectAccess.  
  
## <a name="see-also"></a>Vedere anche  
[NetProvisionComputerAccount (funzione)](https://go.microsoft.com/fwlink/?LinkId=162426)  
[NetRequestOfflineDomainJoin (funzione)](https://go.microsoft.com/fwlink/?LinkId=162427)  
  


