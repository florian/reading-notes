### 24. Continuous Delivery

- > There is a saying among educators that no lesson plan survives its first contact with the student body. In much the same way, no software is perfect at first launch, and the only guarantee is that you’ll have to update it. Quickly
- Being able to quickly iterate on projects is a major competitive advantage
- Releasing faster can actually be safer. This might sound controversial, but the idea is that launching smaller batches is safer because you can identify problems more quickly and easily
- (*Note: Reading this, I'd re-phrase that as launching "more often" is safer, not necessarily more launching "more quickly"*)
- Feature flags
	- New features you want to launch can be guarded by flags
	- This allows you to only launch the feature to a small percentage of users to test it
	- It also allows you to submit to your main branch and only enable the feature when it is ready
	- Depending on the programming language / build system, you might be able to strip the unfinished code from the binary when the feature is not yet ready
- Regular releases improve developers' work-life balance because missing there is less reason to work overtime for a deadline since the next release should be soon anyways
- > the recommended best practice is to aim for change-neutral releases. All new features are flag guarded so that the only change being tested during a rollout is the stability of the deployment itself
- (*Note: The above statement referred to cases where apps do not have enough users for A/B tests with significant results. But it seems like it's generally good advice*)
