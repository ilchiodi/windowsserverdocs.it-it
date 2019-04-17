---
title: Il ruolo del linguaggio di regola attestazione
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 07/07/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.assetid: dda9d148-d72f-4bff-aa2a-f2249fa47e4c
ms.technology: identity-adfs
ms.openlocfilehash: 206da33d33cbded0450db29615dc74eb1161cbce
ms.sourcegitcommit: 84a2bdcb92ba6af45781fab9727617e50fa5e911
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/07/2017
---
>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

# <a name="the-role-of-the-claim-rule-language"></a>Il ruolo del linguaggio di regola attestazione
Active Directory Federation Services (ADFS) di attestazione lingua delle regole agisce come il blocco predefinito amministrativo per il comportamento delle attestazioni in ingresso e in uscita, mentre il motore delle attestazioni funge da motore di elaborazione per la logica nel linguaggio di regola attestazione che definisce la regola personalizzata. Per ulteriori informazioni su come tutte le regole vengono elaborate dal motore delle attestazioni, vedere [The Role of the Claims Engine](The-Role-of-the-Claims-Engine.md).  
  
## <a name="creating-custom-claim-rules-using-the-claim-rule-language"></a>Creazione di regole attestazioni personalizzate usando il linguaggio di regola attestazione  
ADFS offre agli amministratori la possibilità di definire regole personalizzate che possono utilizzare per determinare il comportamento di attestazioni di identità con il linguaggio delle regole attestazione. È possibile utilizzare gli esempi di sintassi linguaggio di regola attestazione in questo argomento per creare una regola personalizzata che enumera, aggiunge, Elimina e modifica attestazioni in base alle esigenze dell'organizzazione. È possibile creare regole personalizzate digitando nella sintassi del linguaggio di regola attestazione nel **inviare attestazioni mediante un'attestazione personalizzata** modello di regola.  
  
Le regole sono separate da altro con un punto e virgola.  
  
Per ulteriori informazioni su quando usare regole personalizzate, vedere [quando usare una regola attestazione personalizzata](When-to-Use-a-Custom-Claim-Rule.md).  
  
## <a name="using-claim-rule-templates-to-learn-about-the-claim-rule-language-syntax"></a>Usando i modelli di regola attestazioni per apprendere la sintassi di linguaggio di regola attestazione  
ADFS offre anche un set di rilascio di attestazioni e modelli di regole di accettazione che è possibile utilizzare per implementare comuni attestazione regole attestazione. Nel **Modifica regole attestazioni** la finestra di dialogo per un trust specifico, è possibile creare una regola predefinita e visualizzare la sintassi del linguaggio di regola di attestazione che costituisce tale regola — facendo il **Visualizza lingua delle regole** scheda per tale regola. Utilizzando le informazioni in questa sezione e **Visualizza lingua delle regole** tecnica può fornire informazioni su come costruire regole personalizzate.  
  
Per ulteriori informazioni sulle regole attestazione e i modelli di regola attestazione, vedere [ruolo delle attestazioni regole](The-Role-of-Claim-Rules.md).  
  
## <a name="understanding-the-components-of-the-claim-rule-language"></a>Informazioni sui componenti del linguaggio di regola attestazione  
Il linguaggio di regola attestazione include i componenti seguenti, separati dal "= >" operatore:  
  
-   Una condizione  
  
-   Un'istruzione di rilascio  
  
### <a name="conditions"></a>Condizioni  
È possibile utilizzare le condizioni in una regola per controllare le attestazioni di input e determinare se è necessario eseguire l'istruzione di rilascio della regola. Una condizione rappresenta un'espressione logica che deve restituire true per eseguire la parte del corpo della regola. Se questa parte manca, si presuppone un true logico; ovvero, il corpo della regola viene sempre eseguito. La parte condizioni contiene un elenco di condizioni che vengono combinate con l'operatore logico di congiunzione ("& &"). Tutte le condizioni nell'elenco devono restituire true per l'intera parte condizionale restituisca true. La condizione può essere un operatore di selezione di attestazioni o una chiamata di funzione di aggregazione. Questi si escludono a vicenda, il che significa che i selettori di attestazioni e le funzioni di aggregazione non possono essere combinate in una parte di condizioni di singola regola.  
  
Le condizioni sono facoltative nelle regole. Ad esempio, la regola seguente non ha una condizione:  
  
```  
=> issue(type = "http://test/role", value = "employee");  
```  
  
Esistono tre tipi di condizioni:  
  
