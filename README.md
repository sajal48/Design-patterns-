## Adapter Pattern
The Adapter pattern allows objects with incompatible interfaces to work together. It acts as a bridge between two incompatible classes. Here's an example of the Adapter pattern implemented in Java:

```
// Target interface
interface MediaPlayer {
    void play(String audioType, String fileName);
}

// Adaptee interface
interface AdvancedMediaPlayer {
    void playVlc(String fileName);
    void playMp4(String fileName);
}

// Adaptee implementation
class VlcPlayer implements AdvancedMediaPlayer {
    public void playVlc(String fileName) {
        System.out.println("Playing VLC file: " + fileName);
    }

    public void playMp4(String fileName) {
        // Do nothing
    }
}

// Adaptee implementation
class Mp4Player implements AdvancedMediaPlayer {
    public void playVlc(String fileName) {
        // Do nothing
    }

    public void playMp4(String fileName) {
        System.out.println("Playing MP4 file: " + fileName);
    }
}

// Adapter class
class MediaAdapter implements MediaPlayer {
    private AdvancedMediaPlayer player;

    public MediaAdapter(String audioType) {
        if (audioType.equalsIgnoreCase("vlc")) {
            player = new VlcPlayer();
        } else if (audioType.equalsIgnoreCase("mp4")) {
            player = new Mp4Player();
        }
    }

    public void play(String audioType, String fileName) {
        if (audioType.equalsIgnoreCase("vlc")) {
            player.playVlc(fileName);
        } else if (audioType.equalsIgnoreCase("mp4")) {
            player.playMp4(fileName);
        }
    }
}

// Client code
class AudioPlayer implements MediaPlayer {
    private MediaAdapter mediaAdapter;

    public void play(String audioType, String fileName) {
        // Built-in support for mp3 files
        if (audioType.equalsIgnoreCase("mp3")) {
            System.out.println("Playing MP3 file: " + fileName);
        }
        // Other media formats are handled by the adapter
        else if (audioType.equalsIgnoreCase("vlc") || audioType.equalsIgnoreCase("mp4")) {
            mediaAdapter = new MediaAdapter(audioType);
            mediaAdapter.play(audioType, fileName);
        }
        // Unsupported media format
        else {
            System.out.println("Invalid media format: " + audioType);
        }
    }
}

// Usage
public class Main {
    public static void main(String[] args) {
        AudioPlayer audioPlayer = new AudioPlayer();
        audioPlayer.play("mp3", "song.mp3");
        audioPlayer.play("vlc", "movie.vlc");
        audioPlayer.play("mp4", "video.mp4");
        audioPlayer.play("avi", "video.avi");
    }
}'''
## Bridge Pattern
The Bridge pattern decouples an abstraction from its implementation, allowing them to vary independently. It uses composition instead of inheritance to achieve this separation. Here's an example of the Bridge pattern implemented in Java:

```
  
  // Abstraction
abstract class Shape {
    protected DrawAPI drawAPI;

    protected Shape(DrawAPI drawAPI) {
        this.drawAPI = drawAPI;
    }

    public abstract void draw();
}

// Refined Abstraction
class Circle extends Shape {
    private int x, y, radius;

    public Circle(int x, int y, int radius, DrawAPI drawAPI) {
        super(drawAPI);
        this.x = x;
        this.y = y;
        this.radius = radius;
    }

    public void draw() {
        drawAPI.drawCircle(radius, x, y);
    }
}

// Implementor interface
interface DrawAPI {
    void drawCircle(int radius, int x, int y);
}

// Concrete Implementor
class RedCircle implements DrawAPI {
    public void drawCircle(int radius, int x, int y) {
        System.out.println("Drawing red circle with radius " + radius + ", x = " + x + ", y = " + y);
    }
}

// Concrete Implementor
class GreenCircle implements DrawAPI {
    public void drawCircle(int radius, int x, int y) {
        System.out.println("Drawing green circle with radius " + radius + ", x = " + x + ", y = " + y);
    }
}

// Usage
public class Main {
    public static void main(String[] args) {
        Shape redCircle = new Circle(100, 100, 10, new RedCircle());
        Shape greenCircle = new Circle(200, 200, 20, new GreenCircle());

        redCircle.draw();
        greenCircle.draw();
    }
}
'''
## Builder Pattern
The Builder pattern separates the construction of an object from its representation. It allows the same construction process to create different representations. Here's an example of the Builder pattern implemented in Java:

```
  
// Product class
class Pizza {
    private String dough;
    private String sauce;
    private String topping;

    public void setDough(String dough) {
        this.dough = dough;
    }

    public void setSauce(String sauce) {
        this.sauce = sauce;
    }

    public void setTopping(String topping) {
        this.topping = topping;
    }

    public void display() {
        System.out.println("Pizza with dough: " + dough + ", sauce: " + sauce + ", topping: " + topping);
    }
}

// Abstract Builder
abstract class PizzaBuilder {
    protected Pizza pizza;

    public Pizza getPizza() {
        return pizza;
    }

    public void createNewPizza() {
        pizza = new Pizza();
    }

    public abstract void buildDough();
    public abstract void buildSauce();
    public abstract void buildTopping();
}

// Concrete Builder
class MargheritaPizzaBuilder extends PizzaBuilder {
    public void buildDough() {
        pizza.setDough("Regular crust");
    }

    public void buildSauce() {
        pizza.setSauce("Tomato sauce");
    }

    public void buildTopping() {
        pizza.setTopping("Mozzarella cheese");
    }
}

// Concrete Builder
class SpicyPizzaBuilder extends PizzaBuilder {
    public void buildDough() {
        pizza.setDough("Thin crust");
    }

    public void buildSauce() {
        pizza.setSauce("Spicy tomato sauce");
    }

    public void buildTopping() {
        pizza.setTopping("Pepperoni and jalapeno");
    }
}

// Director
class PizzaDirector {
    private PizzaBuilder pizzaBuilder;

    public void setPizzaBuilder(PizzaBuilder pizzaBuilder) {
        this.pizzaBuilder = pizzaBuilder;
    }

    public Pizza getPizza() {
        return pizzaBuilder.getPizza();
    }

    public void constructPizza() {
        pizzaBuilder.createNewPizza();
        pizzaBuilder.buildDough();
        pizzaBuilder.buildSauce();
        pizzaBuilder.buildTopping();
    }
}

// Usage
public class Main {
    public static void main(String[] args) {
        PizzaDirector director = new PizzaDirector();
        PizzaBuilder margheritaBuilder = new MargheritaPizzaBuilder();
        PizzaBuilder spicyBuilder = new SpicyPizzaBuilder();

        director.setPizzaBuilder(margheritaBuilder);
        director.constructPizza();
        Pizza margheritaPizza = director.getPizza();
        margheritaPizza.display();

        director.setPizzaBuilder(spicyBuilder);
        director.constructPizza();
        Pizza spicyPizza = director.getPizza();
        spicyPizza.display();
    }
}
'''
  
