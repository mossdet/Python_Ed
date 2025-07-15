# OOP Practical Projects and Exercises

## Table of Contents
- [Project 1: Library Management System](#project-1-library-management-system)
- [Project 2: Vehicle Rental System](#project-2-vehicle-rental-system)
- [Project 3: Game Character System](#project-3-game-character-system)
- [Project 4: Banking System](#project-4-banking-system)
- [Project 5: E-learning Platform](#project-5-e-learning-platform)
- [Advanced Challenges](#advanced-challenges)
- [Code Review Guidelines](#code-review-guidelines)

---

## Project 1: Library Management System

### Project Overview
Build a comprehensive library management system that demonstrates all OOP principles.

### Requirements
1. Multiple types of library items (books, DVDs, magazines)
2. Member management with borrowing limits
3. Fine calculation system
4. Search and filtering capabilities
5. Administrative functions

### Implementation

```python
from abc import ABC, abstractmethod
from datetime import datetime, timedelta
from enum import Enum

class ItemStatus(Enum):
    AVAILABLE = "Available"
    BORROWED = "Borrowed"
    RESERVED = "Reserved"
    MAINTENANCE = "Maintenance"

class LibraryItem(ABC):
    """Abstract base class for all library items"""
    
    item_counter = 0
    
    def __init__(self, title, author_or_creator, publication_year):
        LibraryItem.item_counter += 1
        self.item_id = f"LIB{LibraryItem.item_counter:04d}"
        self.title = title
        self.author_or_creator = author_or_creator
        self.publication_year = publication_year
        self.status = ItemStatus.AVAILABLE
        self.borrowed_by = None
        self.due_date = None
        self.borrow_date = None
        self.reservation_queue = []
    
    @abstractmethod
    def get_loan_period(self):
        """Return the loan period in days"""
        pass
    
    @abstractmethod
    def get_fine_rate(self):
        """Return the fine rate per day"""
        pass
    
    @abstractmethod
    def get_item_type(self):
        """Return the type of item"""
        pass
    
    def borrow(self, member):
        """Borrow the item to a member"""
        if self.status != ItemStatus.AVAILABLE:
            return False, f"Item is not available (Status: {self.status.value})"
        
        if not member.can_borrow():
            return False, "Member has reached borrowing limit"
        
        self.status = ItemStatus.BORROWED
        self.borrowed_by = member
        self.borrow_date = datetime.now()
        self.due_date = self.borrow_date + timedelta(days=self.get_loan_period())
        
        member.borrowed_items.append(self)
        return True, f"Item borrowed successfully. Due date: {self.due_date.strftime('%Y-%m-%d')}"
    
    def return_item(self):
        """Return the item and calculate fine if overdue"""
        if self.status != ItemStatus.BORROWED:
            return False, "Item was not borrowed"
        
        fine = self.calculate_fine()
        member = self.borrowed_by
        
        # Remove from member's borrowed items
        member.borrowed_items.remove(self)
        
        # Add fine to member's account
        if fine > 0:
            member.add_fine(fine)
        
        # Reset item status
        self.status = ItemStatus.AVAILABLE
        self.borrowed_by = None
        self.due_date = None
        self.borrow_date = None
        
        # Check if someone has reserved this item
        if self.reservation_queue:
            next_member = self.reservation_queue.pop(0)
            self.reserve(next_member)
        
        return True, f"Item returned successfully. Fine: ${fine:.2f}"
    
    def calculate_fine(self):
        """Calculate fine for overdue items"""
        if not self.due_date or datetime.now() <= self.due_date:
            return 0.0
        
        overdue_days = (datetime.now() - self.due_date).days
        return overdue_days * self.get_fine_rate()
    
    def reserve(self, member):
        """Reserve the item for a member"""
        if self.status == ItemStatus.AVAILABLE:
            self.status = ItemStatus.RESERVED
            self.reservation_queue.append(member)
            return True, "Item reserved successfully"
        
        if member not in self.reservation_queue:
            self.reservation_queue.append(member)
            position = len(self.reservation_queue)
            return True, f"Added to reservation queue. Position: {position}"
        
        return False, "You have already reserved this item"
    
    def get_info(self):
        """Get item information"""
        return {
            'item_id': self.item_id,
            'title': self.title,
            'author_or_creator': self.author_or_creator,
            'publication_year': self.publication_year,
            'type': self.get_item_type(),
            'status': self.status.value,
            'due_date': self.due_date.strftime('%Y-%m-%d') if self.due_date else None,
            'reservations': len(self.reservation_queue)
        }

class Book(LibraryItem):
    def __init__(self, title, author, publication_year, isbn, genre, pages):
        super().__init__(title, author, publication_year)
        self.isbn = isbn
        self.genre = genre
        self.pages = pages
    
    def get_loan_period(self):
        return 14  # 2 weeks
    
    def get_fine_rate(self):
        return 0.50  # $0.50 per day
    
    def get_item_type(self):
        return "Book"

class DVD(LibraryItem):
    def __init__(self, title, director, publication_year, duration, rating):
        super().__init__(title, director, publication_year)
        self.duration = duration
        self.rating = rating
    
    def get_loan_period(self):
        return 7  # 1 week
    
    def get_fine_rate(self):
        return 1.00  # $1.00 per day
    
    def get_item_type(self):
        return "DVD"

class Magazine(LibraryItem):
    def __init__(self, title, publisher, publication_year, issue_number, month):
        super().__init__(title, publisher, publication_year)
        self.issue_number = issue_number
        self.month = month
    
    def get_loan_period(self):
        return 3  # 3 days
    
    def get_fine_rate(self):
        return 0.25  # $0.25 per day
    
    def get_item_type(self):
        return "Magazine"

class Member:
    """Library member class with encapsulation"""
    
    member_counter = 0
    
    def __init__(self, name, email, phone, address):
        Member.member_counter += 1
        self._member_id = f"MEM{Member.member_counter:04d}"
        self._name = name
        self._email = email
        self._phone = phone
        self._address = address
        self._borrowed_items = []
        self._fine_amount = 0.0
        self._max_borrow_limit = 5
        self._membership_date = datetime.now()
        self._is_active = True
    
    # Properties for encapsulation
    @property
    def member_id(self):
        return self._member_id
    
    @property
    def name(self):
        return self._name
    
    @property
    def email(self):
        return self._email
    
    @email.setter
    def email(self, value):
        if '@' in value:
            self._email = value
        else:
            raise ValueError("Invalid email format")
    
    @property
    def borrowed_items(self):
        return self._borrowed_items
    
    @property
    def fine_amount(self):
        return self._fine_amount
    
    @property
    def is_active(self):
        return self._is_active
    
    def can_borrow(self):
        """Check if member can borrow more items"""
        return (len(self._borrowed_items) < self._max_borrow_limit and 
                self._is_active and self._fine_amount < 50.0)
    
    def add_fine(self, amount):
        """Add fine to member's account"""
        self._fine_amount += amount
    
    def pay_fine(self, amount):
        """Pay fine"""
        if amount <= self._fine_amount:
            self._fine_amount -= amount
            return True, f"Fine paid. Remaining: ${self._fine_amount:.2f}"
        return False, "Amount exceeds fine balance"
    
    def suspend_membership(self):
        """Suspend member's account"""
        self._is_active = False
    
    def reactivate_membership(self):
        """Reactivate member's account"""
        if self._fine_amount == 0:
            self._is_active = True
            return True, "Membership reactivated"
        return False, "Please pay all fines before reactivation"
    
    def get_member_info(self):
        """Get member information"""
        return {
            'member_id': self._member_id,
            'name': self._name,
            'email': self._email,
            'active': self._is_active,
            'borrowed_items': len(self._borrowed_items),
            'fine_amount': self._fine_amount,
            'membership_date': self._membership_date.strftime('%Y-%m-%d')
        }

class Library:
    """Main library class managing all operations"""
    
    def __init__(self, name, address):
        self.name = name
        self.address = address
        self.items = {}
        self.members = {}
        self.transaction_log = []
    
    def add_item(self, item):
        """Add new item to library"""
        self.items[item.item_id] = item
        self.log_transaction(f"Added item: {item.title} ({item.item_id})")
        return item.item_id
    
    def add_member(self, member):
        """Add new member to library"""
        self.members[member.member_id] = member
        self.log_transaction(f"Added member: {member.name} ({member.member_id})")
        return member.member_id
    
    def find_item(self, item_id):
        """Find item by ID"""
        return self.items.get(item_id)
    
    def find_member(self, member_id):
        """Find member by ID"""
        return self.members.get(member_id)
    
    def search_items(self, query, search_type="title"):
        """Search items by title, author, or type"""
        results = []
        query_lower = query.lower()
        
        for item in self.items.values():
            if search_type == "title" and query_lower in item.title.lower():
                results.append(item)
            elif search_type == "author" and query_lower in item.author_or_creator.lower():
                results.append(item)
            elif search_type == "type" and query_lower in item.get_item_type().lower():
                results.append(item)
        
        return results
    
    def borrow_item(self, item_id, member_id):
        """Process item borrowing"""
        item = self.find_item(item_id)
        member = self.find_member(member_id)
        
        if not item:
            return False, "Item not found"
        if not member:
            return False, "Member not found"
        
        success, message = item.borrow(member)
        if success:
            self.log_transaction(f"Item borrowed: {item.title} by {member.name}")
        
        return success, message
    
    def return_item(self, item_id):
        """Process item return"""
        item = self.find_item(item_id)
        
        if not item:
            return False, "Item not found"
        
        success, message = item.return_item()
        if success:
            self.log_transaction(f"Item returned: {item.title}")
        
        return success, message
    
    def get_overdue_items(self):
        """Get list of overdue items"""
        overdue = []
        for item in self.items.values():
            if (item.status == ItemStatus.BORROWED and 
                item.due_date and datetime.now() > item.due_date):
                overdue.append(item)
        return overdue
    
    def get_library_statistics(self):
        """Get library statistics"""
        total_items = len(self.items)
        available_items = sum(1 for item in self.items.values() 
                            if item.status == ItemStatus.AVAILABLE)
        borrowed_items = sum(1 for item in self.items.values() 
                           if item.status == ItemStatus.BORROWED)
        overdue_items = len(self.get_overdue_items())
        
        total_members = len(self.members)
        active_members = sum(1 for member in self.members.values() 
                           if member.is_active)
        
        return {
            'total_items': total_items,
            'available_items': available_items,
            'borrowed_items': borrowed_items,
            'overdue_items': overdue_items,
            'total_members': total_members,
            'active_members': active_members
        }
    
    def log_transaction(self, message):
        """Log library transactions"""
        timestamp = datetime.now().strftime('%Y-%m-%d %H:%M:%S')
        self.transaction_log.append(f"[{timestamp}] {message}")
    
    def get_transaction_log(self, limit=10):
        """Get recent transaction log"""
        return self.transaction_log[-limit:]

# Usage Example
def main():
    # Create library
    library = Library("Central Library", "123 Main St, City")
    
    # Add items
    book1 = Book("Python Programming", "John Smith", 2023, "978-1234567890", "Programming", 350)
    book2 = Book("Data Science Handbook", "Jane Doe", 2022, "978-0987654321", "Data Science", 450)
    dvd1 = DVD("Learning Python", "Educational Films", 2023, 120, "PG")
    magazine1 = Magazine("Tech Today", "Tech Publishers", 2023, 45, "November")
    
    library.add_item(book1)
    library.add_item(book2)
    library.add_item(dvd1)
    library.add_item(magazine1)
    
    # Add members
    member1 = Member("Alice Johnson", "alice@email.com", "555-1234", "456 Oak St")
    member2 = Member("Bob Smith", "bob@email.com", "555-5678", "789 Pine St")
    
    library.add_member(member1)
    library.add_member(member2)
    
    # Borrow items
    success, message = library.borrow_item(book1.item_id, member1.member_id)
    print(f"Borrow book1: {message}")
    
    success, message = library.borrow_item(dvd1.item_id, member2.member_id)
    print(f"Borrow dvd1: {message}")
    
    # Search items
    results = library.search_items("Python", "title")
    print(f"Search results for 'Python': {len(results)} items found")
    
    # Get statistics
    stats = library.get_library_statistics()
    print(f"Library Statistics: {stats}")
    
    # Get transaction log
    log = library.get_transaction_log()
    print("Recent transactions:")
    for entry in log:
        print(f"  {entry}")

if __name__ == "__main__":
    main()
```

---

## Project 2: Vehicle Rental System

### Project Overview
Create a vehicle rental system with different vehicle types, rental policies, and customer management.

### Key Features
- Multiple vehicle types with different rental rates
- Customer management with rental history
- Reservation system
- Insurance and damage tracking
- Pricing calculations with discounts

### Implementation

```python
from abc import ABC, abstractmethod
from datetime import datetime, timedelta
from enum import Enum
import uuid

class VehicleStatus(Enum):
    AVAILABLE = "Available"
    RENTED = "Rented"
    MAINTENANCE = "Maintenance"
    RESERVED = "Reserved"

class VehicleType(Enum):
    ECONOMY = "Economy"
    COMPACT = "Compact"
    MIDSIZE = "Midsize"
    LUXURY = "Luxury"
    SUV = "SUV"
    TRUCK = "Truck"

class Vehicle(ABC):
    """Abstract base class for all vehicles"""
    
    def __init__(self, make, model, year, license_plate, mileage=0):
        self.vehicle_id = str(uuid.uuid4())[:8]
        self.make = make
        self.model = model
        self.year = year
        self.license_plate = license_plate
        self.mileage = mileage
        self.status = VehicleStatus.AVAILABLE
        self.current_rental = None
        self.maintenance_history = []
        self.rental_history = []
    
    @abstractmethod
    def get_daily_rate(self):
        """Return daily rental rate"""
        pass
    
    @abstractmethod
    def get_vehicle_type(self):
        """Return vehicle type"""
        pass
    
    @abstractmethod
    def get_fuel_efficiency(self):
        """Return fuel efficiency in MPG"""
        pass
    
    def rent(self, customer, rental_days, insurance=False):
        """Rent vehicle to customer"""
        if self.status != VehicleStatus.AVAILABLE:
            return False, f"Vehicle not available (Status: {self.status.value})"
        
        rental = Rental(self, customer, rental_days, insurance)
        self.current_rental = rental
        self.status = VehicleStatus.RENTED
        self.rental_history.append(rental)
        customer.add_rental(rental)
        
        return True, f"Vehicle rented successfully. Total cost: ${rental.total_cost:.2f}"
    
    def return_vehicle(self, return_mileage, damage_description=None):
        """Return vehicle and calculate final charges"""
        if self.status != VehicleStatus.RENTED:
            return False, "Vehicle was not rented"
        
        rental = self.current_rental
        additional_charges = 0
        
        # Calculate mileage charges
        mileage_used = return_mileage - self.mileage
        if mileage_used > rental.included_mileage:
            excess_mileage = mileage_used - rental.included_mileage
            additional_charges += excess_mileage * 0.25  # $0.25 per mile
        
        # Handle damage
        if damage_description:
            damage_cost = self.assess_damage(damage_description)
            additional_charges += damage_cost
        
        # Update vehicle
        self.mileage = return_mileage
        self.status = VehicleStatus.AVAILABLE
        rental.return_vehicle(additional_charges, damage_description)
        self.current_rental = None
        
        return True, f"Vehicle returned. Additional charges: ${additional_charges:.2f}"
    
    def assess_damage(self, damage_description):
        """Assess damage cost (simplified)"""
        damage_lower = damage_description.lower()
        if "minor" in damage_lower or "scratch" in damage_lower:
            return 100.0
        elif "major" in damage_lower or "dent" in damage_lower:
            return 500.0
        elif "accident" in damage_lower:
            return 2000.0
        return 250.0  # Default damage cost
    
    def schedule_maintenance(self, maintenance_type, cost):
        """Schedule maintenance"""
        self.status = VehicleStatus.MAINTENANCE
        maintenance = {
            'date': datetime.now(),
            'type': maintenance_type,
            'cost': cost,
            'mileage': self.mileage
        }
        self.maintenance_history.append(maintenance)
        return f"Maintenance scheduled: {maintenance_type}"
    
    def complete_maintenance(self):
        """Complete maintenance and make vehicle available"""
        if self.status == VehicleStatus.MAINTENANCE:
            self.status = VehicleStatus.AVAILABLE
            return "Maintenance completed. Vehicle is now available."
        return "Vehicle is not under maintenance"
    
    def get_info(self):
        """Get vehicle information"""
        return {
            'vehicle_id': self.vehicle_id,
            'make': self.make,
            'model': self.model,
            'year': self.year,
            'license_plate': self.license_plate,
            'mileage': self.mileage,
            'status': self.status.value,
            'type': self.get_vehicle_type().value,
            'daily_rate': self.get_daily_rate(),
            'fuel_efficiency': self.get_fuel_efficiency()
        }

class Car(Vehicle):
    def __init__(self, make, model, year, license_plate, vehicle_type, doors=4, mileage=0):
        super().__init__(make, model, year, license_plate, mileage)
        self.car_type = vehicle_type
        self.doors = doors
    
    def get_vehicle_type(self):
        return self.car_type
    
    def get_daily_rate(self):
        rates = {
            VehicleType.ECONOMY: 25.00,
            VehicleType.COMPACT: 30.00,
            VehicleType.MIDSIZE: 40.00,
            VehicleType.LUXURY: 80.00
        }
        return rates.get(self.car_type, 35.00)
    
    def get_fuel_efficiency(self):
        efficiency = {
            VehicleType.ECONOMY: 35,
            VehicleType.COMPACT: 30,
            VehicleType.MIDSIZE: 25,
            VehicleType.LUXURY: 20
        }
        return efficiency.get(self.car_type, 25)

class SUV(Vehicle):
    def __init__(self, make, model, year, license_plate, seating_capacity=7, mileage=0):
        super().__init__(make, model, year, license_plate, mileage)
        self.seating_capacity = seating_capacity
    
    def get_vehicle_type(self):
        return VehicleType.SUV
    
    def get_daily_rate(self):
        return 60.00
    
    def get_fuel_efficiency(self):
        return 18

class Truck(Vehicle):
    def __init__(self, make, model, year, license_plate, cargo_capacity, mileage=0):
        super().__init__(make, model, year, license_plate, mileage)
        self.cargo_capacity = cargo_capacity
    
    def get_vehicle_type(self):
        return VehicleType.TRUCK
    
    def get_daily_rate(self):
        return 75.00
    
    def get_fuel_efficiency(self):
        return 15

class Customer:
    """Customer class with rental history and preferences"""
    
    customer_counter = 0
    
    def __init__(self, name, email, phone, driver_license, date_of_birth):
        Customer.customer_counter += 1
        self.customer_id = f"CUST{Customer.customer_counter:04d}"
        self.name = name
        self.email = email
        self.phone = phone
        self.driver_license = driver_license
        self.date_of_birth = date_of_birth
        self.rental_history = []
        self.loyalty_points = 0
        self.preferred_vehicle_type = None
        self.registration_date = datetime.now()
    
    def add_rental(self, rental):
        """Add rental to customer history"""
        self.rental_history.append(rental)
        # Award loyalty points (1 point per dollar spent)
        self.loyalty_points += int(rental.total_cost)
    
    def get_loyalty_discount(self):
        """Calculate loyalty discount based on points"""
        if self.loyalty_points >= 1000:
            return 0.15  # 15% discount
        elif self.loyalty_points >= 500:
            return 0.10  # 10% discount
        elif self.loyalty_points >= 100:
            return 0.05  # 5% discount
        return 0.0
    
    def get_rental_count(self):
        """Get total number of rentals"""
        return len(self.rental_history)
    
    def get_customer_info(self):
        """Get customer information"""
        return {
            'customer_id': self.customer_id,
            'name': self.name,
            'email': self.email,
            'phone': self.phone,
            'rental_count': self.get_rental_count(),
            'loyalty_points': self.loyalty_points,
            'loyalty_discount': f"{self.get_loyalty_discount() * 100:.0f}%",
            'registration_date': self.registration_date.strftime('%Y-%m-%d')
        }

class Rental:
    """Rental transaction class"""
    
    rental_counter = 0
    
    def __init__(self, vehicle, customer, rental_days, insurance=False):
        Rental.rental_counter += 1
        self.rental_id = f"RENT{Rental.rental_counter:04d}"
        self.vehicle = vehicle
        self.customer = customer
        self.rental_days = rental_days
        self.insurance = insurance
        self.start_date = datetime.now()
        self.end_date = self.start_date + timedelta(days=rental_days)
        self.actual_return_date = None
        self.included_mileage = rental_days * 100  # 100 miles per day
        self.additional_charges = 0
        self.damage_description = None
        self.total_cost = self.calculate_total_cost()
    
    def calculate_total_cost(self):
        """Calculate total rental cost"""
        base_cost = self.vehicle.get_daily_rate() * self.rental_days
        
        # Insurance cost
        insurance_cost = 0
        if self.insurance:
            insurance_cost = base_cost * 0.15  # 15% of base cost
        
        # Apply loyalty discount
        discount = self.customer.get_loyalty_discount()
        discounted_cost = base_cost * (1 - discount)
        
        # Add taxes
        tax = (discounted_cost + insurance_cost) * 0.08  # 8% tax
        
        return discounted_cost + insurance_cost + tax
    
    def return_vehicle(self, additional_charges, damage_description=None):
        """Process vehicle return"""
        self.actual_return_date = datetime.now()
        self.additional_charges = additional_charges
        self.damage_description = damage_description
        
        # Calculate late fees if returned late
        if self.actual_return_date > self.end_date:
            late_days = (self.actual_return_date - self.end_date).days
            late_fee = late_days * self.vehicle.get_daily_rate() * 1.5  # 1.5x daily rate
            self.additional_charges += late_fee
        
        self.total_cost += self.additional_charges
    
    def get_rental_summary(self):
        """Get rental summary"""
        return {
            'rental_id': self.rental_id,
            'vehicle': f"{self.vehicle.make} {self.vehicle.model}",
            'customer': self.customer.name,
            'rental_days': self.rental_days,
            'start_date': self.start_date.strftime('%Y-%m-%d'),
            'end_date': self.end_date.strftime('%Y-%m-%d'),
            'actual_return_date': self.actual_return_date.strftime('%Y-%m-%d') if self.actual_return_date else None,
            'total_cost': self.total_cost,
            'additional_charges': self.additional_charges,
            'insurance': self.insurance,
            'damage': self.damage_description
        }

class RentalAgency:
    """Main rental agency class"""
    
    def __init__(self, name, address):
        self.name = name
        self.address = address
        self.vehicles = {}
        self.customers = {}
        self.rentals = []
        self.revenue = 0.0
    
    def add_vehicle(self, vehicle):
        """Add vehicle to fleet"""
        self.vehicles[vehicle.vehicle_id] = vehicle
        return f"Vehicle added: {vehicle.make} {vehicle.model} ({vehicle.vehicle_id})"
    
    def add_customer(self, customer):
        """Add customer to system"""
        self.customers[customer.customer_id] = customer
        return f"Customer added: {customer.name} ({customer.customer_id})"
    
    def find_available_vehicles(self, vehicle_type=None):
        """Find available vehicles by type"""
        available = []
        for vehicle in self.vehicles.values():
            if vehicle.status == VehicleStatus.AVAILABLE:
                if vehicle_type is None or vehicle.get_vehicle_type() == vehicle_type:
                    available.append(vehicle)
        return available
    
    def rent_vehicle(self, vehicle_id, customer_id, rental_days, insurance=False):
        """Process vehicle rental"""
        vehicle = self.vehicles.get(vehicle_id)
        customer = self.customers.get(customer_id)
        
        if not vehicle:
            return False, "Vehicle not found"
        if not customer:
            return False, "Customer not found"
        
        success, message = vehicle.rent(customer, rental_days, insurance)
        if success:
            self.rentals.append(vehicle.current_rental)
            self.revenue += vehicle.current_rental.total_cost
        
        return success, message
    
    def return_vehicle(self, vehicle_id, return_mileage, damage_description=None):
        """Process vehicle return"""
        vehicle = self.vehicles.get(vehicle_id)
        
        if not vehicle:
            return False, "Vehicle not found"
        
        return vehicle.return_vehicle(return_mileage, damage_description)
    
    def get_fleet_statistics(self):
        """Get fleet statistics"""
        total_vehicles = len(self.vehicles)
        available = sum(1 for v in self.vehicles.values() if v.status == VehicleStatus.AVAILABLE)
        rented = sum(1 for v in self.vehicles.values() if v.status == VehicleStatus.RENTED)
        maintenance = sum(1 for v in self.vehicles.values() if v.status == VehicleStatus.MAINTENANCE)
        
        return {
            'total_vehicles': total_vehicles,
            'available': available,
            'rented': rented,
            'maintenance': maintenance,
            'total_rentals': len(self.rentals),
            'total_revenue': self.revenue,
            'total_customers': len(self.customers)
        }

# Usage Example
def main():
    # Create rental agency
    agency = RentalAgency("Premium Car Rentals", "123 Business Ave")
    
    # Add vehicles
    car1 = Car("Toyota", "Camry", 2023, "ABC-1234", VehicleType.MIDSIZE)
    car2 = Car("Honda", "Civic", 2022, "XYZ-5678", VehicleType.COMPACT)
    suv1 = SUV("Ford", "Explorer", 2023, "SUV-9012", 7)
    truck1 = Truck("Chevrolet", "Silverado", 2023, "TRK-3456", 2000)
    
    agency.add_vehicle(car1)
    agency.add_vehicle(car2)
    agency.add_vehicle(suv1)
    agency.add_vehicle(truck1)
    
    # Add customers
    customer1 = Customer("John Doe", "john@email.com", "555-1234", "DL123456", "1990-01-01")
    customer2 = Customer("Jane Smith", "jane@email.com", "555-5678", "DL789012", "1985-05-15")
    
    agency.add_customer(customer1)
    agency.add_customer(customer2)
    
    # Rent vehicles
    success, message = agency.rent_vehicle(car1.vehicle_id, customer1.customer_id, 5, insurance=True)
    print(f"Rental 1: {message}")
    
    success, message = agency.rent_vehicle(suv1.vehicle_id, customer2.customer_id, 3, insurance=False)
    print(f"Rental 2: {message}")
    
    # Return vehicles
    success, message = agency.return_vehicle(car1.vehicle_id, 150, "Minor scratch on door")
    print(f"Return 1: {message}")
    
    # Get statistics
    stats = agency.get_fleet_statistics()
    print(f"Fleet Statistics: {stats}")
    
    # Customer info
    print(f"Customer 1 Info: {customer1.get_customer_info()}")

if __name__ == "__main__":
    main()
```

---

## Project 3: Game Character System

### Project Overview
Design a comprehensive game character system with multiple inheritance, abilities, equipment, and progression.

### Key Features
- Character classes with different abilities
- Equipment system with stat bonuses
- Skill progression and leveling
- Combat system with polymorphism
- Inventory management

### Implementation

```python
from abc import ABC, abstractmethod
from enum import Enum
import random

class CharacterClass(Enum):
    WARRIOR = "Warrior"
    MAGE = "Mage"
    ROGUE = "Rogue"
    RANGER = "Ranger"
    CLERIC = "Cleric"

class ItemType(Enum):
    WEAPON = "Weapon"
    ARMOR = "Armor"
    ACCESSORY = "Accessory"
    CONSUMABLE = "Consumable"

class Ability(ABC):
    """Abstract base class for character abilities"""
    
    def __init__(self, name, cost, cooldown=0):
        self.name = name
        self.cost = cost
        self.cooldown = cooldown
        self.current_cooldown = 0
    
    @abstractmethod
    def use(self, user, target=None):
        """Use the ability"""
        pass
    
    def can_use(self, user):
        """Check if ability can be used"""
        return self.current_cooldown == 0 and user.current_mana >= self.cost
    
    def reduce_cooldown(self):
        """Reduce cooldown by 1"""
        if self.current_cooldown > 0:
            self.current_cooldown -= 1

class AttackAbility(Ability):
    def __init__(self, name, cost, damage, cooldown=0):
        super().__init__(name, cost, cooldown)
        self.damage = damage
    
    def use(self, user, target):
        if not self.can_use(user):
            return False, "Cannot use ability"
        
        user.current_mana -= self.cost
        self.current_cooldown = self.cooldown
        
        damage_dealt = self.damage + (user.strength * 0.5)
        target.take_damage(damage_dealt)
        
        return True, f"{user.name} used {self.name} for {damage_dealt} damage"

class HealAbility(Ability):
    def __init__(self, name, cost, healing, cooldown=0):
        super().__init__(name, cost, cooldown)
        self.healing = healing
    
    def use(self, user, target=None):
        if not self.can_use(user):
            return False, "Cannot use ability"
        
        target = target or user
        user.current_mana -= self.cost
        self.current_cooldown = self.cooldown
        
        healing_amount = self.healing + (user.intelligence * 0.3)
        target.heal(healing_amount)
        
        return True, f"{user.name} healed {target.name} for {healing_amount} HP"

class Item:
    """Base class for all items"""
    
    def __init__(self, name, item_type, value=0, stat_bonuses=None):
        self.name = name
        self.item_type = item_type
        self.value = value
        self.stat_bonuses = stat_bonuses or {}
    
    def get_stat_bonus(self, stat):
        """Get stat bonus for a specific stat"""
        return self.stat_bonuses.get(stat, 0)
    
    def get_info(self):
        """Get item information"""
        info = f"{self.name} ({self.item_type.value})"
        if self.stat_bonuses:
            bonuses = ", ".join([f"{stat}: +{bonus}" for stat, bonus in self.stat_bonuses.items()])
            info += f" - {bonuses}"
        return info

class Equipment:
    """Equipment management for characters"""
    
    def __init__(self):
        self.weapon = None
        self.armor = None
        self.accessory = None
    
    def equip_item(self, item):
        """Equip an item"""
        if item.item_type == ItemType.WEAPON:
            self.weapon = item
        elif item.item_type == ItemType.ARMOR:
            self.armor = item
        elif item.item_type == ItemType.ACCESSORY:
            self.accessory = item
        else:
            return False, "Cannot equip this item type"
        
        return True, f"Equipped {item.name}"
    
    def unequip_item(self, item_type):
        """Unequip an item"""
        if item_type == ItemType.WEAPON:
            item = self.weapon
            self.weapon = None
        elif item_type == ItemType.ARMOR:
            item = self.armor
            self.armor = None
        elif item_type == ItemType.ACCESSORY:
            item = self.accessory
            self.accessory = None
        else:
            return None, "Invalid item type"
        
        return item, f"Unequipped {item.name if item else 'nothing'}"
    
    def get_stat_bonus(self, stat):
        """Get total stat bonus from all equipment"""
        total_bonus = 0
        for item in [self.weapon, self.armor, self.accessory]:
            if item:
                total_bonus += item.get_stat_bonus(stat)
        return total_bonus
    
    def get_equipped_items(self):
        """Get list of equipped items"""
        items = []
        if self.weapon:
            items.append(self.weapon)
        if self.armor:
            items.append(self.armor)
        if self.accessory:
            items.append(self.accessory)
        return items

class Inventory:
    """Inventory management for characters"""
    
    def __init__(self, capacity=20):
        self.capacity = capacity
        self.items = []
    
    def add_item(self, item):
        """Add item to inventory"""
        if len(self.items) < self.capacity:
            self.items.append(item)
            return True, f"Added {item.name} to inventory"
        return False, "Inventory is full"
    
    def remove_item(self, item_name):
        """Remove item from inventory"""
        for i, item in enumerate(self.items):
            if item.name == item_name:
                return self.items.pop(i), f"Removed {item_name} from inventory"
        return None, f"{item_name} not found in inventory"
    
    def find_item(self, item_name):
        """Find item in inventory"""
        for item in self.items:
            if item.name == item_name:
                return item
        return None
    
    def get_items_by_type(self, item_type):
        """Get items of specific type"""
        return [item for item in self.items if item.item_type == item_type]
    
    def get_inventory_info(self):
        """Get inventory information"""
        return {
            'capacity': self.capacity,
            'used': len(self.items),
            'items': [item.get_info() for item in self.items]
        }

class Character(ABC):
    """Abstract base class for all characters"""
    
    def __init__(self, name, level=1):
        self.name = name
        self.level = level
        self.experience = 0
        self.experience_to_next_level = 100
        
        # Base stats
        self.max_health = 100
        self.current_health = self.max_health
        self.max_mana = 50
        self.current_mana = self.max_mana
        
        # Primary stats
        self.strength = 10
        self.intelligence = 10
        self.agility = 10
        self.vitality = 10
        
        # Equipment and inventory
        self.equipment = Equipment()
        self.inventory = Inventory()
        
        # Abilities
        self.abilities = []
        
        # Initialize class-specific stats
        self.initialize_stats()
    
    @abstractmethod
    def initialize_stats(self):
        """Initialize class-specific stats"""
        pass
    
    @abstractmethod
    def get_character_class(self):
        """Get character class"""
        pass
    
    def get_total_stat(self, stat):
        """Get total stat including equipment bonuses"""
        base_stat = getattr(self, stat)
        equipment_bonus = self.equipment.get_stat_bonus(stat)
        return base_stat + equipment_bonus
    
    def take_damage(self, damage):
        """Take damage"""
        # Apply damage reduction based on vitality
        damage_reduction = self.get_total_stat('vitality') * 0.1
        actual_damage = max(0, damage - damage_reduction)
        
        self.current_health = max(0, self.current_health - actual_damage)
        
        if self.current_health == 0:
            return f"{self.name} has been defeated!"
        
        return f"{self.name} takes {actual_damage} damage. HP: {self.current_health}/{self.max_health}"
    
    def heal(self, amount):
        """Heal character"""
        self.current_health = min(self.max_health, self.current_health + amount)
        return f"{self.name} healed for {amount} HP. HP: {self.current_health}/{self.max_health}"
    
    def restore_mana(self, amount):
        """Restore mana"""
        self.current_mana = min(self.max_mana, self.current_mana + amount)
        return f"{self.name} restored {amount} MP. MP: {self.current_mana}/{self.max_mana}"
    
    def gain_experience(self, exp):
        """Gain experience and level up if enough"""
        self.experience += exp
        
        if self.experience >= self.experience_to_next_level:
            return self.level_up()
        
        return f"{self.name} gained {exp} experience"
    
    def level_up(self):
        """Level up character"""
        self.level += 1
        self.experience -= self.experience_to_next_level
        self.experience_to_next_level = int(self.experience_to_next_level * 1.5)
        
        # Increase stats
        self.max_health += 20
        self.current_health = self.max_health
        self.max_mana += 10
        self.current_mana = self.max_mana
        
        # Class-specific level up bonuses
        self.level_up_bonuses()
        
        return f"{self.name} leveled up to level {self.level}!"
    
    @abstractmethod
    def level_up_bonuses(self):
        """Apply class-specific level up bonuses"""
        pass
    
    def use_ability(self, ability_name, target=None):
        """Use an ability"""
        for ability in self.abilities:
            if ability.name == ability_name:
                return ability.use(self, target)
        return False, "Ability not found"
    
    def equip_item(self, item_name):
        """Equip an item from inventory"""
        item = self.inventory.find_item(item_name)
        if not item:
            return False, "Item not found in inventory"
        
        # Remove from inventory
        self.inventory.remove_item(item_name)
        
        # Equip item
        success, message = self.equipment.equip_item(item)
        
        if not success:
            # If equipping failed, add back to inventory
            self.inventory.add_item(item)
        
        return success, message
    
    def get_character_info(self):
        """Get character information"""
        return {
            'name': self.name,
            'class': self.get_character_class().value,
            'level': self.level,
            'experience': self.experience,
            'health': f"{self.current_health}/{self.max_health}",
            'mana': f"{self.current_mana}/{self.max_mana}",
            'stats': {
                'strength': self.get_total_stat('strength'),
                'intelligence': self.get_total_stat('intelligence'),
                'agility': self.get_total_stat('agility'),
                'vitality': self.get_total_stat('vitality')
            },
            'equipment': [item.get_info() for item in self.equipment.get_equipped_items()],
            'abilities': [ability.name for ability in self.abilities]
        }

class Warrior(Character):
    def initialize_stats(self):
        self.strength = 15
        self.vitality = 13
        self.max_health = 120
        self.current_health = self.max_health
        
        # Warrior abilities
        self.abilities.append(AttackAbility("Sword Strike", 5, 20, 0))
        self.abilities.append(AttackAbility("Power Attack", 10, 35, 2))
    
    def get_character_class(self):
        return CharacterClass.WARRIOR
    
    def level_up_bonuses(self):
        self.strength += 2
        self.vitality += 2
        self.intelligence += 1

class Mage(Character):
    def initialize_stats(self):
        self.intelligence = 15
        self.agility = 12
        self.max_mana = 80
        self.current_mana = self.max_mana
        
        # Mage abilities
        self.abilities.append(AttackAbility("Fireball", 15, 30, 1))
        self.abilities.append(AttackAbility("Lightning Bolt", 20, 40, 3))
        self.abilities.append(HealAbility("Heal", 12, 25, 0))
    
    def get_character_class(self):
        return CharacterClass.MAGE
    
    def level_up_bonuses(self):
        self.intelligence += 3
        self.agility += 1
        self.vitality += 1

class Rogue(Character):
    def initialize_stats(self):
        self.agility = 15
        self.strength = 12
        self.max_health = 90
        self.current_health = self.max_health
        
        # Rogue abilities
        self.abilities.append(AttackAbility("Backstab", 8, 25, 0))
        self.abilities.append(AttackAbility("Sneak Attack", 15, 45, 4))
    
    def get_character_class(self):
        return CharacterClass.ROGUE
    
    def level_up_bonuses(self):
        self.agility += 2
        self.strength += 2
        self.intelligence += 1

class GameWorld:
    """Game world managing characters and combat"""
    
    def __init__(self):
        self.characters = []
        self.items = self.create_items()
    
    def create_items(self):
        """Create sample items"""
        items = [
            Item("Iron Sword", ItemType.WEAPON, 100, {"strength": 5}),
            Item("Steel Armor", ItemType.ARMOR, 200, {"vitality": 8}),
            Item("Magic Ring", ItemType.ACCESSORY, 150, {"intelligence": 3, "mana": 10}),
            Item("Health Potion", ItemType.CONSUMABLE, 25),
            Item("Mana Potion", ItemType.CONSUMABLE, 25)
        ]
        return items
    
    def add_character(self, character):
        """Add character to world"""
        self.characters.append(character)
        return f"{character.name} has entered the world"
    
    def give_item_to_character(self, character_name, item_name):
        """Give item to character"""
        character = self.find_character(character_name)
        if not character:
            return False, "Character not found"
        
        item = self.find_item(item_name)
        if not item:
            return False, "Item not found"
        
        return character.inventory.add_item(item)
    
    def find_character(self, name):
        """Find character by name"""
        for character in self.characters:
            if character.name == name:
                return character
        return None
    
    def find_item(self, name):
        """Find item by name"""
        for item in self.items:
            if item.name == name:
                return item
        return None
    
    def combat_turn(self, attacker_name, defender_name, ability_name):
        """Process combat turn"""
        attacker = self.find_character(attacker_name)
        defender = self.find_character(defender_name)
        
        if not attacker or not defender:
            return "One or both characters not found"
        
        if attacker.current_health <= 0:
            return f"{attacker.name} is defeated and cannot act"
        
        if defender.current_health <= 0:
            return f"{defender.name} is already defeated"
        
        success, message = attacker.use_ability(ability_name, defender)
        
        # Reduce cooldowns
        for ability in attacker.abilities:
            ability.reduce_cooldown()
        
        return message

# Usage Example
def main():
    # Create game world
    world = GameWorld()
    
    # Create characters
    warrior = Warrior("Conan", 1)
    mage = Mage("Gandalf", 1)
    rogue = Rogue("Legolas", 1)
    
    # Add characters to world
    world.add_character(warrior)
    world.add_character(mage)
    world.add_character(rogue)
    
    # Give items to characters
    world.give_item_to_character("Conan", "Iron Sword")
    world.give_item_to_character("Conan", "Steel Armor")
    world.give_item_to_character("Gandalf", "Magic Ring")
    
    # Equip items
    warrior.equip_item("Iron Sword")
    warrior.equip_item("Steel Armor")
    mage.equip_item("Magic Ring")
    
    # Display character info
    print("Character Information:")
    for character in world.characters:
        info = character.get_character_info()
        print(f"\n{info['name']} (Level {info['level']} {info['class']})")
        print(f"  Health: {info['health']}, Mana: {info['mana']}")
        print(f"  Stats: {info['stats']}")
        print(f"  Equipment: {info['equipment']}")
        print(f"  Abilities: {info['abilities']}")
    
    # Combat example
    print("\n=== Combat Example ===")
    result = world.combat_turn("Conan", "Gandalf", "Power Attack")
    print(result)
    
    result = world.combat_turn("Gandalf", "Conan", "Fireball")
    print(result)
    
    # Level up example
    print("\n=== Level Up Example ===")
    result = warrior.gain_experience(100)
    print(result)
    
    print(f"Updated Character Info:")
    info = warrior.get_character_info()
    print(f"  Health: {info['health']}, Mana: {info['mana']}")
    print(f"  Stats: {info['stats']}")

if __name__ == "__main__":
    main()
```

This comprehensive example demonstrates:

1. **Abstract Base Classes**: `Character` and `Ability` classes define interfaces
2. **Inheritance**: Different character classes inherit from `Character`
3. **Polymorphism**: Different abilities work through the same interface
4. **Encapsulation**: Stats, equipment, and inventory are properly encapsulated
5. **Composition**: Characters contain equipment and inventory objects
6. **Multiple Inheritance**: Could be extended with mixins for special abilities

The system is extensible and demonstrates real-world OOP patterns used in game development.

## Advanced Challenges

### Challenge 1: Design Pattern Implementation
Implement common design patterns in your OOP projects:
- Factory Pattern for creating objects
- Observer Pattern for event handling
- Strategy Pattern for different algorithms
- Decorator Pattern for adding functionality

### Challenge 2: Performance Optimization
Optimize your OOP code for performance:
- Use `__slots__` for memory efficiency
- Implement lazy loading for expensive operations
- Use property caching where appropriate
- Profile your code to identify bottlenecks

### Challenge 3: Testing and Documentation
Create comprehensive tests and documentation:
- Unit tests for all classes and methods
- Integration tests for complex interactions
- Docstrings following PEP 257
- Type hints for better code clarity

### Challenge 4: Error Handling
Implement robust error handling:
- Custom exception classes
- Proper exception hierarchy
- Context managers for resource management
- Graceful degradation strategies

## Code Review Guidelines

When reviewing OOP code, consider:

1. **Class Design**: Are classes focused on a single responsibility?
2. **Inheritance**: Is inheritance used appropriately, not just for code reuse?
3. **Polymorphism**: Can objects be used interchangeably through common interfaces?
4. **Encapsulation**: Are internal details properly hidden from external code?
5. **Composition vs Inheritance**: Is composition used where it's more appropriate?
6. **SOLID Principles**: Does the code follow SOLID design principles?
7. **Code Reusability**: Can components be easily reused in different contexts?
8. **Maintainability**: Is the code easy to understand and modify?

## Summary

These practical projects demonstrate how to apply OOP principles in real-world scenarios. Each project showcases different aspects of object-oriented design and helps you understand how to structure complex systems using classes, inheritance, polymorphism, and encapsulation.

Practice these projects, experiment with different implementations, and gradually increase complexity as you become more comfortable with OOP concepts. The key is to think in terms of objects and their relationships rather than just procedural code.
