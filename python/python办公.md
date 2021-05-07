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
  
  

## 二、处理 Excel 表格

### 1、python 打开及读取 Excel 表格内容

+ 打开Excel表格并获取表格名称

  ```python
  from openpyxl import load_workbook
  
  workbook = load_workbook(filename='test.xlsx')
  print(workbook.sheetnames)
  ```

+ 获取sheet的尺寸范围

  ```python
  from openpyxl import load_workbook
  
  workbook = load_workbook(filename='test.xlsx')
  sheet = workbook['Sheet1']
  print(sheet.dimensions)
  ```

+ 获取某个单元格数值

  ```python
  from openpyxl import load_workbook
  
  workbook = load_workbook(filename='test.xlsx')
  sheet = workbook.active
  cell = sheet['C1']
  print(cell.value)
  ```

+ 获取某个单元格的行、列、坐标

  ```python
  from openpyxl import load_workbook
  
  workbook = load_workbook(filename='test.xlsx')
  sheet = workbook.active
  cell = sheet['C1']
  print(cell.row, cell.column, cell.coordinate)
  ```

  ```python
  from openpyxl import load_workbook
  
  workbook = load_workbook(filename='test.xlsx')
  sheet = workbook.active
  cell = sheet.cell(row=1, column=4)
  print(cell.row, cell.column, cell.coordinate)
  ```

+ 获取一系列的单元格

  ```python
  from openpyxl import load_workbook
  
  workbook = load_workbook(filename='test.xlsx')
  sheet = workbook.active
  for row in sheet.iter_rows(min_row=2, max_row=4, min_col=2, max_col=4):
      for cell in row:
          print(cell)
  ```

+ 迭代整个表格的所有行

  ```python
  from openpyxl import load_workbook
  
  workbook = load_workbook(filename='test.xlsx')
  sheet = workbook.active
  for row in sheet.rows:
      print(row)
  ```

### 2、python 向 Excel 表格中写入内容

+ 向某个单元格中写入内容并保存

  ```python
  from openpyxl import load_workbook
  
  workbook = load_workbook(filename='test.xlsx')
  sheet = workbook.active
  sheet['B2'] = 'hello world'
  workbook.save(filename='test.xlsx')
  ```

+ 插入python列表数据

  ```python
  from openpyxl import load_workbook
  
  workbook = load_workbook(filename='test.xlsx')
  sheet = workbook.active
  data = [
      ['张三', 1],
      ['李四', 2],
      ['王五', 3]
  ]
  for row in data:
      sheet.append(row)
  workbook.save(filename='test.xlsx')
  ```

+ 插入一列

  ```python
  from openpyxl import load_workbook
  
  workbook = load_workbook(filename='test.xlsx')
  sheet = workbook.active
  sheet.insert_cols(idx=2)
  workbook.save(filename='test.xlsx')
  ```

+ 插入多行

  ```python
  from openpyxl import load_workbook
  
  workbook = load_workbook(filename='test.xlsx')
  sheet = workbook.active
  sheet.insert_rows(idx=2, amount=3)
  workbook.save(filename='test.xlsx')
  ```

+ 删除行

  ```python
  from openpyxl import load_workbook
  
  workbook = load_workbook(filename='test.xlsx')
  sheet = workbook.active
  sheet.delete_rows(idx=2, amount=2)
  workbook.save(filename='test.xlsx')
  ```

+ 移动单元格

  ```python
  from openpyxl import load_workbook
  
  workbook = load_workbook(filename='test.xlsx')
  sheet = workbook.active
  sheet.move_range('A1:B2', cols=2, rows=2)
  workbook.save(filename='test.xlsx')
  ```

+ 创建和删除表格

  ```python
  from openpyxl import load_workbook
  
  workbook = load_workbook(filename='test.xlsx')
  print(workbook.sheetnames)
  workbook.create_sheet('MySheet')
  sheet = workbook['Sheet1']
  workbook.remove(sheet)
  workbook.save(filename='test.xlsx')
  ```

+ 复制表格

  ```python
  from openpyxl import load_workbook
  
  workbook = load_workbook(filename='test.xlsx')
  sheet = workbook['MySheet']
  workbook.copy_worksheet(sheet)
  workbook.save(filename='test.xlsx')
  ```

+ 修改表格名称

  ```python
  from openpyxl import load_workbook
  
  workbook = load_workbook(filename='test.xlsx')
  workbook['MySheet'].title = 'MyNewSheet'
  workbook.save(filename='test.xlsx')
  ```

