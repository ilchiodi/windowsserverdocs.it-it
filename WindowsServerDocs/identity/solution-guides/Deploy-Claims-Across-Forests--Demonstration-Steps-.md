---
ms.assetid: 846c3680-b321-47da-8302-18472be42421
title: Distribuire attestazioni nelle foreste (passaggi dimostrativi)
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: 05ca21f343d2ad3db4ce00b53a66b3cd6dd6de16
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80861244"
---
# <a name="deploy-claims-across-forests-demonstration-steps"></a>Distribuire attestazioni nelle foreste (passaggi dimostrativi)

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

In questo argomento si tratterà di uno scenario di base in cui viene illustrato come configurare le trasformazioni delle attestazioni tra foreste trusted e trusting. Si apprenderà come creare oggetti Criteri di trasformazione delle attestazioni e come collegarli alla relazione di trust nella foresta trusting e nella foresta trusted. Sarà quindi possibile convalidare lo scenario.  

## <a name="scenario-overview"></a>Panoramica dello scenario  
Adatum Corporation fornisce servizi finanziari a contoso, Ltd. Ogni trimestre, adatum Accountants copia i fogli di calcolo degli account in una cartella in un file server situato in contoso, Ltd. Esiste una relazione di trust bidirezionale configurata da Contoso a adatum. Contoso, Ltd. vuole proteggere la condivisione in modo che solo i dipendenti di adatum possano accedere alla condivisione remota.  

In questo scenario:  

