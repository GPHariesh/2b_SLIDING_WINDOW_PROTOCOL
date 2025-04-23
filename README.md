# 2b IMPLEMENTATION OF SLIDING WINDOW PROTOCOL
## AIM
## ALGORITHM:
1. Start the program.
2. Get the frame size from the user
3. To create the frame based on the user request.
4. To send frames to server from the client side.
5. If your frames reach the server it will send ACK signal to client
6. Stop the Program
## PROGRAM
```
client.py
import socket


client_socket = socket.socket()
client_socket.connect(('localhost', 8000))


window_size = int(input("Enter the window size: "))
total_frames = int(input("Enter the total number of frames to send: "))


frames = [f"Frame {i+1}" for i in range(total_frames)]


i = 0
while i < total_frames:
    for j in range(i, min(i + window_size, total_frames)):
        client_socket.send(frames[j].encode())
        print(f"Sent: {frames[j]}")

   
    for j in range(i, min(i + window_size, total_frames)):
        ack = client_socket.recv(1024).decode()
        print(f"Received: {ack}")
    
    
    i += window_size


client_socket.send("END".encode())
client_socket.close()

server.py

import socket


server_socket = socket.socket()
server_socket.bind(('localhost', 8000))
server_socket.listen(1)
print("Server is waiting for a connection...")


conn, addr = server_socket.accept()
print(f"Connected to {addr}")

while True:
    frame = conn.recv(1024).decode()
    if frame == "END":
        break
    print(f"Received: {frame}")
    conn.send(f"ACK for {frame}".encode())


conn.close()
server_socket.close()

```
## OUPUT
![Screenshot 2025-04-23 222655](https://github.com/user-attachments/assets/b2fe7f5e-4205-4b7e-a0b6-ce69d246d5fd)
![Screenshot 2025-04-23 222702](https://github.com/user-attachments/assets/952a8ab0-76a4-4265-aa3b-6ef31e672b0b)


## RESULT
Thus, python program to perform stop and wait protocol was successfully executed
