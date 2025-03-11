---
layout: single

title: "[Design Pattern] 템플릿 메서드 패턴"

categories:
    - DesignPattern
tag: [디자인 패턴]

date: 2025-03-11
last_modified_at: 2025-03-11

order : 1030
---

# 템플릿 메서드 패턴

템플릿 메서드(Template Method) 패턴은 객체지향 설계 원칙 중 헐리우드 원칙(Hollywood Principle)을 따르는 패턴으로, 부모 클래스(추상 클래스)에서 알고리즘의 뼈대(골격)를 정의하고, 구체적인 세부 구현은 자식 클래스(구현 크래스)에 맡기는 패턴입니다.

부모 클래스에서 전체 알고리즘의 흐름(순서나 구조)을 고정하되, 일부 단계를 추상(abstract) 메서드 또는 가상(virtual) 메서드로 만들어서 자식 클래스들이 자유롭게 오버라이드(재정의) 할 수 있도록 해줍니다.  
이를 통해, 공통된 알고리즘 구조는 한곳(부모 클래스)에 모아두고 세부 구현(단계별 로직)은 자식 클래스에서만 변경할 수 있어 코드 중복을 줄이고, 동시에 알고리즘 흐름은 안정적으로 유지할 수 있게 됩니다.

템플릿 메서드는 상속(Inheritance)을 기반으로 한 행동 디자인 패턴입니다.  
반면, 전략(Strategy) 패턴이나 상태(State) 패턴은 합성(Composition)을 기반으로 합니다.

알고리즘 단계가 이미 정해져 있으되, 일부 단계를 자식 클래스가 커스터마이징 할 때 상속을 통해 변형을 제공합니다.

컴파일 타임에 알고리즘 골격을 공유하면서 자식이 단계별 구현을 다르게 가져가는 형태입니다.

템플릿 메서드의 구조는 다음과 같습니다.

1. 추상 클래스(인터페이스)로 템플릿 메서드(알고리즘 흐름)를 정의합니다.
    + 필요한 단계(메서드)들을 추상 메서드 또는 기본 구현(가상 메서드) 형태로 선언합니다.
2. 템플릿 메서드에서 알고리즘 수행에 필요한 단계를 순서대로 호출합니다.
    + `final`키워드를 사용해 자식이 재정의하지 못하게 고정하기도 합니다.
3. 구체 자식 클래스에서 부모의 추상 메서드를 재정의하여 필요한 부분을 구현합니다.

+ 장점
    + 알고리즘의 전체적인 구조는 변경하지 않고, 일부 동작만 변경이 가능하므로 중복되는 코드가 줄어들어 코드 재사용성이 증가합니다.
    + 알고리즘 흐름을 한 곳에서 관리할 수 있어, 변경이 필요할 때 일관성을 유지하기 좋아 유지보수에 용이합니다.
    + 하위 클래스에서 필요한 동작만 변경할 수 있어, 새로운 기능을 추가하기 쉽기 때문에 확장성이 높습니다.

+ 단점
    + 상속을 기반으로 하기 때문에, 하위 클래스가 많아질수록 관리하기가 어려워질 수 있습니다.
    + 알고리즘을 지나치게 많은 단위로 쪼개면, 부모 클래스에 메서드가 너무 많아져서 관리가 어려울 수 있습니다.
    + 알고리즘의 흐름이 고정되기 때문에 큰 구조 변경이 필요할 경우 다른 패턴을 고려해야됩니다.
    + 기본 구현을 무시하고 자식이 완전히 다른 행동을 할 경우, 부모 타입으로 다룰 때 부모가 예상한 로직과 다른 결과로인해 어색한 상황이 생길 수 있습니다. (리스코프 치환 원칙 위반 위험)

# 예시

문서 형식별로 데이터를 추출해야 하는 상황을 예시로 들어보겠습니다.

DOC, PDF, CSV 등 서로 다른 형식의 문서가 들어옵니다.  
문서 열기/닫기, 에러 처리, 공통 로그 남기기 등은 대부분 동일합니다.  
단, 실제 데이터를 파싱하는 로직은 문서 형식마다 다르게 구현해야 합니다.

