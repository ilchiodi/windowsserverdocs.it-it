---
title: auditpol
description: Windows Commands (comandi di Windows) per **auditpol**, che visualizza informazioni su ed esegue funzioni per modificare i criteri di controllo.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: a02cfb9d-732f-4e77-aeba-f18265daa3af
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 00365b0e46b8bff761cf991dbdbd09d8f5e9c687
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80851134"
---
# <a name="auditpol"></a>auditpol

Visualizza informazioni su ed esegue funzioni per modificare i criteri di controllo.

Per esempi di come è possibile usare questo comando, vedere la sezione Esempi in ogni argomento.

## <a name="syntax"></a>Sintassi

```
auditpol command [<sub-command><options>]
```

#### <a name="parameters"></a>Parametri

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

## <a name="remarks"></a>Note

Lo strumento da riga di comando per i criteri di controllo può essere utilizzato per:

- Impostare ed eseguire una query sui criteri di controllo del sistema.

- Impostare ed eseguire una query su un criterio di controllo per utente.

- Opzioni di controllo set e query.

- Impostare ed eseguire una query sul descrittore di sicurezza utilizzato per delegare l'accesso ai criteri di controllo.

- Segnalare o eseguire il backup di un criterio di controllo in un file di testo con valori delimitati da virgole (CSV).

- Caricare i criteri di controllo da un file di testo CSV.

- Configurare SACL di risorse globali.

## <a name="additional-references"></a>Altre informazioni di riferimento

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)