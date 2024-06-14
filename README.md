# MenuItemSelection
MIST 4600 Group Project, selecting items off of a fictional menu for a restaurant
Baseline:
The user of the program adds whatever menu items they are satisfied with. Then they are prompted if they are a part of the rewards program.

Scenario 1:
The first scenario gives the user the option to either say YES to being a part of the rewards program, and provide an id that is within the system and thus obtain a discount, or completely ignore the rewards system entirely. 

Scenario 2:
The second scenario user indicates that they are NOT a part of the rewards program. They are then asked if they would like to become a rewards member. Upon selecting yes, they are then able to make their own id, provided that it does not exist, and meets the requirements (specified by the program). After successfully completing that part of the program, the rewards id is stored within the system and is able to be used for future orders.



Is-a / has-a explanation:
There is an is-a relationship between the CustomerInterfaceV2 class and the MenuItemV2 class, as CustomerInterfaceV2 extends MenuItemV2. This means that CustomerInterfaceV2 is a subclass of MenuItemV2, and inherits all of its methods and fields.
There is a has-a relationship between the MenuItemV2 class and several other classes, as they are using instances of those classes. For example:
MenuItemV2 has a Scanner object (scnr).
MenuItemV2 has an array of integers (menuInventory).
MenuItemV2 has an ArrayList of MenuItemV2 objects (order).
MenuItemV2 has a HashMap of String keys and Double values (menu).
MenuItemV2 has a HashSet of Integer objects (rewardsMembers).
Encapsulation: The attributes of the MenuItemV2 class are declared as protected, and their access is restricted through getters methods that are public.
Composition: The CustomerInterfaceV2 class extends the MenuItemV2 class, creating a composition relationship between them.
Private attributes, public methods, accessors, and mutators: The MenuItemV2 class has private attributes (itemName, itemPrice, salesTax) that are accessed only through getter methods (getMenuItem(), getMenuItemQuant()). The class also has public methods (printMenu(), setItem(), setItemQuantity()) that can be accessed from other classes. These methods are used to set and retrieve the values of the attributes.
Superclasses and subclasses: The CustomerInterfaceV2 class extends the MenuItemV2 class, creating a superclass-subclass relationship between them.
Method overloading and overriding: There are no examples of method overloading in this code, but the printMenu() method is an example of method overriding. The printMenu() method is defined in the MenuItemV2 class and overridden in the CustomerInterfaceV2 class.
Static data and methods: The MenuItemV2 class has static data (menuInventory, order, menu, rewardsMembers) and static methods (printMenu(), setItem(), setItemQuantity()) that can be accessed without instantiating the class.
// This is the MenuItemsV2 class where the majority of our methods for the menu lie

package mist4600;
import java.util.*;
public class MenuItemV2 {
	protected int menuItem;
	protected int menuItemQuant;
	protected String itemName;
	protected double itemPrice;
	final static double salesTax = 1.07;
	//final static double discount = 0.90;
	static Scanner scnr = new Scanner(System.in);
	
	static int[] menuInventory = new int[2];
	static ArrayList<MenuItemV2> order = new ArrayList<>();
	static HashMap<String, Double> menu = new HashMap<>();
	static HashSet<Integer> rewardsMembers = new HashSet<>();
	
