---
ms.assetid: 846c3680-b321-47da-8302-18472be42421
title: Distribuire attestazioni nelle foreste (passaggi dimostrativi)
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 3bab7a396061ecae8a187cc6986d6d026a9e4b32
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59830352"
---
# <a name="deploy-claims-across-forests-demonstration-steps"></a>Distribuire attestazioni nelle foreste (passaggi dimostrativi)

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

In questo argomento si affronterà uno scenario di base che illustra come configurare le trasformazioni di attestazioni tra foreste trusted e trusting. Si apprenderà come gli oggetti Criteri di trasformazione delle attestazioni possono essere creati e collegati per la relazione di trust foresta trusting e la foresta trusted. È quindi convaliderà lo scenario.  
  
## <a name="scenario-overview"></a>Panoramica dello scenario  
Adatum Corporation offre servizi finanziari a Contoso, Ltd. Ogni trimestre, Adatum accountants copiare i fogli di calcolo di account in una cartella in un file server che si trova in Contoso, Ltd. È presente un trust bidirezionale configurati da Contoso Adatum. Contoso, Ltd. vuole proteggere la condivisione in modo che solo Adatum dipendenti possono accedere alla condivisione remota.  
  
In questo scenario:  
  
1.  [Configurare i prerequisiti e l'ambiente di test](Deploy-Claims-Across-Forests--Demonstration-Steps-.md#BKMK_1.1)  
  
2.  [Configurare la trasformazione delle attestazioni nella foresta trusted (Adatum)](Deploy-Claims-Across-Forests--Demonstration-Steps-.md#BKMK_3)  
  
3.  [Configurare la trasformazione delle attestazioni nella foresta trusting (Contoso)](Deploy-Claims-Across-Forests--Demonstration-Steps-.md#BKMK_4)  
  
4.  [Convalidare lo scenario.](Deploy-Claims-Across-Forests--Demonstration-Steps-.md#BKMK_5)  
  
## <a name="BKMK_1.1"></a>Configurare i prerequisiti e l'ambiente di test  
La configurazione di test comporta la configurazione di due insiemi di strutture: Adatum Corporation e Contoso, Ltd e avere una relazione di trust bidirezionale tra Contoso e Adatum. "adatum.com" è la foresta trusted e "contoso.com" è la foresta trusting.  
  
Lo scenario di trasformazione delle attestazioni dimostra trasformazione di un'attestazione nella foresta trusted per un'attestazione nella foresta trusting. A tale scopo, è necessario configurare una nuova foresta denominata adatum.com e popolare la foresta con un utente di test con il valore aziendale 'Adatum'. È quindi necessario configurare un trust bidirezionale tra contoso.com e adatum.com.  
  
> [!IMPORTANT]  
> Quando si configurano le foreste di Contoso e Adatum, è necessario assicurarsi che entrambi i domini radice sono al livello di Windows Server 2012 dominio funzionale per la trasformazione delle attestazioni per lavorare.  
  
È necessario impostare quanto segue per l'ambiente lab. Queste procedure sono illustrate in dettaglio in [appendice b: Impostazione dell'ambiente di Test](Appendix-B--Setting-Up-the-Test-Environment.md)  
  
È necessario implementare le procedure seguenti per configurare l'ambiente lab per questo scenario:  
  
1.  [Impostare Adatum come foresta trusted per Contoso](Appendix-B--Setting-Up-the-Test-Environment.md)  
  
2.  [Creare il tipo di attestazione "Company" in Contoso](Appendix-B--Setting-Up-the-Test-Environment.md#BKMK_2.8)  
  
3.  [Abilitare la proprietà di risorsa "Company" in Contoso](Appendix-B--Setting-Up-the-Test-Environment.md#BKMK_2.55)  
  
4.  [Creare la regola di accesso centrale](Appendix-B--Setting-Up-the-Test-Environment.md#BKMK_2.9)  
  
5.  [Creare i criteri di accesso centrale](Appendix-B--Setting-Up-the-Test-Environment.md#BKMK_2.10)  
  
6.  [Pubblicare i nuovi criteri tramite criteri di gruppo](Appendix-B--Setting-Up-the-Test-Environment.md#BKMK_2.11)  
  
7.  [Creare la cartella Earnings sul file server](Appendix-B--Setting-Up-the-Test-Environment.md#BKMK_2.12)  
  
8.  [Impostare la classificazione e applicare i criteri di accesso centrale per la nuova cartella](Appendix-B--Setting-Up-the-Test-Environment.md#BKMK_2.13)  
  
Usare le informazioni seguenti per completare questo scenario:  
  
|Objects|Dettagli|  
|-----------|-----------|  
|Utenti|Jeff Low, Contoso|  
|Attestazioni utente su Contoso e Adatum|ID: ad: / / EST/aziendale: ContosoAdatum,<br /><br />Attributo di origine: azienda<br /><br />Valori suggeriti: Contoso, Adatum **importanti:** È necessario impostare l'ID su "Company" tipo di attestazione in Contoso e Adatum sia lo stesso per la trasformazione delle attestazioni per lavorare.|  
|Regola di accesso centrale in Contoso|AdatumEmployeeAccessRule|  
|Criteri di accesso centrale in Contoso|Criteri di accesso unico adatum|  
|Criteri di trasformazione delle attestazioni in Contoso e Adatum|DenyAllExcept Company|  
|Cartella di file in Contoso|D:\EARNINGS.|  
  
## <a name="BKMK_3"></a>Configurare la trasformazione delle attestazioni nella foresta trusted (Adatum)  
In questo passaggio si crea un criterio di trasformazione in Adatum negare tutte le attestazioni, ad eccezione di "Company" per passare a Contoso.  
  
Il modulo di Active Directory per Windows PowerShell fornisce il **DenyAllExcept** argomento, che elimina tutto, tranne le attestazioni specificate nei criteri di trasformazione.  
  
Per configurare una trasformazione di attestazioni, è necessario creare un criterio di trasformazione delle attestazioni e crea un collegamento tra le foreste trusted e trusting.  
  
### <a name="BKMK_2.2"></a>Creare criteri di trasformazione delle attestazioni in Adatum  
  
##### <a name="to-create-a-transformation-policy-adatum-to-deny-all-claims-except-company"></a>Per creare un criterio di trasformazione Adatum negare tutte le attestazioni, ad eccezione di "Company"  
  
1.  Accedere al controller di dominio, adatum.com come amministratore con la password **pass@word1**.  
  
2.  Aprire un prompt dei comandi con privilegi elevati in Windows PowerShell e digitare quanto segue:  
  
    ```  
    New-ADClaimTransformPolicy `  
    -Description:"Claims transformation policy to deny all claims except Company"`  
    -Name:"DenyAllClaimsExceptCompanyPolicy" `  
    -DenyAllExcept:company `  
    -Server:"adatum.com" `  
  
    ```  
  
### <a name="BKMK_2.3"></a>Impostare un collegamento di trasformazione delle attestazioni sull'oggetto di dominio di trust del Adatum  
In questo passaggio è applicare i criteri di trasformazione delle attestazioni appena creata sull'oggetto di dominio di trust del Adatum per Contoso.  
  
##### <a name="to-apply-the-claims-transformation-policy"></a>Per applicare i criteri di trasformazione delle attestazioni  
  
1.  Accedere al controller di dominio, adatum.com come amministratore con la password **pass@word1**.  
  
2.  Aprire un prompt dei comandi con privilegi elevati in Windows PowerShell e digitare quanto segue:  
  
    ```  
  
      Set-ADClaimTransformLink `  
    -Identity:"contoso.com" `  
    -Policy:"DenyAllClaimsExceptCompanyPolicy" `  
    '"TrustRole:Trusted `  
  
    ```  
  
## <a name="BKMK_4"></a>Configurare la trasformazione delle attestazioni nella foresta trusting (Contoso)  
In questo passaggio si crea un criterio di trasformazione delle attestazioni in Contoso (la foresta trusting) per negare tutte le attestazioni, ad eccezione di "Company". È necessario creare un criterio di trasformazione delle attestazioni e crea un collegamento per il trust tra foreste.  
  
### <a name="BKMK_4.1"></a>Creare criteri di trasformazione delle attestazioni in Contoso  
  
##### <a name="to-create-a-transformation-policy-adatum-to-deny-all-except-company"></a>Per creare un criterio di trasformazione Adatum per negare tutto tranne "Company"  
  
1.  Accedere al controller di dominio, contoso.com come Administrator con la password **pass@word1**.  
  
2.  Aprire un prompt dei comandi con privilegi elevati in Windows PowerShell e digitare quanto segue:  
  
    ```  
    New-ADClaimTransformPolicy `  
    -Description:"Claims transformation policy to deny all claims except company" `  
    -Name:"DenyAllClaimsExceptCompanyPolicy" `  
    -DenyAllExcept:company `  
    -Server:"contoso.com" `  
  
    ```  
  
### <a name="BKMK_4.2"></a>Impostare un collegamento di trasformazione delle attestazioni sull'oggetto di dominio di trust Contoso  
In questo passaggio si applica l'oggetto appena creato criteri di trasformazione delle attestazioni dell'oggetto di dominio di trust contoso.com per Adatum consentire "Company" passate a contoso.com. Oggetto trust di dominio è denominato adatum.com.  
  
##### <a name="to-set-the-claims-transformation-policy"></a>Per impostare criteri di trasformazione di attestazioni  
  
1.  Accedere al controller di dominio, contoso.com come Administrator con la password **pass@word1**.  
  
2.  Aprire un prompt dei comandi con privilegi elevati in Windows PowerShell e digitare quanto segue:  
  
    ```  
  
      Set-ADClaimTransformLink   
    -Identity:"adatum.com" `  
    -Policy:"DenyAllClaimsExceptCompanyPolicy" `  
    -TrustRole:Trusting `  
  
    ```  
  
## <a name="BKMK_5"></a>Convalidare lo scenario.  
In questo passaggio si tenta di accedere alla cartella d:\earnings. che è stata configurata nel file server FILE1 per convalidare che l'utente abbia accesso alla cartella condivisa.  
  
#### <a name="to-ensure-that-the-adatum-user-can-access-the-shared-folder"></a>Per assicurarsi che l'utente Adatum possono accedere alla cartella condivisa  
  
1.  Accedi a Client computer, CLIENT1 come Jeff Low con la password **pass@word1**.  
  
2.  Passare alla cartella \\\FILE1.contoso.com\Earnings.  
  
3.  Jeff Low deve essere in grado di accedere alla cartella.  
  
## <a name="additional-scenarios-for-claims-transformation-policies"></a>Scenari aggiuntivi per i criteri di trasformazione delle attestazioni  
Seguito è riportato un elenco di altri scenari comuni nella trasformazione delle attestazioni.  
  
|Scenario|Condizione|  
|------------|----------|  
|Consenti tutte le attestazioni provenienti da Adatum di giungere ad Contoso Adatum|Codice: <br />New-ADClaimTransformPolicy \`<br /> -Description: "Criteri di trasformazione per consentire tutte le attestazioni delle attestazioni" \`<br />-Name:"AllowAllClaimsPolicy" \`<br />-AllowAll \`<br />-Server:"contoso.com" \`<br />Set-ADClaimTransformLink \`<br />-Identity:"adatum.com" \`<br />-Policy:"AllowAllClaimsPolicy" \`<br />-TrustRole:Trusting \`<br />-Server:"contoso.com" `|  
|Negare tutte le attestazioni provenienti da Adatum di giungere ad Contoso Adatum|Codice: <br />New-ADClaimTransformPolicy \`<br />-Description: "Criteri di trasformazione per negare tutte le attestazioni delle attestazioni" \`<br />-Name:"DenyAllClaimsPolicy" \`<br /> -DenyAll \`<br />-Server:"contoso.com" \`<br />Set-ADClaimTransformLink \`<br />-Identity:"adatum.com" \`<br />-Policy:"DenyAllClaimsPolicy" \`<br />-TrustRole:Trusting \`<br />-Server:"contoso.com"`|  
|Consenti tutte le attestazioni provenienti da Adatum ad eccezione di "Company" e "Department" di giungere ad Contoso Adatum|Codice <br />Nuova-ADClaimTransformationPolicy \`<br />-Description: "Criteri di trasformazione per consentire tutte le attestazioni, ad eccezione della società e reparto delle attestazioni" \`<br /> -Name:"AllowAllClaimsExceptCompanyAndDepartmentPolicy" \`<br />-AllowAllExcept: aziendale, reparto \`<br />-Server:"contoso.com" \`<br />Set-ADClaimTransformLink \`<br /> -Identity:"adatum.com" \`<br />-Policy:"AllowAllClaimsExceptCompanyAndDepartmentPolicy" \`<br /> -TrustRole:Trusting \`<br />-Server:"contoso.com" `|  
  
## <a name="BKMK_Links"></a>Vedere anche  
  
-   Per un elenco di tutti i cmdlet di Windows PowerShell disponibili per la trasformazione delle attestazioni, vedere [Active Directory PowerShell Cmdlet Reference](https://go.microsoft.com/fwlink/?LinkId=243150).  
  
-   Per attività avanzate che coinvolgono l'esportazione e importazione delle informazioni di configurazione dell'applicazione livello dati tra due foreste, usare il [riferimento di PowerShell controllo di accesso dinamiche](https://go.microsoft.com/fwlink/?LinkId=243150)  
  
-   [Distribuire attestazioni nelle foreste](Deploy-Claims-Across-Forests.md)  
  
-   [Linguaggio delle regole di trasformazione delle attestazioni](Claims-Transformation-Rules-Language.md)  
  
-   [Controllo dinamico degli accessi: Panoramica dello scenario](Dynamic-Access-Control--Scenario-Overview.md)  
  

