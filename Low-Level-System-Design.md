# Low-Level-System-Design (Under Construction)

## Object Oriented Programming

- Object Oriented Programming is a programming paradigm in which low level design of a software revolves around objects and classes.
- Object is anything that is state and behaviour.In OOP's, object maintains state through properies and exposes its behaviour through methods.
- A class is a blueprint for creating an object.
- An Interface is a blueprint for creating a class and used to achieve loose coupling and total abstraction.

## 4 Pillars of OOPS

## **Encapsulation**
-  Hiding internal state of an object by requiring all interactions with object to be done through publically exposed methods only.

![](https://firebasestorage.googleapis.com/v0/b/boom-b9a18.appspot.com/o/LLD%2Fa1.png?alt=media&token=1bab9e2c-7a75-4f11-9d19-6a4b12a59d33)

## **Inheritence**
- A way to organise interrelated classes by making a hierarchchial structure by making subclasses extend the methods and properties of superclasses.
- `Types of Inheritence`:
  - Single Inheritence
  - Multilevel Inheritence
  - Hierarchial Inheritence
  - Multiple Inheritence `Interface Only`
  - Hybrid Inheritence `Interface Only`

*PS: Multiple/Hybrid Inheritence is not supported in java classes because of ambiguity.*

![](https://firebasestorage.googleapis.com/v0/b/boom-b9a18.appspot.com/o/LLD%2Fa2.png?alt=media&token=af4e2005-e3e7-496f-8a4b-eea2826153cd)

## **Polymorphism**

![](https://firebasestorage.googleapis.com/v0/b/boom-b9a18.appspot.com/o/LLD%2Fa3.png?alt=media&token=0a18126f-35c9-44d0-bf35-074e323e9413)

## **Abstraction**

- Abstraction is providing abstract view of functionality of class by hiding method implementation and providing only method defination.
- It is implemented using Abstract classes and Interfaces.

![](https://firebasestorage.googleapis.com/v0/b/boom-b9a18.appspot.com/o/LLD%2Fa4.png?alt=media&token=e307c94b-56c8-40b8-87cf-fbe8cd3d6b75)

---

## Binding in java

- Binding refers to the process of converting identifiers (such as variable and performance names) into addresses.
- For functions, it means that matching the call with the right function definition by the compiler
- It takes place either at compile time or at runtime.

![](https://firebasestorage.googleapis.com/v0/b/boom-b9a18.appspot.com/o/LLD%2Fa5.png?alt=media&token=8ae8e2aa-d887-49f9-a3ce-5a9aa9fb2dea)

### Micellaneous

- [Link 1](https://firebasestorage.googleapis.com/v0/b/boom-b9a18.appspot.com/o/LLD%2Fl1.png?alt=media&token=01b6dd18-cd7b-4356-95c3-311d4ee691b9)
- [Link 2](https://firebasestorage.googleapis.com/v0/b/boom-b9a18.appspot.com/o/LLD%2Fl2.png?alt=media&token=c98152fb-d93c-4ac2-974b-ed480496f05f)
- [Link 3](https://firebasestorage.googleapis.com/v0/b/boom-b9a18.appspot.com/o/LLD%2Fl3.png?alt=media&token=d9cc0cff-eda1-4f43-8cd5-c17822ed7df1)

# Design Patterns

## State Machine

```java
// Define the CharacterState interface
interface CharacterState {
    void handleEvent(GameCharacter character, CharacterEvent event);
}

// Define the GameCharacter class
class GameCharacter {
    private CharacterState currentState;

    public GameCharacter(CharacterState initialState) {
        this.currentState = initialState;
    }

    public void setCurrentState(CharacterState state) {
        this.currentState = state;
    }

    public void handleEvent(CharacterEvent event) {
        currentState.handleEvent(this, event);
    }
}

// Define the CharacterEvent enum
enum CharacterEvent {
    START_WALKING,
    STOP_WALKING,
    START_ATTACKING,
    STOP_ATTACKING
}

// Define the IdleState class
class IdleState implements CharacterState {
    @Override
    public void handleEvent(GameCharacter character, CharacterEvent event) {
        switch (event) {
            case START_WALKING:
                character.setCurrentState(new WalkingState());
                System.out.println("Character starts walking");
                break;
            case START_ATTACKING:
                character.setCurrentState(new AttackingState());
                System.out.println("Character starts attacking");
                break;
            default:
                // No transition for other events in the IDLE state
                break;
        }
    }
}

// Define the WalkingState class
class WalkingState implements CharacterState {
    @Override
    public void handleEvent(GameCharacter character, CharacterEvent event) {
        switch (event) {
            case STOP_WALKING:
                character.setCurrentState(new IdleState());
                System.out.println("Character stops walking");
                break;
            case START_ATTACKING:
                character.setCurrentState(new AttackingState());
                System.out.println("Character starts attacking while walking");
                break;
            default:
                // No transition for other events in the WALKING state
                break;
        }
    }
}

// Define the AttackingState class
class AttackingState implements CharacterState {
    @Override
    public void handleEvent(GameCharacter character, CharacterEvent event) {
        switch (event) {
            case STOP_ATTACKING:
                character.setCurrentState(new IdleState());
                System.out.println("Character stops attacking");
                break;
            case STOP_WALKING:
                character.setCurrentState(new IdleState());
                System.out.println("Character stops walking and attacks");
                break;
            default:
                // No transition for other events in the ATTACKING state
                break;
        }
    }
}

// Main class to demonstrate the character state machine
public class CharacterStateMachineDemo {
    public static void main(String[] args) {
        // Create a game character with an initial state (Idle)
        GameCharacter character = new GameCharacter(new IdleState());

        // Simulate events
        character.handleEvent(CharacterEvent.START_WALKING); // Character starts walking
        character.handleEvent(CharacterEvent.STOP_WALKING);  // Character stops walking
        character.handleEvent(CharacterEvent.START_ATTACKING); // Character starts attacking
        character.handleEvent(CharacterEvent.STOP_ATTACKING);  // Character stops attacking
        character.handleEvent(CharacterEvent.STOP_WALKING);   // Character stops walking and attacks
    }
}

```
