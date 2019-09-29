---
ms.assetid: fde99b44-cb9f-49bf-b888-edaeabe6b88d
title: Indicazioni sui test di clonazione dei controller di dominio virtualizzati per fornitori di applicazioni
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: 2cafb040257b0fbc511e8225b0f07a2071012122
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71360132"
---
# <a name="virtualized-domain-controller-cloning-test-guidance-for-application-vendors"></a>Indicazioni sui test di clonazione dei controller di dominio virtualizzati per fornitori di applicazioni

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

In questo argomento vengono illustrati i fornitori di applicazioni da considerare per garantire che l'applicazione continui a funzionare come previsto dopo il completamento del processo di clonazione del controller di dominio virtualizzato. Vengono illustrati gli aspetti del processo di clonazione che interessano i fornitori e gli scenari di applicazioni che possono richiedere test aggiuntivi. I fornitori di applicazioni che hanno convalidato il funzionamento dell'applicazione nei controller di dominio virtualizzati che sono stati clonati sono invitati a elencare il nome dell'applicazione nel contenuto della community nella parte inferiore di questo argomento, insieme a un collegamento al sito Web dell'organizzazione in cui gli utenti possono ottenere ulteriori informazioni sulla convalida.  
  
## <a name="overview-of-virtualized-dc-cloning"></a>Panoramica della clonazione del controller di dominio virtualizzato  
Il processo di clonazione di controller di dominio virtualizzati è descritto in dettaglio in [Introduzione alla virtualizzazione di Active Directory Domain Services (ad DS) (livello 100)](https://technet.microsoft.com/library/hh831734.aspx) e [riferimento tecnico per i controller di dominio virtualizzati (livello 300)](https://technet.microsoft.com/library/jj574214.aspx). Dal punto di vista del fornitore di un'applicazione, si tratta di alcune considerazioni da tenere in considerazione quando si valuta l'effetto della clonazione nell'applicazione:  
  
-   Il computer originale non viene eliminato definitivamente. Rimane in rete, interagendo con i client. A differenza di una ridenominazione in cui vengono rimossi i record DNS del computer originale, restano i record originali per il controller di dominio di origine.  
  
-   Durante il processo di clonazione, il nuovo computer viene inizialmente eseguito per un breve periodo di tempo sotto l'identità del vecchio computer fino a quando non viene avviato il processo di clonazione e apporta le modifiche necessarie. Le applicazioni che creano record sull'host devono assicurarsi che il computer clonato non sovrascriva i record relativi all'host originale durante il processo di clonazione.  
  
-   La clonazione è una funzionalità di distribuzione specifica per solo i controller di dominio virtualizzati, non un'estensione per utilizzo generico, per clonare altri ruoli del server. Alcuni ruoli del server non sono specificamente supportati per la clonazione:  
  
    -   Dynamic Host Configuration Protocol (DHCP)  
  
    -   Servizi certificati Active Directory  
  
    -   Active Directory Lightweight Directory Services  
  
-   Come parte del processo di clonazione, viene copiata l'intera VM che rappresenta il controller di dominio originale, quindi viene copiato anche qualsiasi stato dell'applicazione in tale macchina virtuale. Verificare che l'applicazione si adatti a questa modifica nello stato dell'host locale nel controller di dominio clonato o se è necessario un intervento, ad esempio il riavvio del servizio.  
  
-   Come parte della clonazione, il nuovo controller di dominio ottiene una nuova identità del computer ed effettua il provisioning come un controller di dominio di replica nella topologia. Verificare che l'applicazione dipenda dall'identità del computer, ad esempio il nome, l'account, il SID e così via. Si adatta automaticamente alla modifica dell'identità del computer nel clone? Se l'applicazione memorizza nella cache i dati, assicurarsi che non si basino sui dati di identità del computer che possono essere memorizzati nella cache.  
  
## <a name="what-is-interesting-for-application-vendors"></a>Che cosa è interessante per i fornitori di applicazioni?  
  
### <a name="customdccloneallowlistxml"></a>File CustomDCCloneAllowList. XML  
Un controller di dominio che esegue l'applicazione o il servizio non può essere clonato fino a quando l'applicazione o il servizio non è uno dei seguenti:  
  
-   Aggiunta al file file CustomDCCloneAllowList. XML tramite il cmdlet Get-ADDCCloningExcludedApplicationList di Windows PowerShell  
  
-Oppure-  
  
-   Rimosso dal controller di dominio  
  
La prima volta che l'utente esegue il cmdlet Get-ADDCCloningExcludedApplicationList restituisce un elenco di servizi e applicazioni in esecuzione sul controller di dominio, ma non nell'elenco predefinito di servizi e applicazioni supportati per la clonazione. Per impostazione predefinita, il servizio o l'applicazione non saranno elencati. Per aggiungere il servizio o l'applicazione all'elenco di applicazioni e servizi che possono essere clonati in modo sicuro, l'utente esegue di nuovo il cmdlet Get-ADDCCloningExcludedApplicationList con l'opzione-GenerateXML per aggiungerlo al file file CustomDCCloneAllowList. XML. Per ulteriori informazioni, vedere [Step 2: Eseguire il cmdlet Get-ADDCCloningExcludedApplicationList @ no__t-0.  
  
### <a name="distributed-system-interactions"></a>Interazioni del sistema distribuito  
In genere i servizi isolati nel computer locale passano o non riescono quando partecipano alla clonazione. Per un breve periodo di tempo, i servizi distribuiti devono preoccuparsi di avere due istanze del computer host nella rete. Questo può manifestarsi come un'istanza del servizio che tenta di estrarre informazioni da un sistema partner in cui il clone è stato registrato come nuovo fornitore dell'identità. Oppure entrambe le istanze del servizio possono eseguire il push delle informazioni nel database di servizi di dominio Active Directory allo stesso tempo con risultati diversi. Ad esempio, non è deterministico con quale computer verrà comunicato quando due computer che dispongono di un servizio Windows Testing Technologies (WTT) si trovano in rete con il controller di dominio.  
  
Per il servizio server DNS distribuito, il processo di clonazione evita con attenzione la sovrascrittura dei record DNS del controller di dominio di origine quando il controller di dominio clone inizia con un nuovo indirizzo IP.  
  
Non fare affidamento sul computer per rimuovere tutte le identità obsolete fino alla fine della clonazione. Dopo la promozione del nuovo controller di dominio all'interno del nuovo contesto, selezionare i provider Sysprep vengono eseguiti per pulire lo stato aggiuntivo del computer. A questo punto, ad esempio, i certificati obsoleti del computer vengono rimossi e i segreti di crittografia a cui il computer può accedere vengono modificati.  
  
Il fattore più importante che varia l'intervallo di clonazione è il numero di oggetti da replicare dal PDC. I supporti meno recenti aumentano il tempo necessario per completare la clonazione.  
  
Poiché il servizio o l'applicazione è sconosciuto, viene lasciato in esecuzione. Il processo di clonazione non modifica lo stato dei servizi non Windows.  
  
Inoltre, il nuovo computer ha un indirizzo IP diverso da quello del computer originale. Questi comportamenti possono causare effetti collaterali al servizio o all'applicazione in base al comportamento del servizio o dell'applicazione in questo ambiente.  
  
## <a name="additional-scenarios-suggested-for-testing"></a>Scenari aggiuntivi consigliati per i test  
  
### <a name="cloning-failure"></a>Errore di clonazione  
I fornitori di servizi devono testare questo scenario perché quando la clonazione non riesce, il computer viene avviato in modalità ripristino servizi directory, un tipo di modalità provvisoria. A questo punto la clonazione del computer non è stata completata. È possibile che alcuni Stati siano stati modificati e che alcuni Stati rimangano dal controller di dominio originale. Testare questo scenario per comprendere l'effetto che può avere sull'applicazione.  
  
Per provocare un errore di clonazione, provare a clonare un controller di dominio senza concedere l'autorizzazione per la clonazione. In questo caso, il computer avrà modificato solo gli indirizzi IP e avrà comunque la maggior parte dello stato del controller di dominio originale. Per ulteriori informazioni sulla concessione di un'autorizzazione per la clonazione di un controller di dominio, vedere [Step 1: Concedere all'origine il controller di dominio virtualizzato l'autorizzazione da clonare @ no__t-0.  
  
### <a name="pdc-emulator-cloning"></a>Clonazione emulatore PDC  
I fornitori di servizi e applicazioni devono testare questo scenario perché è presente un riavvio aggiuntivo quando l'emulatore PDC viene clonato. Inoltre, la maggior parte della clonazione viene eseguita con un'identità temporanea per consentire al nuovo clone di interagire con l'emulatore PDC durante il processo di clonazione.  
  
### <a name="writable-versus-read-only-domain-controllers"></a>Controller di dominio scrivibili e di sola lettura  
I fornitori di servizi e applicazioni devono testare la clonazione usando lo stesso tipo di controller di dominio, ovvero in un controller di dominio scrivibile o di sola lettura, su cui è pianificato l'esecuzione del servizio.  
  


