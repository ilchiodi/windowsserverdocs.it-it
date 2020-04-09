---
ms.assetid: 407d5e81-c04c-4275-9ae9-35f65b4a371a
title: Pianificazione del posizionamento del server di catalogo globale
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: af25688634252ac9f0640ca037d7f2a19b8a8a24
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80822164"
---
# <a name="planning-global-catalog-server-placement"></a>Pianificazione del posizionamento del server di catalogo globale

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

La selezione host del catalogo globale richiede la pianificazione tranne se si dispone di una foresta a dominio singolo. In una foresta a dominio singolo configurare tutti i controller di dominio come server di catalogo globale. Poiché ogni controller di dominio archivia l'unica partizione di directory di dominio nella foresta, la configurazione di ogni controller di dominio come server di catalogo globale non richiede l'utilizzo di spazio su disco aggiuntivo, l'utilizzo della CPU o il traffico di replica. In una foresta a dominio singolo, tutti i controller di dominio fungono da server di catalogo globale virtuali; ovvero possono rispondere a qualsiasi richiesta di autenticazione o di servizio. Questa condizione speciale per le foreste a dominio singolo è da progettazione. Le richieste di autenticazione non richiedono il contatto di un server di catalogo globale come quando sono presenti più domini e un utente può essere membro di un gruppo universale presente in un dominio diverso. Tuttavia, solo i controller di dominio designati come server di catalogo globale possono rispondere alle query del catalogo globale sulla porta 3268 del catalogo globale. Per semplificare l'amministrazione in questo scenario e garantire risposte coerenti, la designazione di tutti i controller di dominio come server di catalogo globale elimina la preoccupazione che i controller di dominio possano rispondere alle query del catalogo globale. In particolare, ogni volta che un utente usa Start\Search\For persone o trova stampanti o espande i gruppi universali, queste richieste passano solo al catalogo globale.  
  
Nelle foreste con più domini, i server di catalogo globale facilitano le richieste di accesso degli utenti e le ricerche a livello di foresta. Nella figura seguente viene illustrato come determinare quali percorsi richiedono server di catalogo globale.  
  
![pianificazione del posizionamento GC](media/Planning-Global-Catalog-Server-Placement/8fc4777c-47b6-4ee7-b8ad-a04e7c5ee67f.gif)  
  
Nella maggior parte dei casi, è consigliabile includere il catalogo globale quando si installano nuovi controller di dominio. Si applicano le seguenti eccezioni:  
  
- Larghezza di banda limitata: nei siti remoti, se il collegamento Wide Area Network (WAN) tra il sito remoto e il sito hub è limitato, è possibile usare la memorizzazione nella cache dell'appartenenza a gruppi universali nel sito remoto per soddisfare le esigenze di accesso degli utenti nel sito.  
- Incompatibilità del ruolo master operazioni infrastruttura: non collocare il catalogo globale in un controller di dominio che ospita il ruolo di master operazioni dell'infrastruttura nel dominio, a meno che tutti i controller di dominio nel dominio non siano server di catalogo globale o la foresta disponga di un solo dominio.  
  
## <a name="adding-global-catalog-servers-based-on-application-requirements"></a>Aggiunta di server di catalogo globale in base ai requisiti dell'applicazione

Alcune applicazioni, ad esempio Microsoft Exchange, Accodamento messaggi (anche noto come MSMQ) e le applicazioni che utilizzano DCOM, non forniscono una risposta adeguata su collegamenti WAN latenti e pertanto necessitano di un'infrastruttura di catalogo globale a disponibilità elevata per garantire una bassa latenza delle query. Determinare se le applicazioni che eseguono un collegamento WAN lento sono in esecuzione in posizioni o se i percorsi richiedono Microsoft Exchange Server. Se le località includono applicazioni che non forniscono una risposta adeguata su un collegamento WAN, è necessario inserire un server di catalogo globale nella posizione per ridurre la latenza delle query.  
  
> [!NOTE]  
> È possibile alzare di livello i controller di dominio di sola lettura (RODC) allo stato del server di catalogo globale. Tuttavia, alcune applicazioni abilitate alla directory non possono supportare un RODC come server di catalogo globale. Ad esempio, nessuna versione di Microsoft Exchange Server utilizza RODC. Tuttavia, Microsoft Exchange Server funziona in ambienti che includono RODC, purché siano disponibili controller di dominio scrivibili. Exchange Server 2007 ignora in modo efficace RODC. Exchange Server 2003 ignora inoltre RODC in condizioni predefinite in cui i componenti di Exchange rilevano automaticamente i controller di dominio disponibili. Non è stata apportata alcuna modifica a Exchange Server 2003 per renderla compatibile con i server di directory di sola lettura. Pertanto, il tentativo di forzare i servizi e gli strumenti di gestione di Exchange Server 2003 per l'utilizzo di RODC può causare un comportamento imprevedibile.  
  
## <a name="adding-global-catalog-servers-for-a-large-number-of-users"></a>Aggiunta di server di catalogo globale per un numero elevato di utenti

Posizionare i server di catalogo globale in tutte le posizioni che contengono più di 100 utenti per ridurre la congestione dei collegamenti WAN di rete e prevenire la perdita di produttività in caso di errore del collegamento WAN.  
  
## <a name="using-highly-available-bandwidth"></a>Uso della larghezza di banda a disponibilità elevata

Non è necessario inserire un catalogo globale in un percorso che non includa applicazioni che richiedono un server di catalogo globale, include meno di 100 utenti ed è connesso anche a un'altra posizione che include un server di catalogo globale tramite un collegamento WAN 100% per Active Directory Domain Services (AD DS). In questo caso, gli utenti possono accedere al server di catalogo globale tramite il collegamento WAN.  
  
Gli utenti del roaming devono contattare i server di catalogo globale ogni volta che accedono per la prima volta in qualsiasi posizione. Se il tempo di accesso tramite il collegamento WAN non è accettabile, inserire un catalogo globale in una posizione visitata da un numero elevato di utenti mobili.  
  
## <a name="enabling-universal-group-membership-caching"></a>Abilitazione della memorizzazione nella cache dell'appartenenza a gruppi universali

Per le posizioni che includono meno di 100 utenti e che non includono un numero elevato di utenti mobili o applicazioni che richiedono un server di catalogo globale, è possibile distribuire controller di dominio che eseguono Windows Server 2008 e abilitare la memorizzazione nella cache dell'appartenenza a gruppi universali. Verificare che i server di catalogo globale non siano più di un hop di replica dal controller di dominio in cui è abilitata la memorizzazione nella cache dell'appartenenza a gruppi universali, in modo che sia possibile aggiornare le informazioni sul gruppo universale nella cache. Per informazioni sul funzionamento della memorizzazione nella cache dei gruppi universali, vedere l'articolo funzionamento [del catalogo globale](https://go.microsoft.com/fwlink/?LinkId=107063).  
  
Per un foglio di lavoro che consente di documentare il punto in cui si prevede di posizionare i server di catalogo globale e i controller di dominio con la memorizzazione nella cache di gruppo universale abilitata, vedere la pagina [relativa ai supporti per i processi per Windows Server 2003 Deployment Kit](https://go.microsoft.com/fwlink/?LinkID=102558), scaricare Job_Aids_Designing_and_Deploying_Directory_and_Security_Services. zip e aprire la selezione host del controller di dominio (DSSTOPO_4 Vedere le informazioni sui percorsi in cui è necessario inserire i server di catalogo globale quando si distribuisce il dominio radice della foresta e i domini regionali.  
