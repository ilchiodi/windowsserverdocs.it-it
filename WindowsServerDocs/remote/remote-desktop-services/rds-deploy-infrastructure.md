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
ms.openlocfilehash: acfdd99fa67e218f58fe650de5607f2a5ba97bf8
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59833832"
---
# <a name="deploy-your-remote-desktop-environment"></a>Distribuire l'ambiente Desktop remoto

>Si applica a: Windows Server (canale semestrale), Windows Server 2016

Usare la procedura seguente per distribuire i server di Desktop remoto nell'ambiente in uso. È possibile installare i ruoli del server su computer fisici o macchine virtuali, a seconda se si siano creando un on-premise, basati sul cloud o ambiente ibrido. 

Se si usa macchine virtuali per uno qualsiasi dei server di Servizi Desktop remoto, assicurarsi di avere [preparare le macchine virtuali](rds-prepare-vms.md).
  
  
1.  Aggiungere tutti i server di cui che si intende usare per Servizi Desktop remoto a Server Manager:  
    1.  In Server Manager fare clic su **Gestisci > Aggiungi server**.  
    2.  Fare clic su **trova**.  
    3.  Fare clic su ogni server nella distribuzione (ad esempio, Contoso-Cb1, Contoso-WebGw1 e Sh1 di Contoso) e fare clic su **OK**.  
2.  Creare una distribuzione basata su sessione per distribuire i componenti Servizi Desktop remoto:  
    1.  In Server Manager fare clic su **Gestisci > Aggiungi ruoli e funzionalità**.  
    2.  Fare clic su **installazione di Servizi Desktop remoto**, **Standard Deployment**, e **distribuzione desktop basati su sessione**.  
    3.  Selezionare i server appropriati per il server Gestore connessione desktop remoto, accesso Web desktop remoto e server Host sessione Desktop remoto (ad esempio, Contoso-Cb1, Contoso-WebGw1 e Contoso-SH1, rispettivamente).  
    4.  Selezionare **riavvia automaticamente il server di destinazione se necessario**, quindi fare clic su **Distribuisci**.  
    5.  Attendere il completamento della distribuzione  
3.  Aggiungere Server licenze Desktop remoto:  
    1.  In Server Manager fare clic su **Servizi Desktop remoto > Panoramica > + servizio licenze Desktop remoto**.  
    2.  Selezionare la macchina virtuale in cui verrà installato il server licenze Desktop remoto (ad esempio, Contoso-Cb1).  
    3.  Fare clic su **successivo**, quindi fare clic su **Add**.  
4.  Attivare il Server licenze Desktop remoto e aggiungerla al gruppo server licenze:  
    1.  In Server Manager fare clic su **strumenti > Servizi Terminal > Gestione licenze Desktop remoto**.  
    2.  In Gestione licenze Desktop remoto, selezionare il server e quindi fare clic su **azione > Attiva Server**.  
    3.  Accettare i valori predefiniti nell'attivazione guidata Server accettando le impostazioni predefinite fino a raggiungere il **informazioni società** pagina. Quindi, immettere le informazioni della società.  
    4.  Accettare i valori predefiniti per le pagine rimanenti finché la pagina finale. Deselezionare **Avvia installazione guidata licenze**, quindi fare clic su **fine**.  
    5.  Fare clic su **azione > esaminare la configurazione > Aggiungi a gruppo > OK**. Immettere le credenziali per un utente nel gruppo di AAD DC Administrators e registrare come SCP. Questo passaggio potrebbe non funzionare se si usa Azure AD Domain Services, ma è possibile ignorare eventuali avvisi o errori.  
5.  Aggiungere il nome di server e il certificato di Gateway Desktop remoto:  
    1.  In Server Manager fare clic su **Servizi Desktop remoto > Panoramica > + Gateway Desktop remoto**.  
    2.  Nella procedura guidata Aggiungi server Gateway Desktop remoto, selezionare la macchina virtuale in cui si desidera installare il server Gateway Desktop remoto (ad esempio, Contoso-WebGw1).  
    3.  Immettere il nome del certificato SSL per il server Gateway Desktop remoto con l'esterno completamente DNS nome completo (FQDN) del server Gateway Desktop remoto. In Azure, questo è il **nome DNS** assegnare un'etichetta e Usa il formato servicename.location.cloudapp.azure.com. Ad esempio, contoso.westus.cloudapp.azure.com.  
    4.  Fare clic su **successivo**, quindi fare clic su **Add**.
