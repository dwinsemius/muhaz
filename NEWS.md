# muhaz 1.2.6.6

## Bug fixes

* Added a guard in `muhaz()` raising an informative error when the
  number of observations exceeds 20000, the implicit limit imposed
  by fixed-size arrays in the underlying Fortran code. Previously
  this would silently overrun the array rather than fail cleanly.
 
* Fixed: tied event and censoring times producing hazard estimates
  that depended on the input row order of the tied observations.
  `muhaz()` sorts observations by time before estimation; when two
  observations shared an identical time, R's stable sort preserved
  whatever order they happened to arrive in, and the underlying
  Fortran risk-set calculation (`n - i + 1`, where `i` is sort rank)
  is sensitive to that order. Ties are now broken by placing events
  before censored observations at the same time, matching the
  convention used elsewhere in survival analysis (e.g.
  `survival::Surv`) that a censored subject is still at risk when an
  event occurs at the same recorded time. This may produce slightly
  different hazard estimates at tied times compared to previous
  versions; estimates away from ties are unaffected. Originally
  reported in 2021 by Justin Clark at UMN. 

# muhaz 1.2.6.5

## Bug fixes

* Fixed a crash in `muhaz()` when a group has zero events. The default
  pilot bandwidth calculation (`bw.pilot <- endz/8/(nz^.2)`) divided by
  zero when `sum(delta) == 0`, producing `Inf` and crashing the
  downstream Fortran routine. `nz` is now clamped to a minimum of 1,
  which yields a finite bandwidth and a correctly flat zero hazard
  estimate when no events are present. Reported and fixed by
  Ken Beath <ken@kjbeath.id.au>.

## Documentation

* Fixed `man/muhaz.object.Rd`: replaced `\arguments` with `\value` /
  `\describe`, resolving an R CMD check NOTE ("Rd files without
  \usage... \arguments should not be documented without \usage").

## Package maintenance

* muhaz is now maintained by David Winsemius
  <dwinsemius@comcast.net>, following its orphaning by the previous
  maintainer.
* Added GitHub Actions / R-hub v2 CI configuration for cross-platform
  checks.

# muhaz 1.2.6.4

* Version on CRAN prior to this maintainer transition.
