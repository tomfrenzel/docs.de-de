---
title: 'Vorgehensweise: Verbessern der Leistung durch das Vermeiden der automatischen Skalierung'
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- automatic scaling
- images [Windows Forms], improving performance
- images [Windows Forms], using without automatic scaling
- performance [Windows Forms], improving image
ms.assetid: 5fe2c95d-8653-4d55-bf0d-e5afa28f223b
ms.openlocfilehash: dd1a1545dce33de1ce11938db8495ebf311dadda
ms.sourcegitcommit: b1cfd260928d464d91e20121f9bdba7611c94d71
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/02/2019
ms.locfileid: "67506207"
---
# <a name="how-to-improve-performance-by-avoiding-automatic-scaling"></a>Vorgehensweise: Verbessern der Leistung durch das Vermeiden der automatischen Skalierung
GDI + kann automatisch ein Bild zu skalieren, wie Sie es zeichnen, die Leistung verringern würden. Alternativ können Sie steuern die Skalierung des Bilds durch die Maße des Zielrechtecks zum Übergeben der <xref:System.Drawing.Graphics.DrawImage%2A> Methode.  
  
 Beispielsweise der folgende Aufruf von der <xref:System.Drawing.Graphics.DrawImage%2A> Methode gibt eine linke obere Ecke des (50, 30), aber nicht über ein Zielrechteck angibt.  
  
 [!code-csharp[System.Drawing.WorkingWithImages#31](~/samples/snippets/csharp/VS_Snippets_Winforms/System.Drawing.WorkingWithImages/CS/Class1.cs#31)]
 [!code-vb[System.Drawing.WorkingWithImages#31](~/samples/snippets/visualbasic/VS_Snippets_Winforms/System.Drawing.WorkingWithImages/VB/Class1.vb#31)]  
  
 Obwohl dies die einfachste Version von ist das <xref:System.Drawing.Graphics.DrawImage%2A> Methode hinsichtlich der Anzahl der erforderlichen Argumente, es ist nicht unbedingt die effizienteste. Wenn die Auflösung von GDI + (in der Regel 96 dpi) verwendet die Lösung, die in gespeicherten unterscheidet die <xref:System.Drawing.Image> -Objekt, das <xref:System.Drawing.Graphics.DrawImage%2A> Methode wird das Bild skaliert. Nehmen wir beispielsweise an ein <xref:System.Drawing.Image> Objekt verfügt über eine Breite von 216 Pixel und den Wert gespeicherten horizontale Auflösung 72 Punkte pro Zoll. Da 216/72-3, ist <xref:System.Drawing.Graphics.DrawImage%2A> wird das Bild skaliert, sodass er eine Breite von 3 Zoll mit einer Auflösung von 96 DPI-Wert aufweist. D. h. <xref:System.Drawing.Graphics.DrawImage%2A> zeigt ein Image mit einer Breite von 96 × 3 = 288 Pixel.  
  
 Auch wenn eine Bildschirmauflösung von 96 DPI-Wert unterscheidet, werden GDI + wahrscheinlich das Bild skalieren als wäre die Auflösung von 96 DPI-Wert. Der Grund dafür ist eine GDI + <xref:System.Drawing.Graphics> Objekt einen Gerätekontext zugeordnet ist, und wenn GDI + Gerätekontext für die Bildschirmauflösung abgefragt wird, das Ergebnis ist in der Regel 96, unabhängig von der tatsächlichen Bildschirmauflösung. Sie können die automatische Skalierung, indem das Zielrechteck im Vermeiden der <xref:System.Drawing.Graphics.DrawImage%2A> Methode.  
  
## <a name="example"></a>Beispiel  
 Im folgende Beispiel wird das gleiche Image zweimal zeichnet. Im ersten Fall ist die Breite und Höhe des Zielrechtecks nicht angegeben, und das Bild wird automatisch skaliert. Im zweiten Fall sind die Breite und Höhe (gemessen in Pixel) des Zielrechtecks identisch sein, als die Breite und Höhe des Originalbilds angegeben. Die folgende Abbildung zeigt das Bild zweimal gerendert:  
  
 ![Screenshot mit Bildern mit skalierte Struktur.](./media/how-to-improve-performance-by-avoiding-automatic-scaling/two-scaled-texture-images.png)  
  
 [!code-csharp[System.Drawing.WorkingWithImages#32](~/samples/snippets/csharp/VS_Snippets_Winforms/System.Drawing.WorkingWithImages/CS/Class1.cs#32)]
 [!code-vb[System.Drawing.WorkingWithImages#32](~/samples/snippets/visualbasic/VS_Snippets_Winforms/System.Drawing.WorkingWithImages/VB/Class1.vb#32)]  
  
## <a name="compiling-the-code"></a>Kompilieren des Codes  
 Das obige Beispiel ist für die Verwendung mit Windows Forms konzipiert und erfordert <xref:System.Windows.Forms.PaintEventArgs> `e`, ein Parameter von der <xref:System.Windows.Forms.Control.Paint> -Ereignishandler. Ersetzen Sie Texture.jpg mit einem Image-Name und Pfad, der auf Ihrem System gültig sind.  
  
## <a name="see-also"></a>Siehe auch

- [Bilder, Bitmaps und Metadateien](images-bitmaps-and-metafiles.md)
- [Arbeiten mit Bildern, Bitmaps, Symbolen und Metadateien](working-with-images-bitmaps-icons-and-metafiles.md)
