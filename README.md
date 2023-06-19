## Adapter Pattern
The Adapter pattern allows objects with incompatible interfaces to work together. It acts as a bridge between two incompatible classes. Here's an example of the Adapter pattern implemented in Java:

```Java
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
}
```


## Bridge Pattern
The Bridge pattern decouples an abstraction from its implementation, allowing them to vary independently. It uses composition instead of inheritance to achieve this separation. Here's an example of the Bridge pattern implemented in Java:

```Java
  
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
```

## Builder Pattern
The Builder pattern separates the construction of an object from its representation. It allows the same construction process to create different representations. Here's an example of the Builder pattern implemented in Java:

```Java
  

public class Card {

    private String cardNo = "";
    private String cardType = "";
    private String userName = "";
    private Integer fee = 0;
    private Integer maxNoOfUsePerMonth = 0;

    public void setFee(Integer fee) {
        this.fee = fee;
    }

    public void setCardType(String cardType) {
        this.cardType = cardType;
    }

    public void setMaxNoOfUsePerMonth(Integer maxNoOfUsePerMonth) {
        this.maxNoOfUsePerMonth = maxNoOfUsePerMonth;
    }

    public Card(String cardNo, String userName) {
        this.cardNo = cardNo;
        this.userName = userName;
    }



    public String getCardNo() {
        return cardNo;
    }

    public String getUserName() {
        return userName;
    }
    public Integer getFee() {
        return fee;
    }
    public Integer getMaxNoOfUsePerMonth() {
        return maxNoOfUsePerMonth;
    }

    @Override
    public String toString() {
        return "Card{" +
                "Card No ='" + cardNo + '\'' +
                ", Card Type='" + cardType + '\'' +
                ", User Name='" + userName + '\'' +
                ", Fee=" + fee +
                ", Max No Of Use Per Month=" + maxNoOfUsePerMonth +
                '}';
    }
}


public class BankRepresentative {
  private CardBuilder cardBuilder;

  public BankRepresentative setCardType(CardType type){
      switch (type){
          case DEBIT -> cardBuilder = new DebitCardBuilder();
          case  CREDIT -> cardBuilder = new CreditCardBuilder();
      }
      return this;
  }

  public BankRepresentative setHolderName(String cardHolderName){
      cardBuilder.createCard(cardHolderName);
      return  this;
  }

  public void makeCard(){
        cardBuilder.setType();
        cardBuilder.setFee();
        cardBuilder.setMaxUsage();
  }
  public Card handCard(){
      return cardBuilder.getCard();
  }

}

public abstract class CardBuilder {
    protected Card card;

    public void createCard(String name){
        Random rnd = new Random();
        int n = 100000 + rnd.nextInt(900000);
        card = new Card(Integer.toString(n),name);
    }
    Card getCard(){
        return card;
    }
    public abstract void setType();
    public abstract void setFee();
    public abstract void setMaxUsage();
}

public class CreditCardBuilder  extends CardBuilder{


    @Override
    public void setType() {
        card.setCardType("Credit Card");

    }

    @Override
    public void setFee() {
        card.setFee(1299);

    }
    @Override
    public void setMaxUsage() {
        card.setMaxNoOfUsePerMonth(20);
    }
}

public class DebitCardBuilder  extends CardBuilder{


    @Override
    public void setType() {
        card.setCardType("Debit Card");
    }

    @Override
    public void setFee() {
        card.setFee(750);
    }
    @Override
    public void setMaxUsage() {
        card.setMaxNoOfUsePerMonth(50);
    }
}


 public static void main(String[] args) {

        BankRepresentative bankRepresentative = new BankRepresentative();

        bankRepresentative.setCardType(CardType.CREDIT)
                .setHolderName("Person A");
        bankRepresentative.makeCard();
        Card creditCard = bankRepresentative.handCard();
        System.out.println(creditCard);

        bankRepresentative.setCardType(CardType.DEBIT)
                .setHolderName("Person B")
                .makeCard();
        Card debitCard = bankRepresentative.handCard();
        System.out.println(debitCard);

    }






  
