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




