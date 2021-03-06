# Midtown Comics, Pt. 1

## Summary

Software development is all about building useful tools and applications. We've spent a lot of time completing exercises designed to solidify concepts in computer science, but now we're going to work on some more tangible projects.

We're going to build an admittedly watered down point-of-sale system for Midtown Comics. Our point-of-sale system is going to handle both in-store purchases, as well as orders that will be shipped to the customer. To keep things simple, we're going to assume the shop only sells comic books.

## Application Design

Before we starting writing any code, it's important to outline the design and structure of your application. It's not very flashy, but it will save you a lot of time and headaches down the road.

* Product \(in this case, a comic book\)
* Customer
* Order

These are the three primary building blocks upon which our application will be built. Customers are going to come into our shop and place an order. Those orders will contain one or more products. It's not complicated, but it is important that we get this part right.

We're using a model-view-controller \(MVC\) approach to writing our program, and each of these entities are going to be part of what you'll later to come to understand as the model. When you think of the model, think of real things we're trying to digitally represent in code.

The view is how we'll present this information to the user. For our purposes, the user of a point-of-sale system will be an employee of Midtown Comics. Like most applications, we'll have more than one view that will depend on what the user's trying to do. Again, we'll dive much more deeply into this later.

The last piece to the puzzle is the controller. It's the glue that keeps everything together. It sits between the model and the view, and manages their communication. The view will report certain actions the user takes that may impact the model, and the controller will route that information where it needs to go. Likewise, if the model data changes, the controller is responsible for communicating this to the view.

## Model

Each of our model entities are going to become their own classes.

* `Product`
* `Customer`
* `Order`
* `OrderItem`

### Product

Let's start with `Product`, since we wouldn't be much of a comic book shop without comic books. We're going to want to keep track of certain pieces of information about each comic book. Most of this should be pretty intuitive.

* Product ID
* Title
* Author
* Release date
* Issue number
* Price
* Remaining copies

The question is how do we convert this into a Java class. Remember, actions generally translate to methods and data corresponds to instance variables. All of this is data. The only actions we'd likely need to perform is retrieving or changing the data, which will take the form of getters and setters.

{% code title="Product.java" %}
```java
package org.ucvts.comics.model;

public class Product {

    private static long lastProductId = 1L; // initial product ID

    private long productId;
    private String title;
    private String author;
    private long releaseDate;
    private int issue;
    private double unitPrice;
    private int copies;

    /**
     * Creates a default instance of the Product class.
     */

    public Product() {
        this.productId = Product.lastProductId++; // auto-generate ID
    }

    /**
     * Creates an instance of the Product class.
     * 
     * @param productId   the product ID
     * @param title       the title
     * @param author      the creator or artist
     * @param releaseDate the date it was released
     * @param issue       the issue number
     * @param unitPrice   the price
     * @param copies      the number of remaining copies
     */

    public Product(long productId, String title, String author, long releaseDate, int issue, double unitPrice,
            int copies)
    {
        this.productId = productId;
        this.title = title;
        this.author = author;
        this.releaseDate = releaseDate;
        this.issue = issue;
        this.unitPrice = unitPrice;
        this.copies = copies;
    }

    /**
     * Creates an instance of the Product class.
     * 
     * @param title       the title
     * @param author      the creator or artist
     * @param releaseDate the date it was released
     * @param issue       the issue number
     * @param unitPrice   the price
     * @param copies      the number of remaining copies
     */

    public Product(String title, String author, long releaseDate, int issue, double unitPrice, int copies) {
        this.productId = Product.lastProductId++; // auto-generate ID
        this.title = title;
        this.author = author;
        this.releaseDate = releaseDate;
        this.issue = issue;
        this.unitPrice = unitPrice;
        this.copies = copies;
    }

    /**
     * Returns the product ID.
     *
     * @return productId
     */

    public long getProductId() {
        return productId;
    }

    /**
     * Returns the title of the comic book.
     *
     * @return title
     */
     
    public String getTitle() {
        return title;
    }

    /**
     * Returns the author.
     *
     * @return author
     */

    public String getAuthor() {
        return author;
    }
    
    /**
     * Returns the date this issue was released.
     *
     * @return releaseDate
     */

    public long getReleaseDate() {
        return releaseDate;
    }

    /**
     * Returns the issue number.
     *
     * @return issue
     */
     
    public int getIssue() {
        return issue;
    }

    /**
     * Returns the price of one copy.
     *
     * @return unitPrice
     */
     
    public double getUnitPrice() {
        return unitPrice;
    }

    /**
     * Sets the price of one copy.
     *
     * @param unitPrice the new price
     */
     
    public void setUnitPrice(double unitPrice) {
        this.unitPrice = unitPrice;
    }

    /**
     * Returns the number of copies available.
     *
     * @return copies
     */
     
    public int getCopies() {
        return copies;
    }

    /**
     * Sets the number of copies available.
     *
     * @param copies the new number of copies
     */
     
    public void setCopies(int copies) {
        this.copies = copies;
    }
}
```
{% endcode %}

Model classes are traditionally pretty simple. They represent an entity in our application, but don't do a whole lot beyond that. If you're paying attention, you'll notice I included setter methods only for data that might reasonably change. Everything has a getter method.

A note on the `releaseDate`. This is a number that we're using to store a date in `YYYYMMDD` format.

### Customer

Next, let's move on to our `Customer` class. We'll need to have both contact and shipping information on file, since customers often request that we ship their orders to them.

* Customer ID
* First name
* Last name
* Telephone number
* Email address
* Mailing address

We'll convert this to a Java class just like we did with the `Product` class.

{% code title="Customer.java" %}
```java
package org.ucvts.comics.model;

public class Customer {

    private static long lastCustomerId = 1L; // initial customer ID

    private long customerId;
    private String firstName;
    private String lastName;
    private long phone;
    private String email;
    private String streetAddress;
    private String city;
    private String state;
    private String postalCode;

    /**
     * Creates a default instance of the Customer class.
     */

    public Customer() {
        this.customerId = Customer.lastCustomerId++; // auto-generate ID
    }

    /**
     * Creates an instance of the Customer class.
     * 
     * @param firstName     the first name
     * @param lastName      the last name
     * @param phone         the telephone number
     * @param email         the email address
     * @param streetAddress the street address
     * @param city          the city
     * @param state         the state
     * @param postalCode    the postal code
     */

    public Customer(long customerId, String firstName, String lastName, long phone, String email, String streetAddress,
            String city, String state, String postalCode)
    {
        this.customerId = customerId;
        this.firstName = firstName;
        this.lastName = lastName;
        this.phone = phone;
        this.email = email;
        this.streetAddress = streetAddress;
        this.city = city;
        this.state = state;
        this.postalCode = postalCode;
    }

    /**
     * Returns the customer ID.
     *
     * @return customerId
     */

    public long getCustomerId() {
        return customerId;
    }
    
    /**
     * Returns the customer's first name.
     *
     * @return firstName
     */

    public String getFirstName() {
        return firstName;
    }

    /**
     * Sets the customer's first name.
     *
     * @param firstName the new first name
     */
     
    public void setFirstName(String firstName) {
        this.firstName = firstName;
    }
    
    /**
     * Returns the customer's last name.
     *
     * @return lastName
     */

    public String getLastName() {
        return lastName;
    }
    
    /**
     * Sets the customer's last name.
     *
     * @param lastName the new last name
     */

    public void setLastName(String lastName) {
        this.lastName = lastName;
    }
    
    /**
     * Returns the customer's phone number.
     *
     * @return phone
     */
     
    public long getPhone() {
        return phone;
    }
    
    /**
     * Sets the customer's phone number.
     *
     * @param phone the new phone number
     */

    public void setPhone(long phone) {
        this.phone = phone;
    }
    
    /**
     * Returns the customer's email address.
     *
     * @return email
     */
     
    public String getEmail() {
        return email;
    }
    
    /**
     * Sets the customer's email address.
     *
     * @param email the new email address
     */

    public void setEmail(String email) {
        this.email = email;
    }
    
    /**
     * Returns the customer's street address.
     *
     * @return streetAddress
     */

    public String getStreetAddress() {
        return streetAddress;
    }
    
    /**
     * Sets the customer's street address.
     *
     * @param streetAddress the new street address
     */

    public void setStreetAddress(String streetAddress) {
        this.streetAddress = streetAddress;
    }
    
    /**
     * Returns the city in which the cutomer lives.
     *
     * @return city
     */

    public String getCity() {
        return city;
    }
    
    /**
     * Sets the city in which the customer lives.
     *
     * @param city the new city
     */

    public void setCity(String city) {
        this.city = city;
    }
    
    /**
     * Returns the state in which the customer lives.
     *
     * @return state
     */

    public String getState() {
        return state;
    }
    
    /**
     * Sets the state in which the customer lives.
     *
     * @param state the new state
     */

    public void setState(String state) {
        this.state = state;
    }
    
    /**
     * Returns the postal code in which the customer lives.
     *
     * @return postalCode
     */

    public String getPostalCode() {
        return postalCode;
    }
    
    /**
     * Sets the postal code in which the customer lives.
     *
     * @param postalCode the new postalCode
     */

    public void setPostalCode(String postalCode) {
        this.postalCode = postalCode;
    }
}
```
{% endcode %}

