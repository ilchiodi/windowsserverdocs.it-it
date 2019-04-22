---
title: Errore del criterio di QoS e messaggi di evento
description: In questo argomento fornisce un elenco di messaggi di errore e di eventi per i criteri di qualità del servizio (QoS) in Windows Server 2016.
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 76974e10-6a57-4533-83be-cfd5a0d364a3
manager: brianlic
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 774d9473beed1da861c6827357710133aeaca15e
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59824052"
---
# <a name="qos-policy-error-and-event-messages"></a>Errore del criterio di QoS e messaggi di evento

>Si applica a: Windows Server (canale semestrale), Windows Server 2016

Di seguito sono i messaggi di errori ed eventi che sono associati criteri QoS.  
  
## <a name="informational-messages"></a>Messaggi informativi  

Seguito è riportato un elenco dei messaggi informativi criteri QoS.

|||  
|-|-|  
|**MessageId**|16500|  
|**Severity**|Informativo|  
|**SymbolicName**|EVENT_EQOS_INFO_MACHINE_POLICY_REFRESH_NO_CHANGE|  
|**Lingua**|Inglese|  
|**Messaggio**|Criteri QoS di computer sono stati aggiornati. Nessuna modifica rilevata.|  
  
|||  
|-|-|  
|**MessageId**|16501|  
|**Severity**|Informativo|  
|**SymbolicName**|EVENT_EQOS_INFO_MACHINE_POLICY_REFRESH_WITH_CHANGE|  
|**Lingua**|Inglese|  
|**Messaggio**|Criteri QoS di computer sono stati aggiornati. Sono state rilevate modifiche.|  
  
|||  
|-|-|  
|**MessageId**|16502|  
|**Severity**|Informativo|  
|**SymbolicName**|EVENT_EQOS_INFO_USER_POLICY_REFRESH_NO_CHANGE|  
|**Lingua**|Inglese|  
|**Messaggio**|Criteri QoS di utente sono stati aggiornati. Nessuna modifica rilevata.|  
  
|||  
|-|-|  
|**MessageId**|16503|  
|**Severity**|Informativo|  
|**SymbolicName**|EVENT_EQOS_INFO_USER_POLICY_REFRESH_WITH_CHANGE|  
|**Lingua**|Inglese|  
|**Messaggio**|Criteri QoS di utente sono stati aggiornati. Sono state rilevate modifiche.|  
  
|||  
|-|-|  
|**MessageId**|16504|  
|**Severity**|Informativo|  
|**SymbolicName**|EVENT_EQOS_INFO_TCP_AUTOTUNING_NOT_CONFIGURED|  
|**Lingua**|Inglese|  
|**Messaggio**|L'impostazione di QoS avanzate per il livello di velocità effettiva in entrata TCP è stato aggiornato. Impostazione di valore non è specificato alcun criterio QoS. Verrà applicato quello predefinito per il computer locale.|  
  
|||  
|-|-|  
|**MessageId**|16505|  
|**Severity**|Informativo|  
|**SymbolicName**|EVENT_EQOS_INFO_TCP_AUTOTUNING_OFF|  
|**Lingua**|Inglese|  
|**Messaggio**|L'impostazione di QoS avanzate per il livello di velocità effettiva in entrata TCP è stato aggiornato. Impostazione valore è il livello 0 (velocità effettiva minima).|  
  
|||  
|-|-|  
|**MessageId**|16506|  
|**Severity**|Informativo|  
|**SymbolicName**|EVENT_EQOS_INFO_TCP_AUTOTUNING_HIGHLY_RESTRICTED|  
|**Lingua**|Inglese|  
|**Messaggio**|L'impostazione di QoS avanzate per il livello di velocità effettiva in entrata TCP è stato aggiornato. Impostazione valore è 1 livello.|  
  
|||  
|-|-|  
|**MessageId**|16507|  
|**Severity**|Informativo|  
|**SymbolicName**|EVENT_EQOS_INFO_TCP_AUTOTUNING_RESTRICTED|  
|**Lingua**|Inglese|  
|**Messaggio**|L'impostazione di QoS avanzate per il livello di velocità effettiva in entrata TCP è stato aggiornato. Valore dell'impostazione è di livello 2.|  
  
|||  
|-|-|  
|**MessageId**|16508|  
|**Severity**|Informativo|  
|**SymbolicName**|EVENT_EQOS_INFO_TCP_AUTOTUNING_NORMAL|  
|**Lingua**|Inglese|  
|**Messaggio**|L'impostazione di QoS avanzate per il livello di velocità effettiva in entrata TCP è stato aggiornato. Valore dell'impostazione è il livello 3 (velocità effettiva massima).|  
  
