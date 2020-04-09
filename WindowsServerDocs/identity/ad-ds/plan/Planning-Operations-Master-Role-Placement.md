---
ms.assetid: bd64a766-5362-4f29-b963-5465c2bb79e7
title: Pianificazione del posizionamento del ruolo master operazioni
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/08/2018
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: 990f93d44189a6061653d5e190a176b049a280c4
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80822104"
---
# <a name="planning-operations-master-role-placement"></a>Pianificazione del posizionamento del ruolo master operazioni

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Active Directory Domain Services (AD DS) supporta la replica multimaster dei dati di directory, il che significa che qualsiasi controller di dominio può accettare le modifiche della directory e replicare le modifiche in tutti gli altri controller di dominio. Tuttavia, alcune modifiche, ad esempio le modifiche dello schema, non sono pratiche da eseguire in modo multimaster. Per questo motivo, alcuni controller di dominio, noti come master operazioni, tengono i ruoli responsabili dell'accettazione delle richieste per determinate modifiche specifiche.  
  
> [!NOTE]  
> I titolari del ruolo master operazioni devono essere in grado di scrivere alcune informazioni nel database di Active Directory. A causa della natura di sola lettura del database di Active Directory in un controller di dominio di sola lettura (RODC), **RODC non può fungere da titolari del ruolo di master operazioni**.  
  
In ogni dominio sono presenti tre ruoli master operazioni, noti anche come FSMO (Flexible Single Master Operation):  
  
- Il master operazioni dell'emulatore del controller di dominio primario (PDC) elabora tutti gli aggiornamenti delle password.  

- Il master operazioni ID relativo (RID) gestisce il pool di RID globale per il dominio e alloca pool RID locali a tutti i controller di dominio per garantire che tutte le entità di sicurezza create nel dominio abbiano un identificatore univoco.  
- Il master operazioni dell'infrastruttura per un determinato dominio mantiene un elenco delle entità di sicurezza di altri domini membri dei gruppi all'interno del dominio.  

Oltre ai tre ruoli master operazioni a livello di dominio, in ogni foresta sono presenti due ruoli master operazioni:  
  