-   Singola condizione, questa è la forma più semplice di una condizione. Controlli vengono effettuati per sola espressione; ad esempio, windows account name = utente di dominio.  
  
-   Condizione multipla: questa condizione richiede controlli aggiuntivi per elaborare più espressioni nel corpo della regola; ad esempio, windows account name = utente di dominio e gruppo = contosopurchasers.  
  
> [!NOTE]  
> Esiste un'altra condizione, ma è un subset di singola condizione o condizione multipla. Viene definito come una condizione di espressione regolare (Regex). Viene usato per acquisire un'espressione di input e corrisponde all'espressione con un modello specifico. Può essere utilizzato un esempio di come è illustrato di seguito.  
  
Gli esempi seguenti mostrano alcune costruzioni di sintassi, basate sui tipi di condizione, che è possibile utilizzare per creare regole personalizzate.  
  
#### <a name="single--condition-examples"></a>Single - condizione esempi  
Single - condizioni dell'espressione sono descritti nella tabella seguente. Vengono costruite per controllare semplicemente la presenza di un'attestazione con un tipo di attestazione specificato o di un'attestazione con un tipo di attestazione specificato e il valore dell'attestazione.  
  
|Descrizione della condizione|Esempio di sintassi della condizione|  
|-------------------------|----------------------------|  
|Questa regola presenta una condizione per verificare la presenza di un'attestazione di input con un tipo di attestazione specificato ("http://test/name"). Se un'attestazione corrispondente si trova nelle attestazioni di input, la regola copia l'attestazione corrispondente o le attestazioni al set di attestazioni di output.|``` c: [type == "http://test/name"] => issue(claim = c );```|  
|Questa regola presenta una condizione per verificare la presenza di un'attestazione di input con un tipo di attestazione specificato ("http://test/name") e attestazione valore ("Terry"). Se un'attestazione corrispondente si trova nelle attestazioni di input, la regola copia l'attestazione corrispondente o le attestazioni al set di attestazioni di output.|``` c: [type == "http://test/name", value == "Terry"] => issue(claim = c);```|  
  
-Più condizioni complesse vengono visualizzate nella sezione successiva, incluse le condizioni per verificare la presenza di più attestazioni, le condizioni per controllare l'emittente di un'attestazione e condizioni controllare i valori che corrispondono a un modello di espressione regolare.  
  
#### <a name="multiple--condition-examples"></a>Esempi di condizione più-  
La tabella seguente fornisce un esempio di più - condizioni dell'espressione.  
  
|Descrizione della condizione|Esempio di sintassi della condizione|  
|-------------------------|----------------------------|  
|Questa regola presenta una condizione per verificare la presenza di due attestazioni di input, ognuno con un tipo di attestazione specificato ("http://test/name" e "http://test/email"). Se i due attestazioni corrispondenti si trovano nelle attestazioni di input, la regola copia l'attestazione del nome per il set di attestazioni di output.|``` c1: [type  == "http://test/name"] && c2: [type == "http://test/email"] => issue (claim  = c1 );```|  
  
#### <a name="regular--condition-examples"></a>Regolare - condizione esempi  
Nella tabella seguente fornisce un esempio di un'espressione regolare,-in condizione di base.  
  
|Descrizione della condizione|Esempio di sintassi della condizione|  
|-------------------------|----------------------------|  
|Questa regola presenta una condizione che usa un'espressione regolare per verificare una e-attestazione di posta elettronica che terminano con "@fabrikam.com". Se un'attestazione corrispondente viene trovata nelle attestazioni di input, la regola copia l'attestazione corrispondente al set di attestazioni di output.|```c: [type  == "http://test/email", value  =~ "^. +@fabrikam.com$" ] => issue (claim  = c );```|  
  
### <a name="issuance-statements"></a>Istruzioni di rilascio  
Regole personalizzate vengono elaborate in base alle istruzioni di rilascio (*problema* o *aggiungere* ) programmate nella regola di attestazione. A seconda del risultato desiderato, l'istruzione di issue o aggiungere può essere scritta nella regola per popolare il set di attestazioni di input o set di attestazioni di output. Una regola personalizzata che usa l'istruzione add in modo esplicito popola i valori solo per l'input mentre una regola attestazioni personalizzata che usa l'istruzione issue popola i valori di input set di attestazioni e nell'output del set di attestazioni di attestazione set di attestazioni di attestazione. Questo può essere utile quando un valore attestazione deve essere usato solo dalle regole future nel set di regole attestazione.  
  
