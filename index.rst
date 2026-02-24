##############################################
Covariance Estimation for AOS Closed-Loop OFC
##############################################

.. abstract::

   This technote documents the estimation and validation of a new covariance
   matrix for use in the Optical Feedback Control (OFC) system of the Vera C.
   Rubin Observatory Active Optics System (AOS). The covariance matrix
   characterizes the uncertainty of wavefront measurements and is used by the
   OFC to weight corrections during closed-loop operations. The analysis
   explores multiple stability conditions and evaluates the impact of the
   updated covariance on OFC performance.

   The companion Python package (``lsst.sitcom.tn177``) provides reusable
   utilities for covariance computation and OFC integration, while the
   notebooks in the ``notebooks/`` directory contain the detailed analysis.

Introduction
============

This technote is part of the AOS Closed-Loop Optimization effort.
The parent task (RSO-99, *Covariance Estimation*) aims to use the covariance
inside OFC to understand the uncertainty of the measurement.

The Optical Feedback Control (OFC) system in
`ts_ofc <https://github.com/lsst-ts/ts_ofc>`_ uses a ``StateEstimator`` to
infer degrees of freedom (DOFs) — hexapod positions and mirror bending
modes — from wavefront Zernike coefficients measured by the four corner
wavefront sensors (R00, R04, R40, R44).  By default, the estimator assumes
uniform measurement noise.  Providing an explicit noise covariance matrix
allows the estimator to weight each Zernike mode and each corner sensor
according to its actual measurement uncertainty, which can improve the
accuracy and stability of the closed-loop corrections.

The analysis presented here builds such a covariance matrix from on-sky
stability test data and evaluates its impact on the estimated DOF states.
The detailed computations are in the companion notebook
``notebooks/covariance_multiple_stabilities_extended_with_ofc_clean.ipynb``;
this document summarizes the methodology and findings.

Analysis overview
-----------------

The notebook proceeds through seven main stages:

1. **Data collection** — LSSTCam corner wavefront sensor Zernike coefficients
   (Z4--Z26, four corners: R00/R40/R04/R44) are fetched from the Butler and
   ConsDB across multiple stability test nights (October--November 2025).

2. **Covariance construction** — Cumulative covariance matrices are built from
   detrended Zernike time series (92-element vectors: 23 Zernikes × 4 corners),
   and convergence is tracked across stability blocks.

3. **Remapping to the OFC target grid** — The measured covariance is remapped
   from Z4--Z26 to the OFC target range Z4--Z28 (25 × 4 = 100).  Missing modes
   (Z27, Z28) and zero-valued modes (Z20, Z21) are assigned diagonal = 1 and
   off-diagonal = 0.  Because the OFC truncation index drops these higher modes,
   this assignment does not affect the state estimates.

4. **Corner-order canonicalization** — The covariance matrix ``C_new`` is
   permuted from the historical build order [R00, R40, R04, R44] to the
   ``ts_ofc`` canonical order [R00, R04, R40, R44].

5. **State estimation comparison** — ``StateEstimator.dof_state()`` is run
   under three noise-covariance regimes (no covariance, simulated ``covM``,
   and measured ``C_new``) and the resulting DOF distributions are compared
   via histograms and corner plots.

6. **Cross-corner Zernike analysis** — Pearson correlations and RMS differences
   per Zernike across corner pairs are computed to assess common-mode versus
   local behavior.

7. **Variance comparison** — Per-corner, per-Zernike diagonal variance is
   compared between the simulated covariance (nm²) and the measured
   covariance (µm² converted to nm²).

Known issues and caveats
------------------------

The following items were identified during review and are documented here for
future reference.

Corner order in the YAML export
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

The YAML export cell in the notebook runs *before* the corner-order
canonicalization step.  At that point, ``C_new`` is still in the historical
build order [R00, R40, R04, R44], not the ``ts_ofc`` canonical order
[R00, R04, R40, R44].  The YAML header documents the matrix dimensions and
Zernike range but does not record the corner ordering.

This does not affect the notebook's own results, because all subsequent
``StateEstimator`` calls use the in-memory ``C_new`` that is properly
canonicalized later.  However, any downstream consumer of the exported YAML
(e.g., ``ts_ofc`` or ``ts_mtaos``) that assumes the canonical corner order
would see the R04 and R40 blocks swapped.

**Recommendation:** Either move the YAML export after the canonicalization
cell, or explicitly permute before writing and document the corner order in
the YAML header.

Hardcoded absolute paths
^^^^^^^^^^^^^^^^^^^^^^^^^

Several notebook cells reference absolute paths such as
``/home/dsanmartim/notebooks/repos/lsst-ts/ts_config_mttcs/MTAOS/v8/ofc/``
for the OFC configuration directory and the YAML output path.  These will
break for other users.  A future refactoring will move these into
configurable variables at the top of the notebook or into the
``lsst.sitcom.tn177`` Python package.

State vector sign convention
^^^^^^^^^^^^^^^^^^^^^^^^^^^^

The estimated DOF states are stored with a sign negation
(``-state_cov``, ``-state_nocov``).  This follows the OFC convention where
the state estimator returns the correction to be *subtracted*.  The
negation converts it to the physical state offset, which is what the
comparison plots show.

Code duplication and magic numbers
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

The helper function ``parse_dof_str`` is defined twice (in the helper section
and again in the three-pass estimation cell).  The expected vector length
``92`` (4 corners × 23 Zernikes) is hardcoded in ``process_day``.  These will
be addressed when the notebook is refactored and utility functions are moved
into the ``lsst.sitcom.tn177`` Python module.
