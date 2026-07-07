# Game Events

> Source: https://help.shuffle.us/en/articles/11882157-game-events
> Fetched: 2026-07-07 (verbatim mirror, converted to markdown)

## What are game events

Previously, we have generated random floats between 0 to 1 using the generateFloats and byteGenerator functions. Game events translate these floats generated into actual game outcomes. This happens in different ways for different games. In Plinko, floats help to determine the direction of the ball drop. In Dice, floats help to determine the outcome of the dice roll.

Below is a detailed explanation of how we translate floats into fair events or outcome for every original game we have on our platform.

* * *

### Tower

In Tower, the objective of the game is to reach the top of the tower and save the princess by finding the keys and avoiding the poison apples. Each row of the tower contains a predefined number of safe tiles (keys) and poison apples, depending on the selected difficulty.

The game event in Tower is to determine the position of the safe tiles (or keys) in each row.

There are a total of 9 rows in the tower. The number of columns and distribution of keys and poison apples vary according to the chosen difficulty:

Easy: 4 columns - 3 keys and 1 poison apple per row

Medium: 3 columns - 2 keys and 1 poison apple per row

Hard: 2 columns - 1 key and 1 poison apple per row

Expert: 3 columns: - 1 key and 2 poison apples per row

Master: 4 columns - 1 key and 3 poison apples per row

The outcome for each row is determined by generating random positions of the keys (safe tiles) within the available columns based on the difficulty configuration.
    
    
    const TOWER_DIFFICULTY_TO_CONFIG: Record<TowerDifficulty, DifficultyConfig> = {  
     [TowerDifficulty.EASY]: {  
     count: 3,  
     columns: 4,  
     },  
     [TowerDifficulty.MEDIUM]: {  
     count: 2,  
     columns: 3,  
     },  
     [TowerDifficulty.HARD]: {  
     count: 1,  
     columns: 2,  
     },  
     [TowerDifficulty.EXPERT]: {  
     count: 1,  
     columns: 3,  
     },  
     [TowerDifficulty.MASTER]: {  
     count: 1,  
     columns: 4,  
     },  
    };  
      
    // Generate floats to determine the positions of safe tiles (keys) for each row  
    const floats = generateFloats({  
     serverSeed,  
     clientSeed,  
     nonce,  
     cursor,  
     count,  
    });  
      
    // Get the tower configuration based on the selected difficulty  
    const towerConfig = TOWER_DIFFICULTY_TO_CONFIG[difficulty];  
    const results: number[][] = [];  
    const resultPerRow = towerConfig.count;  
      
    // Iterate through each row to determine the positions of safe tiles for that row  
    for (let i = 0; i < TOWER_TOTAL_ROWS.toNumber(); i++) {  
     const result: number[] = [];  
     const values = Array.from(Array(towerConfig.columns).keys());  
      
     // Get positions for safe tiles in the current row  
     for (let j = 0; j < resultPerRow; j++) {  
     const float = floats[i * resultPerRow + j];  
     if (float === undefined) {  
     break;  
     }  
      
     const position = Math.floor(float * (towerConfig.columns - j));  
     result.push(values.splice(position, 1)[0] ?? 0);  
     }  
      
     results.push(result);  
    }  
      
    return results;  
      
    // Example: when clientSeed='abc', serverSeed='def', nonce=1, difficulty='easy'  
    // results: [  
    // [0, 1, 2],   
    // [0, 2, 3],   
    // [0, 2, 3],   
    // [0, 1, 2],   
    // [0, 1, 3],   
    // [0, 1, 2],   
    // [1, 2, 3],   
    // [0, 1, 3],   
    // [0, 1, 3]  
    // ]  
    // Each sub-array represents the column indexes of safe tiles for that row  
    // (e.g., for the first row, safe tiles are in columns 0, 1, and 2)

### Plinko

In Plinko the Players can choose between a pyramid of height 8 to 16. As the ball is released from the top, it will either fall left of right 8 to 16 times every time it hits a pin on the Plinko board. The game event in Plinko is to determine if the ball falls left or right at every level, represented by 0 and 1.

// Create multiple floats between 0 and 1 

// Example: floats = [0.8320531346835196, 0.8531235849950463, 0.18227359023876488, 0.774284637067467, 0.18627392360940576, 0.6012540922965854, 0.608081541955471, 0.46018488449044526] when clientSeed='abc', serverSeed='def', nonce=1, count=8 

// count refers to the number of plinko rows 

const floats = generateFloats({ serverSeed, clientSeed, nonce, cursor, count }); 

