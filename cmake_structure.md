# XDB CMake Build System

XDB 프로젝트의 CMake 빌드 시스템에 대해 서술합니다.

## Contents

### [1.Directory Tree](#directory-tree)

- [build](#build)
- [cmake_modules](#cmake_modules)
- [thirdparty](#thirdparty)
- [src](#src)

### [2.File Structure](#file-structure)

- [root CMakeLists.txt](#root-cmakelists.txt)
- [src CMakeLists.txt](#src-cmakelists.txt)
- [Find[PACKAGE].cmake](#find-package-.cmake)

----------

## Directory Tree

$ tree xdb  

📦xdb  
 ┣ 📂build  
 ┣ 📂cmake_modules  
 ┣ 📂src  
 ┃ ┣ 📂module1  
 ┃ ┃ ┗ 📜CMakeLists.txt  
 ┃ ┣ 📂module2  
 ┃ ┃ ┗ 📜CMakeLists.txt  
 ┃ ┗ 📂module3  
 ┃ ┃ ┗ 📜CMakeLists.txt  
 ┣ 📂thirdparty  
 ┃ ┣ 📜download_dependencies.sh  
 ┃ ┗ 📜versions.txt  
 ┗ 📜CMakeLists.txt  

### build

실행 바이너리 파일, CMakeCache.txt등 빌드의 결과물이 생성되는 디렉토리입니다.

### cmake_modules

빌드에 필요한 .cmake 파일들이 저장된 디렉토리입니다. .cmake 파일에는 아래 항목들이 구현되어 있습니다.

- third party library에 대한 [find_library](https://cmake.org/cmake/help/latest/command/find_library.html?highlight=find_library)
- 빌드에 필요한 utility 함수들

### thirdparty

thirdparty library의 아카이브 파일형식(tar.gz; zip)으로 저장되어 있습니다. 빌드 타임에 이 파일들이 압축해제되고 빌드되어 xdb에 링크됩니다.

download_dependencies.sh
: 파일 다운로드를 수행하는 스크립트

version.txt
: library 버전과 다운로드 URL이 저장 된 파일

### src

소스 파일이 저장되어 있습니다. 각각의 디렉토리에 CMakeLists.txt를 갖고 있습니다.

## File Structure

### root CMakeLists.txt

### src CMakeLists.txt

### Find[PACKAGE].cmake
