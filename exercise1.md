# CI with Python
## context
a six-person team is preparing to release their project and want to make sure the deployment pipeline is clearly defined and robust.
## code hygiene
We need automatic __linting__, __testing__ and __building__. 
### linting
Python provides an extensive normative style-guide, `PEP8`, and a linting module that implements it, `pycodestyle`. The most popular linters, however, use a superset of the `PEP8` norms. These supersets vary and are very configurable. The two main systems are `Pylint` and `flake8`, either of which would work for any size of project.

Some people use an auto-formatter as a part of their continuous integration, making the pipeline more forgiving for developers. But there are tradeoffs there: it means that the code that gets processed isn't the exactly the code submitted by the developer, and it's not hard to see how that might cause problems. The simplest solution is probably also normally better: code that doesn't satisfy the linter should just be rejected cold and fixed by its owner.
### testing
the standard testing framework is `pytest` and the standard test-runner is `tox`, which simulates a clean install on each run of the tests, smoothing the transition for the developer to CI tests and going some way to eliminating 'runs-on-my-machine' syndrome.

### build
Python is basically free of any build-step.

## CI management
Apart from Github Actions and Jenkins, a very popular CI pipeline makes use of `travis-ci`, a cloud service that watches specified brances of a github repo and runs the branch's tests against the repo in a virtual machine whenever it notices a new commit. Of course, there's no way to reject a commit to master once it's already made, so `travis` should watch a `testing` branch instead.

There's an associated service called `coveralls` which hooks into this pipeline very easily and quantifies test-coverage.
## Self-hosted or cloud?

It's likely that this project would be best served by cloud services, so long as the security of the codebase wasn't a very high priority. A six-person team preparing a crunch should be minimising time spent fiddling with config files, and the cloud solutions make it easy to dodge this burden. The only exception would be if the overhead of the testing suite was too high, because of the complexity of the tests, of the platform, or of the size of the testdata. Any of these conditions could overwhelm the resources available as usual on the cloud, and might necessitate self-maintained infrastructure. But it's rather late in the day for that...