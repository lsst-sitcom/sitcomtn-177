Environment-Clipped Covariance Analysis
=======================================

This chapter investigates how covariance matrices built from subsets of data
(clipped by observing conditions) affect DOF state estimates.

Motivation
----------

The measurement noise covariance depends on observing conditions.  During
poor seeing, wavefront measurements have higher variance and different
correlation structure than during good seeing.  Building a covariance matrix
from a specific subset of conditions may better represent the expected noise
during similar future observations.

Clipping Criteria
-----------------

Environment-clipped covariance matrices are built using two filters:

1. **Donut blur FWHM**: Bins at <= 1.0, 1.0--1.25, 1.25--1.5, and > 1.5 arcsec
2. **Altitude window**: Optional elevation range (e.g., 55--65 degrees)

An "aggregate" clipped covariance is also computed using all data with
donut blur <= 1.25 arcsec (good-to-moderate seeing).

.. figure:: /_static/08_clipped_covariance_comparison.png
   :alt: Clipped covariance matrices by blur bin
   :width: 100%

   Side-by-side comparison of covariance matrices for each blur bin.
   Left column: unclipped reference.  Right column: clipped variant.
   The covariance structure changes with seeing conditions -- better seeing
   (lower blur) produces lower overall variance and tighter correlations.

DOF State Comparison
--------------------

State estimates are recomputed using each clipped covariance variant and
compared to the baseline (Cov=False) and unclipped covariance.

.. figure:: /_static/09_dof_clipped_overlay.png
   :alt: DOF states with clipped covariances
   :width: 100%

   Corner-style overlay plot comparing DOF state distributions across
   covariance variants:

   - Cov=False (baseline)
   - Unclipped (all data)
   - Aggregate clipped (blur <= 1.25 arcsec)

Positive Semi-Definiteness Constraint
-------------------------------------

A valid noise covariance matrix must be positive semi-definite (PSD) --
all eigenvalues must be non-negative.  When the number of data points
used to build the covariance is too small relative to the matrix dimension
(100x100 = 10,000 elements), the resulting matrix may fail to be PSD due
to sampling noise.

This occurs in practice for the stricter blur bins (e.g., blur <= 1.0 arcsec)
combined with altitude windowing, where only a few dozen data points may
survive the filters.  The notebook reports PSD status for each clipped
variant and skips non-PSD matrices in state estimation comparisons.

.. note::

   For production use, the recommended covariance is the **aggregate clipped**
   variant (blur <= 1.25 arcsec with altitude windowing), which balances
   data quality filtering with sufficient sample size for a stable PSD matrix.

Conclusions
-----------

1. **Environment clipping is effective**: Covariance matrices built from
   good-seeing subsets have lower diagonal variance and cleaner off-diagonal
   structure.

2. **Sample size matters**: Aggressive clipping (blur <= 1.0 arcsec) can
   produce non-PSD matrices when combined with altitude windowing.  The
   aggregate clipped variant (blur <= 1.25 arcsec) provides a good balance.

3. **DOF estimates are robust**: The estimated DOF states are relatively
   insensitive to the choice of clipped vs unclipped covariance, suggesting
   the measurement correlations are well-characterized across conditions.

4. **Recommendation**: Use the aggregate clipped covariance
   (``onsky_stability_covariance_clipped.yaml``) for closed-loop operations.
   It represents typical good-seeing conditions and has been validated as PSD.