```cpp
// 추상 클래스

class BaseDocumentParser
{
public:
	void ParseDocument(std::string filePath)
	{
		OpenFile(filePath);     // 1. 파일 열기
		ParseData();            // 2. 실제 데이터 파싱 (추상 메서드)
		AnalyzeData();          // 3. 데이터 분석 (기본 구현 or 가상 메서드)
		CloseFile();            // 4. 파일 닫기
	}

protected:
    // 공통 구현 (필요하다면 virtual로 오버라이딩 가능)
    virtual void OpenFile(std::string filePath)
    {
        std::cout << "[공통] 파일 " << filePath << " 열기" << std::endl;;
    }

    // 추상 메서드 - 구체 파서마다 구현해야 함
    virtual void ParseData() = 0;

    // 기본 구현(가상 메서드) - 필요한 경우 자식에서 재정의 가능
    virtual void AnalyzeData()
    {
        std::cout << "[공통] 파싱된 데이터 분석" << std::endl;
    }

    // 공통 구현
    virtual void CloseFile()
    {
        std::cout << "[공통] 파일 닫기" << std::endl;
    }
};
```

`ParseDocument`함수가 템플릿 메서드 역할을 합니다.

`ParseData`함수는 각 형식마다 달라지므로 추상 메서드로 선언했습니다.

`OpenFile`, `CloseFile`, `AnalyzeData`함수들은 기본 구현(또는 공통 코드)을 제공합니다.

```cpp
// 구체 자식 클래스
class DocParser : public BaseDocumentParser
{
protected:
    void ParseData() override
    {
        std::cout << "[DOC 파서] DOC 형식으로 데이터를 파싱합니다." << std::endl;
        // DOC 파일 특유의 파싱 로직
    }
};

class PdfParser : BaseDocumentParser
{
protected:
    void ParseData() override
    {
        std::cout << "[PDF 파서] PDF 형식으로 데이터를 파싱합니다." << std::endl;
    }

    void AnalyzeData() override
    {
        std::cout << "[PDF 파서] PDF 전용 고급 분석 로직 수행" << std::endl;
    }
};

class CsvParser : BaseDocumentParser
{
protected:
    void ParseData() override
    {
        std::cout << "[CSV 파서] CSV 형식으로 데이터를 파싱합니다." << std::endl;
    }
};
```

DOC 파서는 기본 분석 로직만 사용하고, `ParseData`함수 부분만 구현했습니다.  
PDF 파서는 분석 과정도 다를 수 있어 `AnalyzeData`함수를 재정의했습니다.  
CSV 파서는 기본 분석이 충분하므로, `ParseData`함수만 구현하고 나머지는 기본 구현을 사용합니다.

```cpp
// 사용 예시
int main()
{
    BaseDocumentParser* parser;

    parser = new DocParser;
    parser->ParseDocument("sample.doc");
    delete parser;

    std::cout << std::endl;

    parser = new PdfParser;
    parser->ParseDocument("sample.pdf");
    delete parser;

    std::cout << std::endl;

    parser = new CsvParser;
    parser->ParseDocument("sample.csv");
    delete parser;
}
```

`ParseDocument`함수 하나의 메서드로 전체 로직을 호출합니다.  
그 안에서 `OpenFile`, `ParseData`, `AnalyzeData`, `CloseFile`함수가 순서대로 실행됩니다.  

파일 형식만 교체하면(= 다른 파서 객체 사용), 로직 순서는 동일하지만 구현이 달라집니다.

실행 결과는 다음과 같습니다.

```
[공통] 파일 sample.doc 열기
[DOC 파서] DOC 형식으로 데이터를 파싱합니다.
[공통] 파싱된 데이터 분석
[공통] 파일 닫기

[공통] 파일 sample.pdf 열기
[PDF 파서] PDF 형식으로 데이터를 파싱합니다.
[PDF 파서] PDF 전용 고급 분석 로직 수행
[공통] 파일 닫기

[공통] 파일 sample.csv 열기
[CSV 파서] CSV 형식으로 데이터를 파싱합니다.
[공통] 파싱된 데이터 분석
[공통] 파일 닫기
```