# How to verify game outcomes

> Source: https://help.shuffle.us/en/articles/11882154-how-to-verify-game-outcomes
> Fetched: 2026-07-07 (verbatim mirror, converted to markdown)

**Commitment Scheme in provably fair**

  1. **Initial Commitment** : When a game is initiated, both the player and the operator generate a commitment. The commitment is hashed and is kept a secret to prevent tempering.

  2. **Playing** : The game proceeds as planned, with both parties aware of each other's commitments but not the secrets they conceal.

  3. **Reveal and Verify** : After the game has ended, reveal any hashed values to obtain the original secret values so that you can verify the results.

**How do we reveal and verify the game outcomes?**

The goal here is to reveal the secrets of the bets and verify them.

**Step 1: Open the round modal and click on rotate pair**

[![](https://downloads.intercomcdn.com/i/o/i7l0ll3e/1652108138/96357cbbea6f23c7676d6406ee63/Step+1.png?expires=1783408500&signature=5f747a408b02b187b9931e0a2cb9d2dd054c4c71f9165dfb04a7cc0c4a06b955&req=dSYiFMh%2BlYBcUfMW1HO4zRhdMYy1tpwKPgRCgmG5Zfwm8M3xB8Jdicf7KUzC%0AZf9o0YDm%2FNeQojI%2BYzA%3D%0A)](https://downloads.intercomcdn.com/i/o/i7l0ll3e/1652108138/96357cbbea6f23c7676d6406ee63/Step+1.png?expires=1783408500&signature=5f747a408b02b187b9931e0a2cb9d2dd054c4c71f9165dfb04a7cc0c4a06b955&req=dSYiFMh%2BlYBcUfMW1HO4zRhdMYy1tpwKPgRCgmG5Zfwm8M3xB8Jdicf7KUzC%0AZf9o0YDm%2FNeQojI%2BYzA%3D%0A)

**Step 2: Input your desired next client seed and click change. This will commit the new client seed and new server seed for the next round.**

Remember to complete any existing games before changing.

[![](https://downloads.intercomcdn.com/i/o/i7l0ll3e/1652108226/1fa46e81e99e43d26f96bde8a1bd/Step+2.png?expires=1783408500&signature=73a05f92dc1ce63f168e09fabb089b60dce115ebbbfb1f4ef6431ba367cf60de&req=dSYiFMh%2BlYNdX%2FMW1HO4zSr%2FG2SBo%2Fx4%2F2NabBw287G%2Bfjd9CuvjjOx0rhsZ%0Av225ZLb%2FLFpIy0O291k%3D%0A)](https://downloads.intercomcdn.com/i/o/i7l0ll3e/1652108226/1fa46e81e99e43d26f96bde8a1bd/Step+2.png?expires=1783408500&signature=73a05f92dc1ce63f168e09fabb089b60dce115ebbbfb1f4ef6431ba367cf60de&req=dSYiFMh%2BlYNdX%2FMW1HO4zSr%2FG2SBo%2Fx4%2F2NabBw287G%2Bfjd9CuvjjOx0rhsZ%0Av225ZLb%2FLFpIy0O291k%3D%0A)

**Step 3: Close the provably fair modal and return to the round model. For that particular round you played you would notice that the server seed has been unhashed and revealed to you. The Client Seed and Nonce for the round does not change.**

[![](https://downloads.intercomcdn.com/i/o/i7l0ll3e/1652108258/7919e541cca8ccf0797f2da2650b/Step+3.png?expires=1783408500&signature=fdcaf2aca11f75c880f81738feb3c5478b62065a2400c43331b7c6a13bd01fd2&req=dSYiFMh%2BlYNaUfMW1HO4zWrySz5Zx8VHF2Mv0W8temfUsi5BKh%2FVu312ZJOz%0AMII%2FT5%2BrBIh6Un3n3Sc%3D%0A)](https://downloads.intercomcdn.com/i/o/i7l0ll3e/1652108258/7919e541cca8ccf0797f2da2650b/Step+3.png?expires=1783408500&signature=fdcaf2aca11f75c880f81738feb3c5478b62065a2400c43331b7c6a13bd01fd2&req=dSYiFMh%2BlYNaUfMW1HO4zWrySz5Zx8VHF2Mv0W8temfUsi5BKh%2FVu312ZJOz%0AMII%2FT5%2BrBIh6Un3n3Sc%3D%0A)

**Step 4: Verify your bet. Click Verify to proceed**

[![](https://downloads.intercomcdn.com/i/o/i7l0ll3e/1652108302/9b6b9b61eeba769d3c7023dd9728/Step+4.png?expires=1783408500&signature=464ced2a86f7b307eaa4f8060fee1cb1b637b949d1de001d719591398e541610&req=dSYiFMh%2BlYJfW%2FMW1HO4zZX7Q%2BpbXO8GZ3qDj8nznSe3HjTca0HQHtvu6yzF%0ATHpqV8jRbguq9p7deG0%3D%0A)](https://downloads.intercomcdn.com/i/o/i7l0ll3e/1652108302/9b6b9b61eeba769d3c7023dd9728/Step+4.png?expires=1783408500&signature=464ced2a86f7b307eaa4f8060fee1cb1b637b949d1de001d719591398e541610&req=dSYiFMh%2BlYJfW%2FMW1HO4zZX7Q%2BpbXO8GZ3qDj8nznSe3HjTca0HQHtvu6yzF%0ATHpqV8jRbguq9p7deG0%3D%0A)

**Step 5: As you notice, by inputting the Client Seed, Server Seed, and Nonce of the round played, you get the same output as the original round result shown.**

[![](https://downloads.intercomcdn.com/i/o/i7l0ll3e/1652108372/ba57e41670b65c325eff7aecfa44/Step+5.png?expires=1783408500&signature=b044d8b34e7df41dd3ef5ddce21cd634ef8d2a4c9ef301cf2d707b9361ba5769&req=dSYiFMh%2BlYJYW%2FMW1HO4zZCa4Zd8AlXAGJUJ9UZVFMzeOK8pIftlokHaHEl%2F%0ABNqfyIAOdnV%2FowZyCvI%3D%0A)](https://downloads.intercomcdn.com/i/o/i7l0ll3e/1652108372/ba57e41670b65c325eff7aecfa44/Step+5.png?expires=1783408500&signature=b044d8b34e7df41dd3ef5ddce21cd634ef8d2a4c9ef301cf2d707b9361ba5769&req=dSYiFMh%2BlYJYW%2FMW1HO4zZCa4Zd8AlXAGJUJ9UZVFMzeOK8pIftlokHaHEl%2F%0ABNqfyIAOdnV%2FowZyCvI%3D%0A)
