---
title: Thread sichere Aufrufe an Steuerelemente erstellen
ms.date: 02/19/2019
dev_langs:
- csharp
- vb
f1_keywords:
- EHInvalidOperation.WinForms.IllegalCrossThreadCall
helpviewer_keywords:
- thread safety [Windows Forms], calling controls [Windows Forms]
- calling controls [Windows Forms], thread safety [Windows Forms]
- CheckForIllegalCrossThreadCalls property [Windows Forms]
- Windows Forms controls [Windows Forms], multithreading
- BackgroundWorker class [Windows Forms], examples
- threading [Windows Forms], cross-thread calls
- controls [Windows Forms], multithreading
ms.assetid: 138f38b6-1099-4fd5-910c-390b41cbad35
ms.openlocfilehash: 101457ab2a02b8de49d06c354ca307e07b690ba2
ms.sourcegitcommit: de17a7a0a37042f0d4406f5ae5393531caeb25ba
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/24/2020
ms.locfileid: "76736165"
---
# <a name="how-to-make-thread-safe-calls-to-windows-forms-controls"></a>Gewusst wie: Erstellen von Thread sicheren Aufrufen von Windows Forms-Steuerelementen

Multithreading kann die Leistung von Windows Forms-apps verbessern, aber der Zugriff auf Windows Forms-Steuerelemente ist nicht grundsätzlich Thread sicher. Multithreading kann Ihren Code für sehr ernste und komplexe Fehler verfügbar machen. Zwei oder mehr Threads, die ein Steuerelement bearbeiten, können das Steuerelement in einen inkonsistenten Zustand versetzen und zu Racebedingungen, Deadlocks und einfriert bzw. hängen. Wenn Sie Multithreading in der APP implementieren, stellen Sie sicher, dass Thread übergreifende Steuerelemente auf Thread sichere Weise aufgerufen werden. Weitere Informationen finden Sie unter [bewährte Methoden für das verwaltete Threading](../../../standard/threading/managed-threading-best-practices.md). 

Es gibt zwei Möglichkeiten, um ein Windows Forms-Steuerelement aus einem Thread, der dieses Steuerelement nicht erstellt hat, sicher aufzurufen. Sie können die <xref:System.Windows.Forms.Control.Invoke%2A?displayProperty=fullName>-Methode verwenden, um einen im Haupt Thread erstellten Delegaten aufzurufen, der wiederum das-Steuerelement aufruft. Sie können auch eine <xref:System.ComponentModel.BackgroundWorker?displayProperty=nameWithType>implementieren, bei der ein ereignisgesteuerte Modell verwendet wird, um im Hintergrund Thread durchgeführte Arbeiten von der Berichterstellung für die Ergebnisse zu trennen. 

## <a name="unsafe-cross-thread-calls"></a>Unsichere Thread übergreifende Aufrufe

Es ist unsicher, ein Steuerelement direkt von einem Thread aufzurufen, der ihn nicht erstellt hat. Der folgende Code Ausschnitt veranschaulicht einen unsicheren-aufrufbefehl für das <xref:System.Windows.Forms.TextBox?displayProperty=nameWithType>-Steuerelement. Der `Button1_Click`-Ereignishandler erstellt einen neuen `WriteTextUnsafe` Thread, der die <xref:System.Windows.Forms.TextBox.Text%2A?displayProperty=nameWithType>-Eigenschaft des Haupt Threads direkt festlegt. 

```csharp
private void Button1_Click(object sender, EventArgs e)
{
    thread2 = new Thread(new ThreadStart(WriteTextUnsafe));
    thread2.Start();
}
private void WriteTextUnsafe()
{
    textBox1.Text = "This text was set unsafely.";
}
```

```vb
Private Sub Button1_Click(ByVal sender As Object, e As EventArgs) Handles Button1.Click
    Thread2 = New Thread(New ThreadStart(AddressOf WriteTextUnsafe))
    Thread2.Start()
End Sub

Private Sub WriteTextUnsafe()
    TextBox1.Text = "This text was set unsafely."
End Sub
```

