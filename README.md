# Movie-tecket-booking-system
import heapq
import tkinter as tk
from tkinter import messagebox, ttk

class MovieTicketBooking:
    def _init_(self, root):
        self.root = root
        self.root.title("Movie Ticket Booking System")
        self.root.geometry("500x600")
        self.root.configure(bg="#2c3e50")
        
        self.theaters = {
            "PVR Ludhiana": {"seats": 50, "distance": 5, "location": "Ludhiana"},
            "INOX Amritsar": {"seats": 40, "distance": 15, "location": "Amritsar"},
            "Cinepolis Jalandhar": {"seats": 30, "distance": 10, "location": "Jalandhar"},
            "IMAX Chandigarh": {"seats": 20, "distance": 12, "location": "Chandigarh"},
            "PVR Patiala": {"seats": 35, "distance": 8, "location": "Patiala"},
            "INOX Bathinda": {"seats": 45, "distance": 18, "location": "Bathinda"},
            "Cinepolis Mohali": {"seats": 25, "distance": 6, "location": "Mohali"},
            "IMAX Zirakpur": {"seats": 30, "distance": 7, "location": "Zirakpur"}
        }
        
        self.create_widgets()
    
    def find_nearest_theater(self):
        movie_type = self.movie_type_var.get()
        nearest = None
        min_distance = float('inf')
        
        for theater, info in self.theaters.items():
            if movie_type.lower() in theater.lower() and info["distance"] < min_distance:
                nearest = theater
                min_distance = info["distance"]
        
        if nearest:
            self.nearest_theater.set(nearest)
            messagebox.showinfo("Nearest Theater", f"Nearest {movie_type} theater is {nearest} at {min_distance} km.")
        else:
            messagebox.showerror("Error", "No matching theater found.")
    
    def book_ticket(self):
        theater = self.nearest_theater.get()
        try:
            num_seats = int(self.seat_var.get())
        except ValueError:
            messagebox.showerror("Error", "Please enter a valid number of seats.")
            return
        
        if theater in self.theaters and self.theaters[theater]["seats"] >= num_seats:
            self.theaters[theater]["seats"] -= num_seats
            messagebox.showinfo("Success", f"Successfully booked {num_seats} ticket(s) at {theater}.")
        else:
            messagebox.showerror("Error", "Not enough seats available or invalid theater.")
    
    def create_widgets(self):
        title_label = tk.Label(self.root, text="Movie Ticket Booking", font=("Arial", 16, "bold"), bg="#2c3e50", fg="white")
        title_label.pack(pady=10)
        
        tk.Label(self.root, text="Enter Your Address:", bg="#2c3e50", fg="white", font=("Arial", 12)).pack(pady=5)
        self.address_var = tk.Entry(self.root, font=("Arial", 12))
        self.address_var.pack(pady=5)
        
        tk.Label(self.root, text="Select Movie Theater Type:", bg="#2c3e50", fg="white", font=("Arial", 12)).pack(pady=5)
        self.movie_type_var = tk.StringVar()
        self.movie_type_var.set("PVR")
        theater_menu = ttk.Combobox(self.root, textvariable=self.movie_type_var, values=["PVR", "INOX", "Cinepolis", "IMAX"], state="readonly")
        theater_menu.pack(pady=5)
        
        find_btn = tk.Button(self.root, text="Find Nearest Theater", command=self.find_nearest_theater, bg="#f39c12", fg="white", font=("Arial", 12, "bold"))
        find_btn.pack(pady=10)
        
        self.nearest_theater = tk.StringVar()
        
        tk.Label(self.root, text="Enter Number of Seats:", bg="#2c3e50", fg="white", font=("Arial", 12)).pack(pady=5)
        self.seat_var = tk.Entry(self.root, font=("Arial", 12))
        self.seat_var.pack(pady=5)
        
        book_btn = tk.Button(self.root, text="Book Ticket", command=self.book_ticket, bg="#27ae60", fg="white", font=("Arial", 12, "bold"))
        book_btn.pack(pady=10)

if _name_ == "_main_":
    root = tk.Tk()
    app = MovieTicketBooking(root)
    root.mainloop()
