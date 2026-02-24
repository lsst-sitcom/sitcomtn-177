Corner-Order Sensitivity and DOF Diagnostics
============================================

This chapter tests the sensitivity of state estimates to corner-order
assumptions and provides detailed DOF diagnostic plots.

SimCov corner-order sensitivity
===============================

The simulated covariance matrix ``covM`` may have been stored with a different
corner block ordering than what ``ts_ofc`` expects.  This section tests
whether using the wrong corner packing assumption (historical vs canonical)
affects state estimates.

.. figure:: /_static/24_dof_distributions_packing.png
   :alt: DOF distributions under different packing assumptions
   :width: 100%

   Per-DOF state distributions comparing:

   - **MeasuredCov (C_new)**: canonical corner order
   - **SimCov (canonical)**: assuming ``covM`` is in canonical order
   - **SimCov (historical)**: assuming ``covM`` is in historical order
   - **Cov=False**: baseline with no covariance

   If curves overlap, the DOF estimate is insensitive to the packing
   assumption.

DOFs corner plots
=================

Grid of scatter plots comparing per-DOF state estimates across the four
regimes.  These "corner plots" show joint distributions and help identify
correlations or biases between methods.

.. figure:: /_static/25_corner_plot_overlay.png
   :alt: Corner plot overlay
   :width: 100%

   Corner-style overlay plot with all covariance variants shown together.
   Each panel shows DOF_i vs DOF_j, with different colors/markers for each
   method.

.. figure:: /_static/26_corner_plot_uniform_diagonal.png
   :alt: Corner plot with uniform diagonal
   :width: 100%

   Corner plot with uniform diagonal scatter, showing self-consistency of
   each method along the diagonal panels.

Baseline-vs-method triangle plot
================================

A triangular diagnostic grid with Cov=False on the x-axis and each method
on the y-axis, used to identify cross-talk or mixing between DOFs relative
to the baseline.

.. figure:: /_static/27_baseline_vs_method_triangle.png
   :alt: Baseline vs method triangle plot
   :width: 100%

   Baseline-vs-method triangular plot.  Diagonal panels compare the same DOF
   (baseline vs method) with a 1:1 reference line.  Off-diagonal panels show
   whether baseline DOF_j variations are absorbed into method DOF_i
   (cross-talk).
