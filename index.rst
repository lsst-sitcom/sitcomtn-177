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

.. include:: chapters/01_introduction.rst

.. include:: chapters/02_data_acquisition.rst

.. include:: chapters/03_state_estimation.rst

.. include:: chapters/04_correlation_analysis.rst

.. include:: chapters/05_sensitivity_diagnostics.rst

.. include:: chapters/06_clipped_covariance.rst

Known issues and caveats
========================

The following items were identified during review and are documented here for
future reference.

Corner order in the YAML export
-------------------------------

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

**Pending:** Either move the YAML export after the canonicalization
cell, or explicitly permute before writing and document the corner order in
the YAML header.

Hardcoded absolute paths
------------------------

Several notebook cells reference absolute paths such as
``/home/dsanmartim/notebooks/repos/lsst-ts/ts_config_mttcs/MTAOS/v8/ofc/``
for the OFC configuration directory and the YAML output path.  These will
break for other users.  A future refactoring will move these into
configurable variables at the top of the notebook or into the
``lsst.sitcom.tn177`` Python package.

State vector sign convention
----------------------------

The estimated DOF states are stored with a sign negation
(``-state_cov``, ``-state_nocov``).  This follows the OFC convention where
the state estimator returns the correction to be *subtracted*.  The
negation converts it to the physical state offset, which is what the
comparison plots show.

Code duplication and magic numbers
----------------------------------

The helper function ``parse_dof_str`` is defined twice (in the helper section
and again in the three-pass estimation cell).  The expected vector length
``92`` (4 corners × 23 Zernikes) is hardcoded in ``process_day``.  These will
be addressed when the notebook is refactored and utility functions are moved
into the ``lsst.sitcom.tn177`` Python module.
