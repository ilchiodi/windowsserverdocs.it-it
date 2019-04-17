---
ms.assetid: bd64a766-5362-4f29-b963-5465c2bb79e7
title: Pianificazione del posizionamento del ruolo di Master operazioni
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 9d271ed3a54e78106aed0d4dbfdb610cc8b9a460
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/12/2017
---
# <a name="planning-operations-master-role-placement"></a>Pianificazione del posizionamento del ruolo di Master operazioni

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Servizi di dominio Active Directory (AD DS) supporta la replica multimaster dei dati di directory, ovvero qualsiasi controller di dominio può accettare le modifiche di directory e replicare le modifiche a tutti gli altri controller di dominio. Tuttavia, alcune modifiche, ad esempio le modifiche dello schema, sono impossibili da eseguire in modo multimaster. Per questo motivo determinati controller di dominio, noto come master operazioni, detengono ruoli responsabile per accettare le richieste per alcune modifiche specifiche.  
  
> [!NOTE]  
> Titolari ruolo di master operazioni devono essere in grado di scrivere alcune informazioni nel database di Active Directory. A causa della natura di sola lettura del database di Active Directory su un controller di dominio di sola lettura (RODC), lettura non può fungere da titolari ruolo di master operazioni.  
  
Ruoli di master tre operazioni (noto anche come flexible single master operation FSMO, Flexible) disponibili in ogni dominio:  
  
-   Il master operazioni di dominio primario (PDC) controller emulatore elabora tutti gli aggiornamenti di password.  
  
-   Il master ID relativo (RID) operations mantiene il pool di RID globale per il dominio e assegna i pool di RID locali in tutti i controller di dominio per assicurarsi che tutte le entità di sicurezza creata nel dominio dispone di un identificatore univoco.  
  
-   Ruolo di master infrastrutture per un determinato dominio mantiene un elenco di entità di sicurezza da altri domini che sono membri dei gruppi all'interno del dominio.  
  
Oltre ai ruoli di master tre operazioni a livello di dominio, i ruoli di master due operazioni presenti in ogni foresta:  
  
-   Il master operazioni schema determina le modifiche allo schema.  
  
-   Il master operazioni denominazione domini aggiunge e rimuove i domini e altre partizioni di directory (ad esempio, sistema DNS (Domain Name) applicative) da e verso l'insieme di strutture.  
  
Posizionare i controller di dominio che ospita i ruoli di master operazioni nelle aree in cui è elevata affidabilità della rete e assicurarsi che l'emulatore PDC e sul master RID siano disponibili in modo coerente.  
  
