# python 办公

## 一、文件处理

### 1、输出目录下所有文件及文件夹

+ 获取当前python程序运行目录

  ```python
  import os
  print(os.getpwd())
  ```

+ 路径连接

  ```python
  import os
  print(os.path.join('pythonTest', 'test1'))
  ```

+ 列出某个文件夹下所有文件和文件夹

  ```python
  import os
  print(os.listdir('D:/mycode/'))
  ```

+ 循环判断指定文件夹中哪些是文件夹

  ```python
  import os
  
  files = os.listdir()
  for file in files:
      print(file, os.path.isdir(file))
  ```

+ `os.scandir()` 查找文件**(更推荐)** 

  ```python
  import os
  
  for file in os.scandir('D:/mycode/'):
      print(file.name, file.path, file.is_dir())
  ```
+ **练习：** 列出当前目录包含hello字符串的文件的个数（不区分大小写） 

  ```python
  import os
  
  num = 0
  for file in os.scandir('./'):
      if not file.is_dir():
          if 'hello' in file.name.lower():
          num += 1
  print("包含hello的文件个数：", num)
  ```

  

### 2、遍历、搜索文件及查询文件信息

+ 遍历文件夹

  ```python
  import os
  
  for root, dirs, files in os.walk('./'):
      print(f'发现文件夹：{root})')
      print(f'文件夹列表：{dirs}')
      print(f'文件列表：{files}')
    print('--------------------------')
  ```

+ 是否某字符串开头、结尾

  ```python
  print('abc.txt'.startswith('ab'))
  print('abc.txt'.endswith('.txt'))
  ```

+ 搜索指定的文件

  | 模式   | 意义                    |
  | ------ | ----------------------- |
  | *      | 匹配所有                |
  | ？     | 匹配任何单个字符        |
  | [seq]  | 匹配seq中的任何字符     |
  | [!seq] | 匹配任何不在seq中的字符 |

  ```python
  import glob
  print(glob.glob('*.txt'))
  ```

+ 递归搜索指定文件

  ```python
  import glob
  print(glob.glob('**/*.txt', recursive=True))
  ```

+ 匹配文件名

  ```python
  import fnmatch
  
  print(fnmatch.fnmatch('lesson1.py', 'le*1.py'))
  print(fnmatch.fnmatch('lesson1.py', 'le*[0-9].py'))
  ```

+ 查询文件信息

  ```python
  import os
  
  import time
  for file in os.scandir():
      print(file.name, file.stat().st_size/1024/1024, time.ctime(file.stat().st_mtime))
  ```

+ datetime模块输出时间

  ```python
  import datetime
  
  that_time = datetime.datetime.fromtimestamp(1567764428)
  print(that_time)
  print(that_time.year, that_time.month, that_time.day, that_time.hour, that_time.minute, that_time.second)
  ```

+ **练习：** 递归搜索当前文件夹中今年修改的后缀名为 `.zip` 的所有文件

  ```python
  import os
  import glob
  import datetime
  
  now_year = datetime.datetime.now().year
  print('当前目录以下文件为今年的.zip压缩文件：')
  for file in glob.glob('**/*.zip', recursive=True):
    file_year = datetime.datetime.fromtimestamp(os.stat(file).st_mtime).year
      if file_year == now_year:
          print(file, f'文件大小为：{os.stat(file).st_size/1024}KB')
  ```

### 3、创建临时文件及文件夹

+ 读取文件

  ```python
  f = open('test.txt', 'r', encoding='utf-8')
  text = f.readlines()
  print(text)
  f.close()
  ```

  with ... as ... 写法：

  ```python
  with open('test.txt', 'r', encoding='utf-8') as f:
      text = f.readlines()
      print(text)
  ```

+ 写入文件

  ```python
  with open('test2.txt', 'w', encoding='utf-8') as f:
      f.write('hello world')
  ```

+ 创建临时文件

  ```python
  from tempfile import TemporaryFile
  
  f = TemporaryFile('w+')
  f.write('hello world')
  f.seek(0)
  data = f.readlines()
  print(data)
  f.close()
  ```

+ 创建临时文件夹

  ```python
  from tempfile import TemporaryDirectory
  
  with TemporaryDirectory() as tmp_folder:
      print(f'临时文件夹:{tmp_folder}')
  ```

### 4、批量创建、复制、移动、删除、重命名文件及文件夹

+ 创建文件夹

  ```python
  import os
  
  if not os.path.exists('dirname'):
  	os.mkdir('dirname')
  ```

+ 创建多层文件夹

  ```python
  import os
  os.makedirs('aa/bb/cc')
  ```

