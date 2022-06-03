## IIT-D Mtech Interview Experience

`
Interview Date - 12th May on Teams
Duration - 25 mins
Panelists - Madhukar Kumar, Subodh Sharma, Smruti Ranjan, Sandeep Sen.
`

Before the interview started they asked me what Im comfortable with and I said Systems.

- ``` What is Virtual Memory and why we need it?```
- [ ] Does every process require Virtual address? - Yes, explained about the security issue if we give them physical address directly.
- [ ] Who does the translation?
- [ ] Where is OS/Kernel involved in the above? - In case of page-faults/cache miss or external interrupts
- [ ] Do we need memory translation for every instruction? - Depends on TLB if we are using and also physical cache.
- [ ] TLB? Explain miss and hit along with physical cache?
- [ ] What is an ideal page size?(Took a random scenario and they asked for total pt overhead used pen/paper, took MM - 8 GB, Process - 8GB, PS = 4KB, explained the translation, also gave them a rough estimate of PTE and final total PT overhead)
- [ ] Does every process need a PT? (Yes)
- [ ] Do we need to flush the PT out of MM every time a new process comes in - took 2 scenarios and explained.
- [ ] Suggest another method where we can reduce PT overhead - segmentation : explained the concept of base and limit, and substantially reduce the overhead.
- [ ] Quick-Sort - Give one line formal statement, was not sure what were they expecting.
- [ ] kind of like how will you sell QS to me, Eg. Merge sort will sort any input in O(nlogn), similarly give me a formal statement 
- [ ] How will you sort an input of Strings - I said heap sort. How will comparison happen - Lexicographically.
