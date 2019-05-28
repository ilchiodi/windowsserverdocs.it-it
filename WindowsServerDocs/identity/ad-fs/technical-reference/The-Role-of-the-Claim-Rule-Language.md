---
title: Ruolo del linguaggio delle regole attestazioni
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 07/07/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.assetid: dda9d148-d72f-4bff-aa2a-f2249fa47e4c
ms.technology: identity-adfs
ms.openlocfilehash: bf36f12803b8ba621f2249b53ad868fcd8f6c4a7
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/24/2019
ms.locfileid: "66188464"
---
# <a name="the-role-of-the-claim-rule-language"></a>Ruolo del linguaggio delle regole attestazioni
Active Directory Federation Services (ADFS) di attestazione lingua delle regole agisce come il blocco predefinito amministrativo per il comportamento di attestazioni in ingresso e in uscita, mentre il motore delle attestazioni agisce come il motore di elaborazione per la logica nel linguaggio di regola attestazione che definisce la regola personalizzata. Per ulteriori informazioni su come tutte le regole vengono elaborate dal motore di attestazioni, vedere [il ruolo del motore di attestazioni](The-Role-of-the-Claims-Engine.md).  
  
## <a name="creating-custom-claim-rules-using-the-claim-rule-language"></a>Creazione di regole attestazioni personalizzate usando il linguaggio delle regole attestazioni  
ADFS offre agli amministratori l'opzione per definire regole personalizzate che possono utilizzare per determinare il comportamento di attestazioni di identità con il linguaggio di regola attestazione. È possibile usare gli esempi di sintassi del linguaggio delle regole attestazioni in questo argomento per creare una regola personalizzata che enumera, aggiunge, elimina e modifica attestazioni in base alle esigenze dell'organizzazione. È possibile creare regole personalizzate digitando nella sintassi del linguaggio delle regole attestazioni nel modello di regole **Inviare attestazioni mediante una regola personalizzata**.  
  
Le regole sono separate le une dalle altre con un punto e virgola.  
  
Per ulteriori informazioni sull'utilizzo di regole personalizzate, vedere [quando utilizzare una regola attestazione personalizzata](When-to-Use-a-Custom-Claim-Rule.md).  
  
## <a name="using-claim-rule-templates-to-learn-about-the-claim-rule-language-syntax"></a>Uso di modelli di regole attestazioni per apprendere la sintassi del linguaggio delle regole attestazioni  
ADFS offre inoltre una serie di rilascio di attestazione predefiniti e modelli di regole di accettazione che è possibile utilizzare per implementare comuni attestazione regole attestazione. Nel **Modifica regole attestazione** la finestra di dialogo per una determinata relazione di trust, è possibile creare una regola predefinita e visualizzare la sintassi di linguaggio di regola attestazione che costituiscono tale regola, facendo la **Visualizza lingua delle regole** scheda per la regola. Utilizzando le informazioni in questa sezione e **Visualizza lingua delle regole** tecnica possibile consentono di comprendere come creare regole personalizzate.  
  
Per ulteriori informazioni sulle regole attestazione e i modelli di regola attestazione, vedere [ruolo delle attestazioni regole](The-Role-of-Claim-Rules.md).  
  
## <a name="understanding-the-components-of-the-claim-rule-language"></a>Informazioni sui componenti del linguaggio delle regole attestazioni  
Il linguaggio di regola attestazione include i componenti seguenti, separati dal "= >" operatore:  
  
-   Condizione  
  
-   Istruzione di rilascio  
  
### <a name="conditions"></a>Condizioni  
È possibile usare le condizioni in una regola per controllare le attestazioni di input e determinare se eseguire l'istruzione di rilascio della regola. Una condizione rappresenta un'espressione logica che deve restituire true per eseguire la parte del corpo della regola. Se questa parte manca, si presuppone un true logico; ciò significa che il corpo della regola viene sempre eseguito. La parte condizioni contiene un elenco di condizioni che vengono combinate con l'operatore logico di congiunzione ("& &"). Tutte le condizioni incluse nell'elenco devono restituire true affinché l'intera parte condizionale restituisca true. La condizione può essere un operatore di selezione di attestazioni o una chiamata di funzione di aggregazione. Questi si escludono a vicenda, il che significa che i selettori di attestazioni e le funzioni di aggregazione non possono essere combinati in una parte relativa alle condizioni di una singola regola.  
  
Le condizioni sono facoltative nelle regole. Ad esempio, la regola seguente non ha una condizione:  
  
```  
=> issue(type = "http://test/role", value = "employee");  
```  
  
