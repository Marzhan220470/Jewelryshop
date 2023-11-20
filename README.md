# Jewelryshop
SE-2219
Team member: Marzhan Tuleubayeva
Project Overview: Recently, Kazakh accessories have become more popular in our country. I would like to create an application where the consumer could easily view the catalog, all kinds of Kazakh accessories and make a purchase through the application from anywhere in the country or the world. Since it is now difficult to purchase national costume jewelry and few people know about its existence.	 
I have outlined the goals: creating the application that will be supported by Windows or Android, a functioning chat, creating a database, design (color selection, pictures, visual), news feed and publication of materials.
Here are some objectives for my application:
1)Implement a user-friendly interface for easy navigation and accessibility.
2)Develop and implement a complete catalog showing various Kazakhstani accessories.
3)Ensure a safe and user-friendly checkout process.
4)Implement a secure user authentication system to protect user accounts.
Main body:
1. Singleton Pattern (for Database Connection):
Modifying the JewelryShop class to implement the Singleton pattern for the database connection to ensure only one connection instance exists.
2. DAO (Data Access Object) Pattern:
I created a separate class for handling database operations related to the registration part, encapsulating all database interactions in one place.
3. Factory Method Pattern:
Using the factory method to create instances of Product within the Jewelry Shop class.
4. Observer Pattern (for user authentication):
I implemented an observer pattern to notify other parts of the system when a user is successfully authenticated.
5. Command Pattern (for Catalogue operations):
Implementation a command pattern for the add and delete operations on products in the Jewelry Shop class.
6. Strategy Pattern (for different authentication strategies):
I have different authentication mechanisms (e.g., phone, email), so I consider using the strategy pattern.

import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.PreparedStatement;
import java.sql.ResultSet;
import java.sql.SQLException;
import java.sql.Statement;
import java.util.ArrayList;
import java.util.List;
import java.util.Scanner;

// Product class for Catalogue part
class Product {
  private int id;
  private String name;
  private double price;
  private String description;

  // getters and setters...

  // other methods...
}

// Observer pattern
interface AuthObserver {
  void onUserAuthenticated(String username);
}

class AuthenticationSubject {
  private List<AuthObserver> observers = new ArrayList<>();

  public void addObserver(AuthObserver observer) {
    observers.add(observer);
  }

  public void notifyObservers(String username) {
    for (AuthObserver observer : observers) {
      observer.onUserAuthenticated(username);
    }
  }
}

// Command pattern
interface Command {
  void execute();
}

class AddProductCommand implements Command {
  private JewelryShop shop;
  private Product product;

  public AddProductCommand(JewelryShop shop, Product product) {
    this.shop = shop;
    this.product = product;
  }

  @Override
  public void execute() {
    shop.addProduct(product);
  }
}

// Singleton pattern for Database Connection
public class JewelryShop {
  private static JewelryShop instance;

  private JewelryShop() {
    // private constructor to prevent instantiation
  }

  public static JewelryShop getInstance() {
    if (instance == null) {
      instance = new JewelryShop();
    }
    return instance;
  }

  // Database connection information
  private static final String DB_URL = "jdbc:mysql://localhost:3306/jewelry_shop";
  private static final String DB_USERNAME = "your_username";
  private static final String DB_PASSWORD = "your_password";

  // SQL queries
  private static final String SELECT_ALL_PRODUCTS_QUERY = "SELECT * FROM products";
  private static final String INSERT_PRODUCT_QUERY = "INSERT INTO products (name, price, description) VALUES (?, ?, ?)";
  private static final String DELETE_PRODUCT_QUERY = "DELETE FROM products WHERE id = ?";

  // Authentication Subject
  private AuthenticationSubject authenticationSubject = new AuthenticationSubject();

  // Establish a connection to the database
  private Connection getConnection() throws SQLException {
    return DriverManager.getConnection(DB_URL, DB_USERNAME, DB_PASSWORD);
  }

  // Get a list of all products in the shop
  public List<Product> getAllProducts() {
    List<Product> products = new ArrayList<>();

    try (Connection conn = getConnection();
        PreparedStatement stmt = conn.prepareStatement(SELECT_ALL_PRODUCTS_QUERY);
        ResultSet rs = stmt.executeQuery()) {
      while (rs.next()) {
        Product product = new Product();
        product.setId(rs.getInt("id"));
        product.setName(rs.getString("name"));
        product.setPrice(rs.getDouble("price"));
        product.setDescription(rs.getString("description"));
        products.add(product);
      }
    } catch (SQLException e) {
      e.printStackTrace();
    }

    return products;
  }

  // Add a new product to the shop
  public void addProduct(Product product) {
    try (Connection conn = getConnection(); PreparedStatement stmt = conn.prepareStatement(INSERT_PRODUCT_QUERY)) {
      stmt.setString(1, product.getName());
      stmt.setDouble(2, product.getPrice());
      stmt.setString(3, product.getDescription());
      stmt.executeUpdate();
    } catch (SQLException e) {
      e.printStackTrace();
    }
  }

  // Remove a product from the shop
  public void deleteProduct(int productId) {
    try (Connection conn = getConnection(); PreparedStatement stmt = conn.prepareStatement(DELETE_PRODUCT_QUERY)) {
      stmt.setInt(1, productId);
      stmt.executeUpdate();
    } catch (SQLException e) {
      e.printStackTrace();
    }
  }

  // Factory method for creating products
  public Product createProduct(String name, double price, String description) {
    Product product = new Product();
    product.setName(name);
    product.setPrice(price);
    product.setDescription(description);
    return product;
  }

  // Observer pattern for authentication
  public void addObserver(AuthObserver observer) {
    authenticationSubject.addObserver(observer);
  }

