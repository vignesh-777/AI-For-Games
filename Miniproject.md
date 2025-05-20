# Ex.No: 10  Implementation of 2D/3D game in unity
### DATE:    14 / 05 / 2025                                                                        
### REGISTER NUMBER : 212223240177
### AIM: 
To develop a 3D platformer game in Unity using C# and to implement artificial intelligence using Unity ML-Agents Toolkit.

### DESCRIPTION:
This project is a 3D platformer game where the player can move, jump across platforms, avoid AI-controlled enemies, and reach the goal zone. Unity’s ML-Agents Toolkit is used to train enemy characters to intelligently chase the player using reinforcement learning.

### Algorithm:

1.Open Unity and create a 3D project.

2. Design the level using 3D primitives (Cube for platforms, Capsule for player, etc.).

3. Implement Player Movement using Rigidbody and C# script.

4. Add an Enemy Agent and configure ML-Agent script.

5. Train the enemy using Unity ML-Agents (Python integration).

6. Add game elements such as goal zone, reset zones, and colliders.

7. Test the game functionality and integrate trained .onnx model into Unity.
 
### Program:

##### PlayerMovement.cs
```csharp
using UnityEngine;

public class PlayerMovement : MonoBehaviour
{
    public float moveSpeed = 5f;
    public float jumpForce = 7f;
    private Rigidbody rb;
    private bool isGrounded;

    void Start()
    {
        rb = GetComponent<Rigidbody>();
    }

    void Update()
    {
        float moveX = Input.GetAxis("Horizontal") * moveSpeed;
        float moveZ = Input.GetAxis("Vertical") * moveSpeed;
        Vector3 move = transform.right * moveX + transform.forward * moveZ;
        rb.velocity = new Vector3(move.x, rb.velocity.y, move.z);

        if (Input.GetButtonDown("Jump") && isGrounded)
        {
            rb.AddForce(Vector3.up * jumpForce, ForceMode.Impulse);
        }
    }

    private void OnCollisionStay(Collision collision)
    {
        if (collision.gameObject.CompareTag("Ground"))
            isGrounded = true;
    }

    private void OnCollisionExit(Collision collision)
    {
        if (collision.gameObject.CompareTag("Ground"))
            isGrounded = false;
    }
}

```
### ML-Agent
##### EnemyAgent.cs
```
using Unity.MLAgents;
using Unity.MLAgents.Sensors;
using Unity.MLAgents.Actuators;
using UnityEngine;

public class EnemyAgent : Agent
{
    public Transform player;
    private Rigidbody rb;
    public float speed = 3f;

    public override void Initialize()
    {
        rb = GetComponent<Rigidbody>();
    }

    public override void OnEpisodeBegin()
    {
        transform.localPosition = new Vector3(Random.Range(-4, 4), 0.5f, Random.Range(-4, 4));
        player.localPosition = new Vector3(Random.Range(-4, 4), 0.5f, Random.Range(-4, 4));
        rb.velocity = Vector3.zero;
    }

    public override void CollectObservations(VectorSensor sensor)
    {
        sensor.AddObservation(transform.localPosition);
        sensor.AddObservation(player.localPosition);
    }

    public override void OnActionReceived(ActionBuffers actions)
    {
        Vector3 move = new Vector3(actions.ContinuousActions[0], 0, actions.ContinuousActions[1]);
        rb.AddForce(move * speed);

        float distance = Vector3.Distance(transform.localPosition, player.localPosition);
        if (distance < 1.2f)
        {
            SetReward(1f); // reward for catching player
            EndEpisode();
        }

        if (transform.localPosition.y < -1f)
        {
            SetReward(-1f); // penalty for falling
            EndEpisode();
        }
    }

    public override void Heuristic(in ActionBuffers actionsOut)
    {
        var continuousActions = actionsOut.ContinuousActions;
        continuousActions[0] = Input.GetAxis("Horizontal");
        continuousActions[1] = Input.GetAxis("Vertical");
    }
}


```
### YAML Configuration
##### EnemyAgentBehavior.yaml
```
behaviors:
  EnemyAgent:
    trainer_type: ppo
    max_steps: 50000
    time_horizon: 64
    summary_freq: 1000
    batch_size: 128
    buffer_size: 2048
    learning_rate: 0.0003
    reward_signals:
      extrinsic:
        gamma: 0.99
        strength: 1.0

```

### Output:
The player moves and jumps across platforms.

An AI enemy chases the player after being trained using Unity ML-Agents.

Game resets if the player is caught or falls.

Victory when player reaches the goal zone.
### Result:
Thus, the 3D platformer game was successfully developed using Unity and adopted Unity ML-Agents artificial intelligence technology to train and control enemy behavior.
