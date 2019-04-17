---
ms.assetid: fe05e52c-cbf8-428b-8176-63407991042f
title: Replica di Active Directory e gestione della topologia mediante Windows PowerShell (livello 200) avanzati
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 1e05616b4b594ae54fcaa3ec6496c0917ecde38b
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/12/2017
---
# <a name="advanced-active-directory-replication-and-topology-management-using-windows-powershell-level-200"></a>Replica di Active Directory e gestione della topologia mediante Windows PowerShell (livello 200) avanzati

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

In questo argomento vengono illustrati i nuovi replica di Active Directory e i cmdlet di gestione della topologia in modo più dettagliato e vengono forniti esempi aggiuntivi. Per un'introduzione, vedere [Introduzione alla replica di Active Directory e gestione della topologia mediante Windows PowerShell & #40; Livello 100 & #41; ](../../../ad-ds/manage/powershell/Introduction-to-Active-Directory-Replication-and-Topology-Management-Using-Windows-PowerShell--Level-100-.md).  
  
1.  [Introduzione](../../../ad-ds/manage/powershell/Advanced-Active-Directory-Replication-and-Topology-Management-Using-Windows-PowerShell--Level-200-.md#BKMK_Intro)  
  
2.  [Replica e metadati](../../../ad-ds/manage/powershell/Advanced-Active-Directory-Replication-and-Topology-Management-Using-Windows-PowerShell--Level-200-.md#BKMK_Repl)  
  
3.  [Get-ADReplicationAttributeMetadata](../../../ad-ds/manage/powershell/Advanced-Active-Directory-Replication-and-Topology-Management-Using-Windows-PowerShell--Level-200-.md#BKMK_ReplAttrMD)  
  
4.  [Get-ADReplicationPartnerMetadata](../../../ad-ds/manage/powershell/Advanced-Active-Directory-Replication-and-Topology-Management-Using-Windows-PowerShell--Level-200-.md#BKMK_PartnerMD)  
  
5.  [Get-ADReplicationFailure](../../../ad-ds/manage/powershell/Advanced-Active-Directory-Replication-and-Topology-Management-Using-Windows-PowerShell--Level-200-.md#BKMK_ReplFail)  
  
6.  [Get-ADReplicationQueueOperation e Get-ADReplicationUpToDatenessVectorTable](../../../ad-ds/manage/powershell/Advanced-Active-Directory-Replication-and-Topology-Management-Using-Windows-PowerShell--Level-200-.md#BKMK_ReplQueue)  
  
7.  [Sincronizzazione-ADObject](../../../ad-ds/manage/powershell/Advanced-Active-Directory-Replication-and-Topology-Management-Using-Windows-PowerShell--Level-200-.md#BKMK_Sync)  
  
8.  [Topologia](../../../ad-ds/manage/powershell/Advanced-Active-Directory-Replication-and-Topology-Management-Using-Windows-PowerShell--Level-200-.md#BKMK_Topo)  
  
## <a name="BKMK_Intro"></a>Introduzione  
Windows Server 2012 estende il modulo di Active Directory per Windows PowerShell con 25 nuovi cmdlet per gestire la replica e topologia della foresta. Precedenza, era necessario utilizzare il metodo generico **\*-AdObject** sostantivi o chiamare funzioni .NET.  
  
Come tutti i cmdlet di Windows PowerShell per Active Directory, questa nuova funzionalità richiede l'installazione di [servizio Gateway di gestione di Active Directory](https://www.microsoft.com/download/details.aspx?displaylang=en&id=2852) in almeno un controller di dominio (e preferibilmente, tutti i controller di dominio).  
  
Nella tabella seguente sono elencati i nuovi cmdlet di replica e topologia aggiunti al modulo Windows PowerShell per Active Directory.  
  
|||  
|-|-|  
|Cmdlet|Spiegazione|  
|Get-ADReplicationAttributeMetadata|Restituisce i metadati di replica per un oggetto dell'attributo|  
|Get-ADReplicationConnection|Restituisce informazioni dettagliate sugli oggetti di connessione al controller di dominio|  
|Get-ADReplicationFailure|Restituisce più recenti errore di replica di un controller di dominio|  
|Get-ADReplicationPartnerMetadata|Restituisce la configurazione della replica di un controller di dominio|  
|Get-ADReplicationQueueOperation|Restituisce il backlog della coda della replica corrente|  
|Get-ADReplicationSite|Restituisce le informazioni sul sito|  
|Get-ADReplicationSiteLink|Restituisce informazioni sul collegamento di sito|  
|Get-ADReplicationSiteLinkBridge|Restituisce le informazioni bridge di collegamenti di sito|  
|Get-ADReplicationSubnet|Restituisce informazioni sulla subnet di Active Directory|  
|Get-ADReplicationUpToDatenessVectorTable|Restituisce il vettore di aggiornamento per un controller di dominio|  
|Get-ADTrust|Restituisce informazioni su una relazione di trust tra domini o tra insiemi di strutture|  
|New-ADReplicationSite|Crea un nuovo sito|  
|Nuovo ADReplicationSiteLink|Crea un nuovo collegamento di sito|  
|Nuovo ADReplicationSiteLinkBridge|Crea un nuovo ponte di collegamenti di sito|  
|Nuovo ADReplicationSubnet|Crea una nuova subnet di Active Directory|  
|Remove-ADReplicationSite|Elimina un sito|  
|Remove-ADReplicationSiteLink|Elimina un collegamento di sito|  
|Remove-ADReplicationSiteLinkBridge|Elimina un ponte di collegamenti di sito|  
|Remove-ADReplicationSubnet|Elimina una subnet di Active Directory|  
|Set-ADReplicationConnection|Modifica una connessione|  
|Set-ADReplicationSite|Modifica un sito|  
|Set-ADReplicationSiteLink|Modifica un collegamento di sito|  
|Set-ADReplicationSiteLinkBridge|Modifica un ponte di collegamenti di sito|  
|Set-ADReplicationSubnet|Modifica una subnet di Active Directory|  
|Sincronizzazione-ADObject|Forza la replica di un singolo oggetto|  
  
La maggior parte di questi cmdlet sono i singoli Repadmin.exe. Altri cmdlet (non elencato) gestiscono funzionalità quali il controllo dinamico degli accessi e account del servizio gestito del gruppo.  
  
Per un elenco completo di tutti i cmdlet di Windows PowerShell per Active Directory, eseguire:  
  
```  
Get-command -module ActiveDirectory  
```  
  
Per un elenco completo di tutti gli argomenti del cmdlet Windows PowerShell per Active Directory, fare riferimento alla Guida. Per esempio:  
  
```  
Get-help New-ADReplicationSite  
  
```  
  
Utilizzare il `Update-Help` cmdlet per scaricare e installare i file della Guida  
  
### <a name="BKMK_Repl"></a>Replica e metadati  
Repadmin.exe convalida l'integrità e la coerenza della replica di Active Directory. Repadmin.exe offre opzioni di modifica dei dati semplice: alcuni argomenti supportano gli output CSV, ad esempio: ma automazione richiede in genere l'analisi tramite output di file di testo. Il modulo Active Directory per Windows PowerShell è il primo tentativo di offerta di un'opzione che consente un controllo reale sui dati restituiti. precedenza, era necessario creare script o utilizzare gli strumenti di terze parti.  
  
Inoltre, i cmdlet seguenti implementano un nuovo set di parametri di **destinazione**, **ambito**, e **EnumerationServer**:  
  
-   **Get-ADReplicationFailure**  
  
-   **Get-ADReplicationPartnerMetadata**  
  
-   **Get-ADReplicationUpToDatenessVectorTable**  
  
Il **destinazione** argomento accetta un elenco delimitato da virgole di stringhe che identificano il server di destinazione, siti, domini o foreste specificate per il **ambito** argomento. Un asterisco (\ *) è consentito anche e indica tutti i server all'interno dell'ambito specificato. Se non viene specificato alcun ambito, indica tutti i server nella foresta dell'utente corrente. Il **ambito** argomento specifica la latitudine della ricerca. I valori accettabili sono **Server**, **sito**, **dominio**, e **foresta**. Il **EnumerationServer** specifica il server che enumera l'elenco di controller di dominio specificato nel **destinazione** e **ambito**. Funziona come il **Server** argomento e richiede il server specificato, eseguire i servizi Web Active Directory.  
  
Per inserire i nuovi cmdlet, ecco alcuni scenari di esempio che mostra funzionalità impossibili da repadmin.exe; Grazie a queste illustrazioni, le possibilità amministrative diventano ovvie. Consultare la Guida del cmdlet per i requisiti di utilizzo specifici.  
  
### <a name="BKMK_ReplAttrMD"></a>Get-ADReplicationAttributeMetadata  
Questo cmdlet è simile a **repadmin.exe /showobjmeta**. Consente di restituire i metadati di replica, ad esempio quando un attributo modificato, il controller di dominio di origine, la versione e informazioni USN e dati dell'attributo. Questo cmdlet è utile per controllare dove e quando si è verificata una modifica.  
  
Diversamente da Repadmin, Windows PowerShell offre ricerca e output flessibili controllare. È possibile, ad esempio, l'output dei metadati dell'oggetto Domain Admins, ordinato come un elenco leggibile:  
  
```  
Get-ADReplicationAttributeMetadata -object "cn=domain admins,cn=users,dc=corp,dc=contoso,dc=com" -server dc1.corp.contoso.com -showalllinkedvalues | format-list  
  
```  
  
![Gestione avanzata con powershell](media/Advanced-Active-Directory-Replication-and-Topology-Management-Using-Windows-PowerShell--Level-200-/ADDS_PSGetADReplAttrMd.png)  
  
In alternativa, è possibile disporre i dati come in repadmin, in una tabella:  
  
```  
Get-ADReplicationAttributeMetadata -object "cn=domain admins,cn=users,dc=corp,dc=contoso,dc=com" -server dc1.corp.contoso.com -showalllinkedvalues | format-table -wrap  
  
```  
  
![Gestione avanzata con powershell](media/Advanced-Active-Directory-Replication-and-Topology-Management-Using-Windows-PowerShell--Level-200-/ADDS_PSGetADReplAttrMdTable.png)  
  
In alternativa, è possibile ottenere i metadati per un'intera classe di oggetti, canalizzando il **Get-Adobject** cmdlet con un filtro, ad esempio tutti i gruppi - quindi combinando i risultati con una data specifica. La pipeline è un canale usato tra più cmdlet per passare i dati. Per visualizzare tutti i gruppi modificati in qualche modo 13 gennaio 2012:  
  
```  
get-adobject -filter 'objectclass -eq "group"' | Get-ADReplicationAttributeMetadata -server dc1.corp.contoso.com | where-object {$_.lastoriginatingchangetime -like "*1/13/2012*" -and $_.attributename -eq "name"} | format-table object  
```  
  
![Gestione avanzata con powershell](media/Advanced-Active-Directory-Replication-and-Topology-Management-Using-Windows-PowerShell--Level-200-/ADDS_PSGetADReplAttrMdClass.png)  
  
Per ulteriori informazioni su ulteriori operazioni di Windows PowerShell con le pipeline, vedere [Piping e Pipeline in Windows PowerShell](https://technet.microsoft.com/library/ee176927.aspx).  
  
In alternativa, per trovare tutti i gruppi di cui è Tony Wang come membro e quando il gruppo dell'ultima modifica:  
  
```  
get-adobject -filter 'objectclass -eq "group"' | Get-ADReplicationAttributeMetadata -server dc1.corp.contoso.com -showalllinkedvalues | where-object {$_.attributevalue -like "*tony wang*"} | format-table object,LastOriginatingChangeTime,version -auto  
  
```  
  
![Gestione avanzata con powershell](media/Advanced-Active-Directory-Replication-and-Topology-Management-Using-Windows-PowerShell--Level-200-/ADDS_PSGetADReplAttrMdFilter.png)  
  
In alternativa, per trovare tutti gli oggetti in modo autorevole ripristinato utilizzando un backup dello stato del sistema nel dominio, in base a relativa versione artificialmente elevata:  
  
```  
get-adobject -filter 'objectclass -like "*"' | Get-ADReplicationAttributeMetadata -server dc1.corp.contoso.com | where-object {$_.version -gt "100000" -and $_.attributename -eq "name"} | format-table object,LastOriginatingChangeTime  
```  
  
![Gestione avanzata con powershell](media/Advanced-Active-Directory-Replication-and-Topology-Management-Using-Windows-PowerShell--Level-200-/ADDS_PSGetADReplAttrMdFilter2.png)  
  
In alternativa, è possibile inviare tutti i metadati utente a un file CSV per l'analisi successiva in Microsoft Excel:  
  
```  
get-adobject -filter 'objectclass -eq "user"' | Get-ADReplicationAttributeMetadata -server dc1.corp.contoso.com -showalllinkedvalues | export-csv allgroupmetadata.csv  
```  
  
### <a name="BKMK_PartnerMD"></a>Get-ADReplicationPartnerMetadata  
Questo cmdlet restituisce informazioni sulla configurazione e stato della replica per un controller di dominio, che consente di monitorare, di inventario o risolvere i problemi. Diversamente da Repadmin.exe, con Windows PowerShell significa che viene visualizzato solo i dati che sono importanti per te, nel formato desiderato.  
  
Ad esempio, lo stato di replica leggibile di un singolo controller di dominio:  
  
```  
Get-ADReplicationPartnerMetadata -target dc1.corp.contoso.com  
```  
  
![Gestione avanzata con powershell](media/Advanced-Active-Directory-Replication-and-Topology-Management-Using-Windows-PowerShell--Level-200-/ADDS_PSGetADReplPartnerMd.png)  
  
In alternativa, l'ultima volta in un controller di dominio di replica in ingresso e formattare i partner, in una tabella:  
  
```  
Get-ADReplicationPartnerMetadata -target dc1.corp.contoso.com | format-table lastreplicationattempt,lastreplicationresult,partner -auto  
```  
  
![Gestione avanzata con powershell](media/Advanced-Active-Directory-Replication-and-Topology-Management-Using-Windows-PowerShell--Level-200-/ADDS_PSGetADReplPartnerMdTable.png)  
  
In alternativa, contattare tutti i controller di dominio nella foresta e visualizzare gli ultimi tentativi di replica non riuscito per qualsiasi motivo:  
  
```  
Get-ADReplicationPartnerMetadata -target * -scope server | where {$_.lastreplicationresult -ne "0"} | ft server,lastreplicationattempt,lastreplicationresult,partner -auto  
  
```  
  
![Gestione avanzata con powershell](media/Advanced-Active-Directory-Replication-and-Topology-Management-Using-Windows-PowerShell--Level-200-/ADDS_PSGetADReplPartnerMdFail.png)  
  
### <a name="BKMK_ReplFail"></a>Get-ADReplicationFailure  
Questo cmdlet può essere utilizzato per restituire informazioni sugli errori recenti nella replica. È analogo a **Repadmin.exe /showreplsum**, ma anche maggiore controllo grazie a Windows PowerShell.  
  
Ad esempio, è possibile restituire gli errori più recenti di un controller di dominio e i partner che non è riuscito a contattare:  
  
```  
Get-ADReplicationFailure dc1.corp.contoso.com  
```  
  
![Gestione avanzata con powershell](media/Advanced-Active-Directory-Replication-and-Topology-Management-Using-Windows-PowerShell--Level-200-/ADDS_PSGetADReplFail.png)  
  
In alternativa, è possibile restituire una visualizzazione tabella per tutti i server in un sito logico Active Directory specifico, ordinato in modo da agevolare la visualizzazione e contenente solo i dati più critici:  
  
```  
Get-ADReplicationFailure -scope site -target default-first-site-name | format-table server,firstfailuretime,failurecount,lasterror,partner -auto  
  
```  
  
![Gestione avanzata con powershell](media/Advanced-Active-Directory-Replication-and-Topology-Management-Using-Windows-PowerShell--Level-200-/ADDS_PSGetADReplFailScoped.png)  
  
### <a name="BKMK_ReplQueue"></a>Get-ADReplicationQueueOperation e Get-ADReplicationUpToDatenessVectorTable  
Entrambi questi cmdlet restituiscono ulteriori aspetti di controller di dominio "fino a aggiornamento", che include in attesa di replica e informazioni sul vettore di versione.  
  
### <a name="BKMK_Sync"></a>Sincronizzazione-ADObject  
Questo cmdlet è analogo a eseguire **Repadmin.exe /replsingleobject**. È molto utile quando si apportano modifiche che richiedono la replica fuori banda, in particolare per correggere un problema.  
  
Ad esempio, se un utente eliminato di account utente del CEO e quindi ripristinato il Cestino per Active Directory, si potrebbe essere replicate in tutti i controller di dominio immediatamente. Inoltre consigliabile eseguire questa operazione senza forzare la replica di tutte le altre oggetto modifiche apportate; Dopo tutto, che è il motivo per cui si dispone di una pianificazione della replica di sovraccaricare i collegamenti WAN.  
  
```  
Get-ADDomainController -filter * | foreach {Sync-ADObject -object "cn=tony wang,cn=users,dc=corp,dc=contoso,dc=com" -source dc1 -destination $_.hostname}  
  
```  
  
![Gestione avanzata con powershell](media/Advanced-Active-Directory-Replication-and-Topology-Management-Using-Windows-PowerShell--Level-200-/ADDS_PSSyncAD.png)  
  
### <a name="BKMK_Topo"></a>Topologia  
Mentre Repadmin.exe funziona bene per restituire le informazioni sulla topologia di replica come siti, collegamenti di sito, ponti di collegamenti di sito e le connessioni, non dispone di un insieme esaustivo di argomenti per apportare modifiche. Infatti, non vi è mai stata configurabile tramite script, nella casella utilità di Windows in modo specifico per gli amministratori possono creare e modificare la topologia di dominio Active Directory. Come Active Directory si è sviluppato in milioni di ambienti cliente, la necessità di massa modificare Active Directory diventano evidente informazioni logiche.  
  
Dopo l'espansione rapida di nuove succursali, combinati con il consolidamento delle altre, ad esempio, potrebbe essere cento le modifiche del sito in base alla posizioni fisiche, le modifiche di rete e nuovi requisiti di capacità. Invece di usare Dssites.msc e Adsiedit.msc per apportare modifiche, è possibile automatizzare. Questo è particolarmente utile quando si inizia con un foglio di calcolo di dati fornito dai team di rete e delle strutture.  
  
Il **ottenere-Adreplication\ *** cmdlet restituiscono informazioni sulla topologia di replica e sono utili per la canalizzazione nel **Set-Adreplication\ *** cmdlet in blocco. **Ottenere** cmdlet non modificano i dati, verranno visualizzati solo i dati o sessione di Windows PowerShell di creare oggetti che possono essere canalizzati **Set-Adreplication\ *** cmdlet. Il **New** e **rimuovere** cmdlet sono utili per la creazione o la rimozione di oggetti di topologia di Active Directory.  
  
Ad esempio, è possibile creare nuovi siti tramite un file CSV:  
  
```  
import-csv -path C:\newsites.csv | new-adreplicationsite  
```  
  
![Gestione avanzata con powershell](media/Advanced-Active-Directory-Replication-and-Topology-Management-Using-Windows-PowerShell--Level-200-/ADDS_PSNewSitesCSV.png)  
  
![Gestione avanzata con powershell](media/Advanced-Active-Directory-Replication-and-Topology-Management-Using-Windows-PowerShell--Level-200-/ADDS_PSImportCSV.png)  
  
In alternativa, creare un nuovo collegamento di sito tra due siti esistenti con un costo intervallo e il sito di replica personalizzate:  
  
```  
new-adreplicationsitelink -name "chicago<-->waukegan" -sitesincluded chicago,waukegan -cost 50 -replicationfrequencyinminutes 15  
```  
  
![Gestione avanzata con powershell](media/Advanced-Active-Directory-Replication-and-Topology-Management-Using-Windows-PowerShell--Level-200-/ADDS_PSNewADReplSite.png)  
  
In alternativa, trovare ogni sito nella foresta e sostituire i **opzioni** gli attributi con il flag per abilitare tra siti per replicare alla velocità massima con compressione notifica delle modifiche:  
  
```  
get-adreplicationsitelink -filter * | set-adobject -replace @{options=$($_.options -bor 1)}  
```  
  
![Gestione avanzata con powershell](media/Advanced-Active-Directory-Replication-and-Topology-Management-Using-Windows-PowerShell--Level-200-/ADDS_PSNewADReplSiteLink.gif)  
  
> [!IMPORTANT]  
> Impostare **- bOPPURE 5** per disabilitare la compressione su anche tali collegamenti di sito.  
  
In alternativa, trovare tutti i siti non sono assegnate subnet, per riconciliare l'elenco con le subnet effettive di tali percorsi:  
  
```  
get-adreplicationsite -filter * -property subnets | where-object {!$_.subnets -eq "*"} | format-table name  
```  
  
![Gestione avanzata con powershell](media/Advanced-Active-Directory-Replication-and-Topology-Management-Using-Windows-PowerShell--Level-200-/ADDS_PSNewADReplSiteFiltrer.png)  
  
## <a name="see-also"></a>Vedere anche  
[Introduzione alla replica di Active Directory e gestione della topologia mediante Windows PowerShell & #40; Livello 100 & #41;](../../../ad-ds/manage/powershell/Introduction-to-Active-Directory-Replication-and-Topology-Management-Using-Windows-PowerShell--Level-100-.md)  
  

