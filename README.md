# Library-management-system
import json
import os
from datetime import datetime, timedelta

class Book:
    def __init__(self, isbn, title, author, total_copies):
        self.isbn = isbn
        self.title = title
        self.author = author
        self.total_copies = int(total_copies)
        self.available_copies = int(total_copies)

    def to_dict(self):
        return {
            "isbn": self.isbn,
            "title": self.title,
            "author": self.author,
            "total_copies": self.total_copies,
            "available_copies": self.available_copies
        }

    @classmethod
    def from_dict(cls, data):
        book = cls(data["isbn"], data["title"], data["author"], data["total_copies"])
        book.available_copies = data["available_copies"]
        return book


class Member:
    def __init__(self, member_id, name):
        self.member_id = member_id
        self.name = name
        self.borrowed_books = []  # List of ISBNs currently borrowed

    def to_dict(self):
        return {
            "member_id": self.member_id,
            "name": self.name,
            "borrowed_books": self.borrowed_books
        }

    @classmethod
    def from_dict(cls, data):
        member = cls(data["member_id"], data["name"])
        member.borrowed_books = data["borrowed_books"]
        return member


class Library:
    def __init__(self, data_file="library_data.json"):
        self.data_file = data_file
        self.books = {}    # isbn -> Book object
        self.members = {}  # member_id -> Member object
        self.load_data()

    def load_data(self):
        """Loads books and members from a JSON file."""
        if not os.path.exists(self.data_file):
            return
        try:
            with open(self.data_file, 'r') as f:
                data = json.load(f)
                for isbn, b_data in data.get("books", {}).items():
                    self.books[isbn] = Book.from_dict(b_data)
                for m_id, m_data in data.get("members", {}).items():
                    self.members[m_id] = Member.from_dict(m_data)
        except Exception as e:
            print(f"⚠️ Error loading data: {e}. Starting with an empty library.")

    def save_data(self):
        """Saves current library state to a JSON file."""
        data = {
            "books": {isbn: b.to_dict() for isbn, b in self.books.items()},
            "members": {m_id: m.to_dict() for m_id, m in self.members.items()}
        }
        with open(self.data_file, 'w') as f:
            json.dump(data, f, indent=4)

    # --- Book Management ---
    def add_book(self, isbn, title, author, copies):
        if isbn in self.books:
            self.books[isbn].total_copies += int(copies)
            self.books[isbn].available_copies += int(copies)
            print(f"📚 Updated copies for existing book: '{title}'.")
        else:

        print("⚠️ Invalid Selection. Choose a number between 1 and 7.")

if __name__ == "__main__":
    main()
        
