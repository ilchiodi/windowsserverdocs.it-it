---
title: auditpol
description: Argomento di riferimento per il comando auditpol, che consente di visualizzare informazioni su ed esegue funzioni per modificare i criteri di controllo.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: a02cfb9d-732f-4e77-aeba-f18265daa3af
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 89fee7ccd3b6671a6f2633c3b5d15d0cbee261fa
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/01/2020
ms.locfileid: "82718824"
---
# <a name="auditpol"></a>auditpol

Visualizza informazioni su ed esegue funzioni per modificare i criteri di controllo, tra cui:

- Impostazione ed esecuzione di query sui criteri di controllo del sistema.

- Impostazione ed esecuzione di query su un criterio di controllo per utente.

- Impostazione ed esecuzione di query sulle opzioni di controllo.

- Impostazione ed esecuzione di query sul descrittore di sicurezza utilizzato per delegare l'accesso a un criterio di controllo.

- Creazione di report o backup di un criterio di controllo in un file di testo con valori delimitati da virgole (CSV).

- Caricamento di un criterio di controllo da un file di testo CSV.

- Configurazione di SACL di risorse globali.

## <a name="syntax"></a>Sintassi

```
auditpol command [<sub-command><options>]
```

### <a name="parameters"></a>Parametri

| Sottocomando | Descrizione |
| ----------- | ----------- |
| /Get | Visualizza i criteri di controllo correnti. Per ulteriori informazioni, vedere [auditpol get](auditpol-get.md) per la sintassi e le opzioni. |
| /set | Imposta i criteri di controllo. Per ulteriori informazioni, vedere [auditpol set](auditpol-set.md) for Syntax and options. |
| /list | Visualizza gli elementi del criterio selezionabili. Per ulteriori informazioni, vedere l' [elenco Auditpol](auditpol-list.md) per la sintassi e le opzioni. |
| /backup | Salva i criteri di controllo in un file. Per ulteriori informazioni, vedere [auditpol backup](auditpol-backup.md) per la sintassi e le opzioni. |
| /Restore | Ripristina i criteri di controllo da un file creato in precedenza tramite auditpol/backup. Per ulteriori informazioni, vedere [auditpol restore](auditpol-restore.md) per sintassi e opzioni. |
| /Clear | Cancella i criteri di controllo. Per altre informazioni, vedere [auditpol clear](auditpol-clear.md) per la sintassi e le opzioni. |
| /remove | Rimuove tutte le impostazioni dei criteri di controllo per singolo utente e Disabilita tutte le impostazioni dei criteri di controllo del sistema. Per ulteriori informazioni, vedere la pagina relativa alla [rimozione di auditpol](auditpol-remove.md) per sintassi e opzioni. |
| /resourceSACL | Configura gli elenchi di controllo di accesso (SACL) globali del sistema di risorse. **Nota:** Si applica solo a Windows 7 e Windows Server 2008 R2. Per ulteriori informazioni, vedere [auditpol resourceSACL](auditpol-resourcesacl.md). |
| /?| Visualizza la guida al prompt dei comandi. |

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)
