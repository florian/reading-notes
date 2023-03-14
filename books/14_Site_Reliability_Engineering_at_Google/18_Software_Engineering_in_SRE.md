## 18. Software Engineering in SRE

- SREs are in a good spot to build automated systems themselves since they are the domain experts and eventual users of these systems
- *Intent-based capacity planning*
    - > Specify the requirements, not the implementation
    - Instead of specifying the exact hardware requirements a service has, it should specify its goal
    - This can be done at various levels of abstractions, going from “I need N cores in cluster X” to “I want to run this service at a reliability of five 9s”
    - Having such a system makes it easier for users but also allows for more efficient, automated packing of resources
- *Auxon* is Google's implementation of such a system
    - This is a good example of SWE work done by SREs
    - Auxon represents constraints as a linear program which it solves
- Any SWE best practices also apply to these SWE projects that SRE work on
- > Ask yourself: if this product were created by a separate dev team, would you onboard the product?

  (*Note: That generally seems like a good practice when building infrastructure*)