Esistono tre tipi di condizioni:  
  
-   Singola condizione: è la forma più semplice di una condizione. Controlli vengono effettuati per sola espressione. ad esempio, windows account name = utente di dominio.  
  
-   Condizione multipla: questa condizione richiede controlli aggiuntivi per elaborare più espressioni nel corpo della regola; ad esempio, windows account name = utente di dominio e il gruppo = contosopurchasers.  
  
> [!NOTE]  
> Esiste un'altra condizione, ma è un subset di singola condizione o di una condizione multipla. Viene indicato come una condizione di espressione regolare (Regex). e viene usata per abbinare un'espressione di input a un modello specifico. Di seguito è riportato un esempio di come è possibile usarla.  
  
Gli esempi seguenti mostrano alcune costruzioni di sintassi, basate sui tipi di condizione, che è possibile usare per creare regole personalizzate.  
  
#### <a name="single--condition-examples"></a>Single - condizione esempi  
Single - delle condizioni dell'espressione sono descritte nella tabella seguente. Vengono costruite per controllare semplicemente la presenza di un'attestazione con un tipo di attestazione specificato o di un'attestazione con un tipo di attestazione valore dell'attestazione specificati.  
  
|Descrizione della condizione|Esempio di sintassi della condizione|  
|-------------------------|----------------------------|  
|Questa regola presenta una condizione per verificare la presenza di un'attestazione di input con un tipo di attestazione specificato ("http://test/name"). Se un'attestazione corrispondente si trova nelle attestazioni di input, la regola copia l'attestazione corrispondente o le attestazioni nel set di attestazioni di output.|``` c: [type == "http://test/name"] => issue(claim = c );```|  
|Questa regola presenta una condizione per verificare la presenza di un'attestazione di input con un tipo di attestazione specificato ("http://test/name") e il valore ("Terry") dell'attestazione. Se un'attestazione corrispondente si trova nelle attestazioni di input, la regola copia l'attestazione corrispondente o le attestazioni nel set di attestazioni di output.|``` c: [type == "http://test/name", value == "Terry"] => issue(claim = c);```|  
  
Informazioni - condizioni complesse vengono visualizzate nella sezione successiva, incluse le condizioni per verificare la presenza di più attestazioni, le condizioni per controllare l'emittente di un'attestazione e condizioni verificare la presenza di valori che corrispondono a un criterio di espressione regolare.  
  
#### <a name="multiple--condition-examples"></a>Esempi di condizione più:  
Nella tabella seguente viene fornito un esempio di più - delle condizioni dell'espressione.  
  
|Descrizione della condizione|Esempio di sintassi della condizione|  
|-------------------------|----------------------------|  
|Questa regola presenta una condizione da verificare per due input attestazioni, ognuno con un tipo di attestazione specificato ("http://test/name" e "http://test/email"). Se due attestazioni corrispondenti si trovano nelle attestazioni di input, la regola copia l'attestazione del nome nel set di attestazioni di output.|``` c1: [type  == "http://test/name"] && c2: [type == "http://test/email"] => issue (claim  = c1 );```|  
  
#### <a name="regular--condition-examples"></a>Regolare - condizione esempi  
Nella tabella seguente fornisce un esempio di un'espressione regolare,-in condizione di base.  
  
|Descrizione della condizione|Esempio di sintassi della condizione|  
|-------------------------|----------------------------|  
|Questa regola presenta una condizione che usa un'espressione regolare per verificare una e-attestazione di posta elettronica che terminano con "@fabrikam.com". Se un'attestazione corrispondente viene trovata nelle attestazioni di input, la regola copia l'attestazione corrispondente nel set di attestazioni di output.|```c: [type  == "http://test/email", value  =~ "^. +@fabrikam.com$" ] => issue (claim  = c );```|  
  
### <a name="issuance-statements"></a>Istruzioni di rilascio  
Regole personalizzate vengono elaborate in base a istruzioni di rilascio (*problema* oppure *aggiungere* ) programmate nella regola di attestazione. A seconda del risultato desiderato, l'istruzione di issue o add può essere scritta nella regola per popolare il set di attestazioni di input o il set di attestazioni di output. Una regola personalizzata che usa l'istruzione add in modo esplicito popola i valori di attestazione solo nel set di attestazioni di input mentre una regola attestazioni personalizzata che usa l'istruzione di emissione popola i valori di attestazione nel set di attestazioni di input e nel set di attestazioni di output. Può essere utile quando un valore attestazione deve essere usato solo dalle regole future nel set di regole attestazioni.  
  
