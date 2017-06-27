Edited by 0xDEADBEF
Tue, 27 Jun 2017 12:43:07 GMT

# Drawing sensor

![](assets/[4]Drawingsensor-a751e.png)

## Description

When any particle is added at anywhere, this is activated.

## Mechanism

two conv are core. above conv's ctype is STKM and below conv's ctype is EMBR. each frame, the particle between them has two state STKM or EMBR.

Thus DTEC(EMBR) generates sprk to heat PTCT. Therefore SPRK can't flow on PSCN - PTCT - NSCN. And then if anything is added at anywhere then STKM will be spawned there. this part is so complicated.(it may be unexpected behavior.)

Actually it is not spawned there. But the particle which has two state is moved there. Finally DTEC stops generate sprk. And we all know sensor'll be activated.
