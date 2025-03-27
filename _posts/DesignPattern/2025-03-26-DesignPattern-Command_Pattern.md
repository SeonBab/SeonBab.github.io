---
layout: single

title: "[Design Pattern] 커맨드 패턴(Command Pattern)"

categories:
    - DesignPattern
tag: [디자인 패턴]

date: 2025-03-26
last_modified_at: 2025-03-26

order : 1090
---

# 커맨드 패턴

커맨드 패턴(Command Pattern)은 요청(명령)을 객체로 캡슐화하여, 요청을 보내는 객체와 처리하는 객체를 서로 분리시키는 디자인 패턴입니다.  
실행할 명령을 매개변수화하고, 요청을 큐에 저장하여 실행을 나중으로 미루거나, 로그로 남길 수 있으며, 실행 취소(undo) 기능을 제공합니다.

명령의 실행, 취소, 재실행, 기록과 같은 특수한 관리가 필요한 경우 사용하는 것이 좋습니다.

+ 핵심 개념
    + 명령(커맨드, Command) 인터페이스: 실행될 명령의 인터페이스를 정의합니다.
    + 콘크리트 커맨드(Concrete Command): 특정 명령을 구현합니다.
    + 인보커(Invoker): 명령을 실행하는 객체로, 요청을 큐에 저장하거나 실행합니다.
    + 리시버(Receiver): 실제 로직을 가지고, 동작을 수행하는 객체입니다.
    + 클라이언트(Client): 어떤 명령을 실행할지 생성하여 인보커에 전달하는 객체입니다.

+ 장점
    - 실행을 요청하는 객체와 실행하는 객체를 분리해 독립적인 객체로 만들어 캡슐화하고, 코드의 결합도를 낮출 수 있습니다.
    - 실행된 명령을 저장하고, 실행 취소 기능을 통해 실행을 취소할 수 있습니다.
    - 새로운 명령을 추가해도 기존 코드에 영향을 주지 않습니다. (OCP, Open-Closed Principle)
    - 명령을 큐에 저장하여 나중에 실행 가능하기 때문에 이벤트 큐 또는 멀티스레딩에서 유용하게 활용됩니다.

+ 단점
    - 명령마다 클래스를 만들어야 하므로 클래스의 수가 많아집니다.
    - 단순한 요청을 수행하는 데도 불필요한 객체가 생성되는 등의 오버헤드가 발생합니다.

+ 게임 개발에서의 활용
    - 키 입력마다 명령 객체를 생성해 캐릭터의 행동(공격, 점프, 이동 등)을 명령 객체로 저장하여 실행 취소 기능 구현
    - 이벤트 시스템에서 특정 행동을 큐에 저장하고 순차적으로 실행
    - UI 시스템으로 버튼 클릭 이벤트를 커맨드 객체로 저장하여 실행 취소 및 재실행 기능 구현
    - 작업(명령)을 큐에 저장하고 다른 스레드에서 실행하도록 설계 (멀티스레딩)
    - 플레이어의 행동을 명령 객체로 기록하면, 행동을 다시 재현하거나 되돌리는 기능

## 예시

<details>
<summary><h5 style="display: inline;">1. 게임 캐릭터가 공격, 점프, 방어 등의 동작을 수행하는 예시</h5></summary>
<div markdown="1">

```cpp
#include <iostream>
#include <vector>

// 커맨드 인터페이스 정의
class Command
{
public:
    virtual void execute() = 0;  // 명령 실행
    virtual void undo() = 0;     // 실행 취소
    virtual ~Command() = default;
};

// 리시버 정의 (게임 캐릭터)
// 실제 동작을 수행합니다.
class Character
{
public:
    void attack() { std::cout << "캐릭터가 공격합니다!" << std::endl; }
    void jump() { std::cout << "캐릭터가 점프합니다!" << std::endl; }
    void defend() { std::cout << "캐릭터가 방어합니다!" << std::endl; }
};

// 명령 클래스들(Concrete Command)
class AttackCommand : public Command
{
private:
    Character& character;

public:
    AttackCommand(Character& c) : character(c) {}
    void execute() override { character.attack(); }
    void undo() override { std::cout << "공격을 취소합니다." << std::endl; }
};

class JumpCommand : public Command
{
    Character& character;
public:
    JumpCommand(Character& c) : character(c) {}
    void execute() override { character.jump(); }
    void undo() override { std::cout << "점프를 취소합니다." << std::endl; }
};

class DefendCommand : public Command
{
    Character& character;
public:
    DefendCommand(Character& c) : character(c) {}
    void execute() override { character.defend(); }
    void undo() override { std::cout << "방어를 취소합니다." << std::endl; }
};

// 인보커(Invoker)
class InputHandler
{
    std::vector<Command*> history;  // 실행된 명령 저장 (Undo 기능)
public:
    void executeCommand(Command* command)
    {
        command->execute();
        history.push_back(command);
    }

    void undoLastCommand()
    {
        if (!history.empty())
        {
            Command* lastCommand = history.back();
            lastCommand->undo();
            history.pop_back();
        }
        else
        {
            std::cout << "실행 취소할 명령이 없습니다." << std::endl;
        }
    }
};

// 클라이언트(Client)
int main()
{
    Character myCharacter;  // 게임 캐릭터 생성

    AttackCommand attack(myCharacter);
    JumpCommand jump(myCharacter);
    DefendCommand defend(myCharacter);

    InputHandler inputHandler;

    // 명령 실행
    inputHandler.executeCommand(&attack);
    inputHandler.executeCommand(&jump);
    inputHandler.executeCommand(&defend);

    // 실행 취소 (Undo)
    inputHandler.undoLastCommand();
    inputHandler.undoLastCommand();
}
```

</div>
</details>