Titolari ruolo di master operazioni vengono assegnati automaticamente quando viene creato il primo controller di dominio in un dominio specifico. Per il primo controller di dominio creato in una foresta vengono assegnati i due ruoli a livello di foresta (master schema e master denominazione domini). Inoltre, i tre ruoli a livello di dominio (master RID master infrastrutture e dell'emulatore PDC) vengono assegnati per il primo controller di dominio creato in un dominio.  
  
> [!NOTE]  
> Le assegnazioni di titolare ruolo di master operazioni automatiche vengono eseguite solo quando viene creato un nuovo dominio e quando viene abbassato di livello un titolare del ruolo corrente. Tutte le altre modifiche per i proprietari del ruolo devono essere avviato da un amministratore.  
  
Le assegnazioni di ruolo di master operazioni automatiche possono causare un utilizzo molto elevato della CPU nel primo controller di dominio creato nella foresta o del dominio. Per evitare questo problema, assegnare (trasferimenti) ruoli di master per diversi controller di dominio nel dominio o foresta. Inserire i controller di dominio che host ruoli di master operazioni nelle aree in cui la rete è affidabile e in cui il master operazioni sia accessibile da tutti gli altri controller di dominio nella foresta.  
  
È inoltre necessario definire le operazioni di standby (alternativo) ruoli di master per tutte le operazioni master. Il master operazioni in standby sono controller di dominio a cui è Impossibile trasferire i ruoli di master operazioni nel caso in cui titolari del ruolo originale esito negativo. Assicurarsi che il master operazioni in standby sono partner di replica diretta dei master operazioni effettive.  
  
## <a name="planning-the-pdc-emulator-placement"></a>Pianificazione del posizionamento dell'emulatore PDC  
L'emulatore PDC elabora le modifiche delle password di client. L'emulatore PDC in ogni dominio nella foresta funge da un solo controller di dominio.  
  
Anche se tutti i controller di dominio vengono aggiornati a Windows 2000, Windows Server 2003 e Windows Server 2008 e il dominio opera a livello di funzionalità nativo di Windows 2000, l'emulatore PDC riceve la replica preferenziale delle modifiche alle password eseguite da altri controller di dominio nel dominio. Se è stata recentemente modificata una password, questa modifica richiede tempo per la replica in ogni controller di dominio nel dominio. Se l'autenticazione di accesso ha esito negativo in un altro controller di dominio a causa di una password errata, il controller di dominio inoltra la richiesta di autenticazione per l'emulatore PDC prima di decidere se accettare o rifiutare il tentativo di accesso.  
  
Inserire l'emulatore PDC in un percorso che contiene un numero elevato di utenti del dominio password inoltro operazioni se necessario. Inoltre, assicurarsi che il percorso è ben connesse ad altre posizioni per ridurre al minimo la latenza di replica.  
  
Per un foglio di lavoro agevolare la documentazione le informazioni di cui si prevede di collocare gli emulatori PDC e il numero di utenti per ogni dominio che è rappresentato in ogni percorso, vedere processo Aid per Windows Server 2003 Deployment Kit ([https://go.microsoft.com/fwlink/?LinkID=102558](https://go.microsoft.com/fwlink/?LinkID=102558) ), scaricare Job_Aids_Designing_and_Deploying_Directory_and_Security_Services.zip e aprire (DSSTOPO_4.doc) posizionamento del Controller di dominio.  
  
È necessario fare riferimento alle informazioni sui percorsi in cui è necessario inserire master emulatore PDC durante la distribuzione di domini regionali. Per ulteriori informazioni sulla distribuzione di domini regionali, vedere [la distribuzione di Windows Server 2008 domini regionali](https://technet.microsoft.com/library/cc755118.aspx).  
  
## <a name="requirements-for-infrastructure-master-placement"></a>Requisiti per la selezione host master infrastrutture  

Il master infrastrutture aggiorna i nomi delle entità di sicurezza da altri domini che vengono aggiunti ai gruppi nel dominio. Ad esempio, se un utente da un dominio è un membro di un gruppo in un secondo dominio e il nome dell'utente viene modificato nel primo dominio, il secondo dominio non viene informato che il nome dell'utente deve essere aggiornato nell'elenco di appartenenza del gruppo. Poiché i controller di dominio in un dominio non replicano le entità di sicurezza per i controller di dominio in un altro dominio, al dominio di secondo non viene mai a conoscenza della modifica in assenza del master infrastrutture.  
  
Il master infrastrutture costantemente monitoraggi appartenenza ai gruppi cercando entità di sicurezza da altri domini. Se viene trovato, controlla con il dominio dell'entità di sicurezza per verificare che le informazioni vengono aggiornate. Se le informazioni non sono aggiornate, il master infrastrutture esegue l'aggiornamento e replica le modifiche in altri controller di dominio nel dominio.  
  
Due eccezioni si applicano a questa regola. Prima di tutto, se tutti i controller di dominio sono server di catalogo globale, il controller di dominio che ospita il ruolo di master infrastrutture è irrilevante perché cataloghi globali di replicano le informazioni aggiornate indipendentemente dal dominio a cui appartengono. In secondo luogo, se la foresta include un solo dominio, il controller di dominio che ospita il ruolo di master infrastrutture è irrilevante perché non esistono entità di sicurezza da altri domini.  
  
Non inserire il master infrastrutture in un controller di dominio che è anche un server di catalogo globale. Se il master infrastrutture e catalogo globale sono sullo stesso controller di dominio, il master infrastrutture non funzionerà. Il master infrastrutture sarà possibile trovare i dati che non sono aggiornati; Pertanto, mai verrà replicate tutte le modifiche agli altri controller di dominio nel dominio.  
  
## <a name="operations-master-placement-for-networks-with-limited-connectivity"></a>Selezione host per le reti con connettività limitata di master operazioni  
Tenere presente che se l'ambiente dispone di una posizione centrale o un sito hub in cui è possibile posizionare i titolari del ruolo di master operazioni, alcune operazioni di controller di dominio che dipendono dalla disponibilità di queste operazioni ruolo di master titolari potrebbero risentirne.  
  
Ad esempio, si supponga che un'organizzazione crea siti A, B, C, e i collegamenti di sito D. esistono tra A e B, tra B e C e tra C e D. connettività di rete esattamente riflette la connettività di rete dei collegamenti di siti. In questo esempio, ruoli di master vengono inseriti in tutte le operazioni del sito e la possibilità di **Bridge i collegamenti di sito** non è selezionata.  
  
Sebbene questa configurazione dà esito positivo della replica tra tutti i siti, le funzioni di ruolo di master operazioni sono le seguenti limitazioni:  
  
-   I controller di dominio in siti C e D non possono accedere l'emulatore PDC in un sito per aggiornare una password o per verificare la presenza di una password che è stata aggiornata di recente.  
  
-   I controller di dominio in siti C e D non possono accedere il master RID nel sito per ottenere un pool di RID iniziale dopo l'installazione di Active Directory e aggiornare i pool di RID come che diventano esauriti.  
  
-   I controller di dominio in siti C e D Impossibile aggiungere o rimuovere directory, DNS o partizioni applicazione personalizzata.  
  
-   I controller di dominio in siti C e D non possono apportare modifiche allo schema.  
  
Per un foglio di lavoro facilitare la pianificazione del posizionamento del ruolo di master operazioni, vedere il materiale di supporto per [Windows Server 2003 Deployment Kit](https://go.microsoft.com/fwlink/?LinkID=102558), scaricare Job_Aids_Designing_and_Deploying_Directory_and_Security_Services.zip e aprire (DSSTOPO_4.doc) posizionamento del Controller di dominio.  
  
È necessario fare riferimento a queste informazioni quando si crea il dominio radice della foresta e domini regionali. Per ulteriori informazioni sulla distribuzione di dominio radice della foresta, vedere l'articolo relativo alla distribuzione un [la distribuzione di un dominio radice della foresta Windows Server 2008](https://technet.microsoft.com/library/cc731174.aspx). Per ulteriori informazioni sulla distribuzione di domini regionali, vedere [la distribuzione di Windows Server 2008 domini regionali](https://technet.microsoft.com/library/cc755118.aspx).  
  


