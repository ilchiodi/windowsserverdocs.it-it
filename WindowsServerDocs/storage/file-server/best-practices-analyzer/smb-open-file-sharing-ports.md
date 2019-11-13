---
title: 'SMB: Le porte per la condivisione di file e stampanti devono essere aperte'
TOCTitle: 'SMB: File and printer sharing ports should be open'
ms.date: 07/02/2012
ms.prod: windows-server
ms.technology: storage
author: JasonGerend
manager: elizapo
ms.author: jgerend
ms.openlocfilehash: 22cd926fdb873538631a6f6850157dceb5a020d7
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71385826"
---
# <a name="smb-file-and-printer-sharing-ports-should-be-open"></a>SMB: Le porte per la condivisione di file e stampanti devono essere aperte


Ultimo aggiornamento: 2 febbraio 2011

Si applica a: Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 e Windows Server 2012, Windows Server 2008 R2

*Questo argomento è destinato a risolvere un problema specifico identificato da un'analisi Best Practices Analyzer. È necessario applicare le informazioni contenute in questo argomento solo ai computer in cui è stato Best Practices Analyzer eseguire i servizi file e si è verificato il problema trattato da questo argomento. Per ulteriori informazioni sulle procedure consigliate e le analisi, vedere* [Best Practices Analyzer](http://go.microsoft.com/fwlink/?linkid=122786%0d%0a).


<table>
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<tbody>
<tr class="odd">
<td><p><strong>Sistema operativo</strong></p></td>
<td><p>Windows Server</p></td>
</tr>
<tr class="even">
<td><p><strong>Prodotto/funzionalità</strong></p></td>
<td><p>Servizi file</p></td>
</tr>
<tr class="odd">
<td><p><strong>Gravità</strong></p></td>
<td><p>Error</p></td>
</tr>
<tr class="even">
<td><p><strong>Categoria</strong></p></td>
<td><p>Configurazione</p></td>
</tr>
</tbody>
</table>

## <a name="issue"></a>Problema

> *Le porte del firewall necessarie per la condivisione di file e stampanti non sono aperte (porte 445 e 139).*

## <a name="impact"></a>Impatto

> *I computer non saranno in grado di accedere alle cartelle condivise e ad altri servizi di rete basati su SMB (Server Message Block) in questo server.*

## <a name="resolution"></a>Risoluzione

> *Abilitare la condivisione di file e stampanti per la comunicazione attraverso il firewall del computer.*

Il requisito minimo per completare questa procedura è l'appartenenza al gruppo **Administrators** locale o a un gruppo equivalente.

## <a name="to-open-the-firewall-ports-to-enable-file-and-printer-sharing"></a>Per aprire le porte del firewall per abilitare la condivisione di file e stampanti

1.  Aprire il pannello di controllo, fare clic su **sistema e sicurezza**, quindi fare clic su **Windows Firewall**.

2.  Nel riquadro sinistro fare clic su **Impostazioni avanzate**e nell'albero della console fare clic su **Regole connessioni in ingresso**.

3.  In **Regole connessioni in ingresso**individuare le regole **condivisione file e stampanti (NB-Session-in)** e **condivisione file e stampanti (SMB-in)** .

4.  Per ogni regola, fare clic con il pulsante destro del mouse sulla regola e quindi scegliere **Abilita regola**.

## <a name="additional-references"></a>Altri riferimenti

[Informazioni sulle cartelle condivise e sulla Windows Firewall](https://technet.microsoft.com/library/cc731402.aspx)(https://technet.microsoft.com/library/cc731402.aspx)