Nella figura seguente, ad esempio, l'attestazione in ingresso viene aggiunta al set di attestazioni di input dal motore di rilascio delle attestazioni. Quando viene eseguita la prima regola attestazione personalizzata e i criteri dell'utente di dominio viene soddisfatta, il motore di rilascio delle attestazioni elabora la logica della regola usando l'istruzione add e il valore della **Editor** viene aggiunto al set di attestazioni di input. Poiché il valore di Editor è presente nel set di attestazioni di input, la regola 2 può elaborare l'istruzione di rilascio nella propria logica e generare un nuovo valore di **Hello**, che viene aggiunto sia al set di attestazioni di output che al set di attestazioni di input per essere usato dalla regola successiva nel set di regole. La regola 3 ora può usare tutti i valori presenti nel set di attestazioni di input come input per l'elaborazione della propria logica.  
  
![Ruoli di AD FS](media/adfs2_customrule.gif)  
  
#### <a name="claim-issuance-actions"></a>Azioni di rilascio di attestazioni  
Il corpo della regola rappresenta un'azione di rilascio di attestazioni. Esistono due azioni di rilascio di attestazioni riconosciute dalla lingua:  
  
-   **Istruzione issue:** l'istruzione issue crea un'attestazione che viene aggiunta sia al set di attestazioni di input che al set di attestazioni di output. Ad esempio, l'istruzione seguente rilascia una nuova attestazione basata sul proprio set di attestazioni di input:  
  
    ```c:[type == "Name"] => issue(type = "Greeting", value = "Hello " + c.value);```  
  
-   **Istruzione add:** l'istruzione add crea una nuova attestazione che viene aggiunta solo alla raccolta di set di attestazioni di input. Ad esempio, l'istruzione seguente aggiunge una nuova attestazione al set di attestazioni di input:  
  
    ```c:[type == "Name", value == "domain user"] => add(type = "Role", value = "Editor");``` 
  
L'istruzione di rilascio di una regola definisce quali attestazioni verranno rilasciate dalla regola quando le condizioni sono soddisfatte. Esistono due forme di istruzioni di rilascio relative agli argomenti e al comportamento dell'istruzione:  
  
-   **Normale**: le istruzioni di rilascio normali possono rilasciare attestazioni usando valori letterali nella regola o i valori delle attestazioni che soddisfano le condizioni. Un'istruzione di rilascio normale può essere costituita da uno o da entrambi i formati seguenti:  
  
    -   *Copia attestazione*: la copia attestazione crea una copia dell'attestazione esistente nel set di attestazioni di output. Questo modulo di rilascio ha senso solo quando viene combinato con l'istruzione di rilascio "issue". Quando viene combinato con l'istruzione di rilascio "add", non ha alcun effetto.  
  
    -   *Nuova attestazione*: Questo formato crea una nuova attestazione, data i valori per varie proprietà di attestazione. È necessario specificare Claim.Type; tutte le altre proprietà di attestazione sono facoltative. L'ordine degli argomenti per questo modulo viene ignorato.  
  
-   **Archivio di attributi**: questo modulo consente di creare attestazioni con i valori che vengono recuperati da un archivio di attributi. È possibile creare più tipi di attestazione usando una singola istruzione di rilascio, che è importante per gli archivi di attributi che rendono le operazioni di rete o un disco (i/o) di input/output durante il recupero di attributi. Quindi, è opportuno limitare il numero di roundtrip tra il motore dei criteri e l'archivio di attributi. È anche consentito creare più attestazioni per un determinato tipo di attestazione. Quando l'archivio di attributi restituisce più valori per un determinato tipo di attestazione, l'istruzione di rilascio crea automaticamente un'attestazione per ogni valore attestazione restituito. Un'implementazione dell'archivio di attributi usa gli argomenti dei parametri per sostituire i segnaposto nell'argomento query con i valori forniti negli argomenti dei parametri. I segnaposto usano la stessa sintassi come funzione .NET String. Format () (ad esempio, {1}, {2}e così via). L'ordine degli argomenti per questa forma di rilascio è importante e deve essere quello previsto nella grammatica seguente.  
  
La tabella seguente descrive alcune costruzioni di sintassi comuni per entrambi i tipi di istruzioni di rilascio nelle regole attestazioni.  
  
