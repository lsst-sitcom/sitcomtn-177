Covariance Matrix Construction
==============================

This chapter describes the data collection, processing, and covariance matrix
construction pipeline.

Data Collection
---------------

LSSTCam corner wavefront sensor Zernike coefficients (Z4--Z26, four corners:
R00, R04, R40, R44) are fetched from the Butler and ConsDB across multiple
stability test nights from October--November 2025.

.. figure:: /_static/01_stability_nights_overview.png
   :alt: Overview of stability test nights
   :width: 100%

   Overview of the stability test nights used for covariance estimation,
   showing the available data blocks and sequence ranges.

.. figure:: /_static/02_seeing_aos_metrics.png
   :alt: Seeing and AOS metrics
   :width: 100%

   Day-separated seeing and AOS metrics (donut blur FWHM, DIMM seeing),
   color-coded by band.

Detrending
----------

Before computing the covariance, Zernike time series are detrended to remove
slow drifts (e.g., thermal evolution, tracking errors) that would inflate
variance estimates.  A rolling median filter is applied to each coefficient
independently.

.. figure:: /_static/03_detrending_comparison.png
   :alt: Detrending comparison
   :width: 100%

   Comparison of Zernike coefficients before (trended) and after (detrended)
   for a representative stability block.

Covariance Matrix Construction
------------------------------

The covariance matrix is computed from the detrended Zernike vectors
(92 elements: 23 Zernikes x 4 corners) using ``np.cov``.  Data from all
stability nights are combined to maximize sample size.

.. figure:: /_static/04_cumulative_covariance.png
   :alt: Cumulative covariance tracking
   :width: 100%

   Cumulative covariance matrix built from all stability blocks.

Remapping to the OFC Target Grid
--------------------------------

The measured covariance (Z4--Z26, 23 modes) is remapped to the OFC target
range (Z4--Z28, 25 modes).  Missing modes (Z27, Z28) and zero-valued modes
(Z20, Z21, which are not measured by the wavefront sensors) are assigned
diagonal variance = 1 and off-diagonal covariance = 0.  This ensures the
matrix is invertible without affecting the state estimates for the truncated
modes.

Corner-Order Canonicalization
-----------------------------

The covariance matrix is permuted from the historical data order
[R00, R40, R04, R44] to the ``ts_ofc`` canonical order [R00, R04, R40, R44].
This step is critical for compatibility with the ``StateEstimator``.

.. figure:: /_static/05_covariance_matrix_canonical.png
   :alt: Remapped covariance matrix
   :width: 100%

   The final covariance matrix ``C_new`` in canonical corner order.
   The 4x4 block structure (one block per corner pair) is visible, with
   each block containing 25x25 Zernike covariances.

YAML Export Format
------------------

The covariance matrix is exported to YAML for use by ``ts_ofc`` and
``ts_mtaos``.  The format matches the existing
``default_noise_covariance.yaml`` in ``ts_config_mttcs``.

The YAML header documents:

- Matrix dimensions and Zernike range
- Corner ordering
- Row index mapping (which rows correspond to which corner/Zernike)
- Data provenance (nights, sequence ranges, clipping criteria)
- Number of data points used

Example header::

   ---
    # Measurement noise covariance matrix (environment-clipped).
    # Clipping: donut_blur_fwhm <= 1.25 and 55 < altitude < 65
    # data_points = 142 (number of data points used to build this matrix)
    # Dimensions: (25*4,25*4) = (100,100). From j_min = 4 to j_max = 28.
    # Corner order: ['R00', 'R04', 'R40', 'R44'] (canonical ts_ofc)
    #
    # Each row is a YAML array '- [...]' representing one row of the
    # covariance matrix. The indexing is blocked by corner, with
    # 25 Zernikes (Z4-Z28) per corner:
    #
    #   Row indices  |  Meaning
    #   -------------|-------------------
    #   0-24          |  Z4-Z28 at corner R00
    #   25-49         |  Z4-Z28 at corner R04
    #   50-74         |  Z4-Z28 at corner R40
    #   75-99         |  Z4-Z28 at corner R44
    #
    # Night -> Sequence ranges (after clipping):
    # 20251022 : 206-244
    # 20251023 : 156-194, 242-280
    # ...

Each subsequent line is a YAML array ``- [v0, v1, ..., v99]`` representing
one row of the 100x100 covariance matrix.