1.  [Configurare i prerequisiti e l'ambiente di test](Deploy-Claims-Across-Forests--Demonstration-Steps-.md#BKMK_1.1)  

2.  [Configurare la trasformazione delle attestazioni su una foresta trusted (adatum)](Deploy-Claims-Across-Forests--Demonstration-Steps-.md#BKMK_3)  

3.  [Configurare la trasformazione delle attestazioni nella foresta trusting (contoso)](Deploy-Claims-Across-Forests--Demonstration-Steps-.md#BKMK_4)  

4.  [Convalidare lo scenario](Deploy-Claims-Across-Forests--Demonstration-Steps-.md#BKMK_5)  

## <a name="set-up-the-prerequisites-and-the-test-environment"></a><a name="BKMK_1.1"></a>Configurare i prerequisiti e l'ambiente di test  
Per la configurazione di test è necessario configurare due foreste: Adatum Corporation e contoso, Ltd e avere un trust bidirezionale tra Contoso e adatum. "adatum.com" è la foresta trusted e "contoso.com" è la foresta trusting.  

Nello scenario di trasformazione delle attestazioni viene illustrata la trasformazione di un'attestazione nella foresta trusted in un'attestazione nella foresta trusting. A tale scopo, è necessario configurare una nuova foresta denominata adatum.com e popolare la foresta con un utente di test con un valore della società "adatum". Sarà quindi necessario configurare un trust bidirezionale tra contoso.com e adatum.com.  

> [!IMPORTANT]  
> Quando si configurano le foreste Contoso e adatum, è necessario assicurarsi che entrambi i domini radice siano al livello di funzionalità del dominio di Windows Server 2012 per il funzionamento della trasformazione delle attestazioni.  

È necessario configurare quanto segue per il Lab. Queste procedure sono illustrate in dettaglio nell' [Appendice B: configurazione dell'ambiente di test](Appendix-B--Setting-Up-the-Test-Environment.md)  

Per configurare il Lab per questo scenario, è necessario implementare le procedure seguenti:  

1.  [Impostare adatum come foresta trusted su contoso](Appendix-B--Setting-Up-the-Test-Environment.md)  

2.  [Creare il tipo di attestazione "Company" in contoso](Appendix-B--Setting-Up-the-Test-Environment.md#BKMK_2.8)  

3.  [Abilitare la proprietà della risorsa ' Company ' in contoso](Appendix-B--Setting-Up-the-Test-Environment.md#BKMK_2.55)  

4.  [Creare la regola di accesso centrale](Appendix-B--Setting-Up-the-Test-Environment.md#BKMK_2.9)  

5.  [Creare i criteri di accesso centrale](Appendix-B--Setting-Up-the-Test-Environment.md#BKMK_2.10)  

6.  [Pubblicare i nuovi criteri tramite Criteri di gruppo](Appendix-B--Setting-Up-the-Test-Environment.md#BKMK_2.11)  

7.  [Creare la cartella guadagni nel file server](Appendix-B--Setting-Up-the-Test-Environment.md#BKMK_2.12)  

8.  [Impostare la classificazione e applicare i criteri di accesso centrale alla nuova cartella](Appendix-B--Setting-Up-the-Test-Environment.md#BKMK_2.13)  

Per completare questo scenario, utilizzare le seguenti informazioni:  

|Oggetti|Dettagli|  
|-----------|-----------|  
|Utenti|Jeff low, contoso|  
|Attestazioni utente su Adatum e contoso|ID: ad://ext/Company:ContosoAdatum,<p>Attributo di origine: società<p>Valori suggeriti: contoso, adatum **importante:** è necessario impostare l'ID per il tipo di attestazione "Company" sia per Contoso che per Adatum affinché la trasformazione delle attestazioni funzioni correttamente.|  
|Regola di accesso centrale in contoso|AdatumEmployeeAccessRule|  
|Criteri di accesso centrale in contoso|Criteri di accesso solo a adatum|  
|Criteri di trasformazione delle attestazioni in Adatum e contoso|Società DenyAllExcept|  
|Cartella di file in contoso|D:\EARNINGS|  

## <a name="set-up-claims-transformation-on-trusted-forest-adatum"></a><a name="BKMK_3"></a>Configurare la trasformazione delle attestazioni su una foresta trusted (adatum)  
In questo passaggio viene creato un criterio di trasformazione in Adatum per negare tutte le attestazioni ad eccezione di "Company" per passare a contoso.  

Il modulo Active Directory per Windows PowerShell fornisce l'argomento **DenyAllExcept** , che elimina tutti gli elementi, ad eccezione delle attestazioni specificate nei criteri di trasformazione.  

Per configurare una trasformazione delle attestazioni, è necessario creare un criterio di trasformazione delle attestazioni e collegarlo tra le foreste trusted e trusting.  

### <a name="create-a-claims-transformation-policy-in-adatum"></a><a name="BKMK_2.2"></a>Creazione di un criterio di trasformazione delle attestazioni in adatum  

##### <a name="to-create-a-transformation-policy-adatum-to-deny-all-claims-except-company"></a>Per creare un criterio di trasformazione Adatum per negare tutte le attestazioni ad eccezione di ' Company '  

1. Accedere al controller di dominio, adatum.com come amministratore con la password <strong>pass@word1</strong>.  

2. Aprire un prompt dei comandi con privilegi elevati in Windows PowerShell e digitare quanto segue:  

   ```  
   New-ADClaimTransformPolicy `  
   -Description:"Claims transformation policy to deny all claims except Company"`  
   -Name:"DenyAllClaimsExceptCompanyPolicy" `  
   -DenyAllExcept:company `  
   -Server:"adatum.com" `  

   ```  

### <a name="set-a-claims-transformation-link-on-adatums-trust-domain-object"></a><a name="BKMK_2.3"></a>Impostare un collegamento di trasformazione delle attestazioni nell'oggetto dominio di trust di adatum  
In questo passaggio si applicano i criteri di trasformazione delle attestazioni appena creati nell'oggetto dominio di trust di Adatum per contoso.  

##### <a name="to-apply-the-claims-transformation-policy"></a>Per applicare i criteri di trasformazione delle attestazioni  

1. Accedere al controller di dominio, adatum.com come amministratore con la password <strong>pass@word1</strong>.  

2. Aprire un prompt dei comandi con privilegi elevati in Windows PowerShell e digitare quanto segue:  

   ```  

     Set-ADClaimTransformLink `  
   -Identity:"contoso.com" `  
   -Policy:"DenyAllClaimsExceptCompanyPolicy" `  
   '"TrustRole:Trusted `  

   ```  

## <a name="set-up-claims-transformation-in-the-trusting-forest-contoso"></a><a name="BKMK_4"></a>Configurare la trasformazione delle attestazioni nella foresta trusting (contoso)  
In questo passaggio vengono creati i criteri di trasformazione delle attestazioni in Contoso (la foresta trusting) per negare tutte le attestazioni ad eccezione di "Company". È necessario creare un criterio di trasformazione delle attestazioni e collegarlo al trust tra insiemi di strutture.  

### <a name="create-a-claims-transformation-policy-in-contoso"></a><a name="BKMK_4.1"></a>Creare criteri di trasformazione delle attestazioni in contoso  

##### <a name="to-create-a-transformation-policy-adatum-to-deny-all-except-company"></a>Per creare un criterio di trasformazione Adatum per negare tutte le eccezioni ' Company '  

1. Accedere al controller di dominio, contoso.com come amministratore con la password <strong>pass@word1</strong>.  

2. Aprire un prompt dei comandi con privilegi elevati in Windows PowerShell e digitare quanto segue:  

   ```  
   New-ADClaimTransformPolicy `  
   -Description:"Claims transformation policy to deny all claims except company" `  
   -Name:"DenyAllClaimsExceptCompanyPolicy" `  
   -DenyAllExcept:company `  
   -Server:"contoso.com" `  

   ```  

### <a name="set-a-claims-transformation-link-on-contosos-trust-domain-object"></a><a name="BKMK_4.2"></a>Impostare un collegamento di trasformazione delle attestazioni nell'oggetto dominio di trust di contoso  
In questo passaggio si applicano i criteri di trasformazione delle attestazioni appena creati nell'oggetto dominio di trust contoso.com per Adatum per consentire a "Company" di essere passati a contoso.com. L'oggetto dominio di trust è denominato adatum.com.  

##### <a name="to-set-the-claims-transformation-policy"></a>Per impostare i criteri di trasformazione delle attestazioni  

1. Accedere al controller di dominio, contoso.com come amministratore con la password <strong>pass@word1</strong>.  

2. Aprire un prompt dei comandi con privilegi elevati in Windows PowerShell e digitare quanto segue:  

   ```  

     Set-ADClaimTransformLink   
   -Identity:"adatum.com" `  
   -Policy:"DenyAllClaimsExceptCompanyPolicy" `  
   -TrustRole:Trusting `  

   ```  

## <a name="validate-the-scenario"></a><a name="BKMK_5"></a>Convalidare lo scenario  
In questo passaggio si tenta di accedere alla cartella D:\EARNINGS configurata nel file server FILE1 per verificare che l'utente abbia accesso alla cartella condivisa.  

#### <a name="to-ensure-that-the-adatum-user-can-access-the-shared-folder"></a>Per assicurarsi che l'utente adatum possa accedere alla cartella condivisa  

1. Accedere al computer client, CLIENT1 come Jeff low con la password <strong>pass@word1</strong>.  

2. Passare alla cartella \\\FILE1.contoso.com\Earnings.  

3. Jeff low dovrebbe essere in grado di accedere alla cartella.  

## <a name="additional-scenarios-for-claims-transformation-policies"></a>Scenari aggiuntivi per i criteri di trasformazione delle attestazioni  
Di seguito è riportato un elenco di casi comuni aggiuntivi nella trasformazione delle attestazioni.  


|                                                 Scenario                                                 |                                                                                                                                                                                                                                           Condizione                                                                                                                                                                                                                                            |
|----------------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|                  Consentire a tutte le attestazioni provenienti da Adatum di passare a Contoso adatum                  |                                                          Codice: <br />\` New-ADClaimTransformPolicy<br /> -Description: "criterio di trasformazione delle attestazioni per consentire tutte le attestazioni" \`<br />-Name: "AllowAllClaimsPolicy" \`<br />-AllowAll \`<br />-Server:"contoso. com" \`<br />Set-ADClaimTransformLink \`<br />-Identity:"adatum. com" \`<br />-Policy: "AllowAllClaimsPolicy" \`<br />-TrustRole: trusting \`<br />-Server:"contoso. com" \`                                                          |
|                  Nega tutte le attestazioni provenienti da Adatum per passare a Contoso adatum                   |                                                            Codice: <br />\` New-ADClaimTransformPolicy<br />-Description: "criterio di trasformazione delle attestazioni per negare tutte le attestazioni" \`<br />-Name: "DenyAllClaimsPolicy" \`<br /> -DenyAll \`<br />-Server:"contoso. com" \`<br />Set-ADClaimTransformLink \`<br />-Identity:"adatum. com" \`<br />-Policy: "DenyAllClaimsPolicy" \`<br />-TrustRole: trusting \`<br />-Server:"contoso. com"\`                                                             |
| Consenti tutte le attestazioni provenienti da Adatum ad eccezione di "società" e "reparto" per passare a Contoso adatum | Codice <br />-New-ADClaimTransformationPolicy \`<br />-Description: "criteri di trasformazione delle attestazioni per consentire tutte le attestazioni ad eccezione della società e del reparto" \`<br /> -Name: "AllowAllClaimsExceptCompanyAndDepartmentPolicy" \`<br />-AllowAllExcept: azienda, reparto \`<br />-Server:"contoso. com" \`<br />Set-ADClaimTransformLink \`<br /> -Identity:"adatum. com" \`<br />-Policy: "AllowAllClaimsExceptCompanyAndDepartmentPolicy" \`<br /> -TrustRole: trusting \`<br />-Server:"contoso. com" \` |

## <a name="see-also"></a><a name="BKMK_Links"></a>Vedere anche  

-   Per un elenco di tutti i cmdlet di Windows PowerShell disponibili per la trasformazione delle attestazioni, vedere [Active Directory riferimento ai cmdlet di PowerShell](https://go.microsoft.com/fwlink/?LinkId=243150).  

-   Per le attività avanzate che coinvolgono l'esportazione e l'importazione di informazioni di configurazione dell'applicazione livello dati tra due foreste, usare il [controllo dinamico degli accessi di PowerShell](https://go.microsoft.com/fwlink/?LinkId=243150)  

-   [Distribuire attestazioni nelle foreste](Deploy-Claims-Across-Forests.md)  

-   [Linguaggio delle regole di trasformazione delle attestazioni](Claims-Transformation-Rules-Language.md)  

-   [Controllo dinamico degli accessi: Panoramica dello scenario](Dynamic-Access-Control--Scenario-Overview.md)  


