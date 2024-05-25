# Get wildfire data from GEE

## Prepare

安装python、conda、国内需要科学上网。

```
git clone git@github.com:LanXiu0523/get-wildfire-data-from-GEE.git
cd get-wildfire-data-from-GEE/next_day_wildfire_spread

conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/free/
conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/main/
conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud/conda-forge/

conda create -n gee37 python=3.7
conda activate gee37

pip install google-api-python-client pycryptodome earthengine-api 
pip install -r requirements.txt

# 仅国内科学上网需设置，win搜索 代理设置 查看 IP:PORT
set http_proxy=http://127.0.0.1:7890
set https_proxy=http://127.0.0.1:7890
```



## Install Google Cloud SDK：

refer: https://blog.csdn.net/nicolelili1/article/details/68947164
```
gcloud init
gcloud auth application-default login
```



## Init ee 
```
earthengine authenticate
```



## Stage_1 export data

```
# bucket：google diver folder start_date：开始日期 end_date：结束日期

# US 
python -m data_export.export_ee_training_data_main --bucket=gee_us_stage_1 --start_date="2020-01-01" --end_date="2021-01-01"

# CN_SW
python -m data_export.export_ee_training_data_main --bucket=gee_cn_sw_stage_1 --start_date="2014-01-01" --end_date="2023-12-31"
```



## Stage_2 data pre-processing

下载google diver的数据到本地
`$PATH1：D:\STU_new\workspace\gee\get-wildfire-data-from-GEE\data\gee_us_stage_1`
本地新建文件目录用于保存数据输出
`$PATH2：D:\STU_new\workspace\gee\get-wildfire-data-from-GEE\data\gee_us_stage_2`

```
pip install immutabledict

# file_pattern: $PATH1 out_file_prefix: $PATH2

# US
python -m data_export.extract_ongoing_fires_main --file_pattern="D:\STU_new\workspace\gee\get-wildfire-data-from-GEE\data\gee_us_stage_1\*.tfrecord.gz" --out_file_prefix="D:\STU_new\workspace\gee\get-wildfire-data-from-GEE\data\gee_us_stage_2\" 

# CN_SW
python -m data_export.extract_ongoing_fires_main --file_pattern="D:\STU_new\workspace\gee\get-wildfire-data-from-GEE\data\gee_cn_sw_stage_1\*.tfrecord.gz" --out_file_prefix="D:\STU_new\workspace\gee\get-wildfire-data-from-GEE\data\gee_cn_sw_stage_2\" 
```



## More details

refer: https://github.com/google-research/google-research/tree/0ced6d0dc454f50b6900981434ebfcc25e56675a/simulation_research/next_day_wildfire_spread