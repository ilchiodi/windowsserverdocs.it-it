---
title: mapadmin
description: 'Argomento i comandi di Windows per * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: b17332c7-8622-4223-9c43-2fb9cf4d992d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 007256fffde11899d930c9197cade6d3bf9be42c
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59868782"
---
# <a name="mapadmin"></a>mapadmin



È possibile usare **Mapadmin** gestire Mapping nomi utente per Microsoft Services for Network File System.

## <a name="syntax"></a>Sintassi
```
mapadmin [<computer>] [-u <user> [-p <password>]]
mapadmin [<computer>] [-u <user> [-p <password>]] {start | stop}
mapadmin [<computer>] [-u <user> [-p <password>]] config <option[...]>
mapadmin [<computer>] [-u <user> [-p <password>]] add -wu <WindowsUser> -uu <UNIXUser> [-setprimary]
mapadmin [<computer>] [-u <user> [-p <password>]] add -wg <WindowsGroup> -ug <UNIXGroup> [-setprimary]
mapadmin [<computer>] [-u <user> [-p <password>]] setprimary -wu <WindowsUser> [-uu <UNIXUser>]
mapadmin [<computer>] [-u <user> [-p <password>]] setprimary -wg <WindowsGroup> [-ug <UNIXGroup>]
mapadmin [<computer>] [-u <user> [-p <password>]] delete <option[...]>
mapadmin [<computer>] [-u <user> [-p <password>]] list <option[...]>
mapadmin [<computer>] [-u <user> [-p <password>]] backup <filename> 
mapadmin [<computer>] [-u <user> [-p <password>]] restore <filename>
mapadmin [<computer>] [-u <user> [-p <password>]] adddomainmap -d <WindowsDomain> {-y <<NISdomain>> | -f <path>}
mapadmin [<computer>] [-u <user> [-p <password>]] removedomainmap -d <WindowsDomain> -y <<NISdomain>>
mapadmin [<computer>] [-u <user> [-p <password>]] removedomainmap -all
mapadmin [<computer>] [-u <user> [-p <password>]] listdomainmaps
```

## <a name="description"></a>Descrizione
Il **mapadmin** utilità della riga di comando consente di amministrare Mapping nomi utente nel computer locale o remoto che esegue Microsoft Services for Network File System. Se è connessi con un account che dispone di credenziali amministrative, è possibile specificare un nome utente e password di un account che esegue.

Oltre agli argomenti di comando specifico, **mapadmin** accetta gli argomenti e le opzioni seguenti:

&lt;computer&gt; specifica il computer remoto che esegue il servizio di Mapping nomi utente che si desidera amministrare. È possibile specificare il computer utilizzando un nome di Windows Internet Name Service (WINS) o un nome di sistema DNS (Domain Name) o indirizzo per IP (Internet Protocol).

-u &lt;utente&gt; specifica il nome utente dell'utente le cui credenziali devono essere utilizzati. Potrebbe essere necessario aggiungere il nome di dominio per il nome utente nel formato *domain***\\***nome utente*.

-p &lt;password&gt; specifica la password dell'utente. Se si specifica la **-u** opzione ma si omette il **-p** opzione, viene richiesto per la password dell'utente.
L'azione specifica che **mapadmin** esegue dipende l'argomento del comando si specifica:

## <a name="parameters"></a>Parametri
### <a name="start"></a>start
Avvia il servizio di Mapping nomi utente.

### <a name="stop"></a>stop
Arresta il servizio di Mapping nomi utente.

### <a name="config"></a>configurazione
Specifica le impostazioni generali per il Mapping tra nome utente. Le opzioni seguenti sono disponibili in questo argomento di comando: **- r &lt;dddd&gt;:&lt;hh&gt;:&lt;mm&gt;**  -specifica l'intervallo di aggiornamento per l'aggiornamento dal database di Windows e NIS in giorni, ore e minuti. L'intervallo minimo è 5 minuti.
**-i {Sì | nessun}** -attiva un mapping semplice (**yes**) o su off (**alcun**). Per impostazione predefinita, un mapping semplice è attivato.
**aggiungere** -crea un nuovo mapping di un utente o gruppo. Le opzioni seguenti sono disponibili con l'argomento di comando:

