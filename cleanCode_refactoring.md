# Refactoring

### Rename
- Cambiar nombres variables, parámetros, métodos, clases, etc
```java
public class Conversor {
	public float conv (float c) {
		float x = c * 166.386f;
		return x;
	   }
}
//
// REFACTORIZADA
//
public class Conversor {
	private static final float EUROS_PESETAS_CHANGE_RATE = 166.386f;

	public float eurosToPesetas (float euros) {
		float pesetas = euros * EUROS_PESETAS_CHANGE_RATE;
		return pesetas;
	   }
}
```

### Encapsulate
- hacer privados atributos y métodos para que no sean usados desde fuera
- usar geters and seters


### Magic Numbers
- Cambiar números poco explicativos por constantes

```java
	public String generatePassword(int length) throws Exception {
		if (length < 6 || length > 15) { ... }
	}		
//
// REFACTORIZADA
//
	private static final int MAX_PASSWORD_LENGTH = 15;
	private static final int MIN_PASSWORD_LENGTH = 6;

	public String generatePassword(int length) throws Exception {
		if (length < MIN_PASSWORD_LENGTH || length > MAX_PASSWORD_LENGTH) {
			throw new Exception("Wrong password length: " + length);
		}
	}

```

### Extract Method
- Dividir chorizo largo en métodos

### Inline Method
- Un paso más que el anterior
```java
	public String clean(String title) {
		String url = trimSpaces(title);

		url = removeSpecialChars(url);
		url = replaceSpaces(url);
		url = url.toLowerCase();

		return url;
	}
//
// REFACTORIZADA
//
	public String clean (String title) {
		return title.trim()
					.replaceAll("[\\.\\:\\,\\?\\!\\_\\;]", "")  // Replaces special chars
					.replaceAll("[\\s]+"," ")                   // Replace duplicated spaces
					.replaceAll("[\\s]","-");                   // Replace spaces with hyphen
	}
```

### Parameter Object
- convertir parámetros en un objeto

### Replace Temp with query
- Reemplazar variables de una funcion por llamada a método
```java
	public float calculateAverage(float homework, float exam) {
		float mark = (homework + exam) / 2;

		if (hasGoodAttitude) {
			return mark + 1;
		} else {
			return mark;
		}
	}
//
// REFACTORIZADA
//
	public float calculateAverage(float homework, float exam) {
		if (hasGoodAttitude) {
			return mark(homework, exam) + 1;
		} else {
			return mark(homework, exam);
		}
	}

	private float mark(float homework, float exam) {
		return (homework + exam) / 2;
	}
```

### Explaining Variable
- utilizar variables con nombres que aclaren lo que se pretende hacer
```java
	if ((age > 17 && age < 66) && (salary - (salary * 0.2f)) < 1000f && totalAmount * 0.5 < 100) {
//
// REFACTORIZADA
//
		boolean isWorkingAge = (age > 17 && age < 66);
		boolean isLowSalary = (salary - (salary * 0.2f)) < 1000f;
		boolean isLittleAmount = totalAmount * 0.5 < 100;
		
		if (isWorkingAge && isLowSalary && isLittleAmount) { ... }
```

### Split Temporary Variable
```java
	public float totalPrice (float price, float vat, float discount) {
		float temp = 0;
		temp = (vat * price) / 100;
		System.out.println("Applied vat: " + temp);
		temp = price + temp;
		System.out.println("Total with vat: " + temp);
		return temp - discount;
	}

//
// REFACTORIZADA
//
	public float totalPrice (float price, float vat, float discount) {
		float appliedVat = (vat * 100) / price;		
		System.out.println("Applied vat: " + appliedVat);
		
		float priceWithVat = price + appliedVat;
		System.out.println("Total: " + priceWithVat);
		
		return priceWithVat - discount;
	}
	//
	// y aun un nivel mas
	public float totalPrice (float price, float vat, float discount) {
		return price + appliedVat(price, vat) - discount;
	}

	private float appliedVat (float price, float vat) {
		return (vat * price) / 100;
	}
```

### Remove Parameter Assignments
- Evitar que se asignen valores a un parametro dentro de un metodo
- Si es necesrio add variable temporal

