---
title: Messaggi di errore e di evento del criterio QoS
description: In questo argomento viene fornito un elenco di messaggi di errore e di evento per i criteri di qualità del servizio (QoS) in Windows Server 2016.
ms.prod: windows-server
ms.technology: networking
ms.topic: article
ms.assetid: 76974e10-6a57-4533-83be-cfd5a0d364a3
manager: brianlic
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 935bda5ab47f3e9a362c81a8aeb99ebf22095725
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71405326"
---
# <a name="qos-policy-error-and-event-messages"></a>Messaggi di errore e di evento del criterio QoS

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016

Di seguito sono riportati i messaggi di errore e di evento associati ai criteri QoS.  
  
## <a name="informational-messages"></a>Messaggi informativi  

Di seguito è riportato un elenco dei messaggi informativi sui criteri QoS.

|||  
|-|-|  
|**MessageId**|16500|  
|**Gravità**|Informativo|  
|**SymbolicName**|EVENT_EQOS_INFO_MACHINE_POLICY_REFRESH_NO_CHANGE|  
|**Lingua**|Inglese|  
|**Messaggio**|I criteri QoS del computer sono stati aggiornati. Non sono state rilevate modifiche.|  
  
|||  
|-|-|  
|**MessageId**|16501|  
|**Gravità**|Informativo|  
|**SymbolicName**|EVENT_EQOS_INFO_MACHINE_POLICY_REFRESH_WITH_CHANGE|  
|**Lingua**|Inglese|  
|**Messaggio**|I criteri QoS del computer sono stati aggiornati. Sono state rilevate modifiche ai criteri.|  
  
|||  
|-|-|  
|**MessageId**|16502|  
|**Gravità**|Informativo|  
|**SymbolicName**|EVENT_EQOS_INFO_USER_POLICY_REFRESH_NO_CHANGE|  
|**Lingua**|Inglese|  
|**Messaggio**|I criteri QoS utente sono stati aggiornati correttamente. Non sono state rilevate modifiche.|  
  
|||  
|-|-|  
|**MessageId**|16503|  
|**Gravità**|Informativo|  
|**SymbolicName**|EVENT_EQOS_INFO_USER_POLICY_REFRESH_WITH_CHANGE|  
|**Lingua**|Inglese|  
|**Messaggio**|I criteri QoS utente sono stati aggiornati correttamente. Sono state rilevate modifiche ai criteri.|  
  
|||  
|-|-|  
|**MessageId**|16504|  
|**Gravità**|Informativo|  
|**SymbolicName**|EVENT_EQOS_INFO_TCP_AUTOTUNING_NOT_CONFIGURED|  
|**Lingua**|Inglese|  
|**Messaggio**|L'impostazione QoS avanzata per il livello di velocità effettiva TCP in ingresso è stata aggiornata correttamente. Il valore dell'impostazione non è specificato da alcun criterio QoS. Verrà applicata l'impostazione predefinita del computer locale.|  
  
|||  
|-|-|  
|**MessageId**|16505|  
|**Gravità**|Informativo|  
|**SymbolicName**|EVENT_EQOS_INFO_TCP_AUTOTUNING_OFF|  
|**Lingua**|Inglese|  
|**Messaggio**|L'impostazione QoS avanzata per il livello di velocità effettiva TCP in ingresso è stata aggiornata correttamente. Il valore dell'impostazione è livello 0 (velocità effettiva minima).|  
  
|||  
|-|-|  
|**MessageId**|16506|  
|**Gravità**|Informativo|  
|**SymbolicName**|EVENT_EQOS_INFO_TCP_AUTOTUNING_HIGHLY_RESTRICTED|  
|**Lingua**|Inglese|  
|**Messaggio**|L'impostazione QoS avanzata per il livello di velocità effettiva TCP in ingresso è stata aggiornata correttamente. Il valore dell'impostazione è livello 1.|  
  
|||  
|-|-|  
|**MessageId**|16507|  
|**Gravità**|Informativo|  
|**SymbolicName**|EVENT_EQOS_INFO_TCP_AUTOTUNING_RESTRICTED|  
|**Lingua**|Inglese|  
|**Messaggio**|L'impostazione QoS avanzata per il livello di velocità effettiva TCP in ingresso è stata aggiornata correttamente. Il valore dell'impostazione è Level 2.|  
  
|||  
|-|-|  
|**MessageId**|16508|  
|**Gravità**|Informativo|  
|**SymbolicName**|EVENT_EQOS_INFO_TCP_AUTOTUNING_NORMAL|  
|**Lingua**|Inglese|  
|**Messaggio**|L'impostazione QoS avanzata per il livello di velocità effettiva TCP in ingresso è stata aggiornata correttamente. Il valore dell'impostazione è livello 3 (velocità effettiva massima).|  
  
