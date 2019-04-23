---
ms.assetid: bd64a766-5362-4f29-b963-5465c2bb79e7
title: Pianificazione del posizionamento del ruolo master operazioni
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/08/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: a3fbe76302199888dce19f845b1c838c07facb82
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59870892"
---
# <a name="planning-operations-master-role-placement"></a>Pianificazione del posizionamento del ruolo master operazioni

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Servizi di dominio Active Directory (AD DS) supporta la replica multimaster di dati di directory, che significa che qualsiasi controller di dominio può accettare le modifiche di directory e replicare le modifiche a tutti gli altri controller di dominio. Tuttavia, alcune modifiche, ad esempio le modifiche dello schema, sono impossibili da eseguire in modo multimaster. Per questo motivo alcuni controller di dominio, noto come master operazioni, i ruoli del responsabile dell'accettazione di richieste per determinate modifiche specifiche.  
  
> [!NOTE]  
> Titolari ruolo di master operazioni devono essere in grado di scrivere informazioni nel database di Active Directory. A causa della natura di sola lettura del database in un controller di dominio di sola lettura (RODC), Active Directory **RODC non possono fungere da titolari del ruolo master operazioni**.  
  
Ruoli di master e tre le operazioni (noto anche come FSMO FSMO, Flexible) disponibili in ogni dominio:  
  
- Il master operazioni dell'emulatore dominio primario (PDC) controller elabora tutti gli aggiornamenti della password.  

- Il master operazioni ID (RID) relativo gestisce il pool di RID globale per il dominio e alloca pool di RID locale in tutti i controller di dominio per assicurarsi che tutte le entità di sicurezza create nel dominio abbiano un nome univoco.  
- Ruolo di master infrastrutture per un determinato dominio gestisce un elenco delle entità di sicurezza da altri domini che sono membri dei gruppi all'interno del dominio.  

Oltre ai ruoli di master e tre le operazioni a livello di dominio, sono disponibili ruoli di master due operazioni in ogni foresta:  
  
- Il master operazioni schema determina le modifiche dello schema.  
- Il master operazioni denominazione domini aggiunge e rimuove altre partizioni di directory (ad esempio, partizioni di applicazione di sistema DNS (Domain Name)) e i domini da e verso la foresta.  
  
Posizionare i controller di dominio che ospita i ruoli di master operazioni nelle aree in cui affidabilità della rete è elevato e assicurarsi che l'emulatore PDC e sul master RID siano disponibili in modo coerente.  
  
Titolari ruolo di master operazioni vengono assegnati automaticamente quando viene creato il primo controller di dominio in un dominio specifico. I due ruoli a livello di foresta (master schema e master denominazione domini) vengono assegnati al primo controller di dominio creato in una foresta. Inoltre, i tre ruoli a livello di dominio (master RID, master infrastrutture ed emulatore PDC) vengono assegnati al primo controller di dominio creato in un dominio.  
  
> [!NOTE]  
> Le assegnazioni titolare di ruolo di master operazioni automatiche vengono eseguite solo quando viene creato un nuovo dominio e quando viene abbassato di livello un titolare del ruolo corrente. Tutte le altre modifiche ai proprietari del ruolo dovranno essere avviate da un amministratore.  
  
Queste assegnazioni di ruolo di master operazioni automatiche possono causare un utilizzo molto elevato della CPU nel primo controller di dominio creato nella foresta o dominio. Per evitare questo problema, assegnare operazioni (trasferimento) ai ruoli di master a vari controller di dominio nel dominio o foresta. Inserire i controller di dominio che host ruoli master operazioni nelle aree in cui la rete sia affidabile e in cui i master operazioni sono accessibile da tutti gli altri controller di dominio nella foresta.  
  
È necessario designare anche le operazioni di standby (alternativa) dei ruoli di master di master per tutte le operazioni. I master operazioni in standby sono controller di dominio a cui è possibile trasferire i ruoli di master operazioni in caso di esito negativo titolari del ruolo originale. Assicurarsi che il master operazioni in standby sono partner di replica diretto dei master operazioni effettive.  
  
## <a name="planning-the-pdc-emulator-placement"></a>Pianificazione del posizionamento dell'emulatore PDC

L'emulatore PDC elabora le modifiche di password del client. Solo un controller di dominio agisce come l'emulatore PDC in ogni dominio nella foresta.  
  
Anche se tutti i controller di dominio vengono aggiornati a Windows 2000, Windows Server 2003 e Windows Server 2008 e il dominio funziona a livello di funzionalità native di Windows 2000, l'emulatore PDC riceve preferenziale della replica delle modifiche alle password eseguite per altri controller di dominio nel dominio. Se è stata recentemente modificata una password, tale modifica impiega tempo per eseguire la replica in ogni controller di dominio nel dominio. Se l'autenticazione di accesso ha esito negativo in un altro controller di dominio a causa di una password errata, il controller di dominio inoltra la richiesta di autenticazione per l'emulatore PDC prima di decidere se accettare o rifiutare il tentativo di accesso.  
  
Posizionare l'emulatore PDC in un percorso che contiene un numero elevato di utenti da quel dominio la password di inoltro operazioni se necessario. Inoltre, assicurarsi che il percorso sia connesso anche in altre posizioni per ridurre al minimo la latenza di replica.  
  