### Replace Method with Method Object
- Extraer método complejo en una clase aparte

###  Decompose Conditional
```java
	public float calculatePayment (float price, float discount, float vat) {
		float payment = 0;
		
		if (customer.getAge() < 18 || customer.getAge() > 65 ) {
		  payment = price * discount * vat;
		} else {
		  payment = price * vat;
		}
		return payment;
//
// REFACTORIZADA
//
	public float calculatePayment (float price, float discount, float vat) {
		float payment = 0;
		
		if (canApplyDiscount() ) {
		  payment = price * discount * vat;
		} else {
		  payment = price * vat;
		}

		return payment;
	}

	private boolean canApplyDiscount() {
		return customer.getAge() < 18 || customer.getAge() > 65;
	}
```

### Consolidate Conditional Expression
- extrae una expresión condicional compleja a un método.
```java
	public float calculateTotal(float subtotal, float vat) {
		if (customer.getAge() < 18)
			return 0;
		if (new GregorianCalendar().get(Calendar.YEAR) > year)
			return 0;
		if (subtotal < 0.5f)
			return 0;

		return subtotal * vat;
	}
//
// REFACTORIZADA
//
	public float calculateTotal(float subtotal, float vat) {
		if (isUselessToCharge(subtotal)) {
			return 0;
		} else {
			return subtotal * vat;
		}
	}

	private boolean isUselessToCharge(float subtotal) {
		return (customer.getAge() < 18) 
				|| (new Date().getYear() > year) || (subtotal < 0.5f);
	}
```

### Consolidate Duplicate Conditional Fragments
- Extraer código que se repite dentro de distintas expresiones condicionales
```java
	public float calculateTotal (float vat, float discount) {
		float subtotal = 0;
		if (customer.isVip()) {
			subtotal = (price * qty) - discount;
			subtotal = subtotal * (1 + (vat/100));
			return subtotal;
		} else {
			subtotal = (price * qty);
			subtotal = subtotal * (1 + (vat/100));
			return subtotal;
		}
	}
//
// REFACTORIZADA
//
	public float calculateTotal (float vat, float discount) {
		float subtotal = 0;
		if (customer.isVip()) {
			subtotal = (price * qty) - discount;
		} else {
			subtotal = (price * qty);
		}
		return subtotal * (1 + (vat/100));
	}
```

### Remove Control Flags

```java
	public int indexOf (String friend) {
		boolean found = false;
		int i = 0;
		
		while (i < friends.length && !found ) { 
			  if (friends[i].equals(friend)) {
			    found = true;
			  }
			  i++;
			}
		
		if (found) {
			return (i-1);
		} else {
			return -1;
		}
	}
//
// REFACTORIZADA
//
	public int indexOf (String friend) {

		int i = 0;
		
		while (i < friends.length ) {
			
			  if (friends[i].equals(friend)) {
			    break;
			  }
			  i++;
			}
		
		if (i == friends.length) {
			return -1;
		} else {
			return i;
		}
	}
```

### Replace Nested Conditional with Guard Clauses

```java
	public float priceForPassenger (Passenger passenger) {
		float price = 0;
		if (passenger.isAChild()) {
			  price = childDiscount();
			} else {
			  if (passenger.isUnemployed()) {
			  price = unemployedDiscount();
			  } else {
			    if (isChristmas()) {
			    price = 0;
			    } else {
			    price = normalPrice();
			    }
			  }
			}
		return price;
	}
//
// REFACTORIZADA
//
	public float priceForPassenger (Passenger passenger) {	
		if (passenger.isAChild()) return childDiscount();
		if (passenger.isUnemployed()) return unemployedDiscount();
		if (isChristmas()) return 0;
		return normalPrice();

	}

	private float unemployedDiscount() {
		return BASE_PRICE * distance * UNEMPLOYED_DISCOUNT;
	}

	private float normalPrice() {
		return BASE_PRICE * distance;
	}

	private boolean isChristmas() {
		return false;
	}

	private float childDiscount() {
		return BASE_PRICE * distance * CHILDREN_DISCOUNT;
	}

```

