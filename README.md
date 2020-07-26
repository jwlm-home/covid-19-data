# A reanalysis of the NY Times _Coronavirus in the U.S.: Latest Map and Case Count_ data.
 
The state-level and county-level analyses of Coronavirus mortality and morbidity
in https://www.nytimes.com/interactive/2020/us/coronavirus-us-cases.html
use sliding windows to smooth out the data. This is correct, insofar as it goes,
but suffers from several limitations: 
(1) it's not continuous, 
(2) it's a lagging indicator,
and
(3) it's unstable when used to compute the rate of change of the counts.
It also has an essential property, however: the average of the differences between successive
days is guaranteed to be positive. Any alternate mechanism of estimating
the rate of change of the counts of cases and deaths must have the same property.

This repository reanalyzes the data using a set of low-degree smoothing splines.
The base splines are computed once for deaths per capita and cases per capita,
and the subsequent estimate of the rate of change is then computed as the derivative
of that smoothed spline. The smoothed spline is computed using a penalized GAM
which is constrained to increase monotonically, thus guaranteeing positivity of
the derivatives at all points.

The smoothed spline is computed using the pyGAM package (Servén D., Brummitt C. (2018). pyGAM: 
Generalized Additive Models in Python. Zenodo. DOI: 10.5281/zenodo.1208723) and then reanalyzed
using the Scikit-learn implementation of a univariate smoothing spline (Pedregosa, F., Varoquaux, Ga"el, 
Gramfort, A., Michel, V., Thirion, B., Grisel, O., … others. (2011). Scikit-learn: Machine learning in Python.
Journal of Machine Learning Research, 12(Oct), 2825–2830.)
