### 19. Critique: Google's Code Review Tool

- *Critique* is Google's code review tool
- Diffing works using longest common subsequences, with some optimizations on top
- (*Note: This inspired me to implement a diffing algorithm on my own and write a [blog post](https://florian.github.io/diffing) about it*)
- Users in Critique can explicitly mark whose turn it is to make changes or to review. This emphasizes the turn-based nature of reviews
- Critique's homepage is a dashboard which shows open changes. Users can customize what exactly is shown
- Because Critique is too tightly integrated into the monorepo, there is a separate tool (*Gerrit*) for open source projects such as Chromium and Android