I broke the mailing address out into its component parts. For the sake of simplicity, we're going to pretend addresses never have secondary street address lines. This time, everything except the customer ID has a setter method, since all of this information can conceivably change.

### Order

Let's tackle the `Order` class. We have products and customers want to buy them.

* Order ID
* Customer placing the order
* Date the order was placed
* Status
* Date the order was shipped \(if applicable\)
* List of items in the order
* Total price

Alright, another time through. This will be slightly more complicated than the last two, simply because we're looking at data types with which we might not be as familiar.

{% code title="Order.java" %}
```java
package org.ucvts.comics.model;

import java.time.LocalDateTime;
import java.util.ArrayList;

public class Order {

    private static long lastOrderId = 1L; // initial order ID

    private long orderId;
    private Customer customer;
    private long orderDate;
    private String status;
    private ArrayList<OrderItem> items;
    private double total;

    /**
     * Creates a default instance of the Order class.
     */

    public Order() {
        this.orderId = Order.lastOrderId++; // auto-generate ID
        this.customer = new Customer();
        this.orderDate = getDate();
        this.status = "Open";
        this.items = new ArrayList<>();
        this.total = 0;
    }

    /**
     * Creates an instance of the Order class.
     * 
     * @param orderId   the order ID
     * @param customer  the customer who placed the order
     * @param orderDate the date the order was placed
     * @param status    the status of the order
     * @param items     the items included in the order
     * @param total     the total price of the order
     */

    public Order(long orderId, Customer customer, long orderDate, String status, ArrayList<OrderItem> items,
            double total)
    {
        this.orderId = orderId;
        this.customer = customer;
        this.orderDate = orderDate;
        this.status = status;
        this.items = items;
        this.total = total;
    }

    /**
     * Adds an OrderItem to the Order.
     * 
     * @param item the item to be added
     */

    public void addItem(OrderItem item) {
        if (items == null) {
            items = new ArrayList<>();
        }

        items.add(item);
        this.updateTotal();
    }

    /**
     * Removes an OrderItem from the Order.
     * 
     * @param item the item to be removed
     */

    public void removeItem(OrderItem item) {
        if (items != null) {
            items.remove(item);
        }

        this.updateTotal();
    }
    
    /**
     * Returns the order ID.
     *
     * @return orderId
     */

    public long getOrderId() {
        return orderId;
    }
    
    /**
     * Returns the Customer.
     *
     * @return customer
     */

    public Customer getCustomer() {
        return customer;
    }
    
    /**
     * Returns the order date.
     *
     * @return orderDate
     */

    public long getOrderDate() {
        return orderDate;
    }
    
    /**
     * Returns the order status.
     *
     * @return status
     */

    public String getStatus() {
        return status;
    }
    
    /**
     * Sets the order status.
     *
     * @param status the new status
     */

    public void setStatus(String status) {
        this.status = status;
    }
    
    /**
     * Returns the list of OrderItems.
     *
     * @return items
     */

    public ArrayList<OrderItem> getItems() {
        return items;
    }
    
    /**
     * Returns the order total.
     *
     * @return total
     */

    public double getTotal() {
        return total;
    }

    /*
     * Gets the current date in YYYYMMDD format.
     */
     
    private long getDate() {
        LocalDateTime now = LocalDateTime.now();

        String year = String.format("%d", now.getYear());
        String month = String.format("%02d", now.getMonthValue());
        String day = String.format("%02d", now.getDayOfMonth());

        return Long.valueOf(year + month + day);
    }
    
    /*
     * Updates the order total using the price of each item.
     */

    private void updateTotal() {
        if (items == null) {
            this.total = 0;
        } else {
            double total = 0;

            for (OrderItem item : items) {
                total = total + item.getPrice();
            }

            this.total = total;
        }
    }
}
```
{% endcode %}

As I said, there's a bit more going on here. We've got a few more methods in addition to the normal getters and setters. And don't worry if this isn't compiling for you. It won't until we write the `OrderItem` class.

* `addItem(OrderItem item)` - this is how we'll add an `OrderItem` to an `Order`. Later, we'll connect this to the UI where employees can create orders the way you might add something to your cart while shopping online.
* `removeItem(OrderItem item)` - if we can add an `OrderItem`, we'll want the ability to remove one, too. Employees make mistakes, customers change their minds. It happens.
* `updateTotal()` - every time we add or remove an `OrderItem`, we'll need to update the `total`. All this does is iterate through `items` and tally the sum. This is a private method, which means it can only be called from within the `Order` class.

Now, remember I mentioned some unfamiliarity with data types? Well, I was mostly talking about the date fields: `orderDate` and `dateShipped`. These are stored numerically, in the format `YYYYMMDD`. Every order has an `orderDate`, but some orders won't have a `dateShipped` value. This is either because they haven't been shipped yet \(in which case the value will be `0`\) or they're in-store orders and won't be shipped at all \(in which case the value will be `-1`\).

### OrderItem

Finally, the `OrderItem` class. Once we wrap this up, we'll be done with our model classes.

* Item ID
* Product
* Quantity

Last time through, I promise. This class is smaller than the others, too.

{% code title="OrderItem.java" %}
```java
package org.ucvts.comics.model;

public class OrderItem {

    private static long lastItemId = 1L; // initial item ID

    private long itemId;
    private Product product;
    private int quantity;

    /**
     * Creates a default instance of the OrderItem class.
     * 
     * @param product the product being purchased
     */

    public OrderItem(Product product) {
        this.itemId = OrderItem.lastItemId++; // auto-generate ID
        this.product = product;
        this.quantity = 1;
    }

    /**
     * Creates an instance of the OrderItem class.
     * 
     * @param itemId   the item ID
     * @param product  the product being purchased
     * @param quantity the quantity being purchased
     */

    public OrderItem(long itemId, Product product, int quantity) {
        this.itemId = itemId;
        this.product = product;
        this.quantity = quantity;
    }

    /**
     * Returns the price based on quantity and product unit price.
     * 
     * @return the price
     */

    public double getPrice() {
        return quantity * product.getUnitPrice();
    }
    
    /**
     * Returns the item ID.
     *
     * @return itemId
     */

    public long getItemId() {
        return itemId;
    }
    
    /**
     * Returns the Product.
     *
     * @return product
     */

    public Product getProduct() {
        return product;
    }
    
    /**
     * Returns the quantity.
     *
     * @return quantity
     */

    public int getQuantity() {
        return quantity;
    }
    
    /**
     * Sets the quantity.
     *
     * @param quantity the new quantity
     */

    public void setQuantity(int quantity) {
        this.quantity = quantity;
    }
}
```
{% endcode %}

Aside from the standard getters and setters, there's only one method of note here.

* `getPrice()` - this returns the price based on the quantity and the product's unit price.

Phew! Tired of writing code yet? I hope not, because we're just getting started!

## Controller

Users are going to be performing actions in the UI that will inevitably impact the underlying model objects. The controller is responsible for managing these interactions. It routes requests from the UI to the model, and pushes changes back to the view for rendering.

To write our controller, we need to stop and think about what interactions might take place. This includes requests from the UI and responses from the model.

* Switching between views
* Adding new products to inventory
* Updating existing products in inventory
* Removing products from inventory
* Adding items to an order
* Updating existing items in an order
* Removing items from an order

This is a critical step in our development process. As you'll see though, it's mostly routing calls to and from the UI. That being said, some of this won't make a whole lot of sense until our view components are implemented and put in place.

