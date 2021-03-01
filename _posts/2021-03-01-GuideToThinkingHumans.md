---
layout: post
title: Summary of Guide to Thinking Humans
subtitle: By Melanie Mitchell
tags: [agi, books, summary]
---

1. Dartmouth summer of AI
	1. John McCarthy 
	2. Marvin Minsky
	3. Allen Newell
	4. Herbert Simon
2. intelligence can be binary (something is or is not intelligent), on a continuum (one thing is more intelligent than another thing), or multidimensional (someone can have high verbal intelligence but low emotional intelligence). Indeed, the word intelligence is an over-packed suitcase, zipper on the verge of breaking.
3. The lack of a precise, universally accepted definition of AI probably has helped the field to grow, blossom, and advance at an ever-accelerating pace.
4. What is AI -  branch of computer science that studies the properties of intelligence by synthesizing intelligence.
5. Some approaches to agi - 
	1. mathematical logic and deductive reasoning, Symbolic Ai
	2. inductive methods, statistical, 
	3. inspiration from biology and psychology
	4. Subsymbolic Ai 
	5. **General Problem Solver, Herbert Simon and Allen newell**
7. Problem with NN
	1. a perceptron’s weights and threshold don’t stand for particular concepts. It’s not easy to translate these numbers into rules that are understandable by humans.
8. If I could open up your head and watch some subset of your hundred billion neurons firing, I would likely not get any insight into what you were thinking or the “rules” you used to make a particular decision. **However, the human brain has given rise to language, which allows you to use symbols (words and phrases) to tell me—often imperfectly—what your thoughts are about or why you did a certain thing**. In this sense, our neural firings can be considered subsymbolic, in that they underlie the symbols our brains somehow create.
9. Rumelhart and McClelland, Parallel Distributed Processing, 1986, Connectionism
10. Subsymbolic systems seem much better suited to perceptual or motor tasks for which humans can’t easily define rules , "bad at logic, good at Frisbee."
11. While there have been some attempts to construct hybrid systems that integrate subsymbolic and symbolic methods, none have yet led to any striking success. 
12. Simulated vs Actual Intelligence, imitation game, Eugene Goostman chatbot
13. Singularity university
14. I assert that the fundamental mode of learning of human beings is experiential. Book learning is a layer on top of that.… If human knowledge, especially knowledge about experience, is largely tacit, i.e., never directly and explicitly expressed, it will not be found in books, and the Kurzweil approach to knowledge acquisition will fail.… It is not in what the computer knows but what the computer does not know and cannot know wherein the problem resides.
15. But as Singularitarians have pointed out, sometimes it’s hard to see an exponential trend if you’re in the midst of it. If you look at an exponential curve like the ones in figure 5, Kurzweil and his adherents imagine that we’re at that point where the curve is increasing slowly, and it looks like incremental progress to us, but it’s deceptive: the growth is about to explode.
16. **David Hubel and Torsten Wiesel** were later awarded a Nobel Prize for their discoveries of hierarchical organization in the visual systems of cats and primates (including humans)
17. It’s important to note that a top-down (or feed-backward) flow of information (from higher to lower layers) also occurs in the visual cortex; in fact, there are about ten times as many feed-backward connections as feed-forward ones. However, the role of these backward connections is not well understood by neuroscientists, although it is well established that our prior knowledge and expectations, presumably stored in higher brain layers, strongly influence what we perceive.
18. long tail problem, deep learning, A commonly proposed solution is for AI systems to use supervised learning on small amounts of labeled data and learn everything else via unsupervised learning.
19. **We have vast background knowledge of the world, both its physical and its social aspects. We have a good sense of how objects—both inanimate and living—are likely to behave, and we use this knowledge extensively in making decisions about how to act in any given situation.**
20. The machine learns what it observes in the data rather than what you (the human) might observe.
21. What, precisely, are these networks learning? In particular, what are they learning that allows them to be so easily fooled? Or perhaps more important, are we fooling ourselves when we think these networks have actually learned the concepts we are trying to teach them?
22. Does deep learning exhibit “true understanding,” or is it instead a computational Clever Hans responding to superficial cues in the data?
23. The most surprising good performer was “random search”: instead of training a Deep Q-Network by reinforcement learning over many episodes, one can simply try out many different convolutional neural networks with randomly chosen weights. Uber AI Labs
24. Another relatively simple algorithm, a so-called genetic algorithm,7 outperformed deep Q-learning on seven out of thirteen games.
25. "The system has learned no such thing; it doesn’t really understand what a tunnel, or what a wall is; it has just learned specific contingencies for particular scenarios. Transfer tests in which the deep reinforcement learning system is confronted with scenarios that differ in minor ways from the ones on which the system was trained show that deep reinforcement learning’s solutions are often extremely superficial."  — Gary Marcus 
26. Deep Q-Learning not able to generalise, change color of bacground etc.
27. In 2016, Stanford University’s natural-language research group proposed such a test, one that quickly became the de facto measure of “reading comprehension” for machines. The Stanford Question Answering Dataset, or SQuAD, as it is commonly known, consists of paragraphs selected from Wikipedia articles, each of which is accompanied by a question. 
28. Winograd schemas
29. The psychologist Lawrence Barsalou is one of the best-known proponents of the “understanding as simulation” hypothesis. In his view, our understanding of the situations we encounter consists in our (subconsciously) performing these kinds of mental simulations. Moreover, Barsalou has proposed that such mental simulations likewise underlie our understanding of situations that we don’t directly participate in—that is, situations we might watch, hear, or read about. He writes, “As people comprehend a text, they construct simulations to represent its perceptual, motor, and affective content. Simulations appear central to the representation of meaning.
30. Barsalou and of Lakoff and Johnson: we understand abstract concepts in terms of core physical knowledge. If the concept of warmth in the physical sense is mentally activated (for example, by holding a hot cup of coffee), this also activates the concept of warmth in more abstract, metaphorical senses, as in judging someone’s personality, and vice versa.
31. This circular causality is akin to what Douglas Hofstadter called the “strange loop” of consciousness, “where symbolic and physical levels feed back into each other and flip causality upside down, with symbols seeming to have free will and to have gained the paradoxical ability to push particles around, rather than the reverse.”
32. Though prescientific idea germs like ‘believe,’ ‘know,’ and ‘mean’ are useful in daily life, they seem technically too coarse to support powerful theories.… Real as ‘self’ or ‘understand’ may seem to us today … they are only first steps towards better concepts.” Minsky went on, pointing out that our confusions about these notions “stem from a burden of traditional ideas inadequate to this tremendously difficult enterprise.… This is still a formative period for our ideas about mind.
33. Cyc(Opencyc) is a symbolic AI system of the kind I described in chapter 1—a collection of statements (“assertions”) about specific entities or general concepts, written in a logic-based computer language. Here are some examples of Cyc’s assertions (translated from logical form into English):
	1.  An entity cannot be in more than one place at the same time.
	2.  Objects age one year per year.
	3.  Each person has a mother who is a female person.
