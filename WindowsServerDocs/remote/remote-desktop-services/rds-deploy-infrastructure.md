---
title: Distribuire l'ambiente Desktop remoto
ms.prod: windows-server
description: Procedura di base per distribuire un ambiente Desktop remoto.
ms.technology: remote-desktop-services
ms.author: elizapo
ms.date: 04/10/2017
ms.topic: article
author: lizap
manager: dongill
ms.localizationpriority: medium
ms.openlocfilehash: 31bb6afaca92b36453d4565c1f79aae35a6f0900
ms.sourcegitcommit: 3a3d62f938322849f81ee9ec01186b3e7ab90fe0
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 04/23/2020
ms.locfileid: "80855754"
---
# <a name="deploy-your-remote-desktop-environment"></a>Distribuire l'ambiente Desktop remoto

>Si applica a: Windows Server (Canale semestrale), Windows Server 2019, Windows Server 2016

Usa la procedura descritta di seguito per distribuire i server Desktop remoto nell'ambiente. Puoi installare i ruoli del server in computer fisici o macchine virtuali, a seconda che tu stia creando un ambiente locale, basato sul cloud o ibrido. 

Se usi macchine virtuali per uno o più server Servizi Desktop remoto, assicurati di aver [preparato tali macchine virtuali](rds-prepare-vms.md).
  
  
1.  Aggiungi a Server Manager tutti i server che userai per Servizi Desktop remoto:  
    1.  In Server Manager fai clic su **Gestisci** > **Aggiungi server**.  
    2.  Fai clic su **Trova**.  
    3.  Fai clic sui singoli server nella distribuzione (ad esempio, Contoso-Cb1, Contoso-WebGw1 e Contoso-Sh1) e scegli **OK**.  
2.  Crea una distribuzione basata su sessione per distribuire i componenti di Servizi Desktop remoto:  
    1.  In Server Manager fai clic su **Gestisci** > **Aggiungi ruoli e funzionalità**.  
    2.  Fai clic su **Installazione di Servizi Desktop remoto**, **Distribuzione standard** e **Distribuzione di desktop basati su sessioni**.  
    3.  Seleziona i server appropriati per il server Gestore connessione Desktop remoto, il server Accesso Web Desktop remoto e il server Host sessione Desktop remoto (ad esempio, Contoso-Cb1, Contoso-WebGw1 e Contoso-SH1 rispettivamente).  
    4.  Seleziona **Riavvia automaticamente il server di destinazione se necessario** e quindi fai clic su **Distribuisci**.  
    5.  Attendi che la distribuzione venga completata correttamente.  
3.  Aggiungi il server licenze Desktop remoto:  
    1.  In Server Manager fai clic su **Servizi Desktop remoto > Panoramica > Servizio licenze Desktop remoto**.  
    2.  Seleziona la macchina virtuale in cui verrà installato il server licenze Desktop remoto (ad esempio, Contoso-Cb1).  
    3.  Fai clic su **Avanti** e quindi su **Aggiungi**.  
4.  Attiva il server licenze Desktop remoto e aggiungilo al gruppo Server licenze:  
    1.  In Server Manager fai clic su **Strumenti > Servizi terminal > Gestione licenze Desktop remoto**.  
    2.  In Gestione licenze Desktop remoto seleziona il server e quindi fai clic su **Azione > Attiva server**.  
    3.  Accetta i valori predefiniti nell'Attivazione guidata server. Continua ad accettare i valori predefiniti fino a raggiungere la pagina **Informazioni società**. Immetti quindi le informazioni sulla società.  
    4.  Accetta i valori predefiniti per le pagine restanti fino alla pagina finale. Deseleziona **Avvia Installazione guidata licenze** e quindi fai clic su **Fine**.  
    5.  Fai clic su **Azione > Verifica configurazione > Aggiungi al gruppo > OK**. Immetti le credenziali per un utente nel gruppo Amministratori AAD DC ed effettua la registrazione come SCP. Questo passaggio potrebbe non funzionare se usi Servizi di dominio Active Directory, ma puoi ignorare gli eventuali avvisi o errori.  
5.  Aggiungi il server Gateway Desktop remoto e il nome del certificato:  
    1.  In Server Manager fai clic su **Servizi Desktop remoto > Panoramica > Gateway Desktop remoto**.  
    2.  Nella procedura guidata per l'aggiunta di server Gateway Desktop remoto seleziona la macchina virtuale in cui vuoi installare il server Gateway Desktop remoto (ad esempio, Contoso-WebGw1).  
    3.  Immetti il nome del certificato SSL per il server Gateway Desktop remoto usando l'FQDN esterno di tale server. In Azure questa è l'etichetta del **nome DNS** nel formato nomeservizio.posizione.cloudapp.azure.com. Ad esempio, contoso.westus.cloudapp.azure.com.  
    4.  Fai clic su **Avanti** e quindi su **Aggiungi**.
