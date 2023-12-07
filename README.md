# CODING-SAMURAI-INTERNSHIP-TASK-3-Command-Line-Library-Management-System
#include <iostream>
#include <vector>
#include <algorithm>

class Book {
public:
    std::string title;
    std::string author;
    bool available;

    Book(const std::string& title, const std::string& author) : title(title), author(author), available(true) {}
};

class Library {
private:
    std::vector<Book> books;

public:
    void addBook(const std::string& title, const std::string& author) {
        books.push_back(Book(title, author));
    }

    void displayBooks() const {
        std::cout << "Books in the library:\n";
        for (const auto& book : books) {
            std::cout << "Title: " << book.title << ", Author: " << book.author
                      << ", Status: " << (book.available ? "Available" : "Checked out") << "\n";
        }
        std::cout << "---------------------------\n";
    }

    bool borrowBook(const std::string& title) {
        auto it = std::find_if(books.begin(), books.end(), [&title](const Book& book) {
            return book.title == title && book.available;
        });

        if (it != books.end()) {
            it->available = false;
            std::cout << "Book '" << title << "' borrowed successfully.\n";
            return true;
        } else {
            std::cout << "Book '" << title << "' not available for borrowing.\n";
            return false;
        }
    }

    bool returnBook(const std::string& title) {
        auto it = std::find_if(books.begin(), books.end(), [&title](const Book& book) {
            return book.title == title && !book.available;
        });

        if (it != books.end()) {
            it->available = true;
            std::cout << "Book '" << title << "' returned successfully.\n";
            return true;
        } else {
            std::cout << "Book '" << title << "' not available for returning.\n";
            return false;
        }
    }
};

int main() {
    Library library;

    library.addBook("The Catcher in the Rye", "J.D. Salinger");
    library.addBook("To Kill a Mockingbird", "Harper Lee");
    library.addBook("1984", "George Orwell");

    int choice;
    std::string title;

    do {
        std::cout << "Menu:\n";
        std::cout << "1. Display Books\n";
        std::cout << "2. Borrow Book\n";
        std::cout << "3. Return Book\n";
        std::cout << "4. Quit\n";
        std::cout << "Enter your choice: ";
        std::cin >> choice;

        switch (choice) {
            case 1:
                library.displayBooks();
                break;
            case 2:
                std::cout << "Enter the title of the book to borrow: ";
                std::cin.ignore(); 
                std::getline(std::cin, title);
                library.borrowBook(title);
                break;
            case 3:
                std::cout << "Enter the title of the book to return: ";
                std::cin.ignore();
                std::getline(std::cin, title);
                library.returnBook(title);
                break;
            case 4:
                std::cout << "Exiting the program.\n";
                break;
            default:
                std::cout << "Invalid choice. Please try again.\n";
        }
    } while (choice != 4);

    return 0;
}
