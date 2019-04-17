---
title: Distribuire l'ambiente Desktop remoto
ms.custom: na
ms.prod: windows-server-threshold
description: Passaggi di base per distribuire un ambiente di Desktop remoto.
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.author: elizapo
ms.date: 04/10/2017
ms.tgt_pltfrm: na
ms.topic: article
author: lizap
manager: dongill
ms.localizationpriority: medium
ms.openlocfilehash: a5a56d56038d94869c5246f8d4d3eae2796616a3
ms.sourcegitcommit: d3f73936160505a40633ad8dd5931ac5fe3eccdb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/12/2019
ms.locfileid: "9297440"
---
# Distribuire l'ambiente Desktop remoto

>Si applica a: Windows Server (canale semestrale), Windows Server 2019, Windows Server 2016

Usare i passaggi seguenti per distribuire il server di Desktop remoto nel tuo ambiente. È possibile installare i ruoli server in computer fisici o macchine virtuali, a seconda se si siano creando un in locale, basata sul cloud o ambiente ibrido. 

Se usi le macchine virtuali per uno qualsiasi dei server di Servizi Desktop remoto, assicurati che hai [preparato le macchine virtuali](rds-prepare-vms.md).
  
  
1.  Aggiungi tutti i server che si desidera utilizzare per Servizi Desktop remoto a Server Manager:  
    1.  In Server Manager fai clic su **Gestisci gt _ Aggiungi server**.  
    2.  Fai clic su **Trova ora**.  
    3.  Fai clic su ogni server di distribuzione (ad esempio, Contoso Cb1, Contoso WebGw1 e Contoso Sh1) e fai clic su **OK**.  
2.  Creare una distribuzione basata su sessione per distribuire i componenti di Servizi Desktop remoto:  
    1.  In Server Manager, fai clic su **Gestisci gt _ Aggiungi ruoli e funzionalità**.  
    2.  Fai clic su **Servizi Desktop remoto installazione**, **Distribuzione Standard**e **distribuzione di desktop basata su sessione**.  
    3.  Seleziona i server appropriati per il server Gestore connessione desktop remoto, server di accesso Web desktop remoto e server Host sessione Desktop remoto (ad esempio, Contoso Cb1, Contoso WebGw1 e Contoso-SH1, rispettivamente).  
    4.  Seleziona **riavviare il server di destinazione automaticamente se richiesto**e quindi fai clic su **Distribuisci**.  
    5.  Attendere la distribuzione di completare correttamente  
3.  Aggiungi Server licenze Desktop remoto:  
    1.  In Server Manager, fai clic su **Servizi Desktop remoto gt _ Panoramica gt _ + licenze Desktop remoto**.  
    2.  Selezionare la macchina virtuale in cui verrà installato il server licenze Desktop remoto (ad esempio, Contoso Cb1).  
    3.  Fai clic su **Avanti**e quindi fai clic su **Aggiungi**.  
4.  Attivare il Server licenze Desktop remoto e aggiungerlo al gruppo di server licenze:  
    1.  In Server Manager, fai clic su **Gestione licenze Desktop remoto gt _ servizi Terminal gt _ di strumenti**.  
    2.  In Gestione licenze Desktop remoto, seleziona il server e quindi fai clic su **gt _ azione Server di attivazione**.  
    3.  Accettare i valori predefiniti dell'attivazione Server guidata accettare le impostazioni predefinite fino a raggiungere la pagina di **informazioni aziendali** . Quindi, Immetti le informazioni sulla tua società.  
    4.  Accettare le impostazioni predefinite per le pagine rimanenti finché la pagina finale. Cancellare **Avvia installazione guidata licenze**e quindi fai clic su **Fine**.  
    5.  Fai clic su **azione gt _ verifica configurazione gt _ aggiungere al gruppo gt _ OK**. Immetti le credenziali per un utente nel gruppo di amministratori di controller di dominio AAD e registrare come SCP. Questo passaggio potrebbe non funzionare se usi Azure AD Domain Services, ma è possibile ignorare eventuali avvisi o errori.  
5.  Aggiungere il nome del certificato e server Gateway Desktop remoto:  
    1.  In Server Manager, fai clic su **Servizi Desktop remoto gt _ Panoramica gt _ + Gateway Desktop remoto**.  
    2.  Nella procedura guidata Aggiungi server Gateway Desktop remoto, selezionare la macchina virtuale in cui si desidera installare il server Gateway Desktop remoto (ad esempio, Contoso WebGw1).  
    3.  Immetti il nome del certificato SSL per il server Gateway Desktop remoto utilizzando l'esterno completamente DNS nome completo (FQDN) del server Gateway Desktop remoto. In Azure, questa è l'etichetta **nome DNS** e Usa il formato servicename.location.cloudapp.azure.com. Ad esempio, contoso.westus.cloudapp.azure.com.  
    4.  Fai clic su **Avanti**e quindi fai clic su **Aggiungi**.
