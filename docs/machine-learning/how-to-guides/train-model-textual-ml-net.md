---
title: 'Anwenden der Funktionsentwicklung für Modelltraining bei Textdaten: ML.NET'
description: Lernen Sie, wie Sie die Funktionsentwicklung für Modelltraining bei Texdaten mit ML.NET anwenden
ms.date: 11/07/2018
ms.custom: mvc,how-to
ms.openlocfilehash: ed24561c8cc821ece8a21ca61e22a11bda2516d1
ms.sourcegitcommit: 35316b768394e56087483cde93f854ba607b63bc
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/26/2018
ms.locfileid: "52297637"
---
# <a name="apply-feature-engineering-for-machine-learning-model-training-on-textual-data-with-mlnet"></a><span data-ttu-id="69aac-103">Anwenden der Funktionsentwicklung für das Modelltraining bei Texdaten für maschinelles Lernen mit ML.NET</span><span class="sxs-lookup"><span data-stu-id="69aac-103">Apply feature engineering for machine learning model training on textual data with ML.NET</span></span>

<span data-ttu-id="69aac-104">Sie müssen jegliche Daten, die nicht vom Typ „float“ sind, in `float`-Datentypen konvertieren, da alle ML.NET-`learners` die Features als `float vector` erwarten.</span><span class="sxs-lookup"><span data-stu-id="69aac-104">You need to convert any non float data to `float` data types since all ML.NET `learners` expect features as a `float vector`.</span></span>

<span data-ttu-id="69aac-105">Um den Lernvorgang bei Textdaten auszuführen, müssen Sie Textfeatures extrahieren.</span><span class="sxs-lookup"><span data-stu-id="69aac-105">To learn on textual data, you need to extract text features.</span></span> <span data-ttu-id="69aac-106">ML.NET verfügt über einige einfache Mechanismen zur Extrahierung von Textfunktionen:</span><span class="sxs-lookup"><span data-stu-id="69aac-106">ML.NET has some basic text feature extraction mechanisms:</span></span>

- <span data-ttu-id="69aac-107">`Text normalization` (Entfernen von Satzzeichen, diakritischen Zeichen, zu Kleinbuchstaben wechseln, etc.)</span><span class="sxs-lookup"><span data-stu-id="69aac-107">`Text normalization` (removing punctuation, diacritics, switching to lowercase etc.)</span></span>
- <span data-ttu-id="69aac-108">`Separator-based tokenization`.</span><span class="sxs-lookup"><span data-stu-id="69aac-108">`Separator-based tokenization`.</span></span>
- <span data-ttu-id="69aac-109">Entfernen von `Stopword`</span><span class="sxs-lookup"><span data-stu-id="69aac-109">`Stopword` removal.</span></span>
- <span data-ttu-id="69aac-110">Extrahierung von `Ngram` und `skip-gram`</span><span class="sxs-lookup"><span data-stu-id="69aac-110">`Ngram` and `skip-gram` extraction.</span></span>
- <span data-ttu-id="69aac-111">Neuskalierung von `TF-IDF`</span><span class="sxs-lookup"><span data-stu-id="69aac-111">`TF-IDF` rescaling.</span></span>
- <span data-ttu-id="69aac-112">Umwandeln von `Bag of words`</span><span class="sxs-lookup"><span data-stu-id="69aac-112">`Bag of words` conversion.</span></span>

<span data-ttu-id="69aac-113">Das folgende Beispiel zeigt mit dem [Wikipedia-Detox-DataSet](https://github.com/dotnet/machinelearning/blob/master/test/data/wikipedia-detox-250-line-data.tsv) durchgeführte ML.NET-Mechanismen zur Extrahierung von Textfunktionen:</span><span class="sxs-lookup"><span data-stu-id="69aac-113">The following example demonstrates ML.NET text feature extraction mechanisms using the [Wikipedia detox dataset](https://github.com/dotnet/machinelearning/blob/master/test/data/wikipedia-detox-250-line-data.tsv):</span></span>

```console
Sentiment   SentimentText
1   Stop trolling, zapatancas, calling me a liar merely demonstartes that you arer Zapatancas. You may choose to chase every legitimate editor from this site and ignore me but I am an editor with a record that isnt 99% trolling and therefore my wishes are not to be completely ignored by a sockpuppet like yourself. The consensus is overwhelmingly against you and your trolling lover Zapatancas,  
1   ::::: Why are you threatening me? I'm not being disruptive, its you who is being disruptive.   
0   " *::Your POV and propaganda pushing is dully noted. However listing interesting facts in a netral and unacusitory tone is not POV. You seem to be confusing Censorship with POV monitoring. I see nothing POV expressed in the listing of intersting facts. If you want to contribute more facts or edit wording of the cited fact to make them sound more netral then go ahead. No need to CENSOR interesting factual information. "
0   ::::::::This is a gross exaggeration. Nobody is setting a kangaroo court. There was a simple addition concerning the airline. It is the only one disputed here.   
```

```csharp
// Create a new context for ML.NET operations. It can be used for exception tracking and logging, 
// as a catalog of available operations and as the source of randomness.
var mlContext = new MLContext();

// Define the reader: specify the data columns and where to find them in the text file.
var reader = mlContext.Data.TextReader(new TextLoader.Arguments
{
    Column = new[] {
        new TextLoader.Column("IsToxic", DataKind.BL, 0),
        new TextLoader.Column("Message", DataKind.TX, 1),
    },
    HasHeader = true
});

// Read the data.
var data = reader.Read(dataPath);

// Inspect the message texts that are read from the file.
var messageTexts = data.GetColumn<string>(mlContext, "Message").Take(20).ToArray();

// Apply various kinds of text operations supported by ML.NET.
var pipeline =
    // One-stop shop to run the full text featurization.
    mlContext.Transforms.Text.FeaturizeText("Message", "TextFeatures")

    // Normalize the message for later transforms
    .Append(mlContext.Transforms.Text.NormalizeText("Message", "NormalizedMessage"))

    // NLP pipeline 1: bag of words.
    .Append(new WordBagEstimator(mlContext, "NormalizedMessage", "BagOfWords"))

    // NLP pipeline 2: bag of bigrams, using hashes instead of dictionary indices.
    .Append(new WordHashBagEstimator(mlContext, "NormalizedMessage", "BagOfBigrams",
                ngramLength: 2, allLengths: false))

    // NLP pipeline 3: bag of tri-character sequences with TF-IDF weighting.
    .Append(mlContext.Transforms.Text.TokenizeCharacters("Message", "MessageChars"))
    .Append(new NgramEstimator(mlContext, "MessageChars", "BagOfTrichar",
                ngramLength: 3, weighting: NgramTransform.WeightingCriteria.TfIdf))

    // NLP pipeline 4: word embeddings.
    .Append(mlContext.Transforms.Text.TokenizeWords("NormalizedMessage", "TokenizedMessage"))
    .Append(mlContext.Transforms.Text.ExtractWordEmbeedings("TokenizedMessage", "Embeddings",
                WordEmbeddingsTransform.PretrainedModelKind.GloVeTwitter25D));

// Let's train our pipeline, and then apply it to the same data.
// Note that even on a small dataset of 70KB the pipeline above can take up to a minute to completely train.
var transformedData = pipeline.Fit(data).Transform(data);

// Inspect some columns of the resulting dataset.
var embeddings = transformedData.GetColumn<float[]>(mlContext, "Embeddings").Take(10).ToArray();
var unigrams = transformedData.GetColumn<float[]>(mlContext, "BagOfWords").Take(10).ToArray();
```