{% code title="ViewManager.java" %}
```java
package org.ucvts.comics.controller;

import java.awt.CardLayout;
import java.awt.Container;
import java.util.ArrayList;
import java.util.List;

import org.ucvts.comics.MidtownComics;
import org.ucvts.comics.model.Order;
import org.ucvts.comics.model.OrderItem;
import org.ucvts.comics.model.Product;
import org.ucvts.comics.view.CartView;
import org.ucvts.comics.view.InventoryView;
import org.ucvts.comics.view.OrderView;
import org.ucvts.comics.view.ProductView;

public class ViewManager {

    private static ViewManager manager;

    private Container views;
    private List<Product> inventory;
    private Order order;

    /*
     * A private constructor, implementing the singleton pattern.
     * 
     * @param views
     */

    private ViewManager(Container views) {
        this.views = views;

        this.populate();
    }
    
    /**
     * Returns an instance of the ViewManager class, creating one if necessary.
     * 
     * @param views the parent container
     * @return an instance of the ViewManager class
     */

    public static ViewManager getInstance(Container views) {
        if (manager == null) {
            manager = new ViewManager(views);
        }

        return manager;
    }

    /**
     * Switches to the specified named view.
     * 
     * @param view the view to switch to
     */

    public void switchTo(String view) {
        ((CardLayout) views.getLayout()).show(views, view);
    }
    
    /**
     * Attaches a product to the product view.
     * 
     * @param product the product to attach
     */
    
    public void attachProduct(Product product) {
        ((ProductView) views.getComponent(MidtownComics.ProductViewIndex)).setProduct(product);
    }
    
    /**
     * Detaches a product from the product view.
     */
    
    public void detachProduct() {
        ((ProductView) views.getComponent(MidtownComics.ProductViewIndex)).setProduct(null);
    }
    
    /**
     * Adds a new Product to inventory.
     * 
     * @param product the new product
     */

    public void addProductToInventory(Product product) {
        inventory.add(product);

        detachProduct();
        refreshInventoryList();
        switchTo(MidtownComics.InventoryView);
    }

    /**
     * Modifies an existing Product in inventory.
     * 
     * @param product the modified product
     */

    public void modifyProductInInventory(Product product) {
        inventory.set(findProductInInventory(product), product);
        
        detachProduct();
        refreshInventoryList();
        switchTo(MidtownComics.InventoryView);
    }

    /**
     * Removes an existing Product from inventory.
     * 
     * @param product the product to be removed
     */

    public void removeProductFromInventory(Product product) {
        inventory.remove(product);

        detachProduct();
        refreshInventoryList();
        switchTo(MidtownComics.InventoryView);
    }

    /**
     * Adds an OrderItem to an Order, creating the Order first if necessary.
     * 
     * @param item the item to be added
     */

    public void addItemToOrder(OrderItem item) {
        if (order == null) {
            order = new Order();
        }

        order.addItem(item);
        refreshCart();
        updateOrderTotal();
        manager.switchTo(MidtownComics.CartView);
    }

    /**
     * Modify the quantity of an OrderItem in an Order.
     * 
     * @param product  the product to be modified
     * @param quantity the new quantity
     */

    public void modifyItemQuantityInOrder(Product product, int quantity) {
        int index = findItemInOrder(product);
        order.getItems().get(index).setQuantity(quantity);        

        refreshCart();
        updateOrderTotal();
        switchTo(MidtownComics.InventoryView);
        switchTo(MidtownComics.CartView);
    }

    /**
     * Remove an OrderItem from an existing Order.
     * 
     * @param product the product to be removed
     */

    public void removeItemFromOrder(Product product) {
        int index = findItemInOrder(product);
        order.getItems().remove(index);

        refreshCart();
        updateOrderTotal();
        switchTo(MidtownComics.InventoryView);
        
        if (order.getItems().size() > 0) {
            switchTo(MidtownComics.CartView);
        }
    }
    
    /**
     * Returns the OrderItem quantity by Product.
     * 
     * @param product the product whose quantity we need
     * @return the order item quantity
     */
    
    public int getOrderItemQuantity(Product product) {
        int index = findItemInOrder(product);
        
        return index != -1 ? order.getItems().get(index).getQuantity() : 0;
    }
    
    /**
     * Returns the subtotal for this Product.
     * 
     * @param product the product whose subtotal we need
     * @return the subtotal
     */
    
    public double getSubtotal(Product product) {
        int index = findItemInOrder(product);
        
        return index != -1 ? order.getItems().get(index).getPrice() : 0;
    }
    
    /**
     * Returns whether or not the Product already exists in the Order.
     * 
     * @param product the product
     * @return whether or not it exists in the order
     */
    
    public boolean productExistsInOrder(Product product) {
        if (order == null) {
            return false;
        }
        
        return findItemInOrder(product) != -1;
    }
    
    /**
     * Submits an order, which will modify the product in inventory.
     */
    
    public void submitOrder() {
        for (int i = 0; i < order.getItems().size(); i++) {
            OrderItem item = order.getItems().get(i);
            Product product = item.getProduct();
            int quantity = item.getQuantity();
            
            int index = findProductInInventory(product);
            if (index != -1) {
                int copies = product.getCopies();
                product.setCopies(copies - quantity);
                
                modifyProductInInventory(product);
            }
        }
        
        clearOrder();
    }

    /**
     * Returns the list of products in inventory.
     *
     * @return inventory
     */

    public List<Product> getInventory() {
        return inventory;
    }
    
    /**
     * Returns the Order.
     *
     * @return order
     */
    
    public Order getOrder() {
        return order;
    }

    /*
     * Populates a set of products in inventory. Ordinarily, this sort of thing
     * would be handled by a database.
     */

    private void populate() {
        if (inventory == null) {
            inventory = new ArrayList<>();
        }

        inventory.add(new Product("The Amazing Spider-Man", "Stan Lee", 19630301L, 1, 19.99, 3));
        inventory.add(new Product("The Amazing Spider-Man", "Stan Lee", 19630510L, 2, 19.99, 7));
        inventory.add(new Product("The Amazing Spider-Man", "Stan Lee", 19630710L, 3, 19.99, 9));
        inventory.add(new Product("The Amazing Spider-Man", "Stan Lee", 19630910L, 4, 19.99, 12));
        inventory.add(new Product("The Amazing Spider-Man", "Stan Lee", 19631010L, 5, 19.99, 3));
        inventory.add(new Product("The Amazing Spider-Man", "Stan Lee", 19631110L, 6, 19.99, 7));
        inventory.add(new Product("The Amazing Spider-Man", "Stan Lee", 19631210L, 7, 19.99, 9));
        inventory.add(new Product("The Amazing Spider-Man", "Stan Lee", 19640110L, 8, 19.99, 12));
        inventory.add(new Product("The Amazing Spider-Man", "Stan Lee", 19640210L, 9, 19.99, 3));
        inventory.add(new Product("The Amazing Spider-Man", "Stan Lee", 19640310L, 10, 19.99, 7));
    }
    
    /*
     * Refreshes the inventory view when the list of products changes.
     */
    
    private void refreshInventoryList() {
        ((InventoryView) views.getComponent(MidtownComics.InventoryViewIndex)).refreshInventoryList();
    }
    
    /*
     * Refreshes the cart view when an item changes.
     */
    
    private void refreshCart() {
        ((CartView) views.getComponent(MidtownComics.CartViewIndex)).refreshCart();
    }
    
    /*
     * Refreshes the order view when the order total changes.
     */
    
    private void updateOrderTotal() {
        ((OrderView) views.getComponent(MidtownComics.OrderViewIndex)).updateOrderTotal(order.getTotal());
    }
    
    /*
     * Refreshes the cart view when an order is submitted.
     */
    
    private void clearOrder() {
        ((OrderView) views.getComponent(MidtownComics.OrderViewIndex)).clearOrder();
    }
    
    /*
     * Finds a product in inventory.
     *
     * @param product the product we're looking for
     * @return the index of the product if found
     */

    private int findProductInInventory(Product product) {
        for (int i = 0; i < inventory.size(); i++) {
            if (inventory.get(i).getProductId() == product.getProductId()) {
                return i;
            }
        }

        return -1;
    }
    
    /*
     * Finds an item in an order.
     *
     * @param product the item we're looking for
     * @return the index of the item if found
     */

    private int findItemInOrder(Product p) {
        for (int i = 0; i < order.getItems().size(); i++) {
            if (order.getItems().get(i).getProduct().getProductId() == p.getProductId()) {
                return i;
            }
        }

        return -1;
    }
}
```
{% endcode %}

A lot of this, understandably, doesn't make sense yet. And that's because we haven't implemented the views that our controller will be managing. We're going to do that next, and touch on each section of the controller as we do.

## View

