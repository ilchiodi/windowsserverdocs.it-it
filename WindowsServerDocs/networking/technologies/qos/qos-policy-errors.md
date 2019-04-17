---
title: Errore relativo ai criteri QoS e messaggi di evento
description: In questo argomento fornisce un elenco di messaggi di eventi ed errori per i criteri di qualità del servizio (QoS) in Windows Server 2016.
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 76974e10-6a57-4533-83be-cfd5a0d364a3
manager: brianlic
ms.author: pashort
author: shortpatti
ms.openlocfilehash: e2e890a7947d4f7de09159d7de606c0542f45045
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/28/2018
---
# <a name="qos-policy-error-and-event-messages"></a>Errore relativo ai criteri QoS e messaggi di evento

>Si applica a: Windows Server (canale annuale e virgola), Windows Server 2016

Di seguito sono i messaggi di eventi ed errori associati ai criteri QoS.  
  
## <a name="informational-messages"></a>Messaggi informativi  

Seguito è riportato un elenco dei messaggi informativi criterio QoS.

|||  
|-|-|  
|**ID messaggio**|16500|  
|**Livello di gravità**|Informativo|  
|**Nome simbolico**|EVENT_EQOS_INFO_MACHINE_POLICY_REFRESH_NO_CHANGE|  
|**Lingua**|Inglese|  
|**Messaggio**|Criteri QoS per computer aggiornati correttamente. Nessuna modifica rilevata.|  
  
|||  
|-|-|  
|**ID messaggio**|16501|  
|**Livello di gravità**|Informativo|  
|**Nome simbolico**|EVENT_EQOS_INFO_MACHINE_POLICY_REFRESH_WITH_CHANGE|  
|**Lingua**|Inglese|  
|**Messaggio**|Criteri QoS per computer aggiornati correttamente. Sono state rilevate modifiche.|  
  
|||  
|-|-|  
|**ID messaggio**|16502|  
|**Livello di gravità**|Informativo|  
|**Nome simbolico**|EVENT_EQOS_INFO_USER_POLICY_REFRESH_NO_CHANGE|  
|**Lingua**|Inglese|  
|**Messaggio**|Criteri di QoS utente aggiornati correttamente. Nessuna modifica rilevata.|  
  
|||  
|-|-|  
|**ID messaggio**|16503|  
|**Livello di gravità**|Informativo|  
|**Nome simbolico**|EVENT_EQOS_INFO_USER_POLICY_REFRESH_WITH_CHANGE|  
|**Lingua**|Inglese|  
|**Messaggio**|Criteri di QoS utente aggiornati correttamente. Sono state rilevate modifiche.|  
  
|||  
|-|-|  
|**ID messaggio**|16504|  
|**Livello di gravità**|Informativo|  
|**Nome simbolico**|EVENT_EQOS_INFO_TCP_AUTOTUNING_NOT_CONFIGURED|  
|**Lingua**|Inglese|  
|**Messaggio**|L'impostazione di QoS avanzate per il livello della velocità effettiva TCP in entrata aggiornati correttamente. Impostazione di valore non viene specificato da alcun criterio QoS. Verrà applicato predefinito per il computer locale.|  
  
|||  
|-|-|  
|**ID messaggio**|16505|  
|**Livello di gravità**|Informativo|  
|**Nome simbolico**|EVENT_EQOS_INFO_TCP_AUTOTUNING_OFF|  
|**Lingua**|Inglese|  
|**Messaggio**|L'impostazione di QoS avanzate per il livello della velocità effettiva TCP in entrata aggiornati correttamente. Valore dell'impostazione è di livello 0 (velocità effettiva minima).|  
  
|||  
|-|-|  
|**ID messaggio**|16506|  
|**Livello di gravità**|Informativo|  
|**Nome simbolico**|EVENT_EQOS_INFO_TCP_AUTOTUNING_HIGHLY_RESTRICTED|  
|**Lingua**|Inglese|  
|**Messaggio**|L'impostazione di QoS avanzate per il livello della velocità effettiva TCP in entrata aggiornati correttamente. Valore dell'impostazione è di livello 1.|  
  
|||  
|-|-|  
|**ID messaggio**|16507|  
|**Livello di gravità**|Informativo|  
|**Nome simbolico**|EVENT_EQOS_INFO_TCP_AUTOTUNING_RESTRICTED|  
|**Lingua**|Inglese|  
|**Messaggio**|L'impostazione di QoS avanzate per il livello della velocità effettiva TCP in entrata aggiornati correttamente. Valore dell'impostazione è di livello 2.|  
  
