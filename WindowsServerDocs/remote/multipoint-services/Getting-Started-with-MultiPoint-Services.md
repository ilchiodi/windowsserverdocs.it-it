---
title: Introduzione a servizi MultiPoint
description: Introduce Servizi MultiPoint e consente di iniziare a usarlo.
ms.prod: windows-server
ms.technology: multipoint-services
ms.topic: article
ms.assetid: aca5f0be-f253-46b5-b1e7-0bffa15f3227
author: lizap
manager: dongill
ms.author: elizapo
ms.date: 08/04/2016
ms.openlocfilehash: 2213c734831c1cde88ad0e2b6fb172f99a31b89c
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80859234"
---
# <a name="getting-started-with-multipoint-services"></a>Introduzione a servizi MultiPoint
Il sistema di servizi MultiPoint consente molti utenti di utilizzare più stazioni fisicamente connessi tramite hub di espansione per un solo computer. Ogni postazione è in genere costituito da una stazione hub, mouse, tastiera e monitor video. Ogni utente in una stazione MultiPoint servizi di cui si verifichi una sessione di calcolo Windows univoca che è possibile gestire tramite Gestione MultiPoint.  
  
I componenti di un sistema di servizi MultiPoint includono quanto segue:  
  
-   Software di sistema ai servizi multiPoint, che supporta più monitor, tastiere, mouse e altri dispositivi del computer.  
  
-   L'applicazione Gestione MultiPoint, che consente di monitorare e intraprendere azioni sulle stazioni servizi MultiPoint.  
  
-   Strumenti di gestione e manutenzione.  
  
-   L'applicazione MultiPoint Dashboard, che consente completare le attività quotidiane, quali la comunicazione con altri utenti.  
  
In questo file della Guida vengono fornite informazioni su come gestire le stazioni MultiPoint servizi con gestione MultiPoint e MultiPoint Dashboard.  
  
## <a name="overview-of-multipoint-manager"></a>Panoramica della gestione MultiPoint  
Gestione multiPoint offre quattro schede da utilizzare quando si gestiscono le stazioni servizi MultiPoint. Ogni scheda e le attività che è possibile eseguire su di essi, descritto più dettagliatamente in ogni argomento della Guida.  
  
Le schede sono i seguenti:  
  
-   **Scheda Home:** passa modalità per eseguire attività amministrative, aggiungere o rimuovere MultiPoint Server, riavviare o arrestare il computer, abilitare la protezione del disco, aggiungere licenze di accesso client, riassociare le stazioni e ottenere Guida in linea o il supporto. Per ulteriori informazioni, vedere il [gestire attività utilizzando MultiPoint gestore di sistema](Manage-System-Tasks-Using-MultiPoint-Manager.md) argomento.  
  
-   **Scheda stazioni:** Visualizzare lo stato del *Desktop* degli utenti e *terminare* o *sospendere* le sessioni utente. Per ulteriori informazioni, vedere il [gestire stazioni](Manage-User-Stations.md) argomento.  
  
-   **Scheda utenti:** creare e gestire *gli account utente standard* e *gli account utente con privilegi amministrativi*. Per altre informazioni, vedere l'argomento [Gestire account utente](Manage-User-Accounts.md).  
  
-   **Scheda desktop virtuale:** abilitare i ruoli di desktop virtuali. Per ulteriori informazioni, vedere il [gestire desktop virtuali](Manage-Virtual-Desktops.md) argomento.  
  
## <a name="multipoint-server-management-and-maintenance"></a>Manutenzione e gestione multiPoint Server  
Dopo aver configurato il sistema di servizi MultiPoint, è possibile utilizzare Gestione MultiPoint per gestire i servizi MultiPoint.  
  
Tipi di azioni che è possibile eseguire utilizzando Gestione MultiPoint includono quanto segue:  
  
-   **Aggiunta di account utente:** utilizzare MultiPoint Manager per creare account utente standard e amministrativi.  
  
-   **Modifica delle impostazioni server:** è possibile configurare il sistema di servizi MultiPoint viene avviato in modalità console, consentire a un account avere più sessioni, assegnare un indirizzo IP univoco per ogni stazione e altre attività.  
  
-   **Passare alla modalità console:** è possibile modificare il sistema MultiPoint Services in modalità console per poter installare il nuovo software sul sistema MultiPoint servizi. È possibile specificare che tutti gli utenti possono eseguire il software o che si può utilizzare solo il software, a seconda dell'installazione e le opzioni del software di licenza.  
  
-   **Risoluzione dei problemi:** Se si riscontrano problemi con i servizi MultiPoint, controllare il [Troubleshooting](Troubleshooting.md) sezione per trovare gli argomenti che consentono di risolvere il problema.  
  
## <a name="overview-of-multipoint-dashboard"></a>Panoramica del Dashboard MultiPoint  
Dashboard multiPoint offre un'esperienza della barra multifunzione in cui è possibile scegliere tra due schede per accedere alle attività giornaliere comuni.  
  
