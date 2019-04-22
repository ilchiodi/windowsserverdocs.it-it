---
ms.assetid: 3ea48a72-20a2-4da4-84e4-26b5728513ce
title: Pianificazione del controllo dell'accesso ai file
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 65e941f6741298da54186be10bd9c0add115ea14
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59820902"
---
# <a name="plan-for-file-access-auditing"></a>Pianificazione del controllo dell'accesso ai file

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Le informazioni contenute in questo argomento viene illustrato il controllo miglioramenti introdotti in Windows Server 2012 e nuove impostazioni che è opportuno considerare quando si distribuisce il controllo dinamico degli accessi nell'organizzazione di controllo di sicurezza. Le impostazioni dei criteri di controllo che verranno effettivamente distribuite dipendono dagli obiettivi dell'utente, ad esempio conformità ai requisiti, analisi forense e risoluzione dei problemi.  
  
> [!NOTE]  
> Per informazioni dettagliate sulla pianificazione e la distribuzione di una strategia globale di controllo della sicurezza per l'organizzazione, vedere [Pianificazione e distribuzione di criteri di controllo di sicurezza avanzati](https://go.microsoft.com/fwlink/?LinkID=191139). Per ulteriori informazioni sulla configurazione e la distribuzione dei criteri di controllo della sicurezza, vedere la [Guida dettagliata ai criteri avanzati di controllo della sicurezza](https://go.microsoft.com/fwlink/?LinkID=191141).  
  
Le seguenti funzionalità di controllo della protezione in Windows Server 2012 è utilizzabile con controllo dinamico degli accessi per estendere la strategia di controllo di sicurezza complessiva.  
  
-   **Criteri di controllo basati su espressioni**. Il controllo dinamico degli accessi consente di creare criteri di controllo mirati utilizzando espressioni basate su attestazioni utente, computer e risorse. È ad esempio possibile creare un criterio di controllo per tenere traccia di tutte le operazioni di lettura e scrittura relative a file classificati come ad alto impatto aziendale ed eseguite da dipendenti che non dispongono di un Nulla Osta Sicurezza di livello elevato. I criteri di controllo basati su espressioni possono essere creati direttamente per un file o una cartella oppure centralmente mediante Criteri di gruppo. Per ulteriori informazioni, vedere l'argomento relativo ai [Criteri di gruppo basati sul controllo di accesso agli oggetti globale](https://go.microsoft.com/fwlink/?LinkId=241498).  
  
-   **Ulteriori informazioni dal controllo dell'accesso agli oggetti**. Il controllo di accesso file non è una novità per Windows Server 2012. Se sono impostati i criteri di controllo appropriati, i sistemi operativi Windows e Windows Server generano un evento di controllo ogni volta che un utente accede a un file. Gli eventi di accesso ai file esistenti (4656, 4663) contengono informazioni sugli attributi del file a cui è avvenuto l'accesso. Queste informazioni possono essere utilizzate dagli strumenti di filtraggio del registro eventi per l'identificazione degli eventi di controllo più significativi. Per ulteriori informazioni, vedere gli argomenti relativi al [controllo della modifica handle](https://technet.microsoft.com//library/dd772626(WS.10).aspx) e al [controllo del sistema gestione degli account di sicurezza](https://go.microsoft.com/fwlink/?LinkId=241501).  
  
