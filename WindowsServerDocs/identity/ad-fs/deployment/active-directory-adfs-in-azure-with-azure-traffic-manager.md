---
title: Distribuzione di disponibilità elevata tra aree geografiche AD FS in Azure con gestione traffico di Azure | Microsoft Docs
description: In questo documento si apprenderà come distribuire ADFS in Azure per ottenere una disponibilità elevata.
keywords: Ad fs con gestione traffico di Azure, ad FS con gestione traffico di Azure, geografici, più Data Center, Data Center geografici, più datacenter geografici, distribuire AD FS in azure, distribuire azure adfs, adfs azure, azure ADFS, distribuire ad FS, distribuire ad fs, adfs in azure, distribuire ad FS in azure, distribuire AD FS in azure, adfs azure, introduzione ad AD FS, Azure, AD FS in Azure, iaas, ADFS, trasferire adfs in azure
services: active-directory
documentationcenter: ''
author: anandyadavmsft
manager: mtillman
editor: ''
ms.assetid: a14bc870-9fad-45ed-acd5-a90ccd432e54
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 09/01/2016
ms.author: anandy;billmath
ms.openlocfilehash: 4af0386ef97694baa9ba7f1e5e7163554d79734a
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59874122"
---
# <a name="high-availability-cross-geographic-ad-fs-deployment-in-azure-with-azure-traffic-manager"></a>Distribuzione di disponibilità elevata tra aree geografiche AD FS in Azure con gestione traffico di Azure
[Distribuzione di AD FS in Azure](how-to-connect-fed-azure-adfs.md) offre istruzioni dettagliate come è possibile distribuire una semplice infrastruttura AD FS per l'organizzazione in Azure. Questo articolo illustra i passaggi successivi per creare una distribuzione di AD FS tra aree geografiche in Azure usando [gestione traffico di Azure](https://docs.microsoft.com/azure/traffic-manager/). Gestione traffico di Azure consente di creare un geograficamente la disponibilità elevata diffusione e l'infrastruttura AD FS a prestazioni elevate per l'organizzazione, rendendo ricorso alla gamma di metodi di routing disponibili in base alle proprie esigenze diverse dall'infrastruttura.

Consente a un'infrastruttura di AD FS a disponibilità elevata tra aree geografiche:

* **Eliminazione del singolo punto di errore:** Con funzionalità di failover di Traffic Manager di Azure, è possibile ottenere un'infrastruttura AD FS a disponibilità elevata anche quando uno dei data center in una parte di tutto il mondo diventa inattivo
* **Miglioramento delle prestazioni:** È possibile utilizzare la distribuzione descritta in questo articolo per fornire un'infrastruttura AD FS a prestazioni elevate che consenta agli utenti di eseguire l'autenticazione più velocemente. 

## <a name="design-principles"></a>Principi di progettazione
![Progettazione complessiva](./media/active-directory-adfs-in-azure-with-azure-traffic-manager/blockdiagram.png)

I principi di progettazione di base sono uguali a quelli elencati nella sezione principi di progettazione dell'articolo distribuzione di AD FS in Azure. Il diagramma precedente illustra una semplice estensione della distribuzione di base a un'altra area geografica. Di seguito sono riportati alcuni aspetti da considerare quando si estende la distribuzione alla nuova area geografica

* **Rete virtuale:** È consigliabile creare una nuova rete virtuale nell'area geografica che si desidera distribuire un'infrastruttura AD FS aggiuntiva. Nel diagramma precedente vengono visualizzati Geo1 VNET e Geo2 VNET sono come le due reti virtuali in ogni area geografica.
* **I controller di dominio e server AD FS nella nuova rete virtuale geografica:** È consigliabile distribuire dominio controller nella nuova area geografica in modo che i server AD FS nella nuova area non debbano contattare un controller di dominio in un altro a portata di mano di rete per completare l'autenticazione, migliorando le prestazioni.
* **Account di archiviazione:** Gli account di archiviazione sono associati a un'area. Poiché si distribuirà macchine nella nuova area geografica, è necessario creare nuovi account di archiviazione da usare nell'area.  
* **Gruppi di sicurezza di rete:** Come gli account di archiviazione creato in un'area gruppi di sicurezza di rete non possono essere usati in un'altra area geografica. Di conseguenza, dovrai creare rete nuovi gruppi di sicurezza simili a quelli della prima area geografica per la subnet INT e DMZ nella nuova area geografica.
* **Etichette DNS per gli indirizzi IP pubblici:** Gestione traffico di Azure possono fare riferimento agli endpoint solo tramite etichette DNS. Pertanto, è necessario creare etichette DNS per gli indirizzi IP pubblici di bilanciamento del carico esterno.
* **Gestione traffico di Azure:** Gestione traffico di Microsoft Azure consente di controllare la distribuzione del traffico utente agli endpoint di servizio in esecuzione nel Data Center diversi dislocati in tutto il mondo. Gestione traffico di Azure funziona a livello di DNS. Usa le risposte DNS per indirizzare il traffico degli utenti finali agli endpoint distribuiti a livello globale. I client quindi si connettono a tali endpoint direttamente. Con diverse opzioni di prestazioni, ponderato e priorità di routing, è possibile scegliere facilmente l'opzione di routing più adatto alle esigenze dell'organizzazione. 
* **V-net alla connettività tra due aree:** Non è necessario avere la connettività tra le reti virtuali se stessa. Poiché ogni rete virtuale abbia accesso ai controller di dominio e dispone di server AD FS e WAP in sé, possono essere usati senza connettività tra le reti virtuali in aree diverse. 

