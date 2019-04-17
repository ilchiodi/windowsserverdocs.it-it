---
title:
- TITOLO DELL'ARTICOLO
description: ''
author:
- GITHUB USERNAME
ms.author:
- MICROSOFT ALIAS
ms.date:
- DATE
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: ''
ms.localizationpriority:
- high/medium/low
ms.openlocfilehash: 4f885680426c0bfa55d5f73a7ef0c2143a8dd5a9
ms.sourcegitcommit: e0479b0114eac7f232e8b1e45eeede96ccd72b26
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 06/22/2018
ms.locfileid: "2082253"
---
# <a name="metadata-and-markdown-template"></a>I metadati e ribasso modello

Questo modello di operazioni contiene esempi di sintassi ribasso, nonché informazioni aggiuntive sull'impostazione dei metadati. Per ottenere la maggior parte di esso, è necessario visualizzare [ribasso non elaborato](https://raw.githubusercontent.com/Microsoft/WindowsServerDocs-pr/master/Contributor-guide/ws-template.md?token=AG1vEhARRHNLtPgKXP35BGjNZGajKOArks5YLNIwwA%3D%3D) e [il rendering di visualizzazione](https://github.com/Microsoft/WindowsServerDocs-pr/blob/master/Contributor-guide/ws-template.md). (Non elaborato ribasso Mostra il blocco di metadati, mentre la visualizzazione viene eseguito il rendering non)

Quando si crea un file ribasso, è consigliabile copiare il modello in un nuovo file, compilare i metadati da insieme specificato di seguito, il titolo di s1 sopra per il titolo dell'articolo ed eliminare il contenuto. Lasciare in maiuscolo parentesi quadre richiede l'attenzione dell'utente.


## <a name="metadata"></a>Metadati 

Il blocco completo metadati è sopra. Alcune note principali:

- È **necessario** disporre di uno spazio tra i due punti (:) e il valore di un elemento di metadati.
- I due punti (ad esempio, un titolo) un valore interrompono il parser dei metadati. Al loro posto, utilizzare la codifica HTML per i due punti di `&#58;` (ad esempio `"title: Azure Rights Management&#58; the basics | Azure RMS"`).
- **titolo**: che apparirà nei risultati dei motori di ricerca. 
- **Author**: nel campo autore deve includere il **nome utente GitHub** dell'autore, non il loro alias.
- **ms.Prod**, **ms.technology**: utilizzare "soglia di server windows" per ms.prod (o w10 se si utilizza il modello per creare contenuto per Windows 10). Parlare con il contatto CX per ottenere il valore ms.technology.

## <a name="basic-markdown-gfm-and-special-characters"></a>Base ribasso, GFM e caratteri speciali

Tutti i ribasso di base e flavored GitHub è supportato. Per ulteriori informazioni su tali funzionalità, vedere:

