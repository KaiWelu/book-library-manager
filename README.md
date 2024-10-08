### Project Name: **Book Library Manager**

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
