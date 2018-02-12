---
layout: post
title:  "The Spatializer"
date:   2018-01-22
categories: audio experiments
---

When Isaac Newton first stepped out onto Plymouth Rock, he looked out and saw fields of blood and despair. This land was not a fertile place, but he would make do. After all, he had no other choice. He knew what he had done, and so he knew what he must do. In penance, he made a sacred vow to himself, a vow to make a world where good could again triumph. He vowed that one day someone would write a program to arrange music into physical space by pitch, and that it wouldn't sound like crap.

Today is the day that I fail to fulfill that vow, once and for all. 

# THE SPATIALIZER

Without further ado, here's some more ado for you to look at.

## Binaural Audio
*This is getting out of hand. Now there are two of them!*

If you're not already familiar with the concept, listen to [this youtube video](https://www.youtube.com/watch?v=IUDTlvagjJA) with headphones. It's worth it, it's the sort of thing you might describe as "neat". 

Humans use an impressive set of auditory cues to determine the origin of sound. We could talk about pinna filtering, which is the fascinating and complex effect of the shape of the pinna on the distribution of frequencies coming from different directions. But replicating that effect is either very difficult or totally impossible. That's not the only obscenely complex part of binaural audio, getting everything spot on could take months. Instead, we're going to focus on ITDs and IIDs. 

### Interaural Time Differences

Pop quiz! What do these three equations have in common?

![Eq. 1](/assets/spatializer/itd_1.gif "Ooh, math.")

![Eq. 2](/assets/spatializer/itd_2.gif "What! A piecewise equation! Gads!")

![Eq. 3](/assets/spatializer/itd_3.gif "I suppose you could cheat at this quiz by looking at the filenames in the source, but it's not a real quiz so I'm not held to the same standards. Even then, I've had quizzes count for a grade where the answer was in the source, so I'm at least operating at that level.")

Answer! They are three different answers to the following question (or variations on it):

![Binaural Diagram](/assets/spatializer/diagram.png "I drew this diagram about 15 times, and look at it. It's awful. This shouldn't be hard. I don't know what to do.")

The number we're looking for is X-Y, or the difference in distance it takes for the sound to hit the right ear after the left. Our brains solve the opposite problem every time you hear a sound, so I thought I would be able to solve it myself with trig (eq. 1). But reading the wikipedia page for sound localization, I learned that the difference is frequency dependent (eq. 2). Those numbers seemed absurdly high (maybe they were not, I do not know), so I sought out and found eq. 3 in a pdf for a CSE class somewhere. Why are they all different? How do you derive the second two? Why is the second one so much larger? What's going on here?

I'd like to contend for a moment that I've tapped into some kind of mathematical conspiracy. I don't its function or who the mastermind behind it is, but I do know this one thing: Stephen Hawking is certainly involved at a very high level, and his plans are darker than the black holes he studies.

### Interaural Intensity Differences