|||  
|-|-|  
|**ID messaggio**|16508|  
|**Livello di gravità**|Informativo|  
|**Nome simbolico**|EVENT_EQOS_INFO_TCP_AUTOTUNING_NORMAL|  
|**Lingua**|Inglese|  
|**Messaggio**|L'impostazione di QoS avanzate per il livello della velocità effettiva TCP in entrata aggiornati correttamente. Valore dell'impostazione è livello 3 (velocità effettiva massima).|  
  
|||  
|-|-|  
|**ID messaggio**|16509|  
|**Livello di gravità**|Informativo|  
|**Nome simbolico**|EVENT_EQOS_INFO_APP_MARKING_NOT_CONFIGURED|  
|**Lingua**|Inglese|  
|**Messaggio**|L'impostazione di QoS avanzate per il contrassegno DSCP ha aggiornato correttamente. Impostazione di valore non viene specificato. Le applicazioni possono impostare i valori DSCP indipendentemente dai criteri QoS.|  
  
|||  
|-|-|  
|**ID messaggio**|16510|  
|**Livello di gravità**|Informativo|  
|**Nome simbolico**|EVENT_EQOS_INFO_APP_MARKING_IGNORED|  
|**Lingua**|Inglese|  
|**Messaggio**|L'impostazione di QoS avanzate per il contrassegno DSCP ha aggiornato correttamente. Le richieste di contrassegno DSCP applicazione verranno ignorate. Solo i criteri QoS possono impostare i valori DSCP.|  
  
|||  
|-|-|  
|**ID messaggio**|16511|  
|**Livello di gravità**|Informativo|  
|**Nome simbolico**|EVENT_EQOS_INFO_APP_MARKING_ALLOWED|  
|**Lingua**|Inglese|  
|**Messaggio**|L'impostazione di QoS avanzate per il contrassegno DSCP ha aggiornato correttamente. Le applicazioni possono impostare i valori DSCP indipendentemente dai criteri QoS.|  
  
|||  
|-|-|  
|**ID messaggio**|16512|  
|**Livello di gravità**|Informativo|  
|**Nome simbolico**|EVENT_EQOS_INFO_LOCAL_SETTING_DONT_USE_NLA|  
|**Lingua**|Inglese|  
|**Messaggio**|Applicazione selettiva dei criteri di QoS in base alla categoria di rete di dominio è stato disattivato. I criteri QoS verranno applicati a tutte le interfacce di rete.|  
  
## <a name="warning-messages"></a>Messaggi di avviso

Ecco un elenco di messaggi di avviso criterio QoS.

|||  
|-|-|  
|**ID messaggio**|16600|  
|**Livello di gravità**|Avviso|  
|**Nome simbolico**|EVENT_EQOS_WARNING_TEST_1|  
|**Lingua**|Inglese|  
|**Messaggio**|EQOS: * * * Testing\ * \ * \ * [, con una stringa] "%2".|  
  
|||  
|-|-|  
|**ID messaggio**|16601|  
|**Livello di gravità**|Avviso|  
|**Nome simbolico**|EVENT_EQOS_WARNING_TEST_2|  
|**Lingua**|Inglese|  
|**Messaggio**|EQOS: * * * Testing\ * \ * \ * [, con due stringhe, stringa1 è] "%2" [, stringa2 è] "%3".|  
  
|||  
|-|-|  
|**ID messaggio**|16602|  
|**Livello di gravità**|Avviso|  
|**Nome simbolico**|EVENT_EQOS_WARNING_MACHINE_POLICY_VERSION|  
|**Lingua**|Inglese|  
|**Messaggio**|Il computer criterio QoS "%2" ha un numero di versione non valida. Non verrà applicato il criterio.|  
  
|||  
|-|-|  
|**ID messaggio**|16603|  
|**Livello di gravità**|Avviso|  
|**Nome simbolico**|EVENT_EQOS_WARNING_USER_POLICY_VERSION|  
|**Lingua**|Inglese|  
|**Messaggio**|L'utente dei criteri QoS "%2" ha un numero di versione non valida. Non verrà applicato il criterio.|  
  
|||  
|-|-|  
|**ID messaggio**|16604|  
|**Livello di gravità**|Avviso|  
|**Nome simbolico**|EVENT_EQOS_WARNING_MACHINE_POLICY_PROFILE_NOT_SPECIFIED|  
|**Lingua**|Inglese|  
|**Messaggio**|I criteri di QoS "%2" del computer non specifica DSCP valore o limitazione di velocità. Non verrà applicato il criterio.|  
  
|||  
|-|-|  
|**ID messaggio**|16605|  
|**Livello di gravità**|Avviso|  
|**Nome simbolico**|EVENT_EQOS_WARNING_USER_POLICY_PROFILE_NOT_SPECIFIED|  
|**Lingua**|Inglese|  
|**Messaggio**|Il criterio QoS "%2" dell'utente non specifica DSCP valore o limitazione di velocità. Non verrà applicato il criterio.|  
  