+ 复制文件

  ```python
  import shutil
  
  shutil.copy('test.txt', './aa/test.txt')
  shutil.copy('test.txt', './aa/new_test.txt')
  ```

+ 复制文件夹 (新文件夹不能已经存在)

  ```python
  import shutil
  shutil.copytree('aa/', 'dd/')
  ```

+ 移动文件或者文件夹

  ```python
  import shutil
  
  shutil.move('test.txt', 'bb/')
  shutil.move('test.txt', 'bb/new_test.txt')
  shutil.move('aa/', 'bb/')
  ```

+ 重命名文件或文件夹

  ```python
  import os
  
  os.rename('test.txt', 'new_test.txt')
  os.rename('aa/', 'new_aa/')
  ```

+ 删除文件

  ```python
  import os
  os.remove('test.txt')
  ```

+ 删除文件夹

  ```python
  import shutil
  shutil.rmtree('aa/')
  ```

+ **练习：** 找出当前文件夹中所有.zip文件，并加上该文件最后修改日期重命名，然后将所有重命名后的文件移动到backup文件夹中。

  ```python
  import os
  import datetime
  import shutil
  
  if not os.path.exists('backup'):
      os.mkdir('backup')
  for file in os.scandir():
      if file.name.endswith('.zip'):
          m_format_time = datetime.datetime.fromtimestamp(os.stat(file.name).st_mtime).strftime('%Y-%m-%d')
          shutil.move(file.name, 'backup/' + m_format_time + '-' + file.name)
  ```

### 5、创建和解压压缩包

+ 读取压缩包

  ```python
  import zipfile
  
  with zipfile.ZipFile('test.zip', 'r') as zipobj:
      for file_name in zipobj.namelist():
          print(file_name)
  ```

+ 读取压缩包内文件的信息

  ```python
  import zipfile
  
  with zipfile.ZipFile('test.zip', 'r') as zipobj:
      for file_name in zipobj.namelist():
          info = zipobj.getinfo(file_name)
          print(file_name, info.file_size, info.compress_size)
  ```

+ 解压压缩包中的单个文件

  ```python
  import zipfile
  
  with zipfile.ZipFile('test.zip', 'r') as zipobj:
      zipobj.extract('test.txt')
  ```

+ 解压所有文件

  ```python
  import zipfile
  
  with zipfile.ZipFile('test.zip', 'r') as zipobj:
      zipobj.extractall()
  ```

+ 将有密码的压缩包解压

  ```python
  import zipfile
  
  with zipfile.ZipFile('test.zip', 'r') as zipobj:
      zipobj.extractall(path='解压/', pwd=b'123456')
  ```

+ 创建压缩包

  ```python
  import zipfile
  
  file_list = ['test1.txt', 'test2.txt', 'test3.txt']
  with zipfile.ZipFile('compress.zip', 'w') as zipobj:
      for file in file_list:
          zipobj.write(file)
  ```

+ 向已有压缩包中添加文件

  ```python
  import zipfile
  
  with zipfile.ZipFile('compress.zip', 'a') as zipobj:
      zipobj.write('test4.txt')
  ```

+ **练习：** 将当前文件夹所有修改时间在今天之前的文件重命名加上最后修改日期，将所有重命名后的文件都添加到带有今天日期的压缩包里，并将压缩包移动到backup文件夹中，删除原始文件。

  ```python
  import os
  import datetime
  import zipfile
  
  new_name_list = []
  now_time_num = int(datetime.datetime.now().strftime('%Y%m%d'))
  for file in os.scandir():
      if not (file.is_dir() or file.name.endswith('.py')):
          file_mtime = os.stat(file.name).st_mtime
          file_mtime_num = int(datetime.datetime.fromtimestamp(file_mtime).strftime('%Y%m%d'))
          if file_mtime_num < now_time_num:
              file_mtime_format = datetime.datetime.fromtimestamp(file_mtime).strftime('%Y-%m-%d')
              file_new_name = file_mtime_format + '-' + file.name
              print(file.name, file_mtime_format)
              os.rename(file.name, file_new_name)
              new_name_list.append(file_new_name)
  
  if not os.path.exists('backup'):
      os.mkdir('backup')
  if len(new_name_list) > 0:
      now_time = datetime.datetime.now()
      now_date_format = now_time.strftime('%y-%m-%d')
      compress_name = now_date_format + '-compress.zip'
      with zipfile.ZipFile('./backup/' + compress_name, 'w') as zipobj:
          for file_name in new_name_list:
              zipobj.write(file_name)
              os.remove(file_name)
  ```