# Java8 – Optional’s Overview with Examples

We can reduce the chance of NullPointerException’s in Java using best practices –

1. Don’t return null’s from methods
2. Don’t pass null’s as arguments

You can also use java.lang.Optional. Java8 Optional formalises the approach used in other languages, and already existed in some Java libraries. Optional is simply an object wrapper.

# Examples

My approach is similar style to my post [Java8 – Streams Cookbook](https://www.javabullets.com/java-8-streams-cookbook/) and have included a class with a number of examples using Optional

```java
import java.util.Optional;
public class OptionalExamples {
    // Populate Bike with Optional<Wheels>
    private static Bike colnagoBike = new Bike(Optional.of(new Wheels("mavic", 32)), "colnago");
    
    // Dont do this - use a Optional.ofNullable
    private static Bike nullBike = new Bike(null, "nowheels");

    // Use a Optional.ofNullable
    private static Bike ofNullableBike = new Bike(Optional.ofNullable(null), "nowheels");
    
    public static void main(String[] args) {
        emptyOptionals();
        nullObjectsInOptionals();
        workingWithValuesInOptionals();
    }
    
    private static void workingWithValuesInOptionals() {
        // Working with Values
	
        // Populated Optional - now begin populating with Object
        Optional<Bike> optionalColnagoBike = Optional.of(colnagoBike);
        if (optionalColnagoBike.isPresent()) {
            System.out.println("isPresent() - ok - but not much improvement on != null");
        }
        
	optionalColnagoBike.ifPresent(
            bike -> System.out.println("ifPresent(Consumer) returns " + bike.getBrand()));
        
	// Filtering and mapping in combination with Lambdas
        Bike orElseThrowBike = optionalColnagoBike.filter(b -> "colnago".equals(b.getBrand())).get();
		
        //
        Optional<String> brand = optionalColnagoBike.map(b -> b.getBrand());
        brand.ifPresent(b -> System.out.println("brand " + b));
        
	// flatMap to prevent Optional<Optional<Wheels>>
        Optional<Wheels> wheels = optionalColnagoBike.flatMap(b -> b.getWheels());
        wheels.ifPresent(w -> System.out.println("flatMap - Wheel Brand " + w.getBrand()));
    }
    
    private static void nullObjectsInOptionals() {
        // of - Populate with null object - Throws Exception
        try {
            Optional<Bike> optionalNullBike = Optional.of(null);
        } catch (java.lang.NullPointerException nfe) {
            System.out.println("Cant call Optional.of(null) - " + nfe.getMessage());
        }
	
        // ofNullable - allows null
        System.out.println("We can pass a null with ofNullable");
        Optional<Bike> optionalOfNullableBike = Optional.ofNullable(null);
        System.out.println("optionalOfNullableBike.isPresent() returns " + optionalOfNullableBike.isPresent());
    }
    
    private static void emptyOptionals() {
    
        // Empty Optional - empty container with no object
        Optional<Bike> optionalEmptyBike = Optional.empty();
        
	// call get() on empty object throws NoSuchElementException
        try {
            Bike emptyBike = optionalEmptyBike.get();
        } catch (java.util.NoSuchElementException e) {
            System.out.println("get() on empty Optional throws java.util.NoSuchElementException " + e.getMessage());
        }
	
        // isPresent - check if object is empty - but not much advantage over !=
        // null checks
        if (!optionalEmptyBike.isPresent()) {
            System.out.println("isPresent() - ok - but not much improvement on != null");
        }
	
        // Better Alternatives -
        // orElse - returns a default object if none set
        Bike orElseBike = optionalEmptyBike.orElse(colnagoBike);
        System.out.println("orElse - Optional is empty so return colnagoBike " + orElseBike.getBrand());
        
	// ifPresent(Consumer<? extends Bike>) - this prints nothing as Optional
        // is empty
        optionalEmptyBike.ifPresent(bike -> System.out.println("ifPresent(Consumer) returns " + bike.getBrand()));

        // orElseThrow - Throw Exception
        try {
            Bike orElseThrowBike = optionalEmptyBike.orElseThrow(NoBikeException::new);
        } catch (NoBikeException nbe) {
            System.out.println("orElseThrow NoBikeException");
        }
    }
}

class Bike {
    private Optional<Wheels> wheels;
    private String brand;
    
    public Bike(Optional<Wheels> wheels, String brand) {
        this.wheels = wheels;
        this.brand = brand;
    }
    
    public Optional<Wheels> getWheels() {
        return wheels;
    }
    
    public void setWheels(Optional<Wheels> wheels) {
        this.wheels = wheels;
    }
    
    public String getBrand() {
        return brand;
    }
    
    public void setBrand(String brand) {
        this.brand = brand;
    }
}

class Wheels {
    private String brand;
    private int spokes;
    
    public Wheels(String brand, int spokes) {
        this.brand = brand;
        this.spokes = spokes;
    }
    
    public String getBrand() {
        return brand;
    }
    
    public void setBrand(String brand) {
        this.brand = brand;
    }
    
    public int getSpokes() {
        return spokes;
    }
    
    public void setSpokes(int spokes) {
        this.spokes = spokes;
    }
}
class NoBikeException extends Exception {
	private static final long serialVersionUID = 1L;
}```


[`techio.yml`](https://github.com/TechDotIO/techio-basic-template/blob/master/techio.yml)  
This *mandatory* file describes both the table of content and the programming project(s). The file path should not be changed.
