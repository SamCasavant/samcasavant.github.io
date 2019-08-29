---
layout: post
title:  "Finding Poetic Equivalence with FlowJacker"
date:   2018-02-05 23:35:00
categories: [lyrics, poetry, programming, experiments]
---

The alt text for [xkcd 778](https://xkcd.com/788/) mentions ballad meter- a structure of poem that lets us sing any of the following to the tune of the theme to *Gilligan's Isle*:


#### Rime of the Ancient Mariner
>It is an ancient Mariner,
>And he stoppeth one of three.
>‘By thy long grey beard and glittering eye,
>Now wherefore stopp’st thou me?

#### Amazing Grace
>Amazing Grace, how sweet the sound,
>That saved a wretch like me!
>I once was lost, but now am found,
>Was blind, but now I see.

#### Casey at the Bat
>The outlook wasn't brilliant for
>The Mudville Nine that day;
>The score stood four to two, with but
>One inning more to play.

#### House of the Rising Sun
>There is a house in New Orleans,
>They call the rising sun.
>And it's been the ruin of many a poor girl,
>And God, I know I'm one.

And because they're interchangeable, you can also sing any of them to *Amazing Grace* or *House of the Rising Sun*. But also it becomes very challenging to remember how House of the Rising Sun goes once you've sung all four to *Gilligan's Isle*. 

I don't think I can put it better than Isaac Newton, who famously said, 

>To ev'ry poem there is structure, 

With those words, the Egyptians and the Hittites were united. Alas, Newton was brutally murdered well before he could bring those words to life. As ever, I am bound to bring conclusion to his nobler works, just as his ghost is bound to pester me until I do. Therefore, I now present

# FlowJacker
*Yo it's about that time
To bring forth the rhythm and the rhyme*

There are two major components to the structure or flow of prose: rhythm and rhyme, alternately stated as 'stress pattern' and 'similarity of word pronunciation after last stressed vowel'. There are other components, like repetition, that will be ignored here. We'll even ignore internal rhyme, because that's a complicated problem (in theory solved by finding rhyming words that occur after equal syllable counts within a line or between lines, but it's even more complicated than that).I'm a busy guy so we'll just look at our major two and you can deal with it, thank you.

### Interpreter

Going back to our examples, we first want to convert 

>Amazing Grace, how sweet the sound,
>That saved a wretch like me!
>I once was lost, but now am found,
>Was blind, but now I see.

into something a little more palatable:

>01011111 A
>010111 B
>01011101 A
>011111 A

Now *that's* what I call music.

Note that line syllable counts and rhyme scheme are both more important/consistent than stress pattern. A poet might be able to tell you which stresses are most important (perhaps first and last stresses), but I am not that poet and I will just use stress pattern as a secondary measure of equivalence. The stress patterns can come from the Carnegie-Melon University Pronunciation Dictionary (CMU Dictionary, for short), via the python library [*pronouncing*](https://github.com/aparrish/pronouncingpy)

In order to effectively and quickly take care of rhyme scheme, I, in my infinite generosity, just used somebody else's code. Now [*poetry-tools*](https://github.com/hyperreality/Poetry-Tools), too, is included in my magnificence. I copied the code directly rather than importing the library because a few changes needed to be made to make it work with *pronouncing*. Rhymes are determined as stated above: pronouncing gives the rhyming part of the word, and then code that I didn't need to steal compares two of them. Then code that I did need to steal checks the end of each line and labels them somehow(?)

In practice, we can't get results at this level of clarity. The CMU (or at least the edition I'm working with) has mistakes. I was briefly puzzled by the pronunciation of A42128 listed for the word 'a'- you know what? I'm still puzzled by that. Anyway, the CMU gives multiple pronunciations for lots of words and gives no pronunciations for other words. One might think that the obvious solution is to move the pronunciations from the words with many to the words with none, but then one is a filthy communist who isn't even considering context. Instead, the solution is to be overzealous wherever necessary, like a filthy capitalist who isn't even considering context. False positives at least give us results to look at, false negatives give us nothing.

As an attempt at this overzealousness, we handle our two issue cases accordingly:

1. Multiple pronunciations: Check which stresses match and keep those, replace the others with an asterisk.

2. No pronunciations: Stick an X in there for arbitrary pronunciation and syllables. 

Amazing Grace run through the final version of my interpreter looks like this:

>010111\*1 A
>\*1\*111 B
>11\*111\*1 A
>\*11111 B

And it is one of the stronger results. Still, line length is almost always accurate, with the two exceptions being words with multiple lengths and words with no listed pronunciation. 


### Getting Lyrics

Lyrics are one of those things that you're allowed to listen to, memorize, and sing, but not write down and share with others. That doesn't stop lyric websites by and large, and so they live in abundance with no natural predators. But it did cost me access to an API and a downloadable database. But for this part of the puzzle, I'd have been able to make this post last week. 

Instead we have to become predators. And by predators, I mean people who predate. And by predate, I mean forego. And by forego, I mean waive. And by waive, I mean kick. And by kick, I mean scuff. And by scuff, I mean scrape. What I'm getting at here is that we have to scrape some lyric websites. Is this against the law? I'd bet that it is technically downloading copyrighted content, but then so is just visiting the website. I don't know what exactly is being protected by lyric copyrights, but I'm sure this is an acceptable use case. Still, to be safe, it'll be best to do this programming with a leather jacket and sunglasses on. 

But we don't have to wear those sunglasses for long! This problem has already been solved by [*pylyrics*](https://github.com/geekpradd/PyLyrics) and, for my purposes, [*pylyrics3*](https://github.com/jameswenzel/pylyrics3). 

We'll make sure to save an interpreted copy of any lyrics we scrape, which will hopefully speed up the process with each search. 

### Comparing Lyrics

