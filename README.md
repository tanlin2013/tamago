# anko
Toolkit for performing anomaly detection algorithm on 1D time series based on numpy, scipy.

Conventional approaches that based on statistical analysis have been implemented, with mainly two approaches included:
1. Normal Distribution  
Data samples are presumably been generated by normal distribution, and therefore anomalous data points can be targeted by analysing the standard deviation.  

2. Fitting Ansatz  
Data samples are fitted by several ansatzs, and in accordance with the residual, anomalous data points can be selected. 

Regarding model selections, models are adopted dynamically by performing normal test and by computing the (Akaike/Bayesian) information criterion.
By default, the algorithm will first try to fit in the data into normal distribution, if it passed [normal test](https://docs.scipy.org/doc/scipy/reference/generated/scipy.stats.normaltest.html).
If this attempt suffers from the loss of convergence or it did not pass [normal test](https://docs.scipy.org/doc/scipy/reference/generated/scipy.stats.normaltest.html) from begining, 
then the algorithm will pass data into the second methods and try to execute all the available fitting ansatzs simultaneously. 
The best fitting ansatz will be selected by information criterion, and finally the algorithm will pick up anomalous points in accordance with the residual.
[click here to see all available methods.](classanko_1_1anomaly__detector_1_1_anomaly_detector.html#a019359334795d2f05a730ab00085e5e9)   

Future development will also include methods that are based on deep learning techniques, such as isolation forest, support vector machine, etc.

## Requirements
* python >= 3.6.0
* numpy >= 1.16.4
* scipy >= 1.2.1

## Installation 
```
pip install anko
```
[anko on PyPI](https://pypi.org/project/anko/)

## Documentation
[anko documentation](https://tanlin2013.github.io/anko/html/index.html)

## Jupyter Notebook Tutorial (in dev)
[Host on mybinder](https://mybinder.org/v2/gh/tanlin2013/anko/master?filepath=anko_tutorial.ipynb)

## Basic Usage
1. Call AnomalyDetector
```
from anko.anomaly_detector import AnomalyDetector  
agent = AnomalyDetector(t, series)
```

2. Define policies and threshold values (optional)
```
agent.thres_params["linregress_res"] = 1.5  
agent.apply_policies["z_normalization"] = True  
agent.apply_policies["info_criterion"] = 'AIC'
```
for the use of [**AnomalyDetector.thres_params**](classanko_1_1anomaly__detector_1_1_anomaly_detector.html#af31bfbbef59010642ad8a33785504c59) 
and [**AnomalyDetector.apply_policies**](classanko_1_1anomaly__detector_1_1_anomaly_detector.html#af58531e67a2d33977a1bdc142b3f0561), 
please refer to the documentation.

3. Run check
```
check_result = agent.check()
```

The type of output **check_result** is [**CheckResult**](classanko_1_1anomaly__detector_1_1_check_result.html), which is basically a dictionary.
> model: 'increase_step_func'  
> popt: [220.3243250055105, 249.03846355234577, 74.00000107457113]  
> perr: [0.4247789247961187, 0.7166253174634686, 0.0]  
> anomalous_data: [(59, 209)]  
> residual: [10.050378152592119]  
> extra_info: ['Info: AnomalyDetector is using z normalization.', 'Info: There are more than 1 discontinuous points detected.']        

## Run Test (in dev)
```
python -m unittest discover -s test -p '*_test.py'
```
or simply
```
make test
```