+ 创建新的Excel文件

  ```python
  from openpyxl import Workbook
  
  workbook = Workbook()
  sheet = workbook.active
  sheet.title = '表格一'
  for i in range(1, 10):
      for j in range(1, 10):
          sheet.cell(row=i, column=j).value = i * j
  workbook.save(filename='output.xlsx')
  ```

+ 冻结窗格

  ```python
  from openpyxl import load_workbook
  
  workbook = load_workbook(filename='test.xlsx')
  sheet = workbook.active
  sheet.freeze_panes = 'B2'
  workbook.save(filename='test.xlsx')
  ```

+ 筛选

  ```python
  from openpyxl import load_workbook
  
  workbook = load_workbook(filename='test.xlsx')
  sheet = workbook.active
  sheet.auto_filter.ref = sheet.dimensions
  workbook.save(filename='test.xlsx')
  ```

+ 修改字体样式

  ```python
  from openpyxl.styles import Font
  from openpyxl import load_workbook
  
  workbook = load_workbook(filename='test.xlsx')
  sheet = workbook.active
  cell = sheet['A2']
  cell.font = Font(name='微软雅黑', size=12, italic=True, bold=True, color='ff0000')
  workbook.save(filename='test.xlsx')
  ```

+ 获取表格中字体的样式

  ```python
  from openpyxl import load_workbook
  
  workbook = load_workbook(filename='test.xlsx')
  sheet = workbook.active
  cell = sheet['B2']
  font = cell.font
  print(font.name, font.size, font.italic, font.bold)
  ```

+ 设置对其样式

  `wrap_text` 是否自动换行

  ```python
  from openpyxl import load_workbook
  from openpyxl.styles import Alignment
  
  workbook = load_workbook(filename='test.xlsx')
  sheet = workbook.active
  cell = sheet['B2']
  cell.value = '锄禾日当午，汗滴禾下土。'
  alignment = Alignment(horizontal='center', vertical='center', text_rotation=45)
  cell.alignment = alignment
  workbook.save(filename='test.xlsx')
  ```

+ 单元格边框设置

  ```python
  from openpyxl import load_workbook
  from openpyxl.styles import Border,Side
  
  workbook = load_workbook(filename='test.xlsx')
  sheet = workbook.active
  cell = sheet['C3']
  side1 = Side(style='thin', color='FF0000')
  side2 = Side(style='dashed', color='0000FF')
  border = Border(left=side1, right=side1, top=side2, bottom=side2)
  cell.border = border
  workbook.save(filename='test.xlsx')
  ```

+ 设置填充样式

  ```python
  from openpyxl import load_workbook
  from openpyxl.styles import PatternFill, GradientFill
  
  workbook = load_workbook(filename='test.xlsx')
  sheet = workbook.active
  cell_d4 = sheet['D4']
  pattern_fill = PatternFill(fill_type='solid', fgColor='FF0000')
  cell_d4.fill = pattern_fill
  cell_e5 = sheet['E5']
  gradient_fill = GradientFill(stop=('FF0000', '00FF00', '0000FF'))
  cell_e5.fill = gradient_fill
  workbook.save(filename='test.xlsx')
  ```

+ 设置行高和列宽

  ```python
  from openpyxl import load_workbook
  
  workbook = load_workbook(filename='test.xlsx')
  sheet = workbook.active
  sheet.row_dimensions[1].height = 50
  sheet.column_dimentsions['A'].width = 50
  workbook.save(filename='test.xlsx')
  ```

+ 合并单元格

  ```python
  from openpyxl import load_workbook
  
  workbook = load_workbook(filename='test.xlsx')
  sheet = workbook.active
  sheet.merge_cells('A1:B2')
  sheet.merge_cells(start_row=5, end_row=7, start_column=5, end_column=7)
  workbook.save(filename='test.xlsx')
  ```

  取消合并单元格 `unmerge_cells` 

+ 插入图片

  ```python
  from openpyxl import load_workbook
  from openpyxl.drawing.image import Image
  
  workbook = load_workbook(filename='test.xlsx')
  sheet = workbook.active
  logo = Image('logo.png')
  logo.width = 100
  logo.height = 50
  sheet.add_image(logo, 'C1')
  workbook.save(filename='test.xlsx')
  ```

+ 插入柱状图

  ```python
  from openpyxl import load_workbook
  from openpyxl.chart import BarChart, Reference
  
  workbook = load_workbook(filename='test.xlsx')
  sheet = workbook.active
  chart = BarChart()
  data = Reference(worksheet=sheet, min_row=1, max_row=6, min_col=2, max_col=6)
  categories = Reference(sheet, min_col=1, min_row=2, max_row=6)
  chart.add_data(data, titles_from_data=True)
  chart.set_categories(categories)
  sheet.add_chart(chart, 'A7')
  workbook.save('test.xlsx')
  ```