// Example: [1, 1, 0, 1, 0, 1, 1, 0] Each 1 represents a drop to the right whereas 0 represents a drop to the left 

const dropDirections = floats.map(val => Math.floor(val * 2)); 

// This sums up all the values in the array to get the final position of the ball from the left. 

// Example: finalPositionFromLeft = 5 (the ball will land on the 6th slot from the left) 

const finalPositionFromLeft = dropDirections.reduce((cur, acc) => cur + acc, 0)

### Dice  

In dice, a roll of the dice can be between the values of 0.00 to 100.00. The game event in Dice is to determine this value. We do this by using the (floats * 10,001) / 100). As we include values up to 2 decimal places, and the number 0, there are a total of 10,000 + 1 = 10,001 possible outcomes (including decimal places).
    
    
    // Creates an array of random numbers   
    // Example: With clientSeed = 'abc', serverSeed = 'def', nonce = 1, floats = [0.43053962965495884]   
    const floats = generateFloats({ serverSeed, clientSeed, nonce, cursor: 0, count: 1 });   
      
    // Converts the value in the array to a number between 0.00 to 100.00 and return the value   
      
    // Example: resultValue = [43.05]   
    const resultValue = floats.map(val => Math.floor(floats * 10001) / 100);   
    return resultValue[0];   
    // Example: 43.05 is returned as the result of the game

### Limbo

In limbo, the goal of the game is to guess a multiplier >1\. If the game result is greater than the multiplier, the player wins the game. The game event is to generate a float value that is above 1 and follows a hyperbolic curve distribution.
    
    
    // Generate float between 0 and 1   
    // Example: floats = [0.43053962965495884], float = 0.43053962965495884,   
    // when clientSeed='abc', serverSeed='def', nonce=1   
    const floats = generateFloats({ serverSeed, clientSeed, nonce, cursor: 0, count: 1 });   
    const float = floats[0]   
    // generate limbo game event results with house edge (1%)   
    // using a hyperbolic 1/x curve   
    // multiplying by this and dividing by (MAX_VALUE + 1) prevents division by 0 error   
      
    const MAX_VALUE = 2 ** 24;   
    const rtp = 0.99;   
      
    // Example: rawValue = 2.298905   
    const rawValue = (rtp * MAX_VALUE) / (MAX_VALUE * float + 1)   
      
    // resultValue = 2.29   
    const resultValue = (0.99 * MAX_VALUE / (MAX_VALUE * float +1)).toFixed(2,ROUND_DOWN);   
    return resultValue;

### Keno  

In Keno, the game event requires the identification of 10 unique gem placements on the keno board. To achieve this, we generate 10 different floats between 1 to 40., representing each location on the keno board. Then a is used to handle duplicated floats and return the final 10 gem locations
    
    
    // Generate 10 different floats as there are 10 positions that are selected when the game is played   
    // Example: floats = [18, 26, 7, 22, 31, 10, 23, 8, 24, 13] when clientSeed='abc', serverSeed='def', nonce=1   
      
    const floats = generateFloats({ serverSeed, clientSeed, nonce, cursor: 0, count: 10 }) .map((val, index) => Math.ceil(val * (40 - index)))   
      
    // array used to store the position of the gems const gemLocations: number[] = [];   
      
    // this loop helps to prevent duplicate gem positions by skipping positions that already contain a gem   
    for (const val of floats) { let currentGem = val;   
    let i = 0;   
    while (i <= currentGem) {   
     if (gemLocations.includes(i)) {   
     currentGem++;   
    }   
     i++;   
     }  
      
    gemLocations.push(currentGem);   
    }   
      
    // returns the location of the gem on the board   
    // Example: [18, 27, 7, 24, 35, 11, 28, 9, 31, 16] return gemLocations;

### Mines  

In Mines, the game event requires the identification of mines placement on the 25 tiled mines board. Based on the player's selection of a number of mines, we generate a list of integers from 0 to 24 to represent the 25 spaces on the board.
    
    
    // Generate 10 different floats   
    // Example: [20, 20, 4] when clientSeed='abc', serverSeed='def', nonce=1, numberOfBombs=3   
    const floats = generateFloats({ serverSeed, clientSeed, nonce, cursor: 0, count: numberOfBombs })   
     .map((val, index) => Math.floor(val * (25 - index)));   
      
    // array used to store the position of the bombs  
     const mineLocations: number[] = [];   
      
    // this loop helps to prevent duplicate bomb positions by skipping positions that already contain a mine  
     for (const val of floats) {   
     let currentMine = val;   
     let i = 0;   
      
     while (i <= currentMine) {   
     if (mineLocations.includes(i)) {  
     currentMine++;  
     } i++; } mineLocations.push(currentMine); }   
      
    // returns the location of the bombs on the board   
    // Example: [20, 21, 4]   
     return mineLocations;

