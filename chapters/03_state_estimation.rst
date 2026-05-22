State Estimation Comparison
===========================

This chapter compares DOF state estimates under different noise covariance
assumptions.

Overview
--------

The ``StateEstimator.dof_state()`` method in ``ts_ofc`` estimates the
optical system state (hexapod positions, mirror bending modes) from
wavefront Zernike measurements.  The noise covariance matrix determines
how measurements are weighted in the least-squares estimation.

Three covariance configurations are compared:

- **Cov=False**: No noise covariance (uniform weighting, equivalent to
  ordinary least squares)
- **SimCov**: A simulated covariance matrix derived from end-to-end
  wavefront sensing simulations
- **MeasuredCov**: The newly measured covariance from on-sky stability data

Simulated Covariance Matrix
---------------------------

The simulated covariance matrix (SimCov) was generated from wavefront sensing
simulations described in :cite:`2024SPIE13103E..1WX` (Xin et al. 2024).  These
simulations model the complete wavefront sensing pipeline, including
atmospheric turbulence, optical aberrations, and the donut-based wavefront
estimation algorithm.

The simulated covariance is dominated by atmospheric contributions, which
introduce correlated noise across Zernike modes and corner sensors.  This
matrix represents the expected measurement uncertainty under typical observing
conditions as predicted by the simulation framework.

Simulated vs Measured Covariance
--------------------------------

Before comparing state estimates, we examine the covariance matrices
themselves.

.. figure:: /_static/06_sim_vs_meas_covariance.png
   :alt: Simulated vs measured covariance
   :width: 100%

   Side-by-side comparison of simulated and measured covariance matrices.
   The simulated matrix (left) is derived from end-to-end wavefront sensing
   simulations and is dominated by atmospheric effects.  The measured matrix
   (right) captures actual on-sky correlations including atmospheric,
   instrumental, and environmental effects not fully represented in the
   simulation.

DOF State Distributions
-----------------------

State estimates are computed for all stability test exposures under each
covariance configuration.  The per-DOF distributions reveal how the
covariance affects the estimated system state.

.. figure:: /_static/07_dof_distributions_comparison.png
   :alt: DOF state distributions
   :width: 100%

   Per-DOF state distributions comparing Cov=False (baseline), SimCov,
   and MeasuredCov.  The distributions show:

   - **Width changes**: Different effective weighting of Zernike modes
   - **Mean shifts**: Potential biases in the estimation
   - **Tail behavior**: Sensitivity to outliers

Key Findings
------------

1. The measured covariance produces DOF estimates that are generally
   consistent with the simulated covariance, validating the simulation
   assumptions.

2. Some DOFs show modest differences between SimCov and MeasuredCov,
   reflecting real on-sky correlations not captured by the simulation.

3. Using any covariance (SimCov or MeasuredCov) produces more stable
   estimates than Cov=False for higher-order modes, where the uniform
   weighting amplifies noise.