+ 插入条形图

  ```python
  from openpyxl import load_workbook
  from openpyxl.chart import LineChart, Reference
  
  workbook = load_workbook(filename='test.xlsx')
  sheet = workbook.active
  chart = LineChart()
  data = Reference(worksheet=sheet, min_row=1, max_row=6, min_col=2, max_col=6)
  categories = Reference(sheet, min_col=1, min_row=2, max_row=6)
  chart.add_data(data, from_rows=True, titles_from_data=True)
  chart.set_categories(categories)
  sheet.add_chart(chart, 'A7')
  workbook.save('test.xlsx')
  ```

## 三、处理 PDF 文件

### 1、提取PDF文字内容

+ 使用 pdfplumber 提取文字

  ```python
  import pdfplumber
  
  with pdfplumber.open('test.pdf') as pdf:
      first_page = pdf.pages[0]
      print(first_page.extract_text())
  ```

+ 使用 pdfplumber 提取单个简单表格

  ```python
  import pdfplumber
  
  with pdfplumber.open('test.pdf') as pdf:
      first_page = pdf.pages[0]
      table = first_page.extract_table()
      print(table)
  ```

+ 使用 pdfplumber 提取多个简单表格

  ```python
  import pdfplumber
  
  with pdfplumber.open('test.pdf') as pdf:
      table_page = pdf.pages[0]
      for table in table_page.extract_tables():
          print(table)
  ```

+ 提取表格时设定参数

  ```python
  import pdfplumber
  
  with pdfplumber.open('test.pdf') as pdf:
      table_page = pdf.pages[0]
      table = table_page.extract_table(
      	table_settings={
              "vertical_strategy": "text",
              "horizontal_strategy": "text"
          }
      )
      print(table)
  ```

+ 使用 pypdf2 分割 pdf

  ```python
  from PyPDF2 import PdfFileReader, PdfFileWriter
  
  pdf_reader = PdfFileReader('test.pdf')
  for page in range(pdf_reader.getNumPages()):
      pdf_writer = PdfFileWriter()
      pdf_writer.addPage(pdf_reader.getPage(page))
      with open(f'./分割后的PDF文件/divide_page{page}.pdf', 'wb') as out:
          pdf_writer.write(out)
  ```

+ 使用 pypdf2 合并 pdf

  ```python
  from PyPDF2 import PdfFileReader, PdfFileWriter
  
  pdf_writer = PdfFileWriter()
  for page in range(2):
      pdf_reader = PdfFileReader(f'./分割后的PDF文件/divide_page{page}.pdf')
      for page_num in range(pdf_reader.getNumPages()):
          pdf_writer.addPage(pdf_reader.getPage(page_num))
  
  with open('merge.pdf', 'wb') as out:
      pdf_writer.write(out)
  ```

+ 使用 pypdf2 旋转 pdf 页面

  ```python
  from PyPDF2 import PdfFileReader, PdfFileWriter
  
  pdf_reader = PdfFileReader('test.pdf')
  pdf_writer = PdfFileWriter()
  for num in range(pdf_reader.getNumPages()):
      if num % 2 == 0:
          page = pdf_reader.getPage(num).rotateClockwise(90)
          pdf_writer.addPage(page)
      else:
          page = pdf_reader.getPage(num).rotateCounterClockwise(90)
          pdf_writer.addPage(page)
  
  with open('rotateTest.pdf', 'wb') as out:
      pdf_writer.write(out)
  ```

+ 使用 pypdf2 给 pdf 加水印

  ```python
  from PyPDF2 import PdfFileReader, PdfFileWriter
  from copy import copy
  
  watermark_pdf = PdfFileReader('watermark.pdf')
  watermark_page = watermark_pdf.getPage(0)
  
  pdf_reader = PdfFileReader('test.pdf')
  pdf_writer = PdfFileWriter()
  
  for page in range(pdf_reader.getNumPages()):
    original_page = pdf_reader.getPage(page)
    new_page = copy(watermark_page)
    new_page.mergePage(original_page)
    pdf_writer.addPage(new_page)
  
  with(open('watermarkFile.pdf', 'wb')) as out:
    pdf_writer.write(out)
  ```

+ 使用 pypdf2 加密 pdf 文件

  ```python
  from PyPDF2 import PdfFileReader, PdfFileWriter
  
  pdf_reader = PdfFileReader("test.pdf")
  pdf_writer = PdfFileWriter()
  for page in range(pdf_reader.getNumPages()):
      pdf_writer.addPage(pdf_reader.getPage(page))
  
  pdf_writer.encrypt("123456")
  with open('test_encrypt.pdf', 'wb') as out:
      pdf_writer.write(out)
  ```

