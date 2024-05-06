---
layout: single

title: "C++ 표준 템플릿 라이브러리(STL) 개요"

categories:
    - CppSTL
tag: [Cpp, CppSTL]

date: 2024-05-07
last_modified_at: 2024-05-07
---

# C++ 표준 템플릿 라이브러리(STL) 개요

표준 템플릿 라이브러리(STL: Standard Template Libray)는 여러 자료 구조, 함수, 알고리즘 등을 사용하기 쉽게 정형화해서 라이브러리화 해둔 것입니다.  
엄밀하게 말하면 이미 만들어진 템플릿을 이용하기 위해 불러와서 사용하는 것은 C++ Standard Library라고 부르는 것이 맞지만 STL이라고 불러왔기 때문에 굳혀져 지금도 STL이라고 부르고 있습니다.

## 표준 라이브러리 구성

C++ 표준 라이브러리의 인터페이스는 다음과 같은 헤더로 정의돼 있습니다.

연결되어있는 링크는 해당 헤더를 공부한 후 블로그에 정리한 내용으로 연결됩니다.

### Concepts library

+ \<concepts>(C++20) Fundamental library concepts

### Coroutines library

+ \<coroutine>(C++20) Coroutine support library

### Utillities library

+ \<any>(C++17) std::any 클래스
+ \<bitset> std::bitset 클래스 템플릿
+ \<chrono> (C++11) C++ 시간 유틸리티
+ \<compare> (C++20) Three-way 비교 연산자 지원
+ \<csetjmp> 실행 컨텍스트에 저장(및 점프)하는 매크로(및 함수)
+ \<csignal> 시그널 관리를 위한 함수 및 매크로 상수
+ \<cstdarg> 가변 길이 인수 목록 처리
+ \<cstddef> 표준 매크로 및 typedef
+ \<cstdlib> 범용 유틸리티: 프로그램 제어, 동적 메모리 할당, 난수, 정렬 및 검색
+ \<ctime> C 스타일 시간/날짜 유틸리티
+ \<debugging> (C++26) 디버깅 라이브러리
+ \<expected> (C++23) std::expected 클래스 템플릿
+ \<functional> 함수 객체, 함수 호출, 바인딩 작업 및 참조 래퍼
+ \<initializer_list> (C++11) std::initializer_list 클래스 템플릿
+ \<optional> (C++17) std::optional 클래스 템플릿
+ \<source_location> (C++20) 소스 코드 위치를 얻는 수단을 제공
+ \<tuple> (C++11) std::tuple 클래스 템플릿
+ \<type_traits> (C++11) 컴파일 타임 자료형 정보
+ \<typeindex> (C++11) std::type_index
+ \<typeinfo> 런타임 자료형 정보 유틸리티
+ \<utility> 각종 유틸리티 구성요소
+ \<variant> (C++17) std::variant 클래스 템플릿
+ \<version> (C++20) 구현에 따른 라이브러리 정보를 제공

#### Dynamic memory management

+ \<memory> 고수준 메모리 관리 유틸리티
+ \<memory_resource> (C++17) 다형성 할당자와 메모리 리소스
+ \<new> 저수준 메모리 관리 유틸리티
+ \<scoped_allocator> (C++11) 중첩 할당자 클래스

#### Numeric limits

+ \<cfloat> float 자료형의 제한
+ \<cinttypes> (C++11) 서식 지정 매크로, intmax_t 및 uintmax_t 수학 및 변환
+ \<climits> 정수 계열 자료형의 제한
+ \<cstdint> (C++11) 고정 크기 자료형 및 기타 자료형의 제한
+ \<limits> 산술 자료형의 속성을 쿼리하는 표준화된 방법
+ \<stdfloat> (C++23) 고정 크기 float 자료형

#### Error handling

+ \<cassert> 인수를 0과 비교하는 조건부 컴파일 매크로
+ \<cerrno> 마지막 오류 번호가 포함된 매크로
+ \<exception> 예외 처리 유틸리티
+ \<stacktrace> (C++23) Stacktrace 라이브러리
+ \<stdexcept> 표준 예외 객체
+ \<system_error> (C++11) 플랫폼 종속 오류 코드인 std::error_code를 정의

### Strings library

+ \<cctype> 문자 데이터에 포함된 자료형을 결정하는 함수
+ \<charconv> (C++17) std::to_chars 와 std::from_chars
+ \<cstring> 다양한 문자열 처리 함수
+ \<cuchar> (C++11) C 스타일 유니코드 문자 변환 함수
+ \<cwchar> 다양한 와이드 및 멀티바이트 문자열 처리 함수
+ \<cwctype> 와이드 문자 데이터에 포함된 자료형을 결정하는 함수
+ \<format> (C++20) std::format을 포함한 형식 지정 라이브러리
+ \<string> std::basic_string 클래스 템플릿
+ \<string_view> (C++17) std::basic_string_view 클래스 템플릿

### Containers library

자료를 저장하는 자료구조들의 클래스 템플릿입니다.

