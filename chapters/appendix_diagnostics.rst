Appendix: Diagnostic Plots
==========================

This appendix contains detailed diagnostic plots that support the main
analysis but are not essential for understanding the core results.

Cross-Corner Correlation Analysis
---------------------------------

Pearson correlations between Zernike measurements at different corner
sensors reveal common-mode vs local behavior.

.. figure:: /_static/A01_cross_corner_correlation.png
   :alt: Cross-corner correlation analysis
   :width: 100%

   Cross-corner correlation coefficients for each Zernike mode.
   High correlations indicate common-mode atmospheric or optical effects;
   low correlations indicate local sensor or field-dependent behavior.

.. figure:: /_static/A02_full_correlation_matrix.png
   :alt: Full correlation matrix
   :width: 100%

   Full correlation matrix across all (corner, Zernike) combinations.
   The block structure reflects corner groupings.

Variance Comparison: Simulated vs Measured
------------------------------------------

Per-corner, per-Zernike diagonal variance comparison between the simulated
covariance (in nm^2) and measured covariance (in um^2, converted to nm^2).

.. figure:: /_static/A03_variance_comparison.png
   :alt: Variance comparison
   :width: 100%

   Diagonal variance comparison.  The simulated covariance was computed at
   770nm wavelength; measured values are in um (OPD length units).

Correlation Structure by Blur Bin
---------------------------------

The measured correlation matrix recomputed for different donut blur bins,
showing how correlation structure changes with seeing conditions.

.. figure:: /_static/A04_correlation_by_blur.png
   :alt: Correlation by blur bin
   :width: 100%

   Correlation matrices for different blur bins compared to the simulated
   reference.  Worse seeing (higher blur) shows increased decorrelation.

Per-Blur-Bin DOF Distributions
------------------------------

Complete set of DOF distributions for all blur-bin clipped covariance
variants (including those that may be non-PSD).

.. figure:: /_static/A05_dof_all_blur_bins.png
   :alt: DOF distributions all blur bins
   :width: 100%

   Corner-style overlay including all per-blur-bin variants.  Some variants
   may be excluded if their covariance matrix is not positive semi-definite.
