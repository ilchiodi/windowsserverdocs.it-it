---
ms.assetid: e831f781-3c45-4d44-b411-160d121d1324
title: Linguaggio delle regole di trasformazione delle attestazioni
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: f391c3f8ef2bb5b12f0dd15db55df4f861c05f9b
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80861274"
---
# <a name="claims-transformation-rules-language"></a>Linguaggio delle regole di trasformazione delle attestazioni

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

La funzionalità di trasformazione delle attestazioni tra foreste consente di colmare le attestazioni per il controllo dinamico degli accessi attraverso i limiti della foresta impostando i criteri di trasformazione delle attestazioni nei trust tra foreste. Il componente primario di tutti i criteri è costituito da regole scritte nel linguaggio delle regole di trasformazione delle attestazioni. In questo argomento vengono fornite informazioni dettagliate su questo linguaggio e vengono fornite indicazioni sulla creazione di regole di trasformazione delle attestazioni.  
  
I cmdlet di Windows PowerShell per i criteri di trasformazione in trust tra foreste includono opzioni per l'impostazione di criteri semplici richiesti negli scenari comuni. Questi cmdlet convertono l'input dell'utente in criteri e regole nel linguaggio delle regole di trasformazione delle attestazioni e quindi li archiviano in Active Directory nel formato previsto. Per ulteriori informazioni sui cmdlet per la trasformazione delle attestazioni, vedere [cmdlet di servizi di dominio Active Directory per il controllo dinamico degli accessi](https://go.microsoft.com/fwlink/?LinkId=243150).  
  
