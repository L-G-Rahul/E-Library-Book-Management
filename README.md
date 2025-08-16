# --- book.py ---
class Book:
    """
    Represents a single book with a title, author, and availability status.
    This class also includes a 'next_book' attribute to be used for the linked list.
    """

    def __init__(self, title: str, author: str):
        """Initializes a new Book instance."""
        self.title = title
        self.author = author
        self.is_available = True
        self.next_book = None  # Pointer to the next book in the linked list

    def __str__(self) -> str:
        """Returns a user-friendly string representation of the book."""
        return f'"{self.title}" by {self.author} (Available: {self.is_available})'


# --- linked_list.py ---
class LinkedList:
    """
    A simple linked list data structure to manage the book inventory.
    Each node in the list holds a Book object.
    """

    def __init__(self):
        """Initializes an empty linked list."""
        self.head = None

    def add_book(self, title: str, author: str):
        """Adds a new book to the end of the list."""
        new_book = Book(title, author)
        # If the list is empty, the new book becomes the head.
        if not self.head:
            self.head = new_book
            return

        # Traverse the list to find the last node.
        current = self.head
        while current.next_book:
            current = current.next_book
        # Link the new book to the last node.
        current.next_book = new_book

    def find_book(self, search_term: str) -> list[Book]:
        """
        Searches for books by title or author, returning a list of matching books.
        The search is case-insensitive.
        """
        results = []
        current = self.head
        while current:
            if search_term.lower() in current.title.lower() or search_term.lower() in current.author.lower():
                results.append(current)
            current = current.next_book
        return results

    def display_books(self):
        """Prints the details of all books in the inventory."""
        current = self.head
        if not current:
            print("The library is empty.")
            return

        print("\n--- Current Inventory ---")
        while current:
            print(current)
            current = current.next_book


# --- stack.py ---
class Stack:
    """
    A simple stack data structure to implement the "undo" functionality.
    It uses a Python list to store actions (tuples of action type and book object).
    """

    def __init__(self):
        """Initializes an empty stack."""
        self.items = []

    def is_empty(self) -> bool:
        """Checks if the stack is empty."""
        return len(self.items) == 0

    def push(self, item):
        """Adds an item to the top of the stack."""
        self.items.append(item)

    def pop(self):
        """Removes and returns the item from the top of the stack. Returns None if the stack is empty."""
        if not self.is_empty():
            return self.items.pop()
        return None

    def peek(self):
        """Returns the item from the top of the stack without removing it."""
        if not self.is_empty():
            return self.items[-1]
        return None

    def size(self) -> int:
        """Returns the number of items in the stack."""
        return len(self.items)


# --- library_manager.py ---
class LibraryManager:
    """
    Manages all library operations, including book inventory and action history.
    It orchestrates the use of the LinkedList and Stack classes.
    """

    def __init__(self):
        """Initializes the manager with a linked list for books and a stack for actions."""
        self.inventory = LinkedList()
        self.action_history = Stack()

    def add_initial_books(self):
        """Pre-populates the library with a few example books."""
        self.inventory.add_book("The Hobbit", "J.R.R. Tolkien")
        self.inventory.add_book("1984", "George Orwell")
        self.inventory.add_book("Pride and Prejudice", "Jane Austen")
        # Added the new book as requested
        self.inventory.add_book("106 ways to torture a person", "L_G_Rahul")
        print("Initial books have been added to the library.")

    def display_menu(self):
        """Prints the main menu of the application."""
        print("\n--- Library Management System ---")
        print("1. Display all books")
        print("2. Borrow a book")
        print("3. Return a book")
        print("4. Search for a book")
        print("5. Undo last action")
        print("6. Exit")

    def find_book_by_title(self, title: str):
        """
        Helper method to find a single book object by its exact title (case-insensitive).
        Returns the Book object or None if not found.
        """
        current = self.inventory.head
        while current:
            if current.title.lower() == title.lower():
                return current
            current = current.next_book
        return None

    def borrow_book(self):
        """Handles the process of borrowing a book."""
        title = input("Enter the title of the book to borrow: ")
        book = self.find_book_by_title(title)

        if not book:
            print("Book not found.")
            return

        if book.is_available:
            book.is_available = False
            # Push a tuple of the action type and the book object to the stack.
            self.action_history.push(('borrow', book))
            print(f'Successfully borrowed "{book.title}".')
        else:
            print("This book is currently not available.")

    def return_book(self):
        """Handles the process of returning a book."""
        title = input("Enter the title of the book to return: ")
        book = self.find_book_by_title(title)

        if not book:
            print("Book not found.")
            return

        if not book.is_available:
            book.is_available = True
            # Push a tuple of the action type and the book object to the stack.
            self.action_history.push(('return', book))
            print(f'Successfully returned "{book.title}".')
        else:
            print("This book is already in the library.")

    def undo_action(self):
        """Reverts the last borrow or return action using the action history stack."""
        last_action = self.action_history.pop()
        if not last_action:
            print("No actions to undo.")
            return

        action_type, book = last_action
        if action_type == 'borrow':
            book.is_available = True
            print(f'Undo successful: "{book.title}" is now available again.')
        elif action_type == 'return':
            book.is_available = False
            print(f'Undo successful: "{book.title}" is now borrowed again.')

    def search_books(self):
        """Prompts the user for a search term and displays matching books."""
        search_term = input("Enter a title or author to search for: ")
        results = self.inventory.find_book(search_term)

        if not results:
            print("No books found matching your search.")
        else:
            print("\nSearch Results:")
            for book in results:
                print(book)

    def run(self):
        """The main loop of the application that handles user interaction."""
        self.add_initial_books()
        while True:
            self.display_menu()
            try:
                choice = int(input("Enter your choice: "))
                if choice == 1:
                    self.inventory.display_books()
                elif choice == 2:
                    self.borrow_book()
                elif choice == 3:
                    self.return_book()
                elif choice == 4:
                    self.search_books()
                elif choice == 5:
                    self.undo_action()
                elif choice == 6:
                    print("Exiting the application. Goodbye!")
                    break
                else:
                    print("Invalid choice. Please enter a number from 1 to 6.")
            except ValueError:
                print("Invalid input. Please enter a number.")

if __name__ == "__main__":
    manager = LibraryManager()
    manager.run()
    