### Roulette

Our Roulette is based on the European style single ‘0’ wheel where there are 37 different pockets ranging from 0 to 36. The game event in roulette is to calculate the pockets from the 37 pockets in which the ball will land.
    
    
    // Generate a float between 0 and 1   
    // Example: floats = [0.43053962965495884]   
    const floats = generateFloats({serverSeed, clientSeed, nonce, cursor: 0, count: 1 });   
      
    // generate an integer from 0 to 37 to represent the pocket to land on   
    // Example: resultValue = 15   
     const resultValue = floats.map(val => Math.floor(val * 37))[0];   
     return resultValue

### Wheel

In Wheel, the player will select the number of segments the wheel has. Depending on which segment the pointer stops, the player will win the multiplier at the segment. The game event is to determine the segment.
    
    
    // Generate a float between 0 and 1   
    // Example: floats = [0.43053962965495884]   
    const floats = generateFloats({serverSeed, clientSeed, nonce, cursor, count:1 });   
      
    // Get an integer representing the number of segments set in the game   
    // Example: resultSegment = 8   
    const resultSegment = floats.map(val => Math.floor(val * numberOfSegments))[0];   
      
    return resultSegment

### Blackjack

In blackjack, the game is played with an unlimited card deck. Hence each turn will always have the same probability. The game event in Blackjack is to determine the card being drawn if the player or the banker draws a card from the deck.
    
    
    // Generate a float between 0 and 1   
    // Example: floats = [0.43053962965495884, 0.6645898742135614, 0.17454026662744582, 0.5729992990382016, 0.850033157505095] (52 elements are generated but only 5 are shown here for brevity)   
    const floats = generateFloats({serverSeed, clientSeed, nonce, cursor, count:52 });   
      
    const CARDS = [ ♦2, ♥2, ♠2, ♣2, ♦3, ♥3, ♠3, ♣3, ♦4, ♥4, ♠4, ♣4, ♦5, ♥5, ♠5, ♣5, ♦6, ♥6, ♠6, ♣6, ♦7, ♥7, ♠7, ♣7, ♦8, ♥8, ♠8, ♣8, ♦9, ♥9, ♠9, ♣9, ♦10, ♥10, ♠10, ♣10, ♦J, ♥J, ♠J, ♣J, ♦Q, ♥Q, ♠Q, ♣Q, ♦K, ♥K, ♠K, ♣K, ♦A, ♥A, ♠A, ♣A   
    ];   
      
    // Get an integer 0 to 51, representing the index of the cards   
    // Example: cardFloats = [22, 34, 9, 29, 44]   
    const cardFloats = floats.map(val => Math.floor(val * 52));   
      
    // Example: cards = [♠7, ♠10, ♥4, ♥9, ♦K]   
    const cards = cardFloats.map(val => CARDS[val])   
    return cards;

### Hilo  
​

In Hilo, the game is played with an unlimited card deck Hence each turn will always have the same probability. The objective of the game is to guess if the next card is of higher or lower value than the existing card. The game event in Hilo is to determine which card a player draws next (if drawn).
    
    
    // Generate a float between 0 and 1   
    // Example: floats = [0.43053962965495884, 0.6645898742135614, 0.17454026662744582, 0.5729992990382016, 0.850033157505095] (an unlimited number of cards can be drawn but only 5 are shown here for brevity)   
    const floats = generateFloats({serverSeed, clientSeed, nonce, cursor, count:52 });   
      
    const CARDS = [   
    ♦2, ♥2, ♠2, ♣2, ♦3, ♥3, ♠3, ♣3, ♦4, ♥4, ♠4, ♣4, ♦5, ♥5, ♠5, ♣5, ♦6, ♥6, ♠6, ♣6, ♦7, ♥7, ♠7, ♣7, ♦8, ♥8, ♠8, ♣8, ♦9, ♥9, ♠9, ♣9, ♦10, ♥10, ♠10, ♣10, ♦J, ♥J, ♠J, ♣J, ♦Q, ♥Q, ♠Q, ♣Q, ♦K, ♥K, ♠K, ♣K, ♦A, ♥A, ♠A, ♣A   
    ];   
      
    // Get an integer 0 to 51, representing the index of the cards   
    // Example: cardFloats = [22, 34, 9, 29, 44]   
    const cardFloats = floats.map(val => Math.floor(val * 52));   
      
    // Example: cards = [♠7, ♠10, ♥4, ♥9, ♦K]   
    const cards = cardFloats.map(val => CARDS[val])   
    return cards;

