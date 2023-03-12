## 12. Effective Troubleshooting

- > Be warned that being an expert is more than understanding how a system is supposed to work. Expertise is gained by investigating why a system doesn't work
- > Ways in which things go right are special cases of the ways in which things go wrong
- Troubleshooting is an iterative process: Based on your observations and your understanding of the system, you come up with potential causes of failure. You investigate them and potentially recurse
- Common pitfalls during debugging
    - Looking at symptoms that are not relevant
    - Not understanding how to change things in the system to get more information
    - Coming up with the wrong theories about what is wrong
    - Paying too much attention to spurious correlations
- Writing good problem reports
    - The expected behavior, the actual behavior, and steps for reproducing the issue should be described
    - Reports should have a consistent structure
    - They should be stored in a common place
- Good logging systems make it easy to filter logs by severity level
- Divide and conquer can be a good debugging strategy. Keep bisecting the problem
- Negative results (i.e. showing something doesn't work in certain cases) provide a lot of information, e.g. in picking designs
