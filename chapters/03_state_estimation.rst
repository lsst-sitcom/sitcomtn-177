Initial State Estimation Comparison
===================================

This chapter presents the initial comparison of DOF state estimates under
different noise-covariance assumptions.

State estimation comparison
===========================

``StateEstimator.dof_state()`` is run under three noise-covariance regimes:

- **Cov=False**: No noise covariance (uniform weighting)
- **Simulated covM**: The pre-existing simulated covariance matrix
- **Measured C_new**: The newly measured covariance from on-sky data

The resulting DOF distributions are compared via histograms.

.. figure:: /_static/09_state_distributions_cov_vs_nocov.png
   :alt: State distributions with and without covariance
   :width: 100%

   Per-DOF state distributions comparing the three noise-covariance regimes.
   Differences in distribution width indicate changes in effective weighting;
   shifts in mean indicate potential bias.

Cross-corner Zernike analysis
=============================

Pearson correlations and RMS differences per Zernike across corner pairs are
computed to assess common-mode versus local behavior.

.. figure:: /_static/10_cross_corner_correlation.png
   :alt: Cross-corner correlation analysis
   :width: 100%

   Cross-corner correlation analysis showing Pearson correlation coefficients
   between Zernike measurements at different corner sensors.

.. figure:: /_static/11_rms_difference_heatmap.png
   :alt: RMS difference heatmap
   :width: 100%

   Heatmap of RMS differences per Zernike mode and corner pair.

.. figure:: /_static/12_full_correlation_matrix.png
   :alt: Full correlation matrix
   :width: 100%

   Full correlation matrix across all selected (corner, Zernike) combinations.

.. figure:: /_static/13_scatter_preview_corners.png
   :alt: Scatter preview across corners
   :width: 100%

   Scatter plots showing Zernike coefficient relationships across corner
   sensors as a sanity check.

Variance comparison
===================

Per-corner, per-Zernike diagonal variance is compared between the simulated
covariance (nm²) and the measured covariance (µm² converted to nm²).

.. note::

   The simulated covariance was computed at 770nm.  The measured ConsDB
   Zernike coefficients are in µm (OPD length units), which are not tied
   to a specific wavelength.  Converting µm → nm (×1000) allows direct
   variance comparison in nm².

.. figure:: /_static/14_correlation_subblock_2x2.png
   :alt: Correlation matrix 2x2 subblock
   :width: 100%

   A 2×2 subblock of the correlation matrix showing inter-corner structure.

.. figure:: /_static/15_diagonal_squared_plot.png
   :alt: Diagonal squared plot
   :width: 100%

   Squared diagonal elements of the correlation matrix.

.. figure:: /_static/16_correlation_matrix_subset.png
   :alt: Correlation matrix subset
   :width: 100%

   Subset of the correlation matrix highlighting specific mode relationships.

.. figure:: /_static/17_sim_vs_meas_correlation.png
   :alt: Simulated vs measured correlation
   :width: 100%

   Side-by-side comparison of simulated and measured Zernike correlation
   matrices.

.. figure:: /_static/18_sim_vs_meas_covariance.png
   :alt: Simulated vs measured covariance
   :width: 100%

   Side-by-side comparison of simulated and measured covariance matrices.

.. figure:: /_static/19_dof_distributions_three_methods.png
   :alt: DOF distributions for three methods
   :width: 100%

   Per-DOF state distributions comparing MeasuredCov, SimCov, and Cov=False.

.. figure:: /_static/20_variance_comparison_sim_vs_meas.png
   :alt: Variance comparison simulated vs measured
   :width: 100%

   Per-corner, per-Zernike variance comparison between simulated (nm²) and
   measured covariances.

.. figure:: /_static/21_variance_residuals_vs_R00.png
   :alt: Variance residuals vs R00
   :width: 100%

   Variance residuals relative to R00 for both measured and simulated
   covariances.
