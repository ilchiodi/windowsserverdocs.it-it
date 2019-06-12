---
ms.assetid: fe05e52c-cbf8-428b-8176-63407991042f
title: Gestione avanzata della topologia e della replica di Active Directory mediante Windows PowerShell (Livello 200)
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: d56bc89189c3b17367549aeb076633a6ea0e1007
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/31/2019
ms.locfileid: "66442738"
---
# <a name="advanced-active-directory-replication-and-topology-management-using-windows-powershell-level-200"></a>Gestione avanzata della topologia e della replica di Active Directory mediante Windows PowerShell (Livello 200)

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

In questo argomento vengono illustrati in dettaglio i nuovi cmdlet di gestione della replica e della topologia di Servizi di dominio Active Directory e vengono forniti esempi aggiuntivi. Per un'introduzione, vedere [Introduzione alla replica di Active Directory e della topologia Management mediante Windows PowerShell &#40;livello 100&#41;](../../../ad-ds/manage/powershell/Introduction-to-Active-Directory-Replication-and-Topology-Management-Using-Windows-PowerShell--Level-100-.md).  
  
1.  [Introduzione](../../../ad-ds/manage/powershell/Advanced-Active-Directory-Replication-and-Topology-Management-Using-Windows-PowerShell--Level-200-.md#BKMK_Intro)  
  
2.  [Replica e metadati](../../../ad-ds/manage/powershell/Advanced-Active-Directory-Replication-and-Topology-Management-Using-Windows-PowerShell--Level-200-.md#BKMK_Repl)  
  
3.  [Get-ADReplicationAttributeMetadata](../../../ad-ds/manage/powershell/Advanced-Active-Directory-Replication-and-Topology-Management-Using-Windows-PowerShell--Level-200-.md#BKMK_ReplAttrMD)  
  
4.  [Get-ADReplicationPartnerMetadata](../../../ad-ds/manage/powershell/Advanced-Active-Directory-Replication-and-Topology-Management-Using-Windows-PowerShell--Level-200-.md#BKMK_PartnerMD)  
  
5.  [Get-ADReplicationFailure](../../../ad-ds/manage/powershell/Advanced-Active-Directory-Replication-and-Topology-Management-Using-Windows-PowerShell--Level-200-.md#BKMK_ReplFail)  
  
6.  [Get-ADReplicationQueueOperation and Get-ADReplicationUpToDatenessVectorTable](../../../ad-ds/manage/powershell/Advanced-Active-Directory-Replication-and-Topology-Management-Using-Windows-PowerShell--Level-200-.md#BKMK_ReplQueue)  
  
7.  [Sync-ADObject](../../../ad-ds/manage/powershell/Advanced-Active-Directory-Replication-and-Topology-Management-Using-Windows-PowerShell--Level-200-.md#BKMK_Sync)  
  
8.  [Topologia](../../../ad-ds/manage/powershell/Advanced-Active-Directory-Replication-and-Topology-Management-Using-Windows-PowerShell--Level-200-.md#BKMK_Topo)  
  
## <a name="BKMK_Intro"></a>Introduzione  
Windows Server 2012 estende il modulo Active Directory per Windows PowerShell con 25 nuovi cmdlet che consentono di gestire la replica e la topologia della foresta. Prima di questa operazione, si era costretti a usare il tipo generico  **\*- Active Directoryobject** sostantivi o chiamare funzioni .NET.  
  
Come tutti i cmdlet di Windows PowerShell per Active Directory, questa nuova funzionalità richiede l'installazione del [servizio Gateway di gestione di Active Directory](https://www.microsoft.com/download/details.aspx?displaylang=en&id=2852) in almeno un controller di dominio (preferibilmente in tutti).  
  
Nella tabella seguente sono elencati tutti i nuovi cmdlet di replica e topologia aggiunti al modulo di Windows PowerShell per Active Directory.  
  
|||  
|-|-|  
|Cmdlet|Spiegazione|  
|Get-ADReplicationAttributeMetadata|Restituisce i metadati di replica dell'attributo per un oggetto|  
|Get-ADReplicationConnection|Restituisce i dettagli relativi all'oggetto connessione del controller di dominio|  
|Get-ADReplicationFailure|Restituisce l'errore di replica più recente per un controller di dominio|  
|Get-ADReplicationPartnerMetadata|Restituisce la configurazione di replica di un controller di dominio|  
|Get-ADReplicationQueueOperation|Restituisce il backlog della coda della replica corrente|  
|Get-ADReplicationSite|Restituisce le informazioni sul sito|  
|Get-ADReplicationSiteLink|Restituisce le informazioni relative al collegamento di sito|  
|Get-ADReplicationSiteLinkBridge|Restituisce le informazioni relative al ponte di collegamenti di sito|  
|Get-ADReplicationSubnet|Restituisce le informazioni relative alla subnet di Active Directory|  
|Get-ADReplicationUpToDatenessVectorTable|Restituisce il vettore di aggiornamento per un controller di dominio|  
|Get-ADTrust|Restituisce informazioni relative al trust tra domini o tra foreste|  
|New-ADReplicationSite|Crea un nuovo sito|  
|New-ADReplicationSiteLink|Crea un nuovo collegamento di sito|  
|New-ADReplicationSiteLinkBridge|Crea un nuovo ponte di collegamenti di sito|  
|New-ADReplicationSubnet|Crea una nuova subnet di Active Directory|  
|Remove-ADReplicationSite|Elimina un sito|  
|Remove-ADReplicationSiteLink|Elimina un collegamento di sito|  
|Remove-ADReplicationSiteLinkBridge|Elimina un ponte di collegamenti di sito|  
|Remove-ADReplicationSubnet|Elimina una subnet di Active Directory|  
|Set-ADReplicationConnection|Modifica una connessione|  
|Set-ADReplicationSite|Modifica un sito|  
|Set-ADReplicationSiteLink|Modifica un collegamento di sito|  
|Set-ADReplicationSiteLinkBridge|Modifica un ponte di collegamenti di sito|  
|Set-ADReplicationSubnet|Modifica una subnet di Active Directory|  
|Sync-ADObject|Forza la replica di un oggetto singolo|  
  
La maggior parte di questi cmdlet si basa su Repadmin.exe. Altri cmdlet (non riportati nell'elenco) gestiscono funzionalità quali il controllo dinamico degli accessi e gli account del servizio di gruppo gestiti.  
  
Per un elenco completo di tutti i cmdlet di Windows PowerShell per Active Directory, eseguire:  
  
```  
Get-command -module ActiveDirectory  
```  
  
Per un elenco completo di tutti gli argomenti dei cmdlet di Windows PowerShell per Active Directory, fare riferimento alla guida. Ad esempio:  
  
```  
Get-help New-ADReplicationSite  
  
```  
  
Usare il cmdlet `Update-Help` per scaricare e installare i file della guida  
  
### <a name="BKMK_Repl"></a>Replica e metadati  
Repadmin.exe convalida l'integrità e la coerenza della replica di Active Directory. Repadmin.exe offre opzioni di modifica dei dati semplificate, ad esempio alcuni argomenti supportano gli output in formato CSV, ma l'automazione richiede in genere l'analisi tramite output di file di testo. Il modulo Active Directory per Windows PowerShell è il primo tentativo di offerta di un'opzione che consente un controllo reale sui dati restituiti. In precedenza, era necessario creare script o usare strumenti di terze parti.  
  
Inoltre, i cmdlet seguenti implementano un nuovo set di parametri di **Target**, **Scope** e **EnumerationServer**:  
  
-   **Get-ADReplicationFailure**  
  
-   **Get-ADReplicationPartnerMetadata**  
  
-   **Get-ADReplicationUpToDatenessVectorTable**  
  
L'argomento **Target** accetta un elenco separato da virgole di stringhe che identificano i server, i siti, i domini o le foreste di destinazione specificati dall'argomento **Scope** . Un asterisco (\*) è consentito anche e significa che tutti i server all'interno dell'ambito specificato. Se viene specificato nessun ambito, indica tutti i server nella foresta dell'utente corrente. L'argomento **Scope** specifica la latitudine della ricerca. I valori accettabili sono **Server**, **Site**, **Domain** e **Forest**. EnumerationServer **specifica il server che enumera l'elenco di controller di dominio specificati in** Target **e** Scope **.** Funziona come l'argomento **Server** e richiede che nel server specificato sia in esecuzione Servizi Web Active Directory.  
  
Per illustrare i nuovi cmdlet, di seguito vengono forniti alcuni scenari di esempio che mostrano le funzionalità impossibili da eseguire con repadmin.exe. Grazie a queste illustrazioni, le possibilità amministrative diventano ovvie. Per informazioni sui requisiti di utilizzo specifici, consultare la guida relativa ai cmdlet.  
  
### <a name="BKMK_ReplAttrMD"></a>Get-ADReplicationAttributeMetadata  
Questo cmdlet è simile a **repadmin.exe /showobjmeta**. Consente di restituire i metadati della replica, ad esempio la data di modifica di un attributo, il controller di dominio di origine, le informazioni sulla versione e sul numero di sequenza di aggiornamento (USN) e i dati dell'attributo. Questo cmdlet è utile per controllare dove e quando si è verificata una modifica.  
  
Diversamente da Repadmin, Windows PowerShell offre opzioni di ricerca e output flessibili. Ad esempio, è possibile eseguire l'output dei metadati dell'oggetto Domain Admins, ordinato come un elenco leggibile:  
  
```  
Get-ADReplicationAttributeMetadata -object "cn=domain admins,cn=users,dc=corp,dc=contoso,dc=com" -server dc1.corp.contoso.com -showalllinkedvalues | format-list  
  
```  
  
![Gestione avanzata con powershell](media/Advanced-Active-Directory-Replication-and-Topology-Management-Using-Windows-PowerShell--Level-200-/ADDS_PSGetADReplAttrMd.png)  
  
In alternativa, è possibile disporre i dati in una tabella come in repadmin:  
  
```  
Get-ADReplicationAttributeMetadata -object "cn=domain admins,cn=users,dc=corp,dc=contoso,dc=com" -server dc1.corp.contoso.com -showalllinkedvalues | format-table -wrap  
  
```  
  
![Gestione avanzata con powershell](media/Advanced-Active-Directory-Replication-and-Topology-Management-Using-Windows-PowerShell--Level-200-/ADDS_PSGetADReplAttrMdTable.png)  
  
Oppure, è possibile ottenere i metadati per un'intera classe di oggetti, canalizzando il cmdlet **Get-Adobject** con un filtro, ad esempio "tutti i gruppi" e quindi combinando i risultati con una data specifica. La pipeline è un canale usato tra più cmdlet per passare i dati. Per visualizzare tutti i gruppi modificati il giorno 13 gennaio 2012:  
  
```  
get-adobject -filter 'objectclass -eq "group"' | Get-ADReplicationAttributeMetadata -server dc1.corp.contoso.com | where-object {$_.lastoriginatingchangetime -like "*1/13/2012*" -and $_.attributename -eq "name"} | format-table object  
```  
  
![Gestione avanzata con powershell](media/Advanced-Active-Directory-Replication-and-Topology-Management-Using-Windows-PowerShell--Level-200-/ADDS_PSGetADReplAttrMdClass.png)  
  
Per altre informazioni su ulteriori operazioni di Windows PowerShell con le pipeline, vedere [Piping e pipeline in Windows PowerShell](https://technet.microsoft.com/library/ee176927.aspx).  
  
In alternativa, per trovare ogni gruppo in cui è incluso il membro Tony Wang e la data dell'ultima modifica del gruppo:  
  
```  
get-adobject -filter 'objectclass -eq "group"' | Get-ADReplicationAttributeMetadata -server dc1.corp.contoso.com -showalllinkedvalues | where-object {$_.attributevalue -like "*tony wang*"} | format-table object,LastOriginatingChangeTime,version -auto  
  
```  
  
![Gestione avanzata con powershell](media/Advanced-Active-Directory-Replication-and-Topology-Management-Using-Windows-PowerShell--Level-200-/ADDS_PSGetADReplAttrMdFilter.png)  
  
Per trovare tutti gli oggetti ripristinati in modo autorevole tramite un backup dello stato del sistema nel dominio, in base alla relativa versione artificialmente elevata:  
  
```  
get-adobject -filter 'objectclass -like "*"' | Get-ADReplicationAttributeMetadata -server dc1.corp.contoso.com | where-object {$_.version -gt "100000" -and $_.attributename -eq "name"} | format-table object,LastOriginatingChangeTime  
```  
  
![Gestione avanzata con powershell](media/Advanced-Active-Directory-Replication-and-Topology-Management-Using-Windows-PowerShell--Level-200-/ADDS_PSGetADReplAttrMdFilter2.png)  
  
È anche possibile inviare tutti i metadati utente a un file CSV per l'analisi successiva in Microsoft Excel:  
  
```  
get-adobject -filter 'objectclass -eq "user"' | Get-ADReplicationAttributeMetadata -server dc1.corp.contoso.com -showalllinkedvalues | export-csv allgroupmetadata.csv  
```  
  
### <a name="BKMK_PartnerMD"></a>Get-ADReplicationPartnerMetadata  
Questo cmdlet restituisce informazioni sulla configurazione e lo stato della replica per un controller di dominio per il monitoraggio, l'inventario o la risoluzione dei problemi. Diversamente da Repadmin.exe, quando si usa Windows PowerShell vengono visualizzati solo i dati ritenuti importanti, nel formato desiderato.  
  
Ad esempio, per ottenere lo stato della replica leggibile di un singolo controller di dominio:  
  
```  
Get-ADReplicationPartnerMetadata -target dc1.corp.contoso.com  
```  
  
![Gestione avanzata con powershell](media/Advanced-Active-Directory-Replication-and-Topology-Management-Using-Windows-PowerShell--Level-200-/ADDS_PSGetADReplPartnerMd.png)  
  
Oppure, l'ultima volta in cui un controller di dominio ha replicato i dati in ingresso e i relativi partner, in formato tabella.  
  
```  
Get-ADReplicationPartnerMetadata -target dc1.corp.contoso.com | format-table lastreplicationattempt,lastreplicationresult,partner -auto  
```  
  
![Gestione avanzata con powershell](media/Advanced-Active-Directory-Replication-and-Topology-Management-Using-Windows-PowerShell--Level-200-/ADDS_PSGetADReplPartnerMdTable.png)  
  
È anche possibile contattare tutti i controller di dominio nella foresta e visualizzare gli ultimi tentativi di replica non riusciti, per qualsiasi motivo:  
  
```  
Get-ADReplicationPartnerMetadata -target * -scope server | where {$_.lastreplicationresult -ne "0"} | ft server,lastreplicationattempt,lastreplicationresult,partner -auto  
  
```  
  
![Gestione avanzata con powershell](media/Advanced-Active-Directory-Replication-and-Topology-Management-Using-Windows-PowerShell--Level-200-/ADDS_PSGetADReplPartnerMdFail.png)  
  
### <a name="BKMK_ReplFail"></a>Get-ADReplicationFailure  
Questo cmdlet può essere usato per restituire informazioni sugli errori recenti nella replica. Corrisponde a **Repadmin.exe /showreplsum** ma, grazie a Windows PowerShell, offre ancora maggiore controllo.  
  
È ad esempio possibile restituire gli errori più recenti di un controller di dominio e i partner che non è riuscito a contattare:  
  
```  
Get-ADReplicationFailure dc1.corp.contoso.com  
```  
  
![Gestione avanzata con powershell](media/Advanced-Active-Directory-Replication-and-Topology-Management-Using-Windows-PowerShell--Level-200-/ADDS_PSGetADReplFail.png)  
  
È anche possibile restituire una visualizzazione tabella per tutti i server in un sito logico di Active Directory specifico, ordinata in modo da agevolare la visualizzazione e contenente solo i dati più critici:  
  
```  
Get-ADReplicationFailure -scope site -target default-first-site-name | format-table server,firstfailuretime,failurecount,lasterror,partner -auto  
  
```  
  
![Gestione avanzata con powershell](media/Advanced-Active-Directory-Replication-and-Topology-Management-Using-Windows-PowerShell--Level-200-/ADDS_PSGetADReplFailScoped.png)  
  
### <a name="BKMK_ReplQueue"></a>Get-ADReplicationQueueOperation and Get-ADReplicationUpToDatenessVectorTable  
Entrambi questi cmdlet restituiscono ulteriori aspetti dell'"aggiornamento" del controller di dominio, tra cui le informazioni sul vettore di versione e sulla replica in attesa.  
  
### <a name="BKMK_Sync"></a>Sync-ADObject  
Questo cmdlet corrisponde all'esecuzione di **Repadmin.exe /replsingleobject**. Risulta utile quando si apportano modifiche che richiedono la replica fuori banda, in particolare per correggere un problema.  
  
Se ad esempio l'account utente del CEO è stato eliminato e quindi ripristinato dal Cestino di Active Directory, si vorrà replicarlo immediatamente in tutti i controller di dominio. È inoltre opportuno che questa operazione venga eseguita senza forzare la replica di tutte le altre modifiche di oggetti per non sovraccaricare i collegamenti WAN, motivo per cui esiste una pianificazione di replica.  
  
```  
Get-ADDomainController -filter * | foreach {Sync-ADObject -object "cn=tony wang,cn=users,dc=corp,dc=contoso,dc=com" -source dc1 -destination $_.hostname}  
  
```  
  
![Gestione avanzata con powershell](media/Advanced-Active-Directory-Replication-and-Topology-Management-Using-Windows-PowerShell--Level-200-/ADDS_PSSyncAD.png)  
  
### <a name="BKMK_Topo"></a>Topologia  
Mentre Repadmin.exe funziona bene per restituire le informazioni relative alla topologia di replica, quali siti, collegamenti di siti, ponti di collegamenti di siti e connessioni, non offre un set completo di argomenti per apportare modifiche. Infatti, non è mai stata disponibile un'utilità di Windows inclusa gestibile tramite script dedicata agli amministratori per la creazione e la modifica della topologia di Servizi di dominio Active Directory. Poiché Active Directory si è sviluppato in milioni di ambienti cliente, l'esigenza di modificare in blocco le informazioni logiche di Active Directory diventa evidente.  
  
Ad esempio, dopo l'espansione rapida di nuove succursali, in combinazione con il consolidamento delle altre, potrebbe essere necessario implementare centinaia di modifiche del sito in base alla sede fisica, alle modifiche alla rete e ai nuovi requisiti in termini di capacità. Invece di usare Dssites.msc e Adsiedit.msc per apportare le modifiche, è possibile automatizzare le operazioni. Questo è particolarmente utile quando si inizia con un foglio di calcolo di dati fornito dai team della rete e delle strutture.  
  
Il **Get-Adreplication\\** * cmdlet restituiscono informazioni sulla topologia di replica e sono utili per la canalizzazione nel **Set-Adreplication\\** * cmdlet in bulk. **Ottenere** i cmdlet di non modificano i dati, li mostrano solo o di Windows PowerShell di creare gli oggetti di sessione che possono essere canalizzati **Set-Adreplication\\** * cmdlet. I cmdlet **New** e **Remove** sono utili per creare o rimuovere oggetti della topologia di Active Directory.  
  
È ad esempio possibile creare nuovi siti tramite un file CSV:  
  
```  
import-csv -path C:\newsites.csv | new-adreplicationsite  
```  
  
![Gestione avanzata con powershell](media/Advanced-Active-Directory-Replication-and-Topology-Management-Using-Windows-PowerShell--Level-200-/ADDS_PSNewSitesCSV.png)  
  
![Gestione avanzata con powershell](media/Advanced-Active-Directory-Replication-and-Topology-Management-Using-Windows-PowerShell--Level-200-/ADDS_PSImportCSV.png)  
  
In alternativa, è possibile creare un nuovo collegamento di sito tra due siti esistenti con un intervallo di replica e un costo del sito personalizzati:  
  
```  
new-adreplicationsitelink -name "chicago<-->waukegan" -sitesincluded chicago,waukegan -cost 50 -replicationfrequencyinminutes 15  
```  
  
![Gestione avanzata con powershell](media/Advanced-Active-Directory-Replication-and-Topology-Management-Using-Windows-PowerShell--Level-200-/ADDS_PSNewADReplSite.png)  
  
Oppure, è possibile trovare ogni sito nella foresta e sostituire i relativi attributi **Options** con il flag per abilitare la notifica delle modifiche tra siti per replicare alla velocità massima con compressione:  
  
```  
get-adreplicationsitelink -filter * | set-adobject -replace @{options=$($_.options -bor 1)}  
```  
  
![Gestione avanzata con powershell](media/Advanced-Active-Directory-Replication-and-Topology-Management-Using-Windows-PowerShell--Level-200-/ADDS_PSNewADReplSiteLink.gif)  
  
> [!IMPORTANT]  
> Impostare **-bor 5** per disabilitare la compressione anche su tali collegamenti di sito.  
  
È anche possibile trovare tutti i siti a cui non sono assegnate subnet, per riconciliare l'elenco con le subnet effettive di tali percorsi:  
  
```  
get-adreplicationsite -filter * -property subnets | where-object {!$_.subnets -eq "*"} | format-table name  
```  
  
![Gestione avanzata con powershell](media/Advanced-Active-Directory-Replication-and-Topology-Management-Using-Windows-PowerShell--Level-200-/ADDS_PSNewADReplSiteFiltrer.png)  
  
## <a name="see-also"></a>Vedere anche  
[Introduzione alla replica di Active Directory e gestione della topologia mediante Windows PowerShell &#40;livello 100&#41;](../../../ad-ds/manage/powershell/Introduction-to-Active-Directory-Replication-and-Topology-Management-Using-Windows-PowerShell--Level-100-.md)  
  