|||  
|-|-|  
|**MessageId**|16509|  
|**Gravità**|Informativo|  
|**SymbolicName**|EVENT_EQOS_INFO_APP_MARKING_NOT_CONFIGURED|  
|**Lingua**|Inglese|  
|**Messaggio**|L'impostazione QoS avanzata per gli override del contrassegno DSCP è stata aggiornata correttamente. Il valore dell'impostazione non è specificato. Le applicazioni possono impostare i valori DSCP indipendentemente dai criteri QoS.|  
  
|||  
|-|-|  
|**MessageId**|16510|  
|**Gravità**|Informativo|  
|**SymbolicName**|EVENT_EQOS_INFO_APP_MARKING_IGNORED|  
|**Lingua**|Inglese|  
|**Messaggio**|L'impostazione QoS avanzata per gli override del contrassegno DSCP è stata aggiornata correttamente. Le richieste di contrassegno DSCP dell'applicazione verranno ignorate. Solo i criteri QoS possono impostare valori DSCP.|  
  
|||  
|-|-|  
|**MessageId**|16511|  
|**Gravità**|Informativo|  
|**SymbolicName**|EVENT_EQOS_INFO_APP_MARKING_ALLOWED|  
|**Lingua**|Inglese|  
|**Messaggio**|L'impostazione QoS avanzata per gli override del contrassegno DSCP è stata aggiornata correttamente. Le applicazioni possono impostare i valori DSCP indipendentemente dai criteri QoS.|  
  
|||  
|-|-|  
|**MessageId**|16512|  
|**Gravità**|Informativo|  
|**SymbolicName**|EVENT_EQOS_INFO_LOCAL_SETTING_DONT_USE_NLA|  
|**Lingua**|Inglese|  
|**Messaggio**|L'applicazione selettiva dei criteri QoS basati sulla categoria rete di dominio è stata disabilitata. I criteri QoS verranno applicati a tutte le interfacce di rete.|  
  
## <a name="warning-messages"></a>Messaggi di avviso

Di seguito è riportato un elenco di messaggi di avviso dei criteri QoS.

|||  
|-|-|  
|**MessageId**|16600|  
|**Gravità**|Avviso|  
|**SymbolicName**|EVENT_EQOS_WARNING_TEST_1|  
|**Lingua**|Inglese|  
|**Messaggio**|EQOS: * * * testing @ no__t-0 @ no__t-1 @ no__t-2 [, con una stringa] "% 2".|  
  
|||  
|-|-|  
|**MessageId**|16601|  
|**Gravità**|Avviso|  
|**SymbolicName**|EVENT_EQOS_WARNING_TEST_2|  
|**Lingua**|Inglese|  
|**Messaggio**|EQOS: * * * testing @ no__t-0 @ no__t-1 @ no__t-2 [, con due stringhe, string1 è] "% 2" [, string2 è] "% 3".|  
  
|||  
|-|-|  
|**MessageId**|16602|  
|**Gravità**|Avviso|  
|**SymbolicName**|EVENT_EQOS_WARNING_MACHINE_POLICY_VERSION|  
|**Lingua**|Inglese|  
|**Messaggio**|Il numero di versione del criterio QoS del computer "% 2" non è valido. Questo criterio non verrà applicato.|  
  
|||  
|-|-|  
|**MessageId**|16603|  
|**Gravità**|Avviso|  
|**SymbolicName**|EVENT_EQOS_WARNING_USER_POLICY_VERSION|  
|**Lingua**|Inglese|  
|**Messaggio**|Il numero di versione del criterio QoS utente "% 2" non è valido. Questo criterio non verrà applicato.|  
  
|||  
|-|-|  
|**MessageId**|16604|  
|**Gravità**|Avviso|  
|**SymbolicName**|EVENT_EQOS_WARNING_MACHINE_POLICY_PROFILE_NOT_SPECIFIED|  
|**Lingua**|Inglese|  
|**Messaggio**|Il criterio QoS del computer "% 2" non specifica un valore DSCP o una frequenza di limitazione. Questo criterio non verrà applicato.|  
  
|||  
|-|-|  
|**MessageId**|16605|  
|**Gravità**|Avviso|  
|**SymbolicName**|EVENT_EQOS_WARNING_USER_POLICY_PROFILE_NOT_SPECIFIED|  
|**Lingua**|Inglese|  
|**Messaggio**|Il criterio QoS utente "% 2" non specifica un valore DSCP o una frequenza di limitazione. Questo criterio non verrà applicato.|  
  
