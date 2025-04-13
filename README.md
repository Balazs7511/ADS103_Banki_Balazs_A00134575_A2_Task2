using System;
using System.Collections.Generic;
using System.Linq;
using System.Security.Cryptography.X509Certificates;
using System.Text;
using System.Threading.Tasks;

namespace ADS103_Banki_Balazs_A00134575_A2_Task2
{
    internal class Program
    {
        static void Main(string[] args)
        {
            Dictionary<int, string> books = new Dictionary<int, string>();              // instantiating dictionary class called books
            books.Add(1, "Don Quixote");                                                // Adding books to the dictionary named books
            books.Add(2, "1984");
            books.Add(3, "Pride and Prejudice");
            books.Add(4, "The Lord of the Rings");
            books.Add(5, "To Kill a Mockingbird");
            books.Add(6, "The Great Gatsby");
            books.Add(7, "Anna Karenina");
            books.Add(8, "One Hundred Years of Solitude");
            books.Add(9, "The Catcher in the Rye");
            books.Add(10, "Lolita");

            Dictionary<int, string> borrowedBook = new Dictionary<int, string>();        // instantiating dictionary class called borrowedBook

            Menu();                                                                     // calling the Menu method

            void Menu()                                                                 // creating the method called Menu
            {
                Console.WriteLine("What would you like to do :\n");                     // displaying messages to the user
                Console.WriteLine(" 1) List all books you have on loan\n");
                Console.WriteLine(" 2) Return a book\n");
                Console.WriteLine(" 3) List all books in the library\n");
                Console.WriteLine(" 4) Borrow a book\n");
                Console.WriteLine(" 5) Exit\n");
                Console.WriteLine(" Enter choice (1-5)\n");
                int choice = int.Parse(Console.ReadLine());                             // reading the input from the user
                if (choice == 5)                                                       // base case, exit condition if user enters 5
                {

                    Environment.Exit(0);                                                  // exiting the program
                }
                switch (choice)                                                     // switch statement 
                {
                    case 1:
                        // listing all borrowedbooks on loan
                        if (borrowedBook.Count == 0)                                    // if there is no book borrowed diplaying message to the user
                        {
                            Console.Clear();                                          // clearing the console
                            Console.WriteLine("You don`t have book(s) in your library \n");
                            Console.WriteLine();
                            Menu();                                                     // recursively calling the Menu method to start over
                        }
                        else                                                             // if there are books borrowed displaying the list of books borrowed
                        {
                            Console.Clear();                                          // clearing the console
                            Console.WriteLine("Here is the list of book(s) you borrowed \n");
                            foreach (var book in borrowedBook)// foreach loop to iterate through the borrowedBook dictionary contents
                            {
                                Console.WriteLine($" {book.Key} => {book.Value}\n");     // displaying the book ID and value
                            }
                            Console.WriteLine();
                            Menu();                                                 // recursively calling the Menu method to start over
                        }
                        break;                                                      // break statement to exit the case
                    case 2:
                        // returning a book
                        try                                                       // try-catch block to handle exceptions
                        {
                            if (borrowedBook.Count == 0)                                  // if there is no book borrowed diplaying message to the user
                            {
                                Console.Clear();                                          // clearing the console
                                Console.WriteLine(" You don`t have book(s) in your library \n");

                                Menu();                                                    // recursively calling the Menu method
                            }
                            else                                                                    // if there are books borrowed
                            {
                                Console.Clear();                                          // clearing the console
                                Console.WriteLine("Enter the book ID you would like to return \n"); // prompting the user to enter the book ID
                                int bookID = int.Parse(Console.ReadLine());            // reading the input from the user and assigning it to bookID integer
                                if (!borrowedBook.ContainsKey(bookID))                     // if the book id not found displaying the message to the user
                                {
                                    Console.WriteLine("This book does not exist in your library \n");
                                    Console.WriteLine();
                                    Menu();                                           // recursively calling the Menu method to start over
                                }
                                else       // if the book is found in the borrowedBook dictionary
                                {
                                    Console.Clear();                                          // clearing the console
                                    borrowedBook.TryGetValue(bookID, out string value);// getting the value of the bookID from the borrowedBook dictionary
                                    Console.WriteLine($"{bookID} => {value} successfully returned\n");// displaying the message to the user
                                    books.Add(bookID, value);       // adding the bookID and value to the books dictionary
                                    borrowedBook.Remove(bookID);  // removing the bookID from the borrowedBook dictionary
                                    Console.WriteLine();
                                    Menu();                        // recursively calling the Menu method
                                }
                            }
                        }
                        catch (Exception ex)                            // handling exceptions if the user enters invalid input
                        {
                            Console.Clear();                                          // clearing the console
                            Console.WriteLine($"{ex.Message}");         // displaying the exception message to the user
                            Menu();                                     // recursively calling the Menu method to start over
                        }

                        break;                                         // break statement to exit the case
                    case 3:
                        // listing all books in the library
                        if (books.Count == 0)                                           // if the library is empty, displaying message to the user
                        {
                            Console.Clear();                                          // clearing the console
                            Console.WriteLine("Library is empty \n");
                            Console.WriteLine();
                            Menu();                                                          // recursively calling the Menu method to start over
                        }
                        Console.Clear();                                          // clearing the console
                        Console.WriteLine(" The following books are in the library \n");  // displaying the list of books in the library
                        foreach (var book in books)                                  // foreach loop to iterate through the books dictionary contents
                        {
                            Console.WriteLine($" {book.Key} => {book.Value}");         // displaying the books dictionary contents
                        }
                        Console.WriteLine();
                        Menu();                                                  // recursively calling the Menu method
                        break;                                                 // break statement to exit the case

                    case 4:
                        // borrowing a book
                        try                                                                 // try-catch block to handle exceptions
                        {
                            Console.Clear();                                          // clearing the console
                            Console.WriteLine("Please enter book ID to be borrowed \n");     // prompting the user to enter the book ID
                            int ID = int.Parse(Console.ReadLine());                       // reading the input from the user and assigning it to ID integer
                            if (!books.ContainsKey(ID))                                 // if the book id not found displaying the message to the user
                            {
                                Console.Clear();                                          // clearing the console
                                Console.WriteLine("Book does not exist in the library \n");
                                Console.WriteLine();
                                Menu();                                                 // recursively calling the Menu method to return to the main menu
                            }
                            else                                          // if the book ID is found
                            {
                                Console.Clear();                                          // clearing the console
                                books.TryGetValue(ID, out string value);         // trygetvalue method of the dictionary class to get the value of the bookID
                                borrowedBook.Add(ID, value);                                   // adding the bookID and value to the borrowedBook dictionary
                                books.Remove(ID);                                                          // removing the bookID from the books dictionary
                                Console.WriteLine($"{ID} => {value} succesfully added to your library \n");  // displaying the message to the user
                            }
                        }
                        catch (Exception ex)                      // handling exceptions if the user enters invalid input
                        {
                            Console.WriteLine($"{ex.Message}"); // displaying the exception message to the user

                        }
                        Menu();                                      // recursively calling the Menu method to return to the main menu
                        break;
                    default:
                        {
                            Console.Clear();                                          // clearing the console
                            Menu();                                             // recursively calling the Menu method to return to the main menu
                        }
                        break;

                }
            }
        }
    }
}


