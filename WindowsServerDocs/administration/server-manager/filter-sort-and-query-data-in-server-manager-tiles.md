---
title: Filtrare e ordinare i dati ed eseguire query su di essi nei riquadri di Server Manager
description: Server Manager
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-server-manager
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 8786f791-73e5-4c75-8d12-46e88a196976
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 51c0e1f3af727c4e7ddf18024fab9a95808d2338
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59868912"
---
# <a name="filter-sort-and-query-data-in-server-manager-tiles"></a>Filtrare e ordinare i dati ed eseguire query su di essi nei riquadri di Server Manager

>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

In Windows Server, a sezioni in Server Manager consentono filtrare e ordinare i dati, creare e salvare query personalizzate. È possibile ordinare, utilizzare filtri ed eseguire query sulle voci di elenco i riquadri eventi, prestazioni, Best Practices Analyzer, servizi e i ruoli e funzionalità in server pagine ruolo o gruppo di Server Manager.  
  
In questo argomento sono incluse le sezioni seguenti.  
  
-   [Filtrare le voci dell'elenco nei riquadri](#BKMK_tiles)  
  
-   [Ordina le voci dell'elenco nei riquadri](#BKMK_sort)  
  
-   [creare ed eseguire query personalizzate nei dati del riquadro](#BKMK_query)  
  
## <a name="BKMK_tiles"></a>Filtrare le voci dell'elenco nei riquadri  
La casella di testo **Filtro** consente di ridurre l'elenco di voci visualizzate in un riquadro mostrando solo quelle che contengono una stringa di testo specificata.  
  
#### <a name="to-apply-a-filter-to-the-list-of-entries-in-a-tile"></a>Per applicare un filtro all'elenco di voci in un riquadro  
  
1.  Aprire una pagina di gruppo di ruoli o di server in Server Manager.  
  
2.  Nel **filtro** casella di testo di un riquadro eventi, prestazioni, Best Practices Analyzer, servizi o i ruoli e funzionalità, digitare una stringa in cui si desidera filtrare.  
  
    ad esempio, se si desidera visualizzare solo gli eventi con ID 1014, digitare **1014** nel **filtro** casella di testo. Tutti gli eventi raccolti che contengono la stringa **1014** in almeno un campo vengono restituiti come risultati.  
  
3.  Il filtro modifica il testo della descrizione sotto il titolo del riquadro. Non viene più visualizzato **Tutti**, ma **Risultati filtrati**.  
  
4.  Per cancellare il filtro, eliminare la stringa nella casella del filtro oppure fare clic su **X**.  
  
## <a name="BKMK_sort"></a>Ordina le voci dell'elenco nei riquadri  
Fare clic sulle intestazioni di colonna per ordinare le voci dell'elenco nei riquadri di Server Manager. Se si fa clic su un'intestazione di colonna i valori della colonna vengono visualizzati in ordine alfanumerico crescente (freccia rivolta verso l'alto). Se si fa di nuovo clic, i valori della colonna vengono visualizzati in ordine alfanumerico decrescente (freccia rivolta verso il basso).  
  
## <a name="BKMK_query"></a>creare ed eseguire query personalizzate nei dati del riquadro  
È possibile creare query personalizzate in riquadri eventi, prestazioni, Best Practices Analyzer, servizi o i ruoli e funzionalità in Server Manager. Per impostazione predefinita, l'area degli strumenti del riquadro in cui si selezionano i criteri per compilare una query personalizzata è nascosta; Fare clic su **espandere** (freccia di espansione sul bordo destro del riquadro barra degli strumenti) per visualizzare i criteri di query.  
  
#### <a name="to-create-a-custom-query-for-tile-data"></a>Per creare una query personalizzata per i dati del riquadro  
  
1.  Aprire una pagina di gruppo di ruoli o di server in Server Manager.  
  
2.  In un riquadro eventi, prestazioni, Best Practices Analyzer, servizi o ruoli e funzionalità, espandere l'area di compilazione query facendo clic **espandere**.  
  
3.  Fare clic su **Aggiungi criteri** per aprire un elenco di attributi (o campi) applicabili alle voci nel riquadro.  
  
4.  Selezionare i criteri da aggiungere. Al termine, fare clic su **aggiungere**. I criteri selezionati vengono aggiunti all'area di compilazione query.  
  
5.  Fare clic sull'operatore ipertestuale per selezionare un operatore. Per i criteri numerici o di data e ora, ad esempio, l'impostazione predefinita è **minore o uguale a**.  
  
6.  Specificare valori accettabili per i criteri. Ad esempio, se è stato selezionato **data e ora**, specificare una data nel formato *g/aaaa*.  
  
7.  Ripetere i passaggi dal passaggio 3 in poi per aggiungere altri criteri alla query.  
  
    È possibile aggiungere dei duplicati di criteri già presenti nella query, tuttavia i duplicati verranno aggiunti alla query con l'operatore **or**.  
  
    ad esempio, per eseguire una query per gli ID evento 1003 o 1014, si sarebbe innanzitutto aggiungere il criterio ID alla query, impostare il valore dell'ID uguale a **1003**, quindi aggiungere un secondo criterio ID alla query, rendendo il valore dell'ID del secondo uguale a  **1014**. La query risultante è **and ID è uguale a 1003 or ID è uguale a 1014**.  
  
8.  Dopo aver aggiunto i criteri e aver specificato gli operatori e i valori, fare clic su **Salva** per salvare la query.  
  
9. Immettere un nome descrittivo per la query. Ad esempio, la query creata nel passaggio precedente può essere denominata **Eventi di gestione delle licenze**.  
  
10. Dopo aver visualizzato i risultati della query, fare clic su **Cancella tutto** per cancellare tutti i filtri e le query e visualizzare tutte le voci nell'elenco.  
  
11. Per eseguire una query salvata, fare clic su **Query salvate**, quindi fare clic sul nome della query salvata da eseguire.  
  
12. Per eliminare una query salvata, fare clic su **Query salvate**, quindi fare clic su **X** accanto al nome della query salvata da eliminare.  
  
## <a name="see-also"></a>Vedere anche  
[Server Manager](server-manager.md)  
[Visualizzare e configurare le prestazioni, eventi e i dati del servizio](view-and-configure-performance-event-and-service-data.md)  
  