### Replace Conditional with Polymorphism
- trata de evitar las estructuras condicionales como los switch/case aplicando el polimorfismo.
```java
	public class Vehicle {

		// ...

		public int move () {
			  int result = 0;
			  switch (vehicleType) {
			    case CAR:
			              result = speed * acceleration * 5;
			              break;
			    case BIKE:
			              result = speed * 10;
			              break;
			    case PLANE:
			              result = acceleration * 2;
			              break;
			  }
			  return result;
		}
	}
//
// REFACTORIZADA
//
	public abstract class Vehicle {
		protected int vehicleType;
		protected int speed;
		protected int acceleration;

		public Vehicle(int vehicleType, int speed, int acceleration) {
			this.vehicleType = vehicleType;
			this.speed = speed;
			this.acceleration = acceleration;
		}
		
		 public abstract int move ();
	}

	public class Bike extends Vehicle {	
		public Bike(int vehicleType, int speed, int acceleration) {
			super(vehicleType, speed, acceleration);
		}
		
		 @Override
		 public int move () {
			    return speed * 10;
		}
	}	
```

### Introduce Null Object
- evitar que se retornen valores null para no tener que hacer comprobaciones
```java
	public class Warrior {
		private Weapon weapon;

		public Warrior(Weapon weapon) {
			this.weapon = weapon;
		}

		public int attack() {
			int damage = 0;
			if (weapon == null) {
				damage = 5;
			} else {
				damage = 2 + weapon.getDamage();
			}
			return damage;
		}
	}
//
// REFACTORIZADA
//
	public class Warrior {
		private Weapon weapon;

		public Warrior(Weapon weapon) {
			this.weapon = weapon;
		}

		public int attack() {
			return 2 + weapon.getDamage();
		}
	}

	public class NullWeapon extends Weapon {
		public NullWeapon(int damage) {
			super(damage);
		}

		@Override
		public int getDamage() {
			return 0;
		}
	}
```

### Separate Query from Modify
- se debe aplicar para que en un mismo método no se pueda consultar y a la vez modificar los atributos de una clase.
```java
	public class Vehicle {
		private int horsePower;
		private String type;

		public Vehicle(int power) {
			initVehicleAndGetType(power);
		}

		private String initVehicleAndGetType(int power) {
			horsePower = power;

			if (power >= 10) {
				type = "Truck";
			} else if (power > 5 && power < 10) {
				type = "Car";
			} else {
				type = "Bike";
			}
			
			return type;
		}
	}
//
// REFACTORIZADA
//
	public class Vehicle {
		private int horsePower;
		private String type;

		public Vehicle(int power) {
			init(power);
		}

		private void init(int power) {
			setPower(power);
			setType();
		}

		private void setPower(int power) {
			horsePower = power;
		}

		private void setType() {
			if (horsePower >= 10) {
				type = "Truck";
			} else if (horsePower > 5 && horsePower < 10) {
				type = "Car";
			} else {
				type = "Bike";
			}
		}
	}
```

### Parameterize Method
- se puede aplicar cuando tenemos métodos que hacen practicamente lo mismo y que se pueden agrupar en un único método si metemos un parámetro.
- Es el inverso de Replace Parameter with Explicit Methods
```java
	public float charge() {
		if (customer.getAge() < 18) {
			return chargeWithUnderageDiscount();
		} else if (customer.payInCash()) {
			return chargeWithCashDiscount();
		} else {
			return chargeNormal();
		}
	}

	private float chargeWithUnderageDiscount() {
		float total = subtotal * 0.5f;
		return total;
	}

	private float chargeWithCashDiscount() {
		float total = subtotal * 0.8f;
		return total;
	}

	private float chargeNormal() {
		return subtotal;
	}
//
// REFACTORIZADA
//
	public float charge() {

		if (customer.getAge() < 18) {
			return charge(0.5f);
		} else if (customer.payInCash()) {
			return charge(0.8f);
		} else {
			return charge();
		}
	}

	public float charge (float discount) {
		return subtotal * discount;
	}
```

