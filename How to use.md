# Endless generate rooms in uinty
```c#
using System.Collections.Generic;
using UnityEngine;

public class RoomManager : MonoBehaviour
{
    public GameObject roomPrefab; // Room prefab
    public Transform player; // Player's position
    public int chunkSize = 5; // Chunk size (number of rooms in a row/column in a chunk)
    public int roomSize = 10; // Size of a single room

    private Dictionary<Vector2, List<GameObject>> chunks = new Dictionary<Vector2, List<GameObject>>();
    private Vector3 playerPosition;
    private Vector2 currentChunk;

    private void Start()
    {
        if (player == null)
        {
            player = Camera.main.transform;
        }

        playerPosition = new Vector3(player.position.x, 0, player.position.z);
        currentChunk = GetChunk(playerPosition);

        // Create initial room chunks
        CreateChunksAroundPlayer(currentChunk);
    }

    private void Update()
    {
        playerPosition = new Vector3(player.position.x, 0, player.position.z);
        Vector2 newChunk = GetChunk(playerPosition);

        if (newChunk != currentChunk)
        {
            // Reallocate rooms when moving to a new chunk
            currentChunk = newChunk;
            CreateChunksAroundPlayer(currentChunk);
        }
    }

    private Vector2 GetChunk(Vector3 position)
    {
        int x = Mathf.FloorToInt(position.x / (chunkSize * roomSize));
        int z = Mathf.FloorToInt(position.z / (chunkSize * roomSize));
        return new Vector2(x, z);
    }

    private void CreateChunksAroundPlayer(Vector2 centerChunk)
    {
        // Deactivate old room chunks
        foreach (var chunk in chunks.Values)
        {
            foreach (var room in chunk)
            {
                room.SetActive(false);
            }
        }

        // Create new room chunks around the player
        for (int x = -chunkSize / 2; x <= chunkSize / 2; x++)
        {
            for (int z = -chunkSize / 2; z <= chunkSize / 2; z++)
            {
                Vector2 chunkOffset = new Vector2(x, z);
                Vector2 chunkPosition = centerChunk + chunkOffset;

                if (!chunks.ContainsKey(chunkPosition))
                {
                    List<GameObject> chunkRooms = new List<GameObject>();

                    for (int i = 0; i < chunkSize; i++)
                    {
                        for (int j = 0; j < chunkSize; j++)
                        {
                            Vector3 roomPosition = new Vector3(chunkPosition.x * chunkSize * roomSize + i * roomSize, 0, chunkPosition.y * chunkSize * roomSize + j * roomSize);
                            GameObject newRoom = Instantiate(roomPrefab, roomPosition, Quaternion.identity);
                            chunkRooms.Add(newRoom);
                        }
                    }

                    chunks.Add(chunkPosition, chunkRooms);
                }
                else
                {
                    // If the chunk already exists, activate its rooms
                    List<GameObject> chunkRooms = chunks[chunkPosition];
                    foreach (var room in chunkRooms)
                    {
                        room.SetActive(true);
                    }
                }
            }
        }
    }
}
```
# How to use
![1](https://github.com/zaveshaa/Endless-generate-rooms-in-uinty/assets/127344512/0571594b-7a09-4c49-adfd-38fc629a200b)
![2](https://github.com/zaveshaa/Endless-generate-rooms-in-uinty/assets/127344512/01af0a33-3a34-4a0b-95db-2a752438c698)
# First, create your room manager as an empty object

# Second, move the script to the manager
![3](https://github.com/zaveshaa/Endless-generate-rooms-in-uinty/assets/127344512/f0e7f65b-c5e0-4964-b57f-64cf46db73cb)

# Third, select a room and player prefab. You can also adjust the chunk sizes)
![4](https://github.com/zaveshaa/Endless-generate-rooms-in-uinty/assets/127344512/3ec536d2-761e-41d4-998e-5786d71ddb70)

# All is ready
![9](https://github.com/zaveshaa/Endless-generate-rooms-in-uinty/assets/127344512/70013ef7-25d4-448f-a2d3-0170b3666235)
> Improve this code




