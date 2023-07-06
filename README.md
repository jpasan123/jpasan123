
//      used data structure: DoublyLinkedList
//      team members: COHDSE231F-030, COHDSE231F-032, COHDSE231F-053

class Book {
    private int isbn;
    private String title;
    private String author;
    private String genre;
    private boolean borrowed;

    public Book(int isbn, String title, String author, String genre) {
        this.isbn = isbn;
        this.title = title;
        this.author = author;
        this.genre = genre;
        this.borrowed = false;      //  when a book added, it's default borrowed state is false
    }

    public int getIsbn() {
        return isbn;
    }

    public void setIsbn(int isbn) {
        this.isbn = isbn;
    }

    public String getTitle() {
        return title;
    }

    public void setTitle(String title) {
        this.title = title;
    }

    public String getAuthor() {
        return author;
    }

    public void setAuthor(String author) {
        this.author = author;
    }

    public String getGenre() {
        return genre;
    }

    public void setGenre(String genre) {
        this.genre = genre;
    }

    public boolean isBorrowed() {   //  return a book is borrowed(true) or not(false)
        return borrowed;
    }

    public void setBorrowed(boolean borrowed) {
        this.borrowed = borrowed;
    }
}

class Node {
    Book book;      //  association, a book is inside a node
    Node prevNode, nextNode;

    public Node(Book book) {
        this.book = book;       //  book automatically created when a node creates
        prevNode = null;
        nextNode = null;
    }

    public Book getBook() {
        return book;
    }

    public void setBook(Book book) {
        this.book = book;
    }

    public Node getPrevNode() {
        return prevNode;
    }

    public void setPrevNode(Node prevNode) {
        this.prevNode = prevNode;
    }

    public Node getNextNode() {
        return nextNode;
    }

    public void setNextNode(Node nextNode) {
        this.nextNode = nextNode;
    }
}

class Library {
    Node head, tail;

    public Library() {
        head = null;
        tail = null;
    }

    public void addBook(Book book) {
        Node node = new Node(book);
        if (head == null) {
            head = node;
            tail = node;
        } else {
            tail.setNextNode(node);     //  add new node to the end
            node.setPrevNode(tail);
            tail = node;
        }
    }

    public void removeBook(String title, String author) {      //  remove a book by title and author
        Node currentNode = head;
        while (currentNode != null) {
            if (currentNode.getBook().getTitle().equals(title) && currentNode.getBook().getAuthor().equals(author)) {
                Node prevNode = currentNode.getPrevNode();
                Node nextNode = currentNode.getNextNode();

                if (currentNode == head) {
                    head = nextNode;
                } else {
                    prevNode.setNextNode(nextNode);
                }

                if (currentNode == tail) {
                    tail = prevNode;
                } else {
                    nextNode.setPrevNode(prevNode);
                }
                return;      //  assume there's only a single book from the author
            }
            currentNode = currentNode.getNextNode();
        }
        System.err.println("There is no such a book to remove");
    }

    public void removeBook(int isbn) {          //  remove a book by isbn
        Node currentNode = head;
        while (currentNode != null) {
            if (currentNode.getBook().getIsbn() == isbn) {
                Node prevNode = currentNode.getPrevNode();
                Node nextNode = currentNode.getNextNode();

                if (currentNode == head) {
                    head = nextNode;
                } else {
                    prevNode.setNextNode(nextNode);
                }

                if (currentNode == tail) {
                    tail = prevNode;
                } else {
                    nextNode.setPrevNode(prevNode);
                }
                return;
            }
            currentNode = currentNode.getNextNode();
        }
        System.err.println("There is no such a book to remove");
    }

    public void countBooksByAuthor(String Author) {
        int count = 0;
        Node currentNode = head;
        while (currentNode != null) {
            if (currentNode.getBook().getAuthor().equalsIgnoreCase(Author)) {
                count++;
            }
            currentNode = currentNode.getNextNode();
        }
        System.out.println("There are " + count + " of books available for author " + Author);
    }

