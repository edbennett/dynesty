# Changelog

All notable changes to dynesty will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/), and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [Unreleased]

### Added
### Fixed

## [1.2.3] - 2022-06-02

### Added
- The .copy() method was added to the results class, as the previous versions
  of dynesty had it.

### Fixed
- Fix the bug where previously you couldn't repeatedly pickle and unpickle
  a sampler
- Small speedup of ellipsoidal sampling

## [1.2.2] - 2022-04-12

### Added

### Fixed
- The problem with biased posteriors was fixed when using multi-ellipsoid
  bounds and rslice and rwalk samplers. Previously the chains did not satisfy
  detailed balance. (issue #364). Original discovery of the problem
  and help by Colm Talbot. In the case of complex posteriors, somewhat slower
  performance may be seen.
- Fix the issue introduced in 1.2.1 when the prior_transform returns a tuple or
  or a list (rather than numpy array). Now that should be accepted.



## [1.2.1] - 2022-04-04

### Added

### Fixed

- The arguments of prior_transform and likelihood function are now explicitely copied, so the sampling can work if those function apply changes to argument vectors ( #362 )
- Fix the compilation of the docs, and update them a bit

## [1.2.0] - 2022-03-31

This version has multiple changes that should improve stability and speed. The default dynamic sampling behaviour has been changed to focus on the effective number of posterior samples as opposed to KL divergence. The rstagger sampler has been removed and the default choice of the sampler may be different compared to previous releases depending on the dimensionality of the problem. dynesty should now provide 100% reproduceable results if the rstate object is provided. It needs to be a new generation Random Generator (as opposed to numpy.RandomState).

Most of the changes in the release have been contributed by [Sergey Koposov](https://github.com/segasai) who has joined the dynesty project.

### Added

- Saving likelihood. It is now possible to save likelihood calls history during sampling into HDF5 file (this is not compatible with parallel sampling yet). The relevant options are  save_history=False, history_filename=None (#235)
- add_batch() function now has the mode parameter that allows you to manually chose the logl range for the batch (#328)

### Changed

- More testing with code coverage of >90% + validation on test problems
- Internal refactoring reducing code duplication (saved_run, integral calculations, different samplers etc)
- Multiple speedups: ellipsoid bounds, bootstrap, jitter_run (#239, #256, #329)
- Exception is raised if unknown arguments are provided for static/dynamic samplers (#295)

- Migrate to new numpy random generator functionality from RandomState (#280)
- Make dynesty fully deterministic if random state is provided (#292)

- Remove the pointvol parameter used in internal calculations, such as ellipsoid splits (#284)
- Get rid of vol_dec parameter (#286)
- Improve bounding ellipsoids algorithms, for example how we are dealing with degenerate ellipsoids (#264, #268)
- Introduce more stable multi-ellipsoidal splitting using BIC (#286)

- Do not use KL divergence function for stopping criteria based on posterior, instead use the criterion based on the number of effective samples. The old behaviour can still be achieved by using the dynesty.utils.old_stopping_function (#332)
- Fix bugs in dynamic sampling that can lead to sampler not finding points in the interval (#244)
- Major refactor of rslice/slice sampling increasing its stability (#269, #271)
- Disable ncdim when slice sampling (#271)
- Change the defaults for slices/walks/bootstrap (vs number of dimensions) (#297)
- Change default samplers (vs ndim, i.e. use rslice for high dimensions) (#286)
- Remove rstagger sampler, as it was producing incorrect results/based on non-Markovian chains (#322)
- Fix rwalk sampler. Previously the chains were not Markovian (#319, #323, #324)
- Change step adaptation of rwalk and rslice (#260, #323)
- Change the calculation of evidence uncertainties, remove factor the unnecessary factor two, and improve numerical stability of calculations (#306, #360)
- Refactor the addition of batches in dynamic sampling, preventing possibly infinite loop (#326)
- Improve stability of resample_equal (#351)
- New Results interface with a dedicated object, rather than a wrapper around the dictionary (#330)

## [1.1.0] - 2021-04-05
- Improved behavior and stability of the bounding distributions (with
  `Sergey Koposov <https://github.com/segasai>`_ and
  `Johannes Buchner <https://github.com/johannesbuchner>`_).

- Added support for specifying the number of clustering dimensions (`'ncdim'`)
  in case these may differ from the number of prior dimensions (`'npdim'`)
  (with `Colm Talbot <https://github.com/ColmTalbot>`_).

- Fixed a bug where ``dynesty`` was not properly enforcing nested sampling's
  monotonically-increasing likelihood condition when sampling
  (with `Colm Talbot <https://github.com/ColmTalbot>`_).

- Improved ability to save sampler objects to disk to backup progress (with
  `Patrick Sheehan <https://github.com/psheehan>`_ and
  `Alex Nitz <https://github.com/ahnitz>`_).

- Limited support for user-defined proposal strategies (with
  `Gregory Ashton <https://github.com/GregoryAshton>`_).

- Additional small bugfixes, references, and documentation updates.

## [1.0.1] - 2020-01-17
### Changed
- Small quality-of-life improvements to plotting.

- Added citation tool.

## [1.0.0] - 2019-09-22
### Changed
- Added support for period and reflective boundaries (with
  `Gregory Ashton <https://github.com/GregoryAshton>`_).

- Added support for interactive progress bar (with
  `Daniel Foreman-Mackey <https://github.com/dfm>`_).

- Added support for stopping criterion based on ESS (with
  `Colm Talbot <https://github.com/ColmTalbot>`_).

- Small bugfixes to code and documentation.

- Small quality-of-life improvements.

## [0.9.7] - 2019-06-13
### Changed
- Ensemble bounds can now adapt to elongated distributions (with
  `Johannes Buchner <https://github.com/JohannesBuchner>`_).

- Random walks now behave differently near boundaries (with
  `Gregory Ashton <https://github.com/GregoryAshton>`_).

- Pickling sampler states should now work better in Python 3 (with
  `Dustin Lang <https://github.com/dstndstnr>`_.

- Doubled output errors in default approximation in line with theoretical
  expectations.

- Small bugfixes and docfixes (with
  `Patricio Cubillos <https://github.com/dstndstnr>`_).


## [0.9.5.3] - 2019-03-29
### Changed
- Various small bugfixes, with contributions by
  `Gregory Ashton <https://github.com/GregoryAshton>`_ and
  `Johannes Buchner <https://github.com/JohannesBuchner>`_.

## [0.9.5] - 2019-03-14
### Changed
- Added support for periodic boundary conditions.

- Set up basic tests for continuous integration.

## [0.9.4] - 2019-03-07
### Changed
- Added a logo!

- Updated and reorganized documentation and demos.

- Added proper support for gradients.

- Changed defaults and added several "quality of life" improvements.

## [0.9.3] - 2019-02-10
### Changed
- Updated documentation.

- Modified re-scaling behavior to better deal with inefficient proposals.

- Improved stability of the current ellipsoid decomposition algorithm.

- Added new `'auto'` options and changed a number of defaults to make things
  easier for general users.

- Plotting now defaults to 95% credible intervals instead of 68%.

## [0.9.2] - 2018-03-17
### Changed
- Added in a fast approximation option for `jitter_run` and `simulate_run`.

- Modified the default stopping heuristic. It now evaluates significantly
  faster but is a less accurate probe of the "true" KL divergence.

- Modified `'rwalk'` behavior to better deal with edge cases.

- Changed defaults so performance should now be more stable (albiet slower)
  for the average user.

- Improved the stability of bounding ellipsoids.

- Fixed performance issues with `'rslice'` and `'hslice'`.

- Small plotting improvements.

## [0.9.1] - 2018-03-01
### Changed

- Fixed a minor bootstrapping bug that affected performance for some users.

- Fixed a serious bug associated with the new singular decomposition algorithm
  and changed its behavior so it no longer auto-kills user runs when it fails.

## [0.9.0] - 2018-02-25
### Changed
- `dynesty` is now on PyPI!

## [0.8.4] - 2018-02-24
### Changed
- Added two new slice sampling options (`'rslice'` and `'hslice'`).

- Changed internals to allow user to access quantities during dynamic batch
  allocation. **WARNING: Breaks some aspects of backwards compatibility
  for advanced users utilizing generators.**

- Simplified parallelism options.

- Fixed a singular decomposition bug that occasionally appeared during runtime.

- Small plotting/utility improvements.

## [0.8.3] - 2017-12-13
### Changed
- Fixed additional Python 2/3 compatibility bugs.

- Added the ability to pass user-specified custom print functions.

- Added importance reweighting.

- Small improvements to plotting utilities.

- Small changes to improve user outputs and basic functionality.

## [0.8.2] - 2017-09-15
### Changed
-  Fixed `map` bugs that broke compatibility between Python 2 and 3.

- Fixed a bug where the sampler could break during the first update from the
  unit cube when using a `pool`.

## [0.8.1] - 2017-09-12
### Changed
- Introduced a function wrapper for `prior_transform` and `loglikelihood`
  functions to allow users to pass `args` and `kwargs`.

- Fixed a small bug that could cause bounding ellipsoids to fail.

- Introduced a stability fix to the default
  `~dynesty.dynamicsampler.weight_function` when computing evidence-based
  weights.

## [0.8.0] - 2017-09-08
### Changed
- Initial beta release.
