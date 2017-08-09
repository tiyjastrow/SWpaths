## About Java

* Sun created Java back in 1995.
* Initially meant for embedded devices, but focus shifted to the internet.
* Google search [Top Programming Languages](https://www.google.com/search?q=Top+Programming+Languages&oq=Top+Programming+Languages&aqs=chrome..69i57j69i65l2.678j0j1&sourceid=chrome&ie=UTF-8)

## Download and Install Greenfoot IDE
Download [Greenfoot] (http://www.greenfoot.org/download)

From the Greenfoot site  
"Greenfoot teaches object orientation with Java. Create 'actors' which live in 'worlds' to build games, simulations, and other graphical programs."  

Greenfoot provides a GUI framework where we can program a Pokemon world and add Pokemon characters to that world. Hence we already have a lot of the programming done for use. For example you will see the PokemonWorld call super to set the size of the world; each Pokemon will have an act method that is called when it is the Pokemon's turn to do something.

## Download Pokemon scenario
* Download this [Pokemon Scenario](https://tiy-learn-content.s3.amazonaws.com/01400b2c-pokemon-20160928T011050Z.zip) (182 KB) 

  * Click on the pokemon -> download
  * double-click the zip file to expand
* Start Greenfoot
* Click on Scenario -> Open -> pokemon

## Pokemon Battle 

#### Pokemon World
Right click on World and then click New Subclass. Name the subclass PokemonWorld, set the background image to white.jpg and click OK.  
  
Now double click on the PokemonWorld to open the Greenfoot editor. Replace the current call to super with the following code. that will be run when the world is created.

```java
public PokemonWorld()
{   
    // Create the world with width=60, height=60, cell size=10 and bounded
    super(60, 60, 10, true); 
}
```
Save the class.  
Run the game and you should see a white background then pause the game.

#### Pokemon
Now it is time to create a base Pokemon class. The base pokemon class will have state and behavior that is common among all Pokemon.  
Can you say DRY?  ("don't repeat yourself" principle)

For this battle game all Pokemon will have the following state:
* name
* health value (how healthy it is)
* damage value (how much damage it can do)
* An alive and dead image

Right click on Actor to create a new subclass named, 'Pokemon' with an Unspecified image.

Add the following variables to the Pokemon class just above the act method.
```java
String name;
int health;
int damage;
String aliveImage;
String deadImage;
```
Save the class.  
Run the game to be sure you do not have any problems (again should see a white world) and then pause the game.

#### Abra
Right click on Pokemon and create a new subclass Abra with the abra-alive.png image.

Add the following constructor to the Abra class above the act method.
```java
public Abra() {
    name = "Abra";
    health = 100;
    damage = 3;
    aliveImage = "abra-alive.png";
    deadImage = "abra-dead.png";
}
```
Save the class.  

Click once on Abra and then move the mouse over the world, hold the shift key and click to add Abra to the world.

#### Mankey
Right click on Pokemon and create a new subclass Mankey with the mankey-alive.png image.

Add the following constructor to the Mankey class above the act method.
```java
public Mankey() {
    name = "Mankey";
    health = 50;
    damage = 7;
    aliveImage = "mankey-alive.png";
    deadImage = "mankey-dead.png";
}
```
Save the class.  

#### Pikachu
Right click on Pokemon and create a new subclass Pikachu with the pikachu-alive.png image.

Add the following constructor to the Pikachu class above the act method.
```java
public Pikachu() {
    name = "Pikachu";
    health = 100;
    damage = 10;
    aliveImage = "pikachu-alive.png";
    deadImage = "pikachu-dead.png";
}
```
Save the class.

Run the game and then pause the game. If you have any problems then the game will not run.

#### Move Pikachu
When run is clicked the Greenfoot framework will call a method name "act" on each actor. Let's make Pikachu call a move method when the act method is called. 

In the Pikachu class add the following lines of code to the act method to allow Pikachu to move to the right

```java
int x = getX();
int y = getY();

if (Greenfoot.isKeyDown("right")) {
    setLocation(x+3,y);
} 
```

Add Pikachu to the game and then run it and move Pikachu to the right (and then pause the game).

Flesh out move to move all four directions

```java
public void act() 
{
    int x = getX();
    int y = getY();

    if (Greenfoot.isKeyDown("right")) {
        setLocation(x+3,y);
    } else if (Greenfoot.isKeyDown("left")) {
        setLocation(x-3,y);
    } else if (Greenfoot.isKeyDown("up")) {
        setLocation(x,y-3);
    } else if (Greenfoot.isKeyDown("down")) {
        setLocation(x,y+3);
    } 
}
```

Add Pikachu to the game and then run it and move Pikachu in all four directions (and then pause the game).

#### Pikachu Check For Neighboring Pokemon
After the If-Then-Else code add the following to check to see if Pikachu shares the same space with another Pokemon.

```java
Actor actor = getOneObjectAtOffset(0, 0, Pokemon.class);
if (actor == null) {
    // do nothing
} else {
    battle((Pokemon)actor);        
}
```

This will not compile until we add a battle method. 

#### Pokemon Battle
It's time to write the code where Pikachu battles other Pokemon. The way they battle is that, for each iteration of the game, each Pokemon's health decreases by the damage of the other Pokemon until one or both are no longer greater than zero.

In the Pokemon class add the battle method above the act

```java
protected void battle(Pokemon enemy) {
    health -= enemy.damage;
    enemy.health -= damage;
    getWorld().showText(name + "'s health " + health, 10, 5);
    getWorld().showText(enemy.name + "'s health " + enemy.health, 10, 10);
}
```

Add Pikachu and Abra to the game and run it. Move Pikachu so that it touches Abra. As you can see this code runs forever. Reset the game.

Enhance battle to check if battle is over  

```java
protected void battle(Pokemon enemy) {
    if (health <= 0 || enemy.health <= 0) {
        return;
    }
    health -= enemy.damage;
    enemy.health -= damage;
    getWorld().showText(name + "'s health " + health, 10, 5);
    getWorld().showText(enemy.name + "'s health " + enemy.health, 10, 10);
}
```

Add Pikachu and Abra to the game and run it. Move Pikachu so that it touches Abra. Now the countdown stops. Pause the game.

Now let's show the Pokemon as dead. In battle add the following code at the end of the battle method.

```java
if (enemy.health <= 0) {
    showPokemonDead(enemy);
}

if (health <= 0) {
    showPokemonDead(this);
}
```

Add the new showPokemonDead method
```java
void showPokemonDead(Pokemon pokemon) {
    getWorld().showText(pokemon.name + " has died", 10, 15);
    pokemon.setImage(pokemon.deadImage);
    getWorld().repaint();
}
```

Add Pikachu, Abra and Mankey to the game and run it.

