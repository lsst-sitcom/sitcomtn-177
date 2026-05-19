Conclusions and Deliverables
============================

Summary
-------

This technote documents the estimation and validation of a noise covariance
matrix for the OFC ``StateEstimator`` from on-sky stability test data.

Key findings:

1. **Measured covariance is consistent with simulation**: The on-sky measured
   covariance matrix shows similar structure to the pre-existing simulated
   covariance, validating the simulation assumptions.

2. **Environment clipping improves covariance quality**: Filtering data by
   donut blur FWHM (good-seeing conditions) produces cleaner covariance
   estimates with lower variance.

3. **Sample size constraints**: Aggressive environment clipping can produce
   non-positive-definite matrices when the number of data points is
   insufficient.  The recommended aggregate clipped variant (blur <= 1.25
   arcsec, altitude 55--65 degrees) balances quality filtering with
   statistical stability.

4. **DOF estimates are robust**: State estimation results are relatively
   insensitive to the choice between simulated, measured, or clipped
   covariance variants, indicating the system is well-characterized.

Deliverables
------------

Two covariance matrices are provided in YAML format for integration with
``ts_ofc`` and ``ts_mtaos``:

1. **onsky_stability_covariance.yaml**: Full covariance built from all
   stability test data (no environment clipping).

2. **onsky_stability_covariance_clipped.yaml**: Aggregate clipped covariance
   built from good-seeing data (donut blur <= 1.25 arcsec, altitude 55--65
   degrees).  This is the recommended matrix for production use.

Both files are located in the notebook output directory
(``covariance_state_estimation/``) and follow the format expected by
``ts_config_mttcs/MTAOS/ofc/noise_covariance/``.

Future Work
-----------

- **Operational validation**: Deploy the measured covariance in closed-loop
  operations and compare on-sky performance to the simulated covariance.

- **Condition-dependent covariance**: Investigate using different covariance
  matrices based on real-time seeing conditions.

- **Extended Zernike range**: When higher-order Zernike measurements (Z27+)
  become available, update the covariance estimation to include them.
