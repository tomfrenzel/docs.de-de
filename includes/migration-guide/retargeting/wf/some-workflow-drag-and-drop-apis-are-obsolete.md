---
ms.openlocfilehash: 297af393e86c65e84ea7271d98eab36dbc6dbb0e
ms.sourcegitcommit: 0be8a279af6d8a43e03141e349d3efd5d35f8767
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/18/2019
ms.locfileid: "59774384"
---
### <a name="some-workflow-drag-and-drop-apis-are-obsolete"></a>Einige Drag & Drop-APIs für WorkFlow sind veraltet.

|   |   |
|---|---|
|Details|Diese Drag & Drop-API für WorkFlow ist veraltet und löst Compilerwarnungen aus, wenn die App mit Version 4.5 neu erstellt wird.|
|Vorschlag|Stattdessen sollten neue <xref:System.Activities.Presentation.DragDropHelper?displayProperty=name>-APIs verwendet werden, die Vorgänge mit mehreren Objekten unterstützen. Alternativ können die Buildwarnungen unterdrückt oder durch die Verwendung eines älteren Compilers vermieden werden. Die APIs werden weiterhin unterstützt.|
|Bereich|Gering|
|Version|4.5|
|Typ|Neuzuweisung|
|Betroffene APIs|<ul><li><xref:System.Activities.Presentation.DragDropHelper.DoDragMove(System.Activities.Presentation.WorkflowViewElement,System.Windows.Point)?displayProperty=nameWithType></li><li><xref:System.Activities.Presentation.DragDropHelper.GetCompositeView(System.Windows.DragEventArgs)?displayProperty=nameWithType></li><li><xref:System.Activities.Presentation.DragDropHelper.GetDraggedModelItem(System.Windows.DragEventArgs)?displayProperty=nameWithType></li><li><xref:System.Activities.Presentation.DragDropHelper.GetDroppedObject(System.Windows.DependencyObject,System.Windows.DragEventArgs,System.Activities.Presentation.EditingContext)?displayProperty=nameWithType></li></ul>|
