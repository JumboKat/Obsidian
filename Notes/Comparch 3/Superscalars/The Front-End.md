![[Pasted image 20261008182329.png|557]]
At the front end is decoupled from the rest of the pipeline by an instruction buffer. 
- The **branch predictor** guesses where the program goes next before a branch has been resolved. the **Branch Target Buffer (BTB)** remembers where branches jumped to previously, while the **Return Address Stack (RAS)** predicts where the function returns go.
- The **I-Cache** fetches predicted instructions.
- The **instruction buffer** stores fetched instructions in a queue. This allows fetch bubbles to be absorbed. Instructions can still be fetched if the core is busy, and the core can still issue instructions if there is a branch misprediction/i-cache miss.