A seconda della configurazione delle attestazioni e dei requisiti inseriti nel trust tra foreste nelle foreste Active Directory, i criteri di trasformazione delle attestazioni possono essere più complessi rispetto ai criteri supportati dai cmdlet di Windows PowerShell per Active Directory. Per creare in modo efficace questi criteri, è essenziale comprendere la semantica e la sintassi del linguaggio delle regole di trasformazione delle attestazioni. Questo linguaggio delle regole di trasformazione delle attestazioni ("lingua") in Active Directory è un subset del linguaggio usato da [Active Directory Federation Services](https://go.microsoft.com/fwlink/?LinkId=243982) per scopi simili e presenta una sintassi e una semantica molto simili. Tuttavia, vi sono meno operazioni consentite e altre restrizioni di sintassi vengono inserite nella versione Active Directory della lingua.  
  
In questo argomento vengono illustrati brevemente la sintassi e la semantica del linguaggio delle regole di trasformazione delle attestazioni in Active Directory e le considerazioni da effettuare durante la creazione dei criteri. Sono disponibili diversi set di regole di esempio per iniziare ed esempi di sintassi non corretta e dei messaggi generati, per consentire di decifrare i messaggi di errore quando si creano le regole.  
  
## <a name="tools-for-authoring-claims-transformation-policies"></a>Strumenti per la creazione di criteri di trasformazione delle attestazioni  
**Cmdlet di Windows PowerShell per Active Directory**: si tratta della modalità preferita e consigliata per creare e impostare i criteri di trasformazione delle attestazioni. Questi cmdlet forniscono opzioni per i criteri semplici e verificano le regole impostate per i criteri più complessi.  
  
**LDAP**: i criteri di trasformazione delle attestazioni possono essere modificati in Active Directory tramite Lightweight Directory Access Protocol (LDAP). Tuttavia, questa operazione non è consigliata perché i criteri hanno diversi componenti complessi e gli strumenti usati non possono convalidare i criteri prima di scriverli in Active Directory. Questo potrebbe richiedere una notevole quantità di tempo per diagnosticare i problemi.  
  
## <a name="active-directory-claims-transformation-rules-language"></a>Lingua delle regole di trasformazione delle attestazioni Active Directory  
  
### <a name="syntax-overview"></a>Cenni preliminari sulla sintassi  
Ecco una breve panoramica della sintassi e della semantica del linguaggio:  
  
-   Il set di regole di trasformazione delle attestazioni è costituito da zero o più regole. Ogni regola ha due parti attive: **selezionare l'elenco di condizioni** e l' **azione della regola**. Se l' **elenco Seleziona condizione** restituisce true, viene eseguita l'azione della regola corrispondente.  
  
-   L'elenco di condizioni **Select** include zero o più **condizioni SELECT**. Tutte le **condizioni Select** devono restituire true affinché l' **elenco SELECT Condition** restituisca true.  
  
-   Ogni **condizione Select** ha un set di zero o più **condizioni di corrispondenza**. Tutte le **condizioni di corrispondenza** devono restituire true perché la condizione SELECT restituisca true. Tutte queste condizioni vengono valutate in base a una singola attestazione. Un'attestazione che corrisponde a una **condizione Select** può essere contrassegnata da un **identificatore** e a cui viene fatto riferimento nell' **azione della regola**.  
  
-   Ogni **condizione di corrispondenza** specifica la condizione in modo che corrisponda al **tipo** o al **valore** o al **ValueType** di un'attestazione utilizzando diversi **operatori di condizione** e **valori letterali stringa**.  
  
    -   Quando si specifica una **condizione di corrispondenza** per un **valore**, è necessario specificare anche una **condizione di corrispondenza** per un **ValueType** specifico e viceversa. Queste condizioni devono trovarsi tra loro nella sintassi.  
  
    -   Le condizioni di corrispondenza **ValueType** devono usare solo valori letterali **ValueType** specifici.  
  
-   Un' **azione regola** può copiare un'attestazione contrassegnata con un **identificatore** o emettere un'attestazione in base a un'attestazione contrassegnata con un identificatore e/o valori letterali stringa specificati.  
  
**Regola di esempio**  
  
In questo esempio viene illustrata una regola che può essere utilizzata per convertire il tipo di attestazioni tra due foreste, a condizione che utilizzino gli stessi ValueType delle attestazioni e abbiano le stesse interpretazioni per i valori delle attestazioni per questo tipo. La regola presenta una condizione di corrispondenza e un'istruzione Issue che usa valori letterali stringa e un riferimento a attestazioni corrispondenti.  
  
```  
C1: [TYPE=="EmployeeType"]    
                 => ISSUE (TYPE= "EmpType", VALUE = C1.VALUE, VALUETYPE = C1.VALUETYPE);  
[TYPE=="EmployeeType"] == Select Condition List with one Matching Condition for claims Type.  
ISSUE (TYPE= "EmpType", VALUE = C1.VALUE, VALUETYPE = C1.VALUETYPE) == Rule Action that issues a claims using string literal and matching claim referred with the Identifier.  
  
```  
  
### <a name="runtime-operation"></a>Operazione di runtime  
È importante comprendere il funzionamento in fase di esecuzione delle trasformazioni delle attestazioni per creare le regole in modo efficace. L'operazione di runtime usa tre set di attestazioni:  
  
1.  **Set di attestazioni di input**: set di attestazioni di input assegnato all'operazione di trasformazione delle attestazioni.  
  
2.  **Working Claims set**: attestazioni intermedie che vengono lette e scritte durante la trasformazione delle attestazioni.  
  
3.  **Output Claims set**: output dell'operazione di trasformazione delle attestazioni.  
  
Ecco una breve panoramica dell'operazione di trasformazione delle attestazioni di runtime:  
  
1.  Le attestazioni di input per la trasformazione delle attestazioni vengono utilizzate per inizializzare il set di attestazioni funzionante  
  
    1.  Quando si elabora ogni regola, il set di attestazioni funzionante viene usato per le attestazioni di input.  
  
    2.  L'elenco delle condizioni di selezione in una regola viene associato a tutti i possibili set di attestazioni del set di attestazioni funzionante.  
  
    3.  Ogni set di attestazioni corrispondenti viene usato per eseguire l'azione nella regola.  
  
    4.  L'esecuzione di un'azione regola restituisce un'attestazione, che viene aggiunta al set di attestazioni di output e al set di attestazioni funzionante. Pertanto, l'output di una regola viene utilizzato come input per le regole successive nel set di regole.  
  
2.  Le regole del set di regole vengono elaborate in ordine sequenziale iniziando con la prima regola.  
  
3.  Quando viene elaborato l'intero set di regole, il set di attestazioni di output viene elaborato per rimuovere attestazioni duplicate e per altri problemi di sicurezza. Le attestazioni risultanti sono l'output del processo di trasformazione delle attestazioni.  
  
È possibile scrivere trasformazioni di attestazioni complesse in base al comportamento di runtime precedente.  
  
**Esempio: operazione di runtime**  
  
In questo esempio viene illustrata l'operazione di runtime di una trasformazione delle attestazioni che utilizza due regole.  
  
```  
  
     C1:[Type=="EmpType", Value=="FullTime",ValueType=="string"] =>  
                Issue(Type=="EmployeeType", Value=="FullTime",ValueType=="string");  
     [Type=="EmployeeType"] =>   
               Issue(Type=="AccessType", Value=="Privileged", ValueType=="string");  
Input claims and Initial Evaluation Context:  
  {(Type= "EmpType"),(Value="FullTime"),(ValueType="String")}  
{(Type= "Organization"),(Value="Marketing"),(ValueType="String")}  
After Processing Rule 1:  
 Evaluation Context:  
  {(Type= "EmpType"),(Value="FullTime"),(ValueType="String")}  
{(Type= "Organization"), (Value="Marketing"),(ValueType="String")}  
  {(Type= "EmployeeType"),(Value="FullTime"),(ValueType="String")}  
Output Context:  
  {(Type= "EmployeeType"),(Value="FullTime"),(ValueType="String")}  
  
After Processing Rule 2:  
Evaluation Context:  
  {(Type= "EmpType"),(Value="FullTime"),(ValueType="String")}  
{(Type= "Organization"),(Value="Marketing"),(ValueType="String")}  
  {(Type= "EmployeeType"),(Value="FullTime"),(ValueType="String")}  
  {(Type= "AccessType"),(Value="Privileged"),(ValueType="String")}  
Output Context:  
  {(Type= "EmployeeType"),(Value="FullTime"),(ValueType="String")}  
  {(Type= "AccessType"),(Value="Privileged"),(ValueType="String")}  
  
Final Output:  
  {(Type= "EmployeeType"),(Value="FullTime"),(ValueType="String")}  
  {(Type= "AccessType"),(Value="Privileged"),(ValueType="String")}  
  
```  
  
### <a name="special-rules-semantics"></a>Semantica delle regole speciali  
Di seguito sono riportate una sintassi speciale per le regole:  
  
1.  Set di regole vuote = = nessuna attestazione di output  
  
2.  Empty Select Condition list = = ogni attestazione corrisponde all'elenco di condizioni Select  
  
    **Esempio: elenco di condizioni Select vuote**  
  
    La regola seguente corrisponde a ogni attestazione nella working set.  
  
    ```  
    => Issue (Type = "UserType", Value = "External", ValueType = "string")  
    ```  
  
3.  Empty Select matching list = = ogni attestazione corrisponde all'elenco di condizioni Select  
  
    **Esempio: Condizioni di corrispondenza vuote**  
  
    La regola seguente corrisponde a ogni attestazione nella working set. Questa è la regola di base "Allow-all" Se viene usata da sola.  
  
    ```  
    C1:[] => Issule (claim = C1);  
    ```  
  
## <a name="security-considerations"></a>Considerazioni sulla sicurezza  
**Attestazioni che entrano in una foresta**  
  
Le attestazioni presentate dalle entità in ingresso a una foresta devono essere controllate accuratamente per garantire che vengano consentite o rilasciate solo le attestazioni corrette. Le attestazioni non corrette possono compromettere la sicurezza della foresta e questo dovrebbe essere una considerazione migliore quando si creano criteri di trasformazione per le attestazioni che entrano in una foresta.  
  
Active Directory presenta le funzionalità seguenti per impedire la configurazione errata delle attestazioni che entrano in una foresta:  
  
-   Se un trust tra foreste non dispone di criteri di trasformazione delle attestazioni impostati per le attestazioni che entrano in una foresta, per motivi di sicurezza Active Directory elimina tutte le attestazioni principali che entrano nell'insieme di strutture.  
  
-   Se l'esecuzione del set di regole sulle attestazioni che immette una foresta produce attestazioni che non sono definite nella foresta, le attestazioni non definite vengono eliminate dalle attestazioni di output.  
  
**Attestazioni che lasciano una foresta**  
  
Le attestazioni che lasciano una foresta presentano un problema di sicurezza inferiore per la foresta rispetto alle attestazioni che entrano nella foresta. Le attestazioni possono lasciare la foresta così com'è anche quando non sono presenti criteri di trasformazione delle attestazioni corrispondenti. È anche possibile rilasciare attestazioni che non sono definite nella foresta come parte della trasformazione delle attestazioni che lasciano la foresta. Questo consente di configurare facilmente trust tra foreste con attestazioni. Un amministratore può determinare se le attestazioni che entrano nella foresta devono essere trasformate e configurare i criteri appropriati. Ad esempio, un amministratore potrebbe impostare un criterio se è necessario nascondere un'attestazione per impedire la divulgazione di informazioni.  
  
**Errori di sintassi nelle regole di trasformazione delle attestazioni**  
  
Se a un determinato criterio di trasformazione delle attestazioni è associato un set di regole sintatticamente errato o se sono presenti altri problemi di sintassi o archiviazione, i criteri vengono considerati non validi. Questo comportamento viene considerato in modo diverso rispetto alle condizioni predefinite indicate in precedenza.  
  
Active Directory non è in grado di determinare lo scopo in questo caso e passa a una modalità non sicura, in cui non vengono generate attestazioni di output su tale trust e direzione di attraversamento. Per risolvere il problema, è necessario l'intervento dell'amministratore. Questo problema può verificarsi se si utilizza LDAP per modificare i criteri di trasformazione delle attestazioni. I cmdlet di Windows PowerShell per Active Directory hanno la convalida per evitare la scrittura di criteri con problemi di sintassi.  
  
## <a name="other-language-considerations"></a>Altre considerazioni sul linguaggio  
  
1.  Sono disponibili diverse parole chiave o caratteri speciali in questo linguaggio (detti terminali). Questi vengono presentati nella tabella dei [terminali del linguaggio](Claims-Transformation-Rules-Language.md#BKMK_LT) più avanti in questo argomento. I messaggi di errore usano i tag per questi terminali per la risoluzione dell'ambiguità.  
  
2.  I terminali possono a volte essere usati come valori letterali stringa. Tuttavia, tale utilizzo potrebbe essere in conflitto con la definizione del linguaggio o avere conseguenze impreviste. Questo tipo di utilizzo non è consigliato.  
  
3.  L'azione della regola non può eseguire conversioni di tipi sui valori di attestazione e un set di regole che contiene tale azione della regola non è considerato valido. Questo potrebbe causare un errore di runtime e non vengono generate attestazioni di output.  
  
4.  Se un'azione della regola fa riferimento a un identificatore che non è stato usato nella parte relativa all'elenco di condizioni SELECT della regola, si tratta di un utilizzo non valido. Questo potrebbe causare un errore di sintassi.  
  
    **Esempio: riferimento all'identificatore errato**  
    Nella regola seguente viene illustrato un identificatore errato utilizzato nell'azione della regola.  
  
    ```  
    C1:[] => Issue (claim = C2);  
    ```  
  
## <a name="sample-transformation-rules"></a>Regole di trasformazione di esempio  
  
-   **Consenti tutte le attestazioni di un determinato tipo**  
  
    Tipo esatto  
  
    ```  
    C1:[type=="XYZ"] => Issue (claim = C1);  
    ```  
  
    Uso di Regex  
  
    ```  
    C1: [type =~ "XYZ*"] => Issue (claim = C1);  
    ```  
  
-   **Non consentire un determinato tipo di attestazione**  
    Tipo esatto  
  
    ```  
    C1:[type != "XYZ"] => Issue (claim=C1);  
    ```  
  
    Uso di Regex  
  
    ```  
    C1:[Type !~ "XYZ?"] => Issue (claim=C1);  
    ```  
  
## <a name="examples-of-rules-parser-errors"></a>Esempi di errori del parser di regole  
Le regole di trasformazione delle attestazioni vengono analizzate da un parser personalizzato per verificare la presenza di errori di sintassi. Questo parser viene eseguito dai cmdlet di Windows PowerShell correlati prima di archiviare le regole in Active Directory. Eventuali errori durante l'analisi delle regole, inclusi gli errori di sintassi, vengono visualizzati nella console. I controller di dominio eseguono anche il parser prima di usare le regole per la trasformazione delle attestazioni e registrano gli errori nel registro eventi (aggiungere i numeri dei log eventi).  
  
In questa sezione vengono illustrati alcuni esempi di regole scritte con sintassi non corretta e gli errori di sintassi corrispondenti generati dal parser.  
  
1. Esempio:  
  
   ```  
   c1;[]=>Issue(claim=c1);  
   ```  
  
   Questo esempio contiene un punto e virgola utilizzato in modo errato al posto di due punti.   
   **Messaggio di errore:**  
   *POLICY0002: non è stato possibile analizzare i dati dei criteri.*  
   *Numero riga: 1, numero di colonna: 2, token di errore:;. Riga:' C1; [] = > problema (Claim = C1); ".*  
   *Errore del parser:' POLICY0030: errore di sintassi,';' imprevisto. è previsto uno dei seguenti elementi:':'.*  
  
2. Esempio:  
  
   ```  
   c1:[]=>Issue(claim=c2);  
   ```  
  
   In questo esempio, il Tag Identifier nell'istruzione Copy rilascio non è definito.   
   **Messaggio di errore**:   
   *POLICY0011: nessuna condizione nella regola attestazione corrisponde al tag Condition specificato in CopyIssuanceStatement:' C2'.*  
  
3. Esempio:  
  
   ```  
   c1:[type=="x1", value=="1", valuetype=="bool"]=>Issue(claim=c1)  
   ```  
  
   "bool" non è un terminale nel linguaggio e non è un ValueType valido. I terminali validi sono elencati nel messaggio di errore seguente.   
   **Messaggio di errore:**  
   *POLICY0002: non è stato possibile analizzare i dati dei criteri.*  
   Numero riga: 1, numero di colonna: 39, token di errore: "bool". Riga:' C1: [tipo = = "x1", valore = = "1", ValueType = = "bool"] = > problema (Claim = C1);'.   
   *Errore del parser:' POLICY0030: errore di sintassi,' STRING ' imprevisto. è previsto uno dei seguenti elementi:' INT64_TYPE '' UINT64_TYPE '' STRING_TYPE '' BOOLEAN_TYPE '' IDENTIFIER '*  
  
4. Esempio:  
  
   ```  
   c1:[type=="x1", value==1, valuetype=="boolean"]=>Issue(claim=c1);  
   ```  
  
   Il numero **1** in questo esempio non è un token valido nel linguaggio e tale utilizzo non è consentito in una condizione corrispondente. Deve essere racchiuso tra virgolette doppie per renderla una stringa.   
   **Messaggio di errore:**  
   *POLICY0002: non è stato possibile analizzare i dati dei criteri.*  
   *Numero di riga: 1, numero di colonna: 23, token di errore: 1. riga:' C1: [tipo = = "x1", valore = = 1, ValueType = = "bool"] = > problema (Claim = C1);'.* <em>Errore del parser:' POLICY0029: input imprevisto.</em>  
  
5. Esempio:  
  
   ```  
   c1:[type == "x1", value == "1", valuetype == "boolean"] =>   
  
        Issue(type = c1.type, value="0", valuetype == "boolean");  
   ```  
  
   In questo esempio è stato usato un segno di uguale doppio (= =) invece di un singolo segno di uguale (=).   
   **Messaggio di errore:**  
   *POLICY0002: non è stato possibile analizzare i dati dei criteri.*  
   *Numero riga: 1, numero di colonna: 91, token di errore: = =. Riga:' C1: [tipo = = "x1", valore = = "1",*  
   *ValueType = = "Boolean"] = > problema (tipo = C1. tipo, valore = "0", ValueType = = "Boolean"); ".*  
   *Errore del parser:' POLICY0030: errore di sintassi,' = =' imprevisto. è previsto uno dei seguenti elementi:' ='*  
  
6. Esempio:  
  
   ```  
   c1:[type=="x1", value=="boolean", valuetype=="string"] =>   
  
         Issue(type=c1.type, value=c1.value, valuetype = "string");  
   ```  
  
   Questo esempio è sintatticamente e semanticamente corretto. Tuttavia, l'utilizzo di "Boolean" come valore stringa è associato a una confusione ed è consigliabile evitarlo. Come indicato in precedenza, l'uso di terminali di linguaggio come valori di attestazioni deve essere evitato laddove possibile.  
  
## <a name="language-terminals"></a><a name="BKMK_LT"></a>Terminali del linguaggio  
La tabella seguente elenca il set completo di stringhe terminali e i terminali di linguaggio associati usati nel linguaggio delle regole di trasformazione delle attestazioni. Queste definizioni utilizzano stringhe UTF-16 senza distinzione tra maiuscole e minuscole.  
  
|String|Terminale|  
|----------|------------|  
|"= >"|IMPLICA|  
|";"|VIRGOLA|  
|":"|VIRGOLA|  
|","|VIRGOLE|  
|"."|PUNTO|  
|"["|O_SQ_BRACKET|  
|"]"|C_SQ_BRACKET|  
|"("|O_BRACKET|  
|")"|C_BRACKET|  
|"=="|EQ|  
|"!="|NEQ|  
|"=~"|REGEXP_MATCH|  
|"!~"|REGEXP_NOT_MATCH|  
|"="|ASSEGNARE|  
|"& &"|AND|  
|problema|PROBLEMA|  
|tipo|TYPE|  
|valore|VALORE|  
|ValueType|VALUE_TYPE|  
|attestazione|ATTESTAZIONE|  
|"[_A-za-z] [_A-Za-z0-9] *"|IDENTIFICATORE|  
|"\\" [^\\"\n] *\\" "|STRINGA|  
|UInt64|UINT64_TYPE|  
|Int64|INT64_TYPE|  
|"string"|STRING_TYPE|  
|Boolean|BOOLEAN_TYPE|  
  
## <a name="language-syntax"></a>Sintassi del linguaggio  
Il linguaggio delle regole di trasformazione delle attestazioni seguente viene specificato in formato ABNF. Questa definizione usa i terminali specificati nella tabella precedente, oltre alle produzioni ABNF definite qui. Le regole devono essere codificate in UTF-16 e i confronti di stringa devono essere considerati senza distinzione tra maiuscole e minuscole.  
  
```  
Rule_set        = ;/*Empty*/  
             / Rules  
Rules         = Rule  
             / Rule Rules  
Rule          = Rule_body  
Rule_body       = (Conditions IMPLY Rule_action SEMICOLON)  
Conditions       = ;/*Empty*/  
             / Sel_condition_list  
Sel_condition_list   = Sel_condition  
             / (Sel_condition_list AND Sel_condition)  
Sel_condition     = Sel_condition_body  
             / (IDENTIFIER COLON Sel_condition_body)  
Sel_condition_body   = O_SQ_BRACKET Opt_cond_list C_SQ_BRACKET  
Opt_cond_list     = /*Empty*/  
             / Cond_list  
Cond_list       = Cond  
             / (Cond_list COMMA Cond)  
Cond          = Value_cond  
             / Type_cond  
Type_cond       = TYPE Cond_oper Literal_expr  
Value_cond       = (Val_cond COMMA Val_type_cond)  
             /(Val_type_cond COMMA Val_cond)  
Val_cond        = VALUE Cond_oper Literal_expr  
Val_type_cond     = VALUE_TYPE Cond_oper Value_type_literal  
claim_prop       = TYPE  
             / VALUE  
Cond_oper       = EQ  
             / NEQ  
             / REGEXP_MATCH  
             / REGEXP_NOT_MATCH  
Literal_expr      = Literal  
             / Value_type_literal  
  
Expr          = Literal  
             / Value_type_expr  
             / (IDENTIFIER DOT claim_prop)  
Value_type_expr    = Value_type_literal  
             /(IDENTIFIER DOT VALUE_TYPE)  
Value_type_literal   = INT64_TYPE  
             / UINT64_TYPE  
             / STRING_TYPE  
             / BOOLEAN_TYPE  
Literal        = STRING  
Rule_action      = ISSUE O_BRACKET Issue_params C_BRACKET  
Issue_params      = claim_copy  
             / claim_new  
claim_copy       = CLAIM ASSIGN IDENTIFIER  
claim_new       = claim_prop_assign_list  
claim_prop_assign_list = (claim_value_assign COMMA claim_type_assign)  
             /(claim_type_assign COMMA claim_value_assign)  
claim_value_assign   = (claim_val_assign COMMA claim_val_type_assign)  
             /(claim_val_type_assign COMMA claim_val_assign)  
claim_val_assign    = VALUE ASSIGN Expr  
claim_val_type_assign = VALUE_TYPE ASSIGN Value_type_expr  
Claim_type_assign   = TYPE ASSIGN Expr  
  
```  
  


