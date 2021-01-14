# XDB C++ Coding Style

XDB의 C++ 코딩 규칙을 정의합니다. [Google C++ Style Guide](https://google.github.io/styleguide/cppguide.html) 에서 일부 항목을 추려 정리하였습니다. [cpplint.py](https://raw.githubusercontent.com/google/styleguide/gh-pages/cpplint/cpplint.py) 를 사용해 style error를 검출 할 수 있습니다.

## Contents

### [1. Header Files](#1.-header-files)

- [The #define Guard](#the-define-guard)
- [Inline Functions](#inline-functions)
- [Order of Includes](#order-of-includes)

### [2. Scoping](#2.-scoping)

- [Namespaces](#namespaces)
- [Local Variables](#local-variables)

### [3. Other C++ Features](#3.-other-c++-features)

- [Casting](#casting)
- [Preincrement and Predecrement](#preincrement-and-predecrement)
- [Integer Types](#integer-types)

### [4. Naming](#4.-naming)

- [General Naming Rules](#general-naming-rules)
- [File Names](#file-names)
- [Type Names](#type-names)
- [Variable Names](#variable-names)
- [Constant Names](#constant-names)
- [Function Names](#function-names)

### [5. Comments](#5.-comments)

----------

## 1. Header Files

main() 함수를 포함하고 있는 .cc 파일이거나 특수한 경우를(e.g. unit test를 구현한 .cc파일) 제외하고 모든 .cc 파일은 대응하는 .h 파일을 가지도록 합니다.

### The #define Guard

헤더파일의 중복 include를 막기 위해 #define guards를 사용합니다. 심볼 형식은 다음과 같습니다. **\<PROJECT\>\_\<PATH\>\_\<FILE\>\_H\_**. 예로 *xdb/src/archiver/csv_parser.h* 파일의 내용은 아래와 같습니다.

```C++
#ifndef XDB_ARCHIVER_CSV_PARSER_H_
#define XDB_ARCHIVER_CSV_PARSER_H_

//...

#endif // XDB_ARCHIVER_CSV_PARSER_H_
```

### Inline Functions

함수를 inline으로 정의하고 싶다면 10줄 이내의 짧은 함수만 inline 함수로 정의합니다.

### Order of Includes

header의 include 순서는 아래와 같은 순서를 따릅니다.  

1. cc 파일과 직접 연관된 header 파일  
2. 빈 줄  
3. C system header 파일들  
4. 빈 줄  
5. C++ standard library header 파일들  
6. 빈 줄  
7. 다른 라이브러리 header 파일들  
8. 해당 프로젝트의 header 파일들  

같은 섹션 내부에서는 알파벳의 오름차순으로 정렬합니다.  
예를 들어, xdb/src/archiver/csv_parser.cc 내부의 include는 아래와 같습니다.

```C++
#include "archiver/csv_parser.h"

#include <sys/types.h>
#include <unistd.h>

#include <algorithm>
#include <vector>

#include "arrow/api.h"
#include "glog/logging.h"
#include "ipc/ipc.h" // xdb/src/ipc/ipc.h
```

C system header 파일들은 대체로 C++ 헤더파일로 대체가 가능한 경우가 있습니다. C++ 헤더를 include 하도록 합니다.

```C++
//#include <stddef.h>
#include <cstddef>
```

----------

## 2. Scoping

### Namespaces

- 프로젝트 이름에 기반한 namespace를 갖도록 합니다.

```C++
namespace xdb {

    ...

} // namespace xdb
```

- *using namespace* 지시자는 namespace의 혼동을 초래하므로 사용을 지양합니다.

```C++
using namespace xdb; // Bad
using xdb::QueryParser; // Good
```

- protocol 메시지를 사용할 때 .proto 파일에 [package](https://developers.google.com/protocol-buffers/docs/reference/cpp-generated#package) 지정자를 사용합니다.

### Local Variables

가독성을 높이기 위해 가능한 지역변수의 선언과 초기화를 동시에 수행합니다.  

```C++
int i;
i = f(); // Bad

int i = f() // Good

std::vector<int> v;
v.push_back(1);
v.push_back(2); // Bad

std::vector<int> v = {1, 2}; // Good
```

## 3. Other C++ Features

### Casting

C 스타일의 캐스팅은 사용 목적을 분명히 드러내기 어렵습니다. 따라서 C++ 스타일의 type casting을 사용하도록 합니다.

- static_cast
- const_cast
- dynamic_cast
- reinterpret_cast

단, dynamic_cast를 사용할 때 클래스 상속관계를 분명히 파악하고 사용하여야 합니다.

### Preincrement and Predecrement

의미상 postincrement(i++ or i--)를 사용할 필요가 없다면 preincrement 연산자(++i or --i)를 사용하도록 합니다.

### Integer Types

short, long long 등의 built-in type 보다 \<cstdint\> 에서 정의한 int16_t, int64_t 등의 type을 사용하도록 합니다. 단, int32_t의 의미가 명확히 필요한 상황이 아니면 int를 사용은 괜찮습니다.

## 4. Naming

### General Naming Rules

보편적으로 사용되는 축약어(e.g. comma-seperated values: csv) 외에 축약어의 사용을 최소화 합니다. 코드의 길이를 줄이는 것 보다 독자의 이해를 높이는 것을 목표로 합니다.

프로그래밍 관습으로 알려진 축약어의 사용은 허용합니다.

- **i** for iteration variable
- **T** for template parameter

### File Names

파일이름은 모두 소문자로 하고 단어사이에는 underscore ( _ ) 를 포함 합니다. C++ 파일의 확장자는 .cc 로 하고 헤더파일의 확장자는 .h 로 합니다.  

- my_usefull_class.cc
- my_usefull_class_test.cc
- my_usefull_class.h

구체적인 이름을 갖도록 합니다.

- ~~logs.h~~
- http_server_logs.h

### Type Names

각 단어는 첫 문자를 대문자로하고 나머지는 소문자로 합니다. 각 단어 사이에 underscore (_) 를 포함하지 않습니다. 이 규칙을 따르는 타입은 다음과 같습니다.  

- class
- struct
- type aliase
- enum
- type template parameter

```C++
//classes and structs
class UrlTable { ...
class UrlTableTester { ...
struct UrlTableProperties { ...

// type aliases
using PropertiesMap = ...

// enums
enum class UrlTableError { ...

// type template parameter
template <typename TypeClass>
```

### Variable Names

일반 변수, 함수 파라미터, 멤버 변수는 모두 소문자로 합니다. 단어사이는 underscore로 잇습니다. class 멤버 변수에는 끝에는 underscore를 붙입니다.  

```C++
// common variable
std::string table_name; 

// function parameter
void PrintTableInfo(std::string table_name);

// class
class TableInfo {
...
private:
    std::string table_name_; // underscore at end
    static Pool<TableInfo>* pool_; // static 멤버 변수도 마찬가지
};

// struct
struct TableInfo {
    std::string table_name; // no underscore at end
    static Pool<TableInfo>* pool; // static 멤버 변수도 마찬가지
};
```

### Constant Names

constexpr 혹은 const 변수는 *k* 로 시작합니다. 이 후 단어의 첫 글자를 대문자로 하고 나머지는 소문자로 합니다.

```C++
const int kBufferSize = 64;
```

### Function Names

일반 함수들은 [Type names](#type-names) 와 같은 규칙을 따릅니다. 예외로 accessor와 mutator 함수에 대해서는 변수와 같은 이름을 가질 수 있습니다.  

```C++
// 일반 함수
void ReplyRpc();
int64_t CalculateMemoryUsage();

// accessor and mutator
class MyClass {
public:
    // accessor
    int count();
    // mutator
    void set_count(int count);
private:
    int count_;
};
```
