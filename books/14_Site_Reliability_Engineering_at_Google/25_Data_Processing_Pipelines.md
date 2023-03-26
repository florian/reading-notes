## 25. Data Processing Pipelines

- Pipelines can run once, periodically or virtually non-stop
- For the latter two categories of repeated runs, the jobs are likely initially well configured to run fast with a certain number of workers. However, over time the data and environment change, and that config is no longer a good one
- > The key breakthrough of Big Data is the widespread application of "embarrassingly parallel" algorithms to cut a large workload into chunks small enough to fit onto individual machines
- When periodic jobs take longer than expected, they might not complete before the next one starts. In the worst case, the nearly completed run is killed over and over again
- Moiré load patter
    - When two larger pipelines run simultaneously and access the same resources at similar times, the load on these resources is very unevenly distributed
    - (*Note: I like the reference of the name: [https://en.wikipedia.org/wiki/Moiré_pattern](https://en.wikipedia.org/wiki/Moir%C3%A9_pattern)*)
- *Google Workflow*
    - A system developed to solve the issues above
    - It can be useful to think of it analogously to the model-view-controller (*MVC*) pattern for developing user interfaces
    - model: A server called the “task master” which maintains a database of jobs to run
    - view: worker that continually updates the system state by completing work
    - controller: supports auxiliary tasks that affect the pipeline, such as scaling the runtime
    - (*Note: I'm really not sure that analogy was helpful to me, even though I'm quite familiar with MVC*)
    - Workflow has strong consistency guarantees. Even if two workers attempt to do the same work, they will write results to different locations which ensures that the results will not be corrupted
    - If you expect a pipeline to run periodically over a long period of time, it should be built on a system like Workflow
