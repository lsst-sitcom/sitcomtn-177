Environment-Clipped Covariance Analysis
=======================================

This chapter investigates whether using covariance matrices built from
subsets of data (clipped by observing conditions) affects the DOF state
estimates.

Clipping the covariance matrix
==============================

Environment-clipped covariance matrices are built by:

- **Blur bins**: ≤1.0, 1.0--1.25, 1.25--1.5, >1.5 arcsec
- **Good-binning aggregate**: all data with donut blur ≤1.25 arcsec
- **Altitude window**: optional elevation clipping

Each clipped covariance is remapped to the Z4--Z28 basis and canonicalized
to ``ts_ofc`` corner order.

.. figure:: /_static/28_clipped_covariance_by_blur.png
   :alt: Clipped covariance matrices by blur bin
   :width: 100%

   Side-by-side comparison of environment-clipped covariance matrices for
   each blur bin variant.  Left column shows the unclipped reference; right
   column shows the clipped variant.  Differences reveal how the covariance
   structure changes with seeing conditions.

DOF state estimation with environment-clipped covariance
========================================================

This section tests whether the estimated DOF states change when using a
clipped covariance (e.g., good-seeing subset) instead of the full unclipped
covariance.

.. figure:: /_static/29_dof_clipped_cov_selected.png
   :alt: DOF states with selected clipped covariances
   :width: 100%

   DOF state comparison using:

   - **Cov=False**: baseline
   - **Cov Matrix unclipped**: all data
   - **Cov Matrix clipped (aggregate)**: good-binning subset (blur ≤1.25 arcsec)

Corner-style overlay plot
=========================

All covariance variants (Cov=False, unclipped, good-binning aggregate,
per-blur-bin) are overlaid with distinct colors and markers to compare
DOF distributions in a single figure.

.. figure:: /_static/30_dof_clipped_cov_all_bins.png
   :alt: DOF states with all clipped covariance variants
   :width: 100%

   Corner-style overlay plot including all per-blur-bin clipped covariance
   variants.  This comprehensive view shows how DOF estimates vary across
   the full range of observing condition subsets.
