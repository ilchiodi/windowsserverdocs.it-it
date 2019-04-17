---
ms.assetid: e831f781-3c45-4d44-b411-160d121d1324
title: Linguaggio delle regole di trasformazione delle attestazioni
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 4a6b378bc4aef180ebedd260008febaa2f2a76ae
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/12/2017
---
# <a name="claims-transformation-rules-language"></a>Linguaggio delle regole di trasformazione delle attestazioni

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

La foresta in attestazioni trasformazione funzionalità consente di bridge attestazioni per controllo dinamico degli accessi tra i limiti delle foreste mediante l'impostazione di criteri di trasformazione delle attestazioni sul trust tra foreste. Il componente principale di tutti i criteri è regole che vengono scritti nel linguaggio delle regole di trasformazione delle attestazioni. In questo argomento vengono fornite informazioni dettagliate su questa lingua e vengono fornite indicazioni sulla creazione di regole di trasformazione delle attestazioni.  
  
I cmdlet di Windows PowerShell per criteri di trasformazione su tra foreste considera attendibili sono opzioni per impostare i criteri semplici che sono necessari in comune gli scenari. Questi cmdlet tradurre l'input dell'utente in criteri e regole nel linguaggio delle regole di trasformazione delle attestazioni e quindi archiviarli in Active Directory in formato previsto. Per ulteriori informazioni sui cmdlet per la trasformazione delle attestazioni, vedere il [cmdlet di Active Directory per controllo dinamico degli accessi](https://go.microsoft.com/fwlink/?LinkId=243150).  
  
A seconda della configurazione di attestazioni e i requisiti inseriti nella relazione di trust tra foreste le foreste di Active Directory, i criteri di trasformazione delle attestazioni potrebbero dover essere più complesse rispetto ai criteri supportati tramite i cmdlet di Windows PowerShell per Active Directory. Per creare in modo efficace tali criteri, è essenziale per comprendere la sintassi del linguaggio delle regole attestazioni trasformazione e semantica. Questa attestazioni linguaggio delle regole di trasformazione ("la lingua") in Active Directory è un sottoinsieme del linguaggio che viene utilizzato da [Active Directory Federation Services](https://go.microsoft.com/fwlink/?LinkId=243982) simile e scopi, è una sintassi e semantica molto simili. Tuttavia, esistono meno operazioni consentite e restrizioni di sintassi aggiuntive vengono inserite nella versione di Active Directory della lingua.  
  
Questo argomento descrive brevemente la sintassi e semantica del linguaggio delle regole di trasformazione delle attestazioni in Active Directory e considerazioni per essere apportate durante la creazione di criteri. Fornisce diversi set di regole di esempio per iniziare ed esempi di sintassi non corretta e i messaggi che vengano generati, che consentono di decifrare messaggi di errore quando si creano le regole.  
  
## <a name="tools-for-authoring-claims-transformation-policies"></a>Strumenti per la creazione di criteri di trasformazione delle attestazioni  
**Cmdlet di Windows PowerShell per Active Directory**: questo è il modo preferito e consigliato per creare e impostare i criteri di trasformazione delle attestazioni. Questi cmdlet forniscono opzioni per i criteri semplici e verificare regole impostate per i criteri più complessi.  
  
**LDAP**: è possibile modificare i criteri di trasformazione delle attestazioni in Active Directory Lightweight Directory Access Protocol (LDAP). Tuttavia, questa operazione è sconsigliata perché i criteri hanno diversi componenti complessi e strumenti da utilizzare potrebbero non convalidare i criteri prima della scrittura per Active Directory. Successivamente, questo può richiedere una notevole quantità di tempo per diagnosticare i problemi.  
  
## <a name="active-directory-claims-transformation-rules-language"></a>Il linguaggio delle regole di trasformazione di attestazioni di Active Directory  
  
### <a name="syntax-overview"></a>Panoramica di sintassi  
Ecco una breve panoramica della sintassi e semantica del linguaggio:  
  
-   Il set di regole di trasformazione delle attestazioni è costituito da zero o più regole. Ogni regola è suddivisa in due parti active: **selezionare condizione** e **azione della regola**. Se il **selezionare condizione** restituisce TRUE, viene eseguito l'azione della regola corrispondente.  
  
-   **Selezionare elenco condizione** è zero o più **selezionare condizioni**. Tutti i **selezionare condizioni** deve restituire TRUE per il **selezionare condizione** in modo che restituisca TRUE.  
  
-   Ogni **selezionare condizione** include un set di zero o più **condizioni di corrispondenza**. Tutti i **condizioni di corrispondenza** deve restituire TRUE per la condizione selezionare in modo che restituisca TRUE. Tutte queste condizioni vengono valutate da una singola attestazione. Un'attestazione che corrisponde a un **selezionare condizione** possono essere contrassegnate da un **identificatore** e in cui il **azione della regola**.  
  
-   Ogni **condizione di corrispondenza** specifica la condizione per trovare la corrispondenza di **tipo** o **valore** o **ValueType** di un'attestazione con diversi **operatori condizione** e **stringhe letterali**.  
  
    -   Quando si specifica un **condizione di corrispondenza** per un **valore**, è necessario specificare anche un **condizione di corrispondenza** per uno specifico **ValueType** e viceversa. Queste condizioni devono essere uno accanto a altro nella sintassi.  
  
    -   **ValueType** corrispondenti condizioni deve usare specifico **ValueType** solo i valori letterali.  
  
-   Un **azione della regola** possibile copiare un'attestazione che viene contrassegnata con un **identificatore** o rilasciare un'attestazione in base a un'attestazione che viene contrassegnata con un identificatore e/o assegnato stringhe letterali.  
  
**Regola di esempio**  
  
Questo esempio mostra una regola che può essere usata per convertire il tipo di attestazioni tra due foreste, condizione che possano usare le attestazioni stesso ValueType e avere la stesse interpretazioni per attestazioni, i valori per questo tipo. La regola ha una condizione corrispondente e un'istruzione di problema che utilizza le stringhe letterali e un riferimento di attestazioni corrispondente.  
  
```  
C1: [TYPE=="EmployeeType"]    
                 => ISSUE (TYPE= "EmpType", VALUE = C1.VALUE, VALUETYPE = C1.VALUETYPE);  
[TYPE=="EmployeeType"] == Select Condition List with one Matching Condition for claims Type.  
ISSUE (TYPE= "EmpType", VALUE = C1.VALUE, VALUETYPE = C1.VALUETYPE) == Rule Action that issues a claims using string literal and matching claim referred with the Identifier.  
  
```  
  
### <a name="runtime-operation"></a>Operazione di runtime  
È importante comprendere il funzionamento di runtime di trasformazioni di attestazioni per creare le regole in modo efficace. L'operazione di runtime utilizza tre set di attestazioni:  
  
1.  **Set di attestazioni di input**: il set di attestazioni che viene assegnato all'operazione di trasformazione di attestazioni di input.  
  
2.  **Working set di attestazioni**: intermedio attestazioni che sono di lettura e scritte a durante la trasformazione di attestazioni.  
  
3.  **Set di attestazioni di output**: Output dell'operazione di trasformazione delle attestazioni.  
  
Ecco una breve panoramica dell'operazione di trasformazione di attestazioni di runtime:  
  
1.  Le attestazioni di input per la trasformazione di attestazioni vengono utilizzate per inizializzare il working set di attestazioni.  
  
    1.  Durante l'elaborazione di ogni regola, viene utilizzato il working set di attestazioni per le attestazioni di input.  
  
    2.  Elenco di condizioni di selezione in una regola viene confrontato con tutti i possibili set di attestazioni nel set di attestazioni di lavoro.  
  
    3.  Ogni set di attestazioni corrispondenti viene utilizzato per eseguire l'azione in tale regola.  
  
    4.  Esecuzione di una regola i risultati dell'azione in un'attestazione, che viene aggiunto all'output attestazioni set e il working set di attestazioni. Pertanto, l'output di una regola viene utilizzata come input per le regole successive nel set di regole.  
  
2.  Le regole nel set di regole vengono elaborate in ordine sequenziale a partire dalla prima regola.  
  
3.  Durante l'elaborazione del set di regole intero set di attestazioni di output viene elaborato per rimuovere attestazioni duplicate e per altri problemi di protezione. Le attestazioni risultante sono l'output del processo di trasformazione delle attestazioni.  
  
È possibile scrivere le trasformazioni di attestazioni complesse in base al comportamento di runtime precedente.  
  
**Esempio: Operazione di Runtime**  
  
Questo esempio illustra il funzionamento di runtime di una trasformazione di attestazioni che usa due regole.  
  
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
  
### <a name="special-rules-semantics"></a>Semantica di regole speciali  
Sintassi speciale per le regole sono i seguenti:  
  
1.  Svuotare i Set di regole = = non attestazioni di Output  
  
2.  Svuotare selezionare condizione = = ogni attestazione corrispondenze elenco selezionare condizione  
  
    **Esempio: Elenco vuoto selezionare condizione**  
  
    La regola seguente corrisponde a ogni attestazione nel working set.  
  
    ```  
    => Issue (Type = "UserType", Value = "External", ValueType = "string")  
    ```  
  
3.  Svuotare selezionare elenco corrispondenza = = corrispondenze ogni attestazione elenco selezionare condizione  
  
    **Esempio: Condizioni di corrispondenza vuoto**  
  
    La regola seguente corrisponde a ogni attestazione nel working set. Si tratta della regola "Consenti a tutti" base se viene utilizzato da solo.  
  
    ```  
    C1:[] => Issule (claim = C1);  
    ```  
  
## <a name="security-considerations"></a>Considerazioni sulla sicurezza  
**Attestazioni che Immetti un insieme di strutture**  
  
Le attestazioni presentate dall'entità che sono in ingresso a una foresta devono essere controllati attentamente per garantire che abbiamo consentire o rilasciare solo le attestazioni corrette. Attestazioni non corrette possono compromettere la sicurezza della foresta, e questo deve essere una considerazione superiore durante la creazione dei criteri di trasformazione di attestazioni che Immetti un insieme di strutture.  
  
Active Directory include le funzionalità seguenti per evitare errori di configurazione di attestazioni che Immetti un insieme di strutture:  
  
-   Se nessun criterio di trasformazione delle attestazioni impostare per le attestazioni che Immetti un insieme di strutture, per motivi di sicurezza, dispone di un trust tra foreste Active Directory elimina tutte le attestazioni dell'entità immettere l'insieme di strutture.  
  
-   Se la regola impostata su attestazioni che passa i risultati un insieme di strutture nelle attestazioni che non sono definite nella foresta è in esecuzione, le attestazioni non definite vengono eliminate dalle attestazioni di output.  
  
**Attestazioni che lasciare un insieme di strutture**  
  
Le attestazioni che lasciare un insieme di strutture presentano una minore protezione per la foresta più importante le attestazioni che immettere l'insieme di strutture. Le attestazioni sono autorizzate a lasciare alla foresta-anche quando non esiste nessuna attestazione corrispondente criteri di trasformazione in posizione. È inoltre possibile emettere attestazioni che non sono definite nella foresta come parte della trasformazione di attestazioni che lasciano l'insieme di strutture. Questo serve a configurare facilmente le relazioni di trust tra foreste con attestazioni. Un amministratore può determinare se le attestazioni che Immetti la foresta devono essere trasformato e impostare i criteri appropriati. Ad esempio, un amministratore può impostare un criterio se è necessario per nascondere un'attestazione per impedire la divulgazione di informazioni.  
  
**Errori di sintassi in regole di trasformazione delle attestazioni**  
  
Se un criterio di trasformazione di attestazioni specifico ha un set di regole che è errato o se sono presenti altri problemi di sintassi o di archiviazione, il criterio è considerato non valido. Questo è gestito in modo diverso rispetto alle condizioni predefinito indicate in precedenza.  
  
Active Directory è in grado di determinare l'intento in questo caso e passa a una modalità provvisoria, in cui non attestazioni di output generate in tale trust + direzione di scorrimento. Per risolvere il problema, è necessario l'intervento dell'amministratore. Questo problema può verificarsi se LDAP viene utilizzato per modificare i criteri di trasformazione delle attestazioni. Cmdlet di Windows PowerShell per Active Directory utilizza una convalida di impedire la scrittura di un criterio con problemi di sintassi.  
  
## <a name="other-language-considerations"></a>Altre considerazioni sulla lingua  
  
1.  Esistono diverse parole chiave o i caratteri speciali in tale lingua (detta terminali). Questi sono presentati nel [terminali lingua](Claims-Transformation-Rules-Language.md#BKMK_LT) tabella più avanti in questo argomento. I messaggi di errore di usare i tag per questi terminali per la rimozione di ambiguità.  
  
2.  Terminali a volte possono essere usati come stringhe letterali. Tuttavia, tale utilizzo potrebbe in conflitto con la definizione della lingua o hanno conseguenze indesiderate. Questo tipo di utilizzo non è consigliato.  
  
3.  L'azione della regola non può eseguire conversioni di tipi sui valori di attestazione e un set di regole che contiene tale azione regola è considerato non valido. Ciò potrebbe causare un errore di runtime, e non vengono eseguite attestazioni di output.  
  
4.  Se un'azione della regola fa riferimento a un identificatore che non è stato utilizzato nella parte selezionare condizione della regola, è un utilizzo non valido. Ciò potrebbe causare un errore di sintassi.  
  
    **Esempio: Riferimento identificatore errato**  
    La regola seguente illustra un identificatore errato utilizzato in azione della regola.  
  
    ```  
    C1:[] => Issue (claim = C2);  
    ```  
  
## <a name="sample-transformation-rules"></a>Regole di trasformazione di esempio  
  
-   **Consentire a tutte le richieste di un certo tipo**  
  
    Tipo esatto  
  
    ```  
    C1:[type=="XYZ"] => Issue (claim = C1);  
    ```  
  
    Uso delle espressioni regolari  
  
    ```  
    C1: [type =~ "XYZ*"] => Issue (claim = C1);  
    ```  
  
-   **Non consentire a un determinato tipo di attestazione**  
    Tipo esatto  
  
    ```  
    C1:[type != "XYZ"] => Issue (claim=C1);  
    ```  
  
    Uso delle espressioni regolari  
  
    ```  
    C1:[Type !~ "XYZ?"] => Issue (claim=C1);  
    ```  
  
## <a name="examples-of-rules-parser-errors"></a>Esempi di errori del parser di regole  
Regole di trasformazione delle attestazioni vengono analizzate da un parser personalizzato per controllare gli errori di sintassi. Il parser viene eseguito dal cmdlet Windows PowerShell correlati prima di archiviare le regole in Active Directory. Eventuali errori di analisi le regole, compresi gli errori di sintassi, vengono stampati nella console. Controller di dominio eseguono anche il parser prima di utilizzare le regole per la trasformazione di attestazioni e accedono errori nel registro eventi (aggiungere numeri registro eventi).  
  
In questa sezione vengono illustrati alcuni esempi di regole che vengono scritti con sintassi non corretta e la sintassi corrispondente gli errori generati dal parser.  
  
1.  Esempio:  
  
    ```  
    c1;[]=>Issue(claim=c1);  
    ```  
  
    Questo esempio ha un punto e virgola erroneamente usato al posto di due punti.   
    **Messaggio di errore:**  
    *POLICY0002: Impossibile analizzare dati dei criteri.*  
    *Numero di riga: 1, numero di colonna: 2, errore token:;. Riga: ' c1; [] = > Issue(claim=c1);'.*  
    *Errore del parser: ' POLICY0030: errore di sintassi, imprevisto ';', previsto uno dei seguenti: ':'.'*  
  
2.  Esempio:  
  
    ```  
    c1:[]=>Issue(claim=c2);  
    ```  
  
    In questo esempio, il tag di identificazione nell'istruzione di rilascio copia è definito.   
    **Messaggio di errore**:   
    *POLICY0011: Alcuna condizione della regola attestazione non corrispondano al tag di condizione specificato il CopyIssuanceStatement: 'c2'.*  
  
3.  Esempio:  
  
    ```  
    c1:[type=="x1", value=="1", valuetype=="bool"]=>Issue(claim=c1)  
    ```  
  
    "bool" non è un terminale nella lingua e non è un tipo di valore valido. Nel messaggio di errore seguente sono elencati terminali validi.   
    **Messaggio di errore:**  
    *POLICY0002: Impossibile analizzare dati dei criteri.*  
    Numero di riga: 1, numero di colonna: 39, token di errore: "bool". Riga: ' c1: [tipo = = "x1", valore = = "1", valuetype = = "bool"] = > Issue(claim=c1);'.   
    *Errore del parser: ' POLICY0030: errore di sintassi, imprevisto 'STRING', previsto uno dei seguenti: 'INT64_TYPE' 'UINT64_TYPE' 'STRING_TYPE' 'BOOLEAN_TYPE' 'Identificatore'*  
  
4.  Esempio:  
  
    ```  
    c1:[type=="x1", value==1, valuetype=="boolean"]=>Issue(claim=c1);  
    ```  
  
    Il numero **1** in questo esempio non è un token valido nel linguaggio di e non è consentito l'utilizzo in una condizione corrispondente. Deve essere racchiuso tra virgolette doppie per rendere una stringa.   
    **Messaggio di errore:**  
    *POLICY0002: Impossibile analizzare dati dei criteri.*  
    *Numero di riga: 1, numero di colonna: 23, errore token: 1. riga: ' c1: [tipo = = "x1", valore = = 1, valuetype = = "bool"] = > Issue(claim=c1);'. * *Errore del parser: ' POLICY0029: input imprevisto.*  
  
5.  Esempio:  
  
    ```  
    c1:[type == "x1", value == "1", valuetype == "boolean"] =>   
  
         Issue(type = c1.type, value="0", valuetype == "boolean");  
    ```  
  
    In questo esempio viene utilizzato un doppio segno di uguale (= =) invece di un unico segno di uguale (=).   
    **Messaggio di errore:**  
    *POLICY0002: Impossibile analizzare dati dei criteri.*  
    *Numero di riga: 1, numero di colonna: 91, errore token: = =. Riga: ' c1: [tipo = = "x1", valore = = "1",*  
    *ValueType = = "boolean"] = > problema (type=c1.type, valore = "0", valuetype = = "boolean");'.*  
    *Errore del parser: ' POLICY0030: errore di sintassi, imprevisto '= =' previsto uno dei seguenti: '='*  
  
6.  Esempio:  
  
    ```  
    c1:[type=="x1", value=="boolean", valuetype=="string"] =>   
  
          Issue(type=c1.type, value=c1.value, valuetype = "string");  
    ```  
  
    Questo esempio è sintatticamente e livello semantico corretto. Tuttavia, l'utilizzo "boolean" come creare confusione è associato un valore di stringa, e deve essere evitato. Come indicato in precedenza, utilizzando terminali lingua, come i valori delle attestazioni devono essere evitati laddove possibile.  
  
## <a name="BKMK_LT"></a>Terminali lingua  
La tabella seguente elenca il set completo di stringhe terminale e i terminali associate a una lingua che vengono utilizzati nel linguaggio delle regole di trasformazione delle attestazioni. Queste definizioni di usano stringhe UTF-16 tra maiuscole e minuscole.  
  
|Stringa|Terminal|  
|----------|------------|  
|"=>"|IMPLICA|  
|";"|PUNTO E VIRGOLA|  
|":"|I DUE PUNTI|  
|","|ELENCO DELIMITATO DA|  
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
|"&&"|E|  
|"problema"|PROBLEMA|  
|"tipo"|TIPO|  
|"value"|VALORE|  
|"valuetype"|VALUE_TYPE|  
|"reclamo"|ATTESTAZIONE|  
|"[_A-Za-z][_A-Za-z0-9]*"|IDENTIFICATORE|  
|"\\"[^\\"\n]*\\""|STRINGA|  
|"uint64"|UINT64_TYPE|  
|"int64"|INT64_TYPE|  
|"string"|STRING_TYPE|  
|"boolean"|BOOLEAN_TYPE|  
  
## <a name="language-syntax"></a>Sintassi del linguaggio  
Il linguaggio delle regole di trasformazione delle attestazioni seguenti viene specificato nel formato ABNF. Questa definizione Usa terminali sono specificate nella tabella precedente oltre la produzione ABNF qui definite. Le regole devono essere codificate in UTF-16 e devono essere trattati i confronti di stringhe tra maiuscole e minuscole.  
  
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
  