## <a name="steps-to-integrate-azure-traffic-manager"></a>Passaggi per l'integrazione di gestione traffico di Azure
### <a name="deploy-ad-fs-in-the-new-geographical-region"></a>Distribuire AD FS nella nuova area geografica
Seguire i passaggi e le linee guida in [distribuzione di AD FS in Azure](how-to-connect-fed-azure-adfs.md) per distribuire la stessa topologia nella nuova area geografica.

### <a name="dns-labels-for-public-ip-addresses-of-the-internet-facing-public-load-balancers"></a>Etichette DNS per gli indirizzi IP pubblici dei bilanciamenti del carico con connessione Internet (pubblico)
Come indicato in precedenza, gestione traffico di Azure può fare riferimento solo alle etichette DNS come endpoint e pertanto è importante creare etichette DNS per gli indirizzi IP pubblici di bilanciamento del carico esterno. Lo screenshot seguente illustra come configurare l'etichetta DNS per l'indirizzo IP pubblico. 

![Etichetta DNS](./media/active-directory-adfs-in-azure-with-azure-traffic-manager/eastfabstsdnslabel.png)

### <a name="deploying-azure-traffic-manager"></a>Distribuzione di gestione traffico di Azure
Attenersi alla procedura seguente per creare un profilo di gestione traffico. Per altre informazioni, è anche possibile fare riferimento a [gestire un profilo di gestione traffico di Azure](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-manage-profiles).

1. **Creare un profilo di Traffic Manager:** Assegnare il profilo di gestione traffico un nome univoco. Questo nome del profilo fa parte del nome DNS e funge da prefisso per l'etichetta di nome di dominio di Traffic Manager. Il nome / prefisso viene aggiunto a. trafficmanager.net per creare un'etichetta DNS per il traffico di gestione. La schermata seguente mostra gestione traffico prefisso DNS impostato come mysts ed etichetta DNS sarà mysts.trafficmanager.net. 
   
    ![Creazione del profilo di gestione traffico](./media/active-directory-adfs-in-azure-with-azure-traffic-manager/trafficmanager01.png)
2. **Metodo di routing del traffico:** Sono disponibili tre opzioni di routing in Gestione traffico:
   
   * Priority 
   * Prestazioni
   * Ponderato
     
     **Prestazioni** è l'opzione consigliata per ottenere la velocità di risposta elevata dell'infrastruttura di AD FS. Tuttavia, è possibile scegliere qualsiasi metodo di routing ottimale per le esigenze di distribuzione. Funzionalità di AD FS non è interessato dall'opzione di routing selezionato. Visualizzare [metodi di routing del traffico di gestione traffico](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-routing-methods) per altre informazioni. Nell'esempio di screenshot precedente è possibile vedere le **prestazioni** metodo selezionato.
3. **Configurare gli endpoint:** Nella pagina di gestione traffico, fare clic su endpoint e selezionare Aggiungi. Verrà aperta una pagina Aggiungi endpoint simile allo screenshot seguente
   
   ![Configurare gli endpoint](./media/active-directory-adfs-in-azure-with-azure-traffic-manager/eastfsendpoint.png)
   
   Per gli input diversi, seguire le linee guida seguenti:
   
   **Tipo:** Selezionare l'endpoint di Azure come destinazione è per un indirizzo IP pubblico di Azure.
   
   **Nome:** Creare un nome che si desidera associare all'endpoint. Ciò non è il nome DNS e non è rilevante per i record DNS.
   
   **Tipo di risorsa di destinazione:** Selezionare l'indirizzo IP pubblico come valore per questa proprietà. 
   
   **Risorsa di destinazione:** Ciò fornirà un'opzione per scegliere tra le varie etichette DNS che disponibili nella sottoscrizione. Scegliere l'etichetta DNS corrispondente all'endpoint che si sta configurando.
   
   Aggiungere l'endpoint per ogni area geografica che cui gestione traffico di Azure per indirizzare il traffico.
   Per altre informazioni e procedure dettagliate su come aggiungere o configurare gli endpoint in Gestione traffico, vedere [aggiungere, disabilitare, abilitare o eliminare endpoint](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-manage-endpoints)
