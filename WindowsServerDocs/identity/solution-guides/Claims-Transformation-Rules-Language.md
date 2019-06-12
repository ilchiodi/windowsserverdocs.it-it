---
ms.assetid: e831f781-3c45-4d44-b411-160d121d1324
title: Linguaggio delle regole di trasformazione delle attestazioni
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: a1f5c724d041a9f64c3b2697a8b5acd17a2a7bd9
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/31/2019
ms.locfileid: "66445817"
---
# <a name="claims-transformation-rules-language"></a>Linguaggio delle regole di trasformazione delle attestazioni

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

La foresta in attestazioni consente di funzionalità di trasformazione per controllo dinamico degli accessi limiti delle foreste di bridge di attestazioni per l'impostazione di criteri di trasformazione delle attestazioni nel trust tra foreste. Il componente principale di tutti i criteri è regole che vengono scritti nel linguaggio delle regole di trasformazione delle attestazioni. Questo argomento vengono fornite informazioni dettagliate su questo linguaggio e vengono fornite informazioni aggiuntive sulla creazione di regole di trasformazione delle attestazioni.  
  
I cmdlet di Windows PowerShell per i criteri di trasformazione su tra foreste e trust di avranno opzioni per impostare criteri semplici che sono necessarie in comune scenari. Questi cmdlet convertire l'input dell'utente in criteri e regole nel linguaggio delle regole di trasformazione attestazioni e quindi archiviano in Active Directory nel formato previsto. Per altre informazioni sui cmdlet per la trasformazione delle attestazioni, vedere la [cmdlet di Active Directory per controllo dinamico degli accessi](https://go.microsoft.com/fwlink/?LinkId=243150).  
  
A seconda della configurazione di attestazioni e i requisiti inseriti nella relazione di trust tra foreste in foreste di Active Directory, i criteri di trasformazione delle attestazioni debba risultare più complesse che i criteri supportati dai cmdlet di Windows PowerShell per Active Directory. Per creare in modo efficiente tali criteri, è essenziale per comprendere la sintassi del linguaggio delle regole di trasformazione attestazioni e la semantica. Questa attestazioni il linguaggio delle regole di trasformazione ("language") in Active Directory è un subset del linguaggio che viene utilizzato dagli [Active Directory Federation Services](https://go.microsoft.com/fwlink/?LinkId=243982) per simile a scopo e si ha una sintassi e semantica molto simili. Tuttavia, esistono un minor numero di operazioni consentite e nella versione di Active Directory del linguaggio vengono applicate restrizioni di sintassi aggiuntiva.  
  
In questo argomento viene illustrato brevemente la sintassi e semantica del linguaggio delle regole di trasformazione delle attestazioni in Active Directory e le considerazioni da prendere durante la creazione di criteri. Fornisce diversi insiemi di regole di esempio per iniziare a usare ed esempi di sintassi non corretta e i messaggi che generano, che consentono di decrittografare i messaggi di errore quando si creano le regole.  
  
## <a name="tools-for-authoring-claims-transformation-policies"></a>Strumenti per la creazione di criteri di trasformazione delle attestazioni  
**I cmdlet di Windows PowerShell per Active Directory**: Questo è il modo preferito e consigliato per creare e impostare criteri di trasformazione delle attestazioni. Questi cmdlet forniscono opzioni per i criteri semplici e verificare le regole impostate per i criteri più complessi.  
  
**LDAP**: Criteri di trasformazione delle attestazioni possono essere modificati in Active Directory Lightweight Directory Access Protocol (LDAP). Tuttavia, questa operazione è sconsigliata perché i criteri hanno diversi componenti complessi, e gli strumenti che usi il criterio non venga convalidato prima della scrittura su Active Directory. Successivamente, ciò può richiedere una notevole quantità di tempo per diagnosticare i problemi.  
  
## <a name="active-directory-claims-transformation-rules-language"></a>Linguaggio delle regole di trasformazione di attestazioni di Active Directory  
  
### <a name="syntax-overview"></a>Panoramica della sintassi  
Ecco una breve panoramica della sintassi e semantica del linguaggio:  
  
-   Il set di regole di trasformazione delle attestazioni è costituito da zero o più regole. Ogni regola è costituita da due parti attive: **Selezionare elenco condizioni** e **Rule Action**. Se il **Select List condizione** restituisce TRUE, viene eseguita l'azione di regola corrispondente.  
  
-   **Selezionare elenco condizioni** dispone di zero o più **seleziona condizioni**. Tutti i **seleziona condizioni** devono restituire TRUE per il **selezionare Elenco condizioni** restituisca TRUE.  
  
-   Ciascuna **Seleziona condizione** dispone di un insieme di zero o più **condizioni di corrispondenza**. Tutti i **condizioni di corrispondenza** devono restituire TRUE per la condizione di selezionare in modo che restituisca TRUE. Tutte queste condizioni vengono valutate a fronte di una singola attestazione. Un'attestazione che corrisponde a un **Seleziona condizione** può essere contrassegnata da un **identificatore** e fa riferimento nel **Rule Action**.  
  
-   Ogni **condizione di corrispondenza** specifica la condizione in modo che corrisponda il **tipo** oppure **valore** o **ValueType** di un'attestazione con diverso **Gli operatori della condizione** e **valori letterali stringa**.  
  
    -   Quando si specifica un **condizione di corrispondenza** per una **valore**, è necessario specificare anche un **condizione di corrispondenza** per uno specifico **ValueType** e viceversa. Queste condizioni devono essere uno accanto a altro nella sintassi.  
  
    -   **ValueType** condizioni di corrispondenza devono usare specifici **ValueType** solo valori letterali.  
  
-   Oggetto **Rule Action** possibile copiare un'attestazione che viene contrassegnata con un **identificatore** o rilasciare un'attestazione basata su un'attestazione che viene contrassegnata con un identificatore e/o specificato i valori letterali stringa.  
  
**Regola di esempio**  
  
In questo esempio illustra una regola che può essere utilizzata per tradurre le attestazioni di tipo tra due foreste, condizione che utilizzano le stesse attestazioni ValueType e avere la stesse interpretazioni per attestazioni, i valori per questo tipo. La regola ha una condizione di corrispondenza e un'istruzione di problema che utilizza i valori letterali stringa e un riferimento di attestazioni corrispondente.  
  
```  
C1: [TYPE=="EmployeeType"]    
                 => ISSUE (TYPE= "EmpType", VALUE = C1.VALUE, VALUETYPE = C1.VALUETYPE);  
[TYPE=="EmployeeType"] == Select Condition List with one Matching Condition for claims Type.  
ISSUE (TYPE= "EmpType", VALUE = C1.VALUE, VALUETYPE = C1.VALUETYPE) == Rule Action that issues a claims using string literal and matching claim referred with the Identifier.  
  
```  
  
### <a name="runtime-operation"></a>Operazione di runtime  
È importante comprendere il funzionamento del runtime di trasformazioni di attestazioni per creare le regole in modo efficace. L'operazione di runtime Usa tre set di attestazioni:  
  
1.  **Set di attestazioni in ingresso**: Set di attestazioni che vengono concesse per l'operazione di trasformazione delle attestazioni di input.  
  
2.  **Utilizzo di set di attestazioni**: Attestazioni intermedi che vengono letti da e scritte durante la trasformazione delle attestazioni.  
  
3.  **Set di attestazioni di output**: Output dell'operazione di trasformazione delle attestazioni.  
  
Ecco una breve panoramica dell'operazione di trasformazione delle attestazioni di runtime:  
  
1.  Le attestazioni di input per la trasformazione delle attestazioni vengono utilizzate per inizializzare il working set di attestazioni.  
  
    1.  Durante l'elaborazione di ogni regola, il working set di attestazioni viene utilizzato per le attestazioni di input.  
  
    2.  L'elenco di condizioni di selezione in una regola vengono confrontato con tutti i possibili set di attestazioni dal working set di attestazioni.  
  
    3.  Ogni set di attestazioni corrispondenti viene utilizzato per eseguire l'azione in tale regola.  
  
    4.  Esecuzione di una regola i risultati dell'azione in un'attestazione, che viene aggiunto all'output attestazioni set e set di attestazioni di lavoro. Pertanto, l'output di una regola viene utilizzata come input per le regole successive nel set di regole.  
  
2.  Le regole nel set di regole vengono elaborate in ordine sequenziale inizia con la prima regola.  
  
3.  Quando il set di regole intera viene elaborato, il set di attestazioni di output viene elaborato per rimuovere attestazioni duplicate e per altri problemi di sicurezza. Le attestazioni risultante sono l'output del processo di trasformazione delle attestazioni.  
  
È possibile scrivere trasformazioni di attestazioni complesse basate sul comportamento di runtime precedente.  
  
**Esempio: Operazione di runtime**  
  
In questo esempio viene illustrato il funzionamento di runtime di una trasformazione di attestazioni che usa due regole.  
  
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
Di seguito sono una sintassi speciale per le regole:  
  
1.  Set di regole vuoto nessuna attestazione di Output di = =  
  
2.  Svuotare selezionare Elenco condizioni = = corrispondenze di ogni attestazione l'elenco di condizioni selezionate  
  
    **Esempio: Elenco selezionare la condizione vuota**  
  
    La regola seguente corrisponde a ogni attestazione nel working set.  
  
    ```  
    => Issue (Type = "UserType", Value = "External", ValueType = "string")  
    ```  
  
3.  Svuotare selezionare elenco corrispondente = = tutte le corrispondenze di attestazione l'elenco di condizioni selezionate  
  
    **Esempio: Condizioni di corrispondenza vuota**  
  
    La regola seguente corrisponde a ogni attestazione nel working set. Si tratta della regola di base "Allow-all" Se viene usato da solo.  
  
    ```  
    C1:[] => Issule (claim = C1);  
    ```  
  
## <a name="security-considerations"></a>Considerazioni sulla sicurezza  
**Attestazioni che immettono una foresta**  
  
Le attestazioni presentate dal entità che sono in arrivo a una foresta dovranno essere controllati attentamente per assicurarsi che è consentire o rilasciare solo le attestazioni corrette. Attestazioni non corrette possono compromettere la sicurezza dell'insieme di strutture e deve essere di importanza fondamentale durante la creazione di criteri di trasformazione di attestazioni che immette un insieme di strutture.  
  
Active Directory ha le seguenti funzionalità per evitare errori di configurazione di attestazioni che immette un insieme di strutture:  
  
-   Se nessun criterio di trasformazione delle attestazioni impostato per le attestazioni che immettono una foresta, per motivi di sicurezza, dispone di un trust tra foreste Active Directory elimina tutte le attestazioni dell'entità immettere l'insieme di strutture.  
  
-   Se in esecuzione la regola impostata su attestazioni che passa i risultati di una foresta nelle attestazioni che non sono definite nella foresta, vengono eliminate le attestazioni non definite dalle attestazioni di output.  
  
**Attestazioni che lasciano una foresta**  
  
Le attestazioni che lasciano una foresta di presentano una questione di sicurezza minori per la foresta più le attestazioni che immettono l'insieme di strutture. Le attestazioni possono lasciare alla foresta, anche quando non sono disponibile nessuna attestazione corrispondente dei criteri di trasformazione. È inoltre possibile rilasciare attestazioni che non sono definite nell'insieme di strutture come parte della trasformazione delle attestazioni che lasciano l'insieme di strutture. Si tratta di impostare facilmente le relazioni di trust tra foreste con attestazioni. Un amministratore può stabilire se le attestazioni che immettono l'insieme di strutture devono essere trasformati e configurare i criteri appropriati. Ad esempio, un amministratore può impostare un criterio se è necessario nascondere un'attestazione per impedire la divulgazione di informazioni.  
  
**Errori di sintassi nelle regole di trasformazione delle attestazioni**  
  
Se un criterio di trasformazione delle attestazioni specificato dispone di un set di regole che è sintatticamente errato o se sono presenti altri problemi di sintassi o di archiviazione, il criterio viene considerato non valido. Ciò viene considerato in modo diverso rispetto alle condizioni di predefinito indicate in precedenza.  
  
Active Directory è in grado di determinare in questo caso la finalità e passa alla modalità di alternativo, in cui nessuna attestazione di output vengono generata su tale trust + direzione di attraversamento. Per risolvere il problema, è necessario l'intervento dell'amministratore. Questo problema può verificarsi se LDAP è possibile modificare i criteri di trasformazione delle attestazioni. I cmdlet di Windows PowerShell per Active Directory hanno la convalida in modo da evitare la scrittura di un criterio con problemi di sintassi.  
  
## <a name="other-language-considerations"></a>Altre considerazioni sul linguaggio  
  
1.  Esistono diverse parole chiave o i caratteri speciali in questa lingua (detta terminali). Questi vengono presentati nel [terminali Language](Claims-Transformation-Rules-Language.md#BKMK_LT) tabella più avanti in questo argomento. I messaggi di errore utilizzano i tag per questi terminali per la risoluzione dell'ambiguità.  
  
2.  I terminali in alcuni casi sono utilizzabile come valori letterali stringa. Tuttavia, tale utilizzo può entrare in conflitto con la definizione del linguaggio o avere conseguenze impreviste. Questo tipo di utilizzo non è consigliato.  
  
3.  L'azione della regola non è possibile eseguire conversioni di tipi in valori di attestazione e un set di regole contenente tale azione regola viene considerato non valido. Ciò provocherebbe un errore di runtime e nessuna attestazione di output vengono generata.  
  
4.  Se un'azione di regola fa riferimento a un identificatore che non è stato usato nella parte Select List condizione della regola, è un utilizzo non valido. Ciò provocherebbe un errore di sintassi.  
  
    **Esempio: Riferimento non corretto di identificatore**  
    La regola seguente illustra un identificatore non corretta utilizzato nell'azione della regola.  
  
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
  
-   **Non consentire un certo tipo di attestazione**  
    Tipo esatto  
  
    ```  
    C1:[type != "XYZ"] => Issue (claim=C1);  
    ```  
  
    Uso di Regex  
  
    ```  
    C1:[Type !~ "XYZ?"] => Issue (claim=C1);  
    ```  
  
## <a name="examples-of-rules-parser-errors"></a>Esempi di errori del parser di regole  
Regole di trasformazione delle attestazioni vengono analizzate da un parser personalizzato per verificare la presenza di errori di sintassi. Questo parser viene eseguito dal cmdlet di Windows PowerShell correlati prima di archiviare le regole in Active Directory. Gli eventuali errori durante l'analisi le regole, compresi gli errori di sintassi, vengono stampati sulla console. Controller di dominio eseguono inoltre il parser prima di usare le regole di trasformazione delle attestazioni e accedono gli errori nel registro eventi (aggiungere numeri di evento log).  
  
In questa sezione vengono illustrati alcuni esempi di regole che vengono scritti gli errori generati dal parser con sintassi non corretta e la sintassi corrispondente.  
  
1. Esempio:  
  
   ```  
   c1;[]=>Issue(claim=c1);  
   ```  
  
   Questo esempio è presente un punto e virgola in modo non corretto usato al posto di un carattere due punti.   
   **Messaggio di errore:**  
   *POLICY0002: Impossibile analizzare i dati dei criteri.*  
   *Numero di riga: 1, numero di colonna: Token di 2, errore:;. Riga: ' c1; [] = > Issue(claim=c1);'.*  
   *Errore del parser: 'POLICY0030: Errore di sintassi imprevisto ';', era previsto uno dei seguenti: ':'.'*  
  
2. Esempio:  
  
   ```  
   c1:[]=>Issue(claim=c2);  
   ```  
  
   In questo esempio, il tag di identificazione nell'istruzione di rilascio di copia non è definito.   
   **Messaggio di errore**:   
   *POLICY0011: Nessuna condizione nella regola di attestazione corrispondono al tag di condizione specificato nel CopyIssuanceStatement: 'c2'.*  
  
3. Esempio:  
  
   ```  
   c1:[type=="x1", value=="1", valuetype=="bool"]=>Issue(claim=c1)  
   ```  
  
   "bool" non è un terminale nel linguaggio e non è un tipo valore valido. I terminali validi sono elencati nel messaggio di errore seguente.   
   **Messaggio di errore:**  
   *POLICY0002: Impossibile analizzare i dati dei criteri.*  
   Numero di riga: 1, numero di colonna: Token di 39, errore: "bool". Line: 'c1:[type=="x1", value=="1",valuetype=="bool"]=>Issue(claim=c1);'.   
   *Errore del parser: 'POLICY0030: Errore di sintassi imprevisto 'STRING', è previsto uno dei seguenti: 'INT64_TYPE' 'UINT64_TYPE' 'STRING_TYPE' 'BOOLEAN_TYPE' 'IDENTIFIER'*  
  
4. Esempio:  
  
   ```  
   c1:[type=="x1", value==1, valuetype=="boolean"]=>Issue(claim=c1);  
   ```  
  
   Il numerale **1** in questo esempio non è un token valido nel linguaggio e tale utilizzo non è consentita in una condizione di corrispondenza. Deve essere racchiuso tra virgolette doppie per renderla una stringa.   
   **Messaggio di errore:**  
   *POLICY0002: Impossibile analizzare i dati dei criteri.*  
   *Numero di riga: 1, numero di colonna: 23, token di errore: 1. Line: 'c1:[type=="x1", value==1, valuetype=="bool"]=>Issue(claim=c1);'.* <em>Parser error: 'POLICY0029: Input imprevisto.</em>  
  
5. Esempio:  
  
   ```  
   c1:[type == "x1", value == "1", valuetype == "boolean"] =>   
  
        Issue(type = c1.type, value="0", valuetype == "boolean");  
   ```  
  
   In questo esempio viene utilizzato un segno di uguale doppio (= =) anziché un solo segno di uguale (=).   
   **Messaggio di errore:**  
   *POLICY0002: Impossibile analizzare i dati dei criteri.*  
   *Numero di riga: 1, numero di colonna: Errore 91, il token: = =. Line: 'c1:[type=="x1", value=="1",*  
   *valuetype=="boolean"]=>Issue(type=c1.type, value="0", valuetype=="boolean");'.*  
   *Errore del parser: 'POLICY0030: Errore di sintassi, non previsto '= =', è previsto uno dei seguenti: '='*  
  
6. Esempio:  
  
   ```  
   c1:[type=="x1", value=="boolean", valuetype=="string"] =>   
  
         Issue(type=c1.type, value=c1.value, valuetype = "string");  
   ```  
  
   In questo esempio è sintatticamente e semanticamente corretto. Tuttavia, l'utilizzo di "boolean" come valore stringa è associato a generare confusione e si consiglia di evitarlo. Come accennato in precedenza, usando i terminali di linguaggio come valori delle attestazioni devono essere evitati laddove possibile.  
  
## <a name="BKMK_LT"></a>Terminali di linguaggio  
La tabella seguente elenca il set completo di stringhe terminale e i terminali associate a una lingua che vengono usati nel linguaggio delle regole di trasformazione attestazioni. Queste definizioni usino le stringhe UTF-16 tra maiuscole e minuscole.  
  
|Stringa|Terminale|  
|----------|------------|  
|"=>"|IMPLICA|  
|";"|SEMICOLON|  
|":"|COLON|  
|","|COMMA|  
|"."|DOT|  
|"["|O_SQ_BRACKET|  
|"]"|C_SQ_BRACKET|  
|"("|O_BRACKET|  
|")"|C_BRACKET|  
|"=="|EQ|  
|"!="|NEQ|  
|"=~"|REGEXP_MATCH|  
|"!~"|REGEXP_NOT_MATCH|  
|"="|ASSIGN|  
|"&&"|E|  
|"issue"|PROBLEMA|  
|"type"|TYPE|  
|"valore"|VALORE|  
|"valuetype"|VALUE_TYPE|  
|"attestazione"|RICHIESTA|  
|"[_A-Za-z][_A-Za-z0-9]*"|IDENTIFICATORE|  
|"\\"[^\\"\n]*\\""|STRINGA|  
|"uint64"|UINT64_TYPE|  
|"int64"|INT64_TYPE|  
|"string"|STRING_TYPE|  
|"boolean"|BOOLEAN_TYPE|  
  
## <a name="language-syntax"></a>Sintassi del linguaggio  
Il linguaggio delle regole di trasformazione delle attestazioni seguente viene specificato usando ABNF. Questa definizione Usa i terminali specificate nella tabella precedente oltre le produzioni ABNF definiti qui. Le regole devono essere codificate in UTF-16 e i confronti tra stringhe deve essere trattato come maiuscole e minuscole.  
  
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
  