6.  Crea e installa certificati autofirmati per i server Gateway Desktop remoto e Gestore connessione Desktop remoto.

       > [!NOTE]
       > Se fornisci e installi certificati provenienti da un'autorità di certificazione attendibile, esegui le procedure dal passaggio h al passaggio k per ogni ruolo. Dovrai avere il file con estensione pfx disponibile per ognuno di questi certificati.
       
    1.  In Server Manager fai clic su **Servizi Desktop remoto > Panoramica > Attività > Edit Deployment Properties** (Modifica proprietà distribuzione).  
    2.  Espandi **Certificati** e quindi scorri verso il basso fino alla tabella. Fai clic su **Gateway Desktop remoto > Crea nuovo certificato**.  
    3.  Immetti il nome del certificato usando l'FQDN esterno del server Gateway Desktop remoto (ad esempio, contoso.westus.cloudapp.azure.com) e quindi immetti la password.  
    4.  Seleziona **Archivia il certificato** e quindi passa alla cartella condivisa creata per i certificati in un passaggio precedente. Ad esempio, \Contoso-Cb1\Certificates.  
    5.  Immetti un nome file per il certificato (ad esempio, ContosoRdGwCert) e quindi fai clic su **Salva**.  
    6.  Seleziona **Consenti aggiunta del certificato all'archivio certificati delle Autorità di certificazione radice attendibili nei computer di destinazione** e quindi fai clic su **OK**.  
    7.  Fai clic su **Applica** e quindi attendi che il certificato venga applicato correttamente al server Gateway Desktop remoto.  
    8.  Fai clic su **Accesso Web Desktop remoto > Seleziona certificato esistente**.  
    9.  Individua il certificato creato per il server Gateway Desktop remoto (ad esempio, ContosoRdGwCert) e quindi fai clic su **Apri**.  
    10. Immetti la password per il certificato, seleziona **Consenti aggiunta del certificato all'archivio certificati delle Autorità di certificazione radice attendibili nei computer di destinazione** e quindi fai clic su **OK**.  
    11. Fai clic su **Applica** e quindi attendi che il certificato venga applicato correttamente al server Accesso Web Desktop remoto.  
    12. Ripeti i passaggi secondari da 1 a 11 per **Gestore connessione Desktop remoto - Abilita Single Sign-On** e **Gestore connessione Desktop remoto - Servizi di pubblicazione** usando l'FQDN interno del server Gestore connessione Desktop remoto come nome del nuovo certificato (ad esempio, Contoso-Cb1.Contoso.com).  
7.  Esporta i certificati pubblici autofirmati e copiali in un computer client. Se usi certificati provenienti da un'autorità di certificazione attendibile, puoi ignorare questo passaggio.  
    1.  Avvia certlm.msc.  
    2.  Espandi **Personale** e quindi fai clic su **Certificati**.  
    3.  Nel riquadro destro fai clic con il pulsante destro del mouse sul certificato del Gestore connessione Desktop remoto previsto per l'autenticazione client, ad esempio **Contoso-Cb1.Contoso.com**.  
    4.  Fai clic su **Tutte le attività > Esporta**.  
    5.  Accetta le opzioni predefinite nell'Esportazione guidata certificati fino a raggiungere la pagina **File to Export** (File da esportare).  
    6.  Passa alla cartella condivisa creata per i certificati, ad esempio \Contoso-Cb1\Certificates.  
    7.  Immetti un nome file, ad esempio ContosoCbClientCert, e quindi fai clic su **Salva**.  
    8.  Fare clic su **Avanti**e quindi su **Fine**.  
    9.  Ripeti i passaggi secondari da 1 a 8 per il certificato di Gateway e Web Desktop remoto (ad esempio, contoso.westus.cloudapp.azure.com), assegnando al certificato esportato un nome file appropriato, ad esempio **ContosoWebGwClientCert**.  
    10. In Esplora risorse passa alla cartella in cui sono archiviati i certificati, ad esempio \Contoso-Cb1\Certificates.  
    11. Seleziona i due certificati client esportati, fai clic su di essi con il pulsante destro del mouse e quindi scegli **Copia**.  
    12. Incolla i certificati nel computer client locale.  
8.  Configura le proprietà di distribuzione del Gateway Desktop remoto e del servizio licenze Desktop remoto:  
    1.  In Server Manager fai clic su **Servizi Desktop remoto > Panoramica > Attività > Edit Deployment Properties** (Modifica proprietà distribuzione).  
    2.  Espandi **Gateway Desktop remoto** e deseleziona l'opzione **Ignora server Gateway Desktop remoto per indirizzi locali**.  
    3.  Espandi **Servizio licenze Desktop remoto** e seleziona **Per Utente**  
    4.  Fare clic su **OK**.  
10. Crea una raccolta di sessioni. Questi passaggi consentono di creare una raccolta di base. Per altre informazioni sulle raccolte, vedi [Creare una raccolta Servizi Desktop remoto per desktop e app per l'esecuzione](rds-create-collection.md).
 
    1.  In Server Manager fai clic su **Servizi Desktop remoto > Raccolte > Attività > Create Session Collection** (Crea raccolta di sessioni).  
    2.  Immetti un nome per la raccolta, ad esempio ContosoDesktop.  
    3.  Seleziona un server Host sessione Desktop remoto (Contoso-Sh1), accetta i gruppi di utenti predefiniti (Contoso\Utenti del dominio) e immetti il percorso UNC (Universal Naming Convention) dei dischi di profilo utente creati precedentemente (\Contoso-Cb1\UserDisks).  
    4.  Imposta una dimensione massima e quindi fai clic su **Crea**.  
  

Hai così creato un'infrastruttura di base di Servizi Desktop remoto. Se devi creare una distribuzione a disponibilità elevata, puoi aggiungere un [cluster di server Gestore connessione](rds-connection-broker-cluster.md) o un [secondo server Host sessione Desktop remoto](rds-scale-rdsh-farm.md).

