---
ms.assetid: 826974ea-3635-40df-aa37-77dd12a363c8
title: "Elenco di controllo: configurazione dell'organizzazione Partner Account"
description: ''
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 5c01daa86b52f3f175d763d09b4c779affef9681
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/24/2019
ms.locfileid: "66192417"
---
# <a name="checklist-configuring-the-account-partner-organization"></a>Elenco di controllo: Configurazione dell'organizzazione partner account

Organizzazione partner account include gli utenti che avranno accesso Web\-applicazioni nel partner risorse. Gli amministratori nell'organizzazione devono utilizzare lo snap di gestione di ADFS\-per creare trust della relying party per rappresentare le relazioni di trust con le organizzazioni partner risorse. A sua volta, l'amministratore di partner risorse deve creare provider di attestazioni per ogni organizzazione partner account che si desidera considerare attendibile.  
  
Questo elenco di controllo include attività per la distribuzione di Active Directory Federation Services \(ADFS\) nell'organizzazione partner account. Include inoltre le attività per la configurazione dei componenti necessari per stabilire una relazione molti\-metà di una relazione di federazione.  
  
Se si distribuisce un [Progettazione di Web SSO](https://technet.microsoft.com/library/dd807033.aspx), non è necessario seguire questo elenco di controllo. Tuttavia, è necessario completare le attività nell'elenco di controllo per distribuire correttamente un [Progettazione di Web SSO federato](https://technet.microsoft.com/library/dd807050.aspx).  
  
> [!IMPORTANT]  
> Assicurarsi che l'amministratore dell'organizzazione partner risorse segue le indicazioni fornite in [elenco di controllo: Configurazione dell'organizzazione Partner risorse](Checklist--Configuring-the-Resource-Partner-Organization.md) per garantire che verranno completate tutte le attività di distribuzione necessari per creare la seconda metà della relazione di federazione.  
  
> [!NOTE]  
> Completare le attività dell'elenco di controllo nell'ordine indicato. Quando un collegamento a un riferimento porta a una procedura, tornare a questo argomento una volta completati i passaggi della procedura, in modo da poter procedere con le operazioni rimanenti nell'elenco di controllo.  
  
![configurare l'organizzazione partner account](media/2b05dce3-938f-4168-9b8f-1f4398cbdb9b.gif)**elenco di controllo: Configurazione dell'organizzazione partner account**  
  
||Attività|Riferimenti|  
|-|--------|-------------|  
|![configurare l'organizzazione partner account](media/icon_checkboxo.gif)|Se si dispone oggi una distribuzione di ADFS 1.0 o 1.1 esistente nell'ambiente di produzione, vedere il collegamento a destra per informazioni su come eseguire la migrazione delle impostazioni dal servizio federativo corrente a un nuovo servizio federativo di AD FS. Se si distribuisce ADFS per la prima volta nell'organizzazione tramite ADFS, è possibile ignorare questo passaggio e continuare per l'attività successiva nell'elenco di controllo per informazioni su come configurare una nuova organizzazione partner account.|![configurare l'organizzazione partner account](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[pianifica una migrazione a AD FS 2.0](https://technet.microsoft.com/library/ff678044.aspx)|  
|![configurare l'organizzazione partner account](media/icon_checkboxo.gif)|In base agli obiettivi di distribuzione, esaminare le informazioni sui componenti necessari per fornire agli utenti l'accesso alle applicazioni federate.|![configurare l'organizzazione partner account](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[Your Claims-Aware Applications e servizi di fornire l'accesso di Active Directory gli utenti](https://technet.microsoft.com/library/dd807071.aspx)<br /><br />![configurare l'organizzazione partner account](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[alle applicazioni e servizi di altre organizzazioni forniscono l'accesso di Active Directory gli utenti](https://technet.microsoft.com/library/dd807123.aspx)<br /><br />![configurare l'organizzazione partner account](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[fornire agli utenti in un'altra organizzazione l'accesso a servizi e Your Claims-Aware Applications](https://technet.microsoft.com/library/dd807099.aspx)|  
|![configurare l'organizzazione partner account](media/icon_checkboxo.gif)|Determinare quale progettazione ADFS questa organizzazione partner account verranno associati.|![configurare l'organizzazione partner account](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[progettazione di Web SSO](https://technet.microsoft.com/library/dd807033.aspx)<br /><br />![configurare l'organizzazione partner account](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[Federated Web SSO Design](https://technet.microsoft.com/library/dd807050.aspx)|  
|![configurare l'organizzazione partner account](media/icon_checkboxo.gif)|Prima di iniziare la distribuzione dei server ADFS, esaminare il; 1.\) vantaggi e svantaggi della scelta di uno dei Database interno di Windows \(WID\) o SQL Server per archiviare il database di configurazione di ADFS 2.\) Tipi di topologia di distribuzione AD FS e i suggerimenti di layout posizionamento e la rete di server associato.|![configurare l'organizzazione partner account](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[determinare la topologia di distribuzione di AD FS](https://technet.microsoft.com/library/gg982491.aspx)<br /><br />![configurare l'organizzazione partner account](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[considerazioni sulla topologia di distribuzione di AD FS](https://technet.microsoft.com/library/gg982489.aspx)|  
|![configurare l'organizzazione partner account](media/icon_checkboxo.gif)|Esaminare la capacità di ADFS sulla pianificazione per determinare il numero corretto di server federativo e server proxy server federativo che è necessario utilizzare nell'ambiente di produzione.|![configurare l'organizzazione partner account](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[pianificazione della capacità dei Server AD FS](https://technet.microsoft.com/library/gg749899.aspx)|  
|![configurare l'organizzazione partner account](media/icon_checkboxo.gif)|Per pianificare e implementare la topologia fisica per la distribuzione di partner account, determinare se la progettazione di ADFS richiede uno o più server federativi o server federativi.|![configurare l'organizzazione partner account](media/bc6cea1a-1c6c-4124-8c8f-1df5adfe8c88.gif)[elenco di controllo: Configurazione di un server federativo](Checklist--Setting-Up-a-Federation-Server.md)<br /><br />![configurare l'organizzazione partner account](media/bc6cea1a-1c6c-4124-8c8f-1df5adfe8c88.gif)[elenco di controllo: Configurazione di un proxy server federativo](Checklist--Setting-Up-a-Federation-Server-Proxy.md)|  
|![configurare l'organizzazione partner account](media/icon_checkboxo.gif)|Determinare il tipo di archivio di attributi che si desidera aggiungere ad ADFS. Quindi, aggiungere l'archivio di attributi utilizzando lo snap-gestione di ADFS\-in.|![configurare l'organizzazione partner account](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[il ruolo di archivi di attributi](../../ad-fs/technical-reference/The-Role-of-Attribute-Stores.md)<br /><br />![configurare l'organizzazione partner account](media/15dd35b6-6cc6-421f-93f8-7109920e7144.gif)[aggiungere un attributo di Store](../../ad-fs/operations/Add-an-Attribute-Store.md)|  
|![configurare l'organizzazione partner account](media/icon_checkboxo.gif)|Se è necessario inviare attestazioni o utilizzare le attestazioni da un partner risorse che utilizzano entrambi un servizio federativo ADFS 1.0 o 1.1, vedere il collegamento a destra per informazioni su come configurare ADFS per interagire con le versioni precedenti di AD FS. Se l'organizzazione partner risorse inoltre utilizza ADFS per l'invio o utilizzare le attestazioni per l'organizzazione, è possibile ignorare questo passaggio e continuare con l'attività successiva nell'elenco di controllo.|![configurare l'organizzazione partner account](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[pianificazione dell'interoperabilità con AD FS 1.x](https://technet.microsoft.com/library/ff678040.aspx)|  
|![configurare l'organizzazione partner account](media/icon_checkboxo.gif)|Dopo aver distribuito il primo server federativo nell'organizzazione partner account, creare una relazione di trust della relying party utilizzando lo snap-gestione di ADFS\-in. Per creare un'attendibilità della relying party, è possibile immettere manualmente i dati su un partner risorse oppure utilizzare un URL dei metadati federativi fornito dall'amministratore dell'organizzazione del partner risorse. È possibile utilizzare i metadati federativi per recuperare automaticamente i dati per il partner risorse. **Nota:** Se il partner risorse pubblica i metadati federativi o può fornire una copia su file di tali metadati, è consigliabile optare per il recupero automatico dei dati per risparmiare tempo.|![configurare l'organizzazione partner account](media/15dd35b6-6cc6-421f-93f8-7109920e7144.gif)[creare una Relying Party Trust manualmente](../../ad-fs/operations/Create-a-Relying-Party-Trust.md)<br /><br />![configurare l'organizzazione partner account](media/15dd35b6-6cc6-421f-93f8-7109920e7144.gif)[creare una Relying Party Trust usando metadati federativi](../../ad-fs/operations/Create-a-Relying-Party-Trust.md)|  
|![configurare l'organizzazione partner account](media/icon_checkboxo.gif)|A seconda delle esigenze dell'organizzazione, creare uno o più regole attestazione imposta per ogni trust della relying party specificato nello snap di gestione di ADFS\-in modo che le attestazioni e verrà generate in modo appropriato.|![configurare l'organizzazione partner account](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[elenco di controllo: Creazione di regole delle attestazioni per un'istanza di attendibilità del componente](Checklist--Creating-Claim-Rules-for-a-Relying-Party-Trust.md)|  
|![configurare l'organizzazione partner account](media/icon_checkboxo.gif)|Una descrizione di attestazione deve essere creata se non ne esiste già che verrà usata per soddisfare le esigenze dell'organizzazione. ADFS è dotato di un set predefinito di descrizioni di attestazioni che vengono esposte nello snap di gestione di ADFS\-in.|![configurare l'organizzazione partner account](media/15dd35b6-6cc6-421f-93f8-7109920e7144.gif)[aggiungere una descrizione di attestazione](../../ad-fs/operations/Add-a-Claim-Description.md)|  
|![configurare l'organizzazione partner account](media/icon_checkboxo.gif)|Determinare se l'organizzazione sarà necessario utilizzare la delega dell'identità per autorizzare o limitare un account specificato per "agire come" o rappresentare altri utenti. Questo è spesso un requisito quando anteriore\-Web applicazioni devono interagire con back end\-terminare i servizi Web.|![configurare l'organizzazione partner account](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[quando to Use Identity Delegation](https://technet.microsoft.com/library/dd807122.aspx)|  
|![configurare l'organizzazione partner account](media/icon_checkboxo.gif)|Preparazione dei computer client per la federazione da:<br /><br />-Aggiunta dell'URL per il server federativo del partner account all'elenco siti attendibili per il browser client.<br />-Gruppo utilizzo di criteri per effettuare il push Secure Sockets Layer appropriato \(SSL\) certificati ai computer client.|![configurare l'organizzazione partner account](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[preparazione dei computer Client nel Partner Account](https://technet.microsoft.com/library/dd807114(v=ws.11).aspx)<br /><br />![configurare l'organizzazione partner account](media/15dd35b6-6cc6-421f-93f8-7109920e7144.gif)[Configure Client Computers to Trust the Account Federation Server](Configure-Client-Computers-to-Trust-the-Account-Federation-Server.md)<br /><br />![configurare l'organizzazione partner account](media/15dd35b6-6cc6-421f-93f8-7109920e7144.gif)[distribuire i certificati ai computer Client mediante criteri di gruppo](Distribute-Certificates-to-Client-Computers-by-Using-Group-Policy.md)| 