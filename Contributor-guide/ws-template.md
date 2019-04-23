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
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59879562"
---
# <a name="metadata-and-markdown-template"></a>Modello di Markdown e metadati

Questo modello OPS contiene esempi di sintassi di Markdown, nonché indicazioni su come impostare i metadati. Per ottenere la maggior parte di esso, è necessario visualizzare sia le [Markdown non elaborato](https://raw.githubusercontent.com/Microsoft/WindowsServerDocs-pr/master/Contributor-guide/ws-template.md?token=AG1vEhARRHNLtPgKXP35BGjNZGajKOArks5YLNIwwA%3D%3D) e il [visualizzazione sottoposta a rendering](https://github.com/Microsoft/WindowsServerDocs-pr/blob/master/Contributor-guide/ws-template.md). (Il Markdown non elaborato Mostra il blocco dei metadati, mentre la visualizzazione sottoposta a rendering non.)

Quando si crea un file Markdown, è necessario copiare il modello in un nuovo file, compilare i metadati come specificato di seguito, impostare l'intestazione H1 sopra il titolo dell'articolo ed eliminare il contenuto. Qualsiasi elemento in CAPS racchiusa tra parentesi quadre richiede l'attenzione dell'utente.


## <a name="metadata"></a>Metadati 

Il blocco di metadati completo è superiore. Ecco alcuni punti importanti:

- Si **necessario** includere uno spazio tra i due punti (:) e il valore di un elemento dei metadati.
- I due punti in un valore (ad esempio, un titolo) interrompere il parser dei metadati. Al loro posto, usare la codifica HTML per i due punti dei `&#58;` (ad esempio, `"title: Azure Rights Management&#58; the basics | Azure RMS"`).
- **titolo**: Questo titolo verrà visualizzato nei risultati dei motori di ricerca. 
- **author**: Il campo dell'autore deve contenere il **nome utente di GitHub** dell'autore, non il relativo alias.
- **ms.prod**, **ms.technology**: Usare "windows-server-soglia" ms. Prod (o w10 se si usa questo modello per creare contenuto per Windows 10). Rivolgersi al contatto di CX per ottenere il valore ms. Technology.

## <a name="basic-markdown-gfm-and-special-characters"></a>Markdown di base, GFM e caratteri speciali

Markdown di base e gfm tutti è supportato. Per altre informazioni, vedere:

- [Sintassi Markdown di base](https://daringfireball.net/projects/markdown/syntax)
- [Documentazione di Markdown (GFM) specifico per GitHub](https://guides.github.com/features/mastering-markdown)

Markdown Usa caratteri speciali, ad esempio \*, \`, e \# per la formattazione. Se si vuole includere uno di questi caratteri nel contenuto, è necessario eseguire una delle seguenti operazioni:

- Inserire una barra rovesciata prima del carattere speciale come "escape" (ad esempio, \\ \* per un \*)
- Usare il [codice entità HTML](http://www.ascii.cl/htmlcodes.htm) per il carattere (ad esempio \& \#42\; per un &#42;).

## <a name="headings"></a>Intestazioni

Le intestazioni stile atx, ovvero usare 1-6 cancelletto (#) all'inizio della riga per indicare un'intestazione, corrispondenti ai livelli di intestazioni HTML da H1 a H6. Esempi di intestazioni di primo e secondo livello vengono usati in precedenza. 

Non esiste **necessario** da sola intestazione di primo livello (H1) nell'argomento, che verrà visualizzato come titolo pagina.  

Le intestazioni di secondo livello genereranno il sommario della pagina visualizzato nella sezione "In questo articolo" sotto il titolo pagina.

### <a name="third-level-heading"></a>Intestazione di terzo livello
#### <a name="fourth-level-heading"></a>Intestazione di quarto livello
##### <a name="fifth-level-heading"></a>Intestazione di quinto livello
###### <a name="sixth-level-heading"></a>Intestazione di sesto livello

## <a name="text-styling"></a>Stile del testo

*Corsivo* 

**Grassetto** 

~~Barrato~~

## <a name="links"></a>Collegamenti

### <a name="internal-links"></a>Collegamenti interni

Per creare un collegamento a un'intestazione nello stesso file Markdown, visualizzare l'origine dell'articolo pubblicato, trovare l'ID dell'intestazione (ad esempio, `id="blockquote"`) e crea un collegamento usando # + id (ad esempio, `#blockquote`).

- Esempio: [Blockquotes](#blockquote)

Per creare un collegamento a un file Markdown nello stesso repository, usare [collegamenti relativi](https://www.w3.org/TR/WD-html40-970917/htmlweb.html#h-5.1.2), includendo "MD" alla fine del nome del file.

- Esempio: [Suggerimenti e trucchi](tips-gotchas.md)
- Esempio: [Strumenti e il programma di installazione per i collaboratori](../readme.md)

Per creare un collegamento a un'intestazione in un file Markdown nello stesso repository, usare collegamenti relativi e collegamenti con hashtag.

- Esempio: [L'eliminazione di file](tips-gotchas.md#deleting-files)

### <a name="external-links"></a>Collegamenti esterni

Per creare un collegamento a un file esterno, usare l'URL completo come collegamento.

- Esempio: [GitHub](http://www.github.com)

Se viene visualizzato un URL in un file Markdown, questo verrà trasformato in un collegamento ipertestuale.

- Esempio: http://www.github.com

## <a name="lists"></a>Elenchi

### <a name="ordered-lists"></a>Elenchi ordinati

1. Questo 
1. è
1. Una
1. Ordinati
1. List  


#### <a name="ordered-list-with-an-embedded-list"></a>Elenco ordinato con un elenco incorporato

1. Qui
1. viene fornito
1. Un
1. Embedded
    1. Mancato riscontro Scarlett
    1. Il Professor viola
1. Ordinati
1. list


### <a name="unordered-lists"></a>Elenchi non ordinati

- Questo
- è impostata su
- a
- elenco puntato
- list


##### <a name="unordered-list-with-an-embedded-list"></a>Elenco non ordinato con elenco incorporato

- Questo 
- elenco puntato 
- list
    - Pica Mrs.
    - Il signor verde
- Contiene  
- altro
    1. Colonel senape
    1. Mrs vuoti
- Elenchi


## <a name="horizontal-rule"></a>Righello orizzontale

---

## <a name="tables"></a>Tabelle

In quasi tutte le istanze, utilizzare MD formattazione per le tabelle. Mentre le tabelle HTML offrono una maggiore flessibilità non viene utilizzate nei contenuti disponibili. Se si dispone di una tabella HTML nell'articolo, non si unirà tale articolo.

| Tabelle        | sono           | Bello  |
| ------------- |:-------------:| -----:|
| colonna 3      | allineato a destra | $1600 |
| colonna 2      | al centro      |   $12 |
| valore predefinito è 1 col | Allineato a sinistra     |    $1 |


## <a name="code"></a>Codice

### <a name="generic-codeblock"></a>Codeblock generico

Codice di quattro spazi per la codifica di codeblock generico di rientro.

    function fancyAlert(arg) {
      if(arg) {
        $.docs({div:'#foo'})
      }
    }


### <a name="codeblocks-with-language-identifier"></a>Codeblocks con identificatore di lingua

Usare tre apici (&#96;&#96;&#96;) e un ID di lingua per applicare un colore specifico del linguaggio di codifica per un blocco di codice.  Ecco l'elenco completo dei [ID lingua GitHub Flavored Markdown (GFM)](https://github.com/jmm/gfm-lang-ids/wiki/GitHub-Flavored-Markdown-(GFM)-language-IDs).

##### <a name="c9839"></a>C&#9839;

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

Usare apici (&#96;) per `inline code`.

## <a name="blockquotes"></a>Blockquotes

> La siccità si protraeva ormai per dieci milioni di anni, e il Regno dei terribili lucertole era lungo perché è terminata. Qui sull'equatore, nel continente che un giorno sarà noto Africa, la battaglia esistenza aveva raggiunto un nuovo diapason di ferocia, e il vincitore non sono ancora visibili. In questa ' s land quella e arida solo piccole o fulminee o feroci Impossibile prosperare, o sperare di sopravvivere.

## <a name="images"></a>Immagini

### <a name="static-image"></a>Immagine statica

![Questo è il testo alternativo](../windowsserverdocs/get-started/media/wsbanner.png)

### <a name="linked-image"></a>Immagine collegata

[![testo alternativo per l'immagine collegata](../windowsserverdocs/get-started/nano.png)](../windowsserverdocs/get-started/getting-started-with-nano-server.md) 

## <a name="alerts"></a>Avvisi

### <a name="note"></a>Nota

> [!NOTE]
> Questa è nota

### <a name="warning"></a>Avviso

> [!WARNING]
> Questo è avviso

### <a name="tip"></a>Suggerimento

> [!TIP]
> Questo è suggerimento

### <a name="important"></a>Importante

> [!IMPORTANT]
> Questo è importante