|Tipo di istruzione di rilascio|Descrizione dell'istruzione di rilascio|Esempio di sintassi dell'istruzione di rilascio|  
|---------------------------|----------------------------------|-------------------------------------|  
|Normale|La regola seguente rilascia sempre la stessa attestazione ogni volta che un utente ha il tipo e il valore attestazione specificati:|```c: [type  == "http://test/employee", value  == "true"] => issue (type = "http://test/role", value = "employee");```|  
|Normale|La regola seguente converte un tipo di attestazione in un altro. Si noti che nell'istruzione di rilascio viene usato il valore dell'attestazione corrispondente alla condizione "c".|```c: [type  == "http://test/group" ] => issue (type  = "http://test/role", value  = c.Value );```|  
|Archivio di attributi|La regola seguente usa il valore di un'attestazione in ingresso per eseguire una query nell'archivio di attributi di Active Directory:|```c: [Type  == "http://test/name" ] => issue (store  = "Enterprise AD Attribute Store", types  =  ("http://test/email" ), query  = ";mail;{0}", param  = c.Value )```|  
|Archivio di attributi|La regola seguente utilizza il valore di un'attestazione in ingresso per eseguire una query un archivio di attributi Structured Query Language (SQL) configurato in precedenza:|```c: [type  == "http://test/name"] => issue (store  = "Custom SQL store", types  =  ("http://test/email","http://test/displayname" ), query  = "SELECT mail, displayname FROM users WHERE name ={0}", param  = c.value );```|  
  
#### <a name="expressions"></a>Espressioni  
Le espressioni vengono usate sul lato destro sia per i vincoli del selettore di attestazioni che per i parametri delle istruzioni di rilascio. Esistono vari tipi di espressioni supportati dalla lingua. Tutte le espressioni nella lingua sono basate su stringa, cioè usano stringhe come input e producono stringhe. I numeri o altri tipi di dati, come data/ora, non sono supportati nelle espressioni. I tipi di espressioni supportati dalla lingua sono i seguenti:  
  
-   Valore letterale stringa: Valore stringa, delimitato dal carattere di virgolette doppie (") su entrambi i lati.  
  
-   Concatenazione di stringhe di espressioni: il risultato è una stringa che viene prodotta dalla concatenazione dei valori di sinistra e di destra.  
  
-   Chiamata di funzione: La funzione è identificata da un identificatore e i parametri vengono passati come una virgola: elenco delimitato da virgole di espressioni racchiuse tra parentesi quadre ("()").  
  
-   Accesso alle proprietà dell'attestazione in forma di nome proprietà DOT di un nome variabile: il risultato del valore della proprietà dell'attestazione identificata per una determinata valutazione della variabile. La variabile deve prima essere associata a un selettore di attestazioni per poter essere usata in questo modo. Non è consentito usare la variabile associata a un selettore di attestazioni all'interno dei vincoli per lo stesso selettore di attestazioni.  
  
Le proprietà di attestazione seguenti sono disponibili per l'accesso:  
  
-   Claim.Type  
  
-   Claim.Value  
  
-   Claim.Issuer  
  
-   Claim.OriginalIssuer  
  
-   Claim.ValueType  
  
-   Claim. Properties\[proprietà\_nome\] (questa proprietà restituisce una stringa vuota se la nome proprietà non viene trovato nella raccolta delle proprietà dell'attestazione. )  
  
È possibile usare la funzione RegexReplace per chiamare all'interno di un'espressione. Questa funzione abbina un'espressione di input al modello specificato. Se il modello corrisponde, l'output della corrispondenza viene sostituito con il valore di sostituzione.  
  
#### <a name="exists-functions"></a>Funzioni Exists  
La funzione Exists può essere usata in una condizione per valutare se un'attestazione che corrisponde alla condizione esiste nel set di attestazioni di input. Se la corrispondenza esiste, l'istruzione di rilascio viene chiamata una sola volta. Nell'esempio seguente, l'attestazione "origin" viene rilasciata esattamente una volta se esiste almeno un'attestazione nella raccolta di set di attestazioni di input per la quale l'emittente è impostato su "MSFT", indipendentemente dal numero di attestazioni per le quali l'emittente è impostato su "MSFT". L'uso di questa funzione impedisce il rilascio di attestazioni duplicate.  
  
```  
exists([issuer == "MSFT"])  
   => issue(type = "origin", value = "Microsoft");  
```  
  
## <a name="rule-body"></a>Corpo della regola  
Il corpo della regola può contenere solo una singola istruzione di rilascio. Se le condizioni vengono usate senza la funzione Exists, il corpo della regola viene eseguito una sola volta per ogni volta in cui la parte delle condizioni viene abbinata.  
  
## <a name="additional-references"></a>Altri riferimenti  
[Creare una regola per inviare attestazioni mediante una regola personalizzata](https://technet.microsoft.com/library/dd807049.aspx)  
  