|||  
|-|-|  
|**ID messaggio**|16606|  
|**Livello di gravità**|Avviso|  
|**Nome simbolico**|EVENT_EQOS_WARNING_MACHINE_POLICY_QUOTA_EXCEEDED|  
|**Lingua**|Inglese|  
|**Messaggio**|Superato il numero massimo di criteri QoS per computer. Il criterio QoS "%2" e i criteri QoS per computer successivi non verranno applicati.|  
  
|||  
|-|-|  
|**ID messaggio**|16607|  
|**Livello di gravità**|Avviso|  
|**Nome simbolico**|EVENT_EQOS_WARNING_USER_POLICY_QUOTA_EXCEEDED|  
|**Lingua**|Inglese|  
|**Messaggio**|Superato il numero massimo di criteri di QoS di utente. Il criterio QoS "%2" e i criteri QoS utente successivi non verranno applicati.|  
  
|||  
|-|-|  
|**ID messaggio**|16608|  
|**Livello di gravità**|Avviso|  
|**Nome simbolico**|EVENT_EQOS_WARNING_MACHINE_POLICY_CONFLICT|  
|**Lingua**|Inglese|  
|**Messaggio**|I criteri di QoS "%2" del computer è potenzialmente in conflitto con altri criteri QoS. Vedere la documentazione per le regole su cui verrà applicato.|  
  
|||  
|-|-|  
|**ID messaggio**|16609|  
|**Livello di gravità**|Avviso|  
|**Nome simbolico**|EVENT_EQOS_WARNING_USER_POLICY_CONFLICT|  
|**Lingua**|Inglese|  
|**Messaggio**|Il criterio QoS "%2" dell'utente è potenzialmente in conflitto con altri criteri QoS. Vedere la documentazione per le regole su cui verrà applicato.|  
  
|||  
|-|-|  
|**ID messaggio**|16610|  
|**Livello di gravità**|Avviso|  
|**Nome simbolico**|EVENT_EQOS_WARNING_MACHINE_POLICY_NO_FULLPATH_APPNAME|  
|**Lingua**|Inglese|  
|**Messaggio**|I criteri di QoS "%2" del computer è stato ignorato perché il percorso dell'applicazione non può essere elaborato. Il percorso dell'applicazione potrebbe non essere valido, contengono una lettera di unità non valido o contenere un'unità di rete mappata.|  
  
|||  
|-|-|  
|**ID messaggio**|16611|  
|**Livello di gravità**|Avviso|  
|**Nome simbolico**|EVENT_EQOS_WARNING_USER_POLICY_NO_FULLPATH_APPNAME|  
|**Lingua**|Inglese|  
|**Messaggio**|Il criterio QoS "%2" dell'utente è stato ignorato perché il percorso dell'applicazione non può essere elaborato. Il percorso dell'applicazione potrebbe non essere valido, contengono una lettera di unità non valido o contenere un'unità di rete mappata.|  
  
## <a name="error-messages"></a>Messaggi di errore  

Ecco un elenco di messaggi di errore dei criteri QoS.

|||  
|-|-|  
|**ID messaggio**|16700|  
|**Livello di gravità**|Errore|  
|**Nome simbolico**|EVENT_EQOS_ERROR_MACHINE_POLICY_REFERESH|  
|**Lingua**|Inglese|  
|**Messaggio**|Impossibile aggiornare criteri QoS per computer. Codice di errore: "%2".|  
  
|||  
|-|-|  
|**ID messaggio**|16701|  
|**Livello di gravità**|Errore|  
|**Nome simbolico**|EVENT_EQOS_ERROR_USER_POLICY_REFERESH|  
|**Lingua**|Inglese|  
|**Messaggio**|Impossibile aggiornare i criteri QoS utente. Codice di errore: "%2".|  
  
|||  
|-|-|  
|**ID messaggio**|16702|  
|**Livello di gravità**|Errore|  
|**Nome simbolico**|EVENT_EQOS_ERROR_OPENING_MACHINE_POLICY_ROOT_KEY|  
|**Lingua**|Inglese|  
|**Messaggio**|QoS non è riuscito ad aprire la chiave radice a livello di computer per i criteri QoS. Codice di errore: "%2".|  
  
|||  
|-|-|  
|**ID messaggio**|16703|  
|**Livello di gravità**|Errore|  
|**Nome simbolico**|EVENT_EQOS_ERROR_OPENING_USER_POLICY_ROOT_KEY|  
|**Lingua**|Inglese|  
|**Messaggio**|QoS non è riuscito ad aprire la chiave radice a livello di utente per i criteri QoS. Codice di errore: "%2".|  
  
