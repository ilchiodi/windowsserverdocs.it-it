---
ms.assetid: 3ea48a72-20a2-4da4-84e4-26b5728513ce
title: Pianificare per File di controllo dell'accesso
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 65e941f6741298da54186be10bd9c0add115ea14
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/12/2017
---
# <a name="plan-for-file-access-auditing"></a>Pianificare per File di controllo dell'accesso

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Le informazioni contenute in questo argomento viene illustrato il controllo miglioramenti che sono stati introdotti in Windows Server 2012 e nuove impostazioni che è necessario considerare quando si distribuisce controllo dinamico degli accessi nell'organizzazione di controllo di sicurezza. Le impostazioni di criteri di controllo effettivo da distribuire dipende gli obiettivi, che possono includere la conformità alle normative, il monitoraggio, analisi forense e risoluzione dei problemi.  
  
> [!NOTE]  
> Informazioni dettagliate su come pianificare e distribuire una strategia di controllo per l'organizzazione, vedere globale della sicurezza [pianificazione e distribuzione di criteri avanzati di controllo protezione](https://go.microsoft.com/fwlink/?LinkID=191139). Per ulteriori informazioni sulla configurazione e distribuzione di criteri di controllo di sicurezza, vedere il [criteri di controllo di sicurezza avanzati Step-by-Step Guida](https://go.microsoft.com/fwlink/?LinkID=191141).  
  
Le funzionalità seguenti del controllo di sicurezza in Windows Server 2012 è utilizzabile con controllo dinamico degli accessi per estendere la strategia di controllo di sicurezza globale.  
  
