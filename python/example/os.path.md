# os.path 모듈 활용

```python
import os

path1 = "folder/subfolder"
path2 = "file.txt"

# 1. 경로 조인
full_path = os.path.join(path1, path2)
print("Joined Path:", full_path)
# Joined Path: folder/subfolder/file.txt

# 2. 디렉터리 확인
directory_name = os.path.dirname(full_path)
print("Directory Name:", directory_name)
# Directory Name: folder/subfolder

# 3. 파일 이름 확인
base_name = os.path.basename(full_path)
print("Base Name:", base_name)
# Base Name: file.txt

# 4. 경로 존재 유무 확인
path_exists = os.path.exists(full_path)
print("Path Exists:", path_exists)
# Path Exists: False

# 5. 경로가 파일인지 확인
is_file = os.path.isfile(full_path)
print("Is File:", is_file)
# Is File: False

# 6. 경로가 디렉터리인지 확인
is_directory = os.path.isdir(directory_name)
print("Is Directory:", is_directory)
# Is Directory: False

# 7. 절대경로 확인
absolute_path = os.path.abspath(full_path)
print("Absolute Path:", absolute_path)
# Absolute Path: /Users/tf/Desktop/Learn/folder/subfolder/file.txt

# 8. 경로와 파일 분리
path_split = os.path.split(full_path)
print("Split Path:", path_split)
# Split Path: ('folder/subfolder', 'file.txt')

# 9. 파일 확장자 확인
file_root, file_ext = os.path.splitext(full_path)
print("File Root:", file_root)
print("File Extension:", file_ext)
# File Root: folder/subfolder/file
# File Extension: .txt
```

## 모듈 가져오기 목록에 포함시키기

파이썬에서 import를 이용해서 위치에 상관없이 불러오기 위해서는 sys.path 리스트에 경로를 추가해야한다.

```python
import os
import sys

new_path = os.path.join(os.getcwd(), 'your_directory')

if new_path not in sys.path:
    sys.path.append(new_path)

print("Current sys.path:")
for p in sys.path:
    print(p)
```