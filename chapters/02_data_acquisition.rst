Data Acquisition and Covariance Construction
============================================

This chapter describes the data collection and covariance matrix construction
process.

Data collection
===============

LSSTCam corner wavefront sensor Zernike coefficients (Z4--Z26, four corners:
R00/R40/R04/R44) are fetched from the Butler and ConsDB across multiple
stability test nights (October--November 2025).

.. figure:: /_static/01_stability_nights_overview.png
   :alt: Overview of stability test nights
   :width: 100%

   Overview of the stability test nights used for covariance estimation,
   showing the available data blocks across multiple nights.

.. figure:: /_static/02_seeing_aos_metrics.png
   :alt: Seeing and AOS metrics
   :width: 100%

   Day-separated seeing and AOS metrics, color-coded by band, showing the
   observing conditions during the stability tests.

Covariance construction
=======================

Cumulative covariance matrices are built from detrended Zernike time series
(92-element vectors: 23 Zernikes × 4 corners), and convergence is tracked
across stability blocks.

Detrending
----------

Before building the covariance, the Zernike time series are detrended to
remove slow drifts that would inflate the variance estimates.

.. figure:: /_static/03_detrending_comparison.png
   :alt: Detrending comparison
   :width: 100%

   Comparison of Zernike coefficients before and after detrending for a
   representative stability block.

.. figure:: /_static/04_single_coeff_detrending.png
   :alt: Single coefficient detrending
   :width: 100%

   Detailed view of a single Zernike coefficient before and after detrending.

Cumulative covariance tracking
------------------------------

The covariance matrix is built cumulatively as data blocks are added,
allowing convergence to be monitored.

.. figure:: /_static/05_covariance_element_tracking.png
   :alt: Covariance element tracking
   :width: 100%

   Tracking of selected covariance matrix elements as stability blocks are
   added, showing convergence behavior.

.. figure:: /_static/06_covariance_source_selection.png
   :alt: Covariance source selection
   :width: 100%

   Selection of data source (trended vs detrended) for the final covariance
   matrix construction.

.. figure:: /_static/07_cumulative_covariance.png
   :alt: Cumulative covariance
   :width: 100%

   Cumulative covariance matrix after incorporating all stability blocks.

Remapping to the OFC target grid
================================

The measured covariance is remapped from Z4--Z26 to the OFC target range
Z4--Z28 (25 × 4 = 100).  Missing modes (Z27, Z28) and zero-valued modes
(Z20, Z21) are assigned diagonal = 1 and off-diagonal = 0.  Because the OFC
truncation index drops these higher modes, this assignment does not affect
the state estimates.

.. figure:: /_static/08_remapped_covariance_C_new.png
   :alt: Remapped covariance matrix C_new
   :width: 100%

   The remapped covariance matrix ``C_new`` with detector and Zernike block
   structure visible.  The matrix is organized in corner blocks
   (R00, R04, R40, R44) with each block containing Z4--Z28 elements.

Corner-order canonicalization
=============================

The covariance matrix ``C_new`` is permuted from the historical build order
[R00, R40, R04, R44] to the ``ts_ofc`` canonical order [R00, R04, R40, R44].
This ensures compatibility with the ``StateEstimator`` expectations.