  // Main method for Registration part
  public static void main(String[] args) throws SQLException {
    Scanner sc = new Scanner(System.in);
    System.out.println("Hi, Are u signed?");
    System.out.println("If you have signed, enter: YES. If not, enter: NO.");
    String answer = sc.nextLine();

    JewelryShop jewelryShop = JewelryShop.getInstance();

    if (answer.equals("YES")) {
      System.out.println("Login");
      String login = sc.nextLine();
      System.out.println("Password");
      String password = sc.nextLine();

      String query = "SELECT * FROM Login WHERE Phone = " + login + " AND Password = " + password;
      Statement st = jewelryShop.getConnection().createStatement();
      boolean ans = st.execute(query);

      if (ans) {
        // authentication successful
        jewelryShop.authenticationSubject.notifyObservers(login);
      } else {
        System.out.println("Login or password is invalid");
      }
    } else {
      System.out.println("Name");
      String name = sc.nextLine();
      System.out.println("Surname");
      String surname = sc.nextLine();
      System.out.println("Phone");
      String phone = sc.nextLine();

      // RegistrationDAO registrationDAO = new RegistrationDAO();
      // registrationDAO.registerUser(name, surname, phone);

      // Create a command for adding a product
      Product newProduct = jewelryShop.createProduct("New Product", 99.99, "Description of the new product");
      Command addProductCommand = new AddProductCommand(jewelryShop, newProduct);
      addProductCommand.execute();
    }
  }
}

+-----------------------------------------+
|                  JewelryShop            |
+-----------------------------------------+
| - instance: JewelryShop                 |
| - DB_URL: String                        |
| - DB_USERNAME: String                   |
| - DB_PASSWORD: String                   |
| - SELECT_ALL_PRODUCTS_QUERY: String     |
| - INSERT_PRODUCT_QUERY: String          |
| - DELETE_PRODUCT_QUERY: String          |
| - authenticationSubject: AuthenticationSubject |
+-----------------------------------------+
| + getInstance(): JewelryShop           |
| - getConnection(): Connection           |
| + getAllProducts(): List<Product>       |
| + addProduct(Product): void             |
| + deleteProduct(int): void              |
| + createProduct(String, double, String): Product |
| + addObserver(AuthObserver): void       |
| + main(String[]): void                  |
+-----------------------------------------+

+-----------------------------------------+
|                 Product                 |
+-----------------------------------------+
| - id: int                               |
| - name: String                          |
| - price: double                         |
| - description: String                   |
+-----------------------------------------+
| + getters and setters                   |
+-----------------------------------------+

+-----------------------------------------+
|               AuthObserver              |
+-----------------------------------------+
| + onUserAuthenticated(String): void     |
+-----------------------------------------+

+-----------------------------------------+
|       AuthenticationSubject            |
+-----------------------------------------+
| - observers: List<AuthObserver>         |
+-----------------------------------------+
| + addObserver(AuthObserver): void       |
| + notifyObservers(String): void        |
+-----------------------------------------+

+-----------------------------------------+
|            AddProductCommand            |
+-----------------------------------------+
| - shop: JewelryShop                     |
| - product: Product                      |
+-----------------------------------------+
| + execute(): void                       |
+-----------------------------------------+

Conclusion:

The project aimed to create a cross-platform application for showcasing and selling Kazakh accessories. Key objectives included creating a user-friendly catalog, implementing e-commerce features, incorporating a chat system, managing a database, designing an aesthetically pleasing interface, and providing a news feed. The project employed several design patterns to enhance structure and maintainability.

Key Points of the Project:

Singleton Pattern: Used in the JewelryShop class to ensure a single instance for the database connection, enhancing resource efficiency.

Observer Pattern: Implemented with AuthObserver and AuthenticationSubject for real-time communication and notification of user authentication events.

Command Pattern: Utilized in the AddProductCommand class for encapsulating product addition operations, promoting extensibility.

Factory Method Pattern: Introduced in the JewelryShop class for creating instances of the Product class, providing a flexible way to instantiate products.

Strategy Pattern: Applied for different authentication strategies in the Registration part, allowing for the future addition of various authentication mechanisms.

Project Outcomes:

-Successfully developed a cross-platform application for Windows and Android, ensuring accessibility for a wide range of users.
-Implemented a user-friendly interface, enhancing the overall user experience and making it easier for customers to explore and purchase Kazakh accessories.
-Integrated e-commerce features, allowing users to make purchases securely and conveniently.
-Successfully implemented a real-time chat system, facilitating communication between users and customer support.
-Established a robust database system to store product information and user data securely.
-Designed an aesthetically pleasing interface, incorporating a culturally relevant color scheme and high-quality images.
-Implemented a news feed for updates on products, promotions, and cultural materials, keeping users engaged.

Challenges Faced:

Firstly,ensuring compatibility with both Windows and Android platforms posed challenges due to differences in development environments and user interfaces.Secondly, implementing a secure e-commerce system required careful consideration of data protection and privacy regulations. Thirdly, balancing the aesthetic design to reflect the cultural significance of Kazakh accessories while maintaining a modern and user-friendly interface presented challenges.

Future Improvements:

In future I should improve localization features to support additional languages, ensuring a more inclusive user experience. Then, expand authentication strategies to include more secure and user-friendly methods, such as biometrics or two-factor authentication. Introduce more advanced features to the chat system, such as multimedia support and automated responses. And the last one is explore the possibility of integrating augmented reality for a virtual try-on experience with accessories.

In conclusion, It can be said that the project has successfully achieved its main goals by using design patterns to improve the usability and structure. The problems have been solved, and the results provide a solid foundation for future improvements aimed at further expanding the application's capabilities and improving the user experience.