Nella figura seguente, ad esempio, l'attestazione in ingresso viene aggiunto al set dal motore di rilascio delle attestazioni di attestazioni di input. Quando viene eseguita la prima regola attestazione personalizzata e vengono soddisfatti i criteri dell'utente di dominio, il motore di rilascio delle attestazioni elabora la logica della regola usando l'istruzione add e il valore di **Editor** viene aggiunto al set di attestazioni di input. Poiché il valore dell'Editor è presente nel set di attestazioni di input, la regola 2 può correttamente elaborare l'istruzione di rilascio nella propria logica e generare un nuovo valore di **Hello**, che viene aggiunto a entrambi l'output di set di attestazioni e impostare l'attestazione di input impostato per l'utilizzo per la regola successiva nella regola. Regola 3 ora possibile utilizzare tutti i valori presenti nel set come input per la logica di elaborazione di attestazioni di input.  
  
![Ruoli di AD FS](media/adfs2_customrule.gif)  
  
#### <a name="claim-issuance-actions"></a>Azioni di rilascio delle attestazioni  
Il corpo della regola rappresenta un'azione di rilascio delle attestazioni. Esistono due azioni di rilascio di attestazioni riconosciute dalla lingua:  
  
-   **Istruzione:** l'istruzione issue crea un'attestazione che viene aggiunta sia di input e output set di attestazioni. Ad esempio, l'istruzione seguente rilascia una nuova attestazione basata sul proprio set di attestazioni di input:  
  
    ```c:[type == "Name"] => issue(type = "Greeting", value = "Hello " + c.value);```  
  
-   **Istruzione add:** l'istruzione add crea una nuova attestazione che viene aggiunta solo alla raccolta di set di attestazioni di input. Ad esempio, l'istruzione seguente aggiunge una nuova attestazione al set di attestazioni di input:  
  
    ```c:[type == "Name", value == "domain user"] => add(type = "Role", value = "Editor");``` 
  
L'istruzione di rilascio di una regola definisce quali attestazioni verranno rilasciate dalla regola quando le condizioni sono soddisfatte. Esistono due forme di istruzioni di rilascio relative agli argomenti e al comportamento dell'istruzione:  
  
-   **Normale**: istruzioni di rilascio normali possono rilasciare attestazioni usando valori letterali nella regola o i valori delle attestazioni che soddisfano le condizioni. Un'istruzione di rilascio normale può essere costituito da uno o entrambi i formati seguenti:  
  
    -   *Copia attestazione*: la copia attestazione crea una copia dell'attestazione esistente nel set di attestazioni di output. Questo modulo di rilascio ha senso solo quando viene combinato con l'istruzione di rilascio "issue". Quando viene combinato con l'istruzione di rilascio "add", non ha alcun effetto.  
  
    -   *Nuova attestazione*: questo formato crea una nuova attestazione, data i valori per varie proprietà di attestazione. È necessario specificare Claim; tutte le altre proprietà di attestazione sono facoltative. L'ordine degli argomenti per questo modulo viene ignorato.  
  
-   **Archivio di attributi**: questo modulo consente di creare attestazioni con i valori che vengono recuperati da un archivio di attributi. È possibile creare più tipi di attestazione usando una singola istruzione di rilascio, importante per gli archivi di attributi che rendono rete o un disco operazioni di input/output (i/o) durante il recupero di attributi. Pertanto, è opportuno limitare il numero di roundtrip tra il motore dei criteri e l'archivio di attributi. È anche consentito creare più attestazioni per un determinato tipo di attestazione. Quando l'archivio di attributi restituisce più valori per un determinato tipo di attestazione, l'istruzione di rilascio crea automaticamente un'attestazione per ogni valore attestazione restituito. Un'implementazione dell'archivio di attributi Usa gli argomenti dei parametri per sostituire i segnaposto nell'argomento query con i valori forniti negli argomenti dei parametri. I segnaposto viene utilizzata la stessa sintassi come funzione .NET String. Format () (ad esempio, {1}, {2} e così via). L'ordine degli argomenti per questa forma di rilascio è importante e deve essere l'ordine previsto nella grammatica seguente.  
  
La tabella seguente descrive alcune costruzioni di sintassi comuni per entrambi i tipi di istruzioni di rilascio nelle regole attestazione.  
  
