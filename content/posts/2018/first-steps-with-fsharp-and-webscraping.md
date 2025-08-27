---
title: "First steps with F# and Web Scraping"
author: "Cássio"
authorAvatarPath: "/profile.jpeg"
date: "2018-09-17"
summary: "Overview about using F# for Web Scraping."
description: "Overview about using F# for Web Scraping."
toc: false
readTime: true
autonumber: false
math: false
tags: ["fsharp", "web-scraping", "dotnet"]
showTags: true
hideBackToTop: false
hidePagination: false
fediverse: "@cassiofariasmachado@mastodon.social"
---

> ℹ️ This article was originally published in Portuguese on Medium. You can read the original version [here](https://medium.com/cwi-software/primeiros-passos-com-f-e-web-scraping-90ee1b9e586e).

I've always been very curious when I heard about functional languages and the advantages they bring to development. The promise of saying goodbye to `null` and other runtime errors that bother us so much still makes my eyes shine today. So, as a .NET developer, I decided to start in the functional world through F#.

I had been wanting to study the language better and write an article about it for a while, so I decided to start with practice by studying how to use it for Web Scraping. For those unfamiliar with the term, [Web Scraping](https://en.wikipedia.org/wiki/Web_scraping) is nothing more than a method of collecting data from web pages, and F# is a very powerful tool for this.

### Ways to use it

There are basically three ways to apply Web Scraping with F#:

- Use some library made for this in C#;
- Use a Selenium wrapper for F#;
- Or use the `Fsharp.Data` library.

This last one is what I'll cover in this article. It's a library that allows you to work more easily with CSV, XML, JSON and even, don't be surprised, HTML formats.

It also provides helpers for making HTTP requests, conversion to the types already mentioned, and access to [WorldBank](http://www.worldbank.org), but this won't be covered in this article.

### HtmlProvider

Through the `HtmlProvider` it's possible to define a type for the page you want to scrape. It expects to receive a sample HTML that can be a file or a URL, and will serve as the basis for creating the F# type, like this:

```fsharp
type DilbertSearch = HtmlProvider<"https://www.pinterest.pt/search/pins/?q=dilbert%20comic%20strip">
```

This way it's possible, for example, to get the first available images from the search for "dilbert comic strip" on Pinterest:

```fsharp
DilbertSearch().Html.CssSelect(".mainContainer img")
    |> List.map (fun d -> getUrlOfLargestImage(d.AttributeValue("srcset")))
    |> List.iter (printfn "%s")

// → https://i.pinimg.com/736x/90/38/bb/9038bbcabd5b31d6faa6705230df3a78--peanuts-comics-peanuts-gang.jpg
// → https://i.pinimg.com/736x/b4/f5/ba/b4f5bac902a421a8b2eb00f232a227e4--human-resources-online-comics.jpg
// ...
```

The types generated from `HtmlProvider` automatically identify tables and lists (literally `<table>`, `<ul>` or `<ol>`) found in the HTML page, so it's possible, for example, to get the list of characters from the Hagar comic strip on its Wikipedia page:

```fsharp
type HagarWiki = HtmlProvider<"https://en.wikipedia.org/wiki/H%C3%A4gar_the_Horrible">

HagarWiki().Lists.``Cast of characters``.Values
    |> List.ofArray
    |> List.map getCharacterName
    |> List.iter (printf "%s\n")

// → Hägar the Horrible
// → Helga
// ...
```

The name given to the identified list or table is taken from the HTML attributes/tags `id`, `title`, `name`, `summary` or `caption`. If none of them are found then the name given will be `TableXX` or `ListXX`, where `XX` is a sequential number of where the element was found on the page.

But the best part of this is that these tables and lists are available at development time through Visual Studio or Visual Studio Code IntelliSense:

![IntelliSense Example](/posts/2018/first-steps-with-fsharp-and-webscraping/intellisense-example.gif)

In this other example it's possible to search for Star Wars franchise movies and their box office revenue in dollars:

```fsharp
type StarWarsWiki = HtmlProvider<"https://en.wikipedia.org/wiki/List_of_Star_Wars_films_and_television_series">

let filmsByRevenue = StarWarsWiki().Tables.``Box office performance``.Rows
                        |> Seq.filter (fun r -> isReleaseDate r.``Release date``)
                        |> Seq.sortBy (fun x -> convertReleaseDate x.``Release date``)
                        |> Seq.map (fun r -> r.Film, convertRevenue r.``Box office revenue - Worldwide``)
                        |> Seq.toArray

filmsByRevenue
    |> Seq.iter (fun elem -> elem ||> printf "%s - %f Billions \n")

// → Star Wars - 0.775398 Billions
// → The Empire Strikes Back - 0.547969 Billions
// ...
```

And then, it's also possible to plot a chart using the `FSharp.Charting` library, like this:

```fsharp
Chart.Column filmsByRevenue
    |> Chart.WithYAxis(Title = "Billions")
    |> Chart.WithXAxis(Title = "Films")
    |> Chart.Show
```

![Plot Example](/posts/2018/first-steps-with-fsharp-and-webscraping/plot-example.png)

### Repository with examples

The examples used in the article are available in this [GitHub repository](https://github.com/cassiofariasmachado/webscraping-with-fsharp).

In it there are two example projects, one using [.NET Framework](https://github.com/cassiofariasmachado/webscraping-with-fsharp/tree/master/src/samples) and another using [.NET Core](https://github.com/cassiofariasmachado/webscraping-with-fsharp/tree/master/src/samples-core). The only difference between them, besides the .NET version, is that in the latter the `FSharp.Charting` library was removed, as it has a .NET Framework dependency.

### Conclusion

The `Fsharp.Data` library makes F# a very powerful tool for scraping web pages. However, not everything is perfect and if the page has very dynamic content (using Javascript for rendering), there are difficulties in using the library, but they can be overcome by using it together with the second option presented at the beginning of the article, a Selenium wrapper for F#.

Anyway, the first impression with the language and the paradigm was very positive. It's a different way of development that makes the code much clearer, but it requires some theoretical depth, as it demands a mindset change for those accustomed to the object-oriented world. As a next step, I should continue deepening my knowledge of the language to, who knows, bring another article on the subject.

### References

Here are some references used:

- [FSharp Official Website](https://fsharp.org/)
- [Fsharp.Data](http://fsharp.github.io/FSharp.Data/index.html)
- [Why F# is the best language for Web Scraping](https://biarity.gitlab.io/2016/11/23/why-f-is-the-best-langauge-for-web-scraping/)