###  Replace Parameter with Explicit Methods
- se aplica cuando tenemos un método que hace distintas cosas según un parámetro y queremos separarlo en métodos distintos.
- Es el inverso de Parameterize Method.
```java
	public class Vehicle {

		private int acceleration;
		private int speed;
		
		public Vehicle(int acceleration, int speed) {
			this.acceleration = acceleration;
			this.speed = speed;
		}

		public void initVehicle (int type, int value) {
			if (type == 1) {
				acceleration = value;
				return;
			}

			if (type == 2 || type == 3)  {
				speed = value;
				return;
			}
		}
	}
//
// REFACTORIZADA
//
public class Vehicle {

	private int acceleration;
	private int speed;
	

	public Vehicle(int acceleration, int speed) {
		this.acceleration = acceleration;
		this.speed = speed;
	}

	public void setAcceleration(int acceleration) {
		this.acceleration = acceleration;
	}

	public void setSpeed(int speed) {
		this.speed = speed;
	}
}
```

### Substitute Algorithm
- consiste sencillamente en modificar el algoritmo interno de un método, sin que cambie su signatura.

```java
	public class Order {
		public float applyVAT (int vatType) {
			float result = 0;

			switch (vatType) {
				case 1:
					result = 1.04f;
					break;
				case 2:
					result = 1.18f;
					break;
				case 3:
					result = 1.21f;
					break;
				default:
					result = -1;
					break;
		}
		return result;
		}
	}
//
// REFACTORIZADA
//
	public class Order {
		public float applyVAT (int vatType) {
			float result[] = {-1, 1.04f, 1.18f, 1.21f};

			if (vatType > 0 && vatType < result.length) {
				return result[vatType];
			}
			return -1;
		}
	}
```

### Extract Class
- consiste en sacar parte del código de una clase a otra. 
- Se puede aplicar cuando la clase tiene demasiadas funcionalidades.
- Es el inverso de Inline Class.

### Inline Class
 - consiste en meter el código de una clase dentro de otra. 
 - Se puede aplicar cuando la clase es tan sumamente simple que no merece la pena que sea una clase aparte.
- Es el inverso de Extract Class.

### Hide Delegate
- consiste en ocultar llamadas a clases, y hacerlo a través de alguien intermedio.
- Es el inverso de Remove Middle Man.

```java
	public class Main {
		private Player player;
		private Die die;
		
		public Main () {
			init();
		}
		
		private void init () {
			player = new Player();
			die = player.getDie();
		}
		
		public int roll () {
			return die.roll();
		}
	}

	public class Player {
		private Die die;
		
		public Player () {
			this.die = new Die();
		}
		
		public int roll () {
			return die.roll();
		}

		public Die getDie() {
			return die;
		}
	}

	public class Die {
		private Random random = new Random();
		
		public int roll() {
			return random.nextInt(6) + 1;
		}
	}
//
// REFACTORIZADA
//
	public class Main {
		private Player player;
		
		public Main () {
			init();
		}
		
		private void init () {
			player = new Player();
		}
		
		public int roll () {
			return player.roll();
		}
	}

	public class Player {
		private Die die;
		
		public Player () {
			this.die = new Die();
		}
		
		public int roll () {
			return die.roll();
		}
	}

	public class Die {
		private Random random = new Random();
		
		public int roll() {
			return random.nextInt(6) + 1;
		}
	}

```

###  Introduce Foreign Method
- consiste poder añadir un nuevo método a una clase que no podemos modificar. 
- El método se añade en el cliente.
```java
	public class PasswordChecker {
		public String improvePassword (String password) {
			if (password.length() < 5) {
				return "****" + password + "****";
			} else {
				return password;
			}
		}
	}
//
// REFACTORIZADA
//
public class PasswordChecker {
	public String improvePassword (String password) {
		if (password.length() < 5) {
			return makeItPalindrome(password);
		} else {
			return password;
		}
	}
	
	// We extend String functionality
	private static String makeItPalindrome (String password) {
		return new String(password 
							+ new StringBuilder(password)
													.reverse()
														.toString());
	}
}
```