- [Sintassi ribasso previsto](https://daringfireball.net/projects/markdown/syntax)
- [Documentazione delle vendite promozionali (GFM) flavored GitHub](https://guides.github.com/features/mastering-markdown)

Ribasso utilizza caratteri speciali come \ *, \ ", e \ # per la formattazione. Se si desidera includere uno dei caratteri nel contenuto, è necessario eseguire una delle seguenti operazioni:

- Inserire una barra rovesciata prima del carattere speciale "escape" viene (ad esempio, \ \ \ * per un \ *)
- Utilizzare il [codice HTML entità](http://www.ascii.cl/htmlcodes.htm) per il carattere (ad esempio \ & \#42\, ad un & #42;).

## <a name="headings"></a>Titoli

Titoli devono essere eseguiti utilizzando lo stile atx, vale a dire utilizzare 1 a 6 hash caratteri (#) all'inizio della linea per indicare un titolo, corrispondente ai livelli di intestazioni HTML H1 a H6. Esempi delle intestazioni di primo e secondo livello vengono utilizzati in precedenza. 

Non vi **devono** essere solo un titolo di primo livello (H1) nell'argomento, verrà visualizzata come titolo nella pagina.  

Le intestazioni di secondo livello genera il sommario nella pagina visualizzata nella sezione "In questo articolo" di sotto del titolo nella pagina.

### <a name="third-level-heading"></a>Intestazione di terzo livello
#### <a name="fourth-level-heading"></a>Titolo quarto livello
##### <a name="fifth-level-heading"></a>Intestazione livello quinto
###### <a name="sixth-level-heading"></a>Intestazione livello di sesto

## <a name="text-styling"></a>Stile testo

*Corsivo* 

**Bold** 

~~Barrato~~

## <a name="links"></a>Collegamenti

### <a name="internal-links"></a>Collegamenti interni

Per creare un collegamento a un'intestazione dello stesso file ribasso, visualizzare l'origine dell'articolo pubblicato, cercare l'ID del capo (ad esempio, `id="blockquote"`), utilizzando # + id dei collegamenti e (ad esempio, `#blockquote`).

- Esempio: [Blockquotes](#blockquote)

Per creare un collegamento a un file ribasso nella stessa repo, utilizzare [i collegamenti relativi](https://www.w3.org/TR/WD-html40-970917/htmlweb.html#h-5.1.2), ad esempio "MD" alla fine del nome file.

- Esempio: [trucchi o suggerimenti](tips-gotchas.md)
- Esempio: [Strumenti e il programma di installazione per i collaboratori](../readme.md)

Per creare un collegamento a un'intestazione in un file ribasso nella stessa repo, utilizzare il collegamento relativo + hashtag collegamento.

- Esempio: [Eliminazione di file](tips-gotchas.md#deleting-files)

### <a name="external-links"></a>Collegamenti esterni

Per creare un collegamento a un file esterno, utilizzare l'URL completo come il collegamento.

- Esempio: [GitHub](http://www.github.com)

Se è presente un URL in un file delle vendite promozionali, verrà trasformato in un collegamento ipertestuale.

- Esempio:http://www.github.com

## <a name="lists"></a>Elenchi

### <a name="ordered-lists"></a>Elenchi ordinati

1. In questo 
1. È
1. Un
1. Ordinati
1. List  


#### <a name="ordered-list-with-an-embedded-list"></a>Elenco con un elenco incorporato ordinato

1. Qui
1. la forma viene
1. un
1. incorporato
    1. Mancato Scarlett
    1. Prugna docente
1. ordinati
1. list


### <a name="unordered-lists"></a>Elenchi non ordinati

- In questo
-  sia 
- a
- elenco puntato
- list


##### <a name="unordered-list-with-an-embedded-list"></a>Elenco ordinato con un elenco incorporato

- In questo 
- elenco puntato 
- list
    - Coda di pavone Signori
    - Verde Sig.
- contiene  
- altro
    1. Senape colonel
    1. Signori bianco
- elenchi


## <a name="horizontal-rule"></a>Righello orizzontale

---

## <a name="tables"></a>Tabelle

In quasi tutte le istanze, utilizzare MD la formattazione per le tabelle. Mentre le tabelle HTML garantiscono una maggiore flessibilità è non utilizzare il contenuto. Se si dispone di una tabella HTML nell'articolo, non si unirà suddetto articolo.

| Tabelle        | Sono           | Bello  |
| ------------- |:-------------:| -----:|
| colonna 3 viene      | allineato a destra | $1600 |
| colonna 2 è      | allineato al centro      |   $12 |
| colonna 1 è predefinito | allineato a sinistra     |    $1 |


## <a name="code"></a>Codice

### <a name="generic-codeblock"></a>Codeblock generico

Codice quattro spazi per la scrittura di codice codeblock generico di rientro.

    function fancyAlert(arg) {
      if(arg) {
        $.docs({div:'#foo'})
      }
    }


### <a name="codeblocks-with-language-identifier"></a>Codeblocks con identificatore di lingua

Utilizzare tre backticks (& #96; & #96; #96;) + al codice in un ID di lingua per applicare un colore specifici della lingua per un blocco di codice.  Ecco l'intero elenco di [ID di lingua GitHub Flavored ribasso (GFM)](https://github.com/jmm/gfm-lang-ids/wiki/GitHub-Flavored-Markdown-(GFM)-language-IDs).

##### <a name="c9839"></a>C & #9839;

```c#
using System;
namespace HelloWorld
{
    class Hello 
    {
        static void Main() 
        {
            Console.WriteLine("Hello World!");

            // Keep the console window open in debug mode.
            Console.WriteLine("Press any key to exit.");
            Console.ReadKey();
        }
    }
}
```
#### <a name="python"></a>Python

```python
friends = ['john', 'pat', 'gary', 'michael']
for i, name in enumerate(friends):
    print "iteration {iteration} is {name}".format(iteration=i, name=name)
```
#### <a name="powershell"></a>PowerShell

```powershell
Clear-Host
$Directory = "C:\Windows\"
$Files = Get-Childitem $Directory -recurse -Include *.log `
-ErrorAction SilentlyContinue
```

### <a name="inline-code"></a>Codice inline

Utilizzare backticks (96 #;) per `inline code`.

## <a name="blockquotes"></a>Blockquotes

> La siccità hanno durata ora per gli anni dieci milioni e Regno del lizards ad era tempo terminata. Di seguito su equatore, in continenti che un solo giorno sarà noto come Africa, ruolo di rilievo esistenza ha raggiunto un nuovo climax ferocity e il victor non è stato ancora visibili. In questo suolo barren ed essicata solo il funzione piccolo o il codice swift il forte potrebbe decorazioni o anche una sorpresa di sopravvivenza.

## <a name="images"></a>Immagini

### <a name="static-image"></a>Immagine statica

![si tratta del testo alternativo](../windowsserverdocs/get-started/media/wsbanner.png)

### <a name="linked-image"></a>Immagine collegata

[![atesto lt per immagine collegata](../windowsserverdocs/get-started/nano.png)](../windowsserverdocs/get-started/getting-started-with-nano-server.md) 

## <a name="alerts"></a>Avvisi

### <a name="note"></a>Nota

> [!NOTE]
> Si tratta di NOTE

### <a name="warning"></a>Warning

> [!WARNING]
> Questo avviso

### <a name="tip"></a>Suggerimento

> [!TIP]
> Si tratta di suggerimento

### <a name="important"></a>Importante

> [!IMPORTANT]
> Questo è importante

