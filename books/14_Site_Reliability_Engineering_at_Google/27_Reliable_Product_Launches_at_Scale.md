## 27. Reliable Product Launches at Scale

- Launching a new product or feature is the moment of truth for every company
- Google has dedicated engineers who are in charge of releases. They help bridge the trade-off between launching often (which SWEs want) while keeping things running reliably (which SREs are focused on)
- Launch processes should be lightweight yet robust
- (*Note: This trade-off really seems to be at the core of being an SRE. It shows up over and over again in the book*)
- > Experience has demonstrated that engineers are likely to sidestep processes that they consider too burdensome or as adding insufficient value—especially when a team is already in crunch mode, and the launch process is seen as just another item blocking their launch
- Changes should be rolled out gradually, regardless of whether they are changes on the server or on phones
- > To cite a specific example of overload behavior: a service logged debugging information in response to backend errors. It turned out that logging debugging information was more expensive than handling the backend response in a normal case. Therefore, as the service became overloaded and timed out backend responses inside its own RPC stack, the service spent even more CPU time logging these responses, timing out more requests in the meantime until the service ground to a complete halt
