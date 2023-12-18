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
// Define the VendingMachineState interface
interface VendingMachineState {
    void handleEvent(VendingMachine vendingMachine, VendingMachineEvent event);
}

// Define the VendingMachineEvent enum
enum VendingMachineEvent {
    INSERT_COIN,
    SELECT_ITEM,
    DISPENSE_ITEM,
    CANCEL
}

// Define the VendingMachine class
class VendingMachine {
    private VendingMachineState currentState;

    public VendingMachine(VendingMachineState initialState) {
        this.currentState = initialState;
    }

    public void setCurrentState(VendingMachineState state) {
        this.currentState = state;
    }

    public void handleEvent(VendingMachineEvent event) {
        currentState.handleEvent(this, event);
    }
}

// Define the IdleState class
class IdleState implements VendingMachineState {
    @Override
    public void handleEvent(VendingMachine vendingMachine, VendingMachineEvent event) {
        switch (event) {
            case INSERT_COIN:
                vendingMachine.setCurrentState(new AcceptingCoinsState());
                System.out.println("Coins accepted. Select an item.");
                break;
            case CANCEL:
                System.out.println("No action in Idle state.");
                break;
            default:
                // No transition for other events in the IDLE state
                break;
        }
    }
}

// Define the AcceptingCoinsState class
class AcceptingCoinsState implements VendingMachineState {
    @Override
    public void handleEvent(VendingMachine vendingMachine, VendingMachineEvent event) {
        switch (event) {
            case SELECT_ITEM:
                vendingMachine.setCurrentState(new SelectingItemState());
                System.out.println("Item selected. Confirm to dispense.");
                break;
            case CANCEL:
                vendingMachine.setCurrentState(new IdleState());
                System.out.println("Transaction canceled. Insert coins to start again.");
                break;
            default:
                // No transition for other events in the ACCEPTING_COINS state
                break;
        }
    }
}

// Define the SelectingItemState class
class SelectingItemState implements VendingMachineState {
    @Override
    public void handleEvent(VendingMachine vendingMachine, VendingMachineEvent event) {
        switch (event) {
            case DISPENSE_ITEM:
                vendingMachine.setCurrentState(new DispensingItemState());
                System.out.println("Item dispensed. Thank you!");
                break;
            case CANCEL:
                vendingMachine.setCurrentState(new IdleState());
                System.out.println("Transaction canceled. Insert coins to start again.");
                break;
            default:
                // No transition for other events in the SELECTING_ITEM state
                break;
        }
    }
}

// Define the DispensingItemState class
class DispensingItemState implements VendingMachineState {
    @Override
    public void handleEvent(VendingMachine vendingMachine, VendingMachineEvent event) {
        switch (event) {
            case CANCEL:
                System.out.println("Cannot cancel. Item dispensing in progress.");
                break;
            default:
                // No transition for other events in the DISPENSING_ITEM state
                break;
        }
    }
}

// Main class to demonstrate the vending machine state machine
public class VendingMachineStateMachineDemo {
    public static void main(String[] args) {
        // Create a vending machine with an initial state (Idle)
        VendingMachine vendingMachine = new VendingMachine(new IdleState());

        // Simulate events
        vendingMachine.handleEvent(VendingMachineEvent.INSERT_COIN); // Coins accepted. Select an item.
        vendingMachine.handleEvent(VendingMachineEvent.CANCEL);      // No action in Idle state.
        vendingMachine.handleEvent(VendingMachineEvent.SELECT_ITEM);  // Item selected. Confirm to dispense.
        vendingMachine.handleEvent(VendingMachineEvent.DISPENSE_ITEM); // Item dispensed. Thank you!
    }
}

```

## Observer

```java
import java.util.ArrayList;
import java.util.List;

// Subject representing a messaging service
class MessagingService {
    private List<MessageObserver> messageObservers = new ArrayList<>();

    public void addObserver(MessageObserver observer) {
        messageObservers.add(observer);
    }

    public void removeObserver(MessageObserver observer) {
        messageObservers.remove(observer);
    }

    // Simulating receiving a new message
    public void receiveMessage(String sender, String message) {
        System.out.println("Received message from " + sender + ": " + message);
        notifyObservers(sender, message);
    }

    private void notifyObservers(String sender, String message) {
        for (MessageObserver observer : messageObservers) {
            observer.onMessageReceived(sender, message);
        }
    }
}

// Observer interface
interface MessageObserver {
    void onMessageReceived(String sender, String message);
}

// Concrete observer representing a user interface
class UIUpdater implements MessageObserver {
    @Override
    public void onMessageReceived(String sender, String message) {
        System.out.println("UI Updated: New message from " + sender + " - " + message);
        // Logic to update the user interface with the new message
    }
}

// Concrete observer representing a notification system
class NotificationSystem implements MessageObserver {
    @Override
    public void onMessageReceived(String sender, String message) {
        System.out.println("Notification: New message from " + sender);
        // Logic to send a notification about the new message
    }
}

public class MessagingAppExample {
    public static void main(String[] args) {
        MessagingService messagingService = new MessagingService();

        UIUpdater uiUpdater = new UIUpdater();
        NotificationSystem notificationSystem = new NotificationSystem();

        messagingService.addObserver(uiUpdater);
        messagingService.addObserver(notificationSystem);

        messagingService.receiveMessage("John", "Hello there!");

        messagingService.removeObserver(uiUpdater);

        messagingService.receiveMessage("Alice", "How are you?");
    }
}

```
