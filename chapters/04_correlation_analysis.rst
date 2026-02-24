Correlation Structure by Observing Conditions
=============================================

This chapter analyzes how the Zernike correlation structure changes with
observing conditions, specifically donut blur and altitude.

Clipped Zernike correlation matrix analysis
===========================================

The measured correlation matrix is recomputed after binning by donut blur:

- ≤1.0 arcsec (best seeing)
- 1.0--1.25 arcsec
- 1.25--1.5 arcsec
- >1.5 arcsec (worst seeing)

Optional altitude windowing can also be applied to isolate elevation-dependent
effects.

.. figure:: /_static/22_clipped_correlation_by_blur.png
   :alt: Clipped correlation by blur bin
   :width: 100%

   Measured correlation matrices for different donut blur bins compared to the
   simulated reference.  Changes in correlation structure with seeing
   conditions are visible.

Pseudo-diagonal corner-to-corner correlations
=============================================

Same-Zernike correlations across different corners (block-diagonals of the
4×4 corner correlation blocks) are extracted and compared across blur bins.
This diagnostic reveals whether decorrelation with worse seeing is
concentrated in specific modes or is roughly uniform.

.. figure:: /_static/23_corner_correlation_vs_zernike.png
   :alt: Corner correlation vs Zernike index
   :width: 100%

   Mode-resolved correlation lines vs Zernike index, with one curve per blur
   bin and a dashed reference curve for the simulation.  This shows whether
   decorrelation at worse seeing is mode-dependent.
