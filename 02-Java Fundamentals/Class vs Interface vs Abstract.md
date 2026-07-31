
### Class 
This a concrete blueprint, with variables and methods full defined and implemented and we can initialize objects from it. 

Eg - 

```
// A concrete class - we can build a physical Sedan right now.
public class Sedan {
    int speed;

    public void startEngine() {
        System.out.println("Vroom! Engine started.");
    }
    
    public void drive() {
        System.out.println("Driving on four wheels.");
    }
}
// In your main method:
Sedan myCar = new Sedan(); // Perfectly fine! We built a car.
```

### Abstract
These are used when we have a no of child classes with some common methods and few methods differs for them then we build an abstract class 
It contains variable and methods, some methods are implemented and some are left blank so the child classes can implement them
We cannot create object out of this abstract classes, they are purely inherited by the child classes.

Eg- 

```
public abstract class MotorVehicle {
    int fuelLevel;

    // Concrete method: Code is shared with all children
    public void startEngine() {
        System.out.println("Fuel injected, spark plug fired.");
    }

    // Abstract method: No body! Children MUST provide the implementation
    public abstract void drive(); 
}

public class Truck extends MotorVehicle {
    // We get startEngine() for free!
    
    // We MUST implement drive() because the parent left it blank
    @Override
    public void drive() {
        System.out.println("Driving heavily with 18 wheels.");
    }
}

// In your main method:
// MotorVehicle mv = new MotorVehicle(); // ERROR! You can't just build a "generic motor vehicle".
Truck myTruck = new Truck(); // This works.
```

### Interface 
It's an design type of class where there are just empty methods and whosoever implements them they need to implement the methods declared in the interface
We cannot create objects out of it as well, and one class implement multiple interfaces

Eg - 

```
// The Contract
public interface Trackable {
    // By default, interface methods are public and abstract
    void getCoordinates();
}

// A Drone has nothing to do with a MotorVehicle, but it CAN be Trackable
public class DeliveryDrone implements Trackable {
    @Override
    public void getCoordinates() {
        System.out.println("GPS: 40.7128 N, 74.0060 W (In the sky)");
    }
}

// Our Truck from earlier can extend its parent AND sign the contract!
public class Truck extends MotorVehicle implements Trackable {
    @Override
    public void drive() {
        System.out.println("Driving heavily.");
    }

    @Override
    public void getCoordinates() {
        System.out.println("GPS: 40.7128 N, 74.0060 W (On the road)");
    }
}
```

## Doubts & Clearances 

Can a single class extend multiple abstract or class - > No, a single class can only extend one class
Can multiple class extend the same class or abstract class -> Yes
Can a single class implement multiple interface - > Yes we can implement multiple interface
Is it mandatory to call all the methods from a extended class -> No, one who are already implemented in the extended class we can call them whenever needed but who all are empty we need to implement them.

