---
title: "Combinational Logic"
---
 
 ## Why we use computer?

 > the stability that comes from using decision criteria is the primary reason we build digital(discrete) computers

 ## Digital in an Analog World

<img src="../assets/ca25.png" width="800vw" height="600vw">

Above image is transfer function pull and ball. the transfer function produces different values of recorded birghness for differnt values of light. If too much of the light hits the shoulder of the curve, then the image will be overexposed, since the recorded bifhtness values will be closer together than in the actual scene. The goal is to adjust your exposure to hit the linear region, which will yeild the most failthful representation of reality. 
The volume control adjusts the gain, or steepness of the curve. As you can see, the higher the gain, the steeper the curve and the louder the output. A small change in the input causes a jump in the output at the steep part of the curve. It's like jumping from one finger to another after decision criterion, called a **threshold**. This partitions the continuous space into discrete regions, which is what we want for stability and noise immunity. 
With binary-coded decimal represenatation, you could represent bigger number with limited number of representation like your finger. You could represent more than 1000 with bits. Another reason why bits are better than digits for hardware is that with digits, there's no simple way to tweak a transfer function to get 10 distinct thresholds. Of course, if we could build 10 thresholds tn the same space as one, we'd do that. But as we've seen, we'd be better off with 10 bits instead of one digit.

## A Short Primer on Electricity

<img src="../assets/ca28.png" width="800vw" height="600vw">

Above figure is for understanding electicity's 0 and 1 compare to plumber. Just as it takes time for electricity to make its way across a computer chip, it takes time for water to flow or propagate through a pip. This effect is __propagation delay__. Electricity travels through a wire like water travels through a pipe. It's a flow of elctrons.
- the metal inside : conductor
- the covering on the outside : insulator
- valves : switch
- The electrical equivalent of water pressure : voltage
- The amount of flow : current
- Resistance is measured in __ohms__
<img src="../assets/ca212.png" width="800vw" height="600vw">

