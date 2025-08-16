📚 Library Management System 📖
Welcome to the Library Management System, a simple command-line tool built to showcase fundamental data structures in action. This project lets you manage a library's book inventory with ease, including a neat undo feature for recent actions.

✨ Features
Book Inventory 🔗: Our library's book collection is organized using a custom-built linked list. This allows for a clean and efficient way to add and manage books.

Borrow & Return 🔄: Borrow a book when it's available or return one when you're done! The system keeps track of each book's status.

Undo Functionality ⏪: Made a mistake? No problem! A powerful stack data structure records your actions, so you can instantly undo the last borrow or return with a single command.

Search & Filter 🔎: Quickly find any book in the library by searching for its title or author. The search is smart and works with partial names, so you'll always find what you're looking for.

⚙️ Technical Details
This project is a great example of implementing classic data structures:

Linked List: The LinkedList class is the backbone of our inventory system, providing a dynamic way to store and access book records.

Stack: The Stack class is the key to our undo feature, leveraging the Last-In, First-Out (LIFO) principle to reverse the most recent action.

Object-Oriented Design: The code is modular and easy to understand, with dedicated classes for Book, LinkedList, Stack, and LibraryManager.

▶️ How to Run
Save the Code 💾: Save the provided Python code into a single file named library_manager.py.

Open a Terminal 💻: Navigate to the directory where you saved the file.

Run the Script 🚀: Execute the script from your terminal using the Python interpreter:

python library_manager.py

The application will start, and you'll be greeted with a menu to start your library management journey!

📂 Code Structure
The entire application is neatly contained within one file for convenience. Here's a quick overview of what's inside:

The Book class: Defines the book object itself.

The LinkedList and Stack classes: Implement the core data structures.

The LibraryManager class: Combines everything and provides the user interface.

Enjoy managing your digital library! If you have any questions or ideas for improvements, feel free to reach out.
