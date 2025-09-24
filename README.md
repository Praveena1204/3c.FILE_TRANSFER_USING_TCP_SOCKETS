# 3c.CREATION FOR FILE TRANSFER USING TCP SOCKETS
```
Name: A. Praveena
Reg.no: 212224040247
```
## AIM
To write a python program for creating File Transfer using TCP Sockets Links
## ALGORITHM:
1. Import the necessary python modules.
2. Create a socket connection using socket module.
3. Send the message to write into the file to the client file.
4. Open the file and then send it to the client in byte format.
5. In the client side receive the file from server and then write the content into it.
## PROGRAM
SERVER
```
import socket
import os

HOST = '192.168.31.54'
PORT = 64856

def send_file(filename, conn):
    if os.path.isfile(filename):
        file_size = os.path.getsize(filename)
        conn.sendall(f"EXISTS {file_size}".encode('utf-8'))

        client_response = conn.recv(1024).decode('utf-8')
        if client_response == 'READY':
            with open(filename, 'rb') as f:
                chunk = f.read(1024)
                while chunk:
                    conn.sendall(chunk)
                    chunk = f.read(1024)
            print(f"File '{filename}' sent successfully.")
    else:
        conn.sendall(b'NOT_EXISTS')

with socket.socket(socket.AF_INET, socket.SOCK_STREAM) as server_socket:
    server_socket.bind((HOST, PORT))
    server_socket.listen()
    print(f"File server is listening on {HOST}:{PORT}")

    while True:
        conn, addr = server_socket.accept()
        with conn:
            print(f"Connected by {addr}")
            filename = conn.recv(1024).decode('utf-8')
            print(f"Client requested file: {filename}")
            send_file(filename, conn)

```
CLIENT
```
import socket
import os

HOST = '192.168.31.54' 
PORT = 64856

filename = input("Enter the filename you want to download: ")

with socket.socket(socket.AF_INET, socket.SOCK_STREAM) as s:
    s.connect((HOST, PORT))
    s.sendall(filename.encode('utf-8'))

    response = s.recv(1024).decode('utf-8')
    if response.startswith("EXISTS"):
        _, size = response.split()
        file_size = int(size)
        print(f"File '{filename}' exists on server, size: {file_size} bytes.")

        s.sendall(b'READY')
        received_size = 0
        with open('received_' + filename, 'wb') as f:
            while received_size < file_size:
                data = s.recv(1024)
                if not data:
                    break
                f.write(data)
                received_size += len(data)
                print(f"Received {received_size} of {file_size} bytes")

        print(f"File '{filename}' received and saved as 'received_{filename}'")
    else:
        print("File does not exist on server.")

```
## OUPUT
```
SERVER
```
<img width="650" height="190" alt="Screenshot 2025-09-24 104909" src="https://github.com/user-attachments/assets/08406745-6c25-4414-a45d-f647cb8cc422" />

```
CLIENT
```
<img width="814" height="206" alt="Screenshot 2025-09-24 104850" src="https://github.com/user-attachments/assets/26013094-ad2b-4fd3-8ab5-ff42a534e33d" />

## RESULT
Thus, the python program for creating File Transfer using TCP Sockets Links was 
successfully created and executed.