	public int getMenuItem() {
		return menuItem;
	}
	public int getMenuItemQuant() {
		return menuItemQuant;
	}
	public MenuItemV2(int item, int quant) {
		menuItem = item;
		menuItemQuant = quant;
	}
	public static void printMenu() {
		System.out.println("Menu:");
		System.out.println("1. Whopper $8.99 In Stock: " + menuInventory[0]);
		System.out.println("2. Chicken Fries $6.99 In Stock: " + menuInventory[1]);
		System.out.println();
	}
	public static void setItem() {
		System.out.println("What would you like to order? (1 or 2)");
		int userInput = scnr.nextInt();
		String alpha = "yes";
		while (alpha.equals("yes")) {
			if (userInput == 1) {
				// ask item quant
				setItemQuantity(userInput);
				alpha = "no";
			} else if (userInput == 2) {
				// ask item quant
				setItemQuantity(userInput);
				alpha = "no";
			} else {
				System.out.println("Invalid input, enter 1 or 2.");
				userInput = scnr.nextInt();
			}
		}
	}
	public static void setItemQuantity(int j) {
		String item = "";
		if (j == 1) {
			item = "Whoppers";
		} else if (j == 2) {
			item = "Chicken Fries";
		}
		System.out.println("How many " + item + " would you like?");
		int userInput = scnr.nextInt();
		String alpha = "yes";
		while (alpha.equals("yes")) {
			if ((j == 1) && (userInput <= menuInventory[j - 1]) && (userInput > 0)) {
				alpha = "no";
				MenuItemV2 item1 = new MenuItemV2(j, userInput);
				order.add(item1);
				menuInventory[j - 1] = menuInventory[j - 1] - userInput;
			} else if ((j == 2) && (userInput <= menuInventory[j - 1]) && (userInput > 0)) {
				alpha = "no";
				MenuItemV2 item1 = new MenuItemV2(j, userInput);
				order.add(item1);
				menuInventory[j - 1] = menuInventory[j - 1] - userInput;
			} else {
				System.out.println("Invalid quantity, enter something less than or equal to " + menuInventory[j - 1]);
				userInput = scnr.nextInt();
			}
		}
		System.out.println("Would you like to add another item? (y/n)");
		String another = scnr.next();
		String beta = "yes";
		while (beta.equals("yes")) {
			if (another.equals("y")) {
				beta = "no";
				printMenu();
				setItem();
			} else if (another.equals("n")) {
				beta = "no";
			} else {
				System.out.println("Invalid. enter y or n");
				another = scnr.next();
			}
		}
	}
	public static void rewardsMember() {
		double discount = 0.90;
		System.out.println("Are you a rewards member? (y/n)");
		String member = scnr.next();
		String alpha = "yes";
		while (alpha.equals("yes")) {
			if (member.equals("y")) {
				System.out.println("Enter ID: (or 9999 if you want to exit)");
				int userID = scnr.nextInt();
				String beta = "yes";
				while (beta.equals("yes")) {
					if (rewardsMembers.contains(userID)) {
						printReceipt(discount);
						alpha = "no";
						beta = "no";
					} else if (!(rewardsMembers.contains(userID)) && (userID == 9999)) {
						alpha = "no";
						beta = "no";
						printReceipt();
					} else {
						System.out.println("Invalid. enter your ID or \"9999\"");
						userID = scnr.nextInt();
					}
				}
			} else if (member.equals("n")) {
				// asks to be member
				System.out.println("Would you like to be a member for a 10% discount on future transactions? (y/n)");
				String x = scnr.next();
				String beta = "yes";
				while (beta.equals("yes")) {
					if (x.equals("y")) {
						createMember();
						alpha = "no";
						beta = "no";
					} else if (x.equals("n")) {
						printReceipt();
						alpha = "no";
						beta = "no";
					} else {
						System.out.println("Invalid. enter y or n");
					}
				}
			} else {
				System.out.println("Invalid. enter y or n.");
				member = scnr.next();
			}
		}
	}
	public static void createMember() {
		System.out.println("Enter your preferred ID: (4 digits starting with 1-9)");
		int userInputID = scnr.nextInt();
		String alpha = "yes";
		while (alpha.equals("yes")) {
			if ((userInputID < 9999) && (userInputID > 999) && !(rewardsMembers.contains(userInputID))) {
				rewardsMembers.add(userInputID);
				alpha = "no";
				System.out.println("You will receive a 10% discount on future transactions.");
				printReceipt();
			} else {
				System.out.println("Invalid input, try again!");
				userInputID = scnr.nextInt();
			}
		}
	}
	public static void printReceipt() {
		double total = 0.00;
		System.out.println("\nReceipt:");
		for (int i = 0; i < order.size(); i++) {
			String itemName = "fortnite";
			if (order.get(i).getMenuItem() == 1) {
				itemName = "Whopper";
			} else {
				itemName = "Chicken Fries";
			}
			total = total + menu.get(itemName) * order.get(i).getMenuItemQuant();
			System.out.print("+" + order.get(i).getMenuItemQuant());
			System.out.print(" " + itemName + "(s) $");
			System.out.print(String.format("%.2f", menu.get(itemName) * order.get(i).getMenuItemQuant()));
			System.out.println();
		}
		double afterTax = total * salesTax;
		System.out.println("_______________________________");
		System.out.println("Total before tax: $" + (String.format("%.2f", total)));
		System.out.println("Added tax $" + String.format("%.2f", afterTax - total));
		System.out.println("Total after tax: $" + String.format("%.2f", afterTax));
	}
	public static void printReceipt(double discount) {
		System.out.println("\nReceipt:");
		double total = 0.00;
		for (int i = 0; i < order.size(); i++) {
			String itemName = "fortnite";
			if (order.get(i).getMenuItem() == 1) {
				itemName = "Whopper";
			} else {
				itemName = "Chicken Fries";
			}
			total = total + menu.get(itemName) * order.get(i).getMenuItemQuant();
			System.out.print("+" + order.get(i).getMenuItemQuant());
			System.out.print(" " + itemName + "(s) $");
			System.out.print(String.format("%.2f", menu.get(itemName) * order.get(i).getMenuItemQuant()));
			System.out.println();
		}
		double discountTotal = total * discount;
		double afterTax = discountTotal * salesTax;
		double addedDiscount = total - discountTotal;
		System.out.println("_______________________________");
		System.out.println("Total before tax with discount: $" + String.format("%.2f", discountTotal));
		System.out.println(" You saved: -$" + String.format("%.2f", addedDiscount));
		System.out.println("Added tax: $" + String.format("%.2f", afterTax - discountTotal));
		System.out.println("Total after tax: $" + String.format("%.2f", afterTax));
	}
}
// This is the CustomerInterfaceV2 class that inherits MenuItemsV2 methods

package mist4600;
import java.util.Scanner;

public class CustomerInterfaceV2 extends MenuItemV2 {
	
	public CustomerInterfaceV2(int item, int quant) {
		super(item, quant);
		// TODO Auto-generated constructor stub
	}
	public static void main(String[] args) {
		// TODO Auto-generated method stub
		Scanner scnr = new Scanner(System.in);
		
		
		
		menuInventory[0] = 10;
		menuInventory[1] = 15;
		
		rewardsMembers.add(3515);
		rewardsMembers.add(5357);
		rewardsMembers.add(9546);
		rewardsMembers.add(5661);
		rewardsMembers.add(6969);
		rewardsMembers.add(4420);
		rewardsMembers.add(3779);
		rewardsMembers.add(2167);
		rewardsMembers.add(9535);
		rewardsMembers.add(3872);
		
		menu.put("Whopper", 8.99);
		menu.put("Chicken Fries", 6.99);
		
		printMenu();
		setItem();
		rewardsMember();
		//System.out.println(order.get(0).getMenuItem());
		//System.out.println(order.get(0).getMenuItemQuant());
		
		//System.out.println(order.get(1).getMenuItem());
		//System.out.println(order.get(1).getMenuItemQuant());
	
	}
}
// Create new files for each class and try it out for yourself!