Per un foglio di lavoro agevolare così a documentare le informazioni su cui si prevede di inserire il numero di utenti per ogni dominio che viene rappresentato in ogni posizione e di master emulatore PDC, vedere processo di supporto per Windows Server 2003 Deployment Kit ([ https://go.microsoft.com/fwlink/?LinkID=102558 ](https://go.microsoft.com/fwlink/?LinkID=102558)), scaricare Job_Aids_Designing_and_Deploying_Directory_and_Security_Services.zip e aprire il posizionamento dei Controller di dominio (DSSTOPO_4.doc).  
  
È necessario fare riferimento alle informazioni sui percorsi in cui è necessario inserire master emulatore PDC durante la distribuzione di domini regionali. Per ulteriori informazioni sulla distribuzione di domini regionali, vedere [la distribuzione di Windows Server 2008 domini regionali](https://technet.microsoft.com/library/cc755118.aspx).  
  
## <a name="requirements-for-infrastructure-master-placement"></a>Requisiti per la selezione host di master infrastrutture  

Il master infrastrutture per Aggiorna i nomi delle entità di sicurezza da altri domini che vengono aggiunti ai gruppi nel proprio dominio. Ad esempio, se un utente da un dominio è un membro di un gruppo in un secondo dominio e il nome dell'utente viene modificato nel primo dominio, il secondo dominio non viene notificato che il nome dell'utente deve essere aggiornato nell'elenco di appartenenza del gruppo. Dal momento che i controller di dominio in un dominio non replicano le entità di sicurezza ai controller di dominio in un altro dominio, il secondo dominio non diventa mai conoscenza delle modifiche in assenza di master infrastrutture.  
  
Il master infrastrutture costantemente monitoraggi sull'appartenenza ai gruppi, cercando le entità di sicurezza da altri domini. Se ne trova uno, verifica con il dominio dell'entità di sicurezza per verificare che le informazioni vengono aggiornate. Se le informazioni vengono aggiornate, il master infrastrutture esegue l'aggiornamento e quindi replica la modifica in altri controller di dominio nel proprio dominio.  
  
Due eccezioni si applicano a questa regola. In primo luogo, se tutti i controller di dominio sono i server di catalogo globale, il controller di dominio che ospita il ruolo di master infrastrutture non è significativo perché i cataloghi globali replicano le informazioni aggiornate indipendentemente dal dominio a cui appartengono. In secondo luogo, se la foresta ha un solo dominio, il controller di dominio che ospita il ruolo di master infrastrutture è non significativo poiché non sono presenti le entità di sicurezza da altri domini.  
  
Non inserire il master infrastrutture in un controller di dominio che è anche un server di catalogo globale. Se il master infrastrutture e catalogo globale nel controller di dominio stesso, il master infrastrutture non funzionerà. Il master infrastrutture non troverà mai i dati che non sono aggiornati; pertanto mai replicare le modifiche agli altri controller di dominio nel dominio.  
  
## <a name="operations-master-placement-for-networks-with-limited-connectivity"></a>Selezione host per le reti di master operazioni con connettività limitata

Tenere presente che se l'ambiente ha una posizione centrale o sito hub in cui è possibile inserire i titolari ruolo di master operazioni, alcune operazioni di controller di dominio che dipendono dalla disponibilità di tali operazioni master potrebbero essere interessati possessori di ruolo.  
  
Ad esempio, si supponga che un'organizzazione consente di creare siti A, B, C, e i collegamenti di sito D. esistono tra A e B, tra B e C e tra C e D. connettività di rete esattamente rispecchia la connettività di rete dei collegamenti di siti. In questo esempio, i ruoli di master vengono inseriti in tutte le operazioni del sito A e la possibilità di **Bridge i collegamenti di sito** non è selezionata.  
  
Anche se questa configurazione genera corretta esecuzione della replica tra tutti i siti, le funzioni del ruolo di master operazioni presentano le limitazioni seguenti:  
  
- I controller di dominio nei siti di C e D non è possibile accedere all'emulatore PDC nel sito A per aggiornare una password o per verificare la presenza di una password che è stata aggiornata di recente.  
- I controller di dominio in C e D i siti non possono accedere il master RID nel sito A ottenere un pool RID iniziale dopo l'installazione di Active Directory e per aggiornare i pool di RID come essi esaurirsi.  
- I controller di dominio nei siti di C e D non è possibile aggiungere o rimuovere directory, DNS o le partizioni dell'applicazione personalizzata.  
- I controller di dominio nei siti di C e D non è possibile apportare modifiche allo schema.  
  
Per un foglio di lavoro agevolare la pianificazione del posizionamento del ruolo di master operazioni, vedere [processo di supporto per Windows Server 2003 Deployment Kit](https://go.microsoft.com/fwlink/?LinkID=102558), scaricare Job_Aids_Designing_and_Deploying_Directory_and_Security_Services.zip e aprire Posizionamento dei Controller di dominio (DSSTOPO_4.doc).  
  
Dovrai usare queste informazioni quando si crea il dominio radice della foresta e domini regionali. Per altre informazioni sulla distribuzione di dominio radice della foresta, vedere distribuzione un [distribuzione di un dominio radice foresta di Windows Server 2008](https://technet.microsoft.com/library/cc731174.aspx). Per ulteriori informazioni sulla distribuzione di domini regionali, vedere [la distribuzione di Windows Server 2008 domini regionali](https://technet.microsoft.com/library/cc755118.aspx).  

## <a name="next-steps"></a>Passaggi successivi

Altre informazioni sul posizionamento dei ruoli FSMO sono reperibile nell'argomento supporto [FSMO posizionamento e l'ottimizzazione del controller di dominio Active Directory](https://support.microsoft.com/help/223346)