Der Visual Studio-Debugger erkennt diese unsicheren Thread Aufrufe, indem er eine <xref:System.InvalidOperationException> mit der Meldung aufhebt, dass **Thread übergreifender Vorgang ungültig ist. Das Steuerelement "", auf das von einem anderen Thread als dem erstellten Thread zugegriffen wird.** Der <xref:System.InvalidOperationException> tritt immer bei unsicheren Thread übergreifenden aufrufen während des Debuggens von Visual Studio auf und kann zur Laufzeit der app auftreten. Sie sollten das Problem beheben, aber Sie können die Ausnahme deaktivieren, indem Sie die <xref:System.Windows.Forms.Control.CheckForIllegalCrossThreadCalls%2A?displayProperty=nameWithType>-Eigenschaft auf `false`festlegen.

## <a name="safe-cross-thread-calls"></a>Sichere Thread übergreifende Aufrufe 

Die folgenden Codebeispiele veranschaulichen zwei Möglichkeiten, um ein Windows Forms-Steuerelement aus einem Thread, der ihn nicht erstellt hat, sicher aufzurufen: 

1. Die <xref:System.Windows.Forms.Control.Invoke%2A?displayProperty=fullName>-Methode, die einen Delegaten vom Haupt Thread aufruft, um das-Steuerelement aufzurufen. 
2. Eine <xref:System.ComponentModel.BackgroundWorker?displayProperty=nameWithType>-Komponente, die ein ereignisgesteuerte Modell bietet. 

In beiden Beispielen wird der Hintergrund Thread für eine Sekunde in den Ruhezustand versetzt, um die Arbeit in diesem Thread zu simulieren. 

Sie können diese Beispiele als .NET Framework-Apps über die C# Befehlszeile oder Visual Basic erstellen und ausführen. Weitere Informationen finden Sie unter Erstellen über die [Befehlszeile mit csc. exe](../../../csharp/language-reference/compiler-options/command-line-building-with-csc-exe.md) oder [Erstellen über die Befehlszeile (Visual Basic)](../../../visual-basic/reference/command-line-compiler/building-from-the-command-line.md). 

Ab .net Core 3,0 können Sie die Beispiele auch als Windows .net Core-Apps aus einem Ordner erstellen und ausführen, der über einen .net Core-Windows Forms *\<Ordnernamen > csproj* -Projektdatei verfügt. 

## <a name="example-use-the-invoke-method-with-a-delegate"></a>Beispiel: Verwenden der Methode "aufrufen" mit einem Delegaten

Im folgenden Beispiel wird ein Muster zum Sicherstellen von Thread sicheren Aufrufen eines Windows Forms-Steuer Elements veranschaulicht. Er fragt die <xref:System.Windows.Forms.Control.InvokeRequired%2A?displayProperty=fullName>-Eigenschaft ab, die die Erstellungsthread-ID des Steuer Elements mit der aufrufenden Thread-ID vergleicht. Wenn die Thread-IDs identisch sind, wird das-Steuerelement direkt aufgerufen. Wenn sich die Thread-IDs unterscheiden, ruft Sie die <xref:System.Windows.Forms.Control.Invoke%2A?displayProperty=nameWithType>-Methode mit einem Delegaten aus dem Haupt Thread auf, der den eigentlichen Aufruf des Steuer Elements übernimmt.

