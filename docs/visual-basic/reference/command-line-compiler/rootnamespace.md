---
title: -rootnamespace
ms.date: 03/13/2018
f1_keywords:
- /rootnamespace
- rootnamespace
helpviewer_keywords:
- /rootnamespace compiler option [Visual Basic]
- -rootnamespace compiler option [Visual Basic]
- rootnamespace compiler option [Visual Basic]
ms.assetid: e9245edf-6bef-420d-a7c7-324117752783
ms.openlocfilehash: 4df4e74fc13c922f51f5b74c3c152bdea28b4431
ms.sourcegitcommit: eff6adb61852369ab690f3f047818c90580e7eb1
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/07/2019
ms.locfileid: "72005207"
---
# <a name="-rootnamespace"></a>-rootnamespace
Gibt einen Namespace für alle Typdeklarationen an.  
  
## <a name="syntax"></a>Syntax  
  
```console  
-rootnamespace:namespace  
```  
  
## <a name="arguments"></a>Argumente  
  
|Begriff|Definition|  
|---|---|  
|`namespace`|Der Name des Namespace, in dem alle Typdeklarationen für das aktuelle Projekt eingeschlossen werden sollen.|  
  
## <a name="remarks"></a>Hinweise  
 Wenn Sie die ausführbare Visual Studio-Datei (devenv. exe) verwenden, um ein Projekt zu kompilieren, das in der integrierten Entwicklungsumgebung von Visual Studio erstellt wurde, verwenden Sie `-rootnamespace`, um den Wert der Eigenschaft <xref:VSLangProj80.VBProjectProperties3.RootNamespace%2A> anzugeben. Weitere Informationen finden Sie unter [devenv-Befehls Zeilenschalter](/visualstudio/ide/reference/devenv-command-line-switches) .  
  
 Verwenden Sie die Common Language Runtime MSIL-Disassembler (`Ildasm.exe`), um die Namespace Namen in der Ausgabedatei anzuzeigen.  
  
|To Set-RootNamespace in der integrierten Entwicklungsumgebung von Visual Studio|  
|---|  
|1.  Ein Projekt auswählen in **Projektmappen-Explorer**. Klicken Sie im Menü **Projekt** auf **Eigenschaften**. <br />2.  Klicken Sie auf die Registerkarte **Anwendung** .<br />3.  Ändern Sie den Wert im Feld Stamm **Namespace** .|  
  
## <a name="example"></a>Beispiel  
 Der folgende Code kompiliert `In.vb` und schließt alle Typdeklarationen im-Namespace `mynamespace` ein.  
  
```console
vbc -rootnamespace:mynamespace in.vb  
```  
  
## <a name="see-also"></a>Siehe auch

- [Visual Basic-Befehlszeilencompiler](../../../visual-basic/reference/command-line-compiler/index.md)
- [Ildasm.exe (IL-Disassembler)](../../../framework/tools/ildasm-exe-il-disassembler.md)
- [Beispiele für Kompilierungsbefehlszeilen](../../../visual-basic/reference/command-line-compiler/sample-compilation-command-lines.md)