|||  
|-|-|  
|**MessageId**|16509|  
|**Severity**|Informativo|  
|**SymbolicName**|EVENT_EQOS_INFO_APP_MARKING_NOT_CONFIGURED|  
|**Lingua**|Inglese|  
|**Messaggio**|L'impostazione di QoS avanzate per il contrassegno DSCP le sostituzioni sono state aggiornate. Impostazione di valore non è specificato. Le applicazioni possono impostare i valori di DSCP indipendentemente dai criteri QoS.|  
  
|||  
|-|-|  
|**MessageId**|16510|  
|**Severity**|Informativo|  
|**SymbolicName**|EVENT_EQOS_INFO_APP_MARKING_IGNORED|  
|**Lingua**|Inglese|  
|**Messaggio**|L'impostazione di QoS avanzate per il contrassegno DSCP le sostituzioni sono state aggiornate. Le richieste di contrassegno DSCP delle applicazioni verranno ignorate. Solo i criteri QoS possono impostare i valori di DSCP.|  
  
|||  
|-|-|  
|**MessageId**|16511|  
|**Severity**|Informativo|  
|**SymbolicName**|EVENT_EQOS_INFO_APP_MARKING_ALLOWED|  
|**Lingua**|Inglese|  
|**Messaggio**|L'impostazione di QoS avanzate per il contrassegno DSCP le sostituzioni sono state aggiornate. Le applicazioni possono impostare i valori di DSCP indipendentemente dai criteri QoS.|  
  
|||  
|-|-|  
|**MessageId**|16512|  
|**Severity**|Informativo|  
|**SymbolicName**|EVENT_EQOS_INFO_LOCAL_SETTING_DONT_USE_NLA|  
|**Lingua**|Inglese|  
|**Messaggio**|Applicazione selettiva dei criteri di QoS in base alla categoria di rete di dominio è stata disabilitata. I criteri QoS verranno applicati a tutte le interfacce di rete.|  
  
## <a name="warning-messages"></a>Messaggi di avviso

Seguito è riportato un elenco di messaggi di avviso dei criteri QoS.

