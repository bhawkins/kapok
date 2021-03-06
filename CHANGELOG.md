# Change Log


## Unreleased
### Added
- Added kapok.rvog.rvogblselect(), a function which performs baseline selection using a number of different possible baseline ranking criteria.  This allows incoherent multi-baseline forest height inversion.  kapok.Scene.inv() will perform this multi-baseline forest height estimation if the "bl" keyword argument is set to either 'all' (to invert all baselines), or to a list of selected baseline indices.  See help(kapok.Scene.inv) for more details, and help(kapok.rvog.rvogblselect) for details on the baseline ranking criteria that are currently available.  Also added "minkz" keyword to kapok.Scene.inv() which sets the minimum kz value.  For multi-baseline inversions, baselines with kz below this value will be excluded.  For single-baseline inversions, the inversion will still be performed if kz is below this value, but a warning will be printed.
- Downloaded .kz files can now be imported when using the kapok.uavsar.load() function.  The default behaviour is to import the files if they are found in the UAVSAR stack directory.  If they are not found, the software will revert to the previous behavior of calculating the kz values from the .baseline and .lkv files.  The kz importing options can be changed using the 'kzcalc' and 'kzvertical' keywords in kapok.uavsar.load.  See help(kapok.uavsar.load) for more details.
- Added setup.py file.  Kapok can now be more easily installed by running: 'pip install .' in the main Kapok folder.  If you plan to make changes to the code, you should install the library in development mode using the -e flag: 'pip install -e .'  See [here](https://packaging.python.org/installing/#installing-from-a-local-src-tree) for more details.
- Added correction of estimated forest heights for terrain slope angle.  This functionality is accessed through a 'rngslope' keyword argument which has been added to kapok.Scene.inv().  To perform the correction, set kapok.Scene.inv(rngslope=True) to use the built-in DEM to calculate the slope angles, or set 'rngslope' to an array of pre-calculated slope angles.  See help(kapok.Scene.inv) for more details.
- Added kapok.Scene.subset() function which allows Kapok Scene files to be subset (by track, or by azimuth and range indices).
- Added the keyword argument 'saveall' to kapok.Scene.opt().  This keyword can be set to True to save additional parameters of interest besides the optimized coherences, specifically, the minor axis coherences (coherences with minimum separation in the complex plane), and the polarimetric weighting vectors for the optimized coherences.
- Added "nodata" and "tr" keyword arguments to kapok.Scene.geo() method and kapok.geo.radar2ll() function.  These keywords allow the user to set the nodata and output spatial resolution for geocoded output products.  These arguments are passed directly to the call to gdalwarp.
- Added library functions kapok.lib.gaussiansmooth() and kapok.lib.nangaussiansmooth() which perform smoothing using a Gaussian filter.  nangaussiansmooth() ignores NaN values in the input, while gaussiansmooth() does not.

### Changed
- The Kapok Scene file now stores the kz values slightly differently, in order to match the format of .kz files downloaded from the UAVSAR website.  If you load a Kapok Scene file created with the previous version of the software, you will be prompted to convert your file to the new format.  Before doing this conversion, you should make a backup of the file.  The old file format stored kz values for each baseline index.  The new format stores kz values between each flight track and the UAVSAR reference track (the track specified by the downloaded .lkv file).
- The interface for retrieving kz values from the Kapok Scene has changed.  kapok.Scene.kz used to be a link to an HDF5 dataset containing the kz values.  It is now a method which returns the values.  Instead of using 'kz = scene.kz[3]' to get the kz values for baseline index 3, you would now use 'kz = scene.kz(3)'.  See help(kapok.Scene.kz) for more details, and help(kapok.lib.mb) for information on how baselines are indexed in Kapok.

### Fixed
- Fixed bug in handling of matrix regularization (reg keyword argument) for function kapok.cohopt.pdopt().


## v0.1.0 - 2015-11-14
### Added
- Initial software version.  Contains modules for UAVSAR data import, coherence optimization, data visualization, coherence region plotting, ground topography estimation using standard line fit procedure, RVoG forest model inversion, sinc function forest model inversion, and geocoding of output products.  See user's manual and install guide in /docs folder.