|||  
|-|-|  
|**ID messaggio**|16704|  
|**Livello di gravità**|Errore|  
|**Nome simbolico**|EVENT_EQOS_ERROR_MACHINE_POLICY_KEYNAME_TOO_LONG|  
|**Lingua**|Inglese|  
|**Messaggio**|Un criterio QoS per computer supera la lunghezza massima consentita nome. Il criterio è elencato sotto la chiave di radice criteri QoS a livello di computer, con indice "%2".|  
  
|||  
|-|-|  
|**ID messaggio**|16705|  
|**Livello di gravità**|Errore|  
|**Nome simbolico**|EVENT_EQOS_ERROR_USER_POLICY_KEYNAME_TOO_LONG|  
|**Lingua**|Inglese|  
|**Messaggio**|Un criterio QoS per utente supera la lunghezza massima consentita nome. Il criterio è elencato sotto la chiave di radice criteri QoS a livello di utente, con indice "%2".|  
  
|||  
|-|-|  
|**ID messaggio**|16706|  
|**Livello di gravità**|Errore|  
|**Nome simbolico**|EVENT_EQOS_ERROR_MACHINE_POLICY_KEYNAME_SIZE_ZERO|  
|**Lingua**|Inglese|  
|**Messaggio**|Un criterio QoS per computer ha un nome di lunghezza zero. Il criterio è elencato sotto la chiave di radice criteri QoS a livello di computer, con indice "%2".|  
  
|||  
|-|-|  
|**ID messaggio**|16707|  
|**Livello di gravità**|Errore|  
|**Nome simbolico**|EVENT_EQOS_ERROR_USER_POLICY_KEYNAME_SIZE_ZERO|  
|**Lingua**|Inglese|  
|**Messaggio**|Criterio QoS per un utente ha un nome di lunghezza zero. Il criterio è elencato sotto la chiave di radice criteri QoS a livello di utente, con indice "%2".|  
  
|||  
|-|-|  
|**ID messaggio**|16708|  
|**Livello di gravità**|Errore|  
|**Nome simbolico**|EVENT_EQOS_ERROR_OPENING_MACHINE_POLICY_SUBKEY|  
|**Lingua**|Inglese|  
|**Messaggio**|Impossibile aprire la sottochiave del Registro di sistema per un criterio QoS per computer QoS. Il criterio è elencato sotto la chiave di radice criteri QoS a livello di computer, con indice "%2".|  
  
|||  
|-|-|  
|**ID messaggio**|16709|  
|**Livello di gravità**|Errore|  
|**Nome simbolico**|EVENT_EQOS_ERROR_OPENING_USER_POLICY_SUBKEY|  
|**Lingua**|Inglese|  
|**Messaggio**|Impossibile aprire la sottochiave del Registro di sistema per un criterio QoS per utente QoS. Il criterio è elencato sotto la chiave di radice criteri QoS a livello di utente, con indice "%2".|  
  
|||  
|-|-|  
|**ID messaggio**|16710|  
|**Livello di gravità**|Errore|  
|**Nome simbolico**|EVENT_EQOS_ERROR_PROCESSING_MACHINE_POLICY_FIELD|  
|**Lingua**|Inglese|  
|**Messaggio**|QoS non è riuscito a leggere o convalidare il campo "%2" per il computer di criteri di QoS di "%3".|  
  
|||  
|-|-|  
|**ID messaggio**|16711|  
|**Livello di gravità**|Errore|  
|**Nome simbolico**|EVENT_EQOS_ERROR_PROCESSING_USER_POLICY_FIELD|  
|**Lingua**|Inglese|  
|**Messaggio**|QoS non è riuscito a leggere o convalidare il campo "%2" per il criterio QoS "%3" dell'utente.|  
  
|||  
|-|-|  
|**ID messaggio**|16712|  
|**Livello di gravità**|Errore|  
|**Nome simbolico**|EVENT_EQOS_ERROR_SETTING_TCP_AUTOTUNING|  
|**Lingua**|Inglese|  
|**Messaggio**|QoS non è riuscito a leggere o impostare in entrata TCP velocità effettiva livello, codice di errore: "%2".|  
  
|||  
|-|-|  
|**ID messaggio**|16713|  
|**Livello di gravità**|Errore|  
|**Nome simbolico**|EVENT_EQOS_ERROR_SETTING_APP_MARKING|  
|**Lingua**|Inglese|  
|**Messaggio**|QoS non è riuscito a leggere o impostare il contrassegno DSCP sostituire l'impostazione, il codice di errore: "%2".|  

Per l'argomento successivo in questa Guida, vedere [QoS criteri Frequently Asked Questions](qos-policy-faq.md).

Per il primo argomento in questa Guida, vedere [criteri Quality of Service (QoS)](qos-policy-top.md).