|Opzione|Definizione|
|-----|-------|
|-wu &lt;name&gt;|Specifica il nome dell'utente Windows per cui viene creato un nuovo mapping.|
|-uu &lt;nome&gt;|Specifica il nome dell'utente UNIX per cui viene creato un nuovo mapping.|
|-wg &lt;group&gt;|Specifica il nome del gruppo di Windows per cui viene creato un nuovo mapping.|
|-ug &lt;group&gt;|Specifica il nome del gruppo UNIX per cui viene creato un nuovo mapping.|
|-setprimary|Specifica che il nuovo mapping è il mapping principale.|

**setprimary** -specifica quali il mapping è il mapping di un gruppo o utente UNIX primario con più mapping. Le opzioni seguenti sono disponibili con l'argomento di comando:

|Opzione|Definizione|
|-----|-------|
|-wu &lt;name&gt;|Specifica l'utente di Windows del mapping principale. Se esiste più di un mapping per l'utente, usare il **- uu** opzione per specificare il mapping principale.|
|-uu &lt;nome&gt;|Specifica l'utente UNIX del mapping principale.|
|-wg &lt;group&gt;|Specifica il gruppo di Windows del mapping principale. Se esiste più di un mapping per il gruppo, usare il **- ug** opzione per specificare il mapping principale.|
|-ug &lt;group&gt;|Specifica il gruppo di UNIX del mapping principale.|

**Elimina** -rimuove il mapping per un utente o gruppo. Le opzioni seguenti sono disponibili per questo argomento di comando:

|Opzione|Definizione|
|-----|-------|
|-wu &lt;user&gt;|L'utente di Windows per il quale il mapping verrà eliminato, specificato come &lt; *WindowsDomain&gt;\\&lt;nome utente&gt;*. È necessario specificare il **- wu** o nella **- uu** opzione o entrambi. Se si specificano entrambe le opzioni, verrà eliminato il mapping specifico identificato da due opzioni. Se si specifica solo le **- wu** opzione, tutti i mapping per l'utente specificato verrà eliminato.|
|-wg &lt;group&gt;|Il gruppo di Windows per il quale il mapping verrà eliminato, specificato come &lt;WindowsDomain&gt;\\&lt;groupname&gt;. È necessario specificare il **wg -** o nella **- ug** opzione o entrambi. Se si specificano entrambe le opzioni, verrà eliminato il mapping specifico identificato da due opzioni. Se si specifica solo le **wg -** opzione, tutti i mapping verrà eliminato il gruppo specificato.|
|-uu &lt;user&gt;|L'utente UNIX per i quali il mapping verrà eliminato, specificato come &lt;nome utente&gt;. È necessario specificare il **- wu** o nella **- uu** opzione o entrambi. Se si specificano entrambe le opzioni, verrà eliminato il mapping specifico identificato da due opzioni. Se si specifica solo le **- uu** opzione, tutti i mapping per l'utente specificato verrà eliminato.|
|-ug &lt;group&gt;|Il gruppo di UNIX per il quale il mapping verrà eliminato, specificato come &lt;groupname&gt;. È necessario specificare il **wg -** o nella **- ug** opzione o entrambi. Se si specificano entrambe le opzioni, verrà eliminato il mapping specifico identificato da due opzioni. Se si specifica solo le **- ug** opzione, tutti i mapping verrà eliminato il gruppo specificato.|

**elenco** -Visualizza informazioni sui mapping di utenti e gruppi. Le opzioni seguenti sono disponibili con l'argomento di comando:

|Opzione|Definizione|
|-----|-------|
|-tutti|Elenca i mapping semplici e avanzati per utenti e gruppi.|
|-simple|Elenca tutti gli utenti con mapping semplici e i gruppi.|
|-avanzate|Elenca tutti gli utenti con mapping avanzati e i gruppi. Le mappe sono elencate nell'ordine in cui vengono valutati. Mappe primarie, contrassegnate con un asterisco (*), vengono elencate per primi, seguita da secondary esegue il mapping, che è contrassegnato con un accento circonflesso (^).|
|-wu &lt;name&gt;|Elenca i mapping per un utente di Windows specificato.|
|-wg &lt;group&gt;|Elenca i mapping per un gruppo di Windows.|
|-uu &lt;nome&gt;|Elenca i mapping per un utente di UNIX.|
|-ug &lt;group&gt;|Elenca i mapping per un gruppo di UNIX.|

