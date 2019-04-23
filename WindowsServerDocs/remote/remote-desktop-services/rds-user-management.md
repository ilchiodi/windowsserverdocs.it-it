---
title: Gestire gli utenti nella raccolta di Servizi Desktop remoto
description: Informazioni su come gestire gli utenti in Servizi Desktop remoto.
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
ms.openlocfilehash: 4b45061697926a3003712a88610cb17ef3c00c45
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59859642"
---
# <a name="manage-users-in-your-rds-collection"></a>Gestire gli utenti nella raccolta di Servizi Desktop remoto

>Si applica a: Windows Server (canale semestrale), Windows Server 2016

Come amministratore, è possibile gestire direttamente gli utenti che hanno accesso a raccolte specifiche. In questo modo, è possibile creare una raccolta con le applicazioni standard per gli information worker, ma creare quindi una raccolta separata con le applicazioni a elevato utilizzo di grafica modellazione delle minacce per i tecnici. Esistono due passaggi principali per la gestione dell'accesso utente in una distribuzione di Servizi Desktop remoto (RDS):

1.  [Creare utenti e gruppi in Active Directory](#create-your-users-and-groups-in-active-directory)
2.  [Assegnare utenti e gruppi per le raccolte](#assign-users-and-groups-to-collections)


## <a name="create-your-users-and-groups-in-active-directory"></a>Creare utenti e gruppi in Active Directory

In una distribuzione di servizi desktop remoto, servizi di dominio Active Directory (AD DS) è l'origine di tutti gli utenti, gruppi e altri oggetti nel dominio. È possibile gestire Active Directory direttamente con PowerShell, oppure è possibile usare compilato negli strumenti dell'interfaccia utente che aggiungono maggiore facilità e flessibilità. La seguente procedura per installare questi strumenti, ovvero se non si è già installati e quindi usare tali strumenti per gestire utenti e gruppi.

### <a name="install-ad-ds-tools"></a>Installare gli strumenti di Active Directory Domain Services

La procedura seguente illustra come installare gli strumenti di Active Directory Domain Services in un server che esegue già Active Directory Domain Services. Una volta installato, è possibile creare utenti o creare gruppi.

1. Connettersi al server che esegue servizi di dominio Active Directory. Per le distribuzioni di Azure:
   1. Nel portale di Azure, fare clic su **Sfoglia > gruppi di risorse**e quindi fare clic sul gruppo di risorse per la distribuzione
   2. Selezionare la macchina virtuale di Active Directory.
   3. Fare clic su **Connetti > aprire** per aprire il client Desktop remoto. Se **Connect** è visualizzata in grigio, la macchina virtuale potrebbe non essere un indirizzo IP pubblico. Per assegnare uno esegue i passaggi seguenti, quindi riprovare questa operazione.
      1. Fare clic su **Impostazioni > interfacce di rete**, quindi scegliere l'interfaccia di rete corrispondente.
      2. Fare clic su **Impostazioni > indirizzo IP**.
      3. Per **indirizzo IP pubblico**, selezionare **Enabled**, quindi fare clic su **indirizzo IP**.
      4. Se si dispone di un indirizzo IP pubblico esistente da utilizzare, selezionarlo dall'elenco. In caso contrario, fare clic su **Crea nuovo**, immettere un nome e quindi fare clic su **OK** e **Salva**.
   4. Nel client, fare clic su **Connect**, quindi fare clic su **Usa un altro account**. Immettere il nome utente e la password per un account di amministratore di dominio.
   5. Fare clic su **Sì** quando viene chiesto il certificato.
2. Installare gli strumenti di Active Directory Domain Services:
   1. In Server Manager fare clic su **Gestisci > Aggiungi ruoli e funzionalità**.
   2. Fare clic su **installazione basata su ruoli o basata su funzionalità**, quindi fare clic su server AD corrente. Seguire la procedura finché non viene visualizzata la **funzionalità** scheda.
   3. Espandere **strumenti di amministrazione remota del Server > Strumenti di amministrazione ruoli > Active Directory Domain Services e gli strumenti di AD LDS**, quindi selezionare **strumenti di AD DS**.
   4. Selezionare **riavvia automaticamente il server di destinazione se necessario**, quindi fare clic su **installare**.

### <a name="create-a-group"></a>Creare un gruppo

È possibile usare i gruppi di Active Directory Domain Services per concedere l'accesso a un set di utenti che devono usare le stesse risorse remote.

1. In Server Manager sul server di Active Directory Domain Services, scegliere **strumenti > Active Directory Users and Computers**.
2. Espandere il dominio nel riquadro a sinistra per visualizzare le relative sottocartelle.
3. Fare clic sulla cartella in cui si desidera creare il gruppo e quindi fare clic su **nuovo > gruppo**.
4. Immettere un nome di gruppo appropriato e quindi selezionare **Global** e **sicurezza**.

### <a name="create-a-user-and-add-to-a-group"></a>Creare un utente e aggiungere a un gruppo
1. In Server Manager sul server di Active Directory Domain Services, scegliere **strumenti > Active Directory Users and Computers**.
2. Espandere il dominio nel riquadro a sinistra per visualizzare le relative sottocartelle.
3. Fare doppio clic su **gli utenti**, quindi fare clic su **New > utente**.
4. Immettere, minimo, un nome e un nome di accesso utente.
5. Immettere e confermare una password per l'utente. Impostare le opzioni utente appropriato, ad esempio **cambiamento obbligatorio password all'accesso successivo**.
6. Aggiungere il nuovo utente a un gruppo:
   1. Nel **utenti** cartella pulsante destro del mouse del nuovo utente.
   2. Fare clic su **aggiunta a un gruppo**.
   3. Immettere il nome del gruppo a cui si desidera aggiungere l'utente.

## <a name="assign-users-and-groups-to-collections"></a>Assegnare utenti e gruppi per le raccolte
Ora che è stato creato gli utenti e gruppi in Active Directory, è possibile aggiungere alcuni granularità su chi ha accesso alle raccolte di Desktop remoto nella distribuzione.

1. Connettersi al server che esegue il ruolo Gestore connessione Desktop remoto (Gestore connessione desktop remoto), seguendo i passaggi descritti in precedenza.
2. Aggiungere gli altri server di Desktop remoto al pool di Gestore connessione desktop remoto di server gestiti:
   1. In Server Manager fare clic su **Gestisci > Aggiungi server**.
   2. Fare clic su **trova**.
   3. Fare clic su ogni server nella distribuzione che esegue un ruolo di Servizi Desktop remoto e quindi fare clic su **OK**.
3. Modificare una raccolta per assegnare l'accesso a utenti o gruppi specifici:
   1. In Server Manager fare clic su **Servizi Desktop remoto > Panoramica**, quindi fare clic su una raccolta specifica.
   2. Sotto **delle proprietà**, fare clic su **attività > modificare le proprietà**.
   3. Fare clic su **gruppi di utenti**.
   4. Fare clic su **Add** e immettere l'utente o gruppo che si vuole accedere alla raccolta. È anche possibile rimuovere gli utenti e gruppi da questa finestra selezionando l'utente o gruppo che si desidera rimuovere e quindi scegliendo **rimuovere**. 
   
   >[!NOTE] 
   > La finestra di gruppi di utenti non può mai essere vuota. Per restringere l'ambito degli utenti che hanno accesso alla raccolta, è necessario prima di tutto aggiungere utenti o gruppi specifici prima di rimuovere i gruppi più ampi.