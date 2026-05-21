# Restriction Enzyme DNA digest prediction (RE-D-DiP)
Script to fragment real DNA sequence by RE digestion 

I’m going to tell you something we already know: the natural world is not strictly uniform. Is it symmetrical? Sure, often (eyes, ears, arms, etc.). Patterned and mathematical? Yes, like butterfly wings or a nautilus shell, but these amazing creations are not uniform. There is uniformity, to be sure, like the size of cells within the same tissue, or in clones of bacteria, but I’m thinking about this from the perspective of DNA restriction endonuclease (RE) sites for DNA digestion.

Why ask this? I had a problem with long DNA fragments: they were bad for my NGS libraries and, in upstream preparation steps, they created issues with column flow and sample yield. To digest the DNA, I wanted an enzymatic solution because it would be cheaper and faster than COVARIS shearing and would be an easier workflow. I knew I needed to create fragments below 20 kb but longer than 300 bp, and ideally around 2 kb.

Given these constraints, I was left with one key question: how many RE sites for a given enzyme (or blend of enzymes) exist within the human genome and at what frequency?

Let’s take a step back and make the same mistake we always do: we approximate with bad assumptions. Enrico Fermi was famous for (among other things) his power of approximation; back-of-the-envelope quick calculations that are shocking close to the real answer but with minimal effort. The “piano tuner” example is rather well known, in which we estimate how many piano tuners are needed in a city. We do things like assume how many people live in the city, and how many people live in the same home, and assume a certain number of homes have a piano, and assume a piano needs to be tuned at a set frequency. We do the math and we get a very good approximation (based on reasonable assumptions) of how many piano tuners can find gainful employment. That’s why Fermi was so cool.

https://en.wikipedia.org/wiki/Fermi_problem

So, how many RE sites for a given enzyme exist within the human genome?
Sadly, this is where a Fermi-style approximation could fail, because a key assumption we might make is wrong from the start: that biology is uniform, or more specifically, that the order of DNA bases is randomized in a genome, and therefore RE sites will appear at a fixed frequence along the length of a DNA molecule. 
From 4 DNA bases, choose 6 bases at random to make a cutting site = a valid RE site every 4096 bases +/- some deviation -> a normal distribution assumption. 

So, we expect an enzyme that is a 6-base cutter to chop a human genome into 4 kb chunks with minimal deviation from that average. But the base order of DNA is not random. It took millions of years for evolution to pains takingly generate specific binding sequences for transcription factor occupancy (i.e. gene regulation), to create repeat elements, to be invaded by randomly inserting retro-transposons, and to collect DNA replication mistakes over billions of generations. And a DNA strand itself can form tertiary structures that have biological function, as in telomeres and centromeres, because of how non-uniform sequences can fold and stack together. Real DNA sequence is anything but random. 

Here is my neat data science contribution. Yes, it’s been done before, but this baby is mine. To determine which enzyme(s) cocktail I would need to satisfy my digestion requirements, I wrote an R script that leans heavily into the BiocManager and DECIPHER packages to analyze the real sequence of a genome and plot the actual fragment sizes in a histogram.

My take-home learning from this was two-fold:

1) (spoiler alert?) RE cut sites are not uniformly distributed within the human genome. Indeed, for EcoRI there are more sites than predicted by chance alone. Since the RE recognition site is G/AATTC it suggests (to me without a priori knowledge) that our genomes are more selectively AT-rich than by chance alone. Is this true for all human chromosomes, or just the X chromosome that I checked for this example? How would this pattern look for a bacterial extremophile with high GC content?

2) A single 6-base cutter was actually fine for my purpose of generating fragments below 20 kb. Even though a mix of enzymes could better tune the fragments to a tighter average length, just one alone was good enough, so by the razor of Occam, so shall it be.

Now, after all this obviousness and re-inventing of the wheel (but I had fun doing it), I would like to leave you with one thought to think over: given that the universe is not uniform, and science follows the law of the universe, what bad assumptions might we be making about our science? Maybe we’re not even aware of our implicit assumptions and biases, but as scientists, now and for the future, maybe we should think twice about what we think we know.
