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
