# Ex.No: 6  Implementation of Steering behaviour-Pursue and Evade in Unity
### DATE:  07/04/2025                                                           
### REGISTER NUMBER : 212223240177
### AIM: 
To write a program to simulate the process of Pursue and Evade behavior in Unity using NavigationMeshAgent. 
### Algorithm:

1. Create a New Unity Project by Open the  Unity Hub and create a new 3D Project.
2. Name the project "SteeringBehaviors" and select a location. Click Create.
3.Open Unity Scene (default is SampleScene).
  In the Hierarchy, create a Plane:
  Right-click → 3D Object → Plane (this will be the ground).
  Set its Scale to (10, 1, 10) for a larger surface.
  Create three Capsule for the Player, Pursuer, and Evader:
  Rename them to "Player", "Pursuer", and "Evader".
  Set their Y Position to 0.5 (so they sit on the ground).
  Change their Material for better distinction (optional).
3. Add NavMesh and Bake
   Window → AI → Navigation (opens the Navigation tab).
   Select the Plane, go to the Navigation tab, and mark it as Navigation Static.
   Go to the Bake tab and click Bake.
   or
   Add navMeshSurface to plane and bake 
4. Add NavMeshAgent Component
    Select Pursuer, and Evader.
    Click Add Component → Search for NavMeshAgent and add it.
    Adjust NavMeshAgent Settings:
    Player: Set Speed = 5.
    Pursuer: Set Speed = 4.
    Evader: Set Speed = 6.

5. Write a script for  Player_movement behavior and save it
#### PlayerMovement
```c#

using UnityEngine;

public class PlayerMovement : MonoBehaviour
{
    Rigidbody rb;
    [SerializeField] float walkSpeed = 10f;
    float hInput;
    float vInput;

    void Start()
    {
        rb = GetComponent<Rigidbody>();
    }
    void Update()
    {
        hInput = Input.GetAxis("Horizontal");
        vInput = Input.GetAxis("Vertical");
        Vector3 direction = new Vector3(hInput,rb.velocity.y,vInput);
        rb.velocity = direction.normalized * walkSpeed;

    }

}

```
#### Evader script
```c#

using UnityEngine;
using UnityEngine.AI;

public class Evader : MonoBehaviour
{
   // Start is called before the first frame update
    public NavMeshAgent agent;
    public Transform target;
    public float evadeSpeed;
    void Start()
    {
        agent= GetComponent<NavMeshAgent>();
    }

    void evade()
    {
        Vector3 dir = transform.position - target.position;
        Vector3 evadePosition = transform.position + dir.normalized * evadeSpeed;
        agent.SetDestination(evadePosition);

    }
    // Update is called once per frame
    void Update()
    {
        evade();          
     }
}

```
#### Pursuer script
```c#
public class Pursuer: MonoBehaviour
{
    // Start is called before the first frame update
    NavMeshAgent agent;
    public Transform target;
    public float speed;
    void Start()
    {
        agent=this.GetComponent<NavMeshAgent>();
    }
       // Update is called once per frame
    void pursue()
    {
       Vector3 targetvelocity=target.position-transform.position;
       Vector3 futurepos = transform.position + targetvelocity.normalized*speed;
       agent.SetDestination(target.position);
    } 
    // Update is called once per frame
    void Update()
    {
        pursue();          
     }
}
```
7. Attach the Script to each player,pursuer and Evader.
   Drag & Drop the Target from the Hierarchy into the "Target" field in the script component ( For pursuer and Evader).
12. Run the game 
13. Stop the program
    

### Output:

#### Player
![Screenshot 2025-04-07 165039](https://github.com/user-attachments/assets/9253585c-4b48-4e60-8dda-9ab97a63437b)

#### Evader
![Screenshot 2025-04-07 165051](https://github.com/user-attachments/assets/c0c0be93-e047-4af9-8ad6-140cb77acbc9)

#### Pursuer
![Screenshot 2025-04-07 165059](https://github.com/user-attachments/assets/17390858-cec7-4f1b-b187-9ab04bcbe478)







### Result:
Thus the simple pursue and evade behavior was implemented successfully.
