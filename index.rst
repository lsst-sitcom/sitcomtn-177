##############################################
Covariance Estimation for AOS Closed-Loop OFC
##############################################

.. abstract::

   This technote documents the estimation and validation of a noise covariance
   matrix for use in the Optical Feedback Control (OFC) system of the Vera C.
   Rubin Observatory Active Optics System (AOS).  The covariance matrix
   characterizes the uncertainty of wavefront measurements and is used by the
   OFC ``StateEstimator`` to weight corrections during closed-loop operations.

   Using on-sky stability test data from October--November 2025, we construct
   both a full covariance matrix and an environment-clipped variant filtered
   by seeing conditions.  The matrices are validated against simulated
   covariances and exported in YAML format for integration with ``ts_ofc``
   and ``ts_mtaos``.

   The companion notebook ``notebooks/covariance_estimation_analysis.ipynb``
   contains the detailed analysis and can be used to regenerate the results.

.. include:: chapters/01_introduction.rst

.. include:: chapters/02_covariance_construction.rst

.. include:: chapters/03_state_estimation.rst

.. include:: chapters/04_environment_clipped_covariance.rst

.. include:: chapters/05_conclusions.rst

.. include:: chapters/appendix_diagnostics.rst

Related Documentation
=====================

- `SOTN-001: OFC Control Loop Mathematical Description <https://sotn-001.lsst.io>`_
- `ts_ofc documentation <https://ts-ofc.lsst.io>`_
- `ts_config_mttcs repository <https://github.com/lsst-ts/ts_config_mttcs>`_