STL 컨테이너들은 기본(base) 클래스로 사용되기 위한 것이 아닙니다.  
소멸자들은 의도적으로 non-virtual입니다.

+ \<array> (C++11) std::array 컨테이너
+ \<deque> std::deque 컨테이너
+ \<flat_map> (C++23) std::flat_map과 std::flat_multimap 컨테이너 어댑터
+ \<flat_set> (C++23) std::flat_set과 std::flat_multiset 컨테이너 어댑터
+ \<forward_list> (C++11) std::forward_list 컨테이너
+ \<list> std::list 컨테이너
+ \<map> std::map과 std::multimap 연관 컨테이너
+ \<mdspan> (C++23) std::mdspan 뷰 
+ \<queue> std::queue 와 std::priority_queue 컨테이너 어댑터
+ \<set> std::set과 std::multiset 연관 컨테이너
+ \<span> (C++20) std::span 뷰
+ \<stack> std::stack 컨테이너 어댑터
+ \<unordered_map> (C++11) std::unordered_map과 std::unordered_multimap 비정렬 연관 켄테이너
+ \<unordered_set> (C++11) std::unordered_set과 std::unordered_multiset 비정렬 연관 컨테이너
+ \<vector> std::vector 컨테이너

### Iterators library

컨테이너 원소를 접근 또는 요소간 이용하는 용도로 사용됩니다.

반복자로 번역하기도 하지만 단어 의미와 맞지 않은 경우가 있습니다.

+ \<iterator> 범위 이터레이터

### Ranges library

+ \<generator> (C++23) std::generator 클레스 템플릿
+ \<ranges> (C++20) 범위 액세스, 기본 요소, 요구 사항, 유틸리티 및 어댑터

### Algorithms library

정렬, 삭제, 검색등의 템플릿 함수들이 있습니다.

+ \<algorithm> 범위에서 작동하는 알고리즘
+ \<execution> (C++17) 알고리즘의 병렬 버전에 대해 사전 정의된 실행 정책

### Numerics library

일반적인 수학적 함수, 자료형과 최적화된 숫자 배열, 난수 생성 지원이 포함됩니다.

+ \<bit> (C++20) 비트 조작 함수
+ \<cfenv> (C++11) 부동 소수점 환경 액세스 함수
+ \<cmath> 일반적인 수학 함수
+ \<complex> 복소수 자료형
+ \<linalg> (C++26) 기초 선형 대수 알고리즘
+ \<numbers> (C++20) 수학 상수
+ \<numeric> 컨테이너의 값에 대한 숫자 연산
+ \<random> (C++11) 난수 생성기 및 분포
+ \<ratio> (C++11) 컴파일 시간 유리수 산술
+ \<valarray> 값 배열의 표현과 조작을 위한 클래스

### Localization library

+ \<clocale> C 지역화 유틸리티
+ \<codecvt> (C++11) (deprecated in C++17) (removed in C++26) 유니코드 변환 기능
+ \<locale> 현지화 유틸리티
+ \<text_encoding> (C++26) 텍스트 인코딩 식별

### Input/output library

+ \<cstdio> C 스타일 입출력 함수
+ \<fstream> std::basic_fstream, std::basic_ifstream, std::basic_ofstream 클래스 템플릿 및 여러 typedef
+ \<iomanip> 입력 및 출력 형식을 제어하는 헬퍼 함수
+ \<ios> std::ios_base 클래스, std::basic_ios 클래스 템플릿 및 여러 typedef
+ \<iosfwd> 입력/출력 라이브러리에 있는 모든 클래스의 전방 선언
+ \<iostream> 여러 표준 스트림 객체
+ \<istream> std::basic_istream 클래스 템플릿 및 여러 typedef
+ \<ostream> std::basic_ostream, std::basic_iostream 클래스 템플릿 및 여러 typedef
+ \<print> (C++23) std::print를 포함한 형식 지정된 출력 라이브러리
+ \<spanstream> (C++23) std::basic_spanstream, std::basic_ispanstream, std::basic_ospanstream 클래스 템플릿 및 typedefs
+ \<sstream> std::basic_stringstream, std::basic_istringstream, std::basic_ostringstream 클래스 템플릿 및 여러 typedef
+ \<streambuf> std::basic_streambuf 클래스 템플릿
+ \<strstream> (deprecated in C++98) (removed in C++26) std::strstream, std::istrstream, std::ostrstream
+ \<syncstream> (C++20) std::basic_osyncstream, std::basic_syncbuf, 및 typedefs

### Filesystem library

+ \<filesystem> (C++17) std::path 클래스 및 지원 함수

### Regular Expressions library

+ \<regex> (C++11) 정규표현식 처리를 지원하는 클래스, 알고리즘 및 이터레이터

### Atomic Operations library

+ \<atomic> (C++11) Atomic 연산 라이브러리

### Thread support library