+ 使用 pypdf2 解密 pdf 文件

  ```python
  from PyPDF2 import PdfFileReader, PdfFileWriter
  
  pdf_reader = PdfFileReader('test_encrypt.pdf')
  pdf_reader.decrypt("123456")
  pdf_writer = PdfFileWriter()
  
  for page in range(pdf_reader.getNumPages()):
      pdf_writer.addPage(pdf_reader.getPage(page))
  
  with open('test_decrypt.pdf', 'wb') as out:
      pdf_writer.write(out)
  ```

  

## 四、处理Word文件

### 1、Python读取Word文档内容

+ 读取内容

  ```python
  from docx import Document
  
  doc = Document('test.docx')
  for paragraph in doc.paragraphs:
    print(paragraph.text)
  ```

+ 读取文字块

  ```python
  from docx import Document
  
  doc = Document('test.docx')
  for paragraph in doc.paragraphs:
    for run in paragraph.runs:
      print(run.text)
  ```

### 2、Python写入Word文档内容

+ 添加标题、段落

  ```python
  from docx import Document
  
  doc = Document()
  doc.add_heading('一级标题', level=1)
  paragraph1 = doc.add_paragraph('这是第一段段落。')
  paragraph2 = doc.add_paragraph()
  paragraph2.add_run('加粗').bold = True
  paragraph2.add_run('普通')
  paragraph2.add_run('斜体').italic = True
  doc.save('test.docx')
  ```

+ 添加分页

  ```python
  from docx import Document
  
  doc = Document()
  doc.add_heading("第一页", level=2)
  doc.add_paragraph("第一页内容")
  doc.add_page_break()
  doc.add_heading("第二页", level=3)
  doc.add_paragraph("第二页内容")
  doc.save("test2.docx")
  ```

+ 添加图片

  ```python
  from docx import Document
  from docx.shared import Cm
  
  doc = Document()
  doc.add_picture("test.jpg")
  doc.add_picture("test.jpg", width=Cm(5))
  doc.add_picture("test.jpg", width=Cm(5), height=Cm(8))
  doc.save("test3.docx")
  ```

+ 添加表格

  ```python
  from docx import Document
  
  records = [
    ['学号', '姓名', '成绩'],
    [101, '张三', 99],
    [102, '李四', 96],
    [103, '王五', 98]
  ]
  
  doc = Document()
  table = doc.add_table(rows=4, cols=3)
  for row in range(4):
    cells = table.rows[row].cells
    for col in range(3):
      cells[col].text = str(records[row][col])
  
  doc.save('test4.docx')
  ```

### 3、Word文档内容编辑

+ 文字样式的修改

  ```python
  from docx import Document
  from docx.shared import Pt, RGBColor
  from docx.oxml.ns import qn
  
  doc = Document("test.docx")
  for paragraph in doc.paragraphs:
  	for run in paragraph.runs:
  		run.font.bold = True
  		run.font.italic = True
  		run.font.underline = True
  		run.font.strike = True
  		run.font.shadow = True
  		run.font.size = Pt(20)
  		run.font.color.rgb = RGBColor(255, 0, 0)
  		run.font.name = '微软雅黑'
  		r = run._element.rPr.rFonts
  		r.set(qn('w:eastAsia'), '微软雅黑')
  
  doc.save("test_modified.docx")
  ```

+ 段落对齐

  ```python
  from docx import Document
  from docx.enum.text import WD_ALIGN_PARAGRAPH
  
  doc = Document('test.docx');
  for paragraph in doc.paragraphs:
  	paragraph.alignment = WD_ALIGN_PARAGRAPH.CENTER
  
  doc.save('test_modified.docx')
  ```

+ 段落行间距

  ```python
  from docx import Document
  from docx.enum.text import WD_ALIGN_PARAGRAPH
  
  doc = Document('test.docx')
  for paragraph in doc.paragraphs:
  	paragraph.paragraph_format.line_spacing = 2.0
  
  doc.save('test_modified.docx')
  ```

+ 段前段后间距

  ```python
  from docx import Document
  from docx.enum.text import WD_ALIGN_PARAGRAPH
  from docx.shared import Pt
  
  doc = Document('test.docx')
  for paragraph in doc.paragraphs:
  	paragraph.paragraph_format.space_before = Pt(12)
  	paragraph.paragraph_format.space_after = Pt(12)
  
  doc.save('test_modified.docx')
  ```