6.  Creare e installare i certificati autofirmati per i server Gestore connessione desktop remoto e Gateway Desktop remoto.

       > [!NOTE]
       > Se sei fornendo e installare i certificati da un'autorità di certificazione attendibile, eseguire le procedure da h passaggio k per ogni ruolo il passaggio. Devi avere il file con estensione pfx disponibile per ognuno di questi certificati.
       
    1.  In Server Manager, fai clic su **Servizi Desktop remoto gt _ Panoramica gt _ attività gt _ modificare le proprietà di distribuzione**.  
    2.  Espandi **i certificati**e Scorri fino a nella tabella. Fai clic sul **Gateway Desktop remoto gt _ Crea nuovo certificato**.  
    3.  Immetti il nome del certificato, utilizzando il nome FQDN esterno del server Gateway Desktop remoto (ad esempio, contoso.westus.cloudapp.azure.com) e quindi immettere la password.  
    4.  Seleziona **Store questo certificato** e quindi passa alla cartella condivisa creato per i certificati in un passaggio precedente. (Ad esempio, \Contoso Cb1\Certificates).  
    5.  Immetti un nome di file per il certificato (ad esempio, ContosoRdGwCert) e quindi fai clic su **Salva**.  
    6.  Seleziona **Consenti il certificato da aggiungere all'archivio certificati Autorità di certificazione radice attendibili nei computer di destinazione**e quindi fai clic su **OK**.  
    7.  Fare clic su **Applica**e quindi attendere che il certificato correttamente da applicare al server Gateway Desktop remoto.  
    8.  Fai clic su **gt _ accesso Web desktop remoto selezionare certificato esistente**.  
    9.  Selezionare il certificato creato per il server Gateway Desktop remoto (ad esempio, ContosoRdGwCert), quindi **Apri**.  
    10. Immetti la password per il certificato, selezionare **consentire il certificato da aggiungere all'archivio certificati radice attendibili nel computer di destinazione**, quindi fai clic su **OK**.  
    11. Fare clic su **Applica**e quindi attendere che il certificato correttamente da applicare al server di accesso Web desktop remoto.  
    12. Ripetere i passaggi 1-11 per il **Gestore connessione desktop remoto - abilitare Single Sign-On** e il **Gestore connessione desktop remoto - pubblicazione di servizi**, utilizzando il nome FQDN interno del server Gestore connessione desktop remoto per il nuovo nome del certificato (ad esempio, Contoso-Cb1.Contoso.com).  
7.  Esportare i certificati autofirmati pubblici e copiarli in un computer client. Se usi i certificati da un'autorità di certificazione attendibile, è possibile ignorare questo passaggio.  
    1.  Avviare certlm.  
    2.  Espandere **personale**e quindi fai clic su **certificati**.  
    3.  Nel riquadro di destra fai il certificato di Gestore connessione desktop remoto progettato per l'autenticazione client, ad esempio **Contoso Cb1.Contoso.com**.  
    4.  Fai clic su **tutte le attività gt _ esportazione**.  
    5.  Accettare che le opzioni predefinite nell'esportazione guidata certificati accettano le impostazioni predefinite fino a raggiungere la pagina di **File di esportazione** .  
    6.  Passa alla cartella condivisa che hai creato per i certificati, ad esempio \Contoso-Cb1\Certificates.  
    7.  Immetti un nome di File, ad esempio ContosoCbClientCert e quindi fai clic su **Salva**.  
    8.  Fai clic su **Avanti**e quindi fai clic su **Fine**.  
    9.  Ripetere i passaggi 1-8 per il Gateway Desktop remoto e il certificato Web, (ad esempio contoso.westus.cloudapp.azure.com), fornendo un nome di file appropriato, ad esempio **ContosoWebGwClientCert**il certificato esportato.  
    10. In Esplora File passa alla cartella in cui sono archiviati i certificati, ad esempio \Contoso-Cb1\Certificates.  
    11. Scegliere i due certificati client esportato, quindi fai doppio clic, quindi **Copia**.  
    12. Incolla il pubblicamente nel computer client locale.  
8.  Configurare le proprietà di distribuzione di Gateway Desktop remoto e la gestione delle licenze di desktop remoto:  
    1.  In Server Manager, fai clic su **Servizi Desktop remoto gt _ Panoramica gt _ attività gt _ modificare le proprietà di distribuzione**.  
    2.  Espandi **Gateway Desktop remoto** e deseleziona l'opzione **server Gateway Desktop remoto Bypass per indirizzi locali** .  
    3.  Espandere **la gestione delle licenze di desktop remoto** e selezionare **Per ogni utente**  
    4.  Scegli **OK**.  
10. Creare una raccolta di sessione. La procedura di crea una raccolta di base. Per ulteriori informazioni su insiemi, consulta [Crea una raccolta di Servizi Desktop remoto per desktop e le App per l'esecuzione](rds-create-collection.md) .
 
    1.  In Server Manager, fai clic su **Servizi Desktop remoto gt _ raccolte gt _ attività gt _ Crea raccolta di sessione**.  
    2.  Immetti un nome (ad esempio, ContosoDesktop) insieme.  
    3.  Selezionare un Server Host sessione Desktop remoto (Sh1 Contoso) accetta i gruppi di utenti predefiniti (Contoso\Domain utenti) e immetti il percorso Universal Naming Convention (UNC) per i dischi del profilo utente creati in precedenza (\Contoso-Cb1\UserDisks).  
    4.  Impostare una dimensione massima, quindi **Crea**.  
  

Ora hai creato un'infrastruttura di base di Servizi Desktop remoto. Se hai bisogno creare una distribuzione a disponibilità elevata, puoi aggiungere un [cluster di Gestore connessione](rds-connection-broker-cluster.md) o un [secondo server Host sessione Desktop remoto](rds-scale-rdsh-farm.md).

