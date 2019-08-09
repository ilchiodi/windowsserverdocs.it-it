---
title: Ottimizzazione delle prestazioni per server Active Directory
description: Ottimizzazione delle prestazioni per server Active Directory
ms.prod: windows-server-threshold
ms.technology: performance-tuning-guide
ms.topic: landing-page
ms.author: TimWi; ChrisRob; HerbertM; KenBrumf;  MLeary; ShawnRab; v-tea
author: phstee
ms.date: 10/16/2017
ms.openlocfilehash: b8ab1eed003294e3396bcea21b31b7c084180b9c
ms.sourcegitcommit: 02f1e11ba37a83e12d8ffa3372e3b64b20d90d00
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 08/09/2019
ms.locfileid: "68863450"
---
# <a name="performance-tuning-active-directory-servers"></a>Ottimizzazione delle prestazioni per server Active Directory

L'ottimizzazione delle prestazioni di Active Directory è incentrata su due obiettivi:
- Active Directory è configurato in modo ottimale per gestire il carico nel modo più efficiente possibile
- I carichi di lavoro inviati ad Active Directory devono essere quanto più efficienti possibile

È pertanto necessario prestare la dovuta attenzione a tre aree distinte:
- Corretta pianificazione della capacità: assicurarsi che sia disponibile hardware sufficiente per supportare il carico esistente
- Ottimizzazione lato server: configurare i controller di dominio per gestire il carico nel modo più efficiente possibile
- Ottimizzazione client/applicazioni Active Directory: assicurarsi che i client e le applicazioni usino Active Directory in modo ottimale

## <a name="start-with-capacity-planning"></a>Iniziare con la pianificazione della capacità

La distribuzione corretta di un numero sufficiente di controller di dominio nel dominio appropriato e con le impostazioni locali corrette, nonché l'implementazione della ridondanza sono fattori essenziali per garantire che le richieste client vengano gestite in modo tempestivo. Questo è un argomento specifico che non rientra nell'ambito di questa guida. Come passaggio iniziale per l'ottimizzazione delle prestazioni di Active Directory, i lettori sono invitati ad acquisire familiarità con i consigli e le indicazioni disponibili in [Pianificazione della capacità per Active Directory Domain Services](capacity-planning-for-active-directory-domain-services.md).

>[!Important]
> Una configurazione e un ridimensionamento appropriati di Active Directory producono un potenziale impatto significativo sulle prestazioni globali del sistema e del carico di lavoro. I lettori sono invitati a iniziare con la lettura di [Pianificazione della capacità per Active Directory Domain Services](capacity-planning-for-active-directory-domain-services.md).

## <a name="updates-and-evolving-recommendations"></a>Aggiornamenti e consigli in continua evoluzione

Nelle ultime generazioni del sistema operativo sono stati apportati notevoli miglioramenti per le ottimizzazioni delle prestazioni client e di Active Directory e l'impegno continua a essere costante. È consigliabile distribuire le versioni più recenti della piattaforma per ottenere i vantaggi, tra cui:

- Una maggiore affidabilità
- Prestazioni migliori
- Migliori funzionalità di registrazione e strumenti per la risoluzione dei problemi

Poiché tuttavia queste operazioni richiedono tempo e molti ambienti sono in esecuzione in uno scenario in cui è impossibile adottare al 100% la piattaforma più recente, alcuni miglioramenti sono stati aggiunti alle versioni meno recenti della piattaforma e ne verranno aggiunti altri.

È consigliabile mantenersi aggiornati sulle novità, le indicazioni e le procedure consigliate più recenti per la gestione di Active Directory Domain Services seguendo il blog del team ["Ask the Directory Services Team"](https://techcommunity.microsoft.com/t5/Ask-the-Directory-Services-Team/bg-p/AskDS).

## <a name="see-also"></a>Vedi anche

- [Pianificazione della capacità per Active Directory Domain Services](capacity-planning-for-active-directory-domain-services.md)
- [Considerazioni relative ai requisiti hardware](hardware-considerations.md)
- [Considerazioni sull'utilizzo della memoria](memory-usage-considerations.md)
- [Considerazioni relative a LDAP](ldap-considerations.md)
- [Esatto posizionamento dei controller di dominio e considerazioni sul sito](site-definition-considerations.md)
- [Risoluzione dei problemi delle prestazioni di Active Directory Domain Services](troubleshoot.md)  
  