It's the same problem, but with some curiousities. If sound is taking longer to reach a different ear, and if it's reflecting through and around the head, the amplitude is going to be lower or greater than the original. This math is more complicated (and I wasn't exactly nailing the first part), and I can not explain it further.


![Meaningless Garbage](/assets/spatializer/iid.gif "Look! Numbers! Science!")

## Program

Here is the program, in its entirety. Be warned, it is as unnattractive as it is questionably accurate.:

~~~~ 
import sys
import math
import time
from pydub import AudioSegment
import pydub.scipy_effects

freqs = [
 16.3516, 17.3239, 18.3540, 19.4454, 20.6017, 21.8268, 23.1247, 24.4997, 25.9565, 27.5000, 29.1352, 30.8677,
 32.7032, 34.6478, 36.7081, 38.8909, 41.2034, 43.6535, 46.2493, 48.9994, 51.9131, 55.0000, 58.2705, 61.7354,
 65.4064, 69.2957, 73.4162, 77.7817, 82.4069, 87.3071, 92.4986, 97.9989, 103.826, 110.000, 116.541, 123.471,
 130.813, 138.591, 146.832, 155.563, 164.814, 174.614, 184.997, 195.998, 207.652, 220.000, 233.082, 246.942,
 261.626, 277.183, 293.665, 311.127, 329.628, 349.228, 369.995, 391.995, 415.305, 440.000, 466.164, 493.883,
 523.251, 554.365, 587.330, 622.254, 659.255, 698.456, 739.989, 783.991, 830.609, 880.000, 932.328, 987.767,
 1046.50, 1108.73, 1174.66, 1244.51, 1318.51, 1396.91, 1479.98, 1567.98, 1661.22, 1760.00, 1864.66, 1975.53,
 2093.00, 2217.46, 2349.32, 2489.02, 2637.02, 2793.83, 2959.96, 3135.96, 3322.44, 3520.00, 3279.31, 3951.07,
 4186.01, 4434.92, 4698.64, 4978.03, 5274.04, 5587.65]


def getDelay(ear_separation, angle, freq): #distances in centimeters
	angle = math.radians(angle)
	source_distance = 50
	return((9/34)*(angle+math.sin(angle)))

def getGain(angle, freq):
	angle = math.radians(angle)
	l_perc = ((1+math.cos(angle+math.pi/2))*freq + 2*34/9)/(freq + 2*34/9) - 1
	r_perc = ((1+math.cos(angle-math.pi/2))*freq + 2*34/9)/(freq + 2*34/9) - 1
	return (l_perc, r_perc)

def genDelayGain(mono, angle, order, low_freq=0, high_freq=math.inf):
	if low_freq == 0:
		if high_freq == math.inf:
			filtered = mono
		else:
			filtered = mono.low_pass_filter(high_freq, order=order)
	elif high_freq == math.inf:
		filtered = mono.high_pass_filter(low_freq, order=order)
	else:
		filtered = mono.band_pass_filter(low_freq, high_freq, order=order)
	left = right = filtered
	delay = getDelay(21.5, angle,1)
	l_gain, r_gain = getGain(angle, 1)
	spacer = AudioSegment.silent(math.fabs(delay))
	if delay<0:
		left = spacer + (left + l_gain)
		right = (right + r_gain) + spacer
	elif delay>0:
		right = spacer + (right + r_gain)
		left = (left+l_gain) + spacer
	return(left, right)


def spatialize(mono, span=(52, 68), resolution = 2, order = 5):
	outsong = AudioSegment.silent(len(mono)+500)
	l = (span[1]-span[0])
	scalar = 180/(l-1)
	start = int(span[0]/resolution)
	end = int(span[1]/resolution)
	cur_freq = 0
	for n in range(start, end):
		n_res = resolution*n
		pivot = (freqs[n_res]+freqs[n_res+1])/2
		print("Splitting at", pivot)
		angle = -1* ((n-start)*scalar*resolution - 90)
		left, right = genDelayGain(mono, angle, order, cur_freq, pivot)
		cur_freq = pivot
		outsong = outsong.overlay(AudioSegment.from_mono_audiosegments(left, right))
	left, right = genDelayGain(mono, angle, order, cur_freq)
	outsong = outsong.overlay(AudioSegment.from_mono_audiosegments(left, right))
	return outsong



if len(sys.argv) < 2:
    print("Usage: %s <filename>" % sys.argv[0])
    sys.exit(1)

filename = sys.argv[1]

song = AudioSegment.from_file(filename, format=filename[-3:])
mono = song.set_channels(1)


outsong = spatialize(mono)

outsong.export("fixed_%s" % filename, format=filename[-3:])

~~~~

Unfortunately, the outputs have to be tuned song by song (the program takes a few minutes to run, so this is a low-latency operation). As a result, I currently have only one working example. I'm working on a couple fixes which will hopefully be coming soon but which are currently not functional. 

Here is the example, part of a track from *Reprise* on the Spirited Away soundtrack.



[Reprise](/assets/spatializer/sa.mp3 "I think it's neat anyway.")

A big flaw with music that is already produced is that it's hard to tell how much the filter is adding. Because the program converts the music to mono, I think it's safe to assume that any spatial component is being generated. However, it's worth noting that the original is arranged not dissimilarly. 