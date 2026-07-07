# Implementation

> Source: https://help.shuffle.us/en/articles/11882156-implementation
> Fetched: 2026-07-07 (verbatim mirror, converted to markdown)

**How is Provably Fair Implemented in code?**

Assuming that the game is finished and we have the un-hashed server seed, client seed, and nonce, here is how it works.

Three main steps will be required to generate game results.

  1. byteGenerator (Random Bytes Generation)

  2. generateFloats (Converting Bytes to Float (digits))

  3. Float to game events (Converting floats to actual events in original games)

**ByteGenerator Function as a Random Bytes Generator**

The `byteGenerator` function serves as a random byte generator.

It takes unique values of `clientSeed`, `serverSeed`, `nonce`, and `cursor` to generate a random and unique SHA-256 hashed value using the cryptographic HMAC_SHA256 function.

The SHA-256 value generated is 32 bytes in size. To ensure a balance between a sufficiently random game outcome and computational intensity, the 32 bytes is split into 8 sections of 4 bytes* each to generate every game result.

In certain games where more than 8 game result is required, we will utilize the `cursor`. The `cursor` initially starts at 0, and increases to 1,2,3,4 to satisfy the result requirement.

For games where we do not require more than 8 random outcomes, the `cursor` does not increment in value.

*4 bytes of data will give us 2^32 (4,294,967,296) possible outcomes which is a sufficiently large pool for randomness.
    
    
    function* byteGenerator({ serverSeed, clientSeed, nonce, cursor }: ByteGeneratorInterface) {  
      
     // Setup cursor variables let currentRound = Math.floor(cursor / 32);   
     let currentRoundCursor = cursor;   
     currentRoundCursor -= currentRound * 32;   
      
    // Generate outputs until cursor requirement fullfilled   
       while (true) {  
         // HMAC function used to output provided inputs into bytes   
            const hmac = crypto.createHmac('sha256', serverSeed);        
            hmac.update(`${clientSeed}:${nonce}:${currentRound}`);   
            const buffer = hmac.digest();   
      
    // Update curser for next iteration of loop   
       while (currentRoundCursor < 32) {   
        yield Number(buffer[currentRoundCursor]);   
        currentRoundCursor += 1;   
        }   
        currentRoundCursor = 0;   
        currentRound += 1;   
      }   
    }

**GenerateFloats Function To Convert Bytes to Floats**

This function converts the SHA-256 hexadecimal value from bytes into floats for use in further calculations for Game Events.

Below illustrates how a SHA-256 hexadecimal value is converted from byte to float. The final output, numArr, contains all the possible outcomes required by the game. If only 1 outcome is required, the list will contain only one value.

It returns an array of numbers between 0-1. if the count represents the number of elements in the returned array.

Code:
    
    
    // Convert the hash output from the rng byteGenerator to floats   
    export function generateFloats({   
        serverSeed,   
        clientSeed,   
        nonce,   
        cursor,   
        count,   
    }: GenerateFloatsInterface) {  
       // Random number generator function  
    const rng = byteGenerator({ serverSeed, clientSeed, nonce, cursor });   
    // Declare bytes as empty array   
    const bytes = [];   
      
    // Populate bytes array with sets of 4 from RNG output   
    while (bytes.length < count * 4) {  
       bytes.push(rng.next().value!);   
    }  
      
    // Return bytes as floats using lodash reduce function   
     const numArr = chunk(bytes, 4).map(bytesChunk =>  
        bytesChunk.reduce((result, value, i) => {   
           const divider = 256 ** (i + 1);   
           const partialResult = value / divider;   
           return result + partialResult;   
        }, 0),   
    );   
        return numArr;   
    }

All our original games utilize both **ByteGenerator** and **GenerateFloats functions** to generate random floats between 0 to 1. However, it is from here onwards that each game takes a unique procedure to determine the game event from the float generated.

The unique procedure will be explained in detail in **Game Events**.

**Illustrated Example**

Here we will illustrate how inputs are used to generate a dice game event
    
    
    Input Values: Given some random input values   
    serverSeed, clientSeed, nonce, cursor   
      
      
    Step 1: byteGenerator creates a SHA-256 byte  
    "a3f4e0ac7c7e8e9b5f16106c6b1d14e87c2c5a8d59b1d1c6a0b5f3e5a7d4c9a8”   
      
      
    Step 2: 256 bytes is split into 8 equal set of 32 bytes  
    "a3f4e0ac”, “7c7e8e9b”, “5f16106c”, “6b1d14e8”,   
    “7c2c5a8d”, “59b1d1c6”, “a0b5f3e5”, “a7d4c9a8”   
      
      
    Step 3: Each set is broken down into 2 bytes each   
    (only first 2 sets is shown)   
    set 1: a3-f4-e0-ac   
    set 2: 7c-7e-8e-9b   
    ......   
      
      
    Step 4: Each of the 2 bytes represent a number from 0 to 255   
    set 1: [163, 244, 224, 172]   
    set 2: [124, 126, 142, 155]   
    ......   
      
      
    Step 5: Using the formula in the code to generate numArr   
    Float 1 = (163 / (256^1)) + (244 / (256^2)) + (224 / (256^3))   
            + (172 / (256^4)) = 0.64045528601   
    Float 2 = (124 / (256^1)) + (126 / (256^2)) + (142 / (256^3))   
            + (155 / (256^4)) = 0.48630610737 Float 3 ......   
      
      
    Step 6: Output the floats in a list   
    numArr = [0.64045528601, 0.48630610737, ...... ]   
      
    *** Game Event Generation ***   
    Step 7: The numArr output will be used to generate game events,   
    depending on the game requirement   
      
      
    Example 7: Using dice as an example.   
    The game event is generated using the first numArr value.   
    const resultValue = floats.map(val => Math.floor(floats * 10001) / 100);  
      
       resultValue = Math.floor(0.64045528601 * 10001) / 100   
                   = 64.05

Note: In reality when a game is active, the server seed is hashed, hence the player and operator will not be able to view the individual output of this process during the game.

Since the same algorithm and functions are used during the round and after the round, the player is always able to prove the outcome given the same input
