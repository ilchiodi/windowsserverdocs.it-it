---
title: Gestire gli utenti nella raccolta di Servizi Desktop remoto
description: Di seguito viene descritto come gestire gli utenti in Servizi Desktop remoto.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 2727e1ab-69b8-46f3-9f6d-2540324fe596
author: christianmontoya
ms.author: chrimo
ms.date: 03/27/2018
manager: scottman
ms.openlocfilehash: ff782bc4d01709f56d19ee3e9a06a95267cf7a12
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 09/10/2019
ms.locfileid: "70870744"
---
# <a name="manage-users-in-your-rds-collection"></a>Gestire gli utenti nella raccolta di Servizi Desktop remoto

>Si applica a: Windows Server (Canale semestrale), Windows Server 2019, Windows Server 2016

Gli amministratori possono gestire direttamente a quali utenti concedere l'accesso a raccolte specifiche. In questo modo, è possibile creare una raccolta con le applicazioni standard per gli information worker, creando però in seguito una raccolta separata per tecnici con applicazioni di modellazione a uso intensivo di grafica. Esistono due passaggi principali per la gestione dell'accesso degli utenti in una distribuzione di Servizi Desktop remoto (RDS):

1.  [Creare utenti e gruppi in Active Directory](#create-your-users-and-groups-in-active-directory)
2.  [Assegnare utenti e gruppi alle raccolte](#assign-users-and-groups-to-collections)


## <a name="create-your-users-and-groups-in-active-directory"></a>Creare utenti e gruppi in Active Directory

In una distribuzione di Servizi Desktop remoto, Active Directory Domain Services (AD DS) è l'origine di tutti gli utenti, gruppi e altri oggetti nel dominio. È possibile gestire Active Directory direttamente con PowerShell o con gli strumenti dell'interfaccia utente integrati che rendono il tutto più semplice e flessibile. La seguente procedura mostra come installare questi strumenti (se non sono già stati installati) e usarli per gestire utenti e gruppi.

### <a name="install-ad-ds-tools"></a>Installare gli strumenti di Active Directory Domain Services

La procedura seguente mostra nei dettagli come installare gli strumenti di Active Directory Domain Services in un server con Active Directory Domain Services già in esecuzione. Dopo l'installazione, è possibile creare utenti o gruppi.

1. Connettersi al server che esegue Active Directory Domain Services. Per le distribuzioni Azure:
   1. Nel portale Azure fare clic su **Sfoglia > Gruppi di risorse** e fare clic sul gruppo di risorse per la distribuzione
   2. Selezionare la macchina virtuale di Active Directory.
   3. Fare clic su **Connetti > aprire** per aprire il client Desktop remoto. Se **Connetti** è visualizzato in grigio, la macchina virtuale potrebbe non disporre di un indirizzo IP pubblico. Per assegnargliene uno, seguire questa procedura e riprovare.
      1. Fare clic su **Impostazioni > interfacce di rete**, quindi scegliere l'interfaccia di rete corrispondente.
      2. Fare clic su **Impostazioni > indirizzo IP**.
      3. Per **indirizzo IP pubblico**, selezionare **Enabled**, quindi fare clic su **indirizzo IP**.
      4. Se si dispone di un indirizzo IP pubblico esistente da utilizzare, selezionarlo dall'elenco. In caso contrario, fare clic su **Crea nuovo**, immettere un nome e fare clic su **OK**, quindi su **Salva**.
   4. Nel client, fare clic su **Connetti**, quindi fare clic su **Utilizza un altro account**. Immettere il nome utente e la password per un account di amministratore di dominio.
   5. Fare clic su **Sì** sull'avviso relativo al certificato.
2. Installare gli strumenti di Active Directory Domain Services:
   1. In Server Manager fare clic su **Gestisci > Aggiungi ruoli e funzionalità**.
   2. Fare clic su **Installazione basata su ruoli o basata su funzionalità** e quindi fare clic sul server AD corrente. Seguire la procedura finché non viene visualizzata la scheda **Funzionalità**.
   3. Espandere **Strumenti di amministrazione remota del server > Strumenti di amministrazione dei ruoli > Strumenti di Active Directory Domain Services e AD LDS** e quindi selezionare **Strumenti di Active Directory Domain Services**.
   4. Selezionare **Riavvia automaticamente il server di destinazione se necessario**, quindi fare clic su **Installa**.

### <a name="create-a-group"></a>Creare un gruppo

È possibile usare i gruppi di Active Directory Domain Services per concedere l'accesso a un gruppo di utenti che devono usare le stesse risorse remote.

1. In Server Manager sul server che esegue Active Directory Domain Services fare clic su **Strumenti > Utenti e computer di Active Directory**.
2. Espandere il dominio nel riquadro a sinistra per visualizzare le relative sottocartelle.
3. Fare clic con il pulsante destro del mouse sulla cartella in cui creare il gruppo e quindi fare clic su **Nuovo > Gruppo**.
4. Immettere un nome del gruppo appropriato e quindi selezionare **Globale** e **Sicurezza**.

### <a name="create-a-user-and-add-to-a-group"></a>Creare un utente e aggiungerlo a un gruppo
1. In Server Manager sul server che esegue Active Directory Domain Services fare clic su **Strumenti > Utenti e computer di Active Directory**.
2. Espandere il dominio nel riquadro a sinistra per visualizzare le relative sottocartelle.
3. Fare clic con il pulsante destro del mouse su **Utenti** e quindi scegliere **Nuovo > Utente**.
4. Immettere almeno un nome e un nome di accesso utente.
5. Immetti e conferma una password per l'utente. Impostare le opzioni utente appropriate, ad esempio **Cambio di password obbligatorio all'accesso successivo**.
6. Aggiungere il nuovo utente a un gruppo:
   1. Nella cartella **Utenti** fare clic con il pulsante destro del mouse sul nuovo utente.
   2. Fare clic su **Aggiungi a un gruppo**.
   3. Immettere il nome del gruppo a cui si vuole aggiungere l'utente.

## <a name="assign-users-and-groups-to-collections"></a>Assegnare utenti e gruppi alle raccolte
Ora che sono stati creati gli utenti e i gruppi in Active Directory, è possibile aggiungere granularità in relazione a chi ha accesso alle raccolte di Desktop remoto nella distribuzione.

1. Connettersi al server che esegue il ruolo Gestore Connessione Desktop remoto seguendo i passaggi descritti in precedenza.
2. Aggiungi gli altri server di Desktop remoto al pool di server gestiti di Gestore connessione Desktop remoto:
   1. In Server Manager fare clic su **Gestisci > Aggiungi server**.
   2. Fai clic su **Trova**.
   3. Fare clic su ogni server nella distribuzione che esegue un ruolo di Servizi Desktop remoto e quindi fare clic su **OK**.
3. Modificare una raccolta per assegnare l'accesso a utenti o gruppi specifici:
   1. In Server Manager fare clic su **Servizi Desktop remoto > Panoramica** e quindi fare clic su una raccolta specifica.
   2. Sotto **Proprietà** fare clic su **Attività > Modifica proprietà**.
   3. Fare clic su **Gruppi utenti**.
   4. Fare clic su **Aggiungi** e immettere l'utente o gruppo al quale si vuole concedere accesso alla raccolta. È anche possibile rimuovere utenti e gruppi da questa finestra selezionando l'utente o gruppo da rimuovere e facendo clic su **Rimuovi**. 
   
   >[!NOTE] 
   > La finestra Gruppi utente non può mai essere vuota. Per restringere l'ambito degli utenti che hanno accesso alla raccolta, è necessario prima di tutto aggiungere utenti o gruppi specifici prima di rimuovere i gruppi più ampi.