---
layout: post
title:  "Finding Poetic Equivalence with FlowJacker"
date:   2018-02-05 23:35:00
categories: [lyrics, poetry, programming, experiments]
---

The alt text for [xkcd 778](https://xkcd.com/788/) mentions ballad meter- a structure of poem that let's us sing any of the following to the tune of Gilligan's Isle:


####Rime of the Ancient Mariner
>It is an ancient Mariner,
>And he stoppeth one of three.
>‘By thy long grey beard and glittering eye,
>Now wherefore stopp’st thou me?

####Amazing Grace
>Amazing Grace, how sweet the sound,
>That saved a wretch like me!
>I once was lost, but now am found,
>Was blind, but now I see.

####Casey at the Bat
>The outlook wasn't brilliant for
>The Mudville Nine that day;
>The score stood four to two, with but
>One inning more to play.

####House of the Rising Sun
>There is a house in New Orleans,
>They call the rising sun.
>And it's been the ruin of many a poor girl,
>And God, I know I'm one.

I don't think I can put it better than Isaac Newton, who famously said, 

>There's structural commonalities which unite lots of prose. If one could only generalize, their strength would surely grow. 

With those words, the Egyptians and the Hittites were united. Alas, Newton was brutally murdered well before he could bring those words to life. As ever, I am bound to bring conclusion to his nobler works, just as his ghost is bound to pester me until I do. Therefore, I now present

#FlowJacker
*Yo it's about that time
To bring forth the rhythm and the rhyme*

There are two major components to the structure or flow of prose: rhythm and rhyme, for programming's sake stated as 'stress pattern' and 'equivalence of word pronunciation after last stressed vowel.'. There are other components, like repetition, that will be ignored here. We'll even ignore internal rhyme, because that's a complicated problem (in theory solved by finding rhyming words that occur after equal syllable counts within a line or between lines, but it's even more complicated than that). We'll just look at our major two and you can deal with it, thank you.

Going back to our examples, we first want to convert 

>Amazing Grace, how sweet the sound,
>That saved a wretch like me!
>I once was lost, but now am found,
>Was blind, but now I see.

into something a little more palatable

>01011111 A
>010111 B
>01011101 A
>011111 A

Now *that's* what I call music.

Note that line syllable counts and rhyme scheme are both more important/consistent than stress pattern. A poet might be able to tell you which stresses are most important (perhaps first and last stresses), but I am not that poet and I will just use stress pattern as a secondary measure of equivalence.

In practice, we can't get results at this level of clarity. The Carnegie-Melon University Pronunciation Dictionary (CMU Dictionary, for short) gives multiple pronunciations for lots of words and gives no pronunciations for other words. One might think that the obvious solution is to move the pronunciations from the words with many to the words with none, but then one is a filthy communist who isn't even considering context. Instead, the solution is to be overzealous wherever necessary, like a filthy capitalist. False positives at least give us results to look at, false negatives give us nothing. Amazing Grace run through the final version of my converter looks like this:

>010111\*1
>\*1\*111
>11\*111\*1