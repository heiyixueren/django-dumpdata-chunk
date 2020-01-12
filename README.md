# 面对大量数据时：Django1.9版本的 dumpdata和loaddata

记录如何处理Django导出导入大量数据集时，内存超出的问题。
当内存比较小，数据集比较大时，因为会先load到内存而导致内存溢出无法执行后续导入工作，
因此修改原有的dump逻辑，将导出进行分片处理。

## 使用方式
1. 将management文件夹复制到与settings.py同一个目录下；
2. 创建分片文件存储位置，应用到下面命令中的output-folder；
3. 执行命令：

~~~
python manage.py dumpdata_chunks app_name --output-folder=/dump-data-folder/ --max-records-per-chunk=100000
~~~

4. 执行下面命令，拼装loaddata语句，并导出到loaddata.sh脚本中：

~~~
find /dump-data-folder | egrep -o "([0-9]+_[0-9]+)" |sort | awk '{print "python manage.py loaddata /dump-data-folder/"$1".json"}' > loaddata.sh
~~~
5. 加载数据：sh loaddata.sh


如有困惑或使用问题，可联系：1562716988 qq/wx同号