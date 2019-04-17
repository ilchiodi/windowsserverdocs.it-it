---
title: Attivazione di Windows Server 2019
description: Come attivare Windows Server 2019
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: server-general
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 99f7daa4-30ce-4d13-be65-0a4d55cca754
author: coreyp-at-msft
ms.author: coreyp
manager: jasgroce
ms.localizationpriority: medium
ms.openlocfilehash: 0e7a61fe34965ecf47505e6825a5c03680b1b40d
ms.sourcegitcommit: d622f7af181ed0063d716b30278d41887a57db19
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/08/2019
ms.locfileid: "9150871"
---
# Attivazione di Windows Server 2019

>Si applica a: Windows Server 2019, Windows Server 2016

Le informazioni seguenti delineano iniziale considerazioni sulla pianificazione che devi esaminare per l'attivazione di servizi di gestione delle chiavi (KMS) riguardante Windows Server 2019. Per informazioni sull'attivazione mediante il Servizio di gestione delle chiavi riguardante sistemi operativi precedenti a quelli elencati di seguito, vedi il [Passaggio 1: Esaminare e selezionare i metodi di attivazione](https://technet.microsoft.com/library/jj134256(WS.11).aspx).

Per attivare i client, il Servizio di gestione delle chiavi utilizza un modello client-server. I client del Servizio di gestione delle chiavi si connettono per l'attivazione a un server del Servizio di gestione delle chiavi, denominato host del Servizio di gestione delle chiavi. L'host del Servizio di gestione delle chiavi deve risiedere nella rete locale.

Gli host del Servizio di gestione delle chiavi non devono necessariamente essere server dedicati, e il Servizio di gestione delle chiavi può essere ospitato insieme ad altri servizi. È possibile eseguire un host KMS in qualsiasi sistema fisico o virtuale che esegue Windows 10, Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows 8.1 o Windows Server 2012.

Un host del Servizio di gestione delle chiavi eseguito in Windows 10 o Windows 8.1 può attivare solo computer che eseguono sistemi operativi client.
La tabella seguente riepiloga i requisiti client e host KMS per le reti che includono client Windows Server 2016, Windows Server 2019 e Windows 10.

>[!NOTE]
>- Per supportare l'attivazione di uno di questi client più recenti potrebbero essere necessari aggiornamenti sul server KMS. Se vengono visualizzati errori di attivazione, verifica che siano stati installati gli aggiornamenti appropriati elencati dopo questa tabella.
>- Se stai lavorando con le macchine virtuali, vedi [l'Attivazione automatica della macchina virtuale](vm-activation-19.md) per le chiavi AVMA e informazioni.

|Gruppo del codice Product Key|KMS può essere ospitate in|Versioni di Windows attivate da questo host del Servizio di gestione delle chiavi|  
|-----------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-------------------------------------------------|  
|Contratti multilicenza per Windows Server 2019|Windows Server 2012 R2<br /><br />WindowsServer 2016<br /><br />Windows Server 2019<br /><br />|Canale semestrale Windows Server<br /><br />Windows Server 2019 (tutte le edizioni)<br /><br />Windows Server 2016 (tutte le edizioni)<br /><br />Windows 10 Enterprise LTSC 2019 <br /><br />Windows 10 Enterprise LTSC N 2019<br /><br />Windows 10 LTSB (2015 e 2016)<br /><br />Windows 10 Professional<br /><br />Windows 10 Enterprise<br /><br />Windows 10 Pro for Workstations<br /><br />Windows 10 Education<br /><br />Windows Server 2012 R2 (tutte le edizioni)<br /><br />Windows 8.1 Professional<br /><br />Windows8.1 Enterprise<br /><br />Windows Server 2012 (tutte le edizioni)<br /><br />Windows Server 2008 R2 (tutte le edizioni)<br /><br />Windows Server 2008 (tutte le edizioni)<br /><br />Windows7 Professional<br /><br />Windows7 Enterprise<br />| 
|Contratti multilicenza per Windows Server 2016|Windows Server 2012<br /><br />Windows Server 2012 R2<br /><br />WindowsServer 2016<br /><br />|Canale semestrale Windows Server <br><br>Windows Server 2016 (tutte le edizioni)<br /><br />Windows 10 LTSB (2015 e 2016)<br /><br />Windows 10 Professional<br /><br />Windows 10 Enterprise<br /><br />Windows 10 Pro for Workstations<br><br>Windows 10 Education<br><br>Windows Server 2012 R2 (tutte le edizioni)<br /><br />Windows 8.1 Professional<br /><br />Windows8.1 Enterprise<br /><br />Windows Server 2012 (tutte le edizioni)<br /><br />Windows Server 2008 R2 (tutte le edizioni)<br /><br />Windows Server 2008 (tutte le edizioni)<br /><br />Windows7 Professional<br /><br />Windows7 Enterprise<br /><br />| 
|Contratti multilicenza per Windows 10|Windows7<br /><br /> Windows 8.1<br /><br /> Windows10|Windows 10 Professional<br /><br /> Windows 10 Professional N<br /><br /> Windows10 Enterprise<br /><br /> Windows 10 Enterprise N<br /><br /> Windows10 Education<br /><br /> Windows 10 Education N<br /><br /> Windows 10 Enterprise LTSB (2015)<br /><br /> Windows 10 Enterprise LTSB N (2015)<br /><br /> Windows 10 Pro for Workstations<br><br>Windows 8.1 Professional<br /><br /> Windows8.1 Enterprise<br /><br /> Windows7 Professional<br /><br /> Windows7 Enterprise<br /><br />|  
|Contratto multilicenza per "Windows Server 2012 R2 per Windows 10"|Windows Server2008 R2<br /><br /> Windows Server2012 Standard<br /><br /> WindowsServer 2012 Datacenter<br /><br /> WindowsServer 2012 R2 Standard<br /><br />WindowsServer 2012 R2 Datacenter|Windows 10 Professional<br /><br /> Windows 10 Enterprise<br /><br />Windows 10 Enterprise LTSB (2015)<br><br>Windows 10 Pro for Workstations<br><br>Windows 10 Education<br><br> Windows Server 2012 R2 (tutte le edizioni)<br /><br /> Windows 8.1 Professional<br /><br /> Windows8.1 Enterprise<br /><br /> Windows Server 2012 (tutte le edizioni)<br /><br /> Windows Server 2008 R2 (tutte le edizioni)<br /><br /> Windows Server 2008 (tutte le edizioni)<br /><br />Windows7 Professional<br /><br /> Windows7 Enterprise|

> [!NOTE]  
>A seconda del sistema operativo eseguito dal server del Servizio di gestione delle chiavi e dei sistemi operativi da attivare, potrebbe essere necessario installare uno o più degli aggiornamenti seguenti:
>- Le installazioni del Servizio di gestione delle chiavi in Windows 7 o Windows Server 2008 R2 devono essere aggiornate per supportare l'attivazione dei client che eseguono Windows 10. Per altre informazioni, vedi[aggiornamento che consente agli host di Windows Server 2008 R2 KMS di attivare Windows 10 e di Windows 7](https://support.microsoft.com/help/3079821/update-that-enables-windows-7-and-windows-server-2008-r2-kms-hosts-to-activate-windows-10).  
>- Le installazioni di CHIAVI in Windows Server 2012 devono essere aggiornate per supportare l'attivazione dei client che eseguono Windows 10 e Windows Server 2016 o Windows Server 2019, o client più recenti o sistemi operativi. Per altre informazioni, vedi[luglio 2016 aggiornamento cumulativo per Windows Server 2012](https://support.microsoft.com/help/3172615/july-2016-update-rollup-for-windows-server-2012). 
>- Le installazioni di CHIAVI in Windows 8.1 o Windows Server 2012 R2 devono essere aggiornate per supportare l'attivazione dei client che eseguono Windows 10 e Windows Server 2016 o Windows Server 2019, o client più recenti o sistemi operativi. Per altre informazioni, vedi[luglio 2016 aggiornamento cumulativo per Windows 8.1 e Windows Server 2012 R2](https://support.microsoft.com/help/3172614/july-2016-update-rollup-for-windows-8.1-and-windows-server-2012-r2).  
>- Windows Server 2008 R2 non può essere aggiornato per supportare l'attivazione dei client che eseguono Windows Server 2019, Windows Server 2016 o sistemi operativi più recenti. 

Uno stesso host del Servizio di gestione delle chiavi è in grado di supportare un numero illimitato di client del Servizio di gestione delle chiavi. Se sono presenti più di 50 client ti consigliamo di disporre di almeno due host del Servizio di gestione delle chiavi, nel caso che la disponibilità di uno dei due venga a mancare. L'intera infrastruttura della maggior parte delle organizzazioni è normalmente in grado di funzionare con appena due host del Servizio di gestione delle chiavi.

# Requisiti operativi del Servizio di gestione delle chiavi
Il Servizio di gestione delle chiavi può attivare computer fisici e virtuali, ma per qualificarsi per questo servizio la rete deve includere un numero minimo di computer (la cosiddetta soglia di attivazione). I client del Servizio di gestione delle chiavi si attivano solo dopo che questa soglia viene raggiunta. Per assicurarsi che la soglia di attivazione sia raggiunta, un host del Servizio di gestione delle chiavi conta i computer che richiedono l'attivazione nella rete.

Gli host del Servizio di gestione delle chiavi contano le connessioni più recenti. Quando un client o un server contatta l'host del Servizio di gestione delle chiavi, l'host aggiunge l'ID computer al relativo conteggio e quindi restituisce il valore corrente del conteggio nella risposta. Il client o il server verrà attivato se il conteggio è sufficientemente elevato. I client verranno attivati se il conteggio è pari a 25 o superiore. Le edizioni server e con contratti multilicenza dei prodotti Microsoft Office verranno attivate se il conteggio è pari a cinque o superiore. Il Servizio di gestione delle chiavi conta solo connessioni univoche risalenti agli ultimi 30 giorni e archivia solo i 50 contatti più recenti.

Le attivazioni con il Servizio di gestione delle chiavi sono valide per 180 giorni, un periodo noto come intervallo di validità dell'attivazione. Per rimanere attivati, i client del Servizio di gestione delle chiavi devono rinnovare la propria attivazione connettendosi all'host del Servizio di gestione delle chiavi almeno ogni 180 giorni. Per impostazione predefinita i computer client del Servizio di gestione delle chiavi tentano di rinnovare l'attivazione ogni sette giorni. Quando l'attivazione di un client viene rinnovata, l'intervallo di validità dell'attivazione ricomincia.

# Requisiti funzionali del Servizio di gestione delle chiavi

L'attivazione mediante il Servizio di gestione delle chiavi richiede la connettività TCP/IP. Per impostazione predefinita, gli host e i client del Servizio di gestione delle chiavi sono configurati per utilizzare DNS (Domain Name System). Per impostazione predefinita, gli host del Servizio di gestione delle chiavi utilizzano l'aggiornamento dinamico DNS per pubblicare automaticamente le informazioni di cui i client del Servizio di gestione delle chiavi hanno bisogno per trovarli e connettersi a essi. Puoi accettare queste impostazioni predefinite oppure, in presenza di particolari esigenze di configurazione della sicurezza e di rete, configurare manualmente gli host e i client del Servizio di gestione delle chiavi.

Dopo l'attivazione del primo host del Servizio di gestione delle chiavi, con il codice Product Key del Servizio di gestione delle chiavi utilizzato per questo host puoi attivare fino a cinque altri host del Servizio di gestione delle chiavi nella rete. Dopo l'attivazione di un host del Servizio di gestione delle chiavi, gli amministratori possono riattivare lo stesso host fino a nove volte con lo stesso codice.

Se l'organizzazione ha bisogno di più di sei host del Servizio di gestione delle chiavi, devi richiedere attivazioni aggiuntive per il codice Product Key, ad esempio se dieci posizioni fisiche sono comprese in un unico contratto multilicenza e desideri che ciascuna abbia un host del Servizio di gestione delle chiavi locale.

>[!NOTE] 
>Per richiedere questa eccezione, contatta il call center dell'attivazione. Per altre informazioni, vedi [Contratti multilicenza Microsoft](https://go.microsoft.com/fwlink/?LinkID=73076).

I computer che eseguono delle edizioni di Windows 10, Windows Server 2019, Windows Server 2016, Windows 8.1, Windows Server 2012 R2, Windows Server 2012, Windows 7, Windows Server 2008 R2 contratti sono, per impostazione predefinita, i client KMS senza altre configurazioni necessarie.

Se converti un computer in un client del Servizio di gestione delle chiavi a partire da un host del Servizio di gestione delle chiavi, un MAK o un'edizione retail di Windows, installa la chiave di configurazione client del Servizio di gestione delle chiavi pertinente. Per altre informazioni, vedi[Chiavi di configurazione Client KMS](../get-started/KMSclientkeys.md). 
 

 
