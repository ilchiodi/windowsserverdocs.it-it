---
title: Distribuzione di AD FS di disponibilità elevata tra aree geografiche in Azure con gestione traffico di Azure | Microsoft Docs
description: In questo documento si apprenderà come distribuire AD FS in Azure per disponibilità elevati.
keywords: ADFS con gestione traffico di Azure, ADFS con gestione traffico di Azure, geografico, più data center, Data Center geografici, Data Center multigeografia, distribuzione di AD FS in Azure, distribuzione di Azure ADFS, Azure ADFS, Azure AD FS, distribuzione di ADFS, distribuzione di ADFS, ADFS in Azure, distribuire ADFS in Azure, distribuire AD FS in Azure, ad FS Azure, introduzione a AD FS, Azure, AD FS in Azure, IaaS, ADFS, spostare ADFS in Azure
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
ms.openlocfilehash: d98eb126513d707bce7abe3e901c8bf584d2319c
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/10/2019
ms.locfileid: "70868023"
---
# <a name="high-availability-cross-geographic-ad-fs-deployment-in-azure-with-azure-traffic-manager"></a>Distribuzione di AD FS di disponibilità elevata tra aree geografiche in Azure con gestione traffico di Azure
[Ad FS distribuzione in Azure](how-to-connect-fed-azure-adfs.md) fornisce indicazioni dettagliate su come distribuire una semplice infrastruttura ad FS per la propria organizzazione in Azure. Questo articolo illustra i passaggi successivi per creare una distribuzione tra più aree geografiche di AD FS in Azure usando [Gestione traffico di Azure](https://docs.microsoft.com/azure/traffic-manager/). Gestione traffico di Azure consente di creare un'infrastruttura a disponibilità elevata e a elevate AD FS prestazioni geograficamente elevata per la propria organizzazione, usando la gamma di metodi di routing disponibili per soddisfare le diverse esigenze dell'infrastruttura.

Un'infrastruttura di AD FS tra aree geografiche A disponibilità elevata consente di:

* **Eliminazione del singolo punto di errore:** Con le funzionalità di failover di gestione traffico di Azure, è possibile ottenere un'infrastruttura AD FS a disponibilità elevata anche quando uno dei data center in una parte del mondo diventa inattivo
* **Miglioramento delle prestazioni:** È possibile usare la distribuzione suggerita in questo articolo per fornire un'infrastruttura di AD FS a prestazioni elevate che consenta agli utenti di eseguire l'autenticazione più velocemente. 

## <a name="design-principles"></a>Principi di progettazione
![Progettazione complessiva](./media/active-directory-adfs-in-azure-with-azure-traffic-manager/blockdiagram.png)

I principi di progettazione di base saranno uguali a quelli elencati nei principi di progettazione nell'articolo AD FS distribuzione in Azure. Il diagramma precedente illustra una semplice estensione della distribuzione di base a un'altra area geografica. Di seguito sono riportati alcuni aspetti da considerare quando si estende la distribuzione a una nuova area geografica

* **Rete virtuale:** È necessario creare una nuova rete virtuale nell'area geografica in cui si vuole distribuire un'infrastruttura AD FS aggiuntiva. Nel diagramma precedente vengono visualizzati Geo1 VNET e GeO2 VNET come due reti virtuali in ogni area geografica.
* **Controller di dominio e server AD FS nei nuovi VNET geografici:** È consigliabile distribuire i controller di dominio nella nuova area geografica in modo che i server AD FS nella nuova area non debbano contattare un controller di dominio in un'altra rete distante per completare un'autenticazione e quindi migliorare le prestazioni.
* **Account di archiviazione:** Gli account di archiviazione sono associati a un'area. Poiché i computer verranno distribuiti in una nuova area geografica, sarà necessario creare nuovi account di archiviazione da usare nell'area.  
* **Gruppi di sicurezza di rete:** Poiché gli account di archiviazione, i gruppi di sicurezza di rete creati in un'area non possono essere usati in un'altra area geografica. Sarà quindi necessario creare nuovi gruppi di sicurezza di rete simili a quelli nella prima area geografica per la subnet INT e DMZ nella nuova area geografica.
* **Etichette DNS per gli indirizzi IP pubblici:** Gestione traffico di Azure può fare riferimento agli endpoint solo tramite etichette DNS. Pertanto, è necessario creare etichette DNS per gli indirizzi IP pubblici del servizio di bilanciamento del carico esterno.
* **Gestione traffico di Azure:** Gestione traffico di Microsoft Azure consente di controllare la distribuzione del traffico utente agli endpoint di servizio in esecuzione in data center diversi in tutto il mondo. Gestione traffico di Azure funziona a livello di DNS. Usa le risposte DNS per indirizzare il traffico degli utenti finali agli endpoint distribuiti a livello globale. I client si connettono quindi direttamente a questi endpoint. Con diverse opzioni di routing di prestazioni, ponderate e priorità, è possibile scegliere facilmente l'opzione di routing più adatta alle esigenze dell'organizzazione. 
* **Connettività da v-NET a V-NET tra due aree:** Non è necessario disporre di connettività tra le reti virtuali stesse. Poiché ogni rete virtuale ha accesso ai controller di dominio e ha AD FS e server WAP, può funzionare senza alcuna connettività tra le reti virtuali in aree diverse. 

## <a name="steps-to-integrate-azure-traffic-manager"></a>Procedura per l'integrazione di gestione traffico di Azure
### <a name="deploy-ad-fs-in-the-new-geographical-region"></a>Distribuire AD FS nella nuova area geografica
Seguire i passaggi e le linee guida in [ad FS distribuzione in Azure](how-to-connect-fed-azure-adfs.md) per distribuire la stessa topologia nella nuova area geografica.

### <a name="dns-labels-for-public-ip-addresses-of-the-internet-facing-public-load-balancers"></a>Etichette DNS per gli indirizzi IP pubblici dei bilanciamenti del carico con connessione Internet (pubblico)
Come indicato in precedenza, gestione traffico di Azure può fare riferimento solo alle etichette DNS come endpoint ed è quindi importante creare etichette DNS per gli indirizzi IP pubblici dei bilanciamenti del carico esterno. Lo screenshot seguente mostra come è possibile configurare l'etichetta DNS per l'indirizzo IP pubblico. 

![Etichetta DNS](./media/active-directory-adfs-in-azure-with-azure-traffic-manager/eastfabstsdnslabel.png)

### <a name="deploying-azure-traffic-manager"></a>Distribuzione di gestione traffico di Azure
Attenersi alla procedura seguente per creare un profilo di gestione traffico. Per altre informazioni, è anche possibile fare riferimento a [gestire un profilo di gestione traffico di Azure](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-manage-profiles).

1. **Creare un profilo di gestione traffico:** Assegnare al profilo di gestione traffico un nome univoco. Questo nome del profilo fa parte del nome DNS e funge da prefisso per l'etichetta del nome di dominio di Traffic Manager. Il nome o il prefisso viene aggiunto a. trafficmanager.net per creare un'etichetta DNS per gestione traffico. La schermata seguente mostra il prefisso DNS di gestione traffico impostato come mysts e l'etichetta DNS risultante sarà mysts.trafficmanager.net. 
   
    ![Creazione del profilo di gestione traffico](./media/active-directory-adfs-in-azure-with-azure-traffic-manager/trafficmanager01.png)
2. **Metodo di routing del traffico:** In gestione traffico sono disponibili tre opzioni di routing:
   
   * Priority 
   * Prestazioni
   * Ponderata
     
     **Prestazioni** è l'opzione consigliata per ottenere un'infrastruttura di ad FS altamente reattiva. Tuttavia, è possibile scegliere qualsiasi metodo di routing più adatto alle proprie esigenze di distribuzione. Il AD FS funzionalità non è influenzato dall'opzione di routing selezionata. Per ulteriori informazioni, vedere [metodi di routing del traffico di gestione traffico](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-routing-methods) . Nella schermata di esempio precedente è possibile vedere il metodo di **prestazioni** selezionato.
3. **Configurare gli endpoint:** Nella pagina Gestione traffico fare clic su endpoint e selezionare Aggiungi. Verrà aperta una pagina Aggiungi endpoint simile alla schermata seguente.
   
   ![Configurare gli endpoint](./media/active-directory-adfs-in-azure-with-azure-traffic-manager/eastfsendpoint.png)
   
   Per i diversi input, attenersi alle linee guida seguenti:
   
   **Tipo:** Selezionare endpoint di Azure perché verrà puntato a un indirizzo IP pubblico di Azure.
   
   **Name:** Creare un nome che si desidera associare all'endpoint. Questo non è il nome DNS e non ha alcun impatto sui record DNS.
   
   **Tipo di risorsa di destinazione:** Selezionare indirizzo IP pubblico come valore per questa proprietà. 
   
   **Risorsa di destinazione:** In questo modo sarà possibile scegliere tra le diverse etichette DNS disponibili nella sottoscrizione. Scegliere l'etichetta DNS corrispondente all'endpoint che si sta configurando.
   
   Aggiungere un endpoint per ogni area geografica in cui si vuole che gestione traffico di Azure instradare il traffico.
   Per altre informazioni e procedure dettagliate su come aggiungere o configurare endpoint in gestione traffico, vedere [aggiungere, disabilitare, abilitare o eliminare gli endpoint](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-manage-endpoints)
4. **Configurare il probe:** Nella pagina Gestione traffico fare clic su configurazione. Nella pagina configurazione è necessario modificare le impostazioni di monitoraggio per eseguire il probe sulla porta HTTP 80 e sul percorso relativo/ADFS/Probe
   
    ![Configurare il probe](./media/active-directory-adfs-in-azure-with-azure-traffic-manager/mystsconfig.png) 
   
   > [!NOTE]
   > **Verificare che lo stato degli endpoint sia online al termine della configurazione**. Se tutti gli endpoint si trovano nello stato ' danneggiato ', gestione traffico di Azure eseguirà il tentativo migliore di instradare il traffico presumendo che la diagnostica non sia corretta e che tutti gli endpoint siano raggiungibili.
   > 
   > 
5. **Modifica dei record DNS per gestione traffico di Azure:** Il servizio federativo deve essere un CNAME per il nome DNS di gestione traffico di Azure. Creare un record CNAME nei record DNS pubblici in modo che chiunque tenti di raggiungere il servizio federativo raggiunga effettivamente gestione traffico di Azure.
   
    Ad esempio, per puntare il servizio federativo fs.fabidentity.com a gestione traffico, è necessario aggiornare il record di risorse DNS come segue:
   
    <code>fs.fabidentity.com IN CNAME mysts.trafficmanager.net</code>

## <a name="test-the-routing-and-ad-fs-sign-in"></a>Testare il routing e l'accesso AD FS
### <a name="routing-test"></a>Test di routing
Un test molto semplice per il routing consiste nel provare a eseguire il ping del nome DNS del servizio federativo da un computer in ogni area geografica. A seconda del metodo di routing scelto, l'endpoint effettivamente ping verrà riflesso nella visualizzazione ping. Se, ad esempio, è stato selezionato il routing delle prestazioni, verrà raggiunto l'endpoint più vicino all'area del client. Di seguito è riportato lo snapshot di due ping da due computer client di area diversi, uno nell'area EastAsia e l'altro negli Stati Uniti occidentali. 

![Test di routing](./media/active-directory-adfs-in-azure-with-azure-traffic-manager/pingtest.png)

### <a name="ad-fs-sign-in-test"></a>Test di accesso AD FS
Il modo più semplice per testare AD FS consiste nell'usare la pagina IdpInitiatedSignon. aspx. Per poter eseguire questa operazione, è necessario abilitare IdpInitiatedSignOn nelle proprietà AD FS. Attenersi alla procedura seguente per verificare la configurazione del AD FS

1. Eseguire il cmdlet seguente nel server AD FS, usando PowerShell, per impostarlo su abilitato. 
   Set-AdfsProperties-EnableIdPInitiatedSignonPage $true
2. Da qualsiasi computer esterno accedere a<yourfederationservicedns>https:///ADFS/LS/IdpInitiatedSignon.aspx
3. Dovrebbe essere visualizzata la pagina AD FS come riportato di seguito:
   
    ![Test ADFS-Verifica autenticazione](./media/active-directory-adfs-in-azure-with-azure-traffic-manager/adfstest1.png)
   
    al termine dell'accesso, verrà visualizzato un messaggio di operazione completata, come illustrato di seguito:
   
    ![Test ADFS-autenticazione riuscita](./media/active-directory-adfs-in-azure-with-azure-traffic-manager/adfstest2.png)

## <a name="related-links"></a>Collegamenti correlati
* [Distribuzione di AD FS di base in Azure](how-to-connect-fed-azure-adfs.md)
* [Gestione traffico di Microsoft Azure](https://docs.microsoft.com/azure/traffic-manager/)
* [Metodi di routing del traffico di gestione traffico](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-routing-methods)

## <a name="next-steps"></a>Passaggi successivi
* [Gestire un profilo di gestione traffico di Azure](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-manage-profiles)
* [Aggiungere, disabilitare, abilitare o eliminare gli endpoint](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-manage-endpoints) 