|Tipo di istruzione di rilascio|Descrizione dell'istruzione di rilascio|Esempio di sintassi dell'istruzione di rilascio|  
|---------------------------|----------------------------------|-------------------------------------|  
|Normale|La regola seguente rilascia sempre la stessa attestazione ogni volta che un utente ha il tipo di attestazione specificato e il valore:|```c: [type  == "http://test/employee", value  == "true"] => issue (type = "http://test/role", value = "employee");```|  
|Normale|La regola seguente converte un tipo di attestazione in un altro. Si noti che nell'istruzione di rilascio viene utilizzato il valore dell'attestazione che corrisponde alla condizione "c".|```c: [type  == "http://test/group" ] => issue (type  = "http://test/role", value  = c.Value );```|  
|Archivio di attributi|La regola seguente utilizza il valore di un'attestazione in ingresso per eseguire una query l'archivio di attributi di Active Directory:|```c: [Type  == "http://test/name" ] => issue (store  = "Enterprise AD Attribute Store", types  =  ("http://test/email" ), query  = ";mail;{0}", param  = c.Value )```|  
|Archivio di attributi|La regola seguente utilizza il valore di un'attestazione in ingresso per eseguire una query di un archivio di attributi Structured Query Language (SQL) configurato in precedenza:|```c: [type  == "http://test/name"] => issue (store  = "Custom SQL store", types  =  ("http://test/email","http://test/displayname" ), query  = "SELECT mail, displayname FROM users WHERE name ={0}", param  = c.value );```|  
  
#### <a name="expressions"></a>Espressioni  
Le espressioni vengono usate sul lato destro per entrambi i parametri di istruzione di rilascio e i vincoli del selettore di attestazioni. Esistono vari tipi di espressioni supportati dalla lingua. Tutte le espressioni nella lingua sono basate su stringa, il che significa che accettano stringhe come input e producono stringhe. Numeri o altri tipi di dati, ad esempio Data/ora, nelle espressioni non sono supportati. Di seguito sono i tipi di espressioni supportati dalla lingua:  
  
-   Valore letterale stringa: valore stringa, delimitato dal carattere di virgolette (") su entrambi i lati.  
  
-   Concatenazione di stringhe di espressioni: il risultato è una stringa che viene prodotta dalla concatenazione dei valori di sinistra e destra.  
  
-   Chiamata di funzione: la funzione è identificata da un identificatore e i parametri vengono passati come una virgola-elenco delimitato da virgole di espressioni racchiuse tra parentesi quadre ("()").  
  
-   Accesso alle proprietà dell'attestazione sotto forma di un nome proprietà DOT di nome della variabile: il risultato del valore della proprietà dell'attestazione identificata per una determinata valutazione della variabile. La variabile deve prima essere associata a un selettore di attestazioni prima che possa essere utilizzato in questo modo. Non è consentito usare la variabile associata a un selettore di attestazioni all'interno dei vincoli per tale stesso selettore di attestazioni.  
  
Le proprietà di attestazione seguenti sono disponibili per l'accesso:  
  
-   Claim  
  
-   Claim.Value  
  
-   Claim.Issuer  
  
-   Claim.OriginalIssuer  
  
-   Claim.ValueType  
  
-   Claim.Properties\[property\_name\] (questa proprietà restituisce una stringa vuota se Name la proprietà non viene trovato nella raccolta di proprietà dell'attestazione. )  
  
È possibile utilizzare la funzione RegexReplace per chiamare all'interno di un'espressione. Questa funzione richiede un'espressione di input e lo confronta con il modello specificato. Se il modello corrisponde, l'output della corrispondenza viene sostituito con il valore di sostituzione.  
  
#### <a name="exists-functions"></a>Funzioni Exists  
La funzione Exists può essere utilizzata in una condizione per valutare se un'attestazione che corrisponde che alla condizione esiste nell'input set di attestazioni. Se qualsiasi corrispondenza esiste, l'istruzione di rilascio viene chiamato una sola volta. Nell'esempio seguente, l'attestazione "origin" viene rilasciata esattamente una volta, se esiste almeno un'attestazione nella raccolta di set di attestazioni di input che l'emittente è impostato su "MSFT", indipendentemente da quanti attestazioni per le quali l'emittente è impostato su "MSFT". Uso di questa funzione impedisce il rilascio di attestazioni duplicate.  
  
```  
exists([issuer == "MSFT"])  
   => issue(type = "origin", value = "Microsoft");  
```  
  
## <a name="rule-body"></a>Corpo della regola  
Il corpo della regola può contenere solo una singola istruzione di rilascio. Se le condizioni vengono usate senza la funzione Exists, il corpo della regola viene eseguito una volta per ogni volta che la parte condizioni viene abbinata.  
  
## <a name="additional-references"></a>Riferimenti aggiuntivi  
[Creare una regola per inviare attestazioni mediante una regola personalizzata](https://technet.microsoft.com/library/dd807049.aspx)  
  