|||  
|-|-|  
|**MessageId**|16600|  
|**Severity**|Avviso|  
|**SymbolicName**|EVENT_EQOS_WARNING_TEST_1|  
|**Lingua**|Inglese|  
|**Messaggio**|EQOS: * * * test\*\*\*[, con un'unica stringa] "%2".|  
  
|||  
|-|-|  
|**MessageId**|16601|  
|**Severity**|Avviso|  
|**SymbolicName**|EVENT_EQOS_WARNING_TEST_2|  
|**Lingua**|Inglese|  
|**Messaggio**|EQOS: * * * test\*\*\*[, con due stringhe, è string1] "%2" [, string2 ha] "%3".|  
  
|||  
|-|-|  
|**MessageId**|16602|  
|**Severity**|Avviso|  
|**SymbolicName**|EVENT_EQOS_WARNING_MACHINE_POLICY_VERSION|  
|**Lingua**|Inglese|  
|**Messaggio**|I criteri di QoS "%2" del computer contiene un numero di versione non valida. Si applicherà questo criterio.|  
  
|||  
|-|-|  
|**MessageId**|16603|  
|**Severity**|Avviso|  
|**SymbolicName**|EVENT_EQOS_WARNING_USER_POLICY_VERSION|  
|**Lingua**|Inglese|  
|**Messaggio**|L'utente il criterio di QoS per "%2" ha un numero di versione non valida. Si applicherà questo criterio.|  
  
|||  
|-|-|  
|**MessageId**|16604|  
|**Severity**|Avviso|  
|**SymbolicName**|EVENT_EQOS_WARNING_MACHINE_POLICY_PROFILE_NOT_SPECIFIED|  
|**Lingua**|Inglese|  
|**Messaggio**|I criteri di QoS "%2" del computer non specifica una frequenza di limitazione o valore DSCP. Si applicherà questo criterio.|  
  
|||  
|-|-|  
|**MessageId**|16605|  
|**Severity**|Avviso|  
|**SymbolicName**|EVENT_EQOS_WARNING_USER_POLICY_PROFILE_NOT_SPECIFIED|  
|**Lingua**|Inglese|  
|**Messaggio**|L'utente il criterio di QoS per "%2" non specifica una frequenza di limitazione o valore DSCP. Si applicherà questo criterio.|  
  
|||  
|-|-|  
|**MessageId**|16606|  
|**Severity**|Avviso|  
|**SymbolicName**|EVENT_EQOS_WARNING_MACHINE_POLICY_QUOTA_EXCEEDED|  
|**Lingua**|Inglese|  
|**Messaggio**|Superato il numero massimo di criteri QoS per computer. Il criterio di QoS per "%2" e i criteri di QoS successivi non verranno applicati.|  
  
|||  
|-|-|  
|**MessageId**|16607|  
|**Severity**|Avviso|  
|**SymbolicName**|EVENT_EQOS_WARNING_USER_POLICY_QUOTA_EXCEEDED|  
|**Lingua**|Inglese|  
|**Messaggio**|Superato il numero massimo di criteri di QoS di utente. Il criterio di QoS per "%2" e i criteri QoS utente successivi non verranno applicati.|  
  
|||  
|-|-|  
|**MessageId**|16608|  
|**Severity**|Avviso|  
|**SymbolicName**|EVENT_EQOS_WARNING_MACHINE_POLICY_CONFLICT|  
|**Lingua**|Inglese|  
|**Messaggio**|I criteri di QoS "%2" del computer è potenzialmente in conflitto con altri criteri di QoS. Vedere la documentazione per le regole su quali criteri verranno applicati.|  
  
|||  
|-|-|  
|**MessageId**|16609|  
|**Severity**|Avviso|  
|**SymbolicName**|EVENT_EQOS_WARNING_USER_POLICY_CONFLICT|  
|**Lingua**|Inglese|  
|**Messaggio**|L'utente "%2" criteri di QoS potenzialmente in conflitto con altri criteri di QoS. Vedere la documentazione per le regole su quali criteri verranno applicati.|  
  
|||  
|-|-|  
|**MessageId**|16610|  
|**Severity**|Avviso|  
|**SymbolicName**|EVENT_EQOS_WARNING_MACHINE_POLICY_NO_FULLPATH_APPNAME|  
|**Lingua**|Inglese|  
|**Messaggio**|I criteri di QoS "%2" del computer è stato ignorato perché il percorso dell'applicazione non può essere elaborato. Il percorso dell'applicazione potrebbe non essere valido, contenere una lettera di unità non valida o contengono un'unità di rete mappata.|  
  
|||  
|-|-|  
|**MessageId**|16611|  
|**Severity**|Avviso|  
|**SymbolicName**|EVENT_EQOS_WARNING_USER_POLICY_NO_FULLPATH_APPNAME|  
|**Lingua**|Inglese|  
|**Messaggio**|L'utente il criterio di QoS per "%2" è stata ignorata perché il percorso dell'applicazione non può essere elaborato. Il percorso dell'applicazione potrebbe non essere valido, contenere una lettera di unità non valida o contengono un'unità di rete mappata.|  
  
## <a name="error-messages"></a>Messaggi di errore  

Seguito è riportato un elenco di messaggi di errore dei criteri QoS.

|||  
|-|-|  
|**MessageId**|16700|  
|**Severity**|Errore|  
|**SymbolicName**|EVENT_EQOS_ERROR_MACHINE_POLICY_REFERESH|  
|**Lingua**|Inglese|  
|**Messaggio**|Impossibile aggiornare i criteri QoS di computer. Codice di errore: "%2".|  
  
|||  
|-|-|  
|**MessageId**|16701|  
|**Severity**|Errore|  
|**SymbolicName**|EVENT_EQOS_ERROR_USER_POLICY_REFERESH|  
|**Lingua**|Inglese|  
|**Messaggio**|Impossibile aggiornare i criteri QoS di utente. Codice di errore: "%2".|  
  
|||  
|-|-|  
|**MessageId**|16702|  
|**Severity**|Errore|  
|**SymbolicName**|EVENT_EQOS_ERROR_OPENING_MACHINE_POLICY_ROOT_KEY|  
|**Lingua**|Inglese|  
|**Messaggio**|QoS non è stato possibile aprire la chiave radice a livello di computer per i criteri QoS. Codice di errore: "%2".|  
  
|||  
|-|-|  
|**MessageId**|16703|  
|**Severity**|Errore|  
|**SymbolicName**|EVENT_EQOS_ERROR_OPENING_USER_POLICY_ROOT_KEY|  
|**Lingua**|Inglese|  
|**Messaggio**|QoS non è stato possibile aprire la chiave radice a livello di utente per i criteri QoS. Codice di errore: "%2".|  
  
|||  
|-|-|  
|**MessageId**|16704|  
|**Severity**|Errore|  
|**SymbolicName**|EVENT_EQOS_ERROR_MACHINE_POLICY_KEYNAME_TOO_LONG|  
|**Lingua**|Inglese|  
|**Messaggio**|Un criterio QoS per computer supera la lunghezza del nome consentito massimo. Il criterio viene elencato sotto la chiave di primo livello dei criteri QoS a livello di computer, con indice "%2".|  
  
|||  
|-|-|  
|**MessageId**|16705|  
|**Severity**|Errore|  
|**SymbolicName**|EVENT_EQOS_ERROR_USER_POLICY_KEYNAME_TOO_LONG|  
|**Lingua**|Inglese|  
|**Messaggio**|Un criterio di QoS dell'utente supera la lunghezza del nome consentito massimo. Il criterio viene elencato sotto la chiave di primo livello dei criteri QoS a livello di utente, con indice "%2".|  
  
|||  
|-|-|  
|**MessageId**|16706|  
|**Severity**|Errore|  
|**SymbolicName**|EVENT_EQOS_ERROR_MACHINE_POLICY_KEYNAME_SIZE_ZERO|  
|**Lingua**|Inglese|  
|**Messaggio**|Un criterio QoS per computer ha un nome di lunghezza zero. Il criterio viene elencato sotto la chiave di primo livello dei criteri QoS a livello di computer, con indice "%2".|  
  
|||  
|-|-|  
|**MessageId**|16707|  
|**Severity**|Errore|  
|**SymbolicName**|EVENT_EQOS_ERROR_USER_POLICY_KEYNAME_SIZE_ZERO|  
|**Lingua**|Inglese|  
|**Messaggio**|Un criterio di QoS dell'utente ha un nome di lunghezza zero. Il criterio viene elencato sotto la chiave di primo livello dei criteri QoS a livello di utente, con indice "%2".|  
  
|||  
|-|-|  
|**MessageId**|16708|  
|**Severity**|Errore|  
|**SymbolicName**|EVENT_EQOS_ERROR_OPENING_MACHINE_POLICY_SUBKEY|  
|**Lingua**|Inglese|  
|**Messaggio**|QoS non è stato possibile aprire la sottochiave del Registro di sistema per un computer i criteri QoS. Il criterio viene elencato sotto la chiave di primo livello dei criteri QoS a livello di computer, con indice "%2".|  
  
|||  
|-|-|  
|**MessageId**|16709|  
|**Severity**|Errore|  
|**SymbolicName**|EVENT_EQOS_ERROR_OPENING_USER_POLICY_SUBKEY|  
|**Lingua**|Inglese|  
|**Messaggio**|QoS non è stato possibile aprire la sottochiave del Registro di sistema per un criterio di QoS dell'utente. Il criterio viene elencato sotto la chiave di primo livello dei criteri QoS a livello di utente, con indice "%2".|  
  
|||  
|-|-|  
|**MessageId**|16710|  
|**Severity**|Errore|  
|**SymbolicName**|EVENT_EQOS_ERROR_PROCESSING_MACHINE_POLICY_FIELD|  
|**Lingua**|Inglese|  
|**Messaggio**|QoS non è stato possibile leggere o convalidare il campo "%2" per il computer di criteri QoS di "%3".|  
  
|||  
|-|-|  
|**MessageId**|16711|  
|**Severity**|Errore|  
|**SymbolicName**|EVENT_EQOS_ERROR_PROCESSING_USER_POLICY_FIELD|  
|**Lingua**|Inglese|  
|**Messaggio**|QoS non è stato possibile leggere o convalidare il campo "%2" per il criterio di QoS "%3" dell'utente.|  
  
|||  
|-|-|  
|**MessageId**|16712|  
|**Severity**|Errore|  
|**SymbolicName**|EVENT_EQOS_ERROR_SETTING_TCP_AUTOTUNING|  
|**Lingua**|Inglese|  
|**Messaggio**|QoS non è riuscito a leggere o impostare in entrata TCP velocità effettiva a livello, codice di errore: "%2".|  
  
|||  
|-|-|  
|**MessageId**|16713|  
|**Severity**|Errore|  
|**SymbolicName**|EVENT_EQOS_ERROR_SETTING_APP_MARKING|  
|**Lingua**|Inglese|  
|**Messaggio**|QoS non è riuscito a leggere o impostare il contrassegno DSCP l'override delle impostazioni, il codice di errore: "%2".|  

Per l'argomento successivo in questa Guida, vedere [domande frequenti su criteri di QoS](qos-policy-faq.md).

Per il primo argomento in questa Guida, vedere [dei criteri di qualità del servizio (QoS)](qos-policy-top.md).