### Baccarat

In Baccarat, the game is played with an unlimited card deck. Hence each turn will always have the same probability. The game event in Baccarat is to determine the cards being dealt to both Player and Banker hands across the initial deal and any additional draws following standard Baccarat drawing rules.

The cards are dealt in the following order from the generated float sequence:

  1. Player’s first card - float index 0

  2. Banker’s first card - float index 1

  3. Player’s second card - float index 2

  4. Banker’s second card - float index 3

  5. Player’s third card (if drawn) - float index 4

  6. Banker’s third card (if drawn)

     1. float index 4 - if player did not draw the third card

     2. float index 5 - if player draws the third card

    
    
    // Generate a float between 0 and 1  
    // Example: floats = [0.40353962965495884, 0.6645898742135614, 0.17454026662744582, 0.5729992990382016, 0.850033157505095] (an unlimited number of cards can be drawn but only 5 are shown here for brevity)  
    const floats = generateFloats({serverSeed, clientSeed, nonce, cursor, count:52 });  
      
    const CARDS = [  
    ♠2, ♥2, ♦2, ♣2, ♠3, ♥3, ♦3, ♣3, ♠4, ♥4,  
    ♦4, ♣4, ♠5, ♥5, ♦5, ♣5, ♠6, ♥6, ♦6, ♣6,  
    ♠7, ♥7, ♦7, ♣7, ♠8, ♥8, ♦8, ♣8, ♠9, ♥9,  
    ♦9, ♣9, ♠10, ♥10, ♦10, ♣10, ♠J, ♥J, ♦J,  
    ♣J, ♠Q, ♥Q, ♦Q, ♣Q, ♠K, ♥K, ♦K, ♣K, ♠A,  
    ♥A, ♦A, ♣A  
    ];  
      
    // Get an integer 0 to 51, representing the index of the cards  
    // Example: cardFloats = [22, 34, 9, 29, 44]  
    const cardFloats = floats.map(val => Math.floor(val * 52));  
      
    // Example: cards = [♠7, ♠10, ♥4, ♠9, ♦K]  
    const cards = cardFloats.map(val => CARDS[val])  
    return cards;

###   
Blitz

In Blitz, the game is played with an unlimited card deck. Hence, each turn will always have the same probability. The game event in Blitz is to determine the cards being dealt to the player across their chosen draw count, and to evaluate whether all drawn cards are unique.

  
​
    
    
    // Generate a float between 0 and 1  
    // Example: floats = [0.43053962965495884, 0.6645898742135614, 0.17454026662744582, 0.5729992990382016, 0.850033157505095] (an unlimited number of cards can be drawn but only 5 are shown here for brevity)  
    const floats = generateFloats({serverSeed, clientSeed, nonce, cursor, count:52 });  
      
    const CARDS = [  
    ♠2, ♥2, ♣2, ♦2, ♠3, ♥3, ♣3, ♦3, ♠4, ♥4,  
    ♣4, ♦4, ♠5, ♥5, ♣5, ♦5, ♠6, ♥6, ♣6, ♦6,  
    ♠7, ♥7, ♣7, ♦7, ♠8, ♥8, ♣8, ♦8, ♠9, ♥9,  
    ♣9, ♦9, ♠10, ♥10, ♣10, ♦10, ♠J, ♥J, ♣J, ♦J,  
    ♠Q, ♥Q, ♣Q, ♦Q, ♠K, ♥K, ♣K, ♦K, ♠A,  
    ♥A, ♣A, ♦A  
    ];  
      
    // Get an integer 0 to 51, representing the index of the cards  
    // Example: cardFloats = [22, 34, 9, 29, 44]  
    const cardFloats = floats.map(val => Math.floor(val * 52));  
      
    // Example: cards = [♠7, ♠10, ♥4, ♠9, ♦K]  
    const cards = cardFloats.map(val => CARDS[val])  
    return cards;

### Crash

  
There are 2 parts to the calculation of the result of a crash game

  * Bitcoin block hash

  * Seed

#### Bitcoin Hash