+ \<barrier> (C++20) Barriers
+ \<condition_variable> (C++11) Thread waiting conditions
+ \<future> (C++11) Primitives for asynchronous computations
+ \<hazard_pointer> (C++26) Hazard pointers
+ \<latch> (C++20) Latches
+ \<mutex> (C++11) 상호 배제 프리미티브
+ \<rcu> (C++26) 읽기-복사 업데이트 매커니즘 / 동기화 매커니즘
+ \<semaphore> (C++20) 세마포어들
+ \<shared_mutex> (C++14) 공유 상호 배제 프리미티브
+ \<stop_token> (C++20) std::jthread에 대한 중지 토큰
+ \<thread> (C++11) std::thread 클래스 및 지원 함수

### C compatibility headers

xxx.h 형식의 일부 C 표준 라이브러리 헤더의 경우 C++ 표준 라이브러리에는 동일한 이름의 헤더와 다른 형식의 헤더(cxxxx.h)가 포함되어 있습니다.

\<complex.h>를 제외하고 C++ 표준 라이브러리에 포함 된 xxx.h 형식의 C 표준 라이브러리의 헤더는 전역 네임 스페이스입니다.

cxxx 헤더가 std 네임 스페이스에 각 이름을 배치합니다.  
예를 들어 \<cstdlib>를 포함하면 std::malloc과 ::malloc 둘 다 사용할 수 있고, \<stdlib.h>를 포함하면 ::malloc만 사용할 수 있습니다.

+ \<assert.h> Behaves same as \<cassert>
+ \<ctype.h> Behaves as if each name from \<cctype> is placed in global namespace
+ \<errno.h> Behaves same as \<cerrno>
+ \<fenv.h> (C++11) Behaves as if each name from \<cfenv> is placed in global namespace
+ \<float.h> Behaves same as \<cfloat>
+ \<inttypes.h> (C++11) Behaves as if each name from \<cinttypes> is placed in global namespace
+ \<limits.h> Behaves same as \<climits>
+ \<locale.h> Behaves as if each name from \<clocale> is placed in global namespace
+ \<math.h> Behaves as if each name from \<cmath> is placed in global namespace,
except for names of mathematical special functions
+ \<setjmp.h> Behaves as if each name from \<csetjmp> is placed in global namespace
+ \<signal.h> Behaves as if each name from \<csignal> is placed in global namespace
+ \<stdarg.h> Behaves as if each name from \<cstdarg> is placed in global namespace
+ \<stddef.h> Behaves as if each name from \<cstddef> is placed in global namespace,
except for names of std::byte and related functions
+ \<stdint.h> (C++11) Behaves as if each name from \<cstdint> is placed in global namespace
+ \<stdio.h> Behaves as if each name from \<cstdio> is placed in global namespace
+ \<stdlib.h> Behaves as if each name from \<cstdlib> is placed in global namespace
+ \<string.h> Behaves as if each name from \<cstring> is placed in global namespace
+ \<time.h> Behaves as if each name from \<ctime> is placed in global namespace
+ \<uchar.h> (C++11) Behaves as if each name from \<cuchar> is placed in global namespace
+ \<wchar.h> Behaves as if each name from \<cwchar> is placed in global namespace
+ \<wctype.h> Behaves as if each name from \<cwctype> is placed in global namespace

### Empty C headers

\<complex.h>, \<ccomplex>, \<tgmath.h>, \<ctgmath>는 C 표준 라이브러리에서 어떤 내용도 포함하고 있지 않으며, 대신 C++ 표준 라이브러리의 다른 헤더를 포함합니다.

+ \<ccomplex> (C++11) (deprecated in C++17) (removed in C++20) Simply includes the header \<complex>
+ \<complex.h> (C++11) Simply includes the header \<complex>
+ \<ctgmath> (C++11) (deprecated in C++17) (removed in C++20) Simply includes the headers \<complex> and \<cmath>: the overloads equivalent to the contents of the C header tgmath.h are already provided by those headers
+ \<tgmath.h> (C++11) Simply includes the headers \<complex> and \<cmath>

### Meaningless C headers

\<ciso646>, \<cstdalign>, \<cstdbool>는 헤더들에서 제공하는 매크로들이 C++에서 언어 키워드로 이미 존재하기 때문에 의미가 없습니다.

+ \<ciso646> (removed in C++20) Empty header. The macros that appear in iso646.h in C are keywords in C++
+ \<cstdalign> (C++11) (deprecated in C++17) (removed in C++20) Defines one compatibility macro constant
+ \<cstdbool> (C++11) (deprecated in C++17) (removed in C++20) Defines one compatibility macro constant
+ \<iso646.h> Has no effect
+ \<stdalign.h> (C++11) Defines one compatibility macro constant
+ \<stdbool.h> (C++11) Defines one compatibility macro constant

### Unsupported C headers

C 헤더인 \<stdatomic.h>(C++23 이전), \<stdnoreturn.h>, \<threads.h>는 C++에 포함되지 않으며 cxxx에 해당하지 않습니다.

참조  
https://en.cppreference.com/w/