### Introduce Local Extension
- es igual a Hide Delegate ??? (misma descripción)
- consiste en ocultar llamadas a clases, y hacerlo a través de alguien intermedio.
- Es el inverso de Remove Middle Man.

```java
	public class Main {
		private Conversor conversor = new Conversor();
		
		public double convert (double amount) {
			return conversor.euro2Dollar(amount);
		}
	}
	public class Conversor {
		public double euro2Dollar (double qty) {
			return qty * 1.4d;
		}

		public double dollar2Euro (double qty) {
			return qty * 0.6d;
		}
	}
//
// REFACTORIZADA
//
public class Main {
	private CoolConversor conversor = new CoolConversor();
	
	public double convert (double amount) {
		return conversor.euro2Dollar(amount);
	}
}
public class Conversor {
	public double euro2Dollar (double qty) {
		return qty * 1.4d;
	}

	public double dollar2Euro (double qty) {
		return qty * 0.6d;
	}
}
public class CoolConversor extends Conversor {
	public double euro2Pound (double qty) {
		return qty * 0.8d;
	}

	public double pound2Euro (double qty) {
		return qty * 1.3d;
	}
}
```

### Replace Data Value with Object
 - es una especie de caso especial de Extract Class. 
 - Se trata de convertir un conjunto de atributos en una clase.
 - Ejemplo extraer la direccion como una nueva clase

### Encapsulate Collection
- trata de ocultar los detalles de una colección de datos a través de la encapsulación.
```java
	public class Team {
		private String name;
		private Date creation;
		private ArrayList<Player> players = new ArrayList<Player>();

		public Team(String name, Date creation) {
			this.name = name;
			this.creation = creation;
		}

		public String getName() {
			return name;
		}

		public Date getCreation() {
			return creation;
		}
		
		public ArrayList<Player> getPlayers() {
			return players;
		}

		public int totalPlayers() {
			return players.size();
		}
	}
//
// REFACTORIZADA
//
	public class Team {
		private String name;
		private Date creation;
		private ArrayList<Player> players = new ArrayList<Player>();

		public Team(String name, Date creation) {
			this.name = name;
			this.creation = creation;
		}

		public String getName() {
			return name;
		}

		public Date getCreation() {
			return creation;
		}
		
		public Player getPlayer (int index) {
			return players.get(index);
		}

		public void addPlayer (Player player) {
			players.add(player);
		}

		public void  removePlayer (int index) {
			players.remove(index);
		}
		
		public int totalPlayers() {
			return players.size();
		}
	}
```

###  Pull Up
- se aplica en relaciones de herencia, cuando tenemos atributos y métodos que se encuentran en todas las extensiones y se llevan a la clase base.

### Push Down
- se aplica en relaciones de herencia, cuando tenemos atributos y métodos en la clase base no se utilizan en todas las clases extendidas, así que esos atributos y métodos se llevan solo a aquellas que los precisan.

### Replace Array with Object
 - es una especie de caso especial de Extract Class. 
 - Se trata de convertir un array de datos en una clase.
```java
	public class Airplane {

		private String model;
		private String pilotData[] = new String[3];

		public Airplane(String model) {
			this.model = model;
		}

		public void initPilot(String name, String license, int flightHours) {
			pilotData[0] = name;
			pilotData[1] = license;
			pilotData[2] = Integer.toString(flightHours);
		}

		@Override
		public String toString() {
			return "Airplane [model=" + model + ", pilot=" + pilotData[0] + "]";
		}
	}
//
// REFACTORIZADA
//
	public class Airplane {

		private String model;
		private Pilot pilot;

		public Airplane(String model) {
			this.model = model;
		}

		public void initPilot(String name, String license, int flightHours) {
			pilot = new Pilot(name, license, flightHours);
		}

		@Override
		public String toString() {
			return "Airplane [model=" + model + ", pilot=" + pilot + "]";
		}
	}

	public class Pilot {
		private String name;
		private String license;
		private int flightHours;

		public Pilot (String name, String license, int flightHours) {
			this.name = name;
			this.license = license;
			this.flightHours = flightHours;
		}

		public String toString() {
			return name;
		}
	}
```
