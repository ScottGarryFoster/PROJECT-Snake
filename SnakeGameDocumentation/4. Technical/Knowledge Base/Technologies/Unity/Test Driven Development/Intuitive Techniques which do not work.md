#technical #knowleadgebase #technologies #unity #tdd #testdrivendevelopment 

> or rather, headaches with Unity testing and how to solve them.

|Owner|State|Last_update|
|--|--|--|
|@ScottGarryFoster|Development|1st July 2023|

**Table of contents**
- [Summary](#Summary)
	- [Test Cases do not work with Mono Behaviours](#Test%20Cases%20do%20not%20work%20with%20Mono%20Behaviours)

# Summary
When testing within the Unity testing framework there are certain caveats and 'gotchas' which should be noted. This page serves as a reminder and as a record of the solutions to such caveats. 

---
## Test Cases do not work with Mono Behaviours
### Summary
When creating tests a temptation is to pass in 'test cases' in the form of parameters. These are the standard NUnit parameter inputs seen [here](https://docs.nunit.org/articles/nunit/technical-notes/usage/Parameterized-Tests.html). This works perfectly fine when in breakout modules and any test which does require the IEnumerator return value.

IEnumerator is multithreading and the Nunit Parameterized Tests do not gel well together. Take the example below:

### Example
``` c#
[UnityTest]  
public IEnumerator Update_PlayerDoesNotMoveLeft_WhenMovingRightTest(  
[Values(KeyPressMethod.KeyPressed, KeyPressMethod.KeyDown)] KeyPressMethod firstMethod, 
[Values(KeyPressMethod.KeyPressed, KeyPressMethod.KeyDown)] KeyPressMethod secondMethod)  
{  
// Creates this.interactableActor - a game object which moves.
Setup();  
  
// Arrange  
yield return new WaitForEndOfFrame();  
  
Vector2 expectedPosition = CopyVector3(this.interactableActor.transform.position);  
expectedPosition.x += 2;  
  
MockKeyInput(firstMethod, EGameplayButton.DirectionRight);  
yield return RunUpdateCycle();  
MockKeyInput(secondMethod, EGameplayButton.DirectionLeft);  
  
// Act  
yield return RunUpdateCycle();  
  
// Assert  
Vector2 actualPosition = this.interactableActor.transform.position;  
Assert.AreEqual(expectedPosition, actualPosition,  
$"Expected {expectedPosition.ToString()} Actual {actualPosition.ToString()}");  
}
```

This does not work because the order of operations is as follows:
1. Create testcase 1
2. Wait for End of Frame
3. Mock Input
4. RunUpdateCycle for test 1
5. Create test case 2
6. RunUpdateCycle for test 1
7. Wait for End of frame
8. Mock Input
9. RunUpdate Cycle for test 2
10. Create testcase 3
11. RunUpdate Cycle for test 2
12. Wait for End of Frame
13. Mock Input
14. RunUpdate Cycle for test 3
15. Create test case 4
16. RunUpdate Cycle for test 3
17. Wait for End of Frame
18. Mock Input
19. RunUpdate Cycle for test 4
20. RunUpdate Cycle for test 4

This might not always be the case (depends on how the test is setup) however as you may discover what this leads to is one test affecting the other. The third test passes when run with values passed in and not when simply set within a single test.

### Solution
There are three solutions.
1. Do not use Parameterized tests with IEnumerator tests.
2. Store everything locally to the method
	* This is not great as generally we want our tests to be readable. Remember tests are code.
3. Break out logic into non-update / start loop tests.
	* Certainly should not be the norm. Would like more integrated tests than fully mocked ones.

---
## Update loops are a mess for actual testing
### Summary
For as much as testing with real implementation is much better than mocking implementations, the mono behaviour update loop testing within Unity is a complete mess.

Testing Start and Update for very simple things, for example any of the following:
1. Does my object construct correctly
2. Does my player move in the correct direction
3. Does a signal go from A to B when a signal goes through such as animations.

Anything beyond this will lead to fragile, slow and complete messes of tests which are not worth it. Fragile tests completely breaks the trusted feedback loop in test driven development so if the following is what is tested, look to the solution.
1. Waiting particular amounts of time in succession to see the position of the player
2. Using the collision objects to send signals between one another
3. In depth analysis for the Update loop, X step after Y time

### Example
The following example tests the movement of a snake and the tail growth. The game objects remain within the method.

```c#
[UnityTest]  
public IEnumerator CollideWithFood_TailGrows_WhenMoreFoodIsEatenTest()  
{  
//Setup();  
GameObject po = new GameObject();  
po.tag = "Player";  
AddFullCollider(po);  
var sp = po.AddComponent<SnakePlayer>();  
  
// The only reason Movement Speed is internal is to speed up tests  
// We speed up Time delta and slow down frames.  
sp.movementSpeed = 0.025f;  
Time.maximumDeltaTime = 0.0001f;  
  
var mockGameplayInputs2 = new Mock<IGameplayInputs>();  
sp.gameplayInputs = mockGameplayInputs2.Object;  
  
SnakeTail snakeTailPrefab = Resources.Load<SnakeTail>("Actors/Snake/SnakeTail");  
sp.snakeTailPrefab = snakeTailPrefab;  
  
// END SETUP  
  
// Arrange  
yield return new WaitForEndOfFrame();  
  
// Eat  
GameObject food = SetupSnakeToEatFood(po, mockGameplayInputs2);  
yield return RunMovementUpdateCycle(sp);  
Vector3 firstFoodEaten = CopyVector3(po.transform.position);  
yield return new WaitForEndOfFrame();  
yield return new WaitForEndOfFrame();  
  
// Eat  
GameObject food2 = SetupSnakeToEatFood(po, mockGameplayInputs2);  
yield return RunMovementUpdateCycle(sp);  
Vector3 secondFoodEaten = CopyVector3(po.transform.position);  
  
// Act  
yield return RunMovementUpdateCycle(sp);  
  
// Assert  
Vector3 tailPiece = sp.SnakeTailPieces[0].gameObject.transform.position;  
Assert.AreEqual(tailPiece, secondFoodEaten, $"secondFoodEaten == tailPiece | {secondFoodEaten} == {tailPiece}");  
Vector3 tailPiece2 = sp.SnakeTailPieces[1].gameObject.transform.position;  
Assert.AreEqual(tailPiece2, firstFoodEaten, $"firstFoodEaten == tailPiece2 | {firstFoodEaten} == {tailPiece2}");  
  
// Teardown  
Object.DestroyImmediate(food);  
Object.DestroyImmediate(food2);  
}
```

There is nothing particularly incorrect with this test, within all the methods it passes around the objects on the stack and the scene is setup correctly. The issue appears to be conflict with other tests.

This test relies on setting up a collider in front of the snake, moving into the collider and then the snake moving into a second collider to grow two tail pieces. If this test is run alone we get these logs showing the result:
```
Output: 
Move - Snake moves
TestFood - Collider responce
Eat - Snake responds to Collider
Move - Snake Moved
TestFood - Collider responce
Eat - Snake responds to Collider
Move - Snake Moved
Move - Snake Moved
```
This passes the test. Now we press 'Run All Tests' which includes other tests setting up colliders and objects.
```
secondFoodEaten == tailPiece | (0.00, 2.00, 0.00) == (0.00, 3.00, 0.00)

Output: 
Move - Snake moves
Move - Snake moves
Move - Snake moves
Move - Snake moves
TestFood - Collider responce
Eat - Snake responds to Collider
Move - Snake moves
Move - Snake moves
Move - Snake moves
TestFood - Collider responce
Eat - Snake responds to Collider
Move - Snake moves
Move - Snake moves
Move - Snake moves
Move - Snake moves
Move - Snake moves
Move - Snake moves
Move - Snake moves
```
This naturally fails the test but what is actually going on here? Well firstly it is moving many more times than it was before, thankfully it is colliding the same number of times. Why is this occurring on Run All but not when running a single test? The answer can be found by running it several times and counting the move statements.

**Count of Move Updates when running All**:

|Before 1st|Between|After Both|Pass/Fail|
|---|---|---|---|
|1|7|6|Fail|
|4|3|4|Pass|
|4|4|3|Pass|
|3|4|10|Fail|
|4|0|9|Fail|

When attempting to reproduce this it discovered that performance was a large factor in how this operated. When testing the update loop Time.Delta is used if the performance is better the tests perform more reliably as performance weakens, they become more fragile. 

To speed up the tests a smaller span of time was set for movement:
```c#
// The only reason Movement Speed is internal is to speed up tests  
// We speed up Time delta and slow down frames.  
sp.movementSpeed = 0.025f;  
Time.maximumDeltaTime = 0.0001f;  
```
This however means that if the performance when testing is ever below this performance level that the test fails. It is near impossible to tell if the reason is performance or if it is simply a failing test.
For this reason simplier tests should be performed at the Mono Behaviour level as the alternative of increasing this value would lead to much slower tests also violating a rule of test driven development.
### Solution
To create more stable tests test in this manner:
1. Create implementations for Update and Start for the behaviours of objects when Update loops are needed.
	* Test with injection that these implementations are used at the MonoBehaviour level
2. Retain Mono Behaviour tests that do not heavily rely on the Update loop / time delta.
3. Test this other implementation as a product of the methods it contains.

> Also be considerate of these situation for QA tests.

---
## Edit & Play mode breaks
### Summary
For seemingly no reason at all, the option Edit and Play Mode breaks Rider. Whilst creating tests you will probably create both editor and play based tests. An option within Rider might temp you into running all the tests all at once, Edit & Play mode.

The Edit & Play Mode option freezes the Unit Tests to the point where the output occurs and yet the tests do not fully resolve. To fix this, open up task manager and close Unity Editor.

### Example
This is a test project which contains no other code than an Editor based set of tests.
The option selected is Edit & Play. Notice the execution 'stop' button. This will remain forever.
![[T-KB-T-U-TDD-001.png]]
When I close the Unity instance it resolves.
![[T-KB-T-U-TDD-004.png]]
If I ran the tests more than once then you **need to use task manager to resolve the tests**.
![[T-KB-T-U-TDD-003.png]]
![[T-KB-T-U-TDD-002.png]]
When selecting Edit mode, the test passed and then resolved - the Unity instance sent the signal back.

### Solution
* Run Editor tests with Edit mode, Player with Play Mode.