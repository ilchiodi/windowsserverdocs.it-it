---
ms.assetid: fde99b44-cb9f-49bf-b888-edaeabe6b88d
title: Indicazioni sui Test per i fornitori di applicazioni di clonazione del Controller di dominio virtualizzati
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 72c4e818f82d3252c45776b26fb59e095893f2c7
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/12/2017
---
# <a name="virtualized-domain-controller-cloning-test-guidance-for-application-vendors"></a>Indicazioni sui Test per i fornitori di applicazioni di clonazione del Controller di dominio virtualizzati

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Questo argomento illustra cosa è necessario considerare i fornitori di applicazioni per garantire che la loro applicazione continua a funzionare come previsto dopo completamento del processo di clonazione di controller di dominio virtualizzato (DC). Descrive gli aspetti del processo di clonazione di fornitori di applicazioni di interesse e gli scenari che possono giustificare ulteriori verifiche. I fornitori di applicazioni che hanno convalidato che la loro applicazione funziona su controller di dominio virtualizzati duplicati sono invitati a elencare il nome dell'applicazione nel contenuto Community nella parte inferiore di questo argomento, insieme a un collegamento al sito web dell'organizzazione in cui gli utenti per altre informazioni sulla convalida.  
  
## <a name="overview-of-virtualized-dc-cloning"></a>Panoramica di clonazione di controller di dominio virtualizzati  
Processo di clonazione di controller di dominio virtualizzato è descritto in dettaglio in [Introduzione alla virtualizzazione di servizi di dominio Active Directory (AD DS) (livello 100)](https://technet.microsoft.com/library/hh831734.aspx) e [virtualizzato Controller di documentazione tecnica su dominio (livello 300)](https://technet.microsoft.com/library/jj574214.aspx). Dal punto di vista di un fornitore applicazione, ecco alcune considerazioni da tenere in considerazione quando valutare l'impatto della clonazione all'applicazione:  
  
-   Il computer originale non viene eliminato. Rimane nella rete, interagire con i client. A differenza di un'operazione di ridenominazione in cui vengono rimossi i record DNS del computer originale, i record per il controller di dominio di origine originali rimangano.  
  
-   Durante il processo di clonazione, il nuovo computer è inizialmente in esecuzione per un breve periodo di tempo in base all'identità del vecchio computer finché il processo di clonazione viene avviato e apporta le modifiche necessarie. Applicazioni che creano record sull'host è necessario assicurarsi che il computer clonato non sovrascriva record sull'host originale durante il processo di clonazione.  
  
-   La clonazione è una funzionalità di distribuzione specifico per i controller di dominio virtualizzati solo, non un'estensione generica per clonare altri ruoli del server. Alcuni ruoli del server in modo specifico non sono supportati per la clonazione:  
  
    -   Dynamic Host Configuration Protocol (DHCP)  
  
    -   Servizi certificati Active Directory (AD CS)  
  
    -   Active Directory Lightweight Directory Services (AD LDS)  
  
-   Come parte del processo di clonazione, viene copiato l'intera macchina virtuale che rappresenta il controller di dominio originale, in modo che qualsiasi stato dell'applicazione nella macchina virtuale viene inoltre copiato. Verificare che l'applicazione si adatti a questa modifica stato dell'host locale sul controller di dominio clonato, o se sono necessari, come ad esempio un riavvio del servizio siano richiesti interventi.  
  
-   Durante la clonazione, il nuovo controller di dominio ottiene una nuova identità macchina ed effettua il provisioning se stesso come un controller di dominio di replica nella topologia. Stabilire se l'applicazione dipende dall'identità macchina, ad esempio nome, account, SID e così via. Adatteranno automaticamente per la modifica dell'identità macchina nel clone? Se tale applicazione memorizza nella cache i dati, assicurarsi che non si basa sui dati di identità macchina che possono essere memorizzate nella cache.  
  
## <a name="what-is-interesting-for-application-vendors"></a>Che cos'è interessante per fornitori di applicazioni?  
  
### <a name="customdccloneallowlistxml"></a>CustomDCCloneAllowList.xml  
Un controller di dominio che esegue l'applicazione o il servizio non può essere clonato finché l'applicazione o servizio è:  
  
-   Aggiunti al file CustomDCCloneAllowList.xml usando il cmdlet Get-ADDCCloningExcludedApplicationList Windows PowerShell  
  
- O -  
  
-   Rimossi dal controller di dominio  
  
La prima volta che l'utente esegue il cmdlet Get-ADDCCloningExcludedApplicationList, restituisce un elenco di servizi e applicazioni che sono in esecuzione sul controller di dominio ma non sono nell'elenco predefinito di servizi e applicazioni che sono supportate per la clonazione. Per impostazione predefinita, il servizio o l'applicazione non verrà visualizzata. Per aggiungere il servizio o applicazione all'elenco di applicazioni e servizi che possono essere tranquillamente duplicato, il cmdlet Get-ADDCCloningExcludedApplicationList nuovamente con l'opzione - GenerateXML per aggiungerlo a CustomDCCloneAllowList.xml file venga eseguita l'utente. Per ulteriori informazioni, vedere [passaggio 2: il cmdlet Get-ADDCCloningExcludedApplicationList eseguire](https://technet.microsoft.com/library/hh831734.aspx#bkmk6_run_get_addccloningexcludedapplicationlist_cmdlet).  
  
### <a name="distributed-system-interactions"></a>Interazioni di sistema distribuita  
In genere servizi identificati nel computer locale superato o non superato durante la clonazione. Servizi distribuiti necessario preoccuparsi due istanze di computer host nella rete contemporaneamente per un breve periodo di tempo. Questo potrebbe manifesto come un'istanza del servizio tenta di recuperare informazioni dal sistema partner in cui il clone è registrato come nuovo fornitore dell'identità. O entrambe le istanze del servizio possono eseguire il push informazioni nel database di Active Directory nello stesso momento con risultati diversi. Ad esempio, non è deterministico computer a cui verrà comunicato con quando due computer che dispongono del servizio Windows test tecnologie (WTT) si trovano sulla rete con il controller di dominio.  
  
Per il servizio Server DNS distribuito, il processo di clonazione attentamente sovrascritto i record DNS del controller di dominio di origine quando il controller di dominio clone viene avviato con un nuovo indirizzo IP.  
  
Non basarsi sul computer per rimuovere tutta l'identità precedente fino al termine della clonazione. Quando il nuovo controller di dominio viene promosso nel nuovo contesto, selezionare Sysprep per pulire lo stato del computer aggiuntivo vengono eseguiti i provider. Ad esempio, è a questo punto vengono rimossi i certificati del computer precedenti e vengono modificati i segreti di crittografia che il computer può accedere.  
  
Il fattore più importante che varia la tempistica della clonazione è il numero di oggetti per la replica dal controller di dominio primario. Supporti aumentano il tempo necessario per la clonazione completata.  
  
Poiché il servizio o l'applicazione è sconosciuto, resterà in esecuzione. Il processo di clonazione non modifica lo stato dei servizi non Windows.  
  
Inoltre, il nuovo computer ha un indirizzo IP diverso da quello del computer originale. Questi comportamenti potrebbe effetti collaterali per il servizio o l'applicazione a seconda di come si comporta il servizio o applicazione in questo ambiente.  
  
## <a name="additional-scenarios-suggested-for-testing"></a>Altri scenari suggeriti per i test  
  
### <a name="cloning-failure"></a>Errore di clonazione  
Fornitori di servizi necessario testare questo scenario, perché quando si clona ha esito negativo il computer viene avviato in modalità servizi Directory riparazione (DSRM), un modulo della modalità provvisoria. A questo punto il computer non ha completato la clonazione. Alcuni stati sia stato modificato e alcuni Stati possono rimanere dal controller di dominio originale. Questo scenario per comprendere l'impatto che può avere sull'applicazione di test.  
  
Per forzare un errore di clonazione, provare a clonare un controller di dominio senza concedergli l'autorizzazione per la clonazione. In questo caso, il computer sarà solo cambiato gli indirizzi IP e avere comunque la maggior parte dello stato dal controller di dominio originale. Per ulteriori informazioni su come concedere un'autorizzazione di controller di dominio per la clonazione, vedere [passaggio 1: concedere l'autorizzazione per la clonazione di controller di dominio virtualizzati](https://technet.microsoft.com/library/hh831734.aspx#bkmk4_grant_source).  
  
### <a name="pdc-emulator-cloning"></a>Emulatore PDC la clonazione  
Fornitori di servizi e applicazioni necessario testare questo scenario, perché non esiste un ulteriore riavvio quando l'emulatore PDC è duplicato. Inoltre, la maggior parte di clonazione viene eseguita in un'identità temporanea per consentire al nuovo clone interagire con l'emulatore PDC durante il processo di clonazione.  
  
### <a name="writable-versus-read-only-domain-controllers"></a>Scrivibile e i controller di dominio di sola lettura  
I fornitori di servizi e testare la clonazione utilizzando lo stesso tipo di controller di dominio (ovvero, in un controller di dominio di sola lettura o scrivibile) che servizio è pianificato per l'esecuzione in.  
  