Le schede sono i seguenti:  
  
-   **Scheda Home:** blocco o sbloccare le stazioni, impostare web limitando le opzioni, progetto desktop su altri computer desktop, avviare o chiudere le applicazioni, di comunicare tramite messaggistica immediata, aiutare altri utenti tramite controllo remoto del desktop, regolare le anteprime del desktop e attivare o disattivare Messaggistica immediata e avvio automatico dell'applicazione. Per ulteriori informazioni, vedere il [gestire desktop utilizzando MultiPoint Dashboard utente](Manage-User-Desktops-Using-MultiPoint-Dashboard.md) argomento.  
  
-   **Scheda sistemi:** riavviare, arrestare, o modificare il mapping su tutte o sistemi selezionati. Per ulteriori informazioni, vedere il [gestire MultiPoint sistemi utilizzando MultiPoint Dashboard](Manage-MultiPoint-Systems-Using-MultiPoint-Dashboard.md) argomento.  
  
## <a name="daily-use-of-your-multipoint-server-system"></a>Utilizzo quotidiano del sistema MultiPoint Server  
Come iniziare a utilizzare servizi MultiPoint ogni giorno, è informazioni su come utilizzare servizi MultiPoint potrebbe da condividere con gli utenti nel sistema servizi MultiPoint. Queste informazioni includono quanto segue:  
  
**Condivisione del contenuto e mantenimento del contenuto privato:**  
  
-   Un utente può salvare un file o un documento in una cartella privata che può essere visualizzata soltanto dall'utente.  
  
-   Gli utenti possono anche salvare documenti a una cartella pubblica è accessibile a tutti gli utenti del sistema MultiPoint servizi.  
  
-   È importante per gli utenti di MultiPoint Services tenere presente che gli utenti amministrativi hanno accesso a tutti i file e documenti del sistema, anche se vengono archiviati privatamente nella cartella personale di un utente.  
  
Per ulteriori informazioni su come salvare e gestire contenuti privati e pubblici, vedere il [gestione dei file utente](Manage-User-Files.md) argomento.  
  
**Informazioni sulla sessione MultiPoint Services di un utente:**  
  
-   Ogni utente dispone di un nome utente e password e un desktop univoco *sessione* nel sistema servizi MultiPoint.  
  
-   Oggetto *utente standard* non è un *utente amministratore* nel sistema servizi MultiPoint. Gli utenti standard non possono installare alcuni tipi di software, ma salvare file e modificare le impostazioni del desktop, ad eccezione della risoluzione dello schermo. Le modifiche apportate al desktop saranno presenti al successivo accesso dell'utente.  
  
-   Gli utenti possono disconnettersi da una postazione e accedere alla sessione in una stazione di diversi senza perdita di dati. Per altre informazioni, vedere l'argomento [Sospendere e lasciare attiva una sessione utente](Suspend-and-Leave-User-Session-Active.md).  
  
-   La sessione di un utente standard, o tutte le sessioni utente, può essere disconnessa o disconnessa dall'utente amministratore tramite Gestione MultiPoint. Per ulteriori informazioni, vedere il [desktop degli utenti di gestire](manage-user-desktops-using-multipoint-dashboard.md) argomento.  
  
-   Se un utente dimentica una password, è possibile reimpostare la password di **utenti** scheda, che utilizza la funzionalità Gestione account utente Windows standard. Per ulteriori informazioni, vedere il [aggiornare o eliminare un Account utente](Update-or-Delete-a-User-Account.md) argomento.  
  
## <a name="see-also"></a>Vedi anche  
[Gestione del sistema MultiPoint Server](managing-your-multipoint-services-system.md)  
[Informazioni importanti sulla conformità delle licenze software](Important-Information-about-Software-License-Compliance.md)  
[Gestire le attività di sistema tramite Gestione MultiPoint](Manage-System-Tasks-Using-MultiPoint-Manager.md)  
[Gestire file utente](Manage-User-Files.md)  
[Gestire i desktop degli utenti](manage-user-desktops-using-multipoint-dashboard.md)  
[Sospendere e lasciare attiva la sessione utente](Suspend-and-Leave-User-Session-Active.md)  
[Visualizzare lo stato delle connessioni utente](View-User-Connection-Status.md)  
[Gestire l'hardware delle stazioni](Manage-Station-Hardware.md)  
[Configurare una stazione](Set-Up-a-Station.md)  
[Gestire gli account utente](Manage-User-Accounts.md)  
[Aggiornare o eliminare un account utente](Update-or-Delete-a-User-Account.md)  
[Gestire desktop utente tramite Dashboard MultiPoint](Manage-User-Desktops-Using-MultiPoint-Dashboard.md)  
[Gestire sistemi MultiPoint tramite Dashboard MultiPoint](Manage-MultiPoint-Systems-Using-MultiPoint-Dashboard.md)  
[Risoluzione dei problemi](Troubleshooting.md)    
  
