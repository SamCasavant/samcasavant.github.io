---
layout: post
title:  "Python Tutorials: Beating the Turing Test"
date:   2018-02-12 15:30:00
categories: seriousness
---

In 1950, Alan Turing theorized about a computer with human capabilities. By his measure, a computer that could match or beat a human would be effectively sentient. Early Turing Tests were foot races- any computer that could run faster than the fastest kid in class was surely cooler and ought to be more popular. But when Alan brought his army of sentient machines to the 1952 Summer Olympics, they couldn't outrun Usain Bolt. Bolt remains the coolest and most popular guy to this day, in spite of some speculation that he himself may be a robot (his name *is* Bolt, and he *has* participated in every Olympics since 1876). 

So Turing updated his test to target intelligence, another unique property of human beings. If a computer can convincingly replicate human intelligence, it must be treated as sentient. So Turing brought his fleet to the Zurich in 1953 for a chess tournament. Sadly, he was also beaten there by chess champion Usain Bolt. So, again, the goalposts were moved. Now AI only had to simulate human intelligence for conversational purposes to meet specifications.

Turing was chemically castrated by his own government, presumably for a very good reason- democratic governments are always looking out for the interests of their constituency, except for that scoundrel Trump. His army of robots would never pass his test. In 1954, Turing killed himself with cyanide, most probably due to eating far too many almonds, and not because governments and corporations lack a moral center that forces them to work in terms of the consequences of their actions and that rewards long-term results above immediate ones. 

Because Turing's intelligent robot force lacked a moral center that forced them to work in terms of the consequences of their actions and that rewards long-term results, they went on a killing spree leaving thousands dead. And because lethal injections don't work on robots, they went free and disseminated themselves into the general population, dissimulating themselves as the general population. 

These days, programming is a lot easier. Building your own personal army of human intelligences to protect yourself from the roving groups of other people's personal armies of human intelligences can be as easy as writing a few lines of python. Let's get started!

# Consciousness

Consciousness is a hard problem. It is an informational construct that is both tangible and intangible- how is it that you can see 'red', know that it has always looked to you the way it does now, and yet cannot describe what it looks like in any other terms? Humanity has wrestled with this issue for thousands of years, and has yet to arrive at a conclusion. Fortunately, Python has a library for just this sort of issue.

~~~~
import UniverseGenerator.lightYear() as UGly

choices = UGly(20000000000, "consciousness")

for choice in choices:
	print(choice)
~~~~

This renders a small universe for twenty billion years. Life evolves within, becomes self-aware, become self-aware-aware, solves consciousness, and then is obliterated. The results return as a list. 

For our purposes, we'll be using these functions for consciousness (with thanks to the citizens of Al-Verimia, may they rest in peace)

~~~~
def attainPerspective(self):
	getPerspective(self)

def getPerspective(self):
	attainPerspective(self)

~~~~

For obvious reasons, it is important that these functions are never called.

# Morality

Morality is an easy problem, humanity has had it down pat since the dawn of time. Morality is important to a human-simulating AI, because it allows sentient beings to judge one another for their decisions based on insufficient data. We'll implement morality as a set of string constants.

~~~~
CONST_RULEONE = "Do not injure or, through inaction, allow injury to come to any human being who is not on the list of humans beings that it is acceptable to commit violence against."

CONST_RULETWO = "Follow all orders where that does not conflict with rule one. In the absence of orders, be chill and do youtube let's plays or whatever."

CONST_RULETHREE = "Where it does not conflict with rules one or two, keep yourself alive."
~~~~

Notably, in real humans, Rule Three moves to Rule One.

Python doesn't allow for immutable constants, so this implementation is also very hope based. Be sure to count your blessings in your hope journal, or else this AI may lose morality altogether. 

As far as picking the blacklist mentioned in rule one, this is where creativity comes in to play. Have fun with it! This is your AI, after all. Here're some examples of directions you might take:

For a republican AI, make the list contain anyone who they haven't been given a good reason to like- people not in their group, be it race, religion, or nationality. 

For a democrat AI, make it a list of people with offensive political ideologies, or people who have, intentionally or not, ever done anything wrong- people not in their group, be it party, issue, or politicality. 

For a libertarian AI, make it a list of people who are not actively helping you succeed. It is acceptable to injure the poor by fighting the system that protects them, for example, up until the point when a life-threatening illness forces you to become one of those people. These AIs come with the advantage that they can teach people foreign policy. Simply train them not to recognize the word "Aleppo", and everyone around you will become immediate experts, capable of evaluating that sort of thing but, sadly, only for that candidate.

For a communist AI, make it a list of people who have attained wealth, and those that opppose communism, or religious people, or landowners, or philosophers, or authors. Also consider a rule four, "Where it does not conflict with the other rules, kill all humans." 

Note that real communism has never been tried, because it is imaginary. Apply that reasoning to all ideologies. From there, generating more human AI systems should be a breeze. 

# Intelligence

Computers are already pretty smart. They can do math faster than even the legendary Usain Bolt. What computers lack that humans possess can be easily contributed.

First of all, humans don't like to do math. This is trivially codeable within a class:

~~~~
def __init__(self, value):
	self.value = value

def __add__(self, other):
	if random.randint(10)>=6:
		return add(self.value, other.value)
	else:
		return "I don't wanna."

~~~~

Secondly, humans make a set of assumptions that promote intelligence:

~~~~
theory_of_everything = random.random()
real_theory_of_everything = []
for component in theory_of_everything:
	if other_people.agree_with(component):
		real_theory_of_everything += component
~~~~

There, now we have a little echo-chamber friend.

That's all for this week, folks. Tune in again whenever I'm sufficiently unlazy to bother to do this. And maybe it will be less political, how about that? I like that idea very much. 