    public void borrowBook(String title, String author) {
        Node currentNode = head;
        while (currentNode != null) {
            if (currentNode.getBook().getTitle().equals(title) && currentNode.getBook().getAuthor().equals(author)) {
                if (currentNode.getBook().isBorrowed()) {
                    System.out.println("Sorry, the book " + title + " is already borrowed.");
                } else {
                    currentNode.getBook().setBorrowed(true);
                    System.out.println("Book " + title + " has been borrowed.");
                }
                return;     //  stop the execution of the method
            }
            currentNode = currentNode.getNextNode();
        }
        System.err.println("Book " + title + " not found in the library.");
    }

    public void returnBook(String title, String author) {
        Node currentNode = head;
        while (currentNode != null) {
            if (currentNode.getBook().getTitle().equals(title) && currentNode.getBook().getAuthor().equals(author)) {
                if (!currentNode.getBook().isBorrowed()) {
                    System.out.println("The book " + title + " was not borrowed.");
                } else {
                    currentNode.getBook().setBorrowed(false);
                    System.out.println("Book " + title + " has been returned.");
                }
                return;     //  stop the execution of the method
            }
            currentNode = currentNode.getNextNode();
        }
        System.err.println("Book " + title + " does not belong to the library");
    }

    public void findBook(String title, String author) {
        Node currentNode = head;
        while (currentNode != null) {
            if (currentNode.getBook().getTitle().equals(title) && currentNode.getBook().getAuthor().equals(author) && currentNode.getBook().isBorrowed()) {
                System.out.println("Book " + title + " is available, but someone has borrowed it already, check later");
                return;
            } else if (currentNode.getBook().getTitle().equals(title) && currentNode.getBook().getAuthor().equals(author) && !currentNode.getBook().isBorrowed()) {
                System.out.println("Book " + title + " is available");
                return;
            }
            currentNode = currentNode.getNextNode();
        }
        System.err.println("Book " + title + " is not available");
    }

    public void findBook(int isbn) {
        Node currentNode = head;
        while (currentNode != null) {
            if (currentNode.getBook().getIsbn() == isbn && currentNode.getBook().isBorrowed()) {
                System.out.println("Book which has the ISBN " + isbn + " is available, but someone has borrowed it already, check later");
                return;
            } else if (currentNode.getBook().getIsbn() == isbn) {
                System.out.println("Book which has the ISBN " + isbn + " is available");
                return;
            }
            currentNode = currentNode.getNextNode();
        }
        System.err.println("Book which has the ISBN " + isbn + " is not available");
    }

    public void showAllBooks() {
        Node currentNode = head;
        while (currentNode != null) {
            System.out.println(currentNode.book.getTitle());
            currentNode = currentNode.getNextNode();
        }
    }

    public void showAvailableBooks() {
        Node currentNode = head;
        while (currentNode != null) {
            if (!currentNode.getBook().isBorrowed()) {
                System.out.println(currentNode.getBook().getTitle());
            }
            currentNode = currentNode.getNextNode();
        }
    }
}

public class Main {
    public static void main(String[] args) {
        Book book1 = new Book(123456789, "Book 1", "Author 1", "Fantasy");
        Book book2 = new Book(987654321, "Book 2", "Author 2", "Mystery");
        Book book3 = new Book(789456123, "Book 3", "Author 3", "Science Fiction");

        Library lib = new Library();
        lib.addBook(book1);
        lib.addBook(book2);
        lib.addBook(book3);

//        lib.removeBook("Book 3");

        lib.borrowBook("Book 1", "Author 1");
//        lib.returnBook("Book 1","Author 1");

        lib.removeBook("Book 3", "Author 3");
        lib.findBook(789456123);

//        lib.showAllBooks();
        lib.showAvailableBooks();
    }
}