|||  
|-|-|  
|**MessageId**|16606|  
|**Gravità**|Avviso|  
|**SymbolicName**|EVENT_EQOS_WARNING_MACHINE_POLICY_QUOTA_EXCEEDED|  
|**Lingua**|Inglese|  
|**Messaggio**|È stato superato il numero massimo di criteri QoS del computer. I criteri QoS "% 2" e i criteri QoS del computer successivi non verranno applicati.|  
  
|||  
|-|-|  
|**MessageId**|16607|  
|**Gravità**|Avviso|  
|**SymbolicName**|EVENT_EQOS_WARNING_USER_POLICY_QUOTA_EXCEEDED|  
|**Lingua**|Inglese|  
|**Messaggio**|È stato superato il numero massimo di criteri QoS utente. I criteri QoS "% 2" e i criteri QoS utente successivi non verranno applicati.|  
  
|||  
|-|-|  
|**MessageId**|16608|  
|**Gravità**|Avviso|  
|**SymbolicName**|EVENT_EQOS_WARNING_MACHINE_POLICY_CONFLICT|  
|**Lingua**|Inglese|  
|**Messaggio**|Il criterio QoS del computer "% 2" è potenzialmente in conflitto con altri criteri QoS. Vedere la documentazione per le regole relative ai criteri che verranno applicati.|  
  
|||  
|-|-|  
|**MessageId**|16609|  
|**Gravità**|Avviso|  
|**SymbolicName**|EVENT_EQOS_WARNING_USER_POLICY_CONFLICT|  
|**Lingua**|Inglese|  
|**Messaggio**|I criteri QoS utente "% 2" potenzialmente sono in conflitto con altri criteri QoS. Vedere la documentazione per le regole relative ai criteri che verranno applicati.|  
  
|||  
|-|-|  
|**MessageId**|16610|  
|**Gravità**|Avviso|  
|**SymbolicName**|EVENT_EQOS_WARNING_MACHINE_POLICY_NO_FULLPATH_APPNAME|  
|**Lingua**|Inglese|  
|**Messaggio**|Il criterio QoS del computer "% 2" è stato ignorato perché non è possibile elaborare il percorso dell'applicazione. Il percorso dell'applicazione potrebbe non essere valido, contenere una lettera di unità non valida o contenere un'unità mappata alla rete.|  
  
|||  
|-|-|  
|**MessageId**|16611|  
|**Gravità**|Avviso|  
|**SymbolicName**|EVENT_EQOS_WARNING_USER_POLICY_NO_FULLPATH_APPNAME|  
|**Lingua**|Inglese|  
|**Messaggio**|Il criterio QoS utente "% 2" è stato ignorato perché non è possibile elaborare il percorso dell'applicazione. Il percorso dell'applicazione potrebbe non essere valido, contenere una lettera di unità non valida o contenere un'unità mappata alla rete.|  
  
## <a name="error-messages"></a>Messaggi di errore  

Di seguito è riportato un elenco di messaggi di errore dei criteri QoS.

|||  
|-|-|  
|**MessageId**|16700|  
|**Gravità**|Errore|  
|**SymbolicName**|EVENT_EQOS_ERROR_MACHINE_POLICY_REFERESH|  
|**Lingua**|Inglese|  
|**Messaggio**|Non è stato possibile aggiornare i criteri QoS del computer. Codice di errore: "% 2".|  
  
|||  
|-|-|  
|**MessageId**|16701|  
|**Gravità**|Errore|  
|**SymbolicName**|EVENT_EQOS_ERROR_USER_POLICY_REFERESH|  
|**Lingua**|Inglese|  
|**Messaggio**|Non è stato possibile aggiornare i criteri QoS utente. Codice di errore: "% 2".|  
  
|||  
|-|-|  
|**MessageId**|16702|  
|**Gravità**|Errore|  
|**SymbolicName**|EVENT_EQOS_ERROR_OPENING_MACHINE_POLICY_ROOT_KEY|  
|**Lingua**|Inglese|  
|**Messaggio**|QoS non è riuscito ad aprire la chiave radice a livello di computer per i criteri QoS. Codice di errore: "% 2".|  
  
|||  
|-|-|  
|**MessageId**|16703|  
|**Gravità**|Errore|  
|**SymbolicName**|EVENT_EQOS_ERROR_OPENING_USER_POLICY_ROOT_KEY|  
|**Lingua**|Inglese|  
|**Messaggio**|QoS non è riuscito ad aprire la chiave radice a livello di utente per i criteri QoS. Codice di errore: "% 2".|  
  
