### DendroCat

#### To Do
 - [X] Port old modules into object-oriented package
    - [X] Detect
    - [X] Reject
    - [X] Match
    - [X] Flux
    - [X] FFPlot
 - [X] Test and debug FFPlot
 - [X] Write new module to plot spectral energy distributions
 - [X] Add upper limits for detections with 2~3 times higher noise than signal
    - [X] Only plot noise level (downward arrow)
    - [X] Plot three arrows for 1, 2, and 3 sigma
 - [X] Preserve original dendrogram _idx, rework to use a different unique identifier for everything else
 - [X] Match sources with external catalogs
 - [X] Add ability to use external catalog fluxes to extend SED plots
 - [X] Support for adding custom sources to any source catalog, by vstacking a source table
    - [X] `add_sources` method for RadioSource and MasterCatalog
 - [X] Combined aperture grid and SED plots
 - [ ] Add two new apertures using the radio beam
    - [ ] Beam centered on peak flux (point-source aperture)
    - [ ] Ellipse with dimensions 2 x beam centered on peak flux (point-source background)
 - [ ] Add function for custom source names
 - [ ] `match_external` takes an argument for source names -- if a match is made, the source name is replaced
 - [ ] Utils functions to grab a specific source by name, idx, etc (indexing astropy tables is clumsy)
 - [ ] Utils function to get a catalog's non-rejected sources (i.e., `dendrocat.utils.nonrejected(catalog)`)
    - Alternate idea: nonrejected catalog is an attribute of the RadioSource or MasterCatalog object (i.e., `rs1.nonrejected`)
 - [ ] Save and load object information with pickle
 - [ ] Implement circular apertures of different radii
    - [ ] Function name changes with radius
 - [ ] Improve rejection algorithm
    - [ ] Reject high eccentricity source ellipses with off-center peak fluxes
    - [ ] Reject large areas where the peak flux isn't much different than the median (indicator of a large noise pocket)
 - [ ] Make `freq_id` a unique identifier
 - [ ] Make default dendrogram parameter generation more robust
     - [ ] Generate default npix parameter based on source size, image scale
 - [X] Generate default annulus parameters based on image scale
 - [ ] Sort / remove RadioSource instance attributes when catalog is sorted or rows are removed
 - [ ] Finish documenting methods
 
#### Bugs
 - Large amount of `NaN` SNR values when autorejecting a catalog made from a dendrogram with low min values
    - [SOLVED] Caused by empty arrays for annulus pixels. Solution is to adjust default annulus width and padding to ensure annulus region isn't infinitely thin.
 - Elliptical flux sums are sometimes lower than the peak flux
    - [SOLVED] The ellipse may contain negative flux values, if it includes a decent portion of the background. In this case, the sum can be less than the peak flux. Temporary solution is to only sum positive values.
 - Ellipses in plot_grid still appear to be smaller than those in DS9
    - [SOLVED] Made same correction as before -- multiply all astropy ellipse region dimensions by 2
 - Some sources disappear when matched
    - [SOLVED] Matching algorithm was matching accepted sources with rejected sources, overwriting the accepted sources so they don't appear in the master catalog. Fixed by only matching accepted sources to each other.
 - Bright sources with contaminated annululi are mistaken for upper limit detections due to their low SN
 - `MasterCatalog.match_external` only adds one frequency's worth of photometry data
    - [SOLVED] I don't remember exactly how I fixed it, but I found the bug and fixed it. I promise.
