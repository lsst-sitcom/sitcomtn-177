.. image:: https://img.shields.io/badge/sitcomtn--177-lsst.io-brightgreen.svg
   :target: https://sitcomtn-177.lsst.io
.. image:: https://github.com/lsst-sitcom/sitcomtn-177/workflows/CI/badge.svg
   :target: https://github.com/lsst-sitcom/sitcomtn-177/actions/

##############################################
Covariance Estimation for AOS Closed-Loop OFC
##############################################

SITCOMTN-177
============

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

For further context, see `ts_ofc <https://github.com/lsst-ts/ts_ofc>`_ and
`ts_mtaos <https://github.com/lsst-ts/ts_mtaos>`_.

**Links:**

- Publication URL: https://sitcomtn-177.lsst.io
- Alternative editions: https://sitcomtn-177.lsst.io/v
- GitHub repository: https://github.com/lsst-sitcom/sitcomtn-177
- Build system: https://github.com/lsst-sitcom/sitcomtn-177/actions/


Build this technical note
=========================

You can clone this repository and build the technote locally if your system has Python 3.11 or later:

.. code-block:: bash

   git clone https://github.com/lsst-sitcom/sitcomtn-177
   cd sitcomtn-177
   make init
   make html

Repeat the ``make html`` command to rebuild the technote after making changes.
If you need to delete any intermediate files for a clean build, run ``make clean``.

The built technote is located at ``_build/html/index.html``.

Publishing changes to the web
=============================

This technote is published to https://sitcomtn-177.lsst.io whenever you push changes to the ``main`` branch on GitHub.
When you push changes to a another branch, a preview of the technote is published to https://sitcomtn-177.lsst.io/v.

Editing this technical note
===========================

The main content of this technote is in ``index.rst`` (a reStructuredText file).
Metadata and configuration is in the ``technote.toml`` file.
For guidance on creating content and information about specifying metadata and configuration, see the Documenteer documentation: https://documenteer.lsst.io/technotes.

Installing as a Python package
==============================

The notebooks in the ``./notebooks`` folder use the code in the ``./python`` folder as a package. You will need to install this package using EUPS. Here are the steps:

.. code-block:: bash

   # Declare the package
   eups declare lsst_sitcom_tn177 v1 -r $PATH_TO_THIS_REPO/sitcomtn-177

   # Install it
   setup lsst_sitcom_tn177

   # Test it
   python -c "import lsst.sitcom.tn177; print(lsst.sitcom.tn177.__version__)"

The last command line should print the package version. It is OK if it prints ``?``, since versioning might not be fully implemented yet.

If you are running the notebooks in Nublado, you will have to update the ``$HOME/notebooks/.user_setups`` file with the same lines above.