**backup** : nome utente consente di salvare il Mapping di configurazione ed eseguire il mapping dei dati per il file specificato da &lt;filename&gt;.
**ripristinare** -sostituisce i dati di configurazione e il mapping con i dati del file (specificato da &lt;filename&gt;) che è stato creato utilizzando la **backup** argomento di comando.
**adddomainmap** -aggiunge una mappa semplice tra un file di gruppo o la password e dominio NIS e un dominio di Windows. Le opzioni seguenti sono disponibili per questo argomento di comando:

|Opzione|Definizione|
|-----|-------|
|-d &lt;WindowsDomain&gt;|Specifica il dominio di Windows per eseguire il mapping.|
|-y &lt;NISdomain&gt;|Specifica il dominio NIS da mappare. &lt;br /&gt;&lt;br /&gt;**- n** &lt;nisServer&gt; specifica il server NIS per il dominio NIS specificato con il **- y**opzione.|
|-f &lt;percorso&gt;|Specifica il percorso completo della directory contenente i file di password e gruppi per eseguire il mapping. I file devono trovarsi sul computer gestito e non è possibile usare **mapadmin** per gestire un computer remoto per configurare le mappe basate sul file di password e gruppi.|

**removedomainmap** -rimuove un mapping semplice tra un dominio di Windows e un dominio NIS. Le seguenti opzioni e argomenti sono disponibili per questo argomento di comando:

|Opzione|Definizione|
|-----|-------|
|-d &lt;WindowsDomain&gt;|Specifica il dominio di Windows della mappa da rimuovere.|
|-y &lt;NISdomain&gt;|Specifica il dominio NIS della mappa da rimuovere.|
|-tutti|Specifica che devono essere rimosse tutte le mappe semplice tra i domini NIS e Windows. Questo comando rimuoverà qualsiasi mapping semplice tra un file di gruppo e password e dominio di Windows.|

**listdomainmaps** -Elenca i domini di Windows che vengono eseguito il mapping ai domini NIS o ai file di password e gruppi.

## <a name="notes"></a>Note
-   Se non si specifica un argomento di comando **mapadmin** Visualizza le impostazioni correnti per il Mapping tra nome utente.
-   per tutte le opzioni che specificano un nome utente o gruppo, è possibile usare i formati seguenti:
-   per gli utenti di Windows, usare il modulo &lt;domain&gt;\\&lt;nome utente&gt;, \\ \\ &lt;computer&gt;\\&lt;utente nome&gt;, \\ &lt;computer&gt;\\&lt;nome utente&gt;, o &lt;computer&gt;\\&lt;utente nome&gt;
-   per i gruppi di Windows, usare il modulo &lt;domain&gt;\\&lt;&lt;groupname&gt;&gt;, \\ \\ &lt;computer&gt; \\ &lt; &lt;groupname&gt;&gt;, \\ &lt;computer&gt;\\&lt;&lt;groupname&gt; &gt;, o &lt;computer&gt;\\&lt;&lt;groupname&gt;&gt;
-   per gli utenti UNIX, usare il modulo &lt;NISdomain&gt;\\&lt;nome utente&gt;, &lt;nome utente&gt;@&lt;NISdomain&gt;, utente &lt;name&gt;@PCNFS, o PCNFS\\&lt;nome utente&gt;
-   per i gruppi di UNIX, usare il modulo &lt;NISdomain&gt;\\&lt;groupname&gt;, &lt;groupname&gt;@&lt;NISdomain&gt;, &lt;groupname&gt;@PCNFS, o PCNFS\\&lt;groupname&gt;

## <a name="additional-references"></a>Riferimenti aggiuntivi
[Chiave sintassi della riga di comando](command-line-syntax-key.md)
