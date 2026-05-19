Introduction
============

This technote is part of the AOS Closed-Loop Optimization effort.
The parent task (RSO-99, *Covariance Estimation*) aims to use a measured
noise covariance matrix inside the Optical Feedback Control (OFC) system
to properly weight wavefront measurements during closed-loop operations.

The OFC system in `ts_ofc <https://github.com/lsst-ts/ts_ofc>`_ uses a
``StateEstimator`` to infer degrees of freedom (DOFs) -- hexapod positions
and mirror bending modes -- from wavefront Zernike coefficients measured by
the four corner wavefront sensors (R00, R04, R40, R44).  By default, the
estimator assumes uniform measurement noise.  Providing an explicit noise
covariance matrix allows the estimator to weight each Zernike mode and each
corner sensor according to its actual measurement uncertainty, improving the
accuracy and stability of closed-loop corrections.

This technote documents:

1. The construction of a noise covariance matrix from on-sky stability test data
2. Comparison of DOF state estimates using measured vs simulated covariance
3. The effect of environment-clipped covariances (filtered by seeing conditions)
4. The YAML export format for integration with ``ts_ofc`` / ``ts_mtaos``

The detailed computations are in the companion notebook
``notebooks/covariance_estimation_analysis.ipynb``.