Der `SafeCallDelegate` ermöglicht das Festlegen der <xref:System.Windows.Forms.TextBox.Text%2A> Eigenschaft des <xref:System.Windows.Forms.TextBox> Steuer Elements. Die `WriteTextSafe`-Methode fragt <xref:System.Windows.Forms.Control.InvokeRequired%2A>ab. Wenn <xref:System.Windows.Forms.Control.InvokeRequired%2A> `true`zurückgibt, übergibt `WriteTextSafe` das `SafeCallDelegate` an die <xref:System.Windows.Forms.Control.Invoke%2A>-Methode, um den eigentlichen-Befehl an das-Steuerelement zu senden. Wenn <xref:System.Windows.Forms.Control.InvokeRequired%2A> `false`zurückgibt, legt `WriteTextSafe` den <xref:System.Windows.Forms.TextBox.Text%2A?displayProperty=nameWithType> direkt fest. Der `Button1_Click`-Ereignishandler erstellt den neuen Thread und führt die `WriteTextSafe`-Methode aus. 

 [!code-csharp[ThreadSafeCalls#1](~/samples/snippets/winforms/thread-safe/example1/cs/Form1.cs)]
 [!code-vb[ThreadSafeCalls#1](~/samples/snippets/winforms/thread-safe/example1/vb/Form1.vb)]  

## <a name="example-use-a-backgroundworker-event-handler"></a>Beispiel: Verwenden eines BackgroundWorker-Ereignis Handlers

Eine einfache Möglichkeit zum Implementieren von Multithreading ist die <xref:System.ComponentModel.BackgroundWorker?displayProperty=nameWithType> Komponente, die ein Ereignis gesteuerter Modell verwendet. Der Hintergrund Thread führt das <xref:System.ComponentModel.BackgroundWorker.DoWork?displayProperty=nameWithType>-Ereignis aus, das nicht mit dem Haupt Thread interagiert. Der Haupt Thread führt die <xref:System.ComponentModel.BackgroundWorker.ProgressChanged?displayProperty=nameWithType> und <xref:System.ComponentModel.BackgroundWorker.RunWorkerCompleted?displayProperty=nameWithType> Ereignishandler aus, die die Steuerelemente des Haupt Threads abrufen können.

Um einen Thread sicheren aufzurufen, indem Sie <xref:System.ComponentModel.BackgroundWorker>verwenden, erstellen Sie eine Methode im Hintergrund Thread, um die Arbeit durchzuführen, und binden Sie Sie an das <xref:System.ComponentModel.BackgroundWorker.DoWork>-Ereignis. Erstellen Sie eine weitere Methode im Haupt Thread, um die Ergebnisse der Hintergrundarbeit zu melden, und binden Sie Sie an das <xref:System.ComponentModel.BackgroundWorker.ProgressChanged>-oder <xref:System.ComponentModel.BackgroundWorker.RunWorkerCompleted>-Ereignis. Um den Hintergrund Thread zu starten, wenden Sie <xref:System.ComponentModel.BackgroundWorker.RunWorkerAsync%2A?displayProperty=nameWithType>an. 

Im Beispiel wird der <xref:System.ComponentModel.BackgroundWorker.RunWorkerCompleted>-Ereignishandler verwendet, um die <xref:System.Windows.Forms.TextBox.Text%2A>-Eigenschaft des <xref:System.Windows.Forms.TextBox> Steuer Elements festzulegen. Ein Beispiel für die Verwendung des <xref:System.ComponentModel.BackgroundWorker.ProgressChanged> Ereignisses finden Sie unter <xref:System.ComponentModel.BackgroundWorker>. 

 [!code-csharp[ThreadSafeCalls#2](~/samples/snippets/winforms/thread-safe/example2/cs/Form1.cs)]
 [!code-vb[ThreadSafeCalls#2](~/samples/snippets/winforms/thread-safe/example2/vb/Form1.vb)]  

## <a name="see-also"></a>Weitere Informationen

- <xref:System.ComponentModel.BackgroundWorker>
- [Gewusst wie: Ausführen eines Vorgangs im Hintergrund](how-to-run-an-operation-in-the-background.md)
- [Gewusst wie: Implementieren eines Formulars, das einen Hintergrund Vorgang verwendet](how-to-implement-a-form-that-uses-a-background-operation.md)
- [Entwickeln Sie benutzerdefinierte Windows Forms Steuerelemente mit dem .NET Framework](developing-custom-windows-forms-controls.md)