4. **Configurare il probe:** Nella pagina di gestione traffico, fare clic su configurazione. Nella pagina di configurazione, è necessario modificare le impostazioni di monitoraggio per il probe della porta HTTP 80 e il percorso relativo /adfs/probe
   
    ![Configurare il probe](./media/active-directory-adfs-in-azure-with-azure-traffic-manager/mystsconfig.png) 
   
   > [!NOTE]
   > **Assicurarsi che lo stato degli endpoint sia ONLINE dopo aver completato la configurazione**. Se tutti gli endpoint sono nello stato 'danneggiato', gestione traffico di Azure eseguirà un tentativo migliore per indirizzare il traffico supponendo che i dati di diagnostica non è corretto e che tutti gli endpoint siano raggiungibili.
   > 
   > 
5. **Modifica di record DNS per gestione traffico di Azure:** Il servizio federativo deve essere un record CNAME per il nome DNS di Traffic Manager di Azure. Creare un record CNAME nei record DNS pubblici in modo che chiunque provi a raggiungere il servizio federativo raggiunga in realtà gestione traffico di Azure.
   
    Ad esempio, in modo da puntare fs.fabidentity.com il servizio federativo a gestione traffico, è necessario aggiornare il record risorsa DNS per soddisfare le condizioni seguenti:
   
    <code>fs.fabidentity.com IN CNAME mysts.trafficmanager.net</code>

## <a name="test-the-routing-and-ad-fs-sign-in"></a>Testare il routing e accesso in AD FS
### <a name="routing-test"></a>Test di routing
Un test molto semplice per il routing, è possibile provare il ping del nome DNS del servizio federativo da un computer in ogni area geografica. A seconda del metodo di routing scelto, l'endpoint che effettivamente raggiunto dal ping verrà riflesse nella visualizzazione del ping. Ad esempio, se si seleziona il routing delle prestazioni, quindi l'endpoint più vicino all'area del client verrà raggiunto. Di seguito è lo snapshot di due ping da due macchine client area diversa, uno in Asia orientale e uno negli Stati Uniti occidentali. 

![Test di routing](./media/active-directory-adfs-in-azure-with-azure-traffic-manager/pingtest.png)

### <a name="ad-fs-sign-in-test"></a>AD FS sign-in di test
Il modo più semplice per testare AD FS consiste nell'usare la pagina Idpinitiatedsignon. Per essere in grado di eseguire operazioni che, è necessario per abilitare IdpInitiatedSignOn nelle proprietà di AD FS. Attenersi alla procedura seguente per verificare l'installazione di AD FS

1. Eseguire il seguente cmdlet nel server di AD FS, uso di PowerShell, per impostare l'abilitazione. 
   Set-AdfsProperties -EnableIdPInitiatedSignonPage $true
2. Da qualsiasi computer esterno accedere a https://<yourfederationservicedns>/adfs/ls/IdpInitiatedSignon.aspx
3. Verrà visualizzata la pagina di AD FS come il seguente:
   
    ![Test di ad FS - richiesta di autenticazione](./media/active-directory-adfs-in-azure-with-azure-traffic-manager/adfstest1.png)
   
    e sul successo Accedi, fornirà all'utente un messaggio di conferma come illustrato di seguito:
   
    ![Test di ad FS - autenticazione completata](./media/active-directory-adfs-in-azure-with-azure-traffic-manager/adfstest2.png)

## <a name="related-links"></a>Collegamenti correlati
* [Distribuzione di AD FS in Azure](how-to-connect-fed-azure-adfs.md)
* [Gestione traffico di Microsoft Azure](https://docs.microsoft.com/azure/traffic-manager/)
* [Metodi di routing del traffico di gestione traffico](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-routing-methods)

## <a name="next-steps"></a>Passaggi successivi
* [Gestire un profilo di gestione traffico di Azure](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-manage-profiles)
* [Aggiungere, disabilitare, abilitare o eliminare gli endpoint](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-manage-endpoints) 