## 五、处理PPT文件

### 1、Python读取PPT

+ 读取PPT文字

  ```python
  from pptx import Presentation
  
  prs = Presentation('test.pptx')
  for slide in prs.slides:
    for shape in slide.shapes:
      if shape.has_text_frame:
        text_frame = shape.text_frame
        print(text_frame.text)
  ```

+ 读取PPT段落

  ```python
  from pptx import Presentation
  
  prs = Presentation('test.pptx')
  for slide in prs.slides:
    for shape in slide.shapes:
      text_frame = shape.text_frame
      for paragraph in text_frame.paragraphs:
        print(paragraph.text)
  ```

### 2、Python写入PPT

+ 获取PPT模板信息并导出到新PPT中

  ```python
  from pptx import Presentation
  
  prs = Presentation('模板.pptx')
  slide = prs.slides.add_slide(prs.slide_layouts[0])
  for shape in slide.placeholders:
    phf = shape.placeholder_format
    print(f'{phf.idx}--${shape.name}--{phf.type}')
    shape.text = f'{phf.idx}***{phf.type}'
  
  prs.save('output.pptx')
  ```

+ 根据模板往PPT中添加内容

  ```python
  from pptx import Presentation
  from datetime import datetime
  
  prs = Presentation('奖学金证书模板.pptx')
  title_slide_layout = prs.slide_layouts[0]
  slide = prs.slides.add_slide(title_slide_layout)
  
  winner_name = slide.placeholders[0]
  certificate_type = slide.placeholders[1]
  prefix_info = slide.placeholders[20]
  suffix_info = slide.placeholders[19]
  award_presenter = slide.placeholders[17]
  award_date = slide.placeholders[21]
  
  winner_name.text = '张三'
  certificate_type.text = '三好学生奖状'
  prefix_info.text = '兹证明'
  suffix_info.text = '品德优秀'
  award_presenter.text = '李老师'
  award_date.text = str(datetime.now().date())
  
  prs.save('output.pptx')
  ```

+ PPT添加段落、设置层级关系

  ```python
  from pptx import Presentation
  
  prs = Presentation()
  slide = prs.slides.add_slide(prs.slide_layouts[1])
  shapes = slide.shapes
  title_shape = shapes.title
  body_shape = shapes.placeholders[1]
  
  title_shape.text = '添加项目符号列表页'
  
  tf = body_shape.text_frame
  tf.text = '带圆点的项目符号行1'
  
  p = tf.add_paragraph()
  p.text = '带圆点的项目符号行2'
  p.level = 1
  
  p = tf.add_paragraph()
  p.text = '带圆点的项目符号行3'
  p.level = 2
  
  prs.save('output.pptx')
  ```

+ PPT添加文本框

  ```python
  from pptx import Presentation
  from pptx.util import Cm,Pt
  
  psr = Presentation()
  slide = psr.slides.add_slide(psr.slide_layouts[6])
  
  left = top = width = height = Cm(3)
  text_box = slide.shapes.add_textbox(left, top, width, height)
  tf = text_box.text_frame
  tf.text = '这是一段文本框的文字'
  
  p = tf.add_paragraph()
  p.text = '这是第二段文字，加粗，字号40'
  p.font.bold = True
  p.font.size = Pt(40)
  
  psr.save('output.pptx')
  ```

+ PPT添加图片

  ```python
  from pptx import Presentation
  from pptx.util import Cm
  
  psr = Presentation()
  slide = psr.slides.add_slide(psr.slide_layouts[6])
  
  left = top = Cm(3)
  slide.shapes.add_picture('img.png', left, top)
  
  left = Cm(10)
  top = Cm(10)
  height = Cm(2)
  slide.shapes.add_picture('img.png', left, top, height=height)
  
  psr.save('output.pptx')
  ```

+ PPT添加表格

  ```python
  from pptx import Presentation
  from pptx.util import Cm
  
  prs = Presentation()
  slide = prs.slides.add_slide(prs.slide_layouts[6])
  
  rows, cols = 4, 2
  left = top = Cm(5)
  width = Cm(18)
  height = Cm(3)
  
  table = slide.shapes.add_table(rows, cols, left, top, width, height).table
  
  table.columns[0].width = Cm(6)
  table.columns[1].width = Cm(4)
  table.rows[0].height = Cm(2)
  
  data = [
    ['姓名', '成绩'],
    ['张三', 98],
    ['李四', 89],
    ['王五', 80]
  ]
  for row in range(rows):
    for col in range(cols):
      table.cell(row, col).text = str(data[row][col])
  
  prs.save('output.pptx')
  ```

  