6.  Creare e installare i certificati autofirmati per i server Gateway Desktop remoto e Gestore connessione desktop remoto.

       > [!NOTE]
       > Se si è fornire e installare i certificati delle autorità di certificazione attendibile, eseguire le procedure da h di passaggio per passaggio k per ogni ruolo. È necessario disporre del file con estensione pfx disponibile per ognuno di questi certificati.
       
    1.  In Server Manager fare clic su **Servizi Desktop remoto > Panoramica > attività > modificare le proprietà di distribuzione**.  
    2.  Espandere **certificati**e quindi scorrere fino alla tabella. Fare clic su **Gateway Desktop remoto > Crea nuovo certificato**.  
    3.  Immettere il nome del certificato, usando il nome di dominio completo esterna del server Gateway Desktop remoto (ad esempio, contoso.westus.cloudapp.azure.com) e quindi immettere la password.  
    4.  Selezionare **Store questo certificato** e quindi passare alla cartella condivisa creata per i certificati in un passaggio precedente. (Ad esempio, \Contoso Cb1\Certificates.)  
    5.  Immettere un nome file del certificato (ad esempio, ContosoRdGwCert) e quindi fare clic su **salvare**.  
    6.  Selezionare **consentire il certificato da aggiungere all'archivio certificati Autorità di certificazione radice attendibili nel computer di destinazione**, quindi fare clic su **OK**.  
    7.  Fare clic su **applica**, quindi attendere che il certificato da applicare al server Gateway Desktop remoto.  
    8.  Fare clic su **accesso Web desktop remoto > selezionare un certificato esistente**.  
    9.  Selezionare il certificato creato per il server Gateway Desktop remoto (ad esempio, ContosoRdGwCert) e quindi fare clic su **aperto**.  
    10. Immettere la password per il certificato, seleziona **consentire il certificato da aggiungere all'archivio certificati radice Trusted nei computer di destinazione**, quindi fare clic su **OK**.  
    11. Fare clic su **applica**, quindi attendere che il certificato da applicare al server Accesso Web desktop remoto.  
    12. Ripetere l'operazione substeps 1 a 11 per il **Gestore connessione desktop remoto - Attiva Single Sign-On** e **Gestore connessione desktop remoto - pubblicazione di servizi**, usando il nome FQDN interno del server Gestore connessione desktop remoto per il nuovo nome del certificato (ad esempio, Contoso-Cb1.Contoso.com).  
7.  Esportare i certificati pubblici autofirmati e copiarli in un computer client. Se si utilizzano certificati da un'autorità di certificazione attendibile, è possibile ignorare questo passaggio.  
    1.  Avviare certlm. msc.  
    2.  Espandere **personali**, quindi fare clic su **certificati**.  
    3.  Nel riquadro di destra fare doppio clic su previsto per l'autenticazione client, ad esempio il certificato di Gestore connessione desktop remoto **Contoso-Cb1.Contoso.com**.  
    4.  Fare clic su **tutte le attività > Esporta**.  
    5.  Accettare le opzioni predefinite in esportazione guidata certificati accettano le impostazioni predefinite fino a raggiungere il **File da esportare** pagina.  
    6.  Passare alla cartella condivisa creata per i certificati, ad esempio \Contoso-Cb1\Certificates.  
    7.  Immettere un nome di File, ad esempio ContosoCbClientCert e quindi fare clic su **salvare**.  
    8.  Fare clic su **Avanti**e quindi su **Fine**.  
    9.  Ripetere passaggi 1-8 per il certificato di Gateway Desktop remoto e Web, (ad esempio contoso.westus.cloudapp.azure.com), fornendo ad esempio un nome di file appropriato, il certificato esportato **ContosoWebGwClientCert**.  
    10. In Esplora File passare alla cartella in cui sono archiviati i certificati, ad esempio \Contoso-Cb1\Certificates.  
    11. Selezionare i due certificati client esportato, quindi fare doppio clic su essi e fare clic su **copia**.  
    12. Incollare i certificati nel computer client locale.  
8.  Configurare le proprietà di distribuzione di Gateway Desktop remoto e servizio licenze Desktop remoto:  
    1.  In Server Manager fare clic su **Servizi Desktop remoto > Panoramica > attività > modificare le proprietà di distribuzione**.  
    2.  Espandere **Gateway Desktop remoto** e deselezionare le **server Gateway Desktop remoto di Bypass per indirizzi locali** opzione.  
    3.  Espandere **servizio licenze Desktop remoto** e selezionare **Per utente**  
    4.  Fare clic su **OK**.  
10. Creare un insieme di sessioni. Questi passaggi creano una raccolta di base. Consulta [creare una raccolta di Servizi Desktop remoto per desktop e App per l'esecuzione](rds-create-collection.md) per altre informazioni sulle raccolte.
 
    1.  In Server Manager fare clic su **Servizi Desktop remoto > raccolte > attività > Crea insiemi di sessioni**.  
    2.  Immettere una nome (ad esempio, ContosoDesktop) di raccolta.  
    3.  Selezionare un Server di Host sessione Desktop remoto (Contoso-Sh1), accettare i gruppi di utenti predefinito (Contoso\Domain utenti) e immettere il percorso Universal Naming Convention (UNC) per i dischi dei profili utente creati in precedenza (\Contoso-Cb1\UserDisks).  
    4.  Impostare le dimensioni massime e quindi fare clic su **Create**.  
  

È stata creata un'infrastruttura di Servizi Desktop remoto di base. Se è necessario creare una distribuzione a disponibilità elevata, è possibile aggiungere un [cluster di Gestore connessione](rds-connection-broker-cluster.md) o una [secondo server Host sessione Desktop remoto](rds-scale-rdsh-farm.md).