-   **Ulteriori informazioni dagli eventi di accesso utente**. Con i criteri di controllo appropriati impostati, i sistemi operativi Windows generano un evento di controllo ogni volta che un utente accede a un computer da una posizione locale o remota. In Windows Server 2012 o Windows 8, è anche possibile monitorare le attestazioni utente e dispositivo associate al token di sicurezza dell'utente. Gli esempi includono i Nulla Osta Sicurezza, Progetto, Società e Reparto. L'evento 4626 contiene informazioni sulle attestazioni del dispositivo e dell'utente che possono essere utilizzate dagli strumenti di gestione del registro di controllo per mettere in correlazione gli eventi dell'accesso utente agli eventi dell'accesso oggetto per abilitare il filtro eventi in base agli attributi file e utente. Per ulteriori informazioni sul controllo degli accessi utente, vedere l'argomento relativo al [controllo dell'accesso](https://go.microsoft.com/fwlink/?LinkId=241502).  
  
-   **Registrazione di modifiche per nuovi tipi di oggetti a protezione diretta**. La registrazione delle modifiche di oggetti a protezione diretta è importante negli scenari seguenti:  
  
    -   **Registrazione di modifiche per criteri e regole di accesso centrale**. I criteri e le regole di accesso centrale definiscono i criteri centrali da utilizzare per controllare l'accesso alle risorse critiche. Qualsiasi modifica di tali criteri può ripercuotersi direttamente sulle autorizzazioni di accesso ai file che vengono concesse agli utenti su più computer. Per un'organizzazione può essere pertanto importante tenere traccia di tutte le modifiche apportate ai criteri e alle regole di accesso centrale. Poiché i criteri di accesso centrale e le regole di accesso centrale sono archiviate in Active Directory Domain Services (AD DS), è possibile controllare i tentativi di modifica monitorando, ad esempio controllando le modifiche a qualsiasi altro oggetto a protezione diretta in Active Directory Domain Services. Per ulteriori informazioni, vedere l'argomento relativo al [controllo dell'accesso al servizio directory](https://technet.microsoft.com/library/dd941618(WS.10).aspx).  
  
    -   **Registrazione di modifiche per le definizioni nel dizionario delle attestazioni**. La definizione dell'attestazione include nome, descrizione e possibili valori. Qualsiasi modifica apportata alla definizione di un'attestazione può ripercuotersi sulle autorizzazioni di accesso alle risorse critiche. Per un'organizzazione può essere pertanto importante tenere traccia delle definizioni delle attestazioni. Quali criteri di accesso centrale e le regole di accesso centrale, vengono archiviate le definizioni delle attestazioni in AD DS; Pertanto, possono essere controllate, come qualsiasi altro oggetto a protezione diretta in Active Directory Domain Services. Per ulteriori informazioni, vedere l'argomento relativo al [controllo dell'accesso al servizio directory](https://technet.microsoft.com/library/dd941618(WS.10).aspx).  
  
    -   **Registrazione di modifiche per gli attributi dei file**. Gli attributi dei file determinano quale regola di accesso centrale si applica al file. Una modifica degli attributi di un file può dunque avere un potenziale impatto sulle limitazioni di accesso a tale file. Può essere pertanto importante tenere traccia delle modifiche riguardanti gli attributi dei file. È possibile tenere traccia delle modifiche apportate agli attributi dei file su qualsiasi computer configurando il criterio di controllo relativo alla modifica dei criteri di autorizzazione. Per ulteriori informazioni, vedere l'argomento relativo al [controllo della modifica dei criteri di autorizzazione](https://go.microsoft.com/fwlink/?LinkId=241504) e al [controllo dell'accesso agli oggetti per i file system](https://go.microsoft.com/fwlink/?LinkId=241505). In Windows Server 2012, l'evento 4911 riconosce la differenza di modifiche ai criteri di attributo di file altri eventi di modifica dei criteri di autorizzazione.  
  
    -   **Registrazione delle modifiche per i criteri di accesso centrale associati a un file.** L'evento 4913 visualizza gli ID di sicurezza (SID) dei criteri di accesso centrale nuovi e precedenti. Ogni criterio di accesso centrale ha anche un nome descrittivo che può essere cercato utilizzando il SID corrispondente. Per ulteriori informazioni, vedere l'argomento relativo al [controllo della modifica dei criteri di autorizzazione](https://go.microsoft.com/fwlink/?LinkId=241504).  
  
    -   **Registrazione di modifiche per gli attributi utente e computer**. Analogamente ai file, gli oggetti utente e computer possono avere attributi e le modifiche apportate a questi attributi possono influire sulla possibilità dell'utente per accedere ai file. È pertanto importante tenere traccia delle modifiche riguardanti gli attributi di utenti o computer. Gli oggetti utente e computer sono archiviati in Active Directory Domain Services; Pertanto, è possibile controllare le modifiche ai relativi attributi. Per ulteriori informazioni, vedere la pagina relativa all'[accesso ai servizi di dominio](https://go.microsoft.com/fwlink/?LinkId=241508).  
  
-   **Gestione temporanea delle modifiche dei criteri**. Le modifiche dei criteri di accesso centrale possono ripercuotersi sulle decisioni relative al controllo dell'accesso su tutti i computer nei quali i criteri sono applicati. Un criterio troppo ampio potrebbe concedere l'accesso in misura eccessiva, mentre un criterio troppo restrittivo potrebbe generare un numero eccessivo di chiamate al supporto tecnico. Può dunque essere molto utile verificare le modifiche di un criterio di accesso centrale prima di applicarle. A tale scopo, Windows Server 2012 introduce il concetto di "staging". La gestione temporanea consente agli utenti di verificare le modifiche proposte per un criterio prima di applicarle. Per utilizzare la gestione temporanea dei criteri, i criteri proposti vengono distribuiti insieme ai criteri imposti, ma i criteri in gestione temporanea non concedono e non negano veramente le autorizzazioni. Windows Server 2012, invece, registra un evento di controllo (4818) ogni volta che il risultato del controllo di accesso che usa criteri di gestione temporanea è diverso dal risultato di un controllo di accesso che usa criteri imposti.  
  


