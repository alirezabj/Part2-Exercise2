# Part2-Exercise2

For this exercise, in order to solve the two tasks, I will employ the principle of encapsulation. In other words, I will design separate mechanisms to provide library customers with read-only access, while giving library staff the ability to edit.

## Mechanisms and Approach
#### Read-Only Access for Customers
Customers need to view book details, but they should not be able to change any information.

Mechanism: Create a read-only version of the Book object using an interface that only allows access to the book's details through getter methods. 

Consequences:
1. Customers can only retrieve book information but cannot modify it.
2. The internal state of the Book object remains protected.

#### Editable Access for Library Staff
Library staff should be able to update or fix book information when needed

Mechanism: Allow staff to use the full Book class, which includes setter methods to modify the fields. These setter methods validate any changes to ensure the data stays correct.

Consequences:
1. Staff can update the database but must adhere to the defined class invariants.
2. Every change is carefully validated to ensure the information in the database stays reliable


#### Speficiations

#### Interface: `ReadableBook`: This interface exposes only the read-only operations of the Book class.

```java
/**
 * Represents a read-only view of a book
 * Provides access to book details without allowing modifications
 */
public interface ReadableBook {
    /**
     * Returns the title of the book
    
     * @.pre true
     * @.post RESULT == title of the book
     */
    String getTitle();

    /**
     * Returns the publication year of the book
    
     * @.pre true
     * @.post RESULT == publication year of the book
     */
    int getPublicationYear();

    /**
     * Returns the publisher of the book
    
     * @.pre true
     * @.post RESULT == publisher of the book
     */
    String getPublisher();
    
    /**
     * Returns the status of the book 
    
     * @.pre true
     * @.post RESULT == status of the book
     */
    String getStatus();
}

```


#### Class: `Book`: This class implements both the full functionality (for library staff) and the ReadableBook interface (for customers).

```java
/**
 * Represents a book in the library's database
 * Provides both read-only views and editable access depending on the user role
 */
public class Book implements ReadableBook {

    private String title;
    private int publicationYear;
    private String publisher;
    private String status; 

    // Constructor
    /**
     * Constructs a Book object with the specified details.
     * @param title The title of the book
     * @param publicationYear The publication year of the book
     * @param publisher The publisher of the book
     * @param status The initial status of the book
     * @.pre title != null && publisher != null && publicationYear > 0
     * @.post book created with specified details
     */
    public Book(String title, int publicationYear, String publisher, String status) {
        this.title = title;
        this.publicationYear = publicationYear;
        this.publisher = publisher;
        this.status = status;
    }

    // Getters (implements ReadableBook)
    public String getTitle() { return title; }
    public int getPublicationYear() { return publicationYear; }
    public String getPublisher() { return publisher; }
    public String getStatus() { return status; }

    // Setters (for library staff)
    /**
     * Updates the title of the book
     * @param title The new title
     * @.pre title != null
     * @.post this.getTitle() == title
     */
    public void setTitle(String title) {
        this.title = title;
    }

    /**
     * Updates the publication year of the book
     * @param publicationYear The new publication year
     * @.pre publicationYear > 0
     * @.post this.getPublicationYear() == publicationYear
     */
    public void setPublicationYear(int publicationYear) {
        this.publicationYear = publicationYear;
    }

    /**
     * Updates the publisher of the book
     * @param publisher The new publisher
     * @.pre publisher != null
     * @.post this.getPublisher() == publisher
     */
    public void setPublisher(String publisher) {
        this.publisher = publisher;
    }

    /**
     * Updates the status of the book 
     * @param status The new status
     * @.pre status != null
     * @.post this.getStatus() == status
     */
    public void setStatus(String status) {
        this.status = status;
    }
}

```

#### Class Invariants: 
The following invariants must always hold true for the `Book` class:

1. title != null: Every book must have a title.
2. publisher != null: Every book must have a publisher.
3. publicationYear > 0: The publication year must be a positive integer.
4. status != null: The status of the book must always be defined.


#### Database: 
The database is modeled as an in-memory array or list of Book objects. Therefore, both structures are suitable, however, List<Book> is preferred because of its dynamic resizing and convenient methods for querying and updating.

Operations:

Customer View: Customers access the database via a `List<ReadableBook>` view, which is derived from the original `List<Book>` by exposing only the `ReadableBook` interface.
   
Staff View: Library staff access the full `List<Book>` object to perform updates.