[![](https://downloads.intercomcdn.com/i/o/i7l0ll3e/2446639021/7061894d4b70a320fa028cd3a2e1/image.png?expires=1783408500&signature=dcc3f0b826353fbaed2fd64259dcad9d61262da6c07dfdb1a827745b4c31df67&req=diQjEM99lIFdWPMW1HO4zbuzls9vN%2B%2BqtfvlLb5zkh7DQzBS99%2BKPIiY1sQf%0AzW6m%2Bniukn74JmINuB8%3D%0A)](https://downloads.intercomcdn.com/i/o/i7l0ll3e/2446639021/7061894d4b70a320fa028cd3a2e1/image.png?expires=1783408500&signature=dcc3f0b826353fbaed2fd64259dcad9d61262da6c07dfdb1a827745b4c31df67&req=diQjEM99lIFdWPMW1HO4zbuzls9vN%2B%2BqtfvlLb5zkh7DQzBS99%2BKPIiY1sQf%0AzW6m%2Bniukn74JmINuB8%3D%0A)

####   
​The hash of block 949350 can be found on the BTC block explorer (<https://btcscan.org/block/000000000000000000018aa25655f289ef0ee08c91a66500adab6ba1ff4a9790>), and matches the hash that is shown in the tweet.

#### Seed  

The process for generating hashes for 10 million crash games is designed to ensure fairness and transparency, preventing any possibility of manipulation by Shuffle. Here’s how they are generated and verified:

  1. **Initial Seed Generation** : The seed for the 10 millionth game is randomly chosen by Shuffle.

  2. **Sequential Hashing for Seed Generation** : To generate the hash for all other games, we use a backward hashing method:

     * The hash for each game is the result of hashing the hash of the subsequent game. Specifically, the hash for the 9,999,999th game is the SHA256 hash of the 10 millionth game’s hash.

     * This chaining continues such that each game’s hash is derived from hashing the next game’s hash, culminating in the first game's hash being derived from the second game’s hash.

  3. **Verification of Seed Integrity** :

     * By publishing the hashed value of the seed used in the first game, players can independently verify the integrity of the hash for all games up to the 10 millionth game.

     * Using this method, if you know the hash for any specific game, you can verify the hash for all previous games. For example, if you know the seed of game 1323185, you can hash this to verify that it matches the hash of game 1323184, and so on, back to the first game.

       * For example, in game 55447 ([https://shuffle.us/games/originals/crash?md-id=55447&modal=crashGame)](https://shuffle.us/games/originals/crash?md-id=55447&modal=crashGame), the hash is c4b0c1394a9212cb3fc53674e756abba00973a970a4371c322463e8311e6e3e0

       * In game 55446 ([https://shuffle.us/games/originals/crash?md-id=55446&modal=crashGame](https://shuffle.us/games/originals/crash?md-id=55446&modal=crashGame)), the hash is ea98f461df194103890d8778487fc88390026bf1d1046dea6c278f10c98be2c0

       * The hash value of game 55446 is the SHA256 hash of 55447. You can use an online SHA256 calculator like <https://xorbin.com/tools/sha256-hash-calculator> to verify this

  4. **Transparency and Security** :

     * By using a future BTC block hash which is out of the control of Shuffle and publishing the hashed version of the seed of first game, users are can prove that crash game results are fair and were untampered.

### Slide

Slide uses the same provably fair and seeding process as Crash. The seeding was done publicly on [x.com](http://x.com/), and the post can be found [here](https://x.com/shuffleusa/status/2062394241368862904).  
​

[![](https://downloads.intercomcdn.com/i/o/i7l0ll3e/2464214131/70ea1800e36669e51638683cda45/image.png?expires=1783408500&signature=14e38525f1980afd8e2bf9aa413db8449feb08c8a8abbe3561b275f6cabc1a95&req=diQhEst%2FmYBcWPMW1HO4zZCa1V7panXOxOIQRj9LOl%2BEKWRpe1e%2FORazM7uv%0A2Q8%2BQDmW9ZY6oVKUhUo%3D%0A)](https://downloads.intercomcdn.com/i/o/i7l0ll3e/2464214131/70ea1800e36669e51638683cda45/image.png?expires=1783408500&signature=14e38525f1980afd8e2bf9aa413db8449feb08c8a8abbe3561b275f6cabc1a95&req=diQhEst%2FmYBcWPMW1HO4zZCa1V7panXOxOIQRj9LOl%2BEKWRpe1e%2FORazM7uv%0A2Q8%2BQDmW9ZY6oVKUhUo%3D%0A)

  
​

The hash of block 952294 can be found on the BTC block explorer (<https://btcscan.org/block/00000000000000000000a382760ff19cdb7f9f22147b9123f3e471fb3c6d129a>) and matches the hash shown in the tweet.  
​

  
​
