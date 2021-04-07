---
title: ZZ Misconceptions
---
Some people seem to have a few misconceptions about ZZ.  
To clear them up without having to repeat myself all the time, I've written this.

* toc
{:toc}

# EOCross is not ZZ, but CFOP
EOCross is a subset of ZZ (EOLine) *and* CFOP (Cross), so it can clearly be stated that EOCross can be categorized under both methods.
However, sometimes it is necessary to choose one method, which raises the question whether EOCross belongs more to CFOP or more to ZZ.  
The following is a collection of arguments for each side:

## For CFOP
- The Cross shape solved in the beginning is the same for both methods.
- Both utilize a pair approach for F2L like EOLine.
- EOCross does not use blockbuilding.
- Zbigniew Zborowski proposed ZZ as EOLine, not EOCross.
- Supported by most notably Jayden McNeill.[^1]

## For ZZ
- EOLine is closer to EOCross: The chance of accidentally solving two cross edges is higher than that of orienting all the edges.
- Both are (mostly) rotationless, while CFOP is not.
- Ryan Heise proposed EOLine and EOCross simultaneously, before Zborowski.[^2]
- They have exactly the same last layer.
- Both have mainly <RUL(D)>-gen *ZZ*F2L, while CFOP is predominantly <RUFy(L)>.
- EOCross planning is much more similar to EOLine:
  Edges are still oriented the same way, but four instead of two edge pieces are influenced.
- An EOLine solver already has all the knowledge to solve using EOCross (EO, F2L algorithms, Last Layer),
  while a CFOP solver has to learn a lot more new concepts like EO and rotationless solutions for special cases.
- Supported by the majority of EOCross users.

## Conclusion
In my personal opinion, even though EOCross could be categorized under both methods, it belongs more to ZZ than CFOP.  
This is mainly due to the fact that the knowledge obtained in ZZ EOLine transfers to EOCross much better
since the solving experience is generally very similar for both, which is especially true for the first and last steps.

# Moves which break EO are forbidden
F and B moves, slices and x and y rotations, although they misorient edges, can still be used in ZZ.  
There is no rule stating that you cannot use these moves:
Although they do not preserve EO, they can still be used in algorithms which in total still preserve EO.  
Examples of such algorithms are:
- E perm: `x' R U' R' D R U R' D' R U R' D R U' R' D'`
- U ZBLL: `M U' M' F R U R' U' F' M U M'`
- F2L 25: `F' R U R' U' R' F`, `R F R' U R U' F'`
- AF2L 8: `F' U2 L U L' U F`
- F2L 32: `(R' F R F' R')3`, `y' (R' U' R U)3`

# References
[^1]: <https://www.youtube.com/watch?v=VdlN_e1ltGU&lc=UgzL605lqwbZg_3JKhl4AaABAg.9DuBvpGpQIz9DvfepAH39s>{:target="_blank"}
      > "Calling EOcross ZZ just 'ZZ' feels wrong to me since ZZ was intended to be EOline>blockbuilding>ZBLL"

[^2]: <https://www.speedsolving.com/wiki/images/f/fe/Ryan_Heise%27s_ZZ_Proposal.png>{:target="_blank"}