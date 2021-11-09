<div align="center">
<img src="kats_logo.svg" width="40%"/>
</div>

<div align="center">
  <a href="https://github.com/facebookresearch/Kats/actions">
  <img alt="Github Actions" src="https://github.com/facebookresearch/Kats/actions/workflows/build_and_test.yml/badge.svg"/>
  </a>
  <a href="https://pypi.python.org/pypi/kats">
  <img alt="PyPI Version" src="https://img.shields.io/pypi/v/kats.svg"/>
  </a>
  <a href="https://github.com/facebookresearch/Kats/blob/master/CONTRIBUTING.md">
  <img alt="PRs Welcome" src="https://img.shields.io/badge/PRs-welcome-brightgreen.svg"/>
  </a>
</div>

## Description

Kats是专门针对时间序列分析的一个轻量级，使用简单，高度可概括的框架.  时间序列分析也是数据科学、数据工程中不可或却的部分，通过统计分析和特征提取，检测数据异常或是构建回归来预测未来的走势.  Kats致力于提供一种时间序列分析的一站式服务：包括检测，预测，特征提取、融合，多元分析等等......

Kats的最终解释权归Facebook公司下的数据科学团队所有.  你可以在[PyPI](https://pypi.python.org/pypi/kats/)上进行下载. 

## Important links

- Homepage: https://facebookresearch.github.io/Kats/
- Kats Python package: https://pypi.org/project/kats/0.1.0/
- Facebook Engineering Blog Post: https://engineering.fb.com/2021/06/21/open-source/kats/
- Source code repository: https://github.com/facebookresearch/kats
- Contributing: https://github.com/facebookresearch/Kats/blob/master/CONTRIBUTING.md
- Tutorials: https://github.com/facebookresearch/Kats/tree/master/tutorials

## Installation in Python

Kats 已经上架 PyPI, 你可以使用 `pip` 进行安装操作.

```bash
# Install pystan with pip before using pip to install prophet
# pystan>=3.0 is currently not supported
pip install pystan==2.19.1.1

pip install --upgrade pip
pip install kats
```

如果你只需要Kats中的小部分功能，你可以安装mini版本的Kats通过：
```bash
MINIMAL=1 pip install kats
```
这将省略许多依赖组建(也就是`test_requirements.txt`中的所有)

然而它也将失去许多功能，并且在`import kats`的时候会引发warnings. 可以通过`setup.py` 查看更多详细说明及操作. 

## Examples

这里我们将提供少数的实例来说明Kats能够提供的部分功能. 

### Forecasting

使用`Prophet`模型来预测`air_passengers`数据集. 

```python
import pandas as pd

from kats.consts import TimeSeriesData
from kats.models.prophet import ProphetModel, ProphetParams

# take `air_passengers` data as an example
air_passengers_df = pd.read_csv(
    "../kats/data/air_passengers.csv",
    header=0,
    names=["time", "passengers"],
)

# convert to TimeSeriesData object
air_passengers_ts = TimeSeriesData(air_passengers_df)

# create a model param instance
params = ProphetParams(seasonality_mode='multiplicative') # additive mode gives worse results

# create a prophet model instance
m = ProphetModel(air_passengers_ts, params)

# fit model simply by calling m.fit()
m.fit()

# make prediction for next 30 month
fcst = m.predict(steps=30, freq="MS")
```

### Detection

使用 `CUSUM`检测算法模拟数据集. 

```python
# import packages
import numpy as np

from kats.consts import TimeSeriesData
from kats.detectors.cusum_detection import CUSUMDetector

# simulate time series with increase
np.random.seed(10)
df_increase = pd.DataFrame(
    {
        'time': pd.date_range('2019-01-01', '2019-03-01'),
        'increase':np.concatenate([np.random.normal(1,0.2,30), np.random.normal(2,0.2,30)]),
    }
)

# convert to TimeSeriesData object
timeseries = TimeSeriesData(df_increase)

# run detector and find change points
change_points = CUSUMDetector(timeseries).detector()
```

### TSFeatures

通过给予的时间序列数据，提取有价值的特征

```python
# Initiate feature extraction class
from kats.tsfeatures.tsfeatures import TsFeatures

# take `air_passengers` data as an example
air_passengers_df = pd.read_csv(
    "../kats/data/air_passengers.csv",
    header=0,
    names=["time", "passengers"],
)

# convert to TimeSeriesData object
air_passengers_ts = TimeSeriesData(air_passengers_df)

# calculate the TsFeatures
features = TsFeatures().transform(air_passengers_ts)
```

## Changelog

### Version 0.1.0

- 初始版本

## Contributors

Kats is a project with several skillful researchers and engineers contributing to it.

Kats is currently maintained by [Xiaodong Jiang](https://www.linkedin.com/in/xdjiang/) with major contributions coming
from many talented individuals in various forms and means. A non-exhaustive but growing list needs to mention: [Sudeep Srivastava](https://www.linkedin.com/in/sudeep-srivastava-2129484/), [Sourav Chatterjee](https://www.linkedin.com/in/souravc83/), [Jeff Handler](https://www.linkedin.com/in/jeffhandl/), [Rohan Bopardikar](https://www.linkedin.com/in/rohan-bopardikar-30a99638), [Dawei Li](https://www.linkedin.com/in/lidawei/), [Yanjun Lin](https://www.linkedin.com/in/yanjun-lin/), [Yang Yu](https://www.linkedin.com/in/yangyu2720/), [Michael Brundage](https://www.linkedin.com/in/michaelb), [Caner Komurlu](https://www.linkedin.com/in/ckomurlu/), [Rakshita Nagalla](https://www.linkedin.com/in/rakshita-nagalla/), [Zhichao Wang](https://www.linkedin.com/in/zhichaowang/), [Hechao Sun](https://www.linkedin.com/in/hechao-sun-83b9ba4b/), [Peng Gao](https://www.linkedin.com/in/peng-gao-9137a24b/), [Wei Cheung](https://www.linkedin.com/in/weizhicheung/), [Jun Gao](https://www.linkedin.com/in/jun-gao-71352b64/), [Qi Wang](https://www.linkedin.com/in/qi-wang-9231a783/), [Morteza Kazemi](https://www.linkedin.com/in/morteza-kazemi-pmp-csm/), [Tihamér Levendovszky](https://www.linkedin.com/in/tiham%C3%A9r-levendovszky-29639b5/), [Jian Zhang](https://www.linkedin.com/in/jian-zhang-73718917/), [Ahmet Koylan](https://www.linkedin.com/in/ahmetburhan/), [Kun Jiang](https://www.linkedin.com/in/kunqiang-jiang-ph-d-0988aa1b/), [Aida Shoydokova](https://www.linkedin.com/in/ashoydok/), [Ploy Temiyasathit](https://www.linkedin.com/in/nutcha-temiyasathit/), Sean Lee, [Nikolay Pavlovich Laptev](http://www.nikolaylaptev.com/), [Peiyi Zhang](https://www.linkedin.com/in/pyzhang/), [Emre Yurtbay](https://www.linkedin.com/in/emre-yurtbay-27516313a/), [Daniel Dequech](https://www.linkedin.com/in/daniel-dequech/), [Rui Yan](https://www.linkedin.com/in/rui-yan/), and [William Luo](https://www.linkedin.com/in/wqcluo/).


## License

Kats is licensed under the [MIT license](LICENSE).

