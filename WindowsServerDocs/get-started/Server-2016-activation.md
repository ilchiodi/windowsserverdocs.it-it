---
title: Attivazione di Microsoft Server
description: Come attivare Windows Server 2016.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.date: 09/19/2018
ms.technology: server-general
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 99f7daa4-30ce-4d13-be65-0a45d5cc7a54
author: jaimeo
ms.author: jaimeo
manager: dougkim
ms.localizationpriority: medium
ms.openlocfilehash: 88cab1965e299c7d25c177125cb504432bf987e8
ms.sourcegitcommit: 6ef4986391607bb28593852d06cc6645e548a4b3
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 06/07/2019
ms.locfileid: "66810644"
---
# <a name="windows-server-2016-activation"></a>Attivazione di Windows Server 2016

Le informazioni seguenti delineano alcune considerazioni iniziali relative alla pianificazione che devi esaminare per l'attivazione mediante il Servizio di gestione delle chiavi riguardante Windows Server 2016. Per informazioni sull'attivazione del KMS che includono sistemi operativi precedenti a quelli elencati di seguito, vedere [passaggio 1: Esaminare e selezionare i metodi di attivazione](https://technet.microsoft.com/library/jj134256(WS.11).aspx).

Per attivare i client, il servizio di gestione delle chiavi utilizza un modello client-server. I client del servizio di gestione delle chiavi si connettono per l'attivazione a un server del servizio di gestione delle chiavi, denominato host del servizio di gestione delle chiavi. L'host del servizio di gestione delle chiavi deve risiedere nella rete locale.

Gli host del servizio di gestione delle chiavi non devono necessariamente essere server dedicati, e il servizio di gestione delle chiavi può essere ospitato insieme ad altri servizi. Puoi eseguire un host del Servizio di gestione delle chiavi in qualsiasi sistema fisico o virtuale che esegue Windows 10, Windows Server 2016, Windows Server 2012 R2, Windows 8.1 o Windows Server 2012.

Un host del Servizio di gestione delle chiavi eseguito in Windows 10 o Windows 8.1 può attivare solo computer che eseguono sistemi operativi client.
La tabella seguente riepiloga i requisiti client e host del Servizio di gestione delle chiavi per le reti che includono client Windows Server 2016 e Windows 10.

> [!NOTE]
> **Nota:**  Per supportare l'attivazione di uno di questi client più recenti, potrebbe essere necessario aggiornare il server del Servizio di gestione delle chiavi. Se vengono visualizzati errori di attivazione, verificare che siano stati installati gli aggiornamenti appropriati elencati dopo questa tabella.

|Gruppo del codice Product Key|Il Servizio di gestione delle chiavi può essere ospitato in|Versioni di Windows attivate da questo host del Servizio di gestione delle chiavi|  
|-----------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-------------------------------------------------|  
|Contratti multilicenza per Windows Server 2016|Windows Server 2012<br /><br />Windows Server 2012 R2<br /><br />Windows Server 2016<br /><br />|Canale semestrale Windows Server <br><br>Windows Server 2016 (tutte le edizioni)<br /><br />Windows 10 LTSB (2015 e 2016)<br /><br />Windows 10 Professional<br /><br />Windows 10 Enterprise<br /><br />Windows 10 Pro for Workstations<br><br>Windows 10 Education<br><br>Windows Server 2012 R2 (tutte le edizioni)<br /><br />Windows 8.1 Professional<br /><br />Windows 8.1 Enterprise<br /><br />Windows Server 2012 (tutte le edizioni)<br /><br />Windows Server 2008 R2 (tutte le edizioni)<br /><br />Windows Server 2008 (tutte le edizioni)<br /><br />Windows 7 Professional<br /><br />Windows 7 Enterprise| 
|Contratti multilicenza per Windows 10|Windows 7<br /><br />Windows 8.1<br /><br /> Windows 10|Windows 10 Professional<br /><br /> Windows 10 Professional N<br /><br /> Windows 10 Enterprise<br /><br /> Windows 10 Enterprise N<br /><br /> Windows 10 Education<br /><br /> Windows 10 Education N<br /><br /> Windows 10 Enterprise LTSB (2015)<br /><br /> Windows 10 Enterprise LTSB N (2015)<br /><br /> Windows 10 Pro for Workstations<br><br>Windows 8.1 Professional<br /><br /> Windows 8.1 Enterprise<br /><br /> Windows 7 Professional<br /><br /> Windows 7 Enterprise<br /><br />|  
|Contratto multilicenza per "Windows Server 2012 R2 per Windows 10"|Windows Server 2008 R2<br /><br /> Windows Server 2012 Standard<br /><br /> Windows Server 2012 Datacenter<br /><br /> Windows Server 2012 R2 Standard<br /><br />Windows Server 2012 R2 Datacenter|Windows 10 Professional<br /><br /> Windows 10 Enterprise<br /><br />Windows 10 Enterprise LTSB (2015)<br><br>Windows 10 Pro for Workstations<br><br>Windows 10 Education<br><br> Windows Server 2012 R2 (tutte le edizioni)<br /><br /> Windows 8.1 Professional<br /><br /> Windows 8.1 Enterprise<br /><br /> Windows Server 2012 (tutte le edizioni)<br /><br /> Windows Server 2008 R2 (tutte le edizioni)<br /><br />Windows Server 2008 (tutte le edizioni)<br /><br /> Windows 7 Professional<br /><br /> Windows 7 Enterprise|

> [!NOTE]  
> A seconda del sistema operativo eseguito dal server del Server di gestione delle chiavi e dei sistemi operativi che si intende attivare, potrebbe essere necessario installare uno o più degli aggiornamenti seguenti:
> - Le installazioni del Servizio di gestione delle chiavi in Windows 7 o Windows Server 2008 R2 devono essere aggiornate per supportare l'attivazione dei client che eseguono Windows 10. Per altre informazioni, vedere [aggiornamento che consente a Windows 7 e Windows Server 2008 R2 KMS host di attivare Windows 10](https://support.microsoft.com/help/3079821/update-that-enables-windows-7-and-windows-server-2008-r2-kms-hosts-to-activate-windows-10).  
> - Le installazioni del Servizio di gestione delle chiavi in Windows Server 2012 devono essere aggiornate per supportare l'attivazione dei client che eseguono Windows 10 e Windows Server 2016 o sistemi operativi client o server più recenti. Per altre informazioni, vedere [luglio 2016 aggiornamento cumulativo per Windows Server 2012](https://support.microsoft.com/help/3172615/july-2016-update-rollup-for-windows-server-2012). 
> - Le installazioni del Servizio di gestione delle chiavi in Windows 8.1 o Windows Server 2012 R2 devono essere aggiornate per supportare l'attivazione dei client che eseguono Windows 10 e Windows Server 2016 o sistemi operativi client o server più recenti. Per altre informazioni, vedere [luglio 2016 aggiornamento cumulativo per Windows 8.1 e Windows Server 2012 R2](https://support.microsoft.com/help/3172614/july-2016-update-rollup-for-windows-8.1-and-windows-server-2012-r2).  
> - Non è possibile aggiornare Windows Server 2008 R2 per supportare l'attivazione dei client che eseguono Windows Server 2016 o sistemi operativi più recenti. 

Uno stesso host del servizio di gestione delle chiavi è in grado di supportare un numero illimitato di client del servizio di gestione delle chiavi. Se sono presenti più di 50 client ti consigliamo di disporre di almeno due host del Servizio di gestione delle chiavi, nel caso che la disponibilità di uno dei due venga a mancare. L'intera infrastruttura della maggior parte delle organizzazioni è normalmente in grado di funzionare con appena due host del servizio di gestione delle chiavi.

# <a name="addressing-kms-operational-requirements"></a>Requisiti operativi del servizio di gestione delle chiavi
Il servizio di gestione delle chiavi può attivare computer fisici e virtuali, ma per qualificarsi per questo servizio la rete deve includere un numero minimo di computer (la cosiddetta soglia di attivazione). I client del servizio di gestione delle chiavi si attivano solo dopo che questa soglia viene raggiunta. Per assicurarsi che la soglia di attivazione sia raggiunta, un host del servizio di gestione delle chiavi conta i computer che richiedono l'attivazione nella rete.

Gli host del Servizio di gestione delle chiavi contano le connessioni più recenti. Quando un client o un server contatta l'host del Servizio di gestione delle chiavi, l'host aggiunge l'ID macchina al relativo conteggio e quindi restituisce il valore corrente del conteggio nella risposta. Il client o il server verrà attivato se il conteggio è sufficientemente elevato. I client verranno attivati se il conteggio è pari a 25 o superiore. Le edizioni server e con contratti multilicenza dei prodotti Microsoft Office verranno attivata se il conteggio è pari a cinque o superiore. Il Servizio di gestione delle chiavi conta solo connessioni uniche risalenti agli ultimi 30 giorni e archivia solo i 50 contatti più recenti.

Le attivazioni con il servizio di gestione delle chiavi sono valide per 180 giorni, un periodo noto come intervallo di validità dell'attivazione. Per rimanere attivati, i client del servizio di gestione delle chiavi devono rinnovare la propria attivazione connettendosi all'host del servizio di gestione delle chiavi almeno ogni 180 giorni. Per impostazione predefinita i computer client del servizio di gestione delle chiavi tentano di rinnovare l'attivazione ogni sette giorni. Quando l'attivazione di un client viene rinnovata, l'intervallo di validità dell'attivazione ricomincia.

# <a name="addressing-kms-functional-requirements"></a>Requisiti funzionali del servizio di gestione delle chiavi

L'attivazione mediante il servizio di gestione delle chiavi richiede la connettività TCP/IP. Per impostazione predefinita, gli host e i client del servizio di gestione delle chiavi sono configurati per utilizzare DNS (Domain Name System). Per impostazione predefinita, gli host del Servizio di gestione delle chiavi utilizzano l'aggiornamento dinamico DNS per pubblicare automaticamente le informazioni di cui i client del Servizio di gestione delle chiavi hanno bisogno per trovarli e connettersi a essi. È possibile accettare queste impostazioni predefinite oppure, in presenza di particolari esigenze di configurazione della sicurezza e di rete, configurare manualmente gli host e i client del servizio di gestione delle chiavi.

Dopo l'attivazione del primo host del servizio di gestione delle chiavi, con il codice Product Key del servizio di gestione delle chiavi utilizzato per questo host è possibile attivare fino a cinque altri host del servizio di gestione delle chiavi nella rete. Dopo l'attivazione di un host del servizio di gestione delle chiavi, gli amministratori possono riattivare lo stesso host fino a nove volte con lo stesso codice.

Se l'organizzazione ha bisogno di più di sei host del servizio di gestione delle chiavi è necessario richiedere attivazioni aggiuntive per il codice Product Key, ad esempio se dieci posizioni fisiche sono comprese in un unico contratto multilicenza e si desidera che ciascuna abbia un host del servizio di gestione delle chiavi locale.

> [!NOTE] 
> Per richiedere questa eccezione, contattare il call center dell'attivazione. Per altre informazioni, vedere [Contratti multilicenza Microsoft]( https://www.microsoft.com/licensing).

I computer che eseguono versioni con contratti multilicenza di Windows 10, Windows Server 2016, Windows 8.1, Windows Server 2012 R2, Windows Server 2012, Windows 7, Windows Server 2008 R2 sono client del Servizio di gestione delle chiavi per impostazione predefinita, senza necessità di configurazioni aggiuntive.

Se si converte un computer in un client KMS a partire da un host del Servizio di gestione delle chiavi, un codice ad attivazione multipla (MAK) o un'edizione retail di Windows, installare la chiave di configurazione client KMS pertinente. Per altre informazioni, vedere [KMS Client Setup Keys](KMSclientkeys.md). 