Moving on the presentational layer of our application. The view represents the interactive pieces of our program. It's the interface where Midtown Comics employees will point, click, and type in order to serve their loyal customers.

* `InventoryView`
* `ProductView`
* `CartView`
* `OrderView`

To simplify things, some of our views use classes that represent components within that view.

* `InventoryItemPanel`
* `ProductForm`
* `CartItemPanel`
* `PaymentForm`

### InventoryView

The `InventoryView` is where employees will get the full picture of every comic book carried by Midtown Comics \(even if it's currently out-of-stock\). It'll provide all of the product details we are already tracking in our `Product` class, as well as the ability to add an item to a pending order.

{% code title="InventoryView.java" %}
```java
package org.ucvts.comics.view;

import java.awt.BorderLayout;
import java.awt.Font;
import java.awt.event.ActionEvent;
import java.awt.event.ActionListener;

import javax.swing.BoxLayout;
import javax.swing.JButton;
import javax.swing.JLabel;
import javax.swing.JPanel;
import javax.swing.JScrollPane;
import javax.swing.border.EmptyBorder;

import org.ucvts.comics.MidtownComics;
import org.ucvts.comics.controller.ViewManager;

@SuppressWarnings("serial")
public class InventoryView extends JPanel implements ActionListener {
    
    private ViewManager manager;
    private JScrollPane scroll;
    private JButton addProduct;
    private JButton viewCart;
    
    /**
     * Creates an instance of the InventoryView class.
     * 
     * @param manager the controller
     */
    
    public InventoryView(ViewManager manager) {
        super(new BorderLayout());
        
        this.manager = manager;
        this.init();
    }
    
    /**
     * Refreshes the inventory list.
     */
    
    public void refreshInventoryList() {
        this.remove(scroll);
        
        initInventoryList();
    }
    
    /*
     * Initializes all UI components.
     */
    
    private void init() {        
        initHeader();
        initInventoryList();
        initFooter();
    }
    
    /*
     * Initializes the header UI components.
     */
    
    private void initHeader() {
        JPanel panel = new JPanel(new BorderLayout());
        
        JLabel label = new JLabel("Midtown Comics");
        label.setFont(new Font("DialogInput", Font.BOLD, 21));
        label.setBorder(new EmptyBorder(15, 15, 10, 0));
                
        panel.add(label, BorderLayout.WEST);
        this.add(panel, BorderLayout.NORTH);
    }
    
    /*
     * Initializes the inventory list UI components.
     */
    
    private void initInventoryList() {
        JPanel body = new JPanel();
        body.setLayout(new BoxLayout(body, BoxLayout.Y_AXIS));
        body.setBorder(new EmptyBorder(15, 15, 15, 15));
        
        for (int i = 0; i < manager.getInventory().size(); i++) {            
            body.add(new InventoryItemPanel(manager, manager.getInventory().get(i)));
        }
        
        scroll = new JScrollPane(body);
        this.add(scroll, BorderLayout.CENTER);
    }
    
    /*
     * Initializes the footer UI components.
     */
    
    private void initFooter() {
        JPanel panel = new JPanel(new BorderLayout());
        panel.setBorder(new EmptyBorder(10, 15, 15, 15));
        
        addProduct = new JButton("Add Product");
        addProduct.putClientProperty("id", -1L);
        addProduct.addActionListener(this);
        
        viewCart = new JButton("Proceed to Cart");
        viewCart.putClientProperty("id", -1L);
        viewCart.addActionListener(this);
        
        panel.add(addProduct, BorderLayout.WEST);
        panel.add(viewCart, BorderLayout.EAST);
        this.add(panel, BorderLayout.SOUTH);
    }
    
    /*
     * Handles button clicks in this view.
     *
     * @param e the event that triggered this action
     */
    
    @Override
    public void actionPerformed(ActionEvent e) {
        JButton source = (JButton) e.getSource();
        
        if (source.equals(addProduct)) {
            manager.switchTo(MidtownComics.ProductView);
        } else if (source.equals(viewCart)) {
            manager.switchTo(MidtownComics.CartView);
        }
    }
}
```
{% endcode %}

The InventoryView class uses a subcomponent class called `InventoryItemPanel`, whose job is to render individual items in inventory.

{% code title="InventoryItemPanel.java" %}
```java
package org.ucvts.comics.view;

import java.awt.BorderLayout;
import java.awt.Dimension;
import java.awt.Font;
import java.awt.GridLayout;
import java.awt.event.ActionEvent;
import java.awt.event.ActionListener;

import javax.swing.JButton;
import javax.swing.JLabel;
import javax.swing.JPanel;
import javax.swing.JSeparator;
import javax.swing.SwingConstants;

import org.ucvts.comics.MidtownComics;
import org.ucvts.comics.controller.ViewManager;
import org.ucvts.comics.model.OrderItem;
import org.ucvts.comics.model.Product;

@SuppressWarnings("serial")
public class InventoryItemPanel extends JPanel implements ActionListener {
    
    private ViewManager manager;

    /**
     * Creates an instance of the InventoryItemPanel class.
     * 
     * @param manager the controller
     * @param product the product represented by this panel
     */
    
    public InventoryItemPanel(ViewManager manager, Product product) {
        super(new BorderLayout());
        
        this.manager = manager;
        this.init(product);
        this.setMaximumSize(new Dimension(Integer.MAX_VALUE, 100));
    }
    
    /*
     * Initializes all UI components.
     */

    private void init(Product p) {
        JPanel content = getContentPanel(p);
        JPanel actions = getActionPanel(p);
        
        this.add(content, BorderLayout.CENTER);
        this.add(actions, BorderLayout.EAST);
        this.add(new JSeparator(), BorderLayout.SOUTH);
    }
    
    /*
     * Initializes the content panel UI components.
     */
    
    private JPanel getContentPanel(Product p) {
        JPanel panel = new JPanel(new GridLayout(0, 1));
        
        JLabel title = new JLabel(p.getTitle() + " #" + p.getIssue());
        JLabel author = new JLabel("Written by " + p.getAuthor() + " • Released on " + convertDate(p.getReleaseDate()));
        JLabel copies = new JLabel("Available: " + (p.getCopies() == 1 ? p.getCopies() + " copy" : p.getCopies() + " copies"));
        
        title.setFont(new Font("DialogInput", Font.BOLD, 18));
        author.setFont(new Font("DialogInput", Font.ITALIC, 12));
        copies.setFont(new Font("DialogInput", Font.ITALIC, 12));
        
        panel.add(title);
        panel.add(author);
        panel.add(copies);
        
        return panel;
    }
    
    /*
     * Initializes the action panel UI components.
     */
    
    private JPanel getActionPanel(Product p) {
        JPanel panel = new JPanel(new GridLayout(0, 1));
        
        JLabel price = new JLabel("$" + p.getUnitPrice(), SwingConstants.CENTER);
        price.setFont(new Font("DialogInput", Font.BOLD, 15));
        
        JButton buy = new JButton("Buy");
        buy.putClientProperty("id", p.getProductId());
        buy.putClientProperty("type", "BUY");
        buy.addActionListener(this);
        
        JButton edit = new JButton("Edit");
        edit.putClientProperty("id", p.getProductId());
        edit.putClientProperty("type", "EDIT");
        edit.addActionListener(this);

        panel.add(price);
        panel.add(buy);
        panel.add(edit);
        
        return panel;
    }
    
    /*
     * Converts a date in YYYYMMDD format to Month DD, YYYY format.
     */
    
    private String convertDate(long date) {
        String dateStr = String.valueOf(date);
        
        int year = Integer.valueOf(dateStr.substring(0, 4));
        int month = Integer.valueOf(dateStr.substring(4, 6));
        int day = Integer.valueOf(dateStr.substring(6));
        
        return getMonth(month) + " " + day + ", " + year;
    }
    
    /*
     * Converts a numeric month to its string equivalent.
     */
    
    private String getMonth(int month) {
        switch (month) {
            case 1: return "January";
            case 2: return "February";
            case 3: return "March";
            case 4: return "April";
            case 5: return "May";
            case 6: return "June";
            case 7: return "July";
            case 8: return "August";
            case 9: return "September";
            case 10: return "October";
            case 11: return "November";
            case 12: return "December";
            default: return "";
        }
    }
    
    /*
     * Handles button clicks in this view.
     *
     * @param e the event that triggered this action
     */

    @Override
    public void actionPerformed(ActionEvent e) {
        JButton source = (JButton) e.getSource();
        Long id = (Long) source.getClientProperty("id");
        String type = (String) source.getClientProperty("type");
        
        for (Product p : manager.getInventory()) {
            if (id.longValue() == p.getProductId() && type.equals("BUY")) {
                if (!manager.productExistsInOrder(p)) {
                    manager.addItemToOrder(new OrderItem(p));
                }
            } else if (id.longValue() == p.getProductId() && type.equals("EDIT")) {
                manager.attachProduct(p);
                manager.switchTo(MidtownComics.ProductView);
            }
        }
        
    }
}
```
{% endcode %}

Both of these classes interact with the `ViewManager`. From the `InventoryView` class, we can switch the `ProductView` or `CartView` by clicking the buttons. These actions are picked up by our `ActionListener` and routed to the controller.

The `Buy` and `Edit` buttons are handled in the `InventoryItemPanel` class. Clicking `Buy` adds the item to our cart, while clicking `Edit` allows us to change product details.

Make sure you understand what's happening here, especially the calls to the `ViewManager`. Switching views should be pretty easy to wrap your head around, but some of the other controller interactions are a bit more complex.

### ProductView

The ProductView is where employees can change the details of a product, such as its price or the number of available copies.

{% code title="ProductView.java" %}
```java
package org.ucvts.comics.view;

import java.awt.BorderLayout;
import java.awt.Font;
import java.awt.GridLayout;
import java.awt.event.ActionEvent;
import java.awt.event.ActionListener;

import javax.swing.JButton;
import javax.swing.JLabel;
import javax.swing.JPanel;
import javax.swing.JScrollPane;
import javax.swing.border.EmptyBorder;

import org.ucvts.comics.MidtownComics;
import org.ucvts.comics.controller.ViewManager;
import org.ucvts.comics.model.Product;

@SuppressWarnings("serial")
public class ProductView extends JPanel implements ActionListener {

    private ViewManager manager;
    private Product product;
    private ProductForm productForm;
    private JButton save;
    private JButton remove;
    private JButton cancel;

    /**
     * Creates an instance of the ProductView class.
     * 
     * @param manager the controller
     */
    
    public ProductView(ViewManager manager) {
        super(new BorderLayout());

        this.manager = manager;
        this.productForm = new ProductForm();
        this.init();
    }
    
    /**
     * Sets the Product associated with this view.
     *
     * @param product the new product
     */

    public void setProduct(Product product) {
        this.product = product;
        
        remove.setEnabled(true);
        productForm.updateFields(product);
    }

    /*
     * Initializes all UI components.
     */

    private void init() {        
        initHeader();
        initProductForm();
        initFooter();
    }
    
    /*
     * Initializes header UI components.
     */

    private void initHeader() {
        JPanel panel = new JPanel(new BorderLayout());

        JLabel label = new JLabel("Midtown Comics");
        label.setFont(new Font("DialogInput", Font.BOLD, 21));
        label.setBorder(new EmptyBorder(15, 15, 10, 0));

        panel.add(label, BorderLayout.WEST);
        this.add(panel, BorderLayout.NORTH);
    }
    
    /*
     * Initializes the productform.
     */

    private void initProductForm() {
        this.add(new JScrollPane(productForm), BorderLayout.CENTER);
    }
    
    /*
     * Initializes footer UI components.
     */

    private void initFooter() {
        JPanel panel = new JPanel(new GridLayout(1, 0));
        panel.setBorder(new EmptyBorder(10, 15, 15, 15));

        cancel = new JButton("Cancel");
        cancel.addActionListener(this);
        
        remove = new JButton("Remove");
        remove.setEnabled(false);
        remove.addActionListener(this);

        save = new JButton("Save");
        save.addActionListener(this);

        panel.add(cancel);
        panel.add(remove);
        panel.add(save);
        this.add(panel, BorderLayout.SOUTH);
    }

    /*
     * Handles button clicks in this view.
     *
     * @param e the event that triggered this action
     */

    @Override
    public void actionPerformed(ActionEvent e) {
        JButton source = (JButton) e.getSource();
        
        if (source.equals(save)) {
            if (product == null) {
                manager.addProductToInventory(productForm.getProductFromFields());
            } else {
                manager.modifyProductInInventory(productForm.getProductFromFields());
            }
        } else if (source.equals(remove)) {
            manager.removeProductFromInventory(product);
        } else if (source.equals(cancel)) {
            manager.detachProduct();
            manager.switchTo(MidtownComics.InventoryView);
        }
    }
}
```
{% endcode %}

It uses another class, `ProductForm`, to simplify the UI layout.

{% code title="ProductForm.java" %}
```java
package org.ucvts.comics.view;

import java.awt.Color;
import java.awt.Font;
import java.awt.event.KeyAdapter;
import java.awt.event.KeyEvent;

import javax.swing.JComboBox;
import javax.swing.JLabel;
import javax.swing.JPanel;
import javax.swing.JTextField;
import javax.swing.SwingConstants;

import org.ucvts.comics.model.Product;

@SuppressWarnings("serial")
public class ProductForm extends JPanel {

    private JTextField productIdField;
    private JTextField titleField;
    private JTextField authorField;
    private JComboBox<String> monthDropdown;
    private JComboBox<String> dayDropdown;
    private JComboBox<String> yearDropdown;
    private JTextField issueField;
    private JTextField priceField;
    private JTextField copiesField;
    private JLabel errorLabel;
    
    /**
     * Creates a default instance of the ProductForm class.
     */
    
    public ProductForm() {
        this(null);
    }
    
    /**
     * Creates an instance of the ProductForm class.
     * 
     * @param product the source product
     */
    
    public ProductForm(Product product) {
        this.init(product);
    }
    
    /**
     * Updates fields with actual product data.
     * 
     * @param product the product with which to update the fields
     */
    
    public void updateFields(Product product) {
        if (product == null) {
            clearFields();
            
            return;
        }
        
        productIdField.setText(String.valueOf(product.getProductId()));
        titleField.setText(product.getTitle());
        authorField.setText(product.getAuthor());
        
        String date = String.valueOf(product.getReleaseDate());
        String year = date.substring(0, 4);
        int month = Integer.parseInt(date.substring(4, 6));
        int day = Integer.parseInt(date.substring(6));
        
        monthDropdown.setSelectedIndex(month);
        dayDropdown.setSelectedIndex(day);
        yearDropdown.setSelectedItem(year);
        issueField.setText(String.valueOf(product.getIssue()));
        priceField.setText(String.format("%.2f", product.getUnitPrice()));
        copiesField.setText(String.valueOf(product.getCopies()));
    }
    
    /**
     * Returns a product built from the form fields.
     * 
     * @return a product
     */
    
    public Product getProductFromFields() {
        if (productIdField.getText().trim().isEmpty()) {
            return new Product(
                titleField.getText(),
                authorField.getText(),
                parseReleaseDate(),
                Integer.parseInt(issueField.getText()),
                parseUnitPrice(),
                Integer.parseInt(copiesField.getText())
            );
        } else {
            return new Product(
                Long.parseLong(productIdField.getText()),
                titleField.getText(),
                authorField.getText(),
                parseReleaseDate(),
                Integer.parseInt(issueField.getText()),
                parseUnitPrice(),
                Integer.parseInt(copiesField.getText())
            );
        }
    }
    
    /**
     * Updates the form error message.
     * 
     * @param message the new message
     */
    
    public void updateErrorMessage(String message) {
        errorLabel.setText(message);
    }
    
    /*
     * Initializes all UI components.
     */
    
    private void init(Product product) {
        this.setLayout(null);
        
        initProductId(product);
        initTitle(product);
        initAuthor(product);
        initReleaseDate(product);
        initIssue(product);
        initPrice(product);
        initCopies(product);
        initErrorMessage();
    }
    
    /*
     * Initializes the product ID UI field.
     */
    
    private void initProductId(Product product) {
        JLabel label = new JLabel("Product ID");
        label.setFont(new Font("DialogInput", Font.BOLD, 14));
        label.setBounds(25, 15, 100, 40);
        label.setLabelFor(productIdField);
        
        productIdField = new JTextField(10);
        productIdField.setBounds(20, 45, 710, 40);;
        productIdField.setEditable(false);
        
        this.add(label);
        this.add(productIdField);
    }
    
    /*
     * Initializes the title UI field.
     */
    
    private void initTitle(Product product) {
        JLabel label = new JLabel("Title");
        label.setFont(new Font("DialogInput", Font.BOLD, 14));
        label.setBounds(25, 85, 100, 40);
        label.setLabelFor(titleField);
        
        titleField = new JTextField(10);
        titleField.setBounds(20, 115, 710, 40);
        
        this.add(label);
        this.add(titleField);
    }
    
    /*
     * Initializes the author UI field.
     */
    
    private void initAuthor(Product product) {
        JLabel label = new JLabel("Author");
        label.setFont(new Font("DialogInput", Font.BOLD, 14));
        label.setBounds(25, 155, 100, 40);
        label.setLabelFor(titleField);
        
        authorField = new JTextField(10);
        authorField.setBounds(20, 185, 710, 40);
        
        this.add(label);
        this.add(authorField);
    }
    
    /*
     * Initializes the release date UI dropdown menus.
     */
    
    private void initReleaseDate(Product product) {
        JLabel label = new JLabel("Release Date");
        label.setFont(new Font("DialogInput", Font.BOLD, 14));
        label.setBounds(25, 225, 100, 40);
        label.setLabelFor(monthDropdown);
        
        monthDropdown = new JComboBox<>(getMonths());
        monthDropdown.setBounds(20, 255, 320, 40);
        
        dayDropdown = new JComboBox<>(getDays());
        dayDropdown.setBounds(350, 255, 160, 40);
        
        yearDropdown = new JComboBox<>(getYears());
        yearDropdown.setBounds(520, 255, 210, 40);
        
        this.add(label);
        this.add(monthDropdown);
        this.add(dayDropdown);
        this.add(yearDropdown);
    }
    
    /*
     * Initializes the issue UI field.
     */
    
    private void initIssue(Product product) {
        JLabel label = new JLabel("Issue No.");
        label.setFont(new Font("DialogInput", Font.BOLD, 14));
        label.setBounds(25, 295, 100, 40);
        label.setLabelFor(issueField);
        
        issueField = new JTextField(10);
        issueField.setBounds(20, 325, 710, 40);
        issueField.addKeyListener(new KeyAdapter() {
            @Override
            public void keyTyped(KeyEvent e) {
                if (e.getKeyChar() < 48 || e.getKeyChar() > 57) {
                    e.consume();
                }
            }
        });
        
        this.add(label);
        this.add(issueField);
    }
    
    /*
     * Initializes the price UI field.
     */
    
    private void initPrice(Product product) {
        JLabel label = new JLabel("Unit Price");
        label.setFont(new Font("DialogInput", Font.BOLD, 14));
        label.setBounds(25, 365, 100, 40);
        label.setLabelFor(priceField);
        
        priceField = new JTextField(10);
        priceField.setBounds(20, 395, 710, 40);
        priceField.addKeyListener(new KeyAdapter() {
            @Override
            public void keyTyped(KeyEvent e) {
                if (e.getKeyChar() == 46) {
                    if (priceField.getText().contains(".")) {
                        e.consume();
                    }
                } else if (e.getKeyChar() < 48 || e.getKeyChar() > 57) {
                    e.consume();
                } else {
                    int index = priceField.getText().indexOf(".");
                    
                    if (index != -1) {
                        if (priceField.getText().substring(index).length() > 2) {
                            e.consume();
                        }
                    }
                }
            }
        });
        
        this.add(label);
        this.add(priceField);
    }
    
    /*
     * Initializes the available copies UI field.
     */
    
    private void initCopies(Product product) {
        JLabel label = new JLabel("Copies");
        label.setFont(new Font("DialogInput", Font.BOLD, 14));
        label.setBounds(25, 435, 100, 40);
        label.setLabelFor(copiesField);
        
        copiesField = new JTextField(10);
        copiesField.setBounds(20, 465, 710, 40);
        copiesField.addKeyListener(new KeyAdapter() {
            @Override
            public void keyTyped(KeyEvent e) {
                if (e.getKeyChar() < 48 || e.getKeyChar() > 57) {
                    e.consume();
                }
            }
        });
        
        this.add(label);
        this.add(copiesField);
    }
    
    /*
     * Initializes the error message UI field.
     */
    
    private void initErrorMessage() {
        errorLabel = new JLabel("", SwingConstants.CENTER);
        errorLabel.setFont(new Font("DialogInput", Font.ITALIC, 14));
        errorLabel.setForeground(Color.RED);
        errorLabel.setBounds(20, 540, 710, 40);
        
        this.add(errorLabel);
    }
    
    /*
     * Resets all UI fields to their default values.
     */
    
    private void clearFields() {
        productIdField.setText("");
        titleField.setText("");
        authorField.setText("");
        monthDropdown.setSelectedIndex(0);
        dayDropdown.setSelectedIndex(0);
        yearDropdown.setSelectedIndex(0);
        issueField.setText("");
        priceField.setText("");
        copiesField.setText("");
    }
    
    /*
     * Gets the list of values for the months dropdown menu.
     */
    
    private String[] getMonths() {
        return new String[]{
            "--Month--",
            "January",
            "February",
            "March",
            "April",
            "May",
            "June",
            "July",
            "August",
            "September",
            "October",
            "November",
            "December"
        };
    }
    
    /*
     * Gets the list of values for the days dropdown menu.
     */

    private String[] getDays() {
        String[] days = new String[32];
        
        for (int i = 0; i <= 31; i++) {
            if (i == 0) {
                days[i] = "--Day--";
            } else {
                days[i] = String.valueOf(i);
            }
        }
        
        return days;
    }
    
    /*
     * Gets the list of values for the years dropdown menu.
     */
    
    private String[] getYears() {
        String[] years = new String[73];
        
        for (int i = 1949; i <= 2021; i++) {
            if (i == 1949) {
                years[0] = "--Year--";
            } else {
                years[i - 1949] = String.valueOf(i);
            }
        }
        
        return years;
    }
    
    /*
     * Parses a date value into YYYYMMDD format.
     */
    
    private long parseReleaseDate() {
        int year = Integer.parseInt(yearDropdown.getSelectedItem().toString());
        int month = monthDropdown.getSelectedIndex();
        int day = dayDropdown.getSelectedIndex();
        
        return Long.parseLong(String.format("%d%02d%02d", year, month, day));
    }
    
    /*
     * Parses a numeric price from the textfield.
     */
    
    private double parseUnitPrice() {
        return Double.parseDouble(priceField.getText());
    }
}
```
{% endcode %}

All of the controller interactions originate from the `ProductView`. We can add new products to inventory, update existing ones, or remove products entirely.

### CartView

The `CartView` is our shopping cart, which lists all products we've added to the current order.

{% code title="CartView.java" %}
```java
package org.ucvts.comics.view;

import java.awt.BorderLayout;
import java.awt.Font;
import java.awt.event.ActionEvent;
import java.awt.event.ActionListener;

import javax.swing.BoxLayout;
import javax.swing.JButton;
import javax.swing.JLabel;
import javax.swing.JPanel;
import javax.swing.JScrollPane;
import javax.swing.border.EmptyBorder;

import org.ucvts.comics.MidtownComics;
import org.ucvts.comics.controller.ViewManager;

@SuppressWarnings("serial")
public class CartView extends JPanel implements ActionListener {
    
    private ViewManager manager;
    private JScrollPane scroll;
    private JButton back;
    private JButton checkout;
    
    /**
     * Creates an instance of the CartView class.
     * 
     * @param manager the controller
     */
    
    public CartView(ViewManager manager) {
        super(new BorderLayout());
        
        this.manager = manager;
        this.init();
    }
    
    /**
     * Refreshes the contents of the cart.
     */
    
    public void refreshCart() {
        this.remove(scroll);
        
        initCart();
    }
    
    /*
     * Initializes all UI components.
     */
    
    private void init() {        
        initHeader();
        initCart();
        initFooter();
    }
    
    /*
     * Initializes header UI components.
     */
    
    private void initHeader() {
        JPanel panel = new JPanel(new BorderLayout());
        
        JLabel label = new JLabel("Midtown Comics");
        label.setFont(new Font("DialogInput", Font.BOLD, 21));
        label.setBorder(new EmptyBorder(15, 15, 10, 0));
                
        panel.add(label, BorderLayout.WEST);
        this.add(panel, BorderLayout.NORTH);
    }
    
    /*
     * Initializes cart UI components.
     */
    
    private void initCart() {
        JPanel body = new JPanel();
        body.setLayout(new BoxLayout(body, BoxLayout.Y_AXIS));
        body.setBorder(new EmptyBorder(15, 15, 15, 15));
        
        if (manager.getOrder() != null) {
            for (int i = 0; i < manager.getOrder().getItems().size(); i++) {            
                body.add(new CartItemPanel(manager, manager.getOrder().getItems().get(i).getProduct()));
            }
        }
        
        scroll = new JScrollPane(body);
        this.add(scroll, BorderLayout.CENTER);
    }
    
    /*
     * Initializes footer UI components.
     */
    
    private void initFooter() {
        JPanel panel = new JPanel(new BorderLayout());
        panel.setBorder(new EmptyBorder(10, 15, 15, 15));
        
        back = new JButton("Continue Shopping");
        back.addActionListener(this);
        
        checkout = new JButton("Checkout");
        checkout.addActionListener(this);
        
        panel.add(back, BorderLayout.WEST);
        panel.add(checkout, BorderLayout.EAST);
        this.add(panel, BorderLayout.SOUTH);
    }
    
    /*
     * Handles button clicks in this view.
     *
     * @param e the event that triggered this action
     */
    
    @Override
    public void actionPerformed(ActionEvent e) {
        JButton source = (JButton) e.getSource();
        
        if (source.equals(back)) {
            manager.switchTo(MidtownComics.InventoryView);
        } else if (source.equals(checkout)) {
            if (manager.getOrder() != null && manager.getOrder().getItems().size() > 0) {
                manager.switchTo(MidtownComics.OrderView);
            }
        }
    }
}
```
{% endcode %}

As is the trend, it uses a companion class to simplify things: `CartItemPanel`.

{% code title="CartItemPanel.java" %}
```java
package org.ucvts.comics.view;

import java.awt.BorderLayout;
import java.awt.Dimension;
import java.awt.Font;
import java.awt.GridLayout;
import java.awt.event.ActionEvent;
import java.awt.event.ActionListener;

import javax.swing.JComboBox;
import javax.swing.JLabel;
import javax.swing.JPanel;
import javax.swing.JSeparator;
import javax.swing.SwingConstants;

import org.ucvts.comics.controller.ViewManager;
import org.ucvts.comics.model.Product;

@SuppressWarnings("serial")
public class CartItemPanel extends JPanel implements ActionListener {
    
    private ViewManager manager;
    private Product product;
    private JComboBox<Integer> combo;

    /**
     * Creates an instance of the CartItemPanel class.
     * 
     * @param product  the item represented by this panel
     * @param quantity the quantity of this product
     */
    
    public CartItemPanel(ViewManager manager, Product product) {
        super(new BorderLayout());
        
        this.manager = manager;
        this.product = product;
        this.init();
        this.setMaximumSize(new Dimension(Integer.MAX_VALUE, 100));
    }
    
    /*
     * Initializes all UI components.
     */

    private void init() {
        JPanel content = getContentPanel();
        JPanel actions = getActionPanel();
                
        this.add(content, BorderLayout.WEST);
        this.add(actions, BorderLayout.EAST);
        this.add(new JSeparator(), BorderLayout.SOUTH);
    }
    
    /*
     * Initializes content panel UI components.
     */
    
    private JPanel getContentPanel() {
        JPanel panel = new JPanel(new GridLayout(0, 1));
        
        Integer[] available = new Integer[product.getCopies() + 1];
        for (int i = 0; i < available.length; i++) {
            available[i] = i;
        }
        
        JLabel title = new JLabel(product.getTitle() + " #" + product.getIssue());
        combo = new JComboBox<>(available);
        combo.setSelectedIndex(manager.getOrderItemQuantity(product));
        
        title.setFont(new Font("DialogInput", Font.BOLD, 18));
        combo.addActionListener(this);
        
        panel.add(title);
        panel.add(combo);
        
        return panel;
    }
    
    /*
     * Initializes action panel UI components.
     */
    
    private JPanel getActionPanel() {
        JPanel panel = new JPanel(new GridLayout(0, 1));
        
        JLabel price = new JLabel("$" + manager.getSubtotal(product), SwingConstants.CENTER);        
        price.setFont(new Font("DialogInput", Font.BOLD, 15));

        panel.add(price);
        
        return panel;
    }
    
    /*
     * Handles combobox selections in this view.
     *
     * @param e the event that triggered this action
     */

    @Override
    public void actionPerformed(ActionEvent e) {
        Object source = e.getSource();
        
        if (source.equals(combo)) {
            int quantity = (int) combo.getSelectedItem();
            
            if (quantity == 0) {
                manager.removeItemFromOrder(product);
            } else {
                manager.modifyItemQuantityInOrder(product, quantity);
            }
        }
    }
}
```
{% endcode %}

We can do a few things from this view. Updating quantities \(or removing items by setting the quantity to zero\) is handled in the `CartItemPanel`, while returning to the inventory or moving on to checkout is done in the `CartView` class.

### OrderView

Last, but not least, the `OrderView`. This is where we'll enter our payment and shipping information, and submit the order.

{% code title="OrderView.java" %}
```java
package org.ucvts.comics.view;

import java.awt.BorderLayout;
import java.awt.Font;
import java.awt.event.ActionEvent;
import java.awt.event.ActionListener;

import javax.swing.JButton;
import javax.swing.JLabel;
import javax.swing.JPanel;
import javax.swing.SwingConstants;
import javax.swing.border.EmptyBorder;

import org.ucvts.comics.controller.ViewManager;

@SuppressWarnings("serial")
public class OrderView extends JPanel implements ActionListener {
    
    private ViewManager manager;
    private PaymentForm form;
    private JLabel total;
    private JButton submit;
    
    public OrderView(ViewManager manager) {
        super(new BorderLayout());
        
        this.manager = manager;
        this.init();
    }

    /**
     * Updates the order total label.
     * 
     * @param total the new total
     */
    
    public void updateOrderTotal(double total) {
        this.total.setText("Order Total: $" + total);
    }
    
    /**
     * Clears all fields.
     */
    
    public void clearOrder() {
        total.setText("");
        form.clearFields();
    }
    
    /*
     * Initializes all UI components.
     */

    private void init() {
        submit = new JButton("Submit");
        submit.addActionListener(this);
        
        total = new JLabel("Order Total: $", SwingConstants.RIGHT);
        total.setFont(new Font("DialogInput", Font.BOLD, 16));
        total.setBorder(new EmptyBorder(10, 10, 10, 10));
        
        form = new PaymentForm();
        
        this.add(total, BorderLayout.NORTH);
        this.add(form, BorderLayout.CENTER);
        this.add(submit, BorderLayout.SOUTH);
    }
    
    /*
     * Handles button clicks in this view.
     *
     * @param e the event that triggered this action
     */

    @Override
    public void actionPerformed(ActionEvent e) {
        JButton source = (JButton) e.getSource();
        
        if (source.equals(submit)) {
            manager.submitOrder();
        }
    }
}
```
{% endcode %}

And, of course, the companion class: `PaymentForm`.

{% code title="PaymentForm.java" %}
```java
package org.ucvts.comics.view;

import java.awt.Font;
import java.awt.event.KeyAdapter;
import java.awt.event.KeyEvent;

import javax.swing.JComboBox;
import javax.swing.JLabel;
import javax.swing.JPanel;
import javax.swing.JTextField;

@SuppressWarnings("serial")
public class PaymentForm extends JPanel {

    private JTextField nameField;
    private JTextField creditCardField;
    private JComboBox<String> monthDropdown;
    private JComboBox<String> dayDropdown;
    private JComboBox<String> yearDropdown;
    private JTextField securityCodeField;
    private JTextField streetAddressField;
    private JTextField cityField;
    private JComboBox<String> stateDropdown;
    private JTextField postalCodeField;
    
    /**
     * Creates a default instance of the PaymentForm class.
     */
    
    public PaymentForm() {
        super();
        
        this.init();
    }
    
    /**
     * Clears all fields.
     */
    
    public void clearFields() {
        nameField.setText("");
        creditCardField.setText("");
        monthDropdown.setSelectedIndex(0);
        dayDropdown.setSelectedIndex(0);
        yearDropdown.setSelectedIndex(0);
        securityCodeField.setText("");
        streetAddressField.setText("");
        cityField.setText("");
        stateDropdown.setSelectedIndex(0);
        postalCodeField.setText("");
    }
    
    /*
     * Initializes all UI components.
     */
    
    private void init() {
        this.setLayout(null);
        
        initNameField();
        initCreditCardField();
        initExpirationDate();
        initSecurityCode();
        initStreetAddress();
        initCityStatePostalCode();
    }
    
    /*
     * Initializes name UI field.
     */
    
    private void initNameField() {
        JLabel label = new JLabel("Name on Credit Card");
        label.setFont(new Font("DialogInput", Font.BOLD, 14));
        label.setBounds(25, 15, 200, 40);
        label.setLabelFor(nameField);
        
        nameField = new JTextField(10);
        nameField.setBounds(20, 45, 710, 40);;
        
        this.add(label);
        this.add(nameField);
    }
    
    /*
     * Initializes credit card UI field.
     */
    
    private void initCreditCardField() {
        JLabel label = new JLabel("Credit Card No.");
        label.setFont(new Font("DialogInput", Font.BOLD, 14));
        label.setBounds(25, 85, 200, 40);
        label.setLabelFor(creditCardField);
        
        creditCardField = new JTextField(10);
        creditCardField.setBounds(20, 115, 710, 40);
        creditCardField.addKeyListener(new KeyAdapter() {
            @Override
            public void keyTyped(KeyEvent e) {
                if (e.getKeyChar() < 48 || e.getKeyChar() > 57) {
                    e.consume();
                }
            }
        });
        
        this.add(label);
        this.add(creditCardField);
    }
    
    /*
     * Initializes expiration date UI dropdown menus.
     */
    
    private void initExpirationDate() {
        JLabel label = new JLabel("Expiration Date");
        label.setFont(new Font("DialogInput", Font.BOLD, 14));
        label.setBounds(25, 155, 200, 40);
        label.setLabelFor(monthDropdown);
        
        monthDropdown = new JComboBox<>(getMonths());
        monthDropdown.setBounds(20, 185, 320, 40);
        
        dayDropdown = new JComboBox<>(getDays());
        dayDropdown.setBounds(350, 185, 160, 40);
        
        yearDropdown = new JComboBox<>(getYears());
        yearDropdown.setBounds(520, 185, 210, 40);
        
        this.add(label);
        this.add(monthDropdown);
        this.add(dayDropdown);
        this.add(yearDropdown);
    }
    
    /*
     * Initializes security code UI field.
     */
    
    private void initSecurityCode() {
        JLabel label = new JLabel("CVV Code");
        label.setFont(new Font("DialogInput", Font.BOLD, 14));
        label.setBounds(25, 225, 100, 40);
        label.setLabelFor(securityCodeField);
        
        securityCodeField = new JTextField(10);
        securityCodeField.setBounds(20, 255, 710, 40);
        securityCodeField.addKeyListener(new KeyAdapter() {
            @Override
            public void keyTyped(KeyEvent e) {
                if (e.getKeyChar() < 48 || e.getKeyChar() > 57) {
                    e.consume();
                }
            }
        });
        
        this.add(label);
        this.add(securityCodeField);
    }
    
    /*
     * Initializes street address UI field.
     */
    
    private void initStreetAddress() {
        JLabel label = new JLabel("Street Address");
        label.setFont(new Font("DialogInput", Font.BOLD, 14));
        label.setBounds(25, 365, 200, 40);
        label.setLabelFor(securityCodeField);
        
        streetAddressField = new JTextField(10);
        streetAddressField.setBounds(20, 395, 710, 40);
        
        this.add(label);
        this.add(streetAddressField);
    }
    
    /*
     * Initializes cstate UI dropdown menu, as well as the city and postal code UI fields.
     */
    
    private void initCityStatePostalCode() {        
        cityField = new JTextField(10);
        cityField.setBounds(20, 445, 320, 40);
        
        stateDropdown = new JComboBox<>(getStates());
        stateDropdown.setBounds(350, 445, 160, 40);
        
        postalCodeField = new JTextField(10);
        postalCodeField.setBounds(520, 445, 210, 40);
        postalCodeField.addKeyListener(new KeyAdapter() {
            @Override
            public void keyTyped(KeyEvent e) {
                if (postalCodeField.getText().length() >= 5) {
                    e.consume();
                } else if (e.getKeyChar() < 48 || e.getKeyChar() > 57) {
                    e.consume();
                }
            }
        });
        
        this.add(cityField);
        this.add(stateDropdown);
        this.add(postalCodeField);
    }
    
    /*
     * Gets the values for the months dropdown menu.
     */
    
    private String[] getMonths() {
        return new String[]{
            "--Month--",
            "January",
            "February",
            "March",
            "April",
            "May",
            "June",
            "July",
            "August",
            "September",
            "October",
            "November",
            "December"
        };
    }
    
    /*
     * Gets the values for the days dropdown menu.
     */

    private String[] getDays() {
        String[] days = new String[32];
        
        for (int i = 0; i <= 31; i++) {
            if (i == 0) {
                days[i] = "--Day--";
            } else {
                days[i] = String.valueOf(i);
            }
        }
        
        return days;
    }
    
    /*
     * Gets the values for the years dropdown menu.
     */
    
    private String[] getYears() {
        String[] years = new String[73];
        
        for (int i = 1949; i <= 2021; i++) {
            if (i == 1949) {
                years[0] = "--Year--";
            } else {
                years[i - 1949] = String.valueOf(i);
            }
        }
        
        return years;
    }
    
    /*
     * Gets the values for the states dropdown menu.
     */
    
    private String[] getStates() {
        return new String[] {
            "--State--",
            "AL",
            "AK",
            "AS",
            "AZ",
            "AR",
            "CA",
            "CO",
            "CT",
            "DE",
            "DC",
            "FM",
            "FL",
            "GA",
            "GU",
            "HI",
            "ID",
            "IL",
            "IN",
            "IA",
            "KS",
            "KY",
            "LA",
            "ME",
            "MH",
            "MD",
            "MA",
            "MI",
            "MN",
            "MS",
            "MO",
            "MT",
            "NE",
            "NV",
            "NH",
            "NJ",
            "NM",
            "NY",
            "NC",
            "ND",
            "MP",
            "OH",
            "OK",
            "OR",
            "PW",
            "PA",
            "PR",
            "RI",
            "SC",
            "SD",
            "TN",
            "TX",
            "UT",
            "VT",
            "VI",
            "VA",
            "WA",
            "WV",
            "WI",
            "WY"
        };
    }
}
```
{% endcode %}

This is mostly for show. It doesn't actually do any card validations or charging. It does update the available copies in inventory based on the order, though.

## Running the Application

To run the application, we need a class that has a `main` method. Our `MidtownComics` class handles putting all the views in place, as well as attaching the controller to each one. We're using a `CardLayout` to easily swap out currently visible views.

{% code title="MidtownComics.java" %}
```java
package org.ucvts.comics;

import java.awt.CardLayout;

import javax.swing.JFrame;
import javax.swing.JPanel;
import javax.swing.SwingUtilities;

import org.ucvts.comics.controller.ViewManager;
import org.ucvts.comics.view.CartView;
import org.ucvts.comics.view.InventoryView;
import org.ucvts.comics.view.ProductView;
import org.ucvts.comics.view.OrderView;

@SuppressWarnings("serial")
public class MidtownComics extends JFrame {

    public static final int InventoryViewIndex = 0;
    public static final int ProductViewIndex = 1;
    public static final int CartViewIndex = 2;
    public static final int OrderViewIndex = 3;
    
    public static final String InventoryView = "InventoryView";
    public static final String ProductView = "ProductView";
    public static final String CartView = "CartView";
    public static final String OrderView = "OrderView";
    
    /**
     * Initializes the application views and frame.
     */
    
    public void init() {
        JPanel views = new JPanel(new CardLayout());
        ViewManager manager = ViewManager.getInstance(views);

        // add child views to the parent container

        views.add(new InventoryView(manager), InventoryView);
        views.add(new ProductView(manager), ProductView);
        views.add(new CartView(manager), CartView);
        views.add(new OrderView(manager), OrderView);
        
        // configure application frame
        
        this.getContentPane().add(views);
        this.setBounds(0, 0, 750, 750);
        this.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
        this.setLocationRelativeTo(null);
        this.setResizable(false);
        this.setVisible(true);
    }

    ////////// MAIN METHOD /////////////////////////////////////////////////////////
    
    public static void main(String[] args) {
        SwingUtilities.invokeLater(new Runnable() {
            public void run() {
                try {
                    new MidtownComics().init();
                } catch (Exception e) {
                    e.printStackTrace();
                }
            }
        });
    }
}
```
{% endcode %}

Feel free to experiment with this as you wish.

## Deliverables

1. Submit your repository URL.

Follow the tutorial and implement a working Midtown Comics application. It should match mine in every way. We'll enhance this code together as we get to [the second half of this tutorial](midtown-comics-pt-2.md).

## Deadline

All submissions are due on Canvas by 11:59pm on Sunday, November 22, 2020.