- Il master operazioni schema regola le modifiche allo schema.  
- Il master operazioni per la denominazione dei domini aggiunge e rimuove domini e altre partizioni di directory (ad esempio, Domain Name System le partizioni dell'applicazione (DNS)) da e verso la foresta.  
  
Posizionare i controller di dominio che ospitano i ruoli di master operazioni in aree in cui l'affidabilità della rete è elevata e assicurarsi che l'emulatore PDC e il master RID siano costantemente disponibili.  
  
I titolari del ruolo master operazioni vengono assegnati automaticamente al momento della creazione del primo controller di dominio in un determinato dominio. I due ruoli a livello di foresta (master schema e master per la denominazione dei domini) vengono assegnati al primo controller di dominio creato in una foresta. Inoltre, i tre ruoli a livello di dominio (master RID, master infrastrutture e emulatore PDC) vengono assegnati al primo controller di dominio creato in un dominio.  
  
> [!NOTE]  
> Le assegnazioni dei titolari del ruolo master operazioni automatiche vengono effettuate solo quando viene creato un nuovo dominio e quando un proprietario del ruolo corrente viene abbassato di nuovo. Tutte le altre modifiche apportate ai proprietari dei ruoli devono essere avviate da un amministratore.  
  
Queste assegnazioni di ruolo master operazioni automatiche possono causare un utilizzo molto elevato della CPU nel primo controller di dominio creato nella foresta o nel dominio. Per evitare questo problema, assegnare (trasferire) i ruoli di master operazioni a diversi controller di dominio nella foresta o nel dominio. Posizionare i controller di dominio che ospitano i ruoli di master operazioni nelle aree in cui la rete è affidabile e in cui è possibile accedere ai master operazioni da tutti gli altri controller di dominio nella foresta.  
  
È inoltre necessario designare i master operazioni standby (alternativa) per tutti i ruoli di master operazioni. I master operazioni standby sono controller di dominio ai quali è possibile trasferire i ruoli di master operazioni in caso di errore dei titolari del ruolo originale. Assicurarsi che i master operazioni standby siano partner di replica diretta degli effettivi master operazioni.  
  
## <a name="planning-the-pdc-emulator-placement"></a>Pianificazione del posizionamento dell'emulatore PDC

L'emulatore PDC elabora le modifiche della password client. Un solo controller di dominio funge da emulatore PDC in ogni dominio della foresta.  
  
Anche se tutti i controller di dominio vengono aggiornati a Windows 2000, Windows Server 2003 e Windows Server 2008 e il dominio opera al livello di funzionalità nativo di Windows 2000, l'emulatore PDC riceve la replica preferenziale delle modifiche della password eseguite da altri controller di dominio nel dominio. Se una password è stata modificata di recente, tale modifica richiede tempo per la replica a ogni controller di dominio nel dominio. Se l'autenticazione di accesso ha esito negativo in un altro controller di dominio a causa di una password non valida, il controller di dominio trasmette la richiesta di autenticazione all'emulatore PDC prima di decidere se accettare o rifiutare il tentativo di accesso.  
  
Posizionare l'emulatore PDC in un percorso che contenga un numero elevato di utenti da tale dominio per le operazioni di invio della password, se necessario. Assicurarsi inoltre che il percorso sia ben connesso ad altre posizioni per ridurre al minimo la latenza di replica.  
  
Per un foglio di lavoro che consente di documentare le informazioni sulla posizione in cui si prevede di inserire gli emulatori PDC e il numero di utenti per ogni dominio rappresentato in ogni posizione, vedere l'articolo relativo ai processi di supporto per Windows Server 2003 Deployment Kit ([https://go.microsoft.com/fwlink/?LinkID=102558](https://go.microsoft.com/fwlink/?LinkID=102558)), scaricare Job_Aids_Designing_and_Deploying_Directory_and_Security_Services. zip e aprire la posizione dei controller di dominio (DSSTOPO_4. doc).  
  
È necessario fare riferimento alle informazioni sui percorsi in cui è necessario inserire gli emulatori PDC quando si distribuiscono i domini regionali. Per ulteriori informazioni sulla distribuzione di domini regionali, vedere [la distribuzione di Windows Server 2008 domini regionali](https://technet.microsoft.com/library/cc755118.aspx).  
  
## <a name="requirements-for-infrastructure-master-placement"></a>Requisiti per la selezione host per master infrastrutture  

Il master infrastrutture aggiorna i nomi delle entità di sicurezza da altri domini aggiunti ai gruppi nel rispettivo dominio. Se, ad esempio, un utente di un dominio è membro di un gruppo in un secondo dominio e il nome dell'utente viene modificato nel primo dominio, al secondo dominio non viene notificato che il nome dell'utente deve essere aggiornato nell'elenco di appartenenza del gruppo. Poiché i controller di dominio in un dominio non replicano le entità di sicurezza nei controller di dominio in un altro dominio, il secondo dominio non viene mai informato della modifica in assenza del master infrastrutture.  
  
Il master infrastrutture monitora costantemente le appartenenze ai gruppi, cercando le entità di sicurezza da altri domini. Se ne trova uno, verifica con il dominio dell'entità di sicurezza per verificare che le informazioni vengano aggiornate. Se le informazioni non sono aggiornate, il master infrastrutture esegue l'aggiornamento e quindi replica la modifica agli altri controller di dominio nel dominio.  
  
A questa regola si applicano due eccezioni. Innanzitutto, se tutti i controller di dominio sono server di catalogo globale, il controller di dominio che ospita il ruolo di master infrastrutture non è significativo perché i cataloghi globali replicano le informazioni aggiornate indipendentemente dal dominio a cui appartengono. In secondo luogo, se la foresta dispone di un solo dominio, il controller di dominio che ospita il ruolo di master infrastrutture non è significativo perché le entità di sicurezza di altri domini non esistono.  
  
Non collocare il master infrastrutture in un controller di dominio che sia anche un server di catalogo globale. Se il master infrastrutture e il catalogo globale si trovano nello stesso controller di dominio, il master infrastrutture non funzionerà. Il master infrastrutture non troverà mai i dati non aggiornati. Pertanto, non verrà mai replicata alcuna modifica apportata agli altri controller di dominio nel dominio.  
  
## <a name="operations-master-placement-for-networks-with-limited-connectivity"></a>Posizionamento del master operazioni per le reti con connettività limitata

Tenere presente che se l'ambiente dispone di una posizione centrale o di un sito hub in cui è possibile inserire i titolari del ruolo di master operazioni, è possibile che vengano interessate determinate operazioni del controller di dominio che dipendono dalla disponibilità di tali titolari del ruolo master operazioni.  
  
Si supponga, ad esempio, che un'organizzazione crei i siti A, B, C e D. i collegamenti di sito sono presenti tra A e B, tra B e C e tra C e D. la connettività di rete rispecchia esattamente la connettività di rete dei collegamenti di siti. In questo esempio, tutti i ruoli di master operazioni vengono inseriti nel sito A e l'opzione per il **Bridge di tutti i collegamenti di sito** non è selezionata.  
  
Anche se questa configurazione comporta una corretta replica tra tutti i siti, le funzioni del ruolo master operazioni presentano le limitazioni seguenti:  
  
- I controller di dominio nei siti C e D non possono accedere all'emulatore PDC nel sito a per aggiornare una password o per verificare la presenza di una password aggiornata di recente.  
- I controller di dominio nei siti C e D non possono accedere al master RID nel sito a per ottenere un pool di RID iniziale dopo l'installazione di Active Directory e aggiornare i pool di RID Man mano che vengono esauriti.  
- I controller di dominio nei siti C e D non possono aggiungere o rimuovere partizioni di directory, DNS o di applicazioni personalizzate.  
- I controller di dominio nei siti C e D non possono apportare modifiche allo schema.  
  
Per un foglio di lavoro che assiste l'utente nella pianificazione del posizionamento dei ruoli di master operazioni, vedere l'argomento [relativo ai supporti per i processi per Windows Server 2003 Deployment Kit](https://go.microsoft.com/fwlink/?LinkID=102558), scaricare Job_Aids_Designing_and_Deploying_Directory_and_Security_Services. zip e aprire la posizione dei controller di dominio (DSSTOPO_4. doc).  
  
È necessario fare riferimento a queste informazioni quando si creano il dominio radice della foresta e i domini regionali. Per ulteriori informazioni sulla distribuzione del dominio radice della foresta, vedere Distribuzione di un [dominio radice della foresta di Windows Server 2008](https://technet.microsoft.com/library/cc731174.aspx). Per ulteriori informazioni sulla distribuzione di domini regionali, vedere [la distribuzione di Windows Server 2008 domini regionali](https://technet.microsoft.com/library/cc755118.aspx).  

## <a name="next-steps"></a>Passaggi successivi

Ulteriori informazioni sul posizionamento dei ruoli FSMO sono disponibili nell'argomento del supporto [posizionamento FSMO e ottimizzazione nei controller di dominio Active Directory](https://support.microsoft.com/help/223346)