|||  
|-|-|  
|**MessageId**|16704|  
|**Gravità**|Errore|  
|**SymbolicName**|EVENT_EQOS_ERROR_MACHINE_POLICY_KEYNAME_TOO_LONG|  
|**Lingua**|Inglese|  
|**Messaggio**|Un criterio QoS del computer supera la lunghezza massima consentita per il nome. Il criterio offensivo è elencato sotto la chiave radice dei criteri QoS a livello di computer, con indice "% 2".|  
  
|||  
|-|-|  
|**MessageId**|16705|  
|**Gravità**|Errore|  
|**SymbolicName**|EVENT_EQOS_ERROR_USER_POLICY_KEYNAME_TOO_LONG|  
|**Lingua**|Inglese|  
|**Messaggio**|Un criterio QoS utente supera la lunghezza massima consentita per il nome. Il criterio offensivo è elencato sotto la chiave radice dei criteri QoS a livello di utente, con indice "% 2".|  
  
|||  
|-|-|  
|**MessageId**|16706|  
|**Gravità**|Errore|  
|**SymbolicName**|EVENT_EQOS_ERROR_MACHINE_POLICY_KEYNAME_SIZE_ZERO|  
|**Lingua**|Inglese|  
|**Messaggio**|Un criterio QoS del computer ha un nome di lunghezza zero. Il criterio offensivo è elencato sotto la chiave radice dei criteri QoS a livello di computer, con indice "% 2".|  
  
|||  
|-|-|  
|**MessageId**|16707|  
|**Gravità**|Errore|  
|**SymbolicName**|EVENT_EQOS_ERROR_USER_POLICY_KEYNAME_SIZE_ZERO|  
|**Lingua**|Inglese|  
|**Messaggio**|Un criterio QoS utente ha un nome di lunghezza zero. Il criterio offensivo è elencato sotto la chiave radice dei criteri QoS a livello di utente, con indice "% 2".|  
  
|||  
|-|-|  
|**MessageId**|16708|  
|**Gravità**|Errore|  
|**SymbolicName**|EVENT_EQOS_ERROR_OPENING_MACHINE_POLICY_SUBKEY|  
|**Lingua**|Inglese|  
|**Messaggio**|QoS non è riuscito ad aprire la sottochiave del registro di sistema per un criterio QoS del computer. Il criterio è elencato sotto la chiave radice dei criteri QoS a livello di computer, con indice "% 2".|  
  
|||  
|-|-|  
|**MessageId**|16709|  
|**Gravità**|Errore|  
|**SymbolicName**|EVENT_EQOS_ERROR_OPENING_USER_POLICY_SUBKEY|  
|**Lingua**|Inglese|  
|**Messaggio**|QoS non è riuscito ad aprire la sottochiave del registro di sistema per un criterio QoS utente. Il criterio è elencato sotto la chiave radice dei criteri QoS a livello di utente, con indice "% 2".|  
  
|||  
|-|-|  
|**MessageId**|16710|  
|**Gravità**|Errore|  
|**SymbolicName**|EVENT_EQOS_ERROR_PROCESSING_MACHINE_POLICY_FIELD|  
|**Lingua**|Inglese|  
|**Messaggio**|QoS non è riuscito a leggere o convalidare il campo "% 2" per i criteri QoS del computer "% 3".|  
  
|||  
|-|-|  
|**MessageId**|16711|  
|**Gravità**|Errore|  
|**SymbolicName**|EVENT_EQOS_ERROR_PROCESSING_USER_POLICY_FIELD|  
|**Lingua**|Inglese|  
|**Messaggio**|QoS non è riuscito a leggere o convalidare il campo "% 2" per i criteri QoS utente "% 3".|  
  
|||  
|-|-|  
|**MessageId**|16712|  
|**Gravità**|Errore|  
|**SymbolicName**|EVENT_EQOS_ERROR_SETTING_TCP_AUTOTUNING|  
|**Lingua**|Inglese|  
|**Messaggio**|QoS non è riuscito a leggere o impostare il livello di velocità effettiva TCP in ingresso. codice errore: "% 2".|  
  
|||  
|-|-|  
|**MessageId**|16713|  
|**Gravità**|Errore|  
|**SymbolicName**|EVENT_EQOS_ERROR_SETTING_APP_MARKING|  
|**Lingua**|Inglese|  
|**Messaggio**|QoS non è riuscito a leggere o impostare l'impostazione di sostituzione del contrassegno DSCP. codice errore: "% 2".|  

Per l'argomento successivo di questa guida, vedere [domande frequenti sui criteri QoS](qos-policy-faq.md).

Per il primo argomento di questa guida, vedere [criteri di qualità del servizio (QoS)](qos-policy-top.md).
