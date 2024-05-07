## init
```
git clone git@github.com:LanXiu0523/get-wildfire-data-from-GEE.git
cd get-wildfire-data-from-GEE/next_day_wildfire_spread

// 可选：创建虚拟环境
//python -m venv env
//source env/bin/activate

pip install -r requirements.txt
```

## 安装Google Cloud SDK：
https://blog.csdn.net/nicolelili1/article/details/68947164

## 设置项目ID
```
gcloud config set project my-project-to-drive-demo-1
pip install earthengine-api
earthengine authenticate
gcloud auth application-default login
```

## 导出数据
```
python3 -m simulation_research.next_day_wildfire_spread.data_export.export_ee_training_data_main \
--bucket=gs://next_day_wildfire_nuo_0 \
--start_date="2020-01-01" \
--end_date="2021-01-01"
```

## 下载并将解析后的数据储存到本地
```
gcloud config set project my-project-to-drive-demo-1

gsutil ls gs://next_day_wildfire_nuo_0#得到tfrecord文件路径A,即下行第一个path处

gsutil cp gs://next_day_wildfire_nuo_0/path/to/tfrecords/*.tfrecord.gz /path/to/your/local/data/  #本地路径/path/to/your/local/data/你自己选一下

python3 -m simulation_research.next_day_wildfire_spread.data_export.extract_ongoing_fires_main \
--file_pattern="/path/to/your/local/data/*.tfrecord.gz" \
--out_file_prefix="/path/to/your/local/data/" #本地路径
```
