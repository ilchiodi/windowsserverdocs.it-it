---
ms.assetid: fde99b44-cb9f-49bf-b888-edaeabe6b88d
title: Indicazioni sui test di clonazione dei controller di dominio virtualizzati per fornitori di applicazioni
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 0b2303bc837cdaf9f6e7ebd4b3ccbf6c66aa7ad2
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59879342"
---
# <a name="virtualized-domain-controller-cloning-test-guidance-for-application-vendors"></a>Indicazioni sui test di clonazione dei controller di dominio virtualizzati per fornitori di applicazioni

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Questo argomento viene illustrato ciò che i fornitori di applicazioni devono prendere in considerazione per garantire la che propria applicazione continui a funzionare come previsto al termine del processo di clonazione di controller di dominio virtualizzato (DC). Vengono descritti gli aspetti del processo di clonazione tale fornitori di applicazioni di interesse e gli scenari che possono richiedere ulteriori test. I fornitori di applicazioni che sono convalidati che l'applicazione funzioni nei controller di dominio virtualizzati clonati sono invitati a elencare il nome dell'applicazione nei contenuti della Community nella parte inferiore di questo argomento, insieme a un collegamento per il sito web dell'organizzazione in cui gli utenti come descritto in dettaglio la convalida.  
  
## <a name="overview-of-virtualized-dc-cloning"></a>Panoramica della clonazione di controller di dominio virtualizzati  
Processo di clonazione di controller di dominio virtualizzato è descritto dettagliatamente [Introduction to Active Directory Domain Services (AD DS) Virtualization (Level 100)](https://technet.microsoft.com/library/hh831734.aspx) e [virtualizzato tecnico Controller di dominio Riferimento (livello 300)](https://technet.microsoft.com/library/jj574214.aspx). Dal punto di vista del fornitore dell'applicazione, queste sono alcune considerazioni da tenere conto quando si valuta l'impatto della clonazione all'applicazione:  
  
-   Il computer originale non viene eliminato. Il messaggio rimane in rete, l'interazione con i client. A differenza di un'operazione di ridenominazione in cui vengono rimossi i record DNS del computer originale, i record per il controller di dominio di origine originali rimangono.  
  
-   Durante il processo di clonazione, il nuovo computer è inizialmente in esecuzione per un breve periodo di tempo in base all'identità del computer precedente fino a quando il processo di clonazione viene avviato e apporta le modifiche necessarie. Le applicazioni che creano record sull'host è necessario assicurarsi che il computer clonato non sovrascriva record sull'host originale durante il processo di clonazione.  
  
-   La clonazione è una funzionalità di distribuzione specifici per i controller di dominio virtualizzato unico, non un'estensione di utilizzo generico per clonare altri ruoli del server. Alcuni ruoli del server in particolare non sono supportati per la clonazione:  
  
    -   Dynamic Host Configuration Protocol (DHCP)  
  
    -   Servizi certificati Active Directory  
  
    -   Active Directory Lightweight Directory Services  
  
-   Come parte del processo di clonazione, viene copiato l'intera macchina virtuale che rappresenta il controller di dominio originale, in modo che qualsiasi stato dell'applicazione su tale macchina virtuale viene copiato anche. Verificare che l'applicazione si adatta a questa modifica nello stato dell'host locale nel controller di dominio clonato, o se è richiesto, ad esempio un riavvio del servizio alcun intervento.  
  
-   Durante la clonazione, il nuovo controller di dominio ottiene una nuova identità del computer ed esegue il provisioning se stesso come replica controller di dominio nella topologia. Verificare se l'applicazione dipende dall'identità macchina, ad esempio nome, account, SID e così via. Adattano automaticamente per la modifica dell'identità del computer nel clone? Se quell'applicazione memorizza nella cache dei dati, assicurarsi che non si basa sui dati di identità macchina che possono essere memorizzato nella cache.  
  
## <a name="what-is-interesting-for-application-vendors"></a>Che cos'è interessante per i fornitori di applicazioni?  
  
### <a name="customdccloneallowlistxml"></a>CustomDCCloneAllowList.xml  
Non è possibile clonare un controller di dominio che esegue l'applicazione o il servizio finché l'applicazione o servizio è:  
  
-   Aggiunto al file customdccloneallowlist. XML usando il cmdlet Get-ADDCCloningExcludedApplicationList Windows PowerShell  
  
-Oppure-  
  
-   Rimosso dal controller di dominio  
  
La prima volta che l'utente esegue il cmdlet Get-ADDCCloningExcludedApplicationList, restituisce un elenco di servizi e applicazioni che sono in esecuzione nel controller di dominio, ma non sono presenti nell'elenco predefinito dei servizi e applicazioni supportate per la clonazione. Per impostazione predefinita, il servizio o l'applicazione non sarà elencata. Per aggiungere il servizio o l'applicazione all'elenco di applicazioni e servizi che possono essere tranquillamente clonato, le esecuzioni di utente cmdlet Get-ADDCCloningExcludedApplicationList nuovamente con l'opzione - GenerateXML per aggiungerla al customdccloneallowlist. XML file. Per altre informazioni, vedere [passaggio 2: Eseguire il cmdlet Get-ADDCCloningExcludedApplicationList](https://technet.microsoft.com/library/hh831734.aspx#bkmk6_run_get_addccloningexcludedapplicationlist_cmdlet).  
  
### <a name="distributed-system-interactions"></a>Interazioni di sistemi distribuiti  
In genere servizi isolati nel computer locale passano o non riuscire quando che fanno parte di clonazione. Servizi distribuiti sono necessario preoccuparsi di presenza di due istanze di computer host nella rete contemporaneamente per un breve periodo di tempo. Questo può risolversi in un'istanza del servizio tenta di estrarre informazioni da un sistema di partner in cui il clone sia registrato come nuovo fornitore dell'identità. O entrambe le istanze del servizio possono eseguire il push informazioni nel database di Active Directory Domain Services in contemporanea con risultati diversi. Ad esempio, non è deterministica computer a cui verrà comunicato insieme quando due computer che dispone di servizio Windows test tecnologie (WTT) si trovano sulla rete con il controller di dominio.  
  
Per il servizio Server DNS distribuito, il processo di clonazione attentamente evita sovrascrivere i record DNS del controller di dominio di origine quando il controller di dominio clone viene avviato con un nuovo indirizzo IP.  
  
È consigliabile non fare affidamento sul computer da cui rimuovere tutte le identità precedente fino al termine della clonazione. Quando il nuovo controller di dominio viene promosso all'interno del contesto di nuovo, selezionare i provider vengono eseguiti per pulire lo stato del computer aggiuntivo Sysprep. Ad esempio, è a questo punto i certificati del computer precedenti vengono rimossi e vengono modificati i segreti di crittografia che il computer possa accedere.  
  
Il fattore maggiore che variano i tempi della clonazione è il numero di oggetti è per la replica dal controller di dominio primario. Supporti precedenti aumentano il tempo necessario per la clonazione completata.  
  
Poiché il servizio o l'applicazione è sconosciuto, rimarrà in esecuzione. Il processo di clonazione non modifica lo stato dei servizi non Windows.  
  
Inoltre, il nuovo computer ha un indirizzo IP diverso rispetto al computer originale. Questi comportamenti possono causare effetti collaterali al servizio o all'applicazione a seconda del modo in cui il servizio o l'applicazione si comporta in questo ambiente.  
  
## <a name="additional-scenarios-suggested-for-testing"></a>Altri scenari suggeriti per i test  
  
### <a name="cloning-failure"></a>Errori di clonazione  
Fornitori di servizi devono testare questo scenario perché nel caso della clonazione ha esito negativo il computer viene avviato in modalità servizi Directory Repair (DSRM), un form della modalità provvisoria. A questo punto il computer non ha completato la clonazione. Alcuni stati sia stato modificato e uno stato può rimanere dal controller di dominio originale. Testare questo scenario per comprendere quale impatto può avere sull'applicazione.  
  
Per causare un errore di clonazione, cerca di clonare un controller di dominio senza concedergli l'autorizzazione per la clonazione. In questo caso, il computer verrà sono solo stati modificati gli indirizzi IP e la maggior parte del relativo stato dal controller di dominio originale è ancora presente. Per altre informazioni sulla concessione dell'autorizzazione per un controller di dominio da clonare, vedere [passaggio 1: Concedere l'autorizzazione per la clonazione di controller di dominio virtualizzato di origine](https://technet.microsoft.com/library/hh831734.aspx#bkmk4_grant_source).  
  
### <a name="pdc-emulator-cloning"></a>Emulatore PDC la clonazione  
I fornitori di servizio e di applicazione devono testare questo scenario poiché è presente un ulteriore riavvio quando l'emulatore PDC viene clonato. Inoltre, la maggior parte della clonazione viene eseguita con un'identità temporanea per consentire al nuovo clone di interagire con l'emulatore PDC durante il processo di clonazione.  
  
### <a name="writable-versus-read-only-domain-controllers"></a>Accessibile in scrittura e i controller di dominio di sola lettura  
I fornitori di servizio e di applicazione è consigliabile testare la clonazione tramite lo stesso tipo di controller di dominio (vale a dire, in un controller di dominio di sola lettura o scrivibili) che servizio è pianificato per l'esecuzione.  
  