-   **Criteri di controllo basati su espressioni**. Controllo dinamico degli accessi consente di creare criteri di controllo mirati utilizzando espressioni basate su attestazioni utente, computer e risorse. Ad esempio, è possibile creare un criterio di controllo per tenere traccia delle operazioni tutti lettura e scrittura a file classificati come alto impatto aziendale dai dipendenti che non hanno un nulla osta sicurezza elevata. Criteri di controllo basati su espressioni possono essere creati direttamente per un file o una cartella oppure centralmente mediante criteri di gruppo. Per ulteriori informazioni, vedere [criteri di gruppo con controllo dell'accesso oggetto globale](https://go.microsoft.com/fwlink/?LinkId=241498).  
  
-   **Informazioni aggiuntive da controllo di accesso agli oggetti**. Il controllo di accesso file non è una novità per Windows Server 2012. Con i criteri di controllo appropriati sul posto, i sistemi operativi Windows e Windows Server generano un evento di controllo ogni volta che un utente accede a un file. Gli eventi di accesso ai File esistenti (4656, 4663) contengono informazioni sugli attributi del file che ha effettuato l'accesso. Questa informazioni possono essere utilizzate da strumenti di filtraggio del registro eventi per identificare gli eventi di controllo più rilevanti. Per ulteriori informazioni, vedere [controllo della modifica Handle](https://technet.microsoft.com//library/dd772626(WS.10).aspx) e [Gestione account di protezione controllo](https://go.microsoft.com/fwlink/?LinkId=241501).  
  
-   **Ulteriori informazioni dagli eventi di accesso utente**. I criteri di controllo appropriati sul posto, sistemi operativi Windows generano un evento di controllo ogni volta che un utente accede a un computer locale o remota. In Windows Server 2012 o Windows 8, è inoltre possibile monitorare le attestazioni utente e dispositivo associate al token di sicurezza dell'utente. Gli esempi includono nulla osta sicurezza, società, progetto e reparto. Evento 4626 contiene informazioni su queste attestazioni utente e richieste diritti da dispositivo, che possono essere utilizzate dagli strumenti di gestione di log di controllo per mettere in correlazione eventi di accesso utente con gli eventi di accesso oggetto per abilitare il filtro eventi in base agli attributi di file e utente. Per ulteriori informazioni sul controllo degli accessi utente, vedere [Controlla accesso](https://go.microsoft.com/fwlink/?LinkId=241502).  
  
-   **Rilevamento delle modifiche per nuovi tipi di oggetti a protezione diretta**. Tenere traccia delle modifiche agli oggetti a protezione diretta può essere importante negli scenari seguenti:  
  
    -   **Rilevamento delle modifiche per criteri di accesso centrale e regole di accesso centrale**. Criteri di accesso centrale e regole di accesso centrale definiscono i criteri centrali che possono essere utilizzato per controllare l'accesso alle risorse critiche. Qualsiasi modifica di tali può ripercuotersi direttamente sulle autorizzazioni di accesso file che vengono concesse agli utenti in più computer. Tenere traccia delle modifiche ai criteri di accesso centrale e regole di accesso centrale può essere pertanto importante per la tua organizzazione. Poiché i criteri di accesso centrale e regole di accesso centrale sono archiviate in servizi di dominio Active Directory (AD DS), è possibile controllare i tentativi di modifica monitorando, ad esempio, le modifiche apportate a qualsiasi altro oggetto a protezione diretta in servizi di dominio Active Directory. Per ulteriori informazioni, vedere [Controlla accesso al servizio Directory](https://technet.microsoft.com/library/dd941618(WS.10).aspx).  
  
    -   **Rilevamento delle modifiche per le definizioni nel dizionario delle attestazioni**. Le definizioni delle attestazioni includono l'attestazione nome, descrizione e i valori possibili. Qualsiasi modifica apportata alla definizione di un'attestazione può ripercuotersi sulle autorizzazioni di accesso alle risorse critiche. Tenere traccia delle modifiche delle definizioni delle attestazioni può essere pertanto importante nell'organizzazione. Come criteri di accesso centrale e regole di accesso centrale, le definizioni delle attestazioni sono archiviati in Active Directory; Pertanto, è possibile controllare come qualsiasi altro oggetto a protezione diretta in Active Directory. Per ulteriori informazioni, vedere [Controlla accesso al servizio Directory](https://technet.microsoft.com/library/dd941618(WS.10).aspx).  
  
    -   **Rilevamento delle modifiche per gli attributi dei file**. Gli attributi di file determinano quale centrale si applica la regola di accesso al file. Una modifica gli attributi del file potrebbe potenzialmente influire le restrizioni di accesso al file. Può essere pertanto importante tenere traccia delle modifiche agli attributi dei file. È possibile tenere traccia delle modifiche agli attributi dei file in qualsiasi computer configurando il criterio di controllo modifica di criteri di autorizzazione. Per ulteriori informazioni, vedere [il controllo di modifica criteri di autorizzazione](https://go.microsoft.com/fwlink/?LinkId=241504) e [il controllo di accesso agli oggetti per i File System](https://go.microsoft.com/fwlink/?LinkId=241505). In Windows Server 2012, evento 4911 distingue tra file ed attributo da altri eventi di modifica dei criteri di autorizzazione.  
  
    -   **Registrazione di modifiche per criteri di accesso centrale associati a un file.** L'evento 4913 Visualizza gli identificatori di sicurezza (SID) dei criteri di accesso centrale nuovi e precedenti. Ogni criterio di accesso centrale ha anche un nome descrittivo che può essere cercato utilizzando il SID corrispondente. Per ulteriori informazioni, vedere [il controllo di modifica criteri di autorizzazione](https://go.microsoft.com/fwlink/?LinkId=241504).  
  
    -   **Rilevamento delle modifiche per gli attributi utente e computer**. Ad esempio file, gli oggetti utente e computer possono avere attributi e modifiche a questi attributi possono influire sulla possibilità dell'utente per accedere ai file. Pertanto, può essere utile per tenere traccia delle modifiche per gli attributi utente o computer. Gli oggetti utente e computer sono archiviati in Active Directory; Pertanto, è possibile controllare le modifiche ai relativi attributi. Per ulteriori informazioni, vedere [DSAccess](https://go.microsoft.com/fwlink/?LinkId=241508).  
  
-   **Criteri di gestione temporanea delle modifiche**. Le modifiche ai criteri di accesso centrale possono ripercuotersi sulle decisioni relative al controllo di accesso in tutti i computer in cui vengono applicati i criteri. Un criterio troppo ampio potrebbe concedere l'accesso a desiderato e un criterio troppo restrittivo potrebbe generare un numero eccessivo di chiamate al supporto tecnico. Di conseguenza, può essere molto utile verificare le modifiche ai criteri di accesso centrale prima di applicarle. A tale scopo, Windows Server 2012 introduce il concetto di "gestione temporanea". Gestione temporanea consente agli utenti di verificare le modifiche proposte per un criterio prima di applicarle. Per utilizzare gestione temporanea criteri, i criteri proposti vengono distribuiti insieme ai criteri imposti, ma i criteri di gestione temporanea non effettivamente concedere o negare autorizzazioni. Al contrario, Windows Server 2012 registra un evento di controllo (4818) ogni volta che il risultato del controllo di accesso che utilizza criteri di gestione temporanea è diverso dal risultato di un controllo di accesso che utilizza i criteri applicati.  
  


