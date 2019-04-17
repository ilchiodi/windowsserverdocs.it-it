---
ms.assetid: 846c3680-b321-47da-8302-18472be42421
title: Distribuire attestazioni nelle foreste (passaggi dimostrativi)
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 3bab7a396061ecae8a187cc6986d6d026a9e4b32
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/12/2017
---
# <a name="deploy-claims-across-forests-demonstration-steps"></a>Distribuire attestazioni nelle foreste (passaggi dimostrativi)

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

In questo argomento ci occuperemo uno scenario di base che spiega come configurare le trasformazioni di attestazioni tra foreste trusted e trusting. Si apprenderà come oggetti Criteri di trasformazione delle attestazioni possono essere creati e collegati al trust foresta trusting e la foresta trusted. Convalidare quindi lo scenario.  
  
## <a name="scenario-overview"></a>Panoramica dello scenario  
Adatum Corporation provides financial services to Contoso, Ltd. Each quarter, Adatum accountants copy their account spreadsheets to a folder on a file server located at Contoso, Ltd. There is a two-way trust set up from Contoso to Adatum. Contoso, Ltd. wants to protect the share so that only Adatum employees can access the remote share.  
  
In questo scenario:  
  
1.  [Configurare i prerequisiti e l'ambiente di prova](Deploy-Claims-Across-Forests--Demonstration-Steps-.md#BKMK_1.1)  
  
2.  [Configurare la trasformazione delle attestazioni nella foresta trusted (Adatum)](Deploy-Claims-Across-Forests--Demonstration-Steps-.md#BKMK_3)  
  
3.  [Configurare la trasformazione delle attestazioni nella foresta trusting (Contoso)](Deploy-Claims-Across-Forests--Demonstration-Steps-.md#BKMK_4)  
  
4.  [Lo scenario di convalida](Deploy-Claims-Across-Forests--Demonstration-Steps-.md#BKMK_5)  
  
## <a name="BKMK_1.1"></a>Configurare i prerequisiti e l'ambiente di prova  
La configurazione di test prevede l'impostazione dei due insiemi di strutture: Adatum Corporation e Contoso, Ltd e con un trust bidirezionale tra Contoso e Adatum. "adatum.com" è la foresta trusted e "contoso.com" è la foresta trusting.  
  
Lo scenario di trasformazione delle attestazioni illustra trasformazione di un'attestazione nella foresta trusted a un'attestazione nella foresta trusting. A tale scopo, è necessario configurare una nuova foresta denominata adatum.com e popolare la foresta con un utente di prova con un valore aziendale 'Adatum'. È quindi necessario configurare un trust bidirezionale tra contoso.com e adatum.com.  
  
> [!IMPORTANT]  
> Quando si impostano le foreste di Contoso e Adatum, è necessario assicurarsi che entrambi i domini radice sono in Windows Server 2012 livello di funzionalità dominio per la trasformazione delle attestazioni per lavorare.  
  
È necessario configurare quanto segue per il lab. Queste procedure sono spiegate in dettaglio in [appendice b: Setting Up the Test Environment](Appendix-B--Setting-Up-the-Test-Environment.md)  
  
È necessario implementare le procedure seguenti per impostare il laboratorio per questo scenario:  
  
1.  [Impostare Adatum come foresta trusted per Contoso](Appendix-B--Setting-Up-the-Test-Environment.md)  
  
2.  [Creare il tipo di attestazione 'Società' in Contoso](Appendix-B--Setting-Up-the-Test-Environment.md#BKMK_2.8)  
  
3.  [Abilitare la proprietà di risorsa 'Società' in Contoso](Appendix-B--Setting-Up-the-Test-Environment.md#BKMK_2.55)  
  
4.  [Creare la regola di accesso centrale](Appendix-B--Setting-Up-the-Test-Environment.md#BKMK_2.9)  
  
5.  [Creare il criterio di accesso centrale](Appendix-B--Setting-Up-the-Test-Environment.md#BKMK_2.10)  
  
6.  [Pubblicare i nuovi criteri tramite criteri di gruppo](Appendix-B--Setting-Up-the-Test-Environment.md#BKMK_2.11)  
  
7.  [Creare la cartella Earnings sul file server](Appendix-B--Setting-Up-the-Test-Environment.md#BKMK_2.12)  
  
8.  [Impostare la classificazione e applicare i criteri di accesso centrale per la nuova cartella](Appendix-B--Setting-Up-the-Test-Environment.md#BKMK_2.13)  
  
Per completare questo scenario, utilizzare le seguenti informazioni:  
  
|Oggetti|Dettagli|  
|-----------|-----------|  
|Utenti|Jeff Low, Contoso|  
|Attestazioni utente su Adatum e Contoso|ID: Active Directory: / / EST/aziendale: ContosoAdatum,<br /><br />Attributo di origine: società<br /><br />I valori suggeriti: Contoso, Adatum **importante:** è necessario impostare l'ID su 'Società' tipo di attestazione in sia Contoso e Adatum essere lo stesso per la trasformazione delle attestazioni per lavorare.|  
|Regola di accesso centrale in Contoso|AdatumEmployeeAccessRule|  
|Criteri di accesso centrale in Contoso|Criteri di accesso solo adatum|  
|Criteri di trasformazione delle attestazioni Adatum e Contoso|DenyAllExcept aziendale|  
|Cartella file di Contoso|D:\EARNINGS.|  
  
## <a name="BKMK_3"></a>Configurare la trasformazione delle attestazioni nella foresta trusted (Adatum)  
In questo passaggio è creare un criterio di trasformazione in Adatum per negare a tutte le attestazioni, ad eccezione 'Società' per passare a Contoso.  
  
Il modulo Active Directory per Windows PowerShell fornisce il **DenyAllExcept** argomento, che elimina tutto tranne le attestazioni specificate nei criteri di trasformazione.  
  
Per configurare una trasformazione delle attestazioni, è necessario creare un criterio di trasformazione delle attestazioni e crea un collegamento tra le foreste trusted e trusting.  
  
### <a name="BKMK_2.2"></a>Creare un criterio di trasformazione delle attestazioni in Adatum  
  
##### <a name="to-create-a-transformation-policy-adatum-to-deny-all-claims-except-company"></a>Per creare un criterio di trasformazione Adatum per negare a tutte le attestazioni, ad eccezione 'Società'  
  
1.  Accedere al controller di dominio, adatum.com come Administrator con la password **pass@word1**.  
  
2.  Aprire un prompt dei comandi con privilegi elevati in Windows PowerShell e digitare quanto segue:  
  
    ```  
    New-ADClaimTransformPolicy `  
    -Description:"Claims transformation policy to deny all claims except Company"`  
    -Name:"DenyAllClaimsExceptCompanyPolicy" `  
    -DenyAllExcept:company `  
    -Server:"adatum.com" `  
  
    ```  
  
### <a name="BKMK_2.3"></a>Impostare un collegamento di trasformazione delle attestazioni sull'oggetto di dominio del Adatum trust  
In questo passaggio è applicare dei criteri di trasformazione delle attestazioni appena creato nell'oggetto di dominio del Adatum trust per Contoso.  
  
##### <a name="to-apply-the-claims-transformation-policy"></a>Per applicare i criteri di trasformazione delle attestazioni  
  
1.  Accedere al controller di dominio, adatum.com come Administrator con la password **pass@word1**.  
  
2.  Aprire un prompt dei comandi con privilegi elevati in Windows PowerShell e digitare quanto segue:  
  
    ```  
  
      Set-ADClaimTransformLink `  
    -Identity:"contoso.com" `  
    -Policy:"DenyAllClaimsExceptCompanyPolicy" `  
    '"TrustRole:Trusted `  
  
    ```  
  
## <a name="BKMK_4"></a>Configurare la trasformazione delle attestazioni nella foresta trusting (Contoso)  
In questo passaggio si crea un criterio di trasformazione delle attestazioni in Contoso (la foresta trusting), per negare a tutte le attestazioni, ad eccezione 'Società'. È necessario creare un criterio di trasformazione delle attestazioni e collegato il trust tra foreste.  
  
### <a name="BKMK_4.1"></a>Creare un criterio di trasformazione delle attestazioni in Contoso  
  
##### <a name="to-create-a-transformation-policy-adatum-to-deny-all-except-company"></a>Per creare un criterio di trasformazione Adatum per negare a tutti tranne 'Società'  
  
1.  Accedere al controller di dominio, contoso.com come Administrator con la password **pass@word1**.  
  
2.  Aprire un prompt dei comandi con privilegi elevati in Windows PowerShell e digitare quanto segue:  
  
    ```  
    New-ADClaimTransformPolicy `  
    -Description:"Claims transformation policy to deny all claims except company" `  
    -Name:"DenyAllClaimsExceptCompanyPolicy" `  
    -DenyAllExcept:company `  
    -Server:"contoso.com" `  
  
    ```  
  
### <a name="BKMK_4.2"></a>Impostare un collegamento di trasformazione delle attestazioni sull'oggetto di dominio di Contoso trust  
In this step, you apply the newly created claims transformation policy on the contoso.com trust domain object for Adatum to allow "Company" be passed through to contoso.com. The trust domain object is named adatum.com.  
  
##### <a name="to-set-the-claims-transformation-policy"></a>Per impostare criteri di trasformazione di attestazioni  
  
1.  Accedere al controller di dominio, contoso.com come Administrator con la password **pass@word1**.  
  
2.  Aprire un prompt dei comandi con privilegi elevati in Windows PowerShell e digitare quanto segue:  
  
    ```  
  
      Set-ADClaimTransformLink   
    -Identity:"adatum.com" `  
    -Policy:"DenyAllClaimsExceptCompanyPolicy" `  
    -TrustRole:Trusting `  
  
    ```  
  
## <a name="BKMK_5"></a>Lo scenario di convalida  
In questo passaggio si tenta di accedere alla cartella d:\earnings. che è stata configurata nel file server FILE1 per convalidare che l'utente ha accesso alla cartella condivisa.  
  
#### <a name="to-ensure-that-the-adatum-user-can-access-the-shared-folder"></a>Per assicurarsi che l'utente Adatum possono accedere alla cartella condivisa  
  
1.  Accedere al computer Client CLIENT1 come Jeff Low con la password **pass@word1**.  
  
2.  Individuare la cartella \\\FILE1.contoso.com\Earnings.  
  
3.  Jeff Low deve essere in grado di accedere alla cartella.  
  
## <a name="additional-scenarios-for-claims-transformation-policies"></a>Altri scenari per i criteri di trasformazione delle attestazioni  
Ecco un elenco di altri casi comuni in trasformazione delle attestazioni.  
  
|Scenario|Criteri|  
|------------|----------|  
|Consenti tutte le attestazioni provenienti da Adatum da eseguire per Contoso Adatum|Codice- <br />New-ADClaimTransformPolicy \'<br /> -Descrizione: "Criteri di trasformazione per consentire a tutte le richieste di attestazioni" \ "<br />-Nome: "AllowAllClaimsPolicy" \ "<br />-ConsentiTutto \'<br />-Server: "contoso.com" \ "<br />Set-ADClaimTransformLink \'<br />-Identità: "adatum.com" \ "<br />-Policy: "AllowAllClaimsPolicy" \ "<br />-TrustRole: Trusting \'<br />-Server: "contoso.com" '|  
|Nega tutte le attestazioni provenienti da Adatum da eseguire per Contoso Adatum|Codice- <br />New-ADClaimTransformPolicy \'<br />-Descrizione: "Criteri di trasformazione per negare a tutti i crediti di attestazioni" \ "<br />-Nome: "DenyAllClaimsPolicy" \ "<br /> -DenyAll \'<br />-Server: "contoso.com" \ "<br />Set-ADClaimTransformLink \'<br />-Identità: "adatum.com" \ "<br />-Policy: "DenyAllClaimsPolicy" \ "<br />-TrustRole: Trusting \'<br />-Server: "contoso.com" '|  
|Consenti tutte le attestazioni provenienti da Adatum ad eccezione di "Aziendale" e "Reparto" da eseguire per Contoso Adatum|Codice <br />-New-ADClaimTransformationPolicy \'<br />-Descrizione: "Criteri di trasformazione per consentire di tutte le attestazioni, ad eccezione della società e reparto attestazioni" \ "<br /> -Nome: "AllowAllClaimsExceptCompanyAndDepartmentPolicy" \ "<br />-AllowAllExcept: società, reparto \'<br />-Server: "contoso.com" \ "<br />Set-ADClaimTransformLink \'<br /> -Identità: "adatum.com" \ "<br />-Policy: "AllowAllClaimsExceptCompanyAndDepartmentPolicy" \ "<br /> -TrustRole: Trusting \'<br />-Server: "contoso.com" '|  
  
## <a name="BKMK_Links"></a>Vedere anche  
  
-   For a list of all Windows PowerShell cmdlets that are available for claims transformation, see [Active Directory PowerShell Cmdlet Reference](https://go.microsoft.com/fwlink/?LinkId=243150).  
  
-   For advanced tasks that involve export and import of DAC configuration information between two forests, use the [Dynamic Access Control PowerShell Reference](https://go.microsoft.com/fwlink/?LinkId=243150)  
  
-   [Distribuire attestazioni nelle foreste](Deploy-Claims-Across-Forests.md)  
  
-   [Linguaggio delle regole di trasformazione delle attestazioni](Claims-Transformation-Rules-Language.md)  
  
-   [Controllo dinamico degli accessi: Panoramica dello Scenario](Dynamic-Access-Control--Scenario-Overview.md)  
  