34. “Forming abstractions” was one of the key AI abilities listed in the 1955 Dartmouth AI proposal that I described in chapter 1. However, enabling machines to form humanlike conceptual abstractions is still an almost completely unsolved problem.
35. Bongard Problem, Mikhail Bongard
36. AI research often uses so-called microworlds—idealized domains, such as Bongard problems, in which a researcher can develop ideas before testing them in more complex domains.
37. Conceptual Slippage
	1. Suppose that the string of letters abc changes to abd. How would you change the string pqrs in the “same way”
	2. Suppose that the string abc changes to abd. How would you change the string ppqqrrss in the “same way”?
38. https://github.com/fargonauts/copycat
39. Analogy making as perception
40. An essential aspect of human intelligence—one that isn’t discussed much in AI these days—is the ability to perceive and reflect on one’s own thinking. In psychology, this is called metacognition. Have you ever struggled unsuccessfully to solve a problem, finally recognizing that you have been repeating the same unproductive thought processes? This happens to me all the time; however, once I recognize this pattern, I can sometimes break out of the rut. Copycat, like all of the other AI programs I’ve discussed in this book, had no mechanisms for self-perception, and this hurt its performance. The program would sometimes get stuck, trying again and again to solve a problem in the wrong way, and could never perceive that it had previously been down a similar, unsuccessful path.
41. https://github.com/rava-dosa/metacat , Metacognition, James Marshall, James Marshall, at the time a graduate student in Douglas Hofstadter’s research group, took on the project of getting Copycat to reflect on its own “thinking.” He created a program called Metacat, which not only solved analogy problems in Copycat’s letter-string domain but also tried to perceive patterns in its own actions
42. https://github.com/quinnmax/situate , My collaborators and I are developing a program—called Situate—that combines the object-recognition abilities of deep neural networks with Copycat’s active-symbol architecture, in order to recognize instances of particular situations by making analogies. We would like our program to be able to recognize not only straightforward examples, such as the ones in figure 48, but also unorthodox examples that require conceptual slippages. The prototype “walking a dog” situation involves a person (a dog walker), a dog, and a leash. The dog walker is holding the leash, the leash is attached to the dog, and both dog walker and dog are walking.
43.  Our intuitive knowledge of physics lets us reason that Obama’s foot will cause the scale to overestimate the weight of the person on the scale.



