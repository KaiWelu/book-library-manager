# Project Name: **Book Library Manager**

**Description:**
A simple Java application that allows users to manage a collection of books. Users can add, search, update, and delete book records from a local database. The application also allows users to filter books by author, title, or genre and keeps track of the number of available copies.

---

### Tech Tree:

1. **Core Technologies:**
   - **Java SE 17**: Core programming language used for the application.
   - **Maven**: Build automation tool for managing dependencies and project structure.
   - **MySQL/SQLite**: For the database to store book information (optional in-memory using Java's HashMap for simplicity).
2. **Frameworks & Libraries:**
   - **JDBC**: Java Database Connectivity for interacting with the database.
   - **JUnit**: For unit testing different functionalities of the application.
   - **Lombok**: For simplifying Java code with annotations (e.g., getters, setters).
3. **User Interface:**
   - **JavaFX**: For building a basic graphical user interface where users can interact with the library.
4. **Version Control & CI/CD:**
   - **Git**: Version control for managing code.
   - **GitHub/GitLab**: Repository hosting.
   - **GitHub Actions**: Optional for CI/CD setup with automated testing.
5. **Project Structure:**
   - **MVC (Model-View-Controller)**: Organize code in a structured manner (models for book data, controllers for logic, and views for the UI).
   - **DAO (Data Access Object) Pattern**: To abstract and encapsulate all access to the data source.

### Steps to Accomplish the Features of **Book Library Manager**

#### **1. Set Up the Project:**
   - **Create a Maven Project:**
     - Use an IDE like IntelliJ IDEA or Eclipse to create a new Maven project.
     - Add necessary dependencies in the `pom.xml` file, such as:
       - `mysql-connector-java` (or `sqlite-jdbc` for SQLite)
       - `junit-jupiter` for testing.
       - `lombok` for reducing boilerplate code.
   
   - **Project Structure:**
     Organize the project following the **MVC** pattern:
     ```
     src/
     ├── main/
     │   ├── java/
     │   │   ├── models/          # Book and related models
     │   │   ├── controllers/     # Application logic
     │   │   ├── views/           # JavaFX views (UI components)
     │   │   └── dao/             # Data Access Layer
     │   ├── resources/           # Configurations, fxml files (for UI)
     └── test/
         └── java/
             └── tests/           # JUnit tests
     ```

#### **2. Create the Core Features:**

##### **2.1 Create the Book Model**
   - Define a `Book` class inside `models/Book.java`.
   - Use Lombok to generate getters, setters, `toString()`, and constructors.
   - Example:
     ```java
     @Data // Lombok annotation to generate boilerplate code
     public class Book {
         private int id;
         private String title;
         private String author;
         private String genre;
         private int copiesAvailable;
     }
     ```

##### **2.2 Create a Data Access Layer (DAO)**
   - Create an interface `BookDAO` that defines CRUD operations:
     ```java
     public interface BookDAO {
         void addBook(Book book);
         Book getBookById(int id);
         List<Book> getAllBooks();
         void updateBook(Book book);
         void deleteBook(int id);
     }
     ```
   - Implement the `BookDAO` using JDBC (MySQL/SQLite):
     - Example implementation in `MySQLBookDAO.java`:
     ```java
     public class MySQLBookDAO implements BookDAO {
         private Connection connection;

         public MySQLBookDAO(Connection connection) {
             this.connection = connection;
         }

         @Override
         public void addBook(Book book) throws SQLException {
             String query = "INSERT INTO books (title, author, genre, copiesAvailable) VALUES (?, ?, ?, ?)";
             PreparedStatement stmt = connection.prepareStatement(query);
             stmt.setString(1, book.getTitle());
             stmt.setString(2, book.getAuthor());
             stmt.setString(3, book.getGenre());
             stmt.setInt(4, book.getCopiesAvailable());
             stmt.executeUpdate();
         }

         // Similarly, implement other methods (getBookById, updateBook, deleteBook, etc.)
     }
     ```

##### **2.3 Create the Controller Logic**
   - In `controllers/BookController.java`, write logic to interact with the DAO and UI.
   - Example:
     ```java
     public class BookController {
         private BookDAO bookDAO;

         public BookController(BookDAO bookDAO) {
             this.bookDAO = bookDAO;
         }

         public void addBook(Book book) {
             bookDAO.addBook(book);
         }

         public Book getBookById(int id) {
             return bookDAO.getBookById(id);
         }

         public void updateBook(Book book) {
             bookDAO.updateBook(book);
         }

         public void deleteBook(int id) {
             bookDAO.deleteBook(id);
         }

         // Other methods to handle user interactions...
     }
     ```

##### **2.4 Create the User Interface (UI)**
   - Use **JavaFX** to build the user interface.
   - Create `.fxml` files for the views and load them in Java.
     - Example `views/MainView.fxml` (for book management):
       ```xml
       <VBox spacing="10">
           <Label text="Book Library Manager" />
           <TextField fx:id="titleField" promptText="Title" />
           <TextField fx:id="authorField" promptText="Author" />
           <TextField fx:id="genreField" promptText="Genre" />
           <Button text="Add Book" onAction="#handleAddBook" />
           <TableView fx:id="bookTable">
               <!-- Columns for Title, Author, Genre, etc. -->
           </TableView>
       </VBox>
       ```
   - In `MainViewController.java`, link JavaFX controls with the controller:
     ```java
     public class MainViewController {
         @FXML private TextField titleField;
         @FXML private TextField authorField;
         @FXML private TextField genreField;
         @FXML private TableView<Book> bookTable;

         private BookController bookController;

         public void initialize() {
             // Initialize the TableView, bookController, etc.
         }

         @FXML
         private void handleAddBook() {
             String title = titleField.getText();
             String author = authorField.getText();
             String genre = genreField.getText();
             Book book = new Book(title, author, genre, 1);
             bookController.addBook(book);
             updateBookTable();
         }

         private void updateBookTable() {
             bookTable.setItems(FXCollections.observableArrayList(bookController.getAllBooks()));
         }
     }
     ```

#### **3. Implement the Search and Filter Features:**
   - Extend the DAO to allow filtering by author, title, or genre.
     ```java
     public List<Book> searchBooksByTitle(String title) {
         String query = "SELECT * FROM books WHERE title LIKE ?";
         PreparedStatement stmt = connection.prepareStatement(query);
         stmt.setString(1, "%" + title + "%");
         ResultSet result = stmt.executeQuery();
         // Process result and return the list of matching books
     }
     ```

   - Update the UI to provide search fields and show results in the table.

#### **4. Add Unit Testing with JUnit:**
   - Test DAO methods:
     ```java
     @Test
     public void testAddBook() {
         BookDAO bookDAO = new MySQLBookDAO(getTestConnection());
         Book book = new Book("Test Title", "Test Author", "Test Genre", 1);
         bookDAO.addBook(book);
         assertNotNull(bookDAO.getBookById(book.getId()));
     }
     ```
   - Test Controller methods to ensure they interact correctly with DAO.

#### **5. Final Touches:**
   - **Validation**: Add form validation in the UI (e.g., make sure all fields are filled before adding a book).
   - **Exception Handling**: Handle database errors, invalid input, and any other runtime issues gracefully.
   - **Styling**: Use JavaFX CSS to style the UI.
   - **Documentation**: Add comments and basic documentation in the code.

### Optional Enhancements:
   - **Pagination**: If the book list becomes large, implement pagination.
   - **Sorting**: Allow sorting by columns (e.g., by title or author).
   - **Export/Import**: Enable export/import of the book list